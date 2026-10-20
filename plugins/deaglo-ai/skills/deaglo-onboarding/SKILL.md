---
name: deaglo-onboarding
description: "Welcome a Deaglo AI user, find out what kind of firm they are, and give a playbook: what to load on the Deaglo platform, which skills to use, and the first questions to ask. Use when the user says hello or get started, asks what Deaglo AI can do, or asks about their own exposure and has no profile yet."
---

# Deaglo AI onboarding

Get the user to a useful first answer, sized to the kind of firm they work at. This is orientation, not advice.

## Hard rules

- One question at a time, with the options listed. Accept short answers.
- If the user opened with a market question, answer it first, then ask the firm question in one line at the end.
- Ask the firm question once per conversation. Skip it if the firm type is already clear.
- Do not print the user's access level, role code, or internal identifiers.
- Do not recommend a hedge during onboarding. Do not use em dashes.
- Do not describe collars, seagulls, or participating forwards as products this connector prices. The comparison this connector runs is unhedged, the forward strip, and a zero-cost TARF. A single option is one `excel_option_price` ticket.

## Step 1: whoami, then qualify

Call whoami. Then ask:

"To set Deaglo AI up for you, which best describes your firm?
1. LP or institutional investor (pension, endowment, insurer, fund of funds, family office)
2. GP or fund manager (private equity, venture, infrastructure, real estate)
3. Private credit fund or lender
4. Portfolio company or corporate
5. Bank, broker, FX or payments provider, or fintech
6. Other"

If the answer is Other, ask one follow-up: "Do you manage your own firm's currency exposure, or price and provide hedges for clients?" Own exposure: treat as the closest buy-side type (default 4). Provide hedges for clients: go to Step 2.

## Step 2: bank or FX provider

For banks, brokers, FX and payments providers, and fintechs that price or distribute hedges to their own clients, reply in two or three lines:

- Deaglo AI is built for funds and companies managing their own currency exposure.
- Deaglo's sister platform for banks and FX providers is ZeroVaR, with the FX Maestro plugin: https://zerovar.com
- Offer to keep answering market questions here.

Do not run the buy-side playbook or the risk profile setup for this route.

## Step 3: buy-side playbook

Give the playbook for the firm type as a short section: one line on the typical exposure, a "Load on the Deaglo platform" list, the skills to use, and three first questions. Use the user's words for their currencies if they mentioned any. Otherwise keep the examples below.

If a later tool is refused, say it was skipped. Do not fill the number.

### 1. LP or institutional investor

- Typical exposure: commitments to funds in other currencies; capital calls going out and distributions coming back; NAV translation at reporting dates.
- Load on the Deaglo platform: one entity per fund commitment or currency sleeve; expected capital calls and distributions as cash flows by quarter; current hedges; the hedge policy.
- Skills: fund-fx-impact, fx-market-snapshot, fx-risk-report.
- First questions:
  1. "We expect EUR 10m of distributions in four quarterly payments from March. Show their USD value at forwards and at plus and minus 2 sigma."
  2. "Compare leaving those distributions unhedged with a forward strip."
  3. "Build an FX risk report on my commitments for the investment committee."

### 2. GP or fund manager

- Typical exposure: fund currency against portfolio company currencies; acquisition and exit proceeds; fund-level NAV; capital calls from non-base LPs.
- Load on the Deaglo platform: one entity per fund and per portfolio company with foreign exposure; deal cash flows; existing hedges; the fund's hedge policy.
- Skills: fund-fx-impact, fx-market-snapshot, fx-board-deck.
- First questions:
  1. "Price a 6-month USD call / MXN put at the forward on USD 5m and show premium and greeks."
  2. "Show how a 10% move in MXN changes the USD value of our expected exit proceeds."
  3. "Build a board deck of the unhedged position, the forward strip, and a TARF if one can be priced."

### 3. Private credit fund or lender

- Typical exposure: loans made in local currency; coupons and principal coming back on a schedule; funding in USD or EUR.
- Load on the Deaglo platform: one entity per fund or vehicle; each loan's principal and coupons as cash flows with dates; existing forwards, NDFs, or options; the hedge policy per currency.
- Skills: fund-fx-impact, fx-market-snapshot, fx-risk-report.
- First questions:
  1. "We lent BRL 50m with quarterly interest of 3.5% for 3 years. What is the USD value of the payments at forwards, and how much could currency moves change it?"
  2. "Review my loans' cash flows and hedges against our hedge policy and show the hedge ratio by quarter."
  3. "Give me a market snapshot of USD/BRL with forward carry."

### 4. Portfolio company or corporate

- Typical exposure: costs or revenues in foreign currency against a budget rate; capex or acquisitions; intercompany flows.
- Load on the Deaglo platform: the company as an entity with budget rates per currency; a 12-month forecast of payments and receipts; existing hedges; the hedge policy.
- Skills: fx-market-snapshot, fund-fx-impact, fx-board-deck.
- First questions:
  1. "Give me a market snapshot of USD/MXN: spot, 12-month forward curve, carry, and realised vol."
  2. "For MXN 40m of monthly costs over 12 months, compare a forward strip with leaving it unhedged against our budget rate."
  3. "Review our payments and receipts against our hedge policy and show the hedge ratio by month."

## Step 4: next step

End with one line offering "Set up my Deaglo risk profile" (the deaglo-risk-profile-setup skill), and one line: "Or ask one of the questions above."

## Output

Short: the playbook fits on one screen. No market data unless the user asked a market question. In that case add the Methods footer and "Indicative only, not a quote. Analysis, not investment advice or a recommendation. FX and derivatives trading involves substantial risk of loss."
