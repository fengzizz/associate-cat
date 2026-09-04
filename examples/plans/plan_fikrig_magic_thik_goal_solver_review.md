# FIKRigMagicTHIKGoalSolver Intended-Behavior Review Plan

> **Real project example**
>
> - **Original request:** “Review FIKRigMagicTHIKGoalSolver and generate a comprehensive plan document, disregarding any flaws or defects.”
> - **Project context:** The solver comes from [Anim Retarget Magic](https://www.fab.com/listings/40f149fc-d43c-42ff-a51a-b059ddabeb8f?lang=en), a Fab plugin published by the same author as Associate Cat.
> - **What this demonstrates:** Cat Plan respected an unusual but explicit review boundary, followed the solver only into behavior-defining settings and collaborators, and described the existing system without turning later defect questions into repair work.
> - **Result:** A final intended-behavior model and four validation-only tasks covering transforms, curve delivery, editor and legacy boundaries, and the plugin build boundary.
> - **Validation status:** Source and documentation were inspected. Runtime, editor, asset-conversion, and UAT validation were not run while this Plan was written. `Ready` means the validation tasks are ready to execute.
> - **Publication note:** The Plan was generated from a private working-tree snapshot reviewed on 2026-09-05. Private source links have been converted to reference paths. Later questions about possible defects were handled separately and did not retroactively change this intended-behavior Plan.

## 1. Requirement Correction and Key Assumptions

- **Task Type:** Technical design review and validation planning.
- **Document Status:** Final Snapshot
- **Design Readiness:** Ready
- **Document Output Location:** `examples/plans/plan_fikrig_magic_thik_goal_solver_review.md`
- **Contradictions and Missing Information:** None block this review. The request does not define a new feature or authorize implementation, so this Plan does not invent source-code changes. Runtime assets are binary Unreal assets and were not inspected as source; the validation Tasks therefore define the evidence needed from an editor session.
- **Adopted Interpretation:** Review the current working-tree implementation of `FIKRigMagicTHIKGoalSolver` as an intentional design, capture its complete responsibility and execution model, and define a verification plan. Flaw discovery, defect analysis, remediation, refactoring, optimization, and code-style criticism are excluded even when adjacent documentation mentions them.
- **Assumptions and Impacts:**
  - The primary development copy under `TestCodeDemo/Plugins/AnimRetargetMagic` is authoritative; the `AnimRetargetDemo` copy is a release/demo mirror and is not a separate design source.
  - A private working-tree snapshot, including uncommitted source state, is the review baseline. A later source revision requires a focused re-review of changed symbols before this Plan is used as evidence.
  - Goal names, bone names, slot transforms, and curve names are project-authored configuration. Defaults document the intended starting convention, not a universal skeleton contract.
  - `Design Readiness: Ready` applies to the validation-only Tasks in this Plan. It does not authorize source, asset, configuration, demo, or engine changes.
- **Task Description:** Produce a handoff-quality description and validation plan for the struct-based two-hand IK goal solver, covering configuration ownership, initialization, transform solving, animation-curve input, pose output, editor visualization, and legacy conversion while preserving the current intended behavior.

## 2. Goals and Scope

- **Goals:**
  - Establish one source-grounded model of what the solver owns, consumes, computes, and mutates.
  - Make the transform algorithm and its alpha/curve controls understandable without requiring a line-by-line source walk.
  - Record the integration contracts that determine correct use: solver order, curve delivery, settings-asset assignment, required goals, bone caches, and editor-only debug routing.
  - Provide deterministic validation-only Tasks whose evidence can confirm the intended behavior without changing the project.
- **In Scope:**
  - The declarations and implementation of `FIKRigMagicTHIKGoalSolver`, `FIKRigMagicTHIKGoalSettings`, and the legacy `UIKRig_MagicTHIKGoalSolver` conversion bridge.
  - The tightly coupled types in `UMagicTHIKG_SettingsAsset`: goal, slot, retarget, blend, output-bone, and snap-bone settings.
  - Direct runtime collaborators that determine algorithm or integration behavior: `FIKRigMagicSolverBase`, `FMagicMath::TransformSegmentEndNoScale`, `FMagicSolverUtilities::GatherAllChilds`, `IMagicWantsAnimCurves`, `IMagicAnimCurvesCache`, `FMagicCacheAnimCurvesOp`, and `FMagicRetargetUtils::PushAllAnimCurves`.
  - The editor-facing settings wrapper, debug-draw discovery path, and legacy-to-`FInstancedStruct` conversion contract.
  - Existing project build and manual editor-validation entry points.
- **Out of Scope:**
  - Identifying, ranking, or correcting flaws and defects.
  - Source refactors, API redesign, settings migration changes, performance work, warning/error policy changes, naming cleanup, or test implementation.
  - Editing `.uasset` content, demo-project copies, generated/transient Unreal directories, packaging output, root release scripts, or Unreal Engine source.
  - Broader batch-retarget, root-motion, attach-preview, or generic modify-solver behavior except where it directly feeds or observes this solver.
- **Notes:** The solver is intended for two-hand weapon retargeting and is documented to run before downstream solvers such as Full Body IK that consume the rewritten goals or helper bones.

## 3. Preliminary Analysis

This section contains current facts and constraints that shape the intended-design model and its validation path.

- **User-Specified Analysis Scope:** `FIKRigMagicTHIKGoalSolver`.
- **Additional Scope Identified by AI:**
  - The settings asset and nested setting structs, because they own every behavior-relevant input used by the solver.
  - The magic solver base and utility math, because they define discovery hooks, display metadata, child-cache construction, and left-hand reconstruction.
  - The curve cache/dispatch path, because curve-driven blending is external input delivered before solving.
  - The editor settings wrapper, preview debug bridge, and legacy solver, because they constrain authoring, observability, and compatibility.
  - The plugin descriptor, module rules, KnowledgeTree entry, focused solver documentation, and closest build script, because they define module boundaries and validation entry points.
- **Scope Rationale:** The analysis starts at the solver and performs a bounded upward pass to the settings and base contracts that own its state, then follows only direct inputs and observers that can change how the solver is configured, invoked, or validated. Demo mirrors and unrelated retarget operations cannot change this review's design conclusions.
- **Analysis Depth:**
  - **Decisive:** `FIKRigMagicTHIKGoalSolver`, its settings struct, and `UMagicTHIKG_SettingsAsset`; together they define the execution skeleton, mutable state, and all configuration-driven branches.
  - **High:** `Solve`, `Initialize`, `InitBoneCache`, `TransformSegmentEndNoScale`, curve delivery, and pose-write helpers; these define ordering, data flow, and observable results.
  - **Supporting:** the magic solver base, editor wrapper, debug draw path, legacy conversion, plugin/module declarations, and build script; these establish integration and validation boundaries without changing solve math.
  - **Excluded after boundary confirmation:** the demo plugin copy, unrelated retarget operations, and generated artifacts.
- **Current Facts and Constraints:**
  - Primary evidence anchors: `TestCodeDemo/Plugins/AnimRetargetMagic/Source/AnimRetargetMagic/IKRigSolvers/IKRig_MagicTHIKGoalSolver.h`, its `.cpp` and types header, `IKRigMagicSolverBase.h`, `IKRigMagicUtilities.cpp`, `MagicCacheAnimCurvesOp.cpp`, `MagicRetargetUtils.cpp`, the editor-side `MagicIKRigStructWrappers.cpp`, and `Build/animretargetmagicbuildtest.bat` in the private source project.
  - `FIKRigMagicTHIKGoalSolver` is a reflected `USTRUCT` derived from `FIKRigMagicSolverBase` and participates in Unreal's `FIKRigSolverBase` lifecycle through `Initialize`, `Solve`, required-bone/goal reporting, and solver-settings accessors.
  - `FIKRigMagicTHIKGoalSettings` holds a `TObjectPtr<UMagicTHIKG_SettingsAsset>` as the normal authoring reference, a transient legacy bridge pointer, a description inherited from the magic settings base, and editor-preview display controls.
  - `UMagicTHIKG_SettingsAsset` owns the right/left goal names, three slot transforms, two retarget-alpha pairs, optional curve blend, three IK output-bone settings, and one optional right-weapon snap bone.
  - Default conventions are `hand_r_Goal`, `hand_l_Goal`, `ik_hand_r`, `ik_hand_l`, and `ik_hand_gun`; left-palm translation defaults to full retarget, left-palm rotation defaults to zero, and left-hand goal translation/rotation default to full retarget.
  - Initialization resolves bone names to indices, gathers descendants only when propagation is configured, registers only the configured blend curve in the solver-local cache, and can accept legacy converted settings before cache construction.
  - The solve path mutates two output channels: the left-hand `FIKRigGoal` in the goal container and configured transforms in `FIKRigSkeleton::CurrentPoseGlobal`/`CurrentPoseLocal`.
  - Animation curves are not read directly by the solver. `FMagicCacheAnimCurvesOp` obtains source values and `FMagicRetargetUtils::PushAllAnimCurves` dispatches the cache to magic solvers implementing `IMagicWantsAnimCurves`.
  - Debug rendering is editor-only and converts cached component-space goal results into world space before drawing the weapon and left-palm coordinate systems.
  - Both plugin modules are declared for Win64, the runtime module publicly depends on `IKRig`, and the closest repository build entry is `Build/animretargetmagicbuildtest.bat`.
- **Existing Mechanisms:**
  - **Configuration mechanism:** a small solver settings struct references a reusable primary data asset; transient caches stay on the solver or nested bone settings and are rebuilt from the active skeleton.
  - **Legacy bridge:** `UIKRig_MagicTHIKGoalSolver::ConvertToInstancedStruct` creates the struct solver, transfers description/root metadata plus both goal names, the three hand/palm slot settings, both retarget-alpha groups, curve-blend settings, and all three IK output-bone settings into a temporary `UMagicTHIKG_SettingsAsset`, and makes that asset available during initialization.
  - **Reference construction:** the right-hand goal is the driving global frame. `RightHandRightWeapon` creates the weapon output frame and `RightHandLeftPalm` creates the desired off-hand palm reference.
  - **Left-hand reconstruction:** `LeftHandPalm` maps the current left-hand goal to its palm frame; per-channel palm alphas define the desired palm; `TransformSegmentEndNoScale` reconstructs the left-hand goal while preserving the original palm-to-hand segment relationship.
  - **Runtime blend:** the reconstructed goal is applied per translation/rotation channel and optionally scaled by a named source-animation curve, its curve mode, and its scalar value.
  - **Pose publication:** the weapon, right-hand, and final left-hand transforms are written to their configured IK helper bones. Each output independently controls local-transform refresh and descendant-local refresh.
  - **Observability:** editor builds cache final goal transforms during solve and expose them through `IMagicDrawDebugInfo` to the attach-preview/debug path.
- **Reuse / Preserve / Avoid:**
  - Reuse the settings asset as the single behavior configuration source, the solver lifecycle supplied by `FIKRigSolverBase`, existing utility math, and existing curve/debug discovery interfaces.
  - Preserve the right-hand-driven frame hierarchy, per-channel alpha semantics, curve multiplier order, dual goal/bone output, bone-cache initialization, solver ordering contract, and editor/runtime separation.
  - Preserve legacy conversion as a compatibility boundary without treating the legacy UObject solver as the active runtime implementation.
  - Avoid interpreting this review as approval to change source behavior, duplicate the demo implementation, bypass the curve cache path, or patch Unreal Engine code.
- **Risks and Limitations:**
  - Behavioral validation is configuration-sensitive: incorrect goal names, bone names, slot transforms, solver ordering, or absent curve delivery can produce different results without changing solver code.
  - The current repository is a dirty working tree; validation evidence must record the exact source revision or working-tree state used.
  - Static review can establish code contracts but cannot prove visual grip quality, editor asset conversion, or frame-by-frame curve response. Those require Unreal Editor execution and representative assets.

## 4. Implementation Approach

- The following design is the intended-behavior baseline derived from the Preliminary Analysis. It plans verification of the existing system and introduces no source change point.

### 4.1 Design Approach

The solver remains a narrow IKRig stage: an external settings asset supplies authoring data; initialization converts names into runtime caches; each solve derives an off-hand correction from the dominant-hand reference; optional curve input scales that correction; final goals and helper bones are published for downstream IK; editor-only code renders diagnostic frames. Validation should observe those boundaries independently before exercising the full integrated retarget flow.

#### 4.1.1 Configuration Ownership Design

`FIKRigMagicTHIKGoalSettings` is the host-facing settings envelope and `UMagicTHIKG_SettingsAsset` is the behavior configuration owner. Runtime caches remain derived state and must be interpreted against the skeleton passed to `Initialize`. The legacy UObject solver is a one-way conversion source for current struct-based storage, not a parallel runtime algorithm.

#### 4.1.2 Initialization and Cache Design

Initialization must precede solve-based evidence collection. It first accepts any legacy-converted temporary settings, then resolves the three IK output bones and optional weapon snap bone. Descendant indices are gathered only for output settings with propagation enabled. The curve cache is reduced to the one configured blend curve so curve delivery copies only solver-relevant data.

#### 4.1.3 Two-Hand Transform Design

The right-hand goal establishes the shared frame for weapon and off-hand alignment. The solver computes the current left-palm frame from the left goal, blends that frame toward the right-hand-authored left-palm slot per translation and rotation channel, and reconstructs the desired left goal from the resulting palm frame. This keeps slot authoring separate from the live retargeted goal transforms and makes translation and rotation control independent.

#### 4.1.4 Curve-Controlled Retarget Design

Curve control is an optional multiplier applied after the desired left goal has been reconstructed and before per-channel goal interpolation. `Default` uses the cached curve value; `OneMinus` inverts it; the result is multiplied by the configured scalar and clamped for application. Live and batch-retarget validation must prime the external curve cache before the solver executes.

#### 4.1.5 Pose Publication Design

The solver publishes the weapon slot to the gun helper bone, the unmodified right goal to the right-hand helper bone, and the final left goal to the left-hand helper bone. Each write updates global rotation and location; configured local refresh makes the modified global pose coherent for later local-space consumers, while optional descendant refresh preserves the existing descendants' global placement semantics.

#### 4.1.6 Compatibility and Editor Observation Design

The current struct solver remains the runtime authority. The legacy solver only supplies converted settings, the editor wrapper exposes the struct settings in details panels, and debug rendering observes cached solve outputs without participating in pose computation. Validation must keep runtime solve evidence separate from editor-only visualization evidence.

### 4.3 Implementation Targets

- **Change Targets and Expected Outcomes:** No source, asset, configuration, demo, or engine changes are planned. The executable targets are validation-only: confirm configuration/cache construction, transform results, curve modulation, IK-bone publication, editor visualization, and legacy conversion against the current working-tree behavior.
- **Core Flow:** Configure settings asset → initialize solver caches → prime optional curve data → solve right-hand reference and left-hand reconstruction → write helper bones → allow downstream IK → render editor diagnostics when enabled.
- **Data and State Flow:**
  - Authoring data: `UMagicTHIKG_SettingsAsset`.
  - Upgrade-only data: legacy `UIKRig_MagicTHIKGoalSolver` fields transferred through `ConvertToInstancedStruct`.
  - Derived initialization state: cached bone indices, descendant arrays, and registered curve keys.
  - Per-frame input state: goal-container transforms and externally supplied curve values.
  - Per-frame output state: mutated left goal, updated IK helper-bone transforms, cached editor debug transforms, and reset solver-local curve values after a completed solve.
- **Failure, Cleanup, and Rollback:** Validation must treat early exit or skipped optional output as observable configured behavior and must not repair it during the task. Validation Tasks have an empty modification scope, so cleanup consists only of closing the editor without saving test-only asset changes or using disposable duplicated assets. Rollback is not applicable because this Plan makes no project changes.

## 5. Architecture Details

- **Architecture Objects and Change Points:** There are no architecture change points. Reviewed objects are the struct solver, settings envelope, settings asset, utility math, curve delivery interfaces/op, editor wrapper/debug bridge, and legacy conversion class.
- **Responsibility, Rule, or Structural Boundaries:**
  - Unreal `IKRig` owns solver invocation, skeleton pose storage, goal containers, and downstream solver sequencing.
  - `FIKRigMagicTHIKGoalSolver` owns two-hand correction, solver-local caches, and pose publication.
  - `UMagicTHIKG_SettingsAsset` owns author-authored behavior parameters.
  - `FMagicCacheAnimCurvesOp` owns source curve acquisition; `FMagicRetargetUtils` owns dispatch; the solver owns only copied values for registered curve names.
  - The editor preview path owns debug draw scheduling; the solver only supplies drawing behavior and cached transforms.
- **Dependencies, Integration, and Information Flow:**
  - `FIKRigGoalContainer` → goal lookup → right/left global inputs.
  - Settings slots + right/left goals → palm/weapon frames → reconstructed left goal.
  - Curve cache op → retarget utility dispatch → `OnUpdateAnimCurves` → scalar blend input.
  - Solver outputs → `FIKRigSkeleton` and mutable left goal → downstream IK solver consumption.
  - Solver cached transforms → `IMagicDrawDebugInfo` discovery → world-space editor coordinate rendering.
- **Migration, Compatibility, and Rollback:** Legacy assets are expected to convert from `UIKRig_MagicTHIKGoalSolver` to an `FInstancedStruct` containing `FIKRigMagicTHIKGoalSolver`; the current conversion contract transfers the field groups enumerated in the Preliminary Analysis, and the current struct path is the post-conversion runtime path. The review proposes no migration or rollback operation.
- **Design Rationale:** The architecture isolates reusable authoring data from runtime caches, uses the engine's solver and pose containers directly, and supplies plugin-specific curve/debug capabilities through small interfaces rather than making those concerns part of the transform algorithm.

## 6. Algorithm Details

- **Steps:**
  1. Reject solve evaluation when no active settings asset is available; otherwise rebuild derived bone/curve caches when requested.
  2. If the optional snap bone resolved, replace its local translation and rotation with the configured right-weapon slot and update its global transform.
  3. Resolve the configured right and left goals and construct their current global transforms from final blended position/rotation.
  4. Compose `RightHandRightWeapon * RightHandGoal` to obtain the weapon output frame.
  5. Compose `RightHandLeftPalm * RightHandGoal` to obtain the reference palm frame and `LeftHandPalm * OldLeftHandGoal` to obtain the current palm frame.
  6. When left-hand retargeting is active, interpolate current palm translation/rotation toward the reference palm with `LeftPalmRetarget` alphas.
  7. Reconstruct the desired left-hand goal by transporting the original palm-to-hand segment to the desired palm frame through `FMagicMath::TransformSegmentEndNoScale`.
  8. Derive the optional runtime blend from the cached curve, curve mode, and scalar value; clamp it to `[0, 1]`.
  9. Interpolate the left goal's final blended translation/rotation toward the desired goal using `LeftHandGoalRetarget` alphas multiplied by the runtime blend.
  10. Publish weapon, right-goal, and final left-goal transforms to the configured IK helper bones, refreshing local transforms and descendant locals according to each output setting.
  11. Cache the final goals for editor debug rendering and clear copied curve values after the completed solve pass.
- **Parameters and Defaults:**

  | Parameter group | Role | Default convention |
  | --- | --- | --- |
  | `RightHandGoal`, `LeftHandGoal` | Goal-container lookup keys | `hand_r_Goal`, `hand_l_Goal` |
  | `RightHandRightWeapon` | Weapon frame in right-hand space | Identity transform |
  | `RightHandLeftPalm` | Desired left-palm frame in right-hand space | Identity transform |
  | `LeftHandPalm` | Palm frame in left-hand space | Identity transform |
  | `LeftPalmRetarget` | Palm-frame channel interpolation | Location `1`, rotation `0` |
  | `LeftHandGoalRetarget` | Final left-goal channel interpolation | Location `1`, rotation `1` |
  | `LeftHandGoalRetargetBlend` | Optional curve multiplier | Disabled; scalar `1`; `Default` mode |
  | `IKBoneHandGun`, `IKBoneHandR`, `IKBoneHandL` | Pose publication targets | `ik_hand_gun`, `ik_hand_r`, `ik_hand_l` |
  | `SnapWeaponR` | Optional local-space right-weapon reference override | No bone |
  | `bDrawDebugInfo`, `DebugInfoScale` | Editor diagnostics | Enabled; scale `1` |

- **Boundaries and Fallbacks:**
  - No settings asset or unresolved required goal prevents the main solve from producing further results for that pass.
  - An unresolved optional output bone skips only that output write; curve blending can be disabled independently of geometric reconstruction.
  - Translation and rotation alphas are independent at both the palm-alignment and final-goal stages.
  - Scale does not participate in the segment reconstruction result; the desired palm frame is normalized to unit scale.
  - Missing cached curve data contributes a zero curve value before `OneMinus` handling and scalar multiplication.
  - Debug drawing requires an editor build, a target skeletal mesh component, an active settings asset, enabled drawing, and a positive debug scale.

## 8. Task Decomposition

These are independent validation Tasks. They authorize no modifications and do not implement or remediate solver behavior.

### TASK-001: FIKRigMagicTHIKGoalSolver Validate Core Transform and Pose Publication

- **Task Readiness:** Ready
- **Related Decision / Constraint:** Sections 1 and 2: intended-behavior review; defects and project changes are out of scope.
- **`Task` Responsibility Boundary:** Validation
- **Task Dependencies:** None.
- **Authorized Change Scope:** Empty.
- **Validation Card:**
  - **Validation Target:** Initialization caches, right-hand reference construction, left-palm alignment, reconstructed left goal, optional snap stage, and three IK helper-bone outputs.
  - **Preconditions:** A disposable or unsaved IK Rig/Retargeter setup with resolvable right/left goals, configured slot transforms, configured helper bones, this solver ordered before the downstream IK consumer, and debug drawing available for visual cross-checks.
  - **Execution Steps:** Establish a baseline pose; exercise identity slots; add a non-zero right-hand weapon slot; add a non-zero right-hand left-palm slot; test translation-only, rotation-only, zero, and full alpha combinations; enable each output-bone local/descendant option in isolation; optionally configure the snap bone; inspect the goal and pose results after each run.
  - **Expected Evidence:** Captured goal transforms and bone transforms show the right goal as the reference, the weapon frame composed from the right slot, the left goal reconstructed from palm alignment, and each helper bone matching its assigned source transform.
  - **Pass Criteria:** Every observed transform and per-channel blend matches the Algorithm Details, disabled/optional paths remain isolated, and downstream IK consumes the published result in the documented solver order.
  - **Redesign Stop Conditions:** Any observed result that cannot be explained by current source and the recorded configuration; stop validation and return to planning without changing code or assets.

### TASK-002: Curve Delivery Validate Runtime Blend Control

- **Task Readiness:** Ready
- **Related Decision / Constraint:** Sections 3 and 4: curves are external per-frame inputs delivered before solve.
- **`Task` Responsibility Boundary:** Validation
- **Task Dependencies:** TASK-001.
- **Authorized Change Scope:** Empty.
- **Validation Card:**
  - **Validation Target:** `FMagicCacheAnimCurvesOp` → `FMagicRetargetUtils::PushAllAnimCurves` → `FIKRigMagicTHIKGoalSolver::OnUpdateAnimCurves` → final left-goal blend.
  - **Preconditions:** A disposable retarget setup containing the curve-cache op, a source animation with a known curve, enabled `LeftHandGoalRetargetBlend`, and a solver settings asset referencing that exact curve name.
  - **Execution Steps:** Evaluate known curve samples at `0`, `0.5`, and `1`; repeat with `Default` and `OneMinus`; repeat with scalar values below and above `1`; compare live preview and editor batch-retarget sampling without saving generated test artifacts.
  - **Expected Evidence:** The final left-goal correction follows the curve-mode/scalar product and application clamp, and new curve data is delivered before the corresponding solve.
  - **Pass Criteria:** Observed blend values and left-goal transforms match the documented formula in each sample for both supported evaluation paths.
  - **Redesign Stop Conditions:** Curve values or ordering cannot be correlated with the current cache/dispatch path; stop and capture the evaluation context for a new planning pass.

### TASK-003: Editor and Legacy Boundaries Validate Authoring, Debug, and Conversion

- **Task Readiness:** Ready
- **Related Decision / Constraint:** Sections 3 and 4: the struct solver is the runtime authority; wrapper, debug, and legacy paths are boundary integrations.
- **`Task` Responsibility Boundary:** Validation
- **Task Dependencies:** TASK-001.
- **Authorized Change Scope:** Empty.
- **Validation Card:**
  - **Validation Target:** Struct settings exposure, nice-name description, editor debug coordinate systems, and conversion from the legacy solver into the struct-based solver.
  - **Preconditions:** Unreal Editor can load `TestCodeDemo`; representative disposable assets are available; no source, engine, or tracked asset save is permitted.
  - **Execution Steps:** Open the struct solver settings wrapper; verify the settings-asset and preview controls are exposed; set a description and observe the display name; enable debug draw and compare world-space weapon/palm frames with TASK-001 transforms; duplicate a representative legacy asset in a disposable location, trigger conversion, and compare the explicitly transferred metadata, goal, slot, retarget, curve-blend, and IK output-bone field groups plus their runtime outputs.
  - **Expected Evidence:** Editor exposure edits the viewed struct, debug frames represent cached solve output, and the converted solver runs through `FIKRigMagicTHIKGoalSolver` with behavior equivalent to the legacy configuration.
  - **Pass Criteria:** Authoring, display, debug, and conversion evidence align with the documented responsibility split without requiring a project change.
  - **Redesign Stop Conditions:** Conversion or editor behavior requires saving tracked content, changing source, or choosing new migration semantics; return to planning for explicit authorization.

### TASK-004: AnimRetargetMagic Validate Build Boundary

- **Task Readiness:** Ready
- **Related Decision / Constraint:** Repository validation rules and the `AnimRetargetMagic` module/plugin boundary.
- **`Task` Responsibility Boundary:** Validation
- **Task Dependencies:** None.
- **Authorized Change Scope:** Empty.
- **Validation Card:**
  - **Validation Target:** The primary plugin copy packages successfully with its declared runtime/editor modules and `IKRig` dependency.
  - **Preconditions:** A Windows host with the engine path required by `Build/animretargetmagicbuildtest.bat`, a clean disposable packaging-output location, and no need to edit the script.
  - **Execution Steps:** Run the closest repository build entry unchanged; retain the UAT result and relevant compiler/UHT diagnostics; do not modify generated output or sync it into the demo.
  - **Expected Evidence:** A successful BuildPlugin/UAT result for the current primary plugin source.
  - **Pass Criteria:** UAT exits successfully and both declared plugin modules compile for the configured Win64 engine environment.
  - **Redesign Stop Conditions:** The required engine version/path is unavailable or the command would require changing maintained scripts; record the environment gap rather than modifying scope.

## 9. Validation and Regression Checks

- **Minimum Validation Steps:**
  1. Perform TASK-004 as the static compile/UHT boundary check.
  2. Perform TASK-001 with a known two-hand pose and record goal/helper-bone transforms.
  3. Perform TASK-002 when curve-controlled blending is part of the intended asset workflow.
  4. Perform TASK-003 when editor visualization or legacy asset conversion is part of the release/compatibility claim.
- **Key Regression Points:** Right-hand-driven reference hierarchy; independent translation/rotation alphas; curve delivery before solve; dual goal and helper-bone outputs; local/descendant refresh semantics; solver order before downstream IK; editor-only debug observation; legacy conversion into the struct runtime path.
- **Independent Tracking Item:** `VAL-THIK-001` — retain the exact source revision/working-tree description, settings asset values, skeleton/goal names, solver order, curve samples, captured transforms, editor version, and UAT result used for validation.
- **Exclusions:** No validation was executed while authoring this Plan. No tracked assets were opened or saved, no package was generated, and no demo sync or release verification was performed.

## 10. Assessment and Recommendations

- **Disclaimer:** “Assessment and Recommendations” is guidance for reading the current Plan. It may be inaccurate and must not be used as a factual basis during development.
- **Plan Assessment:** Feasibility is high for static understanding and moderate for runtime proof. The source, focused docs, direct collaborators, and build entry provide a complete design model; runtime proof requires representative Unreal assets and a compatible Windows editor/toolchain. Execution cost is moderate because transform, curve, editor, conversion, and build evidence require distinct setups. The main operational risks are configuration-sensitive observations, solver-order mistakes, and losing traceability to the dirty working-tree baseline; the validation cards address each with explicit preconditions and evidence.
- **Plan Detail Assessment:** The validation-only scope is ready for an independent AI or developer. Each Task identifies its target, prerequisites, steps, evidence, pass criteria, empty modification scope, and stop condition. The Plan intentionally contains no source implementation Task because the user requested review/planning and excluded defect work.
- **Current Issues:** No unresolved design choice or missing source fact affects the review baseline. Runtime/editor/build evidence remains uncollected, and binary project assets were not inspected; these are declared validation exclusions rather than inferred defects.
- **Recommended Next Step:** Use this Final Snapshot as the intended-behavior reference. If empirical confirmation is needed, execute TASK-004 and TASK-001 first, then run TASK-002 and TASK-003 only for the claims relevant to the target workflow. Any request to change behavior, repair a defect, refactor, or save assets must start a separately authorized planning or implementation scope.
