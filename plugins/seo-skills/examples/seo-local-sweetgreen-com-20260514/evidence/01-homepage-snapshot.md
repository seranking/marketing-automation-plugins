# 01 — Homepage snapshot: sweetgreen.com

> MCP tool: `mcp__firecrawl-mcp__firecrawl_scrape` with `{ url: "https://www.sweetgreen.com/", formats: ["markdown", "rawHtml"] }`
> WebFetch for parallel content-prose inspection.
> Live capture: 2026-05-14.

## Page metadata

| Field | Value |
|---|---|
| `<title>` | sweetgreen \| Inspiring healthier communities |
| og:title | Not provided in source |
| og:description | Not provided in source |
| `og:site_name` | sweetgreen |
| Twitter | @sweetgreen |
| Canonical | https://www.sweetgreen.com/ |

## NAP signals on homepage

| Signal | Detected? | Notes |
|---|---|---|
| Visible street address | **No** | Sweetgreen's homepage is a brand page, not store page. Expected. |
| Phone number | **No** | No corporate phone displayed |
| Embedded Google Maps iframe | **No** | Not on homepage |
| GBP profile link | **No** | Not on homepage |
| "Find a location" link | **Yes** | `/locations` |
| Yelp / TripAdvisor / Trustpilot link | **No** | None visible |
| `aggregateRating` JSON-LD | **No** | Schema scan returned 0 LD-JSON blocks on homepage |
| Click-to-call `tel:` elements | 0 | None |
| Business hours visible | **No** | Hours live per-location |

## Reading

The homepage is correctly minimal for local-SEO purposes — Sweetgreen runs 379 stores so a single homepage NAP doesn't apply. The local-SEO signals belong at the location level (`/locations/{slug}`). The audit shifts to the location-page pattern, which is sampled in `02-nap-page-samples.md`.

## Brand-tagline observation

- og:title and `<title>` both use "Inspiring healthier communities" branding.
- Word count: ~800-900 (largely menu carousel + brand statement).
- Primary structure: hero + product-category grid (Salads / Warm Bowls / Sides / Beverages) + workplace-solutions block + social-media block.
- No reference to total location count (the 379 stores claim is on `/locations`).

## Hand-off to step 2 (NAP sampling)

The audit now samples the location-page pattern. Pages selected:

1. `/locations/union-square` (NYC, urban, high-traffic)
2. `/locations/cherry-creek` (Denver, suburban, mid-traffic)
3. `/locations/montrose` (Houston, urban, recently-opened)
4. `/locations/dupont-circle` (DC, attempted — returned 404)

Step 2 documents the NAP extracts and the schema-presence variance discovered.
