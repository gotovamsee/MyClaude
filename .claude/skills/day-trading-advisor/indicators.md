# Technical Indicators Reference

Calculation methods and interpretation guide for the day-trading-advisor skill.

## RSI — Relative Strength Index (14-period)

### Calculation
1. For each period, calculate: gain = max(close - prev_close, 0), loss = max(prev_close - close, 0)
2. Average gain = SMA of gains over 14 periods (first), then EMA thereafter
3. Average loss = SMA of losses over 14 periods (first), then EMA thereafter
4. RS = Average Gain / Average Loss
5. RSI = 100 - (100 / (1 + RS))

### Interpretation
| RSI Range | Condition | Day Trading Signal |
|-----------|-----------|-------------------|
| 80-100 | Extremely overbought | Strong sell signal, look for bearish reversal patterns |
| 70-80 | Overbought | Caution for longs, watch for reversal |
| 50-70 | Bullish momentum | Favor long entries on pullbacks |
| 40-50 | Neutral / weak | No clear edge, wait for direction |
| 30-40 | Bearish momentum | Favor short entries on bounces |
| 20-30 | Oversold | Caution for shorts, watch for reversal |
| 0-20 | Extremely oversold | Strong buy signal, look for bullish reversal patterns |

### RSI Divergences (High-value signals)
- **Bullish divergence**: Price makes lower low, RSI makes higher low → likely reversal up
- **Bearish divergence**: Price makes higher high, RSI makes lower high → likely reversal down

## Moving Averages

### SMA (Simple Moving Average)
- **SMA 9**: Short-term trend (very responsive, noisy)
- **SMA 20**: Medium-term trend (day trading standard)
- **SMA 50**: Longer-term trend (swing context for day traders)
- Calculation: Sum of last N closing prices / N

### EMA (Exponential Moving Average)
- **EMA 9**: Fast signal line
- **EMA 21**: Standard day trading trend line
- Multiplier = 2 / (N + 1)
- EMA = (Close - Previous EMA) * Multiplier + Previous EMA

### Moving Average Signals
| Signal | Condition | Strength |
|--------|-----------|----------|
| **Golden cross** | Short MA crosses above long MA | Bullish — strong if 20 crosses above 50 |
| **Death cross** | Short MA crosses below long MA | Bearish — strong if 20 crosses below 50 |
| **Price above all MAs** | Stacked: Price > 9 > 20 > 50 | Strong uptrend |
| **Price below all MAs** | Stacked: Price < 9 < 20 < 50 | Strong downtrend |
| **MA compression** | All MAs converging | Breakout imminent — direction TBD |

## MACD — Moving Average Convergence Divergence

### Calculation (12, 26, 9)
1. MACD Line = EMA(12) - EMA(26)
2. Signal Line = EMA(9) of MACD Line
3. Histogram = MACD Line - Signal Line

### Interpretation
| Signal | Condition | Action |
|--------|-----------|--------|
| Bullish crossover | MACD crosses above signal line | Look for long entry |
| Bearish crossover | MACD crosses below signal line | Look for short entry |
| Histogram growing | Bars increasing in size | Momentum strengthening |
| Histogram shrinking | Bars decreasing in size | Momentum weakening, possible reversal |
| Zero line cross up | MACD crosses above zero | Trend turning bullish |
| Zero line cross down | MACD crosses below zero | Trend turning bearish |

## VWAP — Volume Weighted Average Price

### Calculation
- VWAP = Cumulative(Price * Volume) / Cumulative(Volume)
- Resets each trading session

### Interpretation
| Condition | Signal |
|-----------|--------|
| Price > VWAP | Bullish intraday bias, institutional buying |
| Price < VWAP | Bearish intraday bias, institutional selling |
| Price bouncing off VWAP | VWAP acting as dynamic support/resistance |
| Price far from VWAP | Mean reversion likely, caution on new entries |

## Position Sizing Formula

```
Risk Amount = Portfolio Value * Risk Percentage (default 1-2%)
Position Size (shares) = Risk Amount / (Entry Price - Stop-Loss Price)
Position Value = Position Size * Entry Price
```

### Example (Danish stock in DKK)
- Portfolio: 500,000 DKK
- Risk: 1% = 5,000 DKK
- Entry: 750 DKK, Stop-loss: 735 DKK (15 DKK risk per share)
- Position size: 5,000 / 15 = 333 shares
- Position value: 333 * 750 = 249,750 DKK

### Risk Management Rules
1. **Max risk per trade**: 1-2% of portfolio (never exceed without explicit override)
2. **Max daily loss**: 3-5% of portfolio — stop trading for the day if hit
3. **Risk/Reward minimum**: 1:1.5 (prefer 1:2 or better)
4. **Correlated positions**: If holding multiple trades in same sector, combine risk cannot exceed 4%

## Danish Market Notes

- **Exchange**: Nasdaq Copenhagen (CPH)
- **Trading hours**: 09:00 - 17:00 CET (08:00 - 16:00 UTC)
- **Pre-market**: Limited, 08:00 - 09:00 CET
- **Currency**: DKK (Danish Krone)
- **Index**: OMXC25 (top 25 Danish stocks by market cap)
- **Yahoo Finance suffix**: `.CO` (e.g., NOVO-B.CO, DSV.CO, MAERSK-B.CO)
- **Tax**: Aktieskat — gains on listed shares taxed at 27% (up to 61,000 DKK) and 42% above that threshold (2024 rates, verify current rates)
- **Common large-cap tickers**: NOVO-B.CO, MAERSK-B.CO, DSV.CO, CARL-B.CO, VWS.CO, ORSTED.CO, PNDORA.CO, COLO-B.CO, DEMANT.CO, GN.CO
