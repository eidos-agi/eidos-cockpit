---
id: TASK-17
title: 'PI-3: Critique Loop'
status: Done
assignee: []
created_date: '2026-02-23 15:17'
updated_date: '2026-02-23 15:27'
labels:
  - research
  - critique
  - pi-research
milestone: PI-Research
dependencies: []
references:
  - research_critique.py
  - investment_research.py
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Create `research_critique.py` — skeptical PM reviews draft report, identifies unfounded claims, logical gaps, missing data, bias, and stale data. Returns structured critique with PUBLISH/REVISE/REJECT verdict. REVISE triggers targeted re-gather queries. Max 2 critique iterations. Depends on PI-2.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 research_critique.py created with critique_report() and apply_critique_loop()
- [x] #2 Structured issue format: section, claim, problem, fix_query, severity
- [x] #3 extract_regather_queries() converts critique issues to search queries
- [x] #4 Max 2 iterations before forced exit
- [x] #5 PUBLISH verdict exits loop immediately
- [x] #6 Unit tests cover critique parsing and loop termination
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
Created `research_critique.py` (~280 lines) with skeptical PM review loop. Created `test_research_critique.py` (20 tests, all pass). Key features: structured issue format (section/claim/problem/fix_query/severity), PUBLISH/REVISE/REJECT verdicts, extract_regather_queries() for targeted re-gather, max 2 iterations, mock critique mode for testing (mechanical checks: report length, ticker presence, required sections, URL count). format_critique_summary() for markdown output.
<!-- SECTION:FINAL_SUMMARY:END -->
