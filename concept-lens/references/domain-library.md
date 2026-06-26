# Domain Analogy Library

A library of 20+ domains ConceptLens can map any concept onto. Each domain entry includes: the **core mental model** of that domain (what makes it tick), **vocabulary** to draw on, and 1–2 **worked examples** mapping a common technical/abstract concept into it.

Use this file by:
1. Picking 5–10 domains that are *cognitively varied* (don't pick three sports domains back to back)
2. Following the component-mapping pattern shown in the worked examples — every analogy must map specific parts, not just gesture at a vibe
3. Adapting the worked examples to whatever concept the user actually asked about — these are templates for *how* to build the mapping, not a lookup table of fixed answers

---

## 1. Gaming / Video Games
**Core model:** levels, stats, XP, cooldowns, loot tiers, save states, boss fights, respawns.
**Vocabulary:** grinding, leveling up, buffs/debuffs, aggro, meta, patch notes, NPC, hit points.
**Worked example — Neural network training:**
- Model weights → character stats (strength, agility) that level up
- Training epoch → a grinding session that slowly raises stats
- Loss function → the "damage taken" meter you're trying to minimize
- Overfitting → over-specializing your build for one boss fight, then losing to everything else
- Breaks down at: there's no "save state" in training — you can't always roll back to a better checkpoint without keeping it explicitly (analogous to autosave only at checkpoints).

## 2. Cooking
**Core model:** recipes, ingredients, prep, taste-testing, adjusting seasoning, mise en place, plating.
**Vocabulary:** reduction, mise en place, seasoning to taste, mother sauce, recipe vs. technique.
**Worked example — Gradient descent:**
- Loss value → how far the dish is from "tastes right"
- Gradient → the direction your next pinch of salt should go
- Learning rate → how big a pinch you add each time (too big: you oversalt; too small: bland forever)
- Convergence → the dish finally tasting balanced
- Breaks down at: a chef tastes once and adjusts; gradient descent needs the "taste" (loss) to be differentiable, which not all real cooking is.

## 3. Cricket
**Core model:** innings, overs, strike rotation, fielding placements, run rate, the keeper, DRS reviews.
**Vocabulary:** strike rate, economy, googly, death overs, building an innings.
**Worked example — Reinforcement learning:**
- Policy → a batsman's shot selection strategy
- Reward → runs scored per ball
- Exploration vs exploitation → trying a risky six vs. playing safe singles to build the innings
- Episode → one full innings
- Breaks down at: a batsman gets immediate feedback every ball; RL agents often face delayed/sparse rewards with no per-step "runs."

## 4. Football / Soccer
**Core model:** formations, possession, set pieces, pressing, transitions between attack and defense.
**Vocabulary:** build-up play, pressing trigger, overlap, false nine.
**Worked example — Microservices architecture:**
- Each microservice → a player with one defined role
- API contract → the pre-agreed passing pattern between two players
- Load balancer → the manager rotating who receives the ball based on who's open
- Service failure → a player getting injured mid-match — the team needs cover, not a full restart
- Breaks down at: football players can improvise outside their role; rigid microservices generally can't without redeployment.

## 5. Construction / Engineering (civil)
**Core model:** foundations, load-bearing structure, blueprints, inspections, scaffolding.
**Vocabulary:** load-bearing, foundation, scaffolding, structural integrity, blueprint.
**Worked example — Technical debt:**
- Clean architecture → a building with a proper foundation and inspected wiring
- Technical debt → skipping inspections and using temporary scaffolding as permanent structure
- Refactoring → going back and pouring a real foundation under a building that's already occupied
- Breaks down at: you can't usually "occupy" software while refactoring its foundation the way you can't occupy a building during structural work — software often runs fine on bad foundations far longer than buildings do.

## 6. Medicine
**Core model:** diagnosis, symptoms vs. root cause, treatment plans, dosage, side effects, triage.
**Vocabulary:** symptom, diagnosis, dosage, prognosis, triage.
**Worked example — Debugging:**
- Bug report → a patient's reported symptom
- Stack trace → the lab test results
- Root cause → the actual underlying condition (vs. a symptom you could mask)
- Patch → treatment; regression → relapse
- Breaks down at: doctors often can't run the "test" (lab work) instantly and cheaply the way a debugger can re-run code — and patients aren't deterministic.

## 7. Finance / Investing
**Core model:** risk vs. return, diversification, compounding, liquidity, debt vs. equity.
**Vocabulary:** portfolio, hedge, compounding, liquidity, leverage.
**Worked example — Ensemble learning (e.g., random forests):**
- Individual decision tree → a single stock
- Ensemble → a diversified portfolio
- Reduced variance from averaging → reduced risk from diversification
- Bagging → buying many slightly-correlated-but-different assets instead of one
- Breaks down at: stocks can be correlated by macro shocks in ways that break diversification; trees in a forest are deliberately decorrelated by design (bootstrapping/feature sampling), which has no risk-market equivalent.

## 8. Music / Orchestra
**Core model:** sections, tempo, conductor, rehearsal, harmony vs. dissonance, sheet music as a shared contract.
**Vocabulary:** tempo, harmony, conductor, score, ensemble.
**Worked example — Distributed systems consensus:**
- Conductor → a coordinator node setting tempo (clock/ordering)
- Sheet music → the agreed protocol every node follows
- A section coming in early → a node acting on stale data
- Rehearsal → testing/staging environment
- Breaks down at: an orchestra has one conductor everyone can see; many distributed systems are explicitly leaderless, which has no clean orchestra equivalent.

## 9. Military / Army
**Core model:** chain of command, recon, logistics/supply lines, fog of war, rules of engagement.
**Vocabulary:** chain of command, recon, fog of war, logistics, rules of engagement.
**Worked example — Caching layers:**
- Database → the main supply depot, far from the front
- Cache → a forward supply cache closer to the front lines (faster access)
- Cache invalidation → the problem of the forward cache holding outdated supplies after HQ updates inventory
- Cache miss → a soldier reaching the forward cache and finding it empty, having to go all the way back to the depot
- Breaks down at: military logistics has humans who notice when supplies are stale; cache invalidation is the hard unsolved problem precisely because there's no automatic "notice."

## 10. Space / Astronomy
**Core model:** orbits, gravity wells, launch windows, mission control, points of no return.
**Vocabulary:** orbit, escape velocity, launch window, trajectory correction.
**Worked example — Local minima in optimization:**
- Loss landscape → a gravitational landscape with hills and valleys
- Local minimum → a small crater the spacecraft has settled into, mistaking it for the destination
- Momentum (in optimizers) → extra thrust that lets the spacecraft coast through small craters instead of stopping in them
- Global minimum → the actual deepest valley (the true destination)
- Breaks down at: space terrain is static; loss landscapes can shift as parameters elsewhere update, which has no clean orbital analogy.

## 11. Gardening / Farming
**Core model:** seasons, pruning, soil health, cross-pollination, harvest cycles.
**Vocabulary:** pruning, fertile soil, dormancy, grafting.
**Worked example — Regularization in ML:**
- Unregularized model → an overgrown, untrimmed hedge that grows wild branches to fit every gust of wind
- L1/L2 regularization → pruning, keeping the hedge in a clean, generalizable shape
- Dropout → randomly covering parts of the hedge each day so no single branch becomes load-bearing
- Breaks down at: a real gardener prunes based on judgment about the plant's health; regularization strength is a blunt numeric knob with no equivalent judgment.

## 12. Theater / Film Production
**Core model:** rehearsal, blocking, director's vision, scenes, cuts, continuity.
**Vocabulary:** blocking, take, continuity, director's cut.
**Worked example — Version control (git):**
- A take → a commit
- Editing room / final cut → merging branches into main
- Director's notes between takes → code review comments
- Continuity error → a merge conflict
- Breaks down at: film takes are sequential and discarded; git branches can be developed genuinely in parallel and combined, which film editing doesn't really do.

## 13. Law / Courtroom
**Core model:** evidence, burden of proof, precedent, cross-examination, verdicts.
**Vocabulary:** precedent, burden of proof, cross-examine, ruling.
**Worked example — Statistical hypothesis testing:**
- Null hypothesis → presumption of innocence
- p-value → strength of evidence against the defendant
- Significance threshold (α) → the standard of "beyond reasonable doubt"
- Type I error (false positive) → wrongly convicting an innocent person
- Breaks down at: courts make one binary, final decision; statistical tests are explicitly probabilistic and revisited as more data (evidence) arrives — there's no real courtroom equivalent of "running the trial again with more jurors to lower your error rate."

## 14. Restaurant / Kitchen Operations
**Core model:** front of house vs. back of house, tickets, expediting, mise en place, service rush.
**Vocabulary:** ticket, expo, rush, the pass.
**Worked example — Queueing systems / load balancing:**
- Order tickets → incoming requests
- The expo (expediter) → the load balancer routing tickets to the right station
- Each cooking station → a server handling a request type
- Rush hour ticket pile-up → request queue backlog
- Breaks down at: a kitchen can verbally communicate priority and improvise station coverage; most load balancers route by fixed rules without that judgment.

## 15. Sports — Basketball
**Core model:** possessions, shot clock, pick and roll, fast breaks, defensive rotations.
**Vocabulary:** possession, shot clock, rotation, fast break.
**Worked example — Latency vs. throughput tradeoffs:**
- Shot clock → a hard latency deadline per request
- Fast break → low-latency path with minimal processing
- Methodical half-court offense → high-throughput batch processing that takes longer per "possession" but is more reliable
- Breaks down at: basketball strategy choice is made by humans reading the game; systems usually commit to one strategy (fast-path vs batch) by design, not dynamically per request, unless specifically built to.

## 16. Biology / The Body
**Core model:** homeostasis, immune response, evolution by selection, organs as specialized subsystems.
**Vocabulary:** homeostasis, immune response, mutation, natural selection.
**Worked example — A/B testing & genetic algorithms:**
- Population of variants → a gene pool
- Fitness function (conversion rate, etc.) → survival advantage
- Selecting winning variants → natural selection
- Mutation/crossover → trying small random tweaks each generation
- Breaks down at: biological evolution has no designer choosing the fitness function; A/B tests are explicitly designed and chosen by a human, which changes what "fitness" even means.

## 17. Travel / Road Trips
**Core model:** routes, detours, traffic, rest stops, fuel/range anxiety, GPS recalculating.
**Vocabulary:** detour, route, recalculating, pit stop.
**Worked example — Backpropagation:**
- Forward pass → driving the planned route and seeing where you end up vs. the destination
- Error at the destination → how far off your final position is from where you meant to go
- Backprop → GPS recalculating turn-by-turn, working backward from the wrong destination to find which turns to adjust
- Breaks down at: GPS recalculates the whole route at once; backprop assigns blame turn-by-turn proportionally via the chain rule, which is more surgical than "just reroute."

## 18. Retail / E-commerce
**Core model:** inventory, checkout funnel, supply chain, returns, customer segments.
**Vocabulary:** funnel, inventory turnover, SKU, cart abandonment.
**Worked example — Database indexing:**
- Unindexed table → a warehouse with items dumped in random piles, no shelf labels
- Index → an aisle/shelf numbering system so staff find an item without checking every pile
- Query using the index → walking straight to aisle 12, shelf 4
- Breaks down at: warehouse reorganization (re-indexing) costs labor-hours but doesn't usually have the write-penalty tradeoff that database indexes have (every insert must also update the index).

## 19. Card Games / Poker
**Core model:** hidden information, betting rounds, pot odds, bluffing, reading opponents.
**Vocabulary:** pot odds, bluff, tell, fold equity.
**Worked example — Game-theoretic strategy in multi-agent systems:**
- Hidden hand → hidden state/information one agent has and others don't
- Betting → signaling/costly actions that reveal partial information
- Bluffing → an agent taking an action that misrepresents its true state to influence others
- Nash equilibrium strategy → a strategy no player can unilaterally improve on
- Breaks down at: poker has a fixed, known payout structure (the pot); many multi-agent systems have payoffs that aren't common knowledge, making true equilibrium computation much harder.

## 20. Theme Parks / Roller Coasters
**Core model:** queues, ride capacity, ride safety checks, FastPass-style priority lanes.
**Vocabulary:** queue, capacity, throughput, priority lane.
**Worked example — Rate limiting & API throttling:**
- Ride queue → request queue
- Ride capacity per cycle → requests processed per time window
- FastPass lane → a priority/paid API tier
- Ride breaking down and needing inspection → a server health check failing and being taken out of rotation
- Breaks down at: theme park queues are visible to the people waiting; API rate limits are usually invisible until a request is rejected, with no equivalent of "seeing the line."

## 21. Orchestra Conducting *(see Music above for overlap — use sparingly together)*

## 22. Aviation / Piloting
**Core model:** pre-flight checklists, autopilot, instruments vs. visual flying, air traffic control, black boxes.
**Vocabulary:** checklist, autopilot, instrument flying, black box, mayday.
**Worked example — Automated monitoring/alerting systems:**
- Instrument panel → a monitoring dashboard
- Autopilot → automated remediation/self-healing systems
- Air traffic control → an orchestration layer coordinating multiple independent systems
- Black box recorder → application logs used for post-incident review
- Breaks down at: pilots can always take manual control instantly; many automated systems have a real handoff delay or require a human to even notice something's wrong first.

---

## Tips for using this library well

- **Vary cognitive style, not just subject matter.** Gaming, cricket, and football are all "competitive timed contests" — pairing them together in a tournament is weak variety. Pair one competitive domain with one craft domain (cooking), one systems domain (construction, aviation), and one narrative domain (theater) for real range.
- **Always state where the analogy breaks.** Every worked example above includes a "breaks down at" line — carry this discipline into live answers. It's often the most useful sentence in the whole analogy because it teaches the boundary of the user's new mental model.
- **Pull from what the user already revealed.** If they used cricket terms unprompted, lead with cricket — don't bury it as option #6.
