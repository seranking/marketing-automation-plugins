# 07 — AI-protocol files (evidence)

> Source: would come from `mcp__firecrawl-mcp__firecrawl_scrape` of `notion.com/llms.txt` and `notion.com/.well-known/rsl.json`. Not run for this example to stay within credit budget; would add 2 Firecrawl credits.

## What a full run captures

| File | Endpoint | Expected outputs |
|---|---|---|
| `llms.txt` | `https://www.notion.com/llms.txt` | HTTP status, full body, parsed allow/deny scope, content categories declared, attribution requirements |
| RSL JSON | `https://www.notion.com/.well-known/rsl.json` | HTTP status, JSON parsed, content allow/deny scope, licensing terms |
| Legacy RSL | `https://www.notion.com/RSL.txt` | Same — fallback for older RSL deployments |

## Why this matters for GEO

LLM citation rates correlate with the publisher's explicit stance:

| Stance | Effect on AIO citation rates |
|---|---|
| Present + permissive (`Allow: *` or category-allowlist) | Highest — LLMs preferentially cite content with explicit permission |
| Present + restrictive (specific deny) | Citations honour the scope; AIO will work around restricted sections |
| Absent | Default behaviour — depends on each LLM's own crawl policy |

Notion's overall public stance is permissive in spirit (help content is openly indexed; Notion runs marketing campaigns around AI integrations). The protocol-file declaration is the explicit signal Google + OpenAI + Anthropic look for when deciding which passages are safe to cite verbatim.

## Pending check

Run `mcp__firecrawl-mcp__firecrawl_scrape(url="https://www.notion.com/llms.txt")` and `mcp__firecrawl-mcp__firecrawl_scrape(url="https://www.notion.com/.well-known/rsl.json")` to populate this section in a follow-up. 2 Firecrawl credits.
