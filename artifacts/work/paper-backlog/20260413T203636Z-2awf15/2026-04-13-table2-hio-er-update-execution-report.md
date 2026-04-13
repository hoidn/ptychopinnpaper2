## Completed In This Pass

- Added the accepted same-bundle PyNX HIO/ER `gs1_custom` benchmark to the paper data source, generated table asset, inline Table 2, Results/Discussion text, and provenance records.
- Kept historical Table 2 `gs1_custom` values as context only and used the same-split PtychoPINN row for direct comparison.
- Used the same-split table-block presentation after PDF inspection showed the compact block fit cleanly.

## Completed Plan Tasks

- Tranche 1: provenance gate passed against solver, data-identity, metric-contract, support, outcome-summary, and primary metrics artifacts.
- Tranche 2: `data/sim_lines_4x_metrics.json` gained `same_split_cdi_benchmark`; `tables/scripts/generate_sim_lines_4x_metrics.py` now emits the optional block; `tables/sim_lines_4x_metrics.tex` was regenerated.
- Tranche 3: inline Table 2 gained the same-split experimental-probe benchmark block and caveat text.
- Tranche 4: Results and Discussion now state the same-generated-split, support-constrained, direct-stitch comparison and avoid broad classical-CDI claims.
- Tranche 5: `data/README.md`, `reviewer_revision_checklist.md`, `changelog.txt`, and the backlog item now cite the accepted outcome, table-generation command, support prior, direct-stitch/no-oracle contract, and same-split comparator values.
- Tranche 6: PDF compiled and Table 2 was inspected via text extraction and rendered page image.
- Tranche 7: final JSON, generator, consistency, and staged-whitespace checks were run.

## Remaining Required Plan Tasks

- None.

## Verification

- Provenance gate `jq` checks exited `0`.
- Accepted metrics check returned `eval_status=ok`, `n_test_frames=2178`, `amp_ssim=0.005342965825396031`, `amp_psnr=38.93470659640621`; outcome summary contains the rounded primary/sensitivity and same-split PtychoPINN values.
- `python -m json.tool data/sim_lines_4x_metrics.json >/tmp/sim_lines_4x_metrics.json.pretty` exited `0`.
- `python -m py_compile tables/scripts/generate_sim_lines_4x_metrics.py` exited `0`.
- `python tables/scripts/generate_sim_lines_4x_metrics.py --input data/sim_lines_4x_metrics.json --output /tmp/sim_lines_4x_metrics.tex` followed by `diff -u tables/sim_lines_4x_metrics.tex /tmp/sim_lines_4x_metrics.tex` produced no diff.
- `latexmk` was unavailable; fallback `pdflatex`, `bibtex`, `pdflatex`, `pdflatex` completed and updated `ptychopinn_2025.pdf`.
- `pdftotext -layout -f 8 -l 8 ptychopinn_2025.pdf -` showed the same-split group label, PtychoPINN `70.74`/`0.943`, PyNX HIO/ER `38.93`/`0.005`, and the known-probe/no-oracle caveat.
- Rendered page `/tmp/ptychopinn_table2_page-08.png` showed Table 2 fitting cleanly with readable row labels and caveat text.
- `rg` consistency checks found the expected PyNX/HIO/ER values and caveats across JSON, generated TeX, manuscript, README, checklist, changelog, and backlog.
- `git diff --check -- data/sim_lines_4x_metrics.json tables/scripts/generate_sim_lines_4x_metrics.py tables/sim_lines_4x_metrics.tex ptychopinn_2025.tex data/README.md reviewer_revision_checklist.md changelog.txt docs/backlog/2026-04-13-table2-hio-er-update.md` exited `0`.

## Residual Risks

- The checkout was dirty before this pass. The commit stages only the HIO/ER implementation hunks; unrelated pre-existing changes in shared files and generated `ptychopinn_2025.aux`/`ptychopinn_2025.log`/PDF state are left unstaged.
- Global `git diff --check` still reports trailing whitespace in the generated `ptychopinn_2025.log`, which is outside the staged implementation set.
- The PDF compile/inspection was performed in the current dirty working tree, per instruction not to use another checkout.
