# Visual system for ecosystem reports

Shared by all three skills, so a footprint map, a demand campaign brief and an AI-visibility report read as one family. Values are taken verbatim from the **SER × PLNBL Co-brand Design System**.

**When this applies:** these reports are co-branded material — they present SE Ranking data and Planable data together. That is exactly when the co-brand system activates. Never restyle a report into one brand's colours alone.

**If the co-brand design system is reachable in the session** (a `ser-plnbl-cobrand-design` skill, a synced design system, or an `SER × PLNBL Cobrand` project), read it and prefer its values over the copies below — it is the source of truth and these are a snapshot.

## Tokens

```css
/* Canvases — pick one per report and keep it */
--canvas-green:  #0D2810;   /* deep forest green */
--canvas-indigo: #161466;   /* deep indigo */
--canvas-light:  #E5E6EC;   /* the only light canvas */
--surface-card:  #FFFFFF;   /* cards on light */

/* Text */
--text-on-dark:    #FFFFFF;
--text-on-light-i: #161466;  /* indigo text on light */
--text-on-light-g: #0D2810;  /* green text on light */
--ink-muted:       rgba(13,40,16,0.55);

/* Accents */
--mint:        #4FE7AD;   /* highlight on dark canvases */
--violet:      #5C3CFF;   /* highlight on light; indigo text */
--violet-soft: #A8A7FF;
--green-deep:  #006C58;
--highlight-green: #00B28E;

/* Structure */
--line: #E5E6EC;
--radius-chip: 8px;  --radius-label: 16px;  --radius-card: 24px;
--shadow-card: 0 8px 24px rgba(13,40,16,0.08);   /* the only sanctioned shadow */

/* Type */
--font-display: "TT Fors", "Helvetica Neue", Arial, sans-serif;
--font-body:    "TT Fors", "Helvetica Neue", Arial, sans-serif;
--font-mono:    "IBM Plex Mono", ui-monospace, "SF Mono", Menlo, monospace;
--text-metric: 34px;  --text-title: 28px;  --text-body: 16px;  --text-label: 13px;
--weight-regular: 400;  --weight-medium: 500;
```

Spacing is a strict 8-based scale: 8 / 16 / 24 / 32 / 40 / 48 / 56 / 64. Use it for every gap and pad — nothing off-grid.

**Fonts won't load.** The report is one self-contained file and the brand fonts aren't embeddable, so the stack falls back to Helvetica/Arial. That's expected and fine. Keep the mono family for stat-tile labels and metric values — that's where the brand voice actually shows.

## The four sanctioned combinations

Pick one per report. Never mix two.

| # | Canvas | Text | Highlight |
|---|---|---|---|
| 1 | `#0D2810` green | `#FFFFFF` | `#4FE7AD` mint |
| 2 | `#161466` indigo | `#FFFFFF` | `#4FE7AD` mint |
| 3 | `#E5E6EC` light | `#161466` indigo | `#5C3CFF` violet |
| 4 | `#E5E6EC` light | `#0D2810` green | `#00B28E` green |

Default to **combination 3** for these reports. They are dense, data-heavy documents that get read on a laptop and screenshotted into decks, and a light canvas with indigo text carries small type and table rules better than a dark one. Offer dark mode as a toggle using combination 2, not as the default.

## Charts

Read the `dataviz` skill before writing chart code and follow its procedure. The brand rules below sit on top of it, they don't replace it.

**Two series, light canvas — use `#5C3CFF` violet and `#00B28E` green.** This exact pair has been run through the validator and passes every check: chroma, CVD separation (ΔE 29.1 deutan), normal-vision floor (36.4), lightness band. It is the default chart pair.

**Two series, dark canvas — use `#00A382` green and `#7C63FF` violet.** Validated against the dark surface: passes every check.

Do **not** use `#4FE7AD` mint or `#A8A7FF` soft violet as chart series on a dark canvas. They are the right accents for headings, labels and highlights there, but as chart marks they fail the validator's lightness band (0.833 and 0.764, against a required ~0.48-0.67) — too bright to sit as data on a dark surface. The two colours above are darker steps of the same brand hues, so the chart still reads as the same family. Keep mint for the non-chart accents.

**Three or more categories — do not invent brand hues to fill the slots.** The brand palette is deliberately small and a five-colour version of it does not survive the validator: a hand-built violet ramp failed the normal-vision floor at 11.6 between adjacent steps, meaning readers with full colour vision can't reliably tell two segments apart. Instead, encode with length and label directly:

- Give the whole series one hue and put the value inside or beside each segment, with a 2px surface gap between segments. Length carries the meaning; colour is just ink.
- Or facet into small multiples.

This is the right call on its own terms, not a compromise. A five-segment bar where every segment is labelled needs no colour legend at all.

**Colour follows the entity, never the rank.** In a share-of-voice chart the brand gets the accent and competitors stay neutral grey, whatever order they land in. Re-sorting must never repaint the bars.

**Never eyeball a palette — run `scripts/validate_palette.js` from the dataviz skill.** If a check fails, fix it before shipping. A contrast WARN obliges visible labels or a table view; it is not dismissable.

## Layout conventions

Same across all three reports, so they're recognisable as a set:

- **Header** — title, then a single meta line (brand · market · period · workspace), then the real coverage counts. Never rounded.
- **Caveat callout** directly under the header when the data needs one, in a bordered block on the canvas colour.
- **Stat tiles** — a row of up to four. Metric value in mono at `--text-metric`, label above in mono at `--text-label`, one line of plain-language context below. `--radius-card`, `--shadow-card`, white on light canvas.
- **Tables** — hairline `--line` rules, no zebra striping, mono for numbers so columns align, generous row padding from the 8-grid.
- **Chips** for filters and states — `--radius-chip`, `--line` fill when inactive, highlight colour when active, count in the label.
- **Collapsed `<details>`** for limits and methodology. Present, not dominant.
- Everything inline in one file: CSS, JS, data. No external assets, no network calls, works offline.
