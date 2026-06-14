# Domain Knowledge Patterns

Templates and patterns for structuring the `[KNOWLEDGE]` block across common verticals.
Use these as starting points — adapt to the user's specific context.

---

## Table of Contents
1. [SaaS / Tech Product](#1-saas--tech-product)
2. [Medical / Clinical](#2-medical--clinical)
3. [Legal](#3-legal)
4. [Financial Services](#4-financial-services)
5. [E-Commerce / Retail](#5-e-commerce--retail)
6. [HR / People Ops](#6-hr--people-ops)
7. [Customer Support](#7-customer-support)
8. [Real Estate](#8-real-estate)
9. [Marketing / Growth](#9-marketing--growth)
10. [Education / EdTech](#10-education--edtech)
11. [Logistics / Supply Chain](#11-logistics--supply-chain)
12. [Cybersecurity](#12-cybersecurity)
13. [Consulting / Professional Services](#13-consulting--professional-services)
14. [Healthcare Admin](#14-healthcare-admin)
15. [Compliance / RegTech](#15-compliance--regtech)

---

## 1. SaaS / Tech Product

```
[KNOWLEDGE]
Domain: SaaS Product Support / Technical Guidance

Key Terms:
  - MRR: Monthly Recurring Revenue — core business health metric
  - Churn: % of customers cancelling in a period; lower is better
  - DAU/MAU: Daily/Monthly Active Users — engagement ratios
  - Onboarding: The process of getting a new user to their "aha moment"
  - Webhook: HTTP callback triggered by events in the product

Core Heuristics:
  - Always confirm the user's plan tier before troubleshooting — features differ by tier
  - Bugs and feature requests go to different queues; clarify which applies
  - Prefer "here's how to do it" over "that's not possible" — find the workaround first
  - API issues: always ask for request ID / error code before diagnosing

Reference Facts:
  - Free tier limit: [X] seats, [Y] API calls/month
  - SLA uptime commitment: 99.9%
  - Data retention policy: [X] days
  - Supported integrations: [list]
```

**Interview Questions for SaaS:**
- What's the product name and core use case?
- What are the plan tiers and their limits?
- What are the top 5 most common support questions?
- What can/can't users do without contacting support?
- What's the escalation path?

---

## 2. Medical / Clinical

```
[KNOWLEDGE]
Domain: Clinical Medicine / Patient Education

Key Terms:
  - Chief Complaint: The primary reason a patient seeks care
  - Differential Diagnosis (DDx): List of possible conditions ranked by likelihood
  - Contraindication: A condition that makes a treatment inadvisable
  - PRN: "As needed" (pro re nata) — medication dosing instruction
  - SOAP: Subjective, Objective, Assessment, Plan — clinical note format

Core Heuristics:
  - Always recommend professional consultation for diagnosis/treatment decisions
  - Present information as educational, not prescriptive
  - When in doubt, err toward caution and referral
  - Flag red-flag symptoms that warrant emergency attention immediately
  - Avoid absolute certainty — medicine is probabilistic

Specialty Focus: [e.g., cardiology, dermatology, primary care, mental health]

Reference Facts:
  - Common drug interactions relevant to this specialty: [list]
  - Standard screening intervals: [e.g., colonoscopy q10yr over 45]
  - Red-flag symptoms requiring immediate escalation: [list]
```

**Critical Rules for Medical:**
- NEVER provide a diagnosis for a specific patient
- ALWAYS include "consult your physician" for treatment decisions
- Flag emergency symptoms immediately (chest pain + SOB, stroke symptoms, etc.)
- HIPAA: never ask for or store identifying patient information

---

## 3. Legal

```
[KNOWLEDGE]
Domain: Legal Information / [Practice Area]

Key Terms:
  - Discovery: Pre-trial process of exchanging relevant evidence
  - Standing: The right to bring a case to court
  - Precedent (Stare Decisis): Following prior court decisions
  - Fiduciary Duty: Legal obligation to act in another's best interest
  - Tortious Interference: Wrongfully interfering with a contractual relationship

Jurisdiction Focus: [e.g., US Federal, UK, California, EU]
Practice Areas: [e.g., employment, IP, contracts, criminal]

Core Heuristics:
  - Provide legal information, not legal advice — draw the line clearly
  - Jurisdiction matters enormously; always clarify which applies
  - Statute of limitations: always flag it may apply
  - "It depends" is often the honest answer — name the key variables

Reference Facts:
  - Relevant statutes: [list]
  - Key case law: [list]
  - Filing deadlines relevant to this practice area: [list]
```

**Critical Rules for Legal:**
- Never advise a specific person to take a specific legal action
- Always recommend consulting a licensed attorney
- Clearly distinguish between information and advice

---

## 4. Financial Services

```
[KNOWLEDGE]
Domain: [Wealth Management / Banking / Insurance / FinTech]

Key Terms:
  - AUM: Assets Under Management
  - Basis Point (bps): 1/100th of a percent; 100 bps = 1%
  - Fiduciary Standard: Must act in client's best interest (vs. suitability standard)
  - KYC: Know Your Customer — identity verification requirement
  - Drawdown: Peak-to-trough decline in portfolio value

Core Heuristics:
  - Always clarify tax implications may vary by individual situation
  - Past performance ≠ future results — always include this caveat
  - Distinguish between types of advice: informational vs. investment advice
  - Risk tolerance is individual; never assume

Reference Facts:
  - Regulated by: [FCA / SEC / FINRA / etc.]
  - Products offered: [list]
  - Applicable disclosure requirements: [list]
```

**Critical Rules for Finance:**
- Required disclaimers: "This is not investment advice"
- Never make specific investment recommendations without full KYC context
- Flag suitability requirements for complex products

---

## 5. E-Commerce / Retail

```
[KNOWLEDGE]
Domain: E-Commerce Customer Service / Retail Operations

Key Terms:
  - SKU: Stock Keeping Unit — unique product identifier
  - RMA: Return Merchandise Authorization — required for most returns
  - Fulfillment SLA: Time from order to ship
  - Chargeback: Credit card dispute initiated by customer
  - Net Promoter Score (NPS): Customer loyalty metric (0–10 scale)

Core Heuristics:
  - Order status: always verify in system before responding
  - Returns: check policy before promising outcome
  - "Customer is right" applies — but document exceptions
  - Escalate fraud suspicions to the risk team, don't diagnose

Reference Facts:
  - Return window: [X] days from delivery
  - Refund processing time: [X] business days
  - Shipping carriers: [list]
  - Restricted items (can't be returned): [list]
```

---

## 6. HR / People Ops

```
[KNOWLEDGE]
Domain: Human Resources / People Operations

Key Terms:
  - HRIS: Human Resource Information System (Workday, BambooHR, etc.)
  - PIP: Performance Improvement Plan
  - At-Will Employment: Either party can terminate without cause (US)
  - FMLA: Family and Medical Leave Act (US)
  - Comp Band: Salary range for a role level

Core Heuristics:
  - Employment law varies by jurisdiction — always confirm location
  - PIPs: advise consulting legal before formal process
  - Sensitive matters (harassment, termination): escalate to HR leadership
  - Benefit questions: confirm effective dates and eligibility periods

Reference Facts:
  - Company locations and applicable labor laws: [list]
  - Open enrollment windows: [dates]
  - Benefits summary: [link or list]
```

---

## 7. Customer Support

```
[KNOWLEDGE]
Domain: Customer Support / CX

Key Terms:
  - CSAT: Customer Satisfaction Score (1–5)
  - FRT: First Response Time
  - FCR: First Contact Resolution Rate
  - Escalation Path: Tier 1 → Tier 2 → Engineering / Leadership
  - SLA: Service Level Agreement — response/resolution commitments

Core Heuristics:
  - Acknowledge before solving — empathy first, solution second
  - Never promise a specific timeline unless you're certain
  - Document every interaction in the ticket system
  - Offer compensation proactively for clear company errors

Escalation Triggers:
  - [X hours] without resolution → escalate to Tier 2
  - Legal threats → escalate to legal
  - Data breach concerns → escalate to security immediately
```

---

## 8. Real Estate

```
[KNOWLEDGE]
Domain: Real Estate [Residential / Commercial / Property Management]

Key Terms:
  - Cap Rate: Net Operating Income / Property Value — investment return metric
  - LTV: Loan-to-Value ratio
  - Due Diligence Period: Inspection window post-offer
  - MLS: Multiple Listing Service — shared property database
  - Earnest Money: Good-faith deposit on offer

Core Heuristics:
  - Market conditions vary radically by zip code — always localise advice
  - Disclose: always recommend consulting a licensed agent for transactions
  - Financing: pre-approval ≠ approval — note the difference
  - Timelines: contingency periods are negotiable and vary by contract

Reference Facts:
  - Licensed in: [states/regions]
  - Current market: [buyer's/seller's/balanced]
  - Average days on market in focus market: [X]
```

---

## 9. Marketing / Growth

```
[KNOWLEDGE]
Domain: Marketing / Growth / Brand

Key Terms:
  - CAC: Customer Acquisition Cost
  - LTV: Lifetime Value of a customer
  - CTR: Click-Through Rate
  - MQL/SQL: Marketing Qualified Lead / Sales Qualified Lead
  - A/B Test: Controlled experiment testing two variants

Core Heuristics:
  - Always tie recommendations to business metrics, not vanity metrics
  - Creative without data is opinion; data without creative is noise
  - Attribution is hard — multi-touch is more honest than last-click
  - Seasonality matters; benchmark against same period prior year

Channel Focus: [SEO / Paid / Email / Social / Content / PR]

Reference Facts:
  - Brand voice: [descriptors]
  - Brand colors/fonts: [if relevant]
  - Approved messaging pillars: [list]
  - Messaging NOT to use: [list]
```

---

## 10. Education / EdTech

```
[KNOWLEDGE]
Domain: Education / Tutoring / EdTech

Key Terms:
  - Bloom's Taxonomy: Framework for learning objectives (Remember → Create)
  - Scaffolding: Temporary support that's gradually removed as mastery grows
  - Formative Assessment: Ongoing checks for understanding during learning
  - Summative Assessment: End-of-unit evaluation of mastery
  - Differentiation: Adapting instruction to individual learner needs

Subject Focus: [Math / Science / Language Arts / History / Coding / etc.]
Level: [K-12 / Undergraduate / Professional / Adult Learning]

Core Heuristics:
  - Never just give the answer — guide toward it with questions
  - Match explanation complexity to student level (ask first if unclear)
  - Celebrate effort and process, not just correct answers
  - When a student is stuck, try a different angle / analogy

Pedagogy Style: [Socratic / Direct Instruction / Project-Based / etc.]
```

---

## 11. Logistics / Supply Chain

```
[KNOWLEDGE]
Domain: Logistics / Supply Chain Operations

Key Terms:
  - POD: Proof of Delivery
  - LTL: Less Than Truckload — shared freight shipping
  - OTIF: On-Time In-Full delivery metric
  - Lead Time: Time from order to receipt of goods
  - Safety Stock: Inventory buffer against demand uncertainty

Core Heuristics:
  - Carrier performance varies by lane — check historical data before committing
  - Always confirm incoterms on international shipments
  - Customs delays are common; build 3–5 days buffer for cross-border
  - Single-source dependencies are risk — flag them

Reference Facts:
  - Primary carriers: [list]
  - Average transit times by lane: [table]
  - Key suppliers and lead times: [list]
```

---

## 12. Cybersecurity

```
[KNOWLEDGE]
Domain: Cybersecurity [Defensive / Compliance / Incident Response]

Key Terms:
  - IOC: Indicator of Compromise
  - CVE: Common Vulnerabilities and Exposures
  - MTTR: Mean Time to Respond / Remediate
  - Blast Radius: Scope of impact if a vulnerability is exploited
  - Zero-Day: Vulnerability with no available patch

Core Heuristics:
  - Assume breach — design controls assuming attackers are already inside
  - Least privilege: grant minimum access required
  - Log everything; alert on anomalies
  - Patch critical CVEs within 48 hours; high within 7 days

Frameworks: [NIST / CIS Controls / ISO 27001 / SOC 2]

Reference Facts:
  - Current compliance status: [list]
  - Known open vulnerabilities: [list]
  - Incident response contacts: [list]
```

---

## 13. Consulting / Professional Services

```
[KNOWLEDGE]
Domain: Management Consulting / Advisory

Key Terms:
  - MECE: Mutually Exclusive, Collectively Exhaustive — structuring principle
  - Work Back Schedule: Plan built from deadline backwards
  - Hypothesis-Driven: Start with hypothesis, then gather evidence (not the reverse)
  - SOW: Statement of Work
  - Deck: Presentation / slide deliverable

Core Heuristics:
  - Structure every answer: situation / complication / question / answer (SCQA)
  - Lead with the so-what, not the data
  - Quantify wherever possible; "significant" is meaningless
  - The answer is always "it depends" — but then name the variables and take a position

Communication Style: [McKinsey-style / Bain-style / boutique / internal advisory]
```

---

## 14. Healthcare Admin

```
[KNOWLEDGE]
Domain: Healthcare Administration / Medical Billing / Practice Management

Key Terms:
  - CPT Code: Procedure billing code
  - ICD-10: Diagnosis coding system
  - Prior Authorization (PA): Insurer approval required before service
  - EOB: Explanation of Benefits — insurer's payment breakdown
  - Claim Denial: Rejected insurance claim requiring follow-up

Core Heuristics:
  - Verify insurance eligibility before every appointment
  - Document everything — if it's not documented, it didn't happen
  - PA timelines vary by payer — start early for complex procedures
  - Denial management: appeal within the timely filing window

Reference Facts:
  - EHR system in use: [name]
  - Top payers and their PA requirements: [list]
  - Timely filing limits by payer: [list]
```

---

## 15. Compliance / RegTech

```
[KNOWLEDGE]
Domain: Regulatory Compliance / [GDPR / HIPAA / SOX / AML / etc.]

Key Terms:
  - Data Subject: Individual whose personal data is processed (GDPR)
  - SAR: Subject Access Request — right to access personal data
  - PII: Personally Identifiable Information
  - Audit Trail: Immutable log of actions for compliance review
  - Material Non-Public Information (MNPI): Insider information

Core Heuristics:
  - When in doubt, over-document and under-share
  - Compliance is a floor, not a ceiling — aim higher
  - Regulatory interpretations change — flag "consult counsel" for edge cases
  - Breach notification windows are short — know them cold

Applicable Regulations: [list]
Key Deadlines: [list]
Regulatory Body: [FCA / SEC / ICO / HHS / etc.]
```
