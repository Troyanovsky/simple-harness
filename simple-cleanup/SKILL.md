---
name: simple-cleanup
description: >
  Compact oversized feature artifacts (issues.json and progress-log.md) to keep them concise and
  useful for future agents and developers. Use this skill when issues.json has 15 or more done
  tasks, or progress-log.md exceeds 300 lines. Trigger on phrases like "clean up the issues",
  "compact the progress log", "archive done tasks", "the files are getting too long", or when
  simple-implement or simple-run indicates that a file size threshold has been reached.
disable-model-invocation: true
---

# Simple Cleanup — Compact Feature Artifacts

You compact oversized `issues.json` and `progress-log.md` files for a feature. This keeps them
concise and useful without losing history. Nothing is ever deleted — only archived or summarized,
and always with explicit user approval.

## Folder convention

```
docs/
  <feature-name>/
    issues.json           ← compacted in-place; archived task payloads moved out
    issues-archive.json   ← created/appended by this skill
    progress-log.md       ← older entries replaced with a summary block
```

## Triggers

| File | Threshold |
|---|---|
| `issues.json` | `"done"` task count ≥ 15 |
| `progress-log.md` | total line count > 300 |

Both files are checked and compacted in a single run when both thresholds are met.

## Workflow

### 1. Orient

- Identify the feature. If not specified, check `docs/index.json` for features with status
  `"in_progress"`.
- Read `issues.json` and `progress-log.md` in full.
- Count done tasks and total log lines. Report to the user:
  - e.g. "Feature `auth`: 18 done tasks in issues.json, progress-log.md is 340 lines.
    Both thresholds exceeded."
- Ask the user which files to compact. They may approve one, both, or neither.

### 2. Compact `issues.json`

**Identify archivable tasks:**

A done task is archivable when **no** `"todo"` or `"in_progress"` task lists its ID in
`depends_on`. If any live task depends on it, keep it in `issues.json` as-is so dependency
resolution continues to work.

**Archive:**

- Append the full archivable task objects to `issues-archive.json` (create the file as a JSON
  array if it doesn't exist; append to the existing array if it does — never overwrite).
- In `issues.json`, replace each archived task's entry with a stub:

```json
{
  "id": "TASK-auth-001",
  "title": "Add user model",
  "status": "done",
  "archived": true,
  "archive_ref": "issues-archive.json"
}
```

- All non-archivable tasks (active, blocked, or depended-upon done tasks) remain exactly as-is.

**Show the user a preview** before writing: how many tasks will be archived and approximately
how many lines will be saved.

### 3. Compact `progress-log.md`

**Identify compactable entries:**

Keep the most recent entries intact — default is the last 10 tasks. Everything older is
compactable. Ask the user if they want to adjust the cutoff.

**Replace compacted entries** with a single summary block, inserted after the file header and
before the first kept entry:

```markdown
## [Compacted — TASK-auth-001 through TASK-auth-012]
- **Date compacted**: YYYY-MM-DD
- **Tasks covered**: 12 tasks (TASK-auth-001 to TASK-auth-012)
- **Summary**: 2-3 sentences covering what was accomplished, key decisions made, and anything
  a future agent needs to know. Skip narration of what changed — that's in git.
```

The summary should preserve cross-cutting decisions or patterns established in the compacted
range that are still relevant to remaining work. If nothing carries forward, a single sentence
is enough.

**Show the user a preview** (the summary block you plan to write + the line count reduction)
before writing.

### 4. Write changes

Write only after explicit user approval for each file. The user may approve one but not the
other — respect that.

### 5. Report

Confirm what was done:
- e.g. "Archived 16 tasks to issues-archive.json (issues.json reduced from 48 to 32 entries).
  progress-log.md reduced from 340 to 138 lines."
- Note any tasks intentionally kept in full (e.g. "TASK-auth-003 kept because TASK-auth-019
  depends on it").

## Important notes

- **Never delete.** Archive task payloads to `issues-archive.json`; compact log entries to a
  summary block. History is always recoverable.
- **Stub integrity.** Stubs must preserve `id` and `status` so that `depends_on` resolution
  in simple-implement and simple-run continues to work correctly.
- **issues-archive.json is append-only.** Append to the existing array on subsequent runs.
- **User approval is mandatory.** This skill must never be invoked in automated mode without
  surfacing the compaction decision to the user first.
- **Don't compact what's still relevant.** If an older log entry established a constraint or
  decision that remaining tasks will need, include it in the summary rather than dropping it.
