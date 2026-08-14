# Ecosystem Skills for Claude

The three core use cases for the [SE Ranking](https://seranking.com) + [Planable](https://planable.io) ecosystem, in one plugin.

Search, AI answers and social don't move independently. A brand strong on a topic in one is usually doing something in the others too — and the places where they *disagree* are the most useful thing on the page. These three skills line those channels up, then act on what the gaps show.

```
SEE  (whole-footprint map)  →  ACT  (demand campaign / AI visibility)  →  SEE AGAIN  (re-run the map)
```

Every campaign ships instrumented — a Planable label (`mkt:<slug>`) plus a **campaign passport** recording the starting numbers — so re-running the map answers the question every campaign eventually faces: *did anything move?*

Learn more: [Marketing Skills for Claude](https://seranking.com/claude-marketing-skills.html) · Skills follow [Anthropic's Agent Skills spec](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

## Skills (3)

| Skill | What it produces |
| --- | --- |
| [ecosystem-footprint-map](skills/ecosystem-footprint-map/SKILL.md) | Your topics across search, AI answers and social in one matrix — which channels agree, which topics only one carries, and the routed next move for each |
| [ecosystem-demand-campaign](skills/ecosystem-demand-campaign/SKILL.md) | Real search demand — keyword gaps, competitor wins, ranking losses, question keywords — turned into a labelled, passport-backed social campaign drafted in Planable |
| [ecosystem-ai-visibility](skills/ecosystem-ai-visibility/SKILL.md) | The AI-answer narratives competitors own and you don't, turned into citable social content with baseline share-of-voice recorded |

## How they chain

Start with `ecosystem-footprint-map`. It produces a routed list — every topic ends with a state and a move, and each move names the skill that executes it:

- *ranks but social has never touched it* → `ecosystem-demand-campaign`
- *cited in AI answers but thin everywhere else* → `ecosystem-ai-visibility`

Run the campaign. Wait 2–8 weeks. Re-run the map — it reads the campaign passport and diffs against the baseline.

You can also enter from either campaign skill directly if you already know what you want to target.

## Two seats, one workflow

The same three skills serve two readers:

- **From the search seat** — see your whole footprint instead of one channel at a time, and find where a competitor's channels contradict each other.
- **From the social seat** — find the ranking topics your social has never touched, and show which of your winning posts spotted demand before the keyword data did.

The footprint map detects which seat you're in and leads with the section that matters to you. The underlying analysis is identical.

## What it can't do

Stated up front, because these are the questions that come up in client meetings:

- **No causation.** The map reports which topics move together across channels. It cannot show that a post caused a ranking, or a ranking caused a citation.
- **No sentiment.** SE Ranking's AI Search tools expose presence, share of voice and the prompts behind them — not how a brand is talked about.
- **No conversion data.** No GA4, no Looker. This maps visibility and engagement, not revenue.
- **Google Business Profile and Threads return no metrics** through the Planable connector.
- **Publishing stays in Planable.** Campaign skills create drafts; nothing goes live without approval.

## Install

Add the marketplace, then install:

```
/plugin marketplace add seranking/marketing-automation-plugins
/plugin install ecosystem-skills
```

Both MCP servers (SE Ranking + Planable) are registered automatically on install. You'll need an account on each.

## Relationship to the other plugins

This plugin is deliberately small — the three use cases the SE Ranking and Planable teams are promoting together. If you want the full cross-channel pipeline (23 skills: gap matrices, local, PPC counterplay, agency workflows, reporting), install `marketing-skills` from the same marketplace. `ecosystem-demand-campaign` and `ecosystem-ai-visibility` are the ecosystem-branded equivalents of `marketing-seo-campaign` and `marketing-ai-campaign`; the skills carry different names so both plugins can be installed side by side without duplicates.

## License

MIT — see [LICENSE](LICENSE).
