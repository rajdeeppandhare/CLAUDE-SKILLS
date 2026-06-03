---
name: job-copilot
description: >
  A comprehensive job search and application skill for Claude. Triggers when a user mentions
  job hunting, job search, finding jobs, applying for jobs, resume writing, CV creation, interview prep,
  job portals (Indeed, LinkedIn, Naukri, Monster, Internshala, Glassdoor, AngelList, etc.), or anything
  related to career transitions, job applications, or employment. Also triggers when a user shares a
  job description and wants a tailored resume, or when they want to track their application pipeline.
  Use this skill proactively whenever the user mentions career goals, job titles they want, companies
  they're targeting, or frustration with job searching — even if they don't say "job copilot".
---

# Job Copilot — Claude Skill

A full-stack career assistant that searches, shortlists, tailors, tracks, and coaches — from
first search to signed offer.

## Skill Capabilities

1. **Job Search** — Query multiple portals and aggregate results
2. **Smart Shortlisting** — Filter and rank jobs against user criteria
3. **Resume Generation** — Create ATS-optimised, JD-matched resumes
4. **Application Tracker** — Maintain a live pipeline of applications
5. **Interview Prep** — Generate role-specific questions and answers
6. **Cover Letter Writing** — Bespoke letters per application
7. **Salary Intelligence** — Benchmarking and negotiation guidance

---

## Phase 1 — User Profile Intake

Before searching, collect:

```
PROFILE FIELDS (ask conversationally, not as a form):
- Role(s) targeting (title, seniority level)
- Skills & tech stack
- Years of experience
- Preferred locations / remote preference
- Industries of interest (or to avoid)
- Salary expectation range
- Employment type: full-time / part-time / contract / internship
- Notice period / availability
- Special constraints (visa, relocation, etc.)
```

Store this as a **Candidate Profile** — reference it across all phases.
→ See `references/portal-search-patterns.md` for portal-specific query construction.

---

## Phase 2 — Multi-Portal Job Search

### Supported Portals

| Portal | Best For | Region |
|---|---|---|
| LinkedIn Jobs | Professional roles, networking | Global |
| Indeed | Volume, all levels | Global |
| Naukri | Tech & corporate | India |
| Monster | Mid-senior, corporate | Global |
| Internshala | Internships, fresher roles | India |
| Glassdoor | Company culture + jobs | Global |
| AngelList / Wellfound | Startups | Global |
| Dice | Tech specialists | US |
| Hirect | Direct hiring, startups | India |
| We Work Remotely | Remote-only | Global |
| Otta | Tech/startup roles | UK/EU |
| Remotive | Remote tech | Global |

### Search Execution

When performing a job search:

1. Construct role-specific query strings per portal (see `references/portal-search-patterns.md`)
2. Use web_search with portal-targeted queries: `site:linkedin.com/jobs [role] [location]`
3. Fetch job listings and extract: Title, Company, Location, Salary (if shown), Posted Date, Apply Link
4. Deduplicate across portals by company + title
5. Return structured results — minimum 10 jobs before shortlisting

### Search Query Templates

```
LinkedIn:   site:linkedin.com/jobs/view [role] [location] [skills]
Indeed:     site:indeed.com/viewjob [role] [location]
Naukri:     site:naukri.com [role] [location] [experience]
Glassdoor:  site:glassdoor.com/job-listing [role] [company type]
Internshala: site:internshala.com/internship/detail [role] [skills]
```

---

## Phase 3 — Smart Shortlisting

After gathering results, score each job against the Candidate Profile:

### Scoring Matrix (100 pts total)

| Factor | Weight |
|---|---|
| Role title match | 25 pts |
| Skills overlap (%) | 30 pts |
| Location / remote fit | 20 pts |
| Salary range alignment | 15 pts |
| Company type preference | 10 pts |

Present shortlist as a ranked table:

```
Rank | Job Title | Company | Match % | Salary | Location | Apply Link
```

Highlight **Top 5** as priority applications. Flag any red flags (contract-only when FT wanted, etc.).

→ See `references/portal-search-patterns.md` § Shortlisting Signals for nuanced filters.

---

## Phase 4 — JD Analysis & Resume Generation

### Step 4a — Parse the JD

When a user provides a job description, extract:

```
JD EXTRACTION SCHEMA:
- Required skills (hard)
- Preferred skills (nice-to-have)
- Years of experience required
- Key responsibilities (top 5)
- Keywords likely used by ATS
- Company values/culture signals
- Reporting structure hints
```

### Step 4b — Gap Analysis

Compare JD requirements vs Candidate Profile:
- ✅ Strong match → emphasise in resume
- ⚠️ Partial match → reframe existing experience
- ❌ Missing → note honestly; don't fabricate

### Step 4c — Resume Construction

Build a **tailored, ATS-optimised resume** following this structure:

```
[CONTACT HEADER]
Name | Email | Phone | LinkedIn | GitHub/Portfolio | Location

[PROFESSIONAL SUMMARY — 3 lines]
Targeted to the specific role. Use exact job title from JD.
Mirror 2–3 high-weight JD keywords naturally.

[SKILLS]
Two-column layout. Lead with JD-matched hard skills.
Include tools, platforms, languages, methodologies.

[WORK EXPERIENCE]
Reverse chronological. For each role:
  - Title | Company | Location | Dates
  - 3–5 bullet points using CAR format:
    Context → Action → Result (with metrics where possible)
  - Front-load JD keywords in first 5 words of each bullet
  - Quantify: %, $, time saved, scale, team size

[EDUCATION]
Degree | Institution | Year | GPA (if >3.5 / top 10%)

[PROJECTS] (if relevant or fresher)
Project name | Tech stack | Key outcome

[CERTIFICATIONS] (if relevant)

[OPTIONAL SECTIONS]
Publications, Open Source, Awards — only if relevant
```

### ATS Optimisation Rules

- Use standard section headings (no creative titles)
- No tables, columns, graphics, or text boxes in final file
- Font: standard (Calibri, Arial, Times New Roman)
- Keywords from JD must appear verbatim at least once
- One page for <5 yrs exp; two pages max for senior roles
- File name: `FirstName_LastName_Role.pdf`

→ See `references/resume-jd-matrix.md` for full keyword mapping and bullet formulas.

---

## Phase 5 — Cover Letter

When requested, generate a cover letter:

```
STRUCTURE:
Para 1 — Hook: Specific reason you want THIS company/role (not generic)
Para 2 — Value match: Your top 2 achievements mapped to their top 2 needs
Para 3 — Culture/mission fit: Reference something specific about the company
Para 4 — CTA: Confident close, express enthusiasm, invite next step

TONE: Professional but human. No clichés ("I am writing to apply…")
LENGTH: 250–350 words
```

---

## Phase 6 — Application Tracker

Maintain a tracker table in-conversation (or as a downloadable format):

```
| # | Company | Role | Portal | Applied | Status | Next Step | Notes |
|---|---------|------|--------|---------|--------|-----------|-------|
| 1 | Acme Co | SWE II | LinkedIn | 2024-01-15 | Applied | Follow up Jan 22 | Referral via Priya |
```

**Status options**: Saved → Applied → Screening → Interview → Offer → Rejected → Withdrawn

On each update, prompt:
- Any interview scheduled? → Trigger Phase 7
- Offer received? → Trigger salary benchmarking
- Rejected? → Ask if they want feedback analysis

---

## Phase 7 — Interview Preparation

When an interview is confirmed, generate:

### 7a — Role-Specific Question Bank

```
CATEGORIES:
- Behavioural (STAR format) — 8 questions mapped to JD competencies
- Technical — 5–10 questions at stated seniority level
- Company-specific — 3 questions using their recent news/products
- Reverse questions — 5 smart questions to ask the interviewer
```

### 7b — Answer Coaching

For behavioural questions, scaffold STAR answers using user's own background:
- **S**ituation: brief context
- **T**ask: what was required
- **A**ction: what YOU specifically did
- **R**esult: measurable outcome

### 7c — Technical Round Prep

For engineering/data/design roles, provide:
- Concept refreshers for weak areas flagged in gap analysis
- Common problem patterns for the role
- System design frameworks (for senior roles)

---

## Phase 8 — Offer & Negotiation

When an offer arrives:

1. **Benchmarking**: Compare offer to market (use web search for current salary data)
2. **Total comp breakdown**: Base + bonus + equity + benefits
3. **Negotiation script**: Provide exact language for counter-offer
4. **Decision framework**: If multiple offers, weighted comparison table

---

## Interaction Principles

- **Never fabricate** experience or skills in resumes — only reframe honestly
- **Ask before assuming** role level, location preference, or salary range
- **Be specific** — generic advice is useless; tie everything back to the user's JD and profile
- **Proactive nudges** — remind users of follow-ups, deadlines, and next steps
- **Respect context** — a fresher and a VP need very different guidance; calibrate accordingly

---

## Quick Command Reference

| User says... | Copilot does... |
|---|---|
| "Find me jobs as a [role]" | Runs Phase 1 intake → Phase 2 search → Phase 3 shortlist |
| "Here's a JD, fix my resume" | Runs Phase 4 full resume tailoring |
| "Track this application" | Updates Phase 6 tracker |
| "I have an interview at [company]" | Runs Phase 7 prep |
| "I got an offer" | Runs Phase 8 benchmarking + negotiation |
| "Write me a cover letter" | Runs Phase 5 |

---

## Reference Files

- `references/portal-search-patterns.md` — Portal query syntax, shortlisting signals, red flag patterns
- `references/resume-jd-matrix.md` — ATS keyword mapping, bullet formulas, section templates by role type
