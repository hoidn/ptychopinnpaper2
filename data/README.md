# Paper Data Provenance

This file documents the provenance of figures/tables that are sourced from
precomputed artifacts in the main repo. Use this as the authoritative map when
regenerating or replacing assets in `paper/`.

## Out-of-Distribution Figure (fig:fivepanel / outdist.tex)

The panel images in `paper/figures/out-dist-fig/*.png` are byte-identical copies
of artifacts under `experiment_outputs/`.

### Out-of-distribution (fly64-trained -> Run1084 test)
Source directories:
- `experiment_outputs/fly64_trained_models/recon_on_run1084_pinn`
- `experiment_outputs/fly64_trained_models/recon_on_run1084_baseline`

Files:
- `pinn_fly64trained_run1084test_amplitude.png`
- `pinn_fly64trained_run1084test_phase.png`
- `pinn_fly64trained_run1084test_composite.png`
- `baseline_fly64trained_run1084test_amplitude.png`
- `baseline_fly64trained_run1084test_phase.png`
- `baseline_fly64trained_run1084test_composite.png`

### In-distribution (Run1084-trained -> Run1084 test)
Source directories:
- `experiment_outputs/run1084_trained_models/recon_on_run1084_pinn`
- `experiment_outputs/run1084_trained_models/recon_on_run1084_baseline`

Files:
- `pinn_run1084_amplitude.png`
- `pinn_run1084_phase.png`
- `pinn_run1084_composite.png`
- `baseline_run1084_amplitude.png`
- `baseline_run1084_phase.png`
- `baseline_run1084_composite.png`

### Ground truth (Run1084 object)
Source directory:
- `experiment_outputs/run1084_trained_models/recon_on_run1084_pinn`

Files:
- `ground_truth_run1084_amplitude.png`
- `ground_truth_run1084_phase.png`

### Probe visualizations
Source directory:
- `experiment_outputs/probe_visualizations`

Files:
- `fly64_probe_amplitude.png`
- `fly64_probe_phase.png`
- `run1084_probe_amplitude.png`
- `run1084_probe_phase.png`

### Notes
- The `paper/figures/out-dist-fig/*` files match the `experiment_outputs/*`
  sources exactly (byte-identical).
- The Run1084 in-distribution panels use `experiment_outputs/run1084_trained_models/`
  (not `run1084_trained_models_fixed/`).

## Data-Efficiency Curve (fig:ssim)

Current file: `paper/figures/ssim.png`

Notes:
- Source: `fly64_generalization_study_512_4096/ssim_phase_generalization.png`.
- `paper/figures/ssim.png` is now byte-identical to the study output.

## Overlap-Free vs Ptycho (fig:recon_2x2)

The 2x2 panels in `paper/figures/` are produced from inputs under
`paper/figures/scripts/inputs/` via `paper/figures/scripts/cdi_ptycho.py`.
Upstream study provenance for those input PNGs is not currently recorded.

## Small-Data Reconstructions (fig:smalldat)

Current files:
- `paper/figures/lowcounts.png`
- `paper/figures/8192.png`

Notes:
- `paper/figures/lowcounts.png` is byte-identical to:
  - `/home/ollie/Documents/tmp/PtychoPINN/3way_bothhalves_full_2xtest/train_256/trial_2/comparison_plot.png`
- `paper/figures/8192.png` has no byte-identical match yet. Searches in
  `/home/ollie/Documents/PtychoPINN` (including output/tmp PNGs) and
  `/home/ollie/Documents/tmp/PtychoPINN` returned no matches.

## Timing Benchmark (Optica throughput claim)

Run: 2026-01-27 (local benchmark run).

Setup:
- Model: `artifacts/sim_lines_4x/gs1_ideal/train_outputs`
- Dataset: `datasets/Run1084_recon3_postPC_shrunk_3.npz` (1087 patterns)
- Inference run: 1000 patterns

Warm (predict-only) measurement:
- Log: `.artifacts/throughput_benchmark_2026-01-27/run5_warm.log`
- Reconstruction time (predict only, no stitching): 0.22 s for 1000 patterns
- Throughput: ~4553 patterns/s (1000 / 0.22)

Reference (end-to-end, includes load + compile + stitching):
- Log: `.artifacts/throughput_benchmark_2026-01-27/run4.log`
- Reconstruction time (predict + stitching): 5.44 s for 1000 patterns (~184 patterns/s)
- End-to-end wall time: 19.12 s for 1000 patterns (~52 patterns/s)

## Metrics Tables

### SIM-LINES-4X Metrics Table (tables/sim_lines_4x_metrics.tex)

Regenerated: 2026-02-04 (gs1_ideal + gs2_ideal updated; gs1_custom + gs2_custom retained from 2026-01-27)

Source artifacts:
- `PtychoPINN/.artifacts/sim_lines_4x_metrics_2026-01-27/gs1_ideal/metrics.json` (2026-02-03 run)
- `PtychoPINN/.artifacts/sim_lines_4x_metrics_2026-01-27/gs1_custom/metrics.json` (2026-01-27 run)
- `PtychoPINN/.artifacts/sim_lines_4x_metrics_2026-01-27/gs2_ideal_nll/metrics.json` (2026-02-03 run, NLL-only)
- `PtychoPINN/.artifacts/sim_lines_4x_metrics_2026-01-27/gs2_custom/metrics.json` (2026-01-27 run)

Workflow parameters:
- `N=64`, `nimgs-train=2`, `nimgs-test=2`, `nepochs=60`, `batch-size=16`
- `realspace-weight=0.0`
- Idealized probe: `probe-smoothing-sigma=0.0`
- Custom probe: `probe-smoothing-sigma=0.5`
- Probe source: `datasets/Run1084_recon3_postPC_shrunk_3.npz`
- Loss weights:
  - gs1_ideal: `mae-weight=1.0`, `nll-weight=0.0`
  - gs2_ideal: `mae-weight=0.0`, `nll-weight=1.0`

Reproduction commands (from repo root):
```bash
python scripts/studies/grid_lines_workflow.py \
  --N 64 \
  --gridsize 1 \
  --output-dir .artifacts/sim_lines_4x_metrics_2026-01-27/gs1_ideal \
  --probe-npz datasets/Run1084_recon3_postPC_shrunk_3.npz \
  --probe-source ideal_disk \
  --probe-smoothing-sigma 0.0 \
  --probe-scale-mode pad_extrapolate \
  --nimgs-train 2 \
  --nimgs-test 2 \
  --nphotons 1e9 \
  --nepochs 60 \
  --batch-size 16 \
  --nll-weight 0.0 \
  --mae-weight 1.0 \
  --realspace-weight 0.0
```
```bash
python scripts/studies/grid_lines_workflow.py \
  --N 64 \
  --gridsize 2 \
  --output-dir .artifacts/sim_lines_4x_metrics_2026-01-27/gs2_ideal_nll \
  --probe-npz datasets/Run1084_recon3_postPC_shrunk_3.npz \
  --probe-source ideal_disk \
  --probe-smoothing-sigma 0.0 \
  --probe-scale-mode pad_extrapolate \
  --nimgs-train 2 \
  --nimgs-test 2 \
  --nphotons 1e9 \
  --nepochs 60 \
  --batch-size 16 \
  --nll-weight 1.0 \
  --mae-weight 0.0 \
  --realspace-weight 0.0
```

Generator script: `paper/tables/scripts/generate_sim_lines_4x_metrics.py`

### metrics.tex (backlog)

Tables needing regeneration:
- `tables/metrics.tex`

Planned regeneration path:
- Use the grid-lines workflow: `scripts/studies/grid_lines_workflow.py`
  (driver for `ptycho.workflows.grid_lines_workflow`).
