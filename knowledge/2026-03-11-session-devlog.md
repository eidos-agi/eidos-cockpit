# Devlog — 2026-03-11

## Session 5 (retroactive — auto-closed without /land)

### Pipeline Telemetry (TASK-51)
Added `pipeline_events` table to SQLite (14 columns, 6 indexes). Wired 17 event types across all orchestrator paths. Key files: cost_tracker.py, orchestrator.py, deliberation_classifier.py, crl_runner.py, test_pipeline_telemetry.py. Added `predict_with_confidence()` to classifiers. Created 5 mocked tests. Commits: 855d115, 7b91727.

Bugs found: missing llm_verify_result (mock returned None), missing pod_complete and verify_gate_prediction events, unused variable in _decompose(), crl_runner not passing job_id.

### CRL Experiments (TASK-52)
Ran N=3,5,10,15 benchmarks. Results:
- N=3: A0=93.8%/69s, A3=100%/168s
- N=5: A0=80%/316s, A3=87.5%/175s (Eidos faster AND more accurate)
- N=10: A0=56.3%/796s, A3=73.4%/471s (Eidos faster AND more accurate)
- N=15: A0=13.5%/287s, A3=41.9%/428s (decomposition failure — classifier said NO at 0.58)

### Known gaps
- N=15: complexity classifier needs retraining for large tasks
- Ambiguity classifier backfill not implemented
- Skipped all operational protocols (takeoff, research-first, governance, devlog, bookmark)

### Org-wide commit/push
All 30+ repos committed and pushed. 3 new repos created (elt-forge, tally-money-agent, eidosomni).

---

## Session 6 (current)

### Retroactive protocol catch-up
Filled in everything skipped from session 5: /takeoff, backlog tasks (TASK-51, 52), fleet sync, pre-flight briefing, takeoff.md, cockpit.html, state.json update.

### eidos-capital setup
New repo: github.com/eidos-agi/eidos-capital. AI-driven hedge fund infrastructure.
- Full project scaffold: 13 modules (data, signals, research, portfolio, trading, risk, accounting, audit, legal, infra, api)
- HITL controls with Hancock TOTP gates
- Docs: architecture, risk framework, data sources, research process, data warehouse (bronze/silver/gold), build plan
- Infrastructure research brief: Alpaca wins ($0 commissions, best API), total stack cost $5-25/mo
- Build plan: 6 phases over 9 weeks (Plumbing → First Trade → Get Smart → Risk → Paper Campaign → Go Live)
- Backlog: TASK-53 through TASK-61 + TASK-62

### TASK-62: Found prior quant strategies
Located in ~/repos-aic/investment-research/:
1. **AI PE Compression** — 6-factor model, Spearman r=-0.519, L/S +44pp, R²=0.424
2. **CC ML COGS** — Visa/Mastercard bought risk thesis, ML cost collapse

---

### Lessons
- **Don't skip protocols.** Catching up retroactively takes longer than doing them in the first place.
- **Build mode is seductive.** When momentum is high, operational discipline feels like friction. But the protocols exist because the work isn't useful if it's not findable.
