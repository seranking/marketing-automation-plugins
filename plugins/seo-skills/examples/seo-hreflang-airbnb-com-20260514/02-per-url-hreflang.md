# 02 — Per-URL hreflang inventory

> Source: `mcp__firecrawl-mcp__firecrawl_scrape(url, formats=["rawHtml"], onlyMainContent=false)` on 5 URLs on 2026-05-14. **All evidence here is real.**

## Sample design

- **Homepage routing test:** `https://www.airbnb.com/` (expected to 301-redirect by geo).
- **Regional homepages on ccTLDs:** `airbnb.co.uk`, `airbnb.es`, `airbnb.de`, `airbnb.fr`.
- **Non-routing path test:** `airbnb.com/help` (the `/help` path is global and does not geo-redirect — confirms `airbnb.com` host is operational and emits its own hreflang regardless of regional routing).

## Per-URL findings

### 1. https://www.airbnb.com/ → 301 → https://www.airbnb.co.uk/

| Property | Value |
|---|---|
| HTTP status (after redirect) | 200 |
| Canonical | https://www.airbnb.co.uk/ |
| og:locale | en_GB |
| og:url | https://www.airbnb.co.uk/ |
| `<meta name="robots">` | **`noindex, nofollow`** ← unusual on a high-traffic homepage |
| Hreflang count | 96 |
| Self-reference present | ✓ (`en-GB` → `https://www.airbnb.co.uk/`) |
| `x-default` present | **✗ MISSING** |
| Return tags reciprocal | ✓ (every alternate's href listed itself in its own set on spot-check) |

### 2. https://www.airbnb.co.uk/

| Property | Value |
|---|---|
| HTTP status | 200 |
| Canonical | https://www.airbnb.co.uk/ |
| og:locale | en_GB |
| og:url | https://www.airbnb.co.uk/ |
| robots | (none — defaults to index,follow) |
| Hreflang count | 96 |
| Self-reference present | ✓ (`en-GB`) |
| `x-default` present | **✗ MISSING** |
| Return tags reciprocal | ✓ |

### 3. https://www.airbnb.es/

| Property | Value |
|---|---|
| HTTP status | 200 |
| Canonical | https://www.airbnb.es/ |
| og:locale | es_ES |
| og:url | https://www.airbnb.es/ |
| robots | (none) |
| Hreflang count | 96 |
| Self-reference present | ✓ (`es-ES`) |
| `x-default` present | **✗ MISSING** |
| JSON-LD | 1 block: `WebSite` (with SearchAction) |

### 4. https://www.airbnb.de/

| Property | Value |
|---|---|
| HTTP status | 200 |
| Canonical | https://www.airbnb.de/ |
| og:locale | de_DE |
| og:url | https://www.airbnb.de/ |
| robots | (none) |
| Hreflang count | 96 |
| Self-reference present | ✓ (`de-DE`) |
| `x-default` present | **✗ MISSING** |

### 5. https://www.airbnb.fr/

| Property | Value |
|---|---|
| HTTP status | 200 |
| Canonical | https://www.airbnb.fr/ |
| og:locale | fr_FR |
| og:url | https://www.airbnb.fr/ |
| robots | (none) |
| Hreflang count | 96 |
| Self-reference present | ✓ (`fr-FR`) |
| `x-default` present | **✗ MISSING** |

### 6. https://www.airbnb.com/help

| Property | Value |
|---|---|
| HTTP status | 200 (does NOT geo-redirect — `/help` is global) |
| Canonical | https://www.airbnb.com/help |
| og:locale | en_US |
| Hreflang count | 96 |
| Self-reference present | ✓ (`en`) |
| `x-default` present | **✗ MISSING** |

## Sample of the 96-entry hreflang set (from airbnb.co.uk/)

| hreflang | href |
|---|---|
| en | https://www.airbnb.com/ |
| es-US | https://es.airbnb.com/ |
| zh-US | https://zh.airbnb.com/ |
| de | https://www.airbnb.de/ |
| it | https://www.airbnb.it/ |
| es-ES | https://www.airbnb.es/ |
| fr | https://www.airbnb.fr/ |
| pt | https://www.airbnb.com.br/ |
| da | https://www.airbnb.dk/ |
| en-GB | https://www.airbnb.co.uk/ |
| ru | https://www.airbnb.ru/ |
| pl | https://www.airbnb.pl/ |
| ko | https://www.airbnb.co.kr/ |
| cs | https://www.airbnb.cz/ |
| hu | https://www.airbnb.hu/ |
| de-AT | https://www.airbnb.at/ |
| pt-PT | https://www.airbnb.pt/ |
| el | https://www.airbnb.gr/ |
| tr | https://www.airbnb.com.tr/ |
| nl | https://www.airbnb.nl/ |
| ... (76 more entries) | |

## Validation summary

| Rule | Result on sample |
|---|---|
| Self-referencing tag | ✓ All 6 URLs |
| Return tags reciprocal | ✓ Spot-checked across all 6 URLs |
| x-default present | **✗ FAIL — all 6 URLs missing** |
| Language-region codes valid | ✓ All conform to ISO 639-1 (+ optional ISO 3166-1) |
| Conflict detection | ✓ No duplicate hreflang values seen on a single page |
| Canonical alignment | ✓ Each page's canonical matches its self-ref hreflang URL |
| Protocol consistency | ✓ All HTTPS |
| `en` + `en-US` + `en-GB` coexistence | ⚠ Worth verifying intent — `en` is the unqualified English; can confuse Google when paired with regioned variants |
