# Epics & Tasks Template

Use this template to generate `docs/epics-and-tasks.md`. This is the document most directly consumed by Cursor's AI agent. Each task must be granular enough for an agent to complete in a single session.

---

```markdown
# [Product Name] — Epics & Tasks

**Version:** 1.0
**Date:** [YYYY-MM-DD]
**Related PRD:** [prd.md](prd.md)
**Related Stories:** [user-stories.md](user-stories.md)

## How to Use This Document

Each epic groups related work. Each task within an epic is designed to be
executed by an AI coding agent in a single session. Tasks are ordered by
dependency — complete them top to bottom within each epic.

**Task statuses:** Not Started | In Progress | Done | Blocked

## Epic Overview

| Epic ID | Name | Stories | Task Count | Priority |
|---------|------|---------|------------|----------|
| EP-001 | [Epic Name] | US-001, US-002 | [N] | Must Have |
| EP-002 | [Epic Name] | US-003, US-004 | [N] | Must Have |
| EP-003 | [Epic Name] | US-005 | [N] | Should Have |

## Recommended Execution Order

List epics in the order they should be implemented, considering dependencies:

1. **EP-001** — [Reason: foundational, no dependencies]
2. **EP-002** — [Reason: depends on EP-001 data model]
3. **EP-003** — [Reason: depends on EP-001 and EP-002]

---

## EP-001: [Epic Name]

**Goal:** [One sentence describing what this epic achieves]
**Stories:** US-001, US-002
**Depends on:** None | EP-XXX

### T-001: [Task Title]
- **Story:** US-001
- **Status:** Not Started
- **Description:** [Precise description of what to build or change]
- **Definition of Done:**
  - [ ] [Specific, verifiable outcome]
  - [ ] [Specific, verifiable outcome]
  - [ ] [Tests pass / linter clean / build succeeds]
- **Agent Instructions:**
  - [Specific constraints or patterns the agent must follow]
  - [Files or directories to create/modify]
  - [What NOT to do]
- **Depends on:** None | T-XXX

### T-002: [Task Title]
- **Story:** US-001
- **Status:** Not Started
- **Description:** [Precise description]
- **Definition of Done:**
  - [ ] [Outcome]
  - [ ] [Outcome]
- **Agent Instructions:**
  - [Instructions]
- **Depends on:** T-001

---

## EP-002: [Epic Name]

[Same structure. Repeat for each epic.]

---

## Dependency Graph

Visualize task dependencies for complex projects:

```mermaid
graph TD
    T-001 --> T-002
    T-001 --> T-003
    T-002 --> T-004
    T-003 --> T-004
    T-004 --> T-005
```

## Progress Tracker

| Task | Epic | Status | Depends On |
|------|------|--------|------------|
| T-001 | EP-001 | Not Started | — |
| T-002 | EP-001 | Not Started | T-001 |
| T-003 | EP-002 | Not Started | T-001 |
```

## Guidelines for Writing Tasks

- **One concern per task.** A task should touch a focused area of the codebase.
- **Include Agent Instructions.** These are direct instructions for the AI agent: what patterns to follow, what files to touch, what to avoid.
- **Definition of Done must be verifiable.** Prefer automated checks: "tests pass", "linter reports zero errors", "build succeeds", "endpoint returns 200".
- **Order by dependency.** The agent should be able to pick up tasks top to bottom without needing to jump around.
- **Size tasks for a single agent session.** If a task would take a human more than 2-4 hours, break it down further.
- **Never leave implicit context.** The agent has no memory of previous tasks. Each task must be self-contained with enough context to execute.
