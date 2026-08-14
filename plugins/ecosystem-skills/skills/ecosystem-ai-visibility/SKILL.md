---
name: ecosystem-ai-visibility
description: >
  Find the prompts where a brand is invisible or losing in AI answers (ChatGPT, Perplexity, Gemini,
  AI Overview, AI Mode), then turn those gaps into a social campaign in Planable with before/after
  tracking. Trigger from the search seat: "what should we post to get cited by AI", "where are
  competitors winning in AI answers and we're not", "AEO/GEO content plan", "turn our AI search gaps
  into posts". Trigger equally from the social seat, in their words: "does ChatGPT know we exist",
  "a client asked if AI mentions them", "how do we show up when people ask AI for recommendations",
  "everyone's asking about AI search and I don't know what to post". Either entry point works: give
  it a domain, or give it a Planable workspace and it asks which site and brand to check. Every batch
  is instrumented — label `mkt:{slug}` plus a passport with baseline share of voice — so the effect
  can be proven later. Requires both MCPs. For a whole-footprint read first, use
  ecosystem-footprint-map.
---

# AI-search gaps → social campaign

Use SE Ranking's AI Search data to see which prompts and narratives a brand owns, which competitors own, and which are wide open — then build social content in Planable that stakes a claim in the missing narratives, and instrument it so impact is measurable.

> **Scope note (read this).** SE Ranking's AI Search MCP tools expose brand presence, link presence, share of voice, and the prompts behind them. They do **not** expose sentiment scoring. Do not report or imply sentiment from these tools. Social content is one lever on AI visibility — LLM citation is also driven by website content and authority, which is outside what these two MCPs publish.

## Prerequisites

- **SE Ranking MCP** connected (AI Search Data API; optionally a project for the AI Result Tracker, which enables ongoing prompt tracking).
- **Planable MCP** connected, with the destination workspace and pages.
- The user provides: target domain + brand name, country (default `us`), competitor domains + brand names (up to 10), and optionally which engines to focus on (default: all of `ai-overview`, `ai-mode`, `chatgpt`, `perplexity`, `gemini`).

**Two entry points, one workflow.** Users arrive from either seat and their vocabulary differs:

- **From the search seat** — they name a domain, competitors, engines, and may say AEO or GEO. Take it as given and go.
- **From the social seat** — they name a workspace, a client, or just a worry ("does ChatGPT know we exist"). Start there, then ask for the website and brand name in plain terms: *"which website and brand name should I check? I'll look at what ChatGPT, Perplexity, Gemini and Google's AI answers say about them."* Don't ask which "engines" they want or mention AEO/GEO unless they use those words first — default to all five and say so plainly.

Never infer the domain from the Planable pages. Ask.

## Connector health check

Before doing anything else, verify both MCPs are reachable:

- **SE Ranking:** call `DATA_getSubscription`. If it fails or returns an auth error, stop immediately. **Word this for someone who may never have used SE Ranking**, since a Planable-first user often hasn't:
  > "I can't reach SE Ranking, and I need it to see what the AI engines say about you — that's where the AI-answer data lives. If you already have an account, reconnect it here: https://seranking.com/api/integrations/mcp/ · If you don't have one yet, that link covers setup too."

  Say "connect", not "reconnect", unless you know they had it. Don't call it an error on their part.
- **Planable:** call `list_workspaces`. If it fails or returns an auth error, stop immediately and tell the user:
  > "I can't reach Planable, and that's where the posts get drafted. Connect it here: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools"

Only continue to the process steps below once both calls return a successful response.

## A narrow question gets a narrow answer

The full run researches five engines, clusters the prompts, drafts posts, pushes them to Planable and builds a report. That's right when someone wants a campaign. It's wrong when they ask one question.

If the user asks something bounded — *"does ChatGPT mention us?"*, *"who's winning AI answers for CRM comparisons?"*, *"what's our share of voice?"* — pull only what that needs, answer it directly, then offer the rest in one line: *"That's where you stand. Want me to turn the gaps into a drafted campaign in Planable?"*

Answering the question is not a lesser outcome. Producing a campaign nobody asked for is.

## Process

### 1. Resolve the brand and scope
If the user gives a domain but not the exact brand string, call `DATA_getAiSearchBrand(target, source)` to get the name SE Ranking attributes to it. Do the same for each competitor. Confirm the Planable workspace and target platforms.

**Ask in the order the user is thinking.** If they opened with a workspace or client name, confirm that first, then ask for the website and brand. If they opened with a domain, do the reverse.

Competitors are required here for share of voice, and a social-seat user may not have a list ready. Ask for "the two or three brands you're usually compared to" rather than "competitor domains and brand names (up to 10)".

### 2. Baseline AI visibility
- `DATA_getAiSearchOverview(target, source, brand?)` — capture brand_presence, link_presence, ai_opportunity_traffic, and average_position. **Read `previous` before quoting change:** if it's `null`, this is the first snapshot — report the current values as a baseline and do **not** present the `change_percent` of 100 as real growth.
- `DATA_getAiSearchLeaderboard(primary{target,brand}, competitors[{target,brand}], source, engines[])` — share of voice for the brand vs competitors, per engine. Build a quick heatmap (rows = brands, columns = engines).
  - **This endpoint is heavy and can return a 504 timeout** when you pass many competitors × many engines at once. Query **one engine at a time** (or keep it to ≤3 competitors per call), and retry once on timeout. If it still fails, fall back to calling `DATA_getAiSearchOverview` for each competitor and compare brand_presence / link_presence yourself.

### 3. Find the prompt gaps
For the target and each competitor, pull the prompts behind the presence:

- `DATA_getAiSearchPromptsByBrand(brand, engine, source)` — prompts mentioning the brand by name.
- `DATA_getAiSearchPromptsByTarget(target, engine, source)` — prompts where the domain is cited as a source.

Compare: cluster prompts by topic, then mark each cluster as **owned** (target appears), **contested** (target + competitors), or **missing** (competitors appear, target doesn't). The missing and contested clusters are the campaign targets.

- **AI prompts almost always have `volume: 0`** — they're conversational queries, not search keywords. That is expected and is **not** a signal of low value. Judge a cluster by topical relevance and by *which brands the LLM cites*, never by search volume.
- **Validate brand-name matches.** A brand can surface in loosely related answers ("best year planner", a person's name, etc.). Read the answer text and flag ambiguous matches rather than counting them as real presence.
- Note *where* the target sits when it does appear (e.g. cited 4th of 6 in "best X" answers) — moving up within contested prompts is as valuable as entering missing ones.

### 4. Turn gaps into content hypotheses
For each target cluster, write a hypothesis: *"If we publish clear, citable content asserting [brand] in [narrative], we should start appearing for prompts like [examples]."* Translate each into social angles that make the brand's position explicit and quotable — definitions, head-to-head comparisons, "X vs Y", myth-busting, FAQ-style answers. LLMs favour clear, structured, attributable claims, so write social copy that states the position plainly rather than burying it.

Present the clusters and hypotheses to the user before drafting.

### 5. Draft and create in Planable
Write platform-appropriate copy, then create drafts: `create_post` per page (per-platform copy) or `create_grouped_post` for synced content. Tag the batch with the campaign label `mkt:<slug>` (via `list_labels` / `create_label`; slug short + dated, e.g. `mkt:ai-vis-acme-2026q3`) so the campaign is easy to isolate when measuring. 

**Scheduling — ask before creating.** Don't guess dates or leave everything undated by default. Ask how the user wants the batch dated and offer: **spread evenly** across a window (e.g. the next 7 days, one post per slot at a sensible hour), a **fixed cadence/interval** (e.g. every weekday at 10:00, laid out from a start date they give), **manual** dates per post, or **no dates yet** (undated drafts to place on the calendar later). Convert each chosen time to ISO 8601 and pass it as `scheduledAt`. Keep posts as **proposed drafts** — don't set `publishAtScheduledDate` — so nothing auto-publishes; only set it `true` if the user explicitly wants auto-publishing. Scheduled times are treated as **UTC**, so confirm the timezone or state that times are UTC.

### 6. Campaign contract — instrument before/after measurement
This is what makes the loop real:

- **Campaign passport:** create one post on a Universal content page titled `Campaign passport: mkt:<slug>` (fall back to `campaign-passport-<slug>.md` in outputs if the workspace has no Universal page — check `list_pages` for `type: "universal"`). Contents, factual and compact: goal, launch date, the target prompt clusters with example prompts, the **baseline from step 2** (brand presence, link presence, per-engine SoV vs competitors), tracked prompts, workspace + pages, and the destination pages' **social baseline** — call `get_page_metrics_summary(workspaceId, pageIds, startDate, endDate)` for the 30 days before launch and record it, so the social side has a "before" too. This is read verbatim later as the baseline — don't editorialise.
- **Ongoing AI tracking (if a project exists):** create an AI Result Tracker engine with `PROJECT_createLlmEngine`, add the campaign's target prompts with `PROJECT_addPrompts(site_id, llm_id, prompts[])`, then read movement later with `PROJECT_getPromptsRankings` and `PROJECT_getLlmStatistics`. Record the engine id in the passport.

  **Track only what the campaign targets. This is the step most likely to go wrong.** Research in step 3 surfaces hundreds or thousands of prompts across engines and competitors — a live run collected 1,337. Those are the *research set*, not the tracking set. What goes into the tracker is the small list of prompts this campaign is actually trying to win: the missing and contested clusters chosen in step 4, usually 10–30.

  Before writing anything:

  1. **Build the list from the chosen clusters only.** Never pass the full research set. If the count still exceeds 30, rank by cluster relevance and cut, rather than adding everything "just in case" — every prompt consumes plan limits on the user's live account, every check cycle, forever.
  2. **`PROJECT_addPrompts` accepts at most 100 per call**, and the user's plan may allow fewer. Treat 100 as an absolute ceiling and 30 as the working target.
  3. **Show the list and the count, then ask.** *"I'd add these 18 prompts to your SE Ranking project so we can track them over time. This uses your plan's prompt allowance. Go ahead?"* Never write to their project without an explicit yes, and never confirm a count you haven't shown them.
  4. **If the call still fails on a limit** (e.g. *"You're trying to add too many keywords: 1337. The available limit of adding is 100"*), don't retry blindly and don't present it as a broken campaign. Say what happened in plain words, state that the campaign itself is unaffected because the drafts and the passport are already in place, and offer to add a smaller set.

  Tracking is optional. The campaign is fully measurable without it — the passport holds the baseline and `ecosystem-footprint-map` re-pulls the same metrics later. If the user declines or the project doesn't exist, skip it and say so in one line.
- **Re-checks:** re-run `ecosystem-footprint-map` in 2–8 weeks — it re-pulls `DATA_getAiSearchOverview` / `DATA_getAiSearchLeaderboard`, reads the social side via `get_post_metrics_summary` on the labelled posts, and diffs against the passport baseline. AI visibility moves slowly, so don't check sooner.

> If the user also has the `marketing-skills` plugin installed, `marketing-campaign-impact` reads the same passport and returns a per-campaign verdict. Mention it only if that plugin is present.

If the user declines instrumentation, proceed — but say plainly the campaign won't be measurable later.

## Content pointers: writing for keywords & AI visibility gaps

Keep these in mind when creating social content meant to target a specific keyword or close an AI-visibility gap:

- **Target one intent per post.** Pick a single keyword or question and answer that one thing clearly. Posts that try to cover everything rank and get cited for nothing.
- **Lead with the answer.** Put the takeaway in the first line, then support it. Skimmers and AI engines both extract the clearest, most self-contained statement — don't bury it.
- **Write the way people actually ask.** Phrase hooks, captions, and headers as real questions and plain-language answers. AI prompts are conversational, so natural phrasing beats keyword-stuffing.
- **Make claims quotable on their own.** AI tools lift snippets out of context, so each key sentence should stand alone — one idea, declarative, no "as mentioned above."
- **Be specific.** Numbers, concrete examples, named steps, clear definitions. Specificity is what gets cited and what sets you apart from generic content competitors already own.
- **Fill the gap, don't echo it.** If a competitor already owns a topic, find the sub-question or angle they're missing instead of repeating what's already ranking.
- **Stay consistent across surfaces.** Use the same terms and claims on social, your site, and your profiles so AI builds one coherent picture of what your brand is the answer for.
- **Keep it human.** It still has to read like a good post — optimizing for keywords or AI shouldn't make the writing robotic.

## Output

Every run produces all of this. None of it waits to be asked for.

1. **A short read in the chat** — where the brand stands and the single most useful gap, in 5–8 lines.
2. **`REPORT.md`** — snapshot, clusters, plan, drafts, tracking. The written record.
3. **`prompts.csv`** — every prompt with its cluster, state, engine, and which brands the answer cites.
4. **`index.html`** — the interactive report. This is what gets read and sent on.

Files go in `ecosystem-ai-visibility-{brand-slug}-{YYYYMMDD}/`, and the campaign passport is written as specified in step 6.

**Write the markdown and CSV before starting the HTML.** The HTML is the last and longest step, built when the run is already long and the drafts are already in Planable. If it fails, nothing else may be lost with it.

### The interactive report — build it every run

One self-contained file: inline CSS and JS, no external assets, works offline. **Read `references/brand.md` before styling anything**, and the `dataviz` skill before writing chart code. Sections in this order:

1. **Header** — brand, market, engines queried, date. Then the real coverage line: how many prompts across how many engines, and how many competitors were compared.
2. **Snapshot tiles** — brand presence, link presence, average position, and share of voice with the brand's rank among the competitors named. Each with one line of plain-language context. **If `previous` was `null`, the tile says "first snapshot — baseline" and shows no change figure.** Never render a 100% change as growth.
3. **The share-of-voice heatmap** — brands down, engines across, share in each cell. This is the single most screenshotted thing the skill produces, so give it room. Cells carry the number, not just colour. The brand's row is highlighted; competitors stay neutral. If the leaderboard call had to be split per engine or fell back to per-competitor overview calls, say so under the chart.
4. **The three cluster groups** — owned, contested, missing. One block each, with the example prompts, the competitors winning each, and where the brand sits when it does appear (*"cited 4th of 6"*). Contested and missing are the campaign targets and should read as the point of the page. Flag separately any prompt where the brand is **named in the answer text but a competitor's page is the cited source** — that's a different and more actionable finding than plain absence.
5. **The content plan** — cluster → hypothesis → platform → angle, as a table. The hypothesis is what makes this a plan rather than a list; keep it in full.
6. **The drafts** — every post created, with its platform, its Planable link, its scheduled time, and the cluster it targets. Flag any `validationErrors` (Instagram needing media, most often) right here rather than burying them.
7. **The tracking plan** — the baseline recorded, the prompts added to the AI Result Tracker if that happened, what to re-pull, and when. If tracking was skipped or hit a plan limit, say which and that the campaign is unaffected.
8. **Limits of this report**, collapsed. No sentiment, social is one lever on AI citation, the engines and window queried, and anything from the run that qualifies the numbers.

Do not invent a number to fill a slot. A section with no data says so and stays.

### Where the files go

Local files are the guaranteed output. If the user asks for it as a Google Doc, a Sheet or a Notion page, mirror it and say which copy is canonical — but not unasked.

## Tips

- Respect the Data API rate limit (~10 req/s); with several brands × engines × prompt queries, pace the loop — and prefer narrow leaderboard calls over one giant one (see step 2).
- **Expect the occasional 500 "too many requests"**, most often on `DATA_getAiSearchPromptsByBrand` / `ByTarget` when querying engine after engine. Transient — wait a moment and retry the same call once. Only surface it if the retry also fails.
- **Report the snapshot date.** The prompts endpoints return a `date` field; `DATA_getAiSearchOverview` does not, so don't look for one there. Live runs came back dated ~11 months before the run on four of five engines. Put the date in the report header so nobody reads stale AI data as current.
- Report zero as zero. If an engine returns no prompts for a brand, say so — don't estimate.
- Recommend re-running monthly and diffing — AI visibility moves slowly, so a single snapshot isn't a verdict.

## Edge cases & limits

- **No sentiment.** These tools don't measure how a brand is *talked about*, only whether/where it appears. If the user wants sentiment, say it's not available through the connected MCPs.
- **Social is indirect.** Appearing in AI answers is heavily influenced by citable web content. This skill drives the social lever and tracks the result; it cannot publish or score website pages.
- **Ongoing tracking needs a project.** The one-off `DATA_` AI Search calls work without a project; the AI Result Tracker (prompts over time) requires an SE Ranking project.
- Posts are created as drafts — publishing happens in Planable after approval.
