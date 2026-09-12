# 计划实施规范

本文件只补 Plan 的选择、状态、依赖、产物与写回。共用授权、范围、计划遵从、停止和验证规则遵守主 Skill；结果核对见 `plan-code-handoff.md`。

## Core Concepts

- **Current Plan**: The single logical Plan state for one active planning task. It is created for that task or adopted from an existing Plan when Direct User Request Prose explicitly designates that Plan for continued maintenance. Adopting a Plan as implementation scope does not make it writable.
- **Current Plan Document**: The single persisted Plan document that carries the Current Plan for one active planning task. Current Plan progress write-back requires both an explicit progress-save request and an unambiguous Current Plan Document. A non-Plan document, including a documentation file explicitly included in implementation scope, is never a Current Plan Document.

## 输入与任务选择

- 复用选中范围的前置分析、当前设计、约束、Task、实际依赖和验收，不重新规划。已有 Decided 记录或带来源约束均可作为依据，不强制新增 Decision。
- 只实施用户选中且就绪的任务；相关 Open、Not Ready 或关键事实缺口不能绕过。Draft 不等于禁止实施，Partially Ready 中不受阻塞影响的 Ready 子集可实施。
- 仅缺模板字段而内容足够时，不要求重写 Plan；明确状态与实质内容冲突时核实影响，不自行把 Not Ready 改成 Ready。无正式 Task 协议的材料按主 Skill 的直接请求路线处理。

## 连续实施与产物

理解选中目标、关键约束和实际依赖后连续实施，复用已有披露，不逐 Task 重复许可或执行完整固定门禁；出现影响前提的新事实时再针对性核实。

- 默认遵照 Plan 的任务责任、顺序和安排；实际依赖冲突仅作必要局部调整并说明。Task ID 是稳定身份；不静默合并、拆分或改变 Task 语义。共享编辑或验证不改变任务要求。
- 源码任务使用已有实现卡片；非源码任务使用对象与位置、目标内容、保留/禁止项、依赖和验收，不要求 symbol、代码步骤或源码 diff。
- 已经满足的目标经核实后报告无需修改，不为制造任务产物增加 diff。
- 既定安全中间状态继续遵守，不额外要求每个细步骤单独可编译。

## Validation Task

独立 Validation Task 的修改范围为空，按其目标、依赖、步骤和验收执行，不生成代码。验证发现的偏差只有在已有实施授权覆盖时才能回对应实施任务修复，再复验；严重错误或需实质改变方案时按主 Skill 停止。

## Plan 只读与进度写回

- 采用 Plan 作为实施依据不使其成为可写 Current Plan Document。仅在用户明确要求保存、刷新或同步进度，且 Current Plan Document 身份无歧义时，写回精简进度与验证结果；不更改设计和任务语义。
- 目标不明时在对话中报告，不按文件名、主题、仓库位置或内容相似度猜测写回对象。非 Plan 文档的实施授权不是 Plan 写回授权。
- 已有写回授权在对应活动任务中按主 Skill 的持续授权规则处理；不得据此自动翻译文档或扩大写回内容。
- Progress 和 Validation 使用主 Skill 的状态定义，不记录命令流水、完整日志、修订历史或主观百分比，不创建独立进度文档。
