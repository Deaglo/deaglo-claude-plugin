---
name: fund-fx-impact
description: "Measure how currency moves affect a fund, loan or investment in its reporting currency: capital calls, distributions, loan interest and principal, or an exit value, translated at live forwards and under volatility-based scenarios. Use for LPs, GPs, private credit and portfolio companies asking how FX changes a cash-flow schedule."
---

# Fund FX impact

Translate a set of foreign-currency cash flows into the reporting currency and show how much currency moves could change them. Built for LPs, GPs, private credit funds and portfolio companies. This is analysis, not a forecast or a recommendation.

## Hard rules

- Rates, forwards and volatilities come only from Deaglo AI tool calls in this task, with as_of times. Write every source as "Deaglo". Never print a raw source or provider field.
- Cash-flow amounts and dates come from the user or the book (`my_cash_flows`). Never invent them. If the user gives only a total, ask once for the timing or offer an evenly spaced illustrative schedule labelled "Illustrative example. Figures are simplified to show the mechanics. They are not a projection, a quote or the results of any Deaglo client."
- If `market_volatility`, `market_spot_history`, or `my_cash_flows` is refused, do not retry. Show the values you can price and say which tool was skipped.
- Scenario results are mechanical translations at shocked rates, not forecasts. Never call them expected returns. Do not compute or present a fund's actual or projected performance. Show only the currency component of the cash flows supplied.
- Include in body text: "Hedging has costs, and a hedged position can perform worse than an unhedged one. FX and derivatives trading involves substantial risk of loss." and "Past results are not necessarily indicative of future results." whenever history is shown.
- No directional market views and no recommendation. End with "Indicative only, not a quote. Analysis, not investment advice or a recommendation."
- Do not use em dashes.

## Steps

1. whoami. If a book exists, `my_entities` and `my_cash_flows` (page until `has_more` is false) for the organisation. Otherwise take the user's schedule: currency, direction (receive or pay foreign currency), amount and date per flow, reporting currency.
2. For each pair: `market_spot` and `market_forward_strip` (without `on`; print its spot) out to the last flow date, `market_volatility`, and `market_spot_history` over the investment period or 3 years, whichever is shorter. Pass `on` only for a stored historical curve. Do not walk `on` to an earlier day. If the live strip fails, say so.
3. Translate each flow at the forward for its date (interpolate linearly between strip tenors and say so). Totals: reporting-currency value at spot, at forwards, and the carry effect (forward value minus spot value), labelled Paid or Received.
4. When `market_volatility` returns: shock each flow from its forward by k x sigma x sqrt(t) for k = -2, -1, +1, +2 and sum. Show the reporting-currency value and the change versus the forward value, also as a percent of the total. When `market_spot_history` returns, add one historical line: the largest 12-month move in that window (start and end dates and rates), applied to the flows, labelled "historical move, not a forecast".
5. If the user supplies an IRR or multiple on a local-currency basis, show only the arithmetic link: the same cash flows in reporting currency at forwards and under the plus and minus 2 sigma scenarios, and the resulting reporting-currency IRR or multiple next to the local one. State that this isolates currency translation and ignores everything else.
6. Hedge context, only if asked: run `excel_hedge_compare` on the flows for unhedged versus the forward strip, and a TARF only if the tarf-pricing-guard skill passes. Show p05 / p50 / p95 of the reporting-currency total when those percentiles are returned. Present as alternatives with the hedging risk statement. Do not add a collar, seagull, or participating forward.

## Output

- One-line answer: total reporting-currency value at forwards, and the range at plus and minus 2 sigma when volatility was returned.
- Table per flow: date, currency, amount, forward, reporting-currency value, and the sigma columns when they were computed.
- Chart 1 (matplotlib): reporting-currency value by flow date at forwards, with the plus and minus 1 and 2 sigma bands shaded when those numbers exist (green #006A44 base line, grey #8A8F98 bands, gold #D9991E for the reporting budget if given). Draw only from returned numbers. Never simulate a path.
- Chart 2, when history was returned: spot history over the window with the largest 12-month move marked.
- Methods: tools with as_of times (source Deaglo), interpolation and scenario method, realised volatility, and the closing lines above.
