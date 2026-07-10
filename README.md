# SE Ranking Claude Plugins

Official [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) from [SE Ranking](https://seranking.com) and [Planable](https://planable.io). Three plugins, one repo — install only what you need.

## Plugins

| Plugin | Skills | Best for |
| --- | --- | --- |
| [`content-skills`](plugins/content-skills/) | 9 | Content lifecycle: briefs, audits, keyword research, social drafts |
| [`marketing-skills`](plugins/marketing-skills/) | 21 | Full marketing stack: SEO, AI visibility, social, reporting, agency workflows |
| [`seo-skills`](plugins/seo-skills/) | 32 | Full SEO toolkit: technical audits, backlinks, schema, Google APIs, AI search, local/international SEO |

**Pick one plugin, not all.** `marketing-skills` is a superset of `content-skills` — installing both adds duplicate skills. `seo-skills` overlaps with parts of `marketing-skills` but adds deeper SEO coverage (technical audits, backlinks, schema, Google APIs, and more). For most marketing users, `marketing-skills` is enough; choose `seo-skills` for a dedicated SEO workflow.

## Install

### Claude Code (terminal)

```bash
/plugin marketplace add seranking/claude-plugins
/plugin install marketing-skills@seranking-claude-plugins
```

For the content-only bundle:

```bash
/plugin install content-skills@seranking-claude-plugins
```

For the full SEO toolkit (from [`seranking/seo-skills`](https://github.com/seranking/seo-skills)):

```bash
/plugin install seo-skills@seranking-claude-plugins
```

### Claude Desktop / Cowork

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add**, then choose **Add marketplace**.
3. Enter `seranking/claude-plugins`.
4. Install `marketing-skills`, `content-skills`, or `seo-skills`.

### Connect the MCP servers

Plugins auto-register the required remote MCP servers. On first use, run `/mcp` and sign in via OAuth:

- **SE Ranking** — `https://api.seranking.com/mcp` (all plugins)
- **Planable** — `https://mcp.planable.io/mcp` (`content-skills`, `marketing-skills`)

## Requirements

- **Claude plan**: Pro, Max, or Team/Enterprise (Skills are not available on the free tier).
- **SE Ranking**: MCP access on a plan that includes the Data API.
- **Planable**: MCP access on Team plan or higher.

## Learn more

- [Content Skills for Claude](https://seranking.com/claude-content-skills.html)
- [Marketing Skills for Claude](https://seranking.com/claude-marketing-skills.html)

## License

MIT — see [content-skills/LICENSE](plugins/content-skills/LICENSE), [marketing-skills/LICENSE](plugins/marketing-skills/LICENSE), and [seo-skills/LICENSE](plugins/seo-skills/LICENSE).
