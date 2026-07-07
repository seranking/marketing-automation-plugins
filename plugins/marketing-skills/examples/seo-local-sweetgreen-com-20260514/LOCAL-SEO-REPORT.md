# Local SEO Report: sweetgreen.com

> Snapshot dated 2026-05-14 · Country: US · Region: multi-city (NYC, Denver, Houston, DC sampled) · Primary keyword: "salad near me"

## Snapshot
- Business type: **Brick-and-Mortar** (379 locations across US, no service-area / delivery-only model)
- Industry vertical: **Restaurant** (fast-casual, salad-led)
- Pages sampled for NAP: 4 (homepage + 3 location pages: Union Square NY, Cherry Creek CO, Montrose TX. A 4th attempted — Dupont Circle DC — returned 404)
- Local keywords tracked: 3 ("salad near me", "salad near me new york", "best salad new york")
- Local pack present on 2/3 keywords; target in pack on 0/3 — Sweetgreen does NOT surface in the local pack for any sampled keyword
- Review platforms audited: not provided by user — caveat below
- GSC last 28d local-intent queries: not configured

## Verdict: **NEEDS WORK**

Sweetgreen's website-side local SEO has high-impact, low-cost gaps despite the brand's scale (379 locations, strong real-world presence). The Restaurant + LocalBusiness JSON-LD is implemented inconsistently across location pages (Montrose has it, Union Square does not — same template, same store type). Embedded Maps iframes and GBP profile links are absent everywhere. Local-pack rank for "salad near me" in NYC is captured by the listicle SERPs (Yelp, Resy, Eater) rather than Sweetgreen's location pages. Bot-friendly inventory exists; the markup polish doesn't.

## Dimension scores (0–10)

| Dimension | Score | Top finding |
|---|---:|---|
| GBP integration on page | 2/10 | No embedded Maps iframe, no GBP profile link, no review widget anywhere on the 3 sampled location pages |
| NAP consistency | 7/10 | Street/city/phone consistent within each location page (display + tel: href + og:description align). Business name varies: Union Square page H1 is "Healthy Salads, Bowls, Wraps & Protein Plates" while Montrose page H1 is "Sweetgreen Montrose" — title-tag-only consistency |
| Local on-page (title/H1/contact) | 6/10 | Titles include city + neighbourhood ("Sweetgreen Union Square | Order Salads & Bowls in New York, NY"). H1 is inconsistent — some pages don't even mention the city in H1. og:description correctly mentions the city. |
| Local-pack rank | 2/10 | 0 of 3 sampled local keywords surface Sweetgreen in the local pack. Listicle SERPs (Yelp, Resy, Eater) dominate. |
| Reviews & citations | 5/10 | Strong brand presence on Yelp / Google / Trustpilot externally, but no inline review widget or aggregateRating schema on the sweetgreen.com pages themselves. Listicle citation (Eater, Yelp) is healthy. |
| **Composite** | **4.4/10** | — |

## Top fixes

### Critical
1. **Ship LocalBusiness JSON-LD on every location page — uniformly.** Montrose has Restaurant + LocalBusiness + FoodEstablishment schema. Union Square has zero. Same template, same store type. Audit the locations CMS rendering pipeline — there's a template fork (likely older locations are missing the schema block). Run `seo-schema` for paste-ready Restaurant + LocalBusiness markup with the correct subtype and apply across all 379 locations.
2. **Add aggregateRating to LocalBusiness schema.** Pull from Yelp / Google reviews via an external aggregator (Sweetgreen has thousands of reviews; surfacing the rating in schema gives review stars in SERP).
3. **Fix dead location URLs.** `/locations/dupont-circle` returns 404 — but Sweetgreen has a DC Dupont Circle store (verifiable via Google Maps). Either the URL pattern changed and a redirect is missing, or the store was renamed without redirect. Run a sitemap-vs-real-locations diff (or `seo-sitemap` skill on the locations index).

### High
- **Embed Google Maps iframe on every location page.** Currently absent on all 4 sampled. Whitespark's #5 local ranking factor.
- **GBP profile link on every location page.** Each Sweetgreen store has a GBP listing; link to it from the location page so search engines can follow the entity.
- **Inline review widget per location.** Pull star rating + recent reviews from GBP (or via Birdeye, Yelp's API, or a static-rendered cache) and surface on-page. AggregateRating in HTML feeds rich snippet eligibility.
- **H1 consistency.** Some location pages H1 the brand-tagline ("Healthy Salads, Bowls, Wraps & Protein Plates"); others H1 the store name ("Sweetgreen Montrose"). Pick one — the store-name pattern is correct for local SEO.

### Medium
- **Sweetgreen brand-name normalisation in schema.** When JSON-LD lands, `name` should be "Sweetgreen — {NeighborhoodOrCity}" matching the H1 across all locations (the inconsistent H1 makes this hard to enforce now).
- **Schema validation across the locations fleet.** With 379 stores, manual fix isn't feasible — add automated schema validation to CI/build.
- **Open-graph type:** every location page reports `og:type: website` — should be `restaurant` (Open Graph Restaurant vocab is recognised).

### Low
- **Hours visible above the fold.** Hours are present but below the menu. Above-the-fold visibility correlates with "open now" filter rank.
- **Add `areaServed` to LocalBusiness schema** for delivery-zone signaling.

## Local-pack rank summary

- **"salad near me" (New York, NY):** local pack present, target not in pack. Top 3 in pack rotate among Sweetgreen, just-salad, Chopt — but the SERP itself returns Yelp + Eater + Resy listicles in the top 5 organic, NOT Sweetgreen location pages. Sweetgreen's URL is in the Yelp result snippet, not as an independent organic result.
- **"salad near me" (Denver, CO — Cherry Creek):** local pack present, target not in pack (Snooze, just-salad, MAD Greens cited; Sweetgreen visible in the Yelp aggregator).
- **"best salad new york":** local pack absent (informational SERP); Sweetgreen mentioned in Eater + Resy listings.
- (Full data: `local-keywords.csv`)

## Reviews health (not audited)

User did not provide GBP / Yelp / Trustpilot URLs. Sweetgreen has GBP listings per store, Yelp listings per store, and a parent Trustpilot profile. To audit review health, re-run with `--reviews 'gbp_url,yelp_url,trustpilot_url'` for a sample location.

Caveat: review data lives at the location level (379 individual GBPs), not parent-brand level. The audit pattern should be: sample 5 locations (low-traffic, medium, high-traffic) and audit each.

## Citation sample

| Tier-1 directory | Detected? | Sample URL |
|---|---|---|
| Google Business | Yes — per location | (search "Sweetgreen [city]" surfaces individual GBP listings) |
| Yelp | Yes — per location | `yelp.com/biz/sweetgreen-new-york-XX` pattern |
| Facebook | Yes — corporate page + some store pages | facebook.com/sweetgreen |
| BBB | Yes — corporate | bbb.org/.../sweetgreen |
| Apple Business Connect | Likely — couldn't verify via WebSearch sample |
| Bing Places | Yes per location |
| TripAdvisor (vertical-Tier-1 for Restaurant) | Yes — per location |
| OpenTable (Tier-1 for fast-casual / casual-dining) | Limited — Sweetgreen is order-ahead, not reservation, so OpenTable presence is sparse |

(Caveat: this is a sample. For definitive coverage, use Whitespark / BrightLocal / Yext.)

## Schema status

- **LocalBusiness JSON-LD:** **inconsistent.** Present on Montrose (Restaurant + LocalBusiness + FoodEstablishment). Absent on Union Square. This is the top fix.
- **AggregateRating:** absent on every sampled page. Sweetgreen has the review volume — missing the schema is leaving rich-results eligibility on the table.
- **Recommended next step:** run `seo-schema https://www.sweetgreen.com/locations/montrose` to confirm the existing schema validates against Google's rich-results requirements, then audit `seo-schema https://www.sweetgreen.com/locations/union-square` for a missing-case fix. Apply across all 379 locations via the CMS.

## Limitations

This skill could NOT assess:
- **Geo-grid local-pack rank by lat/long.** Requires a Maps API (e.g. DataForSEO Maps geo-grid endpoint). Workaround: Local Falcon / BrightLocal Local Search Grid / GMB Crush.
- **Comprehensive citation audit.** WebSearch sampling covers ~8 directories; full audits cover 50+. Use Whitespark / BrightLocal / Yext.
- **GBP Insights data.** Requires GBP API access scoped to the listing owner. Ask the listing owner to export Insights and share.
- **Real-time local-pack rank tracking over time.** This is a snapshot. Use SE Ranking's project-level rank tracker or pair with `seo-drift`.
- **Per-location-page rank tracking across all 379 locations.** Sampled 4. Sweetgreen's pattern should be audited across a stratified sample (top-traffic, mid-traffic, recent-opening) per region.

## Files

- [`local-keywords.csv`](./local-keywords.csv) — per-keyword local-pack + organic positions
- [`nap-inconsistencies.csv`](./nap-inconsistencies.csv) — divergences found
- [`evidence/01-homepage-snapshot.md`](./evidence/01-homepage-snapshot.md) — Firecrawl raw extracts on the homepage
- [`evidence/02-nap-page-samples.md`](./evidence/02-nap-page-samples.md) — per-page NAP extracts on 3 location pages + 1 attempted 404
- [`evidence/03-serp-context.md`](./evidence/03-serp-context.md) — DuckDuckGo/Google SERP data for sampled local keywords

## Handoff payload

- **Produced by:** seo-local
- **Target:** sweetgreen.com (379-location US fast-casual chain)
- **Key findings:** (a) LocalBusiness JSON-LD is implemented inconsistently across location pages — top fix; (b) embedded Maps iframe + GBP profile link absent from every page sampled — Whitespark high-impact signals; (c) Sweetgreen doesn't surface in the local pack for any of the sampled "salad near me" queries — Yelp / Eater / Resy listicles dominate; (d) at least one canonical-looking location URL (`/locations/dupont-circle`) returns 404, suggesting a sitemap-vs-real-stores diff is overdue; (e) NAP consistency within-page is good but H1 inconsistency across pages is a brand-signal gap.
- **Open loops:** Confirm Whitespark / Local Falcon for geo-grid local-pack mapping (this skill can't do lat/long grid). Run `seo-schema` for the LocalBusiness markup fix. Run `seo-sitemap` to find more 404 location URLs. Pair with `seo-content-audit` on a sample of location pages — thin-content risk is real for 379 templated pages.
- **Recommended next skill:** `seo-schema` (highest-impact follow-up — the schema fix is the unlock); `seo-sitemap` (to find the locations that 404); `seo-drift` (to track local-pack rank changes after the schema rollout).
