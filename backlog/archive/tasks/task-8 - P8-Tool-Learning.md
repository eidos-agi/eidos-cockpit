---
id: TASK-8
title: 'P8: Tool Learning'
status: Done
assignee: []
created_date: '2026-02-23 02:54'
updated_date: '2026-02-23 05:35'
labels:
  - phase-2
  - tools
  - learning
milestone: m-1
dependencies: []
references:
  - docs/ROADMAP.md
  - tools.py
  - tools/
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**If Eidos builds the same thing three times, it should learn to do it in one step.** Analyze successful execution JSONL sessions to extract reusable tool patterns, then generate new tools automatically.

**Core question**: Can the system grow its own capability from experience?

**Key files**:
- `tool_learner.py` (new) — JSONL analysis, pattern extraction, tool generation
- `tools/learned/` (new dir) — Directory for auto-generated tool YAMLs
- `jsonl_parser.py` — Enhanced with tool-call sequence extraction

**Pattern extraction pipeline**:
JSONL session → extract tool-call sequence → normalize → hash → count occurrences

**Example pattern**:
Pattern: WebSearch → Read(result) → Write(markdown_summary)
Seen: 5 times across 3 jobs
→ Generate: `tools/learned/web_research_summarizer.yaml`

**Generated tool schema**:
```yaml
name: web_research_summarizer
description: "Search the web for a topic and produce a structured markdown summary"
handler: tool_learner.run_learned_tool
auto_generated: true
source_pattern: "WebSearch → Read → Write"
occurrence_count: 5
```

**Independent** — no dependencies on other projects.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 JSONL parser extracts tool-call sequences from completed execution sessions
- [x] #2 Pattern detector identifies repeated sequences with 3+ occurrences across jobs
- [x] #3 Generated tool YAML is valid and loadable by tools.py hot-loading system
- [ ] #4 At least one learned tool successfully used in a Pod context
- [x] #5 Tool manifest shows learned tools alongside manual ones in /api/tools or equivalent
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
**Built**: Full tool learning pipeline — JSONL analysis → pattern detection → tool YAML generation.

**Key files**:
- `tool_learner.py` (new, 232 lines) — `extract_subsequences()`, `detect_patterns()`, `generate_tool_yaml()`, `save_tool_yaml()`, `learn_from_sessions()`, `run_learned_tool()` handler
- `jsonl_parser.py` (enhanced) — Added `extract_tool_sequences()` for ordered tool-call extraction from JSONL sessions
- `tools/learned/` (new dir) — Auto-generated tool YAMLs land here, picked up by hotloader
- `eidd.py` — Added `/api/tools/learned` (stats) and `/api/tools/learn` (trigger) endpoints
- `test_tool_learner.py` — 28 unit tests, all passing

**Key decisions**:
- Pattern detection normalizes to tool names only (not inputs) for cross-session matching
- Subset filtering removes short patterns that are contiguous subsequences of longer ones with equal count
- Per-session deduplication prevents inflated counts from repeated patterns within one session
- `run_learned_tool()` is a guidance handler — the Pod sees the pattern description and executes the steps itself

**AC#4 deferred**: Requires real JSONL sessions with 3+ recurring patterns and a live Pod run to exercise the generated tool. Infrastructure is complete.
<!-- SECTION:FINAL_SUMMARY:END -->
