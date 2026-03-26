# Simple Harness

A minimal skill set for AI coding agents (and humans) to plan, break down, and implement features through structured documents and a stateless execution loop.

## The Problem

AI coding agents are stateless. Each invocation starts from scratch — no memory of what happened before, no understanding of the broader feature context, no awareness of what's already been decided. When features span multiple sessions or multiple agents, context is lost, decisions are revisited, and work is duplicated.

## The Approach

Simple Harness solves this with **documents as state**. Instead of relying on conversation history or agent memory, all planning decisions, task breakdowns, and implementation progress are persisted as files in a `docs/<feature-name>/` directory. Any agent — at any time — can read these files and pick up exactly where the last one left off.

This is a form of **harness engineering**: decomposing complex work into specialized phases, with structured artifacts as the handoff mechanism between them. The approach draws from similar principles described in [Anthropic's effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) and [OpenAI's harness engineering](https://openai.com/index/harness-engineering/).

## Design Principles

**Documents are the contract.** Every skill reads from and writes to well-defined files. The feature name is the linking key. An agent doesn't need memory — it needs a folder.

**Each skill has one job.** Spec defines *what*. Design defines *how*. Tasks define *in what order*. Implement does *one unit of work*. Run *loops*. No skill crosses into another's lane.

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

## Folder Convention

```
docs/
  index.json                ← manifest of all features and their status
  visual.md                 ← how the app looks (app-level, optional)
  <feature-name>/
    spec.md                 ← what and why
    design.md               ← how, technically
    issues.json             ← ordered task queue
    progress-log.md         ← append-only implementation journal
```

The `<feature-name>` is a short kebab-case token (e.g., `auth`, `csv-export`, `workspace-sharing`) that ties all artifacts together by directory.

## Workflow

```
simple-spec → simple-design → simple-visual (optional) → simple-tasks → simple-implement / simple-run
```

### Example A: Manual implementation

1. Run **simple-spec** to write `docs/auth/spec.md`
2. Run **simple-design** to write `docs/auth/design.md`
3. Run **simple-tasks** to write `docs/auth/issues.json`
4. Run **simple-implement** — agent picks task #1, implements it, updates issues + progress log
5. Run **simple-implement** again — agent picks task #2, reads progress log, continues
6. Repeat until all tasks are done

### Example B: Automated implementation

1. Run **simple-spec** to write `docs/auth/spec.md`
2. Run **simple-design** to write `docs/auth/design.md`
3. Run **simple-tasks** to write `docs/auth/issues.json`
4. Run **simple-run** — orchestrator loops through all tasks automatically, spawning a sub-agent for each one, until the feature is complete or a blocker is hit

## References

The harness pattern — decomposing agent work into specialized phases connected by structured artifacts — is explored in depth by both Anthropic and OpenAI:

- [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) — Anthropic's guide to enabling agents to work across multiple context windows using an initializer agent that sets up structured state and progress tracking, and a coding agent that makes incremental progress from that foundation.
- [Harness engineering](https://openai.com/index/harness-engineering/) — OpenAI's take on orchestrating multi-agent systems for complex, extended tasks.
