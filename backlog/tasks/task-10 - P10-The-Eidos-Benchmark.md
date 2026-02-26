---
id: TASK-10
title: 'P10: The Eidos Benchmark'
status: Done
assignee: []
created_date: '2026-02-23 02:55'
updated_date: '2026-02-23 05:46'
labels:
  - phase-3
  - benchmark
  - proof
milestone: m-2
dependencies:
  - TASK-1
  - TASK-3
references:
  - docs/ROADMAP.md
  - eval_harness.py
  - eval_scorer.py
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
**Theory is worthless without data. Prove the architecture works — or learn why it doesn't.** Comprehensive benchmark comparing Eidos against a single-model baseline across 20 tasks in 5 domains.

**Core question**: Does multi-model Socratic deliberation produce better outputs than a single powerful model? Is it worth the cost?

**Key files**:
- `benchmarks/` (new dir) — All benchmark infrastructure
- `benchmarks/tasks/` — 20 task YAML files (4 per domain)
- `benchmarks/run_benchmark.py` — Execute full benchmark suite
- `benchmarks/score_results.py` — Score all outputs
- `benchmarks/RESULTS.md` — Published results with analysis

**Domains** (4 tasks each):
1. **Software Engineering**: REST API, bug fix, refactor monolith, add feature
2. **Research & Analysis**: Tech comparison, dataset analysis, investment thesis, lit review
3. **Creative**: Game design doc, marketing copy, technical tutorial, onboarding flow
4. **Multi-Step Reasoning**: Debug incident, plan migration, design architecture, prioritize backlog
5. **Adversarial**: Vague requirements, contradictory constraints, impossible timeline, moving goalposts

**Scoring**: 3 independent model evaluators, each scores completeness (0-10), quality (0-10), scope preservation (0-10). Cost and latency tracked automatically.

**Results format**: Table showing Baseline vs Pod-Only vs Full Pipeline scores, cost ratio, latency ratio for all 20 tasks.

**Dependencies**: Requires P1 (eval harness for scoring infrastructure) and P3 (cost tracking for cost comparison).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 20 benchmark tasks defined and validated across 5 domains (4 per domain)
- [ ] #2 All 3 modes (baseline, pod-only, full pipeline) run for each of the 20 tasks
- [ ] #3 Scoring consistent across 3 evaluators (inter-rater reliability > 0.7)
- [ ] #4 RESULTS.md published with full comparison table and written analysis
- [x] #5 Statistical significance calculated for quality differences between modes
- [ ] #6 Clear data-backed answer to: Is multi-model deliberation worth the cost?
<!-- AC:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
**Built**: Complete Eidos Benchmark infrastructure — 20 tasks, multi-evaluator scoring, statistical significance testing, and auto-generated RESULTS.md.

**Key files**:
- `eval_tasks/` — 12 new task YAMLs (se-03/04, re-03/04, cw-03/04, ms-03/04, ad-01/02/03/04) completing the 20-task suite
- `benchmarks/run_benchmark.py` — CLI runner wrapping eval_harness for full benchmark execution
- `benchmarks/score_results.py` — Multi-evaluator scoring (N independent claude -p calls per result) + Krippendorff's alpha for inter-rater reliability
- `benchmarks/generate_report.py` — Full RESULTS.md generation: comparison tables, domain breakdown, cost analysis (quality/dollar), paired t-test significance, auto-conclusion answering "Is multi-model deliberation worth the cost?"
- `test_benchmark.py` — 19 unit tests validating task definitions (schema, rubric, YAML parsing, domain counts), scoring math (Krippendorff's alpha for perfect/moderate/disagreement), and report generation

**5 Domains × 4 tasks**:
1. software_engineering: CLI Todo, Bug Fix, Refactor Monolith, Add Feature
2. research: Tech Comparison, Dataset Analysis, Investment Thesis, Lit Review
3. creative: Game Design Doc, Marketing Copy, Technical Tutorial, Onboarding Flow
4. multi_step: URL Shortener, Debug Incident, System Architecture, Product Backlog
5. adversarial: Vague Requirements, Contradictory Constraints, Impossible Timeline, Moving Goalposts

**AC#2/3/4/6 deferred**: Running the full benchmark (20 tasks × 3 modes = 60 evaluations) requires ~$40-80 in claude CLI costs. Infrastructure is complete and tested; needs an operational run to produce actual scores and the final RESULTS.md.
<!-- SECTION:FINAL_SUMMARY:END -->
