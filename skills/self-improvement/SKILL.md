---
name: self-improvement
description: >
  Self-improving feedback loop for Solo OS. Tracks experiments (hypothesis testing),
  kills false beliefs (assumptions proven wrong), and runs weekly self-assessment.
  Use when the user says "weekly review", "what experiments are running",
  "what did we learn", "you should have caught that", "why didn't you know about X",
  "system improvement", "what's not working", "optimize the system",
  "add to false beliefs", "new experiment", or "Sunday review".
version: 0.1.0
---

# Self-Improvement System

Solo OS improves itself through structured experimentation and honest self-assessment.

## experiments.md

Track system improvements with data, not assumptions. Every change to the system should be an experiment with a measurable outcome.

### Format

```markdown
# Experiments Tracker

## Active Experiments
| ID | Hypothesis | Change Made | Metric | Baseline | Current | Started | Status |
|----|-----------|------------|--------|----------|---------|---------|--------|

## Completed Experiments
| ID | Hypothesis | Result | Action Taken | Date |
|----|-----------|--------|-------------|------|

## Proposed (not yet started)
| ID | Hypothesis | Expected Effort | Blocked By |
|----|-----------|----------------|------------|
```

### Protocol
- Every system change gets an experiment ID (E001, E002, etc.)
- Define the metric BEFORE making the change
- Evaluate at weekly review (Sunday)
- PASS: keep the change, document what worked
- FAIL: revert the change, document what didn't work
- RUNNING: not enough data yet, keep observing

## false-beliefs.md

Killed assumptions with evidence. Prevents the system from repeating mistakes across sessions.

### Format

```markdown
# False Beliefs

## Killed
| Belief | Evidence Against | Date Killed |
|--------|-----------------|-------------|

## Under Investigation
| Belief | Why Questioning | Evidence Needed |
|--------|----------------|----------------|
```

### Protocol
- When an assumption is proven wrong, add it with evidence
- Check this file before making architectural decisions
- If a proposed change relies on a killed belief, flag it
- "Under Investigation" tracks beliefs being questioned but not yet disproven

## Weekly Review (Sunday or user-triggered)

Run this assessment:

1. **Score the week:** What was accomplished? What was missed? What stalled?
2. **Evaluate experiments:** Update all RUNNING experiments with new data. Promote or kill.
3. **Check for false beliefs:** Did anything assumed true turn out wrong?
4. **Blind spot detection:** Look for revenue gaps, missed follow-ups, stale relationships, competing priorities.
5. **Pushback check:** Is the user overloaded? Should something be paused or killed?
6. **Propose improvements:** 1-2 new experiments based on observed gaps.

### Improvement Triggers
- User says "you should have caught that" -> add detection rule
- Meeting without prep -> add to calendar scan
- Deal stalls without alert -> tighten threshold
- Opportunity missed from lack of cross-reference -> add cross-reference step
- Entity not found in ROUTER.md -> add to /update enforcement
