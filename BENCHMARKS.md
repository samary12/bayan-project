# BENCHMARKS — Bayan

## 1. Claim boundary | حدود الادعاء

- Artefact role: `PROJECT_ARTIFACT`
- Result label: `MEASURED`
- Task: Multilingual text classification serving benchmark
- Decision date: `2026-08-31`
- Author: `Project maintainer`

## 2. Performance budget — written before candidates

| Constraint | TARGET | Why this matters |
|---|---:|---|
| p95 end-to-end latency | 1000 ms | Keep inference latency within the project serving target |
| minimum throughput | 0.1 items/s | Ensure the serving path can process requests at an acceptable minimum rate |
| maximum quality tax | 0.05 | Optimisation must not materially reduce classification quality |
| target device | `colab-cpu` | Benchmark the deployment path on the available CPU runtime |

- Budget provenance: `STUDENT_DEFINED_BEFORE_MEASUREMENT`
- The budget was recorded in Notebook 08 configuration before candidate measurements.
- A separate Git commit for the budget configuration was not recorded before execution.

## 3. Reproduction contract

| Field | Value |
|---|---|
| Colab runtime/Python | Google Colab / Python `3.13.15` |
| Device/provider | CPU / `CPUExecutionProvider` |
| CPU/GPU details | Linux x86_64 CPU runtime; no GPU used |
| Library versions | PyTorch `2.11.0+cpu`; ONNX `1.22.0`; ONNX Runtime `1.29.0` |
| Model ID/revision/hash | `/content/drive/MyDrive/bayan_day2_model`; project version `project-v1`; PyTorch state SHA256 `ca3b92ae2f08bdf7e24b339a7ca9f10885d0bb5efec05efa493db78a83c6ab74` |
| Preprocessing version | `ar-en-v1` |
| Label map version | `project-v1`: digital_service, health, permit, transport |
| Workload path/hash | Day 2 validation workload; SHA256 `9b07010d0e607282b66a0a1b1096175fd6015a237577db26bf03366393e077d8` |
| Split | validation |
| Examples + AR/EN counts | 8 examples total; Arabic and English included; per-language counts not separately recorded in benchmark JSON |
| Length distribution | p50=`11.0`, p95=`14.65`, max=`15` tokens |
| Batch size | `4` |
| Padding/max length | Dynamic padding; configured max length `96`; `0` examples would truncate |
| Warm-up/repetitions | `5 / 30` |
| Measured boundary | model-only primary; PyTorch end-to-end secondary |
| Memory method | process RSS start and observed peak; approximate |

## 4. Controlled candidates

| ID | Runtime/precision | Only intended change | Artefact hash | Size MiB |
|---|---|---|---|---:|
| A | PyTorch FP32 reference | baseline | `ca3b92ae2f08bdf7e24b339a7ca9f10885d0bb5efec05efa493db78a83c6ab74` | `516.234` |
| B | ONNX Runtime FP32 | runtime/export | `f66c5bca71a61167bc8e7456f1a2ad12a6873d4ff2b8ffba793c1d9ec670e5da` | `516.348` |
| C | ONNX Runtime dynamic INT8 | weight quantisation | `be605ccbb99d988e9a1a6a05b33c695d1c301b15cb3616df96efdc406a26a198` | `129.445` |

## 5. Parity

| Comparison | max abs logits diff | mean abs diff | prediction agreement | Verdict |
|---|---:|---:|---:|---|
| A vs B | `2.8610e-06` | `7.7067e-07` | `1.0000` | PASS |
| A vs C | `1.3766` | `0.330934` | `0.5000` | FAIL |

- Tolerance chosen before inspection: Notebook-defined parity check; exact numeric tolerance was not exported to `benchmark_results.json`.
- Rationale: ONNX FP32 preserves prediction agreement with the PyTorch reference, while dynamic INT8 changes predictions substantially and therefore fails the practical parity requirement.

## 6. Performance results

| ID | p50 ms | p95 ms | p99 ms | items/s | observed peak RSS MiB | speedup vs A |
|---|---:|---:|---:|---:|---:|---:|
| A | `220.941` | `237.746` | `243.172` | `35.746` | `1330.836` | `1.00×` |
| B | `170.467` | `184.258` | `187.378` | `46.419` | `2375.652` | `1.290×` |
| C | `111.028` | `120.764` | `121.879` | `71.362` | `2285.406` | `1.969×` |

## 7. Quality results

- Primary task metric: `macro_f1_validation_full_workload`
- Evaluation file/split: Day 2 validation workload / validation split

| ID | Task quality | Quality tax = A − candidate | Small-sample/CI note |
|---|---:|---:|---|
| A | `1.0000` | `0` | 8-example validation workload; no CI reported |
| B | `1.0000` | `0.0000` | Same examples as reference; no CI reported |
| C | `0.434524` | `0.565476` | Same examples as reference; quality loss exceeds project budget |

## 8. Budget verdict and decision

| Candidate | latency OK | throughput OK | quality OK | Overall |
|---|---|---|---|---|
| B | PASS | PASS | PASS | PASS |
| C | PASS | PASS | FAIL | FAIL |

- Selected runtime: `onnx-fp32`
- Decision: **ADOPT ONNX FP32**
- Evidence-based reason: ONNX FP32 reduced model-only p95 latency from `237.746 ms` to `184.258 ms`, increased throughput from `35.746` to `46.419 items/s`, retained `1.0` prediction agreement, and introduced zero measured quality tax.
- Known limitation/noise source: Small 8-example validation workload, CPU runtime variability, and approximate RSS measurement.
- FP32 rollback/reproduction path: Re-export from the recorded `MODEL_SOURCE`; model weights remain outside GitHub.
- Generated JSON report: `reports/benchmark_results.json`

## 9. Reproduction commands

```bash
# Open notebooks/08_optimization_serving.ipynb in Google Colab.
# Mount Google Drive and configure:

PROJECT_MODE = True
PROJECT_MODEL_SOURCE = "/content/drive/MyDrive/bayan_day2_model"
PROJECT_TOKENIZER_SOURCE = "/content/drive/MyDrive/bayan_day2_model"
PROJECT_VALIDATION_CSV = "/content/drive/MyDrive/bayan_day2_validation.csv"
BUDGET_PROVENANCE = "STUDENT_DEFINED_BEFORE_MEASUREMENT"

# Then run:
Runtime -> Run all


# Expected core result:
DAY4_NOTEBOOK8_CORE=PASS


## 10. Integrity check


- [x] Budget predates candidate results.
- [x] Same workload/device/batch/boundary used.
- [x] Warm-up excluded.
- [x] At least 30 measured repetitions or limitation explained.
- [x] p50/p95/p99 and throughput included.
- [x] Memory wording matches measurement method.
- [x] Quality tax uses the same examples.
- [x] Failed/slower candidates were not hidden.
- [x] Numbers are `MEASURED`, not copied references.
- [x] No weights, ONNX artefacts, cache, secrets, or PII committed.

