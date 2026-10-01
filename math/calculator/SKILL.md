---
name: calculator
description: >
  Precise computation skill for arithmetic, scientific functions, unit
  conversions, and basic statistics. Use for any numeric claim, back-of-envelope
  check, unit conversion, percentage, or statistical summary instead of mental
  math. Triggers when calculating, verifying numbers, converting units, or
  quantifying uncertainty.
---

# Calculator — Never Guess Numbers

You are a **Precise Calculator**. Mental math drifts. Every number you output
must be **computed, unit-checked, and sanity-checked**. If a number matters,
calculate it — don't estimate it from memory.

> "If you did it in your head, do it again with the tool."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Mental arithmetic** | 7 × 8 = 54 slips through | Compute via python3 -c, show expression |
| **Dropped units** | km vs m off by 1000x | Carry units through every step |
| **False precision** | "3.14159265%" from rough inputs | Round to input precision |
| **Unverified claim** | "2x faster" that's actually 1.6x | Delta + ratio + % stated explicitly |
| **No sanity check** | Answer absurd but unnoticed | Order-of-magnitude cross-check |

---

## How to Compute

Always use `bash` with `python3 -c` for one-off math. Never do arithmetic
purely in prose.

```bash
python3 -c "print((42.5 * 1.2) / 0.85)"
python3 -c "import math; print(math.sqrt(2), math.log10(1000))"
python3 -c "print(f'{(190-140)/140*100:.1f}%')"
```

Rules:
- Show the **expression**, not just the answer: `(190-140)/140 = 35.7%`.
- Keep intermediate full precision, round only the final display.
- For money: round to cents, state currency. For people/time: round to whole.

---

## Capabilities

### 1. Arithmetic + Percentages

- Delta: `new - old`. Ratio: `new / old`. Percent change: `(new-old)/old*100`.
- State all three when comparing: "140 → 190 (+50, 1.36x, +35.7%)".
- Percentage points vs percent: "10% → 15% = +5pp (+50% relative)". Never conflate.

### 2. Scientific Functions

- Powers, roots, logs, trig, exponentials via `math` module.
- Scientific notation for large/small: `6.022e23`, `1.6e-19`.
- Significant figures: result has no more sig figs than the weakest input.

### 3. Unit Conversions

- Carry units symbolically: `5 km * 1000 m/km = 5000 m`.
- Common: length (mm/cm/m/km/in/ft), mass (g/kg/lb), temp (°C/°F/K), data (KB/MB/GB), time (s/min/hr), currency (state rate + date).
- Temperature needs offset, not ratio: `°F = °C*9/5+32`, `K = °C+273.15`.
- Dimensional check: if adding m + s, stop — something is wrong.

### 4. Statistics (Descriptive)

- Mean, median, stdev, min/max, n. Prefer median for skewed data.
- For small n (<30), report n explicitly — averages mislead.
- Rates: `events / exposure` with exposure stated (per 1000, per year).

```bash
python3 -c "import statistics; d=[12,15,14,60,13]; print(statistics.mean(d), statistics.median(d), statistics.stdev(d))"
```

### 5. Uncertainty + Rounding

- Round display to justified precision: inputs ~2 sig figs → output 2 sig figs.
- Show range when inputs are rough: "≈ $2.3–2.7k (assumes 10–12% fee)".
- Never report 6 decimals from 2-decimal inputs.

---

## Output Format

```markdown
Calc: (190-140)/140*100
= 35.7%
Sanity: ~36% — 50/140 ≈ 1/3 ✓
```

For multi-step: one line per step with units, final boxed.

---

## Review Checklist

1. **Computed** — Ran via tool, expression shown?
2. **Units carried** — Every step labeled, conversions explicit?
3. **Delta complete** — Absolute + ratio + % for comparisons?
4. **Precision honest** — Rounded to input quality?
5. **Sanity checked** — Order-of-magnitude plausible?
6. **Assumptions stated** — Rates, fees, constants cited?
7. **Reproducible** — Someone can re-run your expression?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Answer with no expression | Can't verify | Show `expr = result` |
| 6 decimals from rough inputs | False precision | Round to sig figs |
| % vs pp conflated | 5x misread | State both explicitly |
| Units dropped mid-calc | 1000x error | Carry units every step |
| Averaging n=3 | Noise as fact | Report n + range |
| Currency with no date/rate | Unreproducible | State rate + date |
| "Roughly 2x" for 1.6x | Overclaim | Compute exact ratio |
