# Marketing Automation Plugins

Official plugin marketplace from [SE Ranking](https://seranking.com) and [Planable](https://planable.io) for marketing automation agents.

Two plugins, zero skill name collisions — install what you need:

| Plugin | Skills | Best for |
| --- | --- | --- |
| [`content-skills`](plugins/content-skills/) | 10 | Content lifecycle pipelines: briefs, editorial plans, refresh, social packs, page recovery |
| [`marketing-skills`](plugins/marketing-skills/) | 23 | Cross-channel marketing: diagnose → act → measure across SEO, AI search, and social |

Both plugins share the same MCP backends (SE Ranking + Planable). They are designed to install side by side with [`seranking/seo-skills`](https://github.com/seranking/seo-skills) and [`Planable/smm-skills`](https://github.com/Planable/smm-skills) — every skill here uses a `content-` or `marketing-` prefix, so names never collide.

## Install — Claude Code

```bash
/plugin marketplace add seranking/marketing-automation-plugins
/plugin install marketing-skills@marketing-automation-plugins
```

Content-only bundle:

```bash
/plugin install content-skills@marketing-automation-plugins
```

### Claude Desktop / Cowork

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add** → **Add marketplace**.
3. Enter `seranking/marketing-automation-plugins`.
4. Install `marketing-skills` and/or `content-skills`.

## Install — Cursor

This repo is also a Cursor multi-plugin marketplace (`.cursor-plugin/marketplace.json`).

1. Add the repository as a plugin source / marketplace in Cursor Customize.
2. Install `content-skills` or `marketing-skills`.
3. Connect MCP servers when prompted (or via `/mcp`).

Each plugin ships `.cursor-plugin/plugin.json` plus `mcp.json` / `.mcp.json` for SE Ranking and Planable.

## Install — Codex

Repo-local Codex marketplace lives at `.agents/plugins/marketplace.json`. Each plugin has `.codex-plugin/plugin.json` pointing at `./skills/` and `./.mcp.json`.

In Codex / ChatGPT Work, add this repository as a marketplace source, then install `content-skills` or `marketing-skills`.

## Connect the MCP servers

Plugins auto-register the required remote MCP servers. On first use, sign in via OAuth:

- **SE Ranking** — `https://api.seranking.com/mcp` ([setup](https://seranking.com/api/integrations/mcp/))
- **Planable** — `https://mcp.planable.io/mcp` ([help](https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools))

## Requirements

- **Agent host**: Claude (Pro/Max/Team), Cursor, or Codex with Skills/plugins support.
- **SE Ranking**: MCP access on a plan that includes the Data API.
- **Planable**: MCP access on Team plan or higher.

## Repository layout

```text
marketing-automation-plugins/
├── .claude-plugin/marketplace.json   # Claude Code marketplace
├── .cursor-plugin/marketplace.json   # Cursor marketplace
├── .agents/plugins/marketplace.json  # Codex marketplace
└── plugins/
    ├── content-skills/
    │   ├── .claude-plugin/plugin.json
    │   ├── .cursor-plugin/plugin.json
    │   ├── .codex-plugin/plugin.json
    │   ├── .mcp.json
    │   ├── mcp.json
    │   └── skills/
    └── marketing-skills/
        ├── .claude-plugin/plugin.json
        ├── .cursor-plugin/plugin.json
        ├── .codex-plugin/plugin.json
        ├── .mcp.json
        ├── mcp.json
        └── skills/
```

Skills and MCP configs are shared; only the `.*-plugin` manifests differ per agent host.

## Learn more

- [Content Skills for Claude](https://seranking.com/claude-content-skills.html)
- [Marketing Skills for Claude](https://seranking.com/claude-marketing-skills.html)
- Full SEO toolkit (separate repo): [`seranking/seo-skills`](https://github.com/seranking/seo-skills)

## License

MIT — see [content-skills/LICENSE](plugins/content-skills/LICENSE) and [marketing-skills/LICENSE](plugins/marketing-skills/LICENSE).
