# AGENTS.md — cursor-prd-skill

## Project Purpose

Development and distribution repository for the `prd-writer` agent skill. This repo is NOT a consumer of the skill — it is where the skill is authored, tested, and published. The skill content is host-agnostic Markdown; Cursor is one documented install target in `README.md`.

## Repository Structure

```
cursor-prd-skill/
├── README.md              # User-facing documentation and installation instructions
├── AGENTS.md              # This file — context for agents working on this repo
├── LICENSE                # MIT license
├── .gitignore
└── skill/                 # The distributable skill (what users copy to install)
    ├── SKILL.md           # Main skill definition (entry point)
    ├── prd-template.md
    ├── technical-specs-template.md
    ├── user-stories-template.md
    ├── epics-tasks-template.md
    └── example-output.md
```

## What the Skill Does

Generates a system of 4 interconnected PRD documents in a target project's `docs/` folder, optimized for consumption by AI coding agents (IDE assistants, CLI agents, or similar with repository context).

## Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Target agent | AI coding agents with repo context | Portable across hosts that load Markdown skill packs |
| Project scope | Greenfield projects | Templates assume new product definition |
| Output format | 4 interrelated documents | Separation of concerns; each doc serves a different stage |
| Interaction model | Hybrid (draft, feedback, refine) | Balances speed with accuracy |
| Input document handling | Gap analysis + validation | Ask only what's missing; validate what's present |
| Task granularity | Down to agent-ready tasks | Each task completable in a single agent session |
| Implementation stance | Agnostic (no frameworks/vendors by default) | Users may supply a stack; otherwise specs stay implementation-neutral |
| Reference architecture | Default high-level topology | Presentation + REST HTTP API + database + optional third-party integrations; discovery captures deviations |
| User persona | Product Manager | Language and workflow optimized for PM mental model |
| Storage of output | `docs/` folder in target project | Standard location, version-controlled |
| Skill distribution | `skill/` dir at repo root | Users copy this folder to install |

## Conventions

- **IDs across generated docs**: Features (F-NNN), User Stories (US-NNN), Epics (EP-NNN), Tasks (T-NNN)
- **Language**: Generated documents default to English unless user requests otherwise
- **Templates**: Agent reads templates from `skill/` before generating each document
- **SKILL.md limit**: Keep `SKILL.md` under 500 lines — a practical cap for skill packs in many clients; put detail in template files
- **References in SKILL.md**: One level deep only (SKILL.md links to sibling files, never nested)

## Development Guidelines

When modifying this skill:

1. Edit files inside `skill/`. That is the distributable unit.
2. Keep `SKILL.md` under 500 lines. Use progressive disclosure — put detail in template files.
3. Test by copying `skill/` to your host's skills location (e.g. `~/.cursor/skills/prd-writer/` for Cursor) and invoking the skill in a test project, or by loading `SKILL.md` the way your target client expects.
4. Update `README.md` if any user-facing behavior changes.
5. Update this `AGENTS.md` if any architectural decisions change.
