---
name: tarf-pricing-guard
description: "Mandatory checks before showing any TARF strike, premium or TARF line in a hedge comparison. Prices every TARF in the orientation where the client buys the base currency, inverts the pair when the client sells it, validates the result against the live forward strip with a control price, and withholds the TARF (never misquotes) if a check fails. Use whenever excel_tarf_program, excel_tarf_strike, excel_tarf, excel_hedge_compare or excel_hedge_from_book involves a TARF."
---

# TARF pricing guard

A TARF quote is shown only if it passes every check below. If any check fails, do not show the TARF number. Show the forward strip and the unhedged case instead and say: "TARF pricing for this currency direction is not available right now." Never present a failed TARF result, never adjust a number by hand, and never guess a strike. Do not invent a seller strike, and do not build a collar, seagull, or participating forward from vanilla legs.

## Why this exists

The TARF engine prices one structure: the client **buys** the base currency. `client_side` SELLS_BASE (is_bought false) only mirrors that same structure, so it returns the buyer's strike for a seller. On some USD-base pairs (USD/MXN, USD/BRL) the TARF engine also uses a forward that does not match the live strip. Forwards and vanilla options are not affected.

## Step 1: orientation

Work out which currency the client **buys** at each fixing.

- Client buys the pair's base currency: price as quoted, `client_side` BUYS_BASE.
- Client sells the pair's base currency (for example a USD/MXN client paying MXN costs, who sells USD and buys MXN): **invert the pair** and price with BUYS_BASE:
  - base = the currency the client buys, foreign = the currency it sells
  - monthly_notional (or each fixing_amount) = original base notional x that month's forward (or the strip average F)
  - target_limit = original target (foreign cash) / F, now in the new foreign currency
  - budget_rate = 1 / original budget
  - strike in the client's quote = 1 / returned strike, and every rate you show is converted back to the pair the user asked about
- Never send SELLS_BASE, and never send is_bought false, to excel_tarf_program, excel_tarf_strike, excel_tarf, excel_hedge_compare or excel_hedge_from_book.

## Step 2: forward reference

Call market_forward_strip for the pricing orientation with horizons that cover the fixings (no `on`). Let F be the average mid over the fixing months. A program result's market.strip_average_mid can be used if it covers the same months.

## Step 3: control price (required)

Run excel_tarf_program in the pricing orientation with the same pair, months and notional, leverage 1, and a target that cannot bind: target_limit = 1.9 x monthly_notional x months (just under the tool's cap). Pass if |control strike - F| / F <= 0.3%. If it fails, the TARF engine is mispricing this orientation: withhold the TARF (see the top of this skill) and add one Methods line: "TARF withheld: control strike X vs forward strip average F."

## Step 4: checks on the real structure

For the user's structure (real target, leverage, schedule), priced in the buyer orientation:

1. Direction: the client's strike must be at or better than the forward. In the pricing orientation, strike <= F. In the user's quote, a client selling the quote's base gets strike >= F; a client buying it gets strike <= F.
2. Size: |strike - F| / F <= 8%, and the strike lies within the 2-year range from market_spot_range, widened by 10%.
3. Zero cost: if you also price with excel_tarf at the solved strike, |premium| must be <= 0.5% of the total notional.

All pass: show the TARF. Any fail: withhold it.

## Presenting a TARF

- State the strike in the pair the user asked about, the forward strip average, and the difference (the rate improvement versus forwards on the hedged amount, in pips or points), the target in foreign cash, the leverage and the number of fixings.
- Say it in measurable terms: the rate it sets while it runs, that it knocks out once the target is reached, and the amount left open after that.
- Always add: "A TARF includes sold optionality. If the rate moves against the client, the leveraged amount can be settled at the strike, and losses can be large. Margin calls can apply."
- Check the hedge policy: TARF must be an allowed product and short options must be allowed (can_short_options true in strategy_prior or my_risk_profile). Otherwise label it "Outside policy".
- Methods: list the orientation used (for example "priced as MXN/USD, client buys MXN; shown as USD/MXN"), the control check result and the forward strip average.

## Hedge comparisons that include a TARF

excel_hedge_compare and excel_hedge_from_book use the same TARF engine. Run the control (Step 3) first, then call excel_hedge_compare in the buyer orientation with the book's open notionals, and convert every rate back to the user's pair (budget = 1 / budget; percentiles invert and swap ends). excel_hedge_from_book cannot be inverted, so use it for a TARF only when the client buys the base currency of the book's pair. If the control fails, leave out the TARF column, show the unhedged case and the forward strip, and state that the TARF was withheld.

## Worked example (client sells USD 500,000 a month for MXN, 6 months)

- Pricing orientation: MXN/USD, BUYS_BASE, monthly_notional = 500,000 x F (about MXN 9.27m), target in USD = MXN target / F.
- Control: strike 0.053970 vs strip average 0.053955 (0.03%): pass.
- A returned strike of 0.053039 shows as 1 / 0.053039 = 18.854 USD/MXN against a USD/MXN strip average of 18.534: the client sells USD at a better rate than the forward, so the direction check passes.
