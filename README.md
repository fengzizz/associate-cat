# Associate Cat

[View both skills on skills.sh](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[简体中文](README.zh-CN.md)

Associate Cat is a lightweight workflow harness for AI coding. It provides two skills for your coding agent: one to help you work out a plan, and another to make and check the changes.

Start with a rough idea, a stubborn bug, or a change you're still thinking through. Tell the AI what you know, as you would a teammate. You can work out the details together.

`cat-plan` is the core of the workflow. It reads the relevant code and documentation, weighs the options, and writes a plan for you to review. Check its understanding of the problem and its proposed design, then tell it what you'd like to change. When you're ready, ask `cat-code` to implement the plan and check the results.

## Why Associate Cat

- **Start before you know the solution.** Describe your goal or the problem you've run into. Cat Plan investigates the project and helps you work through the requirements and possible approaches.
- **Review, revise, and pick up where you left off.** Check whether the AI understood your request and whether the design makes sense, then give feedback on specific parts. The current plan keeps the analysis and design together so you can continue the discussion later.
- **Hand the agreed work to Cat Code.** The plan records the design, scope, tasks, and checks. When you explicitly ask for implementation, Cat Code follows those agreements, makes the changes, and reports what it checked.

## Design Principles

**Give analysis a structure and judgment a foundation.** When a problem first comes up, its requirements, constraints, and solution may still be unclear. Cat Plan investigates relevant facts, defines the scope, and develops a design you can examine piece by piece. A concrete proposal may reveal a missing constraint or prompt you to rethink the original idea. Each stage gives you something to reason about, helping the plan take shape through discussion.

**Build a shared understanding through a consistent format.** Formal plans use an agreed structure for requirements, scope, analysis, design, tasks, and validation. The AI has clear expectations for what to deliver; you know where to find the evidence, examine the design, and give feedback. The structure also makes relationships easier to check: does the design address the requirements, and do the tasks implement the design? You and the AI can revise specific parts and build on them without having to navigate a different format each time.

**Let human judgment shape the work as it develops.** Cat puts the AI in charge of investigation and technical analysis, so you can focus on whether the goal and design fit your needs and which trade-offs you are willing to accept. Feedback should carry through to the affected design and tasks, while findings that still hold remain available. When you explicitly request implementation, Cat Code continues from those agreements. As the problem and its solution take shape, you can make judgments about concrete results and have those judgments guide what happens next.

Match the depth of the process to the task. Simple questions can stay brief; relationships that affect the solution deserve careful analysis.

Cat Plan grew out of the author's day-to-day work on a project. After using and refining it over time, the author adapted it for other projects.

**A practical tip:** For work that spans several conversations, ask the AI to save the plan as Markdown and keep it up to date.

## Where Associate Cat Helps

- **A feature or change that's still taking shape.** Bring your goal and initial ideas to `cat-plan` and work through possible approaches.
- **Bug investigation, refactoring, code review, or design review.** Ask `cat-plan` to examine the relevant code and dependencies. It can give you findings or turn them into a plan for changes.
- **Changes you're ready to make.** Ask `cat-code` to handle a small, clear change directly or implement a plan you've already reviewed.

Start with a bug in one part of your project, a feature change, or a piece of writing that needs a clearer structure.

The author also uses this workflow for game development with UE5. Working in a codebase as large and complex as UE5 makes planning ahead especially valuable: Cat Plan proactively examines the relevant code and dependencies, defines the scope of the changes, and establishes how to verify them. Cat Code then implements the plan. This is what the harness provides: a process for building the context the AI needs before it starts making changes and keeping the work within clear boundaries, helping reduce overlooked dependencies and scope creep.

We use the same Plan→Code workflow to maintain Associate Cat and its documentation.

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

## Real-world examples

Two plans generated from real Codex conversations show what the workflow produces in practice:

- [Reviewing intended behavior in Anim Retarget Magic](examples/plans/plan_fikrig_magic_thik_goal_solver_review.md) follows one solver through configuration, transform logic, curve input, pose output, editor tooling, and legacy conversion. It honors the user's instruction to disregard defects and ends with four ready validation tasks.
- [Investigating a UE 5.8 sphere-sweep contact offset](examples/plans/plan_ue58_sphere_sweep_contact_offset.md) traces a reported `ImpactPoint` error into Chaos, verifies the error with an independent calculation, and stops at `Partially Ready` because the real caller and asset are still unknown.

See the [examples guide](examples/plans/README.md) for the original requests, results, and validation limits.

## License

MIT — see [LICENSE](LICENSE).
