# AID Protocol — Knowledge Summary

**Source**: `/Users/vlr/Workspace/bbytesdev/AFProtocal/aid-protocol`
**Captured**: 2026-02-25
**Relationship**: AID Protocol was **created to support Eidos** — it is the authentication foundation specifically designed for Eidos AGI's agent.

---

## What is AID Protocol?

A comprehensive framework for AI agent identity, authentication, and authorization. 11 lifecycle stages, Ed25519 cryptography, cross-platform delegation, enterprise-grade controls.

**Purpose-Built for Eidos**: This isn't a third-party protocol being adopted — it was designed specifically to enable Eidos's secure operation in the world.

## Package Structure

```
aid-protocol/
├── packages/
│   ├── core/       # Types, constants, errors, interfaces
│   ├── crypto/     # Ed25519 cryptographic operations
│   ├── registry/   # Nonce, revocation, rate limiting, spend tracking
│   ├── verify/     # 11-step verification engine
│   ├── sap/        # Step-Up Authorization Protocol
│   ├── sdk/        # High-level API
│   ├── middleware/ # Express/Fastify integration
│   ├── wellknown/  # .well-known/agent.json discovery
│   └── cli/        # Command-line interface
├── test/           # Integration tests
└── demos/          # Interactive HTML demos
```

## Key Concepts

| Concept | Description |
|---------|-------------|
| **AID** | Agent Identity Document — agent credentials with dual signatures |
| **DT** | Delegation Token — scoped permissions across platforms |
| **SAP** | Step-Up Auth Protocol — human approval for high-value actions |
| **Verify Engine** | 11-step verification pipeline |
| **Registry** | Nonce store, revocation, rate limiting, spend tracking |

## 11 Lifecycle Stages

1. Creation — AID with signed credentials
2. Discovery — `.well-known/agent.json`
3. Authentication — Ed25519 signatures
4. Delegation — Cross-platform DTs
5. Operation — HTTP middleware
6. Verification — 11-step engine
7. Monitoring — Spend tracker, rate limiter, audit logs
8. Step-Up Auth — User approval for high-value actions
9. Revocation — Cascade revocation
10. Expiration — Built-in with key rotation
11. Notifications — Webhook events

## 11-Step Verification

1. Structural validation
2. Signature verification
3. Expiration check
4. Revocation status check
5. Scope verification
6. Rate limiting check
7. Spend tracking
8. Domain restrictions
9. TLS version enforcement
10. Geographic restrictions
11. Nonce freshness validation

## Delegation Token Types

- **Targeted**: Specific action on specific platform
- **Wildcard**: Actions across platforms
- **Chained**: Agent-to-agent delegation

## Auth Assurance Levels

- `aal1`: Basic authentication
- `aal2`: Multi-factor authentication
- `aal3`: Hardware-backed authentication

## Key Advantages

- Zero dependencies (Node.js built-in crypto)
- Cross-platform (not blockchain-specific)
- Enterprise-ready (monitoring, revocation, spending controls)
- Standard compliance (HTTP 402, well-known patterns)
