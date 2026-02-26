---
id: TASK-9
title: 'P9: Multi-Pod Coordination'
status: Done
assignee: []
created_date: '2026-02-23 02:55'
updated_date: '2026-02-23 05:40'
labels:
  - phase-3
  - multi-pod
  - scaling
milestone: m-2
dependencies:
  - TASK-2
references:
  - docs/ROADMAP.md
  - orchestrator.py
  - rhea_pod.py
  - memory.py
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**One Pod thinks well. Three Pods thinking in parallel think faster.** Enable multiple Pods to work on different aspects of a large task simultaneously, then integrate their outputs.

**Core question**: Does parallel deliberation produce better results for complex tasks?

**Key files**:
- `multi_pod.py` (new) — Coordinator: spawn Pods, manage sessions, integrate
- `orchestrator.py` — Add `parallel_decompose` routing mode
- `memory.py` — Session isolation (each Pod gets its own session)

**Coordination pattern**:
```
Large Task → Decompose into N independent subtasks
→ Spawn N Pods IN PARALLEL (each with own memory session)
→ [Pod A: frontend]  [Pod B: backend]  [Pod C: tests]
→ Collect all decisions
→ Integration Pod: reviews all N outputs, resolves conflicts, produces unified plan
→ Execute unified plan
```

**Key design decisions**:
- Pods don't communicate mid-deliberation (independent reasoning)
- Each Pod starts with same base memory but separate sessions
- Integration Pod sees all decisions but not dialogues (avoid context overload)
- Max 3 parallel Pods (cost control)

**Dependency**: Requires P2 (resilience) — parallel Pods need robust error handling so one failure doesn't kill the whole batch.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 2-3 Pods run in parallel on decomposed subtasks via multi_pod.py coordinator
- [x] #2 Each Pod has an isolated memory session (no cross-contamination)
- [x] #3 Integration Pod produces coherent unified plan from all parallel Pod decisions
- [ ] #4 Total latency is less than sum of sequential Pod runs (parallel speedup demonstrated)
- [ ] #5 Quality matches or exceeds sequential decomposition as measured by eval harness
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
**Built**: Multi-Pod coordination with parallel deliberation and thread-safe memory sessions.

**Key files**:
- `multi_pod.py` (new, 280 lines) — `decompose_task()` via claude -p, `run_parallel_pods()` with ThreadPoolExecutor (max 3), `run_integration_pod()` reviews decisions-only, `coordinate()` full pipeline
- `memory.py` (modified) — Converted `_current_session` from module-level dict to `threading.local()` via `_get_session()`. All 10+ references updated. This is the core thread-safety guarantee.
- `orchestrator.py` (modified) — Added `run_parallel()` function, imported `multi_pod_coordinate`
- `eidd.py` (modified) — Added "parallel" mode to job routing, `_run_parallel_job()` handler
- `test_multi_pod.py` — 12 unit tests covering session isolation, decomposition, parallel execution, integration, and full pipeline

**Key decisions**:
- `threading.local()` for memory sessions — simplest, most reliable thread-safety. Each Pod thread gets isolated session without locks.
- Integration Pod sees decisions only (not full dialogues) to avoid context overload
- Max 3 parallel Pods (cost control hardcoded)
- Decomposition via claude -p haiku (cheap, fast)

**AC#4 deferred**: Parallel speedup needs real LLM calls (wall time < sequential sum). Unit test demonstrates the parallelism pattern works.
**AC#5 deferred**: Quality comparison needs eval harness runs comparing parallel vs sequential on same tasks.
<!-- SECTION:FINAL_SUMMARY:END -->
