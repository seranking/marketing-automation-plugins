---
name: marketing-campaign-impact
description: >
  Measure whether a marketing campaign actually moved the needle. Discovers campaigns created by
  the marketing-skills campaign skills (Planable label convention `mkt:{slug}` plus a campaign
  passport with baseline numbers), re-pulls current AI-search visibility, rankings, and social
  engagement, and delivers a per-campaign verdict — working / not yet / rework — with before/after
  numbers side by side. Use whenever the user asks "did the campaign work", "what happened since we
  launched those posts", "is our AI visibility up since the campaign", "measure campaign impact",
  "before/after for [campaign]", "which of our campaigns are working", or 2+ weeks after any
  marketing-seo-campaign / marketing-ai-campaign / marketing-local-campaign run. Requires both the
  SE Ranking and Planable MCPs. Distinct from marketing-report (a period report across all
  channels): use this skill when the question is about a specific campaign's effect, not the
  month's totals.
---

# Campaign impact

Answer the question every campaign eventually faces: **did it work?** This skill closes the loop the campaign skills open — it finds the campaign's label and passport, re-pulls the same metrics that were recorded as the baseline, and returns an honest verdict with the evidence.

## Prerequisites

- **SE Ranking MCP** connected.
- **Planable MCP** connected, with the workspace where the campaign lives.
- A campaign created by a marketing-skills campaign skill (label `mkt:<slug>`; ideally a passport). Campaigns without a passport degrade gracefully — see Edge cases.

## Connector health check

Verify both MCPs before starting: `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to reconnect it (SE Ranking guide: https://seranking.com/api/integrations/mcp/ · Planable guide: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools).

## Process

### 1. Discover the campaigns
- Confirm the workspace (or scan the ones the user names). `list_labels(workspaceId)` → every label matching `mkt:*`.
- If the user asked about a specific campaign, match its slug; otherwise list what was found and let them pick (or run all).

### 2. Read each campaign's passport
- Look for the passport post: `list_pages(workspaceId)` → pages with `type: "universal"` → `list_posts` on them → the post titled `Campaign passport: mkt:<slug>`.
- Parse from it: goal, launch date, source clusters/keywords/prompts, baseline numbers (positions, AI presence/SoV, engagement), tracked terms/prompts, tracker engine id if one was created.
- No passport → note it, and fall back to label-only measurement (Edge cases).

### 3. Pull the campaign's social performance (Planable)
- `list_posts(workspaceId, ...)` filtered by the campaign label → the campaign's posts and their publish states (how many actually went live vs stayed drafts — a campaign that never shipped can't have moved anything; say so).
- `get_post_metrics_summary(workspaceId, pageIds, startDate, endDate)` for the window since launch, and `get_post_metrics` for the campaign's individual posts where available — impressions, engagement, engagement rate.

### 4. Re-pull the search/AI side (SE Ranking)
Mirror whatever the baseline recorded — compare like with like:
- **Tracked terms:** with a project — `PROJECT_getPositionHistory(site_id, date_from=launch, date_to=today)` for the campaign's terms; without — `DATA_getDomainKeywords` filtered to those terms and compare current position vs baseline.
- **AI visibility campaigns:** `DATA_getAiSearchOverview(target, source, brand?)` — brand presence / link presence now vs passport baseline; `DATA_getAiSearchLeaderboard` narrowly (few engines/competitors per call — it can 504) for SoV movement; `DATA_getAiSearchPromptsByBrand` / `ByTarget` to check whether the brand now appears for the campaign's target prompts (count entered vs still missing).
- **Tracker engine recorded in the passport:** `PROJECT_getPromptsRankings` + `PROJECT_getLlmStatistics` give the cleanest prompt-level movement.
- Sequential calls, ~10 req/s; never invent a number — a metric that can't be re-pulled is reported as "not measurable now", not estimated.

### 5. Verdict — per campaign
Weigh the evidence against the time elapsed (AI visibility and rankings move in weeks/months; social engagement in days):

| Verdict | When |
|---|---|
| **working** | target metrics moved in the right direction beyond noise (e.g. entered ≥1 target prompt cluster, tracked terms up meaningfully, engagement at/above account norm) |
| **not yet** | posts shipped recently (<2–3 weeks for AI/rank metrics) or movement is within noise — recommend the next check date instead of forcing a call |
| **rework** | enough time passed, posts shipped and earned reach, but target metrics are flat/negative — say which lever to change (angle, format, citable web content, consistency) |

The verdict must cite numbers: baseline → current for each tracked metric. If `previous`/baseline is missing for a metric, present current as a new baseline — never as growth.

### 6. Recommend the next move
- **working** → double down: extend the winning clusters (`marketing-seo-campaign` / `marketing-ai-campaign` round 2), and update the passport with the new baseline.
- **not yet** → set the re-check date; optionally add tracking that was skipped at launch.
- **rework** → name the weakest link with evidence (e.g. "posts earned reach but the brand still isn't cited — the gap is citable web content, not social volume") and route: content refresh (`marketing-content-refresh`), different angles, or channel mix.

## Output

```markdown
# Campaign impact: mkt:{slug}
Launched {date} · measured {date} ({n} weeks in) · Verdict: **{working | not yet | rework}**

## Before → after
| Metric | Baseline (passport) | Now | Move |
|---|---|---|---|
| Brand presence (ChatGPT) | 4 prompts | 7 prompts | +3 ✅ |
| Avg position "{term}" | — (not ranking) | #28 | entered ✅ |
| Campaign posts engagement | — | 1,840 (ER 4.1%) | above account norm |

## What shipped
{n} of {m} drafts published · {platforms} · reach/engagement summary

## Read
{2–4 sentences: what the numbers say, honestly — including what's noise}

## Next move
{the one recommendation, with the skill to run}
```

Multiple campaigns → one section per campaign plus a one-line scoreboard at the top (slug · age · verdict).

## Tips

- Time honesty beats optimism: an AI-visibility verdict at week 1 is noise — say "not yet" and give the date.
- Drafts that never shipped are the most common "campaign failure" — check publish states before blaming the content.
- Social engagement is the fastest signal and the least connected to the goal; treat it as supporting evidence, not the verdict.
- When a campaign is judged "working", refresh the passport baseline so the next measurement window starts clean.

## Edge cases & limits

- **No passport:** measure what the label allows (social metrics since first post's date; current search/AI snapshot as a *new* baseline) and state plainly that before/after on search/AI isn't possible without a recorded baseline.
- **No `mkt:*` labels at all:** say no instrumented campaigns were found and point to the campaign skills that create them; offer to baseline an existing ad-hoc batch retroactively (label it + write a passport now).
- **Attribution:** this measures correlation against the campaign's targets, not causal attribution — rankings and AI citations are also moved by site content and authority. Say so in the Read when relevant.
- **Leaderboard timeouts:** query narrowly and retry once; fall back to per-domain overviews.
