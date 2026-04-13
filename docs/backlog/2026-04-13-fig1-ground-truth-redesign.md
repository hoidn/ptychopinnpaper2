# Fig. 1 Ground-Truth Redesign

Status: open

## Goal

Redesign Fig. 1 so the synthetic and semi-synthetic cases include ground-truth visuals, then integrate the redesigned figure and matching text into the paper.

This is not complete when the image files are generated. It is complete only when the figure is incorporated into `ptychopinn_2025.tex`, the compiled PDF has been inspected, and the layout has been iterated until it is aesthetically pleasing and reviewer-ready.

## Reviewer Motivation

Reviewer 2 asked for a visual representation of the ground truth in Fig. 1. The current checklist also requires the Fig. 1 caption and Results text to state that the CDI column is the overlap-free, single-frame PtychoPINN result with `$C_g=1`.

## Scope

- Regenerate or redesign the Fig. 1 content to include ground truth for the relevant synthetic/semi-synthetic cases.
- Update the Fig. 1 caption and Results text to clarify:
  - rows: idealized probe versus semi-synthetic experimental-probe case;
  - columns or panels: ground truth, single-shot CDI / `$C_g=1`, and overlapped ptychography / `$C_g=4`, as applicable;
  - CDI column meaning: overlap-free, single-frame PtychoPINN result.
- Adjust figure layout, panel labels, spacing, font sizes, colorbars, crop choices, and caption wording until the compiled PDF page is visually clean.
- Update `reviewer_revision_checklist.md` and `changelog.txt` after the paper-facing update is complete.

## Inputs and References

- Manuscript: `ptychopinn_2025.tex`
- Reviewer comment source: `reviewer_comments/2.txt`
- Current manuscript anchor: `ptychopinn_2025.tex`, figure label `fig:recon_2x2`
- Current Fig. 1 assets:
  - `figures/idealized_cdi_scaled_v5_small.png`
  - `figures/idealized_ptycho_scaled_v5_small.png`
  - `figures/hybrid_cdi_scaled_v5_small.png`
  - `figures/hybrid_ptycho_scaled_v5_small.png`
- Fig. 1 generator/provenance context:
  - `data/README.md`, section `Overlap-Free vs Ptycho (fig:recon_2x2)`
  - `figures/scripts/cdi_ptycho.py`
  - `figures/scripts/inputs/`
- Provenance caveat to resolve or carry explicitly: `data/README.md` currently says the upstream study provenance for the Fig. 1 input PNGs is not recorded. Before replacing paper assets, either recover and document source provenance or record the unresolved provenance caveat with the regenerated figure command.
- Checklist items:
  - `R1: Clarify Fig. 1 as single-shot PtychoPINN`
  - `R2: Redesign Fig. 1 to include ground truth`
  - `R2: Verify figure ordering in the compiled PDF`

## Acceptance Criteria

- [ ] Figure assets are regenerated or assembled from traceable source artifacts.
- [ ] Ground-truth asset source, generation command, and any unresolved provenance caveat are recorded in this backlog item, `data/README.md`, or a linked note.
- [ ] Fig. 1 includes ground-truth visuals for the synthetic/semi-synthetic rows where ground truth exists.
- [ ] Caption and nearby Results text explicitly identify the CDI result as overlap-free, single-frame PtychoPINN with `$C_g=1`.
- [ ] The compiled PDF has been inspected for figure order, panel alignment, crop consistency, typography, whitespace, caption clarity, and colorbar/label readability.
- [ ] Layout and aesthetics have been iterated until no obvious formatting issue remains.
- [ ] `data/README.md` is updated if the figure sources, generation path, or provenance status changes.
- [ ] `reviewer_revision_checklist.md` marks the relevant Fig. 1 items complete or records any remaining caveat.
- [ ] `changelog.txt` records the reviewer-facing Fig. 1 change.
- [ ] Any source commands, scripts, or manual asset steps are documented here or in a linked artifact note.
