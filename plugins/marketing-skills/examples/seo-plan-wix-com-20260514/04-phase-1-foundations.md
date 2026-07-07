# Phase 1 — Foundations (weeks 1–4)

> Owner copy. Source of truth for standups. PLAN.md inlines the Phase summary but this file is the working artefact owners share.

## Goal

Measure the unmeasured pillars. Capture a fresh /seo-drift baseline. Populate Technical / Content / Backlink pillar scores so Phase 2 prioritisation can be defended quantitatively.

## Work items

### 1.1 — Run /seo-technical-audit wix.com
- Owner: SEO Lead
- Effort: M (1–2 days for the audit + 0.5 day triaging)
- Skill: seo-technical-audit
- Cost: varies by page count; ~6 Firecrawl credits for the modern-signals step (per SKILL.md)
- Output: examples/seo-technical-audit-wix-com-<date>/TECH-AUDIT.md (this folder will be created by the specialist run)
- Phase-end metric: Technical pillar score populated. Severity-weighted issue count established.

### 1.2 — Run /seo-content-audit on top 10 traffic pages
- Owner: Content Lead
- Effort: L (audit takes ~1 day per page, 10 days total — parallelise where possible)
- Skill: seo-content-audit
- Cost: ~10–15 SE Ranking credits + ~10 Firecrawl credits (per SKILL.md)
- Dependency: needs top-10 pages list — pulled from DATA_getDomainPages (cap=10, sorted by traffic)
- Output: examples/seo-content-audit-wix-com-<date>/VERDICT-ROLLUP.md + per-page evidence
- Phase-end metric: Content pillar score = average E-E-A-T+CITE across the 10 audits.

### 1.3 — Run /seo-backlinks-profile wix.com
- Owner: SEO Lead
- Effort: M
- Skill: seo-backlinks-profile
- Cost: ~25–40 SE Ranking credits (per SKILL.md)
- Output: examples/seo-backlinks-profile-wix-com-<date>/PROFILE.md
- Phase-end metric: Backlink pillar score; disavow-candidate list reviewed (not auto-disavowed — review only).

### 1.4 — Run /seo-drift baseline wix.com
- Owner: SEO Lead
- Effort: S (one command run)
- Skill: seo-drift (baseline subcommand)
- Cost: ~10–15 SE Ranking credits + 1 Firecrawl credit (per SKILL.md)
- Prerequisites: SE Ranking auth + Google APIs configured (bash extensions/google/install.sh)
- Output: examples/seo-drift-wix-com-<date>/snapshot.json + BASELINE.md
- Phase-end metric: snapshot.json saved; baselines.json entry created. This is the anchor for Phase 3.3.

### 1.5 — Fix the og:image / twitter:image malformation if confirmed on other Wix subdomains
- Owner: Engineering
- Effort: S
- Source: the sibling Notion seo-page example surfaced a malformed og:image (https://www.notion.com + https://img.youtube.com concatenation). Confirm Wix-side: check wix.com homepage og:image on Firecrawl scrape. Today's capture shows the og:image at static.wixstatic.com — properly formed. But Phase 1.5 is a precaution because the bug pattern is common in SPA frameworks; re-confirm on Wix-blog and Wix-help subdomains.
- Output: og:image URL audit across top 5 Wix subdomains.
- Phase-end metric: 0 malformed og:image URLs on Tier-1 Wix subdomains.

### 1.6 — Run /seo-page on the top 3 traffic pages
- Owner: Content Lead
- Effort: M
- Skill: seo-page
- Cost: ~10–15 SE Ranking credits per URL × 3 = ~30–45 credits
- Dependency: top-3 pages list from Phase 1.2
- Output: examples/seo-page-wix-com-<url>-<date>/PAGE.md × 3
- Phase-end metric: KEEP/REFRESH/CONSOLIDATE/KILL verdicts on top 3 pages. Quick-win refresh items captured.

## Phase exit criteria

- All four pillar scores have a real number (Technical, Content, Topical, AI Search; backlink score is a sub-component).
- Drift baseline captured at week 4.
- Top-3 page verdicts produced.
- og:image audit complete; any malformations queued for engineering fix.

## Total estimated cost

SE Ranking: ~80–125 credits (audit + content audit + backlinks + drift baseline + 3× seo-page).
Firecrawl: ~15–20 credits (audit modern signals + 3× seo-page Firecrawl + drift baseline page snapshot).

This fits within most monthly SE Ranking quotas comfortably. The bottleneck is human time on Phase 1.2 (10 page audits at ~1 day each).
