---
id: TASK-25
title: 'Watchlist mode — ticker list in config, auto-research on trigger'
status: To Do
assignee: []
created_date: '2026-02-23 17:22'
updated_date: '2026-02-23 17:23'
labels: []
milestone: m-3
dependencies: []
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Add a watchlist configuration that allows Eidos to manage a list of tickers and automatically run scored research on them.

Currently: research is triggered manually with a hardcoded brief in run_research.py.
Target: a config file (watchlist.yaml or watchlist.json) defines tickers and research parameters. Eidos can iterate through the watchlist and run the v2 pipeline for each ticker or ticker group.

Design:
- watchlist.yaml with entries like:
  ```yaml
  watchlists:
    cybersecurity:
      tickers: [PANW, CRWD, ZS, FTNT]
      brief_template: "Analyze {tickers} in cybersecurity sector..."
      schedule: weekly
      multi_perspective: true
      critique_loop: true
    ai_infrastructure:
      tickers: [NVDA, AMD, AVGO]
      brief_template: "Analyze {tickers} in AI infrastructure..."
      schedule: biweekly
  ```
- run_research.py --watchlist cybersecurity runs all tickers in that group
- run_research.py --watchlist-all runs every watchlist
- Each run is recorded in research_comparison.db for cross-run tracking
- Results stored in output/<timestamp>-<watchlist_name>/

Key files:
- New: watchlist.yaml (configuration)
- Modified: run_research.py (--watchlist flag)
- Uses: research_comparison.record_run() for tracking

Depends on: TASK-23 (orchestrator integration).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 watchlist.yaml configuration file with ticker groups and schedules
- [ ] #2 run_research.py --watchlist <name> runs all tickers in that group
- [ ] #3 run_research.py --watchlist-all iterates all watchlists
- [ ] #4 Each run recorded in research_comparison.db
- [ ] #5 Results stored in output/<timestamp>-<watchlist_name>/
- [ ] #6 Brief template supports {tickers} substitution
<!-- AC:END -->
