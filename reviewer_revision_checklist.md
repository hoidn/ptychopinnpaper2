# Reviewer Revision Checklist

**Goal:** Track manuscript revisions needed to address reviewer comments for `ptychopinn_2025.tex`.

**Canonical files:**
- Manuscript: `ptychopinn_2025.tex`
- Reviewer comments: `reviewer_comments/1.txt`, `reviewer_comments/2.txt`, `reviewer_comments/3.txt`
- Changelog: `changelog.txt`
- Initial study/revision design docs: `revision_designs/`

**Scope decisions:**
- Treat `revision_plan.md` as stale; do not use it as the source of truth.
- New experiments are allowed, but decide case by case whether a reviewer demand is best addressed with new analysis or with a scoped textual response.
- For the SSIM concern, a PSNR-as-MSE-derived explanation is acceptable.
- Redesign Fig. 1 to include ground truth rather than only adding a textual note.
- Do not consider trainable-probe variants for the probe-mischaracterization stress test; if run, treat the probe as fixed and intentionally mischaracterized.

**Update protocol:**
- Update this checklist as progress is made.
- Mark completed items with `[x]`.
- For items that need a decision, add the chosen resolution under the item before marking it complete.
- Keep `changelog.txt` synchronized with completed reviewer-facing changes.

## Checklist

- [ ] **R1: Make the manuscript self-contained.**
  Add a concise "what is new relative to Hoidn et al. 2023" paragraph near the end of the Introduction or start of Methods.

- [ ] **R1: Improve post-Introduction narrative flow.**
  Audit the manuscript after the Introduction for remaining bullet-point-like presentation. Add transitions and paragraph flow where needed. The one-line changelog says this was partly fixed, so treat this as a verification and polish pass.

- [ ] **R1: Clarify Fig. 1 as single-shot PtychoPINN.**
  State in both the Fig. 1 caption and Results text that the CDI column is the overlap-free, single-frame PtychoPINN result with `$C_g=1`.

- [ ] **R1: Rework the extended-probe architecture explanation.**
  Make clear why the "Handling extended probes" subsection belongs in Neural Network Architecture: it motivates the dual-resolution decoder used to avoid probe-tail truncation artifacts.

- [ ] **R1: Define the iterative reference method in the OOD figure.**
  Current text says ePIE. Verify the compiled figure and caption do not still use Tike/Tike reconstruction language; if they do, update them.

- [ ] **R1/R2/R3: State the fixed-probe assumption directly.**
  Add a Methods sentence saying the probe is pre-estimated/supplied and held fixed; PtychoPINN does not jointly solve for probe or position parameters in this study. Cross-reference the same limitation in Discussion.

- [ ] **R2: Verify figure ordering in the compiled PDF.**
  Reviewer 2 said Fig. 1 is discussed after Fig. 2. Current source may already fix this; compile and inspect the PDF.

- [ ] **R2: Redesign Fig. 1 to include ground truth.**
  Add ground-truth visuals for the synthetic/semi-synthetic cases rather than only adding a caption note.

- [ ] **R2: Address SSIM versus direct-error metrics.**
  Add text explaining that PSNR is derived from MSE, so the table reports a direct pixel-error-based metric alongside SSIM.

- [ ] **R3: Decide whether to add a non-ML single-shot CDI benchmark.**
  Decision needed: either add a benchmark against a standard non-ML single-shot phase retrieval method such as ER/HIO/support-constrained or ADMM phase retrieval, or soften competitive single-shot Fresnel CDI claims and explain scope.
  Investigation note (2026-04-12): Existing non-ML baselines are Tike/PtyChi ptychographic reconstructions using multi-position scan data (RPIE/DM/LSQML/PIE), not single-shot CDI ER/HIO/ADMM. No in-repo single-shot CDI phase-retrieval implementation was found. Addressing this experimentally likely requires a new baseline implementation/integration plus a support/initialization/evaluation policy, or a scoped rebuttal that narrows the claim.
  Initial design doc: `revision_designs/non_ml_single_shot_cdi_benchmark.md`. Current decision: attempt a real support-constrained HIO/ER-style benchmark first; pivot to a scoped manuscript/response revision if the baseline cannot be made cleanly comparable or if the result requires changing the paper's claim. The design now requires an external-solver/dependency discovery pass, including version/license/install provenance, before choosing an in-repo or package-based HIO/ER/ADMM path.

- [ ] **R3: Decide whether to add a probe-mischaracterization stress test.**
  Decision needed: either perturb the assumed probe amplitude/phase or blur/defocus and report degradation, or strengthen the fixed-probe limitation and avoid overstating robustness.
  Investigation note (2026-04-12): No direct probe-mischaracterization stress-test run/script was found. Related support exists in grid-lines workflows for probe smoothing, mask diameter, probe source, and scale/transform mode; older `probe_generalization_*` artifacts compare ideal vs experimental probe families but appear to keep train/test probes matched within each arm, so they do not answer the reviewer’s fixed-wrong-probe stress-test request. If addressed with an experiment, define the mismatch axis and keep the supplied probe fixed.
  Initial design doc: `revision_designs/probe_mischaracterization_stress_test.md`. Current decision: run a fixed-probe stress study; do not include trainable-probe variants. Ambiguity to resolve before the full run: verify that perturbing the assumed probe changes the reported reconstruction output rather than only a forward-model/evaluation branch.

- [ ] **R3: Add quantitative metrics for OOD transfer.**
  Add metrics for the Fig. 5 APS -> LCLS transfer against the ePIE reference. State phase alignment and background handling.
  Investigation note (2026-04-12): Fig. 5 panel PNGs are byte-identical copies from `experiment_outputs/*/recon_on_run1084_*`, with the active TikZ montage inlined in `ptychopinn_2025.tex`. Current `experiment_outputs/run1084_trained_models/comparison_metrics.csv` covers the Run1084->Run1084 in-distribution control; `experiment_outputs/fly64_trained_models/comparison_metrics.csv` is fly64->fly64 and must not be used as APS->LCLS OOD evidence. No existing APS-trained/Run1084-test `comparison_metrics.csv` was found. Existing Fig. 5 NPZ panels and cropped references are enough to compute metrics, but the canonical `scripts/compare_models.py` entry point currently fails at import because `ptycho.single_image_frc` is missing, so repair that path or add a dedicated reproducible metrics script before publication.
  Initial design doc: `revision_designs/fig5_ood_metrics_low_frequency_phase.md`. Current decision: choose the metric computation path during design/planning; draft recommendation is to use a dedicated panel-NPZ script unless `scripts/compare_models.py` can be repaired with a small compatibility fix. If FRC50 requires a repaired or replacement package, record version/license/install provenance before using it in the paper.

- [ ] **R3: Explain OOD low-frequency phase artifacts.**
  Add physical discussion of likely causes: phase-reference ambiguity, weak low-frequency constraints under transfer, and probe/geometry mismatch.
  Initial design doc: `revision_designs/fig5_ood_metrics_low_frequency_phase.md`.

- [ ] **R3: Address Eq. 1 epsilon sensitivity.**
  Minimal resolution: explain that `epsilon=10^{-3}` regularizes zero/near-zero support pixels and is negligible where support is nonzero. Stronger resolution: add a small epsilon sweep.
  Initial design doc: `revision_designs/eq1_epsilon_revision.md`. Current decision: prefer a text-only explanation or lower epsilon only after confirming which implementation path generated the reported metrics; no sweep by default.

- [ ] **R3: Clarify dose-ablation data source.**
  Update the dose/FRC50 figure caption to say whether the dose ablation used synthetic Siemens-star data or fully experimental data.

- [ ] **R3: Explain `K=4`.**
  Add a sentence in Methods or the configuration table explaining that `K=4` is a local-neighbor grouping choice intended to provide enough nearby candidates for `$C_g=4$` overlap groups while preserving spatial locality. Verify this matches the actual run configuration.

- [ ] **Changelog: Expand `changelog.txt`.**
  Replace the current one-line changelog with a reviewer-response changelog grouped by issue or reviewer.

- [ ] **Verification: Compile and inspect the paper.**
  Check figure order, ground-truth panel placement, caption clarity, small fonts, stale "Tike", placeholder "X nm", TODOs, and readability.

## Decision Log

- Decided: attempt a real non-ML single-shot CDI benchmark first, with pivot criteria documented in `revision_designs/non_ml_single_shot_cdi_benchmark.md`.
- Decided: run a fixed-probe probe-mischaracterization stress test, with no trainable-probe variants.
- Pending: OOD Fig. 5 metrics computation path (repair `compare_models.py` vs write a small panel-NPZ metrics script); the design doc recommends the panel-NPZ script as the first revision path unless the import failure is trivial.
- Decided: Eq. 1 epsilon response can be text-only or a lower-epsilon code/text change after implementation-path audit; no epsilon sweep by default.

## Study Ask Difficulty Ranking

1. **OOD quantitative metrics for Fig. 5** - easiest. Existing panel NPZs and cropped references are present; the remaining work is a reproducible metric computation path and evaluation-policy writeup.
2. **Probe-mischaracterization stress test** - medium. Existing probe-transform and probe-mask infrastructure helps, but the actual reviewer ask needs new fixed-probe mismatch conditions.
3. **Non-ML single-shot CDI benchmark** - hardest. Existing iterative baselines are ptychographic, not single-shot CDI, so this likely needs external solver discovery/installation or a new ER/HIO/ADMM-style baseline, plus a carefully scoped rebuttal if neither route is clean.

## Progress Notes

- 2026-04-12: Created checklist from reviewer comments and user scope decisions.
- 2026-04-12: Added provenance notes and difficulty ranking for Fig. 5 OOD metrics, probe-mischaracterization stress test, and non-ML single-shot CDI benchmark.
- 2026-04-12: Recorded decision not to consider trainable-probe variants for the probe-mischaracterization stress test.
- 2026-04-12: Drafted initial design docs for the non-ML single-shot CDI benchmark, probe-mischaracterization stress test, Fig. 5 OOD metrics/low-frequency phase analysis, and Eq. 1 epsilon response under `revision_designs/`.
- 2026-04-12: Updated the non-ML benchmark design to require external solver/dependency discovery and install provenance before choosing an in-repo or package-based HIO/ER/ADMM implementation.
- 2026-04-12: Updated the Fig. 5 metrics design to require dependency provenance for any repaired or replacement FRC/metric package.
