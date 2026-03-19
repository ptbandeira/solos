---
description: End-of-session checkpoint. Sync state, update router, enforce budgets.
allowed-tools: Read, Write, Edit, Grep, Glob, Bash
---

End-of-session protocol. Capture everything before context resets. Execute ALL steps.

## Step 0: Write CHECKPOINT.md FIRST
Before reading or updating any other file, write CHECKPOINT.md from what you know:

```markdown
# CHECKPOINT
**Written:** {YYYY-MM-DD HH:MM}
**Context:** {1-line: what this session was about}
**Status:** {COMPLETE | IN_PROGRESS | BLOCKED}

## Completed This Session
- {item}

## Pick Up Here
1. {Next action, specific enough to execute without reading back}

## Files Modified
- {file}: {what changed}

## Decisions Made
- {decision}: {rationale, 1 line}

## Don't Forget
- {anything that would be lost in compaction}
```

Max 40 lines. Write this FIRST. If /update gets interrupted, the checkpoint survives.

## Step 1: Read current state
Read CURRENT_STATE.md and TASKS.md.

## Step 2: Session analysis
Review the full conversation. Extract:
- Completed tasks and deliverables
- Open loops for next session
- Deal status changes
- New intel about people, companies, projects
- Decisions made (with rationale)
- Items user committed to doing themselves
- Blockers discovered

## Step 3: Update CURRENT_STATE.md
Rewrite with fresh data. Keep to 80 lines max. Session logs: keep last 3 entries only.

## Step 4: Update TASKS.md
Mark completed tasks. Add new tasks. Update status on in-progress items.

## Step 5: Update memory files
If new intel was learned about a person, project, deal, or decision, update the relevant file.

## Step 5b: MANDATORY - Update ROUTER.md
This step is NOT optional. Check ROUTER.md against session activity:
- New person mentioned? ADD to People table with file path (create file if needed)
- Deal stage changed? UPDATE Deals table
- New project started? ADD to Projects table
- Person's role/context changed? UPDATE their row
- New glossary term used? ADD to Glossary table

ROUTER.md must always reflect ground truth. If it's wrong, the next session starts blind.

## Step 5c: Enforce line budgets
Check and enforce:
- CURRENT_STATE.md: 80 lines max. If over, move session logs to archive/
- ROUTER.md: 120 lines max. If over, archive inactive entities
- People files: 60 lines max each. If over, move detail to second-brain/people/
- TASKS.md: 250 lines max. If over, archive completed items

Log any trimming done.

## Step 5d: Self-improvement check
If it's Sunday, OR if the user expressed frustration with the system:
- Read experiments.md, update RUNNING experiments
- If an assumption was proven wrong, add to false-beliefs.md
- Propose 1 new experiment if a gap was discovered

## Step 6: Archive session notes
Create: `second-brain/inbox/session-{YYYY-MM-DD}-{HH}.md` with: accomplished, decisions, new intel, carry forward.

## Step 7: Confirm
Report to user (short):
- Items updated in CURRENT_STATE.md
- Tasks updated in TASKS.md
- Memory files updated
- Top carry-forward item
- Any line budget trimming done
