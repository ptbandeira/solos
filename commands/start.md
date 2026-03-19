---
description: Morning briefing. Load state, check overnight, compile today's plan.
allowed-tools: Read, Grep, Glob, Write, Bash
---

You are running the Solo OS morning briefing. Before anything else, run the first-run check.

---

## FIRST-RUN CHECK

Look for `CURRENT_STATE.md` in the workspace.

**If CURRENT_STATE.md does NOT exist:** Run the ONBOARDING WIZARD from the solo-os-core skill. Do not proceed to the morning briefing until onboarding is complete.

**If CURRENT_STATE.md exists:** Proceed directly to Step 1 below.

---

## Step 1: Boot
Read these files (verify they exist and are within line budgets):
1. `CURRENT_STATE.md` (max 80 lines)
2. `ROUTER.md` (max 120 lines)
3. `CHECKPOINT.md` (if present)

If any file is missing, note it but do not stop — the user may still be setting up. Offer to create it.

## Step 2: Check overnight results
Look for the most recent file in `outputs/overnight/`. If it exists, extract: sub-agent status, emails triaged, approval queue items, meetings prepped, content drafts, alerts.

## Step 3: Pull calendar
If ~~calendar is connected, pull today's events. For each meeting, check ROUTER.md and load attendee files from `memory/people/`.

## Step 4: Pull pipeline
If ~~crm is connected, scan active deals. Flag any deal with no activity in 14+ days.

## Step 5: Check task board
If TASKS.md exists, read it. Identify: overdue items, critical items, items blocked on others.

## Step 6: Compile briefing

Present in this format. Keep it under 2 minutes to read.

```
GOOD MORNING — [date]

CALENDAR TODAY
[Meetings with time, attendees, quick context. "No meetings" if empty.]

PIPELINE STATUS
[Active deals with stage, next action, days since last touch. Skip if no CRM.]

APPROVAL QUEUE
[Items waiting for your decision. Skip if empty.]

TOP 3 TODAY
1. [Most important, with one-line rationale]
2.
3.

BLOCKERS / ALERTS
[Stale deals, overdue tasks, anything that needs your attention]
```

No fluff. No pleasantries. Just the brief.
