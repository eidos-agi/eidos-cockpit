---
id: TASK-34
title: Hancock — design doc and minimal prototype
status: To Do
assignee: []
created_date: '2026-02-26'
labels:
  - hancock
  - architecture
  - product
milestone: m-6
dependencies: []
priority: high
---

## Description

Research is complete (see archived DRAFT-1). Now produce a concrete design doc and minimal prototype for Hancock.

### Design Doc Deliverables
- Delegation certificate schema (Human -> Agent -> Sub-agent -> Tool)
- Approval flow sequence diagrams
- 2FA bridge architecture (TOTP relay without exposing seeds)
- API surface for approve/deny/revoke
- Integration points with Eidos orchestrator

### Minimal Prototype
- CLI or web approval dashboard (show pending actions, approve/deny)
- Single delegation certificate issuance
- One 2FA relay flow (TOTP)
- Audit log to SQLite

### Research to Reference
- IETF AAuth (draft-rosenberg-oauth-aauth)
- IETF On-Behalf-Of (draft-oauth-ai-agents-on-behalf-of-user)
- SPIFFE/SPIRE identity model
- MCP OAuth 2.1 authorization
- Competitive: SGNL, Descope, Token Security, Permit.io

## Acceptance Criteria

- [ ] #1 Design doc in decisions/ covering delegation certificates, approval flow, 2FA bridge
- [ ] #2 Minimal CLI prototype: issue certificate, approve action, log audit
- [ ] #3 One end-to-end flow: Agent requests action → Human approves → Certificate issued → Action executes → Audit logged
- [ ] #4 Architecture supports future multi-agent delegation chains
