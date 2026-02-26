---
id: TASK-18
title: 'PI-4: Report v2 Format (Scorecard + Exec Summary)'
status: Done
assignee: []
created_date: '2026-02-23 15:17'
updated_date: '2026-02-23 15:30'
labels:
  - research
  - report
  - pi-research
milestone: PI-Research
dependencies: []
references:
  - investment_research.py
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Update report generation to produce scorecard-first format: Page 1 = investment scorecard table (6 dimensions, composite), Page 2 = multi-perspective consensus table, Page 3 = executive summary (What/Why/Risk in 3 bullets), then deep-dive sections. Depends on PI-1, PI-2, PI-3.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Report opens with scorecard table
- [x] #2 Multi-perspective consensus table on page 2
- [x] #3 Executive summary is 3 bullets max
- [x] #4 Deep-dive sections follow after summary
- [x] #5 Methodology appendix at end
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
Integrated investment_scorer, research_perspectives, and research_critique into the investment_research.py pipeline. Added _phase_write_v2() that generates scorecard-first reports (pre-computed scorecard + perspective tables, then LLM narrative with exec summary). Updated run_research.py with --runs N, --no-perspectives, --no-critique flags and multi-run comparison display. Report v2 format: scorecard table → perspective consensus → 3-bullet exec summary → deep dives → methodology. All 108 tests pass across 4 test suites.
<!-- SECTION:FINAL_SUMMARY:END -->
