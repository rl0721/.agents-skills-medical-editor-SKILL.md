# Sample input and output

## Sample Drafting ContextPack with approved-source lookup

```json
{
  "schema_version": "1.0",
  "module_type": "drafting",
  "module_name": "capsule-draft-writer-edt-prf",
  "meta": {
    "generated_at": "2026-07-02T00:00:00Z",
    "object_type": "topic_prototype",
    "object_id": 3,
    "context_pack_id": 17,
    "run_id": "capsule-draft-writer-edt-prf-cp-17-tp-3",
    "language": "en",
    "market": "multi_market",
    "format_variant": "standard_newsletter",
    "skill_name": "capsule-draft-writer-edt-prf",
    "skill_version": "2026-07-03"
  },
  "task": {
    "goal": "Draft one fixed-section newsletter capsule from the approved topic prototype.",
    "topic_prototype": {
      "id": 3,
      "working_title": "Your brain on high-fat cheese",
      "working_angle": "Explain why a long observational study complicates simple low-fat messaging while remaining cautious about causality and practical interpretation.",
      "summary": "The approved angle is nuance: higher full-fat dairy intake was linked with lower dementia risk in a Swedish cohort, but the finding is observational and should not become a licence to overeat high-fat foods.",
      "selected_source_ids": [1]
    },
    "reference_set": [
      {
        "source_id": 1,
        "external_ref": "source-1",
        "title": "Full-fat dairy intake and dementia risk",
        "authors": "Du Y, et al.",
        "journal": "Neurology",
        "year": "2026",
        "volume": "106",
        "issue": "2",
        "article_number": "e214343",
        "canonical_url": "https://www.neurology.org/doi/10.1212/WNL.0000000000214343",
        "open_url": "https://www.neurology.org/doi/10.1212/WNL.0000000000214343",
        "evidence_note": "A 25-year study of 27,670 Swedish adults linked higher high-fat dairy intake, including high-fat cheese and cream, with lower dementia risk. Observational design; cannot prove causation. Eating at least 50 g/day of high-fat cheese was linked with about 20% lower dementia risk than eating under 15 g/day."
      }
    ]
  },
  "embedded_runtime_config": {
    "pillar": "healthy ageing",
    "editorial_profile": {
      "id": 1,
      "code": "consumer_smart_brevity_balanced",
      "name": "consumer smart brevity balanced",
      "intellectual_entry": "consumer_behavior",
      "evidence_density": "medium",
      "novelty_level": "measured",
      "practicality_level": "lightly_practical",
      "expression_style": "crisp",
      "tone_register": "consumer_premium",
      "abstraction_level": "balanced",
      "risk_posture": "balanced"
    },
    "source_policy": {
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
        "claims that change the approved prototype"
      ]
    },
    "duplication_rules": {"avoid_recent_framing_overlap": true},
    "output_requirements": {
      "target_word_count": 240,
      "required_payload_fields": [
        "object_type",
        "topic_prototype_id",
        "editorial_profile_id",
        "title",
        "citation_line",
        "big_picture",
        "whats_new",
        "by_the_numbers",
        "why_it_matters",
        "yes_but",
        "what_it_means_for_you",
        "reference_ids"
      ]
    }
  },
  "compact_memory_context": {
    "related_capsules": [],
    "do_not_repeat": []
  },
  "briefing": "Write a consumer-facing health Capsule. Keep the finding interesting but medically cautious."
}
```

## Sample output using packed facts only

```json
{
  "schema_version": "1.0",
  "module_name": "capsule-draft-writer-edt-prf",
  "module_type": "drafting",
  "run_id": "capsule-draft-writer-edt-prf-cp-17-tp-3",
  "result_status": "success",
  "warnings": [],
  "meta": {
    "generated_at": "2026-07-02T00:00:00Z",
    "object_type": "topic_prototype",
    "object_id": 3,
    "context_pack_id": 17,
    "run_id": "capsule-draft-writer-edt-prf-cp-17-tp-3",
    "language": "en",
    "market": "multi_market",
    "format_variant": "standard_newsletter",
    "skill_name": "capsule-draft-writer-edt-prf",
    "skill_version": "2026-07-03"
  },
  "payload": {
    "object_type": "capsule",
    "external_ref": "capsule-draft-cp-17-tp-3",
    "topic_prototype_id": 3,
    "editorial_profile_id": 1,
    "title": "Your brain on high-fat cheese",
    "citation_line": "Du Y, et al. Neurology. 2026;106(2):e214343.",
    "big_picture": "Dementia is a worry for many families, and diet is one everyday choice people often connect with long-term brain health. But food advice is usually simpler than the science behind it.",
    "whats_new": "A 25-year study of 27,670 Swedish adults linked higher full-fat dairy intake with a lower dementia signal.",
    "by_the_numbers": "Eating at least 50 g a day of high-fat cheese was linked with about 20% lower dementia risk than eating under 15 g a day. High-fat cream showed a similar, smaller signal.",
    "why_it_matters": "This complicates the neat low-fat-is-always-better story. It suggests dairy fat, or the whole-food matrix around it, may matter more than the label alone.",
    "yes_but": "This was observational research, so it cannot prove cause and effect. Cheese-lovers may also have differed in other ways, and a brain-health signal does not erase fat and salt trade-offs.",
    "what_it_means_for_you": "Moderate full-fat dairy may look less alarming than simple messages suggest. Reassuring, not a licence: let your overall health and clinician's advice guide your choices.",
    "source_note": "Draft based on the selected prototype and supplied reference sources.",
    "language": "en",
    "format_variant": "standard_newsletter",
    "status": "draft",
    "word_count": 159,
    "reference_ids": [1]
  }
}
```

## Sample output with approved-source completion trace

Use this pattern when an approved source lookup supplies citation metadata or numeric facts not explicitly available in the packed summaries.

```json
{
  "schema_version": "1.0",
  "module_name": "capsule-draft-writer-edt-prf",
  "module_type": "drafting",
  "run_id": "capsule-draft-writer-edt-prf-cp-18-tp-36",
  "result_status": "success",
  "warnings": [],
  "meta": {
    "generated_at": "2026-07-03T00:00:00Z",
    "object_type": "topic_prototype",
    "object_id": 36,
    "context_pack_id": 18,
    "run_id": "capsule-draft-writer-edt-prf-cp-18-tp-36",
    "language": "en",
    "market": "multi_market",
    "format_variant": "standard_newsletter",
    "skill_name": "capsule-draft-writer-edt-prf",
    "skill_version": "2026-07-03"
  },
  "payload": {
    "object_type": "capsule",
    "external_ref": "capsule-draft-cp-18-tp-36",
    "topic_prototype_id": 36,
    "editorial_profile_id": 4,
    "title": "Biological age needs a reality check",
    "citation_line": "Nature Medicine. 2024. Validation of biomarkers of aging.",
    "big_picture": "A number that says whether your body is older or younger than your birthday sounds irresistible. The harder question is whether that number has been validated well enough to mean something useful.",
    "whats_new": "A Nature Medicine review frames biological-age biomarkers as promising research tools that still need clearer validation before they can guide clinical translation.",
    "by_the_numbers": "A related Nature Aging study followed 108 adults aged 25-75 and reported molecular shifts around ages 44 and 60. Those numbers make ageing biology feel measurable, but they do not make every biological-age score clinically ready.",
    "why_it_matters": "Longevity content can easily turn a complex biomarker into a simple verdict. The more responsible question is what the score predicts, in whom, against which benchmark, and whether it changes a real decision.",
    "yes_but": "Biological-age tools may help researchers capture ageing biology more precisely than birthdays alone. But validation can vary by platform, population, outcome, and intended use, so a consumer score is not automatically a diagnosis or plan.",
    "what_it_means_for_you": "Treat biological age as an emerging signal, not a final verdict. Do not ask only what your score is; ask what it has been proven to predict for someone like you.",
    "source_note": "Draft based on the selected prototype, approved references, and source-completion trace.",
    "language": "en",
    "format_variant": "standard_newsletter",
    "status": "draft",
    "word_count": 189,
    "reference_ids": [85, 84]
  },
  "trace": {
    "source_completion": [
      {
        "source_id": 85,
        "action": "citation_metadata_extracted",
        "extracted_value": "Nature Medicine. 2024. Validation of biomarkers of aging.",
        "fields_used": ["journal", "year", "title"],
        "used_in": ["citation_line"],
        "source_locator": "https://doi.org/10.1038/s41591-023-02784-9"
      },
      {
        "source_id": 84,
        "action": "numeric_fact_extracted",
        "extracted_value": "108 adults aged 25-75; molecular shifts around ages 44 and 60",
        "fields_used": ["study_population", "age_range", "age_shift_points"],
        "used_in": ["by_the_numbers"],
        "source_locator": "https://doi.org/10.1038/s43587-024-00692-2"
      }
    ]
  }
}
```

## Sample partial success for unavailable lookup and weak numbers

```json
{
  "schema_version": "1.0",
  "module_name": "capsule-draft-writer-edt-prf",
  "module_type": "drafting",
  "run_id": "capsule-draft-writer-edt-prf-cp-18-tp-4",
  "result_status": "partial_success",
  "warnings": [
    {
      "code": "missing_section",
      "severity": "medium",
      "message": "approved source lookup was unavailable and the packed source information did not contain enough meaningful numeric facts for by_the_numbers.",
      "affected_fields": ["by_the_numbers"],
      "reference_ids": [2]
    }
  ],
  "meta": {
    "object_type": "topic_prototype",
    "object_id": 4,
    "context_pack_id": 18,
    "run_id": "capsule-draft-writer-edt-prf-cp-18-tp-4",
    "language": "en",
    "market": "multi_market",
    "format_variant": "standard_newsletter",
    "skill_name": "capsule-draft-writer-edt-prf",
    "skill_version": "2026-07-03"
  },
  "payload": {
    "object_type": "capsule",
    "external_ref": "capsule-draft-cp-18-tp-4",
    "topic_prototype_id": 4,
    "editorial_profile_id": 1,
    "title": "A useful ageing signal",
    "citation_line": "Nature Aging. 2024. Nonlinear dynamics of multi-omics profiles during human aging.",
    "big_picture": "Ageing often feels gradual, but the body may not change at a perfectly steady pace. That matters because timing can shape how people think about prevention and monitoring.",
    "whats_new": "The supplied source describes measurable biological shifts across adulthood, suggesting that some ageing signals may cluster around particular life stages.",
    "by_the_numbers": "The packed source points to midlife and later-life timing signals, but it does not provide enough extractable numeric detail for a stronger numbers section.",
    "why_it_matters": "The finding challenges the simple idea that ageing is one smooth slope. It makes the timing of health changes part of the story.",
    "yes_but": "This does not mean everyone ages in the same way or on the same schedule. It is a population-level signal, not a personal prediction tool.",
    "what_it_means_for_you": "The useful takeaway is awareness, not alarm. Midlife health planning may benefit from nuance, but personal decisions still need individual context.",
    "source_note": "Draft based on the selected prototype and supplied reference sources; numbers section needs editor review.",
    "language": "en",
    "format_variant": "standard_newsletter",
    "status": "draft",
    "word_count": 141,
    "reference_ids": [2]
  },
  "trace": {
    "source_completion": [
      {
        "source_id": 2,
        "action": "lookup_unavailable_fallback_used",
        "extracted_value": null,
        "fields_used": [],
        "used_in": ["by_the_numbers"],
        "source_locator": "approved source locator unavailable in execution environment"
      }
    ]
  }
}
```
