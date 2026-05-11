# Case Study 2 — CFPB Ops Metrics: Regulatory Compliance Delivery Under a Fixed Deadline

**Company:** American Express  
**Program:** ACORN — Ops Metrics Workstream  
**Role:** Sole Product Owner  
**Deadline:** November 30, 2021 (CFPB Debt Collection Rule)  
**Outcome:** Delivered on time · 18 outside agencies onboarded · Architecture adopted as US ACORN technical baseline

---

## The Challenge

The CFPB's Debt Collection Rule, effective November 30, 2021, fundamentally changed compliance obligations for enterprises using third-party collection agencies. Creditors could no longer disclaim knowledge of vendor practices — enterprises were now directly accountable for collection activity conducted on their behalf.

American Express needed a structured data capture capability requiring all outside agencies to report detailed activity back to the platform in real time: call types, contact parties, payment negotiations, letters sent, and legal proceedings. This was a regulatory mandate with a fixed deadline — not a feature request.

The program launched into a difficult environment:

- Business partners had missed their prior-year performance target by approximately 1% — a gap directly attributed to insufficient agency transparency — affecting department ratings and bonuses. Organizational pressure to close that gap quickly was immediate.
- The engineering team was still working through foundational architecture challenges on the new platform.
- Outside agencies were required to simultaneously ingest files AmEx was sending and return structured data back — with very short notice and widely varying levels of technical capability.
- Versions 1 and 2 of the JSON schema distributed to all 21 agencies simultaneously contained technical errors — misspelled field names and malformed code — that prevented agencies from building against them. External feedback was direct: American Express did not know what it was doing.

I was the sole Product Owner responsible for Ops Metrics end-to-end from the first day of the workstream.

---

## My Role

Full product ownership of the Ops Metrics workstream — from requirements definition through agency onboarding through production release — as the only PO on this capability.

Scope:
- Regulatory requirements translation into product architecture
- JSON schema design and quality ownership
- 12-entity data model sequencing and build oversight
- 21 outside agency partner coordination
- Stakeholder management across business, engineering, and agency partners simultaneously
- Strategic decision-making under compliance and organizational pressure

---

## What I Did

### Strategic Decision — Buying Time Through Product Expansion

Reading the organizational environment — the performance miss, the bonus implications, the department ratings pressure — I identified a path that could de-escalate all three stakeholder groups simultaneously without exposing the underlying architecture instability externally.

Business partners had begun requesting additional data elements to hold agencies more accountable. Rather than deferring the request, I recommended incorporating the new elements — framing it as a product expansion decision.

Both things were true simultaneously: more elements were genuinely needed, and more time was genuinely needed. The decision aligned both needs within a single defensible product rationale:
- Business partners were satisfied — they were getting the accountability elements they requested
- Agencies had a legitimate reason to pause development
- Engineering had the runway to stabilize the architecture
- AmEx's external credibility was protected

I used that window to conduct structured discovery — identifying all additional fields required across remaining entities and sequencing the build deliberately, addressing simpler entities first and reserving complex calculated logic for last as the architecture stabilized.

### 12-Entity Data Model — Design and Sequencing

I designed the full 12-entity data model covering the complete spectrum of outside agency activity required for CFPB compliance:

| # | Entity | What It Captured |
|---|---|---|
| 1 | Inventory Item | Account inventory placed with outside agency |
| 2 | Collector Assignment | Which collector was assigned to the account |
| 3 | Phone | Call details — inbound/outbound, party contacted, disposition |
| 4 | Mail | Physical mail and letter communications |
| 5 | Email | Email communications to cardmember |
| 6 | SMS | Text message contacts |
| 7 | Contact Consent | Cardmember consent records — who authorized, method, date |
| 8 | Attorney Details | Attorney and legal representation information |
| 9 | Legal | Legal action records — suits, judgments, court proceedings |
| 10 | Specialty Details | Bankruptcy and deceased account handling |
| 11 | CDMP | Consumer Debt Management Program enrollment and status |
| 12 | Promise | Payment promise records — terms, amounts, dates negotiated |

**Sequencing decision:** Entities 1–9 were persisted to Couchbase. Entities 10–12 — requiring complex calculations, structured financial data, and regulatory record-keeping — were persisted to Oracle. The build progressed 4 → 6 → 8 → 12 entities, with each iteration validated bi-directionally before expansion. This ensured architecture stability was proven before the most complex logic was introduced.

### JSON Schema Quality Ownership

After two rounds of broken schemas were distributed to all 21 agencies simultaneously — crashing their code and generating external feedback that American Express did not know what it was doing — I absorbed quality review of every line of schema documentation before distribution.

**Standards I enforced:**
- Timestamps required full format: MM/DD/YYYY HH:MM:SS with timezone offset
- Enum values required exact match to defined options — no free text permitted
- Character lengths enforced strictly across all fields
- File-level failures applied to critical missing fields; record-level failures to non-critical gaps
- Structured error notifications sent to agencies identifying exactly what was missing and why records were rejected

Version 3 was the schema that held. Development proceeded from Version 3 forward.

### Agency Onboarding Strategy — 21 Partners in Parallel

All 21 agencies received initial schemas simultaneously. The approach to building with them required careful sequencing as technical sophistication varied significantly — three agencies had outsourced IT and were delivering faster and more accurately than the rest.

**Key decision:** I advanced the MVP validation with the three technically strongest agencies as the lead cohort while continuing to bring all remaining agencies forward in parallel. This accelerated the validation timeline without excluding any partner from the development track and produced a stable baseline all subsequent agencies could build against.

Development validated bi-directionally at each entity progression: agencies were required to both ingest AmEx files and return structured data before the build advanced.

### Error Handling — From Manual Triage to Platform Infrastructure

Initially, error handling was entirely manual — engineering identified failures, routed them to me, I coordinated with business stakeholders who relayed issues to agencies. As volumes scaled to hundreds of errors daily I absorbed end-to-end ownership of the failure reconciliation process.

I drove three operational improvements during this phase:
1. Transitioned from file-level rejection to record-level validation — significantly reducing reprocessing overhead
2. Established daily sync calls with agency technical teams — structured sessions that enabled agencies to implement upstream data quality controls
3. Identified systemic limitations in the platform's ability to handle ingestion failures at scale

I translated these learnings into product requirements — designing an automated error reporting infrastructure that removed all human intervention from the process. Engineering built a Spring Boot job executing every 30 minutes, compiling structured JSON error files and delivering them via SFTP. Hundreds of daily manual interventions converted to fully automated structured notifications across all three OA processing pipelines.

### 8-Byte Agency Identifier Architecture

I designed and created the 8-byte agency identifier architecture used by all outside agency partners — foundational to how files were routed, tracked, and attributed across the entire agency network. Every file exchange, every pipeline, every market depended on this identification framework.

---

## Results

| Metric | Outcome |
|---|---|
| Regulatory deadline | Met — November 30, 2021 |
| Entities delivered | 12 (4→6→8→12 progression) |
| Outside agencies onboarded | 18 of 21 released to production |
| Architecture adoption | Adopted as technical baseline for entire US ACORN platform |
| Platform scale after handoff | 12 entities → 38 entities |
| Error handling | Converted hundreds of daily manual interventions to fully automated SFTP notifications |

**Director recognition (June 2021):** *"We enabled valuable new insights with the release of Ops Metrics."* — Federico J. Formica

**Manager recognition (2019):** *"Koka's knowledge of OA Operating Metrics for the US has been invaluable. These skills are core to shaping new tools and capabilities globally."*

**Manager recognition (2020):** *"Koka continues to perform at a high level... she has led the US OA Operating Metrics which impacts over 17 US Outside Agencies... Koka has put in numerous hours to assist her business partner and the Outside Agencies in understanding the information that is being sent to ACORN."*

---

## What Made This Hard

Three stakeholder groups were hostile simultaneously — agencies frustrated by broken schemas, business partners under performance pressure, and engineering working through foundational instability — with no clean line of accountability and organizational pressure compounding from every direction.

The strategic product expansion decision was the key unlock: it de-escalated all three groups simultaneously within a single defensible product rationale, bought engineering the runway to stabilize, and maintained AmEx's external credibility throughout. Both things were true — more elements were genuinely needed, and more time was genuinely needed — and the decision aligned both without exposing the underlying tension externally.

---

*← [Back to Portfolio](../README.md)*
