# Changelog

## 1.3.2

Four fixes and a note, all from a second live test run on a different account. The run also confirmed the skills' own warnings were accurate — `unsupportedPages` returned exactly the four platforms predicted, the AI snapshot was ~11 months stale on four of five engines, `previous` was null so the API's `change_percent: 100` was a first-snapshot artefact, and the LinkedIn-vs-Facebook engagement-rate spread was 132x.

- **The Planable chunking table was over-cautious.** It told the skill to split a 41-80-post page into ~6 two-week chunks. The tester pulled five pages as single 90-day calls, including a 48-post Instagram page, with no timeout — roughly 15 wasted calls per run. Now: one call per page first, bisect only on an actual timeout, pre-emptive chunking reserved for several-hundred-post pages.
- **`DATA_getAiSearchOverview` has no `date` field.** The skill said to check one there. Only the prompts endpoints carry it. Corrected in both skills that read AI data.
- **The fourth stat tile was impossible on most runs.** It was mandatory and set to AI share of voice, which needs competitors, which the same skill says not to pull unless asked. Now three tiles always, a fourth only when competitor data exists, with named alternatives rather than an improvised number.
- **The dark-mode chart pair failed its own validator.** `brand.md` recommended `#4FE7AD` mint + `#A8A7FF` soft violet for dark canvases and told the reader to re-run the validator — which fails them on the lightness band (0.833 and 0.764 against a required ~0.48-0.67). Replaced with `#00A382` + `#7C63FF`, darker steps of the same brand hues, validated as passing every check. Mint stays as a non-chart accent. This was a shortcut: the light pair was validated properly and the dark pair was shipped as a suggestion.
- **Added a note about transient SE Ranking 500s** on the AI prompt endpoints — retry once, only surface it if the retry also fails.

## 1.3.1

Repo-convention fix before publishing. `references/brand.md` sat at the plugin root and was addressed as `${CLAUDE_PLUGIN_ROOT}/references/brand.md`. Every other plugin in this repo keeps reference files **inside the skill folder** and addresses them as a plain relative `references/x.md`; nothing here uses `CLAUDE_PLUGIN_ROOT`. The file now lives in each of the three skills' own `references/` folder.

Maintainer note: the three copies are identical by design. Edit one, copy to the other two.

## 1.3.0

All three skills now ship a designed, shareable report on every run, styled from the SER x PLNBL co-brand design system. Plus two robustness rules.

### Reports

- **`ecosystem-ai-visibility` gains a full report** (`REPORT.md`, `prompts.csv`, `index.html`). It previously produced no file at all — the share-of-voice heatmap, the owned/contested/missing clusters and the tracking plan all lived in chat and scrolled away. The heatmap is given room as the most screenshotted thing the skill makes; prompts where the brand is named but a competitor is cited are flagged separately.
- **`ecosystem-demand-campaign` gains a light brief page** (`campaign-brief.md`, `brief.html`), deliberately not a dashboard: why these themes with their volumes, the posts with their Planable links, the baseline, limits. At most one chart. It is a plan, and overbuilding it would read as padding.
- **`ecosystem-footprint-map`'s report is unchanged in scope** but now draws its styling from the shared brand reference rather than describing it inline.

### Brand

- New `references/brand.md`, shared by all three skills: co-brand tokens verbatim, the four sanctioned colour combinations, layout conventions, and chart rules. Reports read as one family.
- **Chart palettes were validated, not assumed.** The full brand palette fails as a categorical chart set on a light surface — mint sits outside the lightness band, green-deep reads gray, two colours fall under 3:1 contrast. The validated two-series pair is `#5C3CFF` violet + `#00B28E` green (passes every check). For three or more categories the skills encode with length and direct labels under a single hue rather than inventing brand hues: a hand-built five-step violet ramp failed the normal-vision floor at 11.6, meaning full-colour-vision readers couldn't separate adjacent segments.
- Brand colours carry identity (the brand's own bar); competitors stay neutral. Colour follows the entity, never the rank.

### Robustness

- **Plain files are written before the HTML** in all three skills, so a rendering failure at the end of a long run cannot cost the analysis — or, for the campaign skills, leave drafts live in Planable with no brief to explain them.
- **A narrow question now gets a narrow answer.** Each skill answers a bounded question directly and offers the full run in one line, instead of always producing every deliverable. The campaign skills additionally never create posts in a workspace off the back of a question that didn't ask for them.

## 1.2.0

Makes the interactive report a guaranteed deliverable in `ecosystem-footprint-map`, after a tester produced an excellent one and then noted they had to ask for it.

- **`index.html` is now built every run**, not offered. The previous wording said "offer an optional interactive HTML version — don't build it unasked", which is exactly why it only appeared on request. The markdown is the record; the HTML is what gets read and sent on, and a run that stops at markdown under-delivers against the same analysis presented visually.
- **The report's structure is specified**, so it looks the same every time: header with real coverage counts, the three-channels-are-different-dates callout, four stat tiles, a filterable and sortable matrix, two stacked-bar charts, share of voice when competitors were pulled, the routed next steps, and a collapsed "limits of this map".
- **The dating callout is mandatory** whenever the AI snapshot is older than the search and social window, which it usually is.
- **Charts follow the `dataviz` skill** — form first, colour by job, validated palette, hover layer, legend for two or more series.
- **Delivery is pinned:** local files always. Google Docs, Sheets or Notion only when the user asks, with a note on which copy is canonical. A previous run improvised into Google Docs because the connector happened to be there, which made the output vary between runs.

## 1.1.1

Fixes the write-back-to-SE-Ranking step in both campaign skills, reported from a live test run.

- **`ecosystem-ai-visibility` tried to add 1,337 prompts to the user's SE Ranking project** and hit the API's 100-per-call limit. It was passing the full research set from step 3 instead of the campaign's chosen target clusters. The step now states plainly that research set and tracking set are different things, targets 10–30 prompts, treats 100 as an absolute ceiling, and requires the list and count to be shown before anything is written.
- **Same fix applied to `ecosystem-demand-campaign`'s optional keyword tracking**, which had the same shape of instruction and the same risk.
- Both now handle a limit error gracefully: say what happened, state that the campaign itself is unaffected because the drafts and passport are already in place, offer a smaller set. No blind retries.
- Both restate that tracking is optional — the passport plus `ecosystem-footprint-map` already make the campaign measurable without it.

Note for maintainers: `marketing-ai-campaign` and `marketing-seo-campaign` in the `marketing-skills` plugin carry the original wording and the same bug.

## 1.1.0

Both-directions pass. The three skills always required both connectors, but the two campaign skills were written as if the user always sat in the search seat. This release makes either entry point work.

- **Social-seat trigger phrases** added to `ecosystem-demand-campaign` and `ecosystem-ai-visibility`, so a user who never says "SEO" or "keyword" still reaches the right skill: "fill my content calendar for next month", "give me a month of posts for this client", "does ChatGPT know we exist", "a client asked if AI mentions them".
- **Intake follows the user's order.** If they open with a workspace or client, that is confirmed first and the website asked for afterwards, in plain words. If they open with a domain, the reverse. Neither skill infers the domain from Planable pages — social page links point at social profiles and universal pages carry placeholder URLs, so it asks.
- **Missing-connector wording rewritten.** It now says "connect" rather than "reconnect" and names what the connector is for, since a Planable-first user often has never held an SE Ranking account. Applied to all three skills.
- Trigger phrases were trimmed on the search side to keep both `description` fields under the 1024-character cap.

## 1.0.0

Initial release of the **Ecosystem Skills** plugin — the three core SE Ranking + Planable use cases from the joint ecosystem launch, packaged on their own.

### The idea

A small, focused plugin for the three use cases the two teams promote together, rather than the full 23-skill `marketing-skills` pipeline. It's organized as a loop — **See → Act → See again** — held together by the campaign contract: every campaign gets a Planable label (`mkt:{slug}`) and a passport recording its baseline, and re-running the footprint map diffs against it.

Every skill carries the `ecosystem-` prefix so this plugin coexists with `marketing-skills`, `seo-skills` and `smm-skills` without duplicate skill names.

### 3 skills

- **ecosystem-footprint-map** — new. Joins search rankings, AI-answer presence and social performance on a shared topic taxonomy, classifies every topic by which channels carry it, and routes each gap to the skill that closes it. Also serves as the re-measurement step for both campaign skills.
- **ecosystem-demand-campaign** — the ecosystem-branded equivalent of `marketing-seo-campaign`. Search demand into a drafted, labelled, passport-backed social campaign in Planable.
- **ecosystem-ai-visibility** — the ecosystem-branded equivalent of `marketing-ai-campaign`. AI-answer prompt gaps into citable social content with baseline share of voice recorded.

Auto-registers both remote MCP servers (SE Ranking + Planable) on install.

### Notes for maintainers

`ecosystem-demand-campaign` and `ecosystem-ai-visibility` are adapted copies of `marketing-seo-campaign` and `marketing-ai-campaign` from the `marketing-skills` plugin. The process steps are identical; what changed is the skill names and the cross-references — the originals hand off to `marketing-campaign-impact`, `marketing-gap-matrix` and `marketing-report`, which don't exist here, so those handoffs now point at `ecosystem-footprint-map`.

**Fixes to the shared process steps need applying in both plugins.** The two copies will drift otherwise.
