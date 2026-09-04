# UE 5.8 Sphere Sweep Contact Offset Plan

> **Real project example**
>
> - **Original request:** “There is a collision detection bug in the UE5.8 engine. When performing a trace using a sphere, if the trace hits the spherical shape configured within a character's physics body, the calculated collision point is incorrect—there is an offset from the actual position. Please investigate this bug and draft a document outlining a fix plan.”
> - **Follow-up:** “Check the source code directly.”
> - **What this demonstrates:** Cat Plan traced a visible `ImpactPoint` symptom through the public query stack into Chaos, separated a confirmed coordinate-space defect from scene-specific possibilities, and kept production integration out of the ready work until the caller and Physics Asset are known.
> - **Result:** A source-backed diagnosis for the inspected UE 5.8 build, a deterministic numeric fixture, an upstream repair specification, an independent oracle, and two ready project-owned diagnostic tasks.
> - **Validation status:** Installed source inspection and an independent analytic calculation were completed. No compiled Chaos query, character scene, engine patch, or project change was run. `Partially Ready` is therefore the intended and accurate result.
> - **Publication note:** Machine-specific paths and internal project links have been normalized. A short Unreal Engine source excerpt has been replaced with equivalent pseudocode; the diagnosis and calculations are unchanged.

## 1. Requirement Correction and Key Assumptions

- Task Type: Bug investigation and fix planning.
- Document Status: Draft
- Design Readiness: Partially Ready
- Document Output Location: `examples/plans/plan_ue58_sphere_sweep_contact_offset.md`
- Task Description: Investigate an incorrect sphere-trace contact against a sphere in a character Physics Asset, identify the responsible UE 5.8 calculation, and define a repair and regression strategy.
- Adopted Interpretation: “Collision point” means `FHitResult.ImpactPoint`, the contact on the collision geometry. `Location` is the swept sphere's center at impact. A Physics Asset primitive need not coincide with the rendered skin. These distinctions must be checked in the reported scene.
- Contradictions and Missing Information: The failing Blueprint/C++ caller, character/Physics Asset, trace parameters, body transform, and raw hit are unspecified. Source inspection proves a relevant defect in the installed engine; it does not prove that this is the only cause of the user's scene symptom.
- Assumptions and Impacts: The primary reproduction uses a finite, nonzero-length sphere sweep, simple collision, rigid body transforms, unit scale, and no initial overlap. This isolates the geometry defect. Other cases require the separate checks below.

Investigation date: 2026-09-05. Verified engine: **5.8.0, changelist 55116800, branch `++UE5+Release-5.8`**, recorded in `Engine/Build/Build.version`. The project association and generated `UECommon.props` led to this installation; its source and build entrypoint were verified on disk.

## 2. Goals and Scope

- Goals: Explain the offset with source evidence, give a deterministic reproduction, specify the mathematically correct repair, and identify the minimum safe next implementation work.
- In Scope: Sphere-versus-sphere scene sweeps; Physics Asset sphere construction; shape transforms; Chaos specialized dispatch and narrow phase; conversion to `FHitResult`; project-owned diagnostic tests; feasibility of a stock-engine workaround.
- Out of Scope: Editing engine files; implementing this plan in this planning turn; changing either plugin or demo; changing gameplay collision shapes without a confirmed requirement; general Chaos refactoring; physical contact-solver/CCD fixes; patching unrelated primitive combinations.
- Notes: The private project's repository rules make the installed engine read-only. The engine correction below is an upstream repair specification, not an authorized repository modification. Any project workaround must preserve the actual caller's filtering, hit ordering, and body identity.

## 3. Preliminary Analysis

### 3.1 Finding and evidence boundary

**The installed specialized sphere-versus-sphere sweep omits rotation of nonzero local sphere centers.** A sphere is rotationally symmetric about its own center; its center's offset from the body origin still rotates with the body. The affected code treats those two facts as interchangeable.

The main evidence is the locally inspected `Engine/Source/Runtime/Experimental/Chaos/Private/Chaos/GeometryQueries.cpp`, `Chaos::SweepSphereVsSphere`, lines 70–85. Its SHA-256 at inspection was `7814bd56782ba088e8fc5b6d05497090fa46b8ae979f6962244289218b06e16f`.

The decisive coordinate preparation is represented here as equivalent pseudocode:

```text
target translation = target transform translation
swept translation = swept transform translation
local sweep direction = world sweep direction
local sweep start = swept sphere local center + swept translation - target translation
```

Line 82 then supplies the unrotated `TestSphere.GetCenterf()` as the target center. Line 83 restores the output using only `TestGeomLocalToWorld`. There is no call applying either sphere body's rotation in this function. This diagnosis follows directly from installed source; the external report below is corroboration.

Epic's support discussion independently identifies the same local-center rotation defect and warns that a center-relative repair must also change the origin used to restore the world contact. The discussion's initial claim that 5.8 was fixed was subsequently retracted; it provides no verified release changelist to adopt. The local build must therefore be assessed from its actual source and behavior. [Epic support discussion](https://forums.unrealengine.com/t/sphere-trace-for-sphere-primitives-in-skeletal-meshes/2743932)

Evidence levels:

| Evidence | Result |
| --- | --- |
| Installed source inspection | Defective coordinate handling confirmed in CL 55116800. |
| Independent analytic calculation | Reproduces a 25.54 cm contact error for the fixture in section 3.5. |
| Compiled Chaos/world-query execution | Not performed in this planning turn. |
| User's specific character scene | Not reproduced; caller and asset remain unidentified. |

### 3.2 Geometry query ownership and propagation

The smallest cohesive analysis unit is `Chaos::SweepQuery` plus its specialized dispatch, sphere narrow phase, and result transformation. This unit has Very High relevance because input transformation and output restoration must agree. Shape construction and `FHitResult` conversion have High relevance; Blueprint/world dispatch and build wiring have Medium relevance. The two plugins are excluded: the searched project/plugin C++ contains no matching sphere trace, `SweepSingle`, `SweepMulti`, or `ImpactPoint` consumer.

The inspected path is:

1. `UKismetSystemLibrary::SphereTraceSingle` in `Engine/Source/Runtime/Engine/Private/KismetSystemLibrary.cpp` calls `UWorld::SweepSingleByChannel` with `FCollisionShape::MakeSphere`.
2. `UWorld::SweepSingleByChannel` in `Engine/Source/Runtime/Engine/Private/Collision/WorldCollision.cpp` selects `GeomSweepSingle`; a nearly zero shape instead takes a line-trace path.
3. `Engine/Source/Runtime/PhysicsCore/Public/SQVisitor.h` applies shape filtering, then calls `SweepQuery` with geometry, actor transform, query transform, direction, and length. Raycasts use a separate path that transforms the ray into body-local space.
4. `SweepQuery` in `Engine/Source/Runtime/Experimental/Chaos/Public/Chaos/GeometryQueries.h` unwraps transformed targets and tries the specialized implementation before the generic fallback. The sphere/sphere pair dispatches directly to the defective function. No disabling switch is present in this inspected dispatch.
5. `Chaos::Sweeps::SweepSphereVsSphere` in `Engine/Source/Runtime/Experimental/ChaosCore/Private/Chaos/Sweeps.cpp` calls `RaySphere` with the swept radius as thickness and optionally computes minimum translation distance (MTD).
6. `ConvertQueryImpactHitImp` in `Engine/Source/Runtime/Engine/Private/Collision/CollisionConversions.cpp` derives `Time`, `Distance`, and `Location` from the returned sweep distance, and assigns the returned physics position to `ImpactPoint`. It cannot reconstruct a correct collision from an incorrect narrow-phase result.

Constraint: A late offset on `ImpactPoint` cannot repair a missed hit, a wrong nearest blocker, an incorrect time, or an incorrect normal. This is a query geometry defect, not principally a hit-result formatting defect.

### 3.3 Why Physics Asset spheres expose it

`ChaosInterfaceUtils.cpp` creates a Chaos sphere using `ScaledSphereElem.Center` and its scaled radius. The center stays inside the sphere geometry and can be nonzero. `FKSphereElem::GetFinalScaled` in `BodySetup.cpp` applies the relative transform and scale to the center; radius follows the engine's minimum absolute scale convention. A repair must not apply component scale a second time or assume radius uses maximum scale.

In contrast, a normal `FCollisionShape` sphere query is constructed with local center zero in `ChaosEngineInterface.cpp`. Thus the target body's offset/rotation is the main suspect for the reported character case. A nonzero swept center also matters for direct geometry queries.

### 3.4 Coordinate defect and contact semantics

Let `cA`, `cB` be the target and swept sphere's local centers, and let body transforms have translations `tA`, `tB` and rotations `RA`, `RB`. Correct world centers are:

```text
CA = tA + RA * cA
CB = tB + RB * cB
```

The installed code instead solves with swept start `cB + tB - tA`, target center `cA`, and world sweep direction. After adding `tA` to the result, this is equivalent to testing centers `tB + cB` and `tA + cA`. It drops both rotations. The effective target-center error is `cA - RA*cA`; it is zero for a zero center or a rotation that leaves the offset unchanged.

`RaySphere` in `Engine/Source/Runtime/Experimental/ChaosCore/Public/Chaos/Raycasts.h` computes the first intersection against the sum of radii and returns a contact on the target surface. With the wrong center, even a numerically correct quadratic produces the wrong answer. This explains pose-dependent offsets and possible misses without assuming floating-point failure.

`FHitResult` in `Engine/Source/Runtime/Engine/Classes/Engine/HitResult.h` distinguishes the swept center (`Location`) from surface contact (`ImpactPoint`). For a separated sphere/sphere hit, they differ by the query radius along the normal. Initial penetration follows a different contract; it has no unique entry contact and must not be evaluated with the separated-contact oracle.

### 3.5 Bug Diagnosis and Fix Plan

**Reproduction Conditions / Trigger Probability:** Use one query-enabled sphere with local center `(20,0,0)` cm, radius `30` cm, body translation `(0,0,0)`, rotation `+90°` around Z, and unit scale. Sweep radius `5` cm from `(-100,20,0)` to `(100,20,0)`. Use simple collision, no starting overlap, and ensure other components—especially the character capsule—do not block the test channel. The geometry calculation is deterministic; the user's scene trigger frequency is unknown.

**Actual Behavior vs. Expected Behavior:** The physical sphere center is `(0,20,0)`; the defective branch calculates against `(20,0,0)`.

| Quantity | Correct analytic result | Arithmetic of installed branch |
| --- | --- | --- |
| Hit distance, cm | `65` | `91.2771868` |
| Normalized trace time | `0.325` | `0.4563859` |
| Swept center at hit | `(-35,20,0)` | `(-8.7228132,20,0)` |
| Surface contact | `(-30,20,0)` | `(-4.6195542,17.1428571,0)` |
| Normal | `(-1,0,0)` | `(-0.8206518,0.5714286,0)` |

The computed contact error is `25.5407575` cm. The faulty point lies only `5.4317167` cm from the actual target center, despite its `30` cm radius. These are independently calculated predictions, not captured Unreal runtime results.

**Root-Cause Hypotheses**, ordered by supporting evidence:

1. Missing local-center rotation: confirmed source defect and strongest match when offset changes with bone/body orientation.
2. Using `Location` as a surface point: check the actual consumer; expected separation is approximately the query radius in the isolated case.
3. Initial overlap or hitting the character capsule/another body: inspect penetration flag, component, bone, and collision filters.
4. Physics/render pose mismatch or incorrect scaling in a custom consumer: investigate only if authoritative query geometry fails to explain the observation.

**Minimal Fix Point:** Correct the coordinate preparation and matching output origin within `Chaos::SweepSphereVsSphere`; leave the lower quadratic and hit conversion unchanged. Because engine edits are prohibited here, this is an upstream specification. Project integration remains conditional on section 4.4.

**Regression Risks and Observation Points:** Both operands can have offsets; output conversion must use the same origin as input conversion; initial overlap/MTD and shape scaling have different semantics. Neighboring sphere/capsule/box routines also contain offset-sensitive logic, so switching primitive types is not automatically a verified remedy.

## 4. Implementation Approach

### 4.1 Design Approach

#### 4.1.1 Sphere Coordinate Frame Design

The repair specification uses a world-axis-aligned frame whose origin is the target sphere's actual world center. Transform both local centers by their owning rigid transforms, subtract the target world center from the swept center, and perform the existing sphere/sphere calculation with target center zero. Keep the direction in world axes. Restore the output position by adding the target world center; do not rotate the normal again.

This fixes the input geometry and contact reconstruction together while retaining the existing radius, thickness, time, and MTD behavior. It adopts the coordinate correction described in the Epic support discussion; the specification below uses unconditional rigid point transformation to avoid making near-zero-center tolerances part of the correctness contract. This is a reference for an upstream fix, not code to place in the installed engine or copy into a project override.

#### 4.1.2 Stock Engine Validation Design

Use the existing `TestCodeDemo` module to host development-only automation tests of the public `Chaos::SweepQuery` entry. The tests construct mathematical shapes directly, so they can confirm the engine defect without depending on a particular character asset or changing gameplay. Compare to an independent sphere/sphere oracle, including negative cases. Keep the tests as regression detectors: known defects should fail until the underlying query behavior is corrected, rather than accepting wrong values as expected results.

A separate scene reproduction must establish that the reported Blueprint/C++ query takes this path. That requires the actual character and caller. The current design therefore separates a ready diagnostic implementation from a blocked production workaround; it does not choose a new gameplay collision shape or fabricate an integration point.

### 4.3 Implementation Targets

| Target | Change Targets and Expected Outcomes | Responsibility / Dependencies | Local validation |
| --- | --- | --- | --- |
| Target-001 | Add private `Chaos` and `ChaosCore` dependencies to `TestCodeDemo/Source/TestCodeDemo/TestCodeDemo.Build.cs`, restricted to the editor target for these editor-only tests. | Build integration; existing main module. | UE 5.8 `TestCodeDemoEditor` builds; game dependency list remains unaffected. |
| Target-002 | Add `TestCodeDemo/Source/TestCodeDemo/Tests/SphereSweepContactTests.cpp` with automation tests under `WITH_EDITOR && WITH_DEV_AUTOMATION_TESTS`, grouped as `TestCodeDemo.Collision.SphereSweep`. | Regression verification; Target-001. | Exercise native `SweepQuery` and the independent oracle; record failures, controls, and raw outputs. |

The new test file does not yet exist. There is no existing project collision helper to extend. No engine-private include, custom engine macro, plugin module, UObject reference wrapper, or public gameplay API is required for these targets.

### 4.4 Blocked Scope and Impacts

- Type: User-Supplied Facts.
- Related Missing Information or Decision: The concrete caller, character/Physics Asset, failing query and hit, and the collision semantics that must be preserved.
- Affected Components and Responsibilities: Production trace integration, filtering, candidate enumeration, shape identification, and any Physics Asset mitigation. These remain Not Ready and have no formal implementation Task.
- Minimum Input Required from the User: Identify the trace call/Blueprint and character Physics Asset; preferably supply one failing Start/End/Radius, `ImpactPoint`, and `BoneName`. This is a fact request, not a request to approve an engine patch.
- Note: Once identified, reproduce the case and update this same plan's current design, targets, tasks, and validation. There is no verified corrected stock-engine release/CL in the evidence collected here.

The project-side investigation should first check for an official corrected build. If unavailable, assess a narrowly scoped query wrapper at the identified caller. Such a wrapper must obtain the actual physics shape and transform, independently include candidates the defective narrow phase can miss, and preserve query filters and nearest-hit ordering. It cannot rely solely on the original successful hits or choose the first sphere in a bone's body. No wrapper is currently claimed to be implementable with those contracts satisfied.

Changing a Physics Asset sphere to a capsule, or replacing a sphere sweep with a line trace, changes the tested volume and is only a candidate mitigation pending scene requirements. The source adapter also collapses sufficiently short capsules into spheres; a degenerate capsule is not a reliable bypass. These candidates are not current Implementation Targets.

## 6. Algorithm Details

### 6.1 Upstream repair specification

The external change location is `Chaos::SweepSphereVsSphere` in `Engine/Source/Runtime/Experimental/Chaos/Private/Chaos/GeometryQueries.cpp`. Retain its signature and callers. The conceptual changes are:

```text
WorldTargetCenter = TargetTM.TransformPositionNoScale(TargetSphere.Center)
WorldSweptCenter  = SweptTM.TransformPositionNoScale(SweptSphere.Center)
RelativeStart    = WorldSweptCenter - WorldTargetCenter
EffectiveSweptRadius = SweptSphere.Radius + Thickness

Run existing sphere/sphere sweep with:
    start = RelativeStart, direction = WorldSweepDirection
    target center = zero, existing length / radii / MTD flags

Restore valid contact position with WorldTargetCenter as origin.
Retain world-axis normals and existing result-validity / face handling.
```

The input/output-origin changes are one atomic repair. Fixing input centers while retaining the old translation origin produces `Pwrong = Pcorrect - RA*cA`, a separate residual contact offset. With the fixture above, that partial repair reports `(-30,0,0)` instead of `(-30,20,0)`.

Preserve the distinction between low-level `OutTime` (distance along a normalized direction) and `FHitResult.Time` (distance divided by trace length). Preserve existing no-hit and MTD return contracts; do not consume separated-contact outputs for initial overlap. Geometry radii/centers arriving here already reflect shape scaling. Review shape-normal restoration against the actual sphere implementation; do not alter the shared transform helper for other shape pairs.

No repository Task edits this function. Applying and compiling the upstream repair belongs to Epic or a separately authorized source-engine workflow outside this repository's stock-engine boundary. A corrected binary still needs the regression checks below.

### 6.2 Independent regression oracle

For separated spheres, use independently transformed world centers `CB` and `CA`, unit direction `D`, length `L`, swept radius `r`, target radius `R`, and thickness `h`:

```text
M = CB - CA
s = r + h + R
b = dot(M, D)
c = dot(M, M) - s*s
discriminant = b*b - c
distance = -b - sqrt(discriminant)
```

A separated entry exists only if the discriminant is nonnegative and the first intersection is in `[0,L]`. Handle initial overlap separately before applying this rule. At entry, `Q = CB + distance*D`, `N = (Q-CA)/s`, and `P = CA + R*N`. Verify both `P` on the target surface and `Q-P = (r+h)*N`.

Use double precision and fixed fixtures away from near-tangent numerical thresholds for primary pass/fail. For unit-scale centimeter fixtures, start with `0.001` cm position/distance tolerance and `0.00001` normal/time tolerance; report residuals rather than silently widening tolerances. Exact tangency, near tangency, zero-length sweeps, initial overlap, and coincident centers are separate robustness cases. Do not infer a unique contact or normal for coincident centers.

## 8. Task Decomposition

Only the diagnostic changes in section 4.3 are Ready. They do not themselves fix gameplay collision. The upstream repair is Out of Scope for implementation here, and production integration remains blocked by section 4.4.

### TASK-001: Main Module Add Diagnostic Dependencies (Source-Code Change)

- Task Readiness: Ready
- Related Decision / Constraint: Section 2 stock-engine boundary; section 4.1.2 diagnostic design.
- Related `change point`: Target-001; main module dependency list + add editor-only private Chaos dependencies + enable public geometry-query tests.
- `Task` Responsibility Boundary: Build integration.
- Task Dependencies: None.
- Implementation Card: Edit only the existing `TestCodeDemo.Build.cs` named in Target-001. Under an editor-target condition, add `Chaos` and `ChaosCore` to private dependencies; preserve all existing dependencies and target rules. No exported interface changes.
- Local Validation Method: Build `TestCodeDemoEditor` against the verified 5.8 installation and confirm the game target's dependency condition remains false.
- Redesign Stop Conditions: The installed module interfaces require private engine headers, additional production dependencies, or engine changes.

### TASK-002: Sphere Sweep Add Coordinate Regression Tests (Source-Code Change)

- Task Readiness: Ready
- Related Decision / Constraint: Section 1 evidence boundary; section 4.1.2; section 6.2 oracle.
- Related `change point`: Target-002; add native query comparisons + distinguish correct centers/contact restoration from the installed defect.
- `Task` Responsibility Boundary: Regression verification.
- Task Dependencies: TASK-001.
- Implementation Card: Create only the test file named in Target-002. Use public `Chaos/Sphere.h`, `Chaos/GeometryQueries.h`, and Unreal automation facilities under the stated guards. Use value-owned shape/transform fixtures and initialized output fields; create no world actors or assets.
- Ordered Code Steps: Construct the fixtures; invoke `Chaos::SweepQuery`; compare hit/no-hit and valid separated outputs against the independent oracle; report actual and expected distance/contact/normal with center/transform inputs. Include the principal numeric fixture, zero-center rotations, identity-rotation offsets, rotations of both nonzero centers, common translation, separated misses, and finite-segment rejection. Keep special overlap/MTD checks separate from surface-contact assertions.
- Local Validation Method: Compile and run `TestCodeDemo.Collision.SphereSweep`. On the inspected build, the principal rotated-offset case is expected to expose the bug while unaffected controls pass. Record a failing baseline honestly; all correctness assertions must pass on any claimed repair.
- Redesign Stop Conditions: Compiled behavior contradicts the source prediction, public dispatch cannot be exercised, the fixture enters initial overlap unintentionally, or numerical tolerance dominates the error. Reconcile evidence before planning production changes.
- Decomposition Rationale: Fixture generation, independent oracle, and assertions share one verification responsibility and one test entrypoint. Further file-level splitting adds no separate acceptance boundary.

## 9. Validation and Regression Checks

### 9.1 Build and execution

Use the verified engine's `Build.bat` with target `TestCodeDemoEditor Win64 Development`, the main `.uproject`, and `-WaitMutex`. The repository's current plugin scripts target UE 5.7; `animmagicbuildtest.bat` targets UE 5.4 and a different plugin. They do not validate this 5.8 main-module change and should not be run or rewritten for this task.

After the future test implementation, the native Windows commands are:

```bat
"<UE_ROOT>\Engine\Build\BatchFiles\Build.bat" TestCodeDemoEditor Win64 Development -Project="<PROJECT_ROOT>\TestCodeDemo\TestCodeDemo.uproject" -WaitMutex
"<UE_ROOT>\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" "<PROJECT_ROOT>\TestCodeDemo\TestCodeDemo.uproject" -unattended -nop4 -NullRHI -ExecCmds="Automation RunTests TestCodeDemo.Collision.SphereSweep" -TestExit="Automation Test Queue Empty" -log
```

Inspect automation results for the named tests; process launch or successful compilation is not evidence of collision correctness. Because build wiring changes, also verify the project opens in the editor. No build or editor execution occurred during this planning turn.

### 9.2 Scene reproduction and acceptance

Once the asset/caller is identified, use a disposable scene/asset copy in the main development project. Record the installed CL, query node/API, collision settings, component and bone, raw hit fields, authoritative physics body transform, local sphere center, scaled radius, and frame/tick context. Do not compare against a later render pose.

| Check | Acceptance / observation |
| --- | --- |
| Main fixture, target offset + rotation | Contact `(-30,20,0)`, distance `65`, normalized time `0.325`, normal `(-1,0,0)` within stated fixture tolerances. |
| Zero center, arbitrary rotation | Existing correct behavior preserved. |
| Nonzero center, identity rotation | Existing correct behavior preserved; detects double application of the center. |
| Both operand centers rotated | Actual transformed centers determine the result. |
| Common translation / common rotation | Translation moves the contact equally; rotation rotates contact and normals with the entire fixture. |
| Hit and miss near the displaced target boundary | No false negatives/positives caused by using the wrong center. |
| Tangent, near-tangent, endpoint, zero-length | Finite outputs and documented boundary behavior; assessed separately from primary tolerance fixtures. |
| Initial overlap and MTD | Preserve penetration semantics; no invented unique entry contact. |
| Multiple spheres in one body, multiple bones, other blockers | Correct shape/body identity, filtering, closest blocker and multi-hit ordering. |
| Uniform and nonuniform scale | Match actual generated sphere geometry and engine radius convention; no double scaling. |
| Animated, kinematic, and simulated bodies | Compare geometry and trace from the same physics state. |
| Line/sphere distinction and character capsule | Debug marker reads the intended field and intended collision component. |

For any project workaround, also compare query cost at the real call frequency and require unchanged filters, ignore lists, physical-material behavior, and single/multi semantics. A workaround that fixes a marker while retaining missed hits fails acceptance. Rollback means removing the opt-in caller integration and restoring any disposable asset change; do not keep compensating offsets after a verified engine fix.

Exclusions: No character asset was edited, no C++ or configuration was changed, no UE regression suite was executed, and no engine patch was applied. The completed validation is source inspection plus an independent numeric check of the primary fixture.

## 10. Assessment and Recommendations

Disclaimer: This assessment is guidance for reading the current plan, may be inaccurate, and is not a factual basis for development.

- Plan Assessment: The source evidence supports a small, well-defined upstream coordinate repair. Project-owned diagnostic work is limited to two files. The principal risks are an incomplete world-position restoration and assuming that a late hit adjustment can repair missed collisions; both are addressed by the oracle and acceptance criteria.
- Plan Detail Assessment: An independent implementation agent can execute TASK-001 and TASK-002 within their explicit boundaries after implementation is requested. The production workaround is intentionally not executable yet. The plan distinguishes source proof, numeric prediction, native regression execution, and scene confirmation.
- Current Issues: Overall readiness is Partially Ready because the actual consumer/asset is unidentified and this stock-engine repository cannot apply the upstream correction. User-Supplied Facts are needed only to select and scope production integration; they do not block the source diagnosis or the diagnostic test design. A fixed official engine build has not been established.
- Recommended Next Step: Implement and run the diagnostic tests when implementation is requested. Identify the trace caller and Physics Asset, capture one failing query, and use that evidence to resolve section 4.4 and update sections 4, 8, and 9. Do not implement a production point-offset correction or claim the bug fixed before native and scene results satisfy the stated checks.
