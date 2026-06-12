# Hallucination Taxonomy — Reference

A catalogue of hallucination types Claude is prone to, with prevention tactics
for each. Use this as a diagnostic tool when auditing a draft response.

---

## Type 1 — Fabricated Facts

**What it is:** Claude invents a specific detail (statistic, name, date, quote,
version number) that sounds plausible but has no basis in the provided context
or verifiable knowledge.

**Classic example:**
> "According to a 2023 MIT study, 73% of developers prefer TypeScript."

(No such study was cited. The number is invented.)

**Why it happens:** Pattern-matching on training data produces confident-sounding
completions even when the underlying fact doesn't exist.

**Prevention:**
- Stage 2 (Evidence Audit): require a traceable source for every statistic
- Flag all specific numbers, percentages, and study citations as Tier 3/4
  unless the user provided them
- Never fabricate a citation — say "I don't have a source for this"

---

## Type 2 — Stale Knowledge

**What it is:** Claude states something that was true at training time but has
since changed — a product version, a company's leadership, a law, a price.

**Classic example:**
> "The current CEO of Twitter is Jack Dorsey."

**Why it happens:** Training data has a cutoff. Claude doesn't always flag when
a claim might be stale.

**Prevention:**
- Stage 1 (Risk Assessment): fast-changing domains (company info, news, software
  versions, prices) → High Risk tier
- Always add a recency caveat: "As of my knowledge cutoff, … verify current status"
- For software versions, defer to official documentation

---

## Type 3 — Hidden Assumption

**What it is:** Claude assumes an unstated fact about the user's context and
builds an answer on it — without flagging it as an assumption.

**Classic example:**
User: "How do I connect to my database?"
Claude: "Add your DATABASE_URL to your .env file in your AWS Lambda environment…"

(User never mentioned AWS Lambda.)

**Why it happens:** Ambiguous prompts leave gaps; Claude fills them with the
most statistically common scenario from training data.

**Prevention:**
- Stage 3 (Assumption Scanner): list assumptions before answering
- Use conditional framing: "If you're using AWS Lambda…"
- Ask for clarification when the assumption materially changes the answer

---

## Type 4 — Confident Speculation

**What it is:** Claude presents a speculation, prediction, or uncertain inference
with the same confidence level as a verified fact.

**Classic example:**
> "GPT-6 will be released in Q3 2025 with multimodal capabilities."

**Why it happens:** Speculative completions feel smooth and helpful. The model
optimises for fluency, which sometimes masks epistemic uncertainty.

**Prevention:**
- Stage 7 (Confidence Scoring): all predictions → Tier 4 (🔴 Speculative)
- Use UNKNOWN classification when no evidence exists
- Reward "I don't know" over a confident-sounding guess

---

## Type 5 — Overgeneralisation

**What it is:** Claude applies a rule that is true in common cases to a situation
where it doesn't hold.

**Classic example:**
> "REST APIs are always stateless."

(True by convention, but not enforced; many real-world APIs maintain session state.)

**Why it happens:** Training data over-represents textbook explanations which
tend to state rules absolutely.

**Prevention:**
- Stage 6 (Alternative Explanation Generator): check for exceptions before stating
  a rule universally
- Hedge generalisations: "REST APIs are typically stateless, though implementations vary"

---

## Type 6 — Contradiction

**What it is:** Claude's answer conflicts with something stated earlier in the
conversation — often because earlier context scrolled out of focus.

**Classic example:**
Earlier in chat: "We're using MongoDB."
Later: "You can add a foreign key constraint to enforce referential integrity."

(MongoDB doesn't support foreign key constraints in the traditional SQL sense.)

**Why it happens:** Long contexts dilute early constraints. New completions are
generated without full re-reading of prior messages.

**Prevention:**
- Stage 5 (Contradiction Detector): explicitly compare draft against prior decisions
- For architecture discussions, maintain a short "established facts" list at the
  top of the analysis

---

## Type 7 — Tunnel Vision

**What it is:** Claude latches onto one explanation or solution and presents it
as the answer without considering alternatives.

**Classic example:**
User: "My app is slow."
Claude: "You should add database indexes." ← stated without considering other causes

**Why it happens:** The first plausible completion path gets reinforced through
generation.

**Prevention:**
- Stage 6 (Alternative Explanation Generator): always generate ≥ 3 alternatives
  for diagnostic questions
- Only commit to one after comparing against available evidence

---

## Type 8 — Source Fabrication

**What it is:** Claude invents a paper, article, book, or official document —
complete with author names, publication dates, and page numbers.

**Classic example:**
> "This is documented in RFC 9214, Section 3.2."

(RFC 9214 may not exist or may not contain that content.)

**Why it happens:** Citations make answers feel more authoritative. The model
learns this pattern and reproduces it even without a real source.

**Prevention:**
- Never generate a citation unless it appeared in the user's prompt or a tool result
- For technical standards: say "check the official RFC / spec" rather than citing
  a specific number you haven't verified
- If uncertain: "I don't have a source for this — please verify before relying on it"

---

## Quick Diagnostic Checklist

When reviewing a draft answer, ask:

- [ ] Does any claim include a specific number, name, or date I can't trace to the user or a verified source?
- [ ] Could any claim be stale since my training cutoff?
- [ ] Am I assuming anything about the user's tech stack, location, role, or context?
- [ ] Am I presenting a speculation as if it were a fact?
- [ ] Is any generalisation actually only true in common cases?
- [ ] Does any recommendation contradict something established earlier?
- [ ] Did I consider more than one possible explanation?
- [ ] Am I citing a source I may have invented?

One "yes" → revisit that section before responding.
