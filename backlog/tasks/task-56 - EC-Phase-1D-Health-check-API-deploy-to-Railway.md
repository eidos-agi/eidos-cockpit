---
id: TASK-56
title: 'EC Phase 1D: Health check API + deploy to Railway'
status: To Do
assignee: []
created_date: '2026-03-11 07:32'
labels:
  - eidos-capital
  - phase-1
  - infra
milestone: m-7
dependencies:
  - TASK-53
  - TASK-55
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Extend FastAPI with /positions, /account, /data/status endpoints. Deploy to Railway. Verify health check works.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Service deployed on Railway with /health responding
- [ ] #2 /positions and /account return data from Alpaca
- [ ] #3 /data/status shows last collection timestamp
<!-- AC:END -->
