---
name: marketing-ai-visibility-benchmark
description: >
  Benchmark AI-search share of voice for a brand versus competitors across ChatGPT, Perplexity,
  Gemini, Google AI Overview, and AI Mode (SE Ranking) — with the overlay a plain SoV snapshot
  can't give: whether the brand's own social channels even talk about the narratives it's losing
  (Planable post history). Every missing/contested topic cluster comes with "we post about this /
  we're silent" evidence, and the benchmark ends with a one-step offer to launch
  marketing-ai-campaign on the missing clusters. Use when the user asks for AI share of voice,
  an LLM visibility benchmark, "which brands do AI engines cite in our category — and do we even
  cover those topics", AEO/GEO competitive benchmark, or a monthly/quarterly AI-visibility review.
  Requires both the SE Ranking and Planable MCPs. For a pure SoV snapshot without the social
  overlay, use seo-ai-search-share-of-voice from the seo-skills plugin instead.
---

# AI visibility benchmark

Compare AI-search visibility for a target brand against competitors across every major LLM engine, analyse which topic clusters each brand owns — and check whether the brand's own social output backs the narratives it needs to win. The output is a benchmark that says not only *where you're invisible* but *whether you're even trying*.

## Prerequisites

- **SE Ranking MCP** connected.
- **Planable MCP** connected, with the brand's workspace (for the social-narrative overlay).
- User provides: (a) target domain and its brand name, (b) competitor domains and brand names, (c) country (default: `us`), (d) the Planable workspace, and (e) optionally which engines to analyse (default: `ai-overview`, `chatgpt`, `perplexity`, `gemini`, `ai-mode`).

## Connector health check

Verify both MCPs before starting: `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to reconnect it (SE Ranking guide: https://seranking.com/api/integrations/mcp/ · Planable guide: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools).

## Process

1. **Resolve brands** — if the user gives domains but not exact brand strings, `DATA_getAiSearchBrand(target, source)` per domain.

2. **Leaderboard snapshot** `DATA_getAiSearchLeaderboard(primary{target,brand}, competitors[{target,brand}], source, engines[])`
   - Capture share of voice per engine, per brand.
   - **This endpoint is heavy and can 504** on many competitors × many engines. Query one engine at a time (or ≤3 competitors per call) and retry once; if it still fails, fall back to `DATA_getAiSearchOverview` per domain and compare brand_presence / link_presence yourself.

3. **Heatmap table**
   - Rows = brands (target + competitors), columns = engines, cells = share of voice. Highlight the leader per engine and the target's weakest engine.
   - If `previous` values exist, note direction; if `previous` is `null`, this is a first snapshot — a baseline, not growth.

4. **Prompt sampling per brand** `DATA_getAiSearchPromptsByBrand`, `DATA_getAiSearchPromptsByTarget`
   - For each brand: ~10 prompts where the domain is cited as a source and ~10 where the brand is named.
   - Save prompt text + cited sources so the user can validate. AI prompts typically show `volume: 0` — expected, not a value signal; judge clusters by relevance and by which brands the LLM cites.
   - Validate brand-name matches (a brand can surface in unrelated answers); flag ambiguous ones instead of counting them.

5. **Topic clustering — owned / contested / missing**
   - Group prompts by theme. Mark each cluster: **owned** (target appears), **contested** (target + competitors), **missing** (competitors only).
   - Note the target's position within contested clusters (cited 4th of 6 matters).

6. **Social-narrative overlay (signature step)**
   - Pull the brand's recent social history: `list_pages(workspaceId)` → `list_posts(workspaceId, ...)`, last ~90 days.
   - For every missing/contested cluster, scan post texts for the narrative (topical judgment, not substring matching): **social: active** (≥2 recent posts), **thin** (1), **silent** (0).
   - This is the diagnosis a SoV report can't make: *missing in AI + silent on social* = never told the story; *missing in AI + active on social* = telling it where LLMs don't listen — the fix is citable web content + consistency, not more posts.

7. **Synthesis and handoff**
   - 3–5 clusters where the target underperforms, each with: who wins it, example prompts, social overlay verdict, and the concrete next move (post series / citable page / comparison content / FAQ schema).
   - End with the offer: *"Want me to launch `marketing-ai-campaign` on the missing clusters? It will draft the posts in Planable, label them `mkt:<slug>`, and save a baseline passport so `marketing-campaign-impact` can measure the effect."*

## Output format

Create a folder `marketing-ai-visibility-benchmark-{target-slug}-{YYYYMMDD}/` with:

```
marketing-ai-visibility-benchmark-{target-slug}-{YYYYMMDD}/
├── REPORT.md                  # executive benchmark (primary deliverable)
├── 01-leaderboard.md          # raw SoV per engine
├── 02-prompts-{brand}.md      # sampled prompts per brand
├── 03-clusters.md             # owned / contested / missing membership
└── 04-social-overlay.md       # cluster × social activity evidence
```

`REPORT.md` shape:

```markdown
# AI visibility benchmark: {target brand} vs competitors
Market: {country} · Engines: {list} · Social window: {dates}

## Heatmap
| Brand | AI Overview | ChatGPT | Perplexity | Gemini | AI Mode |
|---|---|---|---|---|---|
| {target} | {%} | {%} | {%} | {%} | {%} |

## Cluster ownership × our social reality
| Cluster | Status | Winner | Our social | Diagnosis |
|---|---|---|---|---|
| {theme} | missing | {brand} | silent | never told the story → campaign |
| {theme} | contested (#4 of 6) | {brand} | active | story told, not citable → web content + consistency |

## Top 5 moves
1. {action tied to cluster + overlay evidence}

## Next step
Launch marketing-ai-campaign on clusters {a, b, c} — labels + baseline included.
```

## Tips

- Report zero as zero. If an engine returns no prompts for a brand, say so — never estimate.
- `base_domain` scope is the default; don't narrow to `subdomain` unless asked.
- Respect the Data API rate limit (~10 req/s); pace multi-brand × multi-engine loops.
- This benchmark is designed to be re-run monthly/quarterly and diffed — recommend it, and keep the folder date-stamped so diffs are trivial.
- No sentiment: these tools expose presence and share, not how a brand is talked about. Say so if asked.
