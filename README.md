# Simple Harness

A minimal skill set for AI coding agents (and humans) to plan, break down, and implement features through structured documents and a stateless execution loop.

## The Problem

AI coding agents are stateless. Each invocation starts from scratch — no memory of what happened before, no understanding of the broader feature context, no awareness of what's already been decided. When features span multiple sessions or multiple agents, context is lost, decisions are revisited, and work is duplicated.

## The Approach

Simple Harness solves this with **documents as state**. Instead of relying on conversation history or agent memory, all planning decisions, task breakdowns, and implementation progress are persisted as files in a `docs/<feature-name>/` directory. Any agent — at any time — can read these files and pick up exactly where the last one left off.

This is a form of **harness engineering**: decomposing complex work into specialized phases, with structured artifacts as the handoff mechanism between them. The approach draws from similar principles described in [Anthropic's effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) and [OpenAI's harness engineering](https://openai.com/index/harness-engineering/).

## Design Principles

**Documents are the contract.** Every skill reads from and writes to well-defined files. The feature name is the linking key. An agent doesn't need memory — it needs a folder.

**Each skill has one job.** Spec defines *what*. Design defines *how*. Tasks define *in what order*. Implement does *one unit of work*. Run *loops*. Simplify *audits completed work*. Cleanup *compacts*. Distill *promotes what lasts*. No skill crosses into another's lane.

**Stateless by design.** Every agent invocation is a fresh session. It reads the current state from disk, does its work, writes the updated state back, and exits. The next agent resumes from the files.

**Proportional depth.** A small feature gets a brief spec, a short design, and 3 tasks. A large feature gets detailed documents and 20 tasks. The skills scale to the work, not the other way around.

## Skills

| Skill | Purpose | Input | Output |
|---|---|---|---|
| **simple-spec** | Define what to build and why | User's idea + codebase | `docs/<feature>/spec.md` |
| **simple-design** | Define how to build it | `spec.md` + codebase | `docs/<feature>/design.md` |
| **simple-visual** | Define how it looks *(app-level, optional)* | Codebase / user brief | `docs/visual.md` |
| **simple-tasks** | Break down into ordered tasks | `spec.md` + `design.md` | `docs/<feature>/issues.json` |
| **simple-implement** | Execute one task from the queue | All docs + codebase | Code changes + state updates |
| **simple-run** | Orchestrate the full loop | `issues.json` | Delegates to simple-implement |
| **simple-simplify** | Audit completed work for unnecessary complexity *(optional)* | Implementation diff + feature docs | Simplification proposals |
| **simple-cleanup** | Compact oversized feature artifacts *(maintenance)* | `issues.json` + `progress-log.md` | Compacted files + `issues-archive.json` |
| **simple-distill** | Promote durable knowledge, then archive the feature *(after completion)* | Completed feature docs + codebase | `docs/architecture.md`, `docs/capabilities.md`, `docs/adr/`, `docs/CHANGELOG.md` |

## Folder Convention

```
docs/
  index.json                ← manifest of all features and their status
  visual.md                 ← how the app looks (app-level, durable, optional)
  architecture.md           ← how the app is built, high-level (app-level, durable)
  capabilities.md           ← what the app does, high-level (app-level, durable)
  CHANGELOG.md              ← one entry per distilled feature (append-only)
  adr/                      ← architecture decision records, one per file (immutable)
  archive/
    <feature-name>/         ← retired feature folders (moved here by simple-distill)
  <feature-name>/
    spec.md                 ← what and why
    spec/                   ← optional detail files (when spec.md is split)
    design.md               ← how, technically
    design/                 ← optional detail files (when design.md is split)
    issues.json             ← ordered task queue
    progress-log.md         ← append-only implementation journal
```

The `<feature-name>` is a short kebab-case token (e.g., `auth`, `csv-export`, `workspace-sharing`) that ties all artifacts together by directory.

The **app-level durable docs** (`visual.md`, `architecture.md`, `capabilities.md`, `CHANGELOG.md`, `adr/`) are the slow-changing, high-level layer that outlives any single feature. Feature folders record a *delta*; the durable docs record the *current shape and why*. The code remains the source of truth for detail — durable docs stay high-level on purpose and point to the code rather than duplicating it.

## Workflow

```
simple-spec → simple-design → simple-visual (optional) → simple-tasks → simple-implement / simple-run → simple-simplify (optional) → simple-distill
```

### Example A: Manual implementation

1. Run **simple-spec** to write `docs/auth/spec.md`
2. Run **simple-design** to write `docs/auth/design.md`
3. Run **simple-tasks** to write `docs/auth/issues.json`
4. Run **simple-implement** — agent picks task #1, implements it, updates issues + progress log
5. Run **simple-implement** again — agent picks task #2, reads progress log, continues
6. Repeat until all tasks are done
7. Optionally run **simple-simplify** — audit the completed implementation for unnecessary complexity
8. Run **simple-distill** — promote the feature's durable decisions and behavior into app-level docs, then archive the feature folder

### Example B: Automated implementation

1. Run **simple-spec** to write `docs/auth/spec.md`
2. Run **simple-design** to write `docs/auth/design.md`
3. Run **simple-tasks** to write `docs/auth/issues.json`
4. Run **simple-run** — orchestrator loops through all tasks automatically, spawning a sub-agent for each one, until the feature is complete or a blocker is hit
5. Optionally run **simple-simplify** — audit the completed implementation for unnecessary complexity
6. Run **simple-distill** — promote its durable decisions and behavior into app-level docs, then archive the feature folder

## References

The harness pattern — decomposing agent work into specialized phases connected by structured artifacts — is explored in depth by both Anthropic and OpenAI:

- [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) — Anthropic's guide to enabling agents to work across multiple context windows using an initializer agent that sets up structured state and progress tracking, and a coding agent that makes incremental progress from that foundation.
- [Harness engineering](https://openai.com/index/harness-engineering/) — OpenAI's take on orchestrating multi-agent systems for complex, extended tasks.
