# Eq. 1 Epsilon Revision Design

Status: draft initial design, not yet executed.

## Reviewer and Paper Context

Reviewer 3 comment 4 asks whether the real-space reconstruction in regions of very low overlapping probe intensity is sensitive to the magnitude of the `\epsilon=10^{-3}` denominator constant in Eq. 1.

Relevant manuscript anchors:
- `ptychopinn_2025.tex`: Eq. 1 defines the stitched group reconstruction denominator as an overlap-count sum plus `\epsilon=10^{-3}`.
- `ptychopinn_2025.tex`: The formulation supports `C_g=1`, where nonzero-coverage pixels have exactly one contributing patch.
- `ptychopinn_2025.tex`: Table 2 relies heavily on `C_g=1` overlap-free results.

User-approved resolution space: either add a text-only explanation or lower the value of epsilon. No new epsilon sweep is required unless the text/code audit makes the answer unsatisfactory.

## Current Code Evidence

Observed code paths:
- `ptycho_torch/helper.py` has a Torch reassembly path with `norm_factor = non_zeros_float.real + 0.001`.
- Nearby Torch crop-size branch uses `+ 1e-9`.
- `ptycho/tf_helper.py` reassembly paths use `1e-9 + shift_and_sum(...)`.

This means the manuscript value `10^{-3}` appears to match at least one Torch helper path, but may not describe every path used for the reported TensorFlow-era results. Before lowering epsilon or changing the manuscript value, confirm which path generated the paper results.

## Proposed Resolution

Primary recommendation: text-only explanation.

Draft claim to support:
- The epsilon term regularizes pixels with zero or near-zero support count.
- In any pixel with at least one contributing patch, the denominator is at least 1 before epsilon.
- Therefore, for `\epsilon=10^{-3}`, the relative change in nonzero-coverage pixels is at most about 0.1 percent for `C_g=1` and smaller for higher overlap counts.
- Zero-coverage pixels also have zero numerator under the same stitching mask, so epsilon prevents division by zero rather than changing a measured reconstruction value.
- This makes the reported overlap-free support region insensitive to epsilon at the scale relevant to Table 2, provided the crop/mask excludes unsupported canvas pixels from metrics and display.

If the audit shows the actual reported workflow used `1e-9`, change the manuscript equation or text so the value is provenance-correct instead of lowering code to match the manuscript. If the reported workflow used `10^{-3}`, keep the value and add the explanation.

## Lower-Epsilon Alternative

Only choose this if the authors prefer code/text consistency at a smaller value and are willing to validate it:
- Change the relevant reassembly epsilon to `1e-9` or a named constant.
- Add a narrowly scoped regression test or numerical comparison showing unchanged nonzero-coverage output.
- Rerun or verify the affected paper metric path if the modified code path was used for reported results.
- Update Eq. 1 and text to the new value.

This alternative is more work and may create provenance churn if historical metrics were generated with `10^{-3}`.

## Optional Sweep

If a reviewer response requires empirical support, run a minimal sweep over values such as `10^{-1}, 10^{-2}, 10^{-3}, 10^{-4}, 10^{-6}` on the smallest reproducible Table 2 `C_g=1` setup. Report a one-line supplementary table only if the result is clearer than the analytical explanation.

## Required Final Assets

Minimum assets for text-only resolution:
- Methods or equation-adjacent sentence explaining what epsilon regularizes and why nonzero-coverage pixels are insensitive.
- If needed, a reviewer-response sentence with the relative-effect bound: denominator changes from `n` to `n + epsilon`, with `n >= 1` in supported pixels.
- Changelog entry in `changelog.txt`.
- Checklist update in `reviewer_revision_checklist.md`.

Assets if lowering epsilon:
- Code diff with the named or lowered epsilon.
- Test or numerical check output.
- Manuscript Eq. 1 value update.
- Changelog and checklist updates.

## Verification

Before publication:
- Confirm which implementation path generated the metrics being discussed.
- Confirm unsupported pixels are not interpreted as scientifically meaningful reconstruction regions in figures/tables.
- Compile the paper and inspect Eq. 1 formatting and explanatory text.
