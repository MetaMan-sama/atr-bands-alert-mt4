# ATR Bands Alert — MQL4 Script

A MetaTrader 4 script that constructs dynamic **ATR-based price bands** around the previous bar's close and fires alerts when price crosses above the upper or below the lower band.

---

## Overview

This script calculates upper and lower bands by adding and subtracting a multiple of the current ATR from the previous bar's closing price. When price crosses a band for the first time in that direction, an alert is fired. A state-tracking mechanism prevents duplicate alerts until price crosses to the opposite band, ensuring clean, non-repetitive signals.

---

## Features

- **Dynamic ATR bands** — recalculated each cycle using `iATR()` and `iClose()`
- **State-based deduplication** — `AboveUpperBand` / `BelowLowerBand` flags prevent repeat alerts on the same breakout
- **Three notification channels:** sound alert, email, and mobile push
- **Configurable symbol, timeframe, ATR period, and multiplier**
- **Lightweight loop** — polls once per minute (`Sleep(60000)`)
- Logs all breakout events to the MT4 **Experts** tab

---

## How It Works

1. Every minute, `iATR()` calculates the current ATR over `ATRPeriod` bars
2. Bands are constructed from the previous bar's close (`iClose(..., 1)`):
   - `UpperBand = PrevClose + ATRMultiplier × ATR`
   - `LowerBand = PrevClose − ATRMultiplier × ATR`
3. Current close (`iClose(..., 0)`) is compared to both bands:
   - Price > UpperBand and `!AboveUpperBand` → **Price Crossed Above Upper ATR Band**
   - Price < LowerBand and `!BelowLowerBand` → **Price Crossed Below Lower ATR Band**
4. State flags are toggled to suppress duplicate alerts until the opposite band is crossed

---

## Input Parameters

| Parameter        | Type            | Default     | Description                          |
|------------------|-----------------|-------------|--------------------------------------|
| `TradeSymbol`    | string          | `EURUSD`    | Symbol for analysis                  |
| `Timeframe`      | ENUM_TIMEFRAMES | `PERIOD_H1` | Timeframe for analysis               |
| `ATRPeriod`      | int             | `14`        | Lookback period for ATR calculation  |
| `ATRMultiplier`  | double          | `2.0`       | Band width multiplier applied to ATR |
| `EnableAlerts`   | bool            | `true`      | Fire an on-screen/sound alert        |
| `EnableEmail`    | bool            | `false`     | Send an email notification           |
| `EnablePush`     | bool            | `false`     | Send a mobile push notification      |

---

## Alert Message Format

```
Price Crossed Above Upper ATR Band detected on EURUSD (Timeframe: PERIOD_H1)
Price: 1.08452, Band Level: 1.08310
```

---

## Installation

1. Copy `ATR_Bands_001.mq4` to `MQL4/Scripts/` in your MT4 data folder
2. Compile in MetaEditor (F7)
3. Drag onto any chart from Navigator → Scripts
4. Configure inputs and click **OK**

---

## Requirements

- MetaTrader 4 (`#property strict` compatible build)
- MQL4 compiler (MetaEditor)

---

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
