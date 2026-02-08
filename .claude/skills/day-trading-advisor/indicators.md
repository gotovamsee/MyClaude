# Technical Indicators — Quick Reference

Optimized for swing-leaning day trading on 1H timeframe.

## RSI (14-period)

| RSI | Condition | Action |
|-----|-----------|--------|
| >70 | Overbought | Caution for longs, watch for bearish reversal |
| 50–70 | Bullish momentum | Favor long entries on pullbacks |
| 30–50 | Bearish/neutral | Wait for direction or favor shorts |
| <30 | Oversold | Watch for bullish reversal confirmation |

**Divergences** — Price makes new high/low but RSI doesn't = likely reversal.

## Moving Averages

- **SMA 9/20/50** — short/medium/long term trend
- **EMA 9/21** — faster signals for entry timing
- **Bullish stack**: Price > 9 > 20 > 50 | **Bearish stack**: Price < 9 < 20 < 50
- **Golden cross** (short above long) = bullish | **Death cross** = bearish
- **Compression** (all MAs converging) = breakout imminent

## MACD (12, 26, 9)

- Bullish crossover (MACD above signal) = long trigger
- Bearish crossover = short trigger
- Histogram growing = momentum strengthening
- Zero line cross = trend direction confirmed

## VWAP

- Price > VWAP = bullish bias | Price < VWAP = bearish bias
- VWAP bounce = dynamic support/resistance

## Supplementary (use when they add edge)

- **Bollinger Bands (20,2)**: Squeeze = breakout coming; touch outer band + RSI extreme = reversal
- **Fibonacci**: 38.2% (shallow pullback), 50% (moderate), 61.8% (deep — last chance for trend)
- **ATR (14)**: Stop-loss = Entry +/- 1.5*ATR; high ATR = reduce position size
- **Stochastic RSI**: >0.8 overbought, <0.2 oversold — confirms RSI extremes

## Position Sizing

```
Risk Amount = Account * 1–2%
Shares = Risk Amount / |Entry - Stop|
```

**Saxo commissions**: ~0.1% on DKK/EU (min ~29 DKK) | ~0.02 USD/share on US (min ~3 USD)
**Trading212**: Commission-free, factor in spread.

## Market Hours (CET)

| Market | Open | Close | Pre-market |
|--------|------|-------|------------|
| Copenhagen (CPH) | 09:00 | 17:00 | 08:00–09:00 |
| US (NYSE/NASDAQ) | 15:30 | 22:00 | 10:00–15:30 |

**Yahoo suffixes**: Danish `.CO` | Swedish `.ST` | German `.DE` | US as-is
**Core watchlist**: NOVO-B.CO, ZEAL.CO, VWS.CO, DANSKE.CO
**Tax (aktieskat)**: 27% up to ~61k DKK/year, 42% above (verify current rates)
