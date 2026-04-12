# Fig. 5 OOD Metrics and Low-Frequency Phase Design

Status: draft initial design, not yet executed.

## Reviewer and Paper Context

Reviewer 3 comment 3 asks for quantitative metrics for the Fig. 5 out-of-distribution result and a physical discussion of why the low-frequency phase appears corrupted under APS -> LCLS transfer.

Relevant manuscript anchors:
- `ptychopinn_2025.tex`: Methods says APS-trained models are evaluated on LCLS data without retraining, with beamline-specific probe/geometry parameters substituted at inference.
- `ptychopinn_2025.tex`: Results text for Fig. 5 says the supervised baseline largely collapses under APS -> LCLS transfer, while PtychoPINN preserves edge structure with visible phase artifacts.
- `ptychopinn_2025.tex`: Fig. 5 uses `figures/out-dist-fig/*` panel PNGs and probe insets.
- `ptychopinn_2025.tex`: Discussion currently explains OOD behavior qualitatively but does not quantify the low-frequency phase artifact.

This is the easiest study ask because existing panel NPZs and cropped references are present. The main work is choosing a reproducible metric path and writing the evaluation policy.

## Current Assets and Provenance

Paper provenance:
- `data/README.md` documents that Fig. 5 panel PNGs in `figures/out-dist-fig/` are byte-identical copies from `experiment_outputs/`.

Out-of-distribution APS-trained -> Run1084/LCLS test sources:
- PtychoPINN reconstruction: `experiment_outputs/fly64_trained_models/recon_on_run1084_pinn/reconstruction.npz`
- PtychoPINN reference: `experiment_outputs/fly64_trained_models/recon_on_run1084_pinn/ground_truth_run1084_for_fly64trained.npz`
- Baseline reconstruction: `experiment_outputs/fly64_trained_models/recon_on_run1084_baseline/baseline_reconstruction.npz`
- Baseline reference: `experiment_outputs/fly64_trained_models/recon_on_run1084_baseline/ground_truth_run1084_for_fly64trained.npz`

In-distribution Run1084-trained -> Run1084 test sources:
- PtychoPINN reconstruction: `experiment_outputs/run1084_trained_models/recon_on_run1084_pinn/reconstruction.npz`
- PtychoPINN reference: `experiment_outputs/run1084_trained_models/recon_on_run1084_pinn/ground_truth_run1084.npz`
- Baseline reconstruction: `experiment_outputs/run1084_trained_models/recon_on_run1084_baseline/baseline_reconstruction.npz`

Probe sources for the curvature/mismatch discussion:
- APS/Fly64 probe: `experiment_outputs/probe_visualizations/fly64_probe.npz`
- LCLS/Run1084 probe: `experiment_outputs/probe_visualizations/run1084_probe.npz`

Important provenance warning: `experiment_outputs/fly64_trained_models/reconstructions_aligned.npz` contains Fly64-trained reconstructions aligned to Fly64 ground truth. It should not be used as APS -> LCLS OOD evidence unless additional provenance proves otherwise. Use the `recon_on_run1084_*` NPZ files for Fig. 5 OOD metrics.

Known tool gap: `python scripts/compare_models.py --help` currently fails because `ptycho.evaluation` imports missing module `ptycho.single_image_frc`. The design must choose whether to repair that canonical path or add a dedicated Fig. 5 metrics script over the existing panel NPZs.

If repairing FRC50 requires adding or replacing a package, record the package name, version, license, install command, and exact metric API used in the metrics manifest. Do not add FRC50 columns to the paper table from an unproven replacement dependency.

## Proposed Resolution

Add quantitative metrics and a low-frequency phase artifact analysis for the existing Fig. 5 panels.

Metric deliverable:
- Compute metrics for four rows: ID PtychoPINN, ID supervised baseline, OOD PtychoPINN, OOD supervised baseline.
- Include amplitude metrics and phase metrics separately.
- Include MSE or MAE as a direct error metric, PSNR as the MSE-derived metric, and SSIM for continuity with the current paper.
- Include FRC50 only if the existing single-image FRC dependency is repaired or a validated replacement is used with dependency provenance. Do not block the core reviewer response on FRC50 if the import problem is nontrivial.
- State phase alignment: remove global phase/low-order plane before phase metrics if using the existing `eval_reconstruction` convention. If low-frequency artifact analysis intentionally retains a component removed by plane fitting, say so explicitly.

Low-frequency phase deliverable:
- Compute phase residuals between the OOD PtychoPINN reconstruction and the ePIE/reference phase after the same crop and global/plane alignment policy used for the metrics.
- Plot the azimuthally averaged radial power spectrum of the phase residual, optionally compared to the ID PtychoPINN residual and/or OOD supervised baseline residual.
- Report a compact statistic such as the fraction of residual phase power below a chosen low-q cutoff. Choose the cutoff from image size/ring bins and record it in the script output.
- Optionally compute a simple probe-phase difference statistic between the APS/Fly64 and LCLS/Run1084 probe NPZs to support, but not overclaim, the probe-curvature mismatch discussion.

## Metrics Path Decision

This decision must be part of the implementation plan before execution:

Option A: repair `scripts/compare_models.py`.
- Advantages: canonical model comparison path, likely useful for future regeneration.
- Risks: import failure in `ptycho.evaluation` may reveal broader API drift; repair can expand beyond the reviewer response.
- Use if the import fix is a small compatibility shim or the existing missing module has a clear replacement.

Option B: add `scripts/studies/ood_fig5_metrics.py` over panel NPZs.
- Advantages: fastest, directly tied to the exact Fig. 5 assets, lower risk for the revision.
- Risks: could duplicate metric-loading code unless factored into a shared helper.
- Recommended first path for this revision. If adding shared code, keep it thin, for example `scripts/studies/_metrics_io.py`.

Both options must write a manifest listing source NPZs, crop/alignment policy, metric functions, and output table path.

## Required Final Assets

Minimum assets:
- Reproducible metrics script and manifest.
- Dependency note for any repaired or replacement FRC/metric package, including version, license, install command, and import/API entry point.
- Metrics CSV/JSON for ID/OOD x PtychoPINN/baseline.
- New table or compact inline table near Fig. 5. Suggested columns: condition, model, amplitude MSE, amplitude PSNR, amplitude SSIM, phase MSE, phase PSNR, phase SSIM. Add FRC50 columns only if the implementation is validated.
- Low-frequency phase residual figure or supplementary figure. Suggested output: one radial-power plot plus a one-line statistic in the caption or text.
- Results text update near Fig. 5 summarizing metric outcomes.
- Discussion text explaining likely causes: phase-reference ambiguity under transfer, weak low-frequency constraints in diffraction-domain training, and APS/LCLS probe or geometry mismatch. Phrase these as supported interpretations, not proven mechanisms, unless the probe comparison directly supports them.
- Reviewer-response text describing the exact metric and alignment policy.
- Changelog entry in `changelog.txt`.
- Checklist update in `reviewer_revision_checklist.md`.

## Verification

Before publication:
- Confirm metrics are computed from `recon_on_run1084_*` assets, not Fly64 -> Fly64 aligned arrays.
- Confirm the reference is described as ePIE if that is what the Fig. 5 panel uses.
- Confirm phase metrics and low-frequency residual analysis use a documented alignment/crop policy.
- Compile the paper and inspect table/figure placement near Fig. 5.
