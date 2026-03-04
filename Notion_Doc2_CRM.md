# Notion Codex — Doc 2: CRM: Contacts, Pipeline & Relationship Management
**Version 1.0 | March 4, 2026 | Living Document**
*Part of the Notion Master Reference Suite. Load Doc 0 alongside this document for foundational principles, property rules, and agent governance.*

---

## AI & Agent Instruction Block

**Document scope:** Everything needed to architect a CRM system in Notion — contacts, companies, deals, interaction history, pipeline management, and relationship maintenance. This document does not re-explain concepts covered in Doc 0.

**Load Doc 0 first for:** governing principles, Status property architecture, property type rules, Autofill constraints, agent infrastructure requirements, naming conventions.

**What agents can do with this document:** Build or audit a Contacts/Companies/Deals database system, configure pipeline stages, set up relationship views, write agent SOPs for contact enrichment and follow-up automation.

**What agents must not do without explicit instruction:** Delete contact records, modify pipeline stages, send communications on behalf of the workspace owner, or alter company records.

**Honest scope statement:** Notion CRM is powerful for relationship context, pipeline visibility, and AI-enriched contact management. It is not a replacement for dedicated CRM platforms (HubSpot, Salesforce, Attio) when you need email tracking, call logging, or complex automation at scale. Use this document to build the right Notion CRM for the right use case.

---

## Table of Contents

1. [The CRM Database Architecture](#1-the-crm-database-architecture)
2. [Contacts Database — Schema & Standards](#2-contacts-database--schema--standards)
3. [Companies Database — Schema & Standards](#3-companies-database--schema--standards)
4. [Deals Database — Schema & Standards](#4-deals-database--schema--standards)
5. [Interactions Database — When to Add It](#5-interactions-database--when-to-add-it)
6. [Pipeline Stages as Status — Non-Negotiable](#6-pipeline-stages-as-status--non-negotiable)
7. [AI Autofill for Contact Enrichment](#7-ai-autofill-for-contact-enrichment)
8. [Views — The Right View for Every CRM Context](#8-views--the-right-view-for-every-crm-context)
9. [Page Body Standards for Contacts and Deals](#9-page-body-standards-for-contacts-and-deals)
10. [Agent SOP Templates — Relationship Maintenance](#10-agent-sop-templates--relationship-maintenance)
11. [Common Mistakes and How to Avoid Them](#11-common-mistakes-and-how-to-avoid-them)
12. [Living Document Protocol](#12-living-document-protocol)

---

## 1. The CRM Database Architecture

### 1.1 The decision ladder

Not every CRM needs every database. Build in order and only add the next layer when the current one is genuinely operational.

| Layer | Databases | When to build |
|---|---|---|
| **Minimal** | Contacts only | Solo operators, personal relationship management, small networks |
| **Standard** | Contacts + Companies | Any context where contacts represent organizations, clients, or employers |
| **Full** | Contacts + Companies + Deals | Active sales pipeline, partnership development, recurring business cycles |
| **Extended** | All above + Interactions | When interaction history is business-critical and must be structured, not just in page bodies |

**The default starting point for most use cases:** Contacts + Companies. The relation between them does the work that most people try to solve with a third database.

### 1.2 How the databases relate

```
Companies ──── (one Company : many Contacts) ──── Contacts
    │                                                   │
    └────────── (Company has many Deals) ─────────── Deals
                                                        │
                                             (Deal relates to Contacts)
```

A Contact belongs to one Company. A Company can have many Contacts and many Deals. A Deal links to a Company and optionally to one or more Contacts (the humans involved in that deal). Interactions link to a Contact and optionally to a Deal.

### 1.3 What Notion CRM does well vs. what it doesn't

**Does well:**
- Centralized relationship context — everything about a person or company in one page
- Pipeline visibility — Board view grouped by pipeline stage
- AI-enriched contact records — Autofill summaries and key info from page body notes
- Cross-system relations — linking contacts to projects, tasks, content, assets
- Flexible views — filter by relationship type, geography, stage, owner

**Does not do well:**
- Email open/click tracking without a third-party integration
- Automated email sequences from inside Notion
- Real-time sync with external CRM data
- Complex deal forecasting and reporting at scale

**Decision rule:** If you need email tracking, call logging, or multi-stage automation at scale, use Notion as the knowledge and context layer alongside a dedicated CRM. If you need a structured, AI-enriched contact and pipeline system that lives inside your workspace, build here.

---

## 2. Contacts Database — Schema & Standards

### 2.1 Required properties

| Property | Type | Purpose |
|---|---|---|
| `Contact Name` | Title | Full name — Last, First for sortability |
| `Contact Status` | Status | Relationship lifecycle state — see groups below |
| `Company` | Relation → Companies DB | Employer or affiliated organization |
| `Role` | Text | Job title or role within the company |
| `Email` | Email | Primary contact email |
| `Phone` | Phone | Primary phone number |
| `Contact Type` | Multi-select | Client / Prospect / Partner / Vendor / Collaborator / Personal |
| `Contact Owner` | Person | Who manages this relationship in the workspace |
| `Contact ID` | Unique ID (prefix: `CON-`) | Stable reference for agents |
| `Last Contacted` | Date | Most recent outreach or interaction |
| `Next Follow-up` | Date | Scheduled next touch |

### 2.2 Status groups for Contacts

```
To-do:        New, To Qualify
In progress:  Active, Nurturing, In Negotiation
Complete:     Client, Inactive, Archived
```

This encoding means "pipeline position" for prospects and "relationship state" for active contacts. Both are served by the same Status property — which is the point.

### 2.3 Recommended optional properties

| Property | Type | When to add |
|---|---|---|
| `LinkedIn` | URL | When outreach or social context matters |
| `Location` | Select or Text | When geography is relevant to segmentation |
| `Source` | Select | Referral / Event / Inbound / Cold / Personal — how the contact entered the network |
| `Deals` | Relation → Deals DB | When using the Deals database |
| `Tags` | Multi-select | Freeform additional categorization |
| `AI Summary` | Text (Autofill) | Auto-generated summary of contact page body — see Section 7 |
| `AI Next Action` | Text (Autofill) | Custom prompt extracting recommended next step from notes |

### 2.4 Splitting First Name / Last Name

Split `First Name` (Text) and `Last Name` (Text) from the Title property if you plan to use Autofill or agent actions to generate personalized outreach. The Title `Contact Name` remains the primary identifier; First/Last are supporting fields for message generation.

---

## 3. Companies Database — Schema & Standards

### 3.1 Required properties

| Property | Type | Purpose |
|---|---|---|
| `Company Name` | Title | Organization name |
| `Company Status` | Status | Relationship lifecycle — see groups below |
| `Company Type` | Multi-select | Client / Prospect / Partner / Vendor / Investor |
| `Industry` | Select | Sector or vertical |
| `Website` | URL | Primary web presence |
| `Contacts` | Relation → Contacts DB | All people associated with this company |
| `Deals` | Relation → Deals DB | All deals associated with this company |
| `Company Owner` | Person | Who owns this relationship |
| `Company ID` | Unique ID (prefix: `CO-`) | Stable agent reference |
| `Contact Count` | Rollup → Contacts (Count) | How many contacts are associated |

### 3.2 Status groups for Companies

```
To-do:        New, Researching
In progress:  Active, Engaged, In Negotiation
Complete:     Partner, Client, Inactive, Archived
```

### 3.3 Recommended optional properties

| Property | Type | When to add |
|---|---|---|
| `Deal Value` | Rollup → Deals (Sum of Value) | When tracking revenue pipeline by company |
| `Company Notes` | Text | Brief freeform context if it won't go in the page body |
| `HQ Location` | Select or Text | When geography matters to segmentation |
| `AI Summary` | Text (Autofill) | Auto-generated summary from company page body |

---

## 4. Deals Database — Schema & Standards

`Source: Official Notion CRM guidance + community patterns | Last verified: Mar 4, 2026 | Confidence: High`

### 4.1 When to add the Deals database

Add Deals when:
- You conduct your sales or partnership cycle multiple times with the same company
- You need to track deal value, close probability, or revenue by deal (not just by company)
- Multiple people from the same company are involved in different deals

Skip Deals when:
- You have one ongoing engagement per company — use the Company record itself as the pipeline unit
- The "deal" is a project — use the Projects database (Doc 1) instead

### 4.2 Required properties

| Property | Type | Purpose |
|---|---|---|
| `Deal Name` | Title | Short descriptor: `[Company] — [Opportunity]` |
| `Deal Status` | Status | Pipeline stage — see groups below |
| `Deal Value` | Number | Estimated or confirmed value |
| `Company` | Relation → Companies DB | Parent company |
| `Contacts` | Relation → Contacts DB | Humans involved in this deal |
| `Close Date` | Date | Target or actual close date |
| `Deal Owner` | Person | Who owns this deal |
| `Deal ID` | Unique ID (prefix: `DEAL-`) | Stable agent reference |
| `Probability` | Number (%) | Likelihood of close — used in weighted pipeline math |

### 4.3 Status groups for Deals — the pipeline

```
To-do:        Lead, Qualified
In progress:  Proposal Sent, In Negotiation, Contract Out
Complete:     Closed Won, Closed Lost, On Hold
```

**Why this matters:** Filtering `Deal Status group = In progress` gives you your live pipeline instantly — all deals actively in motion — regardless of which specific stage they're in. This filter never breaks when you rename a stage.

### 4.4 Weighted pipeline formula

```
// Estimated weighted deal value
prop("Deal Value") * (prop("Probability") / 100)
```

Create a `Weighted Value` Number formula property using this. Roll it up in the Companies database to see weighted pipeline by company.

---

## 5. Interactions Database — When to Add It

### 5.1 What the Interactions database is for

An Interactions database creates a structured log of every meaningful touchpoint — calls, emails, meetings, notes — linked back to the Contact and Deal it belongs to.

**Add it when:**
- Multiple people in your workspace are managing the same relationships and need a shared interaction history
- You need to filter or report on interaction frequency or type
- You want agents to act on interaction history (e.g., "flag contacts with no interaction in 30 days")

**Skip it when:**
- You are a solo operator — put interaction notes in the Contact page body
- Your interaction volume is low — structured logs add overhead that doesn't pay off

### 5.2 Minimal Interactions schema

| Property | Type | Purpose |
|---|---|---|
| `Interaction Title` | Title | `[Date] [Type] with [Contact Name]` |
| `Interaction Type` | Select | Call / Email / Meeting / Note / Demo / Event |
| `Interaction Date` | Date | When it happened |
| `Contact` | Relation → Contacts DB | Who this interaction was with |
| `Deal` | Relation → Deals DB | Which deal it relates to (optional) |
| `Summary` | Text (Autofill) | AI-generated summary from page body |
| `Next Step` | Text | Specific follow-up action identified |
| `Interaction Owner` | Person | Who conducted the interaction |

---

## 6. Pipeline Stages as Status — Non-Negotiable

This rule from Doc 0 is especially important in CRM contexts because the temptation to use Select for pipeline stages is strongest here.

**Why it matters for CRM specifically:**

Pipeline views (Board grouped by stage) work the same whether you use Status or Select. The difference shows up in three places:

1. **Filtering across stages:** `Deal Status group = In progress` captures your entire active pipeline in one filter. With Select, you must name every active stage explicitly — and update every filter when you add a stage.

2. **Agent reasoning:** An agent tasked with "summarize all active deals" can use `Status group = In progress` reliably. With Select, you must teach the agent which option names mean "active" — and update the agent instruction every time a stage is renamed.

3. **Rollups in the Companies database:** Rolling up "count of active deals per company" works durably with group semantics. With Select, the rollup breaks if you rename a stage.

**Decision:** Use Status for pipeline stages. Always.

---

## 7. AI Autofill for Contact Enrichment

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

### 7.1 What Autofill can do for a CRM

Autofill reads the **page body** of a Contact, Company, or Deal record and populates structured properties automatically. In a CRM context, this means:

- **AI Summary:** Generates a 2–4 sentence overview of the contact from body notes — useful in Table view to scan without opening each record
- **Key Info:** Extracts specific facts from body notes — decision-making authority, interests, history, constraints
- **Custom prompt:** Generates tailored outputs — "What is the recommended next action for this contact based on these notes?" or "Summarize the current deal status and blockers."

### 7.2 Hard constraints (repeated from Doc 0 — critical here)

- Autofill **only reads page body content** — not file attachments, not URL properties, not linked database entries
- Empty or thin page body = empty or hallucinated output. No exceptions.
- Autofill does **not** read information from the web — it cannot enrich contact records with external data on its own

### 7.3 The practical implication: write in the body

The system only works if contact records have substantive page body notes. A contact record with a name, email, and company but an empty page body will produce nothing useful from Autofill.

**Minimum viable contact body:**
- How you know them
- What they do / what they care about
- History of interactions or context
- Any relevant constraints or preferences
- Current status of the relationship

### 7.4 Recommended Autofill properties for Contacts

| Property | Type | Autofill prompt |
|---|---|---|
| `AI Summary` | Text | Summary — 2-3 sentences summarizing who this contact is and the current state of the relationship |
| `AI Next Action` | Text | Custom: "Based on the interaction history and notes in this page, what is the single most important next action for this relationship?" |
| `AI Key Context` | Text | Key Info — extract the most important facts about this contact relevant to ongoing engagement |

**Auto-update on page edits:** Enable this for `AI Summary` — it will refresh 5 minutes after page body changes, keeping the summary current without manual intervention.

---

## 8. Views — The Right View for Every CRM Context

### 8.1 Contacts database — recommended views

| View | Configuration | Purpose |
|---|---|---|
| **Table — All** | All properties, sort by `Last Contacted` asc | Full contact list with staleness visibility |
| **Board — Pipeline** | Group by `Contact Status` | See contacts by relationship stage |
| **My Contacts** | Filter: `Contact Owner = Me`, Status group ≠ Complete | Personal relationship list |
| **Follow-up Due** | Filter: `Next Follow-up` ≤ today, Status group = In progress | Today's relationship maintenance queue |
| **By Type** | Group by `Contact Type` | Segment by client, prospect, partner, etc. |
| **New Contacts** | Filter: `Contact Status = New`, sort by `Created time` desc | Triage queue for incoming contacts |

### 8.2 Companies database — recommended views

| View | Configuration | Purpose |
|---|---|---|
| **Board — Stage** | Group by `Company Status` | Pipeline visibility by company |
| **Table — Active** | Filter: Status group = In progress, sort by `Last Contacted` asc | Active company relationships sorted by staleness |
| **By Industry** | Group by `Industry` | Segment by sector |

### 8.3 Deals database — recommended views

| View | Configuration | Purpose |
|---|---|---|
| **Board — Pipeline** | Group by `Deal Status` | Live sales pipeline — the primary CRM view |
| **Table — Active** | Filter: Status group = In progress, sort by `Close Date` asc | All active deals sorted by close date |
| **Closing Soon** | Filter: `Close Date` within next 14 days, Status group = In progress | Near-term close focus |
| **Won / Lost** | Filter: Status group = Complete | Retrospective and pattern analysis |

### 8.4 Linked views inside Contact and Company pages

**Inside each Contact page:** Add a linked view of the Deals database filtered to `Contacts contains [this contact]`. This gives immediate visibility into every deal a contact is associated with without leaving their record.

**Inside each Company page:** Add linked views of both the Contacts and Deals databases filtered to that company. Every company page becomes a full relationship command center — who works there, what deals are in flight, all in one place.

---

## 9. Page Body Standards for Contacts and Deals

### 9.1 Contact page body template

```
## Who They Are
[How you know them, what they do, what they care about]

## Relationship History
[Timeline of key interactions, decisions made, commitments given]

## Current Context
[Where things stand right now — active asks, active opportunities, active projects]

## Key Facts
[Anything that affects how to engage: constraints, preferences, sensitivities, interests]

## Next Step
[The specific next action — if known]
```

### 9.2 Company page body template

```
## What This Company Is
[What they do, why they matter to this workspace]

## Relationship Context
[How the relationship started, current state, key people involved]

## Active Opportunities / Deals
[Summary of anything in motion — link to deals if using Deals DB]

## Key Facts
[Constraints, preferences, budget signals, decision-making structure]
```

### 9.3 Deal page body template

```
## What This Deal Is
[What we're trying to accomplish and for whom]

## Current Status
[Where we are in the process — most recent development]

## Key Stakeholders
[Who is involved, who makes the decision, who influences it]

## Blockers / Risks
[What could prevent this from closing]

## Definition of Closed Won
[Exactly what "won" looks like — signed contract, PO received, meeting confirmed, etc.]
```

---

## 10. Agent SOP Templates — Relationship Maintenance

These SOPs follow the universal template from Doc 0 Section 13.

### SOP 1: Stale Contact Flagging

```
SOP: Stale Relationship Agent — Follow-up Flagging

Goal:
- Flag contacts in Active or Nurturing status where Last Contacted
  is more than 30 days ago and no Next Follow-up date is set

Inputs:
- Contacts database, filtered:
  Contact Status = Active OR Nurturing
  Last Contacted < 30 days ago
  Next Follow-up is empty

Allowed actions:
- Set Next Follow-up to today + 3 days
- Append to page body: "[Agent] Flagged stale on [date]. Last contacted: [date]."

Hard constraints:
- Do not change Contact Status
- Do not send any communication
- Do not act on contacts where Contact Status = Archived or Inactive
- Always log before acting

Validation checklist:
- [ ] Confirm Last Contacted is actually empty or past threshold (not a timezone artifact)
- [ ] Confirm Contact Owner exists before flagging
- [ ] Confirm Status is Active or Nurturing — not Complete group

Logging:
- Agent Log: contact ID, name, Last Contacted date, Next Follow-up set to, timestamp

Escalation:
- If Contact Owner is missing → skip, add to Agent Inbox
```

### SOP 2: Daily Follow-up Digest

```
SOP: Follow-up Digest Agent — Daily Relationship Queue

Goal:
- Post a summary of all contacts and deals where follow-up is due
  today or overdue to a designated Slack channel or Notion inbox page

Inputs:
- Contacts database: Next Follow-up ≤ today, Status group = In progress
- Deals database: Close Date ≤ 14 days, Status group = In progress

Allowed actions:
- Read Contacts and Deals databases
- Post formatted digest to Slack OR append to a designated Notion inbox page

Hard constraints:
- Read only — no writes to Contacts or Deals
- If 0 items match, post: "No follow-ups due today."

Validation checklist:
- [ ] Both databases queried before posting
- [ ] Message includes: Name, Type (Contact/Deal), Due Date, Owner

Logging:
- Agent Log: timestamp, counts (contacts due, deals due), post confirmed

Escalation:
- If post fails → create Agent Inbox item with full digest content
```

### SOP 3: New Contact Intake

```
SOP: Contact Intake Agent — New Contact Triage

Goal:
- When a new Contact row is created with Status = New, prompt
  enrichment by appending a structured intake prompt to the page body

Inputs:
- Contacts database, trigger: new row created with Contact Status = New

Allowed actions:
- Append intake prompt to page body (does not overwrite existing content)
- Set Next Follow-up to today + 7 days if empty

Hard constraints:
- Do not change any properties other than Next Follow-up
- Do not delete or overwrite existing page body content
- Always append, never replace

Validation checklist:
- [ ] Confirm row is genuinely new (Created time within last 60 minutes)
- [ ] Confirm page body does not already contain the intake prompt

Logging:
- Agent Log: contact ID, name, intake appended, follow-up set

Escalation:
- If Contact Owner is missing → add to Agent Inbox: "New contact [name] has no owner assigned."
```

**Intake prompt template to append:**

```
## Intake — Please Complete
- [ ] How did you meet this person?
- [ ] What do they do / what do they care about?
- [ ] What's the context for this relationship?
- [ ] What's the immediate next step?
- [ ] Any key facts (constraints, preferences, sensitivities)?
```

---

## 11. Common Mistakes and How to Avoid Them

| Mistake | What goes wrong | The fix |
|---|---|---|
| Using Select for pipeline stages | Group filtering breaks; agents can't reason about state; rollups break on rename | Switch to Status; map all pipeline stages to correct groups |
| Building Deals before Contacts + Companies are operational | Orphaned deals with no relational context; maintenance overhead | Stabilize Contacts + Companies first |
| Empty contact page bodies | Autofill produces nothing; agents have no context | Enforce page body template at creation; add intake prompt via agent |
| Storing interaction history only in page bodies | Can't filter by interaction type or date; agents can't surface patterns | Add Interactions DB when volume or multi-user context warrants it |
| One Contacts DB entry per role rather than per person | Duplicate records when people change jobs; broken relations | One row per person; update Company relation when they move |
| Rollup using option name instead of Status group | Breaks on stage rename | Use `Status group = In progress` in all rollups |
| Merging Contacts and Companies into one database | Structural mismatch; can't roll up company-level metrics; can't relate one company to many contacts cleanly | Keep them separate; use the Relation property |
| Not setting Contact Owner | Agents can't route; follow-up responsibility is unclear | Make Contact Owner a required field in the page template |

---

## 12. Living Document Protocol

**Rebuild triggers:**
- Notion releases native email integration or CRM-specific features
- HubSpot or another CRM platform releases deep Notion MCP integration
- A Contacts or Deals pattern is validated (or invalidated) through operational experience

**Cross-references:**
- Doc 0 Section 3 — Status property architecture (pipeline stages)
- Doc 0 Section 6 — AI Autofill constraints
- Doc 0 Section 7 — Agent infrastructure (Instructions, SOP, Log, Inbox)
- Doc 0 Section 13 — Universal SOP template
- Doc 1 — Projects & Tasks (for linking deals to projects)
- Doc 3 — AI Agents deep dive (for building agents that operate on this system)
- Doc 6 — Automations & Integrations (for HubSpot native integration and webhook-based CRM sync)

**Version history:**
- v1.0 — March 4, 2026 — Initial build by Claude. Verified against official Notion CRM guidance and help documentation.

---

*Notion Codex — Doc 2: CRM | v1.0 | March 4, 2026*
*Load Doc 0 alongside this document. Written with Claude.*
