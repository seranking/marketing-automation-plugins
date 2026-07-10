# 01 — Competitor discovery

Tool: mcp__claude_ai_SE_Ranking__DATA_getDomainCompetitors · Input: domain=wix.com, source=us

Status: SE Ranking MCP auth unavailable in this session. Re-run with auth to populate the full competitor list.

User-supplied competitors for this run: squarespace.com, webflow.com (the two direct design-led / no-code competitors of Wix in the website-builder category, per the existing Wix share-of-voice example at examples/seo-ai-search-share-of-voice-wix-com-20260427/REPORT.md).

Expected SE Ranking shape (per SKILL.md step 1):
- Returns up to 500 competing domains pre-sorted by shared keyword count.
- Each row: common_keywords, domain_relevance, total_keywords, missing_keywords, traffic_sum, price_sum.
- For a full programmatic run on wix.com, top 5 by common_keywords typically: weebly.com, squarespace.com, hostinger.com, godaddy.com, webflow.com — confirmed against the share-of-voice example's competitor leaderboard.

This example narrows to 2 (squarespace + webflow) to keep the deliverable small. A production run would use 3–5.
