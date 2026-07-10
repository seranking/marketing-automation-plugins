# 06 — Topic ownership map

Per-cluster topic ownership across Notion's subdomain footprint. Each row identifies the canonical owner subdomain; the "Cannibalization risk" column flags whether more than one subdomain meaningfully ranks for the cluster.

| Topic cluster | Sample keywords | Canonical owner | Cannibalization risk | Notes |
|---|---|---|---|---|
| Notion product overview | "notion app", "what is notion", "notion features" | www.notion.com (root + /product/*) | None | Apex owns brand queries cleanly |
| Notion templates | "notion templates", "free notion templates", "notion crm template" | www.notion.com/templates/* | None | One big templated tier under apex |
| Notion help / support | "notion help", "how to share notion", "notion permissions" | www.notion.com/help/* | None | Old `notion.so/help/*` 301-mapped except retired slugs (404 risk on a subset) |
| Notion case studies | "notion case study", "{enterprise} notion" | www.notion.com/customers/* | None | Sales-enablement content, not high-traffic |
| Notion blog | "notion blog", topic-led pieces (productivity, AI workflows) | www.notion.com/blog/* | None | Steady editorial cadence |
| Notion release notes | "notion changelog", "notion new features" | www.notion.com/releases/* | None | Date-stamped releases |
| Notion API & SDK | "notion api", "notion sdk", "notion webhooks", "notion connection" | **developers.notion.com** | None | Clean split — different audience, different CMS |
| Notion guides for developers (MCP, integrations) | "notion mcp", "notion api integration" | developers.notion.com/guides/* | None | |
| Notion product comparisons | "notion vs evernote", "notion vs obsidian" | www.notion.com/compare-against, plus 3rd-party blogs Notion doesn't control | None internal | External competition |
| User-workspace pages (Notion Sites) | varies — user controlled | `{workspace}.notion.so/{slug}` AND `notion.com/help/public-pages-and-web-publishing` (the *explainer* for the feature) | None (clean — explainer ≠ user content) | User content is not under Notion's SEO control |

## Why no cannibalization

Notion has architecturally avoided the common SaaS-marketing trap where `blog.example.com` and `example.com/blog/` both rank for editorial content. The decision to keep marketing content all on the apex (with `/blog`, `/help`, `/customers`, `/templates` as path prefixes) means there's no internal competition.

The single architectural split (apex marketing vs `developers.notion.com`) is on different audiences (B2C/SMB vs developers) and produces zero overlap in our spot-check.

## What a full SE Ranking run would add

For each row above:
- Number of keywords each subdomain ranks for in this cluster (`DATA_getDomainKeywords` filtered to the cluster's seed terms).
- Average position per subdomain.
- Total traffic captured per subdomain for the cluster.
- The "overlap" calculation: exact keywords both subdomains rank for in top 50 — the cannibalization signal.

Today's spot-check is structural (URL patterns + content type per subdomain). The numeric layer is pending auth.
