# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Builds and runs bin/hip_memcpy_bw over yaml transfer sizes and directions. direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, and num_iterations are forwarded when non-empty. output_format: csv Sweep dimensions: device_id, direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, num_iterations.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| device_id | `--device-id` | smoke=0, baseline=0, extended=0 | 0 | From Parameter list; see Execution Description With Parameters. |
| direction | `--direction` | smoke=all, baseline=all, extended=all | all | From Parameter list; see Execution Description With Parameters. |
| pinned_memory | `--pinned-memory` | smoke=true, baseline=true, extended=true | true | From Parameter list; see Execution Description With Parameters. |
| min_bytes | `--min-bytes` | smoke=1024, baseline=1024, extended=1024 | 1024 | From Parameter list; see Execution Description With Parameters. |
| max_bytes | `--max-bytes` | smoke=4096, baseline=1073741824, extended=1073741824 | 1073741824 | From Parameter list; see Execution Description With Parameters. |
| step_factor | `--step-factor` | smoke=2, baseline=2, extended=2 | 2 | From Parameter list; see Execution Description With Parameters. |
| warmup_iters | `--warmup-iters` | smoke=1, baseline=5, extended=10 | 5 | From Parameter list; see Execution Description With Parameters. |
| num_iterations | `--num-iterations` | smoke=3, baseline=2100, extended=6080 | 2100 | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Build via scripts/build.sh if needed; run bin/hip_memcpy_bw
```

## Raw Output Format

hip_memcpy_bw stdout plus a CSV row per direction and transfer size

sample_index,status,direction,pinned_memory,transfer_size_bytes,latency_us,bandwidth_GBps,error_message
0,ok,H2D,1,1048576,12.0,18.5,

## Metrics

- **#1: Transfer direction** — stored as `direction`.
- **#2: Copy latency, us** — stored as `latency_us`.
- **#3: Copy bandwidth, GB/s** — stored as `bandwidth_GBps`.

## Framework

Builds and runs bin/hip_memcpy_bw over yaml transfer sizes and directions. direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, and num_iterations are forwarded when non-empty. output_format: csv

## Installation and Execution Summary

Compile and run bin/hip_memcpy_bw with yaml direction, pinned_memory, min_bytes, max_bytes, step_factor, and iteration counts, then parse one row per direction and transfer size, to measure HIP copy bandwidth and latency

## Platform Portability

- **AMD (primary):** ```bash
Build via scripts/build.sh if needed; run bin/hip_memcpy_bw
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

hip_memcpy_bw stdout plus a CSV row per direction and transfer size

sample_index,status,direction,pinned_memory,transfer_size_bytes,latency_us,bandwidth_GBps,error_message
0,ok,H2D,1,1048576,12.0,18.5,

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Builds and runs bin/hip_memcpy_bw over yaml transfer sizes and directions. direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, and num_iterations are forwarded when non-empty. output_format: csv
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Builds and runs bin/hip_memcpy_bw over yaml transfer sizes and directions. direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, and num_iterations are forwarded when non-empty. output_format: csv

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
