---
name: marketing-ads-counterplay
description: >
  Paid-search competitive intelligence turned into an organic counterplay. Pulls SE Ranking's PPC
  data — competitor ad keyword footprints, ad copy patterns, SERP ad-pack visibility — then finds
  the messages competitors PAY for that the brand could win organically, checks whether the
  brand's social channels already cover those angles (Planable history), and delivers ready
  social hooks lifted from the winning ad-copy patterns plus a bid-keyword shortlist. Use when
  the user asks "what are competitors bidding on and how do we counter it", "paid search
  analysis", "competitor ads", "PPC competitive", "ad copy intelligence", "turn competitor ads
  into content", or "which paid messages can we take organically". Requires both the SE Ranking
  and Planable MCPs. For a pure PPC brief without the organic counterplay, use seo-ads from the
  seo-skills plugin instead.
---

# Ads counterplay

Map the paid-search landscape around a brand's target keywords — then the move a plain PPC brief never makes: identify the messages competitors are paying for that this brand can take organically, check whether its social channels already tell those stories, and hand back ready-to-use hooks.

## Prerequisites

- SE Ranking MCP server connected.
- **Planable MCP** connected, with the brand's workspace (for the social-coverage check).
- User provides: (a) a target domain OR a target keyword (skill detects which), (b) target country (default `us`), (c) the Planable workspace.

## Process

1. **Validate input & preflight**
   - Determine: domain mode (analyse a brand's paid footprint) or keyword mode (analyse the bidding landscape for one keyword).
   - `DATA_getSubscription` — surface remaining units. `list_workspaces` — confirm the Planable workspace is reachable.

2. **Domain mode** `DATA_getDomainAdsByDomain`
   - Pull paid keywords the target domain bids on.
   - For each: keyword, search volume, CPC, position, ad copy (title + description), URL.
   - Sort by traffic-weighted score (`volume × CTR-by-paid-position × bid-share`).

3. **Keyword mode** `DATA_getDomainAdsByKeyword`
   - Pull all domains bidding on the target keyword.
   - For each: domain, ad position, ad copy, URL.
   - Surface the top 10 advertisers + their copy patterns.

4. **Intent enrichment** `DATA_getKeywordQuestions`
   - For the keyword(s) in scope, pull related questions.
   - Identifies question-phrased intent variants worth bidding on (often cheaper, higher conversion).

5. **SERP ad/shopping presence** `DATA_getSerpResults`
   - For top 5 keywords (domain mode) or the target keyword (keyword mode):
     - Use SERP-feature filters to detect ad-pack composition: `tads` (top ads above organic), `bads` (bottom ads below organic), `sads` (shopping ads / Google Shopping pack), `mads` (mobile/map-pack ads).
     - Top SERP ad slots (positions 1-4 above organic, 1-3 below).
     - Shopping pack presence (carousel of product cards).
     - Image pack, local pack — these displace ad inventory.
   - Capture which advertisers occupy those slots.

6. **Ad copy pattern analysis**
   - Cluster ad headlines + descriptions by recurring patterns.
   - Identify: USP language used by leaders, pricing/discount mentions, audience segmentation, CTA verbs.
   - Highlight outliers (advertisers doing something different).

7. **Paid-keyword gap (domain mode)** `DATA_getDomainKeywords` with `type: 'adv'`
   - Pull the user's domain's paid keywords using the `type: 'adv'` switch.
   - For each top competitor (from step 2 or `DATA_getDomainCompetitors` with `type: 'adv'`): pull their paid keywords with `type: 'adv'`.
   - Diff: paid keywords competitors bid on that the user's domain doesn't.
   - This becomes the highest-leverage portion of the bid-keyword shortlist (step 8).
   - Skip in keyword mode (no domain to gap against).

8. **Recommended bid-keyword shortlist**
   - For domain mode: paid-keyword gap from step 7 + adjacent question-intent variants.
   - For keyword mode: question-intent and long-tail variants that are likely cheaper than the head term.
   - Each row: keyword, est. CPC, est. volume, who else bids, why-recommended.

9. **Organic counterplay (signature step)**
   - From the ad-copy patterns (step 6) and the paid-keyword gap (step 7), shortlist the messages competitors pay for that are winnable organically: informational/question variants of paid head terms, comparison intent ("X vs Y" bids convert best as pages, not ads), and USP claims a post can contest.
   - Check the brand's social coverage: `list_pages(workspaceId)` → `list_posts` for the last ~90 days; scan topically (meaning, not substrings) — has the brand already told each counterplay story?
   - Output the counterplay table: paid message/keyword → who pays for it (and est. CPC they're paying) → our social coverage (covered / thin / silent) → the organic play (post series / comparison page / FAQ content) → a ready social hook adapted from the winning ad-copy pattern.
   - Hand off: `marketing-seo-campaign` turns the counterplay rows into a labeled, measurable Planable campaign; comparison-intent rows go to `marketing-comparison-page`.

10. **Synthesise** `ADS.md`

## Output format

Create a folder `marketing-ads-counterplay-{target-slug}-{YYYYMMDD}/` with:

```
marketing-ads-counterplay-{target-slug}-{YYYYMMDD}/
├── ADS.md                              (synthesised brief — primary deliverable; inlines paid footprint, bidding landscape, SERP ad/shopping pack, ad copy patterns, paid keyword gap, organic counterplay)
├── recommended-keywords.csv            (bid-keyword shortlist — load-bearing CSV the PPC team pastes into bid tooling)
└── evidence/
    ├── 01-paid-footprint.md           (domain mode: brand's paid keywords — raw step output)
    ├── 02-bidding-landscape.md        (keyword mode: advertisers on the keyword — raw step output)
    ├── 03-question-variants.md        (DATA_getKeywordQuestions enrichment)
    ├── 04-serp-ad-shopping-pack.md    (SERP feature inventory per keyword)
    ├── 05-ad-copy-patterns.md         (clustered headline/description patterns)
    ├── 06-paid-keyword-gap.md         (domain mode: type='adv' diff vs competitors)
    └── 07-organic-counterplay.md      (paid message × social coverage × organic play + hooks)
```

Step files 01, 02, 04, 05, 06 are inlined as sections in `ADS.md`; the copies in `evidence/` preserve the raw step outputs for reproducibility.

`ADS.md` follows this shape:

```markdown
# Paid-Search Intelligence: {target}

> Snapshot dated {YYYY-MM-DD} · Country: {country} · Mode: {domain | keyword}

## Footprint summary
- Paid keywords: {n}
- Estimated paid traffic: {n}/mo
- Average CPC: ${n}
- SERP slots covered: {n} of top-4 above organic across {n} target keywords

## Top 10 paid keywords (domain mode)

| Keyword | Volume | CPC | Position | Ad copy excerpt |
|---|---|---|---|---|
| {kw} | {n} | ${n} | {pos} | "{headline} — {snippet}" |
| ...

## Bidding landscape (keyword mode — for "{keyword}")

| Advertiser | Position | Ad copy excerpt | URL |
|---|---|---|---|
| {domain} | {pos} | "{headline} — {snippet}" | {url} |
| ...

## Ad copy patterns (top patterns observed)

1. **Pricing-led:** "{N}% off — start at ${X}/mo" — used by {n} advertisers.
2. **Outcome-led:** "Get {specific outcome} in {time}" — used by {n}.
3. **Trust-led:** "Trusted by {n} {audience}" — used by {n}.
4. ...

## SERP feature inventory

| Keyword | Top ads | Shopping pack | PAA | Image pack |
|---|---|---|---|---|
| {kw} | {advertiser list} | {✓/✗} | {✓/✗} | {✓/✗} |
| ...

## Recommended bid-keyword shortlist

See `recommended-keywords.csv`. Top 10:

| Keyword | Volume | Est. CPC | Why |
|---|---|---|---|
| {kw} | {n} | ${n} | Question-intent variant; competitor X bids on head term but not this. |
| ...

## Constraints / caveats
- CPC and volume estimates are directional. Actual costs depend on Quality Score, time of day, audience, etc.
- {Note any ad-copy that's clearly seasonal / promotional and may not represent steady-state.}

## Organic counterplay

| Paid message / keyword | Who pays | Our social | Organic play | Ready hook |
|---|---|---|---|---|
| {kw / claim} | {domains, ~${cpc}} | silent | post series | "{hook adapted from their ad copy}" |

## Recommended next step
Launch `marketing-seo-campaign` on the counterplay rows (labeled + measurable); route comparison-intent rows to `marketing-comparison-page`. For deeper keyword architecture, seo-skills' `seo-keyword-cluster` / `seo-keyword-niche` pair well with the shortlist.
```

`recommended-keywords.csv` columns: `keyword,volume,cpc_estimate,position_target,intent,competitor_count,why_recommended`

## Tips

- Respect rate limit. Domain mode: ~3–5 calls. Keyword mode: ~3 calls. Plus a few SERP queries.
- Cost: ~10–20 credits typical for domain mode; ~5–10 for keyword mode.
- **CPC estimates lag.** SE Ranking's CPC data is not real-time auction data; treat as ±30% directional.
- Ad copy often reveals competitor positioning before product launches do — periodic review (quarterly) catches strategic shifts.
- Question-intent variants often have lower CPC and higher conversion than head terms. The shortlist in step 8 prioritises these.
- Pair with seo-skills' `seo-keyword-niche` for organic content opportunities derived from paid keyword research.
- Pair with `marketing-comparison-page` if the bidding landscape reveals "X vs Y" / "alternatives" intent — those keywords convert best as comparison pages, not paid ads.
- **Ads data via shared DATA_* tools** — beyond the dedicated `DATA_getDomainAdsByDomain` / `DATA_getDomainAdsByKeyword`, the `type: 'adv'` enum switch on `DATA_getDomainKeywords`, `DATA_getDomainKeywordsComparison`, `DATA_getDomainCompetitors`, `DATA_getDomainPages`, and similar tools surfaces the paid view of the same data structures. Combine with the `tads/bads/sads/mads` SERP-feature filters and the CPC filter on SERP queries to map paid landscape comprehensively.
- Don't recommend paid keywords without context. The shortlist is a starting point for the PPC team, not an autopilot.
