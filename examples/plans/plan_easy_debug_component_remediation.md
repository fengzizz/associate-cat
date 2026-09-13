# EasyDebugComponent Remediation Plan

> Public case note: This is an edited planning snapshot from an Unreal Engine plugin review. It shows repair-scope reassessment, not a completed fix or an independently rerun validation. The original request is not included because it was not supplied with this report. Project and UE 5.8 engine paths identify the reviewed source snapshot; those source files are not distributed here, and line numbers may differ in other revisions. `Ready` describes planning readiness.

## 1. Requirement Correction and Key Assumptions

- Task Type: Code-review reassessment and bug-fix planning.
- Document Status: Final Snapshot
- Design Readiness: Ready
- Document Output Location: examples/plans/plan_easy_debug_component_remediation.md
- Task Description: Reassess the component review against normal callers and engine lifecycle guarantees; repair only a demonstrated, reachable defect and require validation proportional to that repair.
- Adopted Interpretation: An internal function lacking defensive checks is not automatically defective. A retained finding requires a supported execution path, an observable wrong result, and evidence that existing lifecycle or caller guarantees do not prevent it.
- Assumptions and Impacts: Normal use means the supplied subsystem/Blueprint API and stock HUD lifecycle in Game/PIE worlds. Arbitrary calls to public C++ internals, manual lifecycle dispatch, and custom HUD replacement ordering do not establish a required repair without a concrete supported caller.
- Contradictions and Missing Information: No user runtime reproduction has been supplied. The callback defect is established by current plugin and engine source; runtime validation has not been performed. The project uses UE 5.8, whereas the existing standalone plugin build script targets UE 5.7.

An earlier review proposed three repair tasks. This reassessment checks those findings against supported callers and engine lifecycle guarantees, retaining one routing repair and excluding two unsupported repairs. The earlier review is background material; its implementation scope is not adopted here.

## 2. Goals and Scope

- Goals: Prevent one world's debug layouts from being drawn or advanced by another HUD's render notification.
- In Scope: One entry-condition change in TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugComponent.cpp, OnHUDPostRender. Review context includes its header, the subsystem's setup/reset callers, context processing, and Unreal's HUD and actor lifecycle.
- Out of Scope: BeginPlay retry, teardown ownership redesign, delegate-handle cleanup changes, extra internal null/world checks, signature changes, per-frame deduplication, general split-screen support, engine edits, demo synchronization, and packaging-script maintenance.
- Notes: Preserve the existing first-player HUD selection, component settings, layout algorithms, and hidden-mode processing. No production code is changed by this planning request. Existing working-tree edits must be preserved during subsequent implementation.

## 3. Preliminary Analysis

### 3.1 Component and subsystem form one constrained lifecycle

The component owns a render subscription, not the layouts. BeginPlay sets TargetHud before invoking Super; HandleHud binds once using bIsHandled. The subsystem selects the first controller's HUD, registers a component if needed, calls HandleHud, and assigns the owning subsystem. Context processing obtains layouts from that assigned subsystem and uses the component's world clock while drawing on the callback's HUD/canvas.

That split is safe only when the callback corresponds to the intended world. An instance-looking subscription expression does not enforce that constraint: AHUD::OnHUDPostRender is static. Every HUD broadcast reaches every subscribed component in the same process. OnHUDPostRender currently accepts any non-null HUD/canvas when its subsystem is valid. The downstream processor contains no routing filter.

The component is the primary analysis unit because initialization, subscription, ownership, and processing converge there. Subsystem setup/reset and context processing were examined as direct constraints. Engine lifecycle was examined to distinguish reachable failures from manually manufactured states. Formatting, unrelated layout mutations, and other plugins do not affect this repair.

Source anchors:

- Component callback and lifecycle (`TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugComponent.cpp:22`), especially OnHUDPostRender at line 86.
- Subsystem setup and reset (`TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugWorldSubsystem.cpp:70`), especially RegisterComponent before HandleHud and ResetDebug callers.
- Context lookup (`TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugStringContext.cpp:24`), shutdown (`TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugStringContext.cpp:199`), and processing (`TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugStringContext.cpp:776`).
- Global HUD delegate (`Engine/Source/Runtime/Engine/Classes/GameFramework/HUD.h:497`) and broadcast (`Engine/Source/Runtime/Engine/Private/HUD.cpp:240`).

### 3.2 Finding disposition after reachability analysis

| Previous finding | Current conclusion | Reason and scope consequence |
| --- | --- | --- |
| F1: Foreign HUD callback processing | Retain as P2 correctness bug | Standard same-process multiplayer PIE creates multiple rendering HUDs and per-world plugin state. The engine's static delegate supplies foreign callbacks without misuse of plugin internals. Keep one routing fix. |
| F2: Former component resets a replacement session | Not established as a normal-use bug; remove repair | The proposed reproduction explicitly resets A, sets up B, and only then ends A. No such caller sequence was found in the reviewed production paths. Stock ClientSetHUD destroys the old HUD before spawning its replacement. |
| F3: Setup before component BeginPlay never binds | Conditional risk, not established for normal setup; remove repair | For an owner that has begun or is beginning play, dynamic component registration synchronously invokes component BeginPlay before returning to HandleHud. The prior review omitted that guarantee. |

F2 details: ResetDebug's in-plugin callers are component context shutdown and subsystem Deinitialize. Ordinary component EndPlay reaches shutdown while setup is still active, so its lookup does not start setup again. World EndPlay routes actor EndPlay; subsystem deinitialization occurs in cleanup. The mere presence of lazy setup inside a shutdown helper does not establish that the abnormal branch is reached. A custom caller could force it, but that is insufficient grounds for changing lifecycle semantics in this plan.

F3 details: The relevant engine rule is AActor::HandleRegisterComponentWithWorld (`Engine/Source/Runtime/Engine/Private/Actor.cpp:6427`). Existing registered components are also begun before the actor's Blueprint BeginPlay event. The component itself sets TargetHud before its own Blueprint BeginPlay dispatch through Super. A HUD exposed to callers before it starts play can still create the conditional failure described in the earlier review; no normal project call path establishing that condition was found. This is not a claim that all custom startup sequences are impossible. Reopen only with an actual supported caller and observable missing output.

The stock replacement order is visible in ClientSetHUD_Implementation (`Engine/Source/Runtime/Engine/Private/PlayerController.cpp:1373`). The world lifecycle anchors are actor EndPlay routing (`Engine/Source/Runtime/Engine/Private/World.cpp:6205`) and subsystem deinitialization (`Engine/Source/Runtime/Engine/Private/World.cpp:6486`). These checks support excluding the speculative scenarios rather than adding defensive code for them.

### 3.3 Retained bug: Foreign HUD notifications process local debug data

- Reproduction Conditions / Trigger Probability: Use two clients with rendering HUDs in one PIE process and write different strings through the normal API in each world. Each subscribed component receives broadcasts from both HUDs. This dispatch is deterministic under those conditions; no manual internal calls are needed. A single-HUD process does not exhibit cross-world contamination, and separate processes do not share this delegate.
- Actual Behavior vs. Expected Behavior: A component can draw its world's text into another world's viewport and advance pending-removal counters on the other HUD's renders. Each world should render and advance its debug data on its selected HUD's notification.
- Root-Cause Hypotheses: Confirmed static event subscription plus missing HUD identity filtering. The processor combines component-owned state with event-provided output surfaces.
- Minimal Fix Point: The initial guard in OnHUDPostRender, before visibility evaluation and _OnProcessDebugData.
- Regression Risks and Observation Points: Hiding debug output must still permit update/expiry processing on the matching HUD. Put the routing rejection before processing, but leave the current drawDebug behavior intact.

Priority is P2: this is a real multi-HUD correctness problem, not evidence of a crash, destructive data loss, or universal single-player failure. The earlier P1 rating was stronger than the demonstrated impact warrants.

### 3.4 Internal contracts that do not need extra validation

HandleHudImpl is entered through HandleHud's HUD/handled checks. _OnProcessDebugData is called by the component after its subsystem validity check on the ordinary render path. There is no demonstrated concurrent mutation or arbitrary external invocation requiring duplicate validation at each layer. Retain those existing contracts. HUD identity filtering is necessary event routing, not generalized input hardening.

## 4. Implementation Approach

### 4.1 Design Approach

#### 4.1.1 HUD Notification Routing Design

Accept the engine notification only when its Hud argument equals the component's TargetHud. This matches the existing model in which the subsystem attaches one renderer to the selected first-player HUD. Reject foreign notifications before updating, drawing, or expiring layouts. Keep the current null/canvas/subsystem checks and the visibility logic unchanged.

No new ownership protocol, state, timer, retry, or public interface is necessary. Exact HUD identity is sufficient; extra world equality checks would be redundant for this contract. This plan does not add a renderer for every local player or alter first-player selection.

### 4.3 Implementation Targets

| Target | Object | Action | Semantic effect | Responsibility surface | Dependencies | Local verification |
| --- | --- | --- | --- | --- | --- | --- |
| T1 | OnHUDPostRender initial guard | Add Hud != TargetHud.Get() to the rejection condition | Foreign HUD broadcasts cannot enter local processing | Algorithm/Rule | None | Two same-process PIE clients show only their own strings |

- Change Targets and Expected Outcomes: TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugComponent.cpp only; retain the existing void OnHUDPostRender(AHUD* Hud, UCanvas* DebugCanvas) signature. The engine delegate is the caller; no caller migration or header change is needed.
- Core Flow: Reject existing invalid inputs or a foreign HUD; compute existing visibility; invoke existing processing for the matching HUD.
- Failure, Cleanup, and Rollback: Rejection is a no-op. No cleanup behavior changes. If ordinary selected-HUD output regresses, revert only this guard change and inspect the callback identity.

## 8. Task Decomposition

### TASK-001: OnHUDPostRender Filter Foreign HUD Notifications (Source-Code Change)

- Task Readiness: Ready
- Related Decision / Constraint: User's genuine-bugs-only scope; section 2 preservation constraints; section 4.1.1 routing design.
- Related `change point`: T1, callback guard + reject foreign HUD + isolate local processing.
- `Task` Responsibility Boundary: Algorithm/Rule.
- Task Dependencies: None.
- Implementation Card:
  - Exact File, `symbol`, or Target Location: TestCodeDemo/Plugins/EasyDebugString/Source/EasyDebug/EasyDebugComponent.cpp, OnHUDPostRender initial guard at line 88 in the reviewed source.
  - Single `Task` Responsibility: Route only the selected HUD's notification into this component's context processing.
  - Inputs and Outputs: Engine HUD/canvas callback parameters; either no action for a foreign HUD or existing rendering/expiry behavior for the selected HUD.
  - Preconditions and Invariants: Preserve the existing non-null and valid-subsystem checks, drawDebug calculation, and final processing call. TargetHud remains the HUD initialized from the component owner.
  - Ordered Code Steps: Add Hud != TargetHud.Get() to the existing rejection expression. Review the resulting diff to ensure no lifecycle, API, settings, or layout logic changed.
  - Local Validation Method: Run the compile and focused PIE checks in section 9.
  - Redesign Stop Conditions: If current source no longer uses a static event or has already added an equivalent routing filter, reassess rather than duplicate the guard. No stop is required for hypothetical misuse of internal functions.
- Decomposition Rationale: One expression change with one observable behavior; splitting it or creating a separate validation task adds no useful boundary.

## 9. Validation and Regression Checks

Use the smallest checks that cover the changed behavior:

1. Compile TestCodeDemoEditor, Win64, Development using UE 5.8 and the current project. The generated project properties resolve the installed engine to the UE 5.8 installation; the editor target also declares Unreal5_8 include ordering. Use the existing build tooling for that target. One successful relevant build is sufficient.
2. Run two multiplayer PIE clients with Run Under One Process enabled. Write a distinguishable persistent string in each world through the ordinary plugin API. Confirm each viewport shows only its own string and still shows its own output. If practical, observe the same scenario before the fix to establish the visible baseline.
3. In that same PIE run, toggle hiding on and off and confirm drawing stops and resumes. Check the small diff confirms that the accepted-HUD processing call still occurs when drawDebug is false. No expiry-counter instrumentation or additional timing suite is required.

The repository plugin build entrypoint is debugstringbuildtest.bat (`Build/debugstringbuildtest.bat`). It currently targets UE 5.7, so it is not the default validation for this UE 5.8 project change. Running both versions, packaging, or changing the script is unnecessary for this task. A separately requested UE 5.7 compatibility check would use that script from its Build directory, where its output cleanup is expected.

No synthetic reset/replacement ordering, pre-BeginPlay harness, null-input fuzzing, repeated EndPlay dispatch, delegate-count assertions, all-platform matrix, or new automation framework is required. There are no wiring changes that warrant an additional editor-open test beyond the PIE check itself.

Pass criteria: the relevant build succeeds; local output remains visible; foreign-world output is absent; normal hide/show behavior remains intact. Record unavailable checks as not run, rather than expanding the test scope. Runtime/editor execution and compilation were not performed during this planning review.

## 10. Assessment and Recommendations

Disclaimer: This assessment is reading guidance, may be inaccurate, and is not a source of implementation facts.

- Plan Assessment: Low-cost, local correction of a reachable event-routing defect. It preserves the existing lifecycle and internal contracts. The meaningful regression risk is accidentally suppressing matching-HUD processing; the focused PIE check covers that risk.
- Plan Detail Assessment: Ready for independent implementation after authorization. One target maps to one task with an exact method, condition, compatibility boundary, and minimal verification. No unresolved architectural or user-semantic choice remains.
- Current Issues: Runtime confirmation remains pending. The excluded lifecycle concerns are not proven impossible in arbitrary custom code, but no normal-use reproduction justifies including them. The UE 5.7 build-script mismatch is accounted for by using the UE 5.8 editor target, without expanding this repair.
- Recommended Next Step: Implement TASK-001 with cat-code when requested, then run the single relevant compile and focused PIE checks. Reopen the excluded concerns only if a concrete supported call sequence demonstrates a failure.
