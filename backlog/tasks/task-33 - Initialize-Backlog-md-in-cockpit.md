---
id: TASK-33
title: Initialize Backlog.md MCP in cockpit
status: To Do
assignee: []
created_date: '2026-02-26 06:00'
labels:
  - cockpit
  - setup
dependencies: []
milestone: m-4
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
The cockpit CLAUDE.md has Backlog.md MCP guidelines wired in, but the Backlog.md file itself hasn't been initialized via the MCP. The first pilot session in the cockpit should run the MCP initialization so tasks are managed properly going forward.

Currently tasks were created as raw markdown files. Once the MCP is initialized, these should be validated against the MCP's expected format or recreated through the MCP.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Backlog.md MCP works when Claude Code opens the cockpit repo
- [ ] #2 Existing task files are compatible with the MCP format
- [ ] #3 /takeoff reads backlog tasks as part of the boot sequence
<!-- AC:END -->
