---
id: TASK-51
title: 'Pipeline telemetry: record every orchestrator decision to SQLite'
status: Done
assignee: []
created_date: '2026-03-11 07:04'
labels:
  - eidos-v5
  - telemetry
  - observability
milestone: m-7
dependencies: []
references:
  - eidos-v5/cost_tracker.py
  - eidos-v5/orchestrator.py
  - eidos-v5/deliberation_classifier.py
  - eidos-v5/test_pipeline_telemetry.py
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Added `pipeline_events` table to costs.db with 14 columns + 6 indexes. Wired 17 event types across all orchestrator paths: classifier predictions (with confidence), DAG structure, verification verdicts, execution timing, COMPRESS mode, learning feedback, and classifier backfill. Added `predict_with_confidence()` to deliberation_classifier.py. Created test_pipeline_telemetry.py with 5 mocked tests covering all paths.
<!-- SECTION:DESCRIPTION:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
Implemented comprehensive pipeline telemetry. Every decision point in the Eidos orchestrator now records a row in SQLite — classifier predictions with confidence, verification verdicts, timing, DAG structure, success/failure learning, and classifier feedback loops. 17 event types across 9 stages. Files modified: cost_tracker.py, orchestrator.py, deliberation_classifier.py, crl_runner.py. New file: test_pipeline_telemetry.py. Committed as 855d115 + 7b91727.
<!-- SECTION:FINAL_SUMMARY:END -->
