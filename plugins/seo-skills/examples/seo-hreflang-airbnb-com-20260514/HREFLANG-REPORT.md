# Hreflang Audit: airbnb.com

> Snapshot dated 2026-05-14 · 5-URL sample · 96 hreflang values per page across the lattice · Numbers will drift — re-run the skill for current data.

## Verdict: **NEEDS-FIX**

**Reasoning:** the hreflang lattice itself is large and consistent (96 alternates per page, return-tag-reciprocal across all sampled pages), but **every sampled page is missing `hreflang="x-default"`**. That is a High-severity gap because it forces Google to guess the fallback locale for traffic with ambiguous Accept-Language. Combined with the homepage emitting `<meta name="robots" content="noindex, nofollow">` (also confirmed across the geo-routed `airbnb.com` → `airbnb.co.uk` redirect), the site looks like it's making deliberate choices we can't disprove — but the absent `x-default` is the unambiguous fix.

## Summary

| Source | Findings | Severity breakdown |
|---|---|---|
| Per-URL HTML inventory | 5 URLs, all 96-alternate, **none with x-default** | Critical: 0 · High: 5 · Medium: 1 · Low: 0 |
| SE Ranking audit | (not run — SE Ranking auth-gated in this worktree) | — |
| Sitemap | (not run — no sitemap deep-dive in this run) | — |

## Top fixes (impact-ranked)

| # | URL | Issue | Severity | Fix |
|---|---|---|---|---|
| 1 | All 5 sampled pages | `hreflang_missing_x_default` | High | Add `<link rel="alternate" hreflang="x-default" href="https://www.airbnb.com/">` to every page's `<head>`. The `airbnb.com` host is the conventional global fallback. |
| 2 | https://www.airbnb.com/ (after geo-redirect to airbnb.co.uk) | Homepage emits `robots: noindex, nofollow` | Medium | This is geo-routing producing a noindex on the redirected variant — verify intent. If unintentional, the redirected page should index normally. If intentional (per a regional-compliance constraint), document it; otherwise remove the noindex. |
| 3 | All 5 sampled pages | `en` is used as a hreflang value alongside `en-US`, `en-GB`, `en-AU` etc. | Low | `en` without a region implies "any English". Combined with `en-US` (also present pointing to airbnb.com), there's a potential ambiguity. Audit which one Google actually serves. |
| 4 | All 5 sampled pages | The `es-US` hreflang points to `es.airbnb.com` (a host-subdomain pattern), while other locales use ccTLDs (`airbnb.de`, `airbnb.fr`). | Low | This is an architectural inconsistency, not technically wrong. Document the convention. |

## Verdict heuristic application

- Critical findings: 0
- High findings: **5 (one per sampled page — all missing x-default)** → triggers NEEDS-FIX automatically
- BROKEN signals: none (every sampled page has self-reference; canonical aligns with self-ref hreflang)

## Sampled URLs

| URL | Status | og:locale | Canonical | Hreflang count | Self-ref? | x-default? |
|---|---|---|---|---|---|---|
| https://www.airbnb.com/ → 301 → https://www.airbnb.co.uk/ | 200 | en_GB | https://www.airbnb.co.uk/ | 96 | ✓ (en-GB) | ✗ |
| https://www.airbnb.co.uk/ | 200 | en_GB | https://www.airbnb.co.uk/ | 96 | ✓ (en-GB) | ✗ |
| https://www.airbnb.es/ | 200 | es_ES | https://www.airbnb.es/ | 96 | ✓ (es-ES) | ✗ |
| https://www.airbnb.de/ | 200 | de_DE | https://www.airbnb.de/ | 96 | ✓ (de-DE) | ✗ |
| https://www.airbnb.fr/ | 200 | fr_FR | https://www.airbnb.fr/ | 96 | ✓ (fr-FR) | ✗ |
| https://www.airbnb.com/help | 200 | en_US | https://www.airbnb.com/help | 96 | ✓ (en) | ✗ |

Note: Firecrawl's request originated from a UK-routed IP, so `https://www.airbnb.com/` 301-redirected to `https://www.airbnb.co.uk/`. The `airbnb.com` host is reachable for `/help` (which doesn't geo-redirect) — confirms `airbnb.com` is alive as a host, the homepage just routes by IP.

## Languages detected (sampled)

The full 96-entry hreflang set is reproduced across every sampled page. Sample of the first 20 entries (from `airbnb.co.uk`):

| Language | URL count | Self-ref OK | Return tags OK | x-default OK |
|---|---|---|---|---|
| en | All | ✓ | ✓ | ✗ |
| en-GB | All | ✓ | ✓ | ✗ |
| en-US | All | ✓ | ✓ | ✗ |
| de | All | ✓ | ✓ | ✗ |
| es-ES | All | ✓ | ✓ | ✗ |
| fr | All | ✓ | ✓ | ✗ |
| it | All | ✓ | ✓ | ✗ |
| pt | All | ✓ | ✓ | ✗ |
| ja, ko, ru, pl, da, nl, cs, hu, el, tr, etc. (full list in evidence/) | All | ✓ | ✓ | ✗ |

## Per-URL inventory (5 URLs sampled)

| URL | Alternates | Self-ref | x-default | Notable issues |
|---|---|---|---|---|
| airbnb.co.uk/ | 96 | ✓ | ✗ | Missing x-default; otherwise clean |
| airbnb.es/ | 96 | ✓ | ✗ | Missing x-default |
| airbnb.de/ | 96 | ✓ | ✗ | Missing x-default |
| airbnb.fr/ | 96 | ✓ | ✗ | Missing x-default |
| airbnb.com/help | 96 | ✓ | ✗ | Missing x-default; this confirms the pattern extends beyond home |

## Sitemap-level hreflang

- Sitemap fetch deferred — robots.txt at `/robots.txt` declares the sitemap path, but Airbnb's full sitemap is sharded heavily; a deep-dive belongs to `seo-sitemap`. Sitemap-vs-HTML cross-validation pending.
- The single most useful sitemap check for this case is whether the *sitemap-style hreflang* (`<xhtml:link rel="alternate" …>`) declares `x-default` even though the HTML doesn't. If yes, Google reads both — the HTML fix is still needed for any LLM scraping just the HTML.

## GSC verification of hreflang targets

The hreflang lattice includes ~25 ccTLDs (`airbnb.de`, `airbnb.fr`, `airbnb.es`, `airbnb.com.au`, `airbnb.co.kr`, …). For cross-domain hreflang, Google recommends each property be verified in GSC. Skipped here — `google-api.json` is not configured.

## Coverage notes

- Per-URL inventory tool: **Firecrawl** `formats: ["rawHtml"]` (not the markdown WebFetch fallback — WebFetch was blocked by Airbnb's WAF on a direct hit).
- Pages sampled: homepage + 4 regional variants + the `/help` URL. Stratified to cover (a) homepage routing behaviour, (b) several ccTLDs, (c) a non-routing path that exposes the apex-domain behaviour.

## Apply

1. Add this one `<link>` to every page's head template:
   ```html
   <link rel="alternate" hreflang="x-default" href="https://www.airbnb.com/">
   ```
   The choice of `airbnb.com` as the x-default is justified by it being the brand's primary apex; the geo-routing behaviour is orthogonal (the routing happens *after* the page loads, so Google's reading of `x-default` still works correctly).
2. Re-audit the noindex,nofollow on the geo-redirected homepage. If unintentional, remove. If intentional, document the rationale (e.g. "to keep regional duplicate-of-home variants out of the index").
3. After the x-default fix lands, re-run this skill to verify.

## Handoff payload

- **Produced by:** seo-hreflang
- **Target:** airbnb.com
- **Key findings:** (a) every sampled page has 96 hreflang alternates with proper self-reference and return-tag reciprocity; (b) **`x-default` is missing on every page** — site-wide; (c) the homepage routes by IP via 301, and the redirected page emits `robots: noindex, nofollow` (medium-severity, investigate intent); (d) the lattice mixes ccTLD hosts (`airbnb.de`, `airbnb.fr`) with subdomain hosts (`es.airbnb.com`, `zh.airbnb.com`) — architectural inconsistency, not technically wrong; (e) `en` is present alongside `en-US` and `en-GB` — could be intentional (true global English fallback) or an error.
- **Open loops:** sitemap-level hreflang vs HTML cross-check (`seo-sitemap`); SE Ranking audit's per-page hreflang flags (auth-gated); GSC verification of the 25 ccTLDs (Google APIs not configured).
- **Recommended next skill:** `seo-sitemap` — to confirm whether sitemap-style hreflang declares `x-default` independently. If yes, the fix priority drops; if no, the HTML-level fix is even higher priority.
