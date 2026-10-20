---
name: fx-risk-report
description: "Build a Deaglo-style FX risk report (Word and PDF) for one currency or a book: market snapshot, exposures, hedge policy, unhedged versus forward and TARF alternatives, and methods. Use when the user asks for an FX report, a risk report, or a document for an investment committee."
---

# FX risk report

## Steps

1. whoami. Read the book with `my_entities` and `my_cash_flows` (page until `has_more` is false). Read the hedge policy with `strategy_prior` when the account can. If a read is refused, say it was skipped and continue with the numbers the user supplied. Never invent notionals.
2. Call `market_spot`, `market_forward_strip` without `on`, `market_volatility`, and `market_spot_range` for each pair in the report. Print the strip's spot. Pass `on` only for a stored historical curve. Do not walk `on` to an earlier day. If the live strip fails, say so.
3. If `market_volatility` is refused, omit the volatility section and say it was skipped.
4. Price alternatives with `excel_hedge_compare` or `excel_hedge_from_book`: unhedged, the forward strip, and a zero-cost TARF only if the tarf-pricing-guard skill passes. A single vanilla option, if the user asks for one, is one `excel_option_price` ticket (premium and greeks). Do not assemble a collar, seagull, or participating forward, and do not pass one of those structures to the hedge comparison.
5. Write the source of every rate as Deaglo. Paste any returned `audit[]` as Methods.
6. Charts are matplotlib PNGs from returned numbers and returned percentiles only. Label distributions "shape drawn from returned percentiles". Never simulate a path.

## Sections

- Cover: entity, currency, as-of, and "For institutional and eligible contract participant use only. Not an offer to buy or sell any financial instrument."
- Market: spot, forward strip, carry, realised volatility when returned, and the two-year range.
- Book: open cash flows by date and currency, and the hedge policy when it was returned.
- Alternatives: unhedged, forward strip, and TARF if the guard passed. For each, hedged rate, upfront cost, worst case, and probability of meeting budget when the tool returned them.
- Methods and the line "Indicative only, not a quote. Analysis, not investment advice or a recommendation. FX and derivatives trading involves substantial risk of loss."
- Include: "Hedging has costs, and a hedged position can perform worse than an unhedged one."

Do not use em dashes.
