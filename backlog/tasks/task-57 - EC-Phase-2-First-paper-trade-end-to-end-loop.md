---
id: TASK-57
title: 'EC Phase 2: First paper trade (end-to-end loop)'
status: To Do
assignee: []
created_date: '2026-03-11 07:33'
labels:
  - eidos-capital
  - phase-2
  - trading
milestone: m-7
dependencies:
  - TASK-54
  - TASK-55
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Build order manager (src/trading/order_manager.py), position tracker (src/portfolio/tracker.py), and a dumb buy-and-hold SPY strategy (src/signals/buy_and_hold.py). Prove the full loop: signal → risk check → paper order → fill → position → P&L. Log everything to capital.orders, capital.positions, audit.events.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Paper trade for SPY appears in capital.orders
- [ ] #2 Position tracked in capital.positions with P&L
- [ ] #3 Audit event logged for the trade
- [ ] #4 Order manager enforces basic position size check
<!-- AC:END -->
