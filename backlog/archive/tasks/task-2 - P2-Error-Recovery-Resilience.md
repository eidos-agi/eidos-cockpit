---
id: TASK-2
title: 'P2: Error Recovery & Resilience'
status: Done
assignee: []
created_date: '2026-02-23 02:53'
updated_date: '2026-02-23 05:00'
labels:
  - phase-1
  - resilience
  - infrastructure
milestone: m-0
dependencies: []
references:
  - docs/ROADMAP.md
  - llm_backends.py
  - rhea_pod.py
  - orchestrator.py
  - eidd.py
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**A single model timeout shouldn't kill a Pod.** Make Eidos survive LLM failures, tool crashes, and infrastructure hiccups without losing work.

**Core question**: Can Eidos complete tasks even when backends are flaky?

**Key changes**:
- `llm_backends.py` — Add circuit breaker (3 consecutive fails → 60s cooldown → retry)
- `rhea_pod.py` — Wrap each agent turn in try/except; if one model fails, retry with different model
- `config.py` — Add `FALLBACK_BACKEND` configuration
- `eidd.py` — Add `/api/health` endpoint (checks all backends, memory, disk)
- `orchestrator.py` — Add job checkpoint/resume (save state after each phase)

**Circuit breaker design**:
Normal → [3 failures] → Open (fail-fast) → [60s] → Half-Open (one test) → Normal
                                                  → [test fails] → Open

**Graceful degradation chain**: OpenRouter (3 models) → Claude CLI (haiku/sonnet) → Ollama (local)

**Constraint**: NEVER use anthropic SDK directly — always `claude -p` per CLAUDE.md rules.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Pod completes even when 1 of 3 models times out (fallback activates automatically)
- [x] #2 Circuit breaker prevents cascade failures (fails fast after 3 consecutive errors, 60s cooldown)
- [x] #3 /api/health endpoint returns status of all subsystems (backends, memory, disk)
- [x] #4 Jobs can resume from last checkpoint after daemon restart
- [x] #5 Unit tests for circuit breaker state transitions (Normal → Open → Half-Open → Normal)
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
## Completed: P2 Error Recovery & Resilience

### What was built

**Circuit breaker** (`llm_backends.py`):
- `CircuitBreaker` class: Closed → [3 failures] → Open (fail-fast) → [60s cooldown] → Half-Open (test one request) → Closed
- Per-backend breaker registry (`_get_breaker()`)
- `CircuitBreakerOpen` exception for clean upstream handling
- `get_backend_health()` for the health endpoint
- `call_llm()` now accepts optional `backend` parameter for fallback calls

**Pod fallback** (`rhea_pod.py`):
- `_agent_turn()` wraps call_llm in try/except — on failure, calls `_fallback_respond()`
- `_fallback_respond()` retries with `FALLBACK_BACKEND` models (configurable via env var)
- A single model timeout or circuit breaker trip no longer kills the entire Pod

**Fallback config** (`config.py`):
- `EIDOS_FALLBACK_BACKEND` env var (e.g., "ollama" when primary is "claude")
- `get_fallback_models()` returns model configs for the fallback backend
- `get_api_key()` and `get_base_url()` now accept optional `backend` parameter

**Health endpoint** (`eidd.py`):
- `GET /api/health` returns: overall status, uptime, backend circuit breaker states, memory system health, disk free space
- Status is "healthy" or "degraded"

**Job checkpoints** (`orchestrator.py`):
- `_save_checkpoint()` / `_load_checkpoint()` / `_clear_checkpoint()` — JSON files in `checkpoints/`
- Checkpoint saved after verification (the expensive phase), cleared after execution
- `resume_from_checkpoint()` skips to execution using the saved decision
- `list_checkpoints()` for daemon startup scanning
- `eidd.py` start() scans for incomplete checkpoints and re-queues them

**Unit tests** (`test_circuit_breaker.py`):
- 16 tests covering: initial state, threshold behavior, state transitions (Closed→Open→Half-Open→Closed), re-opening, custom thresholds, full lifecycle, get_status

### AC Status
- AC#1 ✅ Pod completes when 1 of 3 models times out (fallback activates)
- AC#2 ✅ Circuit breaker prevents cascade failures (3 errors → 60s cooldown)
- AC#3 ✅ /api/health returns backend, memory, disk status
- AC#4 ✅ Jobs resume from checkpoint after daemon restart
- AC#5 ✅ 16 unit tests for circuit breaker state transitions

### Commit
`5ac524a` — feat: P2 Error Recovery & Resilience
<!-- SECTION:FINAL_SUMMARY:END -->
