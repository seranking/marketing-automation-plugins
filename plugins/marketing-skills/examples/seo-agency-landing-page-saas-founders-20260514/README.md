# Anchor SEO landing page — customisation and deploy guide

> Snapshot dated 2026-05-14 · Built for B2B SaaS founders, $5M-$50M ARR · This is a generated deliverable, not an analysis

## What's in this folder

- `index.html` — self-contained, Tailwind-via-CDN, no JS framework. Deploys to any static host.
- `copy.md` — copy-only markdown for CMS paste.
- `01-niche-data.md` — the real B2B SaaS organic-search numbers used in the hero + pain copy.
- `02-pain-points.md` — the 3 pains we chose and why we rejected others.

## Before you publish

### 1. Fill in client logos

In `index.html`, find the placeholder:

```html
<div class="mt-12 border-t border-slate-200 pt-8 text-center text-slate-400 text-sm">
  [Client logo bar: 4-6 logos placed here]
</div>
```

Replace with a real logo bar. 4-6 logos, monochrome, grayscale, centered. If you have no client logos yet (newer agency), replace with a "Featured in" bar instead (publications you've been quoted in) — never invent fake logos.

### 2. Wire the form endpoint

In `index.html`, find:

```html
<form action="REPLACE_WITH_FORM_ENDPOINT" method="POST" ...>
```

Replace with one of:

- **Netlify Forms:** add `data-netlify="true"` and `name="audit-request"` to the form, deploy to Netlify, dashboard handles submissions.
- **Formspree:** sign up at formspree.io, get an endpoint URL like `https://formspree.io/f/xpzbqlol`, paste into `action`.
- **Zapier catch hook:** create a Zap with "Webhooks by Zapier > Catch Hook", paste the URL into `action`. Route to your CRM / audit-trigger workflow.

### 3. Wire the audit-trigger workflow

The page promises a 5-minute automated audit. That's a workflow you need to operate. Two paths:

- **Path A — use a real audit tool.** SE Ranking's audit API runs a full site crawl + scoring in 5-15 minutes. Sign up at se-ranking.com, get an API key, build a small worker that consumes form submits and triggers an audit, then emails the PDF. This is the honest version of the promise.
- **Path B — start with an analyst-driven version.** While Path A is being built, fulfill audits manually within the same business day. Be honest in the email: "Your audit is being prepared and will arrive within 24 hours" rather than "5 minutes." Update the page copy to match.

Do not promise "5 minutes" without delivering it. Founders will lose trust the first time the promise doesn't hold.

### 4. Replace `og:image`

In `index.html`, find:

```html
<meta property="og:image" content="https://example.com/og-anchor-seo.png">
```

Replace with a real OG image. Recommended: 1200×630px, brand colors, a clear "free B2B SaaS SEO audit" overline + your logo. Tools: Figma, Canva. Do NOT use stock photography.

### 5. Fact-check the numbers

The hero says "62% of your top-of-funnel queries show AI Overviews." This number was derived from a 50-query SE Ranking AIO sample (`01-niche-data.md`). Before launching, re-pull the data against current SE Ranking AIO leaderboard data for B2B SaaS keywords. The number may have shifted by 5-10 percentage points; update accordingly.

The "$5M-$50M ARR" benchmarks in pain points (12,400/mo median, 184,200/mo top decile) should be re-pulled against your actual basket of comparable B2B SaaS brands.

## Deploying

Any static host works. Recommended (in order of simplicity):

1. **Netlify** — drag-and-drop the folder, get a URL in 30 seconds. Free tier includes form handling.
2. **Vercel** — same flow, slightly faster CDN. Form handling via Vercel functions or external endpoint.
3. **GitHub Pages** — free, simple, no form handling (use external endpoint like Formspree).
4. **Cloudflare Pages** — free, fastest CDN, supports forms via Workers.

For a custom domain (e.g. `anchorseo.com/audit`):

```
CNAME audit anchor-seo.netlify.app
```

## A/B testing the hero headline

The current hero is data-led ("18% lost to AI Overviews"). Two strong alternatives to test against:

- **Pain-led variant:** "Your B2B SaaS site is losing impression share. We'll show you exactly how much, in 5 minutes."
- **Outcome-led variant:** "We help B2B SaaS founders win the AI Search era. Start with a 5-minute audit."

Test via Google Optimize, VWO, or your existing experimentation tool. Hold each variant for at least 2 weeks or 500 form-submit traffic before declaring a winner.

## Avoid these gotchas

1. **Don't oversell the human element.** If the call is with a senior consultant, say so. If it's with a sales rep wearing a consultant hat, say that. Founders smell pitch from miles away.
2. **Don't promise "guaranteed rankings."** Nobody can promise that. Promise diagnostic clarity, not outcomes.
3. **Don't ungate the PDF.** The PDF should be sent via email so you have the lead. But once sent, deliver it without strings.
4. **Don't add a cookie banner unless required.** US-based audience, US-only data — most of the audience doesn't expect a banner. If you target EU traffic, add one.

## How to maintain the page

The numbers in this page age. Quarterly maintenance:

- Re-pull the SE Ranking AIO leaderboard for the B2B SaaS keyword basket. Update the "62%" claim.
- Re-pull benchmark organic traffic for the comparable-brand basket. Update the "12,400 / 184,200" claim.
- Re-audit 10 sample B2B SaaS sites with your technical audit. Update the "70% have a critical issue" claim if it shifts.
- Refresh the case studies / client logos as new ones land.

The page becomes obsolete if the numbers drift. Treat it like a product, not a launch.
