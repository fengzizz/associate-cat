# Real-world Cat Plan examples

These plans were produced from real Codex conversations about an Unreal Engine project. They are lightly edited for public reading: private links and machine-specific paths have been normalized, while the original scope, technical conclusions, readiness, and validation limits remain intact.

The examples intentionally show two different outcomes. A useful plan can finish with all scoped tasks ready, or it can remain `Partially Ready` when the evidence is sufficient to diagnose a defect but insufficient to choose a safe project integration.

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

Each full Plan quotes its original user request verbatim. Private source links are shown as path names so readers can understand the analysis boundary without receiving broken links. The sphere-sweep example replaces a short Unreal Engine source excerpt with equivalent pseudocode. No runtime result or validation status has been upgraded for presentation.
