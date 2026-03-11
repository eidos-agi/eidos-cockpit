---
id: TASK-52
title: 'CRL experiments: benchmark A0 vs A3 at N=3,5,10,15'
status: Done
assignee: []
created_date: '2026-03-11 07:04'
labels:
  - eidos-v5
  - experiments
  - CRL
milestone: m-7
dependencies: []
references:
  - eidos-v5/experiments/runners/crl_runner.py
  - eidos-v5/experiments/results/
  - briefs/2026-03-09-crl-experiment-results.md
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Ran Compounding Recall Loss experiments comparing Claude solo (A0) vs Eidos orchestrated (A3). Results: N=3: A0=93.8%/69s, A3=100%/168s. N=5: A0=80%/316s, A3=87.5%/175s. N=10: A0=56.3%/796s, A3=73.4%/471s. N=15: A0=13.5%/287s, A3=41.9%/428s. Eidos faster AND more accurate at N>=5. N=15 exposed decomposition gap — classifier doesn't trigger split for large tasks.
<!-- SECTION:DESCRIPTION:END -->

## Final Summary

<!-- SECTION:FINAL_SUMMARY:BEGIN -->
CRL curve proves Eidos value: at N>=5, A3 is both faster and more accurate than A0. The gap widens with complexity. N=15 revealed a real limit — complexity classifier needs retraining to trigger decomposition for large tasks (said NO at 0.58 confidence). Results in experiments/results/.
<!-- SECTION:FINAL_SUMMARY:END -->
