# Drafting rules

## Editorial target

Write a short consumer-facing healthcare Capsule, not a journal abstract and not a premium mini-essay.
The final draft should feel like a polished Smart Brevity-style health newsletter item:

- clear
- short
- structured
- memorable
- lightly conversational
- practical
- medically cautious without sounding defensive

Write for reader impact first, then precision.
Keep precision inside the supplied evidence envelope.

## Fixed Capsule structure

Draft into these payload fields:

1. `title`
2. `citation_line`
3. `big_picture`
4. `whats_new`
5. `by_the_numbers`
6. `why_it_matters`
7. `yes_but`
8. `what_it_means_for_you`

Each of the six body sections should be one compact paragraph.
Do not include visible section labels inside the field values.


## Approved-source lookup boundaries

Treat the Context Pack as the editorial authority and the approved reference set as the evidence boundary.
When `source_policy.mode` is `approved_source_lookup`, use lookup only to complete missing details from already approved references. Do not use general search, related-article links, reference lists, or newly discovered sources.

Do not skip targeted lookup merely because the packed summary is sufficient for a safe draft. If `citation_line` lacks formal metadata, or `by_the_numbers` lacks actual numeric facts likely to be present in an approved source, perform approved-source lookup when technically available. Skip lookup only when the packed Context Pack already contains sufficiently formal citation metadata and meaningful numeric facts, lookup is technically unavailable, or the caller disables lookup. Record the reason for any skip or fallback in `trace.source_completion` when `approved_source_lookup` mode is active.

Allowed lookup-derived content is limited to:

- formal citation metadata for `citation_line`
- actual numeric facts needed for `by_the_numbers`
- audit entries in `trace.source_completion`

Do not use lookup-derived facts to change the angle, add mechanisms, strengthen recommendations, or increase clinical specificity.
If lookup is unavailable and `fallback_mode` permits `packed_only`, continue from packed content and warn only when missing completion affects quality.
Record all lookup-derived values, failed attempts, unavailable fallback, or justified no-lookup decisions in `trace.source_completion`.
Use plain URLs or approved locator strings in trace fields; do not format trace locators as Markdown links.
Successful lookup completion is traceable but not automatically a warning.

## Length

Default target: 180-280 words across the six body sections.
Hard upper guide: 330 words unless `output_requirements` explicitly overrides it.
Very short output is acceptable only when evidence is thin, the source is simple, or the caller explicitly requests brevity.

`payload.word_count` must count only these six fields:

- `big_picture`
- `whats_new`
- `by_the_numbers`
- `why_it_matters`
- `yes_but`
- `what_it_means_for_you`

Do not count `title` or `citation_line`.

## Section guidance

### `title`

Create a short, memorable, consumer-friendly headline.
Aim for concrete wording and reader relevance.
Mild playfulness is allowed when medically responsible.
Avoid generic research headlines.

Good patterns:

- a twist in the dairy story
- your brain on high-fat cheese
- ageing may not move smoothly
- a fuller-fat food puzzle
- not all signals age alike

Avoid patterns like:

- new research on dementia
- study finds association
- a review of lifestyle risk
- clinical implications of dietary exposure

### `citation_line`

Use a formal short citation line, not a narrative source summary.
Prefer one primary source that best supports the Capsule's central editorial angle.
Prefer peer-reviewed sources over official statistics when the central claim is clinical, scientific, or translational.
Use supplied metadata and, when allowed, approved-source lookup completion.
Do not invent author names, volume, issue, pages, article numbers, dates, or publisher details.

Preferred format:

`Author(s). Journal/Publisher. Year;volume(issue):pages or article number.`

Fallback examples:

- `Author(s). Journal/Publisher. Year.`
- `Journal/Publisher. Year. Title.`
- `Title. Journal/Publisher. Year.`
- `Publisher. Title. Year.`

For official statistics, use a compact publisher-title-year citation.
If multiple sources are essential to the displayed citation line, separate short citations with semicolons.
Do not start with `Sources:` unless no formal citation can be built from supplied or approved-lookup metadata.
Keep supplemental sources in `reference_ids`, `source_note`, or `trace` rather than overloading `citation_line`.
If metadata remains incomplete, produce the best safe version and warn only when editor review is needed.

### `big_picture`

Start from the reader's world, not the study design.
Use everyday relevance such as ageing, memory, daily choices, common health advice, family concerns, or long-term health worries.
Use broad common-health context only when it does not introduce unsupported statistics, recommendations, or claims.

Good moves:

- dementia is a worry for many families
- diet is one everyday factor people connect with long-term health
- food advice is often simpler than the science behind it
- ageing can feel gradual, but biology may move unevenly

Avoid opening with:

- this cohort study evaluated...
- researchers investigated...
- this paper examined...

### `whats_new`

State the main finding or update crisply.
Include the study duration, population, exposure, comparator, and outcome when those elements are central and supported.
Use health-news language rather than methods language.
For observational evidence, use causal-safe wording.

Good causal-safe wording:

- was linked with
- was associated with
- researchers observed
- the study found a link between
- showed a signal

Avoid unsupported causal wording:

- reduced
- protected
- prevented
- caused
- drove
- improved outcomes

### `by_the_numbers`

Use actual supported numeric facts, not source anchors.
Prioritize 2-4 numbers maximum when available.
Choose numbers a consumer reader can remember:

- participant count
- follow-up duration
- age group
- exposure threshold
- main relative difference
- comparator group
- official population count or share when it directly supports market or public-health relevance
- timing signals such as ages or intervals when they are central to the finding

Do not use this section merely to list source types, publication years, datasets, or evidence anchors.
Do not overload this field with p-values, confidence intervals, model names, or technical epidemiology language unless the caller specifically requires technical detail.
If a number has practical meaning, briefly make it concrete.
Do not choose the most dramatic estimate unless it is clearly the main supported estimate.
If the pack lacks meaningful numbers and approved-source lookup cannot complete them, keep the section short and use `partial_success` or a warning when editor review is needed.

### `why_it_matters`

Explain the editorial significance.
Answer: what simple assumption, conventional wisdom, or common health story does this complicate?
This field should be interpretive, not academic.
Make the reader understand why the finding is interesting.

Useful angles:

- complicates a simple low-fat-is-always-better story
- suggests the whole-food matrix may matter, not just the nutrient label
- adds nuance to common diet or lifestyle advice
- raises a useful question for healthy ageing
- is reassuring but not definitive
- is a signal worth attention, not a new rule

Avoid:

- this finding has important clinical implications
- further studies are warranted
- the mechanism may involve...
- this adds to the literature

### `yes_but`

This field is mandatory.
Include both:

1. an evidence caveat
2. a practical caveat, when the source and broad health context safely support one

Evidence caveat examples:

- this was observational research, so it cannot prove cause and effect
- people with the exposure may have differed in other ways
- diet was measured at one point and may have changed over time
- the link may partly reflect other habits or health differences

Practical caveat examples:

- this is not a licence to overdo it
- a brain-health signal does not erase other nutrition trade-offs
- one study should not overturn personal medical advice
- the finding is interesting, not a new rule

Avoid legalistic caveats such as:

- residual confounding cannot be excluded
- findings should be interpreted with caution
- clinical recommendations cannot be made

### `what_it_means_for_you`

Give a cautious, useful reader takeaway.
Do not give direct medical advice.
Do not tell readers to start, stop, increase, decrease, or replace a diet, medication, screening, treatment, or clinical plan.
End with nuance rather than permission.

Good endings:

- the takeaway is nuance, not permission
- reassuring, not a licence
- let your overall health, not one study, guide your plate
- useful signal, not a new rule

## Evidence and safety rules

Keep these non-negotiable:

1. Do not present association as causation.
2. Do not exaggerate the result.
3. Do not imply that one study changes medical guidance.
4. Do not give diagnosis, treatment, or personalized medical advice.
5. Do not use `proves`, `prevents`, `cures`, `reverses`, or `guarantees`.
6. Preserve key context: population, exposure, comparator, outcome, and main limitation when relevant.
7. Use broad common-health context only when it helps reader understanding and does not become a recommendation.
8. Avoid external statistics, named external guidelines, or detailed mechanisms unless supplied in the Context Pack or completed from an approved reference under the source policy.
9. Keep caveats useful, not so technical that they kill reader impact.
10. Use `partial_success` and warnings when the draft is safe but limited by incomplete support.

## Style controls

Use short, confident sentences.
Prefer plain English or natural target-language equivalents.
Avoid generic wellness filler, motivational copy, and vague uplift phrasing.
Avoid abstract-like phrasing such as:

- potential associations
- clinical implications remain uncertain
- further research is needed
- adds to the body of literature
- was evaluated in a cohort

Good rhythm:

1. everyday concern
2. new finding
3. memorable numbers
4. why it complicates a simple assumption
5. practical caution
6. useful takeaway

## Repetition control

Read `related_capsules` and `do_not_repeat` before composing title, opening, and final takeaway.
Avoid repeating:

- the same opening logic
- the same contrast pair
- the same headline promise
- the same signature phrase or claim
- the same closing moral

If overlap risk remains, narrow the angle or shift the entry point.

## Target-language naturalness

The final prose should read as though it was originally written in the requested output language at the intended register.
Check for:

- literal translation phrasing
- stiff connectors
- unnatural sentence flow
- repetitive sentence openings
- register mismatch
- grammatically correct but non-idiomatic phrasing

Improve naturalness only inside the evidence-safe envelope.
Do not trade reliability for fluency.
Do not make the prose sharper, more certain, or more practical than the references allow just to make it sound smoother.
