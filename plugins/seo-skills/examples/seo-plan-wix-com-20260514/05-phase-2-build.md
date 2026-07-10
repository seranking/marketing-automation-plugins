# Phase 2 — Build (weeks 5–8)

> Owner copy. Source of truth for standups.

## Goal

Close the single highest-leverage competitive gap surfaced by the sibling gap-analysis example: Wix has no defensive /vs/webflow page on its own domain, while Webflow's webflow.com/vs/wix ranks #3 organically on Wix's own brand-comparison query.

Secondary goal: ship Wix Studio /agencies landing page to compete with webflow.com/solutions/agencies (Webflow at position 1 on "website builder for agencies").

## Work items

### 2.1 — Run /seo-competitor-pages wix-vs-webflow
- Owner: Content Lead
- Effort: M (1 day skill run + 1 day review)
- Skill: seo-competitor-pages
- Output: examples/seo-competitor-pages-wix-vs-webflow-<date>/PAGE.md with feature matrix, structured comparison, draft copy, recommended schema.
- Phase-end metric: comparison page draft + feature matrix produced.

### 2.2 — Ship wix.com/vs/webflow AND wix.com/vs/squarespace
- Owner: Content + Engineering (joint)
- Effort: M (1 sprint for both pages)
- Source: 2.1's draft + a sibling /vs/squarespace run.
- Output: two live indexable pages with structured-data validation.
- Phase-end metric: both pages live and indexable. Verify via search console URL Inspection.

### 2.3 — Run /seo-content-brief on "website builder for agencies"
- Owner: Content Lead
- Effort: M
- Skill: seo-content-brief
- Source: gap-analysis sibling example surfaced this as the #1 commercial-intent gap.
- Output: examples/seo-content-brief-wix-studio-agencies-<date>/BRIEF.md.
- Phase-end metric: writer-ready brief delivered to whoever owns wix.com/studio content.

### 2.4 — Ship Wix Studio /agencies landing page
- Owner: Content + Engineering
- Effort: M (1 sprint)
- Source: 2.3's brief.
- Output: live indexable wix.com/studio/agencies (or equivalent path).
- Phase-end metric: page ranks somewhere in top 50 for "website builder for agencies" within 30 days of publishing. Track via Search Console + position monitoring.

### 2.5 — Run /seo-keyword-cluster on the qualitative gap themes
- Owner: Content Lead
- Effort: M
- Skill: seo-keyword-cluster
- Input: the three qualitative gap clusters from examples/seo-competitor-gap-analysis-wix-com-20260514/REPORT.md (Agency / Comparison / Pricing-longtail).
- Output: pillar + spokes architecture covering each theme.
- Phase-end metric: cluster plan covers the agency segment end-to-end (pillar + 4–7 spokes).

### 2.6 — Run /seo-schema on the new /vs/* and /agencies pages
- Owner: Engineering
- Effort: S
- Skill: seo-schema
- Input: the live URLs from 2.2 and 2.4.
- Output: JSON-LD blocks for each page (likely Product/Organization for /vs/*; WebPage + FAQPage for /agencies).
- Phase-end metric: rich-result eligibility validated via Google's Rich Results Test for all three pages.

## Phase exit criteria

- wix.com/vs/webflow, wix.com/vs/squarespace, wix.com/studio/agencies all live and indexable.
- Structured-data schema validated on all three.
- Cluster plan for the agency segment delivered to Content team.

## Total estimated cost

SE Ranking: ~30–50 credits across the specialist runs.
Firecrawl: ~10 credits.
Engineering: 2–3 sprints (parallelisable: pages, schema, cluster work).
Content: 3–4 weeks of writer + editor time.

## Open dependencies on Phase 1

- 2.1 quality depends on Phase 1.1's technical audit identifying any indexation issues that would prevent /vs/* and /agencies from ranking. If technical audit finds noindex bugs (like the sibling Notion seo-page example surfaced), prioritise fixing those before launching new pages.
- 2.4 depends on Phase 1.2's content audit demonstrating the existing content team can execute at the E-E-A-T quality bar Phase 2 requires. If audits return verdicts <70, Phase 2.4 needs additional editorial support.
