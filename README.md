# CLAUDE-SKILLS

Welcome to the **CLAUDE-SKILLS** repository!

After building and tinkering with a number of full stack projects, I wanted to create a space where I could openly share useful skills for any full stack developer. The goal here is to collect and share practical, reusable skills that actually help get things done.

Think of this repo as a handy toolkit. I'll be adding more over time, so check back for updates. For now, here are the skills that have been added:

---

## Current Skills

<details>
  <summary>🧠 <strong>CAG — Claude-Augmented Generation</strong></summary>

_Short overview:_  
A full codebase ingestion and query skill for Claude that turns any GitHub repository into an intelligent knowledge base. Upload a repo URL, and CAG crawls the entire codebase, builds a structured Codebase Knowledge Map (CKM), and lets you query architecture, features, security, and performance with code-level precision.

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
A comprehensive feature planning skill for Claude that takes your current codebase and generates end-to-end implementation blueprints — from schema changes to API endpoints to frontend components.

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
  <summary>🎨 <strong>UI Architect</strong></summary>

_Short overview:_  
A comprehensive UI/UX design and component system skill for Claude that analyzes your codebase, extracts design patterns, and generates production-ready component libraries, design tokens, and style systems.

- **Key features:**
    - Analyzes existing UI code to extract design patterns, conventions, and styling approach
    - Generates component hierarchy with atomic design principles
    - Creates design token system (colors, typography, spacing, shadows)
    - Designs responsive layouts with mobile-first approach
    - Produces accessibility-first components (WCAG 2.1 AA compliant)
    - Generates component library documentation with Storybook support
    - Creates design system guidelines and usage patterns
    - Supplies copy-paste-ready component code matching your tech stack

- **Supported Frameworks:**
    - React, Vue, Svelte, Next.js, Nuxt, Angular
    - CSS: Tailwind, styled-components, CSS Modules, SCSS, Material UI
    - Component documentation: Storybook, Docusaurus

- **Key Capabilities:**
    - Brand consistency enforcement across all components
    - Design token generation (figma-compatible)
    - Responsive breakpoint strategy
    - Dark mode support
    - Animation and interaction patterns
    - Icon system design
    - Color accessibility validation

- **References & details:**
    - [ui-architect/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/ui-architect/README.md)

</details>

<details>
  <summary>🗄️ <strong>DB Architect</strong></summary>

_Short overview:_  
A database design and integration skill for Claude that analyzes your codebase and produces a complete database architecture with schema, setup instructions, and copy-paste-ready code. Drop your GitHub URL and get a firm database recommendation with zero hand-waving.

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
A defensive security skill for Claude that digs through your code, apps, and architecture to surface hidden vulnerabilities — provides plain English explanations and actionable fixes, rated by severity.

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
A full cognitive operating system for high-stakes decisions that runs your choice through 9 analytical frameworks — surfacing the dimensions you're not thinking about, mapping second-order consequences, and helping you decide with clarity instead of gut alone.

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
A comprehensive SEO audit skill for Claude that scans your pages, content, and HTML to reveal ranking opportunities. It gives plain English explanations, clear scores, and copy-paste fixes to boost your search visibility.

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
A portable conversation memory skill for Claude that snapshots your entire session into a downloadable file — so you can resume exactly where you left off, in any new chat, or even on a different device.

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
A multimedia skill for Claude that reads, transcribes, and analyses video files and YouTube links — turning raw footage into insights with no setup required. Extract frames, transcribe speech, and analyze content visually.

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
A browser automation and data science skill for Claude that turns any URL into a clean CSV — then analyzes it like a senior data scientist, complete with ML patterns, forecasting, and hypothesis testing.

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
An AI-powered market analyst skill for Claude that reads OHLCV data, computes technical indicators, detects ML-style patterns, and produces structured trading analysis with plain English reasoning.

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
A full-stack career assistant skill for Claude that searches multiple job portals, shortlists roles against your profile, writes ATS-optimized tailored resumes, and tracks your entire application pipeline.

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
A comprehensive financial skill for Claude that handles multi-currency transactions, tax compliance, invoicing, and financial reporting across different countries and jurisdictions — no setup required.

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
A portfolio of four production-ready Claude skills for marketing — copywriting, conversion rate optimization, cold email outreach, and SEO audits. Built for founders and marketers who want AI assistance that actually moves metrics.

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
A Docker containerization skill for Claude that analyzes your application, generates production-ready Dockerfiles, docker-compose configurations, and deployment instructions. Turn any project into a shippable container in minutes.

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
A community intelligence skill for Claude that mines Reddit for pain points, discovers market opportunities, and extracts real user sentiment — turning raw data into structured, actionable business intelligence.

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
A reasoning integrity and evidence verification skill for Claude that runs a structured 8-stage verification pipeline on every answer — actively hunting for unsupported claims, hidden assumptions, and contradictions before they reach you.

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
A soft fine-tuning skill for Claude that replicates what real model fine-tuning does — through structured prompting. Build a custom AI persona, inject domain knowledge, enforce rules, supply few-shot examples, and run an eval suite — all without touching a model.

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

<details>
  <summary>🔁 <strong>Stack Evolution Engine</strong></summary>

_Short overview:_  
A Staff-Engineer-grade migration skill for Claude that transforms any codebase from any stack to any stack — with a complete, production-safe blueprint tied to your actual repo. Drop in a GitHub URL and get a GO / PARTIAL GO / NO-GO verdict with a full migration roadmap.

- **Key features:**
    - 🔍 **5-Phase Evolution Pipeline** — Scan → Assess → Decide → Blueprint → Deliver
    - ⚡ **GO / PARTIAL GO / NO-GO verdict** — honest recommendation with rationale, never just "it depends"
    - 📊 **4-dimension scoring** — Complexity, Risk, Value, and Feasibility each rated /10
    - 🗺️ **Phased migration blueprint** — each phase has duration, files affected, rollback, and validation checklist
    - 🔄 **Technology mappings** — source → target equivalent for every dep, with no-equivalent hotspots flagged
    - 💥 **Breaking point analysis** — every production risk identified with mitigation and rollback trigger
    - 📋 **PR roadmap** — migration broken into independently deployable, reviewable PRs
    - 🚫 **Pushes back on bad migrations** — recommends within-stack alternatives when migration is the wrong call
    - 📦 **Exportable artefacts** — Evolution Report, JIRA/Linear tickets, PR checklists, rollback runbooks

- **Migration patterns covered:**
    - Big Bang, Strangler Fig, Parallel Run, Branch by Abstraction, Layer-by-Layer

- **Supported migration types:**
    - Languages: JavaScript → TypeScript, Python → Go, Java → Kotlin, Python → Rust
    - Frameworks: React → Next.js, Express → Fastify, Flask/FastAPI → Go, Django → FastAPI, Angular → React
    - Databases: MongoDB → PostgreSQL, MySQL → PostgreSQL, Firebase → Supabase, SQLite → PostgreSQL
    - Architecture: Monolith → Microservices, REST → GraphQL, Sync → Event-Driven
    - Infrastructure: Firebase → Supabase, Docker Compose → Kubernetes, AWS → GCP

- **References & details:**
    - [stack-evolution/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/stack-evolution/README.md)
    - [stack-evolution/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/stack-evolution/SKILL.md)
    - [stack-evolution/references/stack-mappings.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/stack-evolution/references/stack-mappings.md)
    - [stack-evolution/references/evolution-patterns.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/stack-evolution/references/stack-mappings.md)
    - [stack-evolution/references/ticket-templates.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/stack-evolution/references/ticket-templates.md)
    - [stack-evolution/references/no-migration-alternatives.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/stack-evolution/references/no-migration-alternatives.md)

</details>

<details>
  <summary>🔬 <strong>Research Excavator</strong></summary>

_Short overview:_  
A staged research pipeline skill for Claude that replaces single-pass "search and summarize" with decomposition, credibility scoring, and explicit contradiction-surfacing. Turns any research question into a structured, source-verified report — without flattening disagreement between sources into false confidence.

- **Key features:**
    - 🗂️ **8-Stage Pipeline** — Scope classification → sub-question planning → parallel evidence gathering → source credibility scoring → cross-source contradiction checking → gap-filling refinement → themed synthesis → citation audit
    - 🎯 **Scope & Intent Classification** — categorizes questions as Factual Lookup, Comparative Analysis, Open-ended Exploration, or Decision Support — and scales depth accordingly
    - 📋 **Research Planning** — decomposes questions into 3–7 sub-questions with a call budget before searching anything
    - 🌐 **Parallel Evidence Gathering** — searches per angle with no near-identical re-queries; fetches full pages when snippets are too thin
    - ⭐ **Source Credibility Scoring** — checks primary vs. secondary, recency, institutional bias, and corroboration before trusting any source
    - ⚔️ **Cross-Source Contradiction Detection** — explicitly surfaces where sources disagree instead of silently picking a side
    - 🔄 **Gap Detection & Iterative Refinement** — runs a targeted second round only on unanswered sub-questions
    - ✅ **Citation Audit** — traces every claim back to a retrieved source before sending output; cuts or re-verifies anything that can't be traced
    - 📝 **Structured Report Output** — organized by theme (not source), leading with the most decision-relevant finding

- **Best triggered by:**
    - Comparative questions ("X vs Y")
    - "What's the current state of…" overviews
    - Industry or market research
    - Decision-support questions ("should I…")
    - Fact-checking a claim
    - "Research this for me", "give me a deep dive on", "what does the evidence actually say about"

- **References & details:**
    - [research-excavator/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/research-excavator/README.md)
    - [research-excavator/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/research-excavator/SKILL.md)
    - [research-excavator/references/credibility-scoring-rubric.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/research-excavator/references/credibility-scoring-rubric.md)
    - [research-excavator/references/query-decomposition-playbook.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/research-excavator/references/query-decomposition-playbook.md)
    - [research-excavator/references/contradiction-resolution-patterns.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/research-excavator/references/contradiction-resolution-patterns.md)
    - [research-excavator/references/citation-audit-checklist.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/research-excavator/references/citation-audit-checklist.md)
    - [research-excavator/references/source-type-weighting-guide.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/research-excavator/references/source-type-weighting-guide.md)

</details>

<details>
  <summary>🚀 <strong>Founder OS</strong></summary>

_Short overview:_  
A persistent cofounder-style operating layer for Claude that holds context across your startup's decisions, risks, and roadmap — session to session. Not a Q&A tool. Runs deliberate skepticism before building anything, maintains a Founder State Document you carry between sessions, and covers the full founder workflow from idea validation to weekly execution.

- **Key features:**
    - 🧠 **Founder State Document** — a portable plain markdown file you save and re-supply each session; Claude reads it fully before responding, so context never resets to zero
    - ⚔️ **Challenge Mode** — interrogates every new idea like a skeptical investor before any building begins; gives a structured verdict: pursue / pursue with changes / avoid
    - 🔍 **Assumption Hunter** — extracts the load-bearing assumptions behind an idea and ranks them by evidence available; zero-evidence + high-importance ones get tested first
    - 👤 **Customer Discovery Engine** — defines your ICP as specifically as possible and generates non-leading interview scripts targeting your riskiest assumptions
    - 📊 **Market & Competitive Intelligence** — researches rather than guesses; bottom-up market sizing, named competitor tracking with actual feature/pricing/weakness data
    - 📋 **PRD Generator** — scoped to what's actually being built next, not an aspirational everything-doc
    - 🗺️ **Roadmap Builder** — stage-by-stage with concrete "definition of done" per stage (MVP → Beta → Pilot → Launch → Scale)
    - 🎯 **Prioritization Engine** — ranks tasks by impact, effort, risk, and revenue/retention signal
    - 📝 **Meeting Processor** — extracts decisions, action items, and risks from transcripts and folds them into the Decision Log and Risk Register
    - ⚠️ **Contradiction Detector** — catches conflicts between your PRD, pricing, decision log, and roadmap before they become real problems
    - 💰 **Burn Rate Planner** — only when you supply real numbers; never fabricates financials

- **8 operating phases:**
    - Phase 1 — Load State (reads Founder State Document before anything else)
    - Phase 2 — Challenge Mode (skeptical gate on new ideas and pivots)
    - Phase 3 — Validation & Discovery (assumption hunting, ICP, customer discovery scripts)
    - Phase 4 — Market & Competitive Intelligence
    - Phase 5 — Planning (PRD, roadmap, prioritization)
    - Phase 6 — Execution Tracking (weekly planning, progress, meeting processing)
    - Phase 7 — Risk & Consistency Audit (contradiction detection, risk register, burn rate)
    - Phase 8 — State Handoff (full updated Founder State Document, ready to save)

- **Trigger phrases:**
    - "Help me think through this idea", "Should I build this?", "Poke holes in my plan"
    - "Update my roadmap", "Process these meeting notes", "What are the risks here?"
    - "Here's my founder state doc", "Founder mode", any mention of PRD / ICP / runway / pivot

- **References & details:**
    - [founder-os/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/founder-os/README.md)
    - [founder-os/founder-os.skill](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/founder-os/founder-os.skill)
    - [founder-os/references/founder-state-schema.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/founder-os/references/founder-state-schema.md)
    - [founder-os/references/challenge-mode-playbook.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/founder-os/references/challenge-mode-playbook.md)
    - [founder-os/references/planning-templates.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/founder-os/references/planning-templates.md)
    - [founder-os/references/market-and-competitive-intelligence-playbook.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/founder-os/references/market-and-competitive-intelligence-playbook.md)
    - [founder-os/references/decision-and-risk-log-patterns.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/founder-os/references/decision-and-risk-log-patterns.md)

</details>

<details>
  <summary>🕵️ <strong>Market Gap Hunter</strong></summary>

_Short overview:_  
A three-layer competitive intelligence skill for Claude that finds underserved market segments and negative-space opportunities — not just feature checklists. Outputs ranked, scored opportunity briefs ready to drive positioning and GTM decisions.

- **Key features:**
    - 🔍 **Three-layer gap analysis** — Surface (feature matrix), Segment (who's ignored), and Negative-Space (what competitors avoid talking about)
    - 📊 **Gap Scoring formula** — `(Pain Severity × Segment Size) / Why-Nobody-Does-This Risk` kills false positives before they reach the output
    - 🪦 **Graveyard check** — mandatory for top-scoring gaps; surfaces whether a segment was tried and abandoned, and why
    - 🧠 **Segment gap signals** — case study skew, review use-case mining, geography/regulatory gaps, and persona mismatch detection
    - 🔇 **Negative-space scans** — pricing page archaeology, changelog gap analysis, job posting signals, and 1–3 star cross-competitor review mining
    - 📋 **Ranked opportunity briefs** — each gap gets a score, wedge thesis, structural explanation, counter-risk, and verification steps
    - 🎯 **Summary matrix + strategic synthesis** — top 3 gaps bolded with ACT / VALIDATE / MONITOR recommendations and a bottom-line call

- **Three layers:**
    - 🟡 **Layer 1 — Surface Gaps** — feature matrix across competitors; baseline only, never the final output
    - 🟠 **Layer 2 — Segment Gaps** — which buyer personas, verticals, geographies, and use cases are systemically ignored
    - 🔴 **Layer 3 — Negative-Space Gaps** — pricing page archaeology, 12-month changelog gaps, job posting absences, cross-competitor 1–3 star review patterns

- **References & details:**
    - [market-gap-hunter/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/market-gap-hunter/README.md)
    - [market-gap-hunter/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/market-gap-hunter/SKILL.md)
    - [market-gap-hunter/references/gap-frameworks.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/market-gap-hunter/references/gap-frameworks.md)
    - [market-gap-hunter/references/scoring-rubric.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/market-gap-hunter/references/scoring-rubric.md)
    - [market-gap-hunter/references/output-template.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/market-gap-hunter/references/output-template.md)

</details>

<details>
  <summary>🧭 <strong>Decision Audit</strong></summary>

_Short overview:_  
A post-decision quality analyzer for Claude that separates "was it a good decision" from "did it work out," and mines a running decision log for calibration errors and recurring blind spots. The natural counterpart to Decision OS — Decision OS helps you decide, Decision Audit tells you whether your deciding is actually any good.

- **Key features:**
    - ⚖️ **Decision quality vs. outcome luck** — a 2×2 classification (deserved win / bad luck / lucky break / deserved loss) so you stop reinforcing lucky bad decisions and abandoning unlucky good ones
    - 📝 **Two-pass logging schema** — captures alternatives, confidence, key assumption, reversibility, and time pressure *before* the outcome is known, then records outcome and outcome-driver separately at review time
    - 🚫 **Hindsight contamination guard** — actively interrupts and redirects if a past decision gets justified using facts only known after the fact
    - 📊 **Calibration scan** — checks whether stated confidence (1–10) actually matches real hit rates, flags systematic over/underconfidence
    - 🧩 **Domain skew scan** — flags categories of decision you're reliably bad at as a delegation signal, not a "try harder" signal
    - 🐢🐇 **Speed bias scan** — tells you if you're an overthinker or a snap-judger who should slow down
    - 🔁 **Assumption failure clustering** — names specific, recurring blind spots (e.g. "you keep assuming demand without validating it") instead of vague self-improvement advice
    - 🚨 **Lucky Break flagging** — surfaces the highest-priority risk in the whole system: flawed reasoning that happened to work and is primed to repeat

- **Six-phase pipeline:**
    - Phase 1 — Decision Capture (logging mode, before outcome is known)
    - Phase 2 — Log Maintenance (pending-review nudges)
    - Phase 3 — Outcome Recording (review mode)
    - Phase 4 — Decision Quality Scoring (scored before outcome is revealed)
    - Phase 5 — Pattern Analysis (5+ closed decisions minimum)
    - Phase 6 — Report Output (headline finding, calibration, domain breakdown, named blind spots)

- **References & details:**
    - [decision-audit/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-audit/README.md)
    - [decision-audit/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-audit/SKILL.md)
    - [decision-audit/references/log-schema.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-audit/references/log-schema.md)
    - [decision-audit/references/scoring-methodology.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-audit/references/scoring-methodology.md)
    - [decision-audit/references/pattern-analysis-playbook.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-audit/references/pattern-analysis-playbook.md)
    - [decision-audit/references/report-templates.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/decision-audit/references/report-templates.md)

</details>

<details>
  <summary>🦋 <strong>Butterfly Effect Simulator</strong></summary>

_Short overview:_  
A pre-decision consequence mapping skill for Claude that branches a decision into forking causal trees — not linear chains — tags every node with confidence, reversibility, and blast radius, and runs the counterfactual path alongside the real one. The natural pre-decision counterpart to Decision Audit: Butterfly Effect maps what's foreseeable before you act, Decision Audit grades whether your reasoning held up after.

- **Key features:**
    - 🌳 **Branching, not chaining** — every node fans out into multiple genuinely distinct downstream effects across cost, relationships, capability, and perception, instead of one cherry-picked narrative
    - 📉 **Confidence decay by depth** — first-hop mechanical facts run 80-95% confidence, by hop 3+ speculative chains often drop under 40%, so deep chains are never presented with first-hop certainty
    - 🏷️ **Four-tag node system** — Type (mechanical/behavioral/speculative), Confidence, Reversibility (easy/costly/irreversible), Blast Radius (contained/team/company/external)
    - ⚠️ **Priority flagging** — any node tagged irreversible + company/external blast radius gets surfaced explicitly regardless of depth or probability; a rare catastrophe beats a common inconvenience
    - 🔀 **Mandatory counterfactual path** — runs the inaction/alternative tree side by side with the real decision, turning the output into an actual tradeoff comparison
    - 🔄 **Feedback loop detection** — flags reinforcing loops (compounding structural traps) vs. self-correcting loops (natural dampening) across the whole tree, not just within one branch
    - 🎯 **Cheapest intervention point** — names the single highest-leverage edge in the tree where a safeguard now prevents the most dangerous downstream path
    - 🔗 **Decision Audit handoff schema** — structured output (text + JSON) maps directly onto Decision Audit's logging fields, so the predicted chain becomes the baseline graded later — no duplicate modeling work between the two skills

- **Seven-phase pipeline:**
    - Phase 1 — Decision Intake (lock in the decision, domain, and horizon)
    - Phase 2 — First-Hop Branching (mechanical layer)
    - Phase 3 — Deeper Branching (behavioral & speculative layers, confidence decay)
    - Phase 4 — Per-Node Tagging (all four tags, priority flagging)
    - Phase 5 — Counterfactual Path (inaction tree, side by side)
    - Phase 6 — Feedback Loop Detection (reinforcing vs. self-correcting)
    - Phase 7 — Intervention Point & Synthesis (concrete action + Decision Audit handoff)

- **References & details:**
    - [butterfly-effect-simulator/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/butterfly-effect-simulator/README.md)
    - [butterfly-effect-simulator/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/butterfly-effect-simulator/SKILL.md)
    - [butterfly-effect-simulator/references/tree-generation-playbook.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/butterfly-effect-simulator/references/tree-generation-playbook.md)
    - [butterfly-effect-simulator/references/tagging-rubric.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/butterfly-effect-simulator/references/tagging-rubric.md)
    - [butterfly-effect-simulator/references/feedback-loop-patterns.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/butterfly-effect-simulator/references/feedback-loop-patterns.md)
    - [butterfly-effect-simulator/references/output-schema.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/butterfly-effect-simulator/references/output-schema.md)

</details>

<details>
  <summary>🏛️ <strong>History Lens</strong></summary>

_Short overview:_  
A decision analysis engine for Claude that routes your decision to 1-2 matched company archetypes, surfaces the specific historical analog and why it worked structurally for them, strips it to the transferable principle under your actual constraints, forces disagreement between lenses to surface the real tradeoff, and adds the graveyard countercase to kill survivorship bias.

- **Key features:**
    - 🎯 **Archetype matching** — routes your decision to 1-2 genuinely relevant company lenses instead of spraying all five by default
    - 📖 **Three-layer lens analysis** — historical analog → why it worked structurally for them → transferable principle stripped of their specific advantages
    - 🪦 **Mandatory graveyard countercase** — every lens ships with the same company's relevant failure, to kill hero-worship and survivorship bias
    - ⚔️ **Lens disagreement, never resolved for you** — explicitly surfaces where two archetypes give contradictory advice and names the real underlying tradeoff
    - 🧩 **Constraint re-derivation** — re-derives what each principle actually implies under your runway, team size, and market position, not the original company's resources
    - ✅ **Single concrete recommendation** — with the tradeoff you're making spelled out, not a vibe-matched platitude
    - 🔗 **Decision Audit handoff** — packages the key assumption and chosen/rejected archetype as a log-ready entry for grading later

- **Five archetypes:**
    - 📦 **Amazon** — long-horizon bet under public pressure
    - 🍎 **Apple** — ruthless prioritization / saying no
    - 🪟 **Microsoft** — platform/ecosystem leverage
    - 🟩 **Nvidia** — betting on a thesis before the market agrees
    - 💳 **Stripe** — default infrastructure via developer obsession

- **Six-phase pipeline:**
    - Phase 1 — Decision Intake (lock in the concrete decision and its dimensions)
    - Phase 2 — Archetype Matching (select 1–2 lenses, explain why others were left out)
    - Phase 3 — Lens Analysis (historical analog, structural enabler, transferable principle, graveyard countercase)
    - Phase 4 — Lens Disagreement (surface the real tradeoff, never pick a winner)
    - Phase 5 — Constraint Re-derivation (what the principle implies under your actual constraints)
    - Phase 6 — Recommendation (concrete call + tradeoff + Decision Audit handoff)

- **References & details:**
    - [history-lens/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/history-lens/README.md)
    - [history-lens/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/history-lens/SKILL.md)
    - [history-lens/references/amazon.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/history-lens/references/amazon.md)
    - [history-lens/references/apple.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/history-lens/references/apple.md)
    - [history-lens/references/microsoft.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/history-lens/references/microsoft.md)
    - [history-lens/references/nvidia.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/history-lens/references/nvidia.md)
    - [history-lens/references/stripe.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/history-lens/references/stripe.md)

</details>
<details>
  <summary>✍️ <strong>Content Craft</strong></summary>

_Short overview:_  
A context-and-framework engine for Claude that fills the brand/audience/emotion/outcome gap before writing a single word, routes the requested format to its matching structural framework instead of winging it, filters out generic-AI phrasing, and forces multiple angled variants instead of one flat answer.

- **Key features:**
    - 🧩 **BAEO context intake** — fills Brand, Audience, Emotion, Outcome before generating instead of writing blind off a one-line brief
    - 🗺️ **Framework routing** — matches the requested format (tagline, caption, script, etc.) to its own structural formula instead of one generic template for everything
    - 🚫 **Generic-phrase filter** — actively blocks AI-tell phrasing ("take your business to the next level," "experience the difference")
    - 🎭 **Tone calibration** — maps brand personality to sentence rhythm and word choice (premium / warm / bold / luxury / playful)
    - 🔁 **Before → after punch-up mode** — when given rough draft copy, shows the rewrite next to the original and names which principle did the work
    - 🎯 **Multi-angle variant delivery** — ships 3-5 differently-angled options instead of near-duplicate answers, so picking is instinct not re-reading
- **Eight formats covered:**
    - 🏷️ **Tagline** — brand + audience + emotion compressed to one line
    - 📱 **Caption** — Hook → Emotion → Benefit → CTA
    - 🎬 **Video/Ad Script** — Problem → Ritual → Payoff → Brand line
    - 📰 **Blog Hook** — open-loop, contrarian-claim, or specific-stat openings
    - 📧 **Email Subject** — specificity-over-cleverness
    - 💼 **LinkedIn Post** — Hook → Insight → Story → Lesson → CTA
    - 🌱 **Founder Story** — Before → Struggle → Insight → Transformation → Invitation
    - 🍎 **Minimalist Voice** — Apple/Nike-style declarative, verb-first rewrites
- **Four-phase pipeline:**
    - Phase 1 — Context Intake (lock in Brand, Audience, Emotion, Outcome — ask if thin, infer if casual)
    - Phase 2 — Framework Routing (match format to its structural formula)
    - Phase 3 — Quality Pass (strip generic AI tells, calibrate tone to brand personality)
    - Phase 4 — Variant Delivery (3-5 angled options, or before→after for punch-up requests)
- **References & details:**
    - [content-craft/README.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/content-craft/README.md)
    - [content-craft/SKILL.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/content-craft/SKILL.md)
    - [content-craft/references/frameworks.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/content-craft/references/frameworks.md)
    - [content-craft/references/voice-styles.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/content-craft/references/voice-styles.md)
    - [content-craft/references/examples.md](https://github.com/rajdeeppandhare/CLAUDE-SKILLS/blob/main/content-craft/references/examples.md)
</details>

---

I'll keep adding more skills as I go, with simple overviews. Feel free to explore and use what you find useful. Suggestions for new skills or improvements are always welcome!

Stay tuned for more additions as this toolkit grows.
