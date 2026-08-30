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
