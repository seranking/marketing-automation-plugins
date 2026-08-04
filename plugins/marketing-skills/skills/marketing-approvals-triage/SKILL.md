---
name: marketing-approvals-triage
description: >
  Fetch every post pending approval across Planable workspaces and triage the queue by what's
  actually at stake — posts blocking an active `mkt:*` campaign or scheduled within 48h rise to
  the top, posts targeting live demand windows get flagged, and stale approvals are aged — so
  the user clears what matters first instead of reading chronologically. Use when the user asks
  "what needs my approval and what's urgent", "triage the approval queue", "what's blocking our
  campaigns", "what's stuck and does it matter", "approval queue by priority", or wants a
  morning check-in that ranks the queue. Requires the Planable MCP (SE Ranking optional — adds
  the demand-window flag). For a plain chronological approvals list, use
  pending-approvals-roundup from the smm-skills plugin instead, when that plugin is installed.
---

# Approvals impact triage

Surface all posts waiting for approval across one or more Planable workspaces — ranked by stake, not by date, so the queue reads as "what to clear first and why".

## How this skill works

1. Identify which workspaces to check
2. Fetch posts filtered by approval status
3. **Triage by stake — campaign blockers and deadline risks first (signature step)**
4. Summarize clearly — priority-ordered, grouped by workspace/client, with the reason each item ranks where it does

---

## Step 1: Scope the check

Ask (or infer from context):

- **Which workspaces?** — a specific client, a list, or all workspaces
- **Whose approvals?** — posts pending the user's own approval (`PENDING_MY_APPROVAL`) or all pending posts (`NOT_APPROVED`, `PARTIAL_APPROVED`)

**Default behavior if not specified:** fetch `PENDING_MY_APPROVAL` across all workspaces (or the most recently active ones if there are many).

---

## Step 2: Fetch pending posts

For each workspace in scope:

```
list_posts(
  workspaceId,
  approvalStatus: ["PENDING_MY_APPROVAL"]   // or NOT_APPROVED, PARTIAL_APPROVED
)
```

**Approval status reference:**
| Status | Meaning |
|---|---|
| `PENDING_MY_APPROVAL` | Posts where this user is a required approver |
| `NOT_APPROVED` | Posts with no approvals yet |
| `PARTIAL_APPROVED` | Posts with some but not all required approvals |
| `REQUESTED_BY_ME` | Posts where this user requested approval from others |

If the user wants a broader view, query `NOT_APPROVED` and `PARTIAL_APPROVED` together.

---

## Step 3: Triage by stake (signature step)

Rank every pending post into priority tiers — the reason is part of the output:

- **P1 — blocking:** carries an active campaign label (`list_labels` → labels matching `mkt:*`; a pending post wearing one is holding a measured campaign back), OR scheduled within 48h. An unshipped campaign can't move any metric — these clear first.
- **P2 — this week:** scheduled within 7 days, or part of a labeled batch that's partially live (some posts published, this one stuck — the batch's rhythm breaks).
- **P3 — demand window** *(only when the SE Ranking MCP is connected and the brand's domain is known)*: undated posts whose topic currently has live demand — a quick `DATA_getKeywordsMetrics` / `DATA_getKeywordQuestions` check on the post's core topic. A post about a peaking topic is worth approving before the peak passes.
- **P4 — the rest:** undated, no campaign, no deadline. Flag anything pending **>7 days** with its age — stale approvals are where content goes to die.

## Step 4: Organize and present results

**If results exist**, present in priority order, grouped by workspace (client) within each tier. For each post show:
- Platform/page name
- Post preview (first 100–150 chars of `textPreview`)
- Scheduled date (if set)
- Current approval status + approvals received vs. required (if available)
- **The triage reason** — why it sits in this tier

**Format:**

```
🔴 P1 — clear these first
📋 [Workspace name]
  • [Platform] — "[Post preview...]"
    Scheduled: tomorrow 10:00 ⚠️ · Status: not approved
    Why: blocks mkt:ai-vis-2026q3 (campaign has 2/4 posts live)

🟡 P2 — this week
  • ...

🔵 P3 — demand window open
  • [Platform] — "[Post preview...]" · No date set
    Why: topic "{kw}" at {volume}/mo right now

⚪ P4 — no deadline pressure
  • ... (pending 12 days — nudge or kill)
```

**If no pending posts**, say so clearly. Don't pad the response.

---

## Step 5: Optional follow-up actions

After showing results, offer:
- "Want me to show you the full text of any of these?"
- "Should I check a specific workspace in more detail?"
- If P1 items block a campaign: "Once these are approved and live, `marketing-campaign-impact` can start measuring the campaign."
- For the full weekly picture (approvals + calendar + campaigns + demand): `marketing-weekly-ops`.

---

## Edge cases

- **Many workspaces**: If `list_workspaces` returns more than 10, ask the user to confirm scope before fetching each one. Fetching 20+ workspaces sequentially is slow and may not be what they want.
- **No workspace specified**: Default to checking the 5 most recently active workspaces unless the user says "all clients."
- **Scheduled posts with errors**: If `WITH_ERRORS` posts appear in results, call them out separately — these are likely blocking issues.
