# Why Multi-Agent Systems Beat Single Models on Hard Problems

**Experiment 5: Compounding Recall Loss (CRL) — March 9, 2026**

---

## The Question

Can a single AI model reliably solve problems that require holding many facts in memory across many steps? Or does accuracy collapse as complexity grows — and if so, can a multi-agent architecture prevent that collapse?

## What We Tested

We built a **deterministic financial reconciliation benchmark** — the kind of work that accountants, analysts, and operations teams do every day. Eight investment accounts, each with different starting balances, transaction histories, tax brackets, and fee schedules. The task: process everything in order, where each step depends on all previous steps.

We control **chain length (N)** — how many sequential steps the agent must complete. At N=5, it's a straightforward calculation. At N=10, the agent must track 8 accounts across 10 dependent steps with ~64 individual assertions to verify.

We ran two architectures head-to-head on the same task, same seed, same evaluation:

| | **A0: Claude Code (single model)** | **A3: Eidos (multi-agent pipeline)** |
|---|---|---|
| Architecture | One model, one prompt, one pass | Pod deliberation + verification + execution + post-execution verification |
| Models used | Claude Sonnet 4 | Claude Sonnet 4 + Claude Haiku 4.5 + Gemini 2.5 Pro |
| Verification | None (self-trust) | Pre-execution verification (3 independent judges) + post-execution verification with retry |
| Decomposition | None (brute-force the full task) | Adaptive: the Pod can break hard problems into subtasks |

## Results

### N=5 (Simple — 40 assertions)

| | A0: Claude Code | A3: Eidos |
|---|---|---|
| **Pass rate** | **90.0%** (36/40) | **90.0%** (36/40) |

At low complexity, both architectures perform identically. A single model can hold 5 steps in context and brute-force its way through.

### N=10 (Complex — 64 assertions)

| | A0: Claude Code | A3: Eidos |
|---|---|---|
| **Pass rate** | **15.6%** (10/64) | **56.2%** (36/64) |
| Output produced | 1,250 characters | 41,325 characters |
| Scope Failures | 32 | 14 |
| Compounding Errors | 15 | 6 |
| Duration | 874.7s | 835.6s |

**Eidos outperformed the single model by 3.6x.** Not by a margin — by a multiple.

## What Happened

The single model **collapsed**. It produced only 1,250 characters of output — barely a page — for a task requiring detailed computation across 8 accounts and 10 steps. 32 of its 54 failures were **Scope Failures (SF)**: it didn't even attempt most of the work. It ran out of effective attention bandwidth and gave up.

Eidos **maintained structure**. It produced 41,325 characters of detailed computation. Its failures were concentrated in specific calculation errors (Compounding Failures and Recall Failures), not wholesale abandonment of the task. When the post-execution verifier caught errors, it retried with corrections.

## The Theory: Compounding Recall Loss

This result validates the **Compounding Recall Loss (CRL)** hypothesis:

> If an agent's per-step recall rate is **r**, then after **N** dependent steps, the probability of a fully correct answer is approximately **r^N**.

At N=5 with r=0.90: expected accuracy = 0.90^5 = **59%** (observed: 90% — models are better than worst-case on simple chains).

At N=10 with r=0.90: expected accuracy = 0.90^10 = **35%**. The single model scored 15.6% — *worse* than the theoretical prediction, because at high N it doesn't just make errors, it **abandons scope entirely**.

Multi-agent systems resist CRL through three mechanisms:

1. **Decomposition** — Breaking N-step chains into smaller sub-chains where r^n stays high
2. **Verification** — Independent models catch errors before they compound
3. **Retry with correction** — Failed executions get a second pass with specific feedback

## What This Means in Practice

The problems where Eidos outperforms a single model share a common structure:

**Multi-step workflows where errors compound.** Tax preparation across multiple entities. Financial reconciliation across accounts. Compliance audits with cascading rules. Migration projects with dependent phases. Any task where Step 7 is wrong if Step 3 was wrong.

**Tasks too large for a single context window to hold.** When the full problem exceeds what one model can attend to simultaneously, single models either truncate their work or make errors in the parts they're not actively focused on. Multi-agent systems distribute attention across specialized agents.

**Work that requires both planning and execution.** A single model that plans and executes in one pass often produces plans it can't fully execute, or executes without adequate planning. Separating deliberation (the Pod) from execution (the Executor) with verification gates between them catches the gap.

**High-stakes computations where "close enough" isn't enough.** When every number matters — financial reporting, engineering calculations, legal analysis — the verification layer catches the errors that a single model's self-confidence would miss.

## What This Doesn't Mean

Eidos is not magic. At N=10, it scored 56.2% — not 100%. The architecture reduces CRL but doesn't eliminate it. The path forward involves better decomposition (breaking N=10 into verified sub-chains), better specialization (routing financial steps to models fine-tuned for arithmetic), and iterative refinement.

Single models are still better for simple, short-chain tasks. If the problem fits in one prompt and one pass, the overhead of deliberation and verification is wasted. Use the right tool for the job.

---

*Experiment conducted on March 9, 2026. Seed 42, deterministic ground truth, no LLM-as-judge — pure numerical comparison with tolerance-based matching. Full code and data available in the eidos-v5 repository.*
