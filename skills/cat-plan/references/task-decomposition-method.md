# Task Decomposition Method

Read this file when a plan needs formal implementation-task decomposition. It is the single task-decomposition method for Bug fixes, requirements, tool development, refactors, and mixed source tasks.

## Goal
- Turn task decomposition into a constrained procedure instead of free-form writing.
- Force tasks to come from concrete in-scope change points already defined in the plan.
- Make each task answer three questions: what changes, why it stands alone, and how it is verified locally.

## Terms
- `Change point`: one concrete in-scope modification already stated in the implementation, architecture, or algorithm sections, expressed as `Object + Action + Semantic effect`.
- `Responsibility surface`: one primary kind of work carried by a task. Default surfaces are data model, interface bridge, algorithm/rule, lifecycle/flow, compatibility/warning, and validation. Add another surface only when those defaults would distort the task boundary.
- `Local verification`: the smallest check that confirms the task in isolation.

## Task Breakdown Principles

- By default, task breakdown is treated as a breakdown of *implementation steps*, not a retelling of the plan based on the primary business workflow.
- Before drafting the "Task Breakdown," extract a "List of Key Modifications" from development plan sections such as "Implementation plan," "Algorithm Details," and "Architecture Details." For each modification, record at least the following: target, action, semantic effect, primary area of ​​responsibility, dependencies, and local verification method.
- If the planning template requires adding, deleting, or merging sections, the coverage check for the task breakdown relies on the semantic meaning of section titles rather than numerical identifiers.
- Use the following categories for the primary area of ​​responsibility by default: Data Model, Interface Bridging, Algorithm/Rule, Process/Lifecycle, Compatibility/Fallback/Warning, and Verification. If a task clearly does not fit these six categories, a more appropriate category may be added, provided a single primary area of ​​responsibility is maintained and its meaning is clearly defined within the task.
- Break down tasks by primary area of ​​responsibility first, then sequence them according to dependencies; do not group tasks based on the narrative of the primary business workflow.
- By default, a sub-task is permitted only one primary area of ​​responsibility; if a task covers multiple areas, it should be broken down further.
- If a modification involves a distinct local verification method, a unique key function or interface signature, or specific semantics regarding warnings, fallbacks, or compatibility, it should generally be listed as a separate task.
- Merging items into a single task is permitted only when they share the same primary area of ​​responsibility, dependencies, and local verification method, *and* splitting them would not improve clarity regarding implementation sequence, risk isolation, or verification.
- Stop further breakdown if doing so would no longer improve clarity regarding implementation sequence, risk isolation, or verification.
- Before finalizing the task list, perform a coverage check: ensure that every key modification identified in sections like "Implementation plan," "Algorithm Details," and "Architecture Details" is either mapped to a task in the breakdown or explicitly marked in the main text as "Out of Scope," "Warning," or "Future Work."

### Step 1: Build The Change-Point List
- Extract change points only from:
  - implementation-design changes
  - architecture-details change list
  - algorithm-details change list
  - explicit in-scope warnings, fallbacks, wrappers, or compatibility work
- Record at least six fields for each change point:
  - `Object`
  - `Action`
  - `Semantic effect`
  - `Responsibility surface`
  - `Dependencies`
  - `Local verification`

### Step 2: Split By Responsibility Surface
- Split by primary responsibility before ordering by feature flow.
- Keep one primary responsibility surface per task.
- Use the default responsibility surfaces first. Introduce a new surface only when the default list would force an artificial grouping, and name the new surface explicitly.
- Split into separate tasks when a change point has:
  - its own local verification
  - its own key function or interface signature
  - its own compatibility, fallback, wrapper, or warning meaning
- Merge only when change points share the same responsibility surface, dependencies, and local verification, and splitting would not improve order, risk isolation, or verification clarity.

### Step 3: Stop Splitting At The Right Time
- Stop when further splitting:
  - adds no new local verification
  - does not reduce mixed responsibilities
  - only creates mechanical micro-tasks inside the same object

### Step 4: Order Tasks Deterministically
- Build the execution order from real artifact and interface-contract dependencies first. Use the following responsibility-surface order only as a stable tie-breaker among tasks with no dependency relationship:
  1. Data model
  2. Shared helpers or parsers
  3. Interface bridge or signature changes
  4. Entry flow or lifecycle nodes
  5. Isolated algorithms or rules
  6. Compatibility, fallback, wrapper, or warning work
  7. Validation
- Use another tie-breaker only when the plan explains why. Before marking the batch Ready, verify that the dependency graph is acyclic and every completed task leaves a safe, locally verifiable intermediate state.

### Step 5: Run Coverage And Anti-Laziness Checks
- Before writing the Task Breakdown section, verify that:
  - every in-scope change point from the implementation, architecture, and algorithm sections appears in the Task Breakdown or is explicitly marked as out of scope, warning, or future work
  - each task has one primary responsibility surface
  - each task has local verification
  - each change point has exactly one Task owner by `Object + Action + Semantic effect`
  - execution order follows real dependencies and every completed task leaves a safe intermediate state
  - warnings, fallbacks, wrappers, and compatibility work do not disappear into a generic risk section
- Treat the output as insufficient when:
  - task titles do not show both object and action
  - merged tasks do not explain why the change points belong together
  - a task stops at its current granularity without a clear reason when further splitting is still plausible

## Common Anti-Patterns
- Retelling the main flow instead of mapping concrete change points.
- Using abstract titles such as `handle main flow` or `improve XX logic`.
- Listing algorithm or interface changes in the design sections but never turning them into tasks.
- Leaving warnings, fallbacks, wrappers, or compatibility work only in risk notes.
- Making every task rely only on full integration testing.
- Ordering tasks by responsibility surface when real artifact or interface dependencies require another order.
