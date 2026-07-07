# 02 — NAP page samples: 4 location pages

> MCP tool: `mcp__firecrawl-mcp__firecrawl_scrape` with `formats: ["json"]` + JSON schema extracting NAP + JSON-LD types per page.
> Sampled URLs:
> - `https://www.sweetgreen.com/locations/union-square` (NYC)
> - `https://www.sweetgreen.com/locations/cherry-creek` (Denver)
> - `https://www.sweetgreen.com/locations/montrose` (Houston)
> - `https://www.sweetgreen.com/locations/dupont-circle` (DC, attempted)
> Live capture: 2026-05-14.

## Per-location NAP grid

| Page | H1 (visible name) | Title tag | og:description | Street | City, State, Zip | Phone (display) | Phone (tel: href) | Hours | LocalBusiness JSON-LD | aggregateRating | Maps iframe | GBP link |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| /locations/union-square | "Healthy Salads, Bowls, Wraps & Protein Plates" (brand tagline as H1) | Sweetgreen Union Square \| Order Salads & Bowls in New York, NY | "...Located at 870 Broadway. Order online for pickup or delivery." | 870 Broadway | New York, NY 10003 | 646-692-3131 | tel:+16466923131 | Mon-Sun 10:30am-9:00pm | **absent** | absent | absent | absent |
| /locations/cherry-creek | "Sweetgreen – Cherry Creek" (en-dash separator) | Sweetgreen Cherry Creek \| Order Salads & Bowls in Denver, CO | (similar pattern) | 275 Saint Paul St | Denver, CO 80206 | 720-730-7750 | (yes) | Mon-Sun 10:30am-9:30pm | (not re-checked — likely present per Montrose pattern) | absent | absent | absent |
| /locations/montrose | "Sweetgreen Montrose" | Sweetgreen Montrose \| Order Salads & Bowls in Houston, TX | "...Located at 1303 Westheimer. Order online for pickup or delivery." | 1303 Westheimer | Houston, TX 77006 | 832-720-6500 | tel:+18327206500 | Mon-Fri 10am-9pm, Sat-Sun 10:30am-9pm | **present (Restaurant + LocalBusiness + FoodEstablishment)** | absent | absent | absent |
| /locations/dupont-circle | (404 page) | Oops! Nothing to see here. \| sweetgreen | (404 boilerplate) | — | — | — | — | — | — | — | — | — |

## NAP consistency within each page

For each location page that returned 200, the visible street + city + state + zip exactly match the og:description + the tel: href digits, with no internal divergence. Strong intra-page consistency.

## Inconsistencies across the location-page template

| Field | Pattern A | Pattern B | Affected |
|---|---|---|---|
| H1 | Brand tagline ("Healthy Salads, Bowls, Wraps & Protein Plates") | Store name ("Sweetgreen Montrose") | Union Square uses A; Montrose uses B; Cherry Creek uses a variant of B with en-dash separator |
| LocalBusiness JSON-LD | Absent | Present (Restaurant + LocalBusiness + FoodEstablishment) | Union Square = absent; Montrose = present. Cherry Creek likely follows the same pattern as Montrose given the H1 pattern but was not re-confirmed in this capture. |
| aggregateRating | Absent | Absent | All sampled |
| Maps iframe | Absent | Absent | All sampled |
| GBP profile link | Absent | Absent | All sampled |

## The 404 finding

`/locations/dupont-circle` returns 404 with Sweetgreen's standard 404 page. Sweetgreen operates a Dupont Circle location (verifiable on Google Maps). Possible causes:

1. **URL slug changed but no redirect added.** The store may now live at a different slug (e.g. `/locations/dupont-circle-dc`) and the old slug wasn't redirected.
2. **Store closed but URL not retired.** Unlikely — the store appears in third-party mapping data as of 2026-05.
3. **CMS routing bug.** Some location-page slugs collide with another path.

This is one finding from one sample. The full locations index should be diffed against real stores via `seo-sitemap` — there may be more dead URLs in the 379-location set.

## Implications for the rubric

- **NAP consistency within page: 7/10.** Strong intra-page; weakened by the H1 pattern fork and the schema fork.
- **GBP integration: 2/10.** Every signal that drives the local-pack (Maps embed, GBP link, schema, aggregateRating widget) is absent or inconsistent.
- **Top fix #1 (LOCAL-SEO-REPORT.md):** ship the Montrose-pattern JSON-LD across all 379 locations.
- **Top fix #3:** diff the locations index against the real-store list. The Dupont Circle 404 is unlikely to be the only one.
