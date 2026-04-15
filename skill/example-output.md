# Example: TaskFlow — Complete PRD System Output

This shows a condensed example of the four documents generated for a hypothetical task management app. In a real output, each document would be a separate file in `docs/`.

**Note:** TaskFlow follows the skill’s **default reference architecture** (client + REST HTTP backend + persisted data). Epics/tasks therefore read like a typical web product; a different topology would change `technical-specs.md` and task wording accordingly.

---

## docs/prd.md (excerpt)

```markdown
# TaskFlow — Product Requirements Document

**Version:** 1.0
**Date:** 2026-04-15
**Status:** Draft
**Author:** PM Team

## 1. Product Overview

### 1.1 Vision Statement
TaskFlow is a collaborative task management platform that helps small teams
organize work, track progress, and meet deadlines without the complexity of
enterprise project management tools.

### 1.2 Problem Statement
Small teams (2-10 people) struggle with existing tools: spreadsheets lack
structure, and enterprise tools (Jira, Asana) are overengineered for their
needs. Teams waste ~5 hours/week on status communication that a simple shared
board could eliminate.

### 1.3 Target Users

| Persona | Description | Primary Need |
|---------|-------------|--------------|
| Team Lead | Manages a small team of 3-8 | Visibility into who's working on what |
| Contributor | Individual team member | Clear priorities and deadlines |

### 1.4 Value Proposition
Simplicity-first task management: a shared board that takes 5 minutes to set
up and requires zero training.

## 3. Scope

### 3.1 In Scope
- Task CRUD with assignment, due dates, and status
- Board view (Kanban-style)
- Real-time collaboration (multiple users see updates instantly)
- Team management (invite, roles)

### 3.2 Out of Scope
- Time tracking
- Gantt charts / timeline views
- Integrations with external tools
- Mobile native apps (web-only at launch)

## 4. Features

### F-001: Task Board
- **Priority:** Must Have
- **Description:** A Kanban board where users create, move, and manage tasks
  across customizable columns.
- **Acceptance Criteria:**
  - [ ] Users can create tasks with title, description, assignee, due date
  - [ ] Tasks can be dragged between columns
  - [ ] Board updates in real-time for all viewers
  - [ ] Board supports 3-7 customizable columns
- **Constraints:**
  - Maximum 500 tasks per board
  - Column names must be unique within a board
- **Related Stories:** US-001, US-002, US-003

### F-002: Team Management
- **Priority:** Must Have
- **Description:** Invite team members, assign roles (Admin, Member).
- **Acceptance Criteria:**
  - [ ] Admin can invite users by email
  - [ ] Admin can change member roles
  - [ ] Members cannot delete the board or remove other members
- **Related Stories:** US-004, US-005
```

---

## docs/technical-specs.md (excerpt)

```markdown
# TaskFlow — Technical Specifications

**Version:** 1.0
**Date:** 2026-04-15
**Related PRD:** [prd.md](prd.md)

## 2. Data Model

### 2.1 Core Entities

| Entity | Description | Key Attributes |
|--------|-------------|----------------|
| User | Registered user | id, email, name, avatar_url, created_at |
| Team | Group of users | id, name, owner_id, created_at |
| Board | Kanban board | id, team_id, title, columns (JSON), created_at |
| Task | Work item | id, board_id, title, description, assignee_id, status, position, due_date, created_at |

### 2.2 Entity Relationships

erDiagram
    USER ||--o{ TEAM_MEMBER : "belongs to"
    TEAM ||--|{ BOARD : "has"
    BOARD ||--|{ TASK : "contains"
    USER ||--o{ TASK : "assigned to"

### 2.3 Data Constraints

- Board: 3–7 column names, unique per board; max 500 tasks per board
- Task: title required; `position` integer for ordering within a column

### 3.1 API Surface

| HTTP method & path | Purpose | Related Feature |
|--------------------|---------|-----------------|
| POST /teams/{teamId}/boards | Create board | F-001 |
| POST /boards/{boardId}/tasks | Create task | F-001 |
| PATCH /tasks/{taskId} | Update task / move between columns | F-001 |
| GET /boards/{boardId}/tasks | List tasks for a board | F-001 |
| POST /teams/{teamId}/invitations | Invite member | F-002 |

### 3.2 Authentication & Authorization

| Role | Permissions |
|------|------------|
| Admin | Full CRUD on board, tasks, and members |
| Member | CRUD own tasks, read all tasks, cannot manage members |
```

---

## docs/user-stories.md (excerpt)

```markdown
# TaskFlow — User Stories

**Version:** 1.0
**Related PRD:** [prd.md](prd.md)

### Feature: F-001 — Task Board

#### US-001: Create a task
**As a** Contributor,
**I want to** create a new task on the board,
**So that** my team can see what needs to be done.

**Priority:** Must Have
**Complexity:** S

**Acceptance Criteria:**
- [ ] Given I am on a board, when I click "Add Task" and fill in title,
      then a new task appears in the first column
- [ ] Given I create a task, when I set a due date, then the task shows
      the due date badge
- [ ] Given I create a task without a title, then the system shows a
      validation error

**Edge Cases:**
- What if the board has reached 500 tasks? → Show limit reached message.
- What if two users create tasks simultaneously? → Both should succeed;
  positions auto-resolve.

#### US-002: Move a task between columns
**As a** Contributor,
**I want to** drag a task to a different column,
**So that** the board reflects the current status of work.

**Priority:** Must Have
**Complexity:** M

**Acceptance Criteria:**
- [ ] Given a task in column A, when I drag it to column B, then the
      task appears in column B for all viewers within 2 seconds
- [ ] Given a task is moved, when I refresh the page, the task remains
      in the new column

## Story Map Summary

| Story ID | Title | Feature | Priority | Complexity | Epic |
|----------|-------|---------|----------|------------|------|
| US-001 | Create a task | F-001 | Must Have | S | EP-001 |
| US-002 | Move a task | F-001 | Must Have | M | EP-001 |
| US-003 | Customize columns | F-001 | Should Have | M | EP-001 |
| US-004 | Invite member | F-002 | Must Have | M | EP-002 |
| US-005 | Manage roles | F-002 | Must Have | S | EP-002 |
```

---

## docs/epics-and-tasks.md (excerpt)

```markdown
# TaskFlow — Epics & Tasks

**Version:** 1.0
**Date:** 2026-04-15
**Related PRD:** [prd.md](prd.md)
**Related Stories:** [user-stories.md](user-stories.md)

## Epic Overview

| Epic ID | Name | Stories | Task Count | Priority |
|---------|------|---------|------------|----------|
| EP-001 | Task Board Core | US-001, US-002, US-003 | 5 | Must Have |
| EP-002 | Team Management | US-004, US-005 | 3 | Must Have |

## Recommended Execution Order

1. **EP-002** — Team Management (foundational: users and roles needed first)
2. **EP-001** — Task Board Core (depends on user/team model from EP-002)

## EP-001: Task Board Core

**Goal:** Users can create, view, and move tasks on a Kanban board.
**Stories:** US-001, US-002, US-003
**Depends on:** EP-002

### T-001: Create Board data model and CRUD
- **Story:** US-001
- **Status:** Not Started
- **Contract references:** technical-specs.md §2.1 Board entity; §2.2 Board–Team relationship; aligns with POST /teams/{teamId}/boards (§3.1)
- **Assumptions:** Team model and persistence layer from EP-002 exist; auth middleware exists for later API tasks
- **Repo binding:** TBD — e.g. `src/db/board.*`, `src/models/board.*` after scaffold
- **Description:** Define the Board entity with columns stored as JSON.
  Implement create, read, update, delete operations (no HTTP layer in this task).
- **Definition of Done (contract / behavior):**
  - [ ] Board entity exists with fields: id, team_id, title, columns, created_at
  - [ ] CRUD operations enforce title required, 3–7 columns, unique column names within a board
  - [ ] Default columns on creation: "To Do", "In Progress", "Done"
- **Definition of Done (toolchain — when repo exists):**
  - [ ] Unit tests cover CRUD and validation; `TBD` — project test command after bootstrap
  - [ ] Build / linter clean; `TBD` — project lint and build commands after bootstrap
- **Agent Instructions:**
  - Columns: ordered JSON array of `{id, name}` objects
  - Do NOT implement the REST API layer yet
  - Use **Repo binding** once the repository layout is known
- **Depends on:** None

### T-002: Create Task data model and CRUD
- **Story:** US-001
- **Status:** Not Started
- **Contract references:** technical-specs.md §2.1 Task entity; §3.1 POST/PATCH/GET task routes; PRD F-001 constraints (500 tasks per board)
- **Assumptions:** T-001 complete (Board exists)
- **Repo binding:** TBD — data layer module paths after scaffold
- **Description:** Define the Task entity. Implement CRUD with position
  ordering within columns.
- **Definition of Done (contract / behavior):**
  - [ ] Task entity: id, board_id, title, description, assignee_id, status, position, due_date, created_at
  - [ ] Position integer ordering within a column; moves update affected positions
  - [ ] Max 500 tasks per board enforced at persistence layer
- **Definition of Done (toolchain — when repo exists):**
  - [ ] Unit tests pass; `TBD` — test command after bootstrap
- **Agent Instructions:**
  - Position gaps (e.g. increments of 1000) to reduce reorder churn
  - Do NOT implement drag-and-drop UI — data operations only
- **Depends on:** T-001

### T-003: Board and Task REST endpoints
- **Story:** US-001, US-002
- **Status:** Not Started
- **Contract references:** technical-specs.md §3.1 API Surface (board and task routes); §3.2 roles (authenticated callers)
- **Assumptions:** T-001, T-002 complete; session/JWT validation available from EP-002
- **Repo binding:** TBD — router/handler paths after scaffold (e.g. `src/api/boards.ts`)
- **Description:** Expose Board and Task CRUD and task move via REST over HTTP per §3.1.
- **Definition of Done (contract / behavior):**
  - [ ] Routes exist for board CRUD, task CRUD, and move (column + position) matching §3.1
  - [ ] All routes require authentication; responses use correct HTTP semantics (201, 404, 422, etc.)
  - [ ] Request/response bodies match data constraints from §2.1 / §2.3
- **Definition of Done (toolchain — when repo exists):**
  - [ ] Integration tests pass; `TBD` — test command after bootstrap
- **Agent Instructions:**
  - Validate inputs against the same rules as the data layer
  - Do not leak internal errors in production responses
- **Depends on:** T-001, T-002
```

---

This example demonstrates:
- **Consistent IDs** (F-001 → US-001 → EP-001 → T-001) across all documents
- **Traceability** from feature to story to epic to task
- **Reference architecture** (client + REST + DB) with **contract references** on each task
- **Contract vs toolchain DoD** — behavior is fully specified; commands and file paths stay **TBD** until the repo exists, then a refinement pass fills **Repo binding**
- **Dependency ordering** so tasks can be picked up sequentially
