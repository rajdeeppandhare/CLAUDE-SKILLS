# Decision & Risk Log Patterns

Used in Phases 6 and 7 of Founder OS. Contains entry formats for the Decision Log, the Contradiction Detector, and the Risk Register. Load this when processing meeting notes, running a consistency audit, or updating the Founder State Document's risk and decision sections.

---

## Table of Contents

1. [Decision Log Entries](#decision-log-entries)
2. [Risk Register Entries](#risk-register-entries)
3. [Contradiction Detector](#contradiction-detector)
4. [Meeting Processor Output Format](#meeting-processor-output-format)
5. [Maintenance Rules](#maintenance-rules)

---

## Decision Log Entries

The Decision Log exists to prevent re-litigating settled decisions and to make the reasoning visible when context needs to be reconstructed later. A decision that was made for good reasons but looks wrong six months later is very different from one that was made badly — the log is how you tell them apart.

### Entry format

```markdown
### [Date] — [Decision title — short, noun phrase]

- **Decision:** [What was actually decided — one sentence, specific enough to be unambiguous]
- **Alternatives considered:** [List of options that were genuinely on the table]
- **Reasoning:** [Why this option over the others — be honest, including "we didn't have enough information to be sure"]
- **Assumptions this rests on:** [What has to remain true for this to still be the right call]
- **Reversibility:** [Easily reversible / reversible with cost / hard to reverse / irreversible]
- **Status:** Active / Superseded by [later decision title and date]
```

### Example entries

```markdown
### 2024-03-15 — Chose Supabase over Firebase for database

- **Decision:** Use Supabase (PostgreSQL) as the primary database, not Firebase Firestore.
- **Alternatives considered:** Firebase Firestore, PlanetScale, Neon, self-hosted Postgres on Railway.
- **Reasoning:** Relational data model fits our use case better than document store. Supabase gives Postgres + auth + storage in one platform. Team already knows SQL. Firebase vendor lock-in risk outweighed the real-time advantage.
- **Assumptions this rests on:** Our data stays relational; we don't need Firebase's offline-first SDK for mobile.
- **Reversibility:** Reversible with significant migration cost (2–4 weeks).
- **Status:** Active

---

### 2024-04-02 — Deferred mobile app to post-launch

- **Decision:** Build web-only for MVP and launch; no mobile app until after first 10 paying customers.
- **Alternatives considered:** React Native alongside web, PWA, web-only.
- **Reasoning:** Mobile doubles the surface area to maintain. Our ICP (operations managers) primarily works on desktop. No customer has asked for mobile yet.
- **Assumptions this rests on:** ICP continues to work desktop-first; no customer discovery finding contradicts this.
- **Reversibility:** Easily reversible — adding mobile later is additive.
- **Status:** Active
```

### When to add an entry

- Any choice that commits time > 1 week or money > $500
- Any architectural or technology choice
- Any significant product direction change (scope cuts, pivot, ICP change)
- Any pricing or business model decision
- Decisions made in meetings that would otherwise be verbal only

### What doesn't need an entry

- Routine execution choices (which library to use for a minor utility, wording on a button)
- Decisions that are immediately reversed before they affect anything
- Pure aesthetic preferences with no downstream consequences

---

## Risk Register Entries

Risks that are tracked are risks that get managed. Risks that live only in someone's head get forgotten until they materialize. The register isn't about pessimism — it's about having a named list of things worth monitoring so none of them surprise you.

### Entry format

```markdown
### [Risk title — short noun phrase describing the risk]

- **Category:** Technical / Business / Market / Team / Legal / Financial
- **Description:** [What specifically could go wrong, and how]
- **Severity:** High / Medium / Low
- **Likelihood:** High / Medium / Low
- **Reasoning:** [Why this severity and likelihood — not just "seems risky"]
- **Leading indicators:** [What you'd observe early if this risk is materializing — before it fully hits]
- **Mitigation:** [What's being done about it, or "none yet" if it's just being monitored]
- **Status:** Open / Mitigated / Resolved — [date and note if not Open]
```

### Severity guidelines

```
High:   Could end the company or force a major pivot if it materializes
Medium: Would cost significant time/money/momentum but is survivable
Low:    Annoying or costly but manageable without fundamental change
```

### Likelihood guidelines

```
High:   More likely than not to materialize in the current runway window
Medium: Possible — wouldn't be surprised if it happened
Low:    Unlikely but worth watching; would be surprised if it materialized
```

### Example entries

```markdown
### Single-channel acquisition dependency

- **Category:** Business
- **Description:** All current user acquisition comes from one Twitter thread. If that channel stops working or the account is banned, growth stops entirely.
- **Severity:** High
- **Likelihood:** Medium
- **Reasoning:** Twitter has become less predictable post-2022 acquisition. Organic reach is declining. No diversification exists yet.
- **Leading indicators:** Week-over-week growth drops below 5%; Twitter engagement rate falls below baseline two weeks in a row.
- **Mitigation:** Exploring SEO content and direct outreach in parallel. No committed effort yet.
- **Status:** Open

---

### Auth library abandoned / critical vulnerability

- **Category:** Technical
- **Description:** We're using a relatively small open-source auth library. If it's abandoned or a critical vulnerability is discovered with no patch, we'd need an emergency migration.
- **Severity:** Medium
- **Likelihood:** Low
- **Reasoning:** Library has active maintainers and 4k GitHub stars. Unlikely in the short term but worth monitoring.
- **Leading indicators:** No commit activity for 60+ days; open critical CVEs with no response from maintainers.
- **Mitigation:** Abstracted behind a repository interface (Branch by Abstraction), so swapping it out is feasible in 1–2 weeks rather than months.
- **Status:** Open
```

### Risk triage: when to escalate

Move a risk from Low/Medium to High, or from Open to urgent, when:
- A leading indicator fires
- External context changes (competitor launches, regulation announced, key person leaves)
- A customer interview surfaces evidence the risk is materializing
- The risk has been Open for 60+ days without any mitigation progress

---

## Contradiction Detector

Run this check at the end of any session that touched planning documents, PRDs, or roadmap scope. Contradictions between documents are normal — they only become problems when they're invisible.

### Common contradiction patterns

**PRD vs. pricing page**
The feature or limit described in the PRD doesn't match what the pricing page promises.
```
PRD says: "Users can invite unlimited team members."
Pricing page says: "Starter plan: up to 3 seats."
→ Flag: which is correct? If unlimited is the intent, update pricing. If 3-seat cap is the intent, update PRD.
```

**Decision Log vs. current plan**
Something in the current plan contradicts a logged decision without a new decision superseding it.
```
Decision Log (2024-03-01): "No mobile app until after first 10 paying customers."
Current roadmap: "Q3: iOS app launch"
→ Flag: either a new decision was made and not logged, or this is an inconsistency.
```

**ICP in Founder Memory vs. ICP in PRD or Customer Discovery**
The ICP description has drifted between documents.
```
Founder Memory: "Target user: independent consultants"
PRD: "Target user: ops managers at 50–200 person companies"
→ Flag: these are different customers. Which is the actual ICP? Align documents.
```

**Roadmap stage vs. planned features**
Features being built don't match the definition of the current stage.
```
Current stage: "MVP — core loop only, no integrations"
In-progress tasks: "Slack integration, Zapier connector"
→ Flag: integrations sound like Beta scope, not MVP. Is the stage definition wrong, or has scope crept?
```

**Stated business model vs. product decisions**
A product decision is inconsistent with how the company makes money.
```
Business model: "Usage-based pricing per API call"
PRD requirement: "Unlimited API calls on all plans"
→ Flag: unlimited calls undercuts usage-based revenue model.
```

### Output format for contradictions

```markdown
## Contradiction Detected

**Type:** [PRD vs. pricing / Decision log vs. roadmap / ICP drift / etc.]

**Document A says:** [quote or summary]
**Document B says:** [quote or summary]

**Why it matters:** [what breaks if this is left unresolved]

**Resolution options:**
1. [Option A — update Document A to match B]
2. [Option B — update Document B to match A]
3. [Option C — make a new decision that supersedes both]

**Recommended:** [option, with brief reasoning]
```

---

## Meeting Processor Output Format

When given a meeting transcript or notes, extract the following and fold into the Founder State Document. Don't leave meeting output as a separate untracked summary — the value is in updating the live document.

### Extraction format

```markdown
## Meeting: [date] — [meeting type / attendees]

### Decisions made
[Each decision → goes into Decision Log with full entry format above]

- **[Decision title]:** [what was decided] [add to Decision Log]

### Action items
| Action | Owner | Due | Added to roadmap? |
|---|---|---|---|
| [Action 1] | [name] | [date] | Yes / No |
| [Action 2] | [name] | [date] | Yes / No |

### New risks surfaced
[Each risk → goes into Risk Register with full entry format above]

- **[Risk name]:** [brief description] — Severity: [H/M/L] [add to Risk Register]

### Open questions (not yet resolved)
- [Question] — owner: [name], target resolution: [date or "TBD"]

### Context / notes worth preserving
[Anything that doesn't fit the above but is worth having in the state document — customer reactions, market signals, team dynamics]
```

### What not to include in the extraction

- Logistical details (who brought coffee, whether the room was booked)
- Statements that were raised but explicitly dismissed and don't represent a decision or risk
- Redundant information already captured accurately in the state document

---

## Maintenance Rules

These rules keep the Decision Log and Risk Register from becoming stale:

**Never delete — mark superseded or resolved.**
A decision that was made and then reversed is still worth knowing about. Deleting it removes the context for why you went back. Mark it "Superseded by [later decision]" and leave the entry.

**Most recent first.**
Both the Decision Log and Risk Register should have the newest entries at the top. This makes it fast to scan for what's current.

**Date everything.**
Every entry gets a date. A risk that's been sitting in the register for 8 months without any mitigation update is a different kind of concern than one added last week. The date makes the staleness visible.

**Update in-session, not just at Phase 8.**
If a decision gets made mid-conversation, note it immediately rather than trying to reconstruct it at the end. By Phase 8, you're outputting the updated document; don't let anything fall through the cracks between the decision moment and the handoff.

**One decision per entry.**
Don't bundle multiple decisions into one log entry. "We decided to use Supabase and to defer mobile and to target SMBs first" is three decisions. Log them separately so each can be superseded or referenced independently.
