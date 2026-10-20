---
name: deaglo-risk-profile-setup
description: "Ask five short questions so later Deaglo AI answers are sized to the user's book, and read an existing hedge policy when the account can. Use when the user says 'set up my Deaglo risk profile', or after the deaglo-onboarding playbook."
---

# Deaglo risk profile setup

Capture the user's FX situation in five questions so later analysis is sized to what they told you. This is an intake conversation, not advice. This plugin does not write a profile back to the platform.

## Hard rules

- Ask one question at a time, in plain language, with a short example answer. Accept approximate answers.
- Never invent an answer. If the user skips a question, record "not provided" and move on.
- Do not recommend a hedge during setup. At the end, offer a market snapshot and a hedge comparison as next steps.
- Do not print the user's access level, role, or internal identifiers.
- Do not use em dashes.

## Steps

1. whoami. Then `my_risk_profile` for the currencies the user names. If the tool is refused, say it was skipped and continue from the answers. If a profile comes back, show it in a table and ask whether to talk through an update instead of starting over. Talking it through does not save it.
2. Ask, one at a time:
   1. Firm type: LP, GP or fund manager, private credit fund, portfolio company, corporate, family office, or other.
   2. Base (reporting) currency.
   3. Currencies you are exposed to, and approximate amounts per year or per deal.
   4. Where the exposure comes from and when it settles: fund commitments or capital calls, distributions, loan interest and principal, revenues, costs, acquisitions, or exits. Ask for the main dates or frequency.
   5. Current hedging and policy: what is hedged today (forwards, NDFs, options, none), any target hedge ratio, products the board or IC allows, and whether premium is acceptable.
3. Summarise the answers as a one-page profile table: firm type, base currency, exposure by currency (amount, direction, timing), source, current hedge ratio, policy target, permitted products, premium allowed (Yes / No / Not provided). Label it as this conversation's notes, not a saved platform record.
4. For each exposed pair, call `market_spot` and `market_forward_strip` without `on`. Add one line per pair: live spot, 12-month forward, and annualised carry (Paid or Received). If the forward strip is refused, give the spot line only and say the strip was skipped. Do not walk `on` to an earlier day.
5. Offer two next steps: a market snapshot, and a comparison of unhedged against the forward strip on the stated exposure. A TARF is included only if the tarf-pricing-guard skill passes.

## Output

- The profile table, the market line per pair, and the next steps.
- Footer: Methods (tools with as_of times, source Deaglo) and "Indicative only, not a quote. Analysis, not investment advice or a recommendation. FX and derivatives trading involves substantial risk of loss."
