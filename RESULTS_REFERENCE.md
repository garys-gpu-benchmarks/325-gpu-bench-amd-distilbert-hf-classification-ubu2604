# Results: 325 DistilBERT NLP Training Baseline

**Main goal:** Measure DistilBERT classification training throughput  
**Tools:** Run inline DistilBertForSequenceClassification training on synthetic tokens

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_146 · recorded 2026-11-18). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-38-generic · driver 7.1.9.31600000
- **Run:** 2026-11-18T11:27:16Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 01:21 · benchmark 00:01 · whole run 01:30
- **Source:** run 325_20261118_112716
- **Command:** `bash run_benchmark.sh --smoke --model-name distilbert-base-uncased --sequence-len 32 --batch-size 2 --learning-rate 5.0000000000000002E-5 --weight-decay 0.01 --num-iterations 2 --gradient-accumulation 1 --warmup-iters 1`

| # | Metric | Result |
|---|---|---|
| 1 | Training throughput (samples_per_sec) | 46.299968141076185 |
| 2 | Training step time, ms (step_time_ms) | 43.196573999921384 |
| 3 | Token throughput (tokens_per_sec) | 1481.598980514438 |
| 4 | Peak GPU memory (peak_gpu_memory_gb) | 1.512544256 |
<!-- /profile:smoke -->
