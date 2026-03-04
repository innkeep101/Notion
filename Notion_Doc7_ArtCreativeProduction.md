# Notion Codex — Doc 7: Art & Creative Production
**Version 1.0 | March 4, 2026 | Living Document**
*Part of the Notion Master Reference Suite. Load Doc 0 alongside this document for platform truth, naming standards, and agent governance. This document covers the full creative production system: commission tracking, asset library, collaborator management, brief templates, file reference architecture, and agent SOPs for deliverable tracking.*

---

## AI & Agent Instruction Block

**Document scope:** Everything needed to manage visual and audio creative production in this workspace — from initial brief to delivered asset to merch-ready file. The core workflow is: Brief → Commission → Deliverable → Asset Library → Product/Deployment. Notion manages this entire pipeline. It does not store master files (those live in external cloud storage); it references them, tracks their status, and connects them to downstream deployment contexts.

**Critical distinction this document governs:** Notion holds metadata, status, briefs, and links — not master creative files. Large files (PNGs, PSDs, MP4s, audio) should live in Google Drive, Dropbox, or equivalent cloud storage. Notion entries link to those files. This keeps the workspace fast and the assets accessible without duplication.

**Load Doc 0 first for:** Platform truth, Autofill constraints (reads page body only, not file attachments), naming standards, agent governance.
**Load Doc 3 for:** Custom Agent build depth, MCP configuration, credits mechanics.
**Load Doc 2 for:** Collaborator relationship management that extends beyond active commissions.
**Cross-reference BrandMaster:** Ideation engine, audience framework, collaborator status, royalty structures, and merch pricing live there — not here. This document is the production system, not the creative strategy.

---

## Table of Contents

1. [The Creative Production Mental Model](#1-the-creative-production-mental-model)
2. [Database Architecture — Two Core Databases](#2-database-architecture--two-core-databases)
3. [Commissions Database — Full Schema](#3-commissions-database--full-schema)
4. [Asset Library Database — Full Schema](#4-asset-library-database--full-schema)
5. [The Collaborator Layer — Doc 2 Relationship to Doc 7](#5-the-collaborator-layer--doc-2-relationship-to-doc-7)
6. [Commission Status Pipeline](#6-commission-status-pipeline)
7. [Brief Templates — By Deliverable Type](#7-brief-templates--by-deliverable-type)
8. [Autofill Configuration for Creative Production](#8-autofill-configuration-for-creative-production)
9. [File Reference Architecture](#9-file-reference-architecture)
10. [Gallery View — The Asset Library as Visual Surface](#10-gallery-view--the-asset-library-as-visual-surface)
11. [Merch & Product Pipeline Integration](#11-merch--product-pipeline-integration)
12. [Views — Recommended Set](#12-views--recommended-set)
13. [Agent SOP Templates](#13-agent-sop-templates)
14. [Common Mistakes](#14-common-mistakes)
15. [Living Document Protocol](#15-living-document-protocol)

---

## 1. The Creative Production Mental Model

Creative production in this workspace follows a linear but iterating pipeline:

```
Brief (what's needed and why)
  → Commission (who's building it, timeline, payment)
    → Deliverable (files received, reviewed, approved)
      → Asset Library (catalogued, tagged, ready to deploy)
        → Deployment (merch product, stream overlay, content, wiki)
```

Each stage has a Notion home. The pipeline is tracked in the Commissions database. Finished, approved assets graduate to the Asset Library. The Asset Library is the supply chain for every downstream use — merch, overlays, content thumbnails, brand collateral.

**What Notion manages at each stage:**

| Stage | Notion's role | What lives outside Notion |
|---|---|---|
| Brief | Page body template, Autofill summary | Creative references (Pinterest boards, Figma files — linked, not uploaded) |
| Commission | Status tracking, payment logging, contract link | Contract PDFs (Drive), payment receipts (Stripe/SOL records) |
| Deliverable | Review notes, approval decision, file reference | Master files (Drive/Dropbox) |
| Asset Library | Metadata, tags, dimensions, usage rights, linked deployments | Full-resolution files (Drive/Dropbox) |
| Deployment | Relation to merch entry, content entry, or overlay doc | Platform files (Fourthwall, OBS) |

---

## 2. Database Architecture — Two Core Databases

**Two databases. No more.**

| Database | What it tracks | One record = |
|---|---|---|
| **Commissions DB** | Active and historical work orders — briefs, collaborators, timelines, payments, deliverable status | One commissioned body of work (may produce multiple assets) |
| **Asset Library DB** | Approved, deployment-ready creative assets | One discrete asset (one file or set of files representing a single deliverable) |

**Why two and not one:** Commissions have a lifecycle that ends. Assets persist and get reused. A commission for "overlays" produces multiple assets — each asset gets its own Asset Library entry. The commission entry is the production record. The asset entries are the supply inventory.

**Relation between them:** Each Asset Library entry holds a Relation property back to its source Commission. Each Commission entry has a rollup (or filtered view in the page body) showing all assets delivered from it.

**Do not create separate databases for:**
- Illustration commissions vs. animation commissions vs. music commissions → use Commission Type property
- "Active" vs. "archived" commissions → use Status filtering
- Merch assets vs. stream assets → use Asset Type + Usage Context properties

---

## 3. Commissions Database — Full Schema

### Required properties

| Property | Type | Notes |
|---|---|---|
| **Commission Title** | Title | Format: `[Collaborator] — [Deliverable] ([Collection/Context])`. Example: `Luiz — Overlays (Outside Inn S1)` |
| **Commission ID** | Unique ID | Prefix: `COM-` |
| **Status** | Status | Full pipeline defined in Section 6 |
| **Collaborator** | Relation → Contacts DB (Doc 2) | Links to the person record; no duplicate contact data here |
| **Commission Type** | Select | Illustration, Pixel Art, Animation, Music, Audio, Motion, Other |
| **Brief Date** | Date | When the brief was delivered to collaborator |
| **Due Date** | Date | Expected delivery date |
| **Delivered Date** | Date | Actual delivery date (set on Status → Delivered) |
| **Fee — Agreed** | Number | Total agreed commission fee (USD or crypto equivalent at time of agreement) |
| **Fee — Paid** | Number | Amount paid to date |
| **Royalty Rate** | Number | Per-unit royalty (e.g., 0.77) — 0 if no royalty agreement |
| **Contract Status** | Select | Not sent / Sent / Signed / N/A |
| **Contract Link** | URL | Link to signed contract in Google Drive |
| **Assets Delivered** | Rollup | Count of related Asset Library entries — auto-populated via relation |

### Recommended optional properties

| Property | Type | Notes |
|---|---|---|
| **Campaign** | Relation → Projects DB (Doc 1) | Links commission to a launch or project milestone |
| **Priority** | Select | High / Medium / Low |
| **Revision Rounds** | Number | Track revision count — useful for future scope negotiations |
| **Notes** | Text | Brief summary of any out-of-brief agreements, change requests, or context |
| **AI Brief Summary** | Text (Autofill) | Auto-generated summary from page body brief — enable after brief is complete |

### Naming standard

`[Collaborator Last Name] — [Deliverable Type] ([Context])`

Good: `Silva — Stream Overlays (Outside Inn S1)`
Good: `Hasnaoui — Pixel Animation (Outside Inn Logo)`
Bad: `Marwan stuff` / `Commission 4` / `Art project`

The title must be self-describing without opening the record. Agents and Q&A depend on it.

---

## 4. Asset Library Database — Full Schema

### Required properties

| Property | Type | Notes |
|---|---|---|
| **Asset Name** | Title | Format: `[Type] — [Description] ([Collaborator])`. Example: `Overlay — Stream Start Countdown (Hasnaoui)` |
| **Asset ID** | Unique ID | Prefix: `AST-` |
| **Asset Type** | Select | Overlay, Illustration, Portrait, Badge, Pixel Art, Animation, Music, Logo, Merch-Ready, Reference |
| **Status** | Select | In Review / Approved / Revision Requested / Archived |
| **Source Commission** | Relation → Commissions DB | Links back to the commission that produced this asset |
| **Collaborator** | Relation → Contacts DB (Doc 2) | Direct link for quick filtering by artist |
| **File Reference** | URL | Direct link to master file in Google Drive/Dropbox |
| **File Format** | Select | PNG / PSD / MP4 / GIF / WAV / FLAC / MP3 / PDF / SVG / Other |
| **Dimensions / Specs** | Text | e.g., `3840×2160px`, `1hr 40min loop`, `48kHz WAV` |
| **Usage Rights** | Select | Exclusive / Non-exclusive / Royalty-bearing / Royalty-free / Pending |
| **Tags** | Multi-select | Stream / Merch / Brand / Overlay / Portrait / Goose / Animation / Music / etc. |

### Recommended optional properties

| Property | Type | Notes |
|---|---|---|
| **Cover Image** | Files & Media | Low-res preview only — NOT master file. Enables gallery view thumbnail. |
| **Usage Contexts** | Multi-select | Fourthwall / OBS / YouTube / Thumbnail / Wiki / Social |
| **Deployed In** | Relation → Content Calendar (Doc 4) | Links asset to content entries where it's been used |
| **Merch Products** | Text | Comma-separated product names (or Relation to a Merch DB if one is built) |
| **Notes** | Text | Revision history, usage restrictions, specific crop/format guidance |
| **AI Tags** | Multi-select (Autofill) | Auto-generated topic tags from page body |

### Cover Image guidance

`Source: Official Notion help | Last verified: Mar 4, 2026`

The Cover Image (Files & Media property) used as gallery card preview should be a **compressed preview or thumbnail** — not the master file. Master files (especially 4K PNGs, layered PSDs, 480-frame PNG sequences) should live in Drive/Dropbox with a URL in the File Reference property. Uploading masters to Notion creates storage bloat and slows gallery renders.

Recommended workflow: export a 1000×1000px or 1920×1080px JPG preview of each asset → upload that as Cover Image → link master file separately.

---

## 5. The Collaborator Layer — Doc 2 Relationship to Doc 7

Collaborators are **people**. Their full relationship context — communication history, follow-up cadence, relationship health, contract terms — lives in the Contacts database (Doc 2). Doc 7 does not duplicate contact information.

The Commissions database relates to Contacts via a Relation property (Collaborator field). This surfaces commissions directly on a collaborator's Contacts record as a linked view — you can see all commissions from Luiz or Marwan in one place without leaving their contact page.

**What belongs in Contacts (Doc 2) vs. Commissions (Doc 7):**

| Data point | Where it lives |
|---|---|
| Collaborator name, email, payment address | Contacts DB |
| Relationship notes, communication history | Contacts DB |
| Overall contract terms and royalty caps | Contacts DB page body |
| Per-commission payment amounts | Commissions DB |
| Per-commission deliverable status | Commissions DB |
| Per-asset file reference | Asset Library DB |

**Active collaborators as of March 2026** (tracked in Contacts, summarized here for agent context):

- **Luiz Henrique Medeiros Silva** — Illustration, portraits, badges. Contract signed Mar 2. $0.77/unit royalty, 7-year term.
- **Marwan Hasnaoui** — Pixel art, animation. Contract on DocuSign (pending signature as of Mar 4). $0.77/unit royalty, 14-year term.
- **Carson Thatcher** — Music. Contract sent Mar 1. Album deal in negotiation, target close April 4.

These are live records. Cross-reference BrandMaster for relationship strategy and current negotiation status.

---

## 6. Commission Status Pipeline

The Status pipeline for Commissions maps the full lifecycle of a work order. Status groups follow Doc 0 architecture — To-do, In progress, Complete.

```
To-do:
  Planned           ← Identified need, brief not yet written
  Briefing          ← Brief in progress or delivered, collaborator not yet confirmed

In progress:
  Active            ← Collaborator confirmed, work underway
  In Review         ← Deliverable received, under review
  Revision Requested ← Feedback sent, awaiting revised delivery
  Approved          ← Asset approved, ready for Asset Library entry

Complete:
  Paid              ← Commission fee fully paid
  Archived          ← Commission closed (cancelled, superseded, or inactive)
```

**Status rules:**
- "Approved" stays in **In progress** — payment may not yet be complete. Move to Paid only when fee is settled.
- "In Review" triggers the review workflow — assign review notes in page body, not in properties (properties are for status, body is for reasoning).
- A commission can loop between Active → In Review → Revision Requested → In Review multiple times. Track revision count in the Revision Rounds property.
- Asset Library entries are created **at Approved status** — not before. Don't create asset records for unreviewed deliverables.

### Native automations for this pipeline (free, no credits)

**Automation 1 — Delivered date stamp:**
Trigger: Status changes to `In Review`
Action: Set property — Delivered Date = today

**Automation 2 — Revision counter:**
Trigger: Status changes to `Revision Requested`
Action: Set property — Revision Rounds = `Revision Rounds + 1` (formula-based increment via Define Variables)

**Automation 3 — Slack alert on approval:**
Trigger: Status changes to `Approved`
Action: Send Slack notification to #creative-production — "✅ Commission approved: {{Commission Title}}"

---

## 7. Brief Templates — By Deliverable Type

Set as database page templates (New dropdown → one template per Commission Type). The writer selects the type when creating a commission record — the template auto-fills the page body scaffold. This is what Autofill reads. An empty or generic brief produces empty or generic Autofill output.

### Universal Brief Header (all types)

```
## Commission Brief

**Collaborator:** [Name]
**Commission Type:** [Type]
**Context / Campaign:** [What this is for — stream, merch, content, brand]
**Deadline:** [Date]
**Reference Material:** [Links to Drive folders, Figma frames, inspiration images]

---
```

### Illustration / Portrait Brief

```
## Commission Brief
[Universal header above]

### Deliverable Spec
- Subject / Character: [Who or what]
- Style Direction: [Tone — serious, whimsical, detailed, flat, etc.]
- Color Palette: [Specific colors or reference]
- Background: [Transparent / specific / scene]
- Format: [PNG, layered PSD, etc.]
- Dimensions: [Final pixel dimensions + DPI for print if merch-bound]
- Quantity: [How many pieces or variants]

### Intended Usage
- [ ] Stream overlay
- [ ] Merch product
- [ ] Content thumbnail
- [ ] Brand collateral
- [ ] Other: [specify]

### Merch-Specific Notes (if applicable)
- Products: [List Fourthwall products this asset will be used on]
- Bleed/margin requirements: [Per product spec]
- Color profile: [sRGB / CMYK — confirm with printer]

### Reference Links
[Link 1 — Drive folder or specific image]
[Link 2 — Existing brand assets for consistency reference]

### Definition of Done
- [ ] Final PNG delivered at specified resolution
- [ ] Layered source file delivered (if agreed)
- [ ] Transparent background confirmed (if required)
- [ ] Reviewed and approved by Evan
```

### Pixel Art / Animation Brief

```
## Commission Brief
[Universal header above]

### Deliverable Spec
- Subject: [What's being animated / illustrated]
- Style: [Pixel density, palette constraints, animation style]
- Canvas size: [e.g., 1920×1080px]
- Frame count (if animation): [e.g., 480 PNG frames]
- Loop type: [Seamless loop / one-shot / intro + loop]
- Frame rate: [FPS]
- Export format: [PNG sequence / GIF / MP4 / WebM]
- OBS compatibility: [Browser Source dimensions if overlay]

### Animation Beat Notes
[Describe the motion — what moves, how, speed, easing]

### Reference
[Link to existing style reference — past deliverables from this collaborator, external reference]

### Technical Constraints
- [ ] Must run without dropped frames at 1080p60 OBS
- [ ] Loop tested across: [hours without artifacts — e.g., 11h39m tested in production]
- [ ] File size target: [if relevant for web delivery]

### Definition of Done
- [ ] All frames delivered as PNG sequence
- [ ] Loop point confirmed seamless
- [ ] MP4 render delivered (if requested)
- [ ] Tested in OBS Browser Source at target resolution
- [ ] Reviewed and approved by Evan
```

### Music / Audio Brief

```
## Commission Brief
[Universal header above]

### Deliverable Spec
- Type: [Lo-fi loop / score / jingle / theme / SFX]
- Duration: [Target length or loop length]
- BPM range: [If relevant]
- Mood / Energy: [Lo-fi chill / building tension / triumphant / etc.]
- Instrumentation preferences: [What's desired or to avoid]
- Key / Scale: [If relevant]
- Master format: [WAV at 48kHz / FLAC / MP3 at 320kbps]
- Stems required: [Yes / No — specify if yes]

### Intended Use
- [ ] Stream background music
- [ ] Intro/outro music
- [ ] Royalty-free merch/product embed
- [ ] Album release

### Usage Rights
- License type: [Exclusive / non-exclusive / royalty-bearing]
- Platforms: [Twitch / YouTube / Fourthwall / other]
- Term: [Duration of license]

### Reference Tracks
[Link 1] [Link 2]

### Definition of Done
- [ ] Full track delivered at specified format
- [ ] Loop point documented (if looping track)
- [ ] Broadcast test completed (no clipping, correct normalization)
- [ ] Contract signed before delivery accepted
```

---

## 8. Autofill Configuration for Creative Production

`Source: Official Notion help | Last verified: Mar 4, 2026`

**Hard constraint:** Autofill reads page body only. Files in the Files & Media property are not read. URLs in properties are not fetched. A brief with empty or thin body content produces empty or hallucinated Autofill output. The templates in Section 7 exist specifically to ensure Autofill has sufficient context to reason from.

### Recommended Autofill properties — Commissions DB

| Property | Type | Fill mode | Trigger | Purpose |
|---|---|---|---|---|
| **AI Brief Summary** | Text | Summary | Auto-update on page edits | Scannable one-liner for board/table views without opening record |
| **AI Deliverable Tags** | Multi-select | Key Info, prompt: "Extract 3–5 tags describing the deliverable type and intended use" | Auto-update | Fast filtering by deliverable characteristics |

**When to enable Auto-update:** Once the brief body is substantially complete. Enable during Briefing or Active status — not at record creation when body is empty.

### Recommended Autofill properties — Asset Library DB

| Property | Type | Fill mode | Trigger | Purpose |
|---|---|---|---|---|
| **AI Tags** | Multi-select | Key Info, prompt: "Extract 3–5 tags describing this asset's type, style, and deployment context" | Manual | Enriches asset catalog for agent and Q&A discovery |
| **AI Usage Summary** | Text | Summary | Manual | One-sentence description for Q&A surfacing |

**Why manual triggers for Asset Library:** Asset records are created after approval — the page body at that point is review notes and file metadata. Autofill should be triggered once the record is fully populated, not on creation.

---

## 9. File Reference Architecture

`Source: Official Notion help | Last verified: Mar 4, 2026 | Confidence: High`

### The constraint

Notion's AI Autofill cannot read files uploaded to Files & Media properties. Q&A cannot search file contents. Agents cannot open or process file attachments. The only way to give AI systems context about a file is through the **page body** — descriptions, spec notes, review feedback, and metadata written as text.

This means: files uploaded to Notion are opaque to AI. Files linked via URL (with human-readable metadata in the page body) are transparent to AI.

### The recommended architecture

```
Master files → Google Drive / Dropbox (organized by collaborator and collection)
    ↓
Notion Asset Library entry → URL property links to master file in Drive
    ↓
Page body → Written metadata: dimensions, format, usage notes, review feedback
    ↓
Cover Image property → Compressed preview JPG (uploaded to Notion for gallery)
```

### Drive folder structure recommendation

Mirrors the Commissions database structure:

```
/Creative Production
  /[Collaborator Name]
    /[Commission ID] — [Commission Title]
      /Briefs
      /Deliverables
        /Round 1
        /Round 2 (if revisions)
        /Final — Approved
      /Source Files (PSDs, project files)
      /Exports (ready-to-deploy versions)
```

The Commission ID in the folder name creates a stable link between the Drive folder and the Notion record. Agents can construct the path if needed.

### What to write in the Asset Library page body

Each asset entry page body should contain enough text for AI to understand what the asset is without opening the file:

```
## Asset Overview
[1–2 sentences: what this is, who made it, what it's for]

## Technical Specs
- File: [Filename as delivered]
- Format: [PNG / WAV / etc.]
- Dimensions / Duration: [Exact specs]
- Color profile: [sRGB / CMYK / etc. if relevant]
- Transparent background: [Yes / No]

## Usage Notes
- Approved for: [specific uses]
- Restrictions: [any limitations]
- Crop/format guidance: [for merch — per product spec]

## Review Notes
[What was approved, any caveats, revision history summary]

## File Links
- Master: [Drive URL]
- Web-optimized export: [Drive URL if exists]
- OBS-ready export: [Drive URL if exists]
```

---

## 10. Gallery View — The Asset Library as Visual Surface

`Source: Official Notion galleries help | Last verified: Mar 4, 2026`

The Asset Library's primary navigation surface for visual review is Gallery view. Gallery view renders the Cover Image property as the card thumbnail. Without a Cover Image, cards show only title — the gallery loses its value as a visual catalog.

### Gallery view setup for Asset Library

1. Add view → Gallery
2. View options → Card Preview → set to **Cover** (not Page Content)
3. Card size → **Medium** for mixed asset types; **Large** for illustration-heavy collections
4. Properties to show on card: Asset Type, Collaborator, Status, Usage Contexts
5. Sort by: Delivered Date (descending) — newest assets first

### Recommended gallery views

**"All Approved Assets" gallery:** Filter Status = Approved, sort by Collaborator then Asset Type. The master visual catalog — what's in the supply chain and ready to deploy.

**"By Collaborator" gallery:** Group by Collaborator. See all of Luiz's work, all of Marwan's work as separate visual stacks. Useful for reviewing a body of work with a collaborator.

**"Merch-Ready" gallery:** Filter Tags contains "Merch". The product pipeline view — what's cleared and ready for Fourthwall.

**"In Review" gallery:** Filter Status = In Review. Active review queue — what's waiting for an approval decision.

### Cover image compression guidance

Upload a compressed preview (JPG, ~1000px wide, ≤500KB) as the Cover Image — not the master PNG or PSD. Gallery view renders many cards simultaneously. Large file uploads per card create slow renders and storage overhead.

---

## 11. Merch & Product Pipeline Integration

The creative production system connects to the merch pipeline at the Asset Library stage. This section documents that connection.

### The flow

```
Commission Approved → Asset Library entry created
  → Asset tagged "Merch-Ready"
  → Asset Library entry documents Fourthwall product dimensions per asset
  → Product entry created (in Projects DB or a dedicated Merch DB)
  → Asset Library entry → Deployed In relation updated on product launch
```

### Fourthwall product dimension requirements

This workspace has an established print product line. Dimensions must be documented per asset at the Asset Library stage — not discovered during product setup. Brief templates (Section 7) include merch spec fields for exactly this reason.

Luiz's product brief guidance (from BrandMaster): Send Luiz a complete Fourthwall product dimension guide so he can frame artwork correctly for each product type from the start. The goal is that every illustration brief specifies the intended product(s) and their exact crop dimensions — Luiz frames and delivers to spec, not to a generic canvas that needs re-cropping.

### Merch-ready criteria (asset must meet all before tagging)

- [ ] Approved status confirmed in Commissions DB
- [ ] Dimensions verified against target product spec (coaster, print, desk mat, etc.)
- [ ] Color profile confirmed (sRGB for screen products, CMYK-ready for print)
- [ ] Resolution sufficient for largest intended product (see Merch Resolution Standards in BrandMaster)
- [ ] Usage rights confirmed — exclusive, royalty terms documented in Contract Link
- [ ] Cover image preview uploaded to Asset Library entry

### The Goose Series logic

Multiple crops of a single illustration create distinct product SKUs (the Goose Series model from BrandMaster). In the Asset Library, each crop is a separate entry — same Source Commission, different asset record. Tag: Goose Series. This keeps the supply chain clean: one master illustration commission produces N deployable asset variants, each tracked independently.

---

## 12. Views — Recommended Set

### Commissions Database

| View | Type | Filter / Group | Purpose |
|---|---|---|---|
| **Active Commissions** | Board | Group by Status, filter Status ≠ Paid/Archived | Primary working view — pipeline at a glance |
| **By Collaborator** | Table | Group by Collaborator | Who has what in flight |
| **Payment Tracker** | Table | All records, show Fee Agreed, Fee Paid, Contract Status | Financial oversight — what's been paid vs. owed |
| **All Commissions** | Table | No filter, sort by Brief Date desc | Full history |
| **Needs Contract** | Table | Filter Contract Status = Not sent OR Sent | Follow-up queue for unsigned agreements |

### Asset Library Database

| View | Type | Filter / Group | Purpose |
|---|---|---|---|
| **Approved Assets** | Gallery | Filter Status = Approved | Master visual catalog — production-ready supply |
| **In Review** | Gallery | Filter Status = In Review | Active review queue |
| **Merch-Ready** | Gallery | Filter Tags contains Merch | Product pipeline view |
| **By Collaborator** | Gallery | Group by Collaborator | Body of work per artist |
| **All Assets** | Table | No filter, sort by Delivered Date desc | Full inventory with metadata |

---

## 13. Agent SOP Templates

All use the universal SOP format from Doc 0 Section 13. All follow least-permissive defaults — no deletes, no contract or payment field edits, log every action.

---

### SOP 1: Commission Review Reminder

**Goal:** When a Commission enters "In Review" status and remains there for 72+ hours without a Status change, create an Agent Inbox item flagging it for attention.

**Trigger:** Schedule (daily check) OR Database change (Status = In Review)

**Model:** MiniMax M2.5 (simple check, high-volume)

**Inputs:**
- Commissions DB filtered to Status = In Review
- Delivered Date property

**Allowed actions:**
- Create Agent Inbox item with Commission Title, Collaborator name, Delivered Date, and days elapsed
- Append note to Commission page body: `[Agent: Review pending X days as of [date]]`

**Hard constraints:**
- Never change Status property
- Never contact collaborator directly
- Never modify Fee or Contract properties
- Log every run in Agent Log

**Validation checklist:**
- [ ] Status confirmed as In Review (not another status)
- [ ] Delivered Date is populated (not empty — skip if empty)
- [ ] 72+ hours elapsed since Delivered Date
- [ ] No Agent Inbox item already exists for this commission this week

**Escalation:** If Commission has been In Review for 7+ days, flag Priority = High in addition to Inbox item.

---

### SOP 2: Asset Library Entry Generator

**Goal:** When a Commission moves to Approved status, draft a starter Asset Library entry for each expected deliverable and surface it in Agent Inbox for human completion.

**Trigger:** Database change — Status changes to Approved in Commissions DB

**Model:** Claude Opus 4.6 (requires reading brief body and drafting structured output)

**Inputs:**
- Triggering Commission record (full page body — brief, specs, Definition of Done)
- Collaborator name from Contacts relation
- Commission Type property

**Allowed actions:**
- Create new entry in Asset Library DB with:
  - Asset Name (drafted from brief context)
  - Asset Type (inferred from Commission Type)
  - Status = In Review
  - Source Commission (relation to triggering Commission)
  - Collaborator (relation — same as Commission)
  - Notes (drafted from brief's Definition of Done section)
- Create Agent Inbox item: "Asset Library entry drafted for [Commission Title] — review and complete file reference, dimensions, and cover image"

**Hard constraints:**
- Set Asset Status to In Review — never Approved (human approval required)
- Never populate File Reference URL (agent cannot verify file delivery)
- Never populate Usage Rights (legal field — human only)
- Log every creation in Agent Log with Commission ID and Asset ID

**Validation checklist:**
- [ ] Commission Status confirmed as Approved
- [ ] Brief body is present and non-empty (skip and flag if empty)
- [ ] No duplicate Asset Library entry already exists for this Commission
- [ ] Confidence in asset name and type is high — if ambiguous, create Inbox item only, do not create Asset Library entry

---

### SOP 3: Unsigned Contract Tracker

**Goal:** Every Monday, check all active Commissions for Contract Status = Not sent or Sent (not Signed), and post a summary to the Agent Inbox.

**Trigger:** Schedule (Monday 9:00 AM)

**Model:** MiniMax M2.5 (simple scan, low reasoning requirement)

**Inputs:**
- Commissions DB filtered to Status ≠ Paid AND Status ≠ Archived AND Contract Status ≠ Signed AND Contract Status ≠ N/A

**Allowed actions:**
- Create single Agent Inbox item with a list of commissions missing signed contracts: Commission Title, Collaborator, Contract Status, Brief Date
- Sort list by Brief Date ascending (oldest first — most urgent)

**Hard constraints:**
- Do not send external notifications — Agent Inbox only
- Never edit Commission records
- Never mark a contract as Signed
- Log run in Agent Log

**Escalation:** If any commission has Contract Status = Not sent AND Active status for 14+ days, flag it separately at the top of the Inbox item as high-priority.

---

## 14. Common Mistakes

| Mistake | What breaks | The fix |
|---|---|---|
| Uploading master files to Notion instead of Drive | Workspace bloat, slow gallery renders, AI cannot read file contents anyway | Master files in Drive; URL in File Reference; compressed preview only in Cover Image |
| Creating Asset Library entries before Commission is Approved | Asset records for unreviewed deliverables create false inventory | Create Asset Library entries at Approved status only |
| Putting collaborator contact data in Commissions DB | Duplicate records; contacts drift out of sync | Collaborator field is a Relation to Contacts DB — never store contact data directly |
| Generic brief body ("make an overlay") | Autofill produces generic or hallucinated output; collaborator has no precise spec to work from | Use brief templates; fill all required fields before brief is considered sent |
| One Commission entry for multiple distinct deliverables with different timelines | Can't track each deliverable's status independently | One Commission per discrete deliverable body; use Revision Rounds for iterations within the same commission |
| Not specifying merch dimensions in the brief | Asset delivered at wrong resolution or canvas size; crop required post-delivery | Specify all target Fourthwall product dimensions in brief; include merch spec in Definition of Done |
| Cover Image property populated with master files | Gallery slows; Notion storage consumed unnecessarily | Export a compressed JPG preview (≤500KB, ≤1000px wide) specifically for Cover Image |
| Using an agent for contract status tracking (simple scan) | Wastes credits on what a filtered view or native automation handles | Native automation or scheduled MiniMax agent; reserve reasoning models for brief-reading tasks |
| Royalty and contract terms stored in Commission entries | Authoritative terms in two places — Commission record and BrandMaster — drift out of sync | Per-commission fee in Commissions DB; contract terms (royalty rate, cap, term) in Contacts DB page body |
| Skipping the Definition of Done checklist in briefs | Ambiguous approval criteria; revision loops from unclear expectations | Every brief template includes Definition of Done; don't mark In Review until DoD is filled |

---

## 15. Living Document Protocol

**Rebuild triggers:**
- Active collaborator roster changes (new addition, existing relationship ends)
- Fourthwall product catalog changes — new products or dimension updates require Brief Template revision
- Notion adds file content reading capability (would change file reference architecture entirely)
- Autofill behavior changes (e.g., starts reading Files & Media)
- Commission or Asset Library schema changes based on operational experience
- Merch Resolution Standards finalized in BrandMaster — update Section 11 criteria

**Cross-references:**
- Doc 0: Platform truth, Autofill constraints, naming standards, agent governance, SOP template
- Doc 1: Projects & Tasks (Campaign relation for commissions tied to launches)
- Doc 2: Contacts / CRM (collaborator records, relationship context, contract terms)
- Doc 3: Custom Agents (model selection, credits, MCP for external tool actions)
- Doc 4: Content & Publishing (asset deployment into content calendar entries)
- Doc 6: Automations (native automation patterns, webhook to external tools)
- BrandMaster: Collaborator status, royalty structures, merch product catalog, resolution standards, ideation engine

**Version history:**
- v1.0 — March 4, 2026 — Initial build by Claude. Grounded in BrandMaster collaborator data (Luiz, Marwan, Carson), verified Notion gallery and file handling behavior, built for active production pipeline state.

---

*Notion Codex — Doc 7: Art & Creative Production | v1.0 | March 4, 2026*
*Load Doc 0 alongside this document. Written with Claude.*
