# MACD Divergence Optimizer - User Guide

## Overview

The **MACD Divergence Optimizer** is a professional-grade technical analysis indicator for TradingView that automatically detects hidden divergences on MACD with volume weighting. It identifies potential reversal points before price action confirms the move, giving traders an early edge.

---

## What is Divergence?

A **divergence** occurs when price and an oscillator (like MACD) move in opposite directions:

- **Bullish Divergence**: Price makes a lower low, but MACD makes a higher low → Potential uptrend reversal
- **Bearish Divergence**: Price makes a higher high, but MACD makes a lower high → Potential downtrend reversal

Divergences are among the most reliable reversal signals in technical analysis.

---

## Indicator Features

### Volume-Weighted MACD
- Standard MACD is calculated on closing price
- This indicator uses **volume-weighted closing prices** for greater accuracy
- Formula: MACD = (Volume-Weighted EMA₁₂ - Volume-Weighted EMA₂₆)
- Volume weighting gives more importance to high-conviction price moves

### Automatic Swing Detection
- Detects local highs and lows (5-bar lookback)
- Tracks the last 5 swings for divergence analysis
- Only meaningful swings are tracked (filtered for noise)

### Smart Signal Generation
- Green triangle (▲) = Bullish Divergence (BUY signal)
- Red triangle (▼) = Bearish Divergence (SELL signal)
- Triangles appear directly on the MACD line for precise entry timing

### Built-in Alerts
- Real-time notifications for divergence signals
- Alerts can trigger mobile push notifications or sound
- Never miss a trading opportunity

---

## How to Use

### Installation
1. Open TradingView and navigate to the Chart
2. Click "Indicator" → Search "MACD Divergence Optimizer"
3. Click "Add to Chart"
4. The indicator appears in a separate panel below the price chart

### Reading the Indicator

**MACD Panel displays:**
- **Blue Line** = MACD (fast momentum)
- **Orange Line** = Signal line (slow momentum)
- **Histogram** (colored bars) = Difference between MACD and Signal
  - Green bars = MACD above signal (bullish)
  - Red bars = MACD below signal (bearish)

**Divergence Signals:**
- **Green Triangle ▲** = Bullish divergence detected
  - Price is lower, but MACD momentum is strengthening
  - Look for uptrend reversal
  - Confirm with higher closes or volume
  
- **Red Triangle ▼** = Bearish divergence detected
  - Price is higher, but MACD momentum is weakening
  - Look for downtrend reversal
  - Confirm with lower closes or selling volume

---

## Parameters & Settings

### MACD Fast Length (Default: 12)
- Controls the faster moving average period
- **Lower values** → More responsive, more false signals
- **Higher values** → Smoother, fewer signals
- **Typical range**: 8-15

### MACD Slow Length (Default: 26)
- Controls the slower moving average period
- **Lower values** → Faster divergence detection
- **Higher values** → More reliable, fewer signals
- **Typical range**: 20-35

### Signal Smoothing (Default: 9)
- EMA period applied to MACD itself
- **Lower values** → Faster crossover signals
- **Higher values** → Fewer false crossovers
- **Typical range**: 5-15

### Min Divergence Strength (Default: 0.5%)
- Minimum % difference between current MACD and swing MACD
- **Lower values** → More divergence signals (noisier)
- **Higher values** → Only strong divergences (fewer signals)
- **Recommended**: 0.3% - 1.0%

### Lookback Bars (Default: 75)
- Historical window for analysis
- Larger lookback = more context but slower calculation
- **Typical range**: 50-100

---

## Trading Strategy

### Bullish Divergence (Entry Setup)
1. **Identify Signal**: Green triangle appears on MACD
2. **Confirm Price**: Look for price rejection of the low (bounce)
3. **Volume Check**: Buy on increase in volume at the bounce
4. **Entry**: Above the swing low level
5. **Stop Loss**: Below the most recent swing low
6. **Target**: Next swing high or resistance level

### Bearish Divergence (Entry Setup)
1. **Identify Signal**: Red triangle appears on MACD
2. **Confirm Price**: Look for price rejection of the high
3. **Volume Check**: Sell on increase in volume at rejection
4. **Entry**: Below the swing high level
5. **Stop Loss**: Above the most recent swing high
6. **Target**: Next swing low or support level

### Risk Management
- **Position Size**: Risk only 1-2% per trade
- **Stop Loss**: Place beyond recent swings
- **Take Profit**: Scale out at 1:1, 1:2, 1:3 risk-reward ratios
- **Filter**: Use on higher timeframes (4H, Daily) for reliability

---

## Timeframe Recommendations

| Timeframe | Best For | Signal Quality |
|-----------|----------|---|
| **1H** | Scalping, day trading | Moderate (some noise) |
| **4H** | Swing trading | Excellent |
| **Daily** | Position trading | Excellent |
| **Weekly** | Long-term trends | Excellent |

---

## Tips & Best Practices

### ✅ DO:
- **Use on trends**: Divergences work best when there's a clear trend
- **Combine signals**: Look for confirmation from price action, volume, or moving averages
- **Trade the bounce**: Wait for price to react to the swing, then enter
- **Adjust parameters**: Test different MACD lengths for your trading style
- **Use alerts**: Set up mobile alerts so you don't miss signals

### ❌ DON'T:
- **Trade every signal**: Some signals are stronger than others
- **Trade flat/choppy markets**: Divergences fail in ranging markets
- **Ignore support/resistance**: Trade divergences near key levels for best results
- **Over-leverage**: Divergences are probabilistic, not guaranteed
- **Disable volume analysis**: Always check volume when divergence fires

---

## Advanced Features

### Volume Weighting
The indicator uses **volume-weighted MACD** instead of standard MACD. This means:
- High-volume reversals get more emphasis
- Low-volume moves are smoothed out
- More accurate momentum readings
- Better at identifying true trend changes

### Array Tracking
The indicator tracks the last 5 swings in arrays:
- `swingLows[]` = last 5 price lows
- `swingHighs[]` = last 5 price highs
- `swingMacds[]` = corresponding MACD values

This allows detection of **hidden divergences** not visible in traditional analysis.

---

## Common Questions

**Q: Why didn't the indicator trigger a signal when I see a divergence?**
A: The indicator may require:
- MACD histogram to cross the zero line (confirms momentum shift)
- Minimum strength threshold to be met (adjust Min Divergence Strength)
- At least 5 swings to be recorded in the lookback window

**Q: Can I use this on all timeframes?**
A: Yes, but divergences are more reliable on higher timeframes (4H+). Lower timeframes produce more signals but with more noise.

**Q: Should I trade every green/red triangle?**
A: No. Use them as a heads-up for potential reversals. Always confirm with:
- Price action (rejection of the swing)
- Volume (increasing volume at reversal)
- Key support/resistance levels

**Q: How do I set alerts?**
A: 
1. Right-click the indicator → Edit Alerts
2. Check "Bullish Divergence" and/or "Bearish Divergence"
3. Choose notification type (browser, mobile, email)
4. Set frequency to "Once per bar close"

**Q: What's the difference between regular and hidden divergence?**
A: This indicator detects **hidden divergences** (also called continuation divergences):
- **Regular**: Price makes new extreme, but oscillator doesn't
- **Hidden**: Price makes new extreme, oscillator makes new extreme in different direction
- Hidden divergences are often more reliable for continuation plays

---

## Disclaimer

This indicator is provided for educational and informational purposes only. It is not financial advice. Past performance does not guarantee future results. Always use proper risk management and combine with other analysis methods. Trading and investing carry risk of loss. Do your own research before making trading decisions.

---

## Support & Updates

For issues, feature requests, or questions:
- Check the indicator settings and parameter values
- Test on historical data first before live trading
- Adjust parameters to match your trading style and timeframe

---

**Version**: 1.0  
**Last Updated**: November 2025  
**Compatible**: TradingView v6+
