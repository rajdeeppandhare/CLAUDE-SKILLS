# Output Schema

The structured artifact produced at the end of every simulation. Designed to be saved as-is, and to map directly onto Decision Audit's logging schema as the "predicted chain" baseline — so the same decision can later be graded for quality against what was actually foreseeable at decision time.

---

## Why This Schema Exists

Butterfly Effect Simulator runs **before** a decision is made. Decision Audit runs **before and after**. The handoff point is this: whatever Butterfly Effect predicts becomes the `key_assumption` and confidence baseline that Decision Audit's `confidence` and `key_assumption` fields are scored against later. If the schemas don't line up, that handoff requires manual translation — so this schema is built to drop straight into a Decision Audit log entry with minimal editing.

**Mapping to Decision Audit fields:**
| Butterfly Effect output | Decision Audit field |
|---|---|
| `decision` | `decision` |
| Root-level confidence average or single highest-priority node's confidence | `confidence` |
| The single most load-bearing assumption identified across the tree (usually the first behavioral-type node) | `key_assumption` |
| `decision_type` derived from reversibility tags in the tree | `decision_type` |
| `domain` from Phase 1 intake | `domain` |

---

## Full Output Structure

```
═══════════════════════════════════════════════════
BUTTERFLY EFFECT SIMULATION
═══════════════════════════════════════════════════

DECISION: [stated decision, one sentence]
DOMAIN: [technical / financial / hiring / product / strategic / personal]
HORIZON: [days-weeks / months / years]
DATE SIMULATED: [date]

───────────────────────────────────────
REAL DECISION TREE
───────────────────────────────────────

[Full branching tree, each node tagged]

Node: [causal claim]
  Type: [mechanical/behavioral/speculative] | Confidence: [X]%
  Reversibility: [easy/costly/irreversible] | Blast radius: [contained/team/company/external]
  [⚠️ PRIORITY FLAG if irreversible + company/external]

  ├─→ Node: [child causal claim]
  │     Type: [...] | Confidence: [X]%
  │     Reversibility: [...] | Blast radius: [...]
  │
  └─→ Node: [child causal claim]
        Type: [...] | Confidence: [X]%
        Reversibility: [...] | Blast radius: [...]

[continue for full tree depth]

───────────────────────────────────────
COUNTERFACTUAL TREE (inaction / alternative path)
───────────────────────────────────────

ALTERNATIVE MODELED: [what "not doing this" actually means here]

[Same branching structure and tagging as above]

───────────────────────────────────────
FEEDBACK LOOPS
───────────────────────────────────────

[One entry per loop found, using the format from feedback-loop-patterns.md]

🔄 [Reinforcing / Self-Correcting]: [Node A] → [Node B] → [Node C] → back to [Node A]
   Closure depth: [N] hops
   Why it matters: [one sentence]

[If none found: "No significant feedback loops detected."]

───────────────────────────────────────
PRIORITY FLAGS
───────────────────────────────────────

[List every node tagged irreversible + company/external blast radius,
regardless of depth or confidence]

⚠️ [Node, depth N, confidence X%]: [one sentence on why this is the kind of
   thing worth protecting against even at low probability]

[If none: "No irreversible/high-blast-radius nodes identified."]

───────────────────────────────────────
CHEAPEST INTERVENTION POINT
───────────────────────────────────────

EDGE: [Specific node → node link in the tree]
ACTION: [Specific, concrete safeguard or change — not vague advice]
WHY HERE: [One sentence on why this is the highest-leverage point, e.g.
   it's upstream of multiple dangerous downstream branches, or it's cheap
   to act on now vs. expensive to fix later]

───────────────────────────────────────
SYNTHESIS
───────────────────────────────────────

[2-3 sentences: what's the actual tradeoff between the real decision and
the counterfactual? What's the headline risk, and what's the headline
upside?]

───────────────────────────────────────
DECISION AUDIT HANDOFF SCHEMA
───────────────────────────────────────

decision: [same as above]
domain: [same as above]
confidence: [single number, 1-10 scale — convert from the root/highest-priority
  node's percentage, e.g. 85% → 8 or 9]
key_assumption: [the single most load-bearing assumption in the tree — usually
  the first behavioral-type node, stated as a belief: "Assumes [X] will hold"]
decision_type: [reversible / irreversible — derived from whether ANY priority-
  flagged node exists; if yes, treat the overall decision as carrying
  irreversible risk even if the immediate action is technically reversible]
time_pressure: [carry over from intake if known, otherwise leave for the
  user to fill in when logging]
alternatives_considered: [the counterfactual path, summarized as the
  alternative that was weighed]

═══════════════════════════════════════════════════
```

---

## JSON Variant (for programmatic use)

When the user wants machine-readable output (e.g. for a script, spreadsheet, or to paste into a tool), use this structure instead of the text format above:

```json
{
  "decision": "string",
  "domain": "string",
  "horizon": "days-weeks | months | years",
  "date_simulated": "ISO date",
  "real_tree": {
    "nodes": [
      {
        "id": "string",
        "claim": "string",
        "parent_id": "string | null",
        "depth": "integer",
        "type": "mechanical | behavioral | speculative",
        "confidence": "integer 1-100",
        "reversibility": "easy | costly | irreversible",
        "blast_radius": "contained | team | company | external",
        "priority_flag": "boolean"
      }
    ]
  },
  "counterfactual_tree": {
    "alternative_modeled": "string",
    "nodes": [ "... same structure as real_tree.nodes ..." ]
  },
  "feedback_loops": [
    {
      "type": "reinforcing | self_correcting",
      "path": ["node_id", "node_id", "node_id"],
      "closure_depth": "integer",
      "explanation": "string"
    }
  ],
  "priority_flags": ["node_id", "node_id"],
  "intervention_point": {
    "edge": ["parent_node_id", "child_node_id"],
    "action": "string",
    "rationale": "string"
  },
  "decision_audit_handoff": {
    "decision": "string",
    "domain": "string",
    "confidence": "integer 1-10",
    "key_assumption": "string",
    "decision_type": "reversible | irreversible",
    "alternatives_considered": ["string"]
  }
}
```

---

## Formatting Rules

- **Every node in the tree, not just leaves, gets all four tags.** A mid-tree node with no tags breaks the priority-flag scan.
- **The Decision Audit handoff block is mandatory output**, even if the user didn't ask for it explicitly — it's what makes this skill compose with the rest of the portfolio rather than being a standalone diagram generator.
- **Confidence in the handoff schema is on a 1–10 scale** (matching Decision Audit's schema) even though the tree itself uses percentages — convert at the handoff step, don't make the user do the math.
- **If no priority flags or loops exist, say so explicitly** in those sections rather than omitting them — an empty section with "none found" is informative; a missing section looks like an oversight.
