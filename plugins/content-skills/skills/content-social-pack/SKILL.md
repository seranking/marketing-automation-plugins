---
name: content-social-pack
description: Atomize one published article into a platform-adapted social pack drafted in Planable. Give it a URL and get a LinkedIn post or carousel copy, an X/Twitter thread, a Google Business Profile post, a newsletter section, and quote hooks — grouped as a Planable campaign linked to the source article, with a preview before anything is created. Use whenever the user wants to repurpose existing content — "repurpose this article", "turn this blog post into social content", "atomize this URL", "make a social pack from this article", "promote this article on social", "squeeze more out of this post". Requires the Planable MCP. For drafting a generic batch of posts from a topic or brief rather than from an existing article, use draft-post-batch from Planable/smm-skills instead, when that plugin is installed.
---

# Content Social Pack

Turn one published article into a platform-adapted social pack and create the drafts in Planable. The input is an article URL: start with "The article-to-pack derivation" section below, which prefills the inputs for Steps 1–4.

## How this skill works

0. Fetch the article and derive the default pack (see "The article-to-pack derivation")
1. Gather the remaining inputs (client/workspace, platforms, tone/angle)
2. Identify the right workspace and page(s) in Planable
3. Generate the post copy
4. Create each draft directly in Planable via the connector

---

## Step 1: Gather inputs

Before writing anything, confirm:

- **Workspace / client name** — which Planable workspace to post into
- **Platform(s)** — LinkedIn, Instagram, Facebook, X/Twitter, TikTok, etc.
- **Source article** — the URL the pack is derived from (already fetched in the derivation step)
- **Pack contents** — the default pack from the derivation step, unless the user asks for a different mix
- **Tone or angle** — optional; if not provided, use the platform's natural register
- **Proposed schedule** — optional; if provided, space posts evenly across the range
- **Labels** — optional; ask only if the user mentions them
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

## Step 3: Write the posts

Generate all drafts before creating any of them in Planable.

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
- Vary the hook and format across the batch — don't repeat the same structure
- Avoid filler marketing words (seamless, streamline, effortlessly, unleash, all-in-one…); a `brand-profile.md` banned-word list takes precedence
- Active voice, contractions, short sentences

Show the user all drafts in a clear preview **before** creating them in Planable, unless they've explicitly said "just create them."

---

## Step 4: Create the drafts in Planable

After showing the preview and getting confirmation (or if the user said to go ahead directly):

**Single platform:**
```
create_post(
  workspaceId,
  pageId,
  text,
  scheduledAt (if provided),
  labels (if provided),
  teamOnly (default: false)
)
```

**Multiple platforms with same content:**
```
create_grouped_post(
  workspaceId,
  pageIds[],
  text,
  scheduledAt (if provided),
  labels (if provided)
)
```

**Multiple platforms with adapted content:**
Call `create_post` once per platform with the platform-specific version of the copy.

---

## Step 5: Confirm and summarize

After all posts are created, report back:
- How many drafts were created
- Which workspace and page(s) they're in
- Proposed schedule times (if any)
- Any `validationErrors` returned (e.g., Instagram requires media — flag this clearly)
- A direct note that posts are drafts only and will not publish until approved/scheduled in Planable

---

## Edge cases

- **Media**: This skill creates text-only drafts by default. If the user provides a public image URL, pass it as `mediaUrl`. If they reference a file from their computer, explain that Planable requires a public URL and suggest uploading to Google Drive or similar first.
- **Stories / Reels**: Supported via `classification` parameter. Ask the user if they want post, story, or reels format if the platform supports multiple (Instagram, TikTok, YouTube).
- **Rate**: Create posts one at a time (not in bulk API calls) to surface any per-post validation errors clearly.

---

## Brand context

If `brand-profile.md` exists in the working folder (created by `content-brand-setup`), read it before writing anything and apply its voice, banned words, emoji/hashtag policy, and CTA conventions to every draft this skill produces. If it doesn't exist, proceed with a neutral professional tone and suggest running `content-brand-setup` once at the end.

## The article-to-pack derivation (the first thing to run)

This skill's input is a published article URL, not a raw topic. The steps above are the drafting engine; this section defines what feeds it.

1. **Fetch the article** (WebFetch, or Firecrawl when available) and extract: the core claim, 3–5 standout stats or quotes, the section structure, and the CTA target.
2. **Derive the default pack** (adapt to what the user asks for):
   - LinkedIn: 1 insight post or carousel copy (slide-by-slide text)
   - X/Twitter: 1 thread (5–8 tweets) walking the article's argument
   - Google Business Profile: 1 short post with CTA, if relevant to the brand
   - Newsletter: 1 ~100-word teaser section
   - 3 quote hooks for future graphics
3. **Run Steps 1–4 above** with these as the prefilled inputs — same preview-before-commit rule: show the full pack, get approval, then create.
4. **Group the pack.** If the Planable connector offers campaigns, create one named after the article and assign all pack drafts to it; otherwise apply one shared label named after the article (`list_labels` / `create_label`). Either way the pack stays traceable to its source. Items with no Planable page (the newsletter teaser, quote hooks) go in SOCIAL-PACK.md only. Reference the article URL in each draft's internal note or comment where the platform supports it.

## Final output

Drafts in Planable grouped as one campaign (or sharing one label), plus **SOCIAL-PACK.md** — a summary table (platform, hook, status, Planable link) the user can skim to see the whole pack at a glance.
