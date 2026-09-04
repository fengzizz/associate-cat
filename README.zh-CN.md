# Associate Cat

[在 skills.sh 查看两个 Skill](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[English](README.md)

Associate Cat 是一套轻量级的 AI 编程工作流 Harness，由 `cat-plan` 和 `cat-code` 两个 Skill 组成。安装后，你可以在常用的编程 Agent 中和 AI 一起分析问题、讨论方案，再让它按照方案修改代码，并验证修改的结果。

即使你只有一个初步的想法，或者遇到了一个还没查清原因的 Bug，也可以先开始讨论。就像平时和同事交流一样，把你已经知道的情况告诉 AI，其他细节可以在讨论的过程中慢慢补充。

`cat-plan` 是这套工作流的核心。它会阅读相关的代码和资料，分析有哪些可行的做法，并整理出一份可以审阅和修改的方案。你可以看看它有没有理解你的需求，提出的设计是否合理，再告诉它哪些地方需要调整。等方案确定下来，你就可以让 `cat-code` 按照方案实施。

## 为什么选择 Associate Cat

- **想法还没有成熟，也可以开始讨论。** 你只需要先说明自己想做什么，或者遇到了什么问题，`cat-plan` 就可以和你一起把需求以及具体的做法想清楚。
- **结合你的项目来制定方案。** 两个 Skill 都会先阅读相关的代码、项目规则和文档，了解项目现有的验证方法，再根据这些信息提出建议。
- **围绕当前的问题，有针对性地调查。** `cat-plan` 会从与你的问题有关的代码入手，继续查看相关的依赖，把调查集中在与任务有关的部分。
- **在写代码之前，先看看方案是否合理。** 如果 AI 理解错了需求，把某项功能安排在了不合适的模块里，或者把修改的范围定得太大，你都可以先让它调整方案。
- **根据任务的需要，选择合适的工作方式。** 如果你只想分析问题，可以单独使用 `cat-plan`；小改动可以直接交给 `cat-code`；比较复杂的任务，则可以先讨论方案，再开始实施。
- **讨论好的方案，可以交给 Cat Code 继续实施。** 已经确定的设计、修改范围、任务和检查要求，都会记录在同一份方案文档里。`cat-code` 可以按照这些约定完成修改，并检查结果是否符合要求。

## Cat Plan 的特点

- **从需求到任务，各个阶段的结果都可以审阅。** 方案文档会写清楚 AI 对需求的理解、限定的工作范围，以及对相关内容的分析结果，并给出具体的设计方案和任务拆分。你可以逐项检查这些结果，发现理解或设计上的偏差时，及时提出修改意见。
- **在讨论方案的过程中，逐渐想清楚需求。** 看过方案以后，你可能会发现之前遗漏的条件，也可能会有新的想法。这时可以直接提出意见，让 Cat Plan 继续修改这份文档。以后再讨论这个问题时，也可以从同一份方案接着往下想。
- **让 AI 先把能够自行查明的问题查清楚。** 对于项目里可以找到答案的问题，Cat Plan 会先去调查，常规的技术选择也会由它来分析。遇到业务上的取舍、你的个人偏好，或者需要你判断是否接受某项重要风险时，它会请你来决定。

Cat Plan 最初是作者为自己的项目编写的 Skill。在长期使用和反复调整之后，作者把其中的做法整理成了通用版本，供其他项目使用。

**使用建议：** 如果一个任务需要反复讨论，或者要分几次才能完成，你可以让 AI 把方案保存为 Markdown 文件，并在后续讨论中持续更新这份文档。这样，你就可以随时查看方案、提出修改意见，等到准备实施的时候，再让 Cat Code 按照方案执行。

## 适用场景

- **还在酝酿中的功能或改动。** 你可以先说说自己的目标和想法，再和 `cat-plan` 一起讨论有哪些可行的做法。
- **Bug 调查、重构、代码评审或设计评审。** 你可以让 `cat-plan` 查看相关的代码和依赖，先给出分析结论。如果需要进一步修改，再让它整理成具体的方案。
- **修改要求已经明确的任务。** 对于简单、明确的小改动，可以直接让 `cat-code` 动手；如果已经有了确定的方案，也可以让它按照方案实施。

UE5 游戏项目就是一个例子。这类项目往往代码量比较大，状态也比较复杂，各个系统之间还有不少依赖。Cat Plan 可以从当前的需求或 Bug 出发，分析相关的代码和项目规则，说明哪些地方需要修改，以及应该怎么改。其他大型项目，或者需要长期维护的代码库，也可以采用这种工作方式。

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
| [`cat-plan`](skills/cat-plan/SKILL.md) | 需要分析需求、调查 Bug、讨论设计，或者进行评审时。 | 阅读相关的代码和资料，比较不同的做法，再给出分析结论或修改方案。 |
| [`cat-code`](skills/cat-code/SKILL.md) | 修改要求已经说清楚，或者已经有了一份可以实施的方案时。 | 在约定的范围内修改代码，完成相关的检查，并说明检查结果。 |

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

## 许可证

MIT，详见 [LICENSE](LICENSE)。
