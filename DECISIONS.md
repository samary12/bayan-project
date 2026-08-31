
# Decisions
## Day 1 — Tokenizer decision

Checkpoint/tokenizer: `google-bert/bert-base-multilingual-cased` for comparison; local WordPiece tokenizer used for measured fertility/truncation metrics.

Corpus slice: Small mixed Arabic-English synthetic sample.

Arabic fertility [MEASURED]: 1.39
English fertility [MEASURED]: 1.32
Truncation rate at max_length=8 [MEASURED]: 40%
Truncation rate at max_length=12 [MEASURED]: 0%

Known limitation: Measurements are based on a very small synthetic sample, and the fertility/truncation values were measured using the local WordPiece tokenizer rather than the mBERT tokenizer.

Decision and reason: Keep the local WordPiece tokenizer for the Core reproducible workflow, while using `google-bert/bert-base-multilingual-cased` as the external multilingual comparison checkpoint. The local tokenizer works offline and produces documented measurable behaviour, while mBERT provides a realistic multilingual reference when network access is available.

## Day 2 — Gate B

### Checkpoint
- Checkpoint: distilbert/distilbert-base-multilingual-cased
- Reason: Supports multilingual text and is suitable for the classification task.

### Execution
- Execution type: frozen
- Training mode: CPU partial fine-tuning; the base model is frozen except for the final Transformer layer.

### Data Split
- Split strategy: Group-based split using `group_id`.
- Leakage check: Zero group overlap between train, validation, and test sets (`group_overlap = 0`).

### Classification
- Baseline: TF-IDF + Logistic Regression
- Baseline metric: Validation macro-F1 = 0.6667 (MEASURED_SMOKE)
- Transformer metric: Validation macro-F1 = 1.0 (MEASURED_SMOKE)
- Results are labeled as `MEASURED_SMOKE`.

### NER
- NER alignment policy: The first subword keeps the word label; continuation subwords and special tokens use -100.
- Result: DAY2_NOTEBOOK4_CORE=PASS

### Extractive QA
- QA null policy: Return None when the null/no-answer score is better than the best valid answer span.
- Result: DAY2_NOTEBOOK4_CORE=PASS

### Limitation
- The small synthetic dataset cannot prove production-level model quality or generalization to real-world data.

- ## Day 3 — Semantic Search

### Model
- Embedding model: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
- Model card: Hugging Face model card
- License: Apache-2.0

### Retrieval
- Index type: FAISS `IndexFlatIP`
- Embeddings are L2-normalised for both corpus and query.
- Results are labeled as `MEASURED_SMOKE`.

### No-answer threshold
- Threshold: `0.4592095613479614`
- The threshold was tuned on the validation split only.
- The selected threshold was then frozen and applied to the test split.

## Day 3 — Evaluation and Error Analysis

### Evaluation
- Evaluation data: `COURSE_FIXTURE`
- Split: validation only
- The fixture is used for learning the evaluation workflow and is not a production benchmark.
- Macro-F1 was evaluated with a 95% bootstrap confidence interval.
- A paired bootstrap comparison was used to compare predictions A and B.
- The paired confidence interval includes zero, so the observed difference does not support a directional claim.

### Error Analysis and Fixes

1. **Gulf coverage for health and transport**
   - Evidence: `dialect_gap` tags in the validation fixture.
   - Change: Collect/review targeted Gulf examples.
   - Acceptance test: New behavioural cases plus sliced CI.

2. **Class confusion around app/status wording**
   - Evidence: `EV-019` and `EV-020`.
   - Change: Add contrastive examples and review the label guide.
   - Acceptance test: Paired comparison without regression in other slices.

3. **Underspecified short requests**
   - Evidence: `hard_or_ambiguous` tags.
   - Change: Request context or abstain when confidence is low.
   - Acceptance test: Ambiguity behavioural suite.

### Interpretation
- The confidence interval must be reported together with the metric because the validation sample is small.
- The paired comparison does not show reliable evidence that one prediction set is better than the other because the interval includes zero.
- Slice and error-taxonomy analysis are used to identify specific failure modes instead of relying only on the overall average.

## Day 4 — Gate D

### Serving optimisation decision

- Artefact role: `PROJECT_ARTIFACT`
- Result label: `MEASURED`
- Selected runtime: `onnx-fp32`
- Decision: `ADOPT_ONNX_FP32`

### Evidence

- PyTorch FP32 p95 latency: `237.746 ms`
- ONNX FP32 p95 latency: `184.258 ms`
- ONNX FP32 throughput: `46.419 items/s`
- ONNX FP32 prediction agreement: `1.0`
- ONNX FP32 quality tax: `0.0`
- ONNX FP32 budget: PASS

### INT8 decision

Dynamic INT8 was faster, but it was rejected because prediction agreement dropped to `0.5` and quality tax increased to `0.565476`, exceeding the project quality budget.

### Rollback

If ONNX FP32 causes a deployment issue, revert to the recorded PyTorch FP32 project model and re-export from the saved model source.
