---
id: TASK-19
title: 'PI-5: Multi-Run Comparison & Cache Proof'
status: Done
assignee: []
created_date: '2026-02-23 15:17'
updated_date: '2026-02-23 17:21'
labels:
  - research
  - comparison
  - cache
  - pi-research
milestone: PI-Research
dependencies: []
references:
  - research_comparison.py
  - run_research.py
  - research_cache.py
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Create `research_comparison.py` with SQLite-backed run tracking. Extend `run_research.py` with --runs N mode, --ticker filter, and comparison table display. Proves cache savings increase and duration decreases across runs while score remains stable.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 research_comparison.py created with SQLite research_runs table
- [ ] #2 record_run() persists after each pipeline run
- [ ] #3 compare_runs() shows deltas across runs
- [ ] #4 run_research.py supports --runs N mode
- [ ] #5 Terminal and markdown comparison table formatting
- [ ] #6 Unit tests cover schema, recording, and comparison math
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
Created research_comparison.py (~300 lines) with SQLite-backed multi-run tracking. Created test_research_comparison.py (16 tests, all pass). Features: auto-incrementing run numbers, delta computation between runs, trend analysis (cache_savings_improving, duration_decreasing, score_stable, speedup). Integrated into run_research.py with --runs N flag.
<!-- SECTION:FINAL_SUMMARY:END -->
