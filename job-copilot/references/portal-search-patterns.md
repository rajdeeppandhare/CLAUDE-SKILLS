# Portal Search Patterns & Shortlisting Signals

Reference guide for Job Copilot — query construction, portal behaviour, and shortlisting logic.

---

## Table of Contents

1. [Portal Query Construction](#1-portal-query-construction)
2. [Portal-Specific Behaviours & Tips](#2-portal-specific-behaviours--tips)
3. [Shortlisting Signals](#3-shortlisting-signals)
4. [Red Flag Patterns](#4-red-flag-patterns)
5. [Freshness & Recency Signals](#5-freshness--recency-signals)

---

## 1. Portal Query Construction

### LinkedIn Jobs
```
site:linkedin.com/jobs/view [job title] [city OR "remote"] [skill]
site:linkedin.com/jobs [title] [company size hint] posted:past-week

Examples:
site:linkedin.com/jobs/view "product manager" "bangalore" "b2b saas"
site:linkedin.com/jobs "senior data engineer" "remote" "spark" "python"
```
**Tips**: LinkedIn de-ranks aggregator scrapes. Add company names for targeted search.

### Indeed
```
site:indeed.com/viewjob [title] [location] [years]
site:indeed.com/jobs [title] [industry] [employment type]

Examples:
site:indeed.com/viewjob "frontend developer" "london" "react"
site:indeed.com/jobs "marketing manager" "fmcg" "full-time"
```
**Tips**: Indeed indexes salary more reliably. Add salary hints for senior roles.

### Naukri (India)
```
site:naukri.com [title] [location] [experience range] [skills]

Examples:
site:naukri.com "java developer" "pune" "3-5 years" "microservices"
site:naukri.com "business analyst" "mumbai" "banking"
```
**Tips**: Naukri titles are often inflated. Map "Team Lead" to Senior individual contributor level.

### Monster
```
site:monster.com/jobs [title] [location] [keyword]

Examples:
site:monster.com/jobs "devops engineer" "hyderabad" "kubernetes"
```

### Internshala
```
site:internshala.com/internship/detail [field] [skills]
site:internshala.com/jobs/detail [title] [skills]

Examples:
site:internshala.com/internship/detail "machine learning" "python" "tensorflow"
site:internshala.com/jobs/detail "ui ux designer" "figma"
```
**Tips**: Internshala has both internships AND full-time "fresher jobs". Distinguish by URL path.

### Glassdoor
```
site:glassdoor.com/job-listing [title] [company] [location]

Examples:
site:glassdoor.com/job-listing "data scientist" "fintech" "new york"
```
**Tips**: Glassdoor listings include review data — pull company rating when available.

### AngelList / Wellfound
```
site:wellfound.com/jobs [title] [stage] [skills]

Examples:
site:wellfound.com/jobs "backend engineer" "series-a" "golang"
site:wellfound.com/jobs "growth marketer" "b2c" "saas"
```
**Tips**: Equity info is often listed. Note vesting schedules when extracting.

### Dice (US Tech)
```
site:dice.com/jobs [title] [skill] [employment-type]

Examples:
site:dice.com/jobs "cloud architect" "aws" "contract"
```

### We Work Remotely
```
site:weworkremotely.com/remote-jobs [category] [title]

Examples:
site:weworkremotely.com/remote-jobs/listings "rails developer"
```

### Remotive
```
site:remotive.com/remote-jobs [category] [title]
```

### Otta (UK / EU)
```
site:otta.com/jobs [title] [skills]
```

---

## 2. Portal-Specific Behaviours & Tips

| Portal | Salary Data | Remote Filter | Freshness | Notes |
|---|---|---|---|---|
| LinkedIn | Estimated only | Yes (filter) | Past 24h filter | Best for referral context |
| Indeed | Often listed | Yes | Date posted filter | Highest volume |
| Naukri | Required field | Partial | Date filter | India-best for experienced |
| Internshala | Fixed/stipend | Partial | "Recently posted" | Best for 0–2 yrs |
| Glassdoor | Estimated range | Yes | Good | Dual use: ratings + jobs |
| Wellfound | Equity shown | Yes | Real-time | Startup-only |
| Dice | Contract rates | Yes | Good | US contract specialists |
| WWR | N/A | Remote-only | Weekly digest | Curated quality |

---

## 3. Shortlisting Signals

### Positive Signals (boost score)
- Title matches target role verbatim or with ≤1 seniority gap
- 70%+ skills overlap with candidate profile
- Company in preferred industry
- Posted within 7 days (< 3 days = high priority)
- Salary range includes or exceeds target
- Remote/hybrid when candidate prefers it
- Company Glassdoor rating ≥ 3.8
- Growth-stage company (Series B–D) if candidate targets startups
- Role reports to VP or above (for mid-senior candidates — signals scope)

### Negative Signals (reduce score or flag)
- Salary below target by >20%
- Posted >30 days ago (likely filled or slow process)
- "Multiple openings" with no team context (often mass hire / churn signal)
- Contract when FT preferred (or vice versa)
- Requires relocation when remote preferred
- Vague JD with no concrete responsibilities
- No company name listed (often recruiter spam)

---

## 4. Red Flag Patterns

Flag these in shortlist output with ⚠️:

| Pattern | Likely Issue |
|---|---|
| "Competitive salary" with no range | Salary likely below market |
| "Immediate joiner only" | Desperation or high churn |
| JD copy-pasted from 3 different roles | Poorly defined role / no clarity |
| "We work hard and play harder" culture language | Burnout culture risk |
| No LinkedIn company page | Legitimacy concern |
| Asks for personal documents upfront | Scam risk |
| JD requires 10+ yrs for junior title | Exploitative or unrealistic |
| "Unlimited PTO" without structure | Often means no PTO culture |

---

## 5. Freshness & Recency Signals

Prioritise recency in output:

- **< 3 days**: 🟢 Apply immediately
- **3–7 days**: 🟡 Apply within 48 hrs
- **8–14 days**: 🟠 Still worth applying
- **15–30 days**: ⚠️ Check if still open
- **> 30 days**: 🔴 May be filled — verify before investing time

When date isn't extractable from search snippet, note "Date unknown — verify on portal".
