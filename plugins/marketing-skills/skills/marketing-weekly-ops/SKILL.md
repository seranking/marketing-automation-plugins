---
name: marketing-weekly-ops
description: >
  Run the weekly cross-channel marketing ops ritual in one pass: the approval queue triaged by
  stake, this week's calendar checked against live search/AI demand, the status of every active
  `mkt:*` campaign (what's stuck, what's live, what's moving), and a fill-the-gaps proposal —
  ending in one prioritized briefing with concrete actions. Use when the user says "weekly
  marketing check", "Monday briefing", "what needs attention this week across marketing", "run
  my marketing ops", "weekly review of content and campaigns", or wants a recurring weekly
  overview spanning approvals, calendar, campaigns, and demand. Works great as a scheduled task.
  Requires both MCPs. Distinct from marketing-approvals-triage (approvals only) and
  marketing-calendar-audit (calendar only): this is the whole weekly picture in one briefing.
---

# Weekly marketing ops

One run, one briefing, every Monday question answered: *what's stuck, what's going out, does it match demand, are the campaigns alive, and what do we do about it — in what order.*

This skill chains the checks the atomic skills do individually, keeps each one shallow, and spends its depth on the synthesis: a prioritized action list.

## Prerequisites

- **SE Ranking MCP** connected (demand + campaign term checks).
- **Planable MCP** connected, with the workspace(s) to cover.
- User provides: the workspace (or "all my clients"), the brand domain + 3–5 core topics (for the demand layer; reuse from previous runs when known), and optionally the week window (default: today through +7 days).

## Connector health check

Verify both MCPs before starting: `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to reconnect it (SE Ranking guide: https://seranking.com/api/integrations/mcp/ · Planable guide: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools).

## Process

Keep every section shallow — this is a briefing, not four audits. Sequential SE Ranking calls (~10 req/s); reuse data across sections instead of re-pulling.

### 1. Approvals — triaged, not listed
- `list_posts(workspaceId, approvalStatus: ["PENDING_MY_APPROVAL"])` (or `NOT_APPROVED` + `PARTIAL_APPROVED` for the broader view).
- Rank exactly like `marketing-approvals-triage`: **P1** carries an active `mkt:*` label or is scheduled <48h · **P2** scheduled this week / part of a partially-live batch · **P3+** the rest, with age flags (>7 days pending).
- Only P1/P2 make the briefing; the rest is one count line ("+6 low-priority pending").

### 2. Calendar × demand — this week only
- `list_posts(workspaceId, postStatus: ["SCHEDULED"], scheduledAfter/Before: week window)` + `postStatus: ["WITH_ERRORS"]` + `["NO_DATE_SET"]` (count only for undated).
- Empty days and error posts get named. Then the demand pass: `DATA_getKeywordQuestions` on the core topics (1 call per topic, aggregate) — is anything people are actively asking uncovered this week?
- Output: the week grid in one compact block + up to 3 "uncovered demand" lines with figures.

### 3. Campaign status — every `mkt:*` label
- `list_labels(workspaceId)` → labels matching `mkt:*`. For each: `list_posts` filtered by the label → publish states (drafts stuck vs live), and read the campaign passport on the Universal page when one exists.
- For campaigns with tracked terms in the passport and ≥2 weeks of age: one shallow check (`DATA_getDomainKeywords` filtered to those terms, or `PROJECT_getPositionHistory` when a project id is recorded) — direction only, not a full measurement.
- Verdict per campaign, one line each: **launching** (drafts pending — point at the P1 approvals) / **live** (posts out, too early to judge) / **moving** (tracked terms drifting up) / **stalled** (live 4+ weeks, nothing moving → queue `marketing-campaign-impact` for the real verdict).

### 4. The briefing — one prioritized action list
Synthesise into a single block, ordered by impact. Every action names its skill:

```
# Weekly marketing ops — {workspace} — week of {date}

## Do first
1. Approve 2 posts blocking mkt:ai-vis-2026q3 (P1 — campaign 2/4 live)     → Planable
2. Wednesday + Friday empty while "content approval" demand is live (1.9k/mo, 6 questions)
                                                                            → marketing-post-batch
3. mkt:gaps-acme live 5 weeks, tracked terms flat → run the measurement    → marketing-campaign-impact

## This week's calendar
Mon ✅2 · Tue ✅1 · Wed ⚠️empty · Thu ✅1 ⏳1 · Fri ⚠️empty · weekend —
Errors: 1 (Instagram media missing — fix before Thu)

## Campaigns
mkt:ai-vis-2026q3 — launching (2 approvals pending) · mkt:gaps-acme — stalled (5w, flat) · mkt:local-austin — moving (↑ avg pos)

## Counts
Approvals: 2 P1 · 3 P2 · +6 low-priority | Undated drafts: 4 | Uncovered demand topics: 2
```

### 5. Offer the ritual
If this was run manually, offer to schedule it: *"Want this every Monday morning? I can set it up as a scheduled task."* Multi-workspace agencies: one briefing per client, then a 3-line agency rollup on top.

## Tips

- Shallow beats thorough here — the atomic skills exist for depth. If any section demands real analysis, name the skill and move on; don't inflate the briefing.
- The briefing's order is the deliverable. "Everything is fine" weeks should say so in three lines — padding a quiet week erodes trust in the loud ones.
- Reuse the previous run's core topics and domain; ask only when they're unknown.
- Never write anything in this skill — it reads, ranks, and recommends. Creation goes through the skills it names.

## Edge cases & limits

- **No `mkt:*` labels:** skip section 3 with one line — and suggest the campaign skills that create measurable campaigns.
- **Many workspaces (>5):** confirm scope first; a full agency sweep is slower and usually wanted only for the rollup.
- **No domain/core topics:** run sections 1 and 3, mark the demand layer unavailable, and ask for the domain for next time.
- **Scheduled-task mode:** no questions available — use defaults, note assumptions at the bottom of the briefing.
