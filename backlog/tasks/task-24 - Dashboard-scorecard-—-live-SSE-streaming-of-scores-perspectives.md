---
id: TASK-24
title: Dashboard scorecard — live SSE streaming of scores + perspectives
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
Add a research scorecard view to the Flask dashboard (templates/dashboard.html) that shows investment scores and multi-perspective analysis in real-time via SSE.

Currently: the dashboard shows Pod deliberation events but knows nothing about investment scores, perspectives, or critique results.
Target: when a research job runs, the dashboard streams live updates showing:
- Phase progress (gathering → condensing → scoring → deliberating → perspectives → writing → critiquing)
- Investment scorecard table (6 dimensions per ticker, composite score, recommendation)
- Multi-perspective consensus table (Growth/Value/Contrarian scores + agreement level)
- Critique verdict (PUBLISH/REVISE/REJECT with issue count)
- Key debates from meta-synthesis
- Run comparison table (if multiple runs exist for the same tickers)

Key changes:
- Add /api/research/scorecard endpoint that returns latest scorecard data
- Add SSE event types for research:ticker_scored, research:perspectives, research:critique
- New dashboard panel or tab for research results
- Use format_scorecard(), format_perspective_table(), format_critique_summary() for rendering
- Show confidence indicators (LOW CONFIDENCE warning when data sparse)

Depends on: TASK-23 (orchestrator integration).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Dashboard shows live phase progress during research runs via SSE
- [ ] #2 Investment scorecard table renders with 6 dimensions per ticker
- [ ] #3 Multi-perspective consensus table shows Growth/Value/Contrarian + agreement level
- [ ] #4 Critique verdict displayed with issue count
- [ ] #5 Run comparison table shown when multiple runs exist
- [ ] #6 LOW CONFIDENCE warning displayed when data is sparse
<!-- AC:END -->
