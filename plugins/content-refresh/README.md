# content-refresh

A two-skill Claude plugin that audits your existing content, tells you *why* each page decayed, proposes the lightest fix that works — and only writes the refreshed draft after you approve the plan in chat.

It's a simplified, share-ready version of the internal content refresh pipeline we run at SE Ranking. The internal one spans two plugins, ~30 subagents, a Notion config database, Slack approval threads, and BigQuery checks. This version keeps the thinking and drops the infrastructure: no databases, no channels, no config files. Just chat, two connectors, and plain markdown files.

```
/refresh-audit  →  audit report + plan  →  you approve in chat  →  /refresh-update  →  refreshed drafts
```

## Before you start

This plugin needs your own accounts, connected as Claude connectors — it doesn't come with SE Ranking or Firecrawl access bundled in:

- **SE Ranking** — any plan with API/MCP access. AI-search visibility checks specifically need SE Ranking's AI Search add-on; if you don't have it, the skill just skips that one signal and says so in the report.
- **Firecrawl** — a web-scraping API. Occasional use fits in a free tier; a paid plan is worth it if you're running audits regularly.

Connect both under claude.ai → Settings → Connectors before running `/refresh-audit`.

Two things worth knowing before your first run:

- **It's not instant.** A 5-page discovery audit typically makes several dozen API calls across SE Ranking, Firecrawl, and web search, and can take a few minutes. Try `max_pages=2` or `3` on your first run before running the default (5) or the max (10).
- **It also scrapes competitors.** To diagnose competitor gaps, the audit automatically scrapes 2–3 top-ranking pages per target keyword — on every run, not just discovery mode. Only point this at domains you own or manage; it's not a tool for pulling a competitor's site at scale.

## The ideas that survived the simplification

**Diagnose before you rewrite.** Every audited page gets a primary decay cause — staleness, competitor gap, intent shift, SERP-feature loss, AI-visibility loss, cannibalization, or "never performed" — backed by evidence, not vibes.

**The 5-level update depth ladder.** `metadata_only → micro_update → section_refresh → substantial_update → full_rewrite`. The audit always recommends the *lightest* depth that fixes the diagnosis. A page that still ranks gets minimal churn, not an enthusiastic rewrite.

**Preservation locks.** The URL, the H1's target keyword, passages winning featured snippets or AI citations, working internal links, and schema markup are locked before any editing starts. The refresh can't break what's working.

**A human gate.** The audit proposes; you approve, skip pages, adjust depth, or add notes — right in the chat. Nothing is written until then.

**Fact-checked output.** Every new number in a draft traces to a real source URL, and an AI-cliché sweep runs before delivery.

**Ground truth when you have it.** SE Ranking's position and traffic data is an estimate. If you also have a Google Search Console MCP connected, the audit uses its real clicks/impressions/CTR/position for the exact page — plus an indexing-health check that can catch "this isn't a content problem, it's not indexed" before recommending a rewrite that wouldn't have helped. Fully optional: without it, everything works exactly as described above.

## Requirements

| What | Why | Where |
|---|---|---|
| SE Ranking connector (MCP) | keyword & position data, SERP analysis, question keywords, AI-search visibility, decayed-page discovery | claude.ai → Settings → Connectors |
| Firecrawl connector (MCP) | scraping your pages and the competitors' | claude.ai → Settings → Connectors |
| Google Search Console connector (MCP) — optional | real clicks/impressions/CTR/position for the exact audited URL, indexing health, cannibalization cross-check | Not in the standard connector directory yet — if you run one (self-hosted or third-party), the skill auto-detects it by capability. Without one, both skills work exactly as before |

AI-search visibility checks use SE Ranking's AI toolkit; if your plan doesn't include it, the skill simply skips that signal and says so.

## Install

**From the marketplace** (this plugin ships in [`seranking/marketing-automation-plugins`](https://github.com/seranking/marketing-automation-plugins)):

```
/plugin marketplace add seranking/marketing-automation-plugins
/plugin install content-refresh@marketing-automation-plugins
```

The two flows below are the standalone alternative — for sharing the plugin as a single file outside the marketplace:

**If you're using Claude Cowork or the Claude desktop app:** you'll have received (or downloaded) a file called `content-refresh.plugin`. Open that file in your chat — Claude shows you a preview of what's inside — then click **Accept**. The two skills below are now available in any chat: type `/refresh-audit` or `/refresh-update` to use them.

**If you're using Claude Code (the command-line tool):** download `content-refresh.plugin` and unzip it into a folder, then run these two commands, pointing the first one at that folder:

```
claude plugin marketplace add /path/to/the/unzipped/folder
claude plugin install content-refresh
```

## Quick start

```
/refresh-audit https://yourdomain.com/blog/post-a https://yourdomain.com/blog/post-b
/refresh-audit yourdomain.com                 ← discovery: finds the decayed pages for you
/refresh-audit yourdomain.com/blog max_pages=3 context="focus on our comparison posts"
```

Read the audit report, answer the approval question in chat, then:

```
/refresh-update        ← or just say "go ahead"
```

Everything lands in `content-refresh-runs/{date}-{domain}/` — the audit report, a `refresh-plan.json` (the audit↔update contract), per-page research notes with sources, and the finished drafts.

Optional: drop a `brand-voice.md` (voice notes, banned words) into your working folder and both skills will honor it. Without one, the drafts mirror the voice of each page's untouched sections.

## What this plugin deliberately doesn't do

It never publishes anywhere — the output is markdown drafts you review and paste into your CMS. It doesn't need a blog database — candidates come from your URLs or live SE Ranking data. And it won't rewrite for the sake of rewriting — "leave it alone" is a first-class recommendation.

## License

MIT. Built by Oleksii Khoroshun (SE Ranking GTM). Share it, fork it, adapt it to your stack.
