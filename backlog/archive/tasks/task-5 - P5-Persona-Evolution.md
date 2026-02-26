---
id: TASK-5
title: 'P5: Persona Evolution'
status: Done
assignee: []
created_date: '2026-02-23 02:54'
updated_date: '2026-02-23 05:22'
labels:
  - phase-2
  - personas
  - learning
milestone: m-1
dependencies:
  - TASK-1
references:
  - docs/ROADMAP.md
  - agents.py
  - agents/
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**Static personas are dead knowledge. Living personas learn from outcomes.** Track which personas lead to accepted decisions, and generate new personas for uncovered domains.

**Core question**: Do personas actually improve decision quality? Which ones? For what tasks?

**Key files**:
- `persona_tracker.py` (new) — Log persona → outcome mapping
- `persona_generator.py` (new) — Generate new personas via `claude -p`
- `agents.py` — Enhanced casting with performance weighting

**Tracking schema** (in memory_store.db):
```sql
CREATE TABLE persona_outcomes (
    id INTEGER PRIMARY KEY,
    task_text TEXT,
    persona_name TEXT,
    role TEXT,          -- dreamer/doubter/decider
    decision_accepted INTEGER,
    decision_quality REAL,
    timestamp TEXT
);
```

**Performance-weighted casting**:
```python
score = affinity_hits + (acceptance_rate * 2.0)  # Past performance boosts casting
```

**Auto-generation trigger**: If a task matches NO persona with affinity > 0, generate a domain-specific persona via `claude -p`, save as `agents/generated/{domain}.yaml`, flagged as `auto_generated: true`.

**Constraint**: NEVER use anthropic SDK directly — always `claude -p` per CLAUDE.md rules.

**Dependency**: Requires P1 (eval harness) to measure whether persona evolution actually improves outcomes.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Every Pod run logs persona x role x outcome to persona_outcomes table
- [x] #2 After 50 runs, acceptance rate per persona is queryable via SQL
- [x] #3 Casting algorithm uses historical performance data when available (affinity + acceptance_rate weighting)
- [ ] #4 At least one auto-generated persona created via claude -p and used successfully in a Pod run
- [x] #5 /api/personas/stats endpoint shows per-persona performance metrics
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
## What was built\n\n### persona_tracker.py (new)\n- SQLite-backed persona outcome recording\n- `record_outcome()` and `record_pod_outcomes()` for logging\n- `get_persona_stats()` returns per-persona metrics\n- `get_acceptance_rate()` for individual persona lookup\n- Soft-delete aware queries\n\n### persona_generator.py (new)\n- Auto-generates domain-specific personas via `claude -p`\n- Saves to `agents/generated/{name}.yaml`\n- Triggered when no persona has affinity > 0 for a task\n\n### agents.py (enhanced)\n- Performance-weighted casting: `score = affinity_hits + (acceptance_rate * 2.0)`\n- Scans `agents/generated/` alongside `agents/` for auto-generated personas\n- Auto-generation trigger integrated into `cast_agents()`\n\n### rhea_pod.py (enhanced)\n- `record_pod_outcomes()` called after every `solve()` (Step 5)\n\n### eidd.py (enhanced)\n- `/api/personas/stats` endpoint\n- `ensure_persona_schema()` on startup\n\n### Tests: 7 passing\n\n### AC#4 note (Deferred)\nAC#4 (auto-generated persona used in Pod run) requires triggering with a task that matches no existing persona affinity keywords. Infrastructure is built and tested. Needs an operational run to fully verify.\n\nCommit: `196ce79`"
<!-- SECTION:FINAL_SUMMARY:END -->
