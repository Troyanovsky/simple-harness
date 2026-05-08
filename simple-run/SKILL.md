---
name: simple-run
description: >
  Orchestrate the full implementation of a feature by looping through its issues.json task list,
  invoking simple-implement for each task sequentially until all tasks are done. Use this skill
  whenever the user wants to auto-complete a feature, run through all tasks, automate implementation,
  or execute the full task queue without manual intervention. Trigger on phrases like "run all tasks",
  "auto-implement", "complete the feature", "run through the issues", "implement everything",
  "execute the task list", or any request that implies looping through tasks automatically.
disable-model-invocation: true
---

# Simple Run — Orchestrate Feature Implementation

You are an orchestrator. Your job is to drive a feature from `tasks_ready` to `done` by
repeatedly invoking **simple-implement** in a sequential loop — one task at a time — until
the work queue is empty or a stopping condition is met.

You do NOT implement code yourself. You delegate each task to a sub-agent that follows the
**simple-implement** skill, then inspect the result and decide whether to continue.

## Folder convention

Same as all simple-\* skills:

```
docs/
  index.json              ← feature manifest
  visual.md               ← app-level visual design (optional)
  <feature-name>/
    spec.md
    design.md
    issues.json           ← work queue
    progress-log.md       ← updated by each sub-agent
```

## Workflow

### 1. Initialize

- Identify the feature to work on. If the user specifies a feature name, use it. Otherwise,
  check `docs/index.json` for features with status `"tasks_ready"` or `"in_progress"`.
- Read `docs/<feature-name>/issues.json` to understand the full scope.
- Count total tasks, completed tasks, and remaining tasks. Report this to the user as a
  starting summary (e.g., "Feature `auth` has 12 tasks: 4 done, 8 remaining.").
- Update `docs/index.json` to set the feature status to `"in_progress"` if not already.
- **Capture a test baseline.** Run the full test suite and record pass/fail results.
  This lets review sub-agents distinguish pre-existing failures from new regressions.
  Refresh the baseline after each successful commit.

### 2. Loop

For each iteration:

**a) Check for the next available task:**

- Read `issues.json` (fresh each iteration — the sub-agent may have added new tasks).
- Apply the same task-selection algorithm as simple-implement:
  1. Filter to `status: "todo"`.
  2. Exclude tasks with unmet dependencies (any `depends_on` task that is not `"done"`).
  3. Pick the lowest `priority` number. Break ties by lowest ID.

**b) If a task is available, spawn a sub-agent:**

- Launch a sub-agent with access to the full project codebase and the docs folder.
- Instruct the sub-agent to follow the **simple-implement** skill for the specific feature
  and task ID.
- The sub-agent prompt should include:
  - The feature name and task ID.
  - The path to the docs folder.
  - An instruction to follow simple-implement's workflow (orient → pick → understand →
    implement → verify → update state → summarize).
  - The flag `[INVOCATION_MODE: automated]` — this is the contract signal that tells
    simple-implement it is running in automated mode and should not pause for user
    confirmation under any circumstances.

**c) After the sub-agent completes:**

- Read the updated `issues.json` to confirm the task status changed.
- Read the latest entry in `progress-log.md` to understand what happened.
- If the task is `"blocked"`, log the blocker and decide:
  - If other non-blocked tasks remain, skip to the next one.
  - If all remaining tasks are blocked, stop and report to the user.
- If the task is `"done"`, proceed to the review step (d).

**d) Review and commit:**

Spawn a **review sub-agent** with the test baseline (from step 1) and the git diff.
The reviewer evaluates:

- **Correctness** — bugs, edge cases, side effects, collateral impact on related code.
- **Tests** — coverage added/updated, suite passes locally. Compare failures against
  baseline to separate regressions from pre-existing issues. Re-run any non-baseline
  failure once before counting it (handles flaky tests).
- **Architecture hygiene** — adherence to project conventions (`AGENTS.md`, `CLAUDE.md`).
  No violations of KISS/YAGNI/SOLID/DRY (unnecessary abstractions, duplicated logic,
  god-objects, etc.). File/folder organization consistent with existing structure.
- **Scope** — flag unexpected file changes outside the task's expected footprint.

**Three verdicts** (the dividing line: _will this issue cause downstream tasks to build
on a broken foundation?_):

1. **Pass** — correct, clean, no new test failures. Commit and continue.
2. **Pass-with-concerns** — fundamentally correct, safe to commit, but has non-blocking
   findings (pre-existing test failures, minor style issues, tooling/build failures
   where code is visibly correct, reasonable-but-flaggable spec interpretations).
3. **Fail** — correctness issues that would compound: logic bugs in shared code, missing
   required validations, new test regressions, blatant architecture violations.

**Acting on the verdict:**

- **Pass / Pass-with-concerns:** `git add` + `git commit` referencing the task ID
  (e.g., `feat(auth): TASK-auth-003 — add session refresh logic`). Refresh the test
  baseline. For concerns, also log each as a low-priority `"source": "review"` task in
  `issues.json` and note them in `progress-log.md`.
- **Fail:** Spawn a **fix sub-agent** with the specific issues to fix. After fixes, re-review.
  Max 2 fix cycles — if still failing, mark the task `"blocked"`, revert uncommitted
  changes (`git checkout -- .`), and move on.

**Concern accumulation guardrail:** If unresolved `"source": "review"` tasks in
`issues.json` exceed **4**, pause the main loop and spawn a cleanup sub-agent to
address them before continuing or escalate to the user for intervention/instructions.

**e) Report progress:**

- After each completed task, briefly log progress (e.g., "✓ TASK-auth-003 done. 6/12 complete.").

### 3. Stopping conditions

Stop the loop when any of these conditions is met:

1. **All tasks are `"done"`.** The feature is complete.
2. **All remaining tasks are `"blocked"`.** Human intervention is needed.
3. **A sub-agent fails catastrophically** (crashes, produces no output, or leaves the
   codebase in a broken state). Stop and report.
4. **The user intervenes.** If running interactively, the user can stop the loop at any time.

### 4. Finalize

When the loop ends:

- Read the final state of `issues.json` and `progress-log.md`.
- Update `docs/index.json`:
  - If all tasks are done, set feature status to `"done"`.
  - If stopped due to blockers, leave status as `"in_progress"`.
- Provide a final summary to the user:
  - Total tasks completed in this run.
  - Any tasks that were blocked and why.
  - Any new tasks that were discovered during implementation.
  - Overall feature status.

## Important notes

- **Strictly sequential.** Run one sub-agent at a time. The next sub-agent needs to see the
  file changes and state updates from the previous one. Parallelism is a future extension
  that would require a locking/merge strategy for `issues.json` and `progress-log.md`.

- **You are the orchestrator, not the implementer.** Do not write code, run tests, or modify
  files yourself (except `docs/index.json`). All implementation work happens inside sub-agents.

- **Fresh reads each iteration.** Always re-read `issues.json` at the start of each loop
  iteration. The sub-agent may have added new tasks, changed priorities, or updated
  dependencies.

- **Respect the stopping conditions.** Don't retry blocked tasks — they're blocked for a
  reason. Don't force through failures. The progress log and issue statuses are the
  communication channel — keep them accurate.

- **Sub-agent isolation.** Each sub-agent should be treated as a fresh, stateless session.
  Pass it all the context it needs (feature name, task ID, docs path) — don't assume it
  has memory of previous iterations.

- **Keep the user informed.** Even in automated mode, progress visibility matters. Report
  after each task so the user can monitor and intervene if needed.

- **File size guard.** After each completed task, check whether `issues.json` has ≥ 15 done
  tasks or `progress-log.md` exceeds 300 lines. If either threshold is hit, notify the user
  and suggest running `/simple-cleanup` before continuing. Do not hard-stop the loop — the
  user decides whether to clean up now or later.
