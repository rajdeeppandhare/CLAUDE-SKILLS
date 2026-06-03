# 💼 Job Copilot — Claude Skill

> A full-stack career assistant for Claude that searches multiple job portals, shortlists roles against your profile, writes ATS-optimised tailored resumes, and tracks your entire application pipeline — from first search to signed offer.

---

## ✨ What It Does

Tell Claude what role you're targeting and Job Copilot takes over:

- 🔍 **Searches** LinkedIn, Indeed, Naukri, Monster, Internshala, Glassdoor, Wellfound & more — simultaneously
- 🎯 **Shortlists** jobs by scoring them against your skills, location, salary, and preferences
- 📄 **Writes** a tailored, ATS-optimised resume for each specific JD — not a generic template
- 📬 **Drafts** bespoke cover letters per application
- 📊 **Tracks** your entire pipeline: Applied → Screening → Interview → Offer
- 🎤 **Preps** you for interviews with role-specific questions + coached STAR answers
- 💰 **Benchmarks** offers and gives you a negotiation script

---

## 🌐 Supported Portals

| Portal | Strength | Region |
|---|---|---|
| 🔵 LinkedIn Jobs | Professional roles, referral context | Global |
| 🟡 Indeed | Volume across all levels | Global |
| 🟠 Naukri | Tech & corporate careers | India |
| 🟣 Monster | Mid-to-senior corporate | Global |
| 🟢 Internshala | Internships & fresher roles | India |
| ⚫ Glassdoor | Company culture + job search | Global |
| 🔴 Wellfound (AngelList) | Startup roles + equity | Global |
| 🔷 Dice | US tech specialists | United States |
| 🌍 We Work Remotely | Remote-only positions | Global |
| 🇬🇧 Otta | Tech & startup roles | UK / EU |
| ☁️ Remotive | Remote tech jobs | Global |

---

## 🚀 What It Can Do For You

| You say... | Copilot does... |
|---|---|
| "Find me jobs as a data engineer" | Profile intake → multi-portal search → scored shortlist |
| "Here's a JD, make me a resume" | JD parsing → gap analysis → ATS-optimised tailored resume |
| "Write me a cover letter for this" | Bespoke letter with company-specific hook |
| "Track this application at Stripe" | Adds to pipeline tracker with status + next step |
| "I have an interview at Google" | Question bank + STAR answer coaching + reverse questions |
| "I got an offer for ₹24L" | Market benchmarking + negotiation script |

---

## 🧠 How Resume Tailoring Works

Job Copilot doesn't use a generic template. For every JD:

1. **Extracts** required skills, preferred skills, ATS keywords, and culture signals from the JD
2. **Gaps analysis** — maps what you have vs. what they want
3. **Rewrites** your bullets using CAR/STAR-Lite format with JD keywords front-loaded
4. **Optimises** for ATS: standard headings, no tables/graphics, keyword density checked
5. **Calibrates** length and tone to your seniority level (intern → director)

### Example Bullet Transformation

| Before | After |
|---|---|
| "Responsible for managing the data pipeline" | "Engineered Airflow ETL pipeline processing 12M daily records, reducing data prep from 6hrs to 18min" |
| "Helped with frontend features" | "Shipped 14 React features over 3 sprints, driving 22% improvement in checkout conversion" |
| "Managed a team" | "Led 6-person backend team, delivering payment integration 2 weeks ahead of schedule" |

---

## 📊 Application Tracker

Your pipeline, always up to date:

```
# | Company    | Role         | Portal    | Applied    | Status     | Next Step
1  | Stripe     | PM II        | LinkedIn  | 2024-01-15 | Interview  | Loop – Jan 24
2  | Razorpay   | Senior SWE   | Naukri    | 2024-01-17 | Screening  | Call – Jan 20
3  | Meesho     | Data Analyst | Indeed    | 2024-01-18 | Applied    | Follow up Jan 25
```

Status flow: `Saved → Applied → Screening → Interview → Offer → Accepted / Rejected`

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `job-copilot.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `job-copilot/` folder
3. Rename to `job-copilot.skill`
4. Upload via **Settings → Skills → Install Skill**

### Option 3 — Copy-paste SKILL.md
1. Open `SKILL.md`
2. Copy the full contents
3. Paste into a **Custom Instruction** or **System Prompt** in your Claude setup

---

## 💬 Example Usage

```
You: I'm a frontend developer with 3 years of React experience looking for
     senior roles in Bangalore or remote. Targeting ₹20–28L. Find me jobs.

Copilot: [Runs multi-portal search across LinkedIn, Naukri, Glassdoor]
         [Scores and shortlists top 10 matching roles]
         [Flags 3 high-priority applications posted this week]

You: This one at PhonePe looks good. Here's the JD. [pastes JD]

Copilot: [Analyses JD for ATS keywords and requirements]
         [Writes tailored resume with PhonePe-specific bullet rewrites]
         [Highlights your React + performance optimisation experience]
         [Flags 2 skills to mention even though not in your profile yet]
```

---

## 📁 Skill Structure

```
job-copilot/
├── SKILL.md                           # Core skill logic (8-phase workflow)
├── README.md                          # This file
├── job-copilot.skill                  # Installable skill package
└── references/
    ├── portal-search-patterns.md      # Query syntax for 10+ portals, red flags, freshness signals
    └── resume-jd-matrix.md            # ATS keyword mapping, bullet formulas, templates by role type
```

### Reference Sources

Unlike traditional tools that scrape fixed databases, Job Copilot leverages:

- **[LinkedIn Jobs](https://linkedin.com/jobs)** — Professional network job graph
- **[Indeed](https://indeed.com)** — Largest job index globally
- **[Naukri](https://naukri.com)** — India's leading career portal
- **[Internshala](https://internshala.com)** — India's leading internship & fresher platform
- **[Glassdoor](https://glassdoor.com)** — Jobs + company reviews + salary data
- **[Wellfound](https://wellfound.com)** — Startup jobs with equity data
- **ATS Best Practices** from Greenhouse, Lever, and Workday documentation
- **STAR Interview Framework** for behavioural question coaching
- **Salary benchmarking** via real-time web search (Levels.fyi, Glassdoor, AmbitionBox)

---

## 🎯 Who This Is For

- **Job seekers** tired of sending the same resume to 100 jobs
- **Career switchers** who need to reframe experience for a new industry
- **Freshers** who need to structure internships and projects into compelling resumes
- **Senior professionals** managing multiple applications and offers simultaneously
- **Recruiters & coaches** who want a structured research and tailoring framework

---

## 📌 Part of the Claude Skills Portfolio

This skill is part of a growing portfolio of Claude skills designed to bring structured, expert-level workflows into conversational AI.

**Other skills in the portfolio:**
- 🔍 [Cybersecurity Excavator](https://github.com/yourusername/cybersecurity-excavator) — Vulnerability scanning and remediation
- 🧠 [Context Memory](https://github.com/yourusername/context-memory) — Session continuity across token resets

---

*Built with the Claude Skills Framework · Designed for Claude.ai*
