# CRL Resistance Architecture — How Eidos Beats Context Degradation

**Date**: March 9, 2026
**Status**: Design complete, implementation in progress
**Related**: THE-LOOP.md, CRL experiment results brief

---

## The Problem

All current AI agents — Claude Code, Cursor, Copilot — suffer from **Compounding Recall Loss (CRL)**. If an agent's per-step recall rate is `r`, then after `N` dependent steps, the probability of a fully correct answer is approximately `r^N`.

At N=10 with r=0.90: accuracy drops to **35%** theoretical, **15.6%** observed (Claude Code).
At N=200: accuracy is effectively **zero** for any single-pass agent.

The problem isn't model capability. It's architecture. A single model accumulating raw tool output degrades in context quality until it's working blind. More context window doesn't help — it just means more noise to drown in.

## The Architecture

Four mechanisms, each targeting a different failure mode:

### 1. Decomposition (targets: Scope Failures)

A single LLM call before deliberation decides whether to split the task into subtasks. Each subtask gets its own full PERCEIVE → ACT cycle. This keeps each execution unit small enough for reliable per-step recall.

**Key insight**: Decomposition should prefer small chunks (2-3 dependent steps) rather than large ones (5+). Internal compounding within a chunk is r^2 vs r^5.

**Already implemented**: `_decompose()` in orchestrator.py. Validated: eliminated all Scope Failures in N=10.

### 2. Compression at Seams (targets: Recall Failures)

After each subtask completes, a compression step produces a **transition summary** before the next subtask begins. Three questions:

1. **What was produced?** — Key results, values, artifacts
2. **What matters next?** — Which results the next subtask needs
3. **What would I do next?** — Suggested approach for continuation

This replaces the current handoff (raw file paths + 500-char previews) with a clean, relevant context. The next subtask starts with exactly what it needs, not a pile of raw output to sift through.

**Implementation**: Single LLM call (Haiku-class) after each subtask in `_run_decomposed()`. The compressed summary becomes the primary context for the next subtask, with file paths available for the executor to read full data if needed.

### 3. Execution Skills + Verification (targets: Compounding Failures)

Two layers of quality:

**Front-loaded (Skill)**: The executor receives general reliability techniques:
- Use code for computation, not mental math
- Assert intermediate values before using them downstream
- Cross-check totals against inputs
- Print verification summaries

**Back-checked (Verify)**: Post-execution verification checks **evidence**, not vibes:
- Did assertions pass? What was the exit code?
- Do cross-checks reconcile?
- If failed: targeted corrections (fix line 47), not full 15K rewrites

**Implementation**: Skill injected into executor prompt via `_build_prompt()`. Verifier split into pre-exec (approach soundness) and post-exec (evidence checking).

### 4. Learning from Failure (targets: Repeated Failures)

When verification catches a failure, the system writes a structured learning to a searchable store. Before future SPECIALIZE and DECOMPOSE steps, the system retrieves relevant learnings via RRF and injects them into the planning context.

A learning captures: what happened, why it failed, what to do differently. Not a static skill update — a growing corpus of searchable experience.

**Implementation**: Learnings stored as JSON in `learnings/` directory. Retrieved via keyword matching + recency weighting before planning.

## CRL Math With This Architecture

Single model at N=200: `r^200 ≈ 0` (for any r < 1)

Eidos at N=200 with chunk size 3:
- 67 chunks, each with internal compounding of `r^3`
- At r=0.95: each chunk accuracy = 0.857
- But compression resets context quality between chunks
- And learnings improve r over time as the system accumulates experience
- Effective accuracy stays **bounded**, not decaying to zero

The math isn't close at scale. This is the moat.

## Why This Can't Be Replicated by Bigger Context Windows

The problem isn't context capacity. It's context **degradation**. More context = more noise. A 1M token window with 800K tokens of accumulated tool output is worse than a 200K window with clean, compressed context.

Compression, retrieval, and learning are architectural properties, not model features. They can't be shipped as a model update. They require the system to be designed this way from the start.

## Predicted Impact on CRL Benchmark

Current A3 N=10: **64.1%** (41/64) — with DECOMPOSE only

| Fix | Expected Recovery |
|-----|-------------------|
| Compression at seams | +5-7 RF recovered |
| Execution skill | +5-7 RF recovered |
| Evidence-based verification | +2-3 RF recovered |
| Targeted corrections | +3-4 CF recovered |
| **Overlap/diminishing** | -3-4 |
| **Predicted score** | **52-57/64 (81-89%)** |

Getting to 90%+ consistently likely requires the learning loop to run across multiple trials.

---

*This architecture implements THE-LOOP philosophy: PERCEIVE → DECOMPOSE → SPECIALIZE → ACT → COMPRESS → VERIFY → LEARN → RETRY*
