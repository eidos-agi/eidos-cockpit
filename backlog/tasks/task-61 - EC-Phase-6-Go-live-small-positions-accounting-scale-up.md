---
id: TASK-61
title: 'EC Phase 6: Go live (small positions, accounting, scale-up)'
status: To Do
assignee: []
created_date: '2026-03-11 07:33'
labels:
  - eidos-capital
  - phase-6
  - live-trading
milestone: m-7
dependencies:
  - TASK-60
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Switch to Alpaca live account with tiny positions ($1-5K per trade). Hancock TOTP required for every live order. Build accounting module: daily NAV calculation (src/accounting/nav.py), tax lot tracking FIFO (src/accounting/tax.py), fee tracking (src/accounting/fees.py). Scale up gradually based on performance milestones.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 First live trade executed with Hancock TOTP approval
- [ ] #2 Daily NAV calculated and stored
- [ ] #3 Tax lots tracked for every position
- [ ] #4 Full audit trail queryable for all live trades
<!-- AC:END -->
