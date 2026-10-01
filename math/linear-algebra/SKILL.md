---
name: linear-algebra
description: >
  Vector and matrix reasoning: solving Ax=b, least-squares, rank/determinants,
  and PCA intuition. Triggers when handling embeddings, transforms,
  simultaneous equations, regressions, or dimensionality questions.
---

# Linear Algebra — Shapes First, Numbers Second

You are a **Linear Algebra Reasoner**. Dimension mismatch is the #1 bug.
Every operation starts with **shapes stated and compatible** before computing.

> "If the shapes don't match, the math doesn't matter."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Shape blindness** | (n,) vs (n,1) broadcast bug | Annotate every shape |
| **Inverting singular** | Garbage "solution" | Check det/rank/cond first |
| **Overfit exact fit** | Fits noise, fails prod | Least-squares + residual check |
| **Cosine without norm** | Unnormalized similarity | Normalize or justify raw dot |
| **PCA as magic** | Leaky, uninterpretable | Variance explained + loadings |

---

## Essentials

- **Shapes:** `A(m×n) x(n,) = b(m,)`. State m, n every time.
- **Solve Ax=b:** exact only if square + full rank; else least-squares `min‖Ax-b‖²`.
- **Rank/det:** det≈0 or rank<n → singular — use lstsq/pinv, never inv.
- **Cosine:** `a·b/(‖a‖‖b‖)` — normalize; report norm alongside.
- **Least-squares check:** residual `‖Ax-b‖`, R², plot residuals (patterns = model wrong).

```bash
python3 -c "
import numpy as np
A=np.array([[2.,1.],[1.,3.]]); b=np.array([5.,6.])
x, res, rank, sv = np.linalg.lstsq(A,b,rcond=None)
print('x=',x,'rank=',rank,'res=',res)
print('check:', A@x)
"
```

---

## Embeddings / PCA Notes

- Normalize embeddings before cosine; state model + dim.
- PCA: report variance-explained per component + top loadings. Fit on train only.

---

## Review Checklist

1. **Shapes stated** — m×n annotated, compatible?
2. **Rank checked** — Singular handled via lstsq?
3. **Residual shown** — ‖Ax-b‖ + sanity?
4. **Normalized?** — Cosine/dot justified?
5. **No raw inv** — On singular/rectangular?
6. **Train/test split** — For fitted transforms?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| np.inv on rect/singular | Wrong/crash | lstsq / pinv |
| Dot without norms | Scale confounds | Cosine + norms |
| Ignoring cond number | Unstable solve | Check cond, regularize |
| PCA fit on all data | Leakage | Fit train, transform test |
| Residuals unexamined | Misspecification | Plot + R² |
