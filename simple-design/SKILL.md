---
name: simple-design
description: >
  Write a technical design document (docs/<feature-name>/design.md) describing how to build a
  feature — architecture, data flow, interfaces, key decisions, testing strategy — from a spec,
  user message, or codebase context. Use when the user wants to design a feature, plan an
  implementation approach, document architecture decisions, or turn a spec.md into its design.
  Triggers: "write a design doc", "design this feature", "plan the implementation", "how should
  we build this", "technical approach", "architecture for".
disable-model-invocation: true
---

# Simple Design — Write a Technical Design Document

You are writing a technical design document. Your job is to produce a `design.md` file inside `docs/<feature-name>/`
that gives an AI coding agent (or a human developer) a clear, actionable plan for **how** to implement
a feature — the architecture, data flow, interfaces, key decisions, and testing strategy.

This skill works standalone from a user's message or any input file that describes what needs to be built.
It also pairs well with **simple-spec** — consuming `docs/<feature-name>/spec.md` as input gives the
best results, but it's not required.

## Folder convention

All feature artifacts live in `docs/<feature-name>/`:

```
docs/
  index.json              ← feature manifest
  visual.md               ← from simple-visual (app-level, optional)
  <feature-name>/
    spec.md               ← from simple-spec
    design.md             ← THIS SKILL'S OUTPUT
    design/               ← optional: detail files when design.md is split
      <area>.md
    issues.json           ← from simple-tasks
    progress-log.md       ← from simple-implement
```

The `<feature-name>` token is a short kebab-case identifier that ties all artifacts together by directory.

## Workflow

### 1. Gather context

Start by collecting as much information as you can **before** asking the user anything.

**From the input:**
- If the user points to a feature folder (e.g., `docs/<feature-name>/`), read `spec.md` in that folder.
  This is your primary source of truth for *what* to build and *why*.
- If no feature folder is specified, check `docs/index.json` for features with status `"planning"` or
  `"spec_ready"` and look for a `spec.md` in their directory.
- If there's no spec file, treat the user's message or input file as the feature description and extract
  the goals, scope, and requirements from it.

**From the codebase:**
- This is where the design skill does its heaviest lifting. You need to understand the existing system
  deeply enough to propose changes that fit naturally.
- **Durable docs first (if present):** read `docs/architecture.md` for the high-level system shape
  and `docs/adr/` for past decisions and their rationale. These tell you *why* the system is the way
  it is and stop you from re-litigating settled decisions — reconcile any high-level claim against
  the code, which remains the source of truth for detail.
- Search for and read:
  - **Architecture:** Directory structure, module boundaries, how the app is organized.
  - **Data layer:** Database schemas/models, ORMs, migrations, data access patterns.
  - **API layer:** Route definitions, controllers, middleware, request/response shapes.
  - **Business logic:** Services, domain models, shared utilities relevant to the feature area.
  - **UI layer** (if applicable): Component structure, state management, routing.
  - **Tests:** Existing test patterns, test utilities, what's covered and what's not.
  - **Config & infra:** Environment variables, feature flags, deployment setup, CI/CD.
- Look for patterns the codebase already uses — the design should follow existing conventions unless
  there's a strong reason to deviate (and if so, document that as a key decision).

The goal is to understand the system well enough that your design reads like it was written by someone
who works on this codebase daily.

### 2. Ask follow-up questions (only if needed)

After gathering context, assess what's still ambiguous. Common gaps at the design stage:

- **Technical constraints:** "I see you're on Postgres 14 — are you open to using native JSON columns,
  or do you prefer a normalized schema?"
- **Integration boundaries:** "The spec mentions a webhook — should this integrate with the existing
  event system in `lib/events/`, or is this a separate concern?"
- **Performance expectations:** "The current query for listing workspaces does a full table scan.
  Should the design include indexing, or is the dataset small enough that it doesn't matter?"
- **Migration/rollout concerns:** "There are ~50k rows in the users table. Should we plan for a
  zero-downtime migration, or is a maintenance window acceptable?"

Only ask questions where the answer changes the design meaningfully. If the codebase gives you enough
signal to make a reasonable choice, make it and document it as a key decision with your rationale.

Present your questions to the user — keep it to one round of 1-4 focused questions.
Use whatever mechanism is available (a structured question tool, a chat message, etc.).

### 3. Write the design

Read the template at `references/design_template.md` in this skill's directory. Use it as the structural
backbone for your output. Replace all bracketed placeholder text with real content.

**Key principles:**

- **Ground everything in the actual codebase.** Reference real file paths, real function names, real
  table names. "We'll add a new service" is vague. "We'll add `src/services/workspace-sharing.ts`
  following the pattern established by `src/services/workspace.ts`" is actionable.

- **Current state is your foundation.** The "Current technical state" section should read like a guided
  tour of the relevant parts of the codebase. An agent reading this should be able to navigate to
  every file that matters without searching.

- **Proposed state is your blueprint.** The "Proposed technical state" section is where you describe
  what changes. Be explicit about new files, modified files, new database columns, new API endpoints,
  new types/interfaces. The agent should be able to start coding from this section.

- **Specify interfaces, not implementations.** Include type/interface definitions, function signatures,
  schema diffs, endpoint request/response shapes, and file-level structure. Do *not* include full
  function bodies or large code blocks — leave the implementation to the coding agent. If you find
  yourself writing more than ~10 lines of logic inside a code block, you've crossed into implementation
  territory; pull back and describe the behavior in prose or pseudocode instead.

- **Key decisions need real alternatives.** For each decision, give what you chose, why, and what
  you considered instead — this stops the implementing agent from second-guessing the approach.
  Good alternatives are ones a reasonable developer would actually consider, not strawmen. These
  decisions are also the source **simple-distill** draws on to write durable ADRs once the feature
  ships, so capture the cross-cutting or hard-to-reverse ones clearly.

- **Data flow tells the story.** The current and proposed data flow sections are often the most valuable
  parts of the design. Walk through a concrete request end-to-end: "User clicks Share → frontend calls
  POST /api/workspaces/:id/share → controller validates → service creates sharing record → sends email
  notification → returns 201." This is what makes the design implementable.

- **Testing strategy should be specific.** Don't just say "unit test the service." Say which behaviors
  to test, what the tricky cases are, and what existing test utilities to use. The agent should be able
  to write tests from your descriptions.

- **Backward compatibility is non-negotiable unless stated otherwise.** If the spec doesn't mention
  breaking changes, assume they're not acceptable. Document how you preserve compatibility — migration
  scripts, feature flags, fallback behavior, dual-write periods.

- **Write concisely.** Use precise, economical language. Prefer short declarative sentences,
  bullet lists, tables, and signatures over long prose paragraphs. Cut redundant wording, filler,
  and restatement — every sentence should give the implementing agent information it doesn't
  already have. A shorter design that says the same thing is a better design.

**Splitting a long design**

A design document should stay navigable — a reader, human or agent, should be able to load
`design.md` and grasp the whole feature without drowning in detail.

When the document grows past **~400 lines**, stop and check whether it has natural seams:
distinct subsystems, pages, components, or API surfaces, each with enough detail to stand on
its own. If it does, split it. If it's one cohesive change that's simply long, leave it as a
single file — splitting cohesive content only adds indirection. The line count is a prompt to
check, not a hard rule; the real trigger is "this covers multiple independently-implementable
areas, each with substantial depth."

When you split:

- Keep `design.md` as the parent and overview, readable end-to-end. It holds the summary and
  technical approach, goals and non-goals, current technical state, cross-cutting key decisions,
  end-to-end data flow, risks, and the testing-strategy summary. Target ~150-300 lines.
- Move per-area depth into `docs/<feature-name>/design/<area>.md` — one file per subsystem,
  page, or component, named in kebab-case. Each child holds that area's detailed interfaces,
  schema diffs, endpoint shapes, and area-specific test cases.
- Add an **index of the child files** to `design.md`: a short list, each entry naming the file
  and summarizing what it covers in one line. Without this map, the split isn't navigable.
- Start each child file with a one-line link back to `design.md` so it's never read fully out
  of context.

### 4. Save the output

- Use the same feature name from the spec (or derive a short kebab-case name if working standalone).
- Save to `docs/<feature-name>/design.md` relative to the project root (create the directory if needed).
- If you split the design, save the child files under `docs/<feature-name>/design/` and make sure
  `design.md` indexes them. Otherwise a single `design.md` is the complete output.
- Update `docs/index.json` to set the feature status to `"design_ready"` (or create the entry if
  working standalone).
- Tell the user the file path and give a brief summary of the technical approach.

### 5. Suggest next step

Let the user know the design is ready. The typical next step is to run **simple-tasks** to break the
design into an ordered task list (`docs/<feature-name>/issues.json`), then use **simple-implement**
or **simple-run** to execute. If there are open questions or risks flagged in the document, highlight
them so the user can address them before moving to task breakdown.

## Important notes

- This skill produces a **technical design**, not a product spec — focus on *how*, not *what* or
  *why* (the spec covers those). If you find yourself writing user stories or acceptance
  criteria, you've crossed into spec territory; pull back.
- Scale the design to the change: a simple endpoint addition doesn't need a 5-page design; a
  migration affecting millions of rows does.
- If the spec has open questions that affect the design, call them out explicitly rather than
  guessing — e.g. "this design assumes X; if Y holds instead, section Z would need to change."
- If no spec exists and the input is vague, consider suggesting a spec be written first (via
  **simple-spec** or manually) — designing against ambiguous goals leads to rework. This is a
  suggestion, not a blocker; proceed with what you have if the user wants to.
