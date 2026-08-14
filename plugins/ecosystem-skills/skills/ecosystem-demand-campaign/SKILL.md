---
name: ecosystem-demand-campaign
description: >
  Turn real search demand from SE Ranking — keyword gaps, competitor wins, ranking losses, question
  keywords — into a drafted, scheduled social campaign in Planable. Trigger from the search seat:
  "what should we post based on our SEO data", "turn these keyword gaps into posts", "competitor
  ranks for X, make social content about it", "build a campaign from search demand". Trigger equally
  from the social seat, where SEO is never mentioned: "fill my content calendar for next month",
  "I need post ideas backed by data", "give me a month of posts for this client", "what are people
  actually searching for in our space", "I'm staring at an empty calendar". Either entry point works:
  give it a domain, or give it a Planable workspace and it asks which site to pull search data for.
  Every batch is instrumented — label `mkt:{slug}` plus a passport with baseline numbers — so movement
  can be proven later. Requires both MCPs. Run ecosystem-footprint-map first to see which topics
  deserve a campaign.
---

# Search demand → social campaign

Connect SE Ranking (where the demand and the gaps live) to Planable (where content gets made, approved, and published). The goal is a focused social campaign whose every post is grounded in real search demand — not a generic brainstorm.

## Prerequisites

- **SE Ranking MCP** connected (Data API for research; a project is optional but unlocks ranking-loss detection).
- **Planable MCP** connected, with the destination workspace and at least one connected page.
- The user provides: target domain, market/country (default `us`), the Planable workspace, and ideally 1–3 competitor domains. Number of posts defaults to 6 if unspecified.

**Two entry points, one workflow.** Users arrive from either seat and their vocabulary differs:

- **From the search seat** — they name a domain, competitors, keywords. Take it as given and go.
- **From the social seat** — they name a workspace or a client, and may never say "SEO", "keyword" or "domain" at all. They are not being vague; the calendar is simply where their work lives. Start from the workspace, then ask for the website in plain terms: *"which website should I pull search data for? I need it to see what people are actually searching for in your space."* Don't ask for a "target domain" or a "market database" — ask for the site and the country their audience is in.

Never infer the domain from the Planable pages. Social page links point at the social profiles, and universal pages often carry a placeholder URL. Ask.

## Connector health check

Before doing anything else, verify both MCPs are reachable:

- **SE Ranking:** call `DATA_getSubscription`. If it fails or returns an auth error, stop immediately. **Word this for someone who may never have used SE Ranking**, since a Planable-first user often hasn't:
  > "I can't reach SE Ranking, and I need it for the search side of this — it's where the demand data comes from. If you already have an account, reconnect it here: https://seranking.com/api/integrations/mcp/ · If you don't have one yet, that link covers setup too."

  Say "connect", not "reconnect", unless you know they had it. Don't call it an error on their part.
- **Planable:** call `list_workspaces`. If it fails or returns an auth error, stop immediately and tell the user:
  > "I can't reach Planable, and that's where the posts get drafted. Connect it here: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools"

Only continue to the process steps below once both calls return a successful response.

## A narrow question gets a narrow answer

The full run researches search demand, clusters it into themes, drafts posts and pushes them to Planable. That's right when someone wants a campaign. It's wrong when they ask one question.

If the user asks something bounded — *"what are people searching for around X?"*, *"which keywords is this competitor beating us on?"*, *"is there demand for this topic?"* — answer it directly from the research, then offer the rest in one line: *"Want me to turn these into a batch of drafts in Planable?"*

Never create posts in someone's workspace off the back of a question that didn't ask for them.

## Process

### 1. Scope the campaign
Confirm: target domain, country, competitors (optional), the Planable workspace + which platforms, how many posts, and any campaign window. If the workspace is ambiguous, call `list_workspaces` and let the user pick — do not guess.

**Ask in the order the user is thinking.** If they opened with a workspace or client name, confirm that first, then ask for the website and country. If they opened with a domain, do the reverse. Asking a social manager for a "target domain and market database" before you've acknowledged the client they named reads as the wrong tool.

Keep the whole intake to plain words: which client, which website, which country, which channels, how many posts, by when.

### 2. Discover opportunities in SE Ranking
Work sequentially (respect the Data API limit of ~10 requests/second). Save raw results as you go.

- `DATA_getDomainOverviewWorldwide` + `DATA_getDomainKeywords` — the target's current footprint and best existing keywords. The target's own strong/striking-distance keywords are often the most on-brand campaign fuel, so mine these first.
- `DATA_getDomainCompetitors` — confirm the real organic competitors (sort by shared keywords; take the top 3–5). Note: this call returns the full set and may be written to a file — read and parse it.
- `DATA_getDomainKeywordsComparison` — the core gap list: keywords competitors rank for that the target does not. Filter to keep it useful (e.g. informational/commercial intent, volume > a sensible floor, difficulty the domain can realistically win). **Then judge relevance, not just the numbers** — competitor gap lists are noisy and surface off-brand junk (glossary entries, follower-farming terms like "free followers", unrelated blog tangents). Discard anything that isn't a topic the target could credibly post about; a high-volume term the brand has no business addressing is not an opportunity. If a strict filter returns almost nothing, loosen one threshold or try another competitor rather than forcing a campaign out of junk.
- `DATA_getRelatedKeywords` + `DATA_getKeywordQuestions` — expansion terms and real questions people ask. **Question keywords are gold for social hooks** — they map directly to post openers. These endpoints often return only a handful of rows per seed, so query **several seeds (3–5 core topics) and aggregate** rather than trusting one; a seed returning 1–2 questions is normal, not a sign there's no demand.
- `DATA_getSerpResults` for the 2–4 strongest themes — read the intent and the SERP features so the social angle matches what searchers actually want.
- **Ranking losses (if a project exists):** call `PROJECT_getPositionHistory` (type `avg_pos` or `visibility`) to find terms that slipped. A recovering or slipping keyword is a strong "we have something to say" social trigger. If there is no project, skip this and rely on the gap analysis above.

### 3. Cluster into themes, then into angles
Group the keywords/questions into 3–6 content themes. For each theme, write a one-line rationale tied to the data (volume, gap size, intent, or a ranking move). Then translate each theme into concrete social angles — a question keyword becomes a hook, a comparison term becomes a carousel, a how-to becomes a tips post.

Surface the themes to the user **before** drafting, so they can steer.

### 4. Draft the posts
Write platform-appropriate copy for each post (see length/format norms below). Vary hooks and formats across the batch — don't repeat one structure. Keep each post traceable to its source keyword/question so the user understands the "why".

| Platform | Length | Notes |
|---|---|---|
| LinkedIn | ~3,000 chars | Professional, can be long-form, line breaks |
| Instagram | ~2,200 chars | Visual-first; hashtags at end or first comment; requires media |
| Facebook | shorter performs better | Conversational |
| X/Twitter | 280 chars | Punchy |
| TikTok | ~2,200 chars | Casual, trend-aware |

Show all drafts in a preview before creating anything, unless the user said "just create them".

### 5. Create the drafts in Planable
`list_pages(workspaceId)` to get page IDs for the requested platforms. Then:

- Same content across platforms → `create_grouped_post(workspaceId, pageIds[], text, scheduledAt?, labels?)`. **Caveat:** a grouped post shares one text across every page, so grouping platforms with very different limits (e.g. Facebook + X) forces the shared copy under the strictest one (X's 280 chars). When copy should differ in length or tone per platform, use separate `create_post` calls instead.
- Per-platform copy → `create_post(workspaceId, pageId, text, scheduledAt?, labels?, firstComment?)` once per page.
- **Scheduling — ask before creating.** Don't guess dates or leave everything undated by default. Ask how the user wants the batch dated and offer: **spread evenly** across a window (e.g. the next 7 days, one post per slot at a sensible hour), a **fixed cadence/interval** (e.g. every weekday at 10:00, laid out from a start date they give), **manual** dates per post, or **no dates yet** (undated drafts to place on the calendar later). Convert each chosen time to ISO 8601 and pass it as `scheduledAt`. Keep posts as **proposed drafts** — don't set `publishAtScheduledDate` — so nothing auto-publishes; only set it `true` if the user explicitly wants auto-publishing. Scheduled times are treated as **UTC**, so confirm the timezone or state that times are UTC.
- Tag the batch with the campaign label from the contract below — `list_labels` / `create_label`, then pass the label UUIDs on every draft.

### 6. Campaign contract (what makes it measurable)

1. **Label**: create/reuse `mkt:<campaign-slug>` and attach it to every draft in the batch. Slug = short + dated, e.g. `mkt:gaps-acme-2026q3`.
2. **Campaign passport**: one post on a Universal content page titled `Campaign passport: mkt:<slug>` (fall back to saving `campaign-passport-<slug>.md` to outputs if the workspace has no Universal page — check `list_pages` for `type: "universal"`). Contents, factual and compact: goal, launch date, source themes with their keywords/questions and volumes, the **baseline numbers already pulled during research** (target's current positions for the targeted terms, traffic figures), tracked terms, workspace + pages used, and the destination pages' **social baseline** — call `get_page_metrics_summary(workspaceId, pageIds, startDate, endDate)` for the 30 days before launch and record it, so the social side has a "before" too. This is read verbatim later as the baseline — don't editorialise it.
3. **Optional deeper tracking** (needs an SE Ranking project): `PROJECT_addKeywords(site_id, keywords[])` so position movement is tracked automatically.

   **Add only the campaign's target terms.** Step 2 pulls hundreds or thousands of keywords — that's the research set, not the tracking set. What goes in is the handful of terms the posts actually target, usually 10–30, one or two per theme. Never pass the full research pull: `PROJECT_addKeywords` caps at 100 per call, the user's plan may allow fewer, and every tracked keyword consumes their allowance on every check cycle from then on.

   **Show the list and the count, then ask.** *"I'd add these 14 keywords to your SE Ranking project to track position over time. This uses your plan's keyword allowance. Go ahead?"* Never write to their project without an explicit yes. If the call fails on a limit, say what happened plainly, note that the campaign is unaffected because the drafts and passport are already in place, and offer a smaller set.

If the user declines instrumentation, proceed — but say plainly the campaign won't be measurable later.

### 7. Confirm and hand off
Report how many drafts were created, in which workspace/pages, the proposed schedule, the campaign label + passport location, and any `validationErrors` (e.g. Instagram needs media — flag it). Close with: *"In 2–8 weeks, re-run `ecosystem-footprint-map` to see whether these topics moved across search, AI answers, and social — the passport has the starting numbers to compare against."*

> If the user also has the `marketing-skills` plugin installed, `marketing-campaign-impact` reads the same passport and gives a per-campaign verdict. Mention it only if that plugin is present.

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

1. A short **content plan** in the chat the user can scan: theme → source keyword/question (with volume/difficulty) → platform → angle.
2. The **created drafts** summary with Planable links.
3. **`campaign-brief.md`** — the same plan as a file. Written every run, not on request.
4. **`brief.html`** — the shareable version. Written every run.

Files go in `ecosystem-demand-campaign-{slug}-{YYYYMMDD}/`, alongside the campaign passport from step 6.

**Write the markdown before the HTML**, so a rendering failure can't cost you a plan whose drafts are already live in Planable.

### The brief page — keep it light

**This is a plan, not a dashboard. Overbuilding it is worse than underbuilding it.** The footprint map is the analysis product; this one exists so a content lead can see what's coming and why, and forward it to whoever approves things. One screen of scrolling, not five.

**Read `references/brand.md` before styling** so it matches the other two reports. One self-contained file, inline CSS and JS, no external assets.

Five sections, in this order:

1. **Header** — brand, market, workspace, campaign label, how many posts across which platforms, and the window they're scheduled across.
2. **Why these themes** — one short block. Each theme with its source keyword or question, volume and difficulty, and the one-line rationale tied to the data. This is the part that makes the calendar defensible in a meeting, so keep the numbers visible rather than summarising them away.
3. **The posts** — a table. Platform, hook or opening line, the theme it came from, scheduled time, Planable link, and any `validationErrors` flagged inline. The Planable link is the most-clicked thing on the page; make it obvious.
4. **The baseline**, in three or four lines — the positions and traffic recorded in the passport, and the note that re-running `ecosystem-footprint-map` in 2–8 weeks compares against it.
5. **Limits**, collapsed. Drafts only, nothing publishes without approval; visual platforms need media; ranking-loss detection needed a project and whether there was one.

**At most one chart, and only if it earns its place.** Themes by search volume as a simple horizontal bar is usually the only one worth having, and a table often beats it. If you draw it: one hue, direct labels, no legend. Do not build a stat-tile row here — four hero numbers on a six-post campaign reads as padding.

### Where the files go

Local files are the guaranteed output. Mirror to Google Docs, Sheets or Notion only if asked, noting which copy is canonical.

## Tips

- Never invent search volume or difficulty. If SE Ranking returns null, mark it unknown rather than guessing — the credibility of the plan depends on it.
- Relevance beats volume. A 320-volume question your brand can answer brilliantly will outperform a 50k-volume term that has nothing to do with the product.
- Match intent to platform: informational questions → educational posts; commercial/comparison terms → proof, demos, comparisons.
- Posts are created as drafts. They will not publish until approved/scheduled in Planable. Say this explicitly so nothing goes live unexpectedly.

## Edge cases & limits

- **Ranking-loss detection needs an SE Ranking project** with position history. Without one, the campaign is built from competitor gap analysis (still strong, just not loss-driven).
- **No SE Ranking Content Editor via MCP.** This skill drafts the social copy directly; it does not produce a Content-Editor optimization score. If the user needs website/blog copy scored, that step stays in the SE Ranking UI.
- **Instagram/visual platforms require media.** Text-only drafts will flag a validation error; offer to attach a public image URL via `mediaUrls`, or note that media must be added in Planable before publishing.
- This skill creates social drafts only — publishing happens in Planable after approval.
