---
name: marketing-calendar-audit
description: >
  Audit a Planable content calendar against real demand. Maps what's scheduled, what's missing a
  date, what has publishing errors and what risks going out unapproved — then overlays live search
  and question demand (SE Ranking) for the brand's core topics to flag what the audience is asking
  for that the calendar doesn't cover, with fill proposals routed to marketing-post-batch. Use
  whenever the user asks "what's scheduled this week and what are we missing", "does our calendar
  match what people search", "calendar audit with demand", "gaps in the schedule", "review the
  content plan for [client]", or wants a weekly/monthly calendar review that judges substance,
  not just dates. Requires both the SE Ranking and Planable MCPs. For a schedule-only review
  without the demand layer, use content-calendar-audit from the smm-skills plugin instead, when that plugin is installed.
---

# Calendar × demand audit

Review a Planable workspace's content schedule for a given period — what's ready, what's missing, what needs attention — and then the question a plain calendar audit never answers: **does what's scheduled match what the audience is actually asking for right now?**

## How this skill works

1. Identify the workspace and time window
2. Fetch scheduled and unscheduled posts
3. Surface gaps, issues, and a clean calendar view
4. **Overlay live demand for the brand's core topics (SE Ranking) — the signature step**
5. Flag anything that needs action, with demand-backed fill proposals

## Connector health check

Verify both MCPs before starting: `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to reconnect it (SE Ranking guide: https://seranking.com/api/integrations/mcp/ · Planable guide: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools).

---

## Step 1: Gather inputs

- **Workspace / client name** — which workspace to audit
- **Time window** — default to the next 7 days if not specified; accept "this week", "next week", "this month", or a custom range
- **Platforms** — all pages by default, or specific platforms if requested
- **Brand domain + 3–5 core topics** — for the demand overlay (ask if unknown; core topics = the themes this brand should own, e.g. for a scheduling tool: "social media scheduling", "content approval", "instagram planning")

---

## Step 2: Fetch posts

```
list_workspaces → find workspaceId
list_pages(workspaceId) → get pageIds for context

// Scheduled posts in window
list_posts(workspaceId, postStatus: ["SCHEDULED"], scheduledAfter: [start], scheduledBefore: [end])

// Posts with no date set (drafts that may be intended for this period)
list_posts(workspaceId, postStatus: ["NO_DATE_SET"])

// Posts with publishing errors
list_posts(workspaceId, postStatus: ["WITH_ERRORS"])
```

Also check for posts needing approval that are scheduled soon:
```
list_posts(workspaceId, approvalStatus: ["NOT_APPROVED", "PARTIAL_APPROVED"], scheduledAfter: [start], scheduledBefore: [end])
```

---

## Step 3: Build the audit view

### Calendar overview

Show scheduled posts grouped by day. For each day:
- List platform + post preview (first 80 chars)
- Flag approval status inline (✅ approved / ⏳ pending / ❌ not approved)
- Flag any errors (🔴)

**Example format:**
```
Monday, May 19
  • LinkedIn — "Three things we learned running 50 client campaigns..."  ✅
  • Instagram — "Behind the scenes of our Q2 content sprint..."  ⏳ pending approval

Tuesday, May 20
  ⚠️ Nothing scheduled

Wednesday, May 21
  • Facebook — "Meet the team: this month we're spotlighting..."  ✅
  🔴 Instagram — Publishing error — needs attention
```

### Schedule gap analysis

- **Total posts scheduled:** [n] across [n] platforms
- **Days with no content:** list them
- **Posts with no date set:** [n] drafts not yet placed on the calendar
- **Posts with errors:** [n] — list them with platform and preview
- **Posts scheduled but not yet approved:** [n] — flag urgency if within 24–48h

---

## Step 4: Demand overlay (signature step)

For each core topic, pull what the audience is asking right now (sequentially; ~10 req/s):

- `DATA_getKeywordQuestions` on the topic seed — live questions people ask. Aggregate across 2–3 seeds per topic; single seeds returning 1–2 questions is normal.
- `DATA_getRelatedKeywords` — the topic's active demand family with volumes.

Then compare against the scheduled posts (topical judgment, not substring matching): does anything in the window address this topic?

**Demand coverage table:**

```
| Core topic | Demand evidence | In calendar this window? | Verdict |
|---|---|---|---|
| content approval | 1.9k/mo family + 6 live questions | 0 posts | ❗ uncovered demand |
| instagram planning | 3.2k/mo family | 2 posts (Tue, Thu) | ✅ covered |
| social media scheduling | 8.1k/mo family + 11 questions | 1 post (Mon) | ⚠️ thin vs demand size |
```

Uncovered demand + an empty day = the strongest possible fill recommendation: it says exactly *what* to post and *when*, backed by a number.

### Recommendations

2–4 short, specific callouts that combine both layers. Examples:
- "Wednesday is empty and 'content approval workflow' (1.9k/mo, 6 live questions) has zero coverage — one post fills both holes."
- "2 Instagram posts go out tomorrow without approval."
- "3 LinkedIn drafts have no date — placing them on the two empty days would close the week."

Keep recommendations specific to the data. Don't generate generic best-practice advice.

---

## Step 5: Optional follow-up

- "Want me to fill the gaps? `marketing-post-batch` will draft the uncovered-demand topics with their figures attached."
- "Should I check another client or a different date range?"

---

## Edge cases

- **Many workspaces**: If the user asks to audit "all clients", loop through workspaces and produce one summary section per workspace. Warn if there are more than 5 — it may take a moment. The demand overlay needs a domain per client; skip the overlay (and say so) for clients whose domain you don't have.
- **Empty calendar**: If nothing is scheduled, say so directly, then lead with the demand table — it becomes the proposed plan instead of an overlay.
- **No core topics known**: Derive candidates from the brand's recent posts (recurring themes) and confirm with the user before pulling demand.
- **Recurring / synced posts**: Grouped posts show as one unit — note this if multiple platforms are covered by a single grouped post.
- **Time zones**: Scheduled times are returned in UTC. If the user's timezone is known, convert. Otherwise, note that times shown are UTC.
