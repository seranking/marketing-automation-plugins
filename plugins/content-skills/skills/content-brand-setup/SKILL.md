---
name: content-brand-setup
description: One-time brand onboarding for all content-skills. Interviews the user about brand voice, audience, product, banned words, competitors, and CTA and formatting conventions, then saves a brand-profile.md that every other content-skills skill applies automatically — so briefs, plans, and social drafts come out on-brand instead of generic. Use whenever the user wants to set up or update their brand context — "set up my brand profile", "make outputs sound like us", "brand voice setup", "add our banned words", "update the brand profile", "why do the drafts sound generic" — and suggest it proactively the first time any content-skills skill runs without a brand-profile.md present. No MCPs required.
---

# Content Brand Setup

Create or update `brand-profile.md` — the shared brand context every content-skills skill reads automatically. Five minutes once; on-brand briefs, plans, and drafts in every skill after that.

## Why this exists

Without shared context, every skill produces competent-but-generic output, and the user re-explains their brand in every conversation. One small profile file fixes both: skills read it when present and write in the brand's voice from the first draft. This is the difference between "a good draft" and "a draft we'd actually post".

## Process

1. **Check for an existing profile.** If `brand-profile.md` already exists in the working folder, read it, summarize what's on file, and ask what to update — don't re-interview from scratch.

2. **Interview.** Ask in 2–3 compact rounds, not 15 separate questions. Cover:
   - **Brand and product**: name, domain, one-line description, main markets and languages
   - **Audience**: who reads and buys, their sophistication level, 1–2 personas if the team uses them
   - **Voice**: 3 adjectives plus one example sentence that "sounds like us"; formality level; emoji and hashtag policy per platform
   - **Boundaries**: banned words and phrases, competitors never to name, claims to avoid, compliance notes
   - **Conventions**: default CTAs, link and UTM habits, preferred deliverable language

   Accept partial answers — a profile with three filled sections beats no profile. Mark unknowns as `TBD` so the next update knows where the gaps are.

3. **Write `brand-profile.md`** in the working folder using the template below. Keep it under ~80 lines — every content-skills skill reads this file, so brevity is a feature, not a limitation.

4. **Confirm.** Show the saved profile and note that all content-skills skills will now pick it up automatically — no further setup needed.

## brand-profile.md template

```markdown
# Brand profile: {Brand}
Updated: {YYYY-MM-DD}

## Brand & product
- Name / domain:
- What it is (1 line):
- Markets & languages:

## Audience
- Primary audience:
- Sophistication level:
- Personas:

## Voice
- 3 adjectives:
- Example sentence that sounds like us:
- Formality:
- Emoji / hashtags policy:

## Boundaries
- Banned words & phrases:
- Competitors not to mention:
- Claims to avoid:

## Conventions
- Default CTA:
- Deliverable language:
- Other:
```

## Notes

- The profile is a plain markdown file the user owns — they can edit it by hand anytime; skills always read the latest version.
- If the user manages multiple brands, create one file per brand (`brand-profile-{slug}.md`) and ask which one to apply at the start of each skill run.
