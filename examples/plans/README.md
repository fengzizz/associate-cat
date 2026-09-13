# Real-world Cat Plan examples

These plans come from work on an Unreal Engine project. They are lightly edited for public reading: private links and machine-specific paths have been normalized, while the original scope, technical conclusions, readiness, and validation limits remain intact.

The examples show three useful outcomes: narrowing an overbroad repair scope, documenting intended behavior within an explicit review boundary, and diagnosing a defect while leaving project integration `Partially Ready` until missing evidence is available.

## Code-review reassessment: three proposed repairs become one

- **Background:** An earlier EasyDebugComponent review proposed three repair tasks. The reassessment checks whether those findings are supported by normal callers and engine lifecycle guarantees.
- **Target:** EasyDebugString's component, subsystem callers, context processing, and UE 5.8 HUD/actor lifecycle.
- **Output:** One retained P2 HUD event-routing repair, with two unsupported lifecycle repairs excluded from the implementation scope.
- **Status:** `Final Snapshot / Ready`. This is a planning result; no runtime reproduction was supplied, and compilation and runtime validation were not performed. The report records the UE 5.8 project / UE 5.7 build-script mismatch.
- **Demonstrates:** Checking reachability and impact before accepting review findings, revising severity, and limiting implementation and validation to the supported repair. It does not establish that excluded scenarios are impossible under arbitrary custom callers.

[Read the complete EasyDebugComponent remediation plan](plan_easy_debug_component_remediation.md)

## Intended-behavior review for a published UE plugin

- **Input:** Review one solver comprehensively while disregarding flaws and defects.
- **Target:** `FIKRigMagicTHIKGoalSolver` from [Anim Retarget Magic](https://www.fab.com/listings/40f149fc-d43c-42ff-a51a-b059ddabeb8f?lang=en), a Fab plugin published by the same author as Associate Cat.
- **Output:** A source-grounded description of configuration ownership, initialization, transform solving, curve input, pose output, editor visualization, legacy conversion, and four validation-only tasks.
- **Status:** `Final Snapshot / Ready`. The tasks are ready to run; runtime, editor, and build validation were not run while the plan was written.
- **Demonstrates:** Precise scope control, bounded source exploration, and a plan that documents existing intended behavior without turning adjacent defects into unauthorized repair work.

[Read the complete FIKRigMagicTHIKGoalSolver plan](plan_fikrig_magic_thik_goal_solver_review.md)

## UE 5.8 sphere-sweep bug investigation

- **Input:** Investigate an offset `ImpactPoint` when a sphere trace hits a sphere in a character Physics Asset, then inspect the source directly.
- **Target:** The UE 5.8 sphere-versus-sphere query path from Blueprint trace entry to Chaos narrow phase and `FHitResult` conversion.
- **Output:** A confirmed coordinate-space defect in the inspected engine build, a numeric reproduction, an upstream repair specification, an independent regression oracle, and two ready project-owned diagnostic tasks.
- **Status:** `Draft / Partially Ready`. Source inspection and analytic validation are complete; the real caller, asset, compiled query, and scene reproduction remain unavailable.
- **Demonstrates:** Following a symptom through a large codebase, separating confirmed facts from missing evidence, and refusing to invent a production workaround before its integration point is known.

[Read the complete UE 5.8 sphere-sweep plan](plan_ue58_sphere_sweep_contact_offset.md)

## What has been edited

The solver and sphere-sweep plans quote their original user requests verbatim. The remediation plan adds self-contained background; its original request was not supplied and has not been reconstructed. Private source links are shown as path names so readers can understand the analysis boundary without receiving broken links. The sphere-sweep example replaces a short Unreal Engine source excerpt with equivalent pseudocode. No runtime result or validation status has been upgraded for presentation.
