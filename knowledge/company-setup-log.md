# Eidos AGI — Company Setup Execution Log

> Living log of infrastructure and company setup actions. Updated as work completes.

## Completed

### 1. Company Plan Written (2026-02-28)
- **Doc:** `briefs/2026-02-28-company-plan.md`
- 10-section founding document: Why Now, What We're Building, The Moat, Team, What Exists, Company Structure, Business Model, Roadmap, Infrastructure & Costs, The Ask
- Serves as internal alignment doc and investor deck backbone

### 2. Founding Agreement Recorded (2026-02-28)
- **Doc:** `decisions/2026-02-28-founding-agreement.md`
- Daniel Shanklin + Vybhav Reddy KC, 50/50 equity
- Entity type TBD (Delaware C-Corp if fundraising, LLC if bootstrapping)

### 3. Founder Profiles Documented (2026-02-28)
- **Doc:** `knowledge/founder-profiles.md`
- Daniel: Dir. AI & Technology at AIC Holdings, full-stack builder
- Vybhav: Sr Mgr Data Science at Socure, AID Protocol creator

### 4. Email & Domain Infrastructure Decision (2026-02-28)
- **Doc:** `decisions/2026-02-28-email-and-domain-infrastructure.md`
- Cloudflare Registrar for domain, Migadu Standard for email, Claude Code as email client

### 5. Contacts/Rolodex Repo Created (2026-02-28)
- **Repo:** `eidos-agi/eidos-rolodex` (private)
- 8 contact files, 1 org file, CONNECTIONS.md graph
- Covers: Daniel, Vybhav, Ziggy Shanklin, Greg Bird, Laura Bird, Collin Bird, William Holloway, EJ Bartolomei, Jason Bartolomei, Will Radcliffe

### 6. Cloudflare Account Created (2026-02-28)
- **Account email:** danielshanklin@gmail.com
- **Account ID:** 3c1d42c77978e6af0e458b6f1130c01b
- Email verified (required manual verification — registrar had separate verify-email gate)

### 7. Domain Registered: eidosagi.com (2026-02-28)
- **Registrar:** Cloudflare
- **Cost:** $10.46/yr (renews at $10.46)
- **Expires:** Feb 28, 2027
- **Nameservers:** Cloudflare (automatic)
- **Registrant:** Daniel Shanklin / Eidos AGI
- **WHOIS privacy:** Cloudflare redacts registrant info from public WHOIS

### 8. Migadu Account Created (2026-03-01)
- **Account email:** danielshanklin@gmail.com
- **Organization:** Eidos AGI
- **Admin URL:** https://admin.migadu.com
- **Plan:** Free trial (ending March 15, 2026 — upgrade to Standard $29/mo before then)
- Email confirmed, account active

### 9. Domain Added to Migadu: eidosagi.com (2026-03-01)
- **Domain ID:** 199606
- **DNS mode:** External nameservers (Cloudflare)
- **Default addresses created:** admin, postmaster, abuse
- **Domain verified active:** 2026-03-01T06:46:21Z

### 10. DNS Records Configured on Cloudflare (2026-03-01)
All records added to Cloudflare DNS for `eidosagi.com`:
- **TXT** `@` → `hosted-email-verify=p4tfyxmq` (Migadu verification)
- **MX** `@` → `aspmx1.migadu.com` priority 10
- **MX** `@` → `aspmx2.migadu.com` priority 20
- **CNAME** `key1._domainkey` → `key1.eidosagi.com._domainkey.migadu.com` (DKIM)
- **CNAME** `key2._domainkey` → `key2.eidosagi.com._domainkey.migadu.com` (DKIM)
- **CNAME** `key3._domainkey` → `key3.eidosagi.com._domainkey.migadu.com` (DKIM)
- **TXT** `@` → `v=spf1 include:spf.migadu.com -all` (SPF)
- **TXT** `_dmarc` → `v=DMARC1; p=quarantine;` (DMARC)
- **CNAME** `autoconfig` → `autoconfig.migadu.com` (email client autodiscovery)
- All CNAME records set to DNS-only (not proxied)

### 11. Founder Mailbox Created: daniel@eidosagi.com (2026-03-01)
- **Mailbox:** daniel@eidosagi.com
- **Status:** Activated
- **Connection Details:**
  - **IMAP:** `imap.migadu.com:993` (TLS)
  - **SMTP:** `smtp.migadu.com:465` (TLS)
  - **Webmail:** https://webmail.migadu.com
  - **ManageSieve:** `imap.migadu.com:4190` (STARTTLS)
  - **POP3:** `imap.migadu.com:995` (TLS)
  - **Username:** `daniel@eidosagi.com`
- `vybhav@eidosagi.com` — TBD when Vybhav is ready

## Remaining

### 12. Upgrade Migadu to Standard ($29/mo)
- Free trial expires March 15, 2026
- Upgrade before sending real email (free tier has limits)

### 13. Create Vybhav's Mailbox
- `vybhav@eidosagi.com`

### 14. Verify Email Works
- Send test email from daniel@eidosagi.com → danielshanklin@gmail.com
- Send test email from external → daniel@eidosagi.com
- Verify SPF/DKIM/DMARC pass (check headers)

### 15. Agent Email Setup (Day 2)
- Provision `eidos@eidosagi.com` via Migadu REST API
- Configure IMAP read + SMTP send tooling for Claude Code
- Test: Claude Code sends and reads email as an agent

### 16. Shared Secrets System for Founders
- Need AI-friendly password/secrets sharing between Daniel & Vybhav
- Most password managers (1Password, Bitwarden) are not AI-agent friendly
- Evaluate: Knox, Doppler, Infisical, or custom encrypted vault

## Costs So Far

| Item | Cost | Status |
|------|------|--------|
| `eidosagi.com` (Cloudflare) | $10.46/yr | Paid |
| Migadu Standard | $29/mo | Not yet (free trial until March 15) |
| Cloudflare DNS | Free | Active |
| **Running total** | ~$10.46 one-time | |
