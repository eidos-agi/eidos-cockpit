---
id: TASK-1
title: Migrate planning drafts from eidos-v5 to cockpit
status: To Do
assignee: []
created_date: '2026-02-26 06:00'
labels:
  - cockpit
  - housekeeping
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
DRAFT-1 (Hancock — Agent Auth & Approval Protocol) and DRAFT-2 (Eidos-as-Remote-Employee — Server-Resident Agent) currently live in eidos-v5's backlog/drafts/. They're planning artifacts, not engine code — they belong in the cockpit.

Move both drafts to the cockpit repo, either as backlog items or as briefs in `briefs/`. Remove the originals from eidos-v5 to avoid drift.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 DRAFT-1 (Hancock) content lives in eidos-cockpit
- [ ] #2 DRAFT-2 (Eidos-as-Employee) content lives in eidos-cockpit
- [ ] #3 Originals removed from eidos-v5/backlog/drafts/
- [ ] #4 No information lost in the migration
<!-- AC:END -->
