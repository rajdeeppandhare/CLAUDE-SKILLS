# Technical Indicators Reference

> Reference guide for the Stock Agent skill — defines every indicator used, its standard parameters, interpretation rules, and signal logic.

---

## Trend Indicators

### EMA — Exponential Moving Average
**Periods used:** 20 (short), 50 (medium), 200 (long)
**Formula:** EMA = Price × k + EMA(prev) × (1 − k), where k = 2/(N+1)
**Interpretation:**
- Price above EMA 20/50/200 → bullish structure
- EMA 20 crosses above EMA 50 → Golden Cross (bullish)
- EMA 20 crosses below EMA 50 → Death Cross (bearish)
- All three EMAs stacked (20 > 50 > 200) → strong uptrend
- Price repeatedly rejecting EMA 20 from below → resistance confirmation

**Signal thresholds:**
| Configuration | Signal |
|---|---|
| Price > EMA20 > EMA50 > EMA200 | Strong bullish |
| Price > EMA20, EMA20 < EMA50 | Weak bullish / recovery |
| Price < EMA20 < EMA50 < EMA200 | Strong bearish |
| EMA20 crossing EMA50 | Trend change alert |

---

### ADX — Average Directional Index
**Period:** 14
**Range:** 0–100
**Interpretation:**
- ADX < 20 → no trend / ranging market (avoid trend strategies)
- ADX 20–25 → emerging trend
- ADX 25–40 → established trend (trend strategies valid)
- ADX > 40 → strong trend (momentum strategies valid)
- ADX > 60 → extremely strong, potential exhaustion
- +DI > -DI → uptrend dominant; -DI > +DI → downtrend dominant

---

## Momentum Indicators

### RSI — Relative Strength Index
**Period:** 14
**Range:** 0–100
**Formula:** RSI = 100 − (100 / (1 + RS)), RS = avg gain / avg loss over 14 periods

**Interpretation:**
- RSI > 70 → overbought (potential reversal or pause)
- RSI < 30 → oversold (potential bounce or reversal)
- RSI 40–60 → neutral zone
- RSI > 50 in uptrend → bullish momentum confirmation
- RSI < 50 in downtrend → bearish momentum confirmation

**Divergence rules (high-priority signals):**
- Bullish divergence: price makes lower low, RSI makes higher low → reversal signal
- Bearish divergence: price makes higher high, RSI makes lower high → reversal signal
- Hidden bullish: price higher low, RSI lower low → continuation of uptrend
- Hidden bearish: price lower high, RSI higher high → continuation of downtrend

---

### MACD — Moving Average Convergence Divergence
**Parameters:** Fast EMA 12 / Slow EMA 26 / Signal EMA 9
**Components:**
- MACD Line = EMA(12) − EMA(26)
- Signal Line = EMA(9) of MACD Line
- Histogram = MACD Line − Signal Line

**Interpretation:**
- MACD Line crosses above Signal → bullish crossover
- MACD Line crosses below Signal → bearish crossover
- Histogram turning positive / increasing → bullish momentum
- Histogram shrinking toward zero → momentum losing steam
- MACD above zero line → overall bullish regime
- MACD below zero line → overall bearish regime

**Crossover reliability:**
- Crossover above zero line → stronger bullish signal
- Crossover below zero line → stronger bearish signal
- Crossovers in choppy markets → lower reliability (check ADX)

---

### Stochastic Oscillator
**Parameters:** %K period 14, %D smoothing 3, Slow 3
**Range:** 0–100

**Interpretation:**
- %K > 80 → overbought
- %K < 20 → oversold
- %K crosses above %D in oversold zone → buy signal
- %K crosses below %D in overbought zone → sell signal
- Divergence with price → same rules as RSI divergence

---

## Volatility Indicators

### Bollinger Bands
**Parameters:** 20-period SMA, ±2 standard deviations
**Components:** Upper Band, Middle Band (SMA20), Lower Band

**Interpretation:**
- Price touching upper band → overbought in current range
- Price touching lower band → oversold in current range
- Bands squeezing narrow (BB Width < 1%) → volatility squeeze, breakout imminent
- Band expansion → breakout in progress
- Price walking the upper band → strong uptrend
- Price walking the lower band → strong downtrend
- Mean reversion: price snaps back to middle band after extreme extension

**BB Width formula:** (Upper − Lower) / Middle × 100

---

### ATR — Average True Range
**Period:** 14
**Formula:** True Range = max(High−Low, |High−Prev Close|, |Low−Prev Close|)
ATR = EMA(14) of True Range

**Use cases:**
- Stop-loss sizing: stop = entry − (1.5 × ATR) for long positions
- Take-profit: target = entry + (2 × ATR) for 1:1.33 R:R minimum
- Volatility filter: if ATR > 2× its 50-day average → elevated risk, reduce position size
- Breakout confirmation: volume spike + ATR expansion = valid breakout

**ATR interpretation table:**
| ATR vs 50-day Avg | Regime |
|---|---|
| ATR < 0.7× avg | Low volatility / squeeze |
| ATR 0.7–1.3× avg | Normal |
| ATR > 1.5× avg | Elevated — reduce size |
| ATR > 2.5× avg | Extreme — use caution |

---

## Volume Indicators

### OBV — On-Balance Volume
**Formula:**
- Up day: OBV = OBV(prev) + Volume
- Down day: OBV = OBV(prev) − Volume

**Interpretation:**
- OBV rising with price → healthy trend, institutional accumulation
- OBV falling with price → healthy downtrend, distribution
- OBV rising while price flat/falling → bullish divergence (accumulation before breakout)
- OBV falling while price flat/rising → bearish divergence (distribution before breakdown)

---

### Volume vs 20-day Average
**Threshold rules:**
- Volume > 1.5× avg on up day → strong buying conviction
- Volume > 1.5× avg on down day → strong selling conviction
- Volume < 0.5× avg → low conviction move, treat with caution
- Volume spike on breakout candle → breakout likely valid
- Volume spike on reversal candle → reversal likely valid

---

## Support & Resistance Identification

**Method 1 — Swing highs/lows:**
- Swing high: candle whose high is higher than the 2 candles on each side
- Swing low: candle whose low is lower than the 2 candles on each side
- Recent (within 20 candles) swing levels carry more weight

**Method 2 — Round number levels:**
- Round numbers (100, 150, 200, etc.) act as psychological S/R
- Report the nearest round number above and below current price

**Method 3 — Moving average confluence:**
- When EMA 50 or EMA 200 aligns with a swing level → high-confluence zone

**Strength rating:**
| Touches | Strength |
|---|---|
| 1 touch | Weak |
| 2 touches | Moderate |
| 3+ touches | Strong |

---

## Signal Confluence Scoring

When aggregating signals for a final verdict, score each indicator:

| Indicator | Bullish | Bearish | Neutral |
|---|---|---|---|
| EMA structure | +2 | -2 | 0 |
| ADX (> 25) | confirms | confirms | weakens all |
| RSI | +1 if 50–70 | -1 if 30–50 | 0 |
| MACD crossover | +2 | -2 | 0 |
| BB position | +1 lower band bounce | -1 upper band rejection | 0 |
| Volume | +1 high vol up | -1 high vol down | 0 |
| OBV divergence | +2 bullish div | -2 bearish div | 0 |

**Score interpretation:**
- +5 or higher → BULLISH, High confidence
- +3 to +4 → BULLISH, Medium confidence
- -2 to +2 → NEUTRAL
- -3 to -4 → BEARISH, Medium confidence
- -5 or lower → BEARISH, High confidence
