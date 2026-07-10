# 05 — Cited source comparison

> This section would normally enumerate the AIO-cited sources for each primary keyword and compare passage shapes against the candidate. For this example, SE Ranking AIO endpoints (`DATA_getAiOverview`, `DATA_getAiOverviewLeaderboard`) were unavailable (auth-gated in this worktree).

## What a full run produces

For each AIO query where the candidate URL is NOT cited, a production run:

1. Pulls the AIO leaderboard via `DATA_getAiOverviewLeaderboard`.
2. WebFetches the top 2–3 cited sources.
3. Extracts the cited passage (often a 100–200-word snippet that appears verbatim in the AIO answer).
4. Compares passage shape to the candidate's equivalent section.

## Structural patterns that AIO consistently cites in help-center content

These patterns are widely-documented in GEO literature; they hold across help-center pages we've audited:

| Pattern | Detail | Candidate has it? |
|---|---|---|
| **Short, definitive answer in first 100 words** | A 1–3 sentence answer to the page's primary question, placed above the fold | ✗ (opens with marketing banner) |
| **Numbered or bulleted procedure with named UI elements** | Steps that include specific button/menu names so readers can follow | ✓ (procedural sections exist; lead-paragraph problem is upstream) |
| **Date stamp (datePublished + dateModified visible)** | Both rendered on-page AND in schema | ✗ (neither visible) |
| **Author byline** | Visible byline + linked author bio | ✗ |
| **Q&A-shaped H2s with definitive answers** | The H2 reads as a question, the first paragraph answers it | ✗ (H2s are good; leads are bad) |
| **Citation-friendly stat or version reference** | E.g. "As of Notion v2.40 (Feb 2025)..." | ✗ |

## Citation-readiness check vs the rubric

5 of 6 patterns ABOVE are missing on the candidate. The biggest single fix is the question-first / answer-second structure under each H2 (covered in `04-page-passages.md` recommendations).

## What the SE Ranking call would have surfaced

Once auth is restored, expect this section to populate with:

- 5–10 AIO queries × candidate citation status (✓/✗).
- For ✗ rows: the cited source domain (commonly `notion.com/help/*` competitor articles, `youtube.com/notion`, `reddit.com/r/Notion`, and `medium.com` Notion-power-user blogs).
- Quoted cited passages — usually 80–150 words extracted from the cited URL.
- The candidate's equivalent passage — annotated with the structural gap.

## Cross-reference

For domain-level brand-vs-brand share-of-voice on Notion across all AI engines, run `seo-ai-search-share-of-voice` with target `notion.com`. That produces the macro picture; this `seo-geo` skill produces the URL-level micro picture.
