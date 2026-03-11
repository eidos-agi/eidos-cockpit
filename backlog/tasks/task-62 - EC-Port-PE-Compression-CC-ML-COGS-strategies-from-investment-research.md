---
id: TASK-62
title: 'EC: Port PE Compression + CC ML COGS strategies from investment-research'
status: To Do
assignee: []
created_date: '2026-03-11 07:36'
updated_date: '2026-03-11 07:43'
labels:
  - eidos-capital
  - phase-3
  - research
  - quant
milestone: m-7
dependencies:
  - TASK-53
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Port the two quant strategies from ~/repos-aic/investment-research/ into eidos-capital:

**1. AI PE Compression Model** (`investment-research/ai-pe-compression/`)
- 6-factor model measuring how AI disruption affects public equity valuations
- Factors: SOR Stickiness, AI Replaceability, AI Disintermediation, AI Capability, AI Infrastructure, Repl×Disint interaction
- LLM-powered scoring pipeline: descriptions via `claude -p` → factor scoring with rubrics → statistical analysis
- Key results: Spearman r=-0.519 (Replaceability→12mo), Quintile L/S spread +44pp, Multi-factor R²=0.424
- Robust stats: winsorization, Spearman, Theil-Sen, bootstrap CI, Mann-Whitney (p=0.007)
- 60/101 QQQ names scored, 13 publication-quality charts, self-contained HTML report
- Scripts: step1_describe → step2_score_all → quick_ml_10 → robust_analysis → visualize → build_report
- Data: company_descriptions.json, ai_risk_scores.json, scoring_log.jsonl, parquet scorecards

**2. CC ML COGS — Credit Card Networks Bought Risk Thesis** (`investment-research/cc-ml-cogs/`)
- Thesis: Visa/Mastercard are risk underwriters, not payment processors. ML collapsed the cost of servicing bought risk while take rates rose with digitization
- Three-layer framework: Risk Underwriting → Merchant Value Exchange → ML Engine (flywheel)
- Net income grew ~470% (V) and ~465% (MA) from 2011-2026 while payment volume grew only ~3-4x
- Contains: thesis.md, methodology.md, scripts (ml_predictor, robust_analysis, analyze_returns, factor_model, analyze_margins, pull_financials)
- Charts, data, report, and research directories

**Source locations:**
- `~/repos-aic/investment-research/ai-pe-compression/` (full pipeline + data + charts + HTML report)
- `~/repos-aic/investment-research/cc-ml-cogs/` (thesis + scripts + data + charts)

**Port plan:**
- Copy scoring pipeline and data into `eidos-capital/src/signals/`
- Adapt to use eidos-capital database for storing scores, descriptions, and backtest results
- Store historical results in research.notes and research.backtests
- Wire into the screener framework as signal sources
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Investment scorer ported to eidos-capital with all 58 tests passing
- [ ] #2 Research pipeline produces scorecards from raw research markdown
- [ ] #3 Historical PANW/CRWD research results stored in research.backtests or research.notes
- [ ] #4 Scorer integrated with eidos-capital signal generation framework
<!-- AC:END -->
