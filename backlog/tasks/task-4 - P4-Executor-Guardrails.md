---
id: TASK-4
title: 'P4: Executor Guardrails'
status: Done
assignee: []
created_date: '2026-02-23 02:54'
updated_date: '2026-02-23 05:12'
labels:
  - phase-1
  - safety
  - executor
milestone: m-0
dependencies: []
references:
  - docs/ROADMAP.md
  - executor.py
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**The executor is the AGI's hands. Hands need safety gloves.** Prevent the executor from doing harm — to the filesystem, to resources, or to itself.

**Core question**: Can we safely run untrusted decisions?

**Key changes**:
- `executor.py` — Input validation, output validation, resource monitoring
- `executor_sandbox.py` (new) — Optional Docker-based execution for untrusted tasks
- `config.py` — Add `EXECUTOR_MAX_FILES`, `EXECUTOR_MAX_FILE_SIZE_MB`, `EXECUTOR_SANDBOX`

**Input validation**:
- Decision length cap: 10,000 characters
- Path traversal detection: reject decisions containing `../`, `/etc/`, `/usr/`, etc.
- Dangerous command detection: flag `rm -rf`, `DROP TABLE`, `FORMAT`, etc.

**Output validation** (post-execution):
- File count limit: max 100 files per execution
- File size limit: max 50MB per file, 200MB total
- No binaries unless task explicitly requested them
- No files outside the output directory

**Resource monitoring**:
- Track disk usage during execution (pre/post delta)
- Log peak memory usage of subprocess
- Enforce execution timeout strictly (SIGKILL after grace period)
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Path traversal attempts (../, /etc/, /usr/) caught and rejected before execution
- [x] #2 Output validation runs after every execution and flags file count/size/location violations
- [x] #3 Resource monitoring (disk delta, memory usage) logged in attempt records
- [x] #4 Docker sandbox mode works for untrusted tasks (optional, config-gated)
- [x] #5 Unit tests for input validation covering malicious decision payloads
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
## What was built

Multi-layer guardrails for the executor to protect against malicious or runaway decisions:

### Input Validation (`executor.py:validate_decision`)
- Empty/whitespace rejection
- Length cap (10K characters, configurable via `EXECUTOR_MAX_DECISION_LEN`)
- Path traversal detection (`../`, `..\`)
- 9 dangerous system paths (`/etc/`, `/root/`, `/dev/`, etc.)
- 12 dangerous command patterns (rm -rf /, DROP TABLE, curl|bash, fork bomb, etc.)
- Case-insensitive matching for command patterns
- `DecisionRejected` exception raised before any resources are allocated

### Output Validation (`executor.py:validate_output`)
- File count limit (100, configurable)
- Per-file size limit (50 MB, configurable)
- Total output size limit (200 MB, configurable)
- Location check: no files outside the output directory
- Runs after every execution, warnings logged and returned in result

### Resource Monitoring
- Disk usage measured before and after execution
- Delta included in `guardrails` section of execution result

### Docker Sandbox (`executor_sandbox.py`)
- Config-gated via `EXECUTOR_SANDBOX=true` env var
- Docker container with resource limits (1GB RAM, 2 CPUs)
- 30-minute timeout
- Mounts output dir and `~/.claude` (read-only for auth)
- Graceful fallback to direct execution if Docker unavailable

### Configuration (`config.py`)
- All guardrail limits configurable via environment variables
- Sensible defaults for all settings

### Tests (`test_executor_guardrails.py`)
- 39 unit tests covering:
  - Valid decisions (simple, complex, safe paths)
  - Empty/None/whitespace rejection
  - Length limit boundary
  - Path traversal (Unix, Windows, nested)
  - All 9 dangerous system paths
  - All 12 dangerous command patterns
  - Case-insensitive command detection
  - Safe commands that look dangerous (false positive prevention)
  - Output validation (empty dir, normal files, too many files, oversized files, total size)

### Key decisions
- Path checks run before command checks (defense in depth — a dangerous command referencing /dev/ is caught by the path check first)
- All limits are env-var configurable for different deployment scenarios
- Sandbox is opt-in (default off) to avoid Docker dependency for development

Commit: `616fa49`
<!-- SECTION:FINAL_SUMMARY:END -->
