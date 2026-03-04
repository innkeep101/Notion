# Notion Codex — Doc 1: Projects & Tasks System
**Version 1.0 | March 4, 2026 | Living Document**
*Part of the Notion Master Reference Suite. Load Doc 0 alongside this document for foundational principles, property rules, and agent governance.*

---

## AI & Agent Instruction Block

**Document scope:** Everything needed to architect, maintain, and automate a project and task management system in Notion. This document does not re-explain concepts covered in Doc 0 — it applies them.

**Load Doc 0 first for:** governing principles, Status property architecture, property type rules, formula patterns, agent infrastructure requirements, naming conventions.

**What agents can do with this document:** Build or audit a Projects/Tasks database system, configure sprints, set up task views, write agent SOPs for project-level automation.

**What agents must not do without explicit instruction:** Delete entries, change permissions, restructure source databases, or modify sprint configurations.

---

## Table of Contents

1. [The Four-Database Foundation](#1-the-four-database-foundation)
2. [Projects Database — Schema & Standards](#2-projects-database--schema--standards)
3. [Tasks Database — Schema & Standards](#3-tasks-database--schema--standards)
4. [The Task Database Designation](#4-the-task-database-designation)
5. [Sub-tasks and Dependencies](#5-sub-tasks-and-dependencies)
6. [Sprints — Architecture and Setup](#6-sprints--architecture-and-setup)
7. [Views — The Right View for Every Context](#7-views--the-right-view-for-every-context)
8. [Page Body Standards for Projects and Tasks](#8-page-body-standards-for-projects-and-tasks)
9. [Agent SOP Templates — Project Automation](#9-agent-sop-templates--project-automation)
10. [Common Mistakes and How to Avoid Them](#10-common-mistakes-and-how-to-avoid-them)
11. [Living Document Protocol](#11-living-document-protocol)

---

## 1. The Four-Database Foundation

Most professional project systems in Notion stabilize around four source databases. Adding more before this foundation is solid is the most common architecture mistake.

| Database | What it holds | Parent/child |
|---|---|---|
| **Projects** | Discrete bodies of work with a defined outcome and timeline | Parent of Tasks |
| **Tasks** | Individual actionable work items with a single owner | Child of Projects; child of Sprints (if using) |
| **Sprints** | Time-boxed containers grouping tasks across projects | Parent of Tasks |
| **Domain DB** | One context-specific database (content, CRM, assets, etc.) | Relates to Projects or Tasks as needed |

**The rule:** Projects are parents of Tasks. Tasks are children of exactly one Project. Sprints group Tasks across multiple Projects. A Task belongs to one Project and optionally one Sprint — never more than one of either.

**When to skip Sprints:** Sprints add meaningful overhead. Use them only when you are running time-boxed iterative work — engineering, content production, or any workflow with explicit planning cycles. For personal task management or simple project tracking, Projects + Tasks is enough.

---

## 2. Projects Database — Schema & Standards

### 2.1 Required properties

| Property | Type | Purpose |
|---|---|---|
| `Project Name` | Title | Primary identifier — outcome-oriented verb phrase |
| `Project Status` | Status | Lifecycle state — see groups below |
| `Project Owner` | Person | Accountable individual |
| `Project Start Date` | Date | Planned or actual start |
| `Project Due Date` | Date | Target completion |
| `Project Priority` | Select | High / Medium / Low |
| `Project ID` | Unique ID (prefix: `PROJ-`) | Stable reference for agents and relations |
| `Tasks` | Relation → Tasks DB | Links all child tasks |
| `Task Progress` | Rollup → Tasks (% Complete) | Calculated from task Status groups |

### 2.2 Status groups for Projects

```
To-do:        Planned, Backlog
In progress:  Active, On Hold
Complete:     Done, Cancelled
```

Filter by group, not by option name. A filter for `Project Status group = In progress` captures both Active and On Hold without breaking when options change.

### 2.3 Recommended optional properties

| Property | Type | When to add |
|---|---|---|
| `Project Type` | Select | When projects fall into distinct categories (Campaign, Infrastructure, Research) |
| `Project Notes` | Text | Brief freeform context — only if it won't go in the page body |
| `Project Collaborators` | Person | Multi-person teams |
| `Related Projects` | Relation → Projects DB (self) | For dependent or sequential projects |

### 2.4 Rollup: Task Progress

Connect the `Tasks` relation to a rollup that calculates `% of tasks where Status group = Complete`. This gives a live progress indicator without a formula and survives status option changes because it uses group semantics.

---

## 3. Tasks Database — Schema & Standards

### 3.1 Required properties

| Property | Type | Purpose |
|---|---|---|
| `Task Name` | Title | Verb + specific deliverable |
| `Task Status` | Status | Workflow state — see groups below |
| `Task Owner` | Person | Single responsible owner |
| `Task Due Date` | Date | When this task must be done |
| `Project` | Relation → Projects DB | Parent project — one per task |
| `Task ID` | Unique ID (prefix: `TASK-`) | Stable reference for agents |
| `Sprint` | Relation → Sprints DB | Optional — only if using sprints |
| `Priority` | Select | High / Medium / Low |

### 3.2 Status groups for Tasks

```
To-do:        Not Started, Backlog
In progress:  In Progress, In Review, Blocked
Complete:     Done, Cancelled
```

The `Blocked` option belongs under **In progress**, not To-do. A blocked task is active — it is being monitored and needs action to unblock. This is the correct semantic.

### 3.3 Recommended optional properties

| Property | Type | When to add |
|---|---|---|
| `Estimated Hours` | Number | When tracking effort or velocity |
| `Task Type` | Select | When you need to distinguish task categories (Design, Dev, Copy, etc.) |
| `Blocks` / `Blocked By` | Relation → Tasks DB (self) | When using explicit dependency tracking outside Timeline view |
| `Sub-tasks` | Sub-items (enabled in settings) | When tasks need to break into smaller steps |

### 3.4 Naming convention — non-negotiable

Every task title must follow: **Verb + specific deliverable.**

| Good | Bad |
|---|---|
| `Draft Q2 campaign brief` | `Campaign` |
| `Fix OBS scene transition bug` | `Bug fix` |
| `Review Luiz's overlay design` | `Review` |
| `Send Carson contract follow-up` | `Carson` |

Vague task names are invisible to agents, unsearchable by Q&A, and require mental overhead every time they appear in a view.

---

## 4. The Task Database Designation

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

Converting a database to a **Task database** is a Notion-level designation — not just a naming convention. It is required for Sprints and for Home → My Tasks to work.

**How to enable:**
1. Open database settings (⋯ at top of database)
2. Database settings → More settings → Turn into Tasks
3. Map: Status property → `Task Status`, Assignee → `Task Owner`, Due date → `Task Due Date`
4. If these properties don't exist yet, create them at this step

**What it unlocks:**
- Tasks from this database appear in **Home → My Tasks** (cross-workspace task aggregation)
- Sprint configuration becomes available
- Sub-tasks and Dependencies settings become available
- The database integrates with Notion Calendar task layer

**One-way designation:** You can turn a database into a Task database but cannot easily undo it without losing sprint configuration. Decide before building the schema.

---

## 5. Sub-tasks and Dependencies

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

### 5.1 Sub-tasks

Sub-tasks are child items of a task row, enabled via database settings. They live inside the parent task and inherit the same property schema.

**When to use sub-tasks:**
- A task has 3–7 discrete steps that need individual tracking
- Steps need to be assigned to different people
- You want to track granular progress within a single deliverable

**When not to use sub-tasks:**
- The "steps" are really separate tasks in their own right — create Tasks instead
- You need sub-tasks to relate to different Projects — sub-tasks can't have their own Project relation independently

**How to enable:**
Database settings → More settings → Sub-items → Turn on

### 5.2 Dependencies

Dependencies are visualized as directional arrows in **Timeline view** — the parent task must be complete before the dependent task begins. They are set by hovering over a task in Timeline view and dragging the dependency handle to the blocking task.

**Hard constraint:** Dependencies only appear in Timeline view and require Date properties to be set on both tasks. You cannot add or view dependencies in Table or Board view.

**Agent note:** Dependencies are not readable or writable via standard database operations. Agents cannot set or detect dependencies. Use a `Blocked By` Relation property as an agent-readable proxy when automation is needed.

---

## 6. Sprints — Architecture and Setup

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

### 6.1 What Notion Sprints are

Sprints in Notion are a native feature of Task databases — not a workaround or template. When enabled, Notion creates:

- A **Sprints database** — one row per sprint, with start/end dates and a relation to Tasks
- A **Sprint board** — a dedicated page with four pre-built views: Current Sprint, Sprint Planning, Backlog, and Timeline
- A sprint relation on every task row — `Sprint` property (Relation → Sprints DB)

Sprints are containers. One sprint can include tasks from multiple projects. One task belongs to at most one sprint.

### 6.2 How to enable Sprints

1. Ensure the database is already a Task database (Section 4)
2. Database settings → More settings → Sprints → Turn on sprints
3. Set sprint duration (1 week, 2 weeks, or custom)
4. Notion creates the Sprints DB and Sprint board automatically

### 6.3 The four Sprint board views

| View | What it shows | When to use it |
|---|---|---|
| **Current Sprint** | All tasks in the active sprint, grouped by status | Daily execution — what's on deck right now |
| **Sprint Planning** | Current, next, and previous sprint + unassigned backlog | Planning sessions — drag tasks into sprints |
| **Backlog** | All tasks not yet assigned to a sprint | Grooming — prioritize and assign upcoming work |
| **Timeline** | Visual Gantt of sprints over time | Review cadence — see sprint sequencing and gaps |

### 6.4 Completing a sprint

Sprint board → Current sprint → Complete sprint → Notion prompts: move incomplete tasks to next sprint or to backlog. Always choose consciously — defaulting to "move to next sprint" without reviewing causes sprint bloat.

### 6.5 Sprint schema — what to add beyond the default

The default Sprints database has Title, Start date, End date, and the Tasks relation. Recommended additions:

| Property | Type | Why |
|---|---|---|
| `Sprint Goal` | Text | One sentence per sprint; improves retrospective clarity |
| `Sprint Status` | Status | Planned / Active / Complete — enables cross-sprint filtering |
| `Velocity` | Rollup → Tasks (Count where Done) | Tracks completed tasks per sprint over time |

---

## 7. Views — The Right View for Every Context

Doc 0 covers all view types. This section covers which views matter most for project and task management and how to configure them.

### 7.1 Projects database — recommended views

| View | Configuration | Purpose |
|---|---|---|
| **Board** | Group by `Project Status` | See all projects by lifecycle stage at a glance |
| **Table** | All properties visible, sort by `Project Due Date` | Bulk editing and status updates |
| **Timeline** | Date range = `Project Start Date` → `Project Due Date` | Planning and scheduling across projects |
| **My Projects** | Filter: `Project Owner = Me`, Status group ≠ Complete | Personal focus — active work only |

### 7.2 Tasks database — recommended views

| View | Configuration | Purpose |
|---|---|---|
| **Board** | Group by `Task Status` | Daily execution — drag tasks through workflow |
| **My Tasks** | Filter: `Task Owner = Me`, Status group ≠ Complete, sort by `Task Due Date` | Personal focus — what I need to do today |
| **By Project** | Group by `Project` relation | See all tasks under each project |
| **Overdue** | Filter: `Task Due Date` is before today, Status group ≠ Complete | Weekly review — catch slipped items |
| **Timeline** | Date = `Task Due Date`, dependencies enabled | Sprint planning and sequencing |

### 7.3 Linked database views on project pages

Each Project page body should contain a linked view of the Tasks database, filtered to `Project = [this project]`. This gives immediate access to all tasks for a project without leaving the project page.

**How to add:** Inside the project page body, type `/linked`, select "Linked view of database," choose the Tasks database, and add a filter: `Project = [current page]`.

This is the single most useful structural decision in the entire Projects/Tasks system. Every project page becomes a live command center.

---

## 8. Page Body Standards for Projects and Tasks

Doc 0 establishes the minimum viable page body standard. This section applies it to Projects and Tasks specifically.

### 8.1 Project page body template

```
## What This Project Is
[2-3 sentences: what we're building, why it matters, what success looks like]

## Outcome / Definition of Done
[One clear statement of what "complete" means for this project]

## Key Constraints
[Dates, dependencies, budget, team limitations — anything that shapes the work]

## Context for AI
[Any background an agent needs to act on this project without asking for clarification]

---
[Linked view: Tasks filtered to this project]
```

### 8.2 Task page body template

```
## What This Task Is
[1-2 sentences: what needs to be done and why]

## Definition of Done
[Specific, unambiguous — what does "complete" look like for this task?]

## Notes / Context
[Anything relevant: links, blockers, decisions made, prior attempts]
```

**The rule:** If a task page body is empty, an agent cannot act on it reliably. An agent that reads `Task Name: Fix onboarding bug` with no body has no context for what the fix involves, what "fixed" means, or what constraints apply. Body content is not documentation overhead — it is agent fuel.

---

## 9. Agent SOP Templates — Project Automation

These SOPs follow the universal template from Doc 0 Section 13. Copy, adapt to your schema, and load into the relevant agent's Instructions or SOP page.

### SOP 1: Daily Task Digest

```
SOP: Task Digest Agent — Daily Summary

Goal:
- Post a summary of all tasks due today or overdue to a designated Slack channel

Inputs:
- Tasks database, filtered: Status group ≠ Complete, Due Date ≤ today
- Slack channel: [specify]

Allowed actions:
- Read Tasks database (filtered view)
- Post formatted message to Slack

Hard constraints:
- Read only — no writes to Tasks database
- No DMs — post to channel only
- If 0 tasks match filter, post: "No tasks due today."

Validation checklist:
- [ ] Filter returns results before posting
- [ ] Message includes: Task Name, Owner, Due Date, Project

Logging:
- Agent Log: timestamp, number of tasks found, Slack post confirmed

Escalation:
- If Slack post fails → create Agent Inbox item
```

### SOP 2: Overdue Task Flagging

```
SOP: Overdue Flag Agent — Status Updater

Goal:
- Update Task Status to "Blocked" for any task where Due Date is past and Status is "In Progress"

Inputs:
- Tasks database, filtered: Due Date < today, Task Status = "In Progress"

Allowed actions:
- Update Task Status property to "Blocked"
- Append note to task page body: "[Agent] Flagged overdue on [date]. Was: In Progress."

Hard constraints:
- Do not change Status if already "Blocked", "Done", or "Cancelled"
- Do not delete or move any task
- Always log before acting

Validation checklist:
- [ ] Confirm Due Date is genuinely past (not a timezone artifact)
- [ ] Confirm current Status is "In Progress" only
- [ ] Confirm task has an Owner before flagging

Logging:
- Agent Log: task ID, task name, previous status, new status, timestamp

Escalation:
- If Owner is missing → skip task, add to Agent Inbox with note
```

### SOP 3: Sprint Rollover Summary

```
SOP: Sprint Summary Agent — End of Sprint Report

Goal:
- When a sprint ends, generate a summary: tasks completed, tasks moved, tasks cancelled

Inputs:
- Sprints database: sprint with Status = "Complete" (most recent)
- Tasks database: filtered to that sprint's relation

Allowed actions:
- Read Sprints and Tasks databases
- Create a new page in a designated "Sprint Retrospectives" database with the summary
- Post summary to Slack channel: [specify]

Hard constraints:
- Read only on Sprints and Tasks databases
- Write only to Sprint Retrospectives database
- Do not modify any task or sprint properties

Validation checklist:
- [ ] Confirm sprint Status = "Complete" before running
- [ ] Confirm tasks relation is populated (not empty sprint)

Logging:
- Agent Log: sprint name, date, task counts (complete / moved / cancelled)

Escalation:
- If sprint has 0 tasks → skip and log, do not post
```

---

## 10. Common Mistakes and How to Avoid Them

| Mistake | What goes wrong | The fix |
|---|---|---|
| Using Select instead of Status for task lifecycle | Filtering by group doesn't work; agents can't reason about state | Switch to Status property; map options to correct groups |
| Not enabling Task database designation | Tasks don't appear in Home; Sprints can't be turned on | Enable via Database settings → Turn into Tasks |
| Tasks with no Project relation | Orphaned tasks; no rollup progress; agents can't route them | Make Project relation required in page template |
| Vague task names | Unsearchable; agents produce low-quality outputs | Enforce Verb + deliverable naming at creation |
| Empty task page bodies | Autofill and agents have no context to work with | Add body template as default page template in DB settings |
| Building sub-tasks when separate tasks are needed | Sub-tasks can't have independent Project relations | Use Tasks + relation if items need separate project ownership |
| Adding sprints before Task DB designation | Sprints option doesn't appear in settings | Designate as Task database first |
| Rollup counting task option names instead of groups | Breaks when options are renamed | Use Status group = Complete in rollup, not option name |

---

## 11. Living Document Protocol

**Rebuild triggers:**
- Notion changes the Task database designation flow or Sprint architecture
- New agent capabilities change what's possible in project automation
- A pattern is validated (or invalidated) through operational experience in the live workspace

**Cross-references:**
- Doc 0 Section 3 — Status property architecture
- Doc 0 Section 5 — Formula patterns (progress indicators)
- Doc 0 Section 7 — Agent infrastructure (Instructions, SOP, Log, Inbox)
- Doc 0 Section 13 — Universal SOP template
- Doc 3 — AI Agents deep dive (for building agents that operate on this system)

**Version history:**
- v1.0 — March 4, 2026 — Initial build by Claude. Verified against official Notion help documentation.

---

*Notion Codex — Doc 1: Projects & Tasks System | v1.0 | March 4, 2026*
*Load Doc 0 alongside this document. Written with Claude.*
