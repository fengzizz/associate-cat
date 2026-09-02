# Output Language Terminology for Cat Code

This reference is the controlled display vocabulary for Cat Code. Each row has the form `key | class | source_zh_cn | en | context`. The zh-CN and English values are normative in their controlled positions. A `heading`, `label`, or `status` is exact in its matching structural slot. A `pattern` keeps its fixed text exact and varies only its declared content slots. A `concept` is exact when formally naming the mechanism, defining the rule, presenting terminology comparison, or filling a structured mechanism field; ordinary explanatory prose may inflect or rephrase it naturally while preserving the same identity, responsibility, and boundary. Keep the Progress, Validation, Mapping, Forward, Reverse, and Closure sets independent even when two display values are identical.

## Status and Result Sets

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.status.progress.not_started` | status | 未开始 | Not Started | The implementation item has not started. |
| `code.status.progress.in_progress` | status | 进行中 | In Progress | The implementation item is currently active. |
| `code.status.progress.blocked` | status | 阻塞 | Blocked | Progress cannot continue because of an external or boundary blocker. |
| `code.status.progress.completed` | status | 已完成 | Completed | Implementation is landed and has passed current lightweight checks; required validation may still be pending. |
| `code.status.progress.verified` | status | 已验证 | Verified | Required and authorized validation has passed. |
| `code.status.validation.verified` | status | 已验证 | Verified | The validation was run and supports the conclusion. |
| `code.status.validation.failed` | status | 失败 | Failed | The validation was run and did not meet expectations. |
| `code.status.validation.not_verified` | status | 未验证 | Not Verified | Required validation was not run or evidence is insufficient. |
| `code.status.validation.blocked` | status | 阻塞 | Blocked | Validation cannot run or complete because of an external or boundary blocker. |
| `code.status.validation.not_applicable` | status | 不适用（附依据） | Not Applicable (with rationale) | The validation is irrelevant and includes a rationale. |
| `code.status.mapping.mapped` | status | 已映射 | Mapped | The Plan element has a complete implementation or validation mapping. |
| `code.status.mapping.not_applicable` | status | 不适用（附依据） | Not Applicable (with rationale) | The Plan element has no mapping obligation and includes a rationale. |
| `code.status.mapping.blocked` | status | 阻塞 | Blocked | The Plan element cannot yet be mapped. |
| `code.status.card.complete` | status | 完整 | Complete | The implementation or validation card contains all required information. |
| `code.status.card.missing` | status | 缺失 | Missing | Required implementation or validation card information is absent. |
| `code.status.card.not_applicable` | status | 不适用（附依据） | Not Applicable (with rationale) | The card is irrelevant and includes a rationale. |
| `code.status.forward.conforms` | status | 符合 plan | Conforms to Plan | The Plan element is implemented with matching structure and behavior. |
| `code.status.forward.structural_deviation` | status | 结构偏离 | Structural Deviation | The implementation structure differs from the Plan. |
| `code.status.forward.behavior_gap` | status | 行为或边缘路径缺口 | Behavior or Edge-Path Gap | Required behavior or an edge path is missing. |
| `code.status.forward.not_implemented` | status | 未落地 | Not Implemented | A required Plan element has not been implemented. |
| `code.status.reverse.traceable` | status | 可追溯 | Traceable | The actual change maps to the Plan or current explicit request. |
| `code.status.reverse.structural_deviation` | status | 结构偏离 | Structural Deviation | The actual structure diverges from its Plan basis. |
| `code.status.reverse.unplanned` | status | 计划外实现 | Unplanned Implementation | An actual change cannot be traced to the Plan or current explicit request. |
| `code.status.closure.closed` | status | 已闭环 | Closed | All required implementation and validation obligations are satisfied. |
| `code.status.closure.not_closed` | status | 未闭环 | Not Closed | One or more required items are failed, not verified, blocked, or incomplete. |

## Structured Output Headings

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.heading.mapping` | heading | 计划项映射 | Plan Item Mapping | Structured mapping for one implementation Task. |
| `code.heading.scope_blocker` | heading | 超范围阻塞 | Out-of-Scope Blocker | Report emitted before any out-of-scope modification. |
| `code.heading.forward_check` | heading | plan → 代码正向核对 | Plan → Code Forward Check | Checks that every required Plan element was implemented. |
| `code.heading.reverse_check` | heading | 代码 → plan 反向核对 | Code → Plan Reverse Check | Checks that every actual change is traceable. |
| `code.heading.validation_closure` | heading | 验证闭环 | Validation Closure | Validation state and closure summary. |
| `code.heading.blockers` | heading | 阻塞与未完成项 | Blockers and Incomplete Items | Remaining facts, permissions, scope, or implementation work. |

## Plan Item Mapping Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.mapping.related_decision` | label | 对应决策/约束 | Related Decision / Constraint | Link to a current Decision or Sourced Constraint. |
| `code.mapping.design_source` | label | 当前设计来源 | Current Design Source | Link to Current Design, Target, or another exact anchor. |
| `code.mapping.task_responsibility` | label | `Task` 责任边界 | `Task` Responsibility Boundary | The Task's single primary responsibility. |
| `code.mapping.ordered_steps` | label | 有序代码步骤 | Ordered Code Steps | Ordered object, Action, and semantic-effect steps. |
| `code.mapping.card` | label | 实现/验证卡片 | Implementation / Validation Card | Uses the Card completeness set. |
| `code.mapping.element_coverage` | label | 逐元素消费 | Element-by-Element Coverage | Plan elements mapped to code locations or explicit non-applicability. |
| `code.mapping.validation` | label | 验证映射 | Validation Mapping | Acceptance items mapped to validation methods and status. |
| `code.mapping.code_files` | label | 代码文件 | Code Files | Actual code files for the current Task. |
| `code.mapping.key_objects` | label | 关键对象 | Key Objects | Actual key objects or symbols for the current Task. |
| `code.mapping.authorized_scope` | label | 本轮允许修改的范围 | Authorized Change Scope | Exact objects that may be modified in the current batch. |
| `code.mapping.batch_scope` | label | 用户当前实施目标与本轮范围 | Current Implementation Goal and Batch Scope | Current requested goal and batch boundary. |
| `code.mapping.boundary_source` | label | 边界来源 | Boundary Source | Explicit scope, named object, derived step, or provisional boundary. |
| `code.mapping.behavior` | label | 行为变化 | Behavior Change | Actual observable behavior change. |
| `code.mapping.progress` | label | 进度快照 | Progress Snapshot | Uses the Progress set. |

## Out-of-Scope Blocker Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.blocker.current_item` | label | 当前计划项 | Current Plan Item | Current Task or direct request. |
| `code.blocker.current_status` | label | 当前状态 | Current Status | Always carries Blocked semantics in this report. |
| `code.blocker.impact_surface` | label | 新增影响面 | Additional Impact Surface | Newly discovered required objects or relationships outside the boundary. |
| `code.blocker.reason` | label | 必要原因 | Why It Is Required | Factual reason why the impact surface is necessary. |
| `code.blocker.consequence` | label | 若不修改的后果 | Consequence of Not Changing | Concrete consequence of preserving the current boundary. |
| `code.blocker.required_objects` | label | 当前允许修改范围未覆盖的必要对象 | Required Objects Outside the Authorized Change Scope | Exact objects that are not authorized for modification. |
| `code.blocker.required_boundary` | label | 继续实施所缺少的范围、权限或计划边界 | Scope, Authorization, or Plan Boundary Required to Continue | Minimum new input needed to continue. |
| `code.blocker.modified` | label | 本次是否已实际修改上述内容 | Whether the Above Content Was Modified | Must be No or 否 for the newly discovered out-of-scope content. |

## Batch Review and Delivery Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.review.plan_element` | label | 计划元素 | Plan Element | Design, Task, change point, or validation item under forward review. |
| `code.review.code_location` | label | 实际代码落点 | Actual Code Location | Actual file, object, member, or relationship. |
| `code.review.result` | label | 核对结果 | Check Result | Uses only the Forward check result set in forward review. |
| `code.review.impact` | label | 影响 | Impact | Effect of a deviation, gap, or unimplemented element. |
| `code.review.traceable_pattern` | pattern | `<实际对象> → TASK-... / Decision-...：可追溯` | `<Actual Object> → TASK-... / Decision-...: Traceable` | Reverse-check traceable pattern; IDs remain fixed. |
| `code.review.unexpected_pattern` | pattern | `<意外对象>：结构偏离 / 计划外实现` | `<Unexpected Object>: Structural Deviation / Unplanned Implementation` | Reverse-check deviation pattern. |
| `code.review.acceptance_item` | label | 验收项 | Acceptance Item | Uses the Validation set. |
| `code.review.summary` | label | 总结 | Summary | Uses the Closure set. |

## Controlled Core Concepts

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.concept.explicit_request` | concept | 本轮明确实施请求 | Explicit Request to Implement | Current user request that authorizes entering an implementation route. |
| `code.concept.authorization` | concept | 本轮实施授权 | Implementation Authorization | Current-turn write authorization; never inferred from Ready. |
| `code.concept.entry_criteria` | concept | 开始实施的准入条件 | Implementation Entry Criteria | Facts, readiness, dependencies, and boundaries required before writes. |
| `code.concept.authorized_scope` | concept | 本轮允许修改的范围 | Authorized Change Scope | Exact write boundary, not all related code. |
| `code.concept.minimal_baseline` | concept | 最小实施基线 | Minimal Implementation Baseline | Internal baseline for a direct scoped request. |
| `code.concept.batch` | concept | 实施批次 | Implementation Batch | Continuous work inside one closed boundary. |
| `code.concept.pre_task_check` | concept | 任务开始前检查 | Pre-Task Check | Internal Task gate, not repeated user approval. |
| `code.concept.stop_condition` | concept | 必须停止的条件 | Stop Condition | Mandatory stop for design, fact, authorization, or scope conflict. |
| `code.concept.local_validation` | concept | 局部验证 | Local Validation | Risk-proportionate Task validation, not full regression. |
| `code.concept.validation_task` | concept | `Validation Task` | `Validation Task` | Validation-only Task whose Authorized Change Scope is empty. |
| `code.concept.plan_to_code_handoff` | concept | `Plan-to-Code Handoff` | `Plan-to-Code Handoff` | Element-level handoff from Plan to implementation and validation. |
| `code.concept.forward_verification` | concept | 按计划逐项核对实现 | Plan-to-Code Verification | Forward check for omitted Plan requirements. |
| `code.concept.reverse_traceability` | concept | 按代码改动反查计划依据 | Code-to-Plan Traceability Check | Reverse check for unplanned implementation. |
| `code.concept.outside_plan` | concept | 计划尚未覆盖的必要影响范围 | Required Changes Outside the Plan | Required impact outside the current Plan or request, reported before expansion. |
