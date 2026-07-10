# 04 — Per-subdomain competitors

> Source: `DATA_getDomainCompetitors(domain="<subdomain>")` per subdomain. SE Ranking auth required.

## What a full run captures

For each subdomain, the top organic competitors by `common_keywords`. The signal:

- **Different competitor sets across subdomains** → legitimate separate scopes (sign of healthy split).
- **Same competitor sets across subdomains** → redundant scopes (sign of cannibalization).

## Expected competitor sets (qualitative)

| Subdomain | Expected top competitors | Why |
|---|---|---|
| `www.notion.com` | obsidian.md, evernote.com, clickup.com, monday.com, asana.com, coda.io, airtable.com | Notion's marketing apex competes with productivity / workspace / project-management SaaS |
| `developers.notion.com` | airtable.com/api, asana.com/developers, slack.com/api, atlassian.com/software/jira/api, stripe.com/docs (as a docs benchmark) | API documentation competitors are a different cluster — they're competing on developer adoption signals |
| `www.notion.so` | (declining set — gradually emptying as 301s consolidate signals) | Legacy domain |

## Confirmation signal

Different competitor sets across the two SEO-active subdomains (`www.notion.com` vs `developers.notion.com`) would confirm the split is healthy (different audiences ranking against different competitors). Shared sets would flag redundant scope.

We expect **distinct sets** based on URL pattern + content type analysis already done — confirmation gated on SE Ranking auth.
