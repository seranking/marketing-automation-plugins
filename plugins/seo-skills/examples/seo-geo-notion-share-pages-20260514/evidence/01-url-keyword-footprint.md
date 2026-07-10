# 01 — URL keyword footprint (evidence)

> Source: would normally come from `DATA_getUrlOverviewWorldwide` + `DATA_getDomainKeywords(url="...")`. SE Ranking auth unavailable in this worktree.

## What this step does in a full run

For the target URL, pull:
- Total keywords the URL ranks for in the US database.
- Sum of monthly traffic across those keywords.
- Sorted list of keywords by traffic-weighted score.
- Top 5 keywords selected as the GEO investigation set.

## Inferred primary keywords

Even without SE Ranking, the page's title + meta description + H2 structure point to:

1. **publish notion page to web** (primary)
2. **make notion page public**
3. **share notion page with everyone**
4. **notion sites publish**
5. **notion website publishing**

These are the keywords the production run would feed into `DATA_getAiOverview` calls in step 02.

## URL signal summary (from Firecrawl scrape — real)

| Signal | Value |
|---|---|
| Status code | 200 |
| Title | Publish a Notion Site – Notion Help Center |
| Description | "Any page you build in Notion can be turned into a beautiful Notion Site with just a click. Publish a blog, resume or portfolio, job listing, landing page — the possibilities are as wide as the web itself." |
| H2 count | 6 |
| Hreflang alternates | 16 |
| JSON-LD blocks | 2 (Article, BreadcrumbList) |
| Canonical | self-referencing |
| og:locale | en-US |
