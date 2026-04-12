# Probe-Mischaracterization Stress Test Design

Status: draft initial design, not yet executed.

## Reviewer and Paper Context

Reviewer 3 comment 2 asks for a numerical stress test that injects varying degrees of error into the assumed probe during inference. The concern is that overlap-free reconstruction relies entirely on structured probe phase diversity, so a pre-estimated probe error may make the method vulnerable.

Reviewer 2 also says the paper would be more impactful if it addressed the fixed-probe limitation. Reviewer 1 asks whether probes are solved as unknown variables or supplied.

Relevant manuscript anchors:
- `ptychopinn_2025.tex`: Methods defines the probe as an estimated probe in the forward model.
- `ptychopinn_2025.tex`: Results/Table 2 attributes the strong `C_g=1` result to the experimental structured probe.
- `ptychopinn_2025.tex`: Discussion states the fixed-probe assumption as the main methodological limitation.
- `ptychopinn_2025.tex`: Conclusion says joint probe/position refinement is future work.

Scope decision: do not consider trainable-probe variants. The stress test should keep the model and its assumed probe fixed, then intentionally mischaracterize that probe at inference/evaluation time.

## Current Assets and Provenance

Helpful code paths:
- `ptycho/workflows/grid_lines_workflow.py` contains the Table 2 synthetic line-pattern workflow and probe transform pipeline.
- Probe transform utilities in that workflow already include smoothing, padding, interpolation, and mask application.
- The Table 2 `C_g=1` custom-probe metrics are in `.artifacts/sim_lines_4x_metrics_2026-01-27/gs1_custom/metrics.json`.
- The custom probe is `datasets/Run1084_recon3_postPC_shrunk_3.npz`.
- The standard metric source for this family of studies is `ptycho/evaluation.py::eval_reconstruction`.
- `ptycho/workflows/grid_lines_workflow.py::run_pinn_inference` calls `model.predict([X_test * intensity_scale, coords_nominal])`; the probe is not an explicit runtime input there. The model graph may still contain a probe variable through the forward-model branch, but this must be verified before claiming a pure inference-time probe perturbation.

Open provenance gap: the exact trained checkpoint for the existing 0.904 Table 2 row was not located in `.artifacts/sim_lines_4x_metrics_2026-01-27/gs1_custom`; that directory currently contains metrics and visuals, not `wts.h5.zip`. The study design must either locate the original checkpoint elsewhere, rerun the base `gs1_custom` training once, or adapt the workflow to produce and retain an inference-ready model artifact.

## Proposed Resolution

Run a fixed-probe mismatch study on the `C_g=1` synthetic line-pattern setup, anchored to the experimental-probe Table 2 condition.

Primary plan:
- Train or locate one baseline `C_g=1` experimental-probe PtychoPINN model corresponding to the Table 2 setup.
- Freeze the model weights.
- Run the model path that actually consumes the assumed probe with deliberately perturbed assumed probes.
- Keep the simulated measurements and ground truth fixed so the curves measure sensitivity to probe mischaracterization, not data-generation differences.

Important implementation ambiguity:
- A trained inverse-map object output may not change if only a post hoc evaluation probe is changed. Before the full study, run a smoke test that changes the probe variable or probe layer in the loaded model and verifies whether the reconstructed object output changes on a fixed `X_test`.
- If only the predicted diffraction/forward-model branch changes but the reconstructed object does not, decide whether the reviewer-facing study should instead be a wrong-probe training/reconstruction study, a test-time optimization study, or a narrower textual limitation. Record this decision before running the full perturbation grid.
- If the probe variable is baked into the saved model in a way that cannot be safely replaced, rerun the base workflow with controlled wrong-probe configurations and describe it as a fixed-wrong-probe reconstruction stress test rather than a pure post-training inference perturbation.

Perturbation axes:
- Amplitude scale: for example `1.0, 0.9, 0.8, 0.7`.
- Defocus/curvature mismatch: for example `0, 0.25, 0.5, 1.0` in a normalized Rayleigh-like or clearly defined Fresnel propagation unit.
- Probe phase noise: for example Gaussian phase noise `0, 0.1*pi, 0.2*pi, 0.5*pi`.

Implementation options:
- Add `scripts/studies/probe_mischaracterization_stress_test.py`.
- Add explicit perturbation operations to the existing probe transform machinery only if they are reusable and well tested; otherwise keep them local to the study script to avoid changing core workflow behavior.
- Use three random seeds for perturbation noise and any model retraining only if the checkpoint cannot be reused. For deterministic amplitude scaling and defocus, repeated runs are unnecessary unless model training is rerun.
- Save per-condition probe snapshots, metrics JSON/CSV, and a manifest with exact perturbation parameters.

Do not modify stable core physics/model files for this study. The intended touch points are study scripts and, if needed, narrowly scoped workflow transform utilities.

## Design Decisions Before Launch

The following decisions should be made before running the full grid:

- Checkpoint policy: use an existing checkpoint if it can be found and proven to match Table 2; otherwise rerun the Table 2 `gs1_custom` base model and make that rerun the explicit baseline.
- Probe-consumption policy: verify whether perturbing the assumed probe inside the trained model changes the reconstructed object output; if not, choose the wrong-probe training/reconstruction variant before launching.
- Perturbation-unit policy: define defocus in a physically interpretable way. If that is too slow to validate, use phase-noise and amplitude-scale axes first and reserve defocus for a second pass.
- Metric policy: report amplitude PSNR, amplitude SSIM, and optionally MSE/MS-SSIM to align with Table 2 and the reviewer concern about direct error.
- Plot policy: use a single figure with one panel per perturbation axis, or move the figure to the appendix if the main text is space constrained.
- Failure policy: if the largest perturbations collapse to a metric floor, report the collapse or shrink the perturbation grid after recording the reason. Do not hide floor behavior.

## Required Final Assets

Minimum assets:
- Reproducible stress-test script.
- Output manifest with baseline checkpoint path, data split, probe source, perturbation definitions, and seeds.
- Probe-consumption smoke-test result showing whether the object reconstruction changes under a controlled assumed-probe perturbation.
- Metrics CSV/JSON for all perturbation conditions.
- Figure: recommended 1 x 3 panel figure with perturbation magnitude on the x-axis, amplitude SSIM on the primary y-axis, and amplitude PSNR or MSE as a secondary or companion axis. Include the unperturbed 0.904 Table 2 baseline as a marker.
- Optional supplementary probe panel showing representative assumed probes for the highest non-collapsed perturbation in each axis.
- Results text subsection: "Sensitivity to probe mischaracterization" or appendix paragraph if space is tight.
- Methods text stating that the probe is supplied and held fixed, and identifying exactly where the stress test perturbs the assumed probe after the probe-consumption smoke test resolves the implementation path.
- Discussion cross-reference tying the result to the fixed-probe limitation, without claiming joint probe recovery.
- Reviewer-response text stating no trainable-probe variant was added in this revision.
- Changelog entry in `changelog.txt`.
- Checklist update in `reviewer_revision_checklist.md`.

## Verification

Before publication:
- Confirm perturbed-probe runs do not accidentally regenerate diffraction data with the same perturbed probe unless that is explicitly the study design. The reviewer asks for a wrong assumed probe during inference.
- Confirm the perturbation is not a no-op for the reported reconstruction output.
- Confirm the unperturbed baseline reproduces or clearly explains any deviation from the Table 2 `C_g=1` experimental-probe row.
- Confirm no trainable-probe language enters the study or response.
- Compile the paper and inspect the figure/caption placement.
