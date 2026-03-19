---
description: Morning briefing. Load state, check overnight, compile today's plan.
allowed-tools: Read, Grep, Glob, Bash, Agent
---

You are running the Solo OS morning briefing. Execute these steps in order:

## Step 1: Boot
Read these files (they should already be loaded, but verify):
1. `CURRENT_STATE.md`
2. `ROUTER.md`
3. `CHECKPOINT.md`

## Step 2: Check overnight results
Look for the most recent file in `outputs/overnight/`. If it exists, extract: sub-agent status, emails triaged, approval queue items, meetings prepped, content drafts, alerts.

## Step 3: Pull calendar
If ~~calendar is connected, pull today's events. For each meeting, look up attendees in ROUTER.md and load their people files.

## Step 4: Pull pipeline
If ~~crm is connected, scan active deals. Flag any deal with no activity in 14+ days.

## Step 5: Check task board
Read TASKS.md. Identify: overdue items, critical items, items blocked on others.

## Step 6: Compile briefing
Present to the user in this format:

```
CALENDAR TODAY
[List of meetings with time, attendees, CRM context]

PIPELINE STATUS
[Active deals with stage, next action, days since last activity]

APPROVAL QUEUE
[Items waiting for user decision, with recommended action]

TOP 3 TODAY
1. [Most important, with rationale]
2. [Second]
3. [Third]

BLOCKERS / ALERTS
[Anything that needs attention: stale deals, overdue tasks, calendar conflicts]
```

Keep it concise. No fluff. The user should be able to read this in 2 minutes and know exactly what to do today.
