# 07 — Fragmentation flags

## ⚠ Migration in flight: `notion.so` → `notion.com`

This is the dominant signal in Notion's subdomain landscape today. Mechanics:

- `https://www.notion.so/` → HTTP 301 → `https://www.notion.com/` ✓
- Verified via WebFetch and Firecrawl scrape on 2026-05-14.
- Path-level 301 mapping is *mostly* in place: many old `notion.so/help/...` URLs 301 cleanly to the new `notion.com/help/...` equivalent.

**Exception caught during data collection:**

- `https://www.notion.so/help/guides/share-pages-with-anyone-using-public-link` (the user-supplied target URL for our `seo-geo` example) returns 404 on `notion.com`. The slug was retired in the migration, replaced by `notion.com/help/public-pages-and-web-publishing`.

That single observation extrapolates to a question: how many other old `/help/guides/*` slugs are also retired without 301-mapping to their successors? An audit of:

```
DATA_listAudits(domain="notion.so")
DATA_listAudits(domain="notion.com")
```

cross-referenced against `DATA_getCrawledPages` for each, would surface every old slug that 404s today.

**Recommendation:** run `seo-drift baseline` on `notion.com` now, then `compare` after 60 days to ensure the URL inventory and traffic stabilise. Pair with a manual review of all `notion.so/help/guides/*` slugs in the SE Ranking historical keyword database — these are the highest-link-equity loss-risks.

## User-workspace SEO leakage on `*.notion.so`

A non-trivial slice of the `notion.so` URL space is **user-workspace public pages** — Notion Sites built by Notion's customers and published under `{workspace}.notion.so/{slug}`. From Firecrawl's homepage map (`includeSubdomains: true`), examples surfaced:

```
https://www.notion.so/izaakigwdesign/Arkell-and-Co-Socials-Portal-...
https://www.notion.so/Where-to-Put-New-Things-Routing-Guide-...
https://www.notion.so/Engineering-Hub-Navigation-Index-...
https://app.notion.com/p/nosily/0ef5afc5f5734e2c90b68940e3bde60a
```

These pages:

1. **Are user-controlled.** Notion doesn't decide their content, can't optimise their schema, and shouldn't get credit/blame for their rankings.
2. **Use multiple subdomain patterns.** Some are `www.notion.so/{workspace}/...`, some are `app.notion.com/p/{workspace}/...`. The pattern depends on how the user published — `/p/` is the user-page-by-id path, `/{workspace}` is the custom-workspace pattern.
3. **Can dilute brand attribution in SERPs.** Search-results title-tag formatting often shows the page title alongside "| Notion", which technically credits the brand for content the brand didn't author.

This is **intentional product design**, not a fragmentation problem to consolidate. Notion Sites is the feature (covered in `notion.com/help/public-pages-and-web-publishing`). Users can opt out of search-engine indexing per Site.

**Not flagged for consolidation.** Just documented as a brand dynamic.

## No traditional `blog.notion.com` / `docs.notion.com`

Many SaaS marketing sites split blog and docs onto subdomains. Notion does NOT — all marketing content lives on the apex (`/blog/*`, `/help/*`, `/customers/*`, `/templates/*`, `/releases/*`). The only marketing-adjacent subdomain is `developers.notion.com`, which legitimately serves a different audience.

This is a healthy architectural choice. Subdomain blog-vs-root cannibalization is one of the most common SEO issues we surface in other audits; Notion sidesteps it entirely.

## Recommendations

1. **Audit `notion.so/help/guides/*` 301-coverage.** The one 404 found in data collection is plausibly representative of a larger pattern.
2. **Run `seo-drift baseline` on `notion.com`** to lock the post-migration URL inventory.
3. **Leave the first-party subdomain split as-is.** No consolidation warranted.
