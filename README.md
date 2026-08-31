# Bayan project
## Day 1 — Attention & Transformers

### Shapes
| Item | Shape |
|---|---|
| Input | (2, 5, 12) |
| Split heads | (2, 3, 5, 4) |
| Combined | (2, 5, 12) |
| Encoder output | (2, 5, 12) |
| Attention | (2, 12, 10, 10) |

### Mask
القناع يحدد المواضع التي يسمح للنموذج بالانتباه إليها. في هذا المختبر كانت قيمة True تسمح بالانتباه وFalse تمنعه، لذلك يجب التأكد من دلالة القناع قبل استخدامه حتى لا ينعكس السلوك المتوقع.

### Decision
في مشروع Bayan سأستخدم Padding Mask لمنع النموذج من الانتباه إلى رموز الحشو [PAD] لأنها لا تحمل معنى حقيقيًا. هذا يساعد على أن يعتمد النموذج على الكلمات الفعلية فقط ويحافظ على تمثيل أدق للنص.

### Evidence
DAY1_NOTEBOOK2_CORE=PASS

## Day 2 — Text Classification

### Model
Multilingual DistilBERT

### Baseline
TF-IDF + Logistic Regression

### Data Split
Group-based split to prevent data leakage

### Fine-tuning
One real training step

### Results
- Validation Macro-F1: 1.0
- Test Macro-F1: 0.8667
- Test Accuracy: 0.875

### Evidence
DAY2_NOTEBOOK3_CORE=PASS

## Day 3 — Arabic NLP, Semantic Search & Evaluation

### Arabic NLP
Arabic preprocessing uses a documented search profile while preserving the original display copy.

### Semantic Search
- Embedding model: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
- L2-normalised corpus and query embeddings
- FAISS `IndexFlatIP`
- Test Recall@3: 1.0
- Test MRR@3: 0.6667
- Results labeled as `MEASURED_SMOKE`

### Evaluation
- Macro-F1 with bootstrap 95% CI
- Paired comparison between prediction sets
- Slice evaluation and error taxonomy
- Day 3 tests: 19 passed

### Evidence
- `DAY3_NOTEBOOK5_CORE=PASS`
- `DAY3_NOTEBOOK6_CORE=PASS`
- `DAY3_NOTEBOOK7_CORE=PASS`

## Day 4 — Optimization & Serving

- Gate D: PASS
- Notebook 08: `DAY4_NOTEBOOK8_CORE=PASS`
- Artefact role: `PROJECT_ARTIFACT`
- Result label: `MEASURED`
- Selected runtime: `onnx-fp32`
- PyTorch FP32 p95 latency: `237.746 ms`
- ONNX FP32 p95 latency: `184.258 ms`
- ONNX FP32 quality tax: `0.0`
- Decision: `ADOPT_ONNX_FP32`
- Day 4 tests: `38 passed`
- Final submission tests: `14 passed`

### Day 4 reports

- `BENCHMARKS.md`
- `reports/benchmark_results.json`
- `reports/service_smoke.json`
