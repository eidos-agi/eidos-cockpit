---
id: TASK-27
title: Scheduled re-analysis — daemon runs watchlist on configurable cadence
status: To Do
assignee: []
created_date: '2026-02-23 17:23'
updated_date: '2026-02-23 17:23'
labels: []
milestone: m-3
dependencies: [TASK-25, TASK-26]
priority: low
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Add scheduled execution of watchlist research so Eidos autonomously re-analyzes tickers without human intervention.

Currently: all research requires a human to run a command.
Target: the Eidos daemon (eidd.py) runs watchlist research on a configurable schedule — weekly, biweekly, or on-demand via API.

Design:
- Add a background scheduler to eidd.py (using threading.Timer or APScheduler)
- Read schedule from watchlist.yaml (each watchlist has a `schedule` field)
- On schedule trigger:
  1. Load the watchlist
  2. Run run_investment_research() for each ticker group
  3. Record run via research_comparison.record_run()
  4. Check for score drift (TASK-26)
  5. If drift detected, emit alert event
  6. Save report to output/<timestamp>-<watchlist>/
- API endpoint: POST /api/research/run-now?watchlist=cybersecurity (manual trigger)
- API endpoint: GET /api/research/schedule (show next scheduled runs)
- Respect cost budget: configurable max_daily_spend_usd prevents runaway costs
- Skip runs if prior run completed within minimum_interval (prevent double-runs)

This is the final piece that makes Eidos a self-running research analyst — it monitors a portfolio on schedule and flags when something changes.

Depends on: TASK-25 (watchlist), TASK-26 (drift detection).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Background scheduler in eidd.py reads schedule from watchlist.yaml
- [ ] #2 POST /api/research/run-now triggers immediate watchlist run
- [ ] #3 GET /api/research/schedule shows next scheduled runs
- [ ] #4 max_daily_spend_usd budget cap prevents runaway costs
- [ ] #5 minimum_interval prevents double-runs
- [ ] #6 Drift alerts emitted when scores change significantly
<!-- AC:END -->
