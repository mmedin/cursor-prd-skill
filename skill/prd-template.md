# PRD Template

Use this template to generate `docs/prd.md`. Replace all `[bracketed]` placeholders with project-specific content. Remove any sections that don't apply, but justify the removal with a brief note.

---

```markdown
# [Product Name] — Product Requirements Document

**Version:** 1.0
**Date:** [YYYY-MM-DD]
**Status:** Draft | In Review | Approved
**Author:** [Name/Role]

## 1. Product Overview

### 1.1 Vision Statement
[One paragraph describing the product vision. What world does this product create?]

### 1.2 Problem Statement
[What specific problem does this product solve? For whom? What is the cost of the problem remaining unsolved?]

### 1.3 Target Users

| Persona | Description | Primary Need |
|---------|-------------|--------------|
| [Persona 1] | [Brief description] | [What they need most] |
| [Persona 2] | [Brief description] | [What they need most] |

### 1.4 Value Proposition
[What makes this product uniquely valuable? How does it differ from alternatives?]

## 2. Success Metrics

| Metric | Target | Measurement Method |
|--------|--------|--------------------|
| [e.g. User activation rate] | [e.g. 40% in first month] | [e.g. Analytics event tracking] |
| [e.g. Task completion rate] | [e.g. > 90%] | [e.g. Funnel analysis] |

## 3. Scope

### 3.1 In Scope
[Bulleted list of what this PRD covers]

### 3.2 Out of Scope
[Bulleted list of what is explicitly excluded. This is critical for agent focus.]

### 3.3 Assumptions
[List assumptions the requirements depend on]

### 3.4 Dependencies
[External systems, APIs, services, or teams this product depends on]

## 4. Features

### F-001: [Feature Name]
- **Priority:** Must Have | Should Have | Nice to Have
- **Description:** [What the feature does]
- **User Benefit:** [Why it matters to the user]
- **Acceptance Criteria:**
  - [ ] [Specific, verifiable criterion]
  - [ ] [Specific, verifiable criterion]
- **Constraints:**
  - [Any business rules, limits, or guardrails]
- **Related Stories:** US-001, US-002

### F-002: [Feature Name]
[Same structure as above. Repeat for each feature.]

## 5. Non-Functional Requirements

### 5.1 Performance
- [e.g. Page load time < 2s on 3G connection]
- [e.g. API response time < 200ms p95]

### 5.2 Security
- [e.g. All data encrypted at rest and in transit]
- [e.g. OWASP Top 10 compliance]

### 5.3 Accessibility
- [e.g. WCAG 2.1 AA compliance]

### 5.4 Scalability
- [e.g. Support 10K concurrent users at launch]

### 5.5 Reliability
- [e.g. 99.9% uptime SLA]

## 6. Constraints & Guardrails

[Business rules that the AI agent must enforce during implementation. Be explicit.]

- [e.g. A user cannot perform action X more than N times per day]
- [e.g. All monetary values must be stored in cents as integers]
- [e.g. PII must never be logged]

## 7. Open Questions

| # | Question | Impact | Owner |
|---|----------|--------|-------|
| 1 | [Question] | [What's blocked if unresolved] | [Who decides] |

## 8. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | [Author] | Initial draft |
```
