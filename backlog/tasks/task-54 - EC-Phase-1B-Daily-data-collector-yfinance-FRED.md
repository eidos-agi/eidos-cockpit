---
id: TASK-54
title: 'EC Phase 1B: Daily data collector (yfinance + FRED)'
status: To Do
assignee: []
created_date: '2026-03-11 07:32'
labels:
  - eidos-capital
  - phase-1
  - data
milestone: m-7
dependencies:
  - TASK-53
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Build src/data/collector.py — yfinance for S&P 500 daily OHLCV, FRED for macro indicators. Railway cron job to run after market close (4:30 PM ET). Write to silver.ohlcv.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 silver.ohlcv has 5000+ rows after first run
- [ ] #2 Collector runs on Railway cron without errors
- [ ] #3 FRED macro data collected for key indicators
<!-- AC:END -->
