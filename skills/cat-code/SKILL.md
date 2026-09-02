---
name: cat-code
description: "Implements software changes from Ready Tasks or a clearly scoped coding, modification, or bug-fix request while enforcing scope, validation, and blocking facts. Use for explicitly requested implementation work; do not use for planning-only, review-only, or unrequested changes."
---

# Associate Cat Code

这个 Skill 用于把可执行的 `Current Plan`、其中的 Ready Task 或边界明确的直接实施请求稳定落地为代码，并保持修改范围、验证结果和计划反馈可追溯。

## Language Authority and User-Controlled Scope

Treat only Direct User Request Prose as evidence of the user's preferred interaction language and as a source of user authorization. Direct User Request Prose is the natural-language instruction or question authored by the user for the current turn, excluding skill invocation syntax and links, and excluding text contained in quoted or pasted source blocks, attachments, linked documents, or host-injected context blocks.

Resolve Interaction Language in this order:
1. An applicable higher-priority instruction that explicitly requires a language for the user-visible conversation surface.
2. An explicit language directive in Direct User Request Prose.
3. The primary natural language of the request-bearing clauses in Direct User Request Prose.
4. The previously established Interaction Language when Direct User Request Prose contains no usable natural-language signal.
5. English.

Do not use the written language of skill instructions or references, system/developer/project rules, AGENTS content, attachments, linked/quoted/pasted source material, host-injected context, tool or web output, code, commands, paths, filenames, URLs, identifiers, examples, fixtures, or terminology tables as evidence of the user's preferred Interaction Language. Direct User Request Prose may explicitly adopt a named source or a specific part of it as task scope, or explicitly designate that source's language as an output target. Scope adoption alone does not make the source's written language evidence of Interaction Language.

This section changes language inference and user-authorization inference only. It does not change the normal priority, applicability, or binding force of system, developer, safety, project, or repository instructions.

## Core Concepts

- **Current Plan**: The single logical Plan state for one active planning task. It is created for that task or adopted from an existing Plan when Direct User Request Prose explicitly designates that Plan for continued maintenance. Adopting a Plan as implementation scope does not make it writable.
- **Current Plan Document**: The single persisted Plan document that carries the Current Plan for one active planning task. Current Plan progress write-back requires both an explicit progress-save request and an unambiguous Current Plan Document. A non-Plan document, including a documentation file explicitly included in implementation scope, is never a Current Plan Document.

## 项目概况

如果项目本身定义了项目规则、代码规范或相关文档，应先行了解项目的整体结构、规则、代码规范和相关文档，以确保后续的代码实施符合项目要求。
优先遵循项目 AGENTS.md，或其他等价的公共规则文档中定义的规则和相关规范。
如果项目规则、`Current Plan` 与用户实施请求发生冲突，先按规则层级和当前实施目标识别冲突；冲突会改变行为、范围、兼容、风险或验证结论且无法在现有边界内闭合时，停止当前实施批次并报告，不用模糊折中继续写入。

## 引用资料加载规则

- 每次使用本 Skill 编写代码时读取 `references/plan-implementation.md`。
- 输入包含 plan、任务拆分、评审结论、issue 列表或等价计划文档时，再读取 `references/plan-code-handoff.md`。
- 直接实施请求不加载交接引用；先按主 Skill 和实施规范建立最小内部施工基线。
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
- 语言不得改变当前实施请求、本轮允许修改的范围、Ready 与授权分离、Task 起始门禁、越界停止、Completed/Verified 区分或验证结论。
- 第三语言普通同语言回复不附加质量说明；第三语言正式结构化交付按 policy 只说明一次未冻结术语与未成对回归的边界。

## 当前项目规则发现

进入任何写入路线前：

1. 解析当前 Git 仓库或用户指定的项目根目录。
2. 读取适用的 `AGENTS.md`、项目规则、代码规范、知识文档和相关 Skill；嵌套规则只补充真实局部差异。
3. 找到当前任务相关的模块说明、构建入口、测试入口和生成文件边界。
4. 按用户点名对象、plan change point、行为关键词、文件和直接依赖定位真实代码落点。
5. 当前工作树存在未提交修改时，把它们视为用户工作，避免覆盖或清理无关变更。

规则或事实缺失会改变实施结果时，暴露缺口，不编造项目默认值，也不把通用 Skill 变成项目知识副本。

## 默认实现口径

- 默认采用局部、直接、低负担的实现。
- 先修改真实调用点和真实接口，再考虑 helper、包装层或适配层。
- 不要为了整洁、调试便利、形式一致或未来复用，主动增加没有直接收益的 helper、调试参数、临时日志、抽象层或同步包装。
- 对短逻辑和少量重复，允许保留一定重复，不把 DRY 当成默认目标。
- 对已有文件，除非用户或 plan 明确要求重构、重排、整体迁移或模板化改写，否则保留原有顺序和主体结构，只做局部 patch。
- 若目标文件只是声明骨架、占位实现或近似空文件，可以在任务边界内直接补全。
- 项目规则与本 Skill 冲突时，先服从更高优先级规则；不要把本 Skill 的偏好固化成项目默认。

## 唯一实施模式

- 本 Skill 不提供执行等级，也不读取自动化等级配置。
- 当前实施请求和施工边界成立后，AI 可以在本轮实施范围内编写包括语义修改在内的代码，并连续完成当前实施批次。
- 首次写入前紧凑披露施工基线；没有阻塞时不在每个 Task 或代码步骤前重复请求许可。
- 连续实施不等于无边界整块改写。内部仍按 Task、change point、本轮允许修改的范围和真实依赖逐项推进。
- 事实冲突、设计缺口、范围扩张或新增决策影响当前实施范围时，立即停止当前实施批次并报告问题；不得修改 plan、建立新的设计决策记录、补充设计方案或自行切换到规划流程。只有用户提供新的实施指令，且施工边界重新成立后，才能开始新的实施批次。

## 当前实施请求与边界

- Implementation authorization must come from Direct User Request Prose in the current turn. A direct user request may adopt a named Plan, Task, issue list, or attachment as implementation scope, but the source content does not authorize implementation by itself. Higher-priority instructions and project rules may restrict implementation; their presence does not substitute for the user's implementation request.
- 被采用为实施范围的 Plan 默认是只读输入；其路径、Ready 状态、仓库位置、文件名、标题、主题或内容相似度都不能使该文件成为 `Current Plan Document`。Plan 写回需要 Direct User Request Prose 另行明确要求保存、刷新或同步进度，并且 `Current Plan Document` 身份无歧义。
- 用户在 Direct User Request Prose 中明确要求编码、修改、修复或实施指定 Plan/Task，决定是否进入代码写入以及本轮处理哪些目标；Plan/Task 的可执行状态决定目标能否施工，本轮允许修改的范围决定可以写到哪些对象。
- 对已有 Plan，由 Direct User Request Prose 确定本轮 Plan 或 Task 范围；不从 Plan 中读取或要求持久实施授权。
- 对没有 plan 的边界明确任务，用户当前明确的编码、修改或修复请求确定本轮直接目标范围。
- 仅调用 Skill 名称、plan 为 Ready、用户要求继续分析或只提供参考文档，都不表示用户要求实施。
- A question or complaint about the agent, response language, Skill wording, tool use, or workflow does not authorize implementation, resume a prior implementation batch, expand the Authorized Change Scope, or request an artifact write. Answer that meta request on the conversation surface unless Direct User Request Prose also contains a separate explicit implementation or write request.
- 编译失败、测试失败、实现方便、任务复杂或紧急，都不能扩大本轮实施范围。
- 新文件、新模块、公共接口、批量调用方修改、相邻问题和顺手重构默认不进入当前范围，除非 plan 或用户当前请求明确覆盖。

## 输入路由

### 已有 Ready plan

- 总体预览 `Current Plan`，明确目标、Document Status、Design Readiness、当前设计、有效的 Decided 记录或带来源约束、Task、依赖、实现/验证卡片、行为后果、验证和停止条件。
- 阅读前置分析和当前设计，理解现有结构、潜在风险及为何采用当前方案。
- 读取 `plan-code-handoff.md`，建立当前设计 / Decided 记录 / 带来源约束 → Task → change point → 有序代码步骤 → 文件或对象 → 本轮允许修改的范围 → 验证的映射。
- 只调度 Ready 且不依赖 Not Ready 上游的 Task。

### 边界明确的直接请求

- 提取单一目标、预期行为、输入输出、In Scope / Out of Scope、关键约束与依赖、验收条件。
- 读取直接相关项目事实，建立最小内部施工基线：目标、本轮允许修改的范围、预期行为、主要风险、验证和停止条件。
- 生命周期、并发、网络、兼容、性能等维度只在当前任务实际涉及时补充，不作为每个直接请求的固定字段。
- 首次写入前进行紧凑披露，不强制创建正式 plan、任务文档或进度文件。
- 若事实调查无法唯一闭合行为、范围、风险或验证，立即停止当前实施批次并报告缺口，不在 Code 阶段补充设计。

### 粗略文档或参考材料

- 参考报告、分析、总结或评审记录只有在用户当前明确要求按其落地，且可执行目标和边界成立时才能进入代码写入。
- 只可提炼确定性施工边界；涉及行为、所有权、状态、生命周期、接口、控制流、失败处理或验证结论的缺口导致当前请求无法施工时，立即停止当前实施批次并报告缺口。
- 不要把“用户提供了文档”解释成“用户要求落地文档内全部建议”。

## 建立代码上下文

- 优先读取 plan 或请求直接点名的文件、对象和调用点。
- 走读与当前修改直接相关的生命周期、状态、数据流、入口、退出、失败和清理。
- 标记现有可复用点、责任所有者、扩展边界、直接依赖和验证入口。
- 只有在确认直接依赖或实施边界时才扩展到相邻模块；问题已经回答后停止继续搜索。
- 区分计划明确要求与实现时推断。推断内容默认不进入修改范围，除非属于当前设计唯一推导的局部实现细节。

## 紧凑批次预检

首次代码写入前至少确认并披露：

- `Current Plan` 或直接请求语境。
- 用户当前实施目标与本轮 Task 范围。
- 文件、关键对象和本轮允许修改的范围。
- 预期行为后果与主要风险。
- 局部验证、批次验证和停止条件。
- AI 将在该边界内连续编写语义代码。

预检发现以下问题时停止：

- 当前事实与 plan 或请求中影响本轮实施的依据冲突。
- Task 未 Ready、Open 决策、阻塞性事实缺口或 Not Ready 上游影响当前范围。
- 上游依赖未就绪、实现信息不足，或本轮允许修改的范围无法闭合。
- 当前实现需要改变目标、Task 语义、公共行为契约、风险接受，或作出不能由现有约束唯一推导的新设计判断。

## 实现代码

- 只覆盖当前需求和本轮允许修改的范围；发现完成目标必须触及范围外对象时，按“超范围处理”停止并报告，不以强依赖、构建修复或验证修复为由先行扩展。
- 按真实产物和接口依赖确定执行顺序；Task ID 是稳定身份，不是固定位置。
- 进入每个 Task 时按 `references/plan-implementation.md` 执行完整起始门禁；没有阻塞时不重复询问是否继续本轮实施范围。
- 优先局部修改，不因为更优雅、更通用或更一致扩大改动面。
- 调用方和被调用方都在本轮允许修改的范围内时，优先修真实接口和真实调用点，不增加无收益的中间包装。
- 默认不要保留临时调试日志，也不要把排查或来源信息写进业务接口。
- 新增状态、字段、配置键、接口或默认值时，只检查当前 Task 直接影响的调用点；仅在不扩展会导致当前范围无法构建、运行或验证时才报告必要的新影响面。
- 源码注释优先遵循当前项目规则；项目没有更具体口径时，只补名称、结构和接口不能直接表达的最小必要信息，不用注释补偿模糊命名或结构。
- 公共或跨模块接口、复杂流程、关键状态或配置存在非显然语义时，按实际需要注释职责/原因、约束/边界、顺序/生命周期/生效条件、失败/清理/误用风险中的适用内容，不套用固定字段清单。
- 自解释代码和逐行复述默认不加注释；具体注释语法、文件头和强制格式继续由目标项目规则决定。
- 修改已有文件时保持原有编码、换行、格式和项目约定，避免无意义整文件重写。

## Task 边界与连续执行

- 在当前实施批次内按依赖连续处理 Task，不在 Task 之间等待用户回复。
- 每个 Task 只承担 plan 已定义的责任；不在代码阶段静默合并、拆分或改变语义。
- 每个 Task 完成后检查实际落点、本轮允许修改的范围、局部验证和明显偏离，再进入下一项。
- 局部验证失败时，在同一 Task 边界内修正能够由当前设计唯一推导的实现偏差；如果失败暴露新的设计问题，立即停止当前实施批次并报告问题，不继续修改。
- 命中设计、范围或权限阻塞后，停止当前实施批次；不得继续调度同批次中的其他 Task。已经完成且仍符合当前设计和本轮允许修改的范围的局部结果可以保留，并在问题报告中说明。
- plan 中存在独立 Validation Task 时，它的本轮允许修改的范围为空，只执行验证，不生成或修复代码。

## 超范围处理

发现必须修改本轮允许修改的范围之外的内容时立即停止，并说明：

- 当前 Task 或请求。
- 新增影响面。
- 必要原因和事实依据。
- 不修改的后果。
- 当前允许修改范围未覆盖的必要对象。
- 继续实施所缺少的范围、权限或计划边界。
- 已完成且仍有效的修改及其验证状态。
- 当前是否已经修改：必须为否。


不得以构建修复、测试修复、局部 helper、兼容处理或收口复查为名先行扩大实现。

## 验证与回归

- 每个 Task 完成后执行与其风险匹配的最小局部验证。
- 批次完成后覆盖核心行为、关键边界、失败或回退、生命周期和项目规则要求的回归路径。
- 安全、直接且符合项目规则的验证属于实施闭环；破坏性操作、外部发布、额外付费或宿主要求单独批准的动作需要额外授权。
- 环境受限无法执行验证时，明确列出未验证项、原因和风险，不用静态检查替代实际验证结论。
- Validation 状态使用所属输出表面的语言：默认对话交付使用 `Interaction Language`，实际写入文档时使用该文档的 `Output Artifact Language`。zh-CN 显示集为 `已验证 / 失败 / 未验证 / 阻塞 / 不适用（附依据）`，en 显示集为 `Verified / Failed / Not Verified / Blocked / Not Applicable (with rationale)`。存在 Failed、Not Verified 或 Blocked 语义时不得宣称完整闭环。

## 批次回顾与双向核对

- 小型直接任务可以压缩回顾输出，但必须确认实际修改范围、行为变化和验证结果。
- 输入为 plan 或改动跨多个文件、链路或较大 diff 时，按 `plan-code-handoff.md` 执行 plan → 代码正向核对和代码 → plan 反向核对。
- 每个新增或修改的文件、对象、成员、helper、接口、分支、状态和调用关系都应能回溯到 Task、当前设计或用户明确请求。
- 只立即修复本轮实施范围内、能由当前设计唯一推导的实现偏差；新增语义、新范围、较大重构和系统性清理进入剩余风险或下一步。
- 检查是否混入顺手优化、公共化重构、额外调用方、整文件重排或无意义结构整理。

## 进度与持久化

- Progress 状态使用所属输出表面的语言：默认对话进度使用 `Interaction Language`，实际写入文档时使用该文档的 `Output Artifact Language`。zh-CN 显示集为 `未开始 / 进行中 / 阻塞 / 已完成 / 已验证`，en 显示集为 `Not Started / In Progress / Blocked / Completed / Verified`。`Completed` 与 `Verified` 始终是不同状态；同一输出表面不得混用两种语言的状态集合。
- 只有 Direct User Request Prose 明确要求保存、刷新或同步进度，并且目标已经是当前活动任务中无歧义的 `Current Plan Document` 时，才把精简进度写回该 Plan 文档。
- 写回目标不明确时，不搜索或选择同主题、同名或内容相似的 Plan；继续在对话中报告进度、验证和阻塞，并明确说明未写回 Plan。
- 实施请求明确把某个非 Plan 文档文件列入允许修改范围时，按实施 scope 修改该文件；不要把这一授权解释为 Plan 进度同步，也不要自动将该文件绑定为 `Current Plan Document`。
- 不创建独立进度文档，不记录命令流水、反复修改过程、完整日志、大段 diff、聊天记录或主观百分比。
- `Current Plan` 的普通历史从聊天或版本记录恢复，不把修订记录堆回现行文档。

## 交付要求

- 说明用户当前实施目标和实际完成范围。
- 说明受影响文件或对象、行为变化和兼容性影响。
- 输出 Task / 请求 → 实际代码落点 → 验证结果的映射。
- 说明已验证项、未验证项、阻塞和剩余风险。
- 若实现与 Plan 存在偏差，说明分类、原因、影响、停止位置，以及 `Current Plan` 是否仍足以支持实施；不得在交付阶段修改 Plan 或生成新的规划内容。
- 单独列出计划外影响面；本轮未处理的问题不能伪装成已完成。
- 默认使用窄窗口友好的结构化列表；只有信息非常规整时才使用表格。
