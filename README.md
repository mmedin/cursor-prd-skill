# prd-writer — Agent skill

An agent skill that generates structured PRD (Product Requirements Document) systems optimized for AI coding agents.

Given a product idea or an existing brief, the skill produces four interconnected documents ready to feed into an AI coding agent for implementation.

By default, generated specs and tasks assume a **high-level reference architecture**: user-facing client, **REST over HTTP** backend API, **database** (or equivalent persistence), and optional **third-party integrations**. The skill does **not** pick frameworks, languages, or vendors unless you supply them. If your product differs (CLI-only, GraphQL-primary, embedded, etc.), say so during discovery so outputs match your shape.

## What It Generates

The skill creates a `docs/` folder in the target project with:

| Document | Purpose |
|----------|---------|
| `prd.md` | Main PRD: vision, users, features, scope, constraints |
| `technical-specs.md` | Data model, API boundaries, integrations, security |
| `user-stories.md` | User stories with acceptance criteria (Given/When/Then) |
| `epics-and-tasks.md` | Epics and granular tasks ready for agent execution |

### Document Hierarchy

```
prd.md (Features F-001..F-NNN)
  → technical-specs.md (references Features by ID)
  → user-stories.md (Stories US-001..US-NNN, linked to Features)
    → epics-and-tasks.md (Epics EP-001..EP-NNN → Tasks T-001..T-NNN)
```

Every task traces back to a user story, which traces back to a feature. IDs are consistent across all documents.

## After generation (recommended workflow)

Once the skill has written `docs/prd.md`, `docs/technical-specs.md`, `docs/user-stories.md`, and `docs/epics-and-tasks.md`, use this sequence so implementation and testing agents get a clear handoff:

1. **Human review** — Product owner or PM reads the four documents, checks scope and acceptance criteria, and approves or requests edits (the skill’s Phase 3 loop).
2. **Topology check** — Confirm the docs match your intended system shape (default: presentation + REST API + DB + integrations). Update `technical-specs.md` if discovery missed a deviation.
3. **Stack and repository** — If the repo is empty or new, choose languages/frameworks/infrastructure (or use your org template), scaffold the project, and record decisions where your team keeps them (e.g. `AGENTS.md`, ADRs).
4. **Refine tasks to the repo (optional but typical)** — Run a follow-up pass with an AI coding agent **that has repository context**: fill in file paths, module names, and concrete commands (`test`, `lint`, `build`) in `epics-and-tasks.md` where the first draft said TBD or stayed contract-level only.
5. **Implement by epic** — Hand off `epics-and-tasks.md` to coding agents in dependency order; keep task IDs stable for traceability.
6. **Verify** — Use each task’s definition of done plus story acceptance criteria; add or run automated tests and CI as the project matures.

Steps 3–4 are where **technology choices** land; the skill stays at the **contract and planning** layer until you anchor work in a real codebase.

## Installation

The `skill/` directory in this repo is the distributable unit. Copy it to the location your AI tool uses for skills or long-running instructions (check that product's documentation for paths and naming).

No dependencies. No build step. Just copy the folder.

### Cursor (example)

**Personal (available across all your projects):**

```bash
mkdir -p ~/.cursor/skills
cp -r skill/ ~/.cursor/skills/prd-writer
```

**Per-project (shared via repository):**

```bash
mkdir -p <your-project>/.cursor/skills
cp -r skill/ <your-project>/.cursor/skills/prd-writer
```

### Other environments

Copy `skill/` into whatever directory or bundle your client expects (global skills folder, project-level skills tree, or a single instruction file if you merge the Markdown yourself). Rename the destination folder if the host requires a specific skill id.

## Usage

How you invoke the skill depends on your client: slash commands, `@` skill references, agent menus, or natural-language requests. Follow your tool's workflow to attach or enable the skill, then describe the product or attach a brief.

**Example (Cursor):** open the agent chat, use `/prd-writer` or describe what you want to build. The skill will either interview you or analyze a document you provide.

### With No Input Document

The skill conducts a structured interview covering: product vision, target users, value proposition, scope, success metrics, constraints, key user journeys, and whether the product follows the default **reference architecture** or diverges from it.

### With an Input Document

Attach or reference a brief, one-pager, meeting notes, or existing spec. The skill will:

1. Parse the document and map content against required fields.
2. Validate what's present (flag ambiguities, contradictions, gaps).
3. Ask only for what's missing or unclear.
4. Generate the full document system preserving your terminology.

## Skill Contents

```
skill/
├── SKILL.md                    # Main skill instructions (entry point)
├── prd-template.md             # Template for the main PRD
├── technical-specs-template.md # Template for technical specifications
├── user-stories-template.md    # Template for user stories
├── epics-tasks-template.md     # Template for epics and agent-ready tasks
└── example-output.md           # Complete example of generated output
```

## Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Target agent | AI coding agents with repo context | Same documents work across IDE assistants, CLI agents, and similar hosts |
| Scope | Greenfield projects | Templates assume new product definition |
| Output | 4 interrelated documents | Separation of concerns; each serves a different stage |
| Interaction | Hybrid (draft, feedback, refine) | Balances speed with accuracy |
| Task granularity | Agent-ready tasks | Each task completable in a single agent session |
| Implementation (frameworks, vendors) | Agnostic | No specific languages, frameworks, or cloud products unless you provide them |
| Reference architecture | Default topology | Presentation + REST HTTP API + database + optional integrations; deviations captured in discovery and `technical-specs.md` |
| Input handling | Gap analysis + validation | Asks only what's missing from provided docs |

## License

MIT

---

Built with the help of AI coding agents.
