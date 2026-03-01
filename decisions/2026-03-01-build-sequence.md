# Decision: Company Build Sequence — "The Agent Builds Its Own Company"

**Date:** 2026-03-01
**Status:** Accepted
**Authors:** Daniel Shanklin, Vybhav
**Supersedes:** Previous 4-phase roadmap (Section 8 of company plan)

---

## Context

Eidos AGI has a working product vision, a purpose-built auth protocol, operational infrastructure (domain, email, cockpit), and two founders shipping daily. The question is: what order do we build in?

The previous roadmap (Hancock prototype first, then identity stack, then first worker, then platform) treated this like a normal startup where humans handle legal/finance and AI builds product. That's wrong. **The founding story IS the product demo.**

The insight: a functioning Eidos + Helios fast-starts everything else. The agent doesn't just build the product — it sets up the company, earns money, and proves the thesis by doing it.

Daniel is willing to invest 1-2 days of human time to get Eidos+Helios fully operational. After that, the agent takes over.

## Decision

Build in this order. Each phase depends on the one before it.

---

### Phase 0: Bootstrap the Bootstrap
**Duration:** 1-2 weeks | **Driver:** Human (Daniel)

> *Original estimate was 1-2 days. Revised after gap analysis — see `briefs/2026-03-01-holes-in-the-plan.md`.*

**Goal:** Get Eidos+Helios reliable enough to operate autonomously.

**What exists:**
- Helios is functional (used throughout the Feb 28 session)
- Email works (daniel@eidosagi.com active, DNS configured)
- Cockpit is operational (fleet-aware, multi-pilot)
- Helios Postgres database layer written (db.ts, 585 lines, multi-tenant RLS) but not wired in

**What's needed:**
- Deploy Helios hub to Railway (cloud hub — currently dies when laptop sleeps)
- Wire hub's 38 filesystem operations to Postgres, Dockerize, deploy
- Helios reliability fixes (button clicking failures, CSP issues, site learning)
- Graduated difficulty testing (simple forms → multi-step flows → financial sites)
- Email verification (SPF/DKIM/DMARC pass confirmed in headers)
- Migadu upgrade to Standard ($29/mo) before March 15 deadline
- Vybhav's mailbox (vybhav@eidosagi.com)
- Agent email (eidos@eidosagi.com) via Migadu REST API
- Minimal persistent agent loop (task queue → Claude Code → audit log)
- Shared secrets system for founders + agents
- Server unsuspension (HostRound EPYC)
- Evidence capture setup (audit trail to cloud, screen recording for key moments)

**Human-gated:** Helios debugging (pairing with Daniel), payment for Migadu upgrade, Railway Postgres provisioning

**Success criteria:** Agent can reliably navigate websites, fill forms, read pages, and send email — and can do so when Daniel's laptop is closed.

---

### Phase 1: Eidos Sets Up Its Own Company
**Duration:** 1-2 weeks | **Driver:** Agent (Eidos via Helios)

**Goal:** The agent incorporates Eidos AGI and sets up all company infrastructure. This is the demo.

**Depends on:** Phase 0 (reliable browser automation + email)

**Tasks the agent does:**
- File Delaware C-Corp via Stripe Atlas or similar (browser automation)
- Get EIN from IRS (online SS-4 filing)
- Open business bank account (Mercury, Brex, or similar — all online)
- Set up accounting (QuickBooks, Wave, or similar)
- Register with Delaware Franchise Tax
- Set up registered agent service
- Create founder equity agreements (restricted stock purchase agreements)

**Human-gated:** Signing legal documents (Daniel signs, agent prepares), SSN/personal info entry (Daniel provides, agent fills)

**Success criteria:** Eidos AGI is a legal entity with a bank account, EIN, and clean cap table.

---

### Phase 2: Eidos Earns Money
**Duration:** 2-4 weeks | **Driver:** Agent (proof of concept)

**Goal:** Prove the agent can do real work for real money.

**Depends on:** Phase 1 (bank account to receive payment)

**Tasks:**
- Create Fiverr seller account (agent operates via Helios)
- List services the agent can deliver (data entry, research, document formatting, web scraping)
- Complete tasks, deliver work, collect payment
- Document everything — this is the case study

**Human-gated:** Initial Fiverr account verification (may need ID)

**Success criteria:** Agent earns its first dollar. Not about the revenue — about proving the thesis.

---

### Phase 3: Hancock + Supervision Layer
**Duration:** 4-6 weeks | **Driver:** Agent + Human

**Goal:** Build the trust/auth infrastructure that makes this safe at scale.

**Depends on:** Phase 0 (infrastructure), informed by Phase 1-2 (real operational experience)

**Tasks:**
- Hancock frontend prototype (approval dashboard)
- AID Protocol integration (delegation certificates)
- Supervision gradient: which actions need approval vs autonomous
- 2FA relay for sensitive operations

**Why this moved later:** The original plan had Hancock first. But building auth before having real operations to authorize is premature. Phases 1-2 generate the operational data that informs what needs supervision.

**Success criteria:** Agent operates with proper authorization, audit trail, and human oversight for high-stakes actions.

---

### Phase 4: Platform + Customers
**Duration:** 8-12 weeks | **Driver:** Agent + Human

**Goal:** Package Eidos as a product other people pay for.

**Depends on:** Phase 3 (trust infrastructure)

**Tasks:**
- Multi-tenant provisioning
- Customer onboarding flow
- First paying customer (not Fiverr — a business paying for a digital worker)
- Pricing, billing, support

**Success criteria:** Recurring revenue from customers who aren't the founders.

---

## What Changed

| Before | After | Why |
|--------|-------|-----|
| Hancock first | Bootstrap + self-incorporation first | The demo IS the product. Auth before operations is premature. |
| Linear build toward platform | Agent bootstraps its own company | Proves the thesis, generates case study, fast-starts legal/financial setup |
| Human handles legal/finance | Agent handles legal/finance (human signs) | The founding story becomes the pitch deck |
| Revenue in Phase 3+ | Revenue in Phase 2 (Fiverr) | Earliest possible proof of value creation |

## Consequences

- Hancock prototype moves from Phase 1 to Phase 3. This is deliberate. Real operational experience in Phases 1-2 informs what needs supervision.
- Phase 0 is the critical path. If Helios isn't reliable, nothing else works.
- The story we tell investors/customers changes from "we built auth for AI agents" to "our AI agent incorporated its own company, opened a bank account, and earned money on Fiverr."
- Risk: Phase 1 depends on Helios being rock-solid for complex web flows (Stripe Atlas, IRS, Mercury). If it can't handle these, we fall back to human-assisted with agent prep work.

## References

- `briefs/2026-02-28-company-plan.md` — Full company plan (Section 8 updated to match)
- `decisions/2026-02-26-north-star-universal-computer-control.md` — The capability that makes this possible
- `knowledge/company-setup-log.md` — Infrastructure already in place
