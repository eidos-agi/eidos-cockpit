---
id: TASK-14
title: VCR Run Recorder — full Eidos replay system
status: Done
assignee: []
created_date: '2026-02-23 06:25'
updated_date: '2026-02-23 06:25'
labels:
  - observability
  - infrastructure
  - vcr
dependencies: []
references:
  - run_recorder.py
  - test_run_recorder.py
  - jobs.py
  - eidd.py
  - rhea_pod.py
  - orchestrator.py
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Every Eidos run should be replayable like a VCR tape. The system already emitted ~30 event types through on_event callbacks but they were in-memory only (lost on process exit). Built a persistence layer that intercepts job.emit() and writes enriched JSONL with the 5 W's (who/what/when/where/why) per event.

**What was built:**
- `run_recorder.py`: RunRecorder class, 5W enrichment, terminal replay CLI, list/replay commands, API helpers
- `recordings/` directory for JSONL files (one per job run)
- `/api/recordings` and `/api/recordings/<run_id>` endpoints
- 4 new event emissions: pod:memory_recalled, agent:prompt, agent:fallback, verify:result
- 21 tests

**Surgical modifications (~40 lines across 4 files):**
- jobs.py: _recorder field + write-through in emit()
- eidd.py: start/stop recording per job + API endpoints
- rhea_pod.py: memory recall, prompt tracking, fallback emissions
- orchestrator.py: verify:result with ruling and vote counts
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Every job run writes a JSONL recording to recordings/
- [x] #2 Each event enriched with 5W: who/what/when/where/why
- [x] #3 Terminal replay via `python run_recorder.py replay <run_id>`
- [x] #4 API endpoints for listing and retrieving recordings
- [x] #5 Pod memory recall, prompts, fallbacks, and verification results captured
- [x] #6 21 tests passing
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
## Completed: VCR Run Recorder

Built and tested in one session. Live-tested with a real Pod deliberation (job 63a9da60 — temperature converter task). Recording captured 18 events over 98s: memory recall → Dreamer/Doubter/Decider dialogue → verification (3/3 ACCEPT) → job done. Full replay works in terminal and via API.

### Key design decision
Instead of building a new capture system, intercepted the existing `job.emit()` chokepoint. Every event that was already flowing through the system now also writes to disk. Total new code: 280 lines. Total modifications: 40 lines across 4 files.

### Commit
`3bad2e0` — feat: VCR run recorder — replay every Eidos run like a tape
<!-- SECTION:FINAL_SUMMARY:END -->
