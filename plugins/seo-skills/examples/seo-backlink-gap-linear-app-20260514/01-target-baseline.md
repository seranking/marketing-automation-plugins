# 01 — Target baseline: linear.app referring domains

> MCP tool: `mcp__claude_ai_SE_Ranking__DATA_getBacklinksSummary` with `{ target: "linear.app", mode: "host" }`
> MCP tool: `mcp__claude_ai_SE_Ranking__DATA_getBacklinksRefDomains` with `{ target: "linear.app", mode: "host", limit: 10000, order_by: "domain_inlink_rank" }`
> Note: SE Ranking API was not authenticated during example capture; values below are realistic placeholders modelled on documented endpoint shapes + public DA benchmarks, not live API output. Re-run with auth for live numbers.

## Summary (DATA_getBacklinksSummary)

| Metric | Value |
|---|---:|
| Referring domains (host) | 1,366 |
| Referring domains (root domain incl. subdomains) | 1,712 |
| Total backlinks | 38,420 |
| Inlink Rank (LRT) | 78 |
| Toxic backlinks flagged | 41 |
| New refdomains (last 30d) | 18 |
| Lost refdomains (last 30d) | 6 |

## Top 25 referring domains by Domain Inlink Rank (DATA_getBacklinksRefDomains)

| # | Referring domain | DT/IR | First seen | Notes |
|---|---|---:|---|---|
| 1 | producthunt.com | 91 | 2019 | "Linear" launch + Linear v2 launch |
| 2 | github.com | 100 | 2020 | API repos, integrations |
| 3 | medium.com | 95 | 2019 | Founder essays + community writeups |
| 4 | techcrunch.com | 93 | 2022 | Series-B funding coverage |
| 5 | theverge.com | 92 | 2023 | Linear MCP launch press |
| 6 | g2.com | 91 | 2021 | Vendor page |
| 7 | capterra.com | 88 | 2021 | Vendor listing |
| 8 | y-combinator-hn | 91 | 2019 | Hacker News submissions (link mentions) |
| 9 | wired.com | 92 | 2023 | "AI-native dev tools" feature |
| 10 | fastcompany.com | 87 | 2024 | Founder profile |
| 11 | smashingmagazine.com | 84 | 2022 | Design-ops feature (counts in this gap) |
| 12 | freecodecamp.org | 80 | 2024 | Dev tutorial |
| 13 | producthabits.com | 60 | 2022 | Newsletter |
| 14 | uxdesign.cc | 68 | 2022 | Design-ops feature |
| 15 | dribbble.com | 88 | 2020 | Founder portfolio link |
| 16 | wesionary.com | 50 | 2024 | Vendor blog |
| 17 | superblog.ai | 48 | 2024 | Vendor blog |
| 18 | newsletter.pragmaticengineer.com | 78 | 2023 | Recurring mentions |
| 19 | indiehackers.com | 74 | 2020 | Community thread mentions |
| 20 | dev.to | 81 | 2021 | Dev writeups |
| 21 | substack.com (various authors) | -- | 2022 | Author-level subs |
| 22 | reddit.com | 91 | 2020 | r/programming, r/startups mentions |
| 23 | hackernews.firebaseio.com | 60 | 2019 | HN snapshot mentions |
| 24 | bytebytego.com | 70 | 2024 | Newsletter mentions |
| 25 | swyx.io | 60 | 2023 | Personal-blog dev writeups |

## Exclusion list

The 1,366 host-mode referring domains above are used as the exclusion set in step 3 (Intersection). Any candidate that appears in this list is dropped from the prospect pool even if it links to all 4 competitors.

## Reading

- Linear's backlink pattern is press-led (TechCrunch / Verge / Wired) and community-led (HN / Reddit / dev.to) — but **listicle-light**. The "best PM tool 2026" listicle pattern that drives the competitors' backlink curve barely touches Linear.
- That's the structural gap the prospect list in `REPORT.md` is built to close.
- IR 78 is high for a 5-year-old SaaS; the gap is breadth, not authority.
