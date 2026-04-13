# Fig. 5 OOD Metrics Presentation

Status: open

## Goal

Resolve the Fig. 5 OOD metrics presentation and incorporate the chosen reviewer-facing result into the paper.

The presentation may be either:

- a compact metric table, or
- direct figure annotation on or near Fig. 5.

Choose between these based on aesthetics, convenience, and whether the metric evidence is scientifically safe to report. If the figure itself is revised, include a layout and aesthetics pass like the Fig. 1 task.

## Current Blocker

The checklist records that the refreshed Fig. 5 OOD metrics run is not reviewer-facing yet: the earlier default run recorded `accepted_metrics_artifacts=false` and no accepted metric JSON/CSV/table/summary exists under the study or paper roots.

The current implementation plan has been revised past the earlier design-review blocker. It now requires a `--validate-references-only` dry run so Run1084 coordinate alignment and panel-reference identity validation execute before any metric table values are computed. Treat the earlier stopped run as read-only context, not accepted evidence for the manuscript.

## Scope

- Resolve the scientific presentation decision:
  - accepted metrics table;
  - accepted figure annotation;
  - explicit no-metric/text-only pivot if the metric/reference gates remain unsafe.
- If metrics are accepted, create the paper-facing data artifact and either:
  - a small table included near Fig. 5, or
  - annotations on Fig. 5 with clear labels and formatting.
- If Fig. 5 is modified, repeatedly adjust the layout, annotations, panel labels, colorbar spacing, typography, and caption until the compiled PDF is aesthetically pleasing and readable.
- Update Results/Discussion text to explain OOD phase artifacts and state phase alignment/background handling, but only for accepted metrics.
- Do not claim a specifically low-frequency phase mechanism unless a verified residual diagnostic is added and accepted.
- Update `reviewer_revision_checklist.md` and `changelog.txt` after the paper-facing decision is implemented.

## Inputs and References

- Manuscript Fig. 5 TikZ block in `ptychopinn_2025.tex`
- Fig. 5 assets under `figures/out-dist-fig/`
- Paper data provenance: `data/README.md`, section `Out-of-Distribution Figure (fig:fivepanel / outdist.tex)`
- Paper-side design seed: `revision_designs/fig5_ood_metrics_low_frequency_phase.md`
- Current design/review artifacts:
  - `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/fig5_ood_metrics_low_frequency_phase_20260413T071529Z/approved_design.md`
  - `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/fig5_ood_metrics_low_frequency_phase_20260413T071529Z/implementation_plan.md`
  - `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/fig5_ood_metrics_low_frequency_phase_20260413T071529Z/plan_review.json`
  - `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/fig5_ood_metrics_low_frequency_phase_20260413T071529Z/run/fig5_ood_metrics_manifest.json`
  - `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/fig5_ood_metrics_low_frequency_phase_20260413T071529Z/run/pivot_summary.md`
- Checklist items:
  - `R3: Add quantitative metrics for OOD transfer`
  - `R3: Explain OOD low-frequency phase artifacts` (interpret as phase-artifact discussion unless a low-frequency diagnostic is accepted)
  - `Verification: Compile and inspect the paper`

## Required Metric/Claim Policy

- The accepted metric path must follow the current implementation plan's reference-validation gate before any table values are computed.
- Primary reviewer-facing scores must be coordinate-aligned only unless a documented, human-approved `panel_artifact_exception` is chosen before metric values are inspected.
- Fine registration is diagnostic/sensitivity-only and must not replace the primary table to obtain a more favorable claim.
- If a figure annotation is chosen instead of a table, it must still be backed by the same accepted metric artifact and documented in the paper-side data provenance.
- If no safe metric result is accepted, implement an explicit text-only pivot rather than adding a numeric table or annotation.

## Acceptance Criteria

- [ ] The registration/crop/channel-axis safety issues are resolved, or a documented no-metric/text-only pivot is chosen.
- [ ] The `--validate-references-only` gate, or its documented successor in the implementation plan, has passed before any accepted metric values are used.
- [ ] The chosen presentation format is recorded with rationale: table versus figure annotation versus no-metric pivot.
- [ ] Any accepted metric artifact is generated from traceable source data and saved under the paper repo or linked from a stable artifact path.
- [ ] The manuscript includes the table or annotation, or explicitly implements the approved pivot text.
- [ ] Phase alignment and background-handling conventions are stated if metrics are reported.
- [ ] OOD phase artifacts are discussed in the paper at an appropriate level; any specifically low-frequency claim is supported by an accepted diagnostic or omitted.
- [ ] If the figure is revised, the compiled PDF is visually inspected and layout/aesthetics are iterated until the page is clean.
- [ ] `data/README.md` records the accepted Fig. 5 metric artifact, table/annotation choice, metric policy, and any pivot/caveat.
- [ ] `reviewer_revision_checklist.md` marks the Fig. 5 OOD metrics and low-frequency phase artifact items complete or records a clear remaining caveat.
- [ ] `changelog.txt` records the Fig. 5/OOD update or the text-only pivot.
- [ ] The final presentation decision and source artifact paths are recorded here or in a linked note.
