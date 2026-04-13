# Paper Backlog Generic Stack Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a paper-local workflow that repeatedly selects ready paper backlog items and runs each through the generic design-plan-implementation stack until no ready items remain.

**Architecture:** Reuse the generic `backlog_item_design_plan_impl_stack` and its imported phase workflows without flattening them. Add only a thin selector/driver workflow in the paper repo; selection is an agent judgment step with deterministic output validation, path derivation, and a run-local processed-item ledger so the loop does not reselect the same item.

**Tech Stack:** agent-orchestration DSL 2.7, reusable `call` workflows, Codex provider prompts, Python path-derivation command.

---

### Task 1: Install Generic Reusable Stack Files

**Files:**
- Create: `workflows/library/backlog_item_design_plan_impl_stack.yaml`
- Create: `workflows/library/tracked_design_phase.yaml`
- Create: `workflows/library/tracked_plan_phase.yaml`
- Create: `workflows/library/design_plan_impl_implementation_phase.yaml`
- Create: `workflows/library/prompts/design_plan_impl_stack_v2_call/*.md`

- [x] Copy the generic library stack and prompt bundle from `/home/ollie/Documents/agent-orchestration/workflows/library/` into the paper repo.
- [x] Verify imported `asset_file` prompt paths still resolve relative to `workflows/library/`.

### Task 2: Add Paper Backlog Draining Driver

**Files:**
- Create: `workflows/paper_backlog_next_design_plan_impl_stack.yaml`

- [x] Add a DSL 2.7 workflow importing `library/backlog_item_design_plan_impl_stack.yaml`.
- [x] Add `SelectNextBacklogItem`, which reads the paper backlog and chooses the next item as an agent judgment step.
- [x] Treat status, prerequisites, and current-blocker sections as readiness evidence rather than hard-coded exclusion rules.
- [x] Emit a JSON bundle with the selection decision and selected backlog path.
- [x] Add a deterministic preparation step that derives item id, state roots, and target artifact paths from the selected backlog path.
- [x] Route `READY` to the generic item stack and `NONE_READY` to loop termination.
- [x] Use a run-local processed-item ledger so the selector can skip items already attempted in the current workflow run.
- [x] Repeat selection and execution until the selector returns `NONE_READY`, bounded by a max item count.

### Task 3: Validate

**Files:**
- Inspect: `workflows/paper_backlog_next_design_plan_impl_stack.yaml`

- [x] Run `python -m orchestrator run workflows/paper_backlog_next_design_plan_impl_stack.yaml --dry-run` from the paper repo with `PYTHONPATH=/home/ollie/Documents/agent-orchestration`.
- [x] Confirm dry-run validates the agent selector prompt wiring without launching the provider.
