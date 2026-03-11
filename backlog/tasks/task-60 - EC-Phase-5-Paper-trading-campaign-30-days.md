---
id: TASK-60
title: 'EC Phase 5: Paper trading campaign (30+ days)'
status: To Do
assignee: []
created_date: '2026-03-11 07:33'
labels:
  - eidos-capital
  - phase-5
  - paper-trading
milestone: m-7
dependencies:
  - TASK-58
  - TASK-59
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Deploy full autonomous loop on Railway: data collection → features → momentum signals → risk check → paper order → position tracking → P&L. Run for 30+ days. Build daily reporting (src/portfolio/reporting.py). Add 1-2 additional strategies (mean reversion, quality factor) for multi-strategy testing.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 30 days of daily P&L data with no gaps
- [ ] #2 No unhandled exceptions during campaign
- [ ] #3 Daily report generated automatically
- [ ] #4 Multiple strategies running with correlation monitoring
- [ ] #5 All data stored: signals, orders, positions, P&L, risk snapshots
<!-- AC:END -->
