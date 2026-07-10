# Claude SEO Skills

Production-ready [Claude Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) for SEO, powered by the [SE Ranking remote MCP](https://seranking.com/api/integrations/mcp). Each skill turns raw API data into a finished SEO deliverable — content briefs, AI Search share of voice, page intelligence, structured data, drift monitoring, SXO diagnostics, competitive analysis, and more.

Upstream source: [`seranking/seo-skills`](https://github.com/seranking/seo-skills).

## Install

This plugin is distributed via the [`seranking/claude-plugins`](https://github.com/seranking/claude-plugins) marketplace.

### Claude Desktop (Cowork)

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add**.
3. Choose **Add marketplace** and enter `seranking/claude-plugins`.
4. Install `seo-skills` once the marketplace loads.

### Claude Code (terminal)

```
/plugin marketplace add seranking/claude-plugins
/plugin install seo-skills@seranking-claude-plugins
```

Trigger skills with `/seo-content-brief`, `/seo-page`, etc. — or describe what you want and Claude picks the right skill.

### Connect SE Ranking

The SE Ranking remote MCP auto-registers when you install the plugin. On your first session, run `/mcp` and sign in via OAuth.

### Optional extensions (`seo-google`, `seo-firecrawl`)

Most skills work out of the box. For Google APIs and Firecrawl-backed skills, run from the installed plugin directory:

```bash
bash install.sh --all
```

Or install extensions individually:

```bash
bash extensions/google/install.sh
bash extensions/firecrawl/install.sh
```

## Skills (32)

### Featured

| Skill | What it produces |
| --- | --- |
| [seo-ai-search-share-of-voice](skills/seo-ai-search-share-of-voice/SKILL.md) | Share-of-voice heatmap across ChatGPT, Perplexity, Gemini, AI Overviews, and AI Mode |
| [seo-backlink-gap](skills/seo-backlink-gap/SKILL.md) | Prospect list of referring domains linking to competitors but not to your site |
| [seo-content-brief](skills/seo-content-brief/SKILL.md) | Writer-ready editor brief with keyword research, SERP analysis, and internal linking plan |
| [seo-keyword-cluster](skills/seo-keyword-cluster/SKILL.md) | Intent-grouped clusters with pillar plus spokes architecture |
| [seo-ads](skills/seo-ads/SKILL.md) | Paid-search competitive intelligence and bid-keyword shortlist |
| [seo-content-audit](skills/seo-content-audit/SKILL.md) | E-E-A-T + CITE audit with publish / publish-with-fixes / no-publish verdict |
| [seo-sxo](skills/seo-sxo/SKILL.md) | SERP-backed page-type mismatch analysis with persona scores |

### Full catalogue

`seo-agency-landing-page`, `seo-api`, `seo-backlinks-profile`, `seo-competitor-gap-analysis`, `seo-competitor-pages`, `seo-drift`, `seo-firecrawl`, `seo-geo`, `seo-google`, `seo-hreflang`, `seo-images`, `seo-keyword-niche`, `seo-local`, `seo-page`, `seo-plan`, `seo-schema`, `seo-sitemap`, `seo-subdomain`, `seo-technical-audit`, plus social bridge skills: `ai-search-gaps-to-social-campaign`, `client-onboarding-proposal`, `local-gmb-visibility`, `seo-ai-social-report`, `seo-gaps-to-social-campaign`, `site-audit-to-social-distribution`.

See [`examples/`](examples/) for dated sample outputs.

## Requirements

- **Claude plan**: Pro, Max, or Team/Enterprise (Skills are not available on the free tier).
- **SE Ranking**: MCP access on a plan that includes the Data API.

## License

MIT — see [LICENSE](LICENSE).
