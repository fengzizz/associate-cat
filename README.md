# Associate Cat

[View both skills on skills.sh](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[简体中文](README.zh-CN.md)

Associate Cat is a lightweight workflow harness for AI coding. It provides two skills for your coding agent: one to help you work out a plan, and another to make and check the changes.

Start with a rough idea, a stubborn bug, or a change you're still thinking through. Tell the AI what you know, as you would a teammate. You can work out the details together.

`cat-plan` is the core of the workflow. It reads the relevant code and documentation, weighs the options, and writes a plan for you to review. Check its understanding of the problem and its proposed design, then tell it what you'd like to change. When you're ready, ask `cat-code` to implement the plan and check the results.

## Why Associate Cat

- **Start before you have all the answers.** Describe what you want to build or what's going wrong. `cat-plan` helps you work out the requirements and an approach.
- **Get a plan that fits your project.** Both skills read the relevant code, project rules, and documentation, including how to check changes.
- **Keep the investigation focused.** `cat-plan` follows the code and dependencies that matter to the problem, without scanning unrelated parts of the repository.
- **Review the plan before code.** If the AI misunderstands a requirement, puts a feature in the wrong module, or proposes too much work, correct the plan before it writes code.
- **Choose the workflow you need.** Stop after analysis, send a small change straight to `cat-code`, or plan a complex task before implementing it.
- **Hand over what you've agreed on.** The design, scope, tasks, and checks stay in one document that `cat-code` can follow when making and verifying changes.

## What Makes Cat Plan Different

- **Review the results at every stage.** The plan records the AI's understanding of your request, the scope it has set, and the related parts of the project it examined. It then presents a proposed design and breaks the work into tasks. You can review each result and ask for corrections before implementation if the AI misunderstood something or took the design in the wrong direction.
- **Think through the requirements together.** Reading a plan may reveal a missing constraint or give you a new idea. Tell Cat Plan what to revise. You can return to the same document when you pick up the discussion later.
- **Let the project answer routine questions.** Cat Plan investigates the information available in the project and handles routine technical decisions. It comes to you for business trade-offs, personal preferences, and significant risks.

Cat Plan grew out of the author's day-to-day work on a project. After using and refining it over time, the author adapted it for other projects.

**A practical tip:** If a task needs several discussions or sessions, ask the AI to save the plan as a Markdown file and keep it up to date. You can read it, suggest changes, and hand it to Cat Code when you're ready to implement it.

## Where Associate Cat Helps

- **A feature or change that's still taking shape.** Bring your goal and initial ideas to `cat-plan` and work through possible approaches.
- **Bug investigation, refactoring, code review, or design review.** Ask `cat-plan` to examine the relevant code and dependencies. It can give you findings or turn them into a plan for changes.
- **Changes you're ready to make.** Ask `cat-code` to handle a small, clear change directly or implement a plan you've already reviewed.

A UE5 game project is one example. It may have a large codebase, complex state, and systems that depend on one another. Cat Plan examines the code and project rules relevant to a feature or bug, then explains what to change and how. You can use the same workflow in other large or long-lived codebases.

We use the same Plan→Code workflow to maintain Associate Cat and its documentation.

## Real-world examples

Two plans generated from real Codex conversations show what the workflow produces in practice:

- [Reviewing intended behavior in Anim Retarget Magic](examples/plans/plan_fikrig_magic_thik_goal_solver_review.md) follows one solver through configuration, transform logic, curve input, pose output, editor tooling, and legacy conversion. It honors the user's instruction to disregard defects and ends with four ready validation tasks.
- [Investigating a UE 5.8 sphere-sweep contact offset](examples/plans/plan_ue58_sphere_sweep_contact_offset.md) traces a reported `ImpactPoint` error into Chaos, verifies the error with an independent calculation, and stops at `Partially Ready` because the real caller and asset are still unknown.

See the [examples guide](examples/plans/README.md) for the original requests, results, and validation limits.

## Quick Start

Install both skills with:

```bash
npx skills add fengzizz/associate-cat
```

When prompted, select `cat-plan` and `cat-code`. To use a skill, include its name in your request.

For a global, non-interactive install, run the command for your agent:

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

| Skill | Use it when | What it does |
| --- | --- | --- |
| [`cat-plan`](skills/cat-plan/SKILL.md) | You want to explore a requirement, investigate a bug, discuss a design, or review work. | Reads the relevant code and documentation, compares approaches, and gives you findings or a plan. |
| [`cat-code`](skills/cat-code/SKILL.md) | You have a clear change in mind or a plan ready to implement. | Makes the agreed changes, runs checks, and reports the results. |

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

## License

MIT — see [LICENSE](LICENSE).
