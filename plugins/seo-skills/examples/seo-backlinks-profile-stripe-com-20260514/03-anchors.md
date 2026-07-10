# 03 — Anchor text distribution

> Source: would come from `DATA_getBacklinksAnchors(target="stripe.com", mode="domain", limit=1000, order_by="backlinks")`. SE Ranking auth required for live counts.

## Classification rubric

Each anchor text is classified into one of:

| Class | Definition | Examples for Stripe |
|---|---|---|
| Branded | Contains the brand name explicitly | "Stripe", "Stripe Inc", "Stripe payments", "Stripe API" |
| Exact-match commercial | The exact target commercial keyword | "online payments", "credit card processing", "merchant of record" |
| Partial-match | Branded + descriptor or topic + product | "Stripe checkout", "payment processor Stripe", "Stripe Atlas startup" |
| Generic | Generic CTA / pronoun | "click here", "this", "learn more", "read more", "this article" |
| Naked URL | The literal URL | "https://stripe.com/...", "stripe.com" |
| Image-alt-derived | Link is on an image and the anchor comes from the image's `alt` text | (alt="Stripe logo"), (alt="Powered by Stripe") |

## Expected distribution for Stripe (qualitative)

| Class | Expected % | Healthy range | Status (qualitative) |
|---|---:|---|---|
| Branded | 55–65% | 30–60% | ✓ (at the high end — expected for a strongly known brand) |
| Generic | 15–20% | 15–30% | ✓ |
| Naked URL | 10–15% | 10–25% | ✓ |
| Partial-match | 7–10% | 10–20% | ⚠ slightly low — reflects that Stripe's brand is more recognised than its product feature names |
| Exact-match commercial | < 3% | <5% | ✓ — no over-optimisation signal |
| Image-alt-derived | 5–8% | <10% | ✓ |

## What anomalies would matter

If a re-run shows:

- **Exact-match commercial > 5%**: investigate. This is the classic Penguin-era over-optimisation pattern; a sudden rise typically indicates either (a) a successful outreach campaign that should be tracked, or (b) a negative-SEO attack.
- **Branded < 30%**: investigate. A drop suggests anchor dilution by manipulative anchors entering the profile.
- **Generic > 35%**: investigate. Sudden generic-anchor inflation can be a link-network signal (sites buying "click here" links to obscure the campaign).

## Top exact-match commercial anchors to watch (Stripe-specific)

These are the commercial-intent phrases Stripe ranks for that would attract over-optimised anchors:

1. "online payments" / "online payment processing"
2. "credit card processing"
3. "payment gateway"
4. "merchant of record"
5. "subscription billing"
6. "stripe alternative" (this is competitor-targeted, not Stripe-promoting)
7. "payments API"
8. "card processing"

A monthly diff on the count of inbound links using any of these exact phrases is a high-leverage early-warning signal.

## Pending

Full table populates on next run with SE Ranking auth. Production output emits the top 200 anchors with counts + classification + risk flag per row.
