---
id: TASK-36
title: Agent identity stack — email, phone, Slack, persistent presence
status: To Do
assignee: []
created_date: '2026-02-26'
labels:
  - north-star
  - agent-identity
  - infrastructure
milestone: m-5
dependencies: [TASK-34]
priority: medium
---

## Description

A full agent has its own identity. Build the communication stack so Eidos can be reached and can reach out like a remote employee.

### Communication Channels
- **Email**: AgentMail or similar — dedicated inbox, SPF/DKIM/DMARC auto-configured
- **Phone**: Retell AI or similar — voice calls with <1s latency
- **Slack**: Bot API with full workspace member identity
- **Persistent presence**: Eidos is always "online" — responds to messages, joins meetings, surfaces blockers proactively

### Proactive Behavior
- Confidence-threshold model: autonomous by default, escalate when confidence < 85%
- Scheduled cadence (check-ins) -> Event-driven (respond to messages) -> Pattern-detected (proactive alerts)
- Progressive trust: start conservative, earn autonomy

### Dependencies
- Hancock (TASK-34) needed for authorization of outbound actions
- Memory compression (TASK-28) needed for persistent context across conversations

### Research to Reference
- AgentMail (YC S25), Afterdraft
- Retell AI, Bland AI, Vapi (voice)
- Archived DRAFT-2 (communication stack section)

## Acceptance Criteria

- [ ] #1 Eidos has a dedicated email address it can send/receive from
- [ ] #2 Eidos can make and receive phone calls
- [ ] #3 Eidos has Slack presence (can be messaged, responds)
- [ ] #4 Proactive behavior: surfaces blockers and status updates without being asked
- [ ] #5 All outbound actions gated through Hancock approval flow
