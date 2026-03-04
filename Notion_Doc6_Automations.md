# Notion Codex — Doc 6: Automations & Integrations
**Version 1.0 | March 4, 2026 | Living Document**
*Part of the Notion Master Reference Suite. Load Doc 0 alongside this document. Doc 3 covers the Custom Agent and MCP layers in full depth — reference it for agent-specific automation. This document covers the native automation engine, webhooks, AI Connectors, native integrations, and the external tool stack.*

---

## AI & Agent Instruction Block

**Document scope:** The complete automation and integration layer for this workspace — what Notion can trigger and act on natively, how webhooks connect Notion to external tools, how AI Connectors extend what agents and Q&A can see, which native integrations are available and what they actually do (vs. what they promise), and how external platforms (Make, Zapier, n8n) fill the gaps. This document also owns the definitive Automation vs. Agent decision rule — use it before building anything.

**Load Doc 0 first for:** Platform truth, naming standards, Status property architecture, agent governance.
**Load Doc 3 for:** Custom Agents, MCP integrations, agent SOPs, credits mechanics. Doc 6 does not re-explain those.

**Key principle this document enforces:** Automations are free, deterministic, and immediate. Agents cost credits, require reasoning, and introduce latency. Always start with native automation. Escalate to Custom Agents only when the logic requires AI to interpret or decide. Never use an agent to do what a two-step automation handles fine.

---

## Table of Contents

1. [The Automation Landscape — Four Layers](#1-the-automation-landscape--four-layers)
2. [Native Database Automations](#2-native-database-automations)
3. [Buttons — Manual and Database](#3-buttons--manual-and-database)
4. [Webhook Actions — Notion Outbound to External Tools](#4-webhook-actions--notion-outbound-to-external-tools)
5. [Integration Webhooks — Developer Inbound Events](#5-integration-webhooks--developer-inbound-events)
6. [The Automation vs. Agent Decision Rule](#6-the-automation-vs-agent-decision-rule)
7. [AI Connectors — Extending Agent and Q&A Context](#7-ai-connectors--extending-agent-and-qa-context)
8. [Native Integrations — What Each One Actually Does](#8-native-integrations--what-each-one-actually-does)
9. [External Automation Platforms](#9-external-automation-platforms)
10. [The Notion API](#10-the-notion-api)
11. [Integration Architecture — Decision Logic](#11-integration-architecture--decision-logic)
12. [Common Mistakes](#12-common-mistakes)
13. [Living Document Protocol](#13-living-document-protocol)

---

## 1. The Automation Landscape — Four Layers

Understanding which layer of Notion's automation stack to use for a given job is the most important skill in this document. Getting the layer wrong wastes money, creates fragility, or adds unnecessary complexity.

| Layer | What it is | Cost | When to use |
|---|---|---|---|
| **Native automations** | Property-triggered or button-triggered rules inside a database | Free | Deterministic if/then logic with no AI reasoning — set property, send Slack, create entry |
| **Webhook actions** | Outbound HTTP POST from Notion to an external tool on a trigger or button click | Free (Notion side) | Connecting native triggers to Make, Zapier, n8n, or any external service |
| **Custom Agents** | Autonomous AI workers that read context and make decisions | Credits from May 4, 2026 | Logic that requires reading page body content, classifying inputs, or drafting outputs |
| **External platforms** | Make, Zapier, n8n running complex multi-step flows triggered by Notion or on schedule | Platform pricing | Multi-step cross-tool workflows, polling Notion for changes, push-to-external publishing |

**The principle:** start at the cheapest, simplest layer. Move up only when the simpler layer can't handle it.

---

## 2. Native Database Automations

`Source: Official Notion help center | Last verified: Mar 4, 2026 | Confidence: High`

### 2.1 What they are

Database automations are rules attached to a specific database that fire automatically when a trigger condition is met. No AI, no credits, no external setup. They execute the moment the trigger fires.

**Access:** All paid plans. Slack notification automations available to Free plan users. Webhook actions require paid plans.

**Setup:** Click the ⚡ icon at the top of any database → New automation.

### 2.2 Triggers

| Trigger type | What fires it |
|---|---|
| **Property edited** | A specific property on a database entry changes value |
| **Page added** | A new entry is created in the database |
| **Page edited** | Any edit is made to an existing page in the database |

Triggers can be refined with filters — "When Status changes to In Review AND Priority = High." This narrows the automation to only the records it should act on.

**Critical limit:** Database automations cannot be triggered by other automations. They can only be triggered by human actions or button clicks. This prevents cascading automation chains but means you must plan multi-step flows carefully — either use a single automation with multiple actions, or connect to an external platform via webhook.

### 2.3 Actions

| Action | What it does | Plan required |
|---|---|---|
| **Set property** | Update one or more properties on the triggering entry | All paid |
| **Add page to** | Create a new entry in this database or another database | All paid |
| **Send Slack notification** | Post a message to a Slack channel | Plus, Business, Enterprise |
| **Send webhook** | POST to an external URL (Make, Zapier, n8n, etc.) | All paid |
| **Send email via Gmail** | Send email from a linked Gmail account | All paid (Gmail linked) |
| **Define variables** | Create a custom variable using mentions and formulas for use in other actions | All paid |

Multiple actions can be stacked in one automation — trigger once, execute several actions in sequence.

### 2.4 Key limits and failure behaviors

- **Max 5 webhook actions per automation**
- **Automations won't act on pages with restricted access** — check page sharing settings if an automation isn't firing
- **Automations may fail if they perform date calculations on empty fields** — use a filtered view scoped to entries where the field is populated
- **Failed automations pause automatically** and must be manually re-enabled — an exclamation mark appears on the automation
- **Recurring template automations do not trigger database automations** — only user-initiated actions and button clicks do
- **A page created by one automation creating a page does not trigger another automation** — human button click → creates page → that page creation CAN trigger a downstream automation, but automation → creates page → downstream automation will NOT fire

### 2.5 Copy-ready automation patterns

These are the highest-value, zero-cost automations. Build these before considering agents or external tools.

**Pattern 1 — Status-triggered assignment**
Trigger: Status changes to `In Review`
Action: Set property — Reviewer = [specific person]

**Pattern 2 — Completion timestamping**
Trigger: Status changes to `Published` (or `Done`, `Complete`)
Action: Set property — Completed On = formula: `now()`

**Pattern 3 — New entry Slack alert**
Trigger: Page added to database
Action: Send Slack notification to #[channel] — "New [item type] created: {{Name}}"

**Pattern 4 — Priority escalation**
Trigger: Status property edited AND Due Date < today (use filter)
Action: Set property — Priority = High

**Pattern 5 — Cross-database task creation**
Trigger: Status changes to `Approved`
Action: Add page to Tasks database — Title = "Review: {{Name}}", Project = [relation], Due Date = {{Publish Date}}

---

## 3. Buttons — Manual and Database

`Source: Official | Last verified: Mar 4, 2026 | Confidence: High`

### 3.1 Two button types

**Page buttons:** Placed inline on any Notion page. Clickable by anyone with edit access. Use for manual triggers — "Generate this week's report," "Send to Slack," "Create subtask."

**Database buttons:** A Button property type added to a database column. Appears as a clickable button on every row. Fires actions against the specific row it's clicked on. Use for per-row operations — "Approve this entry," "Trigger webhook for this record."

### 3.2 What buttons can do

Buttons support all the same actions as database automations, plus one critical capability automations don't have: **buttons CAN trigger database automations.** This is the bridge for multi-step flows that need a human checkpoint before executing.

Pattern: Human clicks button → button fires webhook to Make → Make executes multi-step external flow → (optionally) Make calls Notion API to update a property → that property change triggers a downstream Notion automation.

### 3.3 Using buttons as workflow control points

Buttons are underused for human-in-the-loop gates. Instead of automating every handoff, use a button to let a human review before committing:

- "Approve and notify" button: reviews the record visually, clicks to confirm — button sets Status to Approved, sends Slack notification
- "Send to CRM" button: fires a webhook to push a contact record to HubSpot only when a human decides it's ready
- "Publish" button: fires a webhook to Make which triggers the actual publishing flow

This pattern is more reliable and auditable than purely trigger-based automations for consequential actions.

---

## 4. Webhook Actions — Notion Outbound to External Tools

`Source: Official Notion help center | Last verified: Mar 4, 2026 | Confidence: High`

### 4.1 What they are

Webhook actions send an HTTP POST request from Notion to any URL you specify — a Make webhook listener, a Zapier Catch Hook, a custom server endpoint, or any service with a public URL. They fire from database automations or button clicks.

**Available on:** All paid plans (Free plan cannot use webhook actions except Slack notifications).

### 4.2 What gets sent

Only **database page properties** are sent in the payload — not page body content. This is a hard constraint. If you need page body content in the external workflow, you must follow up with a Notion API call (using the page ID sent in the payload) to retrieve the body.

Payload includes: the property values you select, automation ID, action ID, event ID, attempt number.

### 4.3 Known limits

- **POST requests only** — no GET, no PUT
- **Max 5 webhook actions per automation**
- **No authentication built in** — the webhook URL is the secret; keep it out of page bodies and shared views
- **No preview of payload before setup** — use webhook.site or a Make/Zapier test webhook to inspect the payload during setup
- **Workspace-level toggle:** Enterprise admins can disable webhook actions across the entire workspace via Settings → Connections

### 4.4 The two-step pattern for page body content

Because webhook actions only send properties, any flow that needs page body content (draft text, notes, brief content) requires a follow-up API call:

```
Notion fires webhook → payload contains page ID + properties
→ Make/Zapier receives webhook
→ Make/Zapier calls Notion API: GET /pages/{page_id}/blocks
→ Retrieves full page body as block content
→ Processes content (sends to AI, CMS, email, etc.)
```

This pattern adds one API call but makes the full page body available to any external workflow.

---

## 5. Integration Webhooks — Developer Inbound Events

`Source: Official Notion Developers docs | Last verified: Mar 4, 2026 | Confidence: High`

This is a distinct system from webhook actions. Integration webhooks are the **developer API** mechanism — they notify an external server when changes happen in a workspace, rather than Notion pushing outbound on a trigger.

### 5.1 What they do

An integration webhook subscription tells Notion: "When these event types occur in this workspace, send an HTTP POST to this endpoint." The workspace pushes events in near-real-time instead of the integration polling for changes.

**Event types include:** page.content_updated, page.created, page.deleted, database.schema_updated, comment.created, and others.

**Key characteristic:** Webhooks send **sparse payloads** — metadata only (event type, entity ID, timestamp). Your server receives the signal and must then call the Notion API to fetch the full updated content. This is intentional: it keeps payloads small and avoids sending stale data.

### 5.2 Who this is for

Developers building custom integrations against the Notion API — custom tools, enterprise data pipelines, proprietary internal systems. This is not a no-code feature. For no-code external connections, use webhook actions + Make/Zapier (Section 4 + Section 9).

### 5.3 Known limits

- New event type subscriptions don't auto-update — you must explicitly add new event types to your subscription
- No built-in delivery dashboard or logs in Notion — implement logging on your endpoint
- Initial indexing after connecting can take up to 36 hours for some connectors

---

## 6. The Automation vs. Agent Decision Rule

This decision framework is the most important thing in this document. Using an agent for something native automation handles is expensive and unnecessary. Using native automation for something that requires reasoning produces wrong or empty results.

`Cross-reference: Doc 3 Section 15 covers this in full depth with examples.`

| Use native automation when... | Use a Custom Agent when... |
|---|---|
| The logic is fully deterministic — trigger X always produces action Y | The action depends on reading and interpreting content |
| No AI reasoning is required | Classification, summarization, or drafting is required |
| Instant execution matters | A few minutes of latency on a schedule is acceptable |
| The action is structural (set property, create entry, send Slack) | The output requires judgment about what to write or decide |
| The same action always follows the same trigger without exceptions | There are edge cases requiring escalation |

**Examples — native automation:**
- "When Status changes to Done, set Completed On to today" → automation
- "When new entry is created in Requests DB, send Slack to #requests" → automation
- "When Status changes to In Review, assign Reviewer to Jane" → automation

**Examples — Custom Agent:**
- "When a new Slack message arrives in #support, read it, classify the request, and route to the correct owner" → agent (requires reading and classifying content)
- "Every Friday, summarize all completed tasks and draft a status report" → agent (requires synthesis and writing)
- "When a new contact is created, generate a contextual onboarding prompt based on their role and company" → agent (requires generating contextual content)

**The test:** Can you write the complete logic as a simple if/then without the word "decide," "interpret," "classify," or "write"? If yes → native automation. If no → Custom Agent.

---

## 7. AI Connectors — Extending Agent and Q&A Context

`Source: Official Notion help center | Last verified: Mar 4, 2026 | Confidence: High`

### 7.1 What they are

AI Connectors let the Personal Agent and Q&A search across connected external tools alongside Notion content. When you ask "What did we decide about the pricing model?" Notion AI can search Slack channels, Google Drive documents, GitHub issues, and Jira tickets — not just Notion pages — and surface a cited answer.

**Plan required:** Business and Enterprise. Full AI Connector access not available on Free or Plus.

**Setup requirement:** Workspace owner in Notion + admin in the connected app. Both roles required. This is a meaningful barrier in larger organizations where admin access is distributed.

### 7.2 Available connectors (verified March 2026)

| Connector | What it indexes |
|---|---|
| **Slack** | Public channels (+ private channels and DMs as of Notion 3.1) |
| **Google Drive** | Docs, Sheets, Slides in connected Drive |
| **GitHub** | Issues, PRs, repo content |
| **Microsoft Teams** | Channel messages |
| **Outlook** | Email |
| **SharePoint / OneDrive** | Documents and files |
| **Jira** | Issues, project data |
| **Linear** | Issues and projects |
| **Asana** | Tasks and projects |

### 7.3 Sync cadence and indexing limits

- **General new content:** Searchable on an hourly basis
- **Slack, GitHub, Teams:** Update every 30 minutes
- **Initial sync:** Up to 36–72 hours for first index after connection
- **Historical depth:** Typically pulls only the last year of data from connected apps

### 7.4 What AI Connectors enable vs. what they don't

**They enable:**
- Unified search across workspace + external tools in a single Q&A query
- Agents citing Slack threads, Drive documents, or Jira tickets in their responses
- Cross-tool context for the Personal Agent without manual copy-paste

**They do not enable:**
- Writing back to connected tools (Connectors are read-only for search/context)
- Real-time data (up to 30 min–1 hr lag, not live)
- Precise database-style queries across external tools (Q&A is semantic search, not a query engine)

**Disconnect behavior:** When you disconnect a tool, its content becomes unsearchable within the hour (for most connectors). It does not delete data from Notion — only removes it from the AI search index.

### 7.5 AI Connectors vs. MCP

These are different systems and often confused:

| | AI Connectors | MCP |
|---|---|---|
| **Purpose** | Extends Q&A and Agent context — read external content | Enables Custom Agents to take actions in external tools |
| **Direction** | Read-only (Notion reads from external tools) | Read + Write (agents read and act in external tools) |
| **Configured in** | Settings → Connections → AI Connectors | Custom Agent configuration |
| **Used by** | Personal Agent Q&A, Enterprise Search | Custom Agents only |

Use AI Connectors when you want Notion AI to know about your external tools. Use MCP (Doc 3) when you want Custom Agents to act in your external tools.

---

## 8. Native Integrations — What Each One Actually Does

`Source: Official Notion integrations page + verified docs | Last verified: Mar 4, 2026`

### 8.1 Slack

**What works natively:**
- Notion database automations can send Slack notifications (Plus, Business, Enterprise)
- Slack messages unfurl into rich Notion page previews when a Notion URL is shared in Slack
- `/notion create` command in Slack creates a new Notion database entry from Slack
- AI Connector: Notion AI searches Slack content (including private channels as of Notion 3.1)
- Custom Agent MCP: agents can read Slack and post to public channels

**Known limits:**
- Slack automation notifications can only be edited by the automation creator
- `/notion create` command works at channel level only — not inside Slack threads

### 8.2 Jira

**What works natively:**
- Synced Databases: view Jira issues inside Notion as a live database view (read-only unless Enterprise)
- Add Notion-side properties alongside synced Jira data (priority, notes, goals, etc.)
- Build dashboards combining Jira data with Notion project context
- Enterprise: edit key Jira fields directly from Notion and sync changes back (bidirectional as of Notion 3.2)
- AI Connector: Notion AI searches Jira issues and projects

**Known limits:**
- Jira Sync connection required (separate from standard Notion–Jira integration)
- Not all Jira database properties are supported in the sync
- Bidirectional editing is Enterprise-only

### 8.3 GitHub

**What works natively:**
- Synced Databases: view GitHub issues and PRs inside Notion
- AI Connector: Notion AI searches GitHub issues, PRs, and repo content (updates every 30 minutes)
- Enterprise Search can surface GitHub results in Q&A queries

**Known limits:**
- The GitHub integration shows basic metadata — not a deep issue tracking replacement
- Write-back to GitHub from Notion requires the API or an external automation tool

### 8.4 Google Drive

**What works natively:**
- Embed Google Docs, Sheets, and Slides directly on Notion pages
- AI Connector: Notion AI searches Drive documents
- Agents can reference Drive context when AI Connector is active

**Known limits:**
- Embeds are view-only inside Notion — editing requires opening the Drive file
- Drive sync is not bidirectional — changes in Drive don't auto-update Notion databases

### 8.5 Figma

**What works natively:**
- Embed Figma frames and FigJam boards on Notion pages (live preview)
- Custom Agent MCP: agents can access FigJam boards and convert boards to Notion docs, or turn docs into diagrams

**Known limits:**
- Embeds are view-only — editing requires opening Figma
- MCP actions are available via Custom Agents, not native automations

### 8.6 HubSpot

**What works natively:**
- Custom Agent MCP: agents can read and update HubSpot contacts and deals
- Data sync via Zapier/Make for property-level CRM updates

**Known limits:**
- No native bidirectional HubSpot–Notion sync without third-party middleware
- HubSpot write-back from agents requires MCP configuration (see Doc 3)

### 8.7 Notion Mail and Notion Calendar

**What works natively:**
- Full email client inside Notion workspace (manage multiple email addresses)
- AI Meeting Notes: transcription + structured summaries + action items, synced with Calendar
- Agents can read inbox, send email, schedule calendar events
- Notion Calendar connects to Google, Outlook, and iCloud — external events display in Notion Calendar

**Known limits:**
- Notion Calendar pulls in external events (read) but doesn't push Notion database entries as calendar events by default — that requires Make/Zapier automation
- Calendar sync is one-way for external calendar events — edits in Google Calendar don't automatically update a Notion database

---

## 9. External Automation Platforms

When native automations and webhooks aren't enough — multi-step cross-tool flows, complex conditional logic, polling Notion for changes, push-to-external publishing — external platforms fill the gap.

`Source: Verified community patterns | Last verified: Mar 4, 2026 | Confidence: High`

### 9.1 Platform comparison

| Platform | Best for | Notion support | Complexity |
|---|---|---|---|
| **Make (Integromat)** | Visual multi-step flows, advanced logic, moderate technical depth | Native Notion modules + webhook listener | Low–Medium |
| **Zapier** | Quick two-step automations, non-technical users, 7,000+ app library | Native Notion actions + Catch Hook | Low |
| **n8n** | Self-hosted, developer-built flows, complex branching, cost control | Full Notion API node | Medium–High |
| **Pipedream** | Developer-focused, code-optional, event-driven flows | Notion API | High |
| **Relay.app** | Notion-native feel, human-in-the-loop steps | Built for Notion workflows | Low–Medium |

### 9.2 The standard Notion → external platform pattern

```
1. Notion database automation fires on trigger
2. Webhook action sends POST to [Make/Zapier/n8n] webhook URL
3. External platform receives payload (properties only)
4. External platform calls Notion API GET /pages/{id}/blocks if body content needed
5. External platform executes the multi-step flow
6. (Optional) External platform calls Notion API to update a property on completion
7. (Optional) That property update triggers a downstream Notion automation
```

This pattern connects native Notion triggers to unlimited external complexity with no credits consumed on the Notion side.

### 9.3 The polling pattern (alternative)

When Notion webhook actions aren't the right trigger — for example, monitoring for changes across an entire database on a schedule:

```
External platform polls Notion API every N minutes
→ Queries database for entries matching a filter
→ Processes new or changed entries
→ Executes downstream actions
```

Polling is less efficient than webhooks (runs whether or not anything changed) but is more reliable for catching changes that might not fire a specific property trigger.

### 9.4 Choosing an external platform for this workspace

For a creator/operator context with moderate technical depth:
- **Make** is the primary recommendation — visual, Notion-native modules, handles complex branching, webhook listener setup is straightforward
- **Zapier** for simple two-step flows where Make is overkill
- **n8n** if self-hosting or cost control becomes a priority at scale

---

## 10. The Notion API

`Source: Official Notion Developers docs | Last verified: Mar 4, 2026 | Confidence: High`

The Notion API is a REST API for reading and writing workspace data programmatically. It is the foundation for all external integrations, including those built in Make, Zapier, and n8n.

### 10.1 Key capabilities

- Read and write pages, databases, and database entries
- Create, update, and query database properties
- Retrieve page block content (the full page body)
- Search the workspace
- Manage comments
- Subscribe to webhook events (integration webhooks)

### 10.2 Rate limits

The Notion API is rate-limited to **3 requests per second per integration**. For high-volume operations (bulk updates, large database queries), design flows to batch requests and implement backoff logic.

### 10.3 What the API cannot do

- Read file attachments (only file property metadata — name, URL)
- Access private pages the integration hasn't been given access to
- Execute formulas or rollups (those are computed by Notion, not retrievable as raw values)

### 10.4 Authentication

API integrations authenticate via a secret integration token. Internal integrations (for workspace use) are created in Settings → Connections → Develop or manage integrations. The integration must be connected to specific pages or databases to access them — it does not have workspace-wide access by default.

---

## 11. Integration Architecture — Decision Logic

Use this decision tree before building any integration:

```
Is the logic fully deterministic (trigger X → always do Y)?
├── YES → Use native database automation
│    └── Does it need to connect to an external tool?
│         ├── YES → Add webhook action → Make/Zapier/n8n
│         └── NO → Native automation is complete
│
└── NO (logic requires reading content, classifying, or writing output)
     └── Use Custom Agent (see Doc 3)
          └── Does the agent need to act in an external tool?
               ├── YES → Configure MCP in agent (see Doc 3 Section 8)
               └── NO → Agent operates within Notion only
```

**Additional questions:**
- Do you need cross-tool search for Q&A or agents? → AI Connector (Section 7)
- Do you need a human checkpoint before an action fires? → Button (Section 3)
- Do you need to react to changes across a large database on a schedule? → External platform polling (Section 9)
- Are you building a custom integration against the API? → Integration webhooks (Section 5) + Notion API (Section 10)

---

## 12. Common Mistakes

| Mistake | What breaks | The fix |
|---|---|---|
| Using Custom Agents for deterministic logic | Unnecessary credit consumption; agents add latency and reasoning cost to simple if/then | Native automation covers deterministic logic for free |
| Expecting automation to cascade (automation → automation) | Second automation never fires | Either stack actions in one automation, or use a button click as the bridge |
| Webhook action with no follow-up API call for body content | External flow receives only properties — no draft text, no brief, no notes | Add a Notion API GET blocks call in Make/Zapier step 2 to retrieve page body |
| AI Connector setup without admin access in both tools | Connection fails silently or indexes incompletely | Confirm workspace owner in Notion + admin in connected app before setup |
| Conflating AI Connectors with MCP | Connecting a tool as an AI Connector doesn't let agents act in it | AI Connectors = read for context. MCP = act in the tool (configured separately in agent) |
| Polling Notion API without rate limit handling | Integration gets throttled at 3 req/sec; bulk operations fail | Implement backoff and batching; design flows to stay under rate limit |
| Building Make/Zapier flows before validating the native automation can't do it | Over-engineered solution; external platform dependency where none needed | Always check native automation first; only go external when native can't handle it |
| Slack notification automation edited by wrong person | Slack automations can only be edited by the creator; becomes locked if creator leaves | Document who owns each Slack automation; reassign before offboarding |
| Expecting Notion Calendar to push entries as Google Calendar events | Calendar sync is one-way by default — external events show in Notion, but Notion entries don't become Google Calendar events | Build a Make/Zapier flow to push dated Notion entries to Google Calendar when needed |
| Automation acting on restricted-access pages | Automation fires but action silently fails | Confirm page sharing settings include the automation's access scope |

---

## 13. Living Document Protocol

**Rebuild triggers:**
- Notion adds new native automation action types
- New AI Connectors added to the official list
- Jira bidirectional sync capabilities expand beyond Enterprise
- MCP admin controls ship (flagged in Doc 0 as upcoming)
- Notion Workers exits pre-alpha — may introduce new integration patterns
- Rate limits change on the Notion API
- A new external platform becomes preferred over Make/Zapier for this workspace

**Cross-references:**
- Doc 0: Platform truth, agent governance, upcoming features
- Doc 1: Project/Task automation patterns (status-triggered handoffs, sprint rollover)
- Doc 2: CRM automation patterns (stale contact flagging, deal stage transitions)
- Doc 3: Custom Agents (full depth), MCP integrations, credits, security
- Doc 4: Content automation patterns (publishing queue digest, stale draft monitor)
- Doc 5: Wiki automation patterns (page review triggers, knowledge base maintenance)

**Version history:**
- v1.0 — March 4, 2026 — Initial build by Claude. Verified against official Notion automation, webhook, AI Connector, and integrations docs. Source-tagged throughout.

---

*Notion Codex — Doc 6: Automations & Integrations | v1.0 | March 4, 2026*
*Load Doc 0 alongside this document. Written with Claude.*
