---
id: TASK-23
title: Orchestrator integration — route research briefs to v2 pipeline
status: To Do
assignee: []
created_date: '2026-02-23 17:22'
updated_date: '2026-02-23 17:22'
labels: []
milestone: m-3
dependencies: [TASK-21, TASK-22]
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Wire the scored multi-perspective pipeline (investment_scorer + research_perspectives + research_critique) into the Eidos orchestrator so research briefs are automatically routed to the v2 pipeline instead of requiring `python run_research.py` manually.

Currently: a human runs `python run_research.py` from the terminal.
Target: the orchestrator receives a research brief (via dashboard, API, or Pod decision) and runs the full v2 pipeline — gather, condense, score, deliberate, perspectives, write, critique — returning a structured result with scorecard, perspectives, and critique.

Key changes:
- Add a `research_v2` routing mode to orchestrator.py
- The orchestrator should detect research briefs (contains ticker symbols + investment language) and route to the v2 pipeline
- Pass `multi_perspective=True` and `critique_loop=True` by default
- Return the full result including investment_scores, perspective_results, critique_result
- Emit SSE events for each phase so the dashboard can show live progress
- Record the run via research_comparison.record_run() automatically

Depends on: PI-7/PI-8 validation proving the pipeline works end-to-end.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Orchestrator routes research briefs to v2 pipeline automatically
- [ ] #2 multi_perspective and critique_loop enabled by default
- [ ] #3 SSE events emitted for each phase (scoring, perspectives, critique)
- [ ] #4 Run recorded in research_comparison.db after completion
- [ ] #5 Dashboard /api/research endpoint triggers v2 pipeline
- [ ] #6 Works for any ticker, not just PANW/CRWD
<!-- AC:END -->
