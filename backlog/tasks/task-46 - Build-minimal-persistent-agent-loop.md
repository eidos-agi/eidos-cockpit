---
id: TASK-46
title: Build minimal persistent agent loop
status: To Do
assignee: []
created_date: '2026-03-01 07:34'
labels:
  - agent-core
  - phase-0
  - critical-path
milestone: m-7
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
The build sequence assumes 'the agent takes over' after Phase 0, but no persistent agent exists. Claude Code sessions end when Daniel closes the terminal.

Need a thin agent loop that:
1. Pulls tasks from a queue (backlog, GitHub issues, or simple task file)
2. Runs Claude Code sessions to execute them
3. Logs results to audit trail
4. Runs on a schedule or daemon (cron, systemd, Railway worker)

This transforms the narrative from 'human uses AI tool' to 'agent operates autonomously with human checkpoints.' Weekend-scale effort that changes the story.

Options:
- eidos-v5 orchestrator (most aligned, unclear maturity)
- Simple cron + claude -p loop (fastest to build)
- Railway worker service (runs in cloud)
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Agent loop runs without Daniel at terminal
- [ ] #2 Tasks pulled from queue and executed
- [ ] #3 Results logged to persistent audit trail
- [ ] #4 Human can review what agent did async
<!-- AC:END -->
