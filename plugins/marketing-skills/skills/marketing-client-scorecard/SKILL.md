---
name: marketing-client-scorecard
description: >
  Score every client across three channels in one view — social performance (Planable
  workspaces) PLUS each client's search traffic trend and AI-search presence (SE Ranking, via a
  client→domain map) — with mismatch flags like "SEO up, social idle" or "great engagement, zero
  AI presence" that tell an agency where attention pays. Use when the user wants to compare
  clients across channels: "how are all my clients doing", "cross-client scorecard", "agency
  overview across SEO and social", "which client is slipping where", "compare accounts across
  channels". Requires both MCPs. For a social-only multi-workspace comparison, use
  cross-client-metrics-overview from the smm-skills plugin when installed; for one client's deep report, use
  marketing-report.
---

# Cross-client marketing scorecard

Compare client performance across social, search, and AI visibility in one view.
Built for agency operators who need to see the full picture — and the cross-channel mismatches — without opening each workspace and each SEO tool individually.

## How this skill works

1. Identify the workspaces, the client→domain map, and the time period
2. Fetch page and post metrics for each workspace
3. **Add each client's search + AI columns (signature step)**
4. Produce a ranked, comparative scorecard with mismatch flags

---

## Step 1: Gather inputs

- **Workspaces to include** — specific clients, or all workspaces (default: all)
- **Client → domain map** — which website belongs to each workspace (ask once; remember clients without a domain get social columns only, marked "no domain")
- **Date range** — default to previous calendar month if not specified
- **Primary metric to rank by** — engagement rate (default), impressions, posts published, or audience growth

If the user asks for "all clients", proceed — but if there are more than 15 workspaces, confirm before fetching them all (the SE Ranking columns add 2 calls per domain).

---

## Step 2: Fetch data for each workspace

For each workspace:

```
list_pages(workspaceId) → get all pageIds

get_page_metrics_summary(
  workspaceId,
  pageIds[],
  startDate,
  endDate
)

get_post_metrics_summary(
  workspaceId,
  pageIds[],
  startDate,
  endDate
)
```

Store results keyed by workspace name. Handle failures gracefully — if a workspace returns no data, note it rather than stopping the whole report.

---

## Step 3: Search & AI columns (signature step)

For each client with a mapped domain (sequential SE Ranking calls, ~10 req/s):

- `DATA_getDomainOverviewWorldwide` — organic traffic estimate + direction vs the previous period where history allows.
- `DATA_getAiSearchOverview` — AI presence (brand/link presence; `previous: null` = baseline, don't render it as growth).

Two calls per client, kept deliberately shallow — this is a scorecard, not an audit. Never invent a number: domainless clients or null responses show "—".

---

## Step 4: Build the scorecard

### At a glance — ranked table

Rank workspaces by the primary metric. Show as a clean table:

```
Agency scorecard — [Month Year] · ranked by engagement rate

| # | Client | Eng. rate | Impressions | Posts | Organic traffic | AI presence | Flags |
|---|---|---|---|---|---|---|---|
| 1 | Client A | 4.2% | 48K | 22 | 31K ↑ | 210 mentions | — |
| 2 | Client B | 3.8% | 31K | 18 | 89K ↑ | 12 mentions | ⚠️ SEO↑ social thin |
| 3 | Client C | 2.1% | 62K | 30 | 8K ↓ | — (no domain) | 💤 search slipping |
```

Include totals row at the bottom: total impressions, total engagement, total posts across all clients.

### Highlights and mismatch flags

After the table, 3–5 specific callouts. The cross-channel mismatches are the point of this scorecard:

**🏆 Top performer** — [Client] led on [metric] with [number].

**⚠️ SEO up, social idle** — [Client]'s organic traffic grew [X%] while only [n] posts shipped — search wins going unamplified; `marketing-seo-campaign` is the natural next step.

**🔍 Social strong, AI invisible** — [Client] holds [X]% engagement but near-zero AI presence — narratives aren't reaching AI answers; consider `marketing-ai-visibility-benchmark`.

**📉 Slipping on both** — [Client] down on search and social — escalate before the renewal call.

**💤 Low activity** — [Client] had fewer than 3 posts scheduled this month.

Keep callouts specific and data-backed. Don't include a callout category if there's no data to support it.

### Per-client detail (optional)

If the user wants per-client breakdowns, hand off to `marketing-report` (full cross-channel report per client) or `marketing-month-close` (report + next-month plan). Only inline short blocks if explicitly requested, to keep the default output scannable.

---

## Step 5: Tone and formatting

- Write for an operations manager or agency lead reviewing accounts
- Be direct about which clients are underperforming — don't soften it with filler
- Numbers: round to nearest whole number or one decimal. Use K for thousands (48K, not 48,000)
- Don't repeat numbers across sections — show each metric once in the right place
- If data is missing for a workspace, include it in the table with "No data" rather than omitting it

---

## Edge cases

- **Unsupported platforms**: Google My Business and Threads don't return metrics via the connector. Note these per workspace in a "Data not available" footnote.
- **Large workspace count (15+)**: Warn the user that fetching all workspaces may take a moment. Proceed unless they want to narrow scope.
- **Single workspace accidentally**: If only one workspace is found or specified, suggest `marketing-report` (cross-channel) or `marketing-month-close` (report + next-month plan) instead — they're better suited for single-client depth.
- **Mixed plan types**: Some Planable plans may have limited analytics access. If a workspace returns empty metrics, note that analytics may not be available on that workspace's plan and suggest checking Planable directly.
- **Comparing incomparable periods**: If workspaces have different amounts of data (e.g., one client was just onboarded), flag this in the notes section.
