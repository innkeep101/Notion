# Notion Codex — Doc 5: Notion as Wiki & Knowledge Base
**Version 1.0 | March 4, 2026 | Living Document**
*Part of the Notion Master Reference Suite. Load Doc 0 alongside this document for foundational principles, property rules, and agent governance.*

---

## AI & Agent Instruction Block

**Document scope:** Building and maintaining a knowledge base at scale in Notion — information architecture, the Wiki feature, page verification and ownership, Q&A optimization, naming conventions, workspace governance, and the weekly review survival mechanism. This document does not re-explain concepts covered in Doc 0.

**Load Doc 0 first for:** governing principles, naming conventions, Q&A capabilities and limitations, the legibility standard, anti-rot as a design requirement.

**What agents can do with this document:** Audit existing wiki and knowledge base structures for decay, flag unverified or expired pages, draft missing page body content, write agent SOPs for knowledge base maintenance.

**What agents must not do without explicit instruction:** Delete pages, restructure the top-level workspace hierarchy, change page verification status, or modify permissions.

**Honest scope statement:** Notion is a powerful, flexible knowledge base for teams and individuals who need structure + freedom. It is not a purpose-built documentation platform — it lacks native version control, content approval workflows, and customer-facing help center tooling. This document covers what Notion does well natively and notes where its limits matter.

---

## Table of Contents

1. [Knowledge Base vs. Wiki — What Notion Means by Each](#1-knowledge-base-vs-wiki--what-notion-means-by-each)
2. [The Wiki Feature — How It Actually Works](#2-the-wiki-feature--how-it-actually-works)
3. [Page Verification & Ownership — The Anti-Rot Engine](#3-page-verification--ownership--the-anti-rot-engine)
4. [Information Architecture — How to Structure a Knowledge Base](#4-information-architecture--how-to-structure-a-knowledge-base)
5. [The Library View — Navigating at Scale](#5-the-library-view--navigating-at-scale)
6. [Q&A Optimization — Making AI Search Work](#6-qa-optimization--making-ai-search-work)
7. [Naming Conventions for Knowledge Base Pages](#7-naming-conventions-for-knowledge-base-pages)
8. [Page Body Standards for Knowledge Base Entries](#8-page-body-standards-for-knowledge-base-entries)
9. [Governance — Who Owns What and When It Gets Reviewed](#9-governance--who-owns-what-and-when-it-gets-reviewed)
10. [The Weekly Review — Survival Mechanism](#10-the-weekly-review--survival-mechanism)
11. [Agent SOP Templates — Knowledge Base Maintenance](#11-agent-sop-templates--knowledge-base-maintenance)
12. [Common Mistakes and How to Avoid Them](#12-common-mistakes-and-how-to-avoid-them)
13. [Living Document Protocol](#13-living-document-protocol)

---

## 1. Knowledge Base vs. Wiki — What Notion Means by Each

These terms are often used interchangeably, but they describe different things in the context of Notion architecture.

**Knowledge base (broad):** Any organized collection of pages, databases, and linked content that answers questions and documents how things work. The structure can be flat, hierarchical, or relational. Most Notion workspaces are knowledge bases in this sense.

**Wiki (Notion feature):** A specific page type in Notion with built-in database-like views of its child pages, a scoped search function, and native support for page ownership and verification. A wiki is a mechanism for organizing and governing a knowledge base — it is not the knowledge base itself.

**The relationship:** A well-governed knowledge base will typically use the Wiki feature for its top-level navigation page and for high-churn documentation sections that need verification and ownership tracking. Simpler reference material can live as standard pages or databases without wiki treatment.

---

## 2. The Wiki Feature — How It Actually Works

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

### 2.1 What a wiki is in Notion

A wiki is a **page** (not a database) that has been converted to wiki format. This gives it:

- **Database-like views of its child pages** — the pages nested inside it behave like rows in a database, with views (Home, All Pages, Pages I Own) and the ability to create additional filtered/sorted views
- **A scoped search button** — clicking Search at the top-right searches only within the wiki, not the whole workspace
- **Native Verification and Owner properties** on each child page

**Critical constraint:** Only pages can be converted to wikis. Databases cannot be converted to wikis. This means the Wiki feature applies to the navigation and documentation layer of a knowledge base, not to structured data stores.

### 2.2 The three default wiki views

| View | What it shows |
|---|---|
| **Home** | A freeform page view — add any blocks, headings, callouts, descriptions. This is the wiki's landing page. |
| **All Pages** | Database view of every child page in the wiki — filterable, sortable, groupable |
| **Pages I Own** | Database view filtered to pages where the current user is the designated Owner |

Additional views can be created (timeline by verification date, gallery, filtered by category, etc.) just like a linked database.

### 2.3 How the Home view works

The Home view is the wiki's editorial control panel. Use it to:
- Explain what the wiki contains and who it's for (callout block at the top)
- Organize child pages under headings by category (drag and drop pages under headings)
- Surface critical or recently updated pages via linked views

This is different from every other Notion page type because it combines freeform block editing with live database views of child pages. Most users underuse it — it should be the best-designed page in the knowledge base.

### 2.4 Scoped wiki search

The Search button in the wiki's top-right corner opens Notion's standard search interface, pre-filtered to the current wiki. This is significantly more relevant than workspace-wide search when the workspace has hundreds of pages. Users looking for a specific policy or process get results from within that section of the knowledge base only.

**Agent implication:** The scoped search is a UX feature for humans. Agents using Q&A will still search across the full workspace by default — scope the agent's source databases explicitly in its Instructions page rather than relying on the wiki search boundary.

---

## 3. Page Verification & Ownership — The Anti-Rot Engine

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`
`Note: Verification and ownership features require Business or Enterprise plan.`

### 3.1 What verification is

A verified page displays a blue checkmark — a signal that a designated owner has confirmed the content is current and accurate. Verification is the primary mechanism for fighting documentation rot.

Without verification, every page in a knowledge base is equally untrustworthy after six months. With verification, the checkmark tells any reader — human or AI — that this page was reviewed by a responsible person on a known date.

### 3.2 Verification options

| Option | What it means |
|---|---|
| **Verify until a specific date** | Expires on the chosen date; Owner gets notified to re-verify |
| **Verify indefinitely** | Displays "Verified" with the last-verified date and verifier name; no automatic expiry |
| **Predefined windows** | 7, 30, or 90 days — useful for frequently changing content |

**The expiry notification:** When verification expires, the page Owner receives a notification in their Notion inbox and via email. This is the built-in accountability loop — no external reminder system needed.

### 3.3 Setting up verification

**On a wiki page:** Hover over the page title → click Verify → choose expiry type.

**On a non-wiki database:** Add a Verification property to the database → this automatically creates an Owner property as well. Every row in the database can then be verified individually.

**Hard constraint:** A page cannot be verified without at least one Owner assigned. Notion enforces this — verification without ownership is accountability theater.

### 3.4 The Owner property

- By default, the page creator becomes the Owner
- Can be reassigned to any workspace member
- Can be configured to allow single or multiple owners
- If you remove the Verification property from a database, the Owner property is also deleted — consolidate ownership information elsewhere before removing if needed

### 3.5 Verification strategy — what to verify

Verification is not for everything. The overhead is real. Verifying a casual working note is wasted effort. Verifying a hiring policy or a product spec is essential.

**Verify:**
- SOPs and processes teams act on daily
- Policy documents
- Brand and style references
- Onboarding materials
- Spec documents and technical references

**Do not verify:**
- Working notes and drafts
- Meeting notes
- Brainstorm pages
- Content calendar entries
- Project pages (they have their own Status workflow)

**Rule:** If someone might make a decision based on this page being "true," verify it.

### 3.6 The centralized verification hub

Notion provides a centralized view of all verified pages in the workspace: **Settings → Notion AI → Verified Pages** (exact path may vary by plan). Use this view during weekly review to identify pages with expired or upcoming verification windows.

---

## 4. Information Architecture — How to Structure a Knowledge Base

### 4.1 The hierarchy failure mode

The most common knowledge base failure in Notion is unchecked page nesting. Someone creates a page, nests a sub-page inside it, nests another inside that, and six months later the workspace has a 7-level tree that nobody navigates and Q&A can't reliably surface.

**The rule:** Maximum 3 levels of page nesting for knowledge base content.

```
Level 1: Wiki or top-level knowledge area (e.g., "Brand & Creative")
Level 2: Topic or category page (e.g., "Voice & Tone")
Level 3: Individual reference page (e.g., "Words to Avoid")
```

If content naturally lives at level 4 or deeper, it belongs in a database, not a nested page hierarchy.

### 4.2 Pages vs. databases for knowledge base content

| Use pages (including wikis) for... | Use databases for... |
|---|---|
| Reference documents, guides, SOPs | Structured information with filterable properties |
| Narrative explanations and context | Inventories, catalogs, trackers |
| Content that is read linearly | Information queried by date, owner, status, type |
| Material that changes infrequently | Content that changes frequently or has lifecycle states |

**The practical test:** If you ever need to ask "which of these is the latest version?" or "show me only the active ones," that content belongs in a database, not a page.

### 4.3 The top-level knowledge base structure

A knowledge base that can be navigated within 60 seconds needs a top-level wiki with clear categories. For a solo operator or small team, a clean starting structure:

```
📚 Knowledge Base (Wiki)
├── 🧭 Getting Started — orientation for new collaborators
├── 🎯 Strategy & Brand — brand positioning, voice, mission
├── ⚙️ Operations — recurring processes, SOPs, checklists
├── 🛠️ Tools & Tech — platform references, tech stack docs
├── 📋 Templates — reusable page templates by type
└── 📁 Archive — retired pages, superseded versions
```

Each category is a child page of the top-level wiki. Each can itself be a wiki, inheriting the verification and ownership system.

### 4.4 The Archive section is mandatory

A knowledge base without an Archive decays. Pages get outdated, nobody deletes them because "it might still be useful," and the workspace fills with untrustworthy content that Q&A surfaces as answers.

The Archive section exists to receive pages that are no longer active but may have historical value. Moving to Archive is reversible. Deleting is not. When in doubt, archive.

---

## 5. The Library View — Navigating at Scale

`Source: Official Notion releases | Last verified: Mar 4, 2026 | Confidence: High`

The **Library** view, launched in February 2026, is a full-page navigation surface for Notion — a "clean home for everything" that keeps the sidebar navigable without losing pages.

**What it is:** Library is a high-level page that surfaces all content in a workspace or section in a browsable, catalog-style view. Think of it as a cleaner alternative to sidebar sprawl — everything is accessible without cluttering the sidebar navigation.

**How it helps the knowledge base:** For workspaces with 100+ pages, Library provides a high-density browsing surface. Pages can be organized visually without requiring nested sidebar hierarchies.

**Practical use:** Set up Library as the entry point for the knowledge base. The sidebar stays lean; the Library page holds the breadth. For the E.I. Werner workspace specifically, Library is the right home for the full Codex suite when more documents accumulate.

---

## 6. Q&A Optimization — Making AI Search Work

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

### 6.1 What Q&A searches

Q&A searches: page **titles**, page **body content**, and (on Enterprise plans) connected apps via Enterprise Search (Slack, Google Drive, GitHub).

Q&A respects all workspace permissions. A user asking Q&A will only see answers sourced from pages they can access.

### 6.2 What Q&A does well

- Finding and surfacing pages that answer a specific question
- Summarizing content from rich page bodies
- Cross-referencing multiple pages to synthesize an answer
- Linking back to source pages in its response

### 6.3 What Q&A cannot do reliably

- **Count or aggregate database records** — "How many tasks are open?" will not return a reliable count
- **Filter databases** — "Show me contacts in the Active stage" is a database query, not a Q&A question
- **Return real-time data** — Q&A pulls from page body content as of the last index update
- **Answer questions about information that only lives in file attachments** — files attached to pages are not indexed

**The decision rule:** If the question can be answered by reading a page, Q&A handles it. If the question requires running a database query, build a view instead.

### 6.4 What improves Q&A results

| Factor | How to improve it |
|---|---|
| **Page titles** | Specific, descriptive, searchable — not "Notes" or "Misc" |
| **Page body richness** | Full context, not one-line stubs — Q&A reads bodies, not properties |
| **Recency** | Recently updated pages rank higher in Q&A results |
| **Verified pages** | Verification signals to Q&A that this content is authoritative |
| **Descriptive headings** | H1/H2 headings inside pages make content structure scannable |

### 6.5 Designing the knowledge base for Q&A

Every page in the knowledge base that should be discoverable via Q&A needs:
1. A specific, searchable title that includes the question it answers
2. A body that opens with a plain-language answer — don't bury the answer in the middle of a long page
3. Relevant keywords in natural prose — Q&A is semantic, not keyword-matching, but keyword-rich content still performs better
4. A recent last-edited date — pages that haven't been touched in 18 months float lower in results

**Naming principle for Q&A:** Title pages as if answering a question. "What is our refund policy?" outperforms "Refunds" for Q&A discoverability.

---

## 7. Naming Conventions for Knowledge Base Pages

Consistent naming is the difference between a knowledge base that anyone can navigate and one that only its builder can use. These conventions apply to all pages, not just wikis.

### 7.1 Page title conventions

| Content type | Naming pattern | Example |
|---|---|---|
| SOP / process | `How to [do the thing]` | `How to Onboard a New Collaborator` |
| Policy / reference | `[Topic] — [Scope]` | `Merch Pricing Policy — Fourthwall` |
| Guide / explanation | `[Subject]: [What it covers]` | `ONCE Mythology: Writing Guidelines` |
| Template | `Template: [What it's for]` | `Template: Stream Pre-flight Checklist` |
| Decision record | `Decision: [What was decided]` | `Decision: Drop 24/7 Radio in Favor of Bookend` |

### 7.2 What to avoid

- **Vague titles:** "Notes," "Misc," "Info," "Stuff" — these are invisible to Q&A and to humans scanning the sidebar
- **Date-only titles:** "March 2026 Update" — what update? Add context
- **Pronoun titles:** "My Process" — whose process? For what?
- **Acronyms without expansion on first use** — especially important for collaborators and AI agents who don't know the internal vocabulary

### 7.3 Folder vs. page title disambiguation

If a section-level page (wiki or parent page) and a child page have similar names, the child won't be found. Solve this at the parent level: the parent should be the category, the child should be specific.

**Bad:** Parent: `Brand`, Child: `Brand Guidelines` — both match a search for "brand"
**Good:** Parent: `Brand & Identity`, Child: `Voice & Tone Guidelines` — non-overlapping signals

---

## 8. Page Body Standards for Knowledge Base Entries

### 8.1 The minimum viable knowledge base page

Every page that is meant to be found and used must have:

```
## What This Is
[1-2 sentences: what this page covers and who it's for]

## [The Content]
[The actual reference material — organized with headings]

## Last Updated / Owner Note
[Optional: a plain-language note about when this was last reviewed and any important caveats]
```

### 8.2 The SOP / process page template

```
## Purpose
[Why this process exists — what problem it solves]

## Trigger
[What initiates this process — event, schedule, or request]

## Steps
1. [Action — specific enough that someone new can follow it]
2. [Action]
3. [Action]

## Definition of Done
[How you know the process is complete]

## Owner
[Who is responsible for keeping this accurate]

## Notes / Edge Cases
[Anything that doesn't fit the standard flow]
```

### 8.3 The decision record template

Decision records are underused and extremely valuable — they prevent the same decision from being relitigated in six months.

```
## Decision
[What was decided — one sentence]

## Date
[When this decision was made]

## Context
[Why this decision was needed — what problem it solved]

## Options Considered
- Option A: [description and why it was rejected or chosen]
- Option B: [description and why it was rejected or chosen]

## Outcome
[What will happen as a result of this decision]

## Owner
[Who made or owns this decision]
```

---

## 9. Governance — Who Owns What and When It Gets Reviewed

### 9.1 The ownership model

Every page in the knowledge base that matters should have a designated Owner. The Owner is not necessarily the person who wrote it — they are the person responsible for its accuracy going forward.

**Assignment rule:** Assign ownership at creation time. A page created without an owner will not get one later — it will drift into the unverified, un-maintained mass of content that eventually breaks Q&A.

### 9.2 The verification cadence

| Content type | Recommended verification window |
|---|---|
| Actively used SOPs | 90 days |
| Policies (HR, legal, compliance-adjacent) | 90 days or on policy change |
| Platform references (tech specs, integrations) | 30 days — these change fastest |
| Brand and creative references | 180 days or on brand update |
| Archive | No verification needed — archived = acknowledged as potentially stale |
| Working docs, notes | No verification needed |

**The governance loop:**
1. Page is created → Owner assigned → verification window set
2. Verification expires → Owner gets notified in inbox and via email
3. Owner reviews → re-verifies or updates content then re-verifies
4. If Owner is unavailable → any editor can re-assign and re-verify

### 9.3 What Notion's governance gaps are

**No native version history beyond page edit log:** Notion shows who edited a page and when, but does not support named versions or formal change tracking. For pages that need audit trails (legal, compliance), note changes manually in an "Edit History" section at the bottom of the page body.

**No approval workflow:** Notion doesn't natively require approval before publishing changes. This is by design — it's a collaborative workspace, not a CMS with staging. If approval workflows are required, simulate with Status properties on a database or use Comments for the approval signal.

**No content expiration without manual action:** The Verification expiry notifies the Owner, but it does not unpublish or flag the page to readers automatically. Expired pages still display their content — just without the blue checkmark. Educate users: no checkmark = unverified, not deleted.

---

## 10. The Weekly Review — Survival Mechanism

A knowledge base without a weekly review is a knowledge base that decays. The review does not need to be long — 15 minutes per week is enough if it is done consistently.

### 10.1 The weekly review checklist for knowledge bases

**Capture → Triage → Maintain:**

```
WEEKLY KNOWLEDGE BASE REVIEW

□ Review Agent Inbox for any flagged outdated pages
□ Check the Verification hub for expired or upcoming expirations
□ Process any new pages created this week — assign Owners, set verification
□ Check the Archive section — anything recently moved there that shouldn't be?
□ Prune: are there any stub pages (title only, no body) that need content or should be deleted?
□ One dead link check on recently referenced pages
□ Update any page that you've noticed is stale this week
```

### 10.2 The anti-rot principle in practice

From Doc 0's Core Design Philosophy: **prune before it rots, maintain before it drifts, archive before it misleads.**

Applied to a knowledge base:
- A stub page with only a title is noise — either write it or delete it within one review cycle
- A page last edited 12+ months ago is a rot risk — verify it or archive it
- A page with an expired verification badge is actively misleading — re-verify or update immediately

**The 60-second test:** Can a new collaborator — human or AI — orient in this workspace within 60 seconds? If the answer is no after a weekly review, the review isn't deep enough.

---

## 11. Agent SOP Templates — Knowledge Base Maintenance

### SOP 1: Expired Verification Digest

```
SOP: Verification Monitor Agent — Weekly Expiry Digest

Goal:
- Every Monday morning, generate a digest of all pages
  with expired or upcoming (within 7 days) verification,
  and post to a designated Slack channel or Notion inbox page

Inputs:
- All wiki and database pages with Verification property
- Filter: Verification status = Expired OR Verification date ≤ today + 7 days

Allowed actions:
- Read workspace pages and databases
- Post formatted digest to Slack or append to a designated Notion review page

Hard constraints:
- Read only — no changes to verification status or page content
- Do not re-verify pages autonomously — this requires human judgment
- If 0 pages match, post: "No verifications expired or upcoming this week."

Validation checklist:
- [ ] Verification date filter is accurate (includes both expired and within 7 days)
- [ ] Message includes: Page title, Owner, Expiry date, Direct link

Logging:
- Agent Log: timestamp, count of expired pages, count of upcoming, post confirmed

Escalation:
- If Owner is missing on an expired page → include in digest with note: "No owner assigned — needs attention"
```

### SOP 2: Stub Page Flagging

```
SOP: Stub Page Agent — Content Quality Monitor

Goal:
- Weekly, identify pages in the knowledge base wiki with
  no body content (title only) and flag them for the Owner

Inputs:
- Knowledge base wiki child pages
- Filter: Page body length = 0 OR page body contains only whitespace/template placeholder

Allowed actions:
- Append a flag note to stub page body: "[Agent] Stub flagged on [date]. Add content or move to Archive."
- Create Agent Inbox item listing all stubs found this week

Hard constraints:
- Do not delete any page
- Do not change any properties
- Do not flag pages that are intentionally section dividers or navigation headers

Validation checklist:
- [ ] Confirm page is genuinely empty, not just short
- [ ] Confirm page is not a navigation/section header page
- [ ] Check that Owner exists before flagging

Logging:
- Agent Log: timestamp, pages found, flag notes appended

Escalation:
- If more than 10 stubs found → create Agent Inbox item: "High stub count this week — review knowledge base."
```

### SOP 3: Knowledge Base Q&A Agent

```
SOP: KB Q&A Agent — Slack Question Answering

Goal:
- When a question is posted in a designated Slack channel,
  search the knowledge base wiki for a relevant answer,
  and post a response with citation links

Inputs:
- Trigger: New message in #[channel-name] matching question pattern
- Source: Knowledge base wiki pages (read access only)

Allowed actions:
- Search knowledge base pages
- Post Slack message with answer and page citation links

Hard constraints:
- Never invent answers — if not found, say so
- Only cite verified pages (blue checkmark) as primary sources
- If confidence is Low, do not post an answer — route to Agent Inbox instead
- Read only — no writes to knowledge base

Validation checklist:
- [ ] Source pages found are within the knowledge base scope
- [ ] At least one citation link is included in every answer
- [ ] Confidence is High or Medium before posting

Logging:
- Agent Log: trigger, question text, pages searched, response posted, citations, confidence

Escalation:
- If no relevant pages found → post: "I couldn't find a confident answer. Please check the knowledge base directly: [link] or ask [Owner]."
- If confidence Low → create Agent Inbox item: "Unanswered question from Slack: [question text]"
```

---

## 12. Common Mistakes and How to Avoid Them

| Mistake | What goes wrong | The fix |
|---|---|---|
| Nesting pages more than 3 levels deep | Navigation becomes impossible; Q&A surfaces buried content unreliably | Enforce 3-level maximum; move deep content to a database |
| No verification or ownership | Knowledge base decays; Q&A returns untrustworthy content; nobody knows what's current | Assign Owners at creation; set verification windows for high-value pages |
| Verifying everything | Verification overhead is unsustainable; owners stop re-verifying; the checkmark loses meaning | Verify only pages people act on; skip working notes and drafts |
| Stub pages left unwritten | Q&A returns empty or hallucinated results; workspace feels abandoned | Weekly review: write it or delete it within one cycle |
| No Archive section | Outdated content stays in navigation; Q&A surfaces stale answers | Create Archive section; move rather than delete |
| Vague page titles | Q&A can't find the page; humans can't navigate; agents produce poor outputs | Rename pages to answer a question or describe what they contain specifically |
| Empty page bodies | Q&A and Autofill have no context | Every page needs at minimum a "What This Is" section |
| No weekly review | Decay is silent and accelerating | Block 15 minutes every week; treat it as infrastructure maintenance, not optional |
| Using database for all knowledge | Everything becomes a structured record; narrative context is lost | Mix databases (for structured, filterable info) and pages (for guides, SOPs, reference) |
| Converting databases into wikis | Notion does not support this | Only pages can become wikis; use database views on a page instead |

---

## 13. Living Document Protocol

**Rebuild triggers:**
- Notion releases version history or formal change tracking on pages
- Verification feature expands to Free/Plus plans
- Q&A capabilities expand to include database querying
- Library view receives significant new functionality
- A governance pattern is validated or invalidated through operational experience

**Cross-references:**
- Doc 0 Section 1 — Core Design Philosophy (beauty + anti-rot, 60-second legibility test)
- Doc 0 Section 6 — Q&A capabilities and limitations (authoritative source)
- Doc 0 Section 11 — Naming conventions (authoritative source)
- Doc 3 — AI Agents deep dive (for KB Q&A agent build)
- Doc 6 — Automations & Integrations (for verification expiry webhook triggers)
- Notion Mission doc — Milestone 5 (Workspace Legibility) is the primary milestone this document serves

**Version history:**
- v1.0 — March 4, 2026 — Initial build by Claude. Verified against official Notion wiki and verified pages documentation.

---

*Notion Codex — Doc 5: Notion as Wiki & Knowledge Base | v1.0 | March 4, 2026*
*Load Doc 0 alongside this document. Written with Claude.*
