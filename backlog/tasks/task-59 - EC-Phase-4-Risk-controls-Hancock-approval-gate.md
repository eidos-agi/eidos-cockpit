---
id: TASK-59
title: 'EC Phase 4: Risk controls + Hancock approval gate'
status: To Do
assignee: []
created_date: '2026-03-11 07:33'
labels:
  - eidos-capital
  - phase-4
  - risk
  - hancock
milestone: m-7
dependencies:
  - TASK-57
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Build risk monitor (src/risk/monitor.py) with real-time position limit checking, exposure tracking, circuit breakers. Integrate pre-trade risk gate into order manager — block orders violating limits, log breaches. Wire Hancock TOTP approval for live orders (paper auto-approves, live requires human TOTP).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Order exceeding 5% NAV is blocked and logged to audit.risk_breaches
- [ ] #2 Circuit breaker triggers on strategy drawdown > 10%
- [ ] #3 Live order generates Hancock TOTP request
- [ ] #4 Paper order auto-approves without TOTP
- [ ] #5 Risk snapshots written to capital.risk_snapshots
<!-- AC:END -->
