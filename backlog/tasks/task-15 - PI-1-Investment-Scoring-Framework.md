---
id: TASK-15
title: 'PI-1: Investment Scoring Framework'
status: Done
assignee: []
created_date: '2026-02-23 15:16'
labels:
  - research
  - scoring
  - pi-research
milestone: PI-Research
dependencies: []
references:
  - investment_scorer.py
  - test_investment_scorer.py
  - research_scorer.py
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Create `investment_scorer.py` — a ticker-agnostic quantitative scoring framework (0-100) with 6 weighted dimensions: Financial Strength (25%), Valuation (20%), Competitive Position (20%), Growth Catalysts (15%), Risk Profile (10%), AI/Innovation (10%). Mechanical formulas where possible, LLM via `claude -p` for qualitative dimensions. Includes extract_metrics(), score_investment(), score_comparison(), format_scorecard(). Recommendation mapping: 80-100 STRONG BUY, 65-79 BUY, 45-64 HOLD, 25-44 AVOID, 0-24 STRONG AVOID.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 investment_scorer.py created with 6-dimension scoring framework
- [ ] #2 extract_metrics() parses condensed markdown into numeric metrics
- [ ] #3 Mechanical scoring functions for all quantifiable sub-metrics
- [ ] #4 LLM scoring via claude -p for qualitative dimensions
- [ ] #5 score_investment() produces composite 0-100 with confidence level
- [ ] #6 format_scorecard() and format_comparison_scorecard() render markdown
- [ ] #7 58 unit tests pass in test_investment_scorer.py
- [ ] #8 Dimension weights sum to 1.0, sub-metric weights sum to 1.0
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
Created `investment_scorer.py` (~400 lines) with full quantitative scoring framework. 6 dimensions, 19 sub-metrics, mechanical formulas for 12 of them, LLM rubrics for 7. Parses condensed research markdown via regex to extract financial metrics. 58 tests pass covering metric extraction, each mechanical scorer, composite scoring, comparison, and formatting. Tested against real PANW/CRWD condensed data.
<!-- SECTION:FINAL_SUMMARY:END -->
