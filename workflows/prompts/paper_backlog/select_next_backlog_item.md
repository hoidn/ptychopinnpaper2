Select the next paper backlog item to run through the generic design-plan-implementation stack.

Read the candidate backlog files, processed-ledger file, and paper-context files listed above. Treat `docs/backlog/README.md` as the completion policy for these items.

This is a judgment step, not a deterministic filter. Use status, prerequisites, current-blocker sections, acceptance criteria, referenced artifacts, and current repository state as evidence about readiness and value.

Selection guidance:
- Do not implement or edit the selected backlog item.
- Treat the processed-ledger file as the authoritative list of backlog paths already attempted in this workflow run. Do not select any backlog path already listed there.
- Treat `## Current Blocker` as evidence, not an automatic exclusion. If the blocker appears resolved from current artifacts or repo state, the item can be selected.
- Prefer items whose prerequisites are satisfied now and whose required inputs are present or reasonably inspectable.
- Avoid selecting an item if a required upstream workflow is still running, a required artifact is absent, or the backlog item itself says the work is not yet safe to present.
- If several items are ready, choose the one that is most useful to complete next for the paper revision while keeping risk and dependency churn low.
- If all ready items have already been processed in this workflow run, or if no item is ready, return `NONE_READY` and use `docs/backlog/README.md` as `selected_item_path`.

Write only the JSON file required by the output contract. The JSON must contain:
- `selection_decision`: `READY` or `NONE_READY`
- `selected_item_path`: path to the chosen backlog item under `docs/backlog`, or `docs/backlog/README.md` when `NONE_READY`

Create the output file's parent directory if it does not already exist.
