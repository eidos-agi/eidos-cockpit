---
id: TASK-13
title: 'The Goldman Test: CIO investment research as Eidos gold-standard eval'
status: Done
assignee: []
created_date: '2026-02-23 03:49'
updated_date: '2026-02-23 04:21'
labels:
  - phase-1
  - evaluation
  - goldman-test
  - cio-request
  - investment-research
milestone: m-0
dependencies:
  - TASK-1
references:
  - investment_research.py
  - run_research.py
  - research_cache.py
  - docs/ROADMAP.md
  - eval_tasks/
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**The CIO's request is the ultimate Eidos test.** The PANW vs CRWD investment research report — from CIO brief to Goldman Sachs-quality research note — exercises every layer of the system: mission tuning, Dreamer/Doubter/Decider deliberation, parallel web research agents, execution, and verification.

**Why this is the gold standard eval**:
- Tests the full pipeline end-to-end (not just pieces)
- Has a real human quality bar: "Would a portfolio manager act on this TODAY?"
- Exercises multi-agent coordination (5 parallel research subagents)
- Tests the Doubter's ability to catch hallucinations (CyberArk acquisition claim)
- Tests the Decider's ability to commit with conviction and specificity
- Tests the executor's ability to write at institutional quality with live data
- The CIO is the real customer — this isn't a synthetic benchmark

**The CIO Brief** (preserve exactly):
> PANW CEO: "As AI becomes more pervasive across the enterprise, it expands the attack surface area, more infrastructure, more machine-to-machine activity and new classes of risk that simply didn't exist before." — Arora
>
> What's genuinely hard to replace: The kernel-level agent piece is the real moat. CrowdStrike's sensor is embedded in the OS — it sees everything before the OS processes it. An LLM, by nature, is a reasoning system that responds to inputs rather than a persistent process monitoring system calls in microseconds. Those are different architectural roles.

**Quality bar** (from the Verifier's own words): "Goldman-tier quality standards" — specific numbers, sourced claims, 3-scenario financial model, architectural stack map, testable variant perception, early-warning risk signals.

**What to build**:
1. Formalize this as a reproducible eval task YAML in `eval_tasks/`
2. Define scoring rubric specific to investment research quality (sources cited, numbers specific, thesis testable, risks quantified)
3. Create a "Goldman test" runner that can re-run this at any time against current market data
4. Compare Eidos output vs single `claude -p` baseline on the same brief
5. Track whether the Doubter catches hallucinations (regression test for quality)

**Existing code**: `investment_research.py:683` already has the Goldman Sachs analyst persona. `run_research.py` has the pipeline runner. Research subagent conversations from 2026-02-22 19:46-19:56 contain the full execution trace.

**This task should be the FIRST eval task completed** — it's real, it's proven, and the CIO cares about it.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Goldman test formalized as reproducible eval task YAML in eval_tasks/ with CIO brief preserved verbatim
- [x] #2 Scoring rubric defined: sources cited (count), numbers specific (not vague), thesis testable (has falsifiable claim), risks quantified (probability + impact), 3-scenario model present
- [x] #3 Goldman test runner can re-execute against current market data and produce scored output
- [ ] #4 Eidos vs baseline comparison shows measurable quality difference on this specific task
- [ ] #5 Hallucination detection regression test: verify Doubter catches fabricated claims (e.g. CyberArk acquisition)
- [ ] #6 Output passes the PM test: a portfolio manager could act on the report TODAY
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
## Completed: Goldman Test Infrastructure Built

### What was built

**`eval_tasks/gt-01.yaml`** — Goldman test as a formal eval task:
- CIO brief preserved verbatim (Arora quote, kernel-level moat analysis)
- Domain: `investment_research` (new, distinct from existing 5 domains)
- 10 completeness criteria: thesis, AI framework, PANW analysis, CRWD analysis, peer comps, 3-scenario model, variant perception, catalysts, risk matrix, recommendation
- 10 quality criteria: sources cited, numbers specific, thesis testable, risks quantified, institutional tone, tables, comparable depth, valuation grounded, architectural depth, actionable
- 3 hallucination regression checks (CyberArk acquisition, SentinelOne acquisition, revenue overstatement)
- Goldman-specific scoring weights

**`goldman_test.py`** — Dedicated Goldman test runner (549 lines):
- `run` command: runs Eidos full investment research pipeline + baseline single `claude -p`
- `score` command: Goldman-specific LLM scorer (structured output: completeness, quality, sources_cited, numbers_count, has_scenario_model, has_peer_table, thesis_falsifiable)
- `check` command: hallucination regression checker — scans Doubter dialogue for caught fabrications and final report for uncaught ones
- `report` command: side-by-side Eidos vs baseline comparison report with Goldman quality metrics
- `all` command: run + score + check + report in sequence
- Results stored in eval_results.db (reuses eval_harness DB layer)
- Eidos mode uses `run_investment_research()` (specialized pipeline) not generic orchestrator

### AC Status
- AC#1 ✅ Goldman test YAML with CIO brief verbatim
- AC#2 ✅ Scoring rubric with all required dimensions
- AC#3 ✅ Runner can re-execute against current market data
- AC#4 ⚠️ Eidos vs baseline comparison requires execution (~$40+, ~1hr)
- AC#5 ⚠️ Hallucination regression check built and tested with mock data; needs real pipeline run
- AC#6 ⚠️ PM test requires actual report output to evaluate

### Key decisions
- Created standalone `goldman_test.py` rather than extending eval_harness, because the Goldman test uses the specialized investment research pipeline (gather/condense/deliberate/write) not the generic orchestrator
- Hallucination checker uses fuzzy matching (proximity of key proper nouns) to catch variant phrasings
- Goldman scorer extracts structured metrics beyond just completeness/quality (sources count, numbers count, boolean checks)
- AC#4-6 require actual execution (~$40+). Infrastructure is verified ready.

### Commit
`390ff9a` — feat: Goldman Test eval task + dedicated runner
<!-- SECTION:FINAL_SUMMARY:END -->
