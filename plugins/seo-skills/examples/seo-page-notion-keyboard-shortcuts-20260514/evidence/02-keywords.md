# 02 — Ranking keywords

Tool: `mcp__claude_ai_SE_Ranking__DATA_getDomainKeywords` · Input: `url=https://www.notion.com/help/keyboard-shortcuts`, `source=us`, `limit=1000`, `order_field=position`, `order_type=asc`

**Status:** SE Ranking MCP authentication unavailable in this session. Re-run with auth to populate.

Live cross-check (via `mcp__firecrawl-mcp__firecrawl_search` on Google US, 2026-05-14):
- "notion keyboard shortcuts" → position 1
- "notion shortcuts mac" → position 1
- "notion hotkeys" → position 1
- "notion shortcuts cheat sheet" → not in top 5

Expected SE Ranking shape (per SKILL.md step 3):
```
keyword,position,volume,kd,intent,cpc,traffic_estimate
notion keyboard shortcuts,1,<volume>,<kd>,I,<cpc>,<traffic>
notion shortcuts mac,1,<volume>,<kd>,I,<cpc>,<traffic>
notion hotkeys,1,<volume>,<kd>,I,<cpc>,<traffic>
...
```

CTR curve to apply when SE Ranking returns volume (per SKILL.md): 1=28%, 2=15%, 3=11%, 4=8%, 5=7%, 6=5%, 7=4%, 8=3%, 9=2%, 10=2%, 11+=1%.

The traffic-weighted ranking on a position-1 keyword is roughly `volume × 0.28`. If "notion keyboard shortcuts" has even 10K monthly volume, this single keyword captures ~2,800 monthly clicks — the page is a substantial asset on Notion's help-domain footprint.
