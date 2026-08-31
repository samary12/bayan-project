# Decisions

## Day 2 — Text Classification

- Model: Multilingual DistilBERT
- Baseline: TF-IDF + Logistic Regression
- Data split: Group-based split to prevent data leakage
- Fine-tuning: One real training step
- Result: DAY2_NOTEBOOK3_CORE=PASS

## Day 2 — NER & Extractive QA

- Model: Multilingual DistilBERT
- Training mode: Partial fine-tuning on CPU
- NER alignment policy: First subword keeps the word label; continuation subwords and special tokens use -100.
- QA null policy: Return no answer when the null score is better than the best valid span.
- Result: DAY2_NOTEBOOK4_CORE=PASS
