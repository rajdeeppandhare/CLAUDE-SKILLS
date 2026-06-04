# 🧾 Global Accountant — Claude Skill

> A globally-aware accounting and tax skill for Claude that handles personal tax, corporate tax, GST/VAT, financial statements, payroll, and cross-border structures — across every major jurisdiction worldwide, in plain English.

---

## ✨ What It Does

Tell the Global Accountant your country, entity type, and situation — and it will:

- 🌍 **Adapt** to your jurisdiction instantly — India, USA, UK, Australia, Canada, UAE, Singapore, and 40+ more
- 🧮 **Compute** tax liability, deductions, capital gains, GST/VAT, payroll, and more
- 📋 **Guide** you on which form to file, what documents to gather, and what deadlines to hit
- 📊 **Analyse** financial statements — P&L, Balance Sheet, Cash Flow — with ratio analysis and red flag detection
- 🌐 **Navigate** cross-border complexity — double taxation treaties, transfer pricing, PE risk, FATCA/FBAR
- 📒 **Advise** on bookkeeping, journal entries, depreciation schedules, and chart of accounts
- ✅ **Give you the pro tip** — the insight a good CA would volunteer that you didn't know to ask

---

## 🗂️ Topics It Covers

| Domain | Examples |
|---|---|
| 🧾 Personal Tax | Income computation, capital gains, deductions optimizer, tax residency, expat situations |
| 🏢 Corporate Tax | Tax rate & computation, allowable expenses, depreciation, R&D credits, loss carryforward |
| 🧮 GST / VAT / Sales Tax | Registration thresholds, input credits, filing types, reverse charge, cross-border digital services |
| 📊 Financial Statements | P&L & Balance Sheet analysis, ratio analysis, IFRS vs GAAP, red flag detection |
| 📒 Bookkeeping | Journal entries, chart of accounts, accruals, inventory valuation, bank reconciliation |
| 💼 Payroll | Gross-to-net, employer contributions, contractor vs employee, RSU/ESOP tax treatment |
| 🌍 Cross-Border | Tax treaty navigator, transfer pricing, permanent establishment, foreign tax credits, CRS/AEOI |
| 📅 Compliance | Filing deadlines by country, penalty computation, advance tax schedule, audit readiness |

**Jurisdictions**: India · USA · UK · Australia · Canada · UAE · Singapore · Germany · France · Netherlands · Ireland · New Zealand · South Africa · Hong Kong · and all OECD / G20 members

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `global-accountant.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `global-accountant/` folder
3. Rename to `global-accountant.skill`
4. Install via Settings → Skills

---

## 💬 Example Usage

**Personal Tax**
```
I'm an Indian resident with salary income of ₹18L, rental income of ₹3L,
and LTCG from mutual funds of ₹1.5L. Which ITR should I file and what
deductions can I claim?
```

**GST**
```
I run a SaaS business in Australia selling subscriptions to US customers.
Do I need to register for GST? How does it work for digital services?
```

**Financial Statement Analysis**
```
Here's my company's P&L and Balance Sheet for FY2024. Can you run a ratio
analysis and flag anything that looks concerning?
```

**Cross-Border**
```
I'm a UK resident receiving dividends from a US company. Will I be double
taxed? How does the UK-US tax treaty apply?
```

**Bookkeeping**
```
What's the journal entry for receiving a customer advance payment,
then later recognising the revenue when we deliver the service?
```

---

## 📤 Example Output

```
📍 Jurisdiction: India
🏷️ Entity Type: Individual (Resident)
📂 Topic: Personal Tax — ITR Form Selection & Deductions

You should file ITR-2 — you have salary income, rental income, and capital
gains, which rules out the simpler ITR-1 (which doesn't cover capital gains
or more than one house property).

📌 Key Numbers
| Component         | Amount     | Notes                          |
|-------------------|------------|--------------------------------|
| Gross Salary      | ₹18,00,000 | Standard deduction ₹50,000     |
| Rental Income     | ₹3,00,000  | 30% standard deduction applies |
| LTCG (MF)         | ₹1,50,000  | Exempt up to ₹1L; ₹50K taxable |
| 80C Deductions    | Up to ₹1.5L| PF, ELSS, LIC, PPF, home loan  |
| 80D (Health Ins)  | Up to ₹25K | Self + family                  |

⚠️ Watch Out For
- LTCG on equity mutual funds above ₹1L is taxed at 12.5% (Budget 2024)
- Rental income requires you to declare the property details in Schedule HP
- If you have a home loan, claim interest under Section 24(b) — up to ₹2L

✅ Action Steps
1. Collect Form 16 from employer
2. Download AIS/Form 26AS and reconcile
3. Calculate net rental income after 30% deduction
4. Compute LTCG from MF statements (CAMS/KFintech)
5. File ITR-2 before 31st July

💡 Pro Tip
Consider switching to the New Tax Regime if your total deductions are below
~₹3.75L — the breakeven point shifts with the revised slab rates. Run a
quick comparison before filing.

⚠️ This is general guidance. Consult a licensed CA for advice specific to your situation.
```

---

## 📁 Skill Structure

```
global-accountant/
├── SKILL.md                          # Core skill logic & routing
├── README.md                         # This file
├── global-accountant.skill           # Installable skill package
└── references/
    ├── deductions-by-country.md      # Deduction & credit guide by jurisdiction
    ├── tax-forms-by-country.md       # Which form to file, by country & entity
    ├── indirect-tax-guide.md         # GST / VAT / Sales Tax by country
    ├── ifrs-vs-gaap.md               # Reporting standard differences
    ├── tax-treaties-overview.md      # Double taxation treaty navigator
    └── compliance-calendar.md        # Filing deadlines & penalties by country
```

---

## 🔗 Part of the Claude Skills Portfolio

This skill is part of a growing portfolio of domain-specific Claude skills — structured, installable packages that give Claude deep expertise in specialized areas.

**Other skills in this portfolio:**
- 🔍 [Cybersecurity Excavator](../cybersecurity-excavator/) — Vulnerability scanning & security audit
- 🧠 [Context Memory](../context-memory/) — Preserve conversation context across session breaks

---

*Built with the Claude Skills framework · Designed for global CA-level coverage · Part of Raj's skill portfolio*
