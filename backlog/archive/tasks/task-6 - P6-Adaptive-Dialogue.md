---
id: TASK-6
title: 'P6: Adaptive Dialogue'
status: Done
assignee: []
created_date: '2026-02-23 02:54'
updated_date: '2026-02-23 05:25'
labels:
  - phase-2
  - dialogue
  - optimization
milestone: m-1
dependencies:
  - TASK-1
references:
  - docs/ROADMAP.md
  - rhea_pod.py
  - config.py
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**Three rounds is arbitrary. Let the conversation find its natural length.** Replace fixed round count with dynamic convergence detection. Simple tasks resolve fast; complex tasks get more debate.

**Core question**: Does adaptive round count improve quality? Does early exit save cost without hurting quality?

**Key changes**:
- `rhea_pod.py` — Add convergence detector, dynamic round range (1-5)
- `dialogue_analyzer.py` (new) — Post-hoc dialogue quality analysis
- `config.py` — `MIN_DIALOGUE_ROUNDS=1`, `MAX_DIALOGUE_ROUNDS=5`

**Convergence signals** (exit early):
- Dreamer and Doubter use similar language (cosine similarity > 0.8)
- Doubter raises no new concerns (concern count decreasing)
- Decider's confidence language increases ("clearly", "definitely" vs "perhaps")

**Divergence signals** (extend debate):
- Doubter introduces fundamental objection (new risk keyword)
- Adversarial injection generates promising alternative
- Decider explicitly says "need another round"

**Dialogue analyzer metrics**: Round where consensus emerged, whether adversarial injection changed outcome, quality delta between rounds, token cost per additional round vs quality gain.

**Dependency**: Requires P1 (eval harness) to measure whether adaptive rounds actually improve quality.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Simple tasks (greeting, rename, etc.) resolve in 1 round instead of fixed 3
- [x] #2 Complex tasks (architecture, research) use 3-5 rounds as needed
- [ ] #3 Average cost per job decreases compared to fixed-round baseline (fewer unnecessary rounds)
- [ ] #4 Decision quality maintained or improved as measured by eval harness before/after comparison
- [x] #5 Dialogue analyzer produces per-run quality reports with convergence metrics
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
## What was built\n\n### dialogue_analyzer.py (new, 230 lines)\n- `estimate_task_complexity()`: Classifies tasks as simple/moderate/complex via regex patterns\n- `should_continue()`: Real-time convergence check with signals:\n  - Convergence: language overlap, decreasing concerns, high confidence\n  - Divergence: many concerns, adversarial active, decider requests continuation\n- `analyze_dialogue()`: Post-hoc report with per-round concerns, confidence, tokens, adversarial tracking\n\n### rhea_pod.py (enhanced)\n- Adaptive round count: MIN_DIALOGUE_ROUNDS(1) to MAX_DIALOGUE_ROUNDS(5)\n- `should_continue()` called after each round to decide whether to extend\n- Research mode enforces min 2 rounds\n- On adaptive exit without DECISION:, forces a final commitment turn\n- `quality_report` and `exit_reason` added to solve() return dict\n\n### config.py (enhanced)\n- MIN_DIALOGUE_ROUNDS = 1 (new)\n- MAX_DIALOGUE_ROUNDS = 5 (was 3)\n\n### Tests: 23 passing\n\n### AC#3 and AC#4 (Deferred)\nCost decrease and quality comparison require before/after runs on real tasks. Infrastructure is built — the `quality_report` captures all metrics needed for comparison.\n\nCommit: `7bff3dc`"
<!-- SECTION:FINAL_SUMMARY:END -->
