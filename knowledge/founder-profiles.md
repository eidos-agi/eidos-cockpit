# Founder Profiles — Eidos AGI

**Created** 2026-02-28
**Status** Active

---

## Daniel Shanklin — Co-Founder

**Location:** Boone, North Carolina
**LinkedIn:** [linkedin.com/in/danielshanklin](https://linkedin.com/in/danielshanklin)
**GitHub:** dshanklinbv / [github.com/eidos-agi](https://github.com/eidos-agi)

### Role at Eidos
Product vision, architecture, full-stack engineering, business operations. Built Eidos Core (PEFM memory, pod architecture, orchestrator), the operational cockpit, and the fleet-aware multi-pilot infrastructure. 56 commits across 5 repos in the last 7 days.

### Education
- **Texas State University** — BBA, Finance (2004–2007)
- **MIT** — Postgraduate studies in AI, Deep Learning, and Machine Learning
- **University of Texas** — Postgraduate studies in AI/ML

### Career Path
| Period | Company | Role |
|--------|---------|------|
| Early career | **Q Investments** (Fort Worth, TX) | Finance/trading operations — data and cash flow reconciliation on the swaps desk at a major hedge fund |
| 2018–2020 | **Ticket Tumble** | Data Consultant |
| 2021–2022 | **501 Data Solutions** | Data Management Consultant |
| 2022–2023 | **Howjadoo LLC** | Principal Data Consultant |
| 2023–2024 | **Strata Decision Technology** | Consultant |
| — | **AIC Holdings** (Fort Worth, TX) | Director, Artificial Intelligence — principally managed capital firm |
| — | **Boone Voyage Labs** (Boone, NC) | Founder / President / CTO — multi-strategy AI firm and incubator |
| — | **SparrowShield** | Co-founder — AI-driven property insurance underwriting (reduces 5–10 hour process to 30 seconds) |
| — | **Rhea AI Inc.** | Chairman & Chief Architect — AI platform |
| 2025–present | **Eidos AGI** | Co-Founder |

### Technical
- Proficient in 12 programming languages
- Full-stack development, cloud architecture, ML/DL engineering
- Patent holder: "Method for Controlling Remote System Settings Using Cloud-Based Control Platform" (USPTO Application No. 17/511,273)

### Notable
At age 7 (June 1991), piloted a Cessna 172 cross-country from San Diego, CA to Kill Devil Hills, NC — the site of the Wright Brothers' first flight. Nine-day trip. Youngest person documented to have piloted a flight across the United States.

---

## Vybhav Reddy KC — Co-Founder

**Full Name:** Vybhav Reddy Kammireddy Changalreddy
**Location:** India
**LinkedIn:** [linkedin.com/in/vybhavreddykc](https://linkedin.com/in/vybhavreddykc)
**Citizenship:** Non-US (relevant for incorporation via Stripe Atlas)

### Role at Eidos
Authentication protocols, security architecture, AID Protocol creator. Designed and built the entire AID Protocol from scratch — the authentication backbone for every digital worker Eidos deploys. 8 packages, 53 passing tests, zero external dependencies, Ed25519 cryptography.

### Education
- **JNTU Anantapur** — B.Tech, Computer Science
- **Bowling Green State University** — MS, Analytics

### Career Path
| Period | Company | Role |
|--------|---------|------|
| 10+ years | Multiple industries | Data science and engineering across retail, finance, banking, legal, e-commerce |
| Current | **Socure** | Senior Manager, Data Science (promoted Sep 2023) |
| Current | **Eidos AGI** | Co-Founder |

### Key Achievement at Socure
Led the overhaul of Socure's identity resolution system. Built an LSTM model with character-level word embeddings that achieved **96% recall and 98.4% precision** — setting new industry benchmarks for identity matching. The project generated **$5.35 million in direct revenue increase** and enhanced synthetic identity fraud detection.

This is directly relevant to Eidos: Vybhav built identity resolution at scale for one of the leading identity verification companies, then designed a purpose-built identity protocol for AI agents.

### Technical
- Deep learning (LSTM, RNN), NLP, ML engineering
- Identity resolution and fraud detection at scale
- Cryptographic protocols (Ed25519, zero-dependency implementations)
- Full-stack systems (Go, TypeScript, React, gRPC)
- Published researcher: AI-Powered Identity Verification, NLP for Name/Address Normalization, GenAI product innovation

### What He Built for Eidos
**AID Protocol** — a production-ready authentication/authorization framework for AI agents:
- 8 packages: `core`, `crypto`, `registry`, `verify`, `sap`, `sdk`, `middleware`, `wellknown`
- 53 tests passing, zero external dependencies
- Ed25519 dual-signature model (principal + platform)
- 11-step verification engine
- Step-Up Authorization Protocol (human-in-the-loop approval)
- Delegation tokens with financial limits, rate limits, time windows
- Cascade revocation

> *See: `knowledge/aid-protocol-summary.md`, `briefs/2026-02-25-hancock-auth-research.md`*

---

## The Combination

Daniel brings product vision, full-stack architecture, and the business side — from hedge fund trading desks to AI incubators to a patent in cloud control systems. He built the brain (Eidos Core) and the operational infrastructure (cockpit, fleet sync).

Vybhav brings identity resolution at enterprise scale and purpose-built cryptographic protocols. He solved identity matching at Socure (96% recall, 98.4% precision, $5.35M revenue impact), then designed the authentication backbone for AI agents from scratch. He built the trust layer (AID Protocol).

One builds the agent. The other builds the identity. Together: digital workers with real identity.

---

## Sources
- [Daniel Shanklin — LinkedIn](https://linkedin.com/in/danielshanklin)
- [Boone Voyage Labs — About](https://boonevoyagelabs.com/about-us)
- [SparrowShield — About](https://sparrowshield.com/about/)
- [Vybhav Reddy KC — LinkedIn](https://linkedin.com/in/vybhavreddykc)
- [Leadership Success Story: Identity Resolution at Socure — Free Press Journal](https://www.freepressjournal.in/latest-news/leadership-success-story-transforming-identity-resolution-and-fraud-prevention-through-ai-innovation-by-vybhav-reddy-kammireddy-changalreddy)
- [GenAI-Driven Product Innovation — TechLusive](https://www.techlusive.in/features/setting-new-standards-in-genai-driven-product-innovation-by-vybhav-reddy-kammireddy-changalreddy-1548480/)
- [AI and Data Analytics Synergy — India.com](https://www.india.com/business/ai-and-data-analytics-synergy-exploring-best-practices-by-vybhav-reddy-kammireddy-7512536/)
- [13News Now — Kid Pilot, 1991](https://www.13newsnow.com/article/features/the-kid-pilot-who-made-history-in-1991/291-c24b5916-ad83-4995-975e-036759c0d8e3)
- [Tulsa World — 7-Year-Old Pilot](https://tulsaworld.com/archive/7-year-old-pilot-sets-unofficial-flight-record/article_77e49187-ff4c-5fb8-9e4c-1d651bc565b6.html)
