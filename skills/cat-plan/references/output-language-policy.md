# Output Language Policy for Cat Plan

## Purpose

This reference governs language selection and language-invariant behavior for Cat Plan user-visible output. It does not change planning responsibility, readiness, implementation authorization, Task gating, or validation conclusions.

## When This Reference Is Loaded

Read this file only when `SKILL.md` routes an explicit language switch, translation, bilingual output, or terminology comparison; different languages on the interaction surface and an actual output-artifact surface; a language directive whose surface scope cannot be resolved; or a third-language formal structured surface. Never load it merely because a source-only Plan or document uses a different language. Do not load it for an ordinary same-language response in any language.

This reference does not load or require `output-language-terminology.md`. SKILL.md decides whether each language reference is needed and loads each one directly.

## Output Surface Language Resolution

Use the `Interaction Language` already resolved by `SKILL.md`; do not infer it again from the full prompt, source artifacts, or other context. Then resolve one `Output Artifact Language` for each Plan or document that will actually be created or maintained. An existing output artifact keeps its main language unless Direct User Request Prose explicitly requests a language for that artifact; a new artifact uses its explicitly requested language, otherwise `Interaction Language`. A source-only artifact language is input metadata and never overrides an output language or triggers localization by itself.

Scope a language directive to the named surface. A reply, explanation, progress, or conversation directive affects only `Interaction Language`; a Plan, document, report, or translation directive affects only the named artifact. An unscoped `use X` or `output in X` directive affects `Interaction Language` and every new artifact in the request, but does not translate an existing maintained artifact unless the user explicitly requests that change.

## Single-Language and Comparison Output

Use one natural language per output surface by default. Conversation, clarification, blocker, review, and delivery-summary surfaces use `Interaction Language`; each formal Plan or maintained-document surface uses its own `Output Artifact Language`. Different languages on separate surfaces are not bilingual comparison output. Put multiple natural languages on the same surface only when the user explicitly requests translation, bilingual output, or terminology comparison. A comparison must not duplicate the fixed protocol layer.

## Existing Documents

A source-only Plan or document is not an output artifact. When an existing Plan or document is actually maintained, preserve its main language unless the user explicitly requests a language switch for that artifact. When a switch is requested, update the complete current effective document within the requested scope; do not translate only newly added sections.

## Fixed Protocol

Keep `Current Plan` and `Current Plan Document` unchanged in every language. They are defined protocol terms, not localizable prose phrases.

Keep `Document Status`, `Design Readiness`, `Decision Status`, `Task Readiness`, `In Scope`, and `Out of Scope`; the enum values `Draft`, `Final Snapshot`, `Ready`, `Partially Ready`, `Not Ready`, `Open`, `Decided`, `AI`, and `User`; the mechanism words `Target`, `Task`, `Decision`, `change point`, `symbol`, and `Validation`; and ASCII IDs such as `Decision-001`, `TASK-001`, and `VAL-...` unchanged in every language. `Ready` never means approved, authorized, or permission granted. The localizable label for the decision owner is `决策责任方` in zh-CN and `Decision Owner` in en; only its values `AI / User` are fixed.

## Normative Chinese Modality

Before translating a normative Chinese instruction, preserve its obligation class: `必须 / 不得 / 只有……才 / 立即停止` are hard obligations, prohibitions, gates, or stop commands; `默认` defines default behavior; `应 / 建议 / 优先` expresses recommendation or priority; `可以 / 可选 / 若有 / 如适用 / 实际涉及时保留 / 涉及时必填` expresses permission or applicability. Never promote or weaken one class into another.

## Chinese Script and Regional Variants

Resolve a Chinese script or regional variant in this order: an explicitly requested locale or script, the script of the maintained artifact, the script used in Direct User Request Prose, then zh-CN when no usable signal exists. Preserve explicitly requested Traditional Chinese, zh-TW, or zh-HK and do not mix Simplified and Traditional Chinese in one controlled display set. Until a frozen terminology column exists, treat a formal zh-Hant structured surface as a third-language structured surface.

## Third-Language Structured Output

An ordinary same-language response in a language other than zh-CN or en needs no language reference and no quality disclosure. Before rendering a third-language formal structured surface, create a delivery-local display mapping only for the terminology keys actually used. Reuse one display value for the same key throughout that deliverable, do not expose keys, and do not persist the mapping as a frozen terminology set. Keep the fixed protocol unchanged. Put the missing-frozen-terminology and paired-regression disclosure once in a delivery note outside the formal Plan body.

## Quality Gates

Preserve heading order, nesting, required and optional conditions, field order, and responsibility strength across languages. For zh-CN and en, use the exact terminology display value for every controlled heading, label, status, concept, and pattern; never substitute a synonym, abbreviation, alternate capitalization, or stylistic rewrite. Never soften must, only, never, or stop. Do not show two display values for one terminology key unless the user requested a comparison. Do not translate code, commands, paths, filenames, symbols, URLs, IDs, or fixed protocol tokens. If a required controlled key is missing, stop rendering that formal Plan instance and report the missing key, affected structure, and required terminology update in `Interaction Language`. Do not emit a partial formal template and do not invent a substitute display value.
