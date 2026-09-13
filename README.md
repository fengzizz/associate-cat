# Associate Cat

[View both skills on skills.sh](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[简体中文](README.zh-CN.md)

**Turn requirements and problems into grounded, reviewable plans.**

Associate Cat centers on `cat-plan`, a skill for requirements analysis and bug investigation. It brings project facts, design trade-offs, and follow-up work into plan documents you can review and refine. Use it in Codex, Claude Code, and other coding agents; when you want to make changes, the companion `cat-code` skill can handle implementation and checks.

**Structure the investigation and deliverables. Let the model work out the solution.**

## Quick Start

```bash
npx skills add fengzizz/associate-cat
```

When prompted, select `cat-plan`; add `cat-code` if you want the implementation companion. Generate and save a plan directly:

```text
Use cat-plan to analyze the following requirements and generate and save a complete implementation plan: ...
```

```text
Use cat-plan to investigate the following bug and generate and save a complete bug-fix plan: ...
```

You do not need fully worked-out requirements to begin. Let Cat Plan investigate and deliver a complete initial plan, then review its interpretation, evidence, and design. Keep refining the same document through feedback until the plan is ready to implement. Unresolved conditions belong in the plan too, so you can spot and correct problems early.

See the [real examples](#real-world-examples) for the output, or [agent-specific installation](#agent-specific-installation) for global install commands.

## Start with Requirements Analysis and Bug Investigation

A capable model still needs to establish your goals, the current state of your project, its key constraints, and what would count as solving the problem. Cat Plan makes that investigation and its conclusions part of the deliverable.

| What you want to do | What Cat Plan examines | What you can get |
| --- | --- | --- |
| Requirements analysis and feature planning, behavior changes, or refactoring | Goals, current behavior, constraints, reusable approaches, and design trade-offs | A requirements interpretation, design, scope, and validation approach |
| Investigate a bug and identify a repair direction | Reproduction conditions, actual versus expected behavior, relevant paths, and root-cause evidence | Findings, proposed fixes, risks, and checks still needed |
| Code review or design review | Responsibilities, behavior, dependencies, compatibility, and change impact | Review findings or improvement proposals, with supporting evidence |

Cat Plan also supports requirements beyond coding, such as document improvements, process changes, and general planning. Clarifying goals, investigating facts, comparing approaches, and revising a plan through feedback carry over; the materials examined and the deliverables adapt to the task.

## How It Works

**Let the AI investigate and draft the plan.** Starting from a local problem, Cat Plan examines the project facts and relationships that determine the solution and produces a complete initial plan with a clear scope. You do not need to supply the technical answer or assemble a map of the entire project first.

**Participate by reviewing a concrete proposal.** The document brings requirements, evidence, assumptions, design, and follow-up work together, making misunderstandings and unsuitable trade-offs easier to identify early.

**Keep the discussion in one evolving document.** Feedback updates the affected design, tasks, and verification while preserving conclusions that still hold. The plan takes shape through successive reviews and revisions.

Cat grew out of the author's work on specific requirements and bugs in large projects. It generalizes the skills used in that work while retaining the same approach: start with a local problem, investigate the necessary relationships, define a bounded plan, and refine it through review. Associate Cat itself is maintained with this workflow.

## Real-world Examples

These two cases show Cat Plan outputs from bug investigation and review: how findings gain supporting evidence, and how a plan distinguishes actionable work from work that needs more information.

- **[Code review: narrowing the repair scope from three fixes to one](examples/plans/plan_easy_debug_component_remediation.md).** Rechecking actual callers and engine lifecycle guarantees removed two unsupported repairs and retained one HUD event-routing fix. The result is a smaller, justified implementation plan; compilation and runtime validation were not performed.
- **[Debugging: tracing a UE 5.8 sphere-sweep contact offset](examples/plans/plan_ue58_sphere_sweep_contact_offset.md).** The investigation follows an `ImpactPoint` error into Chaos and independently checks the coordinate-space calculation. The plan remains `Partially Ready` because the actual caller and asset are missing, so project integration cannot yet be chosen.

The [examples guide](examples/plans/README.md) includes the background, results, and validation limits of all three published plans, including an intended-behavior review of an animation solver.

## Harness Engineering: Defining Problem and Change Boundaries in Large Codebases

Progress in Agentic Coding makes how we organize an AI's work as important a consideration as model capability. Harness Engineering concerns how goals, context, feedback, and verification support continued progress. Cat applies workflow constraints to planning: the AI investigates and proposes solutions, giving you a complete artifact to review, challenge, and refine early.

**Requirements clarification → Preliminary analysis → Design approach → Decisions → Plan**

**Follow a local problem to the relationships that matter.** In large codebases, a bug or requirement often involves responsibilities and dependencies beyond the initial touchpoint. Cat Plan establishes the goal, then uses dependency analysis to build the project context needed for a solution (Context Engineering). Investigation may span several modules while the resulting change stays local. Scope control follows from analysis and design, rather than the first file named in a request.

**Turn engineering constraints into a concrete plan.** The design explains where changes belong, what can be reused, and which existing contracts must hold. Decisions establish the chosen approach and what falls outside this task. The complete plan identifies changes, tasks, and verification, letting you check for excessive scope or overlooked effects. This form of Spec-driven Development can begin with an initial requirement; you do not have to write the technical answer first.

**Let feedback reshape the next version.** The first plan is the starting point for discussion. You can challenge its interpretation, evidence, or design decisions; Cat Plan revises the affected parts and updates downstream tasks and checks. Human-in-the-loop judgment has a concrete place in this process. The same Living Plan records the current solution throughout iterative planning, ready for Cat Code to continue within an agreed scope when you request implementation.

## Start with Cat Plan, Add Implementation When Needed

| Skill | Use it when | What it does |
| --- | --- | --- |
| [`cat‑plan`](skills/cat-plan/SKILL.md) | You want to explore a requirement, investigate a bug, discuss a design, or review work. | The core planning skill: investigates relevant facts and develops and maintains reviewable plan documents. |
| [`cat‑code`](skills/cat-code/SKILL.md) | You have a clear change in mind or a plan ready to implement. | The implementation companion: makes scoped changes and runs checks; also handles clear, small changes independently. |

You can use `cat-plan` on its own; `cat-code` is optional. Small changes can go straight to implementation without a plan. Cat Code waits for your explicit request before changing code. Saving a plan does not give it permission to start.

## Keep Working from the Same Plan

1. **Generate and save an initial plan.** Use a requirement or bug prompt to have Cat Plan investigate and deliver a complete proposal for review.
2. **Review and revise the same document.** Check the interpretation and scope first, then the design, tasks, and verification. For example: `Prefer reusing the existing module here. Update this plan.`
3. **Implement when the plan is suitable.** `Use cat-code to implement this plan.`

To continue in a new conversation, have it read and update the same plan. Feedback should change the affected content, rather than simply being appended at the end.

You can also plan and revise without implementing, or use Cat Plan for standalone analysis and review. Clear, small changes can go directly to Cat Code. It reports its changes and checks, and explains when it needs to change the agreed design or scope.

## Choose a Skill

| Skill | Use it when | What it does |
| --- | --- | --- |
| [`cat‑plan`](skills/cat-plan/SKILL.md) | You want to explore a requirement, investigate a bug, discuss a design, or review work. | Reads the relevant code and documentation, compares approaches, and gives you findings or a plan. |
| [`cat‑code`](skills/cat-code/SKILL.md) | You have a clear change in mind or a plan ready to implement. | Makes the agreed changes, runs checks, and reports the results. |

You can use `cat-plan` on its own; `cat-code` is optional. Small changes can go straight to implementation without a plan. Cat Code waits for your explicit request before changing code. Saving a plan does not give it permission to start.

## Choose a Workflow

1. **Plan only** — For analysis, investigation, design, or review.

   ```text
   Have a look at this with cat-plan and write up a plan. Don't change any code yet: ...
   ```

2. **Plan→Code** — For work you want to think through before implementation.

   ```text
   Can you take a look at this with cat-plan? Write up a plan and save it to a file: ...
   ```

   You can add a quick note about scope if needed:

   ```text
   Leave module A alone for now; just handle module B ...
   ```

   After you read the plan, just tell it what to change:

   ```text
   ... For this part, use the existing ... No need to create a new module. Update the plan to match.
   ```

   When the plan looks good:

   ```text
   Ok, use cat-code to implement it.
   ```

3. **Direct Code** — For small, clear changes.

   ```text
   Use cat-code to change ... and run ... when you're done.
   ```

Cat Code handles the implementation details within the agreed scope. It pauses and explains if it needs to change the agreed behavior or design, expand the scope, or ask you to accept a risk. When it finishes, it tells you which checks it ran and what remains unverified.

## Agent-specific Installation

These commands install both skills globally without interactive prompts. Run the command for your agent:

For **Codex**:

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a codex -y
```

For **Claude Code**:

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a claude-code -y
```

For **Cursor**:

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a cursor -y
```

For **Gemini CLI**:

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a gemini-cli -y
```

For **GitHub Copilot**:

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a github-copilot -y
```

## Other Information

Cat provides skills, not an agent runtime, automatic cross-session memory, or a guarantee of unattended delivery. An initial plan can include unresolved conditions; gaps that determine the solution still need clarification. Results depend on the model, available tools, and project information. 

`This README was primarily generated by Cat AI and may contain inaccuracies or omissions.`

Bring questions and feedback to [Discussions](https://github.com/fengzizz/associate-cat/discussions).

## License

MIT — see [LICENSE](LICENSE).
