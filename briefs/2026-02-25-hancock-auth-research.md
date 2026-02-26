# Hancock — Authentication Research Brief

**Date** 2026-02-25
**Researcher** Unknown Pilot
**Status** Draft — Ready for Review

---

## Executive Summary

The **AID Protocol** at `/Users/vlr/Workspace/bbytesdev/AFProtocal/aid-protocol` provides a production-ready framework for agent identity, authentication, and delegation.

**Key Insight**: AID Protocol was **created specifically to support Eidos** — this is purpose-built infrastructure, not a third-party adoption.

**Hancock** IS the implementation of AID Protocol for Eidos. Hancock = AID Protocol + Eidos-specific extensions (screen control, persistent identity, approval dashboard).

---

## AID Protocol Overview

### Architecture
- **8 packages**: core, crypto, registry, verify, sap, sdk, middleware, wellknown
- **Zero dependencies**: Uses Node.js built-in crypto (Ed25519)
- **11 lifecycle stages**: Creation → Discovery → Auth → Delegation → Operation → Verification → Monitoring → Step-Up → Revocation → Expiration → Notifications

### Key Components

| Component | What It Does |
|-----------|-------------|
| **AID** (Agent Identity Document) | Agent credentials with dual signatures (principal + issuer) |
| **DT** (Delegation Token) | Scoped permissions across platforms (targeted, wildcard, chained) |
| **SAP** (Step-Up Auth Protocol) | Human approval for high-value actions (SUR → SUA → SUT) |
| **Verify Engine** | 11-step verification (structure, signatures, expiration, revocation, scope, rate limits, spend, domains, TLS, geo, nonce) |
| **Registry** | Nonce store, revocation, rate limiting, spend tracking |

### Crypto Model
- **Ed25519** signatures throughout
- **Dual signatures**: Principal signs agent + principal + capabilities; Platform signs complete AID
- **Base64url encoding** for all keys
- **Key identifiers** for rotation

### Auth Assurance Levels (AAL)
- `aal1`: Basic authentication
- `aal2`: Multi-factor authentication
- `aal3`: Hardware-backed authentication

---

## AID Protocol Elements for Hancock

### 1. Agent Identity Document (AID)
```typescript
interface AgentIdentityDocument {
  aid: {
    spec: string           // 'aid/1.0'
    id: string            // did:aid:web:platform:agents:xxx
    type: 'consumer' | 'worker' | 'assistant' | 'autonomous'
    status: 'active' | 'suspended' | 'revoked'
  }
  principal: {
    id: string            // Principal's DID (human owner)
    displayName: string
    verification: {
      method: 'oidc' | 'saml' | 'passkey' | 'manual'
      provider: string
      assurance: AuthAssuranceLevel
      mfaFactors: string[]
    }
    contact: {
      notificationEndpoint: string  // Webhook for approvals
      publicKey: string
    }
  }
  capabilities: {
    declared: string[]     // Allowed capabilities (e.g., "screen:control", "http:request")
    excluded: string[]     // Explicitly denied
    maxAutonomyLevel: 0-3  // 0=no autonomy, 3=full autonomy
  }
  runtime: AgentRuntime   // Platform endpoints, environment
  trust: {
    issuer: Platform info
    principalSignature: Signature
    issuerSignature: Signature
  }
}
```

**Adopt for Hancock**: This is the foundation. Eidos's AID will include capabilities for `screen:control`, `keyboard:input`, `http:request`, etc.

### 2. Delegation Token (DT)
```typescript
interface DelegationToken {
  dt: {
    spec: string
    id: string
    parentAid: string     // Links back to agent's AID
    type: 'targeted' | 'wildcard' | 'chained'
  }
  scope: {
    granted: string[]     // Allowed actions/domains
    denied: string[]      // Explicitly denied
    categories: string[]  // Pre-defined bundles
  }
  temporal: {
    notBefore: Timestamp
    notAfter: Timestamp
    activeHours: [start, end]
  }
  financial: {
    maxTransactionValue: number
    dailySpendLimit: number
    monthlySpendLimit: number
  }
  rateLimit: {
    perMinute: number
    perHour: number
    perDay: number
  }
  autonomy: {
    maxChainedActions: number
    confirmationRequired: boolean
    stepUpThreshold: number
  }
  trust: {
    signatures: Signature[]
  }
}
```

**Adopt for Hancock**: Use DTs for Eidos-to-Service delegation. Eidos can delegate to:
- Financial services (with spend limits)
- Email providers (with scope restrictions)
- Terminal commands (with allow/deny lists)
- Screen control (with approval thresholds)

### 3. Step-Up Authorization Protocol (SAP)

**Three-object flow for human-in-the-loop approval**:

```
1. SUR (Step-Up Request)  → Platform → Human
   "Eidos wants to transfer $500. Approve?"

2. SUA (Step-Up Approval) → Human → Platform
   Human signs with private key, includes challenge nonce

3. SUT (Step-Up Token)    → Platform → Eidos
   Temporary authorization for this specific action
```

**Adopt for Hancock**: This IS the "approval dashboard" and "2FA relay" from the Eidos vision.

| Use Case | Trigger |
|----------|---------|
| Financial transactions | Value exceeds threshold |
| Sensitive data access | Data classification = PII/PHI |
| Terminal commands | Command matches dangerous pattern |
| Screen control | New domain or first access |
| Autonomous actions | Autonomy level exceeded |

### 4. Verify Engine

**11-step verification** — adopt wholesale for Hancock:

1. Structural validation (JSON schema)
2. Signature verification (principal + issuer)
3. Expiration check (notBefore/notAfter)
4. Revocation status check
5. Scope verification (granted/denied lists)
6. Rate limiting check
7. Spend tracking
8. Domain restrictions
9. TLS version enforcement
10. Geographic restrictions
11. Nonce freshness validation

**Adopt for Hancock**: Use as-is for Eidos's internal verification. Add custom steps for:
- Screen capture verification (no leaked credentials)
- Command pattern matching (dangerous commands)

### 5. Registry Services

| Service | Purpose | Adopt? |
|---------|---------|--------|
| Nonce Store | Prevent replay attacks | ✅ Yes |
| Revocation Store | Cascade revocation | ✅ Yes |
| Rate Limiter | Per-key rate limits | ✅ Yes |
| Spend Tracker | Daily/monthly totals | ✅ Yes |

**Adopt for Hancock**: Use all four. Add Eidos-specific tracking:
- Screen actions per session
- Terminal command patterns
- Data access logs

---

## Eidos-Specific Extensions

### 1. Capability Namespace

Define Eidos-specific capabilities for AID `declared` and DT `granted`:

```
screen:control         — Full screen keyboard/mouse control
screen:read           — Read-only screen access
screen:ocr            — OCR text extraction
keyboard:input        — Direct keyboard input
terminal:exec         — Execute terminal commands
terminal:exec:safe    — Execute non-dangerous commands
http:request          — Make HTTP requests
http:request:tls      — HTTPS-only requests
email:send            — Send email
email:read            — Read email
slack:post            — Post to Slack
slack:dm              — Direct message a user
```

### 2. Autonomy Levels

Define Eidos-specific autonomy levels:

| Level | Meaning | Requires Approval For |
|-------|---------|----------------------|
| 0 | No autonomy | Everything |
| 1 | Low autonomy | New domains, commands with side effects |
| 2 | Medium autonomy | High-value transactions, sensitive data |
| 3 | High autonomy | Nothing (pre-approved ranges) |

### 3. Approval Dashboard

**Hancock Dashboard** — web UI for human approval:

- Real-time approval requests (SURs)
- Action history with replay
- Spend tracking visualization
- Active DTs management
- Revoke button (cascade revocation)
- Autonomy level configuration

**Notification channels**:
- Browser push notifications
- Email
- SMS (for high-value actions)
- Slack DM

### 4. Agent Identity Integration

Eidos has persistent identity. Map to AID:

| Eidos Identity | AID Principal |
|----------------|---------------|
| Email address | `did:email:eidos@example.com` |
| Phone number | `did:tel:+1xxx` |
| Slack user ID | `did:slack:T123:U456` |

Eidos's AID will have **multiple verification methods**:
- Email (OIDC via provider)
- Phone (SMS verification)
- Slack (workspace app)

---

## Recommended Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         HANCOCK                             │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌─────────────────┐    ┌─────────────────────────────────┐ │
│  │  AID Issuer     │    │  Step-Up Auth Protocol (SAP)     │ │
│  │  - Create AID   │    │  - SUR (request)                 │ │
│  │  - Sign certs   │    │  - SUA (approval)                │ │
│  └─────────────────┘    │  - SUT (token)                   │ │
│                         └─────────────────────────────────┘ │
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │  Verify Engine (11 steps)                               │ │
│  │  + Eidos extensions: screen safety, command patterns   │ │
│  └─────────────────────────────────────────────────────────┘ │
│                                                               │
│  ┌─────────────────┐    ┌─────────────────────────────────┐ │
│  │  Registry       │    │  Approval Dashboard              │ │
│  │  - Nonce store  │    │  - Web UI for approvals          │ │
│  │  - Revocation   │    │  - Action history                │ │
│  │  - Rate limits  │    │  - Spend visualization           │ │
│  │  - Spend track  │    │  - Revoke controls               │ │
│  └─────────────────┘    └─────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ uses AID Protocol packages
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    AID Protocol (upstream)                  │
│  core │ crypto │ registry │ verify │ sap │ sdk │ middleware │
└─────────────────────────────────────────────────────────────┘
```

---

## Open Questions

1. ~~**AID Protocol License**~~ — Resolved: AID Protocol was created specifically for Eidos AGI. No licensing concerns.
2. ~~**AID Protocol Maturity**~~ — Resolved: Production-ready, 53 tests passing, purpose-built for Eidos.
3. **Eidos AID Storage** — Where does Eidos store its AID private key? (Knox? HSM?)
4. **Principal DID Method** — Which DID method for human owners? (`did:email`, `did:web`, `did:key`?)
5. **Cross-Platform DTs** — Will other services accept AID DTs? Or does Hancock translate?

---

## Next Steps

1. **Answer open questions** — License, maturity, DID method choice
2. **Prototype Hancock** — Use AID Protocol SDK, add Eidos capabilities
3. **Build approval dashboard** — SAP integration with web UI
4. **Define capability taxonomy** — Full list of Eidos capabilities
5. **Design key management** — Where Eidos stores its AID private key

---

## References

- **AID Protocol**: `/Users/vlr/Workspace/bbytesdev/AFProtocal/aid-protocol`
- **AID Spec**: See `packages/core/` for type definitions
- **SAP Spec**: See `packages/sap/` for Step-Up Auth Protocol
- **Verify Engine**: See `packages/verify/` for 11-step verification
