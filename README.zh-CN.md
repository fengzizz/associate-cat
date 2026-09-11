# Associate Cat

[在 skills.sh 查看两个 Skill](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[English](README.md)

Associate Cat 是一套轻量级的 AI 编程工作流 Harness，由 `cat-plan` 和 `cat-code` 两个 Skill 组成。安装后，你可以在常用的编程 Agent 中和 AI 一起分析问题、讨论方案，再让它按照方案修改代码，并验证修改的结果。

即使你只有一个初步的想法，或者遇到了一个还没查清原因的 Bug，也可以先开始讨论。就像平时和同事交流一样，把你已经知道的情况告诉 AI，其他细节可以在讨论的过程中慢慢补充。

`cat-plan` 是这套工作流的核心。它会阅读相关的代码和资料，分析有哪些可行的做法，并整理出一份可以审阅和修改的方案。你可以看看它有没有理解你的需求，提出的设计是否合理，再告诉它哪些地方需要调整。等方案确定下来，你就可以让 `cat-code` 按照方案实施。

## 为什么选择 Associate Cat

- **还没想好具体做法，也可以开始。** 先说说你的目标，或者遇到了什么问题。Cat Plan 会先了解项目，和你一起把需求与可行的做法想清楚。
- **方案可以边看边改，讨论也可以接着进行。** 你可以先检查 AI 是否理解了需求、设计是否合适，再针对具体内容提出意见。已经形成的分析和设计会保留在当前方案中，方便下一次继续讨论。
- **讨论好的内容，可以交给 Cat Code 实施。** 确定的设计、修改范围、任务和检查要求都在方案里。你明确要求实施后，Cat Code 就按这些约定完成修改，并说明检查结果。

## 设计理念

**让思考有条理，让判断有依据。** 一个问题刚被提出时，需求、约束和解法往往还没有想清楚。Cat Plan 通过调查相关事实、限定范围和梳理设计，把原本含糊的问题整理成可以逐项讨论的内容。看到具体方案后，你可能发现遗漏的条件，也可能重新考虑最初的想法。这样，每一阶段的成果都能成为下一轮思考的依据，方案也在交流中逐渐成形。

**用稳定的框架，建立共同的理解方式。** 正式方案采用约定的结构，说明需求、范围、分析、设计、任务和验证。AI 有明确的交付要求，你也知道去哪里找依据、看设计、提意见。框架还让这些内容之间的关系更容易检查：设计是否回应了需求，任务是否落实了设计。双方可以针对具体内容提出修改、继续讨论，不必每次重新适应一种表达方式。

**让你的判断融入工作，让已有成果继续发挥作用。** Cat 让 AI 主动承担调查和技术分析，你则可以重点判断目标是否准确、设计是否合适，以及哪些取舍可以接受。反馈进入方案后，相关设计和任务应随之调整，仍然成立的分析则保留下来。需要实施时，再由 Cat Code 按明确的约定继续。从逐渐想清问题到确定做法，你都能通过具体成果参与判断，并让意见影响接下来的工作。

流程按任务需要展开。简单问题可以简短处理；影响方案的关键关系，则需要分析清楚。

Cat Plan 最初是作者为自己的项目编写的 Skill。在长期使用和反复调整之后，作者把其中的做法整理成了通用版本，供其他项目使用。

**使用建议：** 需要分几次讨论时，可以让 AI 将方案保存为 Markdown，并在后续讨论中持续更新。

## 适用场景

- **还在酝酿中的功能或改动。** 你可以先说说自己的目标和想法，再和 `cat-plan` 一起讨论有哪些可行的做法。
- **Bug 调查、重构、代码评审或设计评审。** 你可以让 `cat-plan` 查看相关的代码和依赖，先给出分析结论。如果需要进一步修改，再让它整理成具体的方案。
- **修改要求已经明确的任务。** 对于简单、明确的小改动，可以直接让 `cat-code` 动手；如果已经有了确定的方案，也可以让它按照方案实施。

一个局部 Bug、一项功能调整，或者一份需要重新组织的文案，都可以成为起点。

作者也将这套方法用于 UE5 游戏开发。面对 UE5 这样庞大而复杂的工程环境，提前规划的价值更为突出：Cat Plan 会主动梳理当前问题涉及的代码与依赖，明确修改范围和验证方式，再由 Cat Code 按方案实施。这也是 Harness 的作用所在：让 AI 在动手前建立必要的上下文，并在明确的边界内推进工作，帮助减少遗漏关联影响和改动范围不断扩大的问题。

Associate Cat 的仓库和文档，也是通过这套 Plan→Code 流程来维护的。

## 快速开始

使用以下命令安装两个 Skill：

```bash
npx skills add fengzizz/associate-cat
```

安装时，请选择 `cat-plan` 和 `cat-code`。需要使用哪个 Skill，就在请求里写出它的名称。

如果你想全局安装，并跳过交互式提示，可以根据自己使用的 Agent，运行下面对应的命令：

适用于 **Codex**：

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a codex -y
```

适用于 **Claude Code**：

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a claude-code -y
```

适用于 **Cursor**：

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a cursor -y
```

适用于 **Gemini CLI**：

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a gemini-cli -y
```

适用于 **GitHub Copilot**：

```bash
npx skills add fengzizz/associate-cat --skill cat-plan --skill cat-code -g -a github-copilot -y
```

## 选择 Skill

| Skill | 什么时候用 | 会做什么 |
| --- | --- | --- |
| [`cat‑plan`](skills/cat-plan/SKILL.md) | 需要分析需求、调查 Bug、讨论设计，或者进行评审时。 | 阅读相关的代码和资料，比较不同的做法，再给出分析结论或修改方案。 |
| [`cat‑code`](skills/cat-code/SKILL.md) | 修改要求已经说清楚，或者已经有了一份可以实施的方案时。 | 在约定的范围内修改代码，完成相关的检查，并说明检查结果。 |

你可以单独使用 `cat-plan`，有需要时再搭配 `cat-code`。对于要求明确的小改动，也可以直接让 Cat Code 实施，不必先写一份方案。不过，Cat Code 只有在你明确要求实施之后，才会开始修改代码；把方案保存下来，并不代表你已经同意开始实施。

## 选择工作方式

1. **仅规划（Plan only）**——适合分析、调查、设计或评审。

   ```text
   帮我看一下这个问题，用 cat-plan 分析并整理成方案文档，暂时别改代码：……
   ```

2. **先规划后实施（Plan→Code）**——适合想清楚方案后再动手的工作。

   ```text
   有个需求你先看一下，用 cat-plan 来分析，然后出个方案文档，并存盘：……
   ```

   有范围要求时，也可以顺手补一句：

   ```text
   嗯，模块 A 的问题你别管了，做模块 B 部分就行……
   ```

   审阅计划后，可以补充意见：

   ```text
   ……这里用现有的……就行，不用新建模块，按这个思路改一下计划。
   ```

   方案没问题后：

   ```text
   很好，用 cat-code 按这份计划实施。
   ```

3. **直接实施（Direct Code）**——适合说得清楚的小改动。

   ```text
   用 cat-code 把……改一下，改完跑一下……。
   ```

Cat Code 会自行处理约定范围内的实现细节。如果它发现需要改变已经确定的功能或设计、扩大修改范围，或者需要你决定是否接受某项风险，就会先停下来，把情况向你说明。完成修改之后，它也会告诉你做过哪些检查，还有哪些内容没有验证。

## 真实案例

下面两份 Plan 来自真实的 Codex 交互，可以直观看到这套工作流最终会产出什么：

- [审阅 Anim Retarget Magic 中一个 Solver 的预期行为](examples/plans/plan_fikrig_magic_thik_goal_solver_review.md)：从配置、Transform 计算和 Curve 输入，一直梳理到 Pose 输出、Editor 工具与旧版数据转换。它遵守了用户“不考虑缺陷”的范围要求，最后整理出四项可以执行的验证任务。
- [调查 UE 5.8 Sphere Sweep 的接触点偏移](examples/plans/plan_ue58_sphere_sweep_contact_offset.md)：从 `ImpactPoint` 异常一路追到 Chaos 中的具体计算，并用独立计算复核结果。由于还不知道实际调用位置和 Physics Asset，方案如实停在 `Partially Ready`，没有凭空补出项目侧修复方式。

[案例说明](examples/plans/README.md)列出了两次任务的原始请求、最终结果和验证边界。

## 许可证

MIT，详见 [LICENSE](LICENSE)。
