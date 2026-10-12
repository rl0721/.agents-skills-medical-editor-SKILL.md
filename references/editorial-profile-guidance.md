# Editorial profile guidance

Treat the drafting editorial profile as a binding style and risk-control layer after the canonical ModuleImportEnvelope, fixed Capsule section structure, reliability rules, approved-source boundary, and explicit output requirements.

The profile must not change the output shape.
The output shape is always the Capsule payload schema described in `capsule-schema.md`.
Use the profile to tune how the Capsule is written inside that shape.
The profile must not authorize new source discovery or broader use of lookup-derived facts. Source policy controls lookup, and lookup-derived facts remain limited to the caller-approved output uses.

## Expected drafting-profile shape

Expect fields like:

- `id`
- `code`
- `name`
- `intellectual_entry`
- `evidence_density`
- `novelty_level`
- `practicality_level`
- `expression_style`
- `tone_register`
- `abstraction_level`
- `risk_posture`

## Key variables and how they affect the Capsule

### `intellectual_entry`

Use this to choose the opening move and the primary interpretive lens.

Examples:

- `clinical_nuance` -> open with a bounded care, evidence, interpretation, or everyday health tension
- `consumer_behavior` -> open with perception, choice friction, behavior, or daily relevance
- `systems_lens` -> use only if the Capsule is for professional readers; emphasize workflow, market, policy, or access implications
- `public_health` -> open with population-level relevance while avoiding unsupported prevalence numbers

Primary fields affected:

- `big_picture`
- `why_it_matters`
- `what_it_means_for_you`

### `evidence_density`

Use this to decide how much explicit evidence appears in the short format.

- `low` -> lighter attribution, fewer numbers, more reader-facing synthesis
- `medium` -> main finding plus 1-3 numbers
- `high` -> include the strongest supported context and numbers, but do not overload the Capsule

Primary fields affected:

- `whats_new`
- `by_the_numbers`
- `yes_but`
- optional `trace.claim_source_map`

Even with high evidence density, keep the output consumer-readable and avoid p-values, confidence intervals, model names, and dense methods language unless required.

### `novelty_level`

Use this to decide how fresh the framing can feel.

- `low` -> prefer clarity and familiar framing
- `measured` or `medium` -> add a fresh angle without stretching evidence
- `high` -> seek a sharper headline or contrast, but only inside the supplied evidence envelope

Primary fields affected:

- `title`
- `why_it_matters`

### `practicality_level`

Use this to decide how concrete the reader takeaway can be.

- lower practicality -> stay closer to interpretation and nuance
- balanced or lightly practical -> include bounded everyday meaning when supported
- higher practicality -> make implications clearer, but still avoid direct medical advice or behavior-change commands

Primary fields affected:

- `yes_but`
- `what_it_means_for_you`

### `expression_style`

Use this to tune sentence shape and rhythm.

Examples:

- `crisp` -> shorter sentences, punchier transitions
- `analytical` -> more interpretive sequencing and explicit distinctions
- `executive` -> concise, decision-oriented phrasing
- `conversational` -> warmer reader-facing wording without becoming casual or unserious

Primary fields affected:

- all public payload text fields

### `tone_register`

Use this to set social posture.

Examples:

- `professional_neutral` -> composed and informative
- `consumer_premium` -> polished, direct, lightly conversational, non-generic
- `analytical` -> interpretive, specific, evidence-aware
- `premium` -> confident, selective, restrained

Primary fields affected:

- all public payload text fields

### `abstraction_level`

Use this to decide whether the Capsule sits closer to concept, evidence interpretation, everyday choice, or implementation detail.

- more abstract -> category logic and interpretation
- balanced -> connect the finding to everyday meaning
- more concrete -> include practical caveats and reader takeaway, only when supported

Primary fields affected:

- `big_picture`
- `why_it_matters`
- `what_it_means_for_you`

### `risk_posture`

Use this as the hard cap on assertiveness.

- `conservative` -> qualify aggressively, prefer narrower claims, and use more caveat
- `balanced` -> make supported synthesis while avoiding overreach
- `assertive` -> sharpen framing only when evidence clearly supports it

Primary fields affected:

- `whats_new`
- `by_the_numbers`
- `yes_but`
- `what_it_means_for_you`

## Section-level application

Use the profile across sections:

- `title` -> reflect novelty and tone without overselling
- `big_picture` -> choose reader-facing entry point from `intellectual_entry`
- `whats_new` -> set evidence detail from `evidence_density` and certainty from `risk_posture`
- `by_the_numbers` -> choose number density from `evidence_density`
- `why_it_matters` -> express the central interpretation at the right abstraction level
- `yes_but` -> calibrate caveat strength from `risk_posture`
- `what_it_means_for_you` -> calibrate usefulness from `practicality_level` without giving direct advice

## Continuity rule

Do not simulate a different drafting profile from the one provided.
Do not treat the profile as user-preference decoration.
Assume editorial continuity between upstream prototype shaping and draft execution.

## Conflict rule

If the editorial profile suggests a sharper, more concrete, or more practical take but the references do not support it, follow the evidence and downgrade assertiveness or specificity.
If the profile would naturally produce a mini-essay style, keep only the profile's tone, angle, and risk posture; do not change the fixed Capsule structure.
If the profile suggests a more practical or market-specific interpretation, do not use lookup-derived numeric facts to strengthen recommendations or expand the approved angle.

## Missing-field rule

If non-critical trace fields such as `code` or `name` are absent, continue normally.
If critical drafting-control fields are absent, either narrow the draft with warnings or return failure JSON, depending on how much of the Capsule can still be supported safely.
