---
name: fx-market-snapshot
description: "FX market snapshot for one or more currency pairs from Deaglo AI: spot, forward curve, carry, realised volatility, and a spot history chart. Use for a rate check or market context before a hedging discussion."
---

# FX market snapshot

## Steps

1. For each pair: `market_spot` (mid unless the user asks for bid or ask), `market_forward_strip` for 12 months, `analytics_carry` on the 12M forward versus that same spot, `market_volatility`, and `market_spot_history` for 6 months daily.
2. Call `market_forward_strip` without `on`. Print its spot and each tenor's annualised carry (Paid or Received). Pass `on` only to show a stored historical curve. Do not walk `on` to an earlier day. If the live call fails, say so and do not label an older curve as current.
3. If `market_volatility` or `market_spot_history` is refused, say that it was skipped and give the table without it.
4. Write every data source as "Deaglo". Never print the raw source or provider field.
5. Output: a table (spot, 1M/3M/6M/12M forward, forward points in pips, 12M carry in percent and basis points, realised vol when returned) and a line chart of the 6-month history when that series was returned. Build the chart with Python (matplotlib PNG, green #006A44 line, live spot labelled, source line "Deaglo · Indicative only"). Never draw charts as text. Never simulate a path.
6. Two or three factual observations: where spot sits in the returned history, what carry means for a buyer versus a seller of the foreign currency, and the volatility level when it was returned. Describe. Do not forecast, give a directional view, or suggest entry or exit levels. Past moves are not necessarily indicative of future moves.
7. Footer: Methods (tools and as_of times; source written as Deaglo) and "Indicative only, not a quote. Analysis, not investment advice or a recommendation. FX and derivatives trading involves substantial risk of loss."
8. Do not use em dashes.
