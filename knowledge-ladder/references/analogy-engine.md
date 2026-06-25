# Analogy Engine — Domain Libraries

Use these when the user's background suggests a particular world, or when they ask for an analogy in a specific domain. Pick the analogy domain that best matches cues in the conversation. If unsure, ask: "Would you like me to explain this using a [sports / cooking / gaming / finance] analogy?"

---

## How to pick an analogy domain

| User cue | Analogy domain to try |
|----------|-----------------------|
| Mentions a sport, team, player | Sports (specific to their sport) |
| Mentions food, cooking, kitchen | Cooking / recipes |
| Mentions a game, console, genre | Gaming |
| Mentions investing, markets, money | Finance |
| Mentions driving, cars, engines | Automotive |
| Mentions building, construction | Architecture / construction |
| Mentions music, instruments | Music |
| No clear cue | Default: cooking or sports — both are universally understood |

---

## Gaming

Great for: algorithms, optimization, AI, state machines, loops, recursion, databases

| Concept | Gaming analogy |
|---------|----------------|
| Neural network training | A game character grinding XP to level up — each mistake updates their stats |
| Gradient descent | Adjusting your aim slightly after every shot until you're hitting the target |
| Overfitting | Memorizing enemy spawn patterns in one level — then failing on a new map |
| Database index | The minimap — instead of exploring the whole world, you jump straight to the marker |
| Recursion | A quest that requires completing a smaller version of itself first |
| API | The game controller — you press buttons, the console handles the rest |
| Cache | Keeping your most-used items in the quick-slot bar |
| Backpropagation | Rewinding a failed run and adjusting your strategy from where you went wrong |
| Transformer attention | A strategy game where you decide which units on the map deserve your focus this turn |

---

## Cooking / Recipes

Great for: processes, pipelines, ML training, data transformation, dependencies

| Concept | Cooking analogy |
|---------|----------------|
| Neural network | A chef who tastes a dish, adjusts seasoning, tastes again — repeating until it's right |
| Data pipeline | A recipe: raw ingredients → prep → cook → plate |
| Hyperparameters | The oven temperature and cooking time — you set them before cooking starts |
| Overfitting | A chef who's perfected one dish but burns everything else |
| Transfer learning | A trained chef moving to a new cuisine — they don't relearn knife skills |
| Regularization | Forcing a chef not to go overboard on any single spice |
| Feature engineering | Choosing which ingredients to use and how to prep them |
| Batch processing | Cooking 100 meals at once rather than one at a time |

---

## Sports

Great for: optimization, training loops, performance, competition, strategy

| Concept | Sports analogy |
|---------|----------------|
| Neural network (cricket) | A batsman learning from every delivery — each mistake updates his footwork |
| Training loop | Pre-season training: you practice, review film, adjust, repeat |
| Gradient descent | A golfer making small adjustments to their swing after each shot |
| Overfitting | A footballer who's only practiced one type of free kick and can't adapt in a match |
| Ensemble methods | A team sport — no single player wins it; the combination does |
| Hyperparameter tuning | Choosing formation and tactics before the match — you can't change them mid-play |
| Regularization | A team coach limiting how much any one player dominates — forces balanced play |
| Validation set | A friendly match before the league — real conditions, no consequences |

---

## Finance / Investing

Great for: probability, risk, optimization, expected value, tradeoffs

| Concept | Finance analogy |
|---------|----------------|
| Neural network | A trader who reviews thousands of past trades to learn which signals predict moves |
| Overfitting | A strategy that backtests perfectly but loses money live — it memorized noise |
| Regularization | Diversification — don't let any single bet dominate the portfolio |
| Gradient descent | Rebalancing a portfolio slightly after each period to move toward target allocation |
| Expected value | The weighted average of all possible outcomes — what a rational investor thinks about |
| Bias-variance tradeoff | Risk vs. return — lower risk (bias) gives predictable mediocre returns; higher variance means bigger swings |
| Transfer learning | A stock analyst who switches sector — their valuation skills transfer, sector knowledge doesn't |
| Ensemble | A fund-of-funds: combining multiple strategies to reduce single-manager risk |

---

## Automotive / Cars

Great for: systems, components, optimization, feedback loops

| Concept | Car analogy |
|---------|-------------|
| Neural network | The car learning your braking habits and adjusting ABS sensitivity over time |
| Gradient descent | Cruise control making small throttle adjustments to maintain speed on a hill |
| Overfitting | A racing setup tuned perfectly for one track — undriveable on any other |
| Pipeline | Assembly line: chassis → engine → electrics → paint → test |
| Cache | The glove box — you keep the most-needed things within reach, not in the boot |
| Hyperparameters | Tyre pressure and gear ratios — set before the race, affect everything |
| Regularization | A governor that caps the engine — prevents it from going too fast and breaking |

---

## Music

Great for: waves, signals, Fourier transforms, pattern recognition, composition

| Concept | Music analogy |
|---------|---------------|
| Fourier transform | Breaking a chord into its individual notes — decomposing a complex signal into simple frequencies |
| Neural network | A musician who internalizes thousands of songs and can improvise in any style |
| Overfitting | A musician who can only play the songs they practiced — can't improvise |
| Attention mechanism | A conductor listening to the orchestra — deciding which instrument to focus on right now |
| Embedding | Turning any song into a set of coordinates: tempo, key, mood, genre |
| Generative model | A composer who's absorbed every Bach piece and writes new ones in that style |

---

## Architecture / Construction

Great for: system design, APIs, databases, software architecture

| Concept | Architecture analogy |
|---------|----------------------|
| API | A power socket standard — any compatible plug works without knowing the wiring inside the wall |
| Database | The building's filing system — everything has a designated drawer; retrieval is fast |
| Microservices | Individual specialist contractors (plumber, electrician, carpenter) vs. one generalist |
| Cache | Keeping frequently used tools on the workbench rather than fetching from the storeroom |
| Load balancer | A site foreman routing new tasks to whichever worker is least busy |
| Redundancy | Building two staircases so if one is blocked, the building isn't trapped |
