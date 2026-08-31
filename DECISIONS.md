
# Decisions

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
