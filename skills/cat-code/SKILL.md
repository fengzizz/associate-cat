---
name: cat-code
description: "Implements software changes from Ready Tasks or a clearly scoped coding, modification, or bug-fix request while enforcing scope, validation, and blocking facts. Use for explicitly requested implementation work; do not use for planning-only, review-only, or unrequested changes."
---

# Associate Cat Code

Cat Code implements explicitly requested software changes within a clear scope and verifies the results. It is primarily designed to work with Cat Plan, turning executable plans and Ready Tasks into code. It also supports standalone use for clearly scoped coding, modification, or bug-fix requests without requiring a Plan. In either workflow, it keeps changes traceable to the implementation scope and reports validation results, blockers, and remaining work.

## Language Authority and User-Controlled Scope

Treat only Direct User Request Prose as evidence of the user's preferred interaction language and as a source of user authorization. Direct User Request Prose is the natural-language instruction or question authored by the user for the current turn, excluding skill invocation syntax and links, and excluding text contained in quoted or pasted source blocks, attachments, linked documents, or host-injected context blocks.

Resolve Interaction Language in this order:
1. An applicable higher-priority instruction that explicitly requires a language for the user-visible conversation surface.
2. An explicit language directive in Direct User Request Prose.
3. The primary natural language of the request-bearing clauses in Direct User Request Prose.
4. The previously established Interaction Language when Direct User Request Prose contains no usable natural-language signal.
5. English.

Do not use the written language of skill instructions or references, system/developer/project rules, AGENTS content, attachments, linked/quoted/pasted source material, host-injected context, tool or web output, code, commands, paths, filenames, URLs, identifiers, examples, fixtures, or terminology tables as evidence of the user's preferred Interaction Language. Direct User Request Prose may explicitly adopt a named source or a specific part of it as task scope, or explicitly designate that source's language as an output target. Scope adoption alone does not make the source's written language evidence of Interaction Language.

This section changes language inference and user-authorization inference only. It does not change the normal priority, applicability, or binding force of system, developer, safety, project, or repository instructions. The current-turn definition determines the language signal and any new authorization in that message; it does not expire authorization already established for the active task. Authorization continuity follows 当前实施请求与边界 below.

## 引用资料加载规则

- 用户采用正式 Plan 或带有任务状态与依赖的任务清单时，读取 `references/plan-implementation.md` 和 `references/plan-code-handoff.md`，复用选中范围的设计、约束、任务和验收。
- 只有明确目标或可执行修改意见、没有正式 Task 协议的材料，按直接请求处理，保留必要来源；背景材料不触发计划交接，也不能借直接请求路线绕过显式 Not Ready 或 Open。
- 只在缺少可发现的施工事实时读取 `references/implementation-baseline.md` 的相关章节。已有依据直接复用，不因缺模板字段、多文件或较大 diff 重建基线或启动规划。
- 项目语言、框架、代码风格、构建和测试规则从当前项目自身发现，不在本 Skill 中预设或复制。
- 按 `Language Authority and User-Controlled Scope` 先解析 `Interaction Language`，再为每个实际写回的 Plan/文档分别解析 `Output Artifact Language`。输入 Plan 的主语言只是 `Source Artifact Language`；除非本轮明确写回该 Plan，否则它不成为 output artifact，也不得覆盖 `Interaction Language`。既有维护对象在用户未明确指定该 artifact 语言时保持其主语言；新建交付物在用户未明确指定时采用 `Interaction Language`。
- 预检、进度、阻塞、计划项映射、双向核对、Validation 说明和交付摘要默认属于对话表面，使用 `Interaction Language`；只有用户明确要求写入某个文档时，该区块才使用该文档的 `Output Artifact Language`。
- “回复/说明/进度使用 X”只设置 `Interaction Language`；“Plan/文档使用 X”只设置被点名交付物；“翻译这份文档为 X”只切换该文档；未限定对象的“用 X 输出/全程用 X”设置本轮交互和新建交付物，但不自动翻译既有维护对象。
- 任意语言普通同语言回复，以及以异语言 Plan 为只读实施输入但不要求切换、翻译、对照或第三语言正式结构的场景，不读取 policy。目标对话结构需要 en 受控显示时只读取 terminology。
- 只有显式语言切换、翻译、双语或术语对照，对话语言与实际输出 artifact 语言不同，语言指令作用域无法唯一解析，或需要生成 zh-CN/en 之外的正式结构化表面时，才直接读取 `references/output-language-policy.md`。
- 只有目标输出表面需要非 zh-CN 的受控 heading、label、status、pattern、正式 concept，或用户要求术语对照时，才直接读取 `references/output-language-terminology.md`；第三语言正式结构化表面固定同时读取 policy 与 terminology。由本 SKILL 把语言规则与按原条件独立加载的 handoff/implementation 结构组合，且不得用同义词替换冻结显示值。
- 两个语言引用分别按自身条件加载；读取其中一个不触发另一个，也不通过 reference 间接加载 reference。

## 输出语言与本地化

- 预检、进度、阻塞、映射、双向核对、验证说明和交付默认属于对话表面，使用按 `Language Authority and User-Controlled Scope` 解析出的 `Interaction Language`；实际写入的 Plan/文档区块使用对应的 `Output Artifact Language`。
- 每个输出表面默认只使用一种自然语言；只有用户明确要求翻译、双语或术语对照时，才在该表面并列多种语言。
- 固定协议、ASCII ID、代码、命令、路径、文件名、symbol 和 URL 保持原文。zh-CN/en 受控结构按 terminology 对应列逐字显示；第三语言按 policy 建立本次交付映射；普通说明正文按所属输出表面的语言自然表达。
- 语言不得改变当前实施请求、本轮允许修改的范围、Ready 与授权分离、计划遵从、停止条件、Completed/Verified 区分或验证结论。
- 第三语言普通同语言回复不附加质量说明；第三语言正式结构化交付按 policy 只说明一次未冻结术语与未成对回归的边界。

## 当前项目规则发现

- 从当前 Git 仓库或用户指定项目根目录读取适用的 AGENTS.md、项目规则和任务相关文档，定位修改对象及已有验证入口。
- 只沿会影响当前目标、实施边界或验证的直接关系补充上下文；生命周期、并发、失败与清理等按实际涉及程度查看。
- 保留用户未提交修改，遵守项目的生成文件、只读目录和其他限制；不复制项目知识到本 Skill。

## 默认实现口径

有 Plan 时，尽量遵照计划要求实施，自行补齐计划未规定的普通实现细节。发现计划与实际情况冲突时，只作不改变既定目标和关键约束的必要局部微调，并简要说明。
发现严重错误或需要实质改变方案时，及时停止受影响实施并报告。具体判断由 AI 根据实际情况完成，不要求唯一推导或额外的分类流程。
无 Plan 时，在明确目标和范围内自主选择适当实现。

- 优先局部、直接的修改，复用项目现有模式；先处理真实接口和调用点，避免无收益的 helper、日志、包装和抽象，不把 DRY 作为默认目标。
- 遵照已有文件的编码、换行和主体结构，除任务要求外不重排或整文件改写；骨架或占位实现可在范围内补全。
- 命名和注释遵守项目规则；注释只补充名称与结构不能表达的必要职责、原因、约束或误用风险，避免逐行复述。

## 当前实施请求与边界

实施依据来自用户对活动任务的 Direct User Request Prose。
请求可以采用指定 Plan、Task 或材料作为范围，资料和 Skill 本身不提供授权。
已授权目标持续有效，直到完成、撤销或被替换；元问题不自动中止原工作，也不增加范围。
阻塞事实或边界已明确且仍在原授权内时可以继续，无需重复授权；用户明确暂停或要求等待时遵从其要求。

- 仅调用 Skill、提供参考资料、Ready 状态或只要求分析评审，都不授权实施。完成后的新任务或新增范围以用户的新请求为依据。
- 明确排除项和封闭文件范围必须遵守；调查得到的预计文件列表可随实际落点调整。同一目标所必需、且不改变既定契约和风险边界的直接修改可以一并完成，并说明有意义的变化。
- 有 Plan 时继续遵照计划，冲突仅作必要局部微调；超出目标或明确限制时停止相关写入并报告。
- 验证失败、实现方便或任务紧急不扩大实施范围；不要将相邻问题和顺手重构混入当前目标。
- 被采用的 Plan 默认只读，实施授权不包含 Plan 进度写回；具体写回规则见计划实施引用。

## 实施与核对

首次写入前简短说明处理范围和验证方向，已有清楚说明可复用。没有阻塞时连续实施，不逐 Task 重复披露或询问许可，也不强制创建 Plan、任务清单或进度文件。

有 Plan 时复用其任务安排和实际依赖；按计划实施与交接引用处理已有卡片。无 Plan 时读取必要事实后直接实施，不伪造 Task 或 Decision。出现影响前提的新事实时针对性补查，不固定重做完整起始门禁。

结合阅读、实施及最终 diff，核对目标是否落实、关键约束是否满足、是否混入无关改动和必要验证的结果。普通任务不要求逐元素台账或独立的正反两份报告；具体风险需要时再增加核对深度。范围内的实现偏差可以修正。

## 停止与问题报告

发现严重错误或需要实质改变计划时，及时停止受影响实施并报告，可以继续只读调查原因与影响。普通冲突仅在原目标和关键约束内局部微调；未受影响的已授权工作是否可安全继续由 AI 判断，影响无法隔离时停止整批。无 Plan 时，无法在现有依据内确定的目标、明确限制或关键行为同样需要停止相关写入并说明缺口。

可以简短说明建议方向，不擅自实施新设计、改写 Plan 或启动完整规划流程。报告实际问题、影响、已完成部分及继续所需的最小输入即可，不要求固定字段。若意外越界，如实说明，并安全纠正能与用户修改区分的自身改动，不隐瞒或盲目回滚。

## 验证与回归

执行用户、项目规则和所选任务要求的相关验证，按风险选择必要补充，优先复用已有检查。多个 Task 可以共享验证，已约定验收不能悄悄删去。没有新变化、失败或未解决风险时，不重复已足够的检查。

验证不增加写入权限；已有实施范围内的偏差可修正后复验，纯验证任务只报告结果。安全、直接且符合项目规则的验证属于实施闭环；破坏性操作、外部发布、额外付费或宿主要求单独批准的动作仍需对应授权。

环境受限时说明未验证内容、原因和影响，不以其他检查冒充缺失证据。验证失败影响目标正确性时不能把该目标标为已完成；与本次变更无关的既有失败应说明归属依据及影响。

Validation 状态使用所属输出表面的语言。zh-CN：`已验证 / 失败 / 未验证 / 阻塞 / 不适用（附依据）`；en：`Verified / Failed / Not Verified / Blocked / Not Applicable (with rationale)`。存在失败、未验证或阻塞时不得宣称对应范围完整闭环。

## 进度与持久化

Progress 状态使用所属输出表面的语言。zh-CN：`未开始 / 进行中 / 阻塞 / 已完成 / 已验证`；en：`Not Started / In Progress / Blocked / Completed / Verified`。Completed 表示目标实现已完成并通过适用的轻量检查，Verified 表示对应范围的必要验证已实际通过；实现完成但未执行必要验证时，明确说明未验证。同一输出表面不混用两种语言的状态集合。

实施请求明确包含非 Plan 文档时，可按范围修改该文档，不把它自动绑定为 Current Plan Document。默认在对话中报告进度，不创建独立进度文档，不堆积命令流水、完整日志或修订过程。

## 交付要求

简短说明实际完成范围、主要变化、必要局部调整、重要偏差和验证结果。未完成项、未验证内容及其影响如实列出；Task ID 或详细映射只在有助于理解时使用，不要求逐成员和分支追溯表。不要把计划外建议或尚未处理的问题写成已完成。
