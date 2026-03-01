# Holes in the Plan

**Date:** 2026-03-01
**Author:** Daniel Shanklin (via cockpit analysis)
**Status:** Action items identified — some fixed inline, others require decisions

---

## Critical: The Agent Doesn't Exist Yet

The build sequence says "after Phase 0, the agent takes over." But there is no persistent agent. What exists today is Claude Code sessions that end when Daniel closes the terminal.

There's no running process that persists across sessions. No orchestrator routing tasks. No memory carrying context between sessions. No autonomous task queue.

Phase 1 says "agent incorporates the company" — but who kicks off each step? Daniel opens Claude Code, says "do the next thing," closes it. That's human-driven with AI assistance, not an agent taking over.

**The missing piece:** Between Phase 0 and Phase 1, something needs to exist that *is* the agent — a loop, a daemon, a scheduler. eidos-v5 (the brain) doesn't appear anywhere in the build sequence. The three pillars are brain + auth + body, but the plan only builds body (Helios) and auth (Hancock). The brain is absent.

**Options:**
1. Accept that "the agent" in Phase 1 is Claude Code sessions orchestrated by Daniel — honest but weaker story
2. Build a minimal persistent agent loop (cron/daemon that pulls from a task queue, runs Claude Code, logs results) — stronger story, adds Phase 0 scope
3. Use eidos-v5's orchestrator as the agent backbone — most aligned with architecture, but eidos-v5 maturity is unclear

**Recommendation:** Option 2. A thin agent loop (task queue -> Claude Code -> audit log) is a weekend of work and transforms the narrative from "human uses AI tool" to "agent operates autonomously with human checkpoints."

---

## Critical: Helios Dies When the Laptop Sleeps

The hub runs on `localhost:9333` as a launchd service on Daniel's Mac. Phase 0 success criteria says "agent can operate without human intervention" — but the agent's body requires Daniel's laptop to be open and awake.

The cloud hub work was started (Postgres db.ts written, 585 lines) but is not wired in. 38 filesystem operations need converting. No Dockerfile exists. No local-to-cloud connector exists. None of this is in the Phase 0 task list.

**Fixed:** Added "Deploy Helios hub to Railway" as a Phase 0 task (TASK-45).

---

## Phase 0 Duration is Aspirational

Phase 0 lists: Helios reliability fixes, email verification, Migadu upgrade, two new mailboxes, shared secrets system, server unsuspension. Plus the cloud hub deployment above. Plus the agent loop above.

Helios button-clicking reliability alone is days of debugging. The shared secrets system requires evaluation of three options, then deployment. Cloud hub deployment is a multi-day effort.

**Fixed:** Build sequence updated from "1-2 days" to "1-2 weeks" with explicit note that the original estimate was too aggressive.

---

## Phase 1 Targets the Hardest Websites

Stripe Atlas, IRS.gov, Mercury/Brex — specifically designed to resist automation. Government sites have CAPTCHAs and legacy UIs. Financial services have KYC, document upload, multi-step identity verification. Zero site knowledge exists for any of these.

The plan's fallback ("human-assisted with agent prep work") means Phase 1 isn't actually agent-driven if these sites resist.

**Recommendation:** Add a "graduated difficulty" step to Phase 0 — prove Helios on progressively harder sites before attempting financial/legal flows. Start with simple form fills, escalate to multi-step flows, then attempt Stripe Atlas. If Stripe Atlas is too hard, use it as the human-assisted fallback and document the gap honestly.

---

## Phase 2 Fiverr Assumptions are Fragile

- Fiverr ToS likely prohibits automated account operation
- New sellers have near-zero discoverability
- Customer communication and dispute resolution are complex
- Aggressive anti-bot detection

**Recommendation:** Evaluate alternatives before committing to Fiverr. Upwork has different ToS. Direct outreach (agent emails potential clients) is more aligned with the product vision. Internal tasks-for-pay (agent does real work for Daniel's network) avoids platform risk entirely.

---

## Company Plan Has Stale Data

Three places still say domain is "available, $8.88/yr on Porkbun" — it's registered on Cloudflare at $10.46/yr. Infrastructure section says domain not registered and email not activated — both are done.

**Fixed:** Updated Sections 5, 6, and 9 of company plan to reflect reality.

---

## SAFE Notes Error in Build Sequence

Phase 1 says "Create founder equity agreements (SAFE notes, stock purchase agreements)." SAFE notes are for investors, not founders. Founders get restricted stock agreements.

**Fixed:** Corrected to "restricted stock purchase agreements" in the build sequence.

---

## state.json is Stale

- `cockpit.name: "eidos-cockpit"` — should be `cockpit-eidos`
- `fleet` missing 5 repos (helios, cockpit-daniel, cockpit-vybhav, eidos-rolodex, eidos-coo)
- `pilots.*.git_emails` reference `eidos-agi.com` — actual domain is `eidosagi.com`
- Template references dead Rhea org

**Fixed:** state.json updated to match reality.

---

## Vybhav Engagement and IP Assignment

0 cockpit sessions. 1 commit total. AID Protocol lives in Vybhav's personal workspace, not in the eidos-agi org. No IP assignment agreement exists. No vesting schedule decided. 50/50 equity with heavily asymmetric contribution.

**Not a judgment — a structural risk.** If the entity forms without IP assignment and vesting, the cap table is a liability.

**Action needed (human-gated):** Daniel and Vybhav need to discuss:
1. Move AID Protocol repo to eidos-agi org
2. IP assignment agreement (all code assigns to entity)
3. Vesting schedule (standard 4yr/1yr cliff protects both founders)

---

## "Clicking Is Not Configuring" Violation

Cloudflare DNS and Migadu were configured via dashboard clicks. No Infrastructure-as-Code exists. The DNS records are documented in markdown but not executable. If settings get wiped (the exact scenario the brief describes), restoration is manual.

**Action needed:** Create a DNS configuration file (Terraform or even a shell script) that can recreate all Cloudflare DNS records programmatically. Doesn't need to be Terraform — even a `setup-dns.sh` using the Cloudflare API is better than nothing.

---

## No Evidence Capture for the Demo

"The founding story IS the product demo" — but there's no plan to record it:
- No video/screen recording setup
- Helios audit trail only persists locally (dies with laptop)
- No structured logging of agent actions to cloud
- No before/after evidence chain

**Action needed:** Before Phase 1 begins, set up:
1. Helios audit log persistence to cloud (part of cloud hub migration)
2. Screen recording for key moments (GIF creator in Helios, or OBS)
3. Git-based evidence trail (agent commits its work products to a repo)

---

## Summary

| Issue | Severity | Status |
|-------|----------|--------|
| Agent doesn't exist (no persistent loop) | Critical | Backlog task created |
| Helios can't run autonomously | Critical | Backlog task created |
| Phase 0 duration underestimated | High | Fixed in build sequence |
| Phase 1 targets hardest sites | High | Recommendation added |
| Fiverr assumptions fragile | Medium | Recommendation added |
| Company plan stale data | Medium | Fixed |
| SAFE notes error | Low | Fixed |
| state.json stale | Medium | Fixed |
| Vybhav engagement + IP | High | Human-gated, needs discussion |
| Clicking-not-configuring violation | Medium | Backlog task created |
| No evidence capture plan | High | Backlog task created |
