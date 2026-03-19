# solOS

A personal operating system for solo operators running multiple ventures without a team.

## What it does

solOS gives your AI assistant persistent memory across sessions, a system for finding any person, deal, or project in one hop, self-improving feedback loops, and structured daily workflows.

AI sessions are ephemeral. Your business needs continuity. solOS bridges that gap.

## Core features

**Tiered memory** with line budgets. Working memory (loaded every session, ~200 lines), short-term memory (loaded when you mention an entity), long-term memory (loaded for deep work), and vector memory (semantic search). Files auto-prune to prevent context overload.

**Entity routing.** ROUTER.md maps every person, deal, project, and term to a file path. When you say a name, the system finds the file in one hop instead of searching.

**Self-improvement.** Track experiments (hypothesis, metric, result). Kill false beliefs (assumptions proven wrong with evidence). Weekly review scores the week and proposes improvements.

**Daily workflows.** `/start` compiles a morning briefing from your calendar, pipeline, and task board. `/update` checkpoints the session, syncs state, and enforces memory budgets. `/review` runs weekly accountability.

## How to use

1. Install the plugin
2. Start a session. solOS boots with your state files automatically.
3. Work normally. Mention people, deals, or projects and the system retrieves context.
4. End every session with `/update` to persist what you learned.
5. Start each day with `/start` for your briefing.
6. Run `/review` on Sundays for weekly accountability.

## File structure

After first use, your workspace will have:

```
CURRENT_STATE.md     Today's focus and critical items (80 lines max)
ROUTER.md            Entity index (120 lines max)
CHECKPOINT.md        Last session state (40 lines max)
TASKS.md             Task board (250 lines max)
memory/
  people/            Relationship files (60 lines max each)
  projects/          Project summaries
  decisions/         Decision journal
  systems/           System configuration
    experiments.md   Active improvement experiments
    false-beliefs.md Killed assumptions
  glossary.md        Acronyms and shorthand
second-brain/
  inbox/             Raw brain dumps
  projects/          Deep deal research
  people/            Extended profiles
  knowledge/         Market research
outputs/
  overnight/         Automated processing results
  reviews/           Weekly review reports
archive/             Completed and deprecated items
```

## Who this is for

Solo founders, fractional executives, freelance consultants, agency owners, and anyone who is both the strategy and execution layer for multiple concurrent revenue streams.

The person who uses 8 SaaS tools, none of which talk to each other, and spends 2 hours a day on admin instead of revenue work.

## Requirements

- Claude Cowork (desktop app)
- Connected services via MCP connectors (CRM, calendar, email are recommended but optional)
