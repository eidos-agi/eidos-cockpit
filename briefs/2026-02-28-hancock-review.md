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

Your research flagged this as open: "Where does Eidos store its AID private key? (Knox? HSM?)"

This is the highest-stakes choice. The signing key touches every action Eidos takes, and your "not extractable" requirement rules out some obvious options. Things worth thinking through:

- What's the threat model — compromised agent runtime? Stolen disk? Insider access?
- Should the key ever be in Eidos's process memory, or does signing happen out-of-process?
- Is there a key rotation story? One key forever is brittle. Multiple keys (root vs. operational) adds complexity but limits blast radius.
- How does Knox fit here — is it the right layer for this, or does this need something closer to hardware?

**What do you think the right answer is?**

---

## Decision 2: Principal DID Method

Your brief surfaced three candidates: `did:email`, `did:web`, `did:key`. Each has real tradeoffs.

Questions that should drive the choice:

- What happens when a principal loses their device? Rotation and recovery story matters a lot here.
- Do we need resolution (looking up a DID to find keys/endpoints), or is self-contained enough?
- What's the human approval UX — browser/passkeys, mobile push, hardware key, CLI? The DID method should match the signing surface.
- Will principals always be individuals, or do we need to support orgs/teams from day one?

**Which way are you leaning, and why?**

---

## Decision 3: Cross-Platform DT Acceptance

Your DT design (targeted, wildcard, chained) is strong as an internal authorization primitive. The open question is what happens at the boundary — when Eidos needs to actually talk to Stripe, Gmail, or Slack.

Things to figure out:

- Does Hancock translate DTs into service-native auth (OAuth, API keys), or do we push for native DT adoption? What's the realistic near-term path?
- If Hancock translates, where do the service credentials live? How do we scope them so a DT for `slack:post` can't be used to escalate into `slack:read`?
- How do we limit blast radius if a connector is compromised — per-service isolation, separate credential sets, something else?
- What's Knox's role here — vault for all service credentials?

**What's your architecture for this?**

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

## Next Steps

- [ ] **Vybhav**: Respond to the 3 decisions — commit a response doc or annotate this one
- [ ] **Vybhav**: Push AID Protocol repo to `eidos-agi` org so we can both work against it
- [ ] **Both**: Once decisions are locked, write `decisions/2026-03-XX-hancock-architecture.md`

---

*Reviewed by Daniel. Feb 28, 2026.*
