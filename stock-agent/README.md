# 📈 Stock Agent — Claude Skill
> An AI-powered market analyst skill for Claude that reads OHLCV data, computes technical indicators, detects ML-style patterns, and produces structured trading analysis with plain English reasoning.

---

## ✨ What It Does

Paste in OHLCV data, a ticker, or describe your trade idea — Stock Agent will:

- 📊 **Scan** price data for technical signals across trend, momentum, volatility, and volume
- 🤖 **Detect** candlestick and chart patterns using ML-style probabilistic logic
- 💡 **Assist** trade decisions with entry zones, stop-loss, take-profit, and R:R ratios
- 🔬 **Backtest** simple rule-based strategies against your historical data
- 📖 **Explain** every signal in plain English — not just the number, but what it means

---

## 🧠 Analysis Modes

| Mode | Trigger | What It Does |
|---|---|---|
| 📊 Technical Scan | "Analyse this data" / paste OHLCV | RSI, MACD, EMA, Bollinger Bands, ATR, OBV, S/R |
| 🤖 ML Pattern Recognition | "Predict" / "detect patterns" | Candlestick patterns, chart patterns, regression channel, divergence |
| 💡 Decision Assistant | "Should I buy?" / "Entry point?" | Signal confluence, entry/stop/target, R:R, position sizing |
| 🔬 Backtest Evaluator | "Backtest this strategy" | Win rate, drawdown, avg gain/loss, vs buy-and-hold |

---

## 📡 Indicators Covered

| Category | Indicators |
|---|---|
| 📈 Trend | EMA 20/50/200, ADX, Golden/Death Cross |
| ⚡ Momentum | RSI (14), MACD (12/26/9), Stochastic (14,3,3) |
| 🌊 Volatility | Bollinger Bands (20, 2σ), ATR (14), BB Width |
| 📦 Volume | OBV, Volume vs 20-day average |
| 🎯 Levels | Swing highs/lows, round numbers, MA confluence |

---

## 🕯️ Patterns Detected

**Candlestick patterns:**
Doji · Hammer · Shooting Star · Engulfing (Bull/Bear) · Morning/Evening Star · Three White Soldiers · Three Black Crows · Marubozu

**Chart patterns:**
Head & Shoulders · Double Top/Bottom · Bull/Bear Flag · Ascending/Descending/Symmetrical Triangle · Cup and Handle · Rising/Falling Wedge

**ML-style analysis:**
Linear regression channel · RSI/MACD divergence (bullish & bearish) · Momentum convergence scoring · Seasonality detection (2yr+ datasets) · Probabilistic continuation/reversal outlook

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `stock-agent.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `stock-agent/` folder
3. Rename to `stock-agent.skill`
4. Install via Claude.ai Settings → Skills

---

## 🚀 How to Use

Once installed, talk to Claude naturally:

```
"Analyse this OHLCV data"
"Should I buy AAPL right now?"
"Detect chart patterns in this data"
"Backtest: buy when RSI < 30, sell when RSI > 70"
"What's my entry point and stop-loss?"
"Is there a bullish divergence forming?"
"Give me a full technical analysis"
```

Then paste your OHLCV data in any format — CSV, JSON, or plain table.

---

## 📊 Example Output

```
## 📊 Technical Analysis — AAPL (Daily, 90 days)

### Trend
EMA 20 (184.2) > EMA 50 (181.7) > EMA 200 (174.3) — bullish structure
ADX: 31.4 → established trend, trend strategies valid

### Momentum
RSI: 58.2 → neutral-bullish, room to run before overbought
MACD: Line (1.84) above Signal (1.21), histogram expanding → bullish momentum
Stochastic: %K 67 / %D 61, trending upward, not yet overbought

### Volatility
Bollinger Bands: price at midline after bounce from lower band
BB Width: 2.8% → normal volatility, no squeeze/breakout alert
ATR (14): 3.12 → normal for this asset

### Volume
OBV trending upward with price — healthy accumulation
Yesterday's volume: 1.3× 20-day average → above average conviction

### Key Levels
Resistance: 188.40 (recent swing high), 190.00 (round number)
Support: 181.50 (EMA 50 + prior swing low confluence)

### 🔔 Signal
BULLISH — Confidence: Medium
EMA structure, MACD momentum, and OBV are aligned bullish. RSI has room
before overbought. Key risk: resistance cluster at 188–190 may cap upside
short-term. Watch for volume confirmation on any breakout.

⚠️ This is not financial advice. Always do your own research.
```

---

## ⚠️ Disclaimer

Stock Agent is a **research and educational tool**. It:
- Does not have access to live market data — you must paste OHLCV data
- Cannot place trades or connect to any broker
- Does not provide personalised financial advice
- Uses probabilistic pattern analysis — past patterns do not guarantee future results

Always consult a licensed financial adviser before making investment decisions.

---

## 📁 Skill Structure

```
stock-agent/
├── SKILL.md                          # Core skill logic & instructions
├── README.md                         # This file
├── stock-agent.skill                 # Installable skill package
└── references/
    ├── technical-indicators.md       # Full indicator reference & signal rules
    └── ml-strategy-patterns.md      # Candlestick, chart pattern & ML logic
```

---

## 🧠 Built With

- [Claude Skills](https://claude.ai) — Anthropic's skill system for extending Claude
- [Technical Analysis](https://www.investopedia.com/technical-analysis-4689657) — Standard TA methodology
- [Candlestick Patterns](https://www.investopedia.com/articles/active-trading/092315/5-most-powerful-candlestick-patterns.asp) — Japanese candlestick charting
- OHLCV data compatible with Yahoo Finance, TradingView, Alpha Vantage exports

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built as a portfolio project demonstrating Claude skill development, quantitative finance knowledge, and AI-assisted trading analysis.*
