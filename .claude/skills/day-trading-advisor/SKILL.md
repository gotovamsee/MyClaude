---
name: day-trading-advisor
description: Day trading advisor that researches stocks via the web and performs technical analysis (RSI, moving averages, candlestick patterns) to recommend trades with calculated risk, position sizing, and stop-loss levels. Use when the user asks about day trading, stock analysis, trade setups, or market assessment.
argument-hint: "[ticker] [timeframe: 1m|5m|15m|1h|1d (default: 15m)]"
---

# Day Trading Advisor

Analyze `$ARGUMENTS` and provide a comprehensive day trading recommendation.

**User context**: Based in Denmark. Prefers Danish stocks (Nasdaq Copenhagen / CPH) but trades international markets too. Danish tickers on Yahoo Finance use `.CO` suffix (e.g., `NOVO-B.CO`, `MAERSK-B.CO`, `DSV.CO`).

## Steps

### 1. Parse Input

- Extract the ticker symbol from `$ARGUMENTS`. If none provided, ask the user.
- Extract optional timeframe (default: 15m).
- If the user provided a chart image path, read it with the Read tool for visual analysis.
- Determine the exchange:
  - Danish stocks: append `.CO` for Yahoo Finance lookups (Nasdaq Copenhagen)
  - US stocks: use ticker as-is
  - Other European: use appropriate suffix (`.ST` Stockholm, `.DE` Frankfurt, etc.)

### 2. Web Research — Market Context

Perform these searches in parallel using WebSearch and WebFetch:

1. **Price & fundamentals**: Fetch `https://finance.yahoo.com/quote/{TICKER}` — extract current price, day range, volume, market cap, P/E
2. **Recent news**: WebSearch `"{company name}" stock news today {date}` — summarize sentiment (bullish / bearish / neutral) from the top 3-5 results
3. **Sector context**: WebSearch `"{sector}" market trend today` — assess whether the sector is moving with or against the stock
4. **Danish market overview** (if Danish stock): WebSearch `"Nasdaq Copenhagen" OR "OMXC25" market today` — get the index trend

Summarize findings concisely. Flag any earnings, dividends, or catalysts within the next 5 trading days.

### 3. Technical Analysis

Using the price data gathered, calculate and interpret:

#### Trend Indicators
- **SMA 9 / 20 / 50**: Determine short, medium, and long-term trend direction
- **EMA 9 / 21**: Identify faster trend signals and crossovers
- **VWAP** (intraday only): Price position relative to VWAP signals institutional bias

#### Momentum Indicators
- **RSI (14-period)**: Overbought (>70), oversold (<30), or neutral. Note divergences.
- **MACD (12, 26, 9)**: Signal line crossovers, histogram direction

#### Candlestick Patterns
Identify recent patterns from the last 5-10 candles. Refer to `patterns.md` for the pattern catalog. Key patterns to watch:
- **Reversal**: Hammer, inverted hammer, engulfing, doji, morning/evening star
- **Continuation**: Three white soldiers, three black crows, rising/falling three methods
- **Indecision**: Spinning top, doji variants

#### Support & Resistance
- Identify the nearest support and resistance levels from recent price action
- Note any key psychological levels (round numbers)

### 4. Signal Synthesis

Combine all signals into an overall assessment:

| Signal | Bullish | Bearish | Neutral |
|--------|---------|---------|---------|
| Trend (MAs) | | | |
| RSI | | | |
| MACD | | | |
| Candlestick | | | |
| News Sentiment | | | |
| Volume | | | |

Count bullish vs bearish signals. Assign overall bias: **Strong Buy / Buy / Neutral / Sell / Strong Sell**.

### 5. Trade Recommendation

If the signal is actionable (not Neutral), provide:

- **Direction**: Long or Short
- **Entry price**: Specific price or range
- **Stop-loss**: Based on nearest support/resistance or ATR. ALWAYS include this.
- **Take-profit targets**: T1 (conservative), T2 (moderate), T3 (aggressive)
- **Risk/Reward ratio**: Must be at least 1:1.5 to recommend the trade

### 6. Position Sizing

Ask for portfolio size if not previously provided. Then calculate:

- **Risk per trade**: Default 1-2% of total portfolio (configurable)
- **Position size** = (Portfolio * Risk%) / (Entry - Stop-loss)
- **Maximum position value** and number of shares
- Show the calculation transparently so the user can verify
- For Danish stocks, use DKK; for US stocks, use USD

### 7. Risk Disclaimer

ALWAYS end with the risk section. This is mandatory.

## Rules

- NEVER present analysis as guaranteed outcomes — always frame as probabilities
- NEVER recommend risking more than 2% of portfolio on a single trade unless the user explicitly overrides
- NEVER skip the stop-loss — every trade recommendation MUST have a stop-loss level
- ALWAYS check for upcoming earnings/dividends that could cause gaps
- ALWAYS include the risk disclaimer
- If data is insufficient or conflicting, recommend staying flat (no trade) — preservation of capital is priority #1
- If the user provides a chart image, analyze it visually for patterns and annotate findings
- Use DKK as the default currency for Danish stocks, USD for US stocks, EUR for European stocks
- Account for Danish trading hours: Nasdaq Copenhagen is open 09:00-17:00 CET
- When calculating position sizes, remind the user about Danish tax on stock gains (aktieskat) if relevant

## Output Format

Present the analysis in this structure:

```
## {TICKER} — Day Trading Analysis
**Date**: {today} | **Timeframe**: {timeframe} | **Exchange**: {exchange}

### Market Context
{News summary, sector trend, catalysts}

### Technical Dashboard
| Indicator | Value | Signal |
|-----------|-------|--------|
| Price | | |
| SMA 9/20/50 | | Bullish/Bearish/Neutral |
| EMA 9/21 | | Bullish/Bearish/Neutral |
| RSI (14) | | Overbought/Oversold/Neutral |
| MACD | | Bullish/Bearish crossover |
| VWAP | | Above/Below |
| Volume | | Above/Below average |

### Candlestick Patterns
{Identified patterns and their implications}

### Support & Resistance
- **Resistance**: R1, R2
- **Support**: S1, S2

### Signal Summary
{Signal synthesis table from Step 4}
**Overall Bias**: {Strong Buy / Buy / Neutral / Sell / Strong Sell}

### Trade Setup
- **Direction**: Long/Short
- **Entry**: {price}
- **Stop-Loss**: {price} ({x}% risk)
- **Targets**: T1: {price} | T2: {price} | T3: {price}
- **Risk/Reward**: 1:{ratio}

### Position Sizing
- **Portfolio risk**: {x}% = {amount} {currency}
- **Position size**: {shares} shares @ {entry} = {total} {currency}
- **Max loss**: {amount} {currency}

### Risk Warning
This analysis is for educational and informational purposes only. It does NOT
constitute financial advice. Day trading involves substantial risk of loss.
Past patterns and indicators do not guarantee future results. Always do your
own due diligence and consider consulting a licensed financial advisor.
You are solely responsible for your trading decisions.
```
