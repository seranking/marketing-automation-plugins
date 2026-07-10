# 02 — AIO presence per keyword (evidence)

> Source: would come from `DATA_getAiOverview(target=URL, source=us)` per keyword. SE Ranking auth unavailable in this worktree.

## What a full run captures

For each of the top 5 keywords from step 01:
- Is an AIO present on the SERP?
- Is the candidate URL cited within that AIO?
- The full AIO answer text (this reveals what passage shape Google's models preferred).
- The list of cited sources.

## Schema of the captured data (so you know what's expected)

```jsonc
{
  "keyword": "publish notion page to web",
  "aio_present": true,
  "aio_answer_text": "[...]",
  "candidate_cited": false,
  "cited_sources": [
    {"url": "https://www.notion.com/help/...", "passage": "..."},
    {"url": "https://www.youtube.com/watch?v=...", "passage": "(from video transcript)"},
    {"url": "https://www.reddit.com/r/Notion/comments/...", "passage": "..."}
  ]
}
```

## Why the AIO answer text is the gold

AIO answers are short — 100–250 words usually. They are constructed from cited passages. The shape of the AIO answer reveals exactly what citable passage shape Google's models prefer for that intent. If the candidate URL doesn't emit that shape, no citation.

For the candidate URL (`/help/public-pages-and-web-publishing`), the page's passage scores (see `04-page-passages.md`) suggest that for the procedural intent "publish notion page to web", citation likelihood is low until H2 2's video-first lead is rewritten into a 1–3 sentence procedural answer.
