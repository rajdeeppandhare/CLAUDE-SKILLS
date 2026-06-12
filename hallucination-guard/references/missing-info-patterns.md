# Missing Information Patterns — Reference

Used in Stage 4 of the Hallucination Guard pipeline.

The most common reason Claude hallucinates is attempting to answer when key
information is simply absent. This file catalogues the most frequent missing-info
patterns by domain so the pipeline can surface them quickly.

---

## How to Use This File

1. Identify the query domain
2. Find the matching pattern below
3. Check if those items are present in the conversation
4. If not → surface the gaps before answering

---

## Software Debugging

**Query type:** "Why is my app crashing / slow / broken?"

**Required for a reliable answer:**
- [ ] Error message or exception text
- [ ] Stack trace
- [ ] Platform / OS / runtime version
- [ ] Steps to reproduce
- [ ] What changed before the issue started
- [ ] Relevant code snippet

**Response template when missing:**
```
⚠️ Insufficient information to diagnose reliably.

Please share:
1. The exact error message
2. A stack trace (if available)
3. Your platform (OS, runtime, framework versions)
4. What changed before this started
```

---

## Architecture / Infrastructure

**Query type:** "How should I structure / scale / deploy X?"

**Required:**
- [ ] Current tech stack
- [ ] Scale expectations (users, requests/sec, data volume)
- [ ] Budget / hosting constraints
- [ ] Team size and expertise
- [ ] Existing infrastructure

**Common assumptions to flag:**
- Cloud provider (AWS / GCP / Azure / self-hosted?)
- Database type (relational / NoSQL / vector?)
- Auth mechanism
- CI/CD pipeline

---

## Legal

**Query type:** "Is X legal?" / "What are my rights if Y?"

**Required:**
- [ ] Jurisdiction (country, state/province)
- [ ] Entity type (individual, LLC, corporation?)
- [ ] Specific facts of the situation

**Always add:**
```
⚠️ High-Risk Domain — Legal
This is general information, not legal advice.
Laws vary significantly by jurisdiction and fact pattern.
Consult a qualified attorney for your specific situation.
```

---

## Medical

**Query type:** "What does symptom X mean?" / "Is medication Y safe?"

**Required:**
- [ ] Relevant medical history
- [ ] Current medications
- [ ] Jurisdiction (drug approval varies by country)
- [ ] Whether asking for general info vs. personal advice

**Always add:**
```
⚠️ High-Risk Domain — Medical
This is general information, not medical advice.
Consult a qualified healthcare professional for personal health decisions.
```

---

## Financial

**Query type:** "Should I invest in X?" / "Is Y a good financial move?"

**Required:**
- [ ] Risk tolerance
- [ ] Time horizon
- [ ] Current financial situation
- [ ] Jurisdiction (tax laws vary)

**Always add:**
```
⚠️ High-Risk Domain — Financial
This is general information, not financial advice.
Consult a qualified financial advisor for decisions that affect your money.
```

---

## Company / Product Information

**Query type:** "Tell me about Company X" / "What does Product Y do?"

**High-risk missing info:**
- [ ] Whether the company is public or private (private = limited verifiable data)
- [ ] Time sensitivity (company details change; Claude's training has a cutoff)
- [ ] Whether asking about features, pricing, leadership, or revenue

**Flag pattern:**
```
⚠️ Company information may be outdated.
My training data has a cutoff date. For current details on [company],
verify directly at their official website or recent press releases.
```

---

## Future Predictions

**Query type:** "When will X be released?" / "What will Y do next?"

**Key rule:** No information exists that can verify a future event.

**Response template:**
```
UNKNOWN — No public information exists about [specific future event].

SUPPORTED INFERENCE:
- [Trend that makes the outcome plausible]

SPECULATION:
- [Estimate if useful, clearly labelled]
```

---

## Data / Statistics Claims

**Query type:** Involves specific numbers, percentages, growth rates, rankings

**Required for any statistic:**
- [ ] Source name
- [ ] Publication date
- [ ] Methodology (what was measured, how, by whom)

**If not available:**
```
⚠️ I don't have a verified source for this specific figure.
Treat it as approximate and verify with [recommended source type].
```

---

## General Missing-Info Template

When the domain isn't listed above:

```
To answer this reliably I would need:
1. [Most critical missing piece]
2. [Second most critical]
3. [Any additional context]

Without this information I can offer general guidance,
but specific recommendations may not apply to your situation.
```
