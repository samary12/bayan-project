# Progress

## Day 1 — Gate A

- Notebook 01 Text Processing & Tokenisation: PASS
- Notebook 02 Attention & Transformers: PASS
- Day 1 tests: 9 passed
- Tokenizer decision: complete
- Gate A status: complete

### Gate A Commit
- Commit: https://github.com/samary12/bayan-project/commit/b2d1053fb86c81d464c35656b02696b130bec85d

### Exit Ticket

1. Why is `text.split()` not enough for BERT?  
لأن BERT يستخدم tokenizer مرتبط بالـcheckpoint وقد يقسم الكلمات إلى subwords، بينما `text.split()` يفصل النص فقط حسب المسافات.

2. Why should the tokenizer and model come from the same checkpoint?  
لأن الـtoken IDs والـvocabulary لازم تتطابق مع الـembeddings التي تدرب عليها الموديل.

3. Give an example of an Arabic transformation that may remove information.  
إزالة التشكيل؛ لأنها قد تزيل فرقًا لغويًا أو تغيّر المعنى في بعض الحالات.

4. What does Fertility measure, and what does it not prove?  
تقيس متوسط عدد الـtokens الناتجة لكل كلمة تقريبًا، لكنها لا تثبت أن tokenizer أفضل للمهمة؛ هي فقط تقيس مقدار التجزئة.

5. What is the difference between a Token ID and an Embedding?  
Token ID هو رقم يمثل token داخل الـvocabulary، أما Embedding فهو متجه عددي يمثل هذا الـtoken داخل النموذج.

6. What is the shape of the attention matrix for a sequence of length `n` in one head?  
تكون `n × n`، لأن كل token يمكن أن يوزع انتباهه على كل الـtokens في التسلسل.

7. Why are attention scores divided by `sqrt(d_k)`?  
حتى لا تصبح القيم كبيرة جدًا قبل softmax، وهذا يساعد على استقرار حساب الـattention.

8. Why should attention weights not automatically be presented as a causal explanation?  
لأنها توضح أين ركزت آلية attention، لكنها لا تثبت أن هذا الجزء هو السبب الحقيقي في قرار النموذج.

## Day 2 — Gate B

- Text Classification: DAY2_NOTEBOOK3_CORE=PASS
- NER & Extractive QA: DAY2_NOTEBOOK4_CORE=PASS
- Metrics saved in `reports/`
- Decisions documented in `DECISIONS.md`

- Gate B status: complete

### Exit Ticket

1. Why baseline before Transformer?
   
    عشان يكون عندنا مرجع بسيط نقارن فيه، ونعرف هل الـTransformer فعلاً حسّن الأداء أو لا.
3. What does group_id prevent?
   
    يمنع تسرب البيانات بين train وvalidation وtest، بحيث العينات المرتبطة بنفس المجموعة ما تتوزع على أكثر من split.
5. Why use -100 in NER?
   
    عشان الـloss يتجاهل special tokens والـsubwords اللي ما نبغى نحسبها أثناء التدريب.
7. Why must QA not always extract an answer?
   
    لأن بعض الأسئلة ما يكون لها جواب داخل الـcontext، وفي هالحالة المفروض يرجع None بدل ما يخترع إجابة.
9. Difference between MEASURED_SMOKE and production result?
    
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

### Exit Ticket

1. لماذا نحفظ display copy مستقلة؟
لأن display copy تحفظ النص الأصلي للعرض، بينما نسخة المعالجة قد تتغير بسبب normalization أو preprocessing.

2. لماذا يجب تطبيع corpus وquery معًا؟
لأن لازم يكونون بنفس طريقة المعالجة والتمثيل، وإلا المقارنة بين embeddings تصير غير عادلة أو غير دقيقة.

3. ما الفرق بين Recall@k وMRR@k؟
Recall@k يقيس هل النتيجة الصحيحة ظهرت ضمن أول k نتائج، بينما MRR@k يهتم أيضًا بترتيب أول نتيجة صحيحة؛ كلما ظهرت أبكر كان أفضل.

4. ماذا يعني أن CI للفرق تشمل الصفر؟
يعني ما عندنا دليل كافٍ أن أحد النموذجين أفضل فعلاً من الآخر؛ لأن الفرق الحقيقي ممكن يكون صفر.

5. لماذا نجري error analysis على validation؟
لأننا نستخدم validation لفهم الأخطاء واختيار التحسينات بدون لمس test، حتى يبقى test تقييمًا نهائيًا غير متحيز.

6. ما الذي يجبرك على إعادة بناء FAISS index؟
إذا تغيرت embeddings أو preprocessing أو embedding model أو corpus، لازم نعيد بناء الـFAISS index حتى يظل متوافقًا مع البيانات والتمثيلات الجديدة.
  
### Gate C Commit
- Commit:https://github.com/samary12/bayan-project/commit/a4577ca2671192fac1623f5aa793c96be433216c

## Day 4 — Gate D

- `notebooks/08_optimization_serving.ipynb`: `DAY4_NOTEBOOK8_CORE=PASS`
- Artefact role: `PROJECT_ARTIFACT`
- Result label: `MEASURED`
- Selected runtime: `onnx-fp32`
- Adoption decision: `ADOPT_ONNX_FP32`
- Benchmark report: `BENCHMARKS.md`
- Benchmark JSON: `reports/benchmark_results.json`
- Service smoke report: `reports/service_smoke.json`
- Day 4 tests: `38 passed in 0.16s`

### Serving smoke

- `/health`: PASS
- Arabic request: PASS
- English request: PASS
- Empty request rejected: PASS
- Unsupported language rejected: PASS
- Arabic canary: PASS
- English canary: PASS

### Gate D status

`PASS`

### Exit Ticket

1. لماذا لا يكفي المتوسط لقياس latency؟
لأن المتوسط ممكن يخفي الحالات البطيئة، بينما p95 وp99 توضح لنا أسوأ التأخيرات التي قد يلاحظها المستخدم.

2. كيف يمكن أن تزيد throughput بينما تسوء latency الفردية؟
لأن النظام قد يعالج عددًا أكبر من الطلبات معًا باستخدام batching، فيزيد throughput، لكن الطلب الواحد قد ينتظر وقتًا أطول قبل المعالجة.

3. ما الفرق بين ONNX وONNX Runtime؟
ONNX هو صيغة لتمثيل النموذج، بينما ONNX Runtime هو محرك تشغيل ينفذ نموذج ONNX على الجهاز.

4. لماذا لا يعني ملف INT8 أصغر أنه أفضل؟
لأن الحجم الأصغر والسرعة الأعلى لا يكفيان إذا انخفضت جودة التنبؤ أو تغيرت النتائج بشكل كبير.

5. ما الذي تكشفه startup canary؟
تتأكد أن الخدمة بدأت بشكل صحيح وأن النموذج والإعدادات الأساسية تعمل قبل استقبال الطلبات الحقيقية.

6. لماذا لا يكفي SYSTEMS_SMOKE للتسليم؟
لأنه يختبر أن النظام والمسار يعملان فقط، لكنه لا يثبت أداء وجودة artefact المشروع الحقيقي. لذلك نحتاج تشغيل PROJECT_ARTIFACT وقياسات فعلية.

7. ما الدليل الذي يجعلك تثق بأهم رقم في مشروعك؟
أن الرقم ناتج من تشغيل فعلي قابل لإعادة الإنتاج، مع workload معروف، إعدادات موثقة، عدد تكرارات كافٍ، ونتائج محفوظة في تقارير مثل `benchmark_results.json`.

### Gate D checkpoint

https://github.com/samary12/bayan-project/commit/3ec2cb2e0abfe8a598091850af42895cd9aa36ee

## — Gate E (Final Submission)

- Final validation: `validate_submission.py --require-tag` -> PASS
- Contracts verified: `PROJECT_SUMMARY.json`, `SUBMISSION.yml`
- Tag & Release created: `submission-v1.0`
- Clean repo audit: No weights, cache, or raw PII committed.
- Gate E status: complete

### Gate E Release & Commit
- Release: https://github.com/samary12/bayan-project/releases/tag/submission-v1.0
- Commit: https://github.com/samary12/bayan-project/commit/c9a843d
