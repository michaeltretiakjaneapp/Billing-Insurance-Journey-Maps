---
name: journey-map
description: "Create interactive journey maps from any research or process data. Use this skill to: create journey maps, service blueprints, user flows, or experience maps; synthesize research into structured insights; visualize workflows with pain points, opportunities, and supporting evidence; build swimlane diagrams showing which actors perform which tasks; or map any end-to-end workflow. Trigger on: 'journey map', 'service blueprint', 'swimlane', 'user flow', 'experience map', 'pain points and opportunities', 'research synthesis', 'workflow visualization', 'map out this process', 'visualize this workflow', 'synthesize these interviews', 'map this customer experience'. Works with any domain: product, marketing, sales, support, operations, healthcare, finance, and more. Source material can include interview transcripts, community posts, app reviews, NPS comments, support tickets, survey responses, user stories, or verbal descriptions."
---

# Journey Map Skill

Create interactive, horizontally-scrolling HTML journey maps from any research data or workflow description. Supports two views: a column-based step view and a swimlane (actor-row) view with SVG flow connectors.

## When to Use

- User has research interviews, community posts, product feedback, or process docs to synthesize
- User wants to map a workflow, process, customer journey, or service experience
- User wants to visualize pain points and opportunities across a process
- User asks for a service blueprint, swimlane, or experience map
- User wants to see which actors (roles, teams, systems) are involved at each step
- User wants to update or extend an existing journey map

---

## Process

### Step 0: Detect Context

Before building anything, understand the situation:

**Domain** — What product, service, or process is being mapped? (e.g. SaaS onboarding, retail checkout, customer support, hiring pipeline)

**Audience** — Who will primarily use this map?
- **Designers** — Focus on friction, interaction quality, emotional tone
- **Product Managers** — Focus on jobs-to-be-done, drop-offs, prioritization signals
- **Marketers** — Focus on awareness moments, messaging gaps, conversion points
- **Developers** — Focus on system touchpoints, handoffs, technical pain points

**Source material** — What does the user have? (Identify which types apply)
- Interview transcripts or notes
- Community posts, forum threads, Slack/Discord messages
- App reviews, NPS comments, support tickets, surveys
- User stories, process documentation, verbal descriptions

**Propose actors** — Based on the domain, propose 4–6 actors. Do not use generic defaults. For a SaaS product you might propose: User, Admin, Support Agent, System. For a retail experience: Customer, Store Associate, Inventory System, Manager. Confirm with the user before proceeding.

**Actor colors** — Assign each actor a background and text color from this palette:

| Option | Background | Text   |
|--------|-----------|--------|
| A      | #B2EBF2   | #006064 |
| B      | #F8BBD0   | #880E4F |
| C      | #FFE0B2   | #E65100 |
| D      | #D1C4E9   | #4A148C |
| E      | #C8E6C9   | #1B5E20 |
| F      | #E0E0E0   | #424242 |
| G      | #BBDEFB   | #0D47A1 |
| H      | #FFF9C4   | #827717 |

---

### Step 0.5: Onboarding (when no source material is provided)

If the user has **not provided any source material** — no transcripts, documents, reviews, or pasted data — run through these three questions **before doing anything else**. Ask them together in a single message so the user can answer all at once.

> **Before I build the map, a few quick questions:**
>
> 1. **What are we mapping?** What part of the product or user experience should this cover? *(e.g. onboarding, checkout, support escalation, hiring flow)*
>
> 2. **What sources should I draw from?** You can:
>    - Upload or paste transcripts, reviews, survey responses, or docs
>    - Let me search connected tools automatically — I can pull from **Slack**, **Jira**, **Confluence**, **Fellow**, and any other tools connected to this workspace
>    - Both — upload what you have and I'll also search for more
>
> 3. **Include linked Jira issues?** I can attach open epics, stories, and bugs to each task so the map shows active work in progress alongside the insights. *(Recommended — it makes the map far more actionable)*

Once you have the answers:
- If the user names a product area or process, use that to drive the domain detection in Step 0 and the search queries in Step 1
- If the user says to search connected tools, proceed with Step 1 using the named domain as the query focus
- If the user says yes to Jira issues, make linking issues via `data-jira-items` a priority throughout Step 5
- If the user uploads or pastes data after answering, treat it as the source material for Step 2

**Do not skip this step.** A map built without understanding the target area or available sources will be generic and low-value. Two minutes of onboarding produces a dramatically better output.

---

### Step 1: Search Connected Sources

Before reviewing any provided source material, **proactively search connected tools** for insights relevant to the domain. This surfaces quotes, tickets, and feedback the user may not have thought to share directly. Run all searches in parallel where possible.

#### What to search for across all tools
- User complaints, friction, confusion, or workarounds related to the domain
- Feature requests or improvement ideas
- Support escalations, bug reports, or recurring issues
- Retrospective notes, post-mortems, or design reviews
- Meeting discussions or decisions touching on the journey area

#### Slack (`mcp__claude_ai_Slack_Runlayer__search`)
Search for conversations that surface real user language and internal team sentiment. Useful queries:
- `"[product/feature name] confusing"` or `"[feature] broken"`
- `"[process] takes too long"` or `"wish we could"`
- Channel-specific searches: support channels, product feedback channels, customer success

Classify found Slack messages as `data-source="community"` if they are user/customer-facing, or as `data-source="interview"` if they are direct customer quotes shared internally.

#### Jira & Confluence (`mcp__claude_ai_Atlassian_RunLayer__search`, `searchJiraIssuesUsingJql`, `searchConfluenceUsingCql`)
- Search for open bugs, support tickets, or feature requests in Jira — these are high-signal pain points with real frequency data
- Search Confluence for process documentation, research reports, or retrospective pages
- Use JQL for targeted issue searches: `project = SUPPORT AND text ~ "[domain term]" ORDER BY created DESC`
- Classify Jira tickets as `data-source="feedback"`; Confluence research pages may yield interview-style quotes

**CRITICAL — Always link Jira issues to tasks:** When you find relevant Jira epics, stories, bugs, or tasks, you MUST attach them to the corresponding map task using `data-jira-items`. Do not just extract the text insight — link the live issue. This gives the map real-time traceability to work in progress. A collapsed task with linked issues shows a compact Jira badge (logo + count) inline with the quote bar, so viewers can see at a glance whether a pain point or opportunity has active work behind it.

#### Fellow (`mcp__claude_ai_Fellow__search_meetings`, `get_action_items`)
- Search meeting notes for discussions about the domain, user friction, or product decisions
- Look for recurring action items that signal unresolved pain points
- Check recent 1:1s, retrospectives, or customer calls for direct user quotes
- Classify meeting notes as `data-source="interview"` when they contain direct quotes from users or customers; use `data-source="feedback"` for aggregated team observations

#### Other connected tools
Check for any other MCP tools available in the current environment (e.g. Notion, Linear, Intercom, HubSpot, GitHub Issues). If present, search them using the domain keywords identified in Step 0.

#### After searching
1. Group findings by source type (interview, community, feedback)
2. Note any high-frequency patterns — e.g. "8 Jira tickets mention [X]" is stronger evidence than a single mention
3. Only use verbatim quotes where the exact wording is available; otherwise summarise and cite the count
4. If searches return nothing relevant, note it briefly and proceed — do not pad the map with tangential results

---

### Step 2: Understand and Classify Source Material

**CRITICAL — Read every source in full before extracting anything.**

Before you extract a single insight, enumerate every piece of source material available: every file, every document, every paste, every search result from Step 1. Write a brief internal inventory:

> Sources identified: [list them — e.g. "Interview A (Sarah, 45 min)", "Interview B (Marcus, 30 min)", "App Store reviews (22 posts)", "Jira search results (8 tickets)", "Fellow meeting: Product Review 2025-01"]

Then read all of them before proceeding to Step 4. Do not begin extracting insights after the first document and assume it is representative. Maps built from one or two sources while others sit unread are systematically biased — they over-represent whoever spoke longest or wrote most, while silencing other participants entirely. Every source deserves equal attention.

**If there are many documents**, process them in order and keep a running note of which are done. Do not move to Step 3 or Step 4 until all sources have been read.

Handle each source type differently:

#### Interviews
- Extract insights from transcripts or notes
- Use **verbatim quotes** — never paraphrase, truncate, or reword
- Attribute as `— Name` or `— Role`
- Tag as `data-source="interview"`

#### Community Posts (forums, social media, Slack, Discord, Reddit)
- Extract notable quotes when they are specific and quotable verbatim
- For high-volume themes, aggregate: *"12 posts described difficulty finding the export button"*
- Attribute as `— Community Post` or with a public username
- Tag as `data-source="community"`

#### Product Feedback (app reviews, NPS comments, support tickets, surveys)
- Extract verbatim quotes when they are specific and useful
- Include rating context when available: `— 2★ Review` or `— Support Ticket`
- For high-volume patterns, aggregate with counts and representative quotes
- Tag as `data-source="feedback"`

---

### Step 3: Define the Structure

Extract and confirm:

**Goals** — What are the primary goals of the journey participants? (3–5 goals, shown as chips at the top)

**Activities** — The major phases of the journey (numbered 0–N). Each activity contains 1–3 steps.

**Steps** — Sequential stages within each activity (numbered X.1, X.2, etc.). Each step contains tasks.

**Tasks** — The specific actions performed. Each task must have:
- An **actor** assignment
- Zero or more **pain points** with nested verbatim quotes (tagged by source) — use `card-pain`
- Zero or more **opportunities** with nested verbatim quotes (tagged by source) — use `card-opp`
- Zero or more **suggested pain points / opportunities** inferred from general knowledge (no quotes) — use `card-pain-sug` / `card-opp-sug`

---

### Step 4: Extract Insights

For each task, gather:

**Pain Points** — Problems, friction, workarounds, complaints, failure modes. Each pain point has:
- A concise description of the problem
- One or more verbatim quotes nested inside (each tagged by source)

**Opportunities** — Ideas, wishes, requests, potential improvements. Each opportunity has:
- A concise description of the opportunity
- One or more verbatim quotes nested inside (each tagged by source)

**CRITICAL: Quotes must be verbatim.** Never paraphrase, truncate, or reword quotes from source material. Always attribute. Cross-reference against original source before finalizing.

**CRITICAL — Source coverage check.** Before finalizing the insight set, run a coverage check against the inventory you created in Step 2:

- For each source: has it contributed at least one quote or data point to the map?
- If a source contributed nothing, ask: did you actually read it in full, or did you stop early because the map seemed full enough?
- A source that contributed nothing should be noted explicitly (e.g. "Interview C: no quotes surfaced relevant to this journey") — not silently omitted
- Quotes from the same person or document appearing in more than 3 tasks is a signal you may have over-relied on that source; go back to under-represented sources

The goal is that someone reading the finished map could trace every card back to its source and trust that the full body of evidence was consulted — not just the most accessible document.

---

### Step 5: Write the Markdown Preview

Before writing any HTML, write a `.md` file of the full journey map structure. This gives the user something to read and review immediately while the HTML is being built.

**Write the file to the same directory as the output HTML**, named `[map-name]-preview.md`. Write it using the `Write` tool — do not just output it to chat.

The markdown file should contain:

```
# [Map Title]
[Subtitle / product area] · [date]

## Goals
- [Goal 1]
- [Goal 2]
...

---

## [Activity Name]

### [Step X.1 — Step Title]

**[Actor] — [Task Name]**
- 🔴 Pain: [pain point description]
  > "[verbatim quote]" — Source
- 🟢 Opportunity: [opportunity description]
  > "[verbatim quote]" — Source
- 💡 Suggested pain: [suggestion text] *(no evidence yet)*
- 💡 Suggested opportunity: [suggestion text] *(no evidence yet)*
- 🔗 Jira: [KEY-123] [title] ([status])

**[Actor] — [Task Name]**
...

### [Step X.2 — Step Title]
...
```

Rules:
- Include every activity, step, task, pain point, opportunity, and quote — this is a complete structural preview, not a summary
- Use `🔴` for evidence-backed pain, `🟢` for evidence-backed opportunities, `💡` for suggestions (both pain and opportunity), `🔗` for linked Jira issues
- Verbatim quotes go on their own indented line prefixed with `>`
- Attribute every quote exactly as it will appear in the HTML

**Immediately after writing the `.md` file, begin writing the HTML.** Do not pause, do not ask for feedback on the preview — just announce `"Preview written to [filename].md — building the interactive map now."` and proceed directly to Step 6.

---

### Step 6: Build the HTML

**CRITICAL: You MUST use the template at `template.html` (in the skill root) as the literal starting point for every map. Read it with the `view` tool, then copy it in full and replace only the content placeholders. Do NOT write HTML from scratch, do NOT recreate the CSS, do NOT omit or rewrite any JavaScript. Every journey map must be structurally identical to the template — same CSS variables, same class names, same JS functions, same layout system. The only things you should change are: the `<title>`, the `#journey-data` JSON block, the `.page-header` title/subtitle/meta, the `.top-bar` goal chips, and the `.map-row` / `#swimlane-view` content sections.**

#### Quote link icon — auto-injected per task

Each task header automatically gets a small speech-bubble icon button (injected by `initQuoteLinks()` on init) when the task has source quotes. Clicking it opens a floating popover showing all the task's nested quotes with their source badges. This does NOT require any manual HTML — it's added by JS. If you want to include it explicitly in the HTML (e.g. for the first task in the template), use:

```html
<button class="task-quote-link" title="View source quotes" onclick="toggleQuotePopover(event,this)">
  <svg viewBox="0 0 16 16" xmlns="http://www.w3.org/2000/svg"><path d="M2 3a1 1 0 0 1 1-1h10a1 1 0 0 1 1 1v7a1 1 0 0 1-1 1H9l-2 2-2-2H3a1 1 0 0 1-1-1V3zm3 2a.5.5 0 0 0 0 1h6a.5.5 0 0 0 0-1H5zm0 2a.5.5 0 0 0 0 1h4a.5.5 0 0 0 0-1H5z"/></svg>
  <div class="quote-popover"><div class="quote-popover-title">Source Quotes</div></div>
</button>
```

Place it inside `.task-header` — the `handleTaskHeaderClick` function ignores clicks on this button so the task doesn't expand/collapse when the icon is clicked.

**Also update task-header onclick to use `handleTaskHeaderClick`:**
```html
<div class="task-header" onclick="handleTaskHeaderClick(event,this)">
```

#### Jira panel — collapsible section within task-details

Every task can have a collapsible Jira panel showing related epics, projects, stories, bugs, or tasks. Two ways to add it:

**Option A — data attribute (recommended for dynamic/programmatic maps):**
Add `data-jira-items` JSON on the `.task-group` element. JS will auto-create and populate the panel on init:

```html
<div class="task-group" data-actor="user" data-activity="1" data-step="1.1"
  data-jira-items='[
    {"type":"epic","key":"ENG-42","title":"Billing Refactor","status":"in-progress","url":"https://yourorg.atlassian.net/browse/ENG-42"},
    {"type":"story","key":"ENG-55","title":"DOI field validation","status":"todo","url":"https://yourorg.atlassian.net/browse/ENG-55"},
    {"type":"bug","key":"ENG-61","title":"Date of injury resets on save","status":"in-progress"}
  ]'>
```

Supported `type` values: `epic`, `story`, `bug`, `task`, `project`
Supported `status` values: `todo`, `in-progress`, `done` (controls badge color)
`url` is optional — if provided, the item renders as a link.

**Option B — manual HTML (for authored maps):**
Add a `.jira-panel` block inside `.task-details`:

```html
<div class="jira-panel">
  <div class="jira-panel-header">
    <svg class="jira-panel-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M12.004 2.003 7.4 6.607l2.5 2.5 2.104-2.104 4.5 4.5-2.1 2.1 2.5 2.5 4.6-4.6-9.5-9.5z" fill="#2684FF"/><path d="M4.6 9.397 2.004 12l9.5 9.5 4.6-4.6-2.5-2.5-2.1 2.1-4.5-4.5 2.1-2.1-2.504-2.503z" fill="#2684FF"/></svg>
    Related Jira Issues
    <span class="jira-panel-toggle">▶</span>
  </div>
  <div class="jira-panel-body">
    <a class="jira-item" href="https://yourorg.atlassian.net/browse/ENG-42" target="_blank" rel="noopener noreferrer">
      <div class="jira-item-type epic">E</div>
      <div class="jira-item-meta">
        <div class="jira-item-key">ENG-42</div>
        <div class="jira-item-title">Billing Refactor</div>
        <span class="jira-item-status in-progress">In Progress</span>
      </div>
    </a>
  </div>
</div>
```

Type letter codes: `E` = epic, `S` = story, `B` = bug, `T` = task, `P` = project
Status class variants: `todo`, `in-progress`, `done`

**Collapsed Jira indicator — auto-injected:** When a task has linked Jira items, `injectJiraIndicators()` automatically appends a compact badge (Jira logo + item count) to the collapsed task header, shown inline with the quote/suggestion bar at the bottom of `.task-text`. This requires no manual HTML — it is injected on init and hidden when the task is expanded. The badge appears whether items were added via `data-jira-items` or manually authored `.jira-item` elements.

The indicator is designed to sit alongside the quote bar so that a single glance at a collapsed task reveals three things: how much evidence exists (solid bar cells), how many hypotheses exist (hollow bar cells), and whether active Jira work is already tracking this area (Jira badge).



Every actor badge must use inline `style` with the actor's assigned background and text color:
```html
<span class="actor-badge" data-actor="user" style="background:#F8BBD0;color:#880E4F">User</span>
```

Every `task-group` element needs three data attributes:
```html
<div class="task-group" data-actor="user" data-activity="2" data-step="2.1">
```

#### Quote format — include source badge

All nested quotes must include a `data-source` attribute and a `<span class="quote-source-badge">` as the first child:
```html
<div class="nested-quote" data-source="interview">
  <span class="quote-source-badge">Interview</span>
  "Exact verbatim quote from the transcript." — Name
</div>

<div class="nested-quote" data-source="community">
  <span class="quote-source-badge">Community</span>
  "Quote from forum or community post." — Username
</div>

<div class="nested-quote" data-source="feedback">
  <span class="quote-source-badge">Feedback</span>
  "Quote from app review or support ticket." — 3★ Review
</div>
```

For aggregated insights without a single verbatim quote, omit the `nested-quote` and include the count in the card description:
```html
<div class="card card-pain">
  <div class="card-type">Pain Point</div>
  12 community posts described difficulty finding the export button.
  <div class="nested-quote" data-source="community">
    <span class="quote-source-badge">Community</span>
    "Most representative verbatim example of the theme." — Username
  </div>
</div>
```

#### Full pain point / opportunity structure
```html
<div class="card card-pain">
  <div class="card-type">Pain Point</div>
  Concise description of the problem
  <div class="nested-quote" data-source="interview">
    <span class="quote-source-badge">Interview</span>
    "Exact verbatim quote." — Name
  </div>
</div>

<div class="card card-opp">
  <div class="card-type">Opportunity</div>
  Concise description of the opportunity
  <div class="nested-quote" data-source="feedback">
    <span class="quote-source-badge">Feedback</span>
    "Exact verbatim quote." — 4★ Review
  </div>
</div>
```

#### Suggestion cards — insights without source evidence

When a task has no supporting source material (no quotes, no research data), but a pain point or opportunity is reasonable to infer from general domain knowledge, use **suggestion cards** instead of standard evidence-backed cards. These are visually distinct (dashed border, lighter background) and clearly labelled.

**CRITICAL: Never use `card-pain` or `card-opp` for inferred content. Always use `card-pain-sug` / `card-opp-sug`.** Suggestions must never contain `nested-quote` elements — if you have a quote, it belongs in a standard card.

```html
<div class="card card-pain-sug">
  <div class="card-type">Suggested Pain Point</div>
  Description of the suspected friction, framed as a hypothesis. Note that this is based on general knowledge — no source evidence yet.
</div>

<div class="card card-opp-sug">
  <div class="card-type">Suggested Opportunity</div>
  Description of a potential improvement, framed as a hypothesis. Flag for future research validation.
</div>
```

**Collapsed task indicator:** When a task contains suggestion cards, the collapsed quote bar shows **hollow outline cells** (red outline for suggested pains, green outline for suggested opportunities) instead of the solid filled cells used for evidence-backed cards. This lets viewers scan the map and immediately distinguish between researched findings and general-knowledge hypotheses without expanding each task.

- Solid red cell → evidence-backed pain point (has verbatim quote)
- Solid green cell → evidence-backed opportunity (has verbatim quote)
- Hollow red outline → suggested pain point (no source evidence)
- Hollow green outline → suggested opportunity (no source evidence)

A task can have a mix: e.g. one evidence-backed pain (solid) and one suggested opportunity (hollow) will both appear in the bar.

#### JSON data layer — must be populated

Update the `<script type="application/json" id="journey-data">` block with the real journey structure. This is used by the page's JS (to initialize actors and activities) and by Claude (to understand and update the map later):

```json
{
  "title": "Your Map Title",
  "subtitle": "Brief description of what this map covers and who it's for",
  "version": "1.0",
  "lastUpdated": "YYYY-MM-DD",
  "goals": [
    {"id": "G1", "text": "First goal"},
    {"id": "G2", "text": "Second goal"}
  ],
  "actors": [
    {"id": "user", "label": "User", "emoji": "👤", "bg": "#F8BBD0", "color": "#880E4F"},
    {"id": "system", "label": "System", "emoji": "🖥", "bg": "#B2EBF2", "color": "#006064"}
  ],
  "activities": [
    {"num": "0", "name": "Activity Name", "steps": [
      {"id": "0.1", "title": "Step Title"},
      {"id": "0.2", "title": "Second Step Title"}
    ]},
    {"num": "1", "name": "Second Activity", "steps": [
      {"id": "1.1", "title": "Step Title"}
    ]}
  ]
}
```

**Important:** The `actors` array and `activitiesWithSteps` JS array are now auto-populated from this JSON. Do not manually define them in the `<script>` block — the template handles this automatically.

---

### Step 7: Customize Actors

Actors are now fully defined in the JSON data layer and as inline styles on badges. There are no CSS variables to update. The legend is built automatically from the `actors` array.

To add or change an actor:
1. Add it to the `actors` array in `journey-data` JSON
2. Use the correct `data-actor` value on each `task-group` and `actor-badge`
3. Apply the correct inline style on each badge: `style="background:BG;color:TEXT"`

---

### Step 8: Verify Quotes

Before delivering the final map, verify ALL quotes:
1. Search project knowledge or source files for each quote
2. Confirm it is word-for-word verbatim
3. Confirm the attribution and source tag are correct
4. Fix any paraphrased, truncated, or reworded quotes

---

### Step 9: Updating an Existing Map

When the user provides an existing journey map HTML file:

1. **Read the file** — look for `<script type="application/json" id="journey-data">` to understand the current structure (actors, activities, steps)
2. **Read the DOM** — for any content the user may have edited inline in the browser, the DOM content (not just the JSON) is the source of truth
3. **Understand the update** — what is being added, changed, or removed?
4. **Regenerate** — update both the HTML body content and the JSON data layer to reflect the changes. Increment the version if significant; always update `lastUpdated`
5. **Preserve all existing insights** — only modify what was explicitly requested; do not silently remove or reword existing content

---

### Step 10: Check for Relevant Jira Work

After the map is complete (new or updated), run a Jira coverage pass to ensure every task that has an open pain point or opportunity is checked against active work in Jira. This step catches issues that weren't found in Step 1 — sprint tickets created since the last search, work that wasn't obviously named after the domain terms, or issues the team simply hadn't linked yet.

**For each task that has at least one `card-pain` or `card-opp` card:**

1. Search Jira for issues related to that specific pain or opportunity:
   ```
   mcp__claude_ai_Atlassian_RunLayer__searchJiraIssuesUsingJql
   text ~ "[key phrase from pain/opportunity]" AND statusCategory != Done ORDER BY updated DESC
   ```
2. Also try a broader domain search if the specific phrase yields nothing:
   ```
   project IN (...) AND text ~ "[step or actor area]" AND statusCategory != Done
   ```
3. If relevant issues are found that aren't already in `data-jira-items` for that task, add them.

**After the pass, report back to the user:**
- How many tasks were checked
- How many new Jira issues were linked (if any)
- Any tasks where a clear pain or opportunity exists but no Jira issue was found — flag these explicitly as gaps: *"No open Jira issue found for [pain point] — this may be untracked work"*

**Do not silently skip tasks** because the map already has some Jira links. A task with one linked issue may have three more relevant tickets sitting in another project. Check every evidence-backed pain and opportunity card.

---

## Template Reference

The full HTML template is at: `template.html` (in the skill root — same directory as this SKILL.md).

**Read this file with the `view` tool before building any journey map and use it as your literal base — copy it in full, then fill in the content.** It contains:
- Complete CSS design system (colors, cards, layout, animations)
- Column view structure
- Swimlane view with SVG flow connectors (step-to-step purple lines, task-to-task yellow lines)
- Fixed header that pins on scroll (JS-driven, syncs horizontal scroll)
- Quote bar injection system (solid red/green = evidence-backed; hollow red/green outline = suggestions)
- Suggestion cards (`card-pain-sug`, `card-opp-sug`) for general-knowledge hypotheses with no source evidence
- Source filter controls (filter map to show only Interview, Community, or Feedback quotes)
- Stats bar (shows pain point, opportunity, task, and suggestion counts)
- Edit mode (makes content editable in-browser)
- Export / Download button (serializes current state to a new HTML file with updated JSON)
- Expand/collapse animations
- Dynamic arrow redrawing on expand/collapse/resize
- JSON data layer (`#journey-data`) for structured data and Claude re-reads

---

## Output

Always output as a single self-contained `.html` file in `/mnt/user-data/outputs/`. The file should render correctly when opened directly in a browser with no external dependencies (except the Google Fonts import for DM Sans).

---

## Example Prompts

- "Create a journey map from these interview transcripts"
- "Map out our onboarding flow with pain points and opportunities"
- "Build a swimlane showing which team members handle each step"
- "Synthesize these community posts and app reviews into a journey map"
- "Visualize the checkout experience across all our customer touchpoints"
- "Add these 5 new interviews to the existing journey map"
- "Update the map — we've fixed the payment step pain point, add the resolution"
- "Here's the updated transcript — rebuild the discovery phase of the map"

---

## Actor Color Palette (Quick Reference)

```
A: bg #B2EBF2 / text #006064  — Teal
B: bg #F8BBD0 / text #880E4F  — Pink
C: bg #FFE0B2 / text #E65100  — Orange
D: bg #D1C4E9 / text #4A148C  — Purple
E: bg #C8E6C9 / text #1B5E20  — Green
F: bg #E0E0E0 / text #424242  — Gray
G: bg #BBDEFB / text #0D47A1  — Blue
H: bg #FFF9C4 / text #827717  — Yellow
```
