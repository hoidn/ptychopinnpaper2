# Table 2 HIO/ER Benchmark Update

Status: open

## Goal

Update Table 2 and the surrounding manuscript text to include the accepted non-ML single-shot HIO/ER benchmark result. Treat the completed same-bundle PyNX HIO/ER outcome as the intended paper update unless a final provenance sanity check finds a concrete blocker.

This is an end-to-end paper update, not just a data-copy task. Completion requires updating the table source, manuscript interpretation, changelog, checklist, and compiled PDF formatting.

## Current Evidence

The checklist records the same-bundle PyNX non-ML single-shot CDI outcome gate as complete for `gs1_custom`, with primary support `0.05`, support sensitivities `0.01` and `0.10`, `eval_status=ok` on 2178 frames, and PyNX `2024.1` selected.

Use the recorded outcome summary as the first source of truth:

`/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/non_ml_single_shot_cdi_benchmark/full_gs1_custom_pynx_primary_reuse_pinn_bundle_20260413T060016Z/outcome_summary.md`

Do not reopen metric generation unless the final provenance read exposes a specific inconsistency.

## Context and Source Documents

- Reviewer/checklist context: `reviewer_revision_checklist.md`, item `R3: Decide whether to add a non-ML single-shot CDI benchmark`
- Paper-side design seed: `revision_designs/non_ml_single_shot_cdi_benchmark.md`
- Main repo design seed: `/home/ollie/Documents/PtychoPINN/docs/plans/revision-studies/non-ml-single-shot-cdi-benchmark-design-seed.md`
- Execution plan: `/home/ollie/Documents/PtychoPINN/docs/plans/revision-studies/non-ml-single-shot-cdi-benchmark-execution-plan.md`
- PyNX replacement plan: `/home/ollie/Documents/PtychoPINN/docs/plans/revision-studies/non-ml-single-shot-cdi-benchmark-pynx-replacement-plan.md`
- Final outcome summary: `/home/ollie/Documents/PtychoPINN/.artifacts/revision_studies/non_ml_single_shot_cdi_benchmark/full_gs1_custom_pynx_primary_reuse_pinn_bundle_20260413T060016Z/outcome_summary.md`
- Paper metrics provenance: `data/README.md`, section `SIM-LINES-4X Metrics Table (tables/sim_lines_4x_metrics.tex)`
- Table generator: `tables/scripts/generate_sim_lines_4x_metrics.py`

## Caveats To Preserve

- The primary PyNX HIO/ER row is support threshold `0.05`. Support thresholds `0.01` and `0.10` are sensitivity rows unless a separate decision changes the presentation.
- The old Table 2 `gs1_custom` values are historical context only, not a same-data comparator, because exact frozen Table 2 data identity was not proven.
- The same-split PtychoPINN comparator is the valid direct comparator for the PyNX row.
- The PyNX HIO/ER row uses a known-probe support prior derived from the supplied custom probe. Disclose that prior in the table note, caption, or adjacent text.
- The main HIO/ER rows intentionally do not apply oracle CDI ambiguity alignment. Interpret low direct-stitch metrics accordingly.
- The result supports a scoped statement about this direct, support-constrained single-shot CDI baseline on the same generated split. Do not present it as a broad claim about all classical CDI methods.

## Scope

- Decide the final Table 2 presentation:
  - add an HIO/ER row to Table 2, or
  - add a compact companion note if the table becomes too dense.
- Update the paper-side data source and generated table files as appropriate:
  - `data/sim_lines_4x_metrics.json`
  - `tables/sim_lines_4x_metrics.tex`
  - inline Table 2 in `ptychopinn_2025.tex`, if the manuscript currently duplicates the table instead of inputting the generated file.
- Update surrounding Results and Discussion text so claims are scoped to the actual comparator conditions and priors.
- Preserve the distinction between the same-bundle PyNX HIO/ER comparator and historical Table 2 values if any metric-contract caveat remains.
- Compile the paper and inspect Table 2 for width, line breaks, row labels, caption clarity, and consistency with the text.
- Update `data/README.md`, `reviewer_revision_checklist.md`, and `changelog.txt`.

## Acceptance Criteria

- [ ] The accepted HIO/ER metric values and provenance are copied from the final outcome artifact, not from an exploratory or superseded run.
- [ ] The table source used by the paper is updated and reproducible.
- [ ] Table caption or note discloses the HIO/ER method class, support/known-prior assumptions, and same-bundle comparator status at the level needed for reviewer clarity.
- [ ] The text/table does not compare the HIO/ER row to historical Table 2 values as if they were same-data; it uses the same-split PtychoPINN comparator or clearly labels historical context.
- [ ] Sensitivity rows are either omitted from the main table or clearly labeled as support-threshold sensitivity, not alternate primary results.
- [ ] Results text accurately interprets the HIO/ER result without overclaiming.
- [ ] The compiled PDF has been inspected for Table 2 formatting and readability.
- [ ] Any table layout issue is iterated until it is publication-ready.
- [ ] `data/README.md` records the new Table 2 data source, outcome artifact, table-generation command, and caveats.
- [ ] `reviewer_revision_checklist.md` marks the non-ML single-shot CDI benchmark item complete or records a clear remaining caveat.
- [ ] `changelog.txt` records the Table 2/HIO/ER revision.
- [ ] The backlog item records the final artifact path and any table-generation command used.
