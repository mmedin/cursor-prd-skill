---
name: prd-writer
description: Generate structured PRD document systems optimized for AI coding agents. Creates interconnected docs (PRD, technical specs, user stories, epics/tasks) in docs/. Use when starting a new project, writing a PRD, defining product requirements, or planning a greenfield product for agent-driven development.
---

# PRD Writer for AI Agents

Generates a system of interconnected product requirement documents optimized for consumption by AI coding agents (IDE assistants, CLI coding agents, or similar with repository context). Targets Product Managers defining greenfield projects.

## Workflow

Follow this hybrid interaction model:

### Phase 1: Discovery

The discovery process has two paths depending on whether the user provides an input document.

#### Required Information

Regardless of path, the following 7 fields must be resolved before moving to Phase 2:

1. **Product vision**: What is the product? What problem does it solve?
2. **Target users**: Who are the primary and secondary users?
3. **Core value proposition**: What's the unique differentiator?
4. **Scope boundaries**: What is explicitly OUT of scope?
5. **Success metrics**: How will success be measured?
6. **Known constraints**: Regulatory, platform, integration, timeline, budget constraints.
7. **Key user journeys**: The 3-5 most critical workflows.

#### Reference architecture (default)

Before Phase 2, confirm whether the product fits the skill’s **default high-level topology** (no specific frameworks or vendors):

- **Presentation**: user-facing client (e.g. web UI, mobile app).
- **Backend**: application logic behind a **REST over HTTP** API (resources, verbs, status codes as the default contract style).
- **Persistence**: a **database** (or equivalent durable store) for authoritative data.
- **Integrations**: optional **outbound** calls to third-party services (payments, email, identity, etc.).

**Escape hatch:** If the product does *not* fit this model (e.g. CLI-only, GraphQL or gRPC as the primary contract, serverless event-only, embedded firmware, batch-only data pipeline), surface that during discovery, state it explicitly in `technical-specs.md` (architecture summary + API section), and generate epics/tasks that match the chosen shape—do not force REST or three-tier wording where it does not apply.

#### Path A: No Input Document

Conduct a structured interview. If the host environment offers structured prompts (multiple-choice, forms, or similar), use them to reduce back-and-forth; otherwise ask conversationally. Cover all 7 fields above and confirm or adjust the **reference architecture** (see previous subsection).

#### Path B: User Provides a Document

When the user attaches or references a document (brief, one-pager, meeting notes, existing PRD, spec, or any other format), follow this process:

**Step 1 — Parse & Map.** Read the document and map its content against the 7 required fields. Build an internal coverage matrix:

| Field | Status | Source excerpt or note |
|-------|--------|-----------------------|
| Product vision | Covered / Partial / Missing | ... |
| Target users | Covered / Partial / Missing | ... |
| ... | ... | ... |

**Step 2 — Validate what IS present.** For each covered field, check for:
- **Ambiguities**: Subjective terms ("fast", "user-friendly") without quantifiable criteria.
- **Contradictions**: Conflicting statements across sections.
- **Agent-readiness gaps**: Requirements that an AI agent cannot act on due to missing specificity (e.g., "good UX" without concrete criteria).
- **Implicit assumptions**: Decisions that appear assumed but are not stated explicitly.

**Step 3 — Present the analysis.** Show the user a concise summary:
- What was found and mapped for each field.
- Issues detected (ambiguities, contradictions, gaps) with specific quotes from the document.
- Fields that are missing entirely.

**Step 4 — Ask only what's needed.** Formulate targeted questions covering:
- Missing fields (information not in the document at all).
- Clarifications on ambiguous or contradictory content.
- Confirmation of inferred assumptions.
- If system shape is unclear, confirm the **reference architecture** (presentation + REST API + DB + optional integrations) or capture an explicit deviation for `technical-specs.md`.

Do NOT re-ask information that is already clear in the document. Respect the user's existing work.

**Step 5 — Terminology.** Preserve domain-specific terms and naming conventions from the user's document. If the document uses "workspace" instead of "project", carry that terminology into the generated PRD system.

### Phase 2: Draft Generation

Generate the complete document system in `docs/` at the project root:

```
docs/
├── prd.md                  # Main PRD
├── technical-specs.md      # Technical specifications
├── user-stories.md         # User stories with acceptance criteria
└── epics-and-tasks.md      # Epics and granular agent-ready tasks
```

Generate all four documents following the templates in this skill's directory. Read each template before generating its corresponding document:

- [prd-template.md](prd-template.md) → `docs/prd.md`
- [technical-specs-template.md](technical-specs-template.md) → `docs/technical-specs.md`
- [user-stories-template.md](user-stories-template.md) → `docs/user-stories.md`
- [epics-tasks-template.md](epics-tasks-template.md) → `docs/epics-and-tasks.md`

### Phase 3: Review & Refinement

After generating the draft:

1. Present a concise summary of what was generated (document count, epic count, task count, key decisions made).
2. Ask the user to review and provide feedback.
3. Iterate on specific documents or sections based on feedback.
4. Repeat until the user approves.

## Document Relationships

The four documents form a traceable hierarchy:

```
prd.md (Features F-001..F-NNN)
  → technical-specs.md (references Features by ID)
  → user-stories.md (Stories US-001..US-NNN, linked to Features)
    → epics-and-tasks.md (Epics EP-001..EP-NNN → Tasks T-001..T-NNN, linked to Stories)
```

Every task in `epics-and-tasks.md` must trace back to a user story, which traces back to a feature in the PRD. Use consistent IDs across all documents.

## Writing Guidelines

### For All Documents

- Write in English (unless the user explicitly requests another language).
- Use Markdown with clear heading hierarchy.
- Be specific and unambiguous; avoid subjective terms like "fast" or "user-friendly" without quantifiable criteria.
- Prefer bullet lists and tables over prose for requirements.
- Include explicit "Out of Scope" sections to prevent agent drift.
- Keep each document self-contained: include enough context that an agent can understand it without reading the others, but reference related document IDs for traceability.

### For Agent Consumption

These documents will be fed to an AI coding agent. Optimize for:

- **Parseable structure**: Consistent heading levels, numbered IDs, standardized formats.
- **Actionable specificity**: Each task should be completable by an agent in a single session without ambiguity. Prefer **contract-level** clarity (behavior, data rules, API operations); use **repo-level** detail (file paths, exact CLI commands) when the user has a codebase or stack—otherwise mark those as TBD and refine after repository bootstrap.
- **Constraint-first**: State constraints and guardrails before implementation details. Agents work better when they know boundaries upfront.
- **Verifiable outcomes**: Every requirement and task should have a clear definition of done that an agent (or test) can verify.
- **Progressive context**: Start each document with an overview, then drill into detail. Agents read top-down.

### Technology agnosticism and reference architecture

**Implementation-agnostic:** Do NOT recommend or assume specific frameworks, languages, cloud vendors, or databases unless the user has already chosen them. If the user provides a tech stack, document it in `technical-specs.md`. If not, leave concrete technology choices as open decisions while still specifying **contracts** (data model, API operations, integrations, NFRs).

**Reference architecture (default):** For typical greenfield products, structure `technical-specs.md` and `epics-and-tasks.md` around the default topology in **Reference architecture (default)** under Phase 1: presentation + **REST HTTP API** + **database** + optional third-party integrations. REST here means the *shape of the API contract* (resources, HTTP methods, JSON payloads as appropriate), not a particular library.

**Atomicity:** Tasks should be atomic at the **contract / behavior** level first (what must be true when done). Add **repository-bound** instructions (paths, commands from the project’s toolchain) when known; if the repo does not exist yet, state TBD and rely on a follow-up pass once scaffolding exists.

## Completeness Checklist

Before presenting the draft to the user, verify:

- [ ] All features in `prd.md` have unique IDs (F-001, F-002, ...)
- [ ] All user stories reference at least one feature ID
- [ ] All epics group related user stories logically
- [ ] All tasks reference a user story and have a clear definition of done
- [ ] Scope boundaries are explicitly stated in `prd.md`
- [ ] No orphan requirements (every requirement traces to at least one task)
- [ ] No framework, language, or vendor assumptions unless explicitly provided by the user (reference topology and REST-style API contracts are allowed as the default; deviations must be stated in `technical-specs.md`)
- [ ] `technical-specs.md` includes data model, API boundaries, and integration points
- [ ] Security, performance, and accessibility considerations are addressed

## Example

For a complete example of the output document system, see [example-output.md](example-output.md).
