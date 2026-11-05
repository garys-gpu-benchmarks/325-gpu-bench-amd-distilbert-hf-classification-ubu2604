# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Trains DistilBertForSequenceClassification in-process on synthetic token batches (fp32 AdamW). sequence_len, batch_size, num_iterations, model_name, learning_rate, and weight_decay come from yaml. warmup_iters: untimed forward/backward passes before the timer starts. gradient_accumulation: backward passes per optimizer step. Sweep dimensions: model_name, sequence_len, batch_size, learning_rate, weight_decay, num_iterations, gradient_accumulation, warmup_iters.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| model_name | `--model-name` | smoke=distilbert-base-uncased, baseline=distilbert-base-uncased, extended=distilbert-base-uncased | distilbert-base-uncased | From Parameter list; see Execution Description With Parameters. |
| sequence_len | `--sequence-len` | smoke=32, baseline=128, extended=128 | 128 | From Parameter list; see Execution Description With Parameters. |
| batch_size | `--batch-size` | smoke=2, baseline=16, extended=16 | 16 | From Parameter list; see Execution Description With Parameters. |
| learning_rate | `--learning-rate` | smoke=5.0000000000000002E-5, baseline=5.0000000000000002E-5, extended=5.0000000000000002E-5 | 5.0000000000000002E-5 | From Parameter list; see Execution Description With Parameters. |
| weight_decay | `--weight-decay` | smoke=0.01, baseline=0.01, extended=0.01 | 0.01 | From Parameter list; see Execution Description With Parameters. |
| num_iterations | `--num-iterations` | smoke=2, baseline=17300, extended=50900 | 17300 | From Parameter list; see Execution Description With Parameters. |
| gradient_accumulation | `--gradient-accumulation` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| warmup_iters | `--warmup-iters` | smoke=1, baseline=2, extended=5 | 2 | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Run inline DistilBertForSequenceClassification training on synthetic tokens. Does not run pytest test_bench.py
```

## Raw Output Format

CSV with two identical training-sample rows

sample_index,step,loss,samples_per_sec,step_time_ms,tokens_per_sec,peak_gpu_memory_gb
0,0,1.0,40,25,1280,1.5

## Metrics

- **#1: Training throughput** — stored as `samples_per_sec`.
- **#2: Training step time, ms** — stored as `step_time_ms`.
- **#3: Token throughput** — stored as `tokens_per_sec`.
- **#4: Peak GPU memory** — stored as `peak_gpu_memory_gb`.

## Framework

Trains DistilBertForSequenceClassification in-process on synthetic token batches (fp32 AdamW). sequence_len, batch_size, num_iterations, model_name, learning_rate, and weight_decay come from yaml. warmup_iters: untimed forward/backward passes before the timer starts.

## Installation and Execution Summary

Run inline Hugging Face DistilBERT sequence-classification training on synthetic token batches with yaml sequence_len, batch_size, and num_iterations, then write samples/s, step time, and tokens/s, to measure DistilBERT training throughput. This is not pytest test_bench.py

## Platform Portability

- **AMD (primary):** ```bash
Run inline DistilBertForSequenceClassification training on synthetic tokens. Does not run pytest test_bench.py
```
- **NVIDIA:** Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

CSV with two identical training-sample rows

sample_index,step,loss,samples_per_sec,step_time_ms,tokens_per_sec,peak_gpu_memory_gb
0,0,1.0,40,25,1280,1.5

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --smoke
bash run_benchmark.sh --baseline
bash run_benchmark.sh --extended
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Trains DistilBertForSequenceClassification in-process on synthetic token batches (fp32 AdamW). sequence_len, batch_size, num_iterations, model_name, learning_rate, and weight_decay come from yaml. warmup_iters: untimed forward/backward passes before the timer starts.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Trains DistilBertForSequenceClassification in-process on synthetic token batches (fp32 AdamW). sequence_len, batch_size, num_iterations, model_name, learning_rate, and weight_decay come from yaml. warmup_iters: untimed forward/backward passes before the timer starts.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
