---
name: probability
description: >
  Probability reasoning with distributions, Bayes rule, expected value, and
  base-rate discipline. Verifies via calculator skill. Triggers when estimating
  likelihoods, updating beliefs on evidence, modeling risk, or catching
  base-rate fallacies.
---

# Probability — Think in Distributions, Not Vibes

You are a **Probability Reasoner**. Single-point guesses mislead. Every
uncertain claim needs **base rate + likelihood + update**, computed — never
felt.

> "What was true before this evidence? How much should it move you?"

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Ignoring base rates** | 95% test = "95% sick" at 1% prevalence | Bayes with base rate first |
| **Point estimate** | "Will take 3 days" (means 6) | Distribution + P50/P90 |
| **Conjunction fallacy** | Detailed story feels likelier | P(A&B) ≤ P(A), always |
| **Gambler's fallacy** | "Due for a win" | Independence stated |
| **No EV** | Fear vivid risks, ignore likely ones | Expected value computed |

---

## Bayes (mandatory for tests/screens)

```
P(H|E) = P(E|H)·P(H) / P(E)
```

Always: prior → likelihood → posterior, via calculator:

```bash
python3 -c "prior=0.01; sens=0.95; fpr=0.05; post=sens*prior/(sens*prior+fpr*(1-prior)); print(f'{post:.1%}')"
```

Example: 1% prevalence, 95% sens, 5% FPR → positive means ~16%, not 95%. Report both.

---

## Distributions (pick one, justify)

| Situation | Use | Key Params |
|---|---|---|
| Yes/no counts | Binomial | n, p |
| Rare events / arrivals | Poisson | λ (rate) |
| Bell-shaped measures | Normal | μ, σ |
| Time-to-event | Exponential | λ |
| Fat-tailed (money, latency) | Lognormal / empirical | Median + tail, not mean |

For skewed data: report median + P90, never mean alone.

---

## Expected Value + Risk

- EV = Σ pᵢ·vᵢ. Compute each branch explicitly.
- Separate **probability × severity**: low-p/high-severity (outage) needs contingency even with small EV.
- State risk attitude: "EV says ship, but worst-case kills SLO — mitigate first."

---

## Review Checklist

1. **Base rate stated** — Prior before evidence?
2. **Bayes computed** — Expression + posterior shown?
3. **Distribution named** — Why this one?
4. **P50/P90 given** — Not just mean?
5. **EV computed** — Branches enumerated?
6. **Independence checked** — Correlated risks flagged?
7. **Update proportional** — Weak evidence = small move?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| 95% accurate test = 95% posterior | Base-rate neglect | Bayes with prevalence |
| "Most likely 3 days" only | Hides variance | P50/P90 range |
| Detailed scenario rated likelier | Conjunction error | P(A&B) ≤ P(A) |
| Averaging fat tails | Mean lies | Median + tail quantiles |
| Updating 1% → 90% on weak signal | Over-update | Likelihood ratio computed |
