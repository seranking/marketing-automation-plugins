# 04 — Gap raw (pre-filter)

Tool: mcp__claude_ai_SE_Ranking__DATA_getDomainKeywordsComparison · Input (run 1): domain=squarespace.com, compare=wix.com, source=us, diff=1, limit=1000. Run 2: domain=webflow.com, compare=wix.com, source=us, diff=1, limit=1000.

Status: SE Ranking MCP auth unavailable. Re-run with auth to populate the full pre-filter gap list (typically 1,000+ keywords across the two runs).

Qualitative gap themes confirmed by live Firecrawl SERP probes on 2026-05-14:

Theme 1 — Agency / professional intent (Webflow owns; Wix Studio invisible):
- "website builder for agencies" -> Webflow #1
- "best website platform for agencies" (likely; not probed)
- "white label website builder" (likely; not probed)
- Webflow's URL template: /solutions/agencies, /enterprise

Theme 2 — Comparison / alternatives intent (Webflow + Squarespace both own):
- "webflow vs wix" -> Webflow /vs/wix at #3 (Wix domain absent from top 10)
- "wix vs squarespace" -> [pending probe; pattern likely matches]
- "wix alternatives" -> [pending probe]
- "squarespace alternatives" -> [pending probe]

Theme 3 — Pricing-page longtail (Squarespace owns):
- "squarespace pricing" -> Squarespace /pricing at #1
- "squarespace plans" / "squarespace fees" (likely; not probed)

Filtering decisions for the final REPORT.md table:
- Drop keywords where target_position is already in top 20 (Wix already has presence).
- Drop branded competitor keywords ("squarespace login", "webflow account") — those are not real opportunities.
- Keep cross-brand comparison keywords ("webflow vs wix") because owning the narrative on Wix's own brand-comparison SERP is defensible.
- Filter to volume >= 500 and KD <= 60 for the priority list — adjust per user constraints.
