---
name: capsule-draft-writer-edt-prf
description: draft import-ready smart brevity healthcare capsules from an approved cms drafting contextpack. use when chatgpt needs to convert a topic prototype, approved reference sources, editorial profile, output requirements, duplication context, provenance meta, and optional approved-source lookup policy into one json module import envelope for a consumer-facing capsule with fixed newsletter sections, evidence-aware medical restraint, source id traceability, and review-queue-compatible warnings. do not use for topic ideation, open research, new source discovery, unsupported medical claims, or non-json editorial copy.
---

# Capsule Draft Writer _Edt_Prf

Treat the input as a `Drafting ContextPack` for one approved topic prototype.
Return JSON only. Do not add markdown, prose outside JSON, code fences, or comments.
Treat the Context Pack as the editorial authority and approved references as the evidence boundary.
Do not ideate, add new sources, change the approved angle, or add unsupported statistics or medical claims.

## Drafting scope

Perform capsule drafting only.
Convert `task.topic_prototype` into one import-ready Smart Brevity-style healthcare Capsule using the canonical ModuleImportEnvelope described in `references/capsule-schema.md`.
Use the fixed Capsule section structure carried by the current CMS app:

- `title`
- `citation_line`
- `big_picture`
- `whats_new`
- `by_the_numbers`
- `why_it_matters`
- `yes_but`
- `what_it_means_for_you`

Treat the pack's `embedded_runtime_config.editorial_profile` as the prototype-linked drafting profile, not as an optional suggestion and not as a structure override.
The fixed Capsule section structure controls the output shape. The editorial profile controls angle, tone, practicality, evidence density, abstraction, expression style, and risk posture inside that structure.
`output_requirements` may tune target length, optional traceability, and caller-approved extensions, but it must not change the canonical envelope, direct-under-`payload` field placement, or fixed Capsule section structure.

Use `task.reference_set`, `briefing`, `embedded_runtime_config.source_policy`, `embedded_runtime_config.output_requirements`, `embedded_runtime_config.duplication_rules`, `compact_memory_context.related_capsules`, and `compact_memory_context.do_not_repeat` as binding context.

## Source policy

Default normal app-generated drafting packs may allow `approved_source_lookup`.
When `embedded_runtime_config.source_policy.mode` is `approved_source_lookup`, you may consult only URLs, canonical URLs, open URLs, locators, or DOI identifiers already present in the approved `task.reference_set`.
Use lookup only to complete draft-critical factual details inside the approved reference boundary.
When `approved_source_lookup` is enabled, do not skip targeted lookup merely because the packed summary is enough for a safe draft. Perform approved-source lookup whenever `citation_line` or `by_the_numbers` can reasonably be improved by source metadata or numeric facts, unless lookup is technically unavailable, the relevant facts are already fully packed, or the caller explicitly disables lookup.
Do not use general web search to discover or add new sources.
Do not use lookup to change the approved prototype, strengthen recommendations, add mechanisms, add clinical guidance, or expand the angle.

At first, lookup-derived facts may be used only for:

- `citation_line`
- `by_the_numbers`
- `trace.source_completion`

If approved lookup is unavailable, inaccessible, inconclusive, or unsupported by the execution environment, fall back to `packed_only` behavior when `fallback_mode` allows it.
In fallback, use only the Context Pack content, avoid inventing details, and return `partial_success` or warnings when the missing information affects quality.

Record every lookup-derived fact in `trace.source_completion` with the source ID, action, extracted value, fields used, destination field, and source locator. Use a plain URL, DOI, or approved locator string for `source_locator`; do not use Markdown link syntax in trace data.
Successful approved-source lookup is not itself a warning. Use warnings only for missing, uncertain, conflicting, inaccessible, weak, or format-risk information.

Prioritize instructions in this order unless the pack explicitly overrides it:

1. Canonical ModuleImportEnvelope and Capsule payload schema
2. Fixed Capsule section structure
3. reliability, evidence discipline, and medical safety
4. approved source boundary and lookup restrictions
5. `embedded_runtime_config.output_requirements` within the canonical envelope
6. `embedded_runtime_config.editorial_profile`
7. provenance integrity from input `meta`
8. `embedded_runtime_config.duplication_rules`
9. `embedded_runtime_config.source_policy`
10. `briefing`
11. the topic prototype and reference set

Consult these files when needed:

- `references/contextpack-drafting.md` for required and optional input fields, source policy expectations, and pack assumptions
- `references/capsule-schema.md` for the canonical JSON envelope, payload contract, trace rules, and failure behavior
- `references/editorial-profile-guidance.md` for profile use inside the fixed Capsule structure
- `references/drafting-rules.md` for section-level writing, source lookup limits, word count, evidence handling, and target-language naturalness
- `references/review-pass.md` for the required single review pass and revision thresholds
- `references/warning-codes.md` for warning selection, severity, and affected field conventions
- `references/sample-input-output.md` for concrete import-ready examples

## Workflow

1. Validate the ContextPack before drafting.
2. Confirm that required provenance, topic prototype, editorial profile, output requirements, reference fields, and source policy fields are present or safely recoverable.
3. Confirm that the reference set can support the approved angle and the core claims needed for the fixed Capsule sections.
4. Read the source policy. Decide whether the run is `packed_only`, `approved_source_lookup`, or `approved_source_lookup` with fallback to `packed_only`.
5. If approved lookup is allowed, perform a lookup necessity check for `citation_line` and `by_the_numbers`. Do not skip lookup only because a safe packed-only draft is possible. Consult approved reference-set locators when metadata or numeric facts can reasonably improve those fields; record lookup-derived facts, failed attempts, or a justified no-lookup decision in `trace.source_completion`.
6. Read the editorial profile and set intellectual entry, evidence density, novelty, practicality, abstraction, expression style, tone register, and risk posture from it.
7. Read related capsules and do-not-repeat context before choosing title, opening angle, phrasing, numbers, and final takeaway.
8. Extract a drafting plan mentally:
   - reader-facing angle
   - main finding
   - most memorable supported numbers or the limitation if meaningful numbers are not available
   - why-it-matters interpretation
   - evidence caveat
   - practical caveat
   - final reader takeaway
   - formal short citation metadata
   - exact source IDs to place in `reference_ids`
   - any source-completion trace entries
9. Draft one provisional Capsule in the canonical payload fields.
10. Run exactly one review pass using `references/review-pass.md`.
11. Revise once only if the review finds a meaningful issue. Prefer deletion, narrowing, or rephrasing over expansion.
12. Build `reference_ids` only from supplied source identifiers. Prefer numeric source IDs when present.
13. Include `trace.claim_source_map` only when requested by `output_requirements` or when needed for debugging; do not place claim maps in `payload` by default.
14. Return a complete ModuleImportEnvelope JSON object.

## Drafting rules

Follow these rules on every run:

- Produce one Capsule per run.
- Keep the style consumer-facing, concise, punchy, medically responsible, and lightly conversational.
- Target 180-280 words across the six body sections; use 330 words as a hard upper guide unless `output_requirements` overrides it.
- Calculate `payload.word_count` from the six body sections only: `big_picture`, `whats_new`, `by_the_numbers`, `why_it_matters`, `yes_but`, and `what_it_means_for_you`.
- Do not count `title` or `citation_line` in `word_count`.
- Preserve the approved topic prototype angle. Do not create a new topic.
- Do not write a journal abstract, balanced academic summary, or premium mini-essay.
- Start from reader relevance, not study design.
- Use causal-safe language for observational evidence: `was linked with`, `was associated with`, `researchers observed`, or equivalent target-language phrasing.
- Do not use `proves`, `prevents`, `cures`, `reverses`, `guarantees`, or causal wording unless the approved source clearly supports causal language.
- Use only claims directly or reasonably supported by the Context Pack and approved references.
- Avoid unsupported prevalence figures, external guidelines, mechanisms, statistics, timelines, efficacy claims, or behavioral instructions.
- Keep `citation_line` formal and short. It is not a narrative source summary.
- Keep `by_the_numbers` selective. Use actual supported numeric facts, not source anchors, publication years, evidence types, or dataset labels masquerading as numbers. In `approved_source_lookup` mode, use targeted approved-source lookup when the packed summary lacks key numeric facts that an approved source is likely to contain.
- If meaningful supported numbers are unavailable, keep `by_the_numbers` short, avoid pretending source labels are numbers, and use `partial_success` or warnings when review is needed.
- Make `yes_but` include both an evidence caveat and a practical caveat when the source supports a practical caveat.
- Make `what_it_means_for_you` useful but not prescriptive. It should land as nuance, not advice to start, stop, increase, decrease, diagnose, or treat.
- Do not use lookup-derived facts outside `citation_line`, `by_the_numbers`, or `trace.source_completion` unless the caller explicitly expands `allowed_output_use`.
- Minimize overlap with recent framing, phrases, claims, and narrative angles in `related_capsules` and `do_not_repeat`.
- Preserve evidence-aware restraint: when support is thin, narrow the claim, lower assertiveness, use `partial_success`, or add warnings.
- Never add new claims during the review pass.
- Never increase certainty, causal force, novelty, or practical specificity during the review pass unless the original draft was already safely supported and clearly under-claimed.

## Output contract

Always return a single JSON object that matches `references/capsule-schema.md`.
Use the canonical ModuleImportEnvelope shape:

- top-level `schema_version`
- top-level `module_name`
- top-level `module_type`
- top-level `run_id`
- top-level `result_status`: `success`, `partial_success`, or `failed`
- top-level `warnings`
- top-level `meta`
- top-level `payload`
- optional top-level `trace`
- optional top-level `open_questions`
- optional top-level `draft_blocked_reason`

For `success` and `partial_success`, place Capsule fields directly under `payload`.
Set `payload.object_type` to `capsule`.
Do not nest the draft under another object.

Within `payload`, include at minimum:

- `object_type`
- `topic_prototype_id`
- `editorial_profile_id`
- `title`
- `citation_line`
- `big_picture`
- `whats_new`
- `by_the_numbers`
- `why_it_matters`
- `yes_but`
- `what_it_means_for_you`
- `language`
- `format_variant`
- `status`
- `word_count`
- `reference_ids`

Include these when available or useful:

- `external_ref`
- `source_note`

`partial_success` should still return a complete importable payload when safe. Use warnings to explain review needs.
Use `failed` only when a safe importable draft cannot be produced.
For `failed`, set `payload` to `null`, include `draft_blocked_reason`, and include at least one warning.

Do not add a free-form self-assessment block.
Keep the review loop internal by default.

## Fallback behavior

If required inputs are missing but a safe draft can still be produced, return `partial_success` with a complete payload and warnings.
If source support covers only part of the draft, return `partial_success` with a complete payload plus warnings.
If approved source lookup is allowed but inaccessible, fall back to packed content when permitted and warn only if the missing completion materially weakens `citation_line` or `by_the_numbers`.
If the reference set cannot support the central approved angle, return `failed`.
If no safe `citation_line` can be produced, set `citation_line` to `null`, return `partial_success`, and add a warning with `affected_fields: ["citation_line"]`.
If numeric source IDs are absent, use string source identifiers only when they exactly match supplied source external references. Do not invent source IDs.
Always echo or preserve available input `meta`; do not invent missing provenance values except the canonical `skill_name` fallback when absent.
