# Architecture: [App Name]

<!--
Living, app-level document. High-level only — the MAP, not the territory.
Reconciled against code, not copied from feature docs. Target ~200 lines.
Do NOT put here: file/function-level detail, code blocks (except ≤3-line signatures),
anything cheaper to read from the code. Point to where it lives instead.
-->

## Overview
[2-4 sentences: what kind of system this is and the one or two ideas that explain
how it's organized.]

## Major components
[The handful of parts a new engineer must know to navigate. One or two lines each —
responsibility, and where it lives in the tree (not every file).]

- **[Component / module]** (`path/to/dir/`) — [single responsibility]
- **[Component / module]** (`path/to/dir/`) — [single responsibility]

## How they fit together
[How the components communicate at a system level: request flow, events, queues,
shared stores. A short ordered walkthrough of the main path is ideal.]

1. [Step]
2. [Step]
3. [Step]

## Cross-cutting patterns
[Conventions that apply across components: auth/authz, error handling, config,
logging, validation. One line each, pointing to where the pattern is defined.]

- **[Concern]** — [the pattern, and where it's established in code]
- **[Concern]** — [the pattern, and where it's established in code]

## External dependencies
[Major external systems and services this app relies on, and what each is used for.]

- **[Service / system]** — [what it's used for]

## Stable contracts
[Only the public contracts and compatibility guarantees worth stating at a high level —
not an endpoint listing. Omit this section if there are none.]

- [Contract / guarantee]

## Related ADRs
[Links to the ADRs that explain the *why* behind the structure above.]

- [ADR-0001: title](adr/0001-title.md)
