# Content Skills for Claude

Production-ready [Claude Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) for the content lifecycle — plan, brief, create, refresh, distribute, and measure — powered by the [SE Ranking remote MCP](https://seranking.com/api/integrations/mcp/) and the [Planable remote MCP](https://mcp.planable.io/mcp).

Featured on [Content Skills for Claude, by SE Ranking & Planable](https://seranking.com/claude-content-skills.html).

**How this differs from [seranking/seo-skills](https://github.com/seranking/seo-skills) and [Planable/smm-skills](https://github.com/Planable/smm-skills):** those plugins ship single-deliverable tools (a brief, an audit verdict, a post batch). Every skill here is a pipeline — it produces the deliverable *plus* the distribution and measurement around it, and applies your saved brand profile. All three plugins install side by side with zero name collisions; each skill's description tells Claude when to route to the base tool instead.

## Skills

### Plan & brief (SE Ranking + Planable)

| Skill | What it produces | vs the base tool |
| --- | --- | --- |
| [`content-campaign-brief`](skills/content-campaign-brief/SKILL.md) | Writer-ready SEO brief + teaser social drafts in Planable around the publish date + keyword/AI-prompt tracking baseline | `seo-content-brief` stops at the document |
| [`content-editorial-plan`](skills/content-editorial-plan/SKILL.md) | Keyword clusters mapped onto a 12-week publication calendar with cadence, brief queue, and monthly social themes | `seo-keyword-cluster` stops at the architecture |
| [`content-niche-plan`](skills/content-niche-plan/SKILL.md) | Longtail content tier where every candidate is validated against social demand: build now / experiment / skip | `seo-keyword-niche` validates with search data only |
| [`content-ideas-from-social`](skills/content-ideas-from-social/SKILL.md) | Top-performing social topics validated with SE Ranking keyword data → mini content plan | `content-pattern-intelligence` stops at patterns |

### Refresh & recover (SE Ranking + Planable)

| Skill | What it produces | vs the base tool |
| --- | --- | --- |
| [`content-refresh-pipeline`](skills/content-refresh-pipeline/SKILL.md) | Batch E-E-A-T/CITE audit (1–10 URLs) → prioritized refresh queue → per-page fix lists → re-distribution drafts → re-check plan | `seo-content-audit` verdicts one page |
| [`content-page-recovery`](skills/content-page-recovery/SKILL.md) | Weak-page fixes with stakeholder approval in Planable, a relaunch quality gate, internal-link refresh, and post-fix measurement | `site-audit-to-social-distribution` has no relaunch gate |
| [`content-ai-visibility`](skills/content-ai-visibility/SKILL.md) | AI share-of-voice snapshot + passage-level citability fixes for existing pages + social drafts + before/after re-check | `ai-search-gaps-to-social-campaign` is social-only |

### Distribute (Planable, SE Ranking where noted)

| Skill | What it produces | vs the base tool |
| --- | --- | --- |
| [`content-social-pack`](skills/content-social-pack/SKILL.md) | One article URL → platform-adapted derivative set (LinkedIn, X thread, GBP, newsletter, quote hooks) grouped as a Planable campaign | `draft-post-batch` drafts from a topic, not an article |
| [`content-gap-sprint`](skills/content-gap-sprint/SKILL.md) | SEO gaps → social campaign + article briefs for the top 2–3 gaps, sequenced into one sprint timeline | `seo-gaps-to-social-campaign` produces posts only |

### Brand layer (no MCPs required)

| Skill | What it produces |
| --- | --- |
| [`content-brand-setup`](skills/content-brand-setup/SKILL.md) | A `brand-profile.md` (voice, audience, banned words, CTA conventions) that every other skill in this plugin applies automatically |

## Install

Distributed from the [`seranking/marketing-automation-plugins`](https://github.com/seranking/marketing-automation-plugins) marketplace (Claude, Cursor, and Codex ready).

### Claude Code (terminal)

```
/plugin marketplace add seranking/marketing-automation-plugins
/plugin install content-skills@marketing-automation-plugins
```

### Claude Desktop / Cowork

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add**, then choose **Add marketplace**.
3. Enter `seranking/marketing-automation-plugins` and install `content-skills`.

### Connect the data sources

Both MCP servers auto-register when you install the plugin:

- **SE Ranking** (`https://api.seranking.com/mcp`) — required for the plan/brief/refresh skills; needs an SE Ranking account with API/MCP access.
- **Planable** (`https://mcp.planable.io/mcp`) — required for distribution and approval steps; needs a Planable account on the Team plan or higher.

Run `/mcp` on first use and sign in via OAuth for each.

## A typical lifecycle run

1. `content-brand-setup` — once, so everything after speaks in your voice.
2. `content-ideas-from-social` or `content-editorial-plan` — decide what to make.
3. `content-campaign-brief` — brief the article with distribution and tracking built in.
4. `content-social-pack` — atomize the published article into a platform pack.
5. `content-refresh-pipeline` / `content-page-recovery` — keep what you shipped alive.
6. `content-ai-visibility` — win the citations, then prove it with the re-check.

## Requirements

- **Claude plan**: Pro, Max, or Team/Enterprise (Skills are not available on the free tier).
- **SE Ranking plan**: MCP access on a plan that includes the Data API.
- **Planable plan**: MCP access on Team plan or higher.
- **Permissions**: Skills inherit the connected user's SE Ranking and Planable permissions.
- **Drafts only**: skills that touch Planable create drafts; nothing publishes without explicit user action in Planable.

## License

MIT. See `LICENSE`.
