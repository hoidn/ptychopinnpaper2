# Non-ML Single-Shot CDI Benchmark Design

Status: draft initial design, not yet executed.

## Reviewer and Paper Context

Reviewer 3 comment 1 asks for a benchmark of overlap-free `C_g=1` PtychoPINN against a standard non-ML single-shot phase retrieval algorithm, naming ER/HIO with support constraints or ADMM phase retrieval. The reviewer specifically points to Table 2, where the current overlap-free result is compared only against overlapped PtychoPINN.

Relevant manuscript anchors:
- `ptychopinn_2025.tex`: abstract claims overlap-free single-shot reconstruction with experimental-probe SSIM 0.904 and frames the method as single-shot Fresnel CDI.
- `ptychopinn_2025.tex`: Methods defines "single-shot" as a single diffraction measurement with structured probe and no lateral scanning.
- `ptychopinn_2025.tex`: Table 2 reports the synthetic line-pattern overlap ablation.
- `ptychopinn_2025.tex`: Discussion compares idealized and experimental probe structure under `C_g=1` and `C_g=4`.

This item is the hardest study ask because no in-repo single-shot CDI ER/HIO/ADMM baseline was found. Existing iterative baselines are ptychographic multi-position methods, not clean single-frame CDI baselines.

## Current Assets and Provenance

Primary Table 2 pathway:
- Paper table: `tables/sim_lines_4x_metrics.tex`
- Paper data JSON: `data/sim_lines_4x_metrics.json`
- Paper table generator: `tables/scripts/generate_sim_lines_4x_metrics.py`
- Main workflow driver: `scripts/studies/grid_lines_workflow.py`
- Workflow implementation: `ptycho/workflows/grid_lines_workflow.py`
- Existing metrics source: `.artifacts/sim_lines_4x_metrics_2026-01-27/*/metrics.json`
- Existing `C_g=1` experimental-probe row: `.artifacts/sim_lines_4x_metrics_2026-01-27/gs1_custom/metrics.json`
- Probe: `datasets/Run1084_recon3_postPC_shrunk_3.npz`
- Synthetic object source: `ptycho.diffsim.sim_object_image(data_source="lines")`
- Standard metric function used by workflow: `ptycho/evaluation.py::eval_reconstruction`

Provenance warning: `data/README.md` names `.artifacts/sim_lines_4x_metrics_2026-01-27/gs2_ideal_nll/metrics.json`, but the current artifact directory contains `gs2_ideal/metrics.json`, not `gs2_ideal_nll/metrics.json`. Check this before regenerating the table or expanding provenance notes.

Tike is available in the local environment, but the relevant `tike.ptycho` algorithms are ptychographic and multi-frame. Treating those as the requested single-shot CDI baseline would not answer the reviewer without a careful scope explanation.

## External Solver and Dependency Discovery

This study may require finding or installing an external non-ML single-shot phase retrieval solver, especially if an in-repo HIO/ER implementation is too slow or too easy to misimplement.

Before implementing the baseline, run a bounded solver-discovery pass:
- Search the current environment and repository for existing CDI/phase-retrieval packages or scripts.
- Check whether candidate packages implement single-shot CDI HIO/ER/ADMM, not multi-position ptychography.
- Record package name, version, license, install command, and whether it supports known-probe Fresnel CDI or only far-field support-constrained phase retrieval.
- Prefer packages with a clear Python API, active maintenance, and reproducible installation in the current environment.
- If a package must be installed, record the exact command and environment after installation in the benchmark manifest. Add a requirements or environment note only if the dependency becomes part of the reproducible revision workflow.
- If no suitable external solver is available within the bounded discovery pass, fall back to a small in-repo HIO/ER implementation and document the support/initialization policy.

Candidate classes to evaluate:
- CDI HIO/ER packages or examples that operate on a single diffraction pattern plus support.
- General phase-retrieval packages that support custom Fourier-domain magnitude constraints and real-space support/probe constraints.
- ADMM phase-retrieval implementations only if they are easy to adapt and do not expand the revision beyond the benchmark need.

Do not use a package simply because it is available. A multi-frame ptychographic package would need to be explicitly ruled out or framed as outside the reviewer request.

## Proposed Resolution

Attempt to add a real non-ML single-shot baseline. The first implementation target is a small in-repo support-constrained HIO/ER benchmark rather than adapting a ptychography solver.

Candidate implementation:
- First complete the external solver/dependency discovery pass above.
- Add `scripts/reconstruction/hio_cdi_benchmark.py`.
- Build the same synthetic line-pattern test split as Table 2.
- Use the same supplied probe variants as Table 2: idealized probe and experimental/custom probe.
- Reconstruct under `C_g=1` conditions only.
- Use known-probe single-shot phase retrieval with a strict support.
- Start with 300-500 iterations, HIO beta annealed from about 0.5 to 0.1, and a short ER cleanup stage.
- Use 3 random-phase restarts per condition.
- Define support from the known probe-amplitude footprint, for example a thresholded or 0.9x footprint support. Record the exact rule in the output JSON.
- Keep the object phase treatment explicit. The synthetic line-pattern object has constant ground-truth phase, so an amplitude-only or zero-phase prior may be defensible only if disclosed as an oracle prior. A less privileged complex-object HIO baseline is scientifically cleaner but may be less stable.
- Evaluate with the same metric convention as Table 2. Prefer the existing `eval_reconstruction` path or a shared thin wrapper around it so PSNR/SSIM/MSE are comparable.

The design must decide whether the baseline is evaluated patch-by-patch and then stitched using the Table 2 scan/test split, or on a single canonical frame. The default should match the Table 2 stitched-test-split convention, because the reviewer is asking about Table 2. If that mapping is not possible cleanly, pause before reporting numbers.

## Pivot and Decision Criteria

Because the user approved attempting a real benchmark but allowed a pivot if it cannot be made to work or if it changes the claim, use the following decision points:

- If HIO/ER cannot be made numerically stable after a bounded implementation attempt, switch to a scoped textual response explaining why the current manuscript does not claim superiority over all single-shot CDI solvers and narrow the relevant claims.
- If the baseline is stronger than PtychoPINN under an oracle support/probe prior, do not cherry-pick. Decide whether to report it honestly with softened claims, move the comparison to supplementary material, or replace the competitive framing with a throughput/training/inference-scope explanation.
- If the baseline is much weaker but uses a disadvantaged support/initialization policy, do not overstate the result. Describe it as one non-ML reference baseline, not a definitive classical-method comparison.
- If adapting an ADMM or external package becomes substantially longer than the HIO path, keep HIO as the primary attempt and document ADMM as out of scope for this revision unless the reviewer response requires it.
- If an external solver requires fragile installation, unclear licensing, or non-reproducible build steps, do not make it a paper dependency without an explicit decision. Use the in-repo implementation or a textual scope response instead.

## Required Final Assets

Minimum assets if the benchmark is successful:
- Reproducible benchmark script under `scripts/reconstruction/` or `scripts/studies/`.
- Solver/dependency manifest recording whether the implementation is in-repo or external, with package name, version, license, install command, and import/API entry point if external.
- Machine-readable metrics output, preferably JSON, with all HIO/ER hyperparameters and support policy.
- Updated paper data JSON: `data/sim_lines_4x_metrics.json`.
- Regenerated `tables/sim_lines_4x_metrics.tex` with one or two non-ML baseline rows. Suggested labels: `HIO/ER, C_g=1 + idealized probe` and `HIO/ER, C_g=1 + experimental probe`.
- Caption update for Table 2 stating the support and probe priors used by the non-ML baseline.
- Results text update near Table 2 explaining the benchmark outcome and avoiding stronger claims than the data support.
- Reviewer-response text explaining the exact method and whether it is an oracle known-probe/support baseline.
- Changelog entry in `changelog.txt`.
- Checklist update in `reviewer_revision_checklist.md`.

If the benchmark is abandoned after a bounded attempt:
- A short artifact note with the attempted command(s), failure mode, and reason for pivot.
- If external solvers were investigated, a short rejected-candidate table explaining why they were unsuitable.
- Manuscript edits narrowing the single-shot CDI comparison claim.
- Reviewer-response paragraph explaining the scope decision.
- Changelog and checklist updates.

## Verification

Before publication:
- Confirm the new baseline rows use the same data split, photon dose, probe variants, and metric convention as Table 2, or state deviations explicitly.
- Confirm any external dependency is importable in the target environment from a clean command and that its version/license are recorded.
- Confirm PSNR remains tied to MSE-derived direct error in the table discussion.
- Confirm no ptychographic multi-frame solver is labeled as a single-shot CDI benchmark.
- Compile the paper and inspect Table 2 formatting.
