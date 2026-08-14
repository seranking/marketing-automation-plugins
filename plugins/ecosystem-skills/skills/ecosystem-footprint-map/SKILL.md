---
name: ecosystem-footprint-map
description: >
  Line up a brand's search rankings (SE Ranking), AI-answer presence (SE Ranking AI Search) and
  social performance (Planable) side by side, grouped by topic — then report which topics all three
  channels carry, which only one carries, and which gaps are the next move. Use for "see our whole
  footprint", "which topics do our channels agree on", "what ranks that we've never posted about",
  "which of our top posts has no search play", "cross-channel topic analysis", "our social and SEO
  teams are working blind to each other". Also the re-measure step 2–8 weeks after a campaign — it
  diffs against the passport left by ecosystem-demand-campaign or ecosystem-ai-visibility. Analyses
  and routes only; hand off to those two skills to act on a gap. Requires both MCPs.
---

# Whole-footprint map

Put the topics a brand ranks for, the prompts it gets cited in, and the posts that actually perform **side by side, grouped by topic**. What surfaces is the pattern: which topics all three channels agree on, which one channel is carrying alone, and where a competitor's channels disagree.

This is a **routed analysis, not a report**. Every topic ends with a state and a move. It reads only — nothing is written to Planable or SE Ranking.

> **Scope note (read this).** This skill reports *co-occurrence by topic*. It cannot show causation. Never write or imply that a social post caused a ranking, or that a ranking caused an AI citation — the data supports "these move together", nothing stronger. If the user asks which channel is driving the others, say plainly that these MCPs can't answer that.

## Prerequisites

- **SE Ranking MCP** connected (Data API; a project is optional and adds ranking trend + AI Result Tracker history).
- **Planable MCP** connected, with the brand's workspace and its social pages.
- The user provides: (a) target domain + brand name, (b) the Planable workspace, (c) market country (default `us`), (d) the period (default: last 90 days), and optionally competitor domains + brand names, or their own topic taxonomy.

**Why 90 days:** social needs enough posts to show a pattern. A 30-day window on a low-cadence account produces a matrix built on four posts, which is noise. Warn the user if the window yields fewer than 15 posts and offer to widen it.

## Connector health check

Verify both MCPs before starting: `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to connect it, naming what it's for rather than reporting an error — SE Ranking is the search and AI-answer side (https://seranking.com/api/integrations/mcp/), Planable is the social side (https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools). Say "connect", not "reconnect", unless you know they had it: a user arriving from one product often has never held an account on the other.

## A narrow question gets a narrow answer

The full run pulls three channels, builds the topic join and produces four deliverables. That's right when someone asks for the map. It's wrong when they ask one question.

If the user asks something specific and bounded — *"what ranks that we've never posted about?"*, *"which of our top posts has no search play?"*, *"are we losing AI answers to Semrush?"* — pull only the channels that question needs, answer it directly in the chat, and then offer the full run in one line: *"That's the short answer. Want the full footprint map — all three channels, the matrix and the report?"*

Don't produce a four-file deliverable for a question that wanted a list. Don't skip the offer either: most people asking the narrow question don't know the wider one exists.

## Which seat is the user in?

The analysis is identical either way, but the **order of the output changes**. Detect from how they asked, or ask once:

- **Search seat** (SEO / AI-search / multi-channel lead) — lead with the full matrix and the competitor cracks. Their question is "how does our whole presence hold together".
- **Social seat** (social manager / content lead) — lead with the untapped-topics shortlist and the topics social surfaced first. Their question is "what should I post, and what does social already know that nobody else has".

Same data, different first section. Don't produce two reports.

## Process

### 1. Resolve scope

Confirm domain, brand string, country, period, and workspace. If the user gives a domain but not the exact brand string, call `DATA_getAiSearchBrand(target, source)` for the name SE Ranking attributes to it — do the same for each competitor. If the workspace is ambiguous, call `list_workspaces` and let the user pick. Do not guess.

### 2. Pull the search channel

- `DATA_getDomainKeywords` — the target's ranking keywords in the target country. This is the backbone of the topic taxonomy in step 5, so pull generously but cap it (top ~1,000) unless the user asks for everything.
- `DATA_getDomainPages` — which pages carry which topics. Useful later for "ranks but thin on-page".
- `DATA_getDomainOverviewWorldwide` — overall footprint for context.
- **Trend (only if a project exists):** `PROJECT_getPositionHistory` (`avg_pos` or `visibility`) — a topic that is rising or slipping is a different move from a flat one. Skip silently if there is no project.
- **Competitors (only if provided or requested):** `DATA_getDomainCompetitors` to confirm the real organic set, then `DATA_getDomainKeywords` per competitor. Note that `DATA_getDomainCompetitors` returns the full set with no `limit`/`offset` (~60KB for popular domains) and the harness may write it to a file — read the file, parse `{data: [...]}`, sort by `common_keywords` desc.

### 3. Pull the AI-answer channel

- `DATA_getAiSearchOverview(target, source, brand?)` — brand presence, link presence, average position. **Read `previous` before quoting any change.** If it's `null`, this is a first snapshot: report current values as a baseline and never present a `change_percent` of 100 as growth.
- `DATA_getAiSearchPromptsByTarget(target, engine, source)` — prompts where the domain is cited as a source.
- `DATA_getAiSearchPromptsByBrand(brand, engine, source)` — prompts mentioning the brand by name.
- **Competitors:** `DATA_getAiSearchLeaderboard(...)` for share of voice. **This endpoint is heavy and 504s** when passed many competitors × many engines at once. Query **one engine at a time** (or ≤3 competitors per call) and retry once on timeout. If it still fails, fall back to `DATA_getAiSearchOverview` per competitor and compare presence yourself.
- **History (only if the AI Result Tracker is set up):** `PROJECT_getPromptsRankings`, `PROJECT_getLlmStatistics`.

Four things to hold onto — all four were confirmed on a live run:

- **The prompt endpoints return the entire AI answer body**, often 3,000–5,000 characters each. Pulling 50 prompts × 5 engines will flood the context and crash the run. **Cap `limit` at 20 per engine**, read the `prompt`, `type` and `links` fields, and skim the answer only to validate a match — never carry full answer bodies into the analysis.
- **Read the snapshot date off the prompts endpoints and report it.** `DATA_getAiSearchPromptsByTarget` and `DATA_getAiSearchPromptsByBrand` return a `date` field; **`DATA_getAiSearchOverview` does not — don't look for one there.** Live runs on two different accounts came back dated ~11 months before the run, on four of the five engines. AI-answer data can be far staler than the search and social channels sitting beside it in the same matrix, and presenting them as one current picture is misleading. Put the date, per engine where they differ, in the report header.
- **AI prompts almost always return `volume: 0`.** They're conversational queries, not keywords. Expected, and **not** a signal of low value. Never rank an AI topic by search volume.
- **Validate brand-name matches, and check the `links`.** A brand can surface in loosely related answers, and it can be *named in the answer text while a competitor's page is the cited source* — a materially different (and more useful) finding than plain presence. Report those separately.

### 4. Pull the social channel — with Planable's freshness rules

Planable metrics are cached and Instagram syncs slowly. Follow this sequence or the numbers will be stale or the calls will time out.

**4a. Read first, check freshness**

```
list_pages(workspaceId)                                    → pageIds
get_page_metrics(workspaceId, pageIds, startDate, endDate)
```

For each page, read `lastMetricAt`. Within the last 3 hours → fresh, skip to 4c. Older or missing → 4b. Also record the `posts` count per page now; you need it to plan chunk size — if the field is absent, call `get_post_metrics_summary(workspaceId, [pageId], startDate, endDate)` and read `totalPosts` instead.

`get_page_metrics` returns `unsupportedPages` for pages whose platform reports no metrics. **Read that array — do not assume which platforms it contains.** On a real run it returned Google My Business, Threads, **Twitter/X and Pinterest**, so a brand can lose four channels' worth of social signal without warning. Drop those pages and list them in the data notes. Their absence is a connector limit, not a content gap.

**4b. Refresh stale pages**

Call `refresh_page_metrics` for the stale pages. For **Instagram**, tell the user first ("Instagram metrics are syncing — this can take 1–2 minutes"), then wait **60 seconds** before re-reading; **15–20 seconds** for every other platform. Re-call `get_page_metrics` and check `lastMetricAt` again. Still stale → wait 30s, retry once. After two retries, tell the user and let them choose between waiting or proceeding with the most recent available data.

**4c. Fetch post-level metrics**

**Always call `get_post_metrics` with exactly one `pageId` per request.** Multi-page calls compound timeouts.

**Ask for the whole window in one call per page, and only split if that fails.** A live run pulled five pages across 90 days as single calls, including a 48-post Instagram page, with no timeout. Pre-emptive chunking cost about 15 wasted calls per run at that volume. So: one call per page first.

If a call does time out, bisect — halve the window and retry both halves. Below 48 hours stop bisecting, skip that range, and record it in the data notes ("2 IG posts in May 12–14 likely missing — endpoint timed out under a 48h window"). The rest of the dataset is still usable.

Only reach for pre-emptive chunking when a page has several hundred posts in the window, or when a first attempt has already timed out on that workspace.

Also pull `list_posts(workspaceId, ...)` for the window — you need the **post text**, not just the metrics, to assign posts to topics in step 5.

**Normalize before comparing — but only within a platform.** Raw engagement favours the biggest page, so use engagement rate (engagement ÷ impressions). **Engagement rate is not comparable across platforms**, even though the connector returns it in a normalized field: a real run showed LinkedIn at 23.6%, TikTok 3.1%, Instagram 1.2%, Facebook 0.18%, YouTube 0.05% — a 470× spread driven by how each platform counts impressions, not by content quality. Rank topics within each platform, then combine the per-platform ranks. Never sort one list of topics by raw ER across platforms; LinkedIn will win every time.

### 5. Build the topic join (the signature step)

The three datasets **share no common key**. Keywords, AI prompts and social posts each have their own vocabulary, and nothing joins them automatically. This step is where the skill earns its output — and where it fails silently if done carelessly.

1. **Derive the taxonomy from the search side.** Cluster the target's ranking keywords into 8–15 named topics. Search keywords are the largest and most structured of the three sets, so they make the most stable backbone. **Deduplicate first** — the same keyword appears once per ranking URL, so a head term can show up five or six times and inflate its topic. If the user supplied their own taxonomy, use theirs instead and skip this.
2. **Assign AI prompts into those topics.** Judge meaning, not string overlap — "which tool should I use to schedule posts" belongs to *social scheduling* even with no shared words.
3. **Assign social posts into those topics** by reading post text. Same rule: topical judgment, not substring matching.
4. **Keep an `unmapped` bucket and always report it.** Prompts and posts that fit no search-derived topic are the most interesting rows in the analysis — an unmapped cluster of posts is often a topic social invented that search has never seen. Never drop them silently.
5. **Show your work.** The evidence file must name which keywords, prompts and posts landed in each topic. Nobody trusts a matrix they can't audit, and this is the step most likely to be wrong.

**Minimum data to judge a channel.** State these plainly and mark a channel `insufficient data` rather than `absent` when it fails one:

| Channel | Floor |
|---|---|
| Search | ≥ 20 ranking keywords in the country |
| AI answers | ≥ 10 prompts across the queried engines, and a snapshot no more than ~3 months old |
| Social | ≥ 15 posts with metrics in the window |

`absent` and `insufficient data` are different findings. Confusing them invents gaps that aren't there.

### 6. Classify every topic

Every topic gets exactly one state. All seven combinations are covered — if a topic doesn't fit, the topic join in step 5 is wrong, not the table.

| State | Search | AI | Social | The move |
|---|---|---|---|---|
| **All three** | ✓ | ✓ | ✓ | Scale it. This is the proven core — protect it. |
| **Search + AI, no social** | ✓ | ✓ | — | The untapped shortlist. Proven demand nobody has posted about → `ecosystem-demand-campaign` |
| **Search + social, no AI** | ✓ | — | ✓ | You own the topic but AI answers don't cite you. Make the claims quotable → `ecosystem-ai-visibility` |
| **AI + social, no search** | — | ✓ | ✓ | Visible where the conversation happens, absent from the SERP. Check whether search volume exists, then capture it. |
| **Search only** | ✓ | — | — | Ranks into silence. Distribute it → `ecosystem-demand-campaign` |
| **AI only** | — | ✓ | — | Cited but thin everywhere you control. Shore up the page, reinforce socially → `ecosystem-ai-visibility` |
| **Social only** | — | — | ✓ | Social saw it first. Weakest signal of the seven — check whether search volume exists at all before committing. |

**Two sub-cases of "social only" that must not be reported as demand signals.** Both showed up on the first live run:

- **Non-topical content.** Memes, relatable humour and reactive posts often top the engagement chart while being about nothing searchable. They are working as intended — do not list them as topics with no search play, and never recommend "capture this demand". Group them as *non-topical* and exclude them from the untapped list.
- **The brand's own campaign.** If a topic dominates social because the brand is actively promoting it, "social discovered this demand" is exactly backwards — the site simply hasn't caught up with the campaign. You usually cannot tell from the data alone, so flag any social-only topic that maps to a product or launch and say plainly that a human needs to make the call.

**Contested is a separate flag, not a state.** Any topic — including "all three" — can also be contested if a competitor carries it across channels. Mark it in its own column and judge fit before chasing; not every competitor topic is yours.

Report the count per state up front — the distribution is itself the headline (e.g. "11 of your 14 topics are carried by one channel only").

### 7. Competitor cracks (only if competitors were provided)

For each competitor, line their three channels up the same way and look for **disagreement**: cited across AI answers on a topic where their rankings and engagement are both sliding, or dominant on social with no search play. Name which of the brand's channels is strongest to push from. Skip this section entirely rather than thinning it if no competitor data was pulled.

### 8. Prioritize and route

Rank the gap list by: relevance to the brand first, then demand (volume, or prompt frequency for AI-only topics), then competitive weakness, then how much of the footprint already supports it. **Do not rank by search volume alone** — moving away from that is the point of this skill.

Every recommendation names the skill that executes it. That handoff is the deliverable, not a footnote.

## Re-run mode (measuring a campaign)

If the user is re-running after a campaign, ask for the campaign slug, then read the passport — a post titled `Campaign passport: mkt:<slug>` on a Universal content page, or `campaign-passport-<slug>.md` in outputs.

Then, in addition to the normal run:

1. **Isolate the campaign's posts.** `list_labels(workspaceId)` to resolve `mkt:<slug>` to a label id, then `list_posts` filtered to that label, then `get_post_metrics_summary(workspaceId, [pageId], startDate, endDate)` per page for how the batch itself performed.
2. **Diff each channel against the passport**, but only where the passport actually holds a baseline:
   - `ecosystem-demand-campaign` passports hold **search** positions and traffic, plus the destination pages' engagement baseline.
   - `ecosystem-ai-visibility` passports hold **AI** brand presence, link presence and per-engine share of voice, plus the same social baseline.
   - If a channel has no baseline in the passport, report its current value and say plainly there's nothing to compare it to. Do not manufacture a "before".
3. **Report movement, not attribution.** Other things changed in the window — competitors published, algorithms shifted, seasonality happened. Say what moved. Never say the campaign caused it.

## Output format

Every run produces the same four things. None is optional and none waits to be asked for:

1. **A short read in the chat** — the headline pattern in 5–8 lines, standing on its own.
2. **`REPORT.md`** — the written record.
3. **`topics.csv`** — the same data as a spreadsheet.
4. **`index.html`** — the interactive report. This is the one people actually read.

Plus the `evidence/` folder, which is what makes the analysis auditable.

**Write them in that order, and finish the markdown and CSV before starting the HTML.** The analysis is the expensive part; the HTML is the last and longest step, built when the run is already long. If it fails there, everything must already be on disk. Never hold the findings in memory waiting to render them once at the end.

Create a folder `ecosystem-footprint-map-{brand-slug}-{YYYYMMDD}/`:

```
ecosystem-footprint-map-{brand-slug}-{YYYYMMDD}/
├── REPORT.md               (the matrix + routed moves — primary deliverable)
├── topics.csv              (full topic list with per-channel columns)
│                           columns: topic,keywords,avg_position,search_volume,ai_prompts,ai_cited,
│                           ai_engines,posts,avg_engagement_rate,top_post,state,contested,
│                           priority,move,next_skill,rationale
├── index.html             (the interactive report — built every run)
└── evidence/
    ├── 01-search.md
    ├── 02-ai-answers.md
    ├── 03-social.md
    ├── 04-topic-mapping.md    (which keywords/prompts/posts landed in each topic — makes it auditable)
    └── 05-data-notes.md       (gaps, timeouts, skipped ranges, stale metrics)
```

`REPORT.md` structure:

```markdown
# Whole-footprint map: {brand}
Market: {country} · Period: {dates} · Workspace: {name}
Channels: search + AI answers ({AI snapshot date}) + social

## The read
{3–5 lines: the distribution across states, and the single most useful pattern. This is what gets screenshotted — make it stand alone.}

## The matrix

| # | Topic | Search | AI answers | Social | State | Move |
|---|---|---|---|---|---|---|
| 1 | {topic} | #4 avg, 3 kw | cited, 2 prompts | 6 posts, 4.1% ER | **all three** | scale |
| 2 | {topic} | #8 avg, 11 kw | cited, 5 prompts | — | **search + AI, no social** | → ecosystem-demand-campaign |
| 3 | {topic} | — | — | 4 posts, 7.2% ER | **social only** | check demand first |

{Add a "contested" column when competitor data was pulled.}

## Untapped topics ({n})
{The search+AI-no-social rows, ranked. Lead with this section for the social seat.}

## What social saw first ({n})
{Social-only topics, plus anything in the unmapped bucket.}

## Competitor cracks ({n})
{Only if competitor data was pulled.}

## Data notes
{Channels below threshold, timeouts, skipped ranges, stale metrics, unmapped counts.}

## Next steps
1. {topic} → run `ecosystem-demand-campaign`
2. {topic} → run `ecosystem-ai-visibility`
3. Re-run this map in 2–8 weeks to see whether the gaps closed.
```

### The interactive HTML report — build it every run

`index.html` is a **standard deliverable, not an offer**. Build it without being asked. The markdown is the record; the HTML is what actually gets read, screenshotted and sent to a client, and a run that stops at markdown consistently under-delivers against the same analysis presented visually.

One self-contained file: inline CSS and JS, no external assets, works offline, light and dark.

**Read `references/brand.md` before styling anything** — the co-brand tokens, the four sanctioned colour combinations, the validated chart pairs and the shared layout conventions live there, and they're what make all three skills' reports look like one family. **Read the `dataviz` skill before writing any chart code** and follow its procedure: form first, colour by job, run the palette validator, hover layer by default, legend for ≥2 series.

If `brand.md` is unreachable, the minimum to keep it on-brand: light canvas `#E5E6EC`, indigo text `#161466`, violet highlight `#5C3CFF`, white cards, 8-based spacing, mono (`IBM Plex Mono`) for metric values and labels, and `#5C3CFF` + `#00B28E` as the two-series chart pair.

Sections, in this order:

1. **Header.** Brand, market, date range, workspace, then the coverage line: *"478 ranking keywords · 105 sampled AI prompts (5 engines) · 122 social posts (6 pages)"*. Real counts, never rounded.
2. **The dating caveat, as a callout directly under the header.** The three channels are not the same moment in time and the reader must know before they look at anything. State each channel's snapshot date, how old the AI one is, that the AI column is a *sample* so a blank cell means "not in the sample" rather than "absent", and that search and social coverage is complete for the window. This callout is mandatory whenever the AI snapshot is older than the search and social window — which is usually.
3. **Stat tiles — three always, a fourth when it's earned.** Topics mapped (and how many are carried by all three), keywords sitting on topics with no social, and share of posts on topics with no search presence. Those three come from data every run has.

   The fourth tile is **AI share of voice with the brand's rank — only when competitors were pulled**, which only happens when the user supplied them or asked. Without competitors there is no share of voice, so don't leave a hole and don't invent one: either drop to three tiles, or use another figure the run actually produced (topics where the brand is cited in AI answers, or the count below the data threshold). A row of three real numbers beats four with one improvised.

   Each tile carries one line of plain-language context under the number. These are hero numbers, not charts — see the dataviz form heuristic.
4. **The matrix.** Sortable by column, filterable by state via a chip row above the table. Chips show counts. One row per topic with per-channel cells, state, contested-by, priority, and the move naming the skill that executes it. Every cell is a real figure; blank means the channel is absent for that topic, and say so in one line above the table.
5. **Two charts.** A 100%-stacked pair showing what share of each channel's volume the other channel covers, and a single stacked bar showing the distribution of topics across states. Both are magnitude-of-a-whole, so stacked bars are the right form; a 2px surface gap between segments, legend present, hover tooltips on every segment.
6. **Share of voice**, when competitor data was pulled. Horizontal bars, brand highlighted with the categorical accent while competitors stay neutral — color follows the entity, and the brand is the entity in question. Lead with the interpretation, not the ranking: a brand can convert mentions into citations well and still place third on how often it gets named, and that reads as a different problem with a different fix.
7. **What to do next.** The routed list, numbered, each item naming its topic, its evidence in one clause, and the skill that acts on it. Close with the re-run window.
8. **Limits of this map**, collapsed by default. Everything from the Edge cases section that applied to this run, plus whatever landed in the data notes. Collapsed because it must be present without dominating; nobody should be able to say they weren't told.

Do not invent a number to fill a slot. A section with no data says so and stays.

### Where the files go

Local files are the guaranteed output — the folder above plus `index.html`. Deliver them and say where they are.

If the user asks for the report as a Google Doc, a Sheet, a Notion page, or anything else their connectors reach, mirror it there and say which is canonical. Don't do it unasked: it's slower, it varies by what they have connected, and the local files already work.

## Tips

- Data API rate limit is ~10 requests/second. Three channels × several competitors adds up fast; pace the loop and prefer narrow leaderboard calls over one large one.
- **Expect the occasional 500 "too many requests" from SE Ranking**, most often on the AI prompt endpoints when querying engine after engine. It's transient — wait a moment and retry the same call once. Only report it if the retry also fails; a single recovered 500 is not worth mentioning to the user.
- Check `DATA_getSubscription` for remaining units before a large pass. This is a heavier run than a single-channel skill — three channels plus competitors can exhaust a plan mid-analysis and leave a half-built matrix.
- Report zero as zero. If an engine returns no prompts, or a page returns no posts, say so — never estimate to fill a cell.
- Never invent volume, difficulty or engagement figures. Null is "unknown", not a number to guess.
- The topic assignment is judgment, not string matching. When a post is genuinely ambiguous, leave it unmapped rather than forcing it — a wrong assignment is worse than an honest gap.
- Two channels is a valid run. If social has no data, say so and produce the search × AI view rather than refusing.
- One snapshot is a position, not a trend. Re-run 2–8 weeks after a campaign to check it, and quarterly as a standing review even when nothing shipped.

## Edge cases & limits

- **No causation.** See the scope note. This is the failure mode most likely to embarrass the user in front of a client.
- **No sentiment.** SE Ranking's AI Search tools expose presence, share of voice and prompts — not how a brand is talked about.
- **Google My Business and Threads return no metrics** through the Planable connector — they come back in `unsupportedPages`. Exclude them from the social channel and say so in the data notes rather than reading their absence as a gap.
- **No GA4, no Looker, no conversion data.** This maps visibility and engagement, not revenue attribution. State it in the report footer.
- **Ranking trend needs an SE Ranking project**; AI-answer history needs the AI Result Tracker. Without them the map is a single point in time — still useful, but say so.
- **Social-only topics are the weakest signal.** A topic performing on social with no search presence may be genuine early demand, a topic nobody searches for, or the brand's own campaign talking to itself. Check whether volume exists before calling it an opportunity, and see the two sub-cases in step 6.
- **AI-answer data can be much staler than the other two channels.** On the first live run it was 11 months behind. Always report its snapshot date next to the search and social windows so nobody reads the three columns as one moment in time.
- **This skill writes nothing to Planable or SE Ranking** — no posts, no pages, no projects, no tracked keywords, and it consumes no plan limits beyond read requests. It produces local files (the report folder) and nothing else. Every action it recommends is executed by another skill.
