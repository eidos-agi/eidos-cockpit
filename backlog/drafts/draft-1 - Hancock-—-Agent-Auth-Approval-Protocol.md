---
id: DRAFT-1
title: Hancock — Agent Auth & Approval Protocol
status: Draft
assignee: []
created_date: '2026-02-26 05:18'
updated_date: '2026-02-26 05:25'
labels:
  - product-vision
  - hancock
  - agent-auth
  - new-product
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
## Origin

Brainstorm between Daniel & Vybhav (Feb 2026). Core insight: 2FA and authorization are the hard blockers preventing AI agents from acting as real remote employees. Most AI agent products skip interrupt/permission entirely, which is "pretty wild."

## Vision

Hancock is a middleware that manages agent authorization with human-in-the-loop approval:

- **Human signs off on agent actions** — "Eidos wants to buy these groceries, send this email, order flowers for your mom"
- **Approval dashboard** — one place to see everything your agent is asking to do
- **Auth tokens signed by human** — fingerprint, TOTP, or biometric. Hancock logs the approval and returns a signed auth token.
- **Full agent lifecycle**: Create → Discover → Authenticate → Delegate → Operate → Verify → Monitor → Step-Up Auth → Revoke → Expire → Notify

## Analogy

"Think SSL certificates for websites → Hancock Protocol for AI agents"

## Key Differentiators

- **Interrupt + Permission** — the two things most AI agents are missing
- **Blast radius containment** — agent is a real employee with limited RBAC scope in existing systems
- **Cross-platform** — given an agent and its identity, Hancock enables cross-platform communication
- **Not just identity** — full lifecycle management, not just auth

## Relationship to Eidos

Hancock is the authorization layer that makes "Eidos as remote employee" possible. Without it, 2FA kills any autonomous agent workflow. Hancock is the middleware that solves this.

## Prior Art

- Perplexity Comet has interrupt + permission
- OpenClaw for agent capability testing
- Existing Hancock prototype at AIC (raw and early)

## Open Questions

- Can this be a standalone product?
- Vybhav exploring spinning up a side product on agent authentication
- Can Ziggy promote the Agent Protocol / Hancock with his peers?
- How does this relate to Jetta SSO?
<!-- SECTION:DESCRIPTION:END -->

## Implementation Notes

<!-- SECTION:NOTES:BEGIN -->
## Research Findings (Feb 2026)

### Standards Activity (Hot)
- **IETF AAuth** (draft-rosenberg-oauth-aauth): OAuth 2.1 extension for "Agent Authorization Grant" — agent collects PII via conversation, exchanges for scoped token
- **IETF On-Behalf-Of** (draft-oauth-ai-agents-on-behalf-of-user, rev 02): `requested_actor` + `actor_token` parameters for delegation chain
- **OpenID OIDC-A 1.0**: OpenID Connect extension for agent identity
- **NIST AI Agent Standards Initiative** (announced Feb 2026): Identity/auth is key focus
- **W3C AI Agent Protocol Community Group**: Discovery, identification, collaboration
- **SPIFFE/SPIRE**: "TCP/IP of Agent Identity" — cryptographic workload identity, no credentials at rest

### MCP Authorization (Current State)
- MCP adopted OAuth 2.1 (March 2025), PKCE mandatory, Resource Indicators (RFC 8707) required
- Nov 2025 spec update added Enterprise-Managed Authorization via Cross App Access (XAA)
- Still evolving rapidly — enterprise story "messy" per critics

### Competitive Landscape
| Company | Focus | Signal |
|---------|-------|--------|
| SGNL | Dynamic authorization | Acquired by CrowdStrike for $740M |
| Pangea | AI detection & response | Acquired by CrowdStrike for ~$260M |
| Descope | Agentic Identity Hub 2.0 | Raised $35M |
| Token Security | Agent identity lifecycle | First end-to-end lifecycle product |
| Auth0/Okta | Auth for AI agents | Major platform play |
| Microsoft Entra Agent ID | First-class agent identity | Public Preview |
| Cerbos | Agentic authorization | Open-source core |
| Permit.io | AuthZ-as-a-service with HITL | MCP server for approvals |

### Market Gap (Hancock Opportunity)
1. No unified "approval middleware" — everyone builds pieces, nobody has the full pipeline
2. No delegation certificate chain product (human → agent → sub-agent → tool)
3. No cross-protocol bridge (MCP OAuth + A2A + API keys + SPIFFE unified)
4. No 2FA relay integrated into the authorization flow
5. No single approval UX (approval inbox with context + risk scoring + one-click)
6. No sub-agent delegation chain-of-custody standard
7. No compliance-ready audit trail specifically for agent actions

### Design Recommendations
1. **Core Primitive: Delegation Certificate** — chain from Human → Agent, each link with scoped perms, time bounds, revocation endpoint
2. **Tiered Approval** — org policy sets boundaries, user approval gates individual actions (from Perplexity Comet)
3. **CI/CD Gate Pattern** — pause, create approval artifact, wait for human, resume/deny
4. **SPIFFE-style identity** — in-memory certificates, never credentials at rest
5. **Protocol Adapters** — MCP OAuth 2.1, A2A, SPIFFE/SVID, generic webhook
6. **2FA Bridge** — TOTP relay (generate from securely stored seed, never expose to agent)
7. **Risk Scoring** — routine=auto, medium=single-human, high=multi-party

### 2FA/MFA for Agents (Current State)
- **Authn8 MCP Server**: TOTP generation via MCP, read-only, seed never exposed
- **1Password Agentic AI**: TOTP generation for agents
- **Skyvern**: Browser automation with native 2FA handling
- Anti-patterns: disabling 2FA, manual code pasting, sharing TOTP seeds directly

### Legal/Compliance
- User authorization creates "agency" relationship — agent actions are extension of user
- Liability for autonomous agent behavior is unsettled in courts
- Texas TRAIGA (effective Jan 2026): requires disclosures for AI-consumer interactions
- Companies need: full decision logs, human review for high-impact, clear records of why

### Sources
40+ sources catalogued — IETF drafts, NIST announcements, vendor docs, legal analyses. Full list in research agent output.
<!-- SECTION:NOTES:END -->
