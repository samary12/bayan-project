# DATA CARD — Bayan

## Dataset identity

- Name/version: Bayan Day 2 Classification Dataset — course sample fixture
- Source/creator: Bayan Applied NLP Course repository (`almiyead-rgb/bayan-applied-nlp-course`)
- License/permission: Course-provided educational dataset; no separate dataset license is explicitly stated in the classification notebook.
- Data hash or immutable revision: SHA256 `c50de92fdab1aa36b19cf4c0f6e31c0bc521f70d6690635e839d7ba9ec7e9a77`
- Intended educational task: Bilingual Arabic-English text classification, with topic and sentiment annotations used for course exercises.

## Composition

| Split | Rows | Arabic | English | Groups | Notes |
|---|---:|---:|---:|---:|---|
| train | 24 | 12 | 12 | 12 | used for model fitting |
| validation | 8 | 4 | 4 | 4 | used for model selection and validation |
| frozen test | 8 | 4 | 4 | 4 | opened once after freeze |

## Fields and labels

| Field/label | Meaning | Allowed values | Missing-value rule |
|---|---|---|---|
| `example_id` | Unique example identifier | non-empty string | must be present |
| `group_id` | Group identifier used for leakage-safe splitting | non-empty string | must be present |
| `split` | Predefined dataset split | `train`, `validation`, `test` | must be present |
| `language` | Text language | `ar`, `en` | must be present |
| `text` | Input text | Arabic or English text | must be present |
| `topic` | Classification target | `digital_service`, `health`, `permit`, `transport` | must be present |
| `sentiment` | Sentiment annotation | `positive`, `negative`, `neutral` | must be present |

## Collection/generation

The dataset is a small bilingual synthetic/general-purpose educational fixture distributed with the Bayan Applied NLP Course. It contains paired Arabic and English examples covering digital services, permits, health, and transport. It is intended to demonstrate the NLP workflow rather than represent real production traffic or a population-level benchmark. The project does not claim independent human annotation or external real-world collection beyond the course-provided fixture.

## Cleaning and preprocessing

- Display copy rule: Preserve the original input as `display_text`; create a separate derived `model_text` for modelling.
- PII masking rule: Mask course-supported email patterns as `[EMAIL]` and Saudi mobile-number patterns as `[PHONE]`. This is an educational narrow rule and is not a production PII detector.
- Arabic profile/version: Named Arabic normalisation profiles, version `1.0.0`; conservative processing preserves most orthographic distinctions, while the search profile additionally removes diacritics and normalises Alef/Alef-Maksura variants.
- Deduplication/grouping: Use `group_id` to keep related examples within the same split and prevent group leakage.
- Filtering/exclusions: Required rows must have valid split, group, and topic values. Unsupported split names and cross-split group reuse are rejected.

## Split and leakage controls

- Split method/seed: Predefined group-based train/validation/test split; training seed `42`.
- Group isolation evidence: `group_overlap = 0`; split contract reported `PASS`.
- Near-duplicate audit: Related bilingual/grouped examples are controlled through `group_id`; no cross-split group overlap is allowed.
- Frozen-test access date and commit: Test was evaluated once after model/validation settings were fixed. The exact first-access date is not separately recorded in Notebook 03; Gate B evidence is preserved in the repository history and metrics report.

## Known gaps and risks

- Dialects/Arabizi: The dataset is small and does not provide broad coverage of Arabic dialects or Arabizi; later Day 3 evaluation identified dialect gaps as a known issue.
- Class balance: The full dataset is balanced across four topic classes, with 10 examples per topic.
- Synthetic-to-real gap: The dataset is synthetic and very small, so measured results do not establish production quality or real-world generalisation.
- Annotation ambiguity: Short or underspecified requests can be ambiguous between related service categories.
- Small slices/uncertainty: Validation and test each contain only 8 rows, so individual errors can substantially change metrics.
- Misuse/privacy risk: The fixture should not be used to make high-stakes decisions about real people. The project preprocessing masks only limited PII patterns and must not be treated as complete privacy protection.

## Permitted and prohibited use

- Permitted educational use: Learning, reproducing course exercises, testing bilingual NLP pipelines, classification, evaluation, and research-style experimentation.
- Prohibited/high-risk use: Production deployment, high-stakes automated decisions, surveillance, profiling, or claims of population-level performance based on this small educational fixture.
- Human review: Required before using predictions outside the educational workflow, especially for ambiguous, sensitive, or consequential cases.

## Maintenance

- Owner/contact through GitHub: `samary12` — `https://github.com/samary12/bayan-project`
- Change/version policy: Any dataset, preprocessing, split, or label change should be documented and should trigger re-running validation and evaluation artifacts.
- Index/model rebuild triggers: Rebuild/re-evaluate when the dataset, preprocessing profile, label mapping, embedding model, tokenizer, retrieval corpus, or serving model changes.
