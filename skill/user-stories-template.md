# User Stories Template

Use this template to generate `docs/user-stories.md`. Each story must trace back to a feature in `prd.md` via its Feature ID. Write acceptance criteria as verifiable checkboxes that an AI agent or automated test can validate.

---

```markdown
# [Product Name] — User Stories

**Version:** 1.0
**Date:** [YYYY-MM-DD]
**Related PRD:** [prd.md](prd.md)

## Story Format

Each story follows this structure:

> **US-[NNN]**: As a [persona], I want to [action] so that [benefit].

## Stories by Feature

### Feature: F-001 — [Feature Name]

#### US-001: [Short Title]
**As a** [persona],
**I want to** [specific action],
**So that** [measurable benefit].

**Priority:** Must Have | Should Have | Nice to Have
**Complexity:** S | M | L | XL

**Acceptance Criteria:**
- [ ] [Given/When/Then or specific verifiable condition]
- [ ] [Given/When/Then or specific verifiable condition]
- [ ] [Given/When/Then or specific verifiable condition]

**Edge Cases:**
- [What happens when X?]
- [What happens if Y fails?]

**Constraints:**
- [Business rules that apply to this story]

---

#### US-002: [Short Title]
[Same structure. Repeat for each story within the feature.]

---

### Feature: F-002 — [Feature Name]

#### US-003: [Short Title]
[Same structure. Continue for all features.]

---

## Story Map Summary

Provide a summary table for quick reference:

| Story ID | Title | Feature | Priority | Complexity | Epic |
|----------|-------|---------|----------|------------|------|
| US-001 | [Title] | F-001 | Must Have | M | EP-001 |
| US-002 | [Title] | F-001 | Should Have | S | EP-001 |
| US-003 | [Title] | F-002 | Must Have | L | EP-002 |
```

## Guidelines for Writing Stories

- **One action per story.** If a story has "and" in the action, split it.
- **Avoid implementation details.** Stories describe WHAT, not HOW.
- **Acceptance criteria must be binary.** Passed or failed, no ambiguity.
- **Use Given/When/Then** for complex workflows:
  - Given [precondition]
  - When [action]
  - Then [observable result]
- **Estimate complexity** relative to the project, not absolute hours:
  - **S**: Single file change, straightforward logic
  - **M**: Multiple files, some integration
  - **L**: Cross-cutting concern, significant logic
  - **XL**: Architectural decision, multiple subsystems
