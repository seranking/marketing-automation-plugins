---
name: marketing-roadmap
description: >
  Build a phased cross-channel marketing roadmap for a domain — quarter-by-quarter, where every
  phase carries three tracks: SEO, AI visibility, and social. Grounded in the site's competitive
  position, content gaps and AI Search readiness (SE Ranking) AND the brand's actual social
  baseline (Planable cadence + engagement), with a measurement gate closing each phase. Use when
  the user asks for a "marketing plan", "marketing roadmap", "90-day plan across SEO and social",
  "quarterly marketing plan", "where do we focus next across channels", or wants search, AI and
  social work sequenced in one plan. Requires both the SE Ranking and Planable MCPs. For an
  SEO-only roadmap, use seo-plan from the seo-skills plugin instead.
---

# Marketing roadmap

Produce a phased cross-channel roadmap for a brand. Output is a single `PLAN.md` plus per-phase deliverable folders, each phase scoped to a quarter (or a 90-day sprint), each with explicit goals, work items across **three tracks — SEO, AI visibility, social** — owners, metrics, and a measurement gate. The plan is grounded in the brand's actual competitive position and its actual social output — not a generic checklist.

This is the "what should we work on next quarter" skill. It does not replace specialist skills — it composes them, then sequences their outputs.

## Prerequisites

- SE Ranking MCP server connected.
- **Planable MCP** connected, with the brand's workspace (social baseline + the social track's execution surface).
- User provides:
  - Target domain and the Planable workspace.
  - Optionally: target country (default `us`), business type (saas / ecommerce / local / publisher / agency / b2b — auto-detected from the domain if not supplied), planning horizon (default 90 days, options: 30 / 90 / 180 / 365).
  - Optionally: known constraints (engineering capacity, content budget, posting-cadence ceiling, etc.).

## Process

0. **Connector health check.** `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to reconnect it.

1. **Detect business type** `DATA_getDomainOverviewWorldwide`, plus a Firecrawl `scrape` of the homepage if available
   - Inspect title, H1, JSON-LD types, primary nav patterns.
   - Classify as one of: `saas`, `ecommerce`, `local`, `publisher`, `agency`, `b2b-services`, `marketplace`. If ambiguous, ask the user once.
   - Business type drives template selection (see step 6).

2. **Domain baseline** `DATA_getDomainOverviewWorldwide`, `DATA_getDomainOverviewHistory`, `DATA_getDomainAuthority`, `DATA_getBacklinksSummary`
   - Capture: organic keywords, organic traffic estimate, DA, backlink profile health, top countries, traffic trend over the last 12 months.
   - This sets the "where you are now" anchor.

2b. **Social baseline (signature input)** `list_pages`, `get_page_metrics_summary`, `get_post_metrics_summary`, `list_posts`
   - Capture for the last ~90 days: connected channels, posting cadence (posts/week), audience size, impressions, engagement rate, and whether any `mkt:*` campaigns already run.
   - Also `DATA_getAiSearchOverview` for the AI-visibility baseline (brand presence / link presence; `previous: null` = first snapshot, treat as baseline).
   - Without this, a "marketing plan" is an SEO plan wearing a bigger name — these numbers anchor the social and AI tracks.

3. **Competitive frame** `DATA_getDomainCompetitors`
   - Pull top 5–10 organic competitors.
   - For each: organic keywords, traffic share, DA, top topical clusters they own.
   - Identifies who the user is *actually* competing with on the SERP (often different from who they think).

4. **Pull specialist inputs (reuse before you re-research)**
   - In the current working directory, look for fresh (<30 days) outputs from the diagnostic skills and ingest their primary deliverables:
     - `marketing-gap-matrix-*` (routed gap backlog) · `marketing-ai-visibility-benchmark-*` (SoV + social-narrative overlay) · `marketing-content-patterns-*` (what resonates with the audience) · `marketing-local-audit-*` (for local businesses) · seo-skills outputs if present (`seo-technical-audit-*`, `seo-content-audit-*`, `seo-backlinks-profile-*`).
   - Anything missing does NOT block the plan: list it as a **Phase 0 / week-1 work item** ("run marketing-gap-matrix") rather than dispatching it silently — the user controls API spend.

5. **Score the five pillars**
   - **Technical health** (0–100): from technical-audit issue counts (or mark "unscored — Phase 0").
   - **Content quality** (0–100): average content-audit score across audited pages (or unscored).
   - **Topical authority** (0–100): cluster coverage relative to top 3 competitors (gap-matrix input).
   - **AI Search readiness** (0–100): citation share vs SoV competitors (ai-visibility-benchmark input).
   - **Social presence** (0–100): cadence + engagement vs demand — from the step 2b baseline (a workspace posting 0×/week against live demand scores near zero).
   - The lowest pillar becomes the **lead theme** for the first phase.

6. **Apply business-type template** (templates differ — pick one and parameterise)
   - **saas** → product-led pillars + integration pages + comparison/alternatives + JTBD content.
   - **ecommerce** → category page hygiene + product schema + faceted-nav indexation rules + review aggregation.
   - **local** → GBP optimisation + location pages + citation cleanup (note: we don't have a `seo-local` skill yet — flag this as a manual sub-step or external).
   - **publisher** → topical clusters + author E-E-A-T + freshness cadence + AI-Search citations.
   - **agency** → service pages + case studies + comparison content + lead-gen LP (use `seo-agency-landing-page`).
   - **b2b-services** → industry-specific landing pages + thought-leadership clusters + decision-stage content.
   - Template provides default work-item categories; the lowest-pillar score from step 5 weights them.

7. **Phase the plan — three phases × three tracks (signature structure)**
   Every phase carries an **SEO track**, an **AI-visibility track**, and a **social track**, each with its own work items — and closes with a **measurement gate**.
   - **Phase 1 (weeks 1–4): Foundations.** SEO: technical fixes that unblock everything else, 1–2 quick-win refreshes. AI: baseline benchmark (`marketing-ai-visibility-benchmark`) if not yet run. Social: fix the cadence floor — `marketing-calendar-audit` weekly, fill uncovered demand with `marketing-post-batch`.
   - **Phase 2 (weeks 5–8): Build.** SEO: content tier — `marketing-content-brief` per article (brief + distribution in one); comparison page via `marketing-comparison-page` if the competitive frame supports it; schema via seo-skills' `seo-schema`. AI: first `marketing-ai-campaign` on the missing clusters. Social: `marketing-seo-campaign` on the gap-matrix social routes.
   - **Phase 3 (weeks 9–12): Compound + measure.** SEO: refreshes on what moved; backlink outreach starter (seo-skills' `seo-backlink-gap`). AI: extend winning clusters. Social: double down per `marketing-content-patterns`. Gate: `marketing-campaign-impact` verdicts on every `mkt:*` campaign + `marketing-report` for the quarter.
   - **Measurement gates:** each phase ends with `marketing-report` (period view) and, from Phase 2 on, `marketing-campaign-impact` on the phase's campaigns. A phase whose campaigns read "rework" changes the next phase's scope — write that rule into the plan.
   - For longer horizons (180/365 days), repeat phases 2+3 with refreshed inputs and a quarterly retro.
   - Each work item is tagged with the skill that produces it.

8. **Pick metrics**
   - **Lagging (quarterly):** organic traffic, organic keywords ranking top-10, organic conversions.
   - **Leading (weekly/monthly):** technical-issue count, pages with E-E-A-T verdict ≥ 70, AI Search citation count, referring-domain count.
   - One leading + one lagging per phase. Tie each metric to a current value (from step 2) and a phase-end target. Targets must be defensible — call out base rates.

9. **Sequencing + dependencies**
   - Build a dependency map: e.g., "rewrite cluster A pillar" depends on "fix `noindex` on /blog templates" depends on "run `seo-technical-audit`."
   - Surface the critical path. Anything off the critical path is moveable; anything on it blocks the phase.

10. **Synthesise** `PLAN.md`

## Output format

Folder `marketing-roadmap-{domain-slug}-{YYYYMMDD}/`:

```
marketing-roadmap-{domain-slug}-{YYYYMMDD}/
├── PLAN.md                              (synthesis — primary deliverable; inlines 01-baseline, 02-competitive-frame, 07-dependencies, 08-metrics as sections)
├── 04-phase-1-foundations.md            (load-bearing — owners share single phase files in standups)
├── 05-phase-2-build.md                  (load-bearing — owners share single phase files)
├── 06-phase-3-compound.md               (load-bearing — owners share single phase files)
└── evidence/
    ├── 01-baseline.md                   (where you are now — raw data inlined into PLAN.md)
    ├── 02-competitive-frame.md          (who you're actually competing with — raw data inlined into PLAN.md)
    ├── 03-pillar-scores.md              (technical / content / topical / AI Search — scoring math)
    ├── 07-dependencies-and-critical-path.md  (dependency map — inlined as PLAN.md section)
    └── 08-metrics.md                    (metric tables — inlined as PLAN.md section)
```

Top-level: `PLAN.md` + the three phase files (`04`/`05`/`06`). Owners share single phase files in standups, so phase files stay top-level rather than collapsing into PLAN.md. The verbatim-duplicate sections (baseline, competitive frame, dependencies, metrics) are inlined into PLAN.md but the raw step files are preserved in `evidence/` along with the pillar-scoring math.

`PLAN.md` follows this shape:

```markdown
# Marketing roadmap: {domain}

> Plan dated {YYYY-MM-DD} · Horizon: {n} days · Business type: {type} · Country: {country} · Workspace: {planable workspace}

## Where you are
- Organic keywords: {n} (trend: {↑↓→ over 12mo})
- Organic traffic estimate: {n}/mo
- Domain authority: {n} · Referring domains: {n}
- AI visibility: brand presence {n} · link presence {n} (baseline{ or trend})
- Social: {n} channels · {n} posts/week · {n} audience · {n}% engagement rate
- Pillar scores: Technical {n}/100 · Content {n}/100 · Topical {n}/100 · AI Search {n}/100 · Social {n}/100

## Lead theme
{The lowest pillar from step 5, plus a one-line "why this is the constraint."}

## Top 5 competitors
| Domain | DA | Organic kw | Top cluster they own |
|---|---|---|---|
| {comp} | {n} | {n} | {cluster} |

## Phase 1 — Foundations (weeks 1–4)

**Goal:** {1-line outcome, e.g. "remove technical debt blocking content investment and restore the posting floor"}

| # | Track | Work item | Skill / source | Owner | Effort | Phase-end metric |
|---|---|---|---|---|---|---|
| 1.1 | SEO | {item} | seo-skills `seo-technical-audit` follow-up | {role} | {S/M/L} | {metric} |
| 1.2 | AI | {item} | `marketing-ai-visibility-benchmark` | {role} | {S/M/L} | {metric} |
| 1.3 | Social | {item} | `marketing-post-batch` | {role} | {S/M/L} | {metric} |

**Measurement gate:** `marketing-report` for the phase window{; `marketing-campaign-impact` on `mkt:*` campaigns from Phase 2 on}.
**Phase exit criteria:** {what must be true to declare Phase 1 done}

## Phase 2 — Build (weeks 5–8)
{same shape}

## Phase 3 — Compound + measure (weeks 9–12)
{same shape}

## Critical path
{Ordered list of work items that block subsequent phases. Anything not on this list is moveable.}

## Metrics

| Metric | Track | Type | Current | Phase 1 target | Phase 2 target | Phase 3 target |
|---|---|---|---|---|---|---|
| Organic traffic | SEO | Lagging | {n} | {n} | {n} | {n} |
| Technical issue count | SEO | Leading | {n} | {n} | {n} | {n} |
| AI Search citation count | AI | Leading | {n} | {n} | {n} | {n} |
| AI share of voice vs top competitor | AI | Lagging | {n}% | {n}% | {n}% | {n}% |
| Posts/week (cadence) | Social | Leading | {n} | {n} | {n} | {n} |
| Engagement rate | Social | Lagging | {n}% | {n}% | {n}% | {n}% |
| Campaigns verdict "working" | Cross | Lagging | — | — | {n of m} | {n of m} |

## Constraints / caveats
{User-supplied constraints, plus anything the data flags — e.g., "DA gap to top competitor is 25 points; expect 6+ months for keyword parity."}

## Recommended next step
Run Phase 1 work items. After week 4, run `marketing-report` for the phase window and compare against the baseline captured today, then adjust Phase 2 scope. Consider `marketing-weekly-ops` as the weekly heartbeat between gates.
```

## Tips

- **No silent re-research.** Fresh diagnostic outputs get ingested; missing ones become Phase-0 work items. The user controls API spend.
- **Auto-detect business type cheaply.** Homepage `<title>`, schema `@type`, and top-nav anchors are usually enough. Ask the user only when truly ambiguous.
- **The lead theme is the lowest pillar score.** Don't pick the pillar the user is most excited about — pick the one the data says is the constraint. Surface this gap explicitly if they conflict.
- **Three phases, even for 30-day horizons.** Compress, don't drop. A 30-day plan is foundations (weeks 1–2), build (weeks 2–3), measure (week 4). The structure forces sequencing discipline.
- **Targets must be defensible.** Don't write "double organic traffic in Q1." Tie each target to a base rate from competitor data or category benchmarks. If the math doesn't support a target, say so and lower it.
- **Critical path is the deliverable.** Most teams can do *something*; few know what's blocking what. Surface the dependency chain — that's where this skill earns its keep.
- **Local businesses:** route the local layer to `marketing-local-audit` (diagnosis) and `marketing-local-campaign` (city tracking + GBP content) — put them on the local template's Phase 1.
- **Don't generate work items the team can't execute.** If the user said "no JS-render changes allowed" or "2 posts/week max," drop or resize those items even if they're high-leverage. A plan that won't ship is worse than a smaller plan that does.
- **Update cadence.** Re-run at the horizon's end with the original baseline as input; the measurement gates (`marketing-report`, `marketing-campaign-impact`) rewrite "Where you are" and everything downstream updates from there.

## Works well with

- **Predecessors (produce inputs):** `marketing-gap-matrix`, `marketing-ai-visibility-benchmark`, `marketing-content-patterns`, `marketing-local-audit`; seo-skills' `seo-technical-audit` / `seo-content-audit` / `seo-backlinks-profile` when deeper single-channel data exists.
- **Successors (execute the plan):** `marketing-content-brief` (Phase 2 articles + distribution), `marketing-seo-campaign` / `marketing-ai-campaign` (campaign tracks), `marketing-comparison-page`, `marketing-post-batch`, `marketing-weekly-ops` (weekly heartbeat), `marketing-campaign-impact` + `marketing-report` (phase gates).
