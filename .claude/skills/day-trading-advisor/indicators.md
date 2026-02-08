# Technical Indicators Reference

Calculation methods and interpretation guide for the day-trading-advisor skill.
Optimized for swing-leaning day trading on 1H default timeframe.

## Core Indicators (Always Used)

### RSI — Relative Strength Index (14-period)

**Calculation:**
1. Gain = max(close - prev_close, 0), Loss = max(prev_close - close, 0) per period
2. Avg Gain/Loss = SMA over 14 periods (first), then EMA thereafter
3. RS = Avg Gain / Avg Loss
4. RSI = 100 - (100 / (1 + RS))

**Interpretation for 1H swing trading:**
| RSI Range | Condition | Action |
|-----------|-----------|--------|
| 80–100 | Extremely overbought | Strong sell signal, look for bearish reversal patterns |
| 70–80 | Overbought | Caution for longs, tighten stops, watch for reversal |
| 50–70 | Bullish momentum | Favor long entries on pullbacks to 50–55 zone |
| 40–50 | Neutral / weak | No clear edge — wait or use other confirmations |
| 30–40 | Bearish momentum | Favor short entries on bounces to 45–50 zone |
| 20–30 | Oversold | Caution for shorts, watch for bullish reversal |
| 0–20 | Extremely oversold | Strong buy signal with reversal pattern confirmation |

**RSI Divergences (high-value signals):**
- **Bullish divergence**: Price lower low + RSI higher low → likely reversal up
- **Bearish divergence**: Price higher high + RSI lower high → likely reversal down

### Moving Averages

**SMA (Simple Moving Average):**
- SMA 9: Short-term trend (entry timing on 15m)
- SMA 20: Medium-term trend (primary on 1H)
- SMA 50: Longer-term trend (swing context)

**EMA (Exponential Moving Average):**
- EMA 9: Fast signal line
- EMA 21: Standard swing trading trend line
- Multiplier = 2 / (N + 1); EMA = (Close - Prev EMA) * Mult + Prev EMA

**Signals:**
| Signal | Condition | Strength |
|--------|-----------|----------|
| Golden cross | Short MA above long MA | Bullish — strong if 20 crosses above 50 |
| Death cross | Short MA below long MA | Bearish — strong if 20 crosses below 50 |
| Bullish stack | Price > 9 > 20 > 50 | Strong uptrend — look for pullback entries |
| Bearish stack | Price < 9 < 20 < 50 | Strong downtrend — look for bounce shorts |
| MA compression | All MAs converging | Breakout imminent — wait for direction |

### MACD (12, 26, 9)

**Calculation:** MACD Line = EMA(12) - EMA(26) | Signal = EMA(9) of MACD | Histogram = MACD - Signal

| Signal | Condition | Action |
|--------|-----------|--------|
| Bullish crossover | MACD above signal line | Long entry trigger |
| Bearish crossover | MACD below signal line | Short entry trigger |
| Histogram growing | Bars increasing | Momentum strengthening |
| Histogram shrinking | Bars decreasing | Momentum fading, watch for reversal |
| Zero line cross | MACD crosses zero | Trend direction confirmed |

### VWAP — Volume Weighted Average Price

VWAP = Cumulative(Price * Volume) / Cumulative(Volume). Resets each session.

| Condition | Implication |
|-----------|-------------|
| Price > VWAP | Bullish intraday bias, institutional buying |
| Price < VWAP | Bearish intraday bias, institutional selling |
| Bounce off VWAP | Dynamic support/resistance |
| Far from VWAP | Mean reversion likely — caution on new entries |

## Supplementary Indicators (Use When They Add Edge)

### Bollinger Bands (20, 2)
- Middle = SMA(20), Upper/Lower = Middle +/- 2 * StdDev(20)
- **Squeeze** (bands narrow): Low volatility, breakout coming
- **Walk the band**: Strong trend, price hugging upper/lower band
- **Mean reversion**: Price touching outer band + RSI extreme → reversal setup

### Stochastic RSI
- StochRSI = (RSI - Lowest RSI) / (Highest RSI - Lowest RSI) over 14 periods
- Overbought > 0.8, Oversold < 0.2
- Use to confirm RSI extremes — double confirmation is stronger

### Fibonacci Retracements
Key levels after a significant move:
- **38.2%** — shallow pullback (strong trend)
- **50.0%** — moderate pullback (healthy trend)
- **61.8%** — deep pullback (last chance for trend continuation)
- Combine with MA or S/R confluence for high-probability entries

### ATR — Average True Range (14)
- Measures volatility (not direction)
- **Stop-loss placement**: Entry - (1.5 * ATR) for longs, Entry + (1.5 * ATR) for shorts
- **Position filter**: If ATR is unusually high, reduce position size

### Volume Profile
- High-Volume Nodes (HVN): Act as support/resistance magnets
- Low-Volume Nodes (LVN): Price moves quickly through these — breakout zones
- Point of Control (POC): Highest-volume price level — strongest S/R

## Position Sizing

```
Risk Amount = Account Balance * Risk% (1–2%)
Shares = Risk Amount / |Entry - Stop-Loss|
Position Value = Shares * Entry
Net Profit = (Target - Entry) * Shares - Commission
```

**Commission estimates (Saxo):**
- Danish / EU stocks: ~0.1% of trade value (min ~29 DKK)
- US stocks: ~0.02 USD/share (min ~3 USD)

**Trading212 (practice):** Commission-free, but factor in spread

### Examples

**Danish stock (Saxo, 100,000 DKK account):**
- Risk: 1.5% = 1,500 DKK
- Entry: 750 DKK, Stop: 735 DKK → 15 DKK/share risk
- Shares: 1,500 / 15 = 100 shares
- Value: 100 * 750 = 75,000 DKK
- Commission: ~75 DKK (0.1%)
- T1 at 780 DKK → profit = (780-750)*50 - 75 = 1,425 DKK

**US stock (Saxo, ~2,000 USD allocation):**
- Risk: 2% = 40 USD
- Entry: $150, Stop: $146 → $4/share risk
- Shares: 40 / 4 = 10 shares
- Value: 10 * 150 = 1,500 USD
- Commission: ~0.20 USD

## Risk Management Rules

1. **Max risk per trade**: 1–2% of active account
2. **Max daily loss**: 5% — STOP trading for the day, no exceptions
3. **R:R minimum**: 1:1.5 (prefer 1:2+). Below 1:1.5 → skip the trade
4. **Correlated positions**: Combined risk in same sector ≤ 4%
5. **Scale out**: 50% at T1, 25% at T2, 25% trailing at T3
6. **Commission-aware**: Always deduct fees from profit calculations

## Danish Market Notes

- **Exchange**: Nasdaq Copenhagen (CPH)
- **Hours**: 09:00–17:00 CET (08:00–16:00 UTC)
- **Pre-market**: Limited, 08:00–09:00 CET
- **Currency**: DKK
- **Index**: OMXC25
- **Yahoo suffix**: `.CO`
- **Tax (aktieskat)**: 27% on gains up to ~61,000 DKK/year, 42% above (verify current rates)
- **Core watchlist**: NOVO-B.CO, ZEAL.CO, VWS.CO, DANSKE.CO

## US Market Notes

- **Exchanges**: NYSE, NASDAQ
- **Hours**: 15:30–22:00 CET (09:30–16:00 ET)
- **Pre-market**: 10:00–15:30 CET (04:00–09:30 ET)
- **Currency**: USD
- **Indices**: S&P 500, NASDAQ Composite, Dow Jones
- **Check US futures before Copenhagen open** — US sentiment affects EU markets
