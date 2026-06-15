# Ticket Templates Reference

Ready-to-use ticket templates for JIRA, Linear, and GitHub Issues. Copy, fill in the bracketed fields, and paste into your project tracker.

---

## Table of Contents

1. [How to Use These Templates](#how-to-use)
2. [Epic Templates](#epic-templates)
3. [Phase Ticket Templates](#phase-tickets)
4. [Task-Level Ticket Templates](#task-tickets)
5. [Bug / Regression Ticket Templates](#bug-tickets)
6. [GitHub Issues Format](#github-issues)
7. [Linear Format](#linear-format)
8. [Sprint Planning Breakdown](#sprint-planning)

---

## How to Use These Templates

After the Evolution Engine produces a phased blueprint, generate tickets by:

1. Create one **Epic** per migration phase
2. Create one **Phase Ticket** per phase (the "driver" ticket that owns the phase)
3. Create **Task Tickets** for each discrete unit of work within the phase
4. Link all tickets to their parent epic
5. Set dependencies between phases (Phase N blocks Phase N+1)

Effort estimates in these templates use T-shirt sizing mapped to hours:
```
XS = 1–2h    S = 2–4h    M = 4–8h    L = 8–16h    XL = 16–32h
```

---

## Epic Templates

### Migration Epic

```
Title:
  [MIGRATION] {source_stack} → {target_stack} — Phase {N}: {Phase Name}

Description:
  ## Context
  Part of the {repo} stack evolution from {source_stack} to {target_stack}.
  Full blueprint: [link to Evolution Report]

  ## Scope
  This epic covers Phase {N} of {total_phases}: {phase_name}.

  What changes:
  - {change_1}
  - {change_2}
  - {change_3}

  Files affected: {file_count} files across {module_list}

  ## Definition of Done
  - [ ] All task tickets in this epic are closed
  - [ ] Validation checklist passes (see Phase Ticket)
  - [ ] No regression in error rate (baseline: {baseline_error_rate}%)
  - [ ] No regression in p95 latency (baseline: {baseline_p95}ms)
  - [ ] PR reviewed and merged to main
  - [ ] Staging deployment verified

  ## Rollback
  If this phase fails: {rollback_procedure}
  Rollback trigger: error rate > {threshold}% OR p95 latency > {threshold}ms

Labels: migration, phase-{N}, {source_stack}, {target_stack}
Story Points / Estimate: {total_estimate}
Priority: {High / Medium}
```

---

## Phase Ticket Templates

### Standard Phase Driver Ticket

```
Title:
  [Phase {N}] {Phase Name} — {one-line summary}

Type: Task / Story

Description:
  ## What & Why
  {2–3 sentences describing what this phase does and why it's sequenced here}

  ## Files to Change
  {list of files or modules, or "see sub-tasks"}

  ## Implementation Steps
  1. {step_1}
  2. {step_2}
  3. {step_3}

  ## Validation Checklist
  - [ ] {validation_check_1}
  - [ ] {validation_check_2}
  - [ ] {validation_check_3}
  - [ ] All existing tests pass
  - [ ] No new TypeScript / lint errors
  - [ ] Deployed to staging and smoke-tested

  ## Breaking Risk
  {low / medium / high} — {reason}

  ## Rollback
  {specific rollback procedure for this phase}

  ## Dependencies
  Blocked by: {Phase N-1 ticket ID}
  Blocks: {Phase N+1 ticket ID}

Estimate: {size}
Labels: migration, phase-{N}, breaking-risk-{level}
```

---

### Infrastructure Setup Phase

```
Title:
  [Phase 1] Infrastructure — Set up {target_stack} project scaffold

Description:
  ## What & Why
  Establish the new stack's project structure, tooling, and CI/CD pipeline
  before any code migration begins. This phase has zero production impact.

  ## Tasks
  - [ ] Initialise {target_framework} project in /new or separate repo
  - [ ] Configure {build_tool} (tsconfig / vite / go.mod / Cargo.toml)
  - [ ] Set up linter and formatter ({eslint+prettier / golangci-lint / rustfmt})
  - [ ] Write Dockerfile for new stack
  - [ ] Add docker-compose.yml for local development
  - [ ] Set up CI pipeline (GitHub Actions / CircleCI) for new stack
  - [ ] Configure environment variables (.env.example)
  - [ ] Verify: new project builds and runs locally

  ## Validation
  - [ ] `docker build` succeeds
  - [ ] CI pipeline passes on empty project
  - [ ] Local dev server starts without errors
  - [ ] Environment variables load correctly

  ## Breaking Risk
  None — this phase is additive only.

  ## Rollback
  Delete the new project scaffold. No production changes made.

Estimate: S–M (4–8h)
Labels: migration, phase-1, infrastructure, no-risk
```

---

### Database Migration Phase

```
Title:
  [Phase {N}] Database — Migrate {source_db} → {target_db}

Description:
  ## What & Why
  Migrate the data layer from {source_db} to {target_db}. This is the
  highest-risk phase. Dual-write strategy is used to ensure zero data loss.

  ## Pre-Conditions
  - [ ] Full production database backup taken and verified
  - [ ] Down migration tested on a copy of production data
  - [ ] ETL dry run completed — row counts verified

  ## Tasks
  - [ ] Write schema for {target_db} (mirroring {source_db} structure)
  - [ ] Write ETL migration script
  - [ ] Run ETL on staging copy — verify row counts and spot-check data
  - [ ] Enable dual-write: write to {source_db} (primary) + {target_db} (secondary)
  - [ ] Run parallel read comparison for {N} days
  - [ ] Flip reads to {target_db} (keep {source_db} as write primary)
  - [ ] Flip writes to {target_db} ({source_db} becomes replica)
  - [ ] Cutover: retire {source_db}

  ## Validation
  - [ ] Row counts match across source and target
  - [ ] Spot-check 100 random records — data integrity confirmed
  - [ ] All application queries return correct results against new DB
  - [ ] Discrepancy rate < 0.01% in parallel run

  ## Breaking Risk
  High — follow dual-write protocol strictly.

  ## Rollback
  - Before dual-write: drop target schema, no impact
  - During dual-write: disable writes to target, source remains primary
  - After cutover: promote source DB from replica back to primary, redirect app

Estimate: XL (20–40h including ETL, dry runs, parallel run period)
Labels: migration, phase-{N}, database, high-risk
```

---

### Auth Migration Phase

```
Title:
  [Phase {N}] Auth — Migrate {source_auth} → {target_auth}

Description:
  ## What & Why
  Migrate the authentication system from {source_auth} to {target_auth}.
  Auth is intentionally last because it touches every layer. A kill switch
  flag controls the cutover.

  ## Pre-Conditions
  - [ ] All other phases complete and stable
  - [ ] Feature flag system in place (for kill switch)
  - [ ] Session invalidation plan communicated to users if needed

  ## Tasks
  - [ ] Implement {target_auth} alongside {source_auth} (flag-gated)
  - [ ] Test all auth flows: login, logout, refresh, password reset, OAuth
  - [ ] Test protected route access with new auth
  - [ ] Test token expiry and refresh behaviour
  - [ ] Enable flag for internal team
  - [ ] Enable flag for 10% of users — monitor for 48h
  - [ ] Enable flag for 100% of users
  - [ ] Remove old auth system and feature flag

  ## Validation
  - [ ] All auth flows work end-to-end
  - [ ] No user sessions broken during rollout
  - [ ] JWT / session tokens validated correctly
  - [ ] Auth errors surface correctly (401 / 403)

  ## Breaking Risk
  High — session format changes log users out. Coordinate rollout timing.

  ## Rollback
  Flip kill switch flag to revert to {source_auth} within 60 seconds.
  No deployment required.

Estimate: L–XL (12–24h)
Labels: migration, phase-{N}, auth, high-risk, kill-switch
```

---

## Task-Level Ticket Templates

### File Migration Task

```
Title:
  Migrate {filename} from {source_tech} to {target_tech}

Description:
  ## File
  `{path/to/file}`

  ## What to Change
  {description of what needs to change in this specific file}

  ## Source Pattern
  ```{source_lang}
  {source_code_example}
  ```

  ## Target Pattern
  ```{target_lang}
  {target_code_example}
  ```

  ## Gotchas
  - {known_issue_1}
  - {known_issue_2}

  ## Done When
  - [ ] File compiles / passes type check
  - [ ] Existing tests pass
  - [ ] Manual smoke test passes

Estimate: {XS / S / M}
Labels: migration, task, {module_name}
```

---

### Dependency Swap Task

```
Title:
  Replace {old_package} with {new_package}

Description:
  ## Why
  {old_package} is being replaced as part of the {phase_name} phase.
  {new_package} is the target stack equivalent.

  ## Steps
  1. `{uninstall_command}`
  2. `{install_command}`
  3. Find all usages: `grep -r "{old_import}" src/`
  4. Replace imports: `{old_import}` → `{new_import}`
  5. Update usage patterns (see below)

  ## Usage Pattern Changes
  | Old | New |
  |---|---|
  | `{old_usage_1}` | `{new_usage_1}` |
  | `{old_usage_2}` | `{new_usage_2}` |

  ## Done When
  - [ ] Old package removed from package.json / go.mod / requirements.txt
  - [ ] No remaining imports of old package
  - [ ] All tests pass

Estimate: {S / M}
Labels: migration, task, dependencies
```

---

### Test Migration Task

```
Title:
  Migrate test suite from {source_test_framework} to {target_test_framework}

Description:
  ## Scope
  {N} test files across {modules}

  ## Steps
  1. Install {target_test_framework}: `{install_command}`
  2. Update test scripts in package.json / Makefile
  3. Migrate test files — see pattern changes below
  4. Verify all tests pass: `{test_command}`

  ## Pattern Changes
  | {source_framework} | {target_framework} |
  |---|---|
  | `describe / it` | `describe / it` (same) |
  | `jest.fn()` | `vi.fn()` |
  | `jest.mock()` | `vi.mock()` |
  | `jest.spyOn()` | `vi.spyOn()` |
  | `beforeEach / afterEach` | same |
  | `expect(x).toBe(y)` | same |

  ## Done When
  - [ ] All {N} test files migrated
  - [ ] Test suite runs with `{test_command}`
  - [ ] No skipped or failing tests (or all skips documented)
  - [ ] Coverage report generated and reviewed

Estimate: {M / L}
Labels: migration, task, testing
```

---

## Bug / Regression Ticket Templates

### Migration Regression

```
Title:
  [REGRESSION] {feature} broken after {phase_name} migration

Priority: P0 / P1

Description:
  ## Summary
  {1–2 sentences describing the regression}

  ## Phase That Introduced It
  Phase {N}: {phase_name}

  ## Steps to Reproduce
  1. {step_1}
  2. {step_2}
  3. {step_3}

  Expected: {expected_behaviour}
  Actual: {actual_behaviour}

  ## Impact
  - Affected users: {scope}
  - Error rate delta: {before}% → {after}%
  - First seen: {timestamp}

  ## Rollback Decision
  - [ ] Rollback this phase (trigger: {rollback_trigger})
  - [ ] Fix forward (safe if impact < {threshold})

  ## Investigation Notes
  {any findings so far}

Labels: migration, regression, phase-{N}, P{priority}
```

---

## GitHub Issues Format

For teams using GitHub Issues instead of JIRA / Linear:

```markdown
## Context
Part of #{epic_issue_number} — {source_stack} → {target_stack} migration.

## What
{description of this task}

## Steps
- [ ] {step_1}
- [ ] {step_2}
- [ ] {step_3}

## Done When
- [ ] {acceptance_criterion_1}
- [ ] {acceptance_criterion_2}
- [ ] Tests pass (`{test_command}`)

## Rollback
{rollback_procedure}

**Estimate:** {size}
**Risk:** {low / medium / high}
```

---

## Linear Format

Linear tickets use a more compact format. Acceptance criteria go in the description; checklists go in comments during execution.

```
Title: [{Phase N}] {one-line summary}

Status: Backlog → In Progress → In Review → Done

Description:
  {context paragraph}

  **Scope:** {files or modules}
  **Pattern:** {migration pattern used}
  **Risk:** {low / medium / high}
  **Rollback:** {procedure}

  Acceptance criteria:
  1. {criterion_1}
  2. {criterion_2}
  3. {criterion_3}

Priority: Urgent / High / Medium / Low
Estimate: {points or hours}
Labels: migration, phase-{N}
Parent: {Epic ID}
```

---

## Sprint Planning Breakdown

Use this to estimate sprint capacity for the migration:

```yaml
Sprint Planning — {source_stack} → {target_stack} Migration

Total Estimated Effort: {total_hours}h
Team Size:              {N} engineers
Available Capacity:     {hours_per_engineer * N}h per sprint (assuming 70% on migration)

Sprint Allocation:
  Sprint 1 ({dates}):
    Phase 1 — Infrastructure:    {N}h  [assigned: {engineer}]
    Phase 2 — Database (start):  {N}h  [assigned: {engineer}]
    Total:                       {N}h

  Sprint 2 ({dates}):
    Phase 2 — Database (finish): {N}h  [assigned: {engineer}]
    Phase 3 — Data Access:       {N}h  [assigned: {engineer}]
    Total:                       {N}h

  Sprint 3 ({dates}):
    Phase 4 — Business Logic:    {N}h  [assigned: {engineer}]
    Phase 5 — API Layer:         {N}h  [assigned: {engineer}]
    Total:                       {N}h

  Sprint 4 ({dates}):
    Phase 6 — Auth:              {N}h  [assigned: {engineer}]
    Phase 7 — UI / Frontend:     {N}h  [assigned: {engineer}]
    Total:                       {N}h

  Sprint 5 ({dates}):
    Phase 8 — Cleanup:           {N}h  [assigned: {engineer}]
    Buffer / Regression fixes:   {N}h
    Total:                       {N}h

Risk Buffer: 20% added to all estimates above
Schedule Risk: Phase 2 (Database) and Phase 6 (Auth) have the highest variance
```
