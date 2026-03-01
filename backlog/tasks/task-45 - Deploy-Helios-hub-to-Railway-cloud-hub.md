---
id: TASK-45
title: Deploy Helios hub to Railway (cloud hub)
status: To Do
assignee: []
created_date: '2026-03-01 07:34'
labels:
  - helios
  - phase-0
  - critical-path
  - railway
milestone: m-7
dependencies: []
priority: high
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Helios hub currently runs on localhost:9333 as a launchd service on Daniel's Mac. It dies when the laptop sleeps. This blocks autonomous operation.

Work already done: db.ts written (585 lines, multi-tenant RLS, Postgres). Not yet wired in.

Remaining work:
1. Provision Postgres on Railway in Helios project
2. Wire hub's 38 filesystem operations to Postgres via db.ts
3. Dockerize the hub daemon
4. Deploy to Railway
5. Build local agent connector (Mac's Chrome → cloud hub)

This is the critical path for Phase 0. Without it, "autonomous agent" is fiction.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Postgres instance running on Railway
- [ ] #2 Hub filesystem operations converted to Postgres queries
- [ ] #3 Hub runs in Docker container on Railway
- [ ] #4 Local Chrome connects to cloud hub
- [ ] #5 Hub survives laptop sleep — operates 24/7
<!-- AC:END -->
