# Output Language Policy for Cat Code

## Purpose

This reference governs language selection and language-invariant behavior for Cat Code pre-flight, progress, blocker, mapping, validation, and delivery output. It does not grant implementation authorization, expand the Authorized Change Scope, change a Task gate, or alter a validation conclusion.

## When This Reference Is Loaded

Read this file only when `SKILL.md` routes an explicit language switch, translation, bilingual output, or terminology comparison; different languages on the interaction surface and an actual output-artifact surface; a language directive whose surface scope cannot be resolved; or a third-language formal structured surface. Never load it merely because a source-only Plan or document uses a different language. Do not load it for an ordinary same-language response in any language.

This reference does not load or require `output-language-terminology.md`. SKILL.md decides whether each language reference is needed and loads each one directly.

## Output Surface Language Resolution

Use the `Interaction Language` already resolved by `SKILL.md`; do not infer it again from the full prompt, source artifacts, or other context. Then resolve one `Output Artifact Language` for each Plan or document that will actually be written. An input Plan language is `Source Artifact Language`: it is input metadata and never overrides `Interaction Language`; it becomes an output artifact language only when Direct User Request Prose explicitly requests that the Plan be maintained or translated in this turn.

Scope a language directive to the named surface. A reply, explanation, progress, or conversation directive affects only `Interaction Language`; a Plan, document, report, or translation directive affects only the named artifact. An unscoped `use X` or `output in X` directive affects `Interaction Language` and every new artifact in the request, but does not translate an existing maintained artifact unless the user explicitly requests that change.

## Single-Language and Comparison Output

Use one natural language per output surface by default. Pre-flight, progress, blocker, mapping, validation explanation, and delivery-summary surfaces use `Interaction Language`. A Plan or maintained-document surface uses its own `Output Artifact Language` only when it is actually written. Different languages on separate surfaces are not bilingual comparison output. Put multiple natural languages on the same surface only when the user explicitly requests translation, bilingual output, or terminology comparison.

## Plan and Existing Document Language

A source-only Plan is input context, not an output artifact. Preserve the main language of a Plan or document only when this request actually writes or maintains that artifact, unless the user explicitly requests a switch for it. Language selection never changes which Plan or Task the user currently requested to implement.

## Fixed Protocol and Authorization

Keep `Current Plan` and `Current Plan Document` unchanged in every language. They are defined protocol terms, not localizable prose phrases.

Keep `Document Status`, `Design Readiness`, `Decision Status`, `Task Readiness`, `In Scope`, and `Out of Scope`; the enum values `Draft`, `Final Snapshot`, `Ready`, `Partially Ready`, `Not Ready`, `Open`, `Decided`, `AI`, and `User`; the mechanism words `Target`, `Task`, `Decision`, `change point`, `symbol`, and `Validation`; and ASCII IDs such as `Decision-001`, `TASK-001`, and `VAL-...` unchanged in every language. `Ready` never means implementation authorization. Code writes still require the user's current explicit implementation request and a closed Authorized Change Scope. `Completed` and `Verified` remain distinct. Failed, Not Verified, or Blocked semantics never become success or full closure through translation. The localizable label for the decision owner is `决策责任方` in zh-CN and `Decision Owner` in en; only its values `AI / User` are fixed.

## Normative Chinese Modality

Before translating a normative Chinese instruction, preserve its obligation class: `必须 / 不得 / 只有……才 / 立即停止` are hard obligations, prohibitions, gates, or stop commands; `默认` defines default behavior; `应 / 建议 / 优先` expresses recommendation or priority; `可以 / 可选 / 若有 / 如适用 / 实际涉及时保留 / 涉及时必填` expresses permission or applicability. Never promote or weaken one class into another.

## Chinese Script and Regional Variants

Resolve a Chinese script or regional variant in this order: an explicitly requested locale or script, the script of the maintained artifact, the script used in Direct User Request Prose, then zh-CN when no usable signal exists. Preserve explicitly requested Traditional Chinese, zh-TW, or zh-HK and do not mix Simplified and Traditional Chinese in one controlled display set. Until a frozen terminology column exists, treat a formal zh-Hant structured surface as a third-language structured surface.

## Third-Language Structured Output

An ordinary same-language response in a language other than zh-CN or en needs no language reference and no quality disclosure. Before rendering a third-language formal structured surface, create a delivery-local display mapping only for the terminology keys actually used. Reuse one display value for the same key throughout that deliverable, do not expose keys, and do not persist the mapping as a frozen terminology set. Keep the fixed protocol unchanged. Put the missing-frozen-terminology and paired-regression disclosure once in a delivery note outside the mapping, blocker, validation, or batch-review body.

## Quality Gates

Preserve the implementation request, Authorized Change Scope, stop conditions, Task ordering, mapping meaning, and validation result across languages. For zh-CN and en, use the exact terminology display value for every controlled heading, label, status, concept, and pattern; never substitute a synonym, abbreviation, alternate capitalization, or stylistic rewrite. Never soften must, only, never, or stop. Use only one language's display set on one output surface. If a required controlled key is missing, stop rendering that structured template instance, but continue reporting actual changes, validation results, failures, blockers, and risks in `Interaction Language`. Do not invent a substitute display value and do not let localization failure suppress implementation facts.
