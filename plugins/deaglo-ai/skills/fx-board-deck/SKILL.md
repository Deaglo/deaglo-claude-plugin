---
name: fx-board-deck
description: "Build a short Deaglo FX board deck (PowerPoint) from the same numbers as the FX risk report: market, book, and unhedged versus forward and TARF. Use when the user asks for a board deck, slides, or a presentation."
---

# FX board deck

Follow the fx-risk-report skill for data, access, the live strip, the TARF guard, and what may be priced. This skill only changes the format.

## Slides

1. Title: entity, currency, as-of, and "For institutional and eligible contract participant use only. Not an offer to buy or sell any financial instrument."
2. Market: spot, the forward strip, carry, and realised volatility when returned. One chart from the returned history or the returned strip.
3. Book: open flows and the hedge policy, or the schedule the user gave.
4. Alternatives: unhedged, forward strip, and a TARF only if the tarf-pricing-guard skill passed. Numbers only from `excel_hedge_compare`, `excel_hedge_from_book`, or a single `excel_option_price` ticket. No collar, seagull, or participating forward.
5. Methods: tools, as_of, source written as Deaglo, and "Indicative only, not a quote. Analysis, not investment advice or a recommendation. Hedging has costs, and a hedged position can perform worse than an unhedged one. FX and derivatives trading involves substantial risk of loss."

Build the file with Python from tool results. Charts are matplotlib images of returned numbers and returned percentiles only. Never simulate a path. Do not use em dashes.
