# SE Ranking Claude Plugins

Official [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) from [SE Ranking](https://seranking.com) and [Planable](https://planable.io). Two plugins, one repo — install only what you need.

## Plugins

| Plugin | Skills | Best for |
| --- | --- | --- |
| [`content-skills`](plugins/content-skills/) | 9 | Content lifecycle: briefs, audits, keyword research, social drafts |
| [`marketing-skills`](plugins/marketing-skills/) | 21 | Full marketing stack: SEO, AI visibility, social, reporting, agency workflows |

**Pick one plugin, not both.** `marketing-skills` is a superset of `content-skills` — installing both adds duplicate skills. For most users, `marketing-skills` is enough.

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

### Claude Desktop / Cowork

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add**, then choose **Add marketplace**.
3. Enter `seranking/claude-plugins`.
4. Install `marketing-skills` or `content-skills`.

### Connect the MCP servers

Both plugins auto-register the SE Ranking and Planable remote MCP servers. On first use, run `/mcp` and sign in via OAuth:

- **SE Ranking** — `https://api.seranking.com/mcp`
- **Planable** — `https://mcp.planable.io/mcp`

## Requirements

- **Claude plan**: Pro, Max, or Team/Enterprise (Skills are not available on the free tier).
- **SE Ranking**: MCP access on a plan that includes the Data API.
- **Planable**: MCP access on Team plan or higher.

## Learn more

- [Content Skills for Claude](https://seranking.com/claude-content-skills.html)
- [Marketing Skills for Claude](https://seranking.com/claude-marketing-skills.html)

## License

MIT — see [content-skills/LICENSE](plugins/content-skills/LICENSE) and [marketing-skills/LICENSE](plugins/marketing-skills/LICENSE).
