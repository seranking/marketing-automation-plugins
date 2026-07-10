# 05 — Per-subdomain backlinks

> Source: `DATA_getBacklinksSummary(target="<subdomain>", mode="host")` + `DATA_getBacklinksRefDomains(target="<subdomain>", limit=20)` per subdomain. SE Ranking auth required.

## Why this matters for the subdomain analysis

Subdomains often accumulate **separate backlink profiles**. A consolidation decision needs to know how much link equity each subdomain holds independently — a subdomain with a strong, distinct backlink profile is harder to consolidate without losing equity.

## What a full run captures per subdomain

```jsonc
{
  "subdomain": "developers.notion.com",
  "summary": {
    "backlinks_total": 12345,
    "ref_domains_total": 678,
    "dofollow_percent": 71,
    "growth_velocity_30d": "+2.3%"
  },
  "top_ref_domains": [
    {"domain": "github.com", "authority": 96, "links": 234, "anchor_sample": "Notion API"},
    {"domain": "stackoverflow.com", "authority": 95, "links": 178, "anchor_sample": "Notion SDK docs"},
    ...
  ]
}
```

## Expected pattern (qualitative)

- **`www.notion.com`:** Massive backlink profile. Hundreds of thousands of referring domains. Anchor distribution dominated by branded ("Notion") with strong long-tail of generic ("workspace tool", "all-in-one"). DA distribution skews high.
- **`developers.notion.com`:** Smaller-but-stronger profile. Likely dominated by github.com, stackoverflow.com, dev.to, medium.com (developer-blogosphere). DA distribution very high (developer docs accumulate quality, not quantity).
- **`www.notion.so`:** Declining profile. Backlinks accumulated over years before the migration; equity is now consolidating to `notion.com` via the 301. SE Ranking's snapshot would show this transition mid-flight.

## Consolidation implication

`developers.notion.com` should **not** be consolidated to `notion.com/developers/` even if SE Ranking later shows traffic overlap — the backlink profile is distinct, audience is distinct, and the technical writing tooling (likely ReadMe/Stoplight) won't migrate cleanly to the marketing CMS. Recommendation stands: leave the developer-docs subdomain alone.

The `notion.so` → `notion.com` transition is the only consolidation in scope, and that's a domain-level 301, not a subdomain-level decision.

## Pending

A full run with SE Ranking auth populates the tables and confirms the qualitative predictions.
