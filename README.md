# hipMemcpy Bandwidth Test (H2D, D2H, D2D) Benchmark

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![CI](https://img.shields.io/badge/CI-host--safe-green.svg)](.github/workflows/ci.yml)

Target: Ubuntu 26.04 · AMD · see Hardware Requirements. This is a host benchmark, not a laptop `pip install` project.

## Quick Start

```bash
git clone https://github.com/garys-gpu-benchmarks/314-gpu-bench-amd-hipmemcpy-transfer-bandwidth-ubu2604.git
cd 314-gpu-bench-amd-hipmemcpy-transfer-bandwidth-ubu2604
sudo bash setup.sh --assume-yes
bash run_benchmark.sh --profile smoke --validate
```
Results are written to `results/benchmark.db` and `results/summary.json`.

This workload is executed on the validation host after the repository is copied there. `setup.sh` and `run_benchmark.sh` do not open an outbound SSH session.

Prerequisites: Ubuntu 26.04; AMD; Python 3.14.4; root or sudo for `setup.sh`. Framework: Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC. This is a host benchmark, not a laptop `pip install` project.

```mermaid
flowchart LR
  setup.sh --> run_benchmark.sh --> parse_results.py --> results/benchmark.db
```

## 1. Overview

Builds and runs bin/hip_memcpy_bw over yaml transfer sizes and directions. direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, and num_iterations are forwarded when non-empty. output_format: csv Sweep dimensions: device_id, direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, num_iterations.

## 2. What It Validates

- Validates one CSV row per direction and transfer size from bin/hip_memcpy_bw
- #1: Transfer direction (direction); is present and physically sensible.
- #2: Copy latency, us (latency_us); is present and physically sensible.
- #3: Copy bandwidth, GB/s (bandwidth_GBps) is present and physically sensible.

## 3. Metrics Captured

- **#1: Transfer direction** — stored as `direction`.
- **#2: Copy latency, us** — stored as `latency_us`.
- **#3: Copy bandwidth, GB/s** — stored as `bandwidth_GBps`.

## 4. Hardware Requirements

### Supported environment

- OS: Ubuntu 26.04
- GPU vendor: AMD
- Framework family: Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC
- Python: Python 3.14.4

### Reference validation environment

The tables below describe the machine used to generate the reference results. They are not a requirement that every user buy that exact cloud instance.

### System

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC

### GPU

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC

## 5. Software Requirements

| Component | Version |
|---|---|
| OS | Ubuntu 26.04 |
| Kernel | kernel 7.0.0 |
| Python | Python 3.14.4 |
| ROCm | ROCm 7.14 |
| rocBLAS | N/A - rocBLAS not used |

Builds and runs bin/hip_memcpy_bw over yaml transfer sizes and directions. direction, pinned_memory, min_bytes, max_bytes, step_factor, warmup_iters, and num_iterations are forwarded when non-empty. output_format: csv

## 6. Installation

```bash
Build via scripts/build.sh if needed; run bin/hip_memcpy_bw
```

## 7. Running the Benchmark

```bash
Build via scripts/build.sh if needed; run bin/hip_memcpy_bw
```

**Validating results separately:**

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional
python3 -m venv .venv
source ".venv/bin/activate"
".venv/bin/python" scripts/validate_results.py
```

## 8. Output

### `results/benchmark.db` (SQLite)

hip_memcpy_bw stdout plus a CSV row per direction and transfer size

sample_index,status,direction,pinned_memory,transfer_size_bytes,latency_us,bandwidth_GBps,error_message
0,ok,H2D,1,1048576,12.0,18.5,

```bash
Build via scripts/build.sh if needed; run bin/hip_memcpy_bw
```

### `results/summary.json`

Consolidated metrics from the most recent run — suitable for CI artifact upload or dashboard ingestion.

### `results/raw/<timestamp>.txt`

hip_memcpy_bw stdout plus a CSV row per direction and transfer size

sample_index,status,direction,pinned_memory,transfer_size_bytes,latency_us,bandwidth_GBps,error_message
0,ok,H2D,1,1048576,12.0,18.5,

## 9. Baselines / Thresholds

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

## 10. Troubleshooting

**`setup.sh` missing collector**
Create cannot finish without `scripts/collect_workload.py`.

**`self_check` overlay rewritten**
Do not overwrite files listed in `results/overlay_lock.json`.

**Remote SSH drop during setup**
Reconnect and resume `bash setup.sh --assume-yes`. Do not wipe `.venv` or `.cache`.

## 11. NVIDIA H100 Coding Differences

Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Repository layout

```text
.
├── setup.sh
├── run_benchmark.sh
├── benchmark_specification.json
├── config/
├── scripts/
├── src/
├── tests/
├── docs/
├── results/
└── LICENSE
```
