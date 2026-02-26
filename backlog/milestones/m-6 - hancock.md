---
id: m-6
title: "Hancock — Agent Auth & Delegation"
---

## Description

The authorization middleware that makes "Eidos as remote employee" possible. Human-in-the-loop approval for agent actions, delegation certificates, 2FA relay, and blast radius containment.

Key deliverables:
- Delegation certificate chain (Human -> Agent -> Sub-agent -> Tool)
- Approval dashboard (single UX for all pending agent actions)
- 2FA bridge (TOTP relay without exposing seeds)
- RBAC/PBAC integration for scoped agent permissions
- Audit trail for EU AI Act compliance

Research complete (DRAFT-1). Design phase next.
