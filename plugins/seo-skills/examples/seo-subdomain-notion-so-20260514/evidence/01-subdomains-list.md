# 01 — Subdomains discovered

> Source: `mcp__firecrawl-mcp__firecrawl_map(url="https://notion.com", includeSubdomains=true)` + WebFetch spot-checks on 2026-05-14. **Evidence is real.**

## First-party subdomains found

| Subdomain | Resolved | Role |
|---|---|---|
| `www.notion.com` | 200 | Marketing apex (post-migration canonical) |
| `www.notion.so` | 301 → `www.notion.com` | Legacy apex; sitewide redirect; user-workspace pages live under this host as path-based URLs (`/{workspace}/{slug}`) |
| `developers.notion.com` | 200 | API/SDK + integration documentation hub (separate CMS — content covers connections, REST API, webhooks, SDK) |
| `sitemaps.notion.com` | 200 (per robots.txt declaration) | Operational — hosts category/collection sitemaps for crawler consumption |
| `app.notion.com` | 200 (operational; serves user pages at `/p/{workspace}/{slug}`) | Workspace runtime; not a marketing/SEO surface |

## URL pattern: user-workspace SEO leakage

User-published Notion Sites surface on **two patterns**, both technically discoverable by search engines:

1. `https://www.notion.so/{workspace}/{slug}` — older pattern; many active examples found in the Firecrawl map.
2. `https://app.notion.com/p/{workspace}/{slug}` — newer pattern (the `/p/` prefix).

Examples surfaced in this run:

- `https://www.notion.so/izaakigwdesign/Arkell-and-Co-Socials-Portal-299c6bc3faed8031beffc42951ea8e77`
- `https://www.notion.so/qonto/What-is-Product-Discovery-10231ee4c696804bbf5bcccd383329fc`
- `https://www.notion.so/beapp/CVSC-Ehpad-2c30bf59034f802b998cd34093fec473`
- `https://www.notion.so/cognituv/Upfield-Canada-Inc-f5fc60b1e4f5435b8467fea01359a72e`
- `https://app.notion.com/p/nosily/0ef5afc5f5734e2c90b68940e3bde60a`

These are user-controlled content, not Notion-first-party SEO.

## Subdomains NOT used

Notable absences (each typical of competing SaaS architectures):

- `blog.notion.com` — does not exist; blog is on the apex (`notion.com/blog`)
- `docs.notion.com` — does not exist (help is on apex; API docs are on `developers.notion.com`)
- `support.notion.com` — does not exist (help is the support surface)
- `status.notion.com` — exists but not surfaced via the homepage link graph in this run
- `community.notion.com` — does not exist (community lives on third-party platforms like Reddit r/Notion)

## Robots.txt sitemap declarations (subdomain-relevant)

From the user-confirmed `notion.com/robots.txt` summary earlier in this session:

> "Primary domain sitemaps: Main sitemap at /sitemap.xml; Blog, customers, help, markdown, pages, product, releases, and use-case sitemaps.
> Secondary domain sitemaps: Category and collection sitemaps hosted at sitemaps.notion.com."

This confirms `sitemaps.notion.com` is a deliberate first-party operational subdomain.

## What this evidence supports

The "Subdomain inventory" table in `SUBDOMAINS.md` is structurally correct: 5 first-party subdomains identified, plus the user-workspace patterns. The metric cells (keywords, traffic, DA, backlinks) require SE Ranking authenticated calls per subdomain (`DATA_getDomainSubdomains`, `DATA_getDomainOverviewWorldwide`, `DATA_getBacklinksSummary`).
