# Decision: Email & Domain Infrastructure

**Date:** 2026-02-28
**Pilots:** Daniel
**Status:** Accepted

## Context

Eidos needs a domain and email that works for founders today and agents in two days. The email system must be:
- Reliable (critical infrastructure)
- API-automatable (agent mailbox provisioning via REST)
- Not coupled to a human-first service that's hard to automate later
- Usable by Claude Code as the founder email client (dogfooding the agent workflow)

## Decisions

### Domain: Cloudflare Registrar
- Register `eidosagi.com` at Cloudflare — $10.44/yr, at-cost
- Cloudflare DNS API for all record management
- DNSSEC enabled
- Free DMARC reporting dashboard

### Email: Migadu Standard ($29/mo)
- Unlimited mailboxes, flat rate
- REST API for mailbox provisioning (one POST = new agent inbox)
- Standard IMAP for reading, SMTP for sending
- No per-mailbox cost — 2 founders and 50 agents cost the same
- Running since 2013, boring reliable infrastructure

### Email Client: Claude Code
- Founders interact with email through Claude Code using MCP tools
- IMAP/SMTP via tooling, or browser automation against Migadu webmail
- No Gmail. No Outlook app. The AI agent IS the email client.
- Dogfooding: if the founder email experience is painful, fix the tooling — that tooling becomes the product

## Why Not Alternatives

| Option | Rejected Because |
|--------|-----------------|
| Google Workspace | No mailbox provisioning API. $7/seat scales badly for agents. Would need to rebuild in 2 days anyway. |
| AgentMail (YC W25) | Purpose-built but brand new startup. Email is critical infra — don't depend on a company that might not exist in 12 months. |
| Amazon SES + S3 | Cheapest at scale but requires building entire inbox layer. Weeks of work before a working email address. |
| Self-hosted Postfix | Deliverability from fresh IP is painful for months. First investor emails land in spam. |
| Fastmail | No user provisioning API. Can't automate agent mailbox creation. |

## What Gets Built

### Day 1: Foundation
- Register `eidosagi.com` on Cloudflare
- Sign up Migadu Standard ($29/mo)
- Configure DNS: MX, SPF, DKIM, DMARC records on Cloudflare
- Create founder mailboxes: `daniel@eidosagi.com`, `vybhav@eidosagi.com`
- Verify email works (send/receive test)

### Day 2: Agent Email
- Provision agent mailbox via Migadu REST API: `eidos@eidosagi.com`
- Build/configure IMAP read + SMTP send tooling for Claude Code
- Test: Claude Code sends and reads email as an agent

### Later: Scale
- Add SES for outbound volume when Migadu's daily limits become a constraint
- Agent provisioning becomes part of the worker deployment pipeline
- Each new digital worker gets an email identity automatically

## Costs

| Item | Cost |
|------|------|
| `eidosagi.com` domain (Cloudflare) | $10.44/yr |
| Migadu Standard | $29/mo |
| Cloudflare DNS | Free |
| **Total** | ~$30/mo |

## Consequences

- Founders use AI-native email workflow from day one
- Every mailbox (human or agent) is created the same way (API call)
- No vendor lock-in on the reading side (standard IMAP)
- If Migadu outbound limits become a constraint, add SES for sending without changing anything else
- The email experience for founders IS the agent email experience — pain drives product improvement
