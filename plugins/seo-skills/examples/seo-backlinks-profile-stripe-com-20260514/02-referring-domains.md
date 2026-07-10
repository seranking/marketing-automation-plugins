# 02 — Top referring domains

> Source: would come from `DATA_getBacklinksRefDomains(target="stripe.com", mode="domain", limit=200, order_by="domain_inlink_rank")`. SE Ranking auth required for live metrics.

## Schema (load-bearing fields per row)

```jsonc
{
  "domain": "github.com",
  "domain_authority": 96,
  "linking_pages_count": 4521,
  "first_seen": "2014-03-18",
  "last_seen": "2026-05-14",
  "anchor_sample": "Stripe API",
  "dofollow_pct": 0.78,
  "country": "US"
}
```

## Expected top-tier referrers (qualitative — what a real run typically surfaces for Stripe)

| Rank tier | Domain pattern | Why Stripe earns links here | Anchor pattern |
|---|---|---|---|
| 1 | `github.com` | Stripe SDKs, sample apps, official integrations | "Stripe", "Stripe API", "stripe.com" — branded + naked URL |
| 2 | `stackoverflow.com` | Developer Q&A referencing Stripe docs | Mostly naked URLs to specific docs pages |
| 3 | `wikipedia.org` | Stripe company / fintech / payment-processor articles | Branded |
| 4 | Top financial press (`bloomberg.com`, `wsj.com`, `ft.com`, `techcrunch.com`, `theinformation.com`) | Funding news, product launches, profile pieces | Branded |
| 5 | `medium.com`, `dev.to`, `hashnode.com` | Developer-blogosphere tutorials | Branded + partial-match ("payments integration") |
| 6 | Integration partners (every other SaaS that accepts Stripe) | "Payments powered by Stripe" footer/about-page links | Naked URL + branded |
| 7 | Academic / .edu domains | Course materials, fintech-curriculum citations | Branded |
| 8 | Government / .gov domains | Specific regulatory or contracting references | Branded |
| 9 | `producthunt.com` | Stripe product launches consistently feature | Branded |
| 10 | Long tail | Personal blogs, niche forums, regional fintech press | Mixed |

## Why this structure matters

Each tier signals a different motivation for linking:

- **Tier 1-2 (developer ecosystem)** = utility-driven. These links are very hard to disturb because they're embedded in code/documentation.
- **Tier 4 (press)** = newsworthy events. Predictable spikes around launches; manageable with PR cadence.
- **Tier 6 (integration partners)** = transactional. Every Stripe customer technically *could* link; the actual link rate depends on whether their docs/footer surface it.
- **Tier 10 (long tail)** = the noise floor. Most disavow-candidates come from here; most are false positives because the user is a legitimate (if small) customer.

## Production-run output (pending)

A live run produces the top 200 rows in `disavow-candidates.csv` and the full table here. The qualitative pattern above is what to expect; the metric cells are gated.
