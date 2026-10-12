# Warning codes

Use app-recognized warning codes whenever possible.
Each warning should include `code`, `severity`, and `message`.
Add `affected_fields` when the warning concerns specific payload, meta, or trace fields.

## Warning object shape

```json
{
  "code": "missing_section",
  "severity": "medium",
  "message": "citation metadata is incomplete, so the citation line needs editor review.",
  "affected_fields": ["citation_line"]
}
```

Required fields:

- `code`
- `severity`: `low` | `medium` | `high`
- `message`

Optional fields:

- `affected_fields`
- `reference_ids`

## Standard codes

### `insufficient_context`

Severity: `high`

Use when required pack fields are missing, the editorial profile is too incomplete for safe drafting, or the context is too thin to support a stable draft.
Use `failed` if no importable draft can be produced.
Use `partial_success` if the payload is complete but needs review.

### `insufficient_reference_support`

Severity: `high`

Use when the reference set cannot support the central approved angle or a required core claim.
Usually block with `failed` if the central angle cannot be supported.

### `missing_section`

Severity: `medium` by default; `high` when an import-critical field is absent and cannot be safely set.

Use when a required field or section cannot be fully populated from the available pack.
Common use:

```json
{
  "code": "missing_section",
  "severity": "medium",
  "message": "citation metadata is incomplete, so citation_line is null and needs editor review.",
  "affected_fields": ["citation_line"]
}
```

Also use for weak or placeholder sections only when the field is present but needs human attention.
Do not use this when the issue is purely tone or style.

### `claim_support_gap`

Severity: `medium`

Use when one or more useful claims need qualification because direct support is incomplete.
Prefer narrowing the claim before warning.
Use this for practical caveats, mechanisms, subgroup implications, or consumer takeaways that are only partly supported.

### `tone_drift`

Severity: `medium`

Use when the draft can be completed but the editorial profile cannot be followed perfectly because profile controls conflict with evidence, length, safety, or the fixed section structure.

### `over_repetition_risk`

Severity: `medium`

Use when related capsules or do-not-repeat context suggest residual framing overlap after revision.

### `format_risk`

Severity: `low`

Use when the draft is usable but may sit near format, length, section-boundary, or envelope limits.
Examples:

- word count is above target but below hard upper guide
- a section is longer than ideal but still importable
- citation line is safe but not in ideal citation style
- the pack format is unusual but successfully parsed

### `provenance_gap`

Severity: `medium` by default; `high` when importability is affected.

Use when provenance fields such as `run_id`, `context_pack_id`, `object_id`, `skill_version`, or source identifiers are incomplete.
Do not invent missing provenance values except the canonical `skill_name` fallback.

## Code selection rules

- Prefer one precise warning over several vague warnings.
- Use `missing_section` with `affected_fields` for missing or null `citation_line` rather than inventing a specialized code unless the app explicitly supports it.
- Use `claim_support_gap` for evidence limitations inside a populated section.
- Use `format_risk` for length, section boundary, or envelope concerns that do not threaten import.
- Use `insufficient_reference_support` when the core angle itself is not supported.
- Use `failed` rather than many warnings when no safe importable draft can be produced.

## Source-completion warning guidance

Approved-source lookup that succeeds is not a warning. Record it in `trace.source_completion`.
Use warnings only when lookup or completion creates a quality or review issue.

Common mappings:

- missing or unusable citation metadata -> `missing_section` with `affected_fields: ["citation_line"]`
- no meaningful supported numeric facts for `by_the_numbers` -> `missing_section` with `affected_fields: ["by_the_numbers"]` when the section needs review
- approved lookup unavailable and fallback materially weakens output -> `format_risk` or `missing_section`, depending on affected field
- approved lookup enabled but skipped while `citation_line` or `by_the_numbers` remains visibly weak -> `format_risk` or `missing_section` with the affected field
- source values conflict with the packed summary or approved angle -> `claim_support_gap` or `insufficient_reference_support`, depending on severity
- lookup would require a new source or open research -> do not perform lookup; use `insufficient_context` or `insufficient_reference_support` if this blocks a safe draft

When a warning concerns lookup-derived or lookup-missing information, include `reference_ids` when possible and use `affected_fields` to identify the impacted payload or trace field.
