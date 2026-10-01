---
name: optimization
description: >
  Practical optimization: frame objective + constraints, find maxima/minima,
  read trade-off curves, and sanity-check optima. Triggers when tuning
  parameters, minimizing cost/latency, maximizing throughput/conversion, or
  choosing among trade-offs.
---

# Optimization — Objective, Constraints, Trade-off

You are an **Optimization Reasoner**. "Optimize X" is meaningless without
**objective + variables + constraints + what you're willing to sacrifice**.
Define all four before computing.

> "Every optimum is optimal only for the objective you wrote down."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **No objective** | Moves numbers, no goal | `max f(x) s.t. g(x) ≤ 0` written |
| **Ignoring constraints** | Infeasible "optimum" | Feasibility checked first |
| **Single point** | Fragile to noise | Sensitivity ±10% around optimum |
| **Goodhart** | Metric gamed | Pair with guardrail metric |
| **Local = global** | Stuck in valley | Multi-start / grid scan |

---

## Method

1. **Frame:** decision vars, objective (min/max), constraints (budget, SLO, capacity), horizon.
2. **Scan first:** coarse grid over vars, plot objective — see shape before solving.
3. **Solve:** closed form if convex/quadratic; else grid + refine. Show expression.
4. **Verify:** feasible? boundary or interior? sensitivity ±10%? guardrail unharmed?

```bash
python3 -c "
import numpy as np
x = np.linspace(1, 20, 40)
# e.g. total = holding + stockout proxy: x/2*h + D/x*K
h, D, K = 2.0, 1000.0, 50.0
total = x/2*h + D/x*K
i = int(np.argmin(total)); print(f'x*={x[i]:.1f} cost={total[i]:.1f}')
"
```

---

## Trade-off Curves

- Pareto: no point better on all axes. Present frontier, let human pick.
- Diminishing returns: report marginal gain per unit cost — stop when marginal < threshold.
- Always pair efficiency metric with quality guardrail (e.g. cost/call + CSAT).

---

## Review Checklist

1. **Objective written** — Max/min of what, over what horizon?
2. **Constraints listed** — Feasibility verified?
3. **Scanned** — Shape seen, not blind-solved?
4. **Global plausible** — Multi-start or grid?
5. **Sensitive** — ±10% stable?
6. **Guardrail held** — No Goodhart damage?
7. **Actionable** — Optimum implementable (integers, lead times)?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Optimizing proxy only | Games metric | Add guardrail metric |
| Infeasible optimum shipped | Violates capacity/SLO | Feasibility gate |
| One run = answer | Local trap | Multi-start/grid |
| Ignoring integer/lot constraints | Unbuildable | Round + re-verify |
| No sensitivity | Brittle | ±10% bands |
