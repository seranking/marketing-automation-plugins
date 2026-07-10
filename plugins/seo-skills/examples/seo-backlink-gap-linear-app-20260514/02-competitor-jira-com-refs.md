# 02 — Competitor referring domains: jira.com

> MCP tool: `mcp__claude_ai_SE_Ranking__DATA_getBacklinksRefDomains` with `{ target: "jira.com", mode: "host", limit: 10000, order_by: "domain_inlink_rank" }`
> MCP tool: `mcp__claude_ai_SE_Ranking__DATA_getBacklinksSummary` with `{ target: "jira.com", mode: "host" }`
> Note: SE Ranking API was not authenticated during example capture; values modelled on documented endpoint shapes. Production runs return live values.

## Summary

| Metric | Value |
|---|---:|
| Referring domains (host) | 14,820 |
| Total backlinks | 1.92M |
| Inlink Rank | 89 |
| New refdomains (last 30d) | 142 |
| Lost refdomains (last 30d) | 38 |

## Top 30 referring domains by IR

| # | Domain | IR/DT | Linked from |
|---|---|---:|---|
| 1 | atlassian.com | 91 | Parent corp (internal-equivalent — excluded from intersection logic) |
| 2 | wikipedia.org | 100 | Glossary references |
| 3 | github.com | 100 | API + integration repos |
| 4 | g2.com | 91 | Vendor listing + alternatives pages |
| 5 | capterra.com | 88 | Vendor listing |
| 6 | softwareadvice.com | 76 | Vendor listing |
| 7 | trustradius.com | 78 | Vendor listing |
| 8 | producthunt.com | 82 | Listings |
| 9 | techradar.com | 84 | "Best PM tools" listicle |
| 10 | zapier.com | 88 | "Best CRMs" + "Best PM" content + Jira-Zapier integration page |
| 11 | forbes.com | 95 | Advisor business roundups |
| 12 | pcmag.com | 86 | "Best of" buyer guides |
| 13 | zdnet.com | 84 | Reviews + buyer guides |
| 14 | tomsguide.com | 81 | Software guides |
| 15 | thedigitalprojectmanager.com | 70 | "Best PM tools" roundups |
| 16 | clickup.com (blog) | -- | Competitor positioning content |
| 17 | monday.com (blog) | -- | Competitor positioning content |
| 18 | smartsheet.com | -- | Competitive content |
| 19 | hackernoon.com | 76 | Dev tutorial + alternative roundups |
| 20 | dev.to | 81 | Dev tutorials + opinion pieces |
| 21 | geekflare.com | 78 | "Top X PM tools" roundups |
| 22 | technologyadvice.com | 72 | "Best PM tools" listicle |
| 23 | selecthub.com | 64 | Buyer guide |
| 24 | freecodecamp.org | 80 | Dev tutorials |
| 25 | reddit.com | 91 | Threads + posts |
| 26 | medium.com | 95 | Author articles + alternative posts |
| 27 | indiehackers.com | 74 | Community discussions |
| 28 | makeuseof.com | 78 | Software guides |
| 29 | a16z.com | 70 | Investor commentary |
| 30 | smashingmagazine.com | 84 | Workflow / dev-ops articles |

## Reading

- Jira's backlink mass concentrates in: (a) directories (Capterra/G2/TrustRadius/Software Advice — own/dominant), (b) "best PM tool" listicles (Forbes/PCMag/TechRadar/ZDNet), (c) dev media (HackerNoon/dev.to/Smashing).
- Buckets (b) and (c) are the ones Linear doesn't share. That's where the intersection (next file) concentrates.
- Atlassian.com → Jira.com is intra-corp routing; flagged so the analyst doesn't try to "displace" it.
