# Notion Master Reference Suite — Doc 0: Central Hub
**Version 2.1 | March 4, 2026 | Living Document**
*Platform truth + agent rules. EIW-agnostic. Upstream: Notion Mission doc. Downstream: EIW OS and other implementations.*

---

## ⚙️ AGENT & AI INSTRUCTION BLOCK
*Read this before taking any action in this workspace.*

**Document role:** This is the foundational reference layer for all architecture, operations, automations, and agent behavior in this workspace. It is the first document any AI system — Notion Agent, Custom Agent, Claude project file, MCP-connected tool, or Q&A — must load before acting.

**Hub-and-spoke rule:** Foundational concepts are defined here once. Subject documents (Docs 1–8) reference this hub rather than re-explain it. If a concept applies to ≥3 subject documents, it lives here. If it applies to 1–2, it lives in those documents.

**Agent execution rule:** Before taking any action in this workspace, any AI system must comply with:
- The Core Design Philosophy (Section 1)
- Typed property requirements (Section 2)
- Status architecture (Section 3)
- Naming and AI-readability rules (Section 11)
- Agent instruction architecture (Section 13)

**Non-negotiables:**
- Default to one source database, many linked surfaces
- Default to Status (group semantics) for workflows — never Select
- Treat page bodies as required context, not optional notes
- Do not guess property names or option values — read the relevant subject document

**Living doc rule:** Update only during the weekly review process. Record version and date. Flag for rebuild if more than 30 days old.

---

## Architecture: Three Layers

This Codex sits in the middle of a three-layer stack. Understanding the layers prevents conflation.

| Layer | Document | What it contains | Audience |
|---|---|---|---|
| **Mission** | Notion Mission doc *(upstream)* | Why this system exists, what success looks like, milestones, privacy model | Private — me + AI + agents |
| **Codex** | This doc + Docs 1–8 | EIW-agnostic platform truth, best practices, agent-safe patterns, governance | Me + AI + agents + future collaborators |
| **Implementations** | EIW OS + other projects | EIW-specific systems built using Codex knowledge | Project-specific |

**The rule:** Codex docs cite platform truth. Implementations cite Codex entries. Mission sets priorities. Nothing flows backwards.

**→ Load Notion Mission doc for why this suite exists and which milestones should drive Codex updates next.**

---

## Audience + Visibility Model

**Primary readers:** Me + my AIs + my agents. This system is private-first.

| Tier | What it is | Rule |
|---|---|---|
| **Core System** | The full workspace — private, complete, truthful | Never assumes an audience. Never optimized for watchability over accuracy. |
| **Selective Surfaces** | Sanitized snapshots, templates, demos | Built intentionally for sharing. Never the live infrastructure. |
| **Broadcast Content** | Streams, posts, public communication | Communication layer only. Not a driver of system decisions. |

The system never becomes performative. Public proof is intentional and derived — it does not shape the core.

---

## Truth Model — How Claims Are Sourced

Every feature claim in this Codex should carry a source tier. When adding or updating a claim:

| Source tier | What it means |
|---|---|
| **Official** | Directly from notion.com/releases, notion.com/help, or developers.notion.com |
| **Secondary** | Curated third-party coverage with traceable original source |
| **Tested** | Verified by hands-on use in a live workspace |

**Additional tags to apply to time-sensitive claims:**
- `Last verified: [date]`
- `Confidence: High / Medium / Uncertain`
- `Watch: [note if this feature is likely to shift soon]`

**Changelog discipline:** Track source verifications and corrections in a Notion Codex Changelog page — not inline throughout Doc 0. Doc 0 stays readable; the changelog stays auditable. Flag any claim older than 60 days for re-verification.

---

## Table of Contents

1. [Core Design Philosophy](#1-core-design-philosophy)
2. [How Notion Databases Actually Work](#2-how-notion-databases-actually-work)
3. [The Status Property — Architecture and Why It Wins](#3-the-status-property--architecture-and-why-it-wins)
4. [Views — All Types, Layouts, and Filters](#4-views--all-types-layouts-and-filters)
5. [Formulas — Practical Patterns and Formula 2.0](#5-formulas--practical-patterns-and-formula-20)
6. [Notion AI — Four Operational Modes](#6-notion-ai--four-operational-modes)
7. [Notion Agents — Architecture, Capabilities, Credits](#7-notion-agents--architecture-capabilities-credits)
8. [MCP — Notion as Open AI Infrastructure](#8-mcp--notion-as-open-ai-infrastructure)
9. [The Suite Map — All Nine Documents](#9-the-suite-map--all-nine-documents)
10. [Platform Version Timeline — Mid-2024 Through March 2026](#10-platform-version-timeline--mid-2024-through-march-2026)
11. [Naming, Consistency, and the AI-Readable Workspace](#11-naming-consistency-and-the-ai-readable-workspace)
12. [Workspace Architecture Decision Framework](#12-workspace-architecture-decision-framework)
13. [Agent Instruction Architecture — Instructions, Skills, SOPs](#13-agent-instruction-architecture--instructions-skills-sops)
14. [Creator and Partner Ecosystem — Overview](#14-creator-and-partner-ecosystem--overview)
15. [Upcoming Features and Platform Direction](#15-upcoming-features-and-platform-direction)
16. [The Decision Framework — Where Does This Task Live?](#16-the-decision-framework--where-does-this-task-live)
17. [Living Document Protocol](#17-living-document-protocol)

---

## 1. Core Design Philosophy

These principles drive every structural decision in this workspace. Written so an AI agent can apply them to novel situations without additional clarification.

### 1.1 Consistent structure beats clever structure

**Rule:** Prefer repeatable schemas, naming, and view patterns over bespoke one-offs, even if the bespoke version is more elegant.

**Why this matters for AI:** Agents generalize from structural priors. One-off cleverness increases ambiguity, forces prompt-specific reasoning, and expands error surface area. A workspace that any collaborator — human or AI — can understand without a tutorial is worth more than a workspace that impresses its builder.

### 1.2 One source of truth, unlimited purposeful surfaces

**Rule:** Each entity type (Task, Project, Contact, Asset, etc.) has one canonical database. All dashboards are linked database views — filtered, sorted, grouped — of that source. No data lives in two places without a formal relation.

**Why this matters for AI:** Agents can only reliably act if there is a single authoritative record per entity. Duplicate trackers create silent divergence. The question "which one is right?" should never need to be asked.

### 1.3 Typed properties for structure; page bodies are mandatory context

**Rule:** If Notion has a native property type, use it. Reserve Text properties for genuinely unstructured narrative notes. Any data that has a native type — dates, people, status, relations, URLs, numbers — must use that type.

**Why this matters for AI:** Putting structured data in a Text field makes it invisible to filters, rollups, autofill, agents, and Q&A. Notion AI Autofill reads page body content, not file attachments. An entry with rich properties but an empty body is a low-context object — it produces unreliable AI output.

**Minimum viable page body standard (per row):**
- 3–5 sentences of context: purpose, constraints, current state
- A plain-language facts block: owner, dates, links, dependencies
- If the row will be acted on by an agent: a Definition of Done line

### 1.4 Build for the weekly review, not the launch day

**Rule:** A Notion OS survives via maintenance cadence, not cleverness. Optimize for structures that stay accurate when updated weekly, not structures that require constant attention to stay coherent.

**The failure mode to avoid:** Procrasti-planning — spending more time perfecting schema aesthetics than shipping outputs. If a workspace is more impressive to look at than to use, something has gone wrong.

### 1.5 Beauty + anti-rot as active requirements

**Rule:** Elegance and legibility are not aesthetic preferences — they are functional requirements. A system that grows chaotic as it scales has failed its mission. A system that decays because it isn't maintained has also failed. Anti-rot is built in: prune before it rots, maintain before it drifts, archive before it misleads.

**The test:** Can a new collaborator — human or AI — orient in this workspace within 60 seconds? Can you look at any database and immediately understand what it's for? If not, the system needs work before it needs more pages.

**→ See Notion Mission doc for the identity anchor behind this principle.**

---

## 2. How Notion Databases Actually Work

### 2.1 The mental model: rows are pages

A Notion database is a collection of pages.

- **Properties** = structured metadata used for filtering, sorting, grouping, permissions, rollups, and automation
- **Page body** = unstructured context used for human understanding and AI reasoning — specifically, this is what Autofill reads

This distinction is not cosmetic. It determines what AI can and cannot do with a given entry.

### 2.2 Database vs linked database vs view

| Concept | What it is | When to use it |
|---|---|---|
| **Source database** | The canonical dataset | One per entity type; lock once schema is stable |
| **Linked database** | A portal to a source database rendered elsewhere, with its own filters/sorts/grouping | Building dashboards, cross-page surfacing |
| **View** | A saved layout configuration within a source or linked database | Different angles on the same data |

**Operational default:** interact through linked views; reserve source database edits for schema maintenance.

### 2.3 Property types — when to use each, when NOT to use Text

| Property type | Use when | Never use Text instead when... |
|---|---|---|
| **Title** | Primary identifier | Always |
| **Status** | Workflows and lifecycle states | You need filtering by group |
| **Select / Multi-select** | Categories, tags, taxonomy | The value represents a workflow stage |
| **Date** | Due dates, start/end, events | You're tempted to store dates as strings |
| **Person** | Owners, assignees | You're tempted to store names as text |
| **Relation** | Connecting entity types | You're duplicating data across databases |
| **Rollup** | Aggregating across relations | A Formula 2.0 traversal would be simpler |
| **Formula** | Derived logic, flags, indicators | The value is static truth |
| **Checkbox** | Boolean state | You have more than two possible states |
| **Number** | Quantitative values | You're storing numbers in text |
| **Files & Media** | Attachments | You expect AI to read the file contents — it cannot |
| **URL / Email / Phone** | Structured contact info | You need to filter or aggregate on them |
| **Unique ID** | Stable references for agents and cross-DB ops | Agents need durable IDs to reference records |
| **Created time / Last edited** | Audit trail | You'd try to maintain this manually |

### 2.4 Relations and rollups — agent-friendly design

**Relations** define the graph your agents traverse.
- Prefer bidirectional relations (visible on both ends)
- Name relations explicitly: `Project`, `Client`, `Parent Task` — not `Linked to`
- Avoid mega-relations that try to connect everything to everything

**Rollups** are for aggregation: count, sum, min/max, percent complete.

**Performance rule (scale safety):**
- Avoid chains: formula → rollup → formula → rollup across large relations
- Prefer one rollup per metric (count/sum) + one formula for presentation logic
- Use Formula 2.0 direct relation traversal where feasible (see Section 5)
- Use boolean formula flags as filter handles — views filter on them cheaply

### 2.5 Database settings that matter at scale

Enable sub-items, dependencies, and sprints for task databases. Enable Unique ID with a meaningful prefix (e.g., `TASK-`, `PROJ-`) for any database agents will reference by record. Lock source databases once schema is stable to prevent accidental property additions, view drift, and configuration creep. Use the database description field — agents and Q&A read it.

### 2.6 Task databases and what the designation unlocks

Converting a database to a Task database (requires mapping Status, Assignee, and Due date properties) makes tasks appear in **Home → My Tasks** — Notion's cross-database task aggregation surface. Task databases also unlock Sub-tasks, Sprints, and Dependencies settings. This designation is for databases that contain actionable work items — not every database in the workspace.

---

## 3. The Status Property — Architecture and Why It Wins

### 3.1 Status is not "Select with different colors"

Status encodes workflow semantics. It is organized into three fixed macro-groups:

- **To-do**
- **In progress**
- **Complete**

You can add option values within each group. The macro-group labels themselves are fixed at the system level. This is intentional and is what makes Status powerful.

### 3.2 Why Status wins

**Group-aware filtering is future-proof.** Filtering by `Status group = In progress` automatically includes any new status options added under that group later. Filtering by a literal option name (`Status = "In Review"`) breaks the moment someone renames it.

**Group semantics are AI-native.** Agents reason over "Complete" as "done" without bespoke instruction. With Select, you must teach every agent what "Done / Closed / Shipped / Finished" means — and hope they're consistent.

**Rollup calculations against groups are durable.** A rollup calculating "percent of tasks with Status group = Complete" survives schema changes. A rollup against a specific option name does not.

### 3.3 The Status vs Select decision rule — non-negotiable

| Use Status for... | Use Select for... |
|---|---|
| Workflow stages and lifecycle | Categories, types, taxonomy |
| Anything that moves forward over time | Labels that describe, not progress |
| Anything an agent needs to act on based on state | Tags that filter but don't sequence |

If you're ever tempted to use Select for a workflow state, use Status instead.

---

## 4. Views — All Types, Layouts, and Filters

### 4.1 All view types and their best use

| View | Best use | Unique strength |
|---|---|---|
| **Table** | Ops and bulk editing | Fast dense editing across properties |
| **Board** | Workflow execution | Group by Status or assignee; drag between stages |
| **Timeline** | Scheduling and planning | Dependencies + date range visualization |
| **Calendar** | Time-based content/events | Editorial and event rhythm |
| **List** | Reading and triage | Minimal friction; fastest to scroll |
| **Gallery** | Visual catalogs and asset libraries | Cover images and previews |
| **Chart** | Analytics | Native charts — note: you cannot edit entries from chart view |
| **Feed** | Updates and async communication | Post-like browsing, comments, view tracking |
| **Map** | Location-based planning | Works with Place property; drop pins directly |

### 4.2 Layout builder and why it matters for AI

The Layout Builder standardizes page appearance across a database. Consistent page structure reduces variance in what agents must parse. It increases repeatability for Autofill prompts and agent actions. When every entry in a database has the same property arrangement and body structure, AI outputs become more predictable and reliable.

### 4.3 Filters — designing agent-safe surfaces

- Prefer simple predicate filters: Status group, Person, Date windows, boolean flags
- Use formula-generated boolean properties as filter handles (see Section 5)
- Encode complex conditions in formulas so views stay legible; avoid deeply nested filter logic as the first layer
- Filter by group (Status group = In progress) rather than by option name

---

## 5. Formulas — Practical Patterns and Formula 2.0

### 5.1 Formula 2.0 capabilities that matter

Formula 2.0 introduced functional patterns that eliminate most cases where formula → rollup → formula chains were previously required.

- **`lets()`** — declare variables within a formula to avoid repetition and improve readability
- **`.map()`** — transform arrays (relation results)
- **`.filter()`** — filter arrays inline without a separate rollup
- **Direct relation traversal** — `prop("Project").prop("Status")` retrieves a related record's property value without a rollup step

### 5.2 Universal formula patterns — copy/paste ready

```
// 1. Elapsed days since due date (positive = overdue)
dateBetween(now(), prop("Due Date"), "days")
```

```
// 2. Overdue flag — boolean filter handle for views
and(
  prop("Due Date") < now(),
  prop("Status") != "Complete"
)
```

```
// 3. Emoji status indicator — human-readable at a glance
if(
  prop("Status") == "Complete", "✅",
  if(prop("Status") == "In progress", "🔄", "⏳")
)
```

```
// 4. Relation completion percentage — adapt to your schema
lets(
  total, prop("Tasks").length(),
  done, prop("Tasks").filter(current.prop("Status") == "Complete").length(),
  if(total == 0, 0, round(done / total * 100))
)
```

```
// 5. Days until due — negative if overdue
dateBetween(prop("Due Date"), now(), "days")
```

### 5.3 Performance rule

Avoid chains where formulas depend on rollups that depend on formulas across large relations. At approximately 500+ rows, these chains produce noticeable slowdowns. Instead:
- Prefer one rollup per metric
- Use Formula 2.0 direct traversal to eliminate the rollup entirely
- Use boolean flag properties as cheap filter handles

---

## 6. Notion AI — Four Operational Modes

### 6.1 AI Autofill — hard constraints and design rules

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

**What Autofill reads:** Page body content only. It cannot reference text in file attachments, URL properties, or linked database entries.

**What property types it can populate:**
- **Text properties:** Summary, Key Info, Translate, Custom (your own prompt)
- **Select and Multi-select properties:** AI category assignment with optional "Generate new options"; supports "Auto-update on page edits" behavior

**The constraint that governs everything:** Empty or thin page body = empty or hallucinated output. There are no exceptions. Design page body standards before configuring Autofill.

### 6.2 Q&A — what it does well and where it fails

**Searches:** Page titles, body content, and connected apps (Slack, Google Drive, GitHub — Enterprise Search). Respects all workspace permissions.

**Does well:** Finding pages, surfacing information from rich body content, answering questions that are answered somewhere in the workspace.

**Fails reliably:** Structured database queries, counting records, aggregating values across a database, precise filtering. Q&A is not a database query engine — it is a content retrieval system.

**What improves Q&A results:** Recently updated pages, rich body content, descriptive and specific page titles.

### 6.3 Research Mode

When enabled, Notion AI blends workspace knowledge with web research. Treat outputs as draft material. Ground anything critical against primary sources. Availability varies by plan and rollout.

### 6.4 AI Meeting Notes

Transcription plus structured summaries and action items. Notion 3.1 added a dedicated Meetings tab in the sidebar — all meeting notes in one place, synced with calendar. Summaries link each takeaway to the exact transcript moment. Available on mobile with continuous transcription even when switching apps or locking screen (Notion 3.2).

---

## 7. Notion Agents — Architecture, Capabilities, Credits

### 7.1 The three agent types

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

| Agent type | What it is | Autonomy | Plan required |
|---|---|---|---|
| **Personal Agent** (Notion 3.0) | Interactive AI assistant; you prompt, it acts | Reactive — works when asked | Business / Enterprise AI |
| **Custom Agents** (Notion 3.3) | Autonomous agents on triggers/schedules; no prompting after setup | Fully autonomous, 24/7 | Business / Enterprise |
| **Notion Workers** | Developer-facing agent infrastructure; Vercel partnership | "Extreme pre-alpha" — not GA | TBD |

### 7.2 Personal Agent — capabilities

- Up to 20+ minutes of autonomous multi-step execution per session
- Works across hundreds of pages simultaneously
- Connected tools: Slack, Google Drive, Calendar, CSV files, page comments
- Customizable via an Instructions page
- Downloadable Skills (saved prompt templates for repeatable tasks)
- Full mobile parity as of Notion 3.2

**Model selection (as of March 2026):** GPT-5.2, Claude Opus 4.5, Gemini 3, Auto — plus Claude Opus 4.6 added in a later release. Context and memory persist across model switches.

### 7.3 Custom Agents — full capability picture

**Triggers:** Schedule (cron), Slack message, Email received, Database change

**Actions:** Read filtered records, create entries, update properties, append to page bodies, move pages, send Slack messages (read-and-reply permission model), send email, call REST APIs and webhooks, post to connected services via MCP

**Native integrations:** Slack, Notion Mail, Notion Calendar, Linear, Figma, HubSpot, custom MCP servers

**Cost-efficiency option:** MiniMax M2.5 — up to 10x more cost-efficient for basic tasks. Use for high-volume, simple workflow agents.

**Security:** Least-permissive by design. Granular per-agent permissions. Prompt injection detection. All runs logged. Agents auto-pause at credit limit.

### 7.4 Notion Credits — full pricing picture

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High | Watch: Goes live May 4, 2026`

⚠️ **Critical date: May 4, 2026**

| Period | Status | Cost |
|---|---|---|
| Now → May 3, 2026 | **Free beta** — unlimited agents, no credit limit | $0 |
| May 4, 2026 → | **Credits required** | $10 per 1,000 credits |

- Credits apply to Custom Agents only — Personal Agent, AI Meeting Notes, Enterprise Search remain included at no extra cost
- Credits are workspace-shared, reset monthly, no rollover
- Admins buy credits in-product or via Notion sales
- Usage dashboard with alerts at 80% and 100%; agents auto-pause at limit

**What to do before May 4:**
- Build and test all planned agents now
- Split broad agents into single-purpose agents (reduces per-run cost)
- Use MiniMax M2.5 for routine high-volume tasks
- Measure step count per run — leaner agents cost less

### 7.5 Agent design principles

**One agent, one job.** Broad agents fail on credit consumption, reasoning overhead, permissions blast radius, and error surface.

**Required agent infrastructure (minimum):**

| Component | Purpose |
|---|---|
| **Instructions page** | Scope, allowed actions, naming rules, always-on constraints |
| **Agent SOP page** | Operational rules: if X then Y, failure modes, validations |
| **Agent Log** | Audit trail: every run, what changed |
| **Agent Inbox** | Human review queue for uncertain actions |
| **Unique IDs** | Stable record references for reliable agent targeting |

---

## 8. MCP — Notion as Open AI Infrastructure

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

Notion's hosted MCP server implements the Model Context Protocol standard. External AI tools — Claude, ChatGPT, Cursor, v0 — connect via OAuth and read/write with the user's permissions.

**Strategic implication:** Building with consistent schemas and naming conventions makes this workspace operable by any MCP-capable AI tool, not just Notion AI. Workspace structure quality directly determines what external AI can do with it.

**MCP admin controls:** Granular admin controls over which external tools can connect are on the roadmap. Not yet released as of March 2026. `Watch: Upcoming`

---

## 9. The Suite Map — All Nine Documents

*Use this map to route any task or question to the correct document before acting.*

**Doc 0 — Central Hub (this document)**
Platform truth, agent rules, the full suite map, timeline, and decision framework. Load alongside any subject document as universal context.

**Doc 1 — Projects & Tasks System**
Complete architecture for managing work: projects, tasks, sub-tasks, sprint infrastructure, the four-database foundation, Status property architecture, Task database designation, Unique ID configuration, and agent SOP templates for project-level automation.

**Doc 2 — CRM: Contacts, Pipeline & Relationship Management**
Managing people, companies, deals, and relationship context. Contacts and Companies database architecture, pipeline stages as Status, AI Autofill for contact enrichment, and agent patterns for relationship maintenance.

**Doc 3 — AI Agents: Deep Dive**
*Primary focus document.* Full operational depth on Personal Agent, Custom Agents, and Workers. Trigger types, action types, agent infrastructure stack, credits mechanics, MiniMax cost-efficiency, prompt injection defense, MCP layer, agent templates, and build patterns.

**Doc 4 — Content & Publishing System**
Content creation end-to-end: Content Calendar database, Status-driven editorial workflows, Autofill for briefs and summaries, multi-platform publishing tracking, and agent patterns for content triage.

**Doc 5 — Notion as Wiki & Knowledge Base**
Building and maintaining a knowledge base at scale. Information architecture, Q&A optimization, database vs filtered view decision rule, naming conventions, workspace governance, and the weekly review survival mechanism.

**Doc 6 — Automations & Integrations Deep Dive**
Native automation engine + external connections. Button-triggered and property-triggered automations, automation vs agent decision logic, full MCP integration layer, native integrations (Slack, Jira, GitHub, Google Drive, Figma, Linear, HubSpot, Zapier via webhooks), and the API layer.

**Doc 7 — Art & Creative Production in Notion**
Managing visual art commissions, creative briefs, asset pipelines, collaborator relationships, and royalty/contract tracking. Asset Library database, commission tracking, brief templates with Autofill standards, file reference management, and agent patterns for deliverable tracking.

**Doc 8 — Partnering & Succeeding with Notion**
Building a public Notion presence toward a formal partnership. Template Marketplace, Ambassador Program, Solutions Partner track, Notion for Creators, pathway decision logic, and the content and community strategy.

---

## 10. Platform Version Timeline — Mid-2024 Through March 2026

`Source: Official Notion Releases + Help Center | Last verified: Mar 4, 2026`

| Date | Version | What shipped | Why it matters |
|---|---|---|---|
| **May 2025** | — | AI included in Business/Enterprise (no longer add-on for new Free/Plus users) | AI becomes standard infrastructure |
| **Mid-2025** | — | Enterprise Search across Slack, Google Drive, GitHub | Notion becomes cross-tool query surface |
| **Mid-2025** | — | Data Sources architecture; database row permissions | Foundation for granular permissions + modular dashboards |
| **Mid-2025** | — | Notion Mail + Notion Calendar | Full communication layer in workspace |
| **Mid-2025** | — | Formulas 2.0: `lets()`, `.map()`, `.filter()`, direct relation traversal | Eliminates most formula→rollup→formula chains |
| **Mid-2025** | — | Webhook automation actions (Zapier, Make, 1,000+ tools) | Native trigger-and-connect without code |
| **Mid-2025** | — | AI answers from GitHub (beta) | Engineering teams query PRs + issues + docs |
| **Sep 18, 2025** | **Notion 3.0** | Personal Agent — 20+ min autonomous execution, Instructions page, Skills | Agents become first-class workspace citizens |
| **Nov 17, 2025** | **Notion 3.1** | Agent updates (comments, calendar, private Slack, CSV), Meetings tab, Map view, conditional colors, 15% faster loads | Agent context expands; meetings get permanent home |
| **Nov 25, 2025** | — | Claude Opus 4.5 added to model options | Multi-model selection available |
| **Dec 2025** | — | Webhook actions, GitHub AI (beta), iOS Shortcuts, HIPAA AI, Monday.com importer | Automation + compliance expansion |
| **Jan 20, 2026** | **Notion 3.2** | Mobile AI parity, model picker (GPT-5.2 / Claude Opus 4.5 / Gemini 3 / Auto), People Directory, Jira bi-directional sync, enterprise AI analytics | Full agent on mobile; formal multi-model picker |
| **Feb 2026** | — | Library — full-page navigation surface | Keeps workspace navigable at scale |
| **Feb 24, 2026** | **Notion 3.3** | Custom Agents public beta — autonomous 24/7, team-shared, enterprise permissions, MCP integrations | Most significant release since 3.0 |
| **Mar 2, 2026** | — | Presentation mode | Any page becomes a deck; no separate tool needed |
| **Mar 3, 2026** | — | MiniMax M2.5 in agent model picker | Changes economics of high-volume agents |
| **Mar 2026** | — | Claude Opus 4.6 added to model options | Latest Anthropic model in workspace |
| **Mar 2026** | — | Notion Workers announced — "extreme pre-alpha" | Developer agent infrastructure via Vercel; not GA |

---

## 11. Naming, Consistency, and the AI-Readable Workspace

### 11.1 Property naming conventions

**Rule:** Use `[Domain] [Data]` or `[Verb] [Object]` format — pick one and enforce it globally.

| Good | Bad | Why |
|---|---|---|
| `Project Due Date` | `Date` | Ambiguous — which date? |
| `Task Owner` | `Owner` | No domain context for an agent |
| `Client Stage` | `Status` | Without domain, agents can't distinguish which Status |
| `Next Action Date` | `Info` | Meaningless to a filter or agent |
| `Contract Signed?` | `Signed` | Verb form makes boolean intent clear |

### 11.2 The consistency rule — character-for-character

Option values must be identical across every database where they're meant to be interoperable. `In Progress` ≠ `In progress` to a filter or agent. Pick one canonical form and enforce it.

### 11.3 Task and project naming

- **Tasks:** `Verb + specific deliverable` — `Draft sponsorship outreach email`
- **Projects:** `Outcome-oriented verb phrase` — `Launch Notion OS v1`
- **Never:** vague one-word names like `Meeting`, `Review`, `Follow up`

### 11.4 Page body minimum standard

Every actionable row acted on by AI must include: what it is, why it matters, current state, constraints or dependencies, Definition of Done.

---

## 12. Workspace Architecture Decision Framework

### 12.1 The database vs filtered view rule

**Create a new source database only when:**
- The entity has a distinct lifecycle and schema AND
- It will be referenced by ≥2 other systems AND
- It requires independent permissions, automation, or reporting

Otherwise: use a linked view or filtered view of an existing database.

### 12.2 Four databases is usually enough

Most professional systems stabilize around: Tasks, Projects, Contacts/CRM, Assets/Knowledge. When you feel the urge to create another source database, ask first whether a linked view answers the need.

### 12.3 Locking discipline

Lock source databases once schema is stable. Temporarily unlock when schema changes are intentional.

### 12.4 The procrasti-planning failure mode

More time perfecting schema than shipping work = the OS is failing. Remedies: weekly review, minimum viable schema constraint, ship-first rule.

### 12.5 Weekly review as the survival mechanism

Capture → triage → schedule. Normalize property values. Close loops. Prune dead views. Update Doc 0 when platform fundamentals change.

---

## 13. Agent Instruction Architecture — Instructions, Skills, SOPs

### 13.1 Instructions page — the agent's constitution

One active canonical Instructions page per agent. Contains: scope, allowed actions, naming rules, always-on constraints, escalation rules. Write in bullet blocks — machine-parseable. Only one Instructions page can be active at a time for the Personal Agent.

### 13.2 Skills — saved prompts as reusable primitives

Use for: repeatable transforms, structured writes, schema-safe actions.

### 13.3 SOP pages — operational runbook logic

SOP ≠ Instructions. SOPs define operational rules: if X then Y, failure modes, required validations.

**Universal SOP template:**

```
SOP: [Agent Name] — [Single Job]

Goal:
- One sentence outcome

Inputs (required):
- Source databases, views, trigger conditions

Allowed actions:
- Explicit whitelist: create / update / append / notify

Hard constraints (never violate):
- No deletes
- No permission changes
- No actions without logging

Validation checklist (before acting):
- [ ] Entity exists — no duplicate
- [ ] Required properties present
- [ ] Status group semantics respected
- [ ] Action is reversible or logged

Logging:
- Timestamp, trigger, affected pages, fields changed,
  summary, credits used, confidence, follow-up needed

Escalation:
- Low confidence or missing fields → Agent Inbox item + notify owner
```

---

## 14. Creator and Partner Ecosystem — Overview

*Full strategy in Doc 8.*

- **Solutions Partner Program** — enterprise implementation pathway; "Sell" and "Service" tracks with certifications
- **Ambassador Program** — community and education pathway; monthly contribution + cohort review
- **Rule:** Enterprise system builder → Solutions Partner. Creator/educator/template publisher → Ambassador. Both are viable simultaneously.

---

## 15. Upcoming Features and Platform Direction

### ⚠️ Credits go live May 4, 2026

Build and test all planned agents before May 4. See Section 7.4 for full mechanics and preparation steps.

### Notion Workers — extreme pre-alpha

`Source: Official (Jonathan Clem, X) | Confidence: High — exists; Low — timeline/pricing`

Not GA. Developer-facing agent infrastructure via Vercel. Signals Notion's long-term direction: agents as developer-buildable, first-class infrastructure. `Watch: May 2026 announcement`

### MCP admin controls

Granular admin controls for external AI tool connections. Not yet shipped. `Watch: Upcoming`

### May 2026 announcement

Notion has signaled a significant product announcement. No confirmed details. Watch official channels.

---

## 16. The Decision Framework — Where Does This Task Live?

| If the task involves... | Go to... |
|---|---|
| Projects, tasks, sprints, work management | Doc 1 |
| Contacts, companies, deals, relationships | Doc 2 |
| Building, configuring, or debugging an agent | Doc 3 |
| Content creation, editorial calendar, publishing | Doc 4 |
| Page organization, knowledge architecture, governance | Doc 5 |
| Automations, webhooks, API, external integrations | Doc 6 |
| Art commissions, creative briefs, asset tracking | Doc 7 |
| Notion partnership, public brand, template marketplace | Doc 8 |
| Why this suite exists, milestones, privacy model | **Notion Mission doc** |
| Platform truth, feature state, suite navigation | **Doc 0 — this document** |

---

## 17. Living Document Protocol

**Rebuild triggers:**
- Any new Notion major or minor version release
- May 4, 2026: Credits go live — update Section 7
- Notion Workers exits pre-alpha
- Any subject document added, retired, or substantially restructured
- A governing principle changes based on operational experience

**Who updates this document:** Workspace owner, or an AI agent explicitly authorized in writing. Agents do not update Doc 0 autonomously.

**Cross-reference:** → Notion Mission doc sets priorities for what Codex should capture next. If mission milestones shift, update the Codex accordingly.

**Version history:**
- v1.0 — Mar 4, 2026 — Initial build by Claude
- v2.0 — Mar 4, 2026 — Merged with ChatGPT draft; discrepancies verified; formulas, SOP template, corrected timeline added
- v2.1 — Mar 4, 2026 — Added three-layer architecture, Mission pointer, visibility model, truth model with source tags, beauty + anti-rot principle (1.5)

---

*Notion Master Reference Suite — Doc 0 v2.1 | March 4, 2026*
*Written with Claude. Version date is the reliability indicator.*
