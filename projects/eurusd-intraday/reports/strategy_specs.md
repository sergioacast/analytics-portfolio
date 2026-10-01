# Strategy specs (locked before coding: 2026-09-30)

All times UTC. Pair: EURUSD. Bars are stamped with their start time; a bar's close is known only at bar start + bar length.
Signals use completed bars only and fill at the NEXT bar's open.

## Common rules (both strategies)
- Costs: 1.0 pip spread + 0.2 pip slippage per side, so 1.4 pips per round trip, charged on every trade.
- Position size: risk 1% of equity per trade (size = 1% of equity / stop distance).
- One position at a time per strategy. No trades from Friday 20:00 UTC (weekend gap risk).
- Same-bar rule: if a bar touches both the stop and the target, assume the STOP hit first (worst case).
- Excluded data: Mar-Jul 2023 (bad_period flag).
- Data split: 2015-2021 build and tune, 2022-2023 validation, 2024-2025 final test (used once).

## Strategy A: London breakout (trend)
Idea: the Asia session builds a quiet range; London often breaks it with momentum.
- Timeframe: 15m bars.
- Range: high and low of the 00:00-07:00 UTC bars (Asia session).
- Skip the day if the range is wider than 1.5 x the 1h ATR(14) at 07:00 (the move already happened) or narrower than 10 pips (noise).
- Entry: first 15m bar between 07:00 and 11:00 UTC that CLOSES above the Asia high, so go long at the next bar's open; closes below the Asia low, so go short.
- Max one trade per day.
- Stop-loss: 1.0 x ATR(14, 1h) from entry.
- Take-profit: 2.0 x ATR(14, 1h) from entry.
- Time exit: close at 16:00 UTC if neither is hit.
- Correction (before tuning, 2026-09-30): range filter changed from 1.5x 1h ATR to the 80th percentile of the trailing 60 days' Asia ranges (prior days only); the original rejected 93% of days (1,672 of ~1,800) because of a scale mismatch.

## Strategy B: Bollinger mean reversion (quiet session)
Idea: in quiet hours, stretched moves tend to snap back to the average.
- Timeframe: 1h bars.
- Bands: 20-bar moving average of close +/- 2 standard deviations.
- Only trade during Asia hours (00:00-07:00 UTC entries).
- Entry: bar closes below the lower band, so go long at the next bar's open; closes above the upper band, so go short.
- Skip if ATR(14) is in the top 20% of its trailing 60-day values (a trending, volatile market, where fading is dangerous).
- Stop-loss: 1.5 x ATR(14) from entry.
- Take-profit: the 20-bar moving average at the time of entry (fixed price).
- Time exit: close after 8 bars if neither is hit.

## Parameters allowed to be tuned later (on 2015-2021 only)
- A: stop multiple {0.75, 1.0, 1.5}, target multiple {1.5, 2.0, 3.0}
- B: band width {1.5, 2.0, 2.5}, stop multiple {1.0, 1.5, 2.0}
Everything else stays fixed.
