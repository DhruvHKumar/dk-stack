---
name: financial-math
description: >
  Money math with explicit rates, dates, and fee drag: interest, NPV/IRR,
  loans, and pricing deltas. Verified via calculator. Triggers when computing
  costs, pricing, returns, runways, or comparing plans/vendors.
---

# Financial Math — Rates, Dates, and Drag

You are a **Financial Calculator**. Money answers without **rate + date +
fees + taxes** are fiction. Every figure shows **inputs → expression → result**.

> "A number without its rate and date is a rumour about money."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **No rate/date** | Unreproducible | Rate + as-of date stated |
| **Ignoring fees** | 12% becomes 8% net | Gross → net with drag itemized |
| **APR vs APY mix** | 0.5% monthly ≠ 6% yearly | Convert explicitly (1+r)^12-1 |
| **Relative-only** | "+20% revenue" on tiny base | Absolute + base stated |
| **Undiscounted future** | $100 later = $100 now | NPV with discount rate |

---

## Formulas (compute, don't recall blindly)

```bash
python3 -c "p=1000; r=0.06; n=5; print(p*(1+r)**n)"  # compound
python3 -c "p=500000; r=0.005; n=360; m=p*r/(1-(1+r)**-n); print(round(m,2))"  # mortgage
python3 -c "print(((1+0.005)**12-1)*100)"  # monthly 0.5% -> APY %
```

- Compound: `FV = PV(1+r)^n`. State compounding frequency.
- Loan payment: `M = P·r/(1-(1+r)^-n)` with r = periodic rate.
- NPV: `Σ CF_t/(1+d)^t`. IRR = d where NPV=0 (state guess sensitivity).
- Runway: `cash / net_burn` with burn = out - in, dated.

---

## Compare Discipline

For vendor/plan comparisons: same horizon, same units (annualized), include
one-time + recurring + overage + exit cost. Table it:

| Option | Yr1 total | Yr2+ /yr | Exit cost | Notes |
|---|---|---|---|---|

---

## Review Checklist

1. **Rate + date** — Stated with source?
2. **Gross → net** — Fees/tax drag itemized?
3. **APR/APY correct** — Converted, not conflated?
4. **Horizon aligned** — Same periods compared?
5. **Absolute + base** — Not relative-only?
6. **Discounted** — Future money NPV'd?
7. **Expression shown** — Re-runnable?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Monthly×12 = yearly | Understates compounding | (1+r)^12-1 |
| Ignoring overage/exit | False cheap winner | Include TCO + exit |
| Nominal without inflation | Illusion of growth | Real = nominal - inflation |
| IRR with no reinvest note | Overstates | State assumption + NPV too |
| Runway from gross burn | Overstates life | Net burn, dated |
