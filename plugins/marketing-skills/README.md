# Marketing Skills for Claude

Run SEO, AI visibility, and social workflows in Claude using prebuilt Skills powered by [SE Ranking](https://seranking.com) and [Planable](https://planable.io) data — individually or as one connected ecosystem.

Each skill is a packaged instruction set that Claude loads automatically when relevant, following [Anthropic's Agent Skills spec](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview): the `SKILL.md` frontmatter tells Claude when to use it, and the body is loaded only when triggered (progressive disclosure). Together with the SE Ranking and Planable MCP servers, they turn raw marketing data into finished deliverables — audits, reports, content briefs, post batches, campaigns, and roadmaps.

Learn more: [Marketing Skills for Claude](https://seranking.com/claude-marketing-skills.html)

## Skills

### SEO & AI visibility (SE Ranking MCP)

| Skill | What it produces |
| --- | --- |
| [seo-competitor-gap-analysis](skills/seo-competitor-gap-analysis/SKILL.md) | Keyword gap analysis against organic competitors — keywords they rank for that you don't, filtered by intent, volume, and difficulty, prioritized by opportunity |
| [seo-competitor-pages](skills/seo-competitor-pages/SKILL.md) | Page-level competitive teardown — how competitor pages win rankings and what to change on yours |
| [seo-content-brief](skills/seo-content-brief/SKILL.md) | Writer-ready SEO content brief — target keywords, title options, H2/H3 structure, content gaps, internal links, AI search angle |
| [seo-ai-search-share-of-voice](skills/seo-ai-search-share-of-voice/SKILL.md) | AI search share-of-voice heatmap across ChatGPT, Perplexity, Gemini, AI Overviews, and AI Mode with topic-cluster ownership by brand |
| [seo-ads](skills/seo-ads/SKILL.md) | PPC competitive analysis — who bids on your keywords, competitor ad copy patterns, and bid keywords worth testing |
| [seo-local](skills/seo-local/SKILL.md) | Local SEO report — Google Business Profile, NAP consistency, local pack visibility, and citation sources |
| [seo-sxo](skills/seo-sxo/SKILL.md) | Search experience (SXO) diagnostics — persona-based UX and conversion review of ranking pages |
| [seo-agency-landing-page](skills/seo-agency-landing-page/SKILL.md) | SEO-optimized landing page copy backed by keyword and SERP data |
| [seo-plan](skills/seo-plan/SKILL.md) | Strategic SEO roadmap — prioritized plan built from domain, competitor, and keyword data |

### Social media (Planable MCP)

| Skill | What it produces |
| --- | --- |
| [draft-post-batch](skills/draft-post-batch/SKILL.md) | Platform-adapted draft posts created directly in Planable from a brief, with preview before commit, varied hooks, and validation flags |
| [pending-approvals-roundup](skills/pending-approvals-roundup/SKILL.md) | Single roundup of every post awaiting sign-off across all client workspaces, with 48h-deadline posts called out |
| [content-calendar-audit](skills/content-calendar-audit/SKILL.md) | This-week schedule mapped by day with approval status, gap detection, and an offer to draft fill-in content |
| [cross-client-metrics-overview](skills/cross-client-metrics-overview/SKILL.md) | Comparative table across every client/workspace ranked by engagement, impressions, or volume |
| [monthly-performance-summary](skills/monthly-performance-summary/SKILL.md) | Client-ready HTML report from Planable analytics or any CSV export — KPI cards, per-platform breakdowns, top posts |
| [content-pattern-intelligence](skills/content-pattern-intelligence/SKILL.md) | Named recurring content patterns with real post examples and specific hypotheses to test next |

### Ecosystem (SE Ranking + Planable MCP)

These bridge both platforms — turning search and AI-visibility insight into social content, coordinated rewrites, local visibility, client proposals, and cross-channel reporting. They require **both** MCPs connected.

| Skill | What it produces |
| --- | --- |
| [seo-gaps-to-social-campaign](skills/seo-gaps-to-social-campaign/SKILL.md) | Themed social campaign drafted in Planable from SE Ranking keyword gaps, competitor wins, and ranking losses |
| [ai-search-gaps-to-social-campaign](skills/ai-search-gaps-to-social-campaign/SKILL.md) | AI-visibility snapshot plus social drafts targeting missing narratives, with a before/after tracking plan |
| [site-audit-to-social-distribution](skills/site-audit-to-social-distribution/SKILL.md) | Prioritized SEO fix list, rewritten page copy for approval in Planable, and a coordinated social distribution batch |
| [local-gmb-visibility](skills/local-gmb-visibility/SKILL.md) | City-level rank tracking setup, city-by-city ranking snapshot, and Google Business Profile + local social drafts |
| [client-onboarding-proposal](skills/client-onboarding-proposal/SKILL.md) | Data-backed onboarding proposal pairing an SEO + AI audit with the client's current social baseline |
| [seo-ai-social-report](skills/seo-ai-social-report/SKILL.md) | One cross-channel performance report — rankings, AI visibility, and social in a single interactive HTML report |

## Install

This plugin is distributed via the [`seranking/claude-plugins`](https://github.com/seranking/claude-plugins) marketplace. Installing `marketing-skills` registers all 21 skills and auto-configures both the SE Ranking and Planable remote MCP servers in one step.

### Claude Desktop (Cowork)

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add**.
3. Choose **Add marketplace** and enter `seranking/claude-plugins`.
4. Install `marketing-skills` once the marketplace loads.

### Claude Code (terminal)

```
/plugin marketplace add seranking/claude-plugins
/plugin install marketing-skills@seranking-claude-plugins
```

Skills appear under the `marketing-skills` namespace — or just describe what you want ("run a competitor gap analysis for acme.com", "draft 5 LinkedIn posts about our launch") and Claude picks the right skill automatically.

### Connect the MCP servers

Both remote MCPs auto-register when you install the plugin. On your first session, run `/mcp` and sign in:

- **SE Ranking MCP** (`https://api.seranking.com/mcp`) — see the [setup guide](https://seranking.com/api/integrations/mcp/)
- **Planable MCP** (`https://mcp.planable.io/mcp`) — OAuth sign-in, see the [help article](https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools)

## Requirements

- **Claude plan**: Pro, Max, or Teams (Skills are not available on the free tier).
- **SE Ranking plan**: MCP access is available on every plan. Projects are required for ranking-loss detection, ongoing AI Result Tracker, and city-level rank tracking; one-off audits and AI Search snapshots work without a project.
- **Planable plan**: MCP access on Team plan or higher. The Analytics add-on is required for social analytics data in reports.
- **Permissions**: Skills inherit the connected user's SE Ranking and Planable permissions.
- **Drafts only**: social skills create drafts in Planable; nothing publishes to social platforms without explicit user action.

Two skills work without a Planable account by accepting a CSV export from any analytics tool: `monthly-performance-summary` and `content-pattern-intelligence`.

## Example outputs

The `examples/` folder contains real example outputs for the SEO skills — reports, briefs, and plans generated against public domains — linked from the corresponding `SKILL.md` files.

## License

MIT. See `LICENSE`.

## Contributing

Bug reports and improvements welcome. Open an issue with the skill name, the input, and the unexpected output.
