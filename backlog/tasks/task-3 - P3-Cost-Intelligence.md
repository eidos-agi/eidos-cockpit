---
id: TASK-3
title: 'P3: Cost Intelligence'
status: Done
assignee: []
created_date: '2026-02-23 02:53'
updated_date: '2026-02-23 05:06'
labels:
  - phase-1
  - cost-tracking
  - observability
milestone: m-0
dependencies: []
references:
  - docs/ROADMAP.md
  - cost_tracker.py
  - orchestrator.py
  - eidd.py
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**Know what you're spending. Optimize for value, not just quality.** Track real dollar costs per job, per model, per component. Enable cost-aware decisions.

**Core question**: What does each job actually cost? Which components are most expensive?

**Key files**:
- `cost_tracker.py` — Token → dollar conversion, per-model pricing table (file already exists, review and extend)
- `orchestrator.py` — Annotate each phase with cost
- `executor.py` — Track execution cost from JSONL
- `eidd.py` — `/api/costs` endpoint, `/api/costs/summary`

**Pricing table** (per 1M tokens):
- anthropic/claude-sonnet-4: input $3.00, output $15.00, cache_read $0.30
- openai/gpt-4o: input $2.50, output $10.00
- google/gemini-2.0-flash-001: input $0.10, output $0.40
- haiku: input $0.25, output $1.25, cache_read $0.03

**Cost breakdown per job example**: Pod deliberation, verification, execution, memory ops, research — all itemized.

**Note**: `cost_tracker.py` and `costs.db` already exist in the repo. Review existing implementation before extending.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Every job has a dollar cost recorded in the attempt log
- [x] #2 /api/costs returns cost history by job with per-component breakdown
- [x] #3 /api/costs/summary returns daily/weekly/monthly totals
- [x] #4 Cost breakdown by component: pod deliberation, verification, execution, memory, research
- [x] #5 Dashboard shows cost per job in the UI
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
## Completed: P3 Cost Intelligence

### What was built

Most of the cost infrastructure already existed from prior work. This task extended it to cover all 5 ACs.

**cost_tracker.py** — Added `get_all_job_costs()`:
- Lists every job with total cost and per-phase breakdown (deliberation, verification, execution, memory, research)
- Sorted by most recent activity

**eidd.py** — New and enhanced endpoints:
- `GET /api/costs` now includes `jobs` list alongside by_model/by_phase totals (AC#2)
- `GET /api/costs/jobs` — dedicated per-job cost listing
- `GET /api/costs/summary?period=daily|weekly|monthly` — period-grouped cost trends (AC#3)
- `GET /api/jobs` now includes `cost_usd` for completed jobs
- `GET /api/tries` now includes `cost_usd` from attempt `cost_summary`

**investment_research.py** — Research phase cost tracking (AC#4):
- Added `_parse_claude_json()` helper for parsing `--output-format json` responses
- Added `_record_research_phase()` to record costs as phase=\"research\"
- Modified all 3 subprocess calls (gather, condense, report_write) to use `--output-format json` and record costs
- Research costs now appear in cost_tracker alongside other phases

**dashboard.html** — Cost display in UI (AC#5):
- Completed jobs show green `$X.XXXX` cost badge
- Tries table has new \"Cost\" column

### AC Status
- AC#1 ✅ Attempt logs include cost_summary (already existed in pod_logger.py:134)
- AC#2 ✅ /api/costs returns per-job cost history with component breakdown
- AC#3 ✅ /api/costs/summary returns daily/weekly/monthly totals
- AC#4 ✅ All 5 phases tracked: deliberation, verification, execution, memory, research
- AC#5 ✅ Dashboard shows cost per job

### Key decisions
- Existing cost_tracker.py was already very comprehensive — extended rather than rebuilt
- Memory phase was already tracked (memory.py:266) as $0.00 (Ollama is free)
- Research pipeline uses `--output-format json` for cost extraction while preserving text output via JSON parsing
- Research costs recorded with phase=\"research\" and event_type=\"research_{phase_name}\"

### Commit
`54ee548` — feat: P3 Cost Intelligence
<!-- SECTION:FINAL_SUMMARY:END -->
