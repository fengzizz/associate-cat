# Output Language Terminology for Cat Plan

This reference is the controlled display vocabulary for Cat Plan. Each row has the form `key | class | source_zh_cn | en | context`. The zh-CN and English values are normative in their controlled positions. A `heading`, `label`, or `status` is exact in its matching structural slot. A `pattern` keeps its fixed text exact and varies only its declared content slots. A `concept` is exact when formally naming the mechanism, defining the rule, presenting terminology comparison, or filling a structured mechanism field; ordinary explanatory prose may inflect or rephrase it naturally while preserving the same identity, responsibility, and boundary.

## Formal Plan Headings and Patterns

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `plan.heading.document_title` | pattern | `<任务名> 计划` | `<Task Name> Plan` | Required H1; only the task-name slot is variable. |
| `plan.heading.requirements` | heading | 需求校正与关键假设 | Requirement Correction and Key Assumptions | Required H2 section 1. |
| `plan.heading.scope` | heading | 目标与范围 | Goals and Scope | Required H2 section 2. |
| `plan.heading.analysis` | heading | 前置分析 | Preliminary Analysis | Required H2 section 3. |
| `plan.heading.benchmark_analysis` | heading | 直接对标分析 | Direct Benchmark Analysis | Optional H3 under Preliminary Analysis. |
| `plan.heading.implementation` | heading | 实现方案 | Implementation Approach | Required H2 section 4. |
| `plan.heading.design` | heading | 设计思路 | Design Approach | Required H3 under Implementation Approach. |
| `plan.heading.design_item` | pattern | `<业务机制>设计` | `<Business Mechanism> Design` | Optional repeated H4; only the mechanism slot is variable. |
| `plan.heading.decision_records` | heading | 设计决策记录 | Design Decision Records | Conditional H3 for real design divergence. |
| `plan.heading.decision_item` | pattern | `Decision-001：<决策事项>` | `Decision-001: <Decision Topic>` | Conditional repeated H4; the ID and punctuation are fixed. |
| `plan.heading.targets` | heading | 目标实现 | Implementation Targets | H3 for determined implementation targets. |
| `plan.heading.blocked_scope` | heading | 尚未具备交接条件的范围及其影响 | Blocked Scope and Impacts | Conditional H3 that never creates a formal Task. |
| `plan.heading.architecture` | heading | 架构细节 | Architecture Details | Optional H2 for structural design detail. |
| `plan.heading.algorithm` | heading | 算法细节 | Algorithm Details | Optional H2 for algorithm detail. |
| `plan.heading.supplemental` | heading | 其他类型补充建议 | Supplemental Guidance for Other Work Types | Optional H2 for necessary non-standard work. |
| `plan.heading.tasks` | heading | 任务拆分 | Task Decomposition | Conditional H2 for executable Tasks. |
| `plan.heading.source_task` | pattern | `TASK-001：<对象 + 动作>（源码改动点）` | `TASK-001: <Object + Action> (Source-Code Change)` | Repeated H3 source-code Task pattern. |
| `plan.heading.non_code_task` | pattern | `TASK-001：<非源码对象 + 动作>（非源码改动点）` | `TASK-001: <Non-Code Object + Action> (Non-Code Change)` | Repeated H3 non-code Task pattern. |
| `plan.heading.validation_task` | pattern | `TASK-...：<验证对象 + 验证动作>` | `TASK-...: <Validation Target + Validation Action>` | Repeated H3 Validation Task pattern. |
| `plan.heading.validation` | heading | 验证与回归检查 | Validation and Regression Checks | Required H2 before the assessment. |
| `plan.heading.assessment` | heading | 方案评估与建议 | Assessment and Recommendations | Required final H2. |

## Controlled Core Concepts

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `plan.concept.current_plan` | concept | Current Plan | Current Plan | The single logical Plan state for one active planning task; fixed protocol, not localized. |
| `plan.concept.current_plan_document` | concept | Current Plan Document | Current Plan Document | The single persisted Plan document carrying the Current Plan for one active planning task; fixed protocol, not localized. |
| `plan.concept.authoritative_snapshot` | concept | 当前协作基准版本 | Authoritative Working Snapshot | Collaboration priority without claiming inherent correctness. |
| `plan.concept.decision_record` | concept | 决策记录 | Decision Record | A record for real design divergence, not missing information. |
| `plan.concept.decision_owner` | concept | 决策责任方 | Decision Owner | Owner of the decision; values remain AI or User. |
| `plan.concept.missing_information` | concept | 缺少的关键信息 | Missing Required Information | Undiscoverable required facts, not an Open Decision. |
| `plan.concept.user_intervention` | concept | 用户介入 | User Intervention | Minimal user input for facts, business meaning, or risk acceptance. |
| `plan.concept.bounded_decision` | concept | 有明确边界的主动决策 | Bounded Active Decision-Making | AI technical decision-making within the user-defined goal. |
| `plan.concept.requirement_correction` | concept | 需求校正 | Requirement Correction | Correct ambiguity without changing the user's goal. |
| `plan.concept.scoped_upstream_analysis` | concept | 按需向上追溯 | Scoped Upstream Analysis | Bounded upstream analysis, not repository-wide exploration. |
| `plan.concept.preliminary_analysis` | concept | 前置分析 | Preliminary Analysis | Fact model used by design, risk, and validation. |
| `plan.concept.current_design` | concept | 当前采用的设计 | Current Design | The one effective design, not an option set. |
| `plan.concept.sourced_constraint` | concept | 有明确来源的约束 | Sourced Constraint | A constraint from user, project rules, or discoverable facts. |

## Requirements, Scope, and Analysis Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `plan.requirements.task_type` | label | 任务类型 | Task Type | Classification of the overall request, not a Task ID. |
| `plan.requirements.document_output_location` | label | 文档输出位置 | Document Output Location | Chat-only or persisted Plan location, not Document Status. |
| `plan.requirements.contradictions_and_missing_information` | label | 矛盾和缺少的关键信息 | Contradictions and Missing Information | Current contradictions and unavailable required facts, not Decision options. |
| `plan.requirements.adopted_interpretation` | label | 采用口径 | Adopted Interpretation | The current requirement interpretation, not an option set. |
| `plan.requirements.assumptions_and_impacts` | label | 假设及影响 | Assumptions and Impacts | Non-structural assumptions and their consequences. |
| `plan.requirements.task_description` | label | 任务描述 | Task Description | Corrected overall request description, not one implementation Task. |
| `plan.scope.goals` | label | 目标 | Goals | Expected outcomes, not Target records. |
| `plan.scope.notes` | label | 注意事项 | Notes | Necessary usage or execution notes, not substitute constraints. |
| `plan.analysis.user_scope` | label | 用户指定分析范围 | User-Specified Analysis Scope | Analysis objects explicitly requested by the user. |
| `plan.analysis.additional_scope` | label | AI 补充的分析范围 | Additional Scope Identified by AI | Minimum additional objects required by scoped upstream analysis. |
| `plan.analysis.scope_rationale` | label | 范围判断依据 | Scope Rationale | Why the selected analysis scope is closed. |
| `plan.analysis.depth` | label | 分析深度说明 | Analysis Depth | Required depth by object, not implementation priority. |
| `plan.analysis.current_facts` | label | 当前事实与约束 | Current Facts and Constraints | Verifiable current facts and direct constraints, not design advice. |
| `plan.analysis.existing_mechanisms` | label | 现有机制 | Existing Mechanisms | Existing behavior that changes the design. |
| `plan.analysis.reuse_preserve_avoid` | label | 复用/保留/避免项 | Reuse / Preserve / Avoid | Reuse choices and regression-prevention boundaries. |
| `plan.analysis.risks_and_limitations` | label | 风险与限制 | Risks and Limitations | Real risks that affect design or validation. |
| `plan.analysis.bug_diagnosis_and_fix_plan` | label | Bug 定位与修复方案 | Bug Diagnosis and Fix Plan | Conditional Bug analysis and fix-planning block. |
| `plan.analysis.reproduction` | label | 复现条件/触发概率 | Reproduction Conditions / Trigger Probability | Reproduction entry and frequency. |
| `plan.analysis.actual_expected` | label | 实际行为 vs 预期行为 | Actual Behavior vs. Expected Behavior | Observable behavioral deviation. |
| `plan.analysis.root_cause_hypotheses` | label | 根因假设 | Root-Cause Hypotheses | Evidence-ranked hypotheses, not established facts. |
| `plan.analysis.minimal_fix` | label | 最小修复点 | Minimal Fix Point | Smallest current fix location, not implementation authorization. |
| `plan.analysis.regression_points` | label | 回归风险与观察点 | Regression Risks and Observation Points | Regression risks and post-fix observations. |
| `plan.analysis.benchmark_target` | label | 对标对象 | Benchmark Target | Explicit benchmark object. |
| `plan.analysis.structural_findings` | label | 结构与职责结论 | Structural and Ownership Findings | Structural and ownership findings from the benchmark. |
| `plan.analysis.required_consistencies` | label | 必须保持一致的部分 | Required Consistencies | Constraints inherited from the benchmark. |
| `plan.analysis.permitted_additions` | label | 允许新增的部分 | Permitted Additions | Extensions permitted inside the current goal. |
| `plan.analysis.intentional_differences` | label | 必须明确区分的部分 | Intentional Differences | Differences that must remain distinct from the benchmark. |

## Decision, Target, Architecture, and Algorithm Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `plan.decision.user_input_reason` | label | 必须由用户决定的原因 | Why User Input Is Required | Why the issue is outside ordinary AI technical decision responsibility. |
| `plan.decision.options` | label | 备选方案 | Options | Minimal mutually exclusive options for an Open Decision. |
| `plan.decision.recommended_option` | label | 推荐方案及依据 | Recommended Option and Rationale | AI recommendation that is not yet a user decision. |
| `plan.decision.minimum_question` | label | 用户需要回答的最小问题 | Minimum Question for the User | Smallest question that closes the Decision. |
| `plan.decision.outcome` | label | 决策结果 | Decision Outcome | Current effective conclusion for a resolved Decision. |
| `plan.decision.owner` | label | 决策责任方 | Decision Owner | Localizable label; values remain the fixed literals AI or User. |
| `plan.decision.rationale` | label | 决策依据 | Decision Rationale | Facts and constraints supporting the decision. |
| `plan.decision.primary_impacts` | label | 主要影响 | Primary Impacts | Key behavioral or scope effects of the decision. |
| `plan.decision.impact_scope` | label | 影响范围 | Impact Scope | Scope directly constrained by the Decision. |
| `plan.target.change_targets` | label | 变更对象与预期结果 | Change Targets and Expected Outcomes | Objects and behavioral outcomes derived from Current Design. |
| `plan.target.core_flow` | label | 核心流程 | Core Flow | Key runtime or workflow, not a call log. |
| `plan.target.data_state_flow` | label | 数据与状态流 | Data and State Flow | Relevant data, state, and propagation. |
| `plan.target.failure_cleanup_rollback` | label | 失败、清理与回退 | Failure, Cleanup, and Rollback | Failure meaning, cleanup ownership, and rollback. |
| `plan.blocked_scope.type` | label | 类型 | Type | Whether the blocker needs a fact or a user decision. |
| `plan.blocked_scope.related_blocker` | label | 对应的缺失信息或 Decision | Related Missing Information or Decision | Link to the blocking fact or Decision. |
| `plan.blocked_scope.affected_components` | label | 受影响对象与责任面 | Affected Components and Responsibilities | Exact locally blocked objects and responsibilities. |
| `plan.blocked_scope.minimum_input` | label | 用户需要提供的最小输入 | Minimum Input Required from the User | Smallest input that closes the fact or Decision. |
| `plan.blocked_scope.note` | label | 说明 | Note | States that blocked scope does not create a formal Task. |
| `plan.architecture.objects` | label | 架构对象与改动点 | Architecture Objects and Change Points | Structural change objects, not only files. |
| `plan.architecture.boundaries` | label | 职责、规则或结构边界 | Responsibility, Rule, or Structural Boundaries | Ownership and structural constraints. |
| `plan.architecture.dependencies` | label | 依赖、衔接与信息流 | Dependencies, Integration, and Information Flow | Relationships and information propagation. |
| `plan.architecture.migration` | label | 迁移、兼容与回退 | Migration, Compatibility, and Rollback | Actual migration, compatibility, and rollback behavior. |
| `plan.architecture.rationale` | label | 设计动机 | Design Rationale | Why the current structure is used. |
| `plan.architecture.adjustments` | label | 关键结构、契约或接口调整清单 | Key Structure, Contract, or Interface Adjustments | Structural adjustment set, not a Task list. |
| `plan.architecture.adjustment_target` | label | 调整对象 | Target of the Change | Exact changed object; use an interface or signature for source code. |
| `plan.architecture.dependent_impact` | label | 对依赖方和调用方的影响 | Impact on Dependents and Callers | Downstream effect, not Task dependency. |
| `plan.architecture.compatibility` | label | 兼容策略 | Compatibility Strategy | The adopted compatibility behavior. |
| `plan.architecture.validation_point` | label | 验证点 | Validation Point | Local evidence point for the structural change. |
| `plan.algorithm.steps` | label | 步骤 | Steps | Algorithm steps, not Task ordering. |
| `plan.algorithm.parameters` | label | 参数与默认值 | Parameters and Defaults | Behavior-relevant parameters and defaults. |
| `plan.algorithm.boundaries` | label | 边界与回退 | Boundaries and Fallbacks | Input boundaries and algorithm fallback, not version rollback. |
| `plan.algorithm.changes` | label | 关键算法修改清单 | Key Algorithm Changes | Algorithm-level change set. |
| `plan.algorithm.object` | label | 文件/函数或对象 | File / Function / Object | Exact algorithm change location. |
| `plan.algorithm.change_details` | label | 具体修改 | Change Details | Concrete algorithm change, not the complete change point identity. |
| `plan.algorithm.behavior` | label | 行为变化 | Behavior Change | Observable behavior after the algorithm change. |
| `plan.algorithm.risks` | label | 风险与回退 | Risks and Fallbacks | Algorithm risks and local fallback. |

## Task and Change Card Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `plan.task.related_decision` | label | 对应决策/约束 | Related Decision / Constraint | Link to a Decision or Sourced Constraint. |
| `plan.task.related_change_point` | label | 对应 `change point` | Related `change point` | Object plus Action plus semantic effect. |
| `plan.task.responsibility` | label | `Task` 责任边界 | `Task` Responsibility Boundary | Single primary Task responsibility. |
| `plan.task.dependencies` | label | 依赖任务 | Task Dependencies | Product dependencies between Tasks. |
| `plan.task.implementation_card` | label | 实现卡片 | Implementation Card | Source implementation information container. |
| `plan.task.location` | label | 准确文件、`symbol` 或目标位置 | Exact File, `symbol`, or Target Location | Exact implementation location. |
| `plan.task.single_responsibility` | label | 单一 `Task` 责任 | Single `Task` Responsibility | The Task's one primary responsibility. |
| `plan.task.inputs_outputs` | label | 输入输出 | Inputs and Outputs | Behavioral interface, not user response formatting. |
| `plan.task.preconditions` | label | 前置条件与不变量 | Preconditions and Invariants | Start conditions and preserved semantics. |
| `plan.task.ordered_steps` | label | 有序代码步骤 | Ordered Code Steps | Ordered object, Action, and semantic-effect steps. |
| `plan.task.pseudocode` | label | 伪代码与调用顺序 | Pseudocode and Call Order | Included only to remove implementation ambiguity. |
| `plan.task.project_example` | label | 项目内现有示例及采用原因 | Existing Project Example and Why It Applies | Project precedent and why it applies. |
| `plan.task.failure_cleanup` | label | 失败、取消与清理 | Failure, Cancellation, and Cleanup | Runtime failure, cancellation, and cleanup semantics. |
| `plan.task.lifecycle` | label | 并发、分布式或生命周期考虑 | Concurrency, Distribution, or Lifecycle Considerations | Included only when the Task actually has this dimension. |
| `plan.task.local_validation` | label | 局部验证方式 | Local Validation Method | Validation proportionate to one Task's risk. |
| `plan.task.redesign_stop` | label | 重新设计停止条件 | Redesign Stop Conditions | Stop and return to planning instead of designing in Code. |
| `plan.task.decomposition_rationale` | label | 拆分说明 | Decomposition Rationale | Why change points were combined or not split further. |
| `plan.non_code_task.card` | label | 非源码修改卡片 | Non-Code Change Card | Non-code modification information container. |
| `plan.non_code_task.object_location` | label | 改动对象与位置 | Change Object and Location | File or resource plus heading, key, or item. |
| `plan.non_code_task.intended_change` | label | 目标修改 | Intended Change | Add, remove, replace, or converge content and meaning. |
| `plan.non_code_task.must_preserve` | label | 必须保留 | Must Preserve | Required content or behavior that cannot be weakened. |
| `plan.non_code_task.must_not` | label | 不得引入或必须删除 | Must Not Introduce / Must Remove | Prohibited additions and required removals. |
| `plan.non_code_task.dependencies` | label | 依赖 | Dependencies | Actual prerequisite or product dependencies. |
| `plan.non_code_task.acceptance` | label | 局部验收 | Local Acceptance Check | Structural, semantic, or tool-based local check. |
| `plan.non_code_task.replanning` | label | 重新规划触发条件 | Replanning Triggers | Conditions that stop implementation and return to planning. |
| `plan.validation_task.authorized_scope` | label | 本轮允许修改的范围 | Authorized Change Scope | Must be empty for an independent Validation Task. |
| `plan.validation_task.card` | label | 验证卡片 | Validation Card | Independent validation information container. |
| `plan.validation_task.target` | label | 验证对象 | Validation Target | Behavior, artifact, or boundary being validated. |
| `plan.validation_task.preconditions` | label | 前置条件 | Preconditions | Conditions required to execute validation. |
| `plan.validation_task.steps` | label | 执行步骤 | Execution Steps | Validation sequence, not implementation steps. |
| `plan.validation_task.evidence` | label | 预期证据 | Expected Evidence | Observable evidence required for the result. |
| `plan.validation_task.pass_criteria` | label | 通过标准 | Pass Criteria | Deterministic conditions for a passing result. |

## Validation Summary and Assessment Labels

| key | class | source_zh_cn | en | context |
| --- | --- | --- | --- | --- |
| `plan.validation_summary.minimum_steps` | label | 最小验证步骤 | Minimum Validation Steps | Minimum validation set for current risks. |
| `plan.validation_summary.regression_points` | label | 关键回归点 | Key Regression Points | Existing behavior and boundaries most likely to regress. |
| `plan.validation_summary.tracking_item` | label | 独立追踪项 | Independent Tracking Item | Optional VAL item, not a Task. |
| `plan.validation_summary.exclusions` | label | 本轮未覆盖的验证项 | Exclusions | Validation intentionally not run in this batch. |
| `plan.assessment.disclaimer` | label | 声明 | Disclaimer | States that the assessment is guidance, not development fact. |
| `plan.assessment.plan` | label | 方案评介 | Plan Assessment | Feasibility, cost, and risk assessment without rewriting the Plan. |
| `plan.assessment.detail` | label | 计划详细程度评估 | Plan Detail Assessment | Whether an independent AI can execute without new design choices. |
| `plan.assessment.issues` | label | 当前问题 | Current Issues | Real issues that still affect delivery or execution. |
| `plan.assessment.next_step` | label | 建议的下一步行动 | Recommended Next Step | Concrete next action that cannot add a new body decision. |
