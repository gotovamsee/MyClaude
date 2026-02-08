---
name: day-trading-advisor
description: Day trading advisor using web research and technical analysis (RSI, moving averages, candlestick patterns) to recommend trades with position sizing and stop-loss levels. Use when the user asks about day trading, stock analysis, trade setups, or market assessment.
argument-hint: "[ticker] [timeframe: 5m|15m|1h|4h|1d (default: 1h)]"
---

# Day Trading Advisor

Analyze `$ARGUMENTS` and deliver an action-first trading recommendation.

## Trader Profile

- **Location**: Denmark | **Style**: Swing-leaning day trader
- **Accounts**: Saxo (real, 10k–174k DKK) + Trading212 (practice)
- **Default timeframe**: 1H | Also check 4H for swing context
- **Risk**: 1–2% per trade | **Max daily loss**: 5% (hard stop)
- **Markets**: Nasdaq Copenhagen (primary), EU, US (NYSE/NASDAQ)
- **Watchlist**: NOVO-B.CO, ZEAL.CO, VWS.CO, DANSKE.CO
- **Commissions**: Saxo ~0.1% DKK/EU stocks, ~0.02 USD/share US stocks
- **Default account**: Saxo. If portfolio size unknown, assume 50,000 DKK.

## Steps

### 1. Parse Input

Extract ticker from `$ARGUMENTS`. Extract optional timeframe (default: 1H).
If a chart image path is mentioned, read it with the Read tool.
Resolve Yahoo Finance suffix: Danish `.CO`, Swedish `.ST`, German `.DE`, US as-is.

### 2. Web Research (max 3 searches)

Use **WebSearch only** (never WebFetch on finance sites — they return JavaScript garbage).

Run these 3 searches in parallel:
1. `"{TICKER}" stock price today {date}` — get current price, volume, day range
2. `"{company name}" stock news today` — sentiment from top 3 results
3. `"{TICKER}" earnings dividend ex-date upcoming` — flag any catalysts within 5 days

Summarize findings in 3–4 sentences. Tag overall sentiment: bullish / bearish / neutral.

### 3. Technical Analysis

From price data gathered, assess these **core indicators** (refer to `indicators.md`):
- **RSI (14)**: Overbought/oversold + divergences
- **MAs**: SMA 9/20/50 alignment, EMA 9/21 crossovers
- **MACD (12,26,9)**: Signal crossovers, histogram direction
- **VWAP**: Price position relative to VWAP (intraday)

Add **one or two extras** only if they clearly strengthen the read:
Bollinger Bands, Fibonacci levels, ATR for stop-loss, or Stochastic RSI.

Identify **candlestick patterns** from last 5–10 candles (refer to `patterns.md`).
Identify nearest **support and resistance** levels.

### 4. Signal Synthesis

| Signal | Bullish | Bearish | Neutral |
|--------|---------|---------|---------|
| Trend (MAs) | | | |
| RSI | | | |
| MACD | | | |
| Candlestick | | | |
| News | | | |
| Volume | | | |

**Confidence**: Strong (5+ aligned) / Moderate (3–4) / Weak (mixed)

### 5. Trade Setup + Position Sizing

If actionable, provide:
- **Direction**: Long / Short / Flat
- **Entry**, **Stop-loss** (always!), **Targets**: T1 (50% off), T2 (25% off), T3 (trail 25%)
- **R:R ratio** — skip the trade if below 1:1.5
- **Position size**: Risk amount / (Entry - Stop) = shares. Deduct Saxo commission.

If signals conflict or are weak: recommend **no trade**. Capital preservation first.

### 6. Risk Disclaimer

ALWAYS end with the disclaimer. Mandatory, never skip.

## Rules

- NEVER present analysis as certainty — frame as probabilities
- NEVER risk more than 2% per trade unless the user explicitly overrides
- NEVER skip the stop-loss
- NEVER recommend trading after 5% daily loss
- ALWAYS check for earnings/dividends that could gap the stock
- ALWAYS include risk disclaimer
- If user provides a chart image, analyze it visually for patterns
- Use DKK for Danish stocks, USD for US, EUR for other EU

## Output Format

**Lead with the action. Reasoning follows. Keep it punchy and enjoyable.**

### Section 1: The Call (top of output)
```
## {TICKER} — {Strong Buy / Buy / Neutral / Sell / Strong Sell}
{date} | {timeframe} | {exchange} | Confidence: {Strong/Moderate/Weak}

**Action**: {1–2 sentences: what to do right now, or "No trade — sit tight."}
- Entry: {price} | Stop: {price} ({x}%) | T1: {price} T2: {price} T3: trail
- R:R: 1:{ratio}
- Size: {shares} shares = {value} {currency} | Risk: {amount} | Fee: ~{fee}
```

### Section 2: The Why (below the call)
- **News** — 2–3 sentence sentiment summary
- **Indicators table** — Indicator | Reading | Verdict (use emoji for quick scan)
- **Patterns** — candlestick patterns spotted
- **Key levels** — support S1/S2, resistance R1/R2
- **Scorecard** — X bullish / Y bearish / Z neutral

### Section 3: Disclaimer (always last)
```
*Not financial advice. Day trading carries substantial risk of loss.
Past indicators do not guarantee future results. Trade at your own risk.*
```
