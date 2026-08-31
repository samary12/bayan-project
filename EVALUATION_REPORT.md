# تقرير تقييم بيان | Bayan Evaluation Report

## 1. نطاق التقرير

- تاريخ التشغيل: `2026-08-31`
- commit SHA: `5e5f3c3`
- runtime/device: `Google Colab / CPU`
- data version/hash: `bayan_day3_cases.csv` / `7708cbe884a3c268d24ed2cb87ad2f0a8b64b2e6fa6b37a32393b6ae3bd50e5b`
- preprocessing profile/version/backend: `arabic-search/1.0.0 + english-nfc-whitespace/1.0.0` / `camel-tools 1.6.0`
- model/checkpoint IDs:
  - Classification: `distilbert/distilbert-base-multilingual-cased`
  - NER: `distilbert/distilbert-base-multilingual-cased`
  - QA: `distilbert/distilbert-base-multilingual-cased`
  - Retrieval: `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
  - Reranker: `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`
- نوع الأرقام: `MEASURED_SMOKE` لنتائج Day 2 وSemantic Search، و`COURSE_FIXTURE` لنتائج Evaluation/Error Analysis.

## 2. العقود قبل القياس

| العقد | الدليل | الحالة |
|---|---|---|
| لا PII حقيقية | Course fixture / synthetic data only | PASS |
| train/validation/test بلا leakage | Group-based split using `group_id`; `group_overlap = 0` | PASS |
| tokenizer/model متطابقان | Same model/tokenizer configuration used for the corresponding pipelines | PASS |
| Arabic profile متطابقة في train/index/query/serve | `arabic-search/1.0.0` recorded in search manifest | PASS |
| corpus/query embeddings مطبعة L2 | Manifest reports `normalization: l2` | PASS |
| frozen test لم يستخدم في tuning | No-answer threshold tuned on validation only, then frozen for test | PASS |

## 3. نتائج المهام

| المهمة | المقياس الرئيس | النتيجة | CI/تكرار | مجموعة القياس |
|---|---|---:|---|---|
| Classification | Macro-F1 | Validation: `1.0000`; Test: `0.8667` | No CI reported; 12 epochs, selected epoch 9 | `MEASURED_SMOKE` |
| NER | strict entity F1 | `0.5714` | No CI reported; 12 epochs / 48 steps | `MEASURED_SMOKE` |
| QA | valid span + no-answer | Valid span PASS; no-answer returns `None` | No aggregate EM/F1 reported | `MEASURED_SMOKE` |
| Retrieval | Recall@3 / MRR@3 | `1.0000 / 0.6667` | 6 answerable test queries | `MEASURED_SMOKE` |

## 4. شرائح التقييم

| المهمة | الشريحة | n | metric | 95% CI | التحذير/التفسير |
|---|---|---:|---:|---|---|
| Evaluation | `ALL` | 36 | Macro-F1 = `0.781944` | `[0.611175, 0.896175]` | COURSE_FIXTURE |
| Evaluation | `language=ar` | 24 | Macro-F1 = `0.758289` | `[0.560652, 0.926545]` | — |
| Evaluation | `language=en` | 12 | Macro-F1 = `0.828571` | `[0.500000, 1.000000]` | `SMALL_SLICE` |
| Evaluation | `variant=Gulf` | 12 | Macro-F1 = `0.658333` | `[0.294059, 0.895177]` | `SMALL_SLICE` |
| Evaluation | `variant=MSA` | 12 | Macro-F1 = `0.837500` | `[0.530417, 1.000000]` | `SMALL_SLICE` |
| Evaluation | `length_bucket=long` | 18 | Macro-F1 = `0.525926` | `[0.391803, 0.830801]` | Lower-performing length slice |
| Evaluation | `length_bucket=short` | 18 | Macro-F1 = `0.828571` | `[0.625000, 1.000000]` | Higher than long slice |
| Retrieval | `language=ar` | 3 | Recall@3 = `1.0000`; MRR@3 = `0.5000` | Not reported | `SMALL_SLICE` |
| Retrieval | `language=en` | 3 | Recall@3 = `1.0000`; MRR@3 = `0.8333` | Not reported | `SMALL_SLICE` |
| Retrieval | `retrieval_mode=cross_lingual` | 2 | Recall@3 = `1.0000`; MRR@3 = `0.5000` | Not reported | `SMALL_SLICE` |
| Retrieval | `retrieval_mode=monolingual` | 4 | Recall@3 = `1.0000`; MRR@3 = `0.7500` | Not reported | `SMALL_SLICE` |

## 5. مقارنة الإصدارات

- Model A: `COURSE_FIXTURE prediction_a`
- Model B: `COURSE_FIXTURE prediction_b`
- observed difference B−A: `0.0011975404`
- paired 95% CI: `[-0.1047231617, 0.0996300521]`
- القرار المهني: فترة الثقة تشمل الصفر، لذلك لا يوجد دليل كافٍ لادعاء أن Model B أفضل من Model A. كما أن الفرق المرصود صغير جدًا.

## 6. Behavioural tests

| النوع | passed/total | pass rate | فشل مهم |
|---|---:|---:|---|
| Overall behavioural fixture | `3/6` | `0.50` | Gulf health result, English lab result, and MSA bus route cases failed |
| invariance | `Not separately measured` | `Not separately measured` | — |
| directional | `Not separately measured` | `Not separately measured` | — |
| minimum functionality | `Not separately measured` | `Not separately measured` | — |

## 7. تحليل الأخطاء

- المصدر: validation + behavioural failures فقط.
- عدد الأخطاء المقروءة يدويًا: `8`
- رابط worksheet داخل المستودع: `reports/day3_error_taxonomy.csv`

| taxonomy tag | count | مثال آمن مختصر | الفرضية |
|---|---:|---|---|
| `dialect_gap` | 3 | Gulf phrasing for health/transport requests | اللهجة الخليجية قد تخفي الإشارة الواضحة لنوع الخدمة |
| `hard_or_ambiguous` | 3 | Short or underspecified requests | نقص السياق يزيد الغموض |
| `class_confusion` | 2 | App/status wording overlapping with another class | كلمات التطبيق والحالة قد تطغى على النية الأساسية |

## 8. الإصلاحات الثلاثة ذات الأولوية

| الأولوية | الدليل | الإجراء | metric/slice المتوقع | الكلفة | اختبار عدم الرجوع |
|---:|---|---|---|---|---|
| 1 | `dialect_gap` tags in validation fixture | Collect/review targeted Gulf examples for health and transport | Gulf slice + behavioural cases | Data collection/review | New behavioural cases plus sliced CI |
| 2 | `EV-019` and `EV-020` | Add contrastive examples and review label guide | Affected slices + paired comparison | Data/label-guide review | Paired comparison without regression in other slices |
| 3 | `hard_or_ambiguous` tags | Request context or abstain when confidence is low | Ambiguity behavioural suite | UX/policy change | Ambiguity behavioural suite |

## 9. ما الذي لا تثبته النتائج؟

- النتائج مبنية على course fixtures وsmall smoke-test datasets وليست بيانات إنتاج حقيقية.
- بعض الشرائح صغيرة ومعلّمة `SMALL_SLICE`.
- نتائج Classification وNER وQA ناتجة عن تدريب CPU قصير، ولا تمثل جودة production.
- المقارنة الزوجية لا تثبت تفوق أحد الإصدارين لأن فترة الثقة تشمل الصفر.
- Retrieval مبني على 6 answerable test queries فقط.
- قياسات reranking latency تعتمد على بيئة CPU الحالية.
- زيادة عدد bootstrap iterations لا تعوض نقص حجم أو تمثيل البيانات.

## 10. خلاصة للإدارة

The core NLP and semantic-search pipelines run successfully on the course fixtures. Classification outperforms its baseline in the smoke evaluation, while semantic search achieves Recall@3 of 1.0 and MRR@3 of 0.6667. The reranker improves MRR@3 from 0.6667 to 0.7222 on six answerable test queries, but these results remain `MEASURED_SMOKE`.

The main observed weaknesses are Gulf-language coverage, class confusion around app/status wording, and underspecified short requests. The current evidence does not justify a production-quality claim or a directional superiority claim between the two evaluation prediction sets. The next step should focus on targeted data collection and behavioural evaluation around these failure modes.
