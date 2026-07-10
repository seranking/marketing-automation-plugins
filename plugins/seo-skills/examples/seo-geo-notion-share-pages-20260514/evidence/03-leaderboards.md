# 03 — AIO leaderboards (evidence)

> Source: would come from `DATA_getAiOverviewLeaderboard(target=URL, source=us)` per keyword. SE Ranking auth unavailable in this worktree.

## What a full run captures

For each keyword from step 01, the full ranked list of cited sources within the AIO. This is what reveals:

- **Domain-level patterns:** which sites are consistently cited across the cluster? (Often `notion.com/help/*` itself for navigational queries, plus `medium.com` and `reddit.com/r/Notion` for procedural/troubleshooting.)
- **Passage-level patterns:** what structure earns citation? (Usually short answer → procedural list → caveat.)

## Schema

```jsonc
{
  "keyword": "publish notion page to web",
  "aio_leaderboard": [
    {"rank": 1, "url": "...", "domain": "...", "passage_snippet": "..."},
    {"rank": 2, "url": "...", "domain": "...", "passage_snippet": "..."},
    ...
  ]
}
```

## Inferred competitor citers (typical pattern for Notion help queries)

Without the live data, the typical cited-source pattern for Notion procedural queries is:

| Likely citer | Why it's typically cited |
|---|---|
| `notion.com/help/*` — sibling articles | Help-center articles are first-party authority |
| `medium.com` Notion power-user blogs | Author-bylined long-form with practical detail |
| `reddit.com/r/Notion` | Long-tail troubleshooting answers |
| `youtube.com` Notion creator tutorials | Visual procedural content with transcripts AI Overview can extract |
| `notion.com/blog/*` | Notion-first-party but more narrative |

A production run replaces this with real ranked rows.
