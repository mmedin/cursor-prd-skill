# Example: TaskFlow — Complete PRD System Output

This shows a condensed example of the four documents generated for a hypothetical task management app. In a real output, each document would be a separate file in `docs/`.

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

### 3.1 API Surface

| Operation | Purpose | Related Feature |
|-----------|---------|-----------------|
| Create Task | Add a task to a board | F-001 |
| Move Task | Change task column/position | F-001 |
| List Tasks | Fetch all tasks for a board | F-001 |
| Invite Member | Add user to team | F-002 |

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
**Related PRD:** [prd.md](prd.md)

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
- **Description:** Define the Board entity with columns stored as JSON.
  Implement create, read, update, delete operations.
- **Definition of Done:**
  - [ ] Board entity exists with fields: id, team_id, title, columns, created_at
  - [ ] CRUD operations work and pass unit tests
  - [ ] Default columns on creation: "To Do", "In Progress", "Done"
  - [ ] Build succeeds, linter clean
- **Agent Instructions:**
  - Create the Board model in the data layer
  - Columns are stored as ordered JSON array of {id, name} objects
  - Include validation: title required, 3-7 columns, unique column names
  - Do NOT implement the API layer yet
- **Depends on:** None

### T-002: Create Task data model and CRUD
- **Story:** US-001
- **Status:** Not Started
- **Description:** Define the Task entity. Implement CRUD with position
  ordering within columns.
- **Definition of Done:**
  - [ ] Task entity exists with fields: id, board_id, title, description,
        assignee_id, status, position, due_date, created_at
  - [ ] Position is an integer for ordering within a column
  - [ ] Moving a task updates positions of affected tasks
  - [ ] Max 500 tasks per board enforced
  - [ ] Unit tests pass
- **Agent Instructions:**
  - Use a position integer with gaps (e.g. increments of 1000) to allow
    inserts without reordering
  - Enforce the 500-task limit at the data layer, not just the API
  - Do NOT implement drag-and-drop UI — only the data operations
- **Depends on:** T-001

### T-003: Board API endpoints
- **Story:** US-001, US-002
- **Status:** Not Started
- **Description:** Expose Board and Task CRUD via API endpoints.
- **Definition of Done:**
  - [ ] Endpoints for board CRUD and task CRUD exist
  - [ ] Move task endpoint accepts target column and position
  - [ ] All endpoints require authentication
  - [ ] Integration tests pass
- **Agent Instructions:**
  - Follow patterns from existing API layer if present
  - Include input validation matching data model constraints
  - Return appropriate HTTP status codes (201 created, 404 not found, etc.)
- **Depends on:** T-001, T-002
```

---

This example demonstrates:
- **Consistent IDs** (F-001 → US-001 → EP-001 → T-001) across all documents
- **Traceability** from feature to story to epic to task
- **Agent-ready tasks** with specific instructions, constraints, and verifiable outcomes
- **Dependency ordering** so tasks can be picked up sequentially
