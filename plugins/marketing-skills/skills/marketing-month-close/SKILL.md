---
name: marketing-month-close
description: >
  Close the month with a decision, not just a recap: a client-ready social performance summary
  (Planable analytics or a CSV export) that ENDS in next month's content plan — the month's
  winning topics crossed with next month's live search and question demand (SE Ranking) become
  6–10 proposed calendar seeds, ready to hand to marketing-post-batch. Use when the user says
  "close out the month", "month review and what's next", "monthly report plus next month's
  plan", "how did we do and what should we do next month", or wants the month's results turned
  into the next month's calendar. Requires both MCPs (CSV mode works too — demand still pulls if
  the domain is known). For a recap without the forward plan, use monthly-performance-summary
  from the smm-skills plugin; for the cross-channel period report, use marketing-report.
---

# Month close & next plan

Generate a plain-language performance summary from Planable analytics data or a CSV export — then do what a report never does: **decide next month**. The month's winners are crossed with next month's demand, and the deliverable ends in a proposed calendar, not a conclusion.
Works in two modes: connected (pulls data live via Planable) or CSV (user uploads their own export).

---

## Step 0: Detect the input mode

**Mode A — Planable connector** (default)
The user has a Planable account connected and wants to pull live data.

**Mode B — CSV upload**
The user has uploaded (or will upload) a CSV export from Planable or any social media analytics tool.

**How to detect:**
- If the user uploads a file → Mode B
- If the user mentions "export", "CSV", "spreadsheet", or "file" → Mode B
- If the user mentions a workspace name, client, or Planable directly → Mode A
- If unclear, ask: "Do you have a CSV export to upload, or should I pull the data from your Planable workspace?"

---

## Mode A: Planable connector

### Step 1: Gather inputs

- **Workspace / client name** — which workspace to report on
- **Date range** — default to previous calendar month if not specified
- **Platforms to include** — all pages by default, or specific platforms if requested

### Step 2: Fetch data

```
list_workspaces → find workspaceId
list_pages(workspaceId) → get all pageIds
get_page_metrics(workspaceId, pageIds, startDate, endDate)
get_page_metrics_summary(workspaceId, pageIds, startDate, endDate)
get_post_metrics_summary(workspaceId, pageIds, startDate, endDate)
```

For top post highlights, also call:
```
get_post_metrics(workspaceId, pageIds, startDate, endDate, limit: 5)
```

**Note on unsupported platforms:** `get_page_metrics` returns `unsupportedPages` for Google My Business and Threads — note these in the report as "data not available."

### Step 3: Structure the report → go to [Report Format](#report-format)

---

## Mode B: CSV upload

### Step 1: Accept the file

If no file is uploaded yet, ask: "Please upload your CSV export and I'll summarize it."

Supported sources: Planable Analytics export, Hootsuite, Sprout Social, Meta Business Suite, LinkedIn Analytics, or any tabular CSV with date, platform, and metric columns.

### Step 2: Parse the CSV

Read the uploaded file. Identify:
- **Date column** — look for `date`, `period`, `week`, `month`, or similar
- **Platform column** — look for `platform`, `channel`, `network`, `page`, `profile`
- **Metric columns** — impressions, reach, views, engagement, likes, comments, shares, followers, engagement rate

If the structure is unclear or columns are ambiguous, ask one clarifying question before proceeding.

**Flexible parsing rules:**
- Column names may vary by source — match by keyword, not exact name
- Aggregate by platform if data is at post level
- Handle blank rows and footer rows gracefully (skip non-numeric rows in metric columns)
- If a date range is not obvious from the data, ask the user to confirm what period the export covers

### Step 3: Structure the report → go to [Report Format](#report-format)

---

## Report Format

Deliver the report as a **downloadable HTML file** — a self-contained visual report the user can open in a browser, share with a client, or screenshot. Also output a short plain-text summary in chat so the user can see the key numbers without opening the file.

---

### Visual HTML report

Generate a single `.html` file saved to the outputs folder as `[client-name]-[month]-close.html`. Use `present_files` after creating it.

The report must be visually designed — not a raw data dump. Follow these design principles:

**Aesthetic direction:** Clean, modern agency report. Light background, bold accent color (use Planable green `#3BBA7B` as primary accent). Card-based layout. Generous whitespace. Clear hierarchy.

**Required sections and visual treatment:**

#### 1. Header
- Client name (large)
- Month + year
- Tagline: "Social Media Performance Report"
- Subtle gradient bar or divider in Planable green

#### 2. 📊 Headline numbers — KPI cards
4 metric cards in a row (or 2×2 on narrow):
- 👥 **Total audience** (followers across all platforms)
- 👀 **Total impressions**
- 💬 **Total engagement**
- 📅 **Posts published**

Each card: big bold number, label, and trend arrow + % change vs. prior period (if available). Use color: green for up ↑, red for down ↓, gray for flat.

#### 3. 📱 Platform breakdown
One card per platform. Each card shows:
- Platform emoji + name (use: 📘 Facebook, 📸 Instagram, 💼 LinkedIn, 🐦 X/Twitter, 🎵 TikTok, ▶️ YouTube, 📌 Pinterest)
- Audience / followers
- Impressions
- Engagement rate (shown as a mini visual bar, CSS only — width = engagement rate %, max 100%)
- Posts published
- One-line notable (italic, lighter text)

#### 4. 🏆 Top posts
Up to 5 posts in a ranked list. Each entry:
- Rank badge (1st, 2nd, 3rd...)
- Platform emoji
- Post text preview (truncated to ~120 chars, in quotes)
- Key metric pill (e.g. "847 engagements" or "12K impressions") — styled as a badge

#### 5. ✅ What worked / ⚠️ What to watch
Two columns side by side:
- Left: "✅ What worked" — 2–3 bullet points in green
- Right: "⚠️ What to watch" — 1–2 bullet points in amber/yellow

Keep bullets short and specific. No generic advice.

#### 5b. 📅 Next month plan (signature section)
The section that turns the report into a decision. Build it from two inputs:
- **The month's winners** — the top posts' topics and formats (already pulled above).
- **Next month's demand** — for the brand's 3–5 core topics (ask for the domain if unknown): `DATA_getKeywordQuestions` (live questions to answer) + `DATA_getRelatedKeywords` (volume, and seasonal direction where 12-month history shows it). Sequential calls; never invent volume — nulls render as "unknown".

Output a proposed-calendar table of 6–10 seeds:

| # | Topic seed | Why (winner × demand) | Volume / questions | Platform | Format (from winning patterns) |
|---|---|---|---|---|---|
| 1 | {topic} | top post {X} ER × {kw} rising | {n}/mo · {n} q's | LinkedIn | question-hook |

Close the section with: "Draft these with `marketing-post-batch` — each seed carries its demand figure in." If the SE Ranking MCP isn't connected or no domain is known, keep the winners-only version of the table and say the demand column is unavailable.

#### 6. 📝 Notes
Small, muted section at the bottom. Flag data gaps, unsupported platforms, report freshness.

#### 7. Footer
"Generated with Planable + Claude · [date]"

---

### HTML/CSS requirements

- Single self-contained `.html` file — no external dependencies except Google Fonts (loaded via `<link>`)
- Use a distinctive font: `DM Sans` or `Plus Jakarta Sans` for body, `Sora` or `Bricolage Grotesque` for headings
- CSS variables for all colors — define at `:root`
- Responsive: works at 900px+ desktop width; cards wrap gracefully on smaller screens
- No JS required — pure HTML/CSS
- Print-friendly: `@media print` block that hides nothing essential
- Use real data from the report — no placeholder copy

**Color palette:**
```css
:root {
  --green: #3BBA7B;
  --green-light: #EAF7F0;
  --amber: #F59E0B;
  --red: #EF4444;
  --text: #1A1A2E;
  --text-muted: #6B7280;
  --bg: #F8FAFC;
  --card: #FFFFFF;
  --border: #E5E7EB;
}
```

---

### Plain-text chat summary

After presenting the file, also output a short in-chat summary using emojis for scannability:

```
📅 [Client] — [Month Year]

📊 Key numbers
👥 Audience: [X] [↑/↓ X% vs last month]
👀 Impressions: [X]
💬 Engagement: [X] ([X]% rate)
📅 Posts: [X]

📱 By platform
[emoji] [Platform]: [X] impressions · [X]% eng rate · [X] posts
...

🏆 Top post: "[preview]" — [X] engagements

✅ What worked: [one line]
⚠️ Watch: [one line]
📅 Next month: [top 3 seeds from the plan, comma-separated]
```

Keep the in-chat summary to ~15 lines max. The HTML file is the full deliverable.

---

## Tone guidelines

- Write for a human reader, not a dashboard
- Use plain language — avoid jargon like "CTR optimization" or "algorithmic reach"
- Be specific with numbers; round where appropriate (e.g., "~12K" not "12,341")
- Don't pad with filler or repeat the same numbers in multiple sections
- If data is thin or the period was quiet, say so — don't oversell a slow month
- Emojis are used for navigation and scannability, not decoration — every emoji should label something

---

## Edge cases

- **Multi-workspace report**: If the user wants a report across multiple clients, run Mode A per workspace and combine into a single summary with one section per client.
- **Partial month**: If the date range is mid-month, note this in the report header and caveat the numbers accordingly.
- **No data / empty results**: Say so clearly. Don't fabricate trends.
- **CSV with multiple sheets**: Ask the user which sheet to use, or process all sheets and note the sources.
- **Competitor data in CSV**: If the CSV contains competitor benchmarks, include a brief comparison section.
