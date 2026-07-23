---
name: marketing-gap-matrix
description: >
  Cross-channel competitor gap matrix. Compares a target domain to its organic competitors
  (SE Ranking), surfaces the keywords they rank for that the target does not — then checks every
  gap against what the brand has already covered on its site and in its social history (Planable)
  and routes each opportunity: site content, social content, both, or skip. Use whenever the user
  asks "where are our gaps across site and social", "what do competitors own that we haven't
  covered anywhere", "route our keyword opportunities", "cross-channel gap analysis", "build a
  content backlog across channels", or wants competitor gaps turned into a routed, prioritized
  backlog rather than a keyword list. Requires both the SE Ranking and Planable MCPs. For a pure
  single-channel keyword gap without coverage routing, use seo-competitor-gap-analysis from the
  seo-skills plugin instead.
---

# Cross-channel gap matrix

Identify the keywords competitors rank for that the target does not — then, the part a plain gap analysis can't do: check each gap against the brand's existing site coverage **and** social post history, and route every opportunity to the channel where it should be closed.

## Prerequisites

- **SE Ranking MCP** connected.
- **Planable MCP** connected, with the brand's workspace (for the social-coverage check).
- User provides: (a) target domain, (b) 3 to 5 competitor domains (or ask the skill to auto-discover them), (c) market country (default: `us`), (d) the Planable workspace, and optionally filters (min volume, max KD, intent).

## Connector health check

Verify both MCPs before starting: `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to reconnect it (SE Ranking guide: https://seranking.com/api/integrations/mcp/ · Planable guide: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools).

## Process

1. **Validate or discover competitors** `DATA_getDomainCompetitors`
   - If the user did not provide competitors, pull the top 5 organic competitors for the target in the target market.
   - Surface the list to the user and ask them to confirm or override before proceeding.
   - **Note:** the upstream API does not support `limit`/`offset`, so this call returns the full set (~60KB for popular domains) and the MCP harness writes it to a file. Read that file path, parse the `{data: [...]}` JSON, sort by `common_keywords` desc, and take the top 5.

2. **Pull competitor keyword sets** `DATA_getDomainKeywords`
   - For each competitor, pull keywords where they rank in the top 20 of the target country.
   - Save per-competitor lists.

3. **Pull target keyword set** `DATA_getDomainKeywords`
   - For the target domain, pull all ranking keywords in the target country (any position).
   - This is both the exclusion set and the **site-coverage evidence**: a gap keyword where the target ranks nowhere at all is "site: not covered"; position 21–50 is "site: weak" (a page exists but loses).

4. **Compute the gap** `DATA_getDomainKeywordsComparison` (cross-check)
   - Keywords ranked by at least one competitor in the top 20 but not ranked by the target domain at all.
   - Use the comparison endpoint as a cross-check.

5. **Filter and segment**
   - Apply user-specified filters on volume, KD, and intent. Judge relevance, not just numbers — discard off-brand junk (glossary terms, follower-farming phrases, unrelated tangents) a domain has no business targeting.
   - Segment by intent: informational, commercial, transactional, navigational.
   - Segment by competition: how many of the N competitors rank for each gap keyword.

6. **Social-coverage check (signature step)**
   - Pull the brand's recent social history: `list_pages(workspaceId)`, then `list_posts(workspaceId, ...)` for the last ~90 days (published + scheduled). Page through until you have the period covered.
   - Cluster the filtered gap keywords into themes (a theme = a keyword family, not one term). For each theme, scan post texts for topical matches (terms, synonyms, obvious paraphrases — judge meaning, not substrings).
   - Mark each theme: **social: covered** (≥2 recent posts on the theme), **social: thin** (1 post), **social: not covered**.

7. **Route every theme (the decision, not just the data)**
   - `both` — no site page, no social coverage, informational/commercial demand worth owning everywhere.
   - `site` — commercial/transactional term needing a page; or social already covers it but no page captures the demand.
   - `social` — conversational/question-shaped demand a post can answer fast; or a site page exists (even weak) but social is silent while competitors talk.
   - `skip` — off-brand, branded-competitor, or vanity volume. Say why.
   - One-line rationale per routing decision, tied to the data.

## Output format

Create a folder `marketing-gap-matrix-{target-slug}-{YYYYMMDD}/` with:

```
marketing-gap-matrix-{target-slug}-{YYYYMMDD}/
├── REPORT.md          (the routed matrix — primary deliverable)
├── gaps.csv           (full gap list with coverage + route columns)
└── evidence/
    ├── 01-competitors.md
    ├── 02-competitor-keywords-{domain}.md
    ├── 03-target-keywords.md
    ├── 04-gap-raw.md
    └── 05-social-history-scan.md   (themes vs posts matched — so routing is auditable)
```

`REPORT.md` centrepiece — the matrix:

```markdown
# Cross-channel gap matrix: {target}
Market: {country} · Competitors: {list} · Social history window: {dates}

## The matrix (top themes)

| # | Theme (top keyword) | Volume | KD | Comps ranking | Site | Social | Route | Why |
|---|---|---|---|---|---|---|---|---|
| 1 | {kw} | {n} | {n} | 4 of 5 | not covered | not covered | **both** | {one line} |
| 2 | {kw} | {n} | {n} | 3 of 5 | weak (#34) | covered | **site** | {one line} |
| 3 | {kw} | {n} | {n} | 2 of 5 | covered (#8) | not covered | **social** | {one line} |

## Routed backlogs
### → Site ({n} themes) — hand to a content brief
### → Social ({n} themes) — hand to marketing-seo-campaign
### → Both ({n} themes)
### → Skipped ({n}) — with reasons

## Recommended next steps
1. Launch `marketing-seo-campaign` on the social/both routes (it will label and baseline the campaign).
2. Brief the site routes (seo-skills `seo-content-brief`, or marketing-content-brief for brief + distribution in one).
3. Re-run this matrix quarterly; compare coverage columns to see gaps actually closing.
```

`gaps.csv` columns:
`theme,keyword,volume,kd,cpc,intent,competitors_ranking,top_competitor_position,target_position,site_coverage,social_coverage,route,rationale`

## Tips

- Data API rate limit: 10 requests per second. For large sites, `DATA_getDomainKeywords` may paginate heavily; set a ceiling (e.g., top 1,000 keywords per domain) unless the user explicitly asks for the full set.
- Check `DATA_getSubscription` for remaining units before a large pass.
- The `competitors_ranking` count is the best signal of realism: 4-of-5 keywords are validated demand; 1-of-5 may be noise.
- The social scan is topical judgment, not string matching — "how to schedule Instagram posts" covers the theme "Instagram scheduling" even if no keyword appears verbatim. When unsure, mark thin rather than covered.
- Do not recommend capturing branded competitor keywords unless the user explicitly opts in.
- When many gap keywords cluster around one theme, recommend a hub page plus supporting social series rather than 50 individual pieces.
