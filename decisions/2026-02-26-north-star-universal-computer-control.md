# Decision: North Star — Universal Computer Control

**Date:** 2026-02-26
**Pilots:** Daniel, Vybhav
**Status:** Accepted

## Context

Brainstorm between Daniel & Vybhav crystallized the Eidos product vision. The key question: what separates a real agent from a bot?

## Decision

**The agent must work with ANY website, ANY terminal, ANY interface — through screen-and-keyboard control, not pre-built API integrations.**

- APIs are **optimizations** (faster, cheaper for known services)
- Screen control is the **universal fallback** (works with anything)
- Token burn is an acceptable cost for universality
- If it needs a pre-built integration, it's just a bot

## Three Pillars

| Pillar | What | Purpose |
|--------|------|---------|
| Eidos Core (brain) | PEFM memory, pods, orchestrator | Thinking and remembering |
| Hancock (auth) | Delegation certs, approval dashboard, 2FA relay | Permission and trust |
| Agent Identity (body) | Universal computer control, email, phone, Slack | Acting in the world |

## The Test

Give Eidos a website it has never seen. If it can navigate and complete the task through the UI — full agent. If it needs a pre-built integration — just a bot.

## Consequences

- Computer Use / CUA-level screen control is the PRIMARY interaction mode
- API integrations are welcome but never required
- We accept higher token costs for universality
- "Supported integrations" page is antithetical to the vision
