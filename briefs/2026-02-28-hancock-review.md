# Hancock — Thoughts on Your Research

**Date** 2026-02-28
**From** Daniel
**References** `briefs/2026-02-25-hancock-auth-research.md`, `briefs/2026-02-25-hancock-approval-dashboard-design.md`, `knowledge/aid-protocol-summary.md`, `knowledge/aid-protocol-license-analysis.md`

---

## Short Version

Read all four docs. I think this is great and should work. The AID Protocol being purpose-built for Eidos is the key insight — it means Hancock isn't an integration project, it's a completion project. The dashboard design is detailed enough to build from. The capability namespace feels right. I'm bought in.

## What Got Me Excited

- **AID Protocol already exists and is tested** — 8 packages, 53 tests, zero deps. We're not starting from scratch. The fact that it was built for us means there's no impedance mismatch to fight.
- **SAP is exactly the right pattern** — SUR/SUA/SUT maps perfectly to the approval dashboard. A human gets a request, approves or denies it, and Eidos gets a scoped temporary token. That's the whole product.
- **The dashboard design is real** — 4 tabs, component specs, risk assessment logic, notification formats, API contracts. Someone could start building this next week.
- **Autonomy levels** — the 0-3 slider is a clean UX for something that could be very complicated. "How much do you trust Eidos?" as a single dial.

## Proposal: Build the Hancock Frontend

You flagged three open questions — key storage, DID method, cross-platform DTs. All real. But I think we should build the frontend first and let those answers emerge from using it.

**Next step: build the Hancock dashboard as a live app with mock data.** React + Next.js + Tailwind + shadcn/ui, exactly the stack you spec'd. Your wireframes are detailed enough to build from directly. Deploy it somewhere we can both use it.

### What to build

**All four tabs from your design:**
- **Pending** — mock SURs streaming in on a timer (financial, terminal, screen control). Approve/deny/modify buttons that actually update state. Risk assessment badges.
- **Activity** — pre-populated with a realistic day of fake Eidos activity. Filterable by category, searchable, with the status indicators you designed (approved/denied/modified/auto-approved).
- **Delegations** — 3-4 mock DTs with permissions grids, spend tracking bars, and revoke buttons that work.
- **Settings** — autonomy slider that actually changes which mock SURs get auto-approved. Financial limit inputs. Approval trigger checkboxes. The emergency controls.

**Mock data that feels real:**
- Fake SURs that match your examples — "$127.50 to Stripe," "kubectl delete namespace staging," "new domain: internal.admin.example.com"
- Realistic timing — some urgent, some stale, some auto-approved
- Spend tracking that accumulates as you approve financial actions
- Notifications (browser push if possible, or at least toasts)

**SSE or WebSocket for real-time** — mock SURs should appear live, not on page refresh. The whole point is to feel what it's like when Eidos asks for permission mid-mission.

### What this teaches us

Once we're clicking through real approval flows with realistic mock data, a few things become obvious that aren't obvious on paper:
- Whether the approval UX is fast enough or too much friction
- Whether autonomy levels 0-3 are the right granularity
- Whether the activity feed is actually useful or just noise
- What approval types we're missing
- How the dashboard feels on mobile (your responsive spec gets tested for real)

The three open questions (key storage, DID method, cross-platform DTs) don't block this at all — mock data sidesteps them. And by the time we've used the frontend for a week, we'll have much stronger opinions on each one.

Then we plug in a real agent and see what changes.

## One Ask

Push the AID Protocol repo to the `eidos-agi` GitHub org when you get a chance — I want to read the code, and we'll need it there to build against.

---

*Daniel, Feb 28, 2026.*
