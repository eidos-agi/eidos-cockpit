---
id: TASK-38
title: Fix Helios button-clicking reliability
status: To Do
assignee: []
created_date: '2026-03-01 07:24'
labels:
  - helios
  - phase-0
  - critical-path
milestone: m-7
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Helios has intermittent failures clicking buttons on real websites (Migadu admin, Cloudflare dashboard). Fix reliability issues: CSP violations, timing/element detection, site learning for known sites. This is the critical path — if Helios isn't reliable, Phase 1 can't work.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Agent can navigate Migadu admin and perform account operations reliably
- [ ] #2 Agent can navigate Cloudflare dashboard and modify DNS settings reliably
- [ ] #3 Site learning captures patterns for known sites to avoid repeated failures
- [ ] #4 CSP issues resolved or worked around
<!-- AC:END -->
