# Notion Codex — Doc 4: Content & Publishing System
**Version 1.0 | March 4, 2026 | Living Document**
*Part of the Notion Master Reference Suite. Load Doc 0 alongside this document for foundational principles, property rules, and agent governance. This document builds on those — it does not re-explain them.*

---

## AI & Agent Instruction Block

**Document scope:** Everything needed to architect and operate a content creation and publishing system in Notion — Content Calendar database, editorial Status pipeline, per-format page body templates, AI Autofill configuration, multi-platform publishing tracking, repurposing architecture, agent SOPs for content coordination, and the honest boundary of what Notion handles natively vs. what requires external tooling.

**Load Doc 0 first for:** Status property architecture, Autofill page body constraints, naming conventions, agent infrastructure stack, universal SOP template.

**Load Doc 3 for:** Full Custom Agent and Autofill configuration depth. Doc 4 describes what to configure; Doc 3 explains how.

**Honest capability boundary — state this before building:**
Notion plans, writes, tracks, and coordinates content. It does not schedule posts to external platforms or auto-publish natively. Publishing to YouTube, Substack, Instagram, Twitch, TikTok, or any external destination requires either manual execution or a third-party integration (Make, Zapier, Buffer, or a Custom Agent with an MCP-connected publishing tool). This boundary shapes every decision in this document.

---

## Table of Contents

1. [System Architecture — What This System Is and Isn't](#1-system-architecture)
2. [Content Calendar Database — Schema](#2-content-calendar-database--schema)
3. [Editorial Status Pipeline](#3-editorial-status-pipeline)
4. [Page Body Templates by Content Type](#4-page-body-templates-by-content-type)
5. [AI Autofill Configuration](#5-ai-autofill-configuration)
6. [Views — What to Build and Why](#6-views)
7. [Multi-Platform and Repurposing Architecture](#7-multi-platform-and-repurposing-architecture)
8. [The Idea Layer — Capture and Backlog](#8-the-idea-layer)
9. [Linking Content to Projects and Campaigns](#9-linking-content-to-projects-and-campaigns)
10. [Agent SOP Templates](#10-agent-sop-templates)
11. [Common Mistakes](#11-common-mistakes)
12. [Living Document Protocol](#12-living-document-protocol)

---

## 1. System Architecture

### 1.1 What Notion owns in a content workflow

| Stage | Notion handles natively | External tool required |
|---|---|---|
| Ideation | Idea capture, tagging, prioritization, backlog review | — |
| Planning | Calendar scheduling, deadline tracking, assignments | — |
| Briefing | Brief templates, AI Autofill brief/summary generation | — |
| Drafting | Full writing environment per page | — |
| Review | Comments, status handoffs, version notes in page body | Real-time tracked changes if needed |
| Approval | Status workflow, assignee handoffs via automation | — |
| Publishing | Status and date tracking, URL logging after publish | Actual post scheduling to external platforms |
| Performance | Manual metric logging | Auto-pull from analytics (requires integration) |

### 1.2 One database or many — the decision

For most content systems, one Content Calendar database is the right answer. Separate databases for ideas, drafts, and published pieces introduce three schemas to maintain, remove unified pipeline views, and create duplicate records.

**Use one Content Calendar database when:**
- The same piece moves from idea → draft → published in a single entry
- You need a single calendar view across all content
- Your volume doesn't require contributor-level access segregation

**Consider a second Ideas database only when:**
- You have a very high-volume raw idea backlog that benefits from a minimal schema (title + note + tag, nothing else)
- You want to keep the calendar clean while ideas season in a separate holding area

Default: start with one database. Add a filtered Idea Backlog view before ever creating a second database.

---

## 2. Content Calendar Database — Schema

`Source: Official Notion help | Last verified: Mar 4, 2026`

### 2.1 Required properties

| Property | Type | Notes |
|---|---|---|
| `Content Title` | Title | Working title — specific enough to find in search without opening the entry |
| `Content Status` | Status | Editorial pipeline — see Section 3 for groups and options |
| `Content Type` | Select | Long-form / Short-form / Stream / Newsletter / Video / Social / Other |
| `Channel` | Multi-select | YouTube / Twitch / TikTok / Instagram / Newsletter / Substack / etc. |
| `Publish Date` | Date | Target or actual publish date — drives Calendar view |
| `Content Owner` | Person | Who is accountable for this piece reaching Published |
| `Content ID` | Unique ID (prefix: `CNT-`) | Stable agent reference — required for all agent SOPs |
| `Priority` | Select | High / Medium / Low |
| `Tags` | Multi-select | Topic tags for filtering, ideation engine, and agent categorization |

### 2.2 Recommended optional properties

| Property | Type | When to add |
|---|---|---|
| `Publish URL` | URL | Filled after publish — direct link to the live piece |
| `Campaign` | Relation → Projects DB | When content belongs to a launch, campaign, or project (see Section 9) |
| `Source Content` | Relation → self | Links repurposed/derivative pieces to their pillar (see Section 7) |
| `AI Summary` | Text (Autofill) | One-sentence summary of the page body — for scanning in table/calendar views |
| `AI Tags` | Multi-select (Autofill) | Topic tags auto-extracted from body |
| `Word Count / Runtime` | Number | Volume tracking for written or video content |
| `Notes` | Text | Context that doesn't belong in the page body |

### 2.3 Naming standard

Every content title must be specific enough to find and understand without opening the entry.

**Good:** `What is ONCE — mythology explainer (YouTube)`
**Bad:** `Video 3`

**Good:** `March newsletter — creator toolkit roundup`
**Bad:** `Newsletter`

Vague titles are invisible to agents and unsearchable by anyone who isn't the original author.

---

## 3. Editorial Status Pipeline

`Source: Official Notion Status property docs | Last verified: Mar 4, 2026`

### 3.1 Status groups and options

```
To-do:
  Idea        — captured, not yet evaluated
  Backlog     — viable, not yet scheduled or briefed

In progress:
  Briefed     — audience, angle, and format defined in page body
  Drafting    — active writing or production in progress
  In Review   — draft complete, awaiting feedback
  Approved    — feedback incorporated, cleared to schedule
  Scheduled   — date locked, content ready to publish

Complete:
  Published   — live; Publish URL added to record
  Archived    — will not publish; retained for reference
  Cancelled   — discontinued
```

**Why Scheduled lives in In progress:** A scheduled piece hasn't shipped. Filing it under Complete makes it invisible to active pipeline filters and can prevent agents from monitoring it.

**Why Archived is Complete:** Final disposition. Agents and active filters must never surface archived entries.

### 3.2 Stage ownership and "done" definitions

| Status | Who acts next | What "done" means to advance |
|---|---|---|
| Idea | Content Owner | Title captured, at least two sentences in page body |
| Backlog | Content Owner | Idea evaluated as viable, waiting for calendar window |
| Briefed | Writer | Audience, angle, format filled in page body Brief section |
| Drafting | Writer | Active production underway |
| In Review | Editor / Reviewer | Draft complete; reviewer tagged |
| Approved | Publisher | Feedback incorporated; cleared for publish date |
| Scheduled | Publisher | Exact publish date confirmed; content finalized |
| Published | — | Live; URL added to Publish URL property |

### 3.3 Native automation at key handoff points

These status-triggered automations require no AI reasoning — use native automations, not Custom Agents. No credits consumed. Full setup in Doc 6.

| Trigger | Action |
|---|---|
| Status changes to `In Review` | Assign Content Owner to the designated editor |
| Status changes to `Published` | Set today's date in a `Published On` Date property |
| Status changes to `Approved` and Publish Date is empty | Create a reminder or Agent Inbox item to set a date |

---

## 4. Page Body Templates by Content Type

Set these as **database page templates** — click the dropdown arrow next to New, then add a template per Content Type. When a writer creates a new entry and selects a type, the body pre-fills automatically.

This serves two functions: writers start from a scaffold instead of a blank page, and Autofill has enough structured content to produce useful output.

### 4.1 Long-form (article, essay, deep-dive video script)

```
## Brief
**Audience:** [Who specifically — what they already know, what they want]
**Goal:** [What this piece does — inform / change a belief / inspire action]
**Angle:** [The one surprising or contrarian thing this piece argues]
**Format:** [Approx word count or runtime / structure type]

## Outline
[Section headers with one-sentence descriptions]

## Draft
[Full content here]

## Edit Notes
[Feedback, revision history, decisions made]

## Publish Metadata
- Publish Date:
- Channel:
- URL (once live):
- SEO tags:
```

### 4.2 Short-form (social post, clip, thread)

```
## Brief
**Platform:** [Where this posts]
**Goal:** [Click / Share / Follow / Awareness]
**Hook:** [Write the first line before anything else]

## Draft
[Full post text]

## Assets Needed
[Images, clips, links]

## Notes
[Platform constraints — character limits, hashtags, aspect ratios]
```

### 4.3 Newsletter / email

```
## Brief
**Subject Line (draft):**
**Preview Text:**
**Core Message:** [One sentence — what does the reader take away]

## Structure
- Opening hook
- Main content section(s)
- CTA

## Draft
[Full email body here]

## Notes
[Send date / list segment / A/B variant plan if applicable]
```

### 4.4 Stream / live session

```
## Brief
**Session Goal:** [Creative or production goal for this stream]
**Opening Beat:** [First 2–3 minutes — exactly what you say and show]
**Content Beats:** [3–5 major moments or topics planned]
**Closing:** [Sign-off + next session tease]

## Technical Checklist
- [ ] YouTube title and description set
- [ ] Not made for kids confirmed
- [ ] OBS overlays loaded
- [ ] Chat enabled
- [ ] Restream destination verified

## Post-Stream Notes
[Learnings, clip timestamps, moments worth repurposing]
```

### 4.5 Video (pre-produced / edited)

```
## Brief
**Audience:**
**Hook (first 30 sec):**
**Core argument / payoff:**
**Target runtime:**

## Script / Outline
[Full script or section-by-section outline]

## Production Notes
[B-roll list, graphics needed, music, assets required]

## YouTube Metadata
- Title:
- Description:
- Tags:
- Thumbnail concept:

## Post-Publish Notes
[Performance observations, follow-up ideas]
```

---

## 5. AI Autofill Configuration

`Source: Official Notion help center | Last verified: Mar 4, 2026`

### 5.1 The constraint that governs everything

Autofill reads only the **page body** of each entry. Files attached via File property are not readable. External URLs are not fetched. Properties on the entry are not read — only body text.

This means: a content entry with only a title and a publish date gives Autofill nothing. The minimum viable body for useful Autofill output is a completed Brief section — Audience, Goal, and Angle must have real content, not placeholder text.

### 5.2 Recommended Autofill properties for the Content Calendar

| Property | Type | Configuration |
|---|---|---|
| `AI Summary` | Text | Fill: Summary — enable **Auto-update on page edits** (refreshes 5 min after changes) |
| `AI Tags` | Multi-select | Fill: Key Info — prompt: "Extract 3–5 topic tags that describe this content piece" — enable Generate new options |
| `AI Hook` | Text | Fill: Custom — prompt: "Write one punchy, specific hook sentence to grab a cold reader, based on the brief and angle on this page" — manual trigger only |
| `AI Open Items` | Text | Fill: Custom — prompt: "List any unresolved decisions, missing assets, or open questions that must be resolved before this piece can publish" — manual trigger at Approved stage |

**Why manual triggers for Hook and Open Items:** These properties are most useful at specific moments in the workflow (Hook when the brief is solid, Open Items when moving to Approved). Running them on every page edit wastes compute and produces outputs before the body is complete enough to reason from.

**Manual trigger:** Hover over the property value cell in the database and click the wand icon.

### 5.3 Where Autofill adds the most value in a content workflow

AI Summary in Table and Calendar views enables producers to scan 20+ active entries without opening each one — the most high-leverage use in a content calendar. AI Open Items at the Approved stage catches missed decisions before scheduling. AI Tags compound over time into a coverage map that reveals gaps in the content strategy.

---

## 6. Views

### 6.1 Recommended view set

| View | Type | Configuration | Purpose |
|---|---|---|---|
| **Calendar** | Calendar | Group by `Publish Date` | Primary — publishing rhythm and gaps at a glance |
| **Pipeline** | Board | Group by `Content Status` | Editorial workflow — where pieces are stuck |
| **My Content** | Table or Board | Filter: `Content Owner = Me`, Status group ≠ Complete | Personal queue |
| **Publishing Queue** | Table | Filter: Status = `Approved` or `Scheduled`, sort `Publish Date` asc | What's ready and in what order |
| **Idea Backlog** | Table | Filter: Status = `Idea` or `Backlog`, sort `Priority` desc | Weekly review — what to develop next |
| **By Channel** | Board | Group by `Channel` | Platform balance |
| **All Published** | Table | Filter: Status = `Published`, sort `Publish Date` desc | Complete archive |

### 6.2 Calendar is the primary view — set it as default

Every other view serves a specific workflow moment. Calendar tells you whether the publishing strategy is working at a rhythm level: gaps, overloads, channel imbalance. Set it as the default view of the database. Everything else lives in the view switcher.

---

## 7. Multi-Platform and Repurposing Architecture

### 7.1 One entry or many — the decision rule

**One entry, multiple Channel tags:** Use when the same content goes to multiple destinations without meaningful changes. A stream posted as-is to YouTube as a VOD — one entry, `Channel = Twitch, YouTube`.

**Separate entries per platform:** Use when the content is meaningfully different per destination — a 60-second edited clip, a newsletter write-up, and a Twitter thread derived from the same stream. Each derivative is its own entry with its own Status, Owner, and Publish Date.

The `Source Content` self-relation (a Relation property that points to other entries in the same database) links derivatives to their pillar. Don't build this relation until you have derivatives to link — add it when the repurposing workflow is active.

### 7.2 The repurposing waterfall

Documenting the planned waterfall architecture prevents ad-hoc repurposing and ensures nothing slips. A typical creator waterfall:

```
Pillar content (stream / long video / article)
    ├── Short-form clips (Shorts, Reels, TikTok)
    ├── Social posts (quotes, text summaries, carousels)
    ├── Newsletter section or standalone send
    └── Evergreen wiki page (if content has lasting reference value)
```

In practice: the pillar entry should be at or near Published before derivatives move past Drafted. The pillar is the source material — derivatives can't finalize without it.

### 7.3 The publishing gap — Notion's honest boundary

Notion tracks publishing status; it does not trigger publishing. Options to close the gap:

| Approach | What it does | Complexity |
|---|---|---|
| Manual | Execute the publish when piece is Scheduled | Zero setup — right for low volume |
| Buffer / Later | Social scheduling; paste content from Notion when Approved | Low setup |
| Make or Zapier | Status = Scheduled triggers webhook to push to Buffer, Substack, etc. | Medium |
| Custom Agent + MCP | Agent pushes via MCP-connected publishing tool when Status changes | Requires MCP integration for target platform |

Start manual. Automate when manual execution is the actual bottleneck, not before.

---

## 8. The Idea Layer

### 8.1 Capture protocol

Ideas enter as Content Calendar entries with Status = `Idea`. Required at capture: title + at least two sentences in the page body (what the piece is, who it's for). Nothing else required. The cost of capture must be lower than the cost of forgetting.

The Idea Intake Agent (Section 10) can append a structured development prompt to new Idea entries automatically, prompting the owner to develop the brief when they return to it.

### 8.2 Weekly backlog review

The Idea Backlog view is reviewed weekly:

1. Promote viable ideas to Backlog if they have a clear angle and a natural calendar window
2. Archive ideas older than 60 days with no movement — if they were going to be made, they would have been
3. Add new ideas captured during the week

A backlog with 200 stale entries is noise. 20 current, prioritized ideas is signal. Prune ruthlessly.

### 8.3 Where ideas come from — not this document

The ideation engine — audience pain points, credibility framework, angle methodology — lives in the BrandMaster, not the Codex. This document is the production system. The Codex does not generate the ideas; it catches and executes them.

---

## 9. Linking Content to Projects and Campaigns

When content serves a launch, campaign, or project, the `Campaign` Relation property links to the Projects database (Doc 1). This creates bidirectional visibility: content entries show which project they serve; project entries can show a rollup count of linked content pieces.

Inside each Project page body, add a linked view of the Content Calendar filtered to `Campaign = [this project]`. Every campaign gets a live content log with no manual aggregation.

Use this pattern for: product launches, partnership announcements, seasonal campaigns, onboarding content pushes, anything where content timing needs to align with project milestones.

---

## 10. Agent SOP Templates

Follow the universal SOP format from Doc 0 Section 13. Adapt database names to match your actual workspace before deploying.

### SOP 1: Weekly Publishing Queue Digest

```
SOP: Publishing Queue Digest — Weekly

Goal:
Every Monday morning, post a digest of all Approved or Scheduled
content for the coming 7 days to a designated Slack channel.

Inputs:
- Content Calendar filtered:
    Status = Approved OR Scheduled
    Publish Date within next 7 days

Allowed actions:
- Read Content Calendar
- Post to #[channel] in Slack

Hard constraints:
- Read only — no writes to Content Calendar
- If zero matches: post "No approved or scheduled content this week."
- Post format only: Title | Channel | Publish Date | Status | Owner — never include draft body content

Logging:
- Timestamp, piece count, Slack post confirmed

Escalation:
- Slack failure → Agent Inbox item with full digest

Model: MiniMax M2.5
```

### SOP 2: Stale Draft Monitor

```
SOP: Stale Draft Monitor

Goal:
Flag content entries in Drafting or In Review with no page edits
in 14+ days, for owner attention.

Inputs:
- Content Calendar filtered:
    Status = Drafting OR In Review
    Last edited > 14 days ago

Allowed actions:
- Append to page body: "[Agent] Stale flag [date]. Status: [status]. Last edited: [date]."
- Set Priority to High if not already High
- Create Agent Inbox item for Content Owner

Hard constraints:
- Do not change Content Status
- Do not notify externally — Agent Inbox only
- Only act on Drafting or In Review — never Idea, Backlog, or any Complete option

Validation:
- [ ] Content Owner exists — skip if missing, log to Inbox
- [ ] Last edited is confirmed page edit, not property-only update

Logging:
- Content ID, title, status, last edited date, actions taken, timestamp

Model: MiniMax M2.5
```

### SOP 3: Idea Intake Processor

```
SOP: Idea Intake Processor

Goal:
When a new Content Calendar entry is created with Status = Idea,
append a structured brief development prompt to the page body.

Inputs:
- Content Calendar trigger: new row created with Status = Idea

Allowed actions:
- Append intake prompt to page body (never overwrite existing content)
- Set Priority to Low if Priority is empty

Hard constraints:
- Do not change Status
- Append only — never replace or overwrite body content
- Only act on Status = Idea — ignore all other statuses

Validation:
- [ ] Entry created within last 60 minutes
- [ ] Intake prompt not already present in page body

Logging:
- Content ID, title, intake appended, timestamp

Model: MiniMax M2.5
```

**Intake prompt to append:**

```
## Idea Brief — Complete to Advance to Backlog
- [ ] Who is this for? (specific audience — what they know, what they want)
- [ ] What's the angle? (the one surprising or contrarian thing this argues or shows)
- [ ] What format fits this best?
- [ ] Is there a natural window — campaign, event, or season this belongs to?
- [ ] What makes you credible to make this piece?
```

---

## 11. Common Mistakes

| Mistake | What breaks | The fix |
|---|---|---|
| Separate databases for ideas, drafts, published | Three schemas to maintain; no unified pipeline view | One Content Calendar database, full Status lifecycle, filtered views per stage |
| Scheduled under Complete | Active unshipped content disappears from active filters | Scheduled stays In progress until the piece is live |
| No page body template per Content Type | Autofill hallucinated output; writers start from blank | Database page templates in the New dropdown — one per Content Type |
| Calendar as status-only tracker with no brief in body | Agents have nothing to reason about; Autofill useless | Every entry is a brief and draft — content lives in the page body |
| Select for Channel instead of Multi-select | Cross-posting to two platforms requires two entries | Multi-select Channel supports multiple destinations on one entry |
| No Source Content self-relation | Derivatives float unanchored; pillar-to-clip relationship invisible | Self-referential Relation property linking derivatives to pillar |
| Autofill with incomplete or missing brief | Summaries and hooks are empty or hallucinated | Minimum viable body: completed Brief section before running Autofill |
| Idea backlog left to rot | Signal-to-noise collapses; ideation review is demoralizing | Weekly prune — archive ideas older than 60 days with no movement |
| Automating publishing before proving manual is a bottleneck | Over-engineered solution for a low-volume problem | Start manual; automate when manual is the actual constraint |

---

## 12. Living Document Protocol

**Rebuild triggers:**
- Notion releases native social scheduling or publishing capabilities
- A new MCP integration enables direct publishing to a major platform
- Content workflow patterns change through active operational use
- Doc 7 (Art & Creative Production) is complete — cross-reference asset pipeline and visual brief standards

**Cross-references:**
- Doc 0: Status architecture, Autofill constraints, agent infrastructure, universal SOP template
- Doc 1: Projects & Tasks — Campaign relation; content linked to project timelines
- Doc 3: AI Agents — Custom Agent build depth, MiniMax M2.5 model selection, credits
- Doc 6: Automations & Integrations — native automations for status handoffs; external publishing via Zapier/Make
- Doc 7: Art & Creative Production — asset pipeline, visual brief standards for content entries
- BrandMaster: Ideation engine, audience framework, angle methodology

**Version history:**
- v1.0 — March 4, 2026 — Initial build by Claude. Verified against official Notion content calendar guidance and AI Autofill help center. Source-tagged throughout.

---

*Notion Codex — Doc 4: Content & Publishing System | v1.0 | March 4, 2026*
*Load Doc 0 alongside this document. Written with Claude.*
