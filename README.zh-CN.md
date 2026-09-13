# Associate Cat

[在 skills.sh 查看两个 Skill](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[English](README.md)

**把需求与问题，变成有依据、可审阅的方案。**

Associate Cat 以 `cat-plan` 为核心，帮助你分析需求、调查 Bug，并把相关事实、设计取舍和后续工作整理成可审阅、可持续修订的计划文档。它可在 Codex、Claude Code 等编程 Agent 中使用；需要动手修改时，配套的 `cat-code` 可以承接实施与验证。

**规范调查与交付，让模型自主求解。**

## 快速开始

```bash
npx skills add fengzizz/associate-cat
```

安装时选择 `cat-plan`；需要配套实施时，也可以选择 `cat-code`。直接生成并保存计划：

```text
用 cat-plan 分析以下需求，生成完整的实施计划文档并保存：……
```

```text
用 cat-plan 调查以下 Bug，生成完整的修复计划文档并保存：……
```

不必先把需求想完整。让 Cat Plan 先调查并提交一份完整的初步方案，你直接审阅其中的需求理解、分析依据与设计，再提出修改意见。后续讨论持续更新同一份文档，直到方案适合实施。尚不明确的条件也会写入方案，便于尽早发现和纠正问题。

可以先看[真实案例](#真实案例)了解产物，也可以查看[各 Agent 的安装方式](#各-agent-的安装方式)使用全局安装命令。

## 从需求分析与 Bug 调查开始

模型能力再强，也需要先弄清楚你的目标、项目现状与关键约束，以及怎样才算解决了问题。Cat Plan 把这项调查及其结论纳入交付内容。

| 你要处理的事情 | Cat Plan 关注什么 | 可以获得什么 |
| --- | --- | --- |
| 分析需求：开发功能、调整行为或规划重构 | 目标、现状、约束、可复用做法与设计取舍 | 需求理解、方案设计、工作范围和验证思路 |
| 调查 Bug、确定修复方向 | 复现条件、实际与预期行为、相关路径和根因证据 | 调查结论、修复建议、风险和待验证项 |
| 审阅代码或设计 | 职责、行为、依赖、兼容要求和改动影响 | 审阅结论或改进建议，以及依据 |

Cat Plan 也兼容非代码需求，例如文档改进、流程调整和一般方案规划。澄清目标、调查事实、比较做法和根据反馈修订方案，这些过程可以复用；调查对象与交付形式则随任务调整。

## 它如何发挥作用

**让 AI 调查并起草方案。** 从局部问题出发，Cat Plan 查清决定解法的项目事实和必要关联，形成范围明确的完整初稿。你不必先提供技术答案，也不必手动整理整个工程。

**从审阅方案开始参与判断。** 文档将需求理解、依据、假设、设计与后续工作组织在一起，让你尽早指出理解偏差或不合适的取舍。

**让讨论积累在同一份文档中。** 反馈进入受影响的设计、任务和验证，仍然成立的结论继续保留，方案在几轮审阅与修订中逐步收敛。

Cat 源自作者在大型工程中处理具体需求与 Bug 的实践，是日常使用技能的通用化版本。它保留了同一思路：从局部问题出发，查清必要关联，形成边界明确的方案，再通过审阅与修订逐步收敛。Associate Cat 自身也采用这套流程维护。

## 真实案例

下面两份案例展示 Cat Plan 在 Bug 调查与审阅中的具体产物：如何形成有依据的结论，以及如何确定该做和暂时不能做的工作。

- **[代码审阅纠偏：三项修复建议收敛为一项](examples/plans/plan_easy_debug_component_remediation.md)。** 重新检查实际调用方与引擎生命周期保证后，排除两项缺乏正常调用依据的修复，保留一项 HUD 事件路由修复。产物是一份范围更小、依据明确的实施方案；尚未编译或完成运行验证。
- **[Bug 调查：追踪 UE 5.8 Sphere Sweep 接触点偏移](examples/plans/plan_ue58_sphere_sweep_contact_offset.md)。** 从 `ImpactPoint` 异常追到 Chaos 中的坐标计算，并用独立计算复核。由于缺少真实调用位置和资产，方案保持 `Partially Ready`，项目侧接入方式仍待确定。

[案例说明](examples/plans/README.md)汇总三份公开方案的背景、结果和验证边界，其中还包括动画 Solver 的预期行为审阅。

## Harness Engineering：在复杂工程中明确问题与改动边界

Agentic Coding 的进展，让“怎样组织 AI 的工作”成为与模型能力同样值得关注的问题。Harness Engineering 关注目标、上下文、反馈和验证如何共同支持持续推进。Cat 把其中的工作流约束落实到规划阶段：让 AI 主动调查和提出方案，让用户尽早获得能够审阅、质疑和修订的完整产物。

**需求校正 → 前置分析 → 设计思路 → 决策 → 计划**

**从局部问题查清必要关联。** 大工程中的一个 Bug 或需求，往往牵涉触点之外的职责和依赖。Cat Plan 先明确要解决什么，再沿着会影响解法的关联调查，建立必要的项目上下文（Context Engineering）。分析可以跨越多个模块，最终改动仍可保持局部；边界由调查和设计形成，而不是只看最先指出的文件。

**把工程约束收敛为明确的方案。** 设计思路说明改动应落在哪里、可以复用什么、哪些约定需要保持；决策明确采用什么以及哪些工作不在本次范围内。完整计划再落实修改对象、任务和验证，让你检查范围是否过大、是否遗漏必要影响。这样的规范驱动协作（Spec-driven Development）从初步需求就能开始，你无需先替 AI 写好技术答案。

**让反馈改变下一版方案。** 第一版方案就是讨论的起点。你可以指出目标理解、事实依据或设计取舍上的问题，Cat Plan 回到受影响的位置修订，并同步后续任务与验证。人在回路中（Human-in-the-loop）的判断因此有明确落点；同一份持续维护的计划（Living Plan）记录当前有效的方案，需要实施时再由 Cat Code 按明确范围接续。

## 以 Cat Plan 为核心，按需衔接实施

| Skill | 什么时候用 | 会做什么 |
| --- | --- | --- |
| [`cat‑plan`](skills/cat-plan/SKILL.md) | 需要分析需求、调查 Bug、讨论设计，或者进行评审时。 | 核心规划技能：调查相关事实，形成并维护可审阅的计划文档。 |
| [`cat‑code`](skills/cat-code/SKILL.md) | 修改要求已经说清楚，或者已经有了一份可以实施的方案时。 | 配套实施技能：在约定范围内完成修改和检查，也能独立处理明确的小改动。 |

你可以单独使用 `cat-plan`，有需要时再搭配 `cat-code`。对于要求明确的小改动，也可以直接让 Cat Code 实施，不必先写一份方案。不过，Cat Code 只有在你明确要求实施之后，才会开始修改代码；把方案保存下来，并不代表你已经同意开始实施。

## 围绕计划文档持续推进

1. **生成并保存初稿。** 使用需求或 Bug 请求，让 Cat Plan 调查并提交可审阅的完整方案。
2. **审阅并修订同一份文档。** 先检查理解与范围，再看设计、任务和验证。例如：`这里优先复用现有模块，更新这份计划。`
3. **合适后再实施。** `用 cat-code 执行这份计划。`

需要跨会话继续时，让新会话读取并更新这份计划。反馈应进入正文中受影响的内容，而不是只追加在文档末尾。

你也可以只规划和修订、不实施；普通分析与审阅同样支持。明确的小改动可直接交给 Cat Code。实施后，它会说明改动和检查结果；遇到需要改变约定设计或范围的问题，会说明情况。

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

## 各 Agent 的安装方式

下面的命令会全局安装两个技能，并跳过交互式提示。根据自己使用的 Agent 选择对应命令：

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

## 其他说明

Cat 提供的是 Skill，不包含 Agent 运行时、自动跨会话记忆或无人值守交付保证。首版计划可以包含尚待确认的条件；影响方案的关键缺口仍需澄清。结果取决于模型、工具和可用的项目信息。

`此文档主要由 Cat AI 生成，可能存在不准确或不完整的地方。`

问题与建议欢迎到 [Discussions](https://github.com/fengzizz/associate-cat/discussions) 交流。

## 许可证

MIT，详见 [LICENSE](LICENSE)。
