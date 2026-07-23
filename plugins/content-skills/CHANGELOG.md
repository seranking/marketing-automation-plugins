# Changelog

All notable changes to the Content Skills plugin are documented here.

## [1.0.0] - 2026-07-20

Initial public release — ten content-lifecycle **pipelines** plus a shared brand layer, on live SE Ranking + Planable data.

### Added
- **content-brand-setup** — one-time brand profile (`brand-profile.md`) that every other skill reads automatically, so outputs are on-brand instead of generic.
- **content-campaign-brief** — writer-ready SEO brief + teaser social drafts in Planable + a keyword/AI-prompt tracking baseline.
- **content-editorial-plan** — keyword clusters mapped onto a 12-week publication calendar with cadence, brief queue, and monthly social themes.
- **content-niche-plan** — longtail/question mining with per-candidate social-demand validation (build / experiment / skip).
- **content-ideas-from-social** — top-performing social topics validated against SE Ranking search demand into a mini content plan.
- **content-refresh-pipeline** — batch E-E-A-T + CITE audit (1–10 URLs) → priority queue → per-page fixes → re-distribution drafts → re-check.
- **content-page-recovery** — weak-page fixes with Planable approval, a relaunch quality gate, internal-link refresh, and post-fix measurement.
- **content-ai-visibility** — AI share-of-voice snapshot + passage-level citability fixes for existing pages + social drafts + before/after re-check.
- **content-gap-sprint** — SEO gaps → social campaign + article briefs for the top gaps, sequenced into one sprint.
- **content-social-pack** — atomize one published article into a platform-adapted pack grouped as a Planable campaign linked to the source.
- Both MCP servers (SE Ranking, Planable) auto-registered via `.mcp.json`.

### Background
This plugin began as a straight bundle of existing skills from `seranking/seo-skills` and `Planable/smm-skills`. Before release, every skill was reworked so the plugin adds value alongside those base plugins rather than duplicating them:
- **Namespace** — all skills use the `content-*` prefix, so there are zero name collisions when the base plugins are also installed.
- **Outcome** — each skill runs an end-to-end pipeline (plan → create → distribute → measure) that does strictly more than its single-deliverable base counterpart.
- **Routing** — every skill description names its base twin and says when to use that instead.
- **Packaging** — MCP servers are declared once in `.mcp.json` (an early build declared them twice, which fails the marketplace sync).
