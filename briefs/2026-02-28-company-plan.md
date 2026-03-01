# Company Plan — Team Eidos

**Date** 2026-02-28
**Authors** Daniel Shanklin, Vybhav (VLR)
**Status** Draft — Internal Alignment Document

---

## 1. Why Now

Boone Voyage is dead. Rhea is dead. The product isn't.

Two previous company entities ran their course. What survived is the work: five active repositories, a crystallized product vision, a purpose-built authentication protocol, and a two-person team that's been shipping — 57 commits in the last week alone. The question isn't whether to start a company. The question is whether the market window is still open.

It is. Three signals converged in early 2026:

- **NIST AI Agent Standards Initiative** (February 2026) — The US government is formalizing how AI agents should identify themselves, request permissions, and be audited. We've been building exactly this.
- **CrowdStrike spent $1 billion** acquiring SGNL and Pangea — two companies focused on agent identity and authorization. The market just priced this problem at ten figures.
- **IETF OAuth extensions for AI agents** — Active drafts extending OAuth 2.1 for non-human actors. The protocol layer is being standardized in real time.

The thesis — that AI agents need real identity infrastructure, not just API keys — went from speculative to validated in Q1 2026. We have a purpose-built protocol (AID Protocol), a working architecture (Eidos Core), and a team that's been building toward this exact moment for months.

No one has shipped the unified middleware yet. The window is open, but it won't stay open.

---

## 2. What We're Building

**Digital workers with real identity.**

Not a chatbot. Not a copilot. Not a feature bolted onto someone else's product.

An AI agent that has a name, an email address, scoped access to your systems, human supervision when it matters, and a full audit trail. It onboards like a remote employee. It works like a remote employee. It costs a fraction of one.

| | Human VA | Eidos Digital Worker |
|---|---|---|
| **Monthly cost** | $1,500–3,000 | $50–100 |
| **Availability** | Business hours | 24/7 |
| **Onboarding** | Days to weeks | Minutes |
| **Audit trail** | Manual, if at all | Every action logged and signed |
| **Access control** | Shared passwords | Scoped delegation tokens, revocable instantly |
| **Scalability** | Hire another person | Provision another worker |

The product is three pillars:

| Pillar | What It Does | Status |
|--------|-------------|--------|
| **Eidos Core** (brain) | PEFM memory, pod architecture, research pipeline, orchestrator | Working prototype in `eidos-v5` |
| **Hancock** (auth) | Delegation certificates, approval dashboard, 2FA relay, step-up authorization | AID Protocol built (8 packages, 53 tests); dashboard designed |
| **Agent Identity** (body) | Email, OS-level accounts, universal computer control, Slack, phone | Architecture defined; implementation next |

The key design principle: **the agent works with ANY website, ANY terminal, ANY interface through screen-and-keyboard control.** APIs are optimizations for known services. Screen control is the universal fallback. If it needs a pre-built integration to function, it's just a bot.

> *See: `decisions/2026-02-26-north-star-universal-computer-control.md`*

---

## 3. The Moat

**Identity infrastructure, not intelligence.**

Intelligence is rented. We call Anthropic or OpenAI, pay per token, and get the best model available today. When a better model ships tomorrow, we switch. Intelligence is a commodity input — powerful, essential, and available to everyone.

Identity is owned. Identity is:

- **Trust** — Can this agent prove who sent it and what it's allowed to do?
- **Auditability** — Can you see every action it took, why, and who approved it?
- **Revocability** — Can you shut it down in one click, cascading across every system it touches?
- **Replaceability** — Can you swap the brain without rebuilding the trust chain?

CrowdStrike validated this thesis by spending $1 billion to acquire it in pieces (SGNL for authorization, Pangea for identity). We're building it as a unified stack, purpose-built for AI agents from day one.

**The AID Protocol** — our authentication and delegation framework — isn't adapted from human IAM. It was designed specifically for non-human actors with:

- Dual-signature credentials (principal + platform)
- Scoped delegation tokens with financial limits, rate limits, and time windows
- Step-up authorization (human-in-the-loop for high-value actions)
- 11-step verification engine (structure, signatures, expiration, revocation, scope, rate limits, spend tracking, domain restrictions, TLS enforcement, geographic restrictions, nonce freshness)
- Cascade revocation — one click to revoke an agent and every token it holds

No one else has this as a unified, purpose-built protocol. The enterprise IAM companies are retrofitting human systems. We started from the agent.

> *See: `knowledge/aid-protocol-summary.md`, `decisions/2026-02-26-hancock-agent-auth-protocol.md`*

---

## 4. Team

Two builders. Both shipping.

**Daniel Shanklin** — Co-Founder. Product vision, architecture, full-stack engineering, business. BBA Finance (Texas State), postgraduate AI/ML (MIT, UT). Career spans hedge fund trading desks (Q Investments), data consulting, and AI venture building — founded Boone Voyage Labs (AI incubator), co-founded SparrowShield (AI insurance underwriting), and served as Chairman/Chief Architect of Rhea AI. Patent holder in cloud control systems. Built Eidos Core (PEFM memory, pod architecture, orchestrator), the operational cockpit, and fleet-aware multi-pilot infrastructure.

**Vybhav Reddy KC** — Co-Founder. Identity resolution, authentication protocols, AID Protocol creator. B.Tech CS (JNTU Anantapur), MS Analytics (Bowling Green State). Currently Senior Manager of Data Science at Socure, where he led the overhaul of their identity resolution system — LSTM model achieving 96% recall and 98.4% precision, generating $5.35M in direct revenue. Published researcher in AI-powered identity verification and NLP. Designed and built the entire AID Protocol from scratch: 8 packages, 53 passing tests, zero external dependencies, Ed25519 cryptography. This is the authentication backbone for every digital worker we deploy.

**The combination:** Daniel builds the agent. Vybhav builds the identity. One founded AI companies and holds a patent in cloud control. The other built identity resolution at one of the leading identity verification companies and then designed a purpose-built authentication protocol for AI agents. Together: digital workers with real identity.

What we're not: a slide deck team. The code exists. The tests pass. The architecture is documented. We're building, not fundraising-then-building.

> *See: `knowledge/founder-profiles.md` for full profiles*

---

## 5. What Exists Today

This is not vaporware. Inventory of working assets:

### Code & Architecture
- **Eidos Core** (`eidos-v5`) — PEFM memory system, pod architecture, research pipeline, orchestrator. Working prototype.
- **AID Protocol** — 8 packages (`core`, `crypto`, `registry`, `verify`, `sap`, `sdk`, `middleware`, `wellknown`). 53 tests passing. Zero external dependencies. Ed25519 signatures throughout. Production-ready framework.
- **Hancock Dashboard Design** — Complete 4-tab specification (Pending Approvals, Activity Feed, Delegations, Governance Settings). Risk assessment logic defined. Notification system designed. API client interface specified. Ready to build.
- **Eidos Cockpit** (`eidos-cockpit`) — Operational multi-pilot mission control with fleet-aware sync, session tracking, decision logging, and knowledge base.
- **Eidos Infrastructure** (`eidos-infra`) — Deployment and infrastructure configuration.
- **Eidos Philosophy** (`eidos-philosophy`) — 30+ design documents defining the agent model, identity architecture, and product vision.
- **Eidos Studies** (`eidos-studies`) — Research experiments and validation.

### Infrastructure
- **GitHub org**: `eidos-agi` — 5 active repositories
- **Dedicated server**: HostRound EPYC (AMD EPYC 4464P, 64GB RAM) — currently suspended, ~$67/mo to resume
- **Domain planned**: `eidosagi.com` — available, $8.88/yr on Porkbun
- **Email planned**: Migadu — $29/mo, unlimited mailboxes (one domain, infinite workers)

### Research & Strategy
- **"Clicking Is Not Configuring"** — Operational brief on configuration-as-code, born from real production incident. Investor-relevant framing of our infrastructure philosophy.
- **Hancock Briefs** (4 documents) — Auth research, dashboard design, AID protocol analysis, integration review. Complete enough to begin implementation.
- **North Star Decision** — Universal computer control as the defining capability. Accepted and documented.

> *See: `briefs/` directory for all research briefs, `decisions/` for architectural decisions*

---

## 6. Company Structure

### Decisions Made (Feb 28, 2026)

| Decision | Resolution | Status |
|----------|-----------|--------|
| **Equity split** | 50/50 — Daniel and Vybhav | **Agreed** |
| **Initial capital** | Daniel seeds funds for servers, email, domain, incorporation | **Agreed** |
| **Start now** | Company scaffolding begins immediately | **Agreed** |

> *See: `decisions/2026-02-28-founding-agreement.md`*

### Decisions Still Open

| Decision | Options | Notes |
|----------|---------|-------|
| **Company legal name** | "Eidos AGI" (matches GitHub org) or new name | "Eidos AGI" has brand continuity |
| **Entity type** | Delaware C-Corp or LLC | C-Corp if fundraising; LLC if bootstrapping. Can start LLC, convert later. |
| **IP assignment** | Assign existing code to new entity | AID Protocol, Eidos Core, cockpit — all need formal assignment |
| **Vesting schedule** | 4-year/1-year cliff (standard) or immediate | Should be discussed |
| **Domain** | `eidosagi.com` | Available now at $8.88/yr; register immediately |

### Remnants to Clean Up

| Remnant | What It Is | Action |
|---------|-----------|--------|
| **Boone Voyage** | Previous company entity | Confirm dissolved; no lingering obligations |
| **Rhea / Rhea Impact** | Dead platform brand | `rhea-impact` GitHub org has cockpit template; `meetrhea.com` is current server hostname |
| **AIC Holdings** | Referenced as parent in some infra docs | Clarify status; remove references if defunct |
| **Server hostname** | `meetrhea.com` on HostRound server | Rename to `eidosagi.com` after domain registration |

These are administrative tasks, not blockers. But they should be resolved before any external-facing activity (fundraising, customer conversations, public launch).

---

## 7. Business Model

### Revenue: Subscription Per Digital Worker

Each customer pays a monthly fee per digital worker deployed. The worker comes with:
- A real identity (email, accounts, credentials)
- Scoped access via delegation tokens
- Human supervision via the Hancock dashboard
- Full audit trail

**Pricing target: $50–100/month per worker.**

This undercuts human virtual assistants by 15–30x while providing better auditability, 24/7 availability, and instant revocability.

### Supervision Gradient

Workers don't start autonomous. Trust is earned:

| Level | Name | What It Means | Approval Required For |
|-------|------|---------------|----------------------|
| 0 | Locked | Approve everything | All actions |
| 1 | Cautious | Approve new and risky | New domains, side effects, destructive commands |
| 2 | Balanced | Approve high-value only | Financial transactions over threshold, sensitive data |
| 3 | Trusted | Pre-approved ranges | Nothing within approved scope |

New workers start at Level 0 or 1. Customers raise the level as confidence builds. This is the core UX insight: **the product gets better the longer you use it**, because the supervision overhead drops while the trust chain strengthens.

### Namespace Model

One domain. Infinite workers.

With Migadu's flat-rate email ($29/mo for unlimited mailboxes), the marginal cost of adding a worker's email identity is zero. Each worker gets `worker-name@eidosagi.com` (or the customer's domain). The namespace scales without per-seat email costs.

### Unit Economics (Per Worker)

| Cost | Amount | Notes |
|------|--------|-------|
| LLM API (Anthropic/OpenAI) | $5–50/mo | Varies by task volume and model |
| Email identity | ~$0 marginal | Migadu flat-rate |
| Compute | ~$0 marginal | Shared server until scale |
| **Total marginal cost** | **$5–50/mo** | |
| **Revenue** | **$50–100/mo** | |
| **Gross margin** | **50–90%** | Improves as autonomy level rises (fewer tokens for supervision) |

---

## 8. Roadmap

### Phase 1: Hancock Frontend Prototype
**Timeline: Now**
- Build the Hancock approval dashboard with mock data
- React + Next.js + Tailwind + shadcn/ui (tech stack already specified)
- All 4 tabs functional: Pending Approvals, Activity Feed, Delegations, Settings
- Mock SURs streaming in via SSE on a timer
- Deploy on Railway
- **Goal**: Tangible, demonstrable product. Something to show.

### Phase 2: Agent Identity Stack
**Timeline: After Phase 1**
- Register `eidosagi.com`, set up Migadu email
- Create OS-level user accounts for workers
- Wire AID Protocol into real identity provisioning
- First real delegation tokens issued and verified
- **Goal**: A worker with a real email address and real scoped access.

### Phase 3: First Digital Worker
**Timeline: After Phase 2**
- Single customer, single worker, full end-to-end loop
- Worker onboards to customer's systems via screen control
- Hancock dashboard provides live supervision
- Customer can approve, deny, modify actions in real time
- Full audit trail from first action to last
- **Goal**: One paying customer. Revenue. Validation.

### Phase 4: Platform
**Timeline: After Phase 3 validated**
- Multi-tenant provisioning (self-service worker creation)
- Customer-owned domains (workers on customer's email domain)
- Autonomy level progression (data-driven trust escalation)
- Worker templates (pre-configured for common roles)
- **Goal**: Scale beyond the first customer without manual setup.

---

## 9. Infrastructure & Costs

### Current Monthly Costs

| Item | Cost | Status |
|------|------|--------|
| HostRound EPYC server (AMD EPYC 4464P, 64GB RAM) | ~$67/mo | Suspended — needs unsuspension |
| Migadu email (unlimited mailboxes) | $29/mo | Not yet activated |
| `eidosagi.com` domain | ~$0.75/mo ($8.88/yr) | Not yet registered |
| LLM API costs (development) | ~$5–50/mo | Variable |
| GitHub org | $0 | Free tier sufficient |
| **Total (operational)** | **~$102–147/mo** | |

### At Scale

| Scale | Additional Infra | Estimated Monthly |
|-------|-----------------|-------------------|
| 10 workers | Marginal cost is API usage | ~$150–250/mo total |
| 50 workers | May need second server or cloud overflow | ~$300–400/mo total |
| 100 workers | Second dedicated server, load balancing | ~$400–500/mo total |
| 1,000 workers | Multi-server cluster, dedicated ops | Architecture decision needed |

The infrastructure economics are favorable because:
- Email is flat-rate (Migadu doesn't charge per mailbox)
- Compute is shared (workers don't need dedicated VMs)
- The expensive input (LLM tokens) is passed through to the customer in the subscription price
- Identity infrastructure (AID Protocol) runs on the existing server with negligible overhead

---

## 10. The Ask

### Immediate Needs (< $500)

| Need | Cost | Why |
|------|------|-----|
| Unsuspend HostRound server | ~$67 | Development and deployment infrastructure |
| Register `eidosagi.com` | ~$9 | Brand, email, identity |
| Activate Migadu | ~$29 | Worker email identities |
| **Total to resume operations** | **~$105** | |

### Near-Term Needs (3–6 months)

| Need | What It Buys |
|------|-------------|
| Incorporation | Legal entity, IP assignment, equity formalization |
| Operating capital (~$1,000–2,000) | 6 months of infrastructure at current burn rate |
| Time | Both founders building toward Phase 3 (first paying customer) |

### Optional: Seed Funding

Not required, but would accelerate:
- Hiring (frontend engineer for Hancock dashboard)
- Compute (more server capacity for running multiple workers)
- Go-to-market (first 10 customers, not just first 1)

**Target timeline to first revenue: 3–6 months from incorporation.**

The product doesn't need a large team or millions in capital to reach first revenue. It needs a legal entity, basic infrastructure, and time to finish the build.

---

## Summary

| | |
|---|---|
| **What** | Digital workers with real identity — AI agents that onboard like employees, with scoped access, human supervision, and audit trails |
| **Why now** | NIST standards, CrowdStrike's $1B validation, IETF OAuth extensions — the market just arrived |
| **Moat** | Purpose-built identity infrastructure (AID Protocol), not retrofitted human IAM |
| **Team** | Two builders, shipping daily, 5 active repos |
| **Status** | Working prototype (Core), production-ready auth framework (AID Protocol), designed dashboard (Hancock) |
| **Model** | $50–100/mo per worker, 50–90% gross margins |
| **Ask** | Incorporation + ~$105 to resume operations + 3–6 months to first customer |

---

*This document is the founding alignment for Team Eidos. It serves as both internal north star and backbone for external conversations (investor deck, customer pitches, partnership discussions). Open decisions (company name, entity type, equity split) are flagged above and should be resolved before any external-facing activity.*

> **References**
> - `decisions/2026-02-26-north-star-universal-computer-control.md`
> - `decisions/2026-02-26-hancock-agent-auth-protocol.md`
> - `briefs/2026-02-25-hancock-auth-research.md`
> - `briefs/2026-02-25-hancock-approval-dashboard-design.md`
> - `briefs/2026-02-28-hancock-review.md`
> - `briefs/2026-02-28-clicking-is-not-configuring.md`
> - `knowledge/aid-protocol-summary.md`
