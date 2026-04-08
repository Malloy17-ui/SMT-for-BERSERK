# SMT-for-BERSERK
MALLOY LABS
SMT Divergence Indicator
Complete User Guide  ·  TradingView Pine Script v5


Overview
The Malloy Labs SMT Divergence Indicator is built around a session-based time and price model. It detects Smart Money Technique (SMT) divergence between two correlated instruments, filters setups by daily open bias, and provides FVG-based confirmation zones — all within a structured four-session framework.

The indicator is designed for intraday trading, optimized for the 15-minute timeframe, and supports all major forex pairs, BTC/ETH, and equity index futures (NQ/ES).

Session Structure
The trading day is divided into four 6-hour sessions, all times in New York (Eastern) time. The indicator automatically detects and color-codes each session on the chart.

Session
NY Time
UTC
Description
Q1
6:00 PM – 12:00 AM
23:00 – 05:00
Late NY / Early Asia overlap
Q2
12:00 AM – 6:00 AM
05:00 – 11:00
Asian session / London open buildup
Q3
6:00 AM – 12:00 PM
11:00 – 17:00
London session / NY pre-market
Q4
12:00 PM – 6:00 PM
17:00 – 23:00
NY session / London close


Session pairs are the core unit of SMT detection. The indicator only looks for divergence between adjacent sessions:
Q1 → Q2 divergence
Q2 → Q3 divergence
Q3 → Q4 divergence

⚠  SMT signals will not fire within Q1 itself, since there is no prior session in the model to compare against. Q1 establishes the baseline for Q2 detection.

Daily Open & Directional Bias
The daily open is defined as the opening price of the first candle in Q1 (6:00 PM New York time). This resets every day and serves as the directional filter for all SMT signals.

Bullish Bias
Condition: current price is below the daily open.
SMT must form below the daily open
Target: current day's high

Bearish Bias
Condition: current price is above the daily open.
SMT must form above the daily open
Target: current day's low

💡  If the SMT sweep candle is on the wrong side of the daily open, the signal will not fire. This is by design — bias alignment is required for a valid setup.

SMT Divergence Logic
SMT (Smart Money Technique) divergence occurs when two correlated instruments disagree on a key level. One instrument sweeps a session extreme (taking out liquidity), while the other holds — signaling a potential reversal.

Bullish SMT Setup
Primary instrument makes a lower low vs. prior session low
Correlated instrument does NOT make a new session low
The sweep candle closes below the daily open (bullish bias confirmed)
Signal fires once per session — no repeat alerts on subsequent bars

Bearish SMT Setup
Primary instrument makes a higher high vs. prior session high
Correlated instrument does NOT make a new session high
The sweep candle closes above the daily open (bearish bias confirmed)
Signal fires once per session — no repeat alerts on subsequent bars

⚠  The correlated instrument check is a running session comparison, not just bar-by-bar. If the correlated instrument has already swept its level at any point during the current session, the divergence condition fails.

Correlated Symbol Pairs
The indicator compares the chart you are viewing against a single correlated symbol that you configure in the settings. This design is intentional — SMT is always a two-instrument divergence, and you are always watching one pair at a time.

The indicator works from either side of the pair. You can run it on EURUSD with GBPUSD as the correlated symbol, or flip the chart to GBPUSD and set the correlated symbol to EURUSD. The logic is identical in both directions.

Full Pair Reference Table
Use this table to find the correct Correlated Symbol input for any instrument you trade:

Chart Symbol
Correlated Symbol
Market
TradingView Input
EURUSD
GBPUSD
Forex
GBPUSD
GBPUSD
EURUSD
Forex
EURUSD
USDJPY
USDCHF
Forex
USDCHF
USDCHF
USDJPY
Forex
USDJPY
AUDUSD
NZDUSD
Forex
NZDUSD
NZDUSD
AUDUSD
Forex
AUDUSD
GBPJPY
EURJPY
Forex
EURJPY
EURJPY
GBPJPY
Forex
GBPJPY
BTCUSD
ETHUSD
Crypto
ETHUSD
ETHUSD
BTCUSD
Crypto
BTCUSD
NQ1!
ES1!
Futures
ES1!
ES1!
NQ1!
Futures
NQ1!


Important Notes on BTC and NQ
Crypto and futures pairs require specific symbol formatting in TradingView:

Bitcoin / Ethereum
 → set Correlated Symbol to ETHUSD
 → set Correlated Symbol to BTCUSD
TradingView symbol format: BTCUSD or BTCUSDT depending on your exchange feed. Use the same exchange for both instruments (e.g., both from Coinbase or both from Binance) to avoid data mismatches.

NQ / ES Futures
 → set Correlated Symbol to ES1!
 → set Correlated Symbol to NQ1!
Use continuous contract symbols (NQ1!, ES1!) for consistent session detection. Specific contract month symbols (e.g., NQH2026) will work but may require updating at rollover.

⚠  NQ and ES trade during specific market hours, not 24 hours. Sessions outside of RTH (Regular Trading Hours) and ETH (Extended Trading Hours) will have no price data on these instruments. The indicator will still function but some sessions may be empty on futures charts.

Saving Pair Configurations in TradingView
Since you frequently switch between forex pairs, BTC, and NQ, the most efficient workflow is to save separate chart layouts in TradingView — one per instrument. Each layout remembers the correlated symbol setting so you never have to retype it.

Open the chart for your instrument (e.g., EURUSD, 15M)
Apply the indicator and set the Correlated Symbol correctly from the table above
Click the Layout icon in the top toolbar → Save As → name it (e.g., "ML SMT — EURUSD")
Repeat for each pair you trade
Switch between layouts using the Layout dropdown — the correct correlated symbol loads automatically

FVG Confirmation
After an SMT signal fires, the indicator draws Fair Value Gaps (FVGs) as potential entry zones. An FVG is a three-candle imbalance where the first and third candles do not overlap, leaving a price gap in between.

Bullish FVG
Formed when the high of two bars ago is below the low of the current bar
Gap region is drawn as a green box extending to the right
Price is expected to retrace back into this zone for entry
Stop loss: below the bottom of the FVG (or below the SMT sweep low)

Bearish FVG
Formed when the low of two bars ago is above the high of the current bar
Gap region is drawn as a red box extending to the right
Price is expected to retrace back into this zone for entry
Stop loss: above the top of the FVG (or above the SMT sweep high)

FVG Size Filter
The size filter solves the problem of oversized FVGs creating stop losses that are too wide to justify the trade. When enabled, any FVG larger than the configured ATR multiplier is hidden.
Default setting: 1.5× ATR14
Reduce the multiplier (e.g., 1.0×) for tighter entries on lower-volatility pairs like USDCHF
Increase the multiplier (e.g., 2.0×) for higher-volatility instruments like BTC or NQ

Mitigation
When price retraces into an FVG (the candle's wick enters the gap zone), the box dims and stops extending. This visually distinguishes fresh FVGs from ones that have already been tapped.

⚠  A mitigated FVG is not necessarily invalid. Price can tap into an FVG, bounce, and use it again as support/resistance. Use your judgment on whether the level remains significant.

Targets
When an SMT signal fires, a target line is automatically drawn on the chart:
 → dotted green line at the current day's HIGH
 → dotted red line at the current day's LOW

The target is the day's high or low as of the moment the signal fires. As the session progresses and the day's high/low extends, the target line does not update — it represents the level at signal time. This is the intended behavior: you are targeting the liquidity that existed when the setup formed.

Settings Reference

Setting
Default
Description
Correlated Symbol
GBPUSD
The paired instrument for SMT divergence detection. See the Pair Reference Table above.
Show FVGs
On
Toggles FVG box display on the chart.
FVG Size Filter
On
Hides FVGs larger than the ATR multiplier threshold.
Max FVG (× ATR14)
1.5
Maximum allowed FVG size as a multiple of the 14-period ATR. Increase for volatile instruments (BTC, NQ), decrease for tight pairs (USDCHF).
Session Boxes
On
Draws color-coded session boxes (Q1 purple, Q2 blue, Q3 teal, Q4 orange).
Daily Open Line
On
Draws a dashed line at the Q1 opening price, labeled with the exact level.
Target Lines
On
Draws a dotted line to the day high (bull) or day low (bear) when an SMT signal fires.
Info Panel
On
Displays a panel in the top-right corner showing current session, directional bias, correlated symbol, and daily open price.


Alerts
Two alert conditions are built into the indicator. To configure alerts in TradingView:
Right-click the indicator on the chart → Add Alert
Select either "ML·SMT — Bullish Signal" or "ML·SMT — Bearish Signal"
Set notification method (push, email, webhook, etc.)
Set frequency to "Once Per Bar" to match the one-signal-per-session behavior

⚠  Create a separate alert for each chart/pair combination you want notifications on. Alerts are per-chart, not global.

Quick Reference — Trade Checklist

Bullish Setup
Price is below the daily open
In a valid session pair (Q2, Q3, or Q4)
Primary instrument sweeps prior session low
Correlated instrument holds above its prior session low
"▲  SMT · Q#" label fires on the chart
Wait for price to retrace into a green FVG for entry
Stop: below FVG bottom or SMT sweep low
Target: dotted green line at day high

Bearish Setup
Price is above the daily open
In a valid session pair (Q2, Q3, or Q4)
Primary instrument sweeps prior session high
Correlated instrument holds below its prior session high
"▼  SMT · Q#" label fires on the chart
Wait for price to retrace into a red FVG for entry
Stop: above FVG top or SMT sweep high
Target: dotted red line at day low



MALLOY LABS  ·  malloylabs.net  ·  @MalloyAlgos
The operator is the variable.
