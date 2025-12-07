# ACCDv3 - Accumulation/Distribution MACD with Divergence Detection

## Overview

**ACCDv3** (Accumulation/Distribution MACD Version 3) is an advanced volume-weighted momentum indicator that combines the Accumulation/Distribution (A/D) line with MACD methodology and divergence detection. It helps identify trend strength, momentum shifts, and potential reversals by analyzing volume-weighted price movements.

## Key Features

- **Volume-Weighted MACD**: Applies MACD calculation to volume-weighted A/D values for earlier, more reliable signals
- **Divergence Detection**: Identifies when A/D trend diverges from MACD momentum
- **Volume Strength Filtering**: Distinguishes high-volume confirmations from low-volume noise
- **Color-Coded Histogram**: 4-color system showing momentum direction and volume strength
- **Real-Time Alerts**: Background colors and alert conditions for bullish/bearish divergences

## Components

### 1. Accumulation/Distribution (A/D) Line

The A/D line measures buying and selling pressure by comparing the close price to the trading range, weighted by volume:

```
A/D = Σ ((2 × Close - Low - High) / (High - Low)) × Volume
```

- **Rising A/D**: More accumulation (buying pressure)
- **Falling A/D**: More distribution (selling pressure)
- **Doji Handling**: When High = Low, contribution is zero (avoids division errors)

### 2. Volume-Weighted MACD

Instead of simple EMAs, the indicator weights A/D values by volume:

- **Fast Line** (default 12): `EMA(A/D × Volume, 12) / EMA(Volume, 12)`
- **Slow Line** (default 26): `EMA(A/D × Volume, 26) / EMA(Volume, 26)`
- **MACD Line**: Fast Line - Slow Line (green line)
- **Signal Line** (default 9): EMA or SMA of MACD (orange line)
- **Histogram**: MACD - Signal (color-coded columns)

This volume-weighting ensures that periods with higher volume have greater influence on the indicator values.

### 3. Histogram Color System

The histogram uses 4 distinct colors based on **direction** and **volume strength**:

| Condition | Color | Meaning |
|-----------|-------|---------|
| Rising + High Volume | **Dark Green** (#1B5E20) | Strong bullish momentum with volume confirmation |
| Rising + Low Volume | **Light Teal** (#26A69A) | Bullish momentum but weak volume (less reliable) |
| Falling + High Volume | **Dark Red** (#B71C1C) | Strong bearish momentum with volume confirmation |
| Falling + Low Volume | **Light Red/Pink** (#FFCDD2) | Bearish momentum but weak volume (less reliable) |

Additional shading:
- **Light Cyan** (#B2DFDB): Positive but not rising (momentum stalling)
- **Bright Red** (#FF5252): Negative and accelerating down

### 4. Divergence Detection

Divergence occurs when A/D trend and MACD momentum move in opposite directions:

#### Bullish Divergence (Green Background)
- **Condition**: A/D is trending up BUT MACD is negative and trending down
- **Interpretation**: Accumulation increasing while momentum appears weak
- **Signal**: Potential bullish reversal or continuation
- **Action**: Look for entry opportunities or hold long positions

#### Bearish Divergence (Red Background)
- **Condition**: A/D is trending down BUT MACD is positive and trending up
- **Interpretation**: Distribution increasing while momentum appears strong
- **Signal**: Potential bearish reversal or weakening uptrend
- **Action**: Consider exits, tighten stops, or prepare for reversal

## Parameters

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| **Fast Length** | 12 | 1-50 | Period for fast EMA (shorter = more sensitive) |
| **Slow Length** | 26 | 1-100 | Period for slow EMA (longer = smoother) |
| **Signal Smoothing** | 9 | 1-50 | Period for signal line (MACD smoothing) |
| **Signal Line MA Type** | EMA | SMA/EMA | Moving average type for signal calculation |
| **Volume MA Length** | 20 | 5-100 | Period for volume average (strength filter) |

## Usage Guide

### Reading the Indicator

1. **MACD Lines (Green & Orange)**
   - **Crossovers**: When green crosses above orange = bullish, below = bearish
   - **Distance**: Wider gap = stronger momentum
   - **Zero Line**: Above = bullish bias, below = bearish bias

2. **Histogram Colors**
   - Focus on **dark colors** (dark green/red) for high-confidence signals
   - Be cautious with **light colors** (teal/pink) - wait for volume confirmation
   - Watch for **rising red bars** (V-bottom pattern) = potential bullish reversal
   - Watch for **falling green bars** (Λ-top pattern) = potential bearish reversal

3. **Background Divergence Alerts**
   - **Green background**: Bullish divergence - consider long entries
   - **Red background**: Bearish divergence - consider exits or shorts
   - Best used in combination with price action and support/resistance levels

### Trading Strategies

#### Trend Following
1. Wait for MACD to cross above zero line with dark green histogram
2. Enter long when histogram shows consecutive dark green bars
3. Exit when histogram turns light green or red appears

#### Divergence Trading
1. Wait for background divergence alert (green or red)
2. Confirm with price action (support/resistance, candlestick patterns)
3. Enter on next dark-colored histogram bar in divergence direction
4. Set stops beyond recent swing high/low

#### Volume Confirmation
1. Ignore signals during low-volume periods (light colors)
2. Take aggressive positions during high-volume confirmations (dark colors)
3. Use volume strength as position sizing guide (larger size on dark bars)

### Best Practices

✓ **Combine with price action**: Don't rely on indicator alone  
✓ **Wait for dark colors**: High-volume bars are more reliable  
✓ **Watch for divergences**: Early warning signs of reversals  
✓ **Use multiple timeframes**: Confirm signals across 1m, 5m, 15m  
✓ **Respect zero line**: Trading direction should align with MACD side  

✗ **Don't chase light-colored signals**: Low volume = lower reliability  
✗ **Don't ignore context**: Market structure and levels matter  
✗ **Don't over-trade**: Wait for clear, high-volume setups  
✗ **Don't ignore alerts**: Divergences are early warnings  

## Technical Details

### Volume-Weighted Calculation Method

Traditional MACD uses simple price EMAs. ACCDv3 weights each A/D value by its corresponding volume:

```pine
// Volume-weighted fast EMA
close_vol_fast = ta.ema(ad × volume, fast_length)
vol_fast = ta.ema(volume, fast_length)
vw_ad_fast = close_vol_fast / vol_fast

// Same for slow EMA
close_vol_slow = ta.ema(ad × volume, slow_length)
vol_slow = ta.ema(volume, slow_length)
vw_ad_slow = close_vol_slow / vol_slow

// MACD is the difference
macd = vw_ad_fast - vw_ad_slow
```

This ensures high-volume periods have proportionally more impact on the indicator.

### Volume Strength Filter

Determines whether current volume is above or below average:

```pine
vol_avg = ta.sma(volume, vol_length)
vol_strength = volume > vol_avg
```

Used to select dark (high volume) vs light (low volume) histogram colors.

### Divergence Logic

```pine
// A/D trending up if above its 5-period SMA
ad_trend = ad > ta.sma(ad, 5)

// MACD trending up if above zero
macd_trend = macd > 0

// Divergence when trends oppose
divergence = ad_trend != macd_trend

// Specific conditions
bullish_divergence = ad_trend and not macd_trend and macd < 0
bearish_divergence = not ad_trend and macd_trend and macd > 0
```

## Alerts

The indicator includes built-in alert conditions:

- **Bullish Divergence**: "Bullish Divergence: A/D trending up but MACD trending down"
- **Bearish Divergence**: "Bearish Divergence: A/D trending down but MACD trending up"

To enable:
1. Click "Create Alert" button in TradingView
2. Select "ACCDv3" as condition
3. Choose "Bullish Divergence" or "Bearish Divergence"
4. Configure notification method (popup, email, webhook, etc.)

## Comparison with Standard MACD

| Feature | Standard MACD | ACCDv3 |
|---------|---------------|---------|
| **Input** | Close price | Accumulation/Distribution line |
| **Weighting** | Simple EMA | Volume-weighted EMA |
| **Divergence** | Price vs MACD | A/D vs MACD |
| **Volume Analysis** | None | Built-in strength filter |
| **Color System** | 2 colors (up/down) | 4+ colors (direction + volume) |
| **Leading/Lagging** | Lagging | More leading (volume-weighted) |

## Example Scenarios

### Scenario 1: Strong Bullish Signal
- **Chart**: MACD crosses above zero line
- **Histogram**: Dark green bars (#1B5E20) appearing
- **Volume**: Above 20-period average
- **Action**: Enter long, strong momentum with volume confirmation

### Scenario 2: Weak Bearish Signal
- **Chart**: MACD crosses below zero line
- **Histogram**: Light pink bars (#FFCDD2) appearing
- **Volume**: Below 20-period average
- **Action**: Avoid shorting, low volume = unreliable signal

### Scenario 3: Bullish Divergence Reversal
- **Chart**: Price making lower lows
- **Indicator**: A/D line trending up, MACD negative
- **Background**: Green shading appears
- **Histogram**: Transitions from red to dark green
- **Action**: Look for long entry on next dark green bar

### Scenario 4: V-Bottom Reversal
- **Chart**: Downtrend in place
- **Histogram**: Red bars start rising (becoming less negative)
- **Pattern**: Forms "V" shape at bottom
- **Confirmation**: Transitions to dark green bars
- **Action**: Bullish reversal signal, consider long entry

## Timeframe Recommendations

- **1-minute**: Scalping, very fast signals (noisy, use with caution)
- **5-minute**: Intraday momentum trading (recommended)
- **15-minute**: Swing entries, clearer trend signals
- **1-hour+**: Position trading, major trend identification

## Limitations

- **Requires volume data**: Will not work on instruments without volume
- **Lag during consolidation**: MACD is inherently trend-following
- **False signals in chop**: Sideways markets generate noise
- **Not a standalone system**: Should be combined with price action and risk management

## Version History

- **v3**: Removed traditional price MACD, using only volume-weighted A/D MACD with A/D divergence
- **v2**: Added A/D divergence detection, volume strength filtering, enhanced histogram colors
- **v1**: Basic MACD on A/D line with volume-weighted calculation

## Support & Further Reading

For questions, updates, or to report issues, refer to the main project documentation or contact the developer.

**Related Indicators in Suite:**
- **VMACDv3**: Volume-weighted MACD on price (not A/D)
- **RSIv2**: RSI with A/D divergence
- **DMI**: Directional Movement Index with A/D divergence
- **Elder Impulse**: Bar coloring system using volume-weighted MACD

---

*This indicator is for educational purposes. Always practice proper risk management and never risk more than you can afford to lose.*
