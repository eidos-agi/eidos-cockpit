---
id: TASK-39
title: Verify email works end-to-end (SPF/DKIM/DMARC)
status: To Do
assignee: []
created_date: '2026-03-01 07:24'
labels:
  - email
  - phase-0
  - infrastructure
milestone: m-7
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Send test emails from daniel@eidosagi.com, verify delivery, and confirm SPF/DKIM/DMARC pass in received headers. Email deliverability is a prerequisite for Phase 1 (agent will send emails during incorporation).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Send test email from daniel@eidosagi.com to external address
- [ ] #2 Received headers show SPF=pass, DKIM=pass, DMARC=pass
- [ ] #3 Reply works (send and receive confirmed)
- [ ] #4 No spam folder delivery
<!-- AC:END -->
