# Hancock — Research Review & Architecture Decisions

**Date** 2026-02-28
**Reviewer** Daniel
**Status** Awaiting Vybhav's input
**References** `briefs/2026-02-25-hancock-auth-research.md`, `briefs/2026-02-25-hancock-approval-dashboard-design.md`, `knowledge/aid-protocol-summary.md`, `knowledge/aid-protocol-license-analysis.md`

---

## Review Summary

Vybhav's Feb 25 research session was strong. The AID Protocol analysis, capability namespace, and dashboard wireframes are thorough and ready to build on. Three architecture decisions need to be locked before we prototype.

### What's Strong

- **AID Protocol = purpose-built for Eidos** — no integration tax, no license risk, full ownership. Major shortcut.
- **SAP flow (SUR -> SUA -> SUT)** maps naturally to the approval dashboard + 2FA relay from the Eidos vision. Not forced.
- **Dashboard wireframes** are detailed enough to nearly build from — real component specs, risk assessment logic, API contracts.
- **Capability namespace** (`screen:control`, `terminal:exec:safe`, etc.) is well-scoped — not too granular, not too coarse.

### What's Missing

- **Integration surface with Eidos Core** — the research treats Hancock as standalone, but the orchestrator in eidos-v5 needs concrete touchpoints. Specifically: how does a running pod request a DT mid-mission? What event/callback does SAP use to pause the orchestrator while waiting for human approval? Does the orchestrator poll, or does the SUT get pushed back? These are the APIs that connect your dashboard design to the engine.
- **AID Protocol repo** exists only on Vybhav's machine (`/Users/vlr/Workspace/...`). Needs to be in the `eidos-agi` GitHub org before anyone else can build on it.
- **The 3 open questions need answers before prototyping** — otherwise we'll build a prototype and immediately refactor once these are resolved. Locking them now protects everyone's time. See decisions below.

---

## Decision 1: Key Storage

**Question:** Where does Eidos store its AID private key?

**Recommendation:** Building on your AID Protocol crypto analysis (Ed25519, dual signatures), this pushes toward an HSM/KMS-backed signing service. Never load the key into Eidos process memory.

Architecture:
- Tiny signing service exposes only `sign(payload_hash)` over mTLS or Unix socket
- Signer enforces its own policy — rate limits, allowed payload types, spend caps
- Append-only audit log for every signing operation
- Even if Eidos runtime is compromised, attacker can't extract the key — only abuse the signer under existing controls

Key hierarchy:
- **Root key** — offline/cold, signs rotations only
- **Operational signing key** — hot, used for frequent action signing, fast rotation
- **Optional high-risk key** — separate key for dangerous capabilities (terminal:exec, screen:control), requires SAP regardless of autonomy level

**Vybhav:** Does this fit with how you envisioned Knox's role? Is there a simpler interim approach for prototyping while we build toward HSM?

---

## Decision 2: Principal DID Method

**Question:** Which DID method for human owners (the people who delegate to Eidos)?

**Recommendation:** Your brief surfaced `did:email`, `did:web`, and `did:key` as candidates. After review, `did:key` as the cryptographic principal + email as attribute looks like the strongest option.

Rationale:
- `did:key` — self-contained, no resolution dependency, fits AID Protocol's "zero external deps" philosophy
- `did:email` is not a widely standardized DID method with clean resolution/rotation/recovery — skip it as a primitive
- Bind a **verified email address** to the `did:key` for notifications and SAP flows
- Use **WebAuthn/passkeys** for the actual human UX of signing approvals
- Support `did:web` additionally for organizations and enterprises
- Rotation story: old key signs new key, or multi-party recovery with time delay

**Vybhav:** You mentioned `did:email` as an option in the research brief. Any reason to prefer it over `did:key` + email attribute? Also — what's your expected approval UX: WebAuthn, mobile push, hardware keys, or CLI signing?

---

## Decision 3: Cross-Platform DT Acceptance

**Question:** Will external services (Stripe, Gmail, Slack) accept AID Delegation Tokens natively?

**Recommendation:** Your DT design (targeted, wildcard, chained) is the right internal primitive. But external services won't adopt it natively near-term. Hancock translates DTs to service-native auth.

Architecture:
```
DT verified (11-step engine)
  -> Connector selected (Stripe/Gmail/Slack adapter)
    -> Connector uses service-native credentials (OAuth/API key) from Knox
      -> Action performed + signed audit record emitted
```

Key constraints:
- Per-connector sub-credentials to limit blast radius (separate OAuth apps per service)
- DT scope must bind to connector scope (`slack:post` DT can't be used to read DMs)
- Design DTs to be adoptable by third parties later, but ship with translation now
- Knox holds OAuth tokens / API keys for every service Eidos uses

**Vybhav:** This means Knox becomes the vault for all service credentials. Are you good with that responsibility model? Also — is Eidos running locally, cloud-hosted, or hybrid? This affects the connector architecture significantly.

---

## Failure Modes to Address

These came from an external architecture review and aren't covered in the current research:

1. **Compromised runtime** — need process isolation between agent runtime and connectors/secrets. If Eidos is compromised, attacker shouldn't reach OAuth tokens directly.
2. **TOCTOU (time-of-check/time-of-use)** — SUT must cryptographically bind the exact payload hash to what was approved, not just "approve $200 to Stripe."
3. **UI spoofing in step-up** — approval dashboard must show a canonical, signer-verifiable summary derived from the signed bytes. Attacker shouldn't be able to alter what the human sees vs. what gets signed.
4. **Nonce race conditions** — durable atomic check-and-set required, not in-memory. Cross-region clock skew handling needed.
5. **Audit log tampering** — append-only, signed, shipped off-host (WORM-like storage). Logs are security-critical evidence.
6. **Prompt injection -> privilege escalation** — planted instructions in emails/Slack/screens causing Eidos to request legitimate permissions. Need strong content detectors + domain allowlists + "reason required" fields.

---

## Suggested Build Order

1. Non-exportable signing boundary (HSM/KMS + policy + audit)
2. Connector architecture (Stripe/Gmail/Slack with scoped sub-credentials)
3. SAP hardening (payload binding, anti-approval-fatigue, canonical rendering)
4. Durable registries (nonce + revocation + spend with strong consistency)
5. Operational safety rails (egress allowlists, anomaly detection, kill switch)

---

## Action Items

- [ ] **Vybhav**: Respond to the 3 decisions (commit a response doc or annotate this one)
- [ ] **Vybhav**: Push AID Protocol repo to `eidos-agi` org on GitHub
- [ ] **Vybhav**: Answer deployment model — local, cloud-hosted, or hybrid?
- [ ] **Vybhav**: Answer approval UX — WebAuthn, mobile push, hardware keys, or CLI?
- [ ] **Both**: Once decisions are locked, write `decisions/2026-03-XX-hancock-architecture.md`

---

*Review conducted by Daniel with architecture consultation from GPT-5.2. Final decisions are ours jointly — these recommendations are starting positions, not verdicts. Feb 28, 2026.*
