---
id: DRAFT-2
title: Eidos-as-Remote-Employee — Server-Resident Agent
status: Draft
assignee: []
created_date: '2026-02-26 05:18'
updated_date: '2026-02-26 05:33'
labels:
  - product-vision
  - north-star
  - agent-identity
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
## Origin

Brainstorm between Daniel & Vybhav (Feb 2026). "The reason this could be worth millions is if it can truly live on a server and be like a remote employee."

## Vision

An AI agent that is indistinguishable from a remote employee in how it interfaces with a company:

- **Has its own identity**: voice, email, phone number, Slack ID
- **Onboards like a human**: receives company docs, signs them, sets up accounts
- **Scoped access**: limited RBAC in existing company systems (blast radius containment)
- **Completes real work**: not just chatbot responses — full mission completion
- **Proactive**: asks for things it needs, surfaces blockers, doesn't wait to be poked
- **Interruptible**: humans can call/email/Slack it like a real person

## Why "AI Agent in Your Website" Fails

"The 'AI Agent in your website' thing people are doing is destined to fail because it will never have full context to complete a real mission."

The key insight: **context completeness** is the differentiator. A widget doesn't have full context. A remote employee with system access does.

## Proof-of-Concept Test

"If we could prove that it could set up its own accounts as literally a new remote employee in our fake company, holy crap that would be real."

Proposed test: Eidos onboards itself at a fake company — receives docs, signs them, creates accounts, joins Slack, completes first-day tasks.

## Blockers

- **2FA** — "2FA will kill it" → need Hancock middleware
- **Memory compression** — needs TASK-28 to maintain full context across long missions
- **Agent auth** — needs Hancock for human approval of sensitive actions

## Dependencies

- TASK-28 (Memory Compression & Sliding Context Window)
- Hancock (Agent Auth & Approval Protocol)
<!-- SECTION:DESCRIPTION:END -->

## Implementation Notes

<!-- SECTION:NOTES:BEGIN -->
## Research Findings (Feb 2026)

### Competitive Landscape
| Product | Positioning | Key Insight |
|---------|------------|-------------|
| Devin (Cognition) | AI software engineer, "Employee #1" at Goldman Sachs | 67% PR merge rate, plans for "hundreds to thousands" of Devins. Coding only. |
| Manus AI | General-purpose autonomous agent in cloud VM | Powerful but stateless between sessions. Not an employee. |
| Dust.tt | Company-grade assistants with synthetic filesystems | $7.3M ARR, SOC 2 / GDPR / HIPAA. Closest to "knows your company." |
| Lindy AI | No-code AI employees for departments | 70% weekly adoption at 3000+ employee orgs. Workflow automation. |
| OpenAI Operator | Browser-based computer-using agent | Secure virtual browser, not persistent. |
| Anthropic Cowork/Tasks | Delegated multi-step work | Moving toward operational automation. Not an employee. |

**Gap: No product bundles email + phone + Slack + company tools + persistent context + proactive behavior into a single agent identity.**

### Communication Stack (Available Today)
- **Email**: AgentMail (YC S25, backed by Paul Graham/General Catalyst) — API-first, inboxes in milliseconds, SPF/DKIM/DMARC auto-configured
- **Phone**: Retell AI (~600ms latency, $0.07/min), Bland AI (~800ms, high-volume), Vapi (~700ms, developer-focused)
- **Slack**: Bot API for presence — but no standard way to give agents full workspace member identity. Gap.
- **Afterdraft**: Professional email addresses for agents in 5 minutes

### Agent Onboarding (Uncharted Territory)
- AI agents are used to ASSIST with onboarding (Moveworks, Rezolve.ai, Beam.ai)
- **Nobody has tested having an agent GO THROUGH onboarding as the onboardee**
- DocuSign AI contract agents analyze and flag risks but DO NOT sign on behalf of AI
- **Legal reality**: AI cannot sign contracts. Agent "signature" attributed to deploying entity.
- n8n and similar can auto-provision Jira, Notion, GitHub, Okta access

### Legal Framework
- **Current classification**: AI agents are legally TOOLS, not employees or contractors
- **Liability**: Employers fully responsible for AI tool outcomes (EEOC guidance)
- **2026 state laws**: Illinois (Jan 1), Colorado CAIA (Feb 1), Texas TRAIGA (Jan 1) — regulate AI in employment but treat it as a tool
- **EU AI Act Article 14**: Enforcement August 2, 2026 — must prove every AI action was authorized at execution time
- **Estonia** exploring limited legal status for AI; DAOs experimenting with smart contract autonomy
- **Positioning**: Agent's "employee-like" behavior is a UX pattern, not a legal status

### Context Completeness (The Real Moat)
Why chatbots fail (Composio 2025 report):
1. "Dumb RAG" — dumps everything into context instead of curating
2. "Brittle Connectors" — integrations work in demo, fail in production
3. "Polling Tax" — no event-driven architecture, miss real-time updates

What a remote employee has that a chatbot doesn't:
- Persistent organizational context (team structure, reporting lines)
- Ambient information (Slack channels, meeting notes, overheard conversations)
- Institutional memory (past decisions, why things were done)
- Relationship context (who to ask for what)
- Tool fluency (direct read/WRITE access, not just read)

**RAG vs full system access: Full access wins.** Dust.tt's synthetic filesystem is closest (map all sources into navigable Unix-like structures).

### RBAC for Agents
- **RBAC fails** for agents because role isn't predictable until after reasoning (Oso research)
- **PBAC (Policy-Based Access Control)** is the answer — evaluate each action at runtime
- Okta "AI Agent Identity" platform, Auth0 agent security, Oso's Polar policy engine
- **Circuit breaker model**: throttle, downgrade permissions, or quarantine with immediate effect, no code changes
- Real incidents: AI deleted production DB, purchased items without authorization, moved files irretrievably
- >80% of companies use AI agents but <50% have governance (mid-2025)

### Proactive Agent Patterns
- **Confidence-threshold model**: autonomous by default, escalate when confidence < 85%
- Target: 85-90% autonomous, 10-15% escalation rate
- Fallback triggers human intervention in <500ms
- Three tiers: (1) scheduled cadence, (2) event-driven response, (3) pattern-detected initiative with approval gates

### Key Recommendations
1. **Communication stack**: AgentMail + Retell AI + Slack Bot API — all available today
2. **Legal**: Operate as tool/software. Binding actions flow through human approval gates (Hancock). Position as feature.
3. **Context**: Build persistent organizational knowledge graph, not just RAG. Event-driven updates.
4. **Security**: PBAC via Oso from day one. Circuit breakers in core architecture. Full logging for EU AI Act.
5. **Proactive behavior**: Start with scheduled cadence → event-driven → truly proactive (progressive trust)
6. **Onboarding demo**: Agent onboards itself at a fake company. Powerful marketing differentiator.
7. **Multi-instance architecture**: Goldman "1000 Devins" insight — design for many agents, shared context, specialized skills

### Sources
60+ sources catalogued — product reviews, legal analyses, security frameworks, market reports. Full list in research agent output.

## Core Architectural Principle: Universal Computer Control

**The agent must work with ANY website, ANY terminal, ANY interface — not through APIs, but through the same screen-and-keyboard interface a human uses.**

This is the fundamental distinction between:
- **Bot/Automation** (API-first): Only works with pre-integrated services. "Supported integrations" list. Breaks when UIs change. Limited to what's been built.
- **Full Agent** (Computer Use): Works with ANYTHING a human can interact with. The screen IS the universal API. No integration list needed.

A real remote employee doesn't ask "do you have a Zapier integration?" — they open the browser and use it.

### What This Means Technically
- Anthropic Computer Use / OpenAI CUA-level screen control as the PRIMARY interaction mode
- Terminal/shell access for CLI workflows
- Browser automation that handles real-world complexity (CAPTCHAs, 2FA prompts, dynamic UIs, pop-ups)
- NOT dependent on pre-built API connectors — those are optimizations, not requirements
- Token burn is an acceptable cost for universal capability

### The Test
Give Eidos a task involving a website it has never seen before. If it can navigate, understand, and complete the task through the UI — that's a full agent. If it needs a pre-built integration, it's just a bot.

### Prior Art
- Anthropic Computer Use: Direct desktop control via screenshots + cursor
- OpenAI Operator/CUA: Browser-based computer-using agent in secure VM
- Skyvern: AI-powered browser automation that adapts to any UI
- Manus: Full VM sandbox with browser + terminal
- Claude in Chrome (Helios): Browser automation via extension

### Architecture Implication
APIs should be used as OPTIMIZATIONS (faster, cheaper, more reliable for known services) but the agent must ALWAYS be able to fall back to screen-level interaction for unknown/unsupported services. This is the "universal fallback" principle.
<!-- SECTION:NOTES:END -->
