[01-acorn-founding-product-owner.md](https://github.com/user-attachments/files/27610858/01-acorn-founding-product-owner.md)
# Case Study 1 — ACORN: Founding Product Owner, 0→1 Platform Build

**Company:** American Express  
**Program:** ACORN — Advanced Collections through Outside Recovery Network  
**Role:** Founding Product Owner (sole US-based hire)  
**Tenure:** 2017 – January 2022  
**Scale at exit:** 38-entity production platform · $1.3BN annual US collections · 5.3M accounts · 15 outside agencies · 7 global markets · 9 product owners

---

## The Challenge

American Express operated its external collections function through a legacy mainframe system that could not scale to meet the company's global collections needs, compliance obligations, or the business logic requirements of its charge card product. A third-party vendor platform (FICO DM9) had been piloted for the Canadian market but could not be adapted to AmEx's charge card structure — no preset spending limit, no minimum payment construct, and collection thresholds and payoff logic that no off-the-shelf platform had been built to handle.

Leadership made the decision to build a cloud-native collections platform from scratch — purpose-built for AmEx's products and global scale. The platform was named ACORN: Advanced Collections through Outside Recovery Network.

There was no existing roadmap. No existing architecture. No existing product team. The program needed someone who understood the business from the inside — the data, the risk, the regulatory obligations, and what the legacy systems actually did — and who could translate that into a buildable, scalable product.

---

## My Role

I was the **Founding Product Owner and first US-based hire** on the ACORN program. I was embedded with the engineering team in Sunrise, FL while the program manager operated remotely — making me the primary product authority on the ground from day one.

My scope covered:
- End-to-end product ownership from requirements through production release
- Data architecture and global data model design
- Outside agency partner onboarding and integration management
- Technical documentation and knowledge infrastructure
- Program-level oversight across multiple concurrent scrum teams
- Incident response and risk escalation

I held this role for four years — two years beyond the original program scope — as the platform grew to require 9 product owners across 7 global markets.

---

## What I Did

### Data Architecture — Global Foundation

The starting point was Canada's DM9 data mapping — a FICO vendor model built for the general collections industry, not AmEx's charge card structure. Before it could serve as a foundation for ACORN, it required fundamental re-architecture.

**Three core problems I resolved:**

**1. Generic vs. AmEx-specific logic.** The DM9 model did not account for charge card constructs — no preset spending limit, different delinquency behaviors, AmEx-specific thresholds and payoff logic. I embedded these directly into the data model.

**2. System-coded vs. human-readable fields.** Field names were internal system abbreviations (e.g., "MASLA" for master person last name). I translated and standardized all 300+ fields into clear, business-readable definitions usable by product, engineering, operations, and external partners without requiring system-specific knowledge.

**3. Compounded fields from legacy mainframe design.** Many fields combined multiple data elements within a single field due to historical system limitations. I decomposed these into discrete, logically separated elements and redesigned the architecture for global scalability.

To execute this work I ran queries directly against the legacy mainframe to analyze underlying data structures and validate field-level behavior — partnering with business stakeholders to clarify ambiguous definitions.

The model I built became the reference standard adopted across all 7 ACORN market rollouts.

### Platform Build — MVP to Production Scale

I sequenced the ACORN build across four deliberate phases:

| Phase | Scope | Validation Method |
|---|---|---|
| MVP | 12-entity core platform | Bi-directional TDD with 18 outside agencies |
| Scale | 12 → 38 entities | Progressive entity expansion |
| Global | US → 7 markets | Market-specific data model adaptation |
| Full program | Single PO → 9 POs | Knowledge transfer and documentation |

Each entity expansion was validated bi-directionally — agencies were required to both ingest files AmEx sent and return structured data before development advanced.

### Outside Agency Integration — 21 Partners in Parallel

I led the simultaneous onboarding of 21 outside agency partners through parallel bi-directional development — managing JSON/SFTP file exchange validation, schema quality control, and production release across agencies ranging from fully automated to fully manual technical capability.

Key decisions:
- **Designed the 8-byte agency identifier architecture** foundational to all file routing, tracking, and attribution across the full ACORN network
- **Identified the three technically strongest agencies** (those with outsourced IT) and advanced MVP validation with them as the lead cohort while continuing to bring all remaining agencies forward in parallel — accelerating the platform build without excluding any partner
- **Assumed quality review of every schema line** after early versions contained errors that damaged external credibility; the corrected version became the stable baseline all development was built on

18 of 21 agencies reached production.

### Automated Error Reporting Infrastructure

During the MVP phase, error handling was entirely manual — engineering identified failures, routed them to me, and I coordinated communication with business stakeholders who relayed issues to agencies. As volumes scaled to hundreds of errors daily, I absorbed end-to-end ownership of the failure reconciliation process.

Rather than continue managing this reactively, I translated months of manual triage experience into product requirements — designing an automated error reporting infrastructure that removed all human intervention from the process. Engineering built a Spring Boot job executing every 30 minutes, compiling structured JSON error files per agency and delivering them via SFTP. The solution converted hundreds of daily manual interventions to fully automated, structured notifications across all three OA processing pipelines.

### Incident Response — Log4j & 1099C

**Log4j:** During the holiday period with most leadership unavailable, I identified the Log4j security vulnerability in the legacy RMS system as its formal owner, escalated to the vendor with a 24-hour remediation requirement, and made a unilateral decision to halt all active sprint work and redirect engineering to security remediation. Decision affirmed by directors upon return.

**1099C:** While attending cross-team standups, I identified that the 1099C compliance workstream had moved from a missed sprint milestone to an active regulatory deadline risk. I made the decision to descope a concurrent product manager's feature, secured direct PM buy-in, and escalated to the technology director for additional engineering resources. Both the compliance workstream and the descoped feature were delivered within the same sprint.

---

## Results

| Metric | Outcome |
|---|---|
| Platform scale | 12-entity MVP → 38-entity production platform |
| Annual US collections supported | $1.3BN |
| Accounts supported | 5.3M |
| Outside agencies onboarded | 18 of 21 released to production |
| Global markets | 7 (US, Mexico, Argentina, UK, Australia, New Zealand, Canada) |
| Program growth | Single PO → 9 product owners |
| App Health score | Perfect |
| Performance rating | G1/L1 (top available) every year |
| Regulatory deadline | CFPB Ops Metrics delivered November 30, 2021 |
| Industry first | First jBPM PAM V7 implementation in company and industry |

**Director recognition (June 2021):** *"ACORN team ROCKS! We enabled valuable new insights with the release of Ops Metrics... first users of jBPM PAM V7 in the company and industry; perfect App Health score."* — Federico J. Formica

**Manager recognition (2019):** *"Koka's knowledge of OA Operating Metrics for the US has been invaluable. These skills are core to shaping new tools and capabilities globally."*

---

## Key Decisions Worth Noting

**The global architecture decision.** When tasked with building the MX/ARG data mapping using Canada's DM9 model as a starting point, I recognized it had been built for a single market and could not scale. Rather than patch it market by market, I redesigned the architecture to be globally scalable from the outset. Every subsequent market rollout built on that foundation.

**Buying time through product expansion.** During the Ops Metrics build, the engineering team was working through foundational architecture challenges while agencies were already building in parallel and business partners were under pressure from a prior-year performance miss. When business partners requested additional data elements, I recommended incorporating them — genuinely needed, and framing the expansion gave engineering the runway to stabilize without that instability becoming visible externally. Both needs were served through a single defensible product decision.

**The 3-agency MVP pivot.** With 21 agencies onboarding simultaneously, the least technically sophisticated partners were constraining build velocity. I identified the three agencies with outsourced IT who were consistently faster and advanced the MVP validation with them as the lead cohort while continuing to bring all agencies forward in parallel. This accelerated the platform build without excluding any partner.

---

*← [Back to Portfolio](../README.md)*
