# Progress

## Day 1 — Gate A

- Notebook 01 Text Processing & Tokenisation: PASS
- Notebook 02 Attention & Transformers: PASS
- Day 1 tests: 9 passed
- Tokenizer decision: complete
- Gate A status: complete

### Gate A Commit
- Commit: TODO

## Day 2 — Gate B

- Text Classification: DAY2_NOTEBOOK3_CORE=PASS
- NER & Extractive QA: DAY2_NOTEBOOK4_CORE=PASS
- Metrics saved in `reports/`
- Decisions documented in `DECISIONS.md`

- Gate B status: complete

### Exit Ticket

1. Why baseline before Transformer?
    عشان يكون عندنا مرجع بسيط نقارن فيه، ونعرف هل الـTransformer فعلاً حسّن الأداء أو لا.
2. What does group_id prevent?
    يمنع تسرب البيانات بين train وvalidation وtest، بحيث العينات المرتبطة بنفس المجموعة ما تتوزع على أكثر من split.
3. Why use -100 in NER?
    عشان الـloss يتجاهل special tokens والـsubwords اللي ما نبغى نحسبها أثناء التدريب.
4. Why must QA not always extract an answer?
    لأن بعض الأسئلة ما يكون لها جواب داخل الـcontext، وفي هالحالة المفروض يرجع None بدل ما يخترع إجابة.
5. Difference between MEASURED_SMOKE and production result?
    MEASURED_SMOKE نتيجة اختبار صغيرة تثبت أن الـpipeline يشتغل بشكل صحيح، لكنها ما تثبت جودة النموذج في بيئة إنتاج أو على بيانات حقيقية واسعة.
   
### Gate B Commit

Commit: https://github.com/samary12/bayan-project/commit/aead514f4fa69a9a0116f9c872d2c53c7f1d07f0

## Day 3 — Gate C

- Notebook 05 Arabic NLP: PASS
- Notebook 06 Semantic Search: PASS
- Notebook 07 Evaluation and Error Analysis: PASS
- Day 3 tests: 19 passed
- Evaluation report: complete
- Model card: complete
- Gate C status: verified complete
- 
### Gate C Commit
- Commit:https://github.com/samary12/bayan-project/commit/a4577ca2671192fac1623f5aa793c96be433216c
