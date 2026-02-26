# Decision: Hancock — Agent Auth & Approval Protocol

**Date:** 2026-02-26
**Pilots:** Daniel, Vybhav
**Status:** Accepted (Research Phase)

## Context

2FA and authorization are the hard blockers preventing AI agents from acting as real remote employees. Most AI agent products skip interrupt/permission entirely.

## Decision

Build **Hancock** — a middleware that manages agent authorization with human-in-the-loop approval.

"Think SSL certificates for websites → Hancock Protocol for AI agents."

## Key Design Points

- Human signs off on agent actions via approval dashboard
- Auth tokens signed by human (fingerprint, TOTP, biometric)
- Full agent lifecycle: Create → Authenticate → Delegate → Operate → Verify → Monitor → Step-Up Auth → Revoke
- Tiered approval: org policy sets boundaries, user approval gates individual actions
- 2FA relay built into the authorization flow (not a separate tool)

## Market Signal

- CrowdStrike spent $1B acquiring SGNL + Pangea (agent identity companies)
- NIST launched AI Agent Standards Initiative Feb 2026
- IETF has active OAuth extension drafts for AI agents
- Nobody has built the unified approval middleware yet

## Consequences

- Hancock becomes a dependency for the "Eidos as remote employee" vision
- May become a standalone product
- Must be MCP OAuth 2.1 compatible
- Needs SPIFFE-style identity (no credentials at rest)
