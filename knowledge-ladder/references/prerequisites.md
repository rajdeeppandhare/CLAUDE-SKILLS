# Prerequisite Chains by Domain

Use this when a user asks about a topic that has meaningful prerequisite dependencies. Scan the relevant domain section, identify gaps, and surface them with the navigation footer or inline micro-explanations.

**Rule:** Only flag prerequisites that would genuinely block understanding at the user's current level. Don't list everything — list the one or two that matter most right now.

---

## AI / Machine Learning

```
Linear Algebra (vectors, matrices, dot product)
  └─ Calculus (derivatives, chain rule)
      └─ Probability & Statistics (distributions, expectation, Bayes)
          └─ Optimization Theory (gradient descent, convexity)
              └─ Machine Learning Fundamentals (supervised/unsupervised, loss, training)
                  ├─ Deep Learning (neural nets, backprop, activations)
                  │   ├─ CNNs (image)
                  │   ├─ RNNs / LSTMs (sequences)
                  │   └─ Transformers / Attention (NLP, multimodal)
                  │       └─ LLMs (GPT, Claude, BERT family)
                  └─ Classical ML (regression, trees, SVMs, ensembles)
```

**Common gaps:** Users asking about LLMs often lack: probability basics, what "training" means at a high level, difference between parameters and hyperparameters.

---

## Quantum Computing

```
Complex Numbers (i, magnitude, phase)
  └─ Linear Algebra (vector spaces, eigenvalues)
      └─ Probability (superposition framing)
          └─ Classical Computing Basics (bits, gates, circuits)
              └─ Quantum Mechanics Basics (superposition, entanglement, measurement)
                  └─ Quantum Gates & Circuits
                      └─ Quantum Algorithms (Grover, Shor, VQE)
                          └─ Quantum Error Correction
```

**Estimated path from zero:** 120–200 hours. Difficulty: 9/10.

---

## Finance / Investing

```
Basic Arithmetic & Percentages
  └─ Time Value of Money (PV, FV, discount rate)
      └─ Financial Statements (P&L, balance sheet, cash flow)
          ├─ Equity Analysis (valuation, DCF, multiples)
          │   └─ Portfolio Theory (diversification, Sharpe ratio)
          │       └─ Derivatives & Options (Black-Scholes, Greeks)
          └─ Fixed Income (bonds, yield, duration)
              └─ Macro / Rates (central banks, yield curve)
```

**Common gaps:** Users asking about options often lack: what a stock actually is, what "premium" means, basic probability framing.

---

## Physics

```
Algebra & Trigonometry
  └─ Calculus (derivatives, integrals)
      ├─ Classical Mechanics (Newton's laws, energy, momentum)
      │   └─ Thermodynamics (entropy, heat, work)
      │       └─ Statistical Mechanics
      ├─ Electromagnetism (Maxwell's equations)
      │   └─ Optics & Waves
      └─ Quantum Mechanics (Schrödinger, operators, measurement)
          └─ Quantum Field Theory
              └─ Standard Model / String Theory
```

---

## Computer Science / Software

```
Basic Logic (AND, OR, NOT)
  └─ Programming Fundamentals (variables, loops, functions)
      └─ Data Structures (arrays, trees, graphs, hash maps)
          └─ Algorithms (sorting, search, complexity)
              ├─ Systems (OS, memory, concurrency)
              │   └─ Networking (TCP/IP, HTTP, DNS)
              │       └─ Distributed Systems (CAP, consensus, replication)
              └─ Databases (SQL, indexing, transactions, ACID)
                  └─ System Design (scalability, caching, load balancing)
```

---

## Biology / Genetics

```
Cell Biology Basics (cell structure, DNA, RNA, proteins)
  └─ Molecular Biology (transcription, translation, gene expression)
      └─ Genetics (Mendelian, mutations, inheritance)
          └─ Genomics (sequencing, GWAS, epigenetics)
              └─ Biotechnology (CRISPR, PCR, gene therapy)
```

---

## Economics

```
Supply & Demand
  └─ Market Structures (competition, monopoly, oligopoly)
      ├─ Microeconomics (incentives, game theory, welfare)
      └─ Macroeconomics (GDP, inflation, monetary/fiscal policy)
          └─ International Economics (trade, exchange rates, BoP)
              └─ Development Economics / Political Economy
```

---

## Statistics

```
Basic Probability (events, outcomes, Bayes theorem)
  └─ Distributions (normal, binomial, Poisson)
      └─ Descriptive Statistics (mean, variance, correlation)
          └─ Inferential Statistics (hypothesis testing, p-values, CIs)
              └─ Regression (linear, logistic, assumptions)
                  └─ Bayesian Statistics (priors, posteriors, MCMC)
                      └─ Causal Inference (DAGs, RCTs, IV, DiD)
```

**Common gaps:** Users asking about p-values often lack: what a null hypothesis is, what a distribution is, why we sample.
