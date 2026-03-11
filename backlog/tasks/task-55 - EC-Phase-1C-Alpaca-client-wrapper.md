---
id: TASK-55
title: 'EC Phase 1C: Alpaca client wrapper'
status: To Do
assignee: []
created_date: '2026-03-11 07:32'
labels:
  - eidos-capital
  - phase-1
  - trading
milestone: m-7
dependencies:
  - TASK-53
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Build src/trading/alpaca_client.py — thin wrapper around alpaca-py SDK. Core methods: submit_order(), get_positions(), get_account(), get_order_status(). Paper mode by default, env var to switch to live.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Can submit a paper order via the wrapper
- [ ] #2 Can query positions and account balance
- [ ] #3 Paper/live mode controlled by env var
<!-- AC:END -->
