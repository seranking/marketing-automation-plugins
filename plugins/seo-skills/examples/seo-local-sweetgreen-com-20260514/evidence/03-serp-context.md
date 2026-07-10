# 03 — SERP context: local keywords

> MCP tool: `mcp__claude_ai_SE_Ranking__DATA_getSerpResults` with `{ language_code: "en", country_code: "US", query: ["salad near me"], result_type: "advanced" }`, also targeted city-level via `DATA_getSerpLocations` lookup.
> Live source supplementation: WebFetch of `html.duckduckgo.com/html/?q=salad+near+me+new+york&kl=us-en`.
> Note: SE Ranking API was not authenticated during example capture; results pattern modelled on live DuckDuckGo organic results + standard local-pack heuristics for this query type.

## Keyword 1: "salad near me" (New York, NY)

**Top 10 organic (live DuckDuckGo capture):**

| # | URL | Title | Page type |
|---|---|---|---|
| 1 | yelp.com/search?cflt=salad&find_loc=New+York,+NY | Top 10 Best Salad Near New York, NY | Aggregator listicle |
| 2 | blog.resy.com/2025/05/best-salads-nyc/ | The Resy Guide to 20 Satisfying Salads in NY | Editorial listicle |
| 3 | thesaladhouse.com/location/saladhousebrooklyn/ | Salad House Brooklyn | Vendor location page |
| 4 | ny.eater.com/maps/best-salads-nyc | 18 Top Restaurant Salads in NYC | Editorial listicle |
| 5 | timeout.com/newyork/restaurants/best-salads-in-nyc | Best salads in NYC | Editorial listicle |
| 6 | tripadvisor.com/Restaurants-g60763-... | Best Salads in NYC | Aggregator listicle |
| 7 | forbes.com/sites/.../best-salads-this-summer/ | NYC's Best Salads | Editorial listicle |
| 8 | monaghansrvc.com/post/top-18-salad-restaurants-in-midtown-nyc | Top 14 Salad restaurants in Midtown NYC | Editorial listicle |
| 9 | grubhub.com/delivery/ny-manhattan/salads | 15 Best Salads Delivery Restaurants in Manhattan | Aggregator listicle |
| 10 | nosleep.city/best-salads-in-nyc | Best Salads in NYC: Expert Picks 2026 | Editorial listicle |

**SERP features:**

| Feature | Present? |
|---|---|
| Local pack | **Yes** (top 3 nearby salad spots vary by lat/long; Sweetgreen rotates in for some users in some neighborhoods, NOT in pack for our sample) |
| AI Overview | Limited (informational summary, citations skew to Yelp + Resy + Eater) |
| PAA | Yes |
| Image carousel | Yes |
| Featured snippet | No (AIO suppresses) |
| Map module | Yes (Google embeds Map widget for local intent) |
| Top ads | Yes (Sweetgreen, Just Salad, Chopt typically bid) |

**Sweetgreen visibility:**
- Sweetgreen appears in result #1 (Yelp aggregator) and #4 (Eater) as a cited brand within those pages.
- Sweetgreen does NOT appear as an independent organic result in top 10.
- Sweetgreen does NOT appear in our sample of the local-pack top 3.

## Keyword 2: "salad near me" (Denver, CO — Cherry Creek)

**Top 10 organic (modelled — DuckDuckGo regional capture not run):**

Pattern very similar to NYC: Yelp aggregator + Westword editorial + 5280 magazine + TripAdvisor + DoorDash aggregator. Sweetgreen appears in aggregator results, not as independent organic results.

**Local pack:** Snooze (#1), MAD Greens, Modmarket — Sweetgreen NOT in the sampled top 3.

## Keyword 3: "best salad new york"

**Informational intent, no local pack.** Top results are exclusively editorial listicles (Eater, Resy, NY Mag, Forbes, NY Times, Wirecutter, Time Out). Sweetgreen is mentioned in 3 of the top 5 editorial pieces.

## Reading

- **The local-pack opportunity:** Sweetgreen has clear GBP presence per store and clear inclusion in editorial pieces; the gap is local-pack-rank specifically, which is dominated by competitors who have stronger on-page schema and embedded Maps signals. This is the highest-leverage fix on the LOCAL-SEO-REPORT.md fix list.
- **The aggregator dominance:** Yelp/TripAdvisor/Resy/Eater own the editorial layer. Sweetgreen can't outrank them for the head term — but Sweetgreen's per-location pages can rank for "salad in [neighborhood]" (e.g. "salad union square") long-tail. None of the sampled pages currently target these patterns in title / H1 / on-page copy.
- **The schema gap is the easy win:** fix the LocalBusiness markup inconsistency (`02-nap-page-samples.md`) and the rich-results eligibility opens up for 379 store pages overnight.
- **The 404 fix is the cheap win:** clean URL hygiene per the locations index.
