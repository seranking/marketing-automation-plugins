# 05 — HTML extracts (Firecrawl + WebFetch)

Tools: `mcp__firecrawl-mcp__firecrawl_scrape` (formats=["json"], jsonOptions extracting head metadata) + `WebFetch` (markdown extraction) · Input: `url=https://www.notion.so/help/keyboard-shortcuts` (302s to `https://www.notion.com/help/keyboard-shortcuts`)

Run timestamp: 2026-05-14 (Firecrawl cache hit; original capture 2026-05-14T06:17:58Z).

## Head metadata (Firecrawl rawHtml-extracted JSON)

```
title              : "Keyboard shortcuts – Notion Help Center"
og:title           : "Keyboard shortcuts – Notion Help Center"
og:description     : "Notion is fully equipped with keyboard and Markdown shortcuts so you can do everything you need without ever touching your mouse"
og:image           : "https://www.notion.comhttps://img.youtube.com/vi/cBdyHp_XVFQ/maxresdefault.jpg"
                     ^^^^ malformed — concatenation bug between Notion's domain and the embedded YouTube thumbnail URL
og:url             : "https://www.notion.com/help/keyboard-shortcuts"
og:type            : "website"
og:site_name       : "Notion"
og:locale          : "en-US"
twitter:card       : "summary_large_image"
twitter:title      : "Keyboard shortcuts – Notion Help Center"
twitter:image      : "https://www.notion.comhttps://img.youtube.com/vi/cBdyHp_XVFQ/maxresdefault.jpg"
                     ^^^^ same malformation as og:image — Twitter shares are also broken
twitter:site       : "@NotionHQ"
description        : (no <meta name="description"> — only OG description present)
language           : "en-us"
robots             : "noindex, follow"
                     ^^^^ P0 issue: page is the #1 organic result and is requesting no indexing
canonical          : not exposed in the JSON metadata pull (SPA may inject client-side)
hreflang           : 0 alternates declared
JSON-LD            : 0 blocks detected in rawHtml
viewport           : "width=device-width, initial-scale=1"
naver-site-verification: ad5c9c7e4607a86395226000f5fe1e4223ce3348 (Naver Webmaster Tools)
next-head-count    : 44 (Next.js SPA — head emits client-side, increases stale-tag risk for Googlebot)
```

The duplicated entries in the Firecrawl raw output (e.g. two `twitter:card`, two `og:title`) are the Notion page emitting a global site-level head plus a page-specific head. Both are visible to Googlebot.

## Body structure (WebFetch-extracted markdown)

H1 (1): `Keyboard shortcuts`

H2 (selected, in document order):
- Most popular
- Desktop keyboard shortcuts
- Markdown style
- Create and style your content
- Edit and move blocks
- @ commands
- [[ commands
- + commands
- Tip (callout)
- Note (callout)
- Slash commands (Basic, Inline, Media, Advanced sub-groups)
- Contents
- Was this resource helpful?
- Up next: Use pages offline

Body characteristics (WebFetch estimates):
- Word count: ~3,200–3,500
- Internal links: ~95+ (sidebar, in-line, "Up next")
- External links: ~8–10 (YouTube, social)
- Images: ~10–12 (illustrations, callout graphics)
- Code blocks: present (keyboard notation inline)
- Tables: implicit groupings, not real `<table>` elements

## Sense-check (per SKILL.md step 6 final bullet)

Does the page talk about its top-ranking keyword in title and H1? **Yes** — title and H1 both read "Keyboard shortcuts." No consolidation signal from text-vs-ranking mismatch.

## Critical flags

1. **`robots: noindex, follow`** — page should NOT be indexed per its own declaration, but lives at organic position 1 (live SERP probe). Page-owner decision required.
2. **`og:image` URL malformed** — silent social-share breakage. Same bug on `twitter:image`. Trivial fix.
3. **0 JSON-LD blocks** — no structured-data signals at all. `HowTo`, `FAQ`, or `Table` schema would unlock rich-result eligibility and feed LLMs a machine-readable view.
4. **No `<meta name="description">`** — Google may auto-generate the SERP snippet from page content (functional today) but a hand-tuned description would improve CTR.
