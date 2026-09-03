# Associate Cat

[在 skills.sh 查看两个 Skill](https://skills.sh/fengzizz/associate-cat) · [cat-plan](https://skills.sh/fengzizz/associate-cat/cat-plan) · [cat-code](https://skills.sh/fengzizz/associate-cat/cat-code)

[English](README.md)

Associate Cat 是一个面向协作式 AI 编程的轻量、基于 Skill 的工作流 Harness。它在你常用的 Coding Agent 里工作，把项目上下文、方案、实施范围和验证一路串起来。

你可以从一个零碎的想法、一个还没定位清楚的 Bug，或一项尚未想完整的改动开始。像平常和同事沟通一样，先把你知道的告诉 AI。`cat-plan` 会查看项目、找出相关部分，把讨论整理成一份可以继续修改的计划；方向确定后，再让 `cat-code` 按计划实施并验证结果。

## 为什么选择 Associate Cat

- **不用先把需求想得很完整。** 有几个想法、一个期望结果，或者只是眼前的 Bug 现象，就足够让 `cat-plan` 开始工作。
- **方案从真实项目里长出来。** 两个 Skill 会阅读相关规则、代码、文档和验证路径，而不是只围着提示词凭空作答。
- **从问题出发，不把整个仓库翻一遍。** `cat-plan` 只沿相关代码和依赖查下去，弄清问题就收住。
- **写代码前先把方向看明白。** 理解有偏差、范围不合适时，都可以直接改计划，不用等代码写完再返工。
- **任务多大，流程就多大。** 只想分析可以停在 Plan；复杂工作走 Plan→Code；小而明确的修改直接交给 `cat-code`。
- **方案定下来，实施就能接着做。** 已经谈清楚的设计、范围、任务和验证都放在同一份文档里，`cat-code` 可以直接接着做。

## Associate Cat 适合解决什么

- **还没完全想清楚的功能或改动。** 把零碎想法交给 `cat-plan`，让它结合项目把细节想清楚，整理成能审阅、能实施的方案。
- **Bug、重构、代码评审或设计评审。** 让 `cat-plan` 顺着相关代码和依赖查下去，最后可以只给分析结论，也可以继续整理成具体方案。
- **已经可以动手的工作。** 小而明确的修改直接交给 `cat-code`；复杂任务则把已经谈妥的计划交给它实施和验证。

游戏项目就是一个典型例子：工程规模大、状态复杂，涉及的领域多，许多小系统还会互相影响。`cat-plan` 可以从一个具体需求或 Bug 开始，找出真正相关的代码和项目规则，再整理成一份能审阅、能交接的方案。

Associate Cat 的仓库和文档也在使用这里介绍的 Plan→Code 工作方式。

## 快速开始

使用以下命令安装两个 Skill：

```bash
npx skills add fengzizz/associate-cat
```

出现选择提示时，同时选择 `cat-plan` 与 `cat-code`。想指定工作方式时，直接在请求里写出 Skill 名称即可。

如果要全局安装并跳过交互提示，运行与你使用的 Agent 对应的命令：

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

| Skill | 什么时候用 | 它会做什么 |
| --- | --- | --- |
| [`cat-plan`](skills/cat-plan/SKILL.md) | 想梳理需求、调查 Bug、讨论设计，或者做一次评审。 | 查看项目、理清问题、比较方案，最后给出分析结论或一份可以继续修改的计划。 |
| [`cat-code`](skills/cat-code/SKILL.md) | 小修改已经说清楚，或者方案已经定下来了。 | 按约定完成修改、运行相关检查，并说明改了什么、验证了什么。 |

`cat-plan` 是这套工作流的核心。你说目标和想法，它负责查项目、理清相关技术问题，再把结果整理成能审阅的方案。只想分析可以停在这里；需要落地时接上 `cat-code`，小而明确的修改也可以直接交给 `cat-code`。

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

不用一开始就把需求想完整，像和同事聊天一样，把目前知道的先说出来就行。你后面补充想法或改口，`cat-plan` 会继续调查并更新同一份方案；觉得没问题了，再交给 `cat-code`。

## 许可证

MIT，详见 [LICENSE](LICENSE)。
