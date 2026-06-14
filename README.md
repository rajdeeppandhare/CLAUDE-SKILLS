# CLAUDE-SKILLS

Welcome to the **CLAUDE-SKILLS** repository!

After building and tinkering with a number of full stack projects, I wanted to create a space where I could openly share useful skills for any full stack developer. The goal here is to collect and [...]

Think of this repo as a handy toolkit. I'll be adding more over time, so check back for updates. For now, here are the skills that have been added:

---

## Current Skills

<details>
  <summary>🧠 <strong>CAG — Claude-Augmented Generation</strong></summary>

_Short overview:_  
A full codebase ingestion and query skill for Claude that turns any GitHub repository into an intelligent knowledge base. Upload a repo URL, and CAG crawls the entire codebase, builds a structured[...]

- **Key features:**  
    - Full repository ingestion across all files and directory structure
    - Structured Codebase Knowledge Map (CKM) with modules, data models, API surface, and dependencies
    - `.cag` snapshot export — portable Markdown you can upload to any future Claude session
    - Query mode: answer architecture, feature, security, and performance questions with code precision
    - Documentation generation: README, API reference, architecture diagrams, onboarding guides
    - Security audit: systematic sweep of auth, validation, secrets, and vulnerabilities
    - Code review with quality and performance analysis
    - Feature planning with schema, endpoint, and implementation roadmaps
    - Refactor planning with safe migration steps

- **What it detects:**  
    - Tech stack and language fingerprint
    - Architecture type and patterns
    - Data models and relationships
    - API endpoints and surfaces
    - Security issues and configuration problems
    - Performance bottlenecks

- **References & details:**  
    - [CAG/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/CAG/README.md)
    - [CAG/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/CAG/SKILL.md)
    - [CAG/references/doc-templates.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/CAG/references/doc-templates.md)
    - [CAG/references/query-playbook.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/CAG/references/query-playbook.md)

</details>

<details>
  <summary>🏗️ <strong>Feature Architect</strong></summary>

_Short overview:_  
A comprehensive feature planning skill for Claude that takes your current codebase and generates end-to-end implementation blueprints — from schema changes to API endpoints to frontend component[...]

- **Key features:**
    - Analyzes your codebase to extract architecture patterns, tech stack, and conventions
    - Designs complete feature specs with user stories, acceptance criteria, and data models
    - Generates SQL schemas, API endpoints, database migrations, and ORM models
    - Creates frontend component blueprints matching your UI framework and styling approach
    - Produces implementation roadmaps with dependency order and effort estimates
    - Supplies copy-paste-ready code for every layer of the stack
    - Identifies necessary refactors or infrastructure changes upfront
    - Handles cross-cutting concerns: auth, validation, error handling, logging

- **Supported Stacks:**
    - Backends: Node.js/Express, FastAPI, Django, Rails, Laravel, Spring Boot, Go
    - ORMs: Prisma, SQLAlchemy, Mongoose, TypeORM, Sequelize, ActiveRecord
    - Frontends: React, Vue, Svelte, Next.js, Nuxt, Angular
    - Databases: PostgreSQL, MongoDB, MySQL, Firestore

- **References & details:**
    - [feature-architect/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/feature-architect/README.md)

</details>

<details>
  <summary>🗄️ <strong>DB Architect</strong></summary>

_Short overview:_  
A database design and integration skill for Claude that analyzes your codebase and produces a complete database architecture with schema, setup instructions, and copy-paste-ready code. Drop your G[...]

- **Key features:**  
    - Analyzes your codebase across data shape, query patterns, scale signals, and stack
    - Recommends the best database (PostgreSQL, MongoDB, Firebase, Supabase, Neon, PlanetScale, Redis, TimescaleDB, ClickHouse)
    - Generates full schemas in SQL, Prisma, Mongoose, or Firestore formats
    - Provides connection boilerplate and environment templates
    - Delivers step-by-step SETUP.md with provisioning, migration, and verification steps
    - No generic advice — firm, reasoned recommendations based on your actual code
- **Databases supported:**  
    - PostgreSQL, MongoDB Atlas, Firebase Firestore, Supabase, Neon, PlanetScale, Redis, TimescaleDB, ClickHouse
- **References & details:**  
    - [db-architect/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/DB-Archietect/README.md)

</details>

<details>
  <summary>🔍 <strong>Cybersecurity Excavator</strong></summary>

_Short overview:_  
A defensive security skill for Claude that digs through your code, apps, and architecture to surface hidden vulnerabilities — provides plain English explanations and actionable fixes, rated by s[...]

- **Key features:**  
    - Scans for vulnerabilities in source code, configs, and architectures  
    - Ranks findings by severity: Critical / High / Medium / Low  
    - Explains issues clearly, not just with jargon  
    - Suggests concrete code and config fixes  
    - Supports: JavaScript/Node.js, Python, PHP, Java, Go, SQL, Docker, K8s, nginx, AWS configs, and more  
- **References & details:**  
    - [cybersecurity-excavator/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/cybersecurity-excavator/README.md)

</details>

<details>
  <summary>🎯 <strong>Decision OS</strong></summary>

_Short overview:_  
A full cognitive operating system for high-stakes decisions that runs your choice through 9 analytical frameworks — surfacing the dimensions you're not thinking about, mapping second-order consequences, and flagging cognitive biases that might be distorting your judgment.

- **Key features:**
    - 9-stage analytical pipeline: decision intake, weighted matrix, opportunity cost, risk scoring, time/energy analysis, regret minimization, identity alignment, 2nd-order consequences, pre-mortem analysis
    - Weighted Decision Matrix — score options across criteria that matter to you
    - Opportunity Cost Analysis — what are you permanently giving up?
    - Regret Minimisation Framework — project to age 80 and look back
    - Identity & Values Alignment — does this fit who you're becoming?
    - Second-Order Consequence Mapping — what happens 3 moves ahead, not just 1?
    - Pre-Mortem Analysis — assume failure and work backwards to surface risks
    - Cognitive Bias Detection — names and counters the mental distortions in how you're framing the choice
    - Adaptive depth — adjusts pipeline depth based on decision stakes and reversibility

- **Works for:**
    - Career decisions (job offers, startup vs employment, MBA decisions)
    - Life changes (relocation, relationship decisions, major purchases)
    - Business decisions (build vs buy, raise funding, hire vs outsource)
    - Any high-stakes fork-in-the-road choice

- **References & details:**
    - [decision-os/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-os/README.md)
    - [decision-os/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-os/SKILL.md)

</details>

<details>
  <summary>🔍 <strong>SEO Manager</strong></summary>

_Short overview:_  
A comprehensive SEO audit skill for Claude that scans your pages, content, and HTML to reveal ranking opportunities. It gives plain English explanations, clear scores, and copy-paste fixes to boo[...]

- **Key features:**
    - Audits on-page, technical, and content SEO signals
    - Scores your page and provides a full breakdown
    - Explains every issue and its ranking impact
    - Offers exact HTML/code fixes for each problem
    - Prioritizes what to fix first
    - Highlights keyword opportunities

- **References & details:**
    - [seo-manager/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/seo-manager/README.md)

</details>

<details>
  <summary>🧢🚫 <strong>No Cap — Brutally Honest Gen Z Advisor</strong></summary>

_Short overview:_  
A Claude skill that gives brutally honest feedback with Gen Z vibes. Real talk, specific, concise, and genuinely helpful—no sugarcoating, just actionable advice with personality.

- **Key features:**
    - Vibe checks your ideas, writing, code, or decisions
    - Speaks in authentic Gen Z slang (ngl, no cap, mid, bussin)
    - Gives specific, direct, and concise feedback (no essays!)
    - Offers concrete fixes for every problem
    - Earned hype only—won't hype you up unless you deserve it

- **References & details:**
    - [no-cap (on god bro)/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/no-cap%20(on%20god%20bro)/README.md)

</details>

<details>
  <summary>🧠 <strong>Context Memory</strong></summary>

_Short overview:_  
A portable conversation memory skill for Claude that snapshots your entire session into a downloadable file — so you can resume exactly where you left off, in any new chat, or even on a differe[...]

- **Key features:**
    - 🗺️ Maps your entire session — goals, decisions, code, issues, dead ends
    - 📥 Exports a clean Markdown file you can download anytime
    - 🔄 Resumes instantly — upload the file to any new chat and pick up mid-sentence
    - 🤖 Cross-model — works with any Claude instance, or any AI that can read Markdown
    - ⚡ Proactive — Claude can offer a checkpoint automatically when sessions get long

- **References & details:**
    - [context-memory/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/context-memory/README.md)

</details>

<details>
  <summary>🎬 <strong>Video Input</strong></summary>

_Short overview:_  
A multimedia skill for Claude that reads, transcribes, and analyses video files and YouTube links — turning raw footage into insights with no setup required. Extract frames, transcribe speech, [...]

- **Key features:**
    - 🖼️ Extracts frames — pulls evenly-spaced screenshots throughout the video
    - 🎙️ Transcribes speech — converts audio to accurate, timestamped text via Whisper
    - 🔍 Analyzes content — uses Claude Vision to describe, summarize, and interpret footage
    - 🎯 Detects objects, scenes & events — identifies people, items, scene changes, and actions
    - 🌐 Downloads from URLs — works with YouTube, Vimeo, and direct video links

- **References & details:**
    - [Video Input/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/Video%20Input/README.md)

</details>

<details>
  <summary>🕷️ <strong>Web Scraper Analyst</strong></summary>

_Short overview:_  
A browser automation and data science skill for Claude that turns any URL into a clean CSV — then analyzes it like a senior data scientist, complete with ML patterns, forecasting, and hypothesi[...]

- **Key features:**
    - 🌐 **Scrape** any website using Playwright — static pages, JS-heavy SPAs, e-commerce, news feeds, dashboards
    - 📄 **Follow pagination** automatically — next buttons, URL params, infinite scroll, load-more
    - 📦 **Output a clean CSV** — typed columns, deduped rows, ISO dates, metadata included
    - 📊 **Analyze like a data scientist** — descriptive stats, distributions, correlations, outlier detection
    - 🤖 **Find ML patterns** — clustering, feature importance, multivariate anomaly detection
    - 📅 **Forecast trends** — time series analysis, linear extrapolation, confidence intervals
    - 🧪 **Test hypotheses** — auto-selects the right statistical test (t-test, Mann-Whitney, ANOVA, chi-square)
    - 🐍 **Give you runnable code** — complete Playwright scraper + pandas analysis scripts, ready to execute

- **References & details:**
    - [Web Scraper Analyst/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/Web%20Scraper%20Analyst/README.md)

</details>

<details>
  <summary>📈 <strong>Stock Agent</strong></summary>

_Short overview:_  
An AI-powered market analyst skill for Claude that reads OHLCV data, computes technical indicators, detects ML-style patterns, and produces structured trading analysis with plain English reasonin[...]

- **Key features:**
    - 📊 Scans price data for technical signals across trend, momentum, volatility, and volume
    - 🤖 Detects candlestick and chart patterns using ML-style probabilistic logic
    - 💡 Assists trade decisions with entry zones, stop-loss, take-profit, and R:R ratios
    - 🔬 Backtests simple rule-based strategies against historical data
    - 📖 Explains every signal in plain English — not just the number, but what it means

- **References & details:**
    - [stock-agent/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/stock-agent/README.md)

</details>

<details>
  <summary>💼 <strong>Job Copilot</strong></summary>

_Short overview:_  
A full-stack career assistant skill for Claude that searches multiple job portals, shortlists roles against your profile, writes ATS-optimized tailored resumes, and tracks your entire application[...]

- **Key features:**
    - 🔍 Searches LinkedIn, Indeed, Naukri, Monster, Internshala, Glassdoor, Wellfound & more — simultaneously
    - 🎯 Shortlists jobs by scoring them against your skills, location, salary, and preferences
    - 📄 Writes tailored, ATS-optimized resume for each specific job description — not generic
    - 📬 Drafts bespoke cover letters per application
    - 📊 Tracks entire pipeline: Applied → Screening → Interview → Offer
    - 🎤 Preps you for interviews with role-specific questions + coached STAR answers
    - 💰 Benchmarks offers and gives you a negotiation script

- **References & details:**
    - [job-copilot/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/job-copilot/README.md)

</details>

<details>
  <summary>🌍 <strong>Global Accountant</strong></summary>

_Short overview:_  
A comprehensive financial skill for Claude that handles multi-currency transactions, tax compliance, invoicing, and financial reporting across different countries and jurisdictions — no setup r[...]

- **Key features:**
    - 💱 Multi-currency conversion with real-time exchange rates
    - 📊 Invoice generation and payment tracking
    - 📈 Financial reports and analytics
    - 🗂️ Tax compliance across jurisdictions
    - 💰 Expense tracking and budgeting
    - 🔍 Audit-ready documentation and records

- **References & details:**
    - [global-accountant/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/global-accountant/README.md)

</details>

<details>
  <summary>📣 <strong>Marketing Skills Collection</strong></summary>

_Short overview:_  
A portfolio of four production-ready Claude skills for marketing — copywriting, conversion rate optimization, cold email outreach, and SEO audits. Built for founders and marketers who want AI a[...]

- **Skills included:**
    - 📝 **Copywriting** — Writes and rewrites marketing copy for any page (homepage, landing page, pricing, feature pages)
    - 📈 **CRO** — Audits pages and flows for conversion problems, diagnoses root causes, prioritizes fixes by ICE score
    - 📧 **Cold Email** — Writes B2B cold email sequences that are personalized, short, human, and built to get replies
    - 🔍 **SEO Audit** — Audits sites for SEO issues across indexing, technical, on-page, and authority with keyword strategy

- **Key features:**
    - Integrated workflow: SEO audit → copywriting → CRO → cold email
    - Reference docs for each skill with frameworks, checklists, and swipe files
    - All skills check for product context before working
    - Built for real-world marketing use cases

- **References & details:**
    - [marketing-skills/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/marketing-skills/README.md)
    - [Copywriting Skill](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/marketing-skills/copywriting/)
    - [CRO Skill](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/marketing-skills/cro/)
    - [Cold Email Skill](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/marketing-skills/cold-email/)
    - [SEO Audit Skill](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/marketing-skills/seo-audit/)

</details>

<details>
  <summary>🐳 <strong>Docker Ship</strong></summary>

_Short overview:_  
A Docker containerization skill for Claude that analyzes your application, generates production-ready Dockerfiles, docker-compose configurations, and deployment instructions. Turn any project int[...]

- **Key features:**
    - 🔍 Analyzes your codebase to detect tech stack, dependencies, and runtime requirements
    - 📝 Generates optimized, multi-stage Dockerfiles with best practices
    - 🗂️ Creates docker-compose.yml for multi-service orchestration
    - 🚀 Produces deployment guides for various platforms (AWS, GCP, Azure, DigitalOcean, Heroku)
    - 🔒 Implements security best practices (non-root users, minimal base images, vulnerability scanning)
    - ⚡ Optimizes for layer caching and minimal image size
    - 📊 Includes .dockerignore and environment templates

- **References & details:**
    - [Docker Ship/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/Docker%20Ship/README.md)

</details>

<details>
  <summary>🔥 <strong>Reddit Miner</strong></summary>

_Short overview:_  
A community intelligence skill for Claude that mines Reddit for pain points, discovers market opportunities, and extracts real user sentiment — turning raw data into structured, actionable busi[...]

- **Key features:**
    - 🎯 **8 Intelligence Modes** — Startup/Market, Product Manager, SaaS Founder, UX Research, Content Creator, Career Intelligence, Learning Path, Trading/Finance
    - 🌐 **Fetches real Reddit data** via public API — posts, comments, scores, timestamps
    - 🧠 **Synthesizes signals** — clusters pain points, friction, workarounds, wish-lists, and abandonment patterns
    - 💡 **Opportunity scoring** — ranks discoveries across frequency, intensity, market readiness, competition gap, and trend direction
    - 📊 **Structured reporting** — evidence-grounded insights cited to real threads and upvote counts
    - 🔬 **No hallucinations** — fails gracefully when data is unavailable, aware of Reddit demographic biases

- **8 Modes:**
    - 🚀 **Startup / Market** — Discover opportunities in any topic or industry
    - 📦 **Product Manager** — Feature requests, user sentiment, roadmap signals
    - 🏗️ **SaaS Founder** — Competitor weaknesses, market gaps, pricing intelligence
    - 🔬 **UX Research** — Friction points, churn triggers, mental model mismatches
    - 📣 **Content Creator** — YouTube/newsletter ideas, viral angles, audience language
    - 💼 **Career Intelligence** — Role reality, compensation, interview experiences
    - 📚 **Learning Path** — Community-validated resources, sticking points, fastest paths
    - 📈 **Trading / Finance** — Loss patterns, psychology traps, community sentiment

- **References & details:**
    - [Reddit Miner/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/Reddit%20Miner/README.md)

</details>

<details>
  <summary>🛡️ <strong>Hallucination Guard</strong></summary>

_Short overview:_  
A reasoning integrity and evidence verification skill for Claude that runs a structured 8-stage verification pipeline on every answer — actively hunting for unsupported claims, hidden assumptio[...]

- **Key features:**
    - 🔍 **Fabrication Risk Assessment** — Assigns risk tier (Low/Medium/High) based on query domain
    - 📋 **Evidence Audit** — Traces every claim to a source, removes invented details
    - 🔎 **Assumption Scanner** — Lists hidden assumptions, converts facts into conditional statements
    - ⚠️ **Missing Information Check** — Identifies what's needed for reliable answers, surfaces gaps
    - ⚔️ **Contradiction Detector** — Compares draft against prior messages and constraints
    - 🔄 **Alternative Explanation Generator** — Produces ≥3 alternatives before committing
    - 📊 **Confidence Scoring** — Categorizes claims as Verified (95–100%), Supported (80–94%), Plausible (50–79%), or Speculative (0–49%)
    - ✅ **Final Response** — Annotated answers with assumptions, confidence levels, gaps, and resolved conflicts

- **Claim Classification:**
    - ✅ **VERIFIED** — Directly evidenced by user input or verifiable knowledge
    - 🟢 **SUPPORTED INFERENCE** — Reasonable deduction from available evidence
    - 🟡 **SPECULATION** — Possible but lacking meaningful support
    - 🔴 **UNKNOWN** — No evidence available; not guessable

- **Fabrication Risk Tiers:**
    - 🟢 **Low** — Math, programming syntax, established science
    - 🟡 **Medium** — Business advice, tech recommendations, product comparisons
    - 🔴 **High** — Legal, medical, financial, news, company data, future events

- **References & details:**
    - [hallucination-guard/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/hallucination-guard/README.md)
    - [hallucination-guard/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/hallucination-guard/SKILL.md)

</details>

<details>
  <summary>🧬 <strong>Fine-Tuning Simulator</strong></summary>

_Short overview:_  
A soft fine-tuning skill for Claude that replicates what real model fine-tuning does — through structured prompting. Build a custom AI persona, inject domain knowledge, enforce rules, supply few-shot examples, and control output format — all saved as a portable `.ft-config` file you can reload any time. No compute bill, no MLOps, no waiting.

- **Key features:**
    - 🎭 **Persona Builder** — Name, voice, background, and target user — fully customisable
    - 📚 **Knowledge Injection** — Domain terms, heuristics, and reference facts baked into the session
    - 📏 **Rule Enforcement** — Hard rules (never do), soft rules (prefer/avoid), compliance constraints, and escalation logic
    - 💡 **Few-Shot Examples** — Positive and negative demonstrations that shape exact response behaviour
    - 📐 **Format Control** — Response length, markdown, structure, opening/closing, and tone calibration
    - 🧪 **Eval Suite** — Built-in consistency scoring across persona, rules, knowledge, format, and style (each /10)
    - 💾 **Reusable `.ft-config`** — Save your config and reload it in any future session without rebuilding
    - 🔄 **Iteration Commands** — `/ft-tweak`, `/ft-add-rule`, `/ft-compare`, `/ft-eval` and more

- **Works for:**
    - SaaS companies building a consistent support or sales persona
    - Agencies encoding client brand voice into a reusable config
    - Educators creating subject-specific tutors with controlled pedagogy
    - Founders prototyping a fine-tuned model before spending on real fine-tuning
    - Compliance teams enforcing regulatory guardrails on every response

- **Session Commands:**
    - `/ft-status` — Review the full active config
    - `/ft-eval` — Run the consistency evaluation suite with scores
    - `/ft-compare` — Same prompt: base Claude vs. your simulated model, side by side
    - `/ft-tweak [layer] [change]` — Modify one layer without rebuilding
    - `/ft-export` — Re-output the config for saving
    - `/ft-reset` — Return to standard Claude

- **References & details:**
    - [fine-tuning-simulator/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/fine-tuning-simulator/README.md)
    - [fine-tuning-simulator/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/fine-tuning-simulator/SKILL.md)
    - [fine-tuning-simulator/references/domain-knowledge-patterns.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/fine-tuning-simulator/references/domain-knowledge-patterns.md)
    - [fine-tuning-simulator/references/persona-archetypes.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/fine-tuning-simulator/references/persona-archetypes.md)

</details>

---

I'll keep adding more skills as I go, with simple overviews. Feel free to explore and use what you find useful. Suggestions for new skills or improvements are always welcome!

Stay tuned for more additions as this toolkit grows.
