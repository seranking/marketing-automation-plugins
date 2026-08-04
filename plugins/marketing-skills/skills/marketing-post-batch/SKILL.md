---
name: marketing-post-batch
description: >
  Draft a demand-validated batch of social posts directly into Planable. Unlike a plain post
  batch, every topic is checked against real search volume, question keywords, and related-term
  demand (SE Ranking) before a single post is written: the preview shows the demand figure next
  to each post, zero-demand topics are flagged with data-backed alternatives, and every draft
  stays traceable to its source query. Use whenever the user wants posts grounded in what people
  actually search and ask — "draft posts based on demand", "posts about topics people actually
  search for", "validated post batch", "fill the calendar with topics worth posting about",
  "create N posts about [topic] and check they're worth it" — or any batch where topic choice
  should be evidence-based. Requires both the SE Ranking and Planable MCPs. For a quick batch
  from a brief without demand validation, use draft-post-batch from the smm-skills plugin instead, when that plugin is installed.
---

# Demand-validated post batch

Generate a set of on-brand social media post drafts and create them in Planable — with one difference that changes the economics of the batch: **topics are validated against real demand before writing**, so the user spends review time only on posts with an audience behind them.

## How this skill works

1. Gather inputs (client/workspace, platform, topic, number of posts, tone/angle)
2. Identify the right workspace and page(s) in Planable
3. **Validate candidate topics against demand (SE Ranking) — the signature step**
4. Generate the post copy (only for validated topics)
5. Create each draft in Planable via the connector

## Connector health check

Verify both MCPs before starting: `DATA_getSubscription` (SE Ranking) and `list_workspaces` (Planable). If either fails, stop and ask the user to reconnect it (SE Ranking guide: https://seranking.com/api/integrations/mcp/ · Planable guide: https://help.planable.io/hc/en-us/articles/27538577098780-How-to-connect-Planable-MCP-to-your-AI-tools).

---

## Step 1: Gather inputs

Before writing anything, confirm:

- **Workspace / client name** — which Planable workspace to post into
- **Platform(s)** — LinkedIn, Instagram, Facebook, X/Twitter, TikTok, etc.
- **Topic or brief** — what the posts should be about
- **Brand domain + market** — used for the demand check (default market: `us`)
- **Number of posts** — how many drafts to create (default: 5 if not specified)
- **Tone or angle** — optional; if not provided, use the platform's natural register
- **Proposed schedule** — optional; if provided, space posts evenly across the range
- **Labels** — optional; ask only if the user mentions them. For a themed batch, offer a `mkt:batch-<slug>` label so the batch is easy to find and report on later.
- **Team-only / internal note** — optional; default is false (visible to all workspace members)

If the workspace name is ambiguous or not provided, call `list_workspaces` and show the options. Do not guess.

---

## Step 2: Navigate to the right workspace and page

```
list_workspaces → find matching workspace → get workspaceId
list_pages(workspaceId) → find matching page(s) for the requested platform(s)
```

- Match workspace by name (case-insensitive)
- Match page by platform type (`facebook`, `instagram`, `linkedin`, `twitter`, `tiktok`, `youtube`, `pinterest`, `threads`, `universal`)
- If multiple pages exist for the same platform, ask the user which one to use
- If labels are requested, call `list_labels(workspaceId)` to get UUIDs

---

## Step 3: Validate topics against demand (signature step)

Break the brief into concrete topic candidates — one per planned post (a candidate = an angle someone could search or ask about, not a slogan). Then, for each candidate, pull the demand evidence from SE Ranking (sequentially; ~10 req/s limit):

- `DATA_getKeywordsMetrics` on the candidate's core phrase — volume, difficulty, CPC.
- `DATA_getKeywordQuestions` on the 2–3 strongest seeds — real questions people ask. These map directly to hooks; a seed returning only 1–2 questions is normal, so aggregate across seeds.
- `DATA_getRelatedKeywords` when a candidate's phrasing returns nothing — demand often exists under different words; take the best-phrased variant as the post's target.

Classify every candidate:

| Verdict | Evidence | What happens |
|---|---|---|
| **validated** | volume > 0 on phrase or variant, or real questions found | gets written, carries its figure |
| **rephrase** | zero on the brief's wording, demand under a variant | written against the variant; note the swap |
| **no demand** | nothing on phrase, variants, or questions | not written; flagged with 1–2 data-backed alternative topics from the related/questions pool |

Never invent volume. If SE Ranking returns null, mark it unknown rather than guessing — the whole point of this batch is that the numbers are real.

---

## Step 4: Write the posts

Generate all drafts (validated + rephrased topics only) before creating any of them in Planable.

**Platform character/format guidelines:**
| Platform | Max length | Notes |
|---|---|---|
| LinkedIn | ~3,000 chars | Professional tone, can be longer-form, use line breaks |
| Instagram | ~2,200 chars | Visual-first captions, hashtags at end or first comment |
| Facebook | ~63,206 chars | Conversational, shorter tends to perform better |
| X/Twitter | 280 chars | Punchy, no filler |
| TikTok | ~2,200 chars | Casual, trend-aware |
| YouTube | ~5,000 chars (description) | Informational, include keywords naturally |

**Writing guidelines:**
- Match the client's industry and voice if context is available
- Vary the hook and format across the batch — don't repeat the same structure; question keywords make natural hooks
- No banned words: seamless, streamline, effortlessly, optimize, enhance, all-in-one, unleash, eliminate, etc.
- Active voice, contractions, short sentences

**The preview is a demand table, not just copy.** Show every post with its evidence before creating anything (unless the user explicitly said "just create them"):

```
| # | Platform | Post (first 80 chars) | Target query | Volume | Source |
|---|---|---|---|---|---|
| 1 | LinkedIn | "Most teams schedule posts at the wrong…" | best time to post linkedin | 2,400/mo | keyword |
| 2 | Instagram | "You asked: does grid order still matter…" | instagram grid layout | question (3 variants) | questions |
Flagged, not written: "our new office tour" — no search/question demand found; alternatives: {alt1}, {alt2}
```

---

## Step 5: Create the drafts in Planable

After showing the preview and getting confirmation (or if the user said to go ahead directly):

**Single platform:**
```
create_post(workspaceId, pageId, text, scheduledAt?, labels?, teamOnly=false)
```

**Multiple platforms with same content:**
```
create_grouped_post(workspaceId, pageIds[], text, scheduledAt?, labels?)
```
A grouped post shares one text across pages — grouping platforms with very different limits forces the copy under the strictest one (X's 280 chars); use separate `create_post` calls when copy should differ.

**Multiple platforms with adapted content:** call `create_post` once per platform with the platform-specific version.

Keep posts as proposed drafts — don't set `publishAtScheduledDate` — so nothing auto-publishes.

---

## Step 6: Confirm and summarize

After all posts are created, report back:
- How many drafts were created (and how many candidates were flagged out, with their alternatives)
- Which workspace and page(s) they're in, and the label if one was applied
- Proposed schedule times (if any)
- Any `validationErrors` returned (e.g., Instagram requires media — flag this clearly)
- A direct note that posts are drafts only and will not publish until approved/scheduled in Planable

---

## Edge cases

- **Media**: This skill creates text-only drafts by default. If the user provides a public image URL, pass it as `mediaUrl`. If they reference a file from their computer, explain that Planable requires a public URL and suggest uploading to Google Drive or similar first.
- **Update coming**: Post editing is not yet available via the connector. If the user asks to revise an existing post, note this and suggest editing directly in Planable.
- **Stories / Reels**: Supported via `classification` parameter. Ask the user if they want post, story, or reels format if the platform supports multiple (Instagram, TikTok, YouTube).
- **Rate**: Create posts one at a time (not in bulk API calls) to surface any per-post validation errors clearly.
- **Purely internal/brand topics** (office news, culture posts): demand validation doesn't apply — mark them "brand post — demand check skipped" instead of flagging them, and write them as briefed.
