---
id: TASK-58
title: 'EC Phase 3: Research platform (features, backtester, momentum strategy)'
status: To Do
assignee: []
created_date: '2026-03-11 07:33'
labels:
  - eidos-capital
  - phase-3
  - research
milestone: m-7
dependencies:
  - TASK-57
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Build the research loop: feature engine (src/data/features.py) computing returns, volatility, RSI, momentum, MAs from silver.ohlcv into gold.features. Vectorized backtester (src/research/backtester.py) that stores every run in research.backtests. Momentum strategy (12-month, skip last month, monthly rebalance on S&P 500). Screening framework (src/research/screener.py) storing every screen in research.screens.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 gold.features populated nightly from silver.ohlcv
- [ ] #2 Momentum backtest shows Sharpe > 0.5 on S&P 500 (2010-2025)
- [ ] #3 Backtest results stored in research.backtests with equity curve
- [ ] #4 Screener produces ranked symbol list stored in research.screens
<!-- AC:END -->
