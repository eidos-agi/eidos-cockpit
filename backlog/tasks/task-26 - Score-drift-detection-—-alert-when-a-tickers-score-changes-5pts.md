---
id: TASK-26
title: Score drift detection — alert when a ticker's score changes >5pts
status: To Do
assignee: []
created_date: '2026-02-23 17:23'
updated_date: '2026-02-23 17:23'
labels: []
milestone: m-3
dependencies: []
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Add automatic detection of significant score changes across research runs for the same tickers.

Currently: research_comparison.py tracks scores across runs but doesn't flag when scores change significantly.
Target: after each run, compare the new investment score against the prior run for the same tickers. If any ticker's composite score changes by more than 5 points, flag it as a drift event.

Design:
- Add detect_score_drift() to research_comparison.py
  - Compares latest run's investment_score to the previous run for the same ticker group
  - Returns list of drift events: {ticker, old_score, new_score, delta, direction}
  - Threshold configurable (default: 5 points)
- Drift events emitted as SSE events (research:score_drift)
- Drift events logged to a drift_events table in the comparison DB
- run_research.py prints drift warnings after each run
- Optional: send drift alerts via webhook or notification

Drift causes to track:
- New data available (earnings release, guidance update)
- Cache staleness (VOLATILE data refreshed)
- LLM scoring variance (qualitative metrics shifted)

This is the feedback loop that makes re-running research valuable — you know WHEN something changed and by how much.

Depends on: TASK-25 (watchlist mode).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 detect_score_drift() compares latest vs prior run per ticker
- [ ] #2 Drift threshold configurable (default 5 points)
- [ ] #3 Drift events logged to drift_events table in comparison DB
- [ ] #4 run_research.py prints drift warnings after each run
- [ ] #5 SSE event emitted for score drift (research:score_drift)
<!-- AC:END -->
