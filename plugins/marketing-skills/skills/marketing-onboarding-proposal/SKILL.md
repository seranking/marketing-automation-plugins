---
name: marketing-onboarding-proposal
description: >
  Build a data-backed client onboarding proposal for an agency starting a new engagement —
  an SE Ranking SEO + AI-search audit paired with the client's current Planable social baseline —
  and, with the user's go-ahead, instrument that baseline on day one (label, campaign passport,
  optionally tracked terms) so every future report proves progress against signed-off numbers.
  Use whenever an agency wants to show a new or about-to-sign client where they stand and what
  the plan is: "build an onboarding proposal for [client]", "put together the SEO + social
  analysis", "client proposal with audit and baseline", "scope a SEO + AI + social engagement".
  Assumes the client's social accounts are already connected in Planable. Requires both MCPs.
  Distinct from the copy in the channel plugins: this version leaves a measurable baseline
  behind, not just a document.
---

# Client onboarding proposal

Produce the document an agency brings to the start of a client relationship: an honest read of where the client stands across SEO, AI search, and social — and a clear, scoped plan for what the agency will do. It pairs SE Ranking analysis (organic + AI visibility, competitive gaps, site health) with Planable's view of the client's current social performance.

> **Positioning.** This is an **onboarding / early-engagement proposal**, not a cold pitch. It assumes the client's social pages are already connected in the Planable workspace so their real performance can be analysed. (Without that access, the social section can't be produced from data — only the SE Ranking side.)

## Prerequisites

- **SE Ranking MCP** connected (Data API for research + audit; a project is optional, and an existing finished audit can be reused via `PROJECT_listAudits` instead of crawling).
- **Planable MCP** connected, with the **client's workspace and connected social pages** available.
- The user provides: client domain + brand, market/country (default `us`), 2–4 competitors, and the Planable workspace. Optionally the engagement goals (e.g. lead gen, brand, local).

## Connector health check

Before doing anything else, verify both MCPs are reachable:

- **SE Ranking:** call `DATA_getSubscription`. If it fails or returns an auth error, stop immediately and tell the user:
  > "The SE Ranking connector isn't responding — please reconnect it before we continue. Setup guide: https://seranking.com/api/integrations/mcp/"
- **Planable:** call `list_workspaces`. If it fails or returns an auth error, stop immediately and tell the user:
  > "The Planable connector isn't responding — please reconnect it before we continue. Setup guide: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools"

Only continue to the process steps below once both calls return a successful response.

## Process

### 1. Scope the proposal
Confirm the client domain, competitors, the Planable workspace, and what the engagement is meant to achieve. Verify the workspace actually has the client's pages connected (`list_pages`) and check `get_workspace_details` for `hasAnalytics` — this gates the social section.

### 2. SEO + AI-search analysis (SE Ranking)
- **Organic footprint:** `DATA_getDomainOverviewWorldwide` + `DATA_getDomainKeywords` — traffic, top terms, trajectory. Highlight **striking-distance** terms (high volume, position 11–30) — they're the fastest wins.
- **Competitive gaps:** `DATA_getDomainCompetitors` + `DATA_getDomainKeywordsComparison` — what competitors capture that the client doesn't (discard junk/off-brand terms).
- **Site health:** reuse an existing audit (`PROJECT_listAudits` → `getAuditReport`) or run `DATA_createStandardAudit` → `DATA_getAuditReport` — health score and the issues holding pages back.
- **AI visibility:** `DATA_getAiSearchBrand` to resolve the brand, then `DATA_getAiSearchOverview` and `DATA_getAiSearchLeaderboard` vs competitors (query the leaderboard narrowly — see the ai-search skill's timeout note).
- **Demand for content:** `DATA_getKeywordQuestions` + `DATA_getRelatedKeywords` — proof there's an audience to win.
- Separate findings into **quick wins** (low effort, near-term) and **strategic plays** (higher effort, compounding).

### 3. Social baseline (Planable)
- `list_pages(workspaceId)` — the connected channels.
- `get_page_metrics_summary(workspaceId, pageIds, startDate, endDate)` — **account-level** reach: audience (followers), total page impressions, and engagement rate. This is the size-of-presence number.
- `get_post_metrics_summary(workspaceId, pageIds, startDate, endDate)` — **published-content** performance in the window (impressions/engagement *of the posts*). These two impression figures are different scopes (page reach vs post reach) — report them separately and don't add or conflate them.
- `get_post_metrics(workspaceId, pageIds, ..., limit: 5)` — current top posts (what already resonates).
- `list_posts(workspaceId, ...)` — current cadence and how much is actually scheduled vs idle.
- Read it as a baseline: cadence, engagement rate, platform mix, and the obvious gaps.

### 4. Synthesise the proposal
Structure it as: **Where you are** (SEO health + AI visibility + social baseline, with real numbers) → **The opportunity** (competitor gaps, AI gaps, under-used channels, content demand) → **Our plan** (scope across SEO, AI-search optimisation, and social-media management run in Planable) → **First 90 days** (a concrete, sequenced starter plan) → **How we'll measure it** (the metrics to revisit). Keep claims tied to the data pulled — this is what makes an onboarding proposal land.

### 5. Instrument the baseline (signature step — with the user's go-ahead)
The proposal's numbers become worthless the day nobody can re-check them. With confirmation:

- **Label:** create `mkt:onboarding-<client-slug>` in the client's workspace — future campaign skills attach to it and reports can isolate the engagement's output from day one.
- **Campaign passport:** one post on a Universal content page titled `Campaign passport: mkt:onboarding-<client>` (file fallback if none) holding the baseline verbatim: organic traffic/keywords, striking-distance terms, AI presence/SoV vs competitors, social cadence/engagement, audit health score, snapshot date. This is the "where you were when we started" record `marketing-campaign-impact` and `marketing-report` measure against.
- **Optional deeper tracking** (consumes plan limits — confirm explicitly): reuse/create an SE Ranking project, `PROJECT_addKeywords` for the agreed target terms, `PROJECT_addCompetitor` for the named competitors.

If the user declines, produce the document anyway — but note in the measurement-plan section that the baseline exists only inside the proposal text.

### 6. Produce the document
Default to a clean `proposal.md` in the outputs folder. If the user wants a presentation-ready deliverable, build it with the `docx` skill (formal document) or `pptx` skill (pitch deck) — read that skill, then format the synthesised content into it.

## Output

A client-ready proposal containing:
1. **Executive summary** — the three or four findings that matter most.
2. **Current state** — SEO health, AI-search visibility, and social baseline, with numbers.
3. **Opportunities** — competitor/keyword/AI gaps and social headroom.
4. **Proposed scope** — SEO + AI + SMM, with how Planable runs the social workflow (planning, approvals, publishing).
5. **First 90 days** and **measurement plan** — including the instrumented baseline (label + passport + tracked terms) and the cadence: `marketing-report` monthly, `marketing-campaign-impact` per campaign, first re-check date named.

## Tips

- Be honest about weak spots — a credible baseline (including unflattering numbers) builds more trust than an inflated one, and it makes future wins measurable.
- Tie every recommendation to a finding. "Competitor X ranks for 240 commercial terms you don't" beats "improve your SEO".
- Don't fabricate metrics. If SE Ranking or Planable returns nulls/empties, mark them unknown rather than guessing.

## Edge cases & limits

- **Social section needs connected pages.** If the client's pages aren't in the Planable workspace yet, produce the SE Ranking side and clearly mark the social baseline as pending access (frame it as an onboarding step).
- **Limited page-level analytics:** several platforms don't return page-level analytics via the connector — commonly **Twitter/X, Pinterest, Threads, and Google Business Profile**. They appear under `unsupportedPages`; list them as "data not available" rather than zero, and base totals on the supported channels.
- **Audit needs crawl budget** from the plan; reuse a recent audit when one exists, and scope `max_pages` for large sites.
- This skill analyses, proposes, and instruments the baseline; it does not create or publish content (use `marketing-seo-campaign` or `marketing-content-refresh` once the engagement starts — they attach to the same measurement spine).
