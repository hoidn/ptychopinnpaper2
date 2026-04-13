# Probe-Mischaracterization Figure Integration

Status: open

## Goal

Check that the probe-mischaracterization stress figure is correctly incorporated in the paper, then complete a visual layout and aesthetics pass before treating the item as done.

The numeric study and exported paper assets are already recorded as complete in the checklist, but the paper still needs a focused integration review for figure placement, caption quality, visual polish, and consistency with the fixed-probe sensitivity claim.

## Current Assets

- Paper data: `data/probe_mischaracterization_metrics.json`
- Paper figure: `figures/probe_mischaracterization_stress.png`
- Study run: `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/probe_mischaracterization/full_process_isolated_20260413T020739Z`

## Context and Source Documents

- Checklist item: `R3: Decide whether to add a probe-mischaracterization stress test`
- Paper-side design seed: `revision_designs/probe_mischaracterization_stress_test.md`
- Approved stress-test design: `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/probe_mischaracterization/approved_design.md`
- Stress-test execution/review artifacts:
  - `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/probe_mischaracterization/execution_report.md`
  - `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/probe_mischaracterization/implementation_review.md`
- Probe-context panel plan: `/home/ollie/Documents/PtychoPINN/docs/plans/revision-studies/probe-mischaracterization-probe-context-panels-plan.md`
- Paper data provenance file: `data/README.md`
- Current manuscript anchors: `ptychopinn_2025.tex`, figure label `fig:probe_mischaracterization_stress`, and the fixed-probe limitation paragraph in Discussion.
- Checklist progress notes record both the numeric run and an initial compiled-PDF inspection. This backlog item is a focused layout/aesthetic verification and any needed figure polish, not a request to rerun the numeric stress study.

## Scope

- Verify that the figure file in `figures/probe_mischaracterization_stress.png` matches the accepted study output.
- Inspect the manuscript integration in `ptychopinn_2025.tex`, including:
  - figure placement;
  - figure width and page break behavior;
  - caption wording;
  - Results text near the figure;
  - Discussion text on fixed-probe limitation and sensitivity rather than robustness.
- Perform a layout/aesthetics pass before marking complete:
  - adjust figure sizing, panel readability, label visibility, and caption clarity if needed;
  - if the current figure is cramped or too metrics-only, consider regenerating from existing artifacts with probe-context panels rather than rerunning the stress study;
  - if probe-context panels are used, follow the separate-panels contract in the probe-context panel plan and preserve existing numeric metrics, gates, and perturbation grid;
  - compile and inspect the affected page after each substantive layout change.
- Update `reviewer_revision_checklist.md` with the final integration/aesthetic verification result. Update `changelog.txt` if any paper-facing figure, caption, text, or data artifact changes.
- Review `data/README.md` and update it if the paper-side figure/data provenance is missing, stale, or changed; otherwise record that no data-provenance update was needed.

## Acceptance Criteria

- [ ] The included figure asset matches the accepted claim-gated run.
- [ ] The compiled PDF shows the figure in a reasonable location with readable labels and no obvious layout issue.
- [ ] Caption and Results text state fixed true measurements and perturbed reconstruction-side assumed probe.
- [ ] Text avoids robustness claims and uses sensitivity language.
- [ ] Any layout or aesthetics issue is iterated until the figure is reviewer-ready.
- [ ] If probe-context panels are generated, they use separate panels rather than insets and preserve the accepted numeric study outputs.
- [ ] `data/README.md` is updated with probe figure/data provenance or this backlog item records why no update was needed.
- [ ] `reviewer_revision_checklist.md` records the final integration/layout verification, even if no paper-facing file changes were needed.
- [ ] `changelog.txt` records any additional paper-facing figure/layout change, or this backlog item records why no changelog entry was needed.
- [ ] The final compile/inspection result and any regenerated asset command are recorded here or in a linked note.
