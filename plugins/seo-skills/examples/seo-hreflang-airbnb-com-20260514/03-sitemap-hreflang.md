# 03 — Sitemap-level hreflang (deferred)

This skill checks structural-and-consistency only at the sitemap level; full sitemap-vs-audit work belongs to `seo-sitemap`.

For Airbnb the key question is: **does the sitemap declare `xhtml:link rel="alternate" hreflang="x-default"`** even though no page-level HTML emits one? If yes, Google reads both — the per-URL HTML fix becomes lower priority. If no, the page-level HTML is the only source of truth and the fix priority is High (as set).

A deferred check via WebFetch was attempted; airbnb.com/robots.txt was blocked by their WAF in this run. A production run with a sitemap-friendly proxy (or via Firecrawl) would fetch:

1. `https://www.airbnb.com/robots.txt` to discover the sitemap entry point.
2. The sitemap index (likely sharded — Airbnb publishes per-locale child sitemaps).
3. A sample of 1–2 child sitemaps to confirm whether `xhtml:link` entries include `x-default`.

## Pending

Run `seo-sitemap` on airbnb.com to populate this section.
