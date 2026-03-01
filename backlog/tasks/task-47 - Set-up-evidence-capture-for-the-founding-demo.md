---
id: TASK-47
title: Set up evidence capture for the founding demo
status: To Do
assignee: []
created_date: '2026-03-01 07:34'
labels:
  - phase-0
  - demo
  - evidence
milestone: m-7
dependencies: []
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
The founding story IS the product demo, but there's no plan to record it:
- No video/screen recording setup
- Helios audit trail only persists locally
- No structured logging of agent actions to cloud
- No before/after evidence chain

Before Phase 1 begins, set up:
1. Helios audit log persistence to cloud (part of cloud hub migration)
2. Screen recording for key moments (Helios GIF creator, OBS, or Loom)
3. Git-based evidence trail (agent commits its work products)
4. Timeline document that captures each milestone with proof
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Audit logs persist to cloud database
- [ ] #2 Key agent actions produce screen recordings or GIFs
- [ ] #3 Evidence timeline document exists with proof artifacts
- [ ] #4 An investor could watch/read the founding story end-to-end
<!-- AC:END -->
