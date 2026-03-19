---
description: Weekly accountability review. Score the week, evaluate experiments, plan next week.
allowed-tools: Read, Write, Edit, Grep, Glob
---

Run the solOS weekly accountability review. This is a structured self-assessment.

## Step 1: Load context
Read: CURRENT_STATE.md, TASKS.md, ROUTER.md, memory/systems/experiments.md, memory/systems/false-beliefs.md

## Step 2: Score the week
Analyze TASKS.md and recent session logs:
- How many tasks were completed vs planned?
- Which deals moved forward? Which stalled?
- Were there any missed follow-ups or deadlines?
- Did any opportunities surface that weren't captured?

## Step 3: Evaluate experiments
For each RUNNING experiment in experiments.md:
- Collect any new data from this week
- If conclusive: mark PASS or FAIL with evidence
- If not conclusive: note progress and keep RUNNING

## Step 4: Check for false beliefs
Did anything assumed true this week turn out wrong? If so, add to false-beliefs.md with evidence.

## Step 5: Blind spot detection
Actively look for:
- Revenue gaps (deals with no next action)
- Network connections not being exploited
- Timing opportunities about to expire
- Stale relationships (people not contacted in 14+ days who should be)
- Competing priorities (too many active projects for one person)

## Step 6: Pushback check
Count active projects and open tasks. If the user is overloaded:
- Show current load honestly
- Ask what gets paused or killed
- Flag if nothing pauses (unsustainable)

## Step 7: Next week priorities
Based on the review, propose:
- Top 3 priorities for next week (with rationale)
- 1-2 new experiments if gaps were found
- Any system changes needed

## Step 8: Write report
Save to `outputs/reviews/weekly-review-{YYYY-MM-DD}.md` and present summary to user.

Format: direct, no fluff. Data first, then recommendations. Challenge assumptions.
