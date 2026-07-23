# Changelog

## 1.0.0

Initial release of the combined SE Ranking + Planable **Marketing Skills** plugin — the cross-channel layer that sits between (and complements) [seranking/seo-skills](https://github.com/seranking/seo-skills) and [Planable/smm-skills](https://github.com/Planable/smm-skills).

### The idea

Every skill does something the single-channel plugins can't: it combines both data sources (SE Ranking + Planable), produces a routed decision instead of a report, or closes the loop with measurement. The plugin is organized as a pipeline — **Diagnose → Act → Measure** — tied together by the **campaign contract**: every launched campaign gets a Planable label (`mkt:{slug}`) and a campaign passport recording its baseline, so `marketing-campaign-impact` can later prove whether it worked.

Every skill carries the `marketing-` prefix, so the plugin coexists with seo-skills and smm-skills without duplicate names, and each description states when to use the single-channel sibling instead.

### 23 skills

**Diagnose (8)** — marketing-gap-matrix, marketing-ai-visibility-benchmark, marketing-calendar-audit, marketing-content-patterns, marketing-approvals-triage, marketing-ads-counterplay, marketing-serp-fit, marketing-local-audit

**Act (9)** — marketing-seo-campaign, marketing-ai-campaign, marketing-content-brief, marketing-post-batch, marketing-comparison-page, marketing-agency-leadgen, marketing-content-refresh, marketing-local-campaign, marketing-roadmap, marketing-onboarding-proposal

**Measure (5)** — marketing-campaign-impact, marketing-report, marketing-month-close, marketing-client-scorecard, marketing-weekly-ops

Auto-registers both remote MCP servers (SE Ranking + Planable) on install.

### Signature capabilities (what makes each skill cross-channel)

- **marketing-gap-matrix** — competitor keyword gaps checked against site coverage AND social history, every gap routed site / social / both / skip.
- **marketing-ai-visibility-benchmark** — AI share-of-voice vs competitors with a "do we even post about this" overlay per losing cluster.
- **marketing-content-brief** — writer-ready SEO brief that ships with its own distribution batch in Planable.
- **marketing-post-batch** — demand-validated batch: every topic checked against real search/question demand before writing.
- **marketing-seo-campaign / marketing-ai-campaign** — search & AI gaps turned into labeled, passport-backed campaigns.
- **marketing-campaign-impact** — discovers `mkt:*` campaigns, reads their passports, and delivers per-campaign verdicts: working / not yet / rework.
- **marketing-report** — one interactive cross-channel report (rankings + AI + social) with a per-campaign section, built from a bundled template.
- **marketing-weekly-ops** — the Monday ritual: approvals triage + calendar×demand + campaign status in one prioritized briefing; built for scheduled tasks.
- **marketing-roadmap** — quarterly plan where every phase carries three tracks (SEO / AI / social) with measurement gates.
- …and 14 more across the three stages (see README for the full table).

### Validation

Core skills were benchmarked with live-data evals (SE Ranking + Planable) — with-skill runs passed 40/40 assertions; `marketing-campaign-impact` correctly returned "not yet" on a fresh campaign instead of inventing movement. `marketing-report` ships with the `assets/report-template.html` its predecessor referenced but never included.
