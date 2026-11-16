---
name: marketing-serp-fit
description: >
  Diagnose why a page is not ranking by reading the SERP backwards — and check the recommended
  fix against the brand's real audience. Identifies the page type Google rewards for the target
  keyword, scores the candidate page against that pattern from multiple persona perspectives
  (SE Ranking + page teardowns), then validates the recommended format and angles against what
  this brand's audience actually engages with (Planable top-post analytics) so the wireframe
  isn't just SERP-correct but audience-correct. Use when the user asks "why isn't this page
  ranking", "page type mismatch", "SXO", "search experience optimization", "intent mismatch",
  "will this format work for our audience", or wants a wireframe grounded in both the SERP and
  their own engagement data. Requires both MCPs. For the pure SERP diagnosis without the
  audience check, use seo-sxo from the seo-skills plugin instead.
---

# SERP fit diagnosis

Diagnose why a "well-optimized" page doesn't rank. Reads the actual SERP for the target keyword, infers the page type Google is rewarding, scores the candidate page against that pattern from multiple persona perspectives, and recommends the page format that would win the SERP — then runs the reality check a SERP-only diagnosis can't: does this brand's own audience engage with that kind of content?

> **Acknowledgements:** SXO-as-a-skill framework originated in `claude-seo` by AgriciDaniel (with the original concept credited to Florian Schmitz, Pro Hub Challenge). MIT-licensed both directions; this implementation is independent but the framing is theirs.

## Prerequisites

- SE Ranking MCP server connected.
- **Planable MCP** connected, with the brand's workspace (for the audience-signal check).
- Claude's `WebFetch` tool available.
- User provides: (a) target page URL, (b) target keyword the page is meant to rank for, (c) the Planable workspace, optionally (d) target country (default `us`).

## Process

1. **Validate inputs.** Both URL and keyword are required. If keyword missing, ask the user — don't infer.

2. **Pull the SERP** `DATA_getSerpResults(query[], language_code, country_code, result_type)` — `language_code` is required
   - The response is large (about 70 KB, roughly 100 organic results rather than 10) and is usually saved to a file instead of shown inline. Extract only what this step needs with a short script — organic items with `rank_group` 1–10, plus any feature item you need such as `ai_overview` — rather than loading the whole result. Organic rank is in `rank_group`; `position` is a label such as `center`, not a number.
   - Top 10 organic results with URL, title, snippet.
   - SERP features: AI Overview presence, People Also Ask, image carousel, video carousel, shopping pack, Twitter pack, Featured Snippet, etc.
   - **Mode selection (cost driver — read this).** SERP feature data (AIO/PAA/carousels) only comes back when the task runs with `result_type=advanced`. That is the most expensive single call this skill makes: ≈10 units per keyword (measured on a live run; the tool schema also states 10).
     - **Default — full mode:** runs `result_type=advanced`. Returns features + organic. Use when persona scoring needs PAA / AIO / pack signals (most cases).
     - **Lite mode (`result_type=standard`):** organic, ads and featured-snippet items; other SERP features are only named in the summary, without content. Cheaper than advanced. Use when (a) the user is screening many keywords and SERP features aren't load-bearing, (b) credits are constrained, (c) the user explicitly asks for a cheap pass. The persona scoring still runs but the SERP-features row in `SXO-REPORT.md` will read `(skipped — lite mode)` and the dominant-pattern detection will rely on URL/title heuristics alone.
     - Surface the chosen mode + estimated cost up front. If the user didn't specify and the keyword looks ad-heavy or commercial-high-volume, recommend lite mode first and re-run in full mode only if dominant-pattern confidence is low.

3. **Pull AIO context** from the step 2 SERP result
   - In full mode the advanced SERP result carries the AI Overview item when one exists for the keyword: capture its answer text and the sources it cites.
   - In lite mode there is no AIO data — mark the row `(skipped — lite mode)`. Don't use `DATA_getAiSearchOverview` here: it is domain-level (how often AI answers cite a whole site) and takes no keyword.
   - Note which top-10 organic results are also cited in the AIO.

4. **Fetch user's page + top 3 winners** `WebFetch` (always) + the Firecrawl MCP's `firecrawl_scrape` (when available)
   - **WebFetch first** (free): pull markdown for the user's page + top 3 winners. Extract `<title>`, all H-tags, primary content structure (numbered list / table / prose / Q&A), word count, image mentions, comparison-table presence, CTA mentions.
   - **Firecrawl second** (4 Firecrawl credits typical — 1 per page) — recovers what WebFetch can't show:
     - JSON-LD `@type`s per page (Product, FAQPage, BreadcrumbList, Article, Review, ItemList, etc.) — these are **load-bearing** for page-type classification in step 5. WebFetch's markdown can't see schema.
     - `og:title` / `og:image` / `twitter:card` from `metadata`.
     - Real `<title>` length (the markdown first-heading is sometimes wrong).
   - **Screenshots (opt-in, +4 Firecrawl credits):** when the user asks for them, also call `firecrawl_scrape` with `formats: ["screenshot"]` on the user's page + top 3 winners. Save as `screenshots/{page}.png`. Reference in the wireframe (step 8) to ground recommendations in the visual layout, not just the text outline.
   - **If Firecrawl unavailable (or the user asked to skip it):** WebFetch portion runs. Page-type classification in step 5 falls back to URL/title heuristics + content-structure heuristics only — schema-based classification is skipped. Note in `02-page-type-classification.md`: `Schema-based classification: skipped — Firecrawl required.` Confidence in dominant-pattern detection drops accordingly.

5. **Classify each top-10 result by page type**
   - Use the heuristics in `references/page-type-patterns.md`.
   - For each: assign one of {comparison, alternatives, listicle, how-to, definition, product, editorial, forum, video}.
   - Note signals that informed the classification (URL pattern, title pattern, schema, content structure).

6. **Detect the dominant pattern**
   - Count types in top 10. If one type ≥ 6, that's dominant.
   - If two tie at 4–4, the SERP is "split intent" — both work; commercial vs informational angle determines which to choose.
   - Cross-reference with SERP features: video carousel → expect ≥ 2 video results; PAA → expect informational results; shopping pack → commercial intent dominant; AIO → informational consensus.

7. **Score the user's page** against the dominant pattern × 4 personas
   - Use the rubrics in `references/persona-rubrics.md`.
   - 4 personas: Skimmer, Researcher, Buyer, Validator.
   - 0–10 per persona. Apply the intent-weighting profile (also in persona-rubrics.md) and multiply the weighted sum by 10 to get a single 0–100 SXO score.

7b. **Audience-signal check (signature step)** — Planable
   - Pull the brand's top posts for the last ~90 days: `list_pages(workspaceId)` → `get_post_metrics` (one page per call, `limit` ~20).
   - Map the recommended page format and angles to post analogues: comparison page ↔ "X vs Y" posts; how-to ↔ tips/steps posts; listicle ↔ numbered-list posts; data-led ↔ stat posts. Question hooks in top posts ↔ PAA-style sections.
   - Verdict per recommended element: **audience-confirmed** (analogous posts overperform the account norm), **audience-risky** (analogous posts underperform), **untested** (no analogous posts). Cite the actual posts.
   - This doesn't overturn the SERP verdict — Google's reward pattern still decides the page type — but it tunes the execution: which sections lead, which hooks open, what the promotion posts should look like. Note it in the persona-score table's Notes column where relevant.

8. **Synthesise verdict and wireframe**
   - If user's page type matches dominant: SXO score reflects how well it executes the pattern. Recommend specific persona-targeted improvements.
   - If user's page type does NOT match dominant: this is the "page-type mismatch" case. Output a wireframe for the dominant page type, anchored in observed patterns from the top 3 winners — with the audience-signal verdicts marked on each wireframe block.
   - Write `SXO-REPORT.md`.

## Output format

Create a folder `marketing-serp-fit-{target-slug}-{YYYYMMDD}/` with:

```
marketing-serp-fit-{target-slug}-{YYYYMMDD}/
├── 01-serp-snapshot.md            (top 10 + features + AIO)
├── 02-page-type-classification.md (each top-10 result classified)
├── 03-user-page-fingerprint.md    (the candidate page's structure)
├── 04-persona-scores.md           (4 personas × current page)
├── 05-recommendation.md           (verdict + page-type-winning wireframe)
├── 06-audience-signals.md         (top posts × recommended elements — the audience verdicts)
├── screenshots/                   (only if screenshots were requested: candidate.png + winner-1/2/3.png)
└── SXO-REPORT.md                  (executive summary deliverable)
```

`SXO-REPORT.md` shape:

```markdown
# SXO Report: {URL} for keyword "{keyword}"

> Snapshot dated {YYYY-MM-DD} · Country: {country}

## SERP profile
- Top 10 page types: {comparison: 4, listicle: 3, editorial: 2, video: 1}
- Dominant pattern: **{pattern}** ({n} of 10)
- SERP features: AIO ✓ ({n} citations), PAA ✓ ({n} questions), Image carousel ✗, Video carousel ✗, Shopping pack ✗
- Intent: {informational | commercial-investigation | transactional | navigational}

## Your page
- Page type: **{detected type}**
- Page-type match with dominant: **{✓ match | ✗ MISMATCH — see Verdict}**
- Word count: {n}
- Primary content structure: {prose | numbered-list | table | step-blocks | Q&A | mixed}

## SXO score: **{score}/100**

| Persona | Weight | Score | Notes |
|---|---|---|---|
| Skimmer | {%} | {n}/10 | {1-line note} |
| Researcher | {%} | {n}/10 | {1-line note} |
| Buyer | {%} | {n}/10 | {1-line note} |
| Validator | {%} | {n}/10 | {1-line note} |

## Verdict

{One paragraph. If page type matches: "Your page is the right type for this SERP. The score gap is {X} points — see persona-specific gaps below." If MISMATCH: "Your page is a {your type} but the SERP rewards {dominant type}. No amount of on-page optimization will close the gap; ship a {dominant type} page instead. Wireframe below."}

## If MISMATCH — wireframe for the winning page type

\`\`\`
{Page title pattern — e.g., "{Brand A} vs {Brand B}: 2026 Comparison"}

[Hero / TL;DR — first 200 words answer the comparative question]
[Comparison table — must be visually dominant]
[Section per dimension — each with H2 named after the dimension]
[Verdict / recommendation — explicit, justified]
[FAQ — top 3–5 PAA questions]
[Schema — Product (×2) + BreadcrumbList + FAQPage]
\`\`\`

## If MATCH — top 3 changes by persona

1. {Skimmer}: {specific change}
2. {Researcher}: {specific change}
3. {Buyer or Validator}: {specific change}

## Audience reality check
| Recommended element | Post analogue | Audience verdict | Evidence |
|---|---|---|---|
| {e.g. comparison table leads} | "X vs Y" posts | audience-confirmed | {post, ER vs norm} |
| {e.g. long-form explainer} | long text posts | audience-risky | {post, ER vs norm} |

## Raw data
- 02-page-type-classification.md — every top-10 result, classified
- 03-user-page-fingerprint.md — your page's signals
- 04-persona-scores.md — full persona-by-persona breakdown
```

## Tips

- Respect rate limit: 10 req/sec. The SERP calls in step 2/3 are fast; WebFetch calls in step 4 dominate latency, not API.
- **Cost is mode-dependent.** The advanced SERP call is ≈10 units per keyword; the rest of the run is ordinary Data API calls. To quote a total, read `DATA_getSubscription` before and after the first run and use the difference. Always check `DATA_getSubscription` before running and surface the estimate against remaining units. Step 4 adds 4 Firecrawl credits when Firecrawl is available, +4 more if screenshots are requested. Skip both if the user asks.
- **`result_type=advanced` is the only way to get AIO / PAA / pack data.** The standard result returns organic, ads and featured-snippet items; other features are only named in its summary. Don't try to reconstruct SERP features from organic results — that's the cost the user is paying for.
- Page-type classification is a heuristic — `references/page-type-patterns.md` documents the signals so users can override. If the heuristic gets a result wrong, edit that file with the correction.
- The 4 personas are opinionated. They come from the framework's original source — don't invent more without good reason.
- The SXO score is directional. An 85/100 doesn't guarantee ranking; a 35/100 strongly suggests the page won't break through. Treat as a diagnostic, not a forecast.
- When the dominant pattern is split-intent (4-4), ship two pages — one per intent — rather than trying to make one page serve both. Google's SERPs reflect this split for a reason.
- The wireframe in MISMATCH mode is a starting point. The user still needs to write the content. This skill diagnoses; `marketing-content-brief` produces the writer-ready brief with distribution attached.
- An account with very few posts (<10 in 90 days) makes the audience check unreliable — mark everything "untested" and say so rather than reading noise as signal.
