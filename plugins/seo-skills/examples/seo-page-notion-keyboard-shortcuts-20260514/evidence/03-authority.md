# 03 — Page authority + history

Tools: `mcp__claude_ai_SE_Ranking__DATA_getPageAuthority`, `mcp__claude_ai_SE_Ranking__DATA_getPageAuthorityHistory` · Input: `url=https://www.notion.com/help/keyboard-shortcuts`

**Status:** SE Ranking MCP authentication unavailable in this session. Re-run with auth to populate.

Domain context (qualitative, no SE Ranking pull): notion.com is a high-authority SaaS workspace domain with extensive editorial + help content. The help subpath `/help/` is the canonical reference path. Page authority on `/help/keyboard-shortcuts` should track domain authority closely.

Expected SE Ranking shape:
- `DATA_getPageAuthority` returns the current PA + inlink_rank score for the URL.
- `DATA_getPageAuthorityHistory` returns a 12-month time-series. **Watch for the all-zeros caveat (see SKILL.md Tips):** on high-authority pages the history can return flat zero for some metrics — treat as "insufficient history" rather than as a real regression.

Verdict-heuristic dependency: REFRESH hardens when PA dropped >5 in 90 days. KEEP requires stable or rising PA. We can't compute this delta without the historical data — the REFRESH verdict in PAGE.md is being driven by the `noindex` + malformed `og:image` + cheat-sheet-longtail gap, not by a PA drop. PA + history would be a corroborating data point.
