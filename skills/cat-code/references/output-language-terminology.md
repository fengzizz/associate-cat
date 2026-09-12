# Output Language Terminology for Cat Code

This reference is the controlled display vocabulary for Cat Code. Each row has the form `key | class | source_zh_cn | en | context`. The zh-CN and English values are normative in their controlled positions. A `heading`, `label`, or `status` is exact in its matching structural slot. A `pattern` keeps its fixed text exact and varies only its declared content slots. A `concept` is exact when formally naming the mechanism, defining the rule, presenting terminology comparison, or filling a structured mechanism field; ordinary explanatory prose may inflect or rephrase it naturally while preserving the same identity, responsibility, and boundary. Keep Progress, Validation, Forward, Reverse, and Closure sets independent even when display values are identical. Use the following vocabulary only for controlled fields selected for an actual output. Its presence does not require a report, a template, a complete mapping, or additional implementation checks; ordinary explanations remain concise under SKILL.md.

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
| `code.status.forward.conforms` | status | 符合 plan | Conforms to Plan | The reviewed result satisfies the selected Plan requirements, including permitted local adjustments. |
| `code.status.forward.structural_deviation` | status | 结构偏离 | Structural Deviation | An actual structural departure from a Plan requirement, not merely a different unspecified implementation detail. |
| `code.status.forward.behavior_gap` | status | 行为或边缘路径缺口 | Behavior or Edge-Path Gap | Required behavior or an edge path is missing. |
| `code.status.forward.not_implemented` | status | 未落地 | Not Implemented | A required Plan element has not been implemented. |
| `code.status.reverse.traceable` | status | 可追溯 | Traceable | The actual change is supported by the selected Plan or an authorized request for the active task. |
| `code.status.reverse.structural_deviation` | status | 结构偏离 | Structural Deviation | An actual structural departure from a Plan requirement; significance follows SKILL.md. |
| `code.status.reverse.unplanned` | status | 计划外实现 | Unplanned Implementation | An actual change outside the selected Plan and authorized request; report its actual scope and effect. |
| `code.status.closure.closed` | status | 已闭环 | Closed | All required implementation and validation obligations are satisfied. |
| `code.status.closure.not_closed` | status | 未闭环 | Not Closed | One or more required items are failed, not verified, blocked, or incomplete. |

## Structured Output Headings

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.heading.mapping` | heading | 计划项映射 | Plan Item Mapping | Optional detail relating selected Plan requirements to actual results. |
| `code.heading.scope_blocker` | heading | 超范围阻塞 | Out-of-Scope Blocker | Optional report of an unresolved scope boundary; describe actual changes truthfully. |
| `code.heading.forward_check` | heading | plan → 代码正向核对 | Plan → Code Forward Check | Optional check of selected Plan requirements against actual results. |
| `code.heading.reverse_check` | heading | 代码 → plan 反向核对 | Code → Plan Reverse Check | Optional check of whether actual changes stay within the selected Plan or authorized request. |
| `code.heading.validation_closure` | heading | 验证闭环 | Validation Closure | Optional summary of the validation evidence and remaining gaps. |
| `code.heading.blockers` | heading | 阻塞与未完成项 | Blockers and Incomplete Items | Optional summary of blockers and incomplete work. |

## Plan Item Mapping Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.mapping.behavior` | label | 行为变化 | Behavior Change | Actual observable behavior change when relevant. |
| `code.mapping.progress` | label | 进度快照 | Progress Snapshot | Optional progress snapshot using the Progress set. |

## Batch Review and Delivery Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.review.plan_element` | label | 计划元素 | Plan Element | A selected design requirement, Task, change point, or acceptance item under review. |
| `code.review.code_location` | label | 实际代码落点 | Actual Code Location | An actual source-code location when source changes are being described; not required for non-source work. |
| `code.review.result` | label | 核对结果 | Check Result | When a forward or reverse result is reported, use its corresponding result set; no separate report is required. |
| `code.review.impact` | label | 影响 | Impact | Effect of a deviation, gap, or unimplemented element. |
| `code.review.acceptance_item` | label | 验收项 | Acceptance Item | An acceptance item whose validation status uses the Validation set. |
| `code.review.summary` | label | 总结 | Summary | Optional closure summary using the Closure set. |

## Controlled Core Concepts

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `code.concept.explicit_request` | concept | 本轮明确实施请求 | Explicit Request to Implement | Current user request that authorizes entering an implementation route. |
| `code.concept.authorization` | concept | 本轮实施授权 | Implementation Authorization | Authorization for the active task under SKILL.md; persists until completion, withdrawal, or replacement and is never inferred from Ready. |
| `code.concept.entry_criteria` | concept | 开始实施的准入条件 | Implementation Entry Criteria | Facts, readiness, dependencies, and boundaries required before writes. |
| `code.concept.authorized_scope` | concept | 本轮允许修改的范围 | Authorized Change Scope | Exact write boundary, not all related code. |
| `code.concept.minimal_baseline` | concept | 最小实施基线 | Minimal Implementation Baseline | Internal baseline for a direct scoped request. |
| `code.concept.batch` | concept | 实施批次 | Implementation Batch | Continuous work within the scope governed by SKILL.md. |
| `code.concept.stop_condition` | concept | 必须停止的条件 | Stop Condition | Stop for serious errors, substantive plan changes, or unresolved boundaries under SKILL.md; local adjustments follow the same rules. |
| `code.concept.local_validation` | concept | 局部验证 | Local Validation | Risk-proportionate Task validation, not full regression. |
| `code.concept.validation_task` | concept | `Validation Task` | `Validation Task` | Validation-only Task whose Authorized Change Scope is empty. |
| `code.concept.plan_to_code_handoff` | concept | `Plan-to-Code Handoff` | `Plan-to-Code Handoff` | Handoff of selected Plan goals, requirements, and acceptance to actual results; detail follows the task needs. |
| `code.concept.forward_verification` | concept | 按计划逐项核对实现 | Plan-to-Code Verification | Forward check for omitted Plan requirements. |
| `code.concept.reverse_traceability` | concept | 按代码改动反查计划依据 | Code-to-Plan Traceability Check | Reverse check for unplanned implementation. |
| `code.concept.outside_plan` | concept | 计划尚未覆盖的必要影响范围 | Required Changes Outside the Plan | Required impact outside the current Plan or request, reported before expansion. |
