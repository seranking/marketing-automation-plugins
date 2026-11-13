# Сlaude Marketing Skills & Content Skills: SE Ranking x Planable

Official plugin marketplace from [SE Ranking](https://seranking.com) and [Planable](https://planable.io) for marketing automation agents.

- [Claude Marketing Skills](https://seranking.com/claude-marketing-skills.html): 23 skills for SEO, AI Search, paid ads, and social.
- [Claude Content Skills](https://seranking.com/claude-content-skills.html): 10 skills for briefs, editorial plans, refreshes and social packs.

| Plugin | Skills | Best for |
| --- | --- | --- |
| [`content-refresh`](plugins/content-refresh/) | 2 | Standalone refresh flow: diagnose why pages decayed → approve in chat → fact-checked update drafts |
| [`content-skills`](plugins/content-skills/) | 10 | Content lifecycle pipelines: briefs, editorial plans, refresh, social packs, page recovery |
| [`ecosystem-skills`](plugins/ecosystem-skills/) | 3 | The joint SE Ranking + Planable use cases: see → act → see again across search, AI answers, and social |
| [`marketing-skills`](plugins/marketing-skills/) | 23 | Cross-channel marketing: diagnose → act → measure across SEO, AI search, and social |

All four plugins run on the SE Ranking MCP. `content-skills`, `ecosystem-skills`, and `marketing-skills` pair it with Planable; `content-refresh` pairs it with Firecrawl instead and never touches a social channel. They are designed to install side by side with [`seranking/seo-skills`](https://github.com/seranking/seo-skills) and [`Planable/smm-skills`](https://github.com/Planable/smm-skills).

**Which one?** `ecosystem-skills` is the small, focused bundle — the three use cases the SE Ranking and Planable teams promote together: map your whole footprint, turn search demand into a social campaign, and close the gaps where AI answers ignore your brand. `marketing-skills` is the full cross-channel pipeline (gap matrices, local, PPC counterplay, agency workflows, reporting). They overlap by design and can be installed together: `ecosystem-demand-campaign` and `ecosystem-ai-visibility` are the ecosystem-branded equivalents of `marketing-seo-campaign` and `marketing-ai-campaign`, and the skill names differ so nothing collides. `content-refresh` is the standalone audit → approve → draft flow for existing pages: it diagnoses *why* each page decayed and writes surgical, fact-checked markdown drafts — no Planable, no databases, everything in chat and plain files. (For a rubric-scored batch audit with Planable re-distribution instead, that's `content-refresh-pipeline` inside `content-skills`.)

## Install — Claude Code

```bash
/plugin marketplace add seranking/marketing-automation-plugins
/plugin install marketing-skills@marketing-automation-plugins
```

The three joint ecosystem use cases:

```bash
/plugin install ecosystem-skills@marketing-automation-plugins
```

Content-only bundle:

```bash
/plugin install content-skills@marketing-automation-plugins
```

The standalone content-refresh flow:

```bash
/plugin install content-refresh@marketing-automation-plugins
```

### Claude Desktop / Cowork

1. Open **Customize** in the sidebar.
2. Click **Personal plugin** → **+ Add** → **Add marketplace**.
3. Enter `seranking/marketing-automation-plugins`.
4. Install any of `content-refresh`, `content-skills`, `ecosystem-skills`, `marketing-skills`.

## Install — Cursor

This repo is also a Cursor multi-plugin marketplace (`.cursor-plugin/marketplace.json`).

1. Add the repository as a plugin source / marketplace in Cursor Customize.
2. Install any of `content-refresh`, `content-skills`, `ecosystem-skills`, or `marketing-skills`.
3. Connect MCP servers when prompted (or via `/mcp`).

Each plugin ships `.cursor-plugin/plugin.json` plus `mcp.json` / `.mcp.json` for SE Ranking and Planable.

## Install — Codex

Repo-local Codex marketplace lives at `.agents/plugins/marketplace.json`. Each plugin has `.codex-plugin/plugin.json` pointing at `./skills/` and `./.mcp.json`.

In Codex / ChatGPT Work, add this repository as a marketplace source, then install any of `content-refresh`, `content-skills`, `ecosystem-skills`, or `marketing-skills`.

## Connect the MCP servers

Plugins auto-register the required remote MCP servers. On first use, sign in via OAuth:

- **SE Ranking** (all plugins) — `https://api.seranking.com/mcp` ([setup](https://seranking.com/api/integrations/mcp/))
- **Planable** (content-skills, ecosystem-skills, marketing-skills) — `https://mcp.planable.io/mcp` ([help](https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools))
- **Firecrawl** (content-refresh) — `https://mcp.firecrawl.dev/v2/mcp-oauth` ([docs](https://docs.firecrawl.dev/mcp-server))

`content-refresh` can also use a Google Search Console MCP for real click/impression ground truth if you run one (self-hosted or third-party) — auto-detected by capability, never required.

## Requirements

- **Agent host**: Claude (Pro/Max/Team), Cursor, or Codex with Skills/plugins support.
- **SE Ranking**: MCP access on a plan that includes the Data API (all plugins).
- **Planable**: MCP access on Team plan or higher (content-skills, ecosystem-skills, marketing-skills).
- **Firecrawl**: any plan (content-refresh only) — occasional audits fit the free tier.

## Repository layout

```text
marketing-automation-plugins/
├── .claude-plugin/marketplace.json   # Claude Code marketplace
├── .cursor-plugin/marketplace.json   # Cursor marketplace
├── .agents/plugins/marketplace.json  # Codex marketplace
└── plugins/
    ├── content-refresh/
    │   ├── .claude-plugin/plugin.json
    │   ├── .cursor-plugin/plugin.json
    │   ├── .codex-plugin/plugin.json
    │   ├── .mcp.json
    │   ├── mcp.json
    │   └── skills/
    ├── content-skills/
    │   ├── .claude-plugin/plugin.json
    │   ├── .cursor-plugin/plugin.json
    │   ├── .codex-plugin/plugin.json
    │   ├── .mcp.json
    │   ├── mcp.json
    │   └── skills/
    ├── ecosystem-skills/
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

## Links

- SE Ranking: https://seranking.com
- SE Ranking API: https://seranking.com/api.html
- SE Ranking MCP: https://seranking.com/mcp.html
- SE Ranking MCP Docs: https://seranking.com/api/integrations/mcp/
- Planable MCP: https://planable.io/mcp/
- Planable MCP Docs: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools
- Claude SEO Skills: https://seranking.com/claude-seo-skills.html
- Claude Marketing Skills:
- Claude Content Skills: 
- Claude Agent Skills: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- Claude Code Plugins: https://code.claude.com/docs/en/plugins

## License

MIT — see [content-refresh/LICENSE](plugins/content-refresh/LICENSE), [content-skills/LICENSE](plugins/content-skills/LICENSE), [ecosystem-skills/LICENSE](plugins/ecosystem-skills/LICENSE), and [marketing-skills/LICENSE](plugins/marketing-skills/LICENSE).
