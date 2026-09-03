# Associate Cat

[View on skills.sh](https://skills.sh/fengzizz/associate-cat)

[简体中文](README.zh-CN.md)

Associate Cat is a two-skill workflow for software development. Start by describing a requirement or bug in your own words; `cat-plan` investigates it in the context of the project and produces a plan you can review and refine. Reviewing the plan means reviewing the AI's reasoning, which is faster and more direct than judging its direction from code after the fact.

`cat-plan` investigates, corrects, designs, and plans. `cat-code` implements and validates only after you explicitly request it and the scope is closed—and stops when the work needs new design or broader scope.

Associate Cat provides general-purpose AI skills for software development, covering AI planning, software planning, and scope-controlled code implementation. It organizes requirement analysis, bug investigation, technical design, task breakdown, and validation into a project-grounded coding agent workflow.

## Why Associate Cat

- **Start with the project.** The skills inspect applicable rules, code, documentation, and validation entry points before forming a recommendation.
- **Spend less time on low-value back-and-forth.** `cat-plan` investigates discoverable facts and resolves routine technical choices, leaving the choices about goals, value, and risk to you.
- **Focus from the task outward.** It starts from the request or symptom and follows only the responsibilities, root causes, and dependencies that can change the plan; a local problem does not require analyzing the whole codebase first.
- **Use only the workflow the task needs.** Analyze without implementation, create a handoff-ready plan for complex work, or implement a small, clearly bounded change directly.
- **Review the plan, not the code.** Before code is written, you can check whether the AI's reasoning, direction, scope, and validation make sense. That is faster and more direct than inferring its direction from code after the fact; implementation still needs validation and any necessary code review.
- **Keep scope bounded and verification honest.** Planning does not authorize writes; implementation stops when it needs new design or broader scope, and reports only checks that actually ran.

**Why does this also fit mid-sized and large game projects?** A single request in these projects often crosses runtime code, editor tooling, assets and configuration, build scripts, and validation projects, while the relevant logic may be spread across several modules. Instead of analyzing the whole repository up front, `cat-plan` starts from the feature or bug you describe, finds the module that actually owns it, and follows only the dependencies, project rules, and validation paths that can change the plan. That exposes the connections behind a local change while leaving unrelated systems out of scope; actual results still depend on the project materials, model, and host capabilities.


Associate Cat's public version and documentation were themselves planned and refined using Associate Cat.

## Quick Start

Install both skills with:

```bash
npx skills add fengzizz/associate-cat
```

When prompted, select both `cat-plan` and `cat-code`. Mention a skill by name when you want to choose the workflow explicitly.

To install both skills globally for one agent without prompts, run the matching command below. You only need one of these commands.

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

## Choose a Skill

| Skill | What it does | What you get | Stops at |
| --- | --- | --- | --- |
| [`cat-plan`](skills/cat-plan/SKILL.md) | Uses project context to clarify a requirement or bug, investigate relevant facts, compare and converge options, and produce design, tasks, and validation as the work requires. | Analysis, review findings, or a plan that keeps only the current effective content and can be refined and handed off. | Does not modify code, configuration, or resources, or decide business meaning, material risk, or scope expansion for you. |
| [`cat-code`](skills/cat-code/SKILL.md) | Implements a plan or task that you explicitly request and that is prepared for implementation, or a clearly scoped direct change, then runs relevant validation. | Actual changes, behavior notes, validation results, and any necessary blockers or remaining risks. | Does not add design, modify the Plan, or silently expand scope. |

`cat-plan` is the center of the workflow: it handles discoverable project facts and routine technical comparisons first, asking you only when business meaning, subjective preferences, material risk, or scope expansion truly needs your decision. Everyday analysis does not require a formal Plan; complex work keeps the current design, tasks, risks, and validation in the same current plan. Plan and Code are responsibilities, not a mandatory two-step ritual: use Plan alone for analysis, use both for complex work, or use Code directly for a small, fully defined change.

## Choose a Workflow

1. **Plan only** — Use this path when you need analysis, bug investigation, a design, a plan, or a review without implementation.

   ```text
   Take a look at this request and work out how we should approach it. Use cat-plan to put the reasoning and plan in a document, but don't change any code yet: ...
   ```

2. **Plan→Code** — Use this path when the work is complex, needs design, or needs explicit implementation boundaries for handoff.

   ```text
   This change is a bit involved. Use cat-plan to look at it in the context of the project first, then turn it into a plan we can implement: ...
   ```

   Review the plan and give feedback when needed:

   ```text
   Don't ..., Just reuse the existing ... and update the plan around that.
   ```

   Once the plan is confirmed and prepared for implementation, send a separate implementation request:

   ```text
   Ok, use cat-code to implement it.
   ```

3. **Direct Code** — Use this path when the target, allowed scope, expected result, and validation are already clear and no new design is required.

   ```text
   Use cat-code to help me make this change: ... Keep it to ..., then verify ...
   ```

For everyday problems, you do not need to give all details first. Describe the requirement or bug, then add any ideas, expected result, or constraints that matter, and `cat-plan` can begin investigating and produce a plan document. For complex work, save the result as a document and keep refining the same file; reviewing the plan means checking whether the AI's reasoning and proposed implementation match your intent. A few rounds are usually enough to converge, then ask `cat-code` to implement it separately.

## Core Boundaries

- **Project facts first.** The skills inspect applicable project rules, code, documentation, and validation entry points before deciding how the requested work should proceed. Within the user's goal and scope, `cat-plan` investigates discoverable facts and resolves routine technical choices instead of asking the user to decide every implementation detail.
- **You retain goals and risk decisions.** Business meaning, subjective preferences, material risk acceptance, destructive tradeoffs, and scope expansion remain your decisions.
- **Maintain the same current plan.** Each active planning task keeps one effective plan; continued feedback updates that document instead of mixing ordinary revision history into implementation scope.
- **A finished plan does not start coding.** Even when the plan is prepared for implementation, you must separately and explicitly ask `cat-code` to implement it; the plan file or its wording cannot grant write authorization by itself.
- **Stop instead of guessing; verify honestly.** If facts conflict, required design is missing, or scope must expand, `cat-code` stops and reports the blocker. The result reports only the checks that actually ran and does not present failed, blocked, or unrun validation as passed.

Skills are interpreted by the host agent, so behavior can vary with the model, client, permissions, and project materials. Validate critical work in your own environment.

## License

MIT — see [LICENSE](LICENSE).
