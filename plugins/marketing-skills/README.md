# Marketing Skills for Claude

Cross-channel marketing skills that connect [SE Ranking](https://seranking.com) (SEO + AI-search data) with [Planable](https://planable.io) (social content workflow). Every skill in this plugin does something the single-channel plugins can't: it combines both data sources, turns analysis into a routed decision, or closes the loop with measurement.

The plugin is organized as a pipeline:

```
DIAGNOSE  (where are the opportunities)  →  ACT  (create & launch, instrumented)  →  MEASURE  (did it work)
```

What ties it together is the **campaign contract**: every campaign an Act-skill launches gets a Planable label (`mkt:<slug>`) and a **campaign passport** — a durable post recording the goal, launch date, and baseline numbers. The Measure-skills read that contract and answer, with evidence, the question every campaign eventually faces: *did it work?*

Learn more: [Marketing Skills for Claude](https://seranking.com/claude-marketing-skills.html) · Skills follow [Anthropic's Agent Skills spec](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

## Skills (23)

### Diagnose

| Skill | What it produces |
| --- | --- |
| [marketing-gap-matrix](skills/marketing-gap-matrix/SKILL.md) | Competitor keyword gaps checked against your site coverage AND social history — every gap routed: site / social / both / skip |
| [marketing-ai-visibility-benchmark](skills/marketing-ai-visibility-benchmark/SKILL.md) | AI share-of-voice heatmap vs competitors with the "do we even post about this" overlay per losing cluster |
| [marketing-calendar-audit](skills/marketing-calendar-audit/SKILL.md) | Calendar review with a live demand layer — what the audience is asking this week that the schedule doesn't cover |
| [marketing-content-patterns](skills/marketing-content-patterns/SKILL.md) | Winning content patterns validated against search demand — "post more of this" and "make a page for that" |
| [marketing-approvals-triage](skills/marketing-approvals-triage/SKILL.md) | Approval queue ranked by stake: campaign blockers and deadline risks first, stale items aged |
| [marketing-ads-counterplay](skills/marketing-ads-counterplay/SKILL.md) | Competitor PPC intelligence turned into an organic counterplay with ready social hooks |
| [marketing-serp-fit](skills/marketing-serp-fit/SKILL.md) | "Why isn't this page ranking" SERP diagnosis with an audience reality check from your own engagement data |
| [marketing-local-audit](skills/marketing-local-audit/SKILL.md) | Local presence audit (GBP, NAP, local pack, citations, reviews) plus the local content activity slice |

### Act

| Skill | What it produces |
| --- | --- |
| [marketing-seo-campaign](skills/marketing-seo-campaign/SKILL.md) | Keyword gaps and ranking losses turned into a labeled, passport-backed social campaign in Planable |
| [marketing-ai-campaign](skills/marketing-ai-campaign/SKILL.md) | Missing AI-answer narratives turned into a measurable campaign with baseline SoV recorded |
| [marketing-content-brief](skills/marketing-content-brief/SKILL.md) | Writer-ready SEO brief that ships with its own distribution batch drafted in Planable |
| [marketing-post-batch](skills/marketing-post-batch/SKILL.md) | Demand-validated post batch — every topic checked against real search/question demand before writing |
| [marketing-comparison-page](skills/marketing-comparison-page/SKILL.md) | "X vs Y" page draft launched through Planable approval plus a supporting social series |
| [marketing-agency-leadgen](skills/marketing-agency-leadgen/SKILL.md) | Agency demand-gen landing page plus the UTM-tagged social batch that drives traffic to it |
| [marketing-content-refresh](skills/marketing-content-refresh/SKILL.md) | Audit-driven page rewrites through Planable approval + distribution, instrumented for verification |
| [marketing-local-campaign](skills/marketing-local-campaign/SKILL.md) | City-level rank tracking + GBP/local social content, labeled per city with starting positions recorded |
| [marketing-roadmap](skills/marketing-roadmap/SKILL.md) | Quarterly roadmap where every phase carries three tracks — SEO, AI visibility, social — with measurement gates |
| [marketing-onboarding-proposal](skills/marketing-onboarding-proposal/SKILL.md) | Data-backed client proposal that instruments the baseline on day one |

### Measure

| Skill | What it produces |
| --- | --- |
| [marketing-campaign-impact](skills/marketing-campaign-impact/SKILL.md) | Per-campaign verdict — working / not yet / rework — with baseline→current numbers from the passport |
| [marketing-report](skills/marketing-report/SKILL.md) | One interactive cross-channel report: rankings, AI visibility, social, and a per-campaign section |
| [marketing-month-close](skills/marketing-month-close/SKILL.md) | Month summary that ends in next month's plan: winners × next month's demand → calendar seeds |
| [marketing-client-scorecard](skills/marketing-client-scorecard/SKILL.md) | Every client scored across social + search + AI with cross-channel mismatch flags |
| [marketing-weekly-ops](skills/marketing-weekly-ops/SKILL.md) | The Monday ritual: approvals triage + calendar×demand + campaign status in one prioritized briefing |

## How the pipeline chains

A typical engagement: `marketing-onboarding-proposal` (baseline) → `marketing-gap-matrix` / `marketing-ai-visibility-benchmark` (diagnose) → `marketing-seo-campaign` / `marketing-ai-campaign` / `marketing-content-brief` (act, instrumented) → `marketing-weekly-ops` (weekly heartbeat) → `marketing-campaign-impact` + `marketing-report` (measure) → `marketing-month-close` (decide next month). `marketing-roadmap` sequences all of it quarter by quarter.

## Install

Distributed from the [`seranking/marketing-automation-plugins`](https://github.com/seranking/marketing-automation-plugins) marketplace ([Claude plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces); also Cursor + Codex ready). Installing the plugin registers all 23 skills and auto-configures both remote MCP servers.

### Claude Desktop (Cowork)

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add**.
3. Choose **Add marketplace** and enter `seranking/marketing-automation-plugins`.
4. Install `marketing-skills` once the marketplace loads.

### Claude Code (terminal)

```
/plugin marketplace add seranking/marketing-automation-plugins
/plugin install marketing-skills@marketing-automation-plugins
```

### Connect the MCP servers

Both remote MCPs auto-register on install. On your first session, run `/mcp` and sign in:

- **SE Ranking MCP** (`https://api.seranking.com/mcp`) — [setup guide](https://seranking.com/api/integrations/mcp/)
- **Planable MCP** (`https://mcp.planable.io/mcp`) — OAuth sign-in, [help article](https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools)

## How this relates to seo-skills and smm-skills

Three plugins, three jobs — install what fits:

| Plugin | Job | Example |
| --- | --- | --- |
| [seranking/seo-skills](https://github.com/seranking/seo-skills) | Single-channel SEO depth (SE Ranking only) | `seo-competitor-gap-analysis` — the keyword gap list |
| [Planable/smm-skills](https://github.com/Planable/smm-skills) | Single-channel social depth (Planable only) | `draft-post-batch` — a quick batch from a brief |
| **marketing-skills** (this plugin) | **The connected layer (both MCPs)** | `marketing-gap-matrix` — the same gaps routed against site + social coverage |

Every skill here carries the `marketing-` prefix, so the three plugins coexist without name collisions, and each description states when to use the channel-plugin sibling instead.

## Requirements

- **Claude plan**: Pro, Max, or Teams (Skills are not available on the free tier).
- **SE Ranking plan**: MCP access is available on every plan. Projects are required for ranking-loss detection, ongoing AI Result Tracker, and city-level rank tracking; one-off analyses work without a project.
- **Planable plan**: MCP access on Team plan or higher. The Analytics add-on is required for the analytics-based sections.
- **Permissions**: Skills inherit the connected user's SE Ranking and Planable permissions.
- **Drafts only**: content skills create drafts in Planable; nothing publishes to social platforms without explicit user action. Skills that write to SE Ranking projects (tracking setup) always ask first — project writes consume plan limits.

## License

MIT. See `LICENSE`.

## Contributing

Bug reports and improvements welcome. Open an issue with the skill name, the input, and the unexpected output.
