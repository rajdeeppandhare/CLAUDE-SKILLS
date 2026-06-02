# ML Strategy Patterns Reference

> Reference guide for the Stock Agent skill — defines candlestick patterns, chart patterns, ML-style signal logic, and probabilistic pattern rules used in MODE 2 (ML Pattern Recognition).

---

## Candlestick Patterns

### Single-Candle Patterns

#### Doji
**Shape:** Open ≈ Close (body < 5% of total candle range), long wicks
**Context required:** Yes — meaningless in isolation
**Interpretation:**
- After uptrend → indecision, potential reversal
- After downtrend → indecision, potential reversal
- With high volume → more significant
**Historical accuracy:** ~55% reversal signal (low on its own, higher with confirmation)

#### Hammer
**Shape:** Small body at top, lower wick ≥ 2× body, minimal upper wick
**Context:** Must appear after a downtrend
**Interpretation:** Buyers rejected selling pressure — bullish reversal candidate
**Confirmation:** Next candle closes above hammer's high
**Historical accuracy:** ~60% with confirmation

#### Shooting Star
**Shape:** Small body at bottom, upper wick ≥ 2× body, minimal lower wick
**Context:** Must appear after an uptrend
**Interpretation:** Sellers rejected buying pressure — bearish reversal candidate
**Confirmation:** Next candle closes below shooting star's low
**Historical accuracy:** ~58% with confirmation

#### Bullish Engulfing
**Shape:** Second candle's body fully engulfs first candle's body; first is bearish, second is bullish
**Context:** After downtrend
**Historical accuracy:** ~63%

#### Bearish Engulfing
**Shape:** Second candle's body fully engulfs first; first is bullish, second is bearish
**Context:** After uptrend
**Historical accuracy:** ~62%

#### Marubozu (Bullish / Bearish)
**Shape:** Full body, no wicks (or minimal)
**Interpretation:** Strong momentum candle — trend continuation signal
**Note:** More valid at higher volume

---

### Multi-Candle Patterns

#### Morning Star
**Shape:** Three candles — large bearish, small body (gap or near-gap), large bullish closing above midpoint of first candle
**Context:** After downtrend
**Historical accuracy:** ~68%

#### Evening Star
**Shape:** Inverse of morning star — large bullish, small body, large bearish
**Context:** After uptrend
**Historical accuracy:** ~66%

#### Three White Soldiers
**Shape:** Three consecutive bullish candles, each opening in previous body, each closing near high
**Historical accuracy:** ~70% continuation

#### Three Black Crows
**Shape:** Three consecutive bearish candles, each opening in previous body, closing near low
**Historical accuracy:** ~69% continuation

---

## Chart Patterns

### Reversal Patterns

#### Head and Shoulders
**Structure:** Left shoulder (high) → Head (higher high) → Right shoulder (lower high), neckline connecting troughs
**Signal:** Bearish reversal when price breaks neckline
**Price target:** Neckline − (Head − Neckline) projected downward
**Volume:** Should decline from left shoulder to right shoulder; spike on breakdown
**Historical accuracy:** ~74%

#### Inverse Head and Shoulders
**Signal:** Bullish reversal
**Price target:** Neckline + (Neckline − Head) projected upward
**Historical accuracy:** ~72%

#### Double Top
**Structure:** Two highs at approximately the same level, trough between them
**Signal:** Bearish reversal on break below trough
**Price target:** Trough − (Top − Trough)
**Historical accuracy:** ~65%

#### Double Bottom
**Signal:** Bullish reversal on break above peak between the two bottoms
**Historical accuracy:** ~64%

---

### Continuation Patterns

#### Bull Flag
**Structure:** Sharp upward move (flagpole), followed by shallow downward channel (flag)
**Signal:** Bullish continuation on breakout above flag upper boundary
**Price target:** Flagpole length added to breakout point
**Volume:** High on pole, low during flag, high on breakout
**Historical accuracy:** ~68%

#### Bear Flag
**Inverse:** Sharp downward move, upward channel, breakout below lower boundary
**Historical accuracy:** ~67%

#### Ascending Triangle
**Structure:** Flat upper resistance, rising lower trendline
**Signal:** Bullish breakout above resistance
**Historical accuracy:** ~73% bullish resolution

#### Descending Triangle
**Structure:** Flat support, declining upper trendline
**Signal:** Bearish breakdown below support
**Historical accuracy:** ~72% bearish resolution

#### Symmetrical Triangle
**Structure:** Converging trendlines, both at angle
**Signal:** Breakout in direction of prior trend (continuation)
**Historical accuracy:** ~54% — lower reliability, always wait for breakout confirmation

#### Cup and Handle
**Structure:** Rounded bottom (cup), followed by short consolidation with slight downward drift (handle)
**Signal:** Bullish breakout above handle resistance
**Price target:** Cup depth added to breakout
**Historical accuracy:** ~65%

#### Wedge (Rising)
**Structure:** Both support and resistance trendlines slope upward, converging
**Signal:** Typically bearish — falling out of wedge
**Historical accuracy:** ~60%

#### Wedge (Falling)
**Signal:** Typically bullish
**Historical accuracy:** ~62%

---

## ML-Style Signal Logic

### Regression Channel Analysis

**Method:**
1. Compute linear regression on close prices over N periods (default 50)
2. Calculate standard deviation of residuals
3. Draw upper channel (+2σ) and lower channel (−2σ)

**Signal rules:**
- Price at +2σ from regression → overbought relative to trend
- Price at −2σ from regression → oversold relative to trend
- Price returning from +2σ to midline → mean reversion trade (short)
- Price returning from −2σ to midline → mean reversion trade (long)
- Channel slope > 15° upward → strong uptrend regime
- Channel slope < −15° downward → strong downtrend regime
- Price breaking above upper channel with volume → potential regime shift bullish
- Price breaking below lower channel with volume → potential regime shift bearish

---

### Momentum Divergence Detection

**Bullish divergence:**
- Condition: price[n] < price[n-k] AND rsi[n] > rsi[n-k]
- Strength: stronger if divergence spans 10+ candles
- Confirmation: MACD histogram also showing divergence

**Bearish divergence:**
- Condition: price[n] > price[n-k] AND rsi[n] < rsi[n-k]
- Confirmation: look for volume declining into highs

**Priority ranking (most to least reliable):**
1. RSI divergence + MACD divergence + volume divergence → very high reliability
2. RSI divergence + MACD divergence → high reliability
3. RSI divergence only → moderate reliability
4. Single-indicator divergence on short timeframe → low reliability

---

### Seasonality & Cycle Detection

**When to apply:** Only if dataset spans 2+ years (500+ candles on daily)

**Rules:**
- Split dataset into calendar months; compute average return per month
- If any month shows >1.5× average return consistently → flag as seasonal strength
- Quarter-end effects: stocks often rally final 2 weeks of each quarter (window dressing)
- January effect: small caps historically stronger in January
- "Sell in May" pattern: May–October historically weaker for equities (S&P 500 data)

**Output format:**
"Based on historical patterns in this dataset, [Month] has shown an average return of +X% across [N] years of data. This suggests potential seasonal tailwind / headwind."

---

### Probabilistic Outlook Table

Use this when combining multiple pattern signals:

| Patterns Detected | Bullish Probability | Bearish Probability |
|---|---|---|
| 3+ bullish patterns aligned | 70–80% | 20–30% |
| 2 bullish patterns | 60–70% | 30–40% |
| 1 bullish pattern | 55–60% | 40–45% |
| Conflicting signals | 45–55% | 45–55% |
| 1 bearish pattern | 40–45% | 55–60% |
| 2 bearish patterns | 30–40% | 60–70% |
| 3+ bearish patterns aligned | 20–30% | 70–80% |

**Always state:** These are historical base rates, not forecasts. Every trade requires risk management regardless of probability.

---

## Risk Management Rules

### Position Sizing (1% Risk Model)
```
Account Size × 1% = Max Risk per Trade (£/$ amount)
Max Risk ÷ (Entry − Stop-Loss) = Position Size (shares/units)
```

Example: £10,000 account, 1% risk = £100 max loss
Entry £50, Stop £48 → risk per share = £2
Position size = £100 ÷ £2 = 50 shares

### R:R Minimum Thresholds
- R:R < 1:1 → skip the trade
- R:R 1:1 to 1.5:1 → only with high-confidence signal
- R:R 1.5:1 to 2.5:1 → standard acceptable range
- R:R > 3:1 → excellent setup, prioritise

### Stop-Loss Methods
| Method | Formula | Use case |
|---|---|---|
| ATR stop | Entry − (1.5 × ATR14) | Most situations |
| Swing stop | Below most recent swing low | Trend-following |
| Structure stop | Below key S/R level | Breakout trades |
| Fixed % stop | Entry × 0.97 (3% stop) | Simple / beginner |

### Take-Profit Methods
- TP1: Entry + (1.5 × ATR) — partial exit
- TP2: Next key resistance level
- TP3: Pattern price target (e.g. H&S measured move)
- Trailing stop: Move stop to breakeven once TP1 is hit
