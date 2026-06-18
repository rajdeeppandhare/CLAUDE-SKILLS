# Founder State Document Schema

This is the persistence mechanism for Founder OS. Read `SKILL.md`'s "memory reality" section first if you haven't — this document exists because the skill itself can't remember anything on its own. The schema below is deliberately plain markdown, not JSON or a database export, because the user needs to be able to open it and actually read it without tooling.

## Full schema

```markdown
# Founder State: [Company/Project Name]

## Founder Memory
- Vision: [one line]
- Mission: [one or two sentences, optional]
- Target users / ICP: [as specific as possible]
- Core pain being solved: [one line]
- Business model: [how it makes money, or "not yet decided"]
- Current stage: [idea / MVP / beta / pilot / launch / scale]
- Last updated: [date]

## Decision Log
[Most recent first. Never delete entries — superseded decisions stay visible with a note.]

### [Date] — [Decision title]
- Decision: [what was decided]
- Alternatives considered: [list]
- Reasoning: [why this one]
- Status: [active / superseded by <later decision>]

## Roadmap
- Current stage: [matches Founder Memory]
- Stage definition of done: [concrete criteria]
- Stages: MVP → Beta → Pilot → Launch → Scale (adjust if a different shape fits)

### Tasks
- Done: [list]
- In progress: [list]
- Blocked: [list, with what's blocking]

## Risk Register
[Severity: high / med / low. Mark resolved rather than deleting.]

### Technical risks
- [Risk] — [severity] — [reasoning] — [status: open/resolved]

### Business risks
- [Risk] — [severity] — [reasoning] — [status: open/resolved]

### Market risks
- [Risk] — [severity] — [reasoning] — [status: open/resolved]

## Market & Competitive Notes
### Competitors
- [Name]: advantage = [...], weakness = [...], pricing = [...], last checked = [date]

### Market synthesis
- Consensus: [...]
- Disagreements: [...]
- Opportunities: [...]
- Threats: [...]

## Customer Discovery
- ICP: [specific description]
- Key open questions: [list]
- Interview findings: [list, dated]

## Financials (only if the user has supplied real numbers)
- Cash on hand: [amount, date]
- Monthly burn: [amount]
- Runway: [computed]
- Revenue: [amount, or 0]
```

## How to use this in a session

**At the start**: if the user pastes or uploads a state document, read the whole thing before responding to their actual request — especially the Decision Log and Risk Register, since giving advice that contradicts a settled decision or a known risk without acknowledging it undermines the entire point of this skill.

**During the session**: update sections as things happen, not just at the end. If a decision gets made mid-conversation, add it to the Decision Log right then conceptually, even if you're not outputting the full document again until Phase 8.

**At the end (Phase 8)**: output the full updated document, not a diff. The user is going to save this and bring it back; a diff against a version they don't have open is useless to them. Bump "Last updated."

## What this is not

This is not a knowledge graph in the formal sense, and it doesn't need to be. A bullet list of competitors with their relationships to the company is just as queryable by a human (or by Claude reading it next session) as a graph structure would be, without needing graph infrastructure that doesn't exist here. Resist the urge to over-engineer this into something that needs code to read — the entire value of this format is that it's just a markdown file.

## If the user has no state document yet

Don't make them fill out the whole schema before you'll help them. Start with whatever they've told you (even just "Founder Memory" partially filled in), do the actual work they came for, and build the document up incrementally. A sparse but real state document beats a complete one they never finished setting up.
