# Single review pass

Run exactly one review pass after producing a provisional draft.
This is a quality gate, not an open-ended polishing loop.

## Priority order

Protect these priorities in this order:

1. importable JSON envelope and payload schema
2. reliability, evidence discipline, approved-source boundary, and medical safety
3. fixed Capsule section integrity
4. approved-source lookup necessity and source-completion trace integrity
5. adherence to the approved topic angle and editorial profile
6. readability and target-language naturalness
7. stylistic polish

If two priorities conflict, protect the higher priority.
If a stylistic improvement would increase interpretive risk, reject the improvement.

## Review goals

Use the review pass to:

- correct invalid JSON shape before anything else
- ensure Capsule fields sit directly under `payload`
- ensure required payload fields are present for `success` and `partial_success`
- remove or narrow unsupported claims
- reduce accidental overstatement
- correct causal language that exceeds the references
- ensure approved-source lookup did not add new sources, new angles, or recommendation-strengthening facts
- when `source_policy.mode` is `approved_source_lookup`, check whether targeted lookup was needed to improve `citation_line` or `by_the_numbers`
- ensure lookup-derived facts are used only in allowed fields
- ensure `trace.source_completion` records lookup-derived facts, failed attempts, fallback, or justified no-lookup decisions when relevant
- restore section-specific function when sections blur together
- correct drift from the approved editorial profile
- improve target-language naturalness without changing factual meaning
- tighten readability without adding new claims

Do not use the review pass to:

- introduce new evidence or new claims
- sharpen novelty beyond the evidence envelope
- increase practical specificity beyond the references
- rewrite the whole piece for personal preference
- iterate more than once

## Review blocks

### A. JSON and importability gate

Check:

- top-level object is valid JSON
- no markdown code fence, comments, or trailing comma
- `schema_version`, `module_name`, `module_type`, `run_id`, `result_status`, `warnings`, `meta`, and `payload` are present
- `module_type` is `"drafting"`
- `result_status` is `"success"`, `"partial_success"`, or `"failed"`
- `warnings` is an array
- for `success` and `partial_success`, `payload.object_type` is `"capsule"`
- for `success` and `partial_success`, Capsule fields are directly under `payload`
- for `failed`, `payload` is `null`, `draft_blocked_reason` exists, and at least one warning exists
- no unsupported internal review prose appears in public payload fields
- `reference_ids` uses supplied numeric IDs when available, or exact supplied external refs when not
- `word_count` counts only the six body sections
- `trace.claim_source_map`, if present, is under `trace`, not `payload`
- `trace.source_completion`, if present, is under `trace`, not `payload`

If importability is broken, correct it before prose issues.

### B. Trustworthiness gate

Check for:

- unsupported or weakly supported claims stated too strongly
- causal language that exceeds the references
- hidden ambiguity behind fluent prose
- practical recommendations that exceed support
- quantified, temporal, prevalence, efficacy, or certainty language that is not justified
- external facts, statistics, guidelines, mechanisms, or medical advice not supplied by the Context Pack or completed from approved references under the source policy
- lookup-derived facts used outside allowed destinations
- new source discovery, source substitution, or angle expansion disguised as source completion

If a trustworthiness issue is found, correct it first.
Prefer deletion, narrowing, attribution of uncertainty, or reduced assertiveness.

### C. Fixed section integrity

Check whether each section is doing its job:

- `title` is concrete, memorable, and responsible
- `citation_line` is formal, short, source-derived, and not a narrative `Sources:` summary unless no formal citation can be built
- `big_picture` starts from reader relevance, not study design
- `whats_new` states the main source-backed finding clearly
- `by_the_numbers` uses actual supported numeric facts and does not merely list source anchors, publication years, evidence types, or datasets
- `why_it_matters` explains the editorial significance rather than repeating the finding
- `yes_but` includes both evidence caveat and practical caveat when practical caveat is safely supportable
- `what_it_means_for_you` gives useful nuance without direct advice

If a section is accurate but functionally misplaced, move or rewrite within the existing evidence.
Do not add unsupported new substance to fill a section.
Use `partial_success` and warnings if a section is weak because the source pack lacks enough information.



### D. Approved-source lookup necessity gate

When `embedded_runtime_config.source_policy.mode` is `approved_source_lookup`, explicitly check whether lookup was needed for `citation_line` or `by_the_numbers`.

Do not accept `lookup_used: false` merely because the packed summary was enough for a safe draft.

Perform targeted approved-source lookup during the single review pass when all of these are true:

- approved-source lookup is allowed by `source_policy`
- lookup is technically available in the execution environment
- `citation_line` lacks formal citation metadata that an approved source is likely to provide, or `by_the_numbers` lacks actual numeric facts likely to be present in an approved source
- the lookup would use only approved reference-set locators
- the completed facts would be used only in `citation_line`, `by_the_numbers`, or `trace.source_completion`

If lookup is technically unavailable, or the approved source cannot provide usable completion, keep the draft inside the packed evidence boundary, record the fallback or no-usable-completion in `trace.source_completion`, and add a warning when the omission materially weakens `citation_line` or `by_the_numbers`.

If lookup is skipped because packed metadata and numeric facts are already sufficient, record a concise no-lookup decision in `trace.source_completion` or another trace field accepted by the caller. Do not add a warning for a well-justified skip.

If the draft remains packed-only in `approved_source_lookup` mode and `citation_line` or `by_the_numbers` is visibly weak, revise once by performing lookup if available; otherwise add a warning such as `missing_section` or `format_risk` with `affected_fields`.

### E. Source-completion trace integrity

When `approved_source_lookup` was enabled or used, check:

- every lookup-derived citation or numeric fact appears in `trace.source_completion`
- `lookup_used: false` in approved-source lookup mode has a traceable rationale, unless the caller's schema omits lookup-decision tracing
- each trace item includes `source_id`, `action`, `extracted_value`, `fields_used`, `used_in`, and `source_locator` when available
- `source_locator` values are plain URLs, DOIs, or approved locator strings, not Markdown links
- `used_in` is limited to `citation_line`, `by_the_numbers`, or `trace.source_completion` unless the caller explicitly allowed more
- successful lookup completion is recorded as trace, not warning
- warnings are used only for missing, uncertain, conflicting, inaccessible, or format-risk completion
- lookup failures fall back to packed content only when `fallback_mode` allows it

If source-completion trace is missing for lookup-derived content, add or correct trace before changing prose.
If lookup-derived content was used beyond allowed fields, remove or narrow that content.

### F. Editorial profile adherence

Check whether the draft matches the supplied profile for:

- `intellectual_entry`
- `evidence_density`
- `novelty_level`
- `practicality_level`
- `expression_style`
- `tone_register`
- `abstraction_level`
- `risk_posture`

If the draft is accurate but profile-misaligned, revise only as far as the evidence safely allows.
Never force sharper novelty, more confidence, or more operational detail than the references support.
The profile must not alter the fixed Capsule structure.

### G. Readability and target-language naturalness

Check for target-language-specific awkwardness, including:

- literal-translation phrasing
- overly rigid connectors or discourse markers
- unnatural sentence flow
- repetitive discourse patterns
- register mismatch for the target language
- phrasing that is grammatically correct but not idiomatic
- sentence weight or sequencing that makes the piece harder to read than necessary

Improve naturalness only without changing factual meaning, increasing certainty, or adding unsupported specificity.
Aim for prose that reads as if it were originally written in the target language at the intended register.

## Revision threshold

Revise once only.
Revise only when at least one issue is meaningfully present.
Meaningful issues usually include:

- invalid or non-importable JSON structure
- missing required payload fields
- high- or medium-severity trustworthiness issues
- clear approved-source lookup necessity, boundary, or trace-integrity issue
- clear section-function failure
- major editorial profile drift
- significant target-language awkwardness

If no meaningful issue is present, keep the first draft.

## Allowed revision moves

Prefer these moves:

- fix JSON shape
- move content to the correct payload field
- delete unsupported wording
- narrow scope
- weaken certainty
- replace causal verbs with association-safe phrasing
- swap awkward phrasing for idiomatic phrasing
- tighten redundant transitions
- shorten heavy sentences
- reorder nearby sentences for cleaner logic
- perform targeted approved-source lookup when the review finds it was reasonably needed and available
- add or correct `trace.source_completion` for lookup-derived facts, fallback, failed attempts, or justified no-lookup decisions
- add warnings for limitations discovered during review

## Disallowed revision moves

Do not:

- add new substantive claims
- add new source-derived facts outside allowed lookup destinations
- add new evidence summaries not already present in the draft plan
- make the draft sound more certain than the references allow
- add practical instructions that were not already safely implied
- revise repeatedly after the single pass is complete
