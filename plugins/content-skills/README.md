# Content Skills for Claude

Production-ready [Claude Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) for content planning, briefing, auditing, refreshing, and repurposing — powered by the [SE Ranking remote MCP](https://seranking.com/api/integrations/mcp/) and the [Planable remote MCP](https://mcp.planable.io/mcp).

This plugin packages 9 skills already published individually in [seranking/seo-skills](https://github.com/seranking/seo-skills) and [Planable/smm-skills](https://github.com/Planable/smm-skills) into one bundle focused specifically on the content lifecycle, as featured on [Content Skills for Claude, by SE Ranking & Planable](https://seranking.com/claude-content-skills.html).

## Skills

### SEO content skills (SE Ranking data)

| Skill | What it produces | Source |
| --- | --- | --- |
| [`seo-content-brief`](skills/seo-content-brief/SKILL.md) | Writer-ready editor brief from a domain and topic — keyword research, SERP analysis, competitor teardown, internal-linking plan, AI Search citation angle | seo-skills |
| [`seo-keyword-cluster`](skills/seo-keyword-cluster/SKILL.md) | Intent-grouped clusters with pillar + spokes architecture and H1/H2 suggestions per spoke | seo-skills |
| [`seo-content-audit`](skills/seo-content-audit/SKILL.md) | E-E-A-T (60-item) + CITE (30-item) audit for existing content with a publish / publish-with-fixes / no-publish verdict | seo-skills |
| [`seo-keyword-niche`](skills/seo-keyword-niche/SKILL.md) | Longtail + question keyword mining for niche content opportunities, with a content-tier plan and thin-content quality gates | seo-skills |

### Social execution skills (Planable data)

| Skill | What it produces | Source |
| --- | --- | --- |
| [`draft-post-batch`](skills/draft-post-batch/SKILL.md) | Platform-adapted draft posts created directly in Planable from a brief, with preview before commit | smm-skills |
| [`content-pattern-intelligence`](skills/content-pattern-intelligence/SKILL.md) | Recurring content patterns from top-performing posts, with real examples and hypotheses to test next | smm-skills |

### Ecosystem skills (SE Ranking + Planable)

| Skill | What it produces | Source |
| --- | --- | --- |
| [`seo-gaps-to-social-campaign`](skills/seo-gaps-to-social-campaign/SKILL.md) | Themed social campaign drafted in Planable from SE Ranking keyword gaps, competitor wins, and ranking losses | smm-skills |
| [`ai-search-gaps-to-social-campaign`](skills/ai-search-gaps-to-social-campaign/SKILL.md) | AI-visibility snapshot across ChatGPT/Perplexity/Gemini/AI Overview + social drafts targeting missing narratives | smm-skills |
| [`site-audit-to-social-distribution`](skills/site-audit-to-social-distribution/SKILL.md) | Prioritised SEO fix list, rewritten page copy for review, and a coordinated social distribution batch | smm-skills |

These skills are duplicated verbatim from their origin repos (kept in sync manually), not moved — they continue to exist and update independently in `seo-skills` and `smm-skills`.

## Install

This plugin is distributed via the [`seranking/claude-plugins`](https://github.com/seranking/claude-plugins) marketplace.

### Claude Code (terminal)

```
/plugin marketplace add seranking/claude-plugins
/plugin install content-skills@seranking-claude-plugins
```

### Claude Desktop / Cowork

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add**, then choose **Add marketplace**.
3. Enter `seranking/claude-plugins` and install `content-skills`.

### Connect the data sources

Both MCP servers auto-register when you install the plugin:

- **SE Ranking** (`https://api.seranking.com/mcp`) — required for all SEO content skills; requires an SE Ranking account with API/MCP access.
- **Planable** (`https://mcp.planable.io/mcp`) — required for the social execution and ecosystem skills; requires a Planable account on the Team plan or higher.

Run `/mcp` on first use and sign in via OAuth for each.

## How these skills chain

1. `seo-keyword-cluster` or `seo-keyword-niche` — find the topic/cluster to write about.
2. `seo-content-brief` — turn the topic into a writer-ready brief.
3. `seo-content-audit` — check existing pages for E-E-A-T + AI-citation readiness before promoting them.
4. `draft-post-batch` — turn an approved brief or a passing audit into social drafts in Planable.
5. `content-pattern-intelligence` — see what's already working on social, to seed the next brief.
6. `seo-gaps-to-social-campaign` / `ai-search-gaps-to-social-campaign` — turn SEO or AI-visibility gaps directly into a social campaign.
7. `site-audit-to-social-distribution` — coordinate a page rewrite with its social announcement.

## Requirements

- **Claude plan**: Pro, Max, or Team/Enterprise (Skills are not available on the free tier).
- **SE Ranking plan**: MCP access on a plan that includes the Data API.
- **Planable plan**: MCP access on Team plan or higher.
- **Permissions**: Skills inherit the connected user's SE Ranking and Planable permissions.
- **Drafts only**: skills that touch Planable create drafts; nothing publishes without explicit user action in Planable.

## License

MIT. See `LICENSE`.
