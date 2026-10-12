# Drafting ContextPack

This skill expects one ContextPack for one approved topic prototype.
The Context Pack is the editorial authority.
Approved references are the evidence boundary.
The Draft Skill may complete missing factual details only inside that boundary when the source policy allows it.

## Required fields for normal drafting

Treat these fields as required for normal app-driven runs:

- `meta.market`
- `meta.language`
- `meta.context_pack_id`
- `meta.run_id` or another stable run identity
- `meta.skill_name`
- `meta.skill_version`
- `meta.format_variant` when the app supplies one
- `task.topic_prototype`
- `task.reference_set`
- `embedded_runtime_config.pillar`
- `embedded_runtime_config.editorial_profile`
- `embedded_runtime_config.source_policy`
- `embedded_runtime_config.duplication_rules`
- `embedded_runtime_config.output_requirements`
- `compact_memory_context.related_capsules`
- `compact_memory_context.do_not_repeat`
- `briefing`

## Recommended fields

Use when present:

- `meta.generated_at`
- `meta.object_type`
- `meta.object_id`
- `meta.skill_build_ref`
- `task.topic_prototype.topic_prototype_id`
- `task.topic_prototype.id`
- `task.topic_prototype.prototype_editorial_profile_id`
- `compact_memory_context.evidence_note_summary`
- primary source citation metadata in `task.reference_set`
- source DB IDs in `task.reference_set`
- source external references in `task.reference_set`
- key numeric facts, structured facts, excerpts, or article metadata in `task.reference_set`

## Expected `task.topic_prototype`

The prototype is the approved angle.
Do not ideate beyond it.
Use it to extract:

- topic prototype ID
- approved title or angle
- reader-relevant framing
- central finding or tension
- intended audience or market, when present
- high-level caveat, if present
- useful phrase constraints or avoided framings

## Expected `task.reference_set`

The reference set is the only evidence base for factual support.
Each reference should ideally include:

- numeric DB source ID, e.g. `id`, `source_id`, or equivalent
- stable `external_ref` when numeric ID is unavailable
- `reference_id` when used by the pack
- title
- authors or first author
- journal or source name
- year
- volume, issue, article number, pages, DOI, PMID, or URL when supplied
- abstract, summary, relevant excerpt, evidence notes, or structured extraction
- source type and reliability information when available
- `open_url`, `canonical_url`, `url_or_locator`, or DOI identifier for approved lookup when allowed

## Source policy

The source policy controls whether the skill may look beyond the packed summaries to the approved references themselves.
Normal app-generated packs should provide:

```json
{
  "mode": "approved_source_lookup",
  "fallback_mode": "packed_only",
  "allowed_lookup_scope": [
    "reference_set.open_url",
    "reference_set.canonical_url",
    "reference_set.url_or_locator",
    "reference_set.primary_identifier_value when DOI"
  ],
  "allowed_extraction": [
    "citation metadata",
    "key numeric facts for by_the_numbers",
    "study population",
    "follow-up duration",
    "age range",
    "exposure, comparator, and outcome details",
    "official statistics explicitly reported in the approved source"
  ],
  "allowed_output_use": [
    "citation_line",
    "by_the_numbers",
    "trace.source_completion"
  ],
  "disallowed": [
    "new sources",
    "new editorial angle",
    "new recommendation",
    "unsupported clinical implication",
    "claims that change the approved prototype",
    "using completed facts to strengthen recommendations"
  ]
}
```

### `packed_only`

Use only information already inside the Context Pack.
Do not open approved source URLs or DOI links.
If citation metadata or numbers are insufficient, return a safe draft with warnings or `partial_success` when appropriate.

### `approved_source_lookup`

Consult only approved references listed in `task.reference_set`, using only locators supplied in that reference object.
Allowed lookup is for source completion, not for research.
When this mode is active, targeted lookup should be considered for `citation_line` and `by_the_numbers` even if the packed summary is enough for a safe draft. Do not skip lookup when formal citation metadata or meaningful numeric facts are likely available from approved references and would improve those fields.
Use lookup-derived facts only in:

- `citation_line`
- `by_the_numbers`
- `trace.source_completion`

Do not use lookup-derived facts to add a new angle, strengthen recommendations, add mechanisms, or increase clinical specificity.
Do not add sources discovered through links, references, related articles, search pages, or external pages not already in `task.reference_set`.

If lookup cannot be performed and `fallback_mode` is `packed_only`, continue from the Context Pack and warn only when the missing completion materially affects output quality.
If lookup finds facts that contradict the packed summary or approved angle, do not silently reconcile. Narrow the draft, warn, or return `partial_success` or `failed` depending on severity.

## Source ID selection

Build `payload.reference_ids` from identifiers supplied by `task.reference_set`.
Prefer numeric DB IDs because the app can resolve them most safely.
Use string IDs only when they exactly match the supplied Source `external_ref` values.
Do not invent source IDs.
Do not convert a human-readable citation, article title, or local note label into a reference ID.

## Citation metadata handling

Always attempt to produce `payload.citation_line`.
Use supplied metadata and, when allowed, approved-source lookup completion.
Do not search the web generally to complete missing citation details.

Preferred citation line format:

`Author(s). Journal/Publisher. Year;volume(issue):pages or article number.`

When metadata is partial, use the fallback order in `capsule-schema.md`.
If no reliable citation line can be produced, set `citation_line` to `null`, return `partial_success`, and add a warning with `affected_fields: ["citation_line"]`.

## `by_the_numbers` handling

Use actual supported numeric facts when available.
Do not fill `by_the_numbers` with source anchors, publication years, evidence types, or datasets.
When approved lookup is allowed, use it to complete only the numeric facts needed for this field, such as participant count, age range, follow-up duration, exposure threshold, comparator, effect size, or official statistics explicitly reported in an approved source.
If meaningful numbers are still unavailable, keep the section short and add a warning or `partial_success` if the field needs editor review.

## Trace expectations

When lookup-derived information is used, include `trace.source_completion`.
Each trace item should identify:

- `source_id`
- `action`: e.g. `citation_metadata_extracted`, `numeric_fact_extracted`, `lookup_unavailable_fallback_used`
- `extracted_value`: the actual value or citation metadata completed
- `fields_used`: structured labels such as `authors`, `journal`, `year`, `volume`, `pages`, `study_population`, `age_range`, `follow_up`, `official_statistic`
- `used_in`: `citation_line`, `by_the_numbers`, or both
- `source_locator`: approved locator used; write this as a plain URL, DOI, or locator string, not a Markdown link

Successful approved-source completion is normally not a warning.
Warnings are for missing, uncertain, conflicting, inaccessible, or format-risk information.

## Drafting-profile assumption

This skill expects the drafting pack to carry the full editorial profile linked to the approved topic prototype.
Treat the received profile as the same profile family used to shape the prototype upstream.
Do not assume the caller can override the profile at drafting time.
Do not require `editorial_profile.active` in the pack. If the pack reached this skill through the normal app path, active-status checks should already have happened upstream.

## Expected editorial profile shape

The drafting pack should normally include a full profile object such as:

```json
{
  "id": 17,
  "code": "clinical_nuance_balanced_practical",
  "name": "clinical nuance balanced practical",
  "intellectual_entry": "clinical_nuance",
  "evidence_density": "high",
  "novelty_level": "measured",
  "practicality_level": "lightly_practical",
  "expression_style": "crisp",
  "tone_register": "consumer_premium",
  "abstraction_level": "balanced",
  "risk_posture": "balanced"
}
```

For normal drafting, the most important profile fields are:

- `id`
- `intellectual_entry`
- `novelty_level`
- `evidence_density`
- `practicality_level`
- `expression_style`
- `tone_register`
- `abstraction_level`
- `risk_posture`

`code` and `name` are useful for traceability when present.

## Minimum interpretation rules

Validate presence before drafting.
Treat missing required fields as blocking only when they prevent a safe importable payload.
Treat missing but non-critical fields as `partial_success` with warnings.
Treat `output_requirements` as a tuning layer for length, optional traceability, and approved extensions only; it cannot replace the canonical ModuleImportEnvelope or fixed Capsule sections.
Treat `task.topic_prototype` as the approved angle, not a suggestion for new ideation.
Treat `task.reference_set` as the approved source boundary.
Treat `related_capsules` and `do_not_repeat` as anti-duplication context, not source material to paraphrase.

## Practical input reading order

1. `meta`
2. `task.topic_prototype`
3. `embedded_runtime_config.output_requirements`
4. `embedded_runtime_config.source_policy`
5. `embedded_runtime_config.editorial_profile`
6. `task.reference_set`
7. `compact_memory_context.related_capsules`
8. `compact_memory_context.do_not_repeat`
9. `briefing`
10. the remaining runtime config fields

## What to extract before drafting

Extract these working notes mentally before writing:

- canonical envelope identity
- topic prototype ID and approved angle
- source policy mode, fallback mode, allowed lookup scope, and allowed output use
- editorial profile ID and usable profile controls
- market, language, and format variant
- target length and any app-specific output requirements that fit inside the canonical envelope
- approved evidence boundary
- exact reference IDs to cite
- citation line metadata and whether it was packed or lookup-completed
- reader-facing big-picture angle
- main finding for `whats_new`
- 2-4 actual memorable supported numbers for `by_the_numbers`, or the limitation if not available
- interpretive angle for `why_it_matters`
- evidence caveat and practical caveat for `yes_but`
- cautious reader takeaway for `what_it_means_for_you`
- recent framings or phrases to avoid
- any `trace.source_completion` entries

## Defensive failure rule

Return `failed` when the pack and approved references cannot support a safe importable Capsule.
Return `partial_success` when the payload is importable but needs review.
Retain defensive failure behavior even if the app normally blocks incomplete drafting packs before this skill runs.
