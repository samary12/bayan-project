# بطاقة نموذج بيان | Bayan Model Card

## Artefact 1 — Classification

### Model details

- Name/version: `Bayan Classification Smoke Model`
- Base checkpoint: `distilbert/distilbert-base-multilingual-cased`
- Task: Text classification
- License/source: Hugging Face checkpoint; course implementation
- Commit SHA: `5e5f3c3`
- Owner/contact role: `Project maintainer`

### Intended use

- الاستخدام المقصود: اختبار pipeline لتصنيف النصوص ضمن مشروع Bayan التعليمي.
- المستخدمون المقصودون: مطورو ومقيّمو المشروع ضمن بيئة الدورة.
- خارج النطاق: الاستخدام الإنتاجي أو اتخاذ قرارات حقيقية عالية الأثر.

### Data and preprocessing

- Dataset ID/version: `github_course_file`
- Languages/variants: Multilingual course fixture
- Split strategy: Group-based split using `group_id`, with zero group overlap.
- PII policy: Synthetic/course data only; no real PII intended.
- Preprocessing profile/version/backend: Course preprocessing pipeline
- Tokenizer/embedding model: Tokenizer matching `distilbert/distilbert-base-multilingual-cased`

### Evaluation

| metric/slice | n | result | uncertainty | evidence file |
|---|---:|---:|---|---|
| Validation Macro-F1 | small validation split | `1.0000` | No CI reported | `reports/day2_classification_metrics.json` |
| Test Macro-F1 | small test split | `0.8667` | No CI reported | `reports/day2_classification_metrics.json` |
| Test accuracy | small test split | `0.8750` | No CI reported | `reports/day2_classification_metrics.json` |
| Baseline validation Macro-F1 | small validation split | `0.6667` | No CI reported | `reports/day2_classification_metrics.json` |

### Behavioural checks

| capability | pass rate | known failure |
|---|---:|---|
| Training smoke | PASS | Small synthetic dataset limits generalization claims |
| Baseline comparison | PASS | Validation set is small |

### Limitations and risks

1. Synthetic tiny dataset.
2. Validation set is small and was used for epoch selection.
3. Results are `MEASURED_SMOKE`, not production-quality estimates.

### Ethical and privacy notes

- Synthetic/course data only.
- No production claim is made.
- Real PII should not be introduced into this workflow.

### Reproduction

1. افتح notebook: `notebooks/03_text_classification.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت النسخ المستخدمة في بيئة الدورة.
4. شغّل Run all من commit: `5e5f3c3`.
5. قارن النتيجة مع: `reports/day2_classification_metrics.json`.

---

## Artefact 2 — NER

### Model details

- Name/version: `Bayan NER Smoke Model`
- Base checkpoint: `distilbert/distilbert-base-multilingual-cased`
- Task: Named Entity Recognition
- License/source: Hugging Face checkpoint; course implementation
- Commit SHA: `5e5f3c3`
- Owner/contact role: `Project maintainer`

### Intended use

- الاستخدام المقصود: اختبار NER alignment والتدريب والتقييم على بيانات الدورة.
- المستخدمون المقصودون: مطورو ومقيّمو مشروع Bayan.
- خارج النطاق: استخراج الكيانات من بيانات إنتاج حقيقية بدون تقييم إضافي.

### Data and preprocessing

- Dataset ID/version: `github_course_file`
- Languages/variants: Course fixture
- Split strategy: Course-defined split
- PII policy: Synthetic/course data only
- Preprocessing profile/version/backend: NER token alignment pipeline
- Tokenizer/embedding model: Tokenizer matching `distilbert/distilbert-base-multilingual-cased`

### Evaluation

| metric/slice | n | result | uncertainty | evidence file |
|---|---:|---:|---|---|
| Strict entity precision | 4 true entities / 3 predicted entities | `0.6667` | No CI reported | `reports/day2_ner_qa_metrics.json` |
| Strict entity recall | 4 true entities / 3 predicted entities | `0.5000` | No CI reported | `reports/day2_ner_qa_metrics.json` |
| Strict entity F1 | 4 true entities / 3 predicted entities | `0.5714` | No CI reported | `reports/day2_ner_qa_metrics.json` |

### Behavioural checks

| capability | pass rate | known failure |
|---|---:|---|
| NER alignment | PASS | Small fixture only |
| NER training smoke | PASS | Short CPU training |
| Finite NER loss | PASS | Does not imply production quality |

### Limitations and risks

1. Very small synthetic dataset.
2. Short smoke-training run.
3. NER quality is not a production estimate.

### Ethical and privacy notes

- Synthetic/course data only.
- Continuation subwords and special tokens use `-100` so they are ignored by the loss.
- No production claim is made.

### Reproduction

1. افتح notebook: `notebooks/04_ner_and_qa.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. استخدم نفس checkpoint/tokenizer.
4. شغّل Run all من commit: `5e5f3c3`.
5. قارن النتيجة مع: `reports/day2_ner_qa_metrics.json`.

---

## Artefact 3 — Extractive QA

### Model details

- Name/version: `Bayan QA Smoke Model`
- Base checkpoint: `distilbert/distilbert-base-multilingual-cased`
- Task: Extractive Question Answering with no-answer support
- License/source: Hugging Face checkpoint; course implementation
- Commit SHA: `5e5f3c3`
- Owner/contact role: `Project maintainer`

### Intended use

- الاستخدام المقصود: اختبار QA span extraction وno-answer policy.
- المستخدمون المقصودون: مطورو ومقيّمو مشروع Bayan.
- خارج النطاق: الإجابة على أسئلة إنتاجية أو عالية المخاطر.

### Data and preprocessing

- Dataset ID/version: `github_course_file`
- Languages/variants: Course fixture
- Split strategy: Course-defined split
- PII policy: Synthetic/course data only
- Preprocessing profile/version/backend: QA span preprocessing and post-processing
- Tokenizer/embedding model: Tokenizer matching `distilbert/distilbert-base-multilingual-cased`

### Evaluation

| metric/slice | n | result | uncertainty | evidence file |
|---|---:|---:|---|---|
| Valid span test | 1 smoke case | PASS — answer=`الرياض` | No CI | `reports/day2_ner_qa_metrics.json` |
| No-answer test | 1 smoke case | PASS — returns `None` | No CI | `reports/day2_ner_qa_metrics.json` |
| Aggregate EM/F1 | — | Not separately reported | — | `reports/day2_ner_qa_metrics.json` |

### Behavioural checks

| capability | pass rate | known failure |
|---|---:|---|
| Valid span extraction | PASS | Smoke case only |
| Honest no-answer | PASS | Threshold behaviour tested on limited fixture |
| QA training smoke | PASS | Short training run |

### Limitations and risks

1. Aggregate QA EM/F1 was not reported in the available smoke metrics.
2. Small synthetic dataset.
3. QA quality is not a production estimate.

### Ethical and privacy notes

- Synthetic/course data only.
- The system may return `None` when no valid answer is supported by the context.
- No production claim is made.

### Reproduction

1. افتح notebook: `notebooks/04_ner_and_qa.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. استخدم نفس checkpoint/tokenizer and QA post-processing policy.
4. شغّل Run all من commit: `5e5f3c3`.
5. قارن النتيجة مع: `reports/day2_ner_qa_metrics.json`.

---

## Artefact 4 — Semantic Search / Embeddings

### Model details

- Name/version: `Bayan Semantic Search`
- Base checkpoint: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
- Task: Bilingual semantic retrieval
- License/source: `Apache-2.0` / Hugging Face model card
- Commit SHA: `5e5f3c3`
- Owner/contact role: `Project maintainer`

### Intended use

- الاستخدام المقصود: استرجاع دلالي ثنائي اللغة على corpus الدورة.
- المستخدمون المقصودون: مطورو ومقيّمو مشروع Bayan.
- خارج النطاق: البحث الإنتاجي واسع النطاق أو corpus حقيقي حساس.

### Data and preprocessing

- Dataset ID/version: `bayan_day3_cases.csv`
- Languages/variants: Arabic + English, including cross-lingual retrieval cases
- Split strategy: Validation used for no-answer threshold tuning; test kept frozen for final smoke evaluation
- PII policy: Course fixture only; no real PII
- Preprocessing profile/version/backend: `arabic-search/1.0.0 + english-nfc-whitespace/1.0.0`
- Tokenizer/embedding model: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
- Embedding dimension: `384`
- Normalization: `L2`
- Index type: `FAISS IndexFlatIP`

### Evaluation

| metric/slice | n | result | uncertainty | evidence file |
|---|---:|---:|---|---|
| Test Recall@3 | 6 answerable queries | `1.0000` | Small sample | `reports/retrieval_metrics.json` |
| Test MRR@3 | 6 answerable queries | `0.6667` | Small sample | `reports/retrieval_metrics.json` |
| Arabic MRR@3 | 3 | `0.5000` | `SMALL_SLICE` | `reports/retrieval_metrics.json` |
| English MRR@3 | 3 | `0.8333` | `SMALL_SLICE` | `reports/retrieval_metrics.json` |
| Cross-lingual MRR@3 | 2 | `0.5000` | `SMALL_SLICE` | `reports/retrieval_metrics.json` |
| Monolingual MRR@3 | 4 | `0.7500` | `SMALL_SLICE` | `reports/retrieval_metrics.json` |
| No-answer accuracy — validation | course validation fixture | `1.0000` | Small fixture | `reports/retrieval_metrics.json` |
| No-answer accuracy — test | course test fixture | `1.0000` | Small fixture | `reports/retrieval_metrics.json` |

### Behavioural checks

| capability | pass rate | known failure |
|---|---:|---|
| L2 normalization contract | PASS | Does not establish production retrieval quality |
| Frozen no-answer threshold | PASS | Threshold tuned on limited validation data |
| Day 3 retrieval tests | PASS | Small course fixture |

### Limitations and risks

1. Only six answerable test queries.
2. Retrieval slices are very small and flagged `SMALL_SLICE`.
3. Results are `MEASURED_SMOKE`, not production evidence.

### Ethical and privacy notes

- Course fixture only.
- No real PII should be indexed.
- The frozen no-answer threshold is `0.4592095613479614`.
- The test split was not used for threshold tuning.

### Reproduction

1. افتح notebook: `notebooks/06_semantic_search.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت النسخ:
   - `camel-tools 1.6.0`
   - `sentence-transformers 6.0.0`
   - `faiss-cpu 1.15.0`
   - `transformers 5.15.1`
   - `tokenizers 0.22.2`
4. شغّل Run all من commit: `5e5f3c3`.
5. قارن النتيجة مع:
   - `reports/search_manifest.json`
   - `reports/retrieval_metrics.json`

---

## Artefact 5 — Cross-Encoder Reranker

### Model details

- Name/version: `Bayan Experimental Reranker`
- Base checkpoint: `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`
- Task: Reranking semantic-search candidates
- License/source: Hugging Face checkpoint; course experiment
- Commit SHA: `5e5f3c3`
- Owner/contact role: `Project maintainer`

### Intended use

- الاستخدام المقصود: تجربة تحسين ترتيب نتائج semantic search.
- المستخدمون المقصودون: مطورو ومقيّمو مشروع Bayan.
- خارج النطاق: الاعتماد عليه في الإنتاج بناءً على هذه النتائج وحدها.

### Data and preprocessing

- Dataset ID/version: `bayan_day3_cases.csv`
- Languages/variants: Arabic + English retrieval fixture
- Split strategy: Test evaluation after retrieval configuration was fixed
- PII policy: Course fixture only
- Preprocessing profile/version/backend: Same semantic-search preprocessing profile
- Tokenizer/embedding model: `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`

### Evaluation

| metric/slice | n | result | uncertainty | evidence file |
|---|---:|---:|---|---|
| MRR@3 before reranking | 6 answerable queries | `0.6667` | Small sample | `reports/retrieval_metrics.json` |
| MRR@3 after reranking | 6 answerable queries | `0.7222` | Small sample | `reports/retrieval_metrics.json` |
| MRR@3 delta | 6 answerable queries | `+0.0556` | No CI reported | `reports/retrieval_metrics.json` |
| Median rerank latency | CPU runtime | `642.42 ms` | Runtime-dependent | `reports/retrieval_metrics.json` |
| P95 rerank latency | CPU runtime | `788.43 ms` | Runtime-dependent | `reports/retrieval_metrics.json` |

### Behavioural checks

| capability | pass rate | known failure |
|---|---:|---|
| Reranking experiment | PASS | Evidence based on only six answerable queries |
| Warmup exclusion | PASS | CPU timing remains runtime-dependent |

### Limitations and risks

1. Only six answerable test queries.
2. CPU latency depends on the Colab runtime.
3. Improvement is `MEASURED_SMOKE` only and has no reported confidence interval.

### Ethical and privacy notes

- Course fixture only.
- No production deployment claim.
- Decision recorded as `ADOPT_FOR_EXPERIMENT`, not production adoption.

### Reproduction

1. افتح notebook: `notebooks/06_semantic_search.ipynb`.
2. استخدم runtime/device: `Google Colab / CPU`.
3. ثبت نفس runtime dependencies الخاصة بالsemantic-search experiment.
4. شغّل Run all من commit: `5e5f3c3`.
5. قارن النتيجة مع: `reports/retrieval_metrics.json`.
