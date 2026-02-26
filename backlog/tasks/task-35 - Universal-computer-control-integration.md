---
id: TASK-35
title: Universal computer control — screen-and-keyboard as primary interaction
status: To Do
assignee: []
created_date: '2026-02-26'
labels:
  - north-star
  - architecture
  - computer-use
milestone: m-5
dependencies: []
priority: high
---

## Description

The North Star: Eidos works with ANY website, ANY terminal, ANY interface through screen-and-keyboard control. APIs are optimizations, not requirements.

### What to Build
- Integration with Anthropic Computer Use or equivalent screen control capability
- Browser automation layer that handles real-world complexity (CAPTCHAs, dynamic UIs, pop-ups, 2FA prompts)
- Terminal/shell access for CLI workflows
- Fallback chain: API (fast, cheap) -> Screen control (universal)
- Task routing: orchestrator decides API vs screen control per action

### The Test
Give Eidos a task involving a website it has never seen before. If it can navigate, understand, and complete the task through the UI — it's a full agent. If it needs a pre-built integration, it's just a bot.

### Research to Reference
- Anthropic Computer Use documentation
- OpenAI Operator/CUA architecture
- Skyvern (AI browser automation)
- Helios / Claude in Chrome (browser extension control)
- Archived DRAFT-2 (Eidos-as-Remote-Employee)

## Acceptance Criteria

- [ ] #1 Eidos can navigate an unfamiliar website and complete a simple task via screen control
- [ ] #2 Orchestrator routes tasks to screen control when no API integration exists
- [ ] #3 API integrations used as fast-path when available, screen control as fallback
- [ ] #4 Terminal access works for CLI-based workflows
- [ ] #5 End-to-end demo: complete a multi-step task on a never-before-seen website
