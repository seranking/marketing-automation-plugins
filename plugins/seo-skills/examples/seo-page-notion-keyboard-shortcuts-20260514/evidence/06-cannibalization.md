# 06 — Same-domain cannibalization

Tool: `mcp__claude_ai_SE_Ranking__DATA_getDomainPages` · Input: `domain=notion.com`, ranked by traffic, cap 50; cross-check candidate URL's top-3 keywords against the peer-page list.

**Status:** SE Ranking MCP authentication unavailable in this session. Re-run with auth to populate the full peer-page sweep.

Manual SERP probe (`mcp__firecrawl-mcp__firecrawl_search`, query "notion keyboard shortcuts", US, 2026-05-14) surfaced one peer-domain URL in the top 10:

| Position | Peer URL | Keyword overlap |
|---|---|---|
| 9 | `https://www.notion.com/help/notion-calendar-keyboard-shortcuts` | "notion keyboard shortcuts" (the head query) |

This is a **cannibalization signal** per SKILL.md step 8 — a peer URL on the same domain ranking ≤ 20 for one of the candidate's top-3 keywords. The candidate ranks at position 1 (still wins the SERP) and the peer at position 9 captures a much smaller share, so the recommendation is **disambiguate, not consolidate**:

- **Action:** ensure the peer URL's `<title>` and H1 lead with "Notion Calendar" not "Notion" (live capture today: title is "Notion Calendar keyboard shortcuts – Notion Help Center" — already correct). Confirm no other cross-linking is funneling generic "notion keyboard shortcuts" anchor text to the calendar page. Otherwise leave the peer as-is.

A full peer-page sweep (50 top-traffic pages on notion.com) would confirm there are no additional cannibalization sources. Re-run with SE Ranking auth for that.
