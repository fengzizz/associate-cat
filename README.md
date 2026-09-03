# Associate Cat

[View both skills on skills.sh](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[简体中文](README.zh-CN.md)

Associate Cat is a lightweight, skill-based workflow harness for collaborative AI coding. It works inside the coding agent you already use, keeping project context, decisions, implementation scope, and validation connected from the first conversation to the finished change.

Start with whatever you have: a rough idea, a stubborn bug, or a change you have not fully thought through yet. Talk to the AI as you would to a teammate. `cat-plan` reads the project, works out what matters, and turns the conversation into a plan you can challenge and refine. When the direction feels right, `cat-code` carries out the agreed work and checks the result.

## Why Associate Cat

- **Start with an incomplete idea.** A few thoughts, the result you want, or the symptom you are seeing are enough for `cat-plan` to begin.
- **Work from the codebase, not just the prompt.** The skills read the relevant rules, code, documentation, and validation paths before suggesting an approach.
- **Follow the problem, not the whole codebase.** `cat-plan` follows only the code and dependencies that matter, giving each problem the context it needs.
- **Review the direction before any code is written.** If the AI misunderstood something or picked the wrong scope, you can fix the plan before that turns into rework.
- **Use as much workflow as the task needs.** Stop after analysis, use Plan→Code for involved work, or send a small, well-defined change straight to `cat-code`.
- **Keep planning and implementation connected.** The agreed design, scope, tasks, and checks stay together in one document that `cat-code` can follow.

## Where Associate Cat Helps

- **A feature or change that is still taking shape.** Bring the rough idea to `cat-plan`; it will investigate the project, work through the details, and turn it into a practical plan.
- **A bug, refactor, code review, or design review.** Let `cat-plan` trace the relevant code and dependencies. It can give you its findings or turn them into a concrete plan.
- **Work that is ready to implement.** Send a small, clear change directly to `cat-code`, or hand over an agreed plan for implementation and validation.

Game projects are a good example: they are often large, full of interacting systems and complex state, and spread across many technical domains. Starting with one feature or bug, `cat-plan` finds the code and project rules that matter and turns that context into a plan you can review and hand off.

This repository and its documentation are maintained with the same Plan→Code workflow.

## Quick Start

Install both skills with:

```bash
npx skills add fengzizz/associate-cat
```

When prompted, select both `cat-plan` and `cat-code`. Mention the skill by name whenever you want a specific workflow.

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
| [`cat-plan`](skills/cat-plan/SKILL.md) | You want to explore a requirement, investigate a bug, discuss a design, or review existing work. | Reads the project, clarifies the problem, works through the options, and produces findings or a plan you can keep refining. |
| [`cat-code`](skills/cat-code/SKILL.md) | A small change is already clear, or an agreed plan is ready to implement. | Implements the agreed scope, runs the relevant checks, and reports what changed and what was verified. |

`cat-plan` is the heart of the workflow. You bring the goal and the ideas; it investigates the project, works through the technical details, and writes up a plan you can review. Stop there when you only need analysis, add `cat-code` when the work is ready to implement, or go straight to `cat-code` for a small, clear change.

## Choose a Workflow

1. **Plan only** — For analysis, investigation, design, or review.

   ```text
   Have a look at this with cat-plan and write up a plan. Don't change any code yet: ...
   ```

2. **Plan→Code** — For work you want to think through before implementation.

   ```text
   I have a request for you. Take a look with cat-plan and write up a plan and save it: ...
   ```

   You can add a quick note about scope if needed:

   ```text
   Leave module A alone for now; just handle module B ...
   ```

   After you read the plan, just tell it what to change:

   ```text
   ... For this part, use the existing ... No need to create a new module. Update the plan around that.
   ```

   When the plan looks good:

   ```text
   Ok, use cat-code to implement it.
   ```

3. **Direct Code** — For small, clear changes.

   ```text
   Use cat-code to change ... and run ... when you're done.
   ```

You do not need a complete specification to begin. Talk to the AI as you would to a teammate and start with what you know. As you add details or change your mind, `cat-plan` keeps investigating and updates the same plan. When it looks right, hand it to `cat-code`.

## License

MIT — see [LICENSE](LICENSE).
