# Design: [Feature / Change Name]

## Summary / Technical approach
[1-3 short paragraphs.
Describe the implementation approach at a high level.
What is the main technical strategy?
Is this an extension, refactor, replacement, migration, or integration?]

## Goals
- [Goal]
- [Goal]
- [Goal]

## Non-goals
- [Explicitly not solving]
- [Explicitly not changing]
- [Explicitly not optimizing]

## Current technical state
[Describe the relevant existing architecture and behavior.]

### Relevant components
- [Component / service / module]
- [Component / service / module]
- [Component / service / module]

### Current data flow
1. [Step]
2. [Step]
3. [Step]

### Current interfaces / contracts
- [API / event / schema / shared type]
- [API / event / schema / shared type]

### Current storage model
- [Table / collection / cache / file / queue]
- [Table / collection / cache / file / queue]

### Known technical constraints / debt
- [Constraint or debt]
- [Constraint or debt]

## Proposed technical state
[Describe how the system should work after the change.]

### Planned changes
- [Change]
- [Change]
- [Change]

### Proposed data flow
1. [Step]
2. [Step]
3. [Step]

### New or modified interfaces / contracts
- [New/changed API / event / schema / shared type]
- [New/changed API / event / schema / shared type]

### Storage / schema changes
- [Migration or schema change]
- [Persistence change]
- [Index / cache / queue change]

### Backward compatibility / migration approach
- [How compatibility is preserved]
- [How rollout or migration works]
- [Any temporary dual-write / fallback / feature flag behavior]

## Key decisions

### D-1: [Decision title]
**Decision:** [What was chosen]  
**Reason:** [Why this was chosen]  
**Alternatives considered:** [Other options and why they were not chosen]

### D-2: [Decision title]
**Decision:** [What was chosen]  
**Reason:** [Why this was chosen]  
**Alternatives considered:** [Other options and why they were not chosen]

### D-3: [Decision title]
**Decision:** [What was chosen]  
**Reason:** [Why this was chosen]  
**Alternatives considered:** [Other options and why they were not chosen]

## Risks / trade-offs
- [Risk or trade-off]
  - Mitigation: [Mitigation]
- [Risk or trade-off]
  - Mitigation: [Mitigation]
- [Risk or trade-off]
  - Mitigation: [Mitigation]

## Affected components
- [Code path / service / module / package / UI surface]
- [Code path / service / module / package / UI surface]
- [Code path / service / module / package / UI surface]

## Testing strategy

### Unit tests
- [What logic should be covered]
- [What logic should be covered]

### Integration tests
- [What interaction should be covered]
- [What interaction should be covered]

### End-to-end / manual validation
- [What user path or system flow should be validated]
- [What user path or system flow should be validated]

## Observability / rollout
[Optional but useful for non-trivial changes.]

### Logging / metrics / alerts
- [Metric / log / alert]
- [Metric / log / alert]

### Rollout plan
- [Feature flag / staged rollout / migration step]
- [Rollback plan]