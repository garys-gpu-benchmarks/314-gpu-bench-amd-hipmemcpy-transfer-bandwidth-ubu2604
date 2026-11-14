# Results: 314 hipMemcpy Bandwidth Test (H2D, D2H, D2D)

**Main goal:** Measure HIP H2D/D2H/D2D transfer bandwidth  
**Tools:** Build via scripts/build.sh if needed; run bin/hip_memcpy_bw

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_140 · recorded 2026-11-13). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-38-generic · driver 7.1.9.31600000
- **Run:** 2026-11-13T20:33:21Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 00:09 · benchmark 00:02 · whole run 00:17
- **Source:** run 314_20261113_203321
- **Command:** `bash run_benchmark.sh --smoke --device-id 0 --direction all --pinned-memory true --min-bytes 1024 --max-bytes 268435456 --step-factor 2 --warmup-iters 1 --num-iterations 10`

| # | Metric | Result |
|---|---|---|
| 1 | H2D bandwidth at max transfer size, GB/s (bandwidth_h2d_gb_s) | 55.249 |
| 2 | D2H bandwidth at max transfer size, GB/s (bandwidth_d2h_gb_s) | 56.491 |
| 3 | D2D bandwidth at max transfer size, GB/s (bandwidth_d2d_gb_s) | 1934.156 |
| 4 | H2D latency at min transfer size, us (latency_h2d_us) | 12.974 |
| 5 | D2H latency at min transfer size, us (latency_d2h_us) | 11.52 |
| 6 | D2D latency at min transfer size, us (latency_d2d_us) | 4.034 |
<!-- /profile:smoke -->
