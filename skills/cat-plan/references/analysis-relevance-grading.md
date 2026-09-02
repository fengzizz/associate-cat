# Analysis Relevance Grading

## Purpose
- Use this reference to identify which modules, classes, functions, call chains, code-related documents, and reference implementations belong in the analysis scope, then decide how deeply to analyze each one before writing a development plan.
- Convert the vague statement "analyze relevant modules first" into explicit output obligations.
- Use `pre-analysis-writing-guide.md` only after the relevance level is settled, to calibrate wording, compression, and section-writing style.

## Core Principle
- Treat relevance grading as working analysis data, not as the final deliverable by default.
- Grade by decision pressure, not by file size or code volume alone.
- Reflect the grading result through output depth:
  - what is excluded briefly
  - what gets a mechanism summary
  - what gets functional analysis
  - what gets a dedicated focused subsection
- For objects with `High` or `Very High` relevance, describe current behavior and responsibilities, not only reuse intent.
- Analyze fully when needed, but compress the final plan output to the minimum conclusions that directly support scope, reuse, risk, and validation decisions.
- Avoid turning the final plan into a reading log, source walkthrough, or background article.

## Discover Code Scope Before Grading
- Start from task-relevant code touchpoints. Perform one bounded upward pass to the smallest cohesive code unit that owns or coordinates the state, lifecycle, control flow, or extension points relevant to those touchpoints.
- Include that code unit in the analysis candidate set if the planned change modifies code within it or if its behavior can constrain the implementation boundary. Treat multiple touchpoints converging on it, or a change spanning multiple of its responsibilities, as strong escalation signals.
- Treat ownership as a code responsibility, not as inheritance, call-stack position, or directory nesting.
- For a bounded code unit, inspect its declarations, state ownership, lifecycle entry and exit points, extension points, and direct collaborators only as far as they constrain the plan. Do not expand every helper or branch.
- When the task spans multiple responsibilities in the same code unit, inspect their shared state, execution ordering, common entry or exit points, cleanup, and externally visible effects.
- Follow direct code dependencies only to answer a concrete question that can change scope, reuse, risk, or validation. Stop when the question is answered or further inspection cannot change the plan.
- Assign relevance levels only after defining the analysis candidate set and its initial boundary.

## Output Compression Rule
- Keep only facts that directly affect implementation shape, reuse boundaries, risk judgment, or validation path.
- Do not carry over exploration notes, broad background explanation, or full call-chain narration by default.
- For `High` and `Very High` objects, prefer a conclusion-first summary with a few key anchor points over exhaustive description.
- If a detail does not change the plan, it usually does not belong in the final pre-analysis section.

## Fact vs Inference vs plan Constraint
- `Fact`: current behavior, responsibility, state, order, or interface that can be read directly from code or docs.
- `Inference`: feasibility, likely cause, or risk judgment derived from those facts.
- `plan constraint`: what the plan must preserve, avoid, reuse, or verify because of those facts and inferences.
- In the final plan:
  - keep `Fact` and `plan constraint` as the main body
  - keep only decision-relevant `Inference`
  - move implementation suggestions to later sections instead of mixing them into pre-analysis

## Role Tags
- Use role tags only when the signal is explicit enough to justify them.
- Treat role tags as secondary hints; do not let them override the main relevance level judgment.
- `L` = Localization relevance
  - Use when the object clearly narrows where to look or identifies the real entry or call path.
- `D` = Design or repair-shape relevance
  - Use when the object clearly constrains structure, responsibilities, extension points, or repair shape.
- `V` = Validation relevance
  - Use when the object clearly determines how to build, test, verify, or regression-check the change.
- If the signal is not explicit, do not force a role tag.

## Relevance Levels

### Escalation Rule
- When an object sits between two levels, prefer the higher level if misreading it would change or misrepresent scope, structure, lifecycle, sync behavior, or the validation path.
- Prefer the lower level if the object mainly affects terminology, adjacency, or local mechanism understanding without reshaping the plan.

### Low
- Definition:
  - Weakly related background modules, helper files, or documents.
  - Objects checked mainly to exclude them from the main path.
- Minimum analysis depth:
  - One short exclusion note.
- Output obligation:
  - A brief note is enough.

### Medium
- Definition:
  - Related mechanism providers, adjacent modules, or secondary call-chain nodes.
  - Objects that affect terminology, interfaces, or data flow, but do not define the implementation shape.
- Minimum analysis depth:
  - Summarize the current mechanism and why it matters to the task.
  - Add boundary, limitation, or reuse information only when it directly affects the plan.
- Output obligation:
  - Mention concrete responsibility, not only file names.
  - Make the relation to the task visible.
  - Keep the summary compact; do not expand Medium objects into mini design docs.

### High
- Definition:
  - Core modules, classes, or functions on the planned implementation path.
  - Objects whose behavior, lifecycle, state, or extension points directly constrain the plan.
- Minimum analysis depth:
  - Start with the task-relevant conclusion.
  - Explain current responsibilities.
  - Explain key data, state, lifecycle, or call-chain nodes that directly constrain the plan.
  - Explain what should be preserved, reused, avoided, or verified.
- Output obligation:
  - Functional analysis must appear in the final plan.
  - Do not stop at "reusable" or "needs refactor".
  - Keep symbol-level anchors when helpful, but avoid full source walkthroughs.
  - Do not expand every helper or branch unless it directly constrains the plan.

### Very High
- Definition:
  - Objects that have decisive influence on the implementation skeleton, responsibility boundary, or key behavior constraint of the task.
  - Explicit benchmark or reference objects named by the user or clearly implied by the task, such as "对标某类/某模块/某功能".
  - Core implementations whose structure is expected to be mirrored, partially mirrored, or deliberately kept different.
  - Critical base classes, lifecycle owners, state-machine owners, sync chokepoints, or data-entry chokepoints that directly determine what the plan may or may not do.
- Typical signals:
  - the user explicitly says "对标 / 参照 / 仿照 / 对齐 / 沿用"
  - the object defines the extension skeleton or ownership boundary of the planned change
  - the object owns critical lifecycle, state-machine, replication, or authority behavior that the plan must not misread
  - the object is the main choke point for config, runtime input, sync, or validation path
  - multiple task-relevant code touchpoints converge on the same bounded, cohesive code unit
  - the task spans multiple responsibilities within the same code unit, and their shared state, execution ordering, or externally visible effects constrain the implementation structure
- Default rule:
  - Treat explicit "对标 / 参照 / 仿照 / 对齐 / 沿用" targets as `Very High` by default.
  - Do not limit `Very High` to benchmark objects; use it whenever the object is structurally decisive for the task.
- Minimum analysis depth:
  - Start with the task-relevant conclusion.
  - Inspect the structure that constrains the plan.
  - For a code unit, form a conclusion about the unit as a whole that covers the interaction points between task-relevant responsibilities; do not substitute isolated helper-function findings.
  - Describe key members, runtime data, override sets, or extension points only when they define the implementation shape.
  - Explain the key initialization order, runtime state flow, or output responsibilities that must not be misunderstood.
  - When the object is an explicit benchmark, explain mapping to the new plan:
    - what must stay aligned
    - what may be added
    - what must stay different
  - When the object is not an explicit benchmark, explain what it decisively constrains:
    - what the plan must preserve
    - what the plan must not misread or break
    - what extension or modification space remains safe
- Output obligation:
  - Add a dedicated focused analysis block.
  - A simple sentence like "benchmark XX" is not sufficient.
  - Keep the block decision-oriented; do not turn it into a full encyclopedia-style walkthrough.
  - Detail is required, but only around the parts that actually shape or constrain the plan.

## Output Rule
- Do not print the raw `Level + Tag` table in every final plan by default.
- Convert the grading into output depth instead:
  - `Low`: exclusion or de-prioritization note
  - `Medium`: mechanism summary plus relevance reason
  - `High`: functional analysis plus reuse or constraint conclusion
  - `Very High`: dedicated focused analysis plus the decisive alignment or constraint conclusion for the plan
- This file owns level judgment and minimum depth obligation; the writing guide only refines how the result is phrased and compressed.
- If `L`, `D`, or `V` is used, reflect it in the plan content:
  - `L`: explain how the object narrows the search or call path
  - `D`: explain structure, responsibilities, alignment, and exclusions
  - `V`: explain verification path, preconditions, and key failure checks
- If the task includes an explicit benchmark, add a dedicated benchmark-analysis block instead of scattering those conclusions across unrelated sections.
- If the task includes a non-benchmark `Very High` object, make its decisive constraint visible in the final plan instead of hiding it inside generic background summary.
- Do not copy the wording of this document mechanically into the final plan; translate the level judgment into task-specific conclusions.
- Do not use the writing guide to override level selection; use it only to keep the chosen level's output concise, clear, and non-mechanical.

## Quality Gate
- A plan is incomplete if:
  - it cites explicit benchmark objects but never analyzes them directly
  - it lists `High` or `Very High` objects only as file names
  - it claims reuse or alignment without explaining current behavior
  - it proposes structural changes without stating which existing responsibilities should be preserved or excluded
  - it treats validation-critical objects as background context and never explains verification
  - it keeps lengthy reading notes, background exposition, or duplicated constraints that do not directly support the plan
  - it describes `High` or `Very High` objects without reaching a task-relevant conclusion
  - it treats a structurally decisive object as merely `Medium` or background context only because it was not explicitly named as a benchmark
  - it grades task-relevant code touchpoints without first checking the smallest cohesive code unit that owns or coordinates them
  - it analyzes multiple task-relevant responsibilities in isolation but never inspects their shared state, execution ordering, entry or exit points, cleanup, or externally visible effects
