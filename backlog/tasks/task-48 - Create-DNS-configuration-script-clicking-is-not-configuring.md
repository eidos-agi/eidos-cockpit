---
id: TASK-48
title: Create DNS configuration script (clicking-is-not-configuring)
status: To Do
assignee: []
created_date: '2026-03-01 07:34'
labels:
  - infrastructure
  - phase-0
  - iac
milestone: m-7
dependencies: []
priority: medium
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Cloudflare DNS and Migadu were configured via dashboard clicks. No Infrastructure-as-Code exists. If settings get wiped (the exact Sable scenario from briefs/2026-02-28-clicking-is-not-configuring.md), restoration is manual.

Create a setup-dns.sh or Terraform config that can recreate all Cloudflare DNS records for eidosagi.com programmatically using the Cloudflare API.

Records to codify: MX (2), TXT (SPF, DMARC, Migadu verify), CNAME (3x DKIM, autoconfig).
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Script or Terraform config exists in eidos-infra repo
- [ ] #2 Running it recreates all DNS records for eidosagi.com
- [ ] #3 Documented in company-setup-log.md
<!-- AC:END -->
