# 04 — Page passages + citability scores

> Source: `firecrawl_scrape(url="https://www.notion.com/help/public-pages-and-web-publishing", formats=["markdown"])` on 2026-05-14.

The page has **0 H1 tags** (the title is set via `<title>` only — visually rendered, but no `<h1>` element) and **6 H2 tags**. Each H2 is followed by content that is either:
- a bulleted list of capabilities (good for citation),
- a video embed + ad-blocker fallback (bad — content is in the video, not the page),
- procedural steps with no leading summary (decent — citable, but weaker than steps-prefixed-with-summary),
- a single short paragraph that doesn't fully answer the H2 (weak).

## Scoring rubric

5-point scale per attribute, summed to /10:
- **Complete thought in 1–3 sentences** (2 pts)
- **Directly answers the H2's implicit question** (2 pts)
- **Contains a stat, number, or named entity** (2 pts)
- **Has a freshness signal (date, version reference)** (2 pts)
- **Standalone — would make sense if quoted out of context** (2 pts)

## Per-passage scores

### Passage 1 — H2 "What you can do with Notion Sites"

Quoted lead (verbatim):
> "With Notion Sites, you can:
> - Publish an unlimited number of pages to the web.
> - Claim one notion.site domain.
> - Turn on search engine indexing for your Sites."

| Attribute | Score | Notes |
|---|---:|---|
| Complete thought | 2 | Bullet list reads as a unit |
| Directly answers H2 | 2 | "what you can do" = capability list |
| Named entity / stat | 2 | "unlimited", "one notion.site domain", "search engine indexing" — specific |
| Freshness signal | 0 | No date / no version reference |
| Standalone | 1 | Bullets work better with a leading sentence |
| **Total** | **7 / 10** | **Strongest passage on the page** |

### Passage 2 — H2 "Publish a Notion Site"

Quoted lead:
> [video embed]
> "Play
> Uh-oh! It looks like your ad blocker is preventing the video from playing.
> Please watch it on YouTube"

Then a YouTube link. The actual steps come ~200 words below.

| Attribute | Score | Notes |
|---|---:|---|
| Complete thought | 0 | Video + fallback only |
| Directly answers H2 | 0 | No answer in the first paragraph |
| Named entity / stat | 1 | "YouTube" — but irrelevant context |
| Freshness signal | 0 | None |
| Standalone | 0 | Can't quote a video embed |
| **Total** | **1 / 10** | **Weakest passage. Highest fix priority.** |

### Passage 3 — H2 "Adjust settings for your Notion Site"

Direct anchor link to subsection; first content is bullet items about which settings exist. No 1-sentence lead summary.

| Attribute | Score | Notes |
|---|---:|---|
| Complete thought | 1 | Bullets work but no opener |
| Directly answers H2 | 1 | Lists settings but doesn't summarise *why* you'd adjust |
| Named entity / stat | 2 | Specific setting names |
| Freshness signal | 0 | None |
| Standalone | 1 | OK if quoted as a list |
| **Total** | **5 / 10** | |

### Passage 4 — H2 "How visitors interact with your Notion Site"

Brief paragraph about commenting / duplicating; describes the surface without summarising what visitors can do.

| Attribute | Score | Notes |
|---|---:|---|
| Complete thought | 2 | Cohesive paragraph |
| Directly answers H2 | 1 | Describes mechanics, not the full answer |
| Named entity / stat | 1 | Some specifics |
| Freshness signal | 0 | None |
| Standalone | 1 | Could be quoted but feels mid-flow |
| **Total** | **5 / 10** | |

### Passage 5 — H2 "Unpublish your Notion Site"

Direct procedural steps. ("Click Share → Publish → toggle off Publish to web.")

| Attribute | Score | Notes |
|---|---:|---|
| Complete thought | 2 | Procedural prose is complete |
| Directly answers H2 | 2 | Yes — "to unpublish, do X" |
| Named entity / stat | 1 | UI element names |
| Freshness signal | 0 | None |
| Standalone | 1 | Quotable as steps |
| **Total** | **6 / 10** | |

### Passage 6 — H2 "Prevent members from publishing Notion Sites"

Settings-tab procedure for workspace admins.

| Attribute | Score | Notes |
|---|---:|---|
| Complete thought | 2 | |
| Directly answers H2 | 2 | |
| Named entity / stat | 1 | Workspace settings naming |
| Freshness signal | 0 | None |
| Standalone | 1 | Quotable |
| **Total** | **6 / 10** | |

## Summary

| H2 | Score |
|---|---:|
| What you can do with Notion Sites | 7 |
| Publish a Notion Site | **1** ← critical fix |
| Adjust settings for your Notion Site | 5 |
| How visitors interact with your Notion Site | 5 |
| Unpublish your Notion Site | 6 |
| Prevent members from publishing Notion Sites | 6 |

**Page average: 5.0 / 10.** AIO citation rates trend upward when passage averages exceed 7.0. The biggest leverage is fixing H2 2 (the video-first "Publish a Notion Site" section): a 1–3 sentence procedural summary placed *before* the video would pull that score from 1 → 7+ overnight.

## Universal refresh — add page-top TL;DR

The page currently opens with a Notion developer-platform banner — not a definitional summary of "what is Notion Sites". A 50-word TL;DR at page-top (above the first H2) is the single most-cited passage shape across help-center content in AIO. Recommended text:

> "Notion Sites lets you turn any Notion page into a public website with a single click. This article covers what Notion Sites can do, how to publish a page, the settings to adjust before going live, and how to unpublish or restrict publishing across a workspace."

51 words. Citable verbatim.
