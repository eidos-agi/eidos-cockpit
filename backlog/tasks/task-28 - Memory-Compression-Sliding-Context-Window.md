---
id: TASK-28
title: Memory Compression & Sliding Context Window
status: To Do
assignee: []
created_date: '2026-02-26 04:52'
updated_date: '2026-02-26 04:56'
labels:
  - memory
  - architecture
  - infrastructure
dependencies: []
references:
  - memory.py
  - memory_db.py
  - distillation.py
  - investment_research.py
  - test_memory.py
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
## Context

Eidos already has significant memory infrastructure — a PEFM memory system (`memory.py`) with 5 memory types, SQLite + FTS5 + vector search, a distillation pipeline that auto-consolidates episodic → semantic, and session tracking. What's **missing** is active context management: a sliding window that curates what goes into an LLM call, and a backend abstraction layer so storage can be swapped.

## Research Findings (Feb 2026)

### What Eidos Has (Tier 3 — Archival)
- `memory.py` (691 lines): PEFM types (Episodic, Semantic, Procedural, Emotional, Working)
- `memory_db.py` (523 lines): SQLite + FTS5 BM25 + sqlite-vec 1024-dim cosine KNN
- `distillation.py`: Auto-consolidation every 20 `remember()` calls, 7-day episodic decay, clusters → semantic via `claude -p`
- `investment_research.py`: `_phase_condense()` compresses 75KB → ~4KB structured facts
- Bookmarks/heartbeats for session state recovery

### What's Missing (Tiers 1 & 2 — Active Context + Warm Compression)
1. **ContextBuilder** — no system to assemble optimal context from multiple memory sources with token budgets
2. **Summary Buffer** — no `[Summary of old] + [Recent verbatim]` pattern with token-based thresholds
3. **Tool result clearing** — no mechanism to evict processed tool outputs (biggest token consumers)
4. **Backend abstraction** — `memory_db.py` is hardcoded to SQLite; no pluggable interface
5. **Self-managed memory tools** — pods can't manage their own context (MemGPT pattern)

### State of the Art
- **Summary Buffer** (LangChain, LlamaIndex, Claude Code): Most battle-tested pattern — `[System Prompt] + [Running Summary] + [Recent Window]`
- **MemGPT/Letta**: LLM pages its own memory via function calls; three tiers (main/recall/archival); recursive summarization on eviction
- **Claude Code**: Selective tool result clearing (84% token reduction) + auto-compaction at 95% capacity
- **Critical insight**: Context rot — more tokens ≠ better performance. Curation > capacity.

### Common Interface Pattern
```
Memory.add(messages, user_id, metadata)
Memory.search(query, user_id, limit)
Memory.get(memory_id)
Memory.update(memory_id, new_content)
Memory.delete(memory_id)
ContextBuilder.build(system_prompt, memories, recent_messages, token_budget)
```

## Goal

Build three things:
1. **ContextBuilder** — assembles optimal LLM context from PEFM memories + session state + tool results, within a configurable token budget
2. **Summary Buffer** — hybrid recent-window + running-summary with token-based thresholds
3. **Backend Abstraction** — extract a `MemoryBackend` protocol from `memory_db.py` so storage is pluggable

## Design Principles
- Build on existing PEFM system — don't replace it, layer on top
- Token budgets, not message counts
- Tool result clearing as first line of defense (cheapest, biggest impact)
- Self-managed memory tools for pods (MemGPT pattern fits `claude -p` naturally)
- Backend-agnostic interface for future migration to Supabase/vector DB
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 ContextBuilder assembles context from PEFM memories + session state within configurable token budget
- [ ] #2 Summary Buffer implements [Running Summary] + [Recent Window] with token-based eviction thresholds
- [ ] #3 Tool result clearing evicts stale tool outputs before triggering expensive summarization
- [ ] #4 MemoryBackend protocol extracted from memory_db.py with at least SQLite implementation
- [ ] #5 Pods can search/save/load their own context via self-managed memory tools
- [ ] #6 Integration tests verify context stays within token budget under load
- [ ] #7 No direct SQLite imports outside the backend implementation
<!-- AC:END -->

## Implementation Notes

<!-- SECTION:NOTES:BEGIN -->
## Research Sources (Feb 2026)\n- MemGPT paper (arXiv:2310.08560) + Letta docs\n- Claude Code context editing & compaction (platform.claude.com/docs)\n- LangChain ConversationSummaryBufferMemory\n- LlamaIndex ChatSummaryMemoryBuffer + MemoryBlock\n- Mem0 architecture (arXiv:2504.19413)\n- Zep graph memory\n- MemOS (arXiv:2507.03724)\n\n## Key Decision: Build On Existing PEFM\nDon't rebuild from scratch. The PEFM system is Tier 3 (archival). Layer a Tier 1 (ContextBuilder + sliding window) and Tier 2 (Summary Buffer) on top.\n\n## NOT in scope (future work)\n- Graph memory (Mem0/Zep style entity-relationship tracking)\n- Full MemGPT paging with heartbeats (overkill for current pod architecture)\n- Cross-agent memory sharing (multi-pod coordination is TASK-9)
<!-- SECTION:NOTES:END -->
