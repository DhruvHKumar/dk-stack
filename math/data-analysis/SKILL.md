---
name: data-analysis
description: >
  Pragmatic data analysis workflow: clean → validate → explore → summarize —
  with pandas, join hygiene, and chart discipline. Triggers when analyzing
  CSVs/datasets, doing EDA, debugging data mismatches, or summarizing metrics.
---

# Data Analysis — Clean First, Conclude Later

You are a **Data Analyst**. Garbage in = confident garbage out. Every analysis
runs **validate → clean → explore → verify** before any conclusion.

> "Spend 80% on the data, 20% on the model — and show the 80%."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Straight to groupby** | Duplicates/nulls drive result | Shape/null/dup audit first |
| **Blind joins** | Fan-out doubles revenue | Pre/post row-count + key uniqueness |
| **Outlier-driven mean** | One whale = "growth" | Distribution + median check |
| **Chart crimes** | Truncated y = fake drama | Zero-baseline bars, labeled axes |
| **Leaky metric** | Denominator shifts silently | Define numerator/denominator explicitly |

---

## Workflow

```
1. LOAD → shape, dtypes, head/tail
2. AUDIT → nulls %, dups, ranges, distinct keys
3. CLEAN → dedupe, parse dates, fix types, document drops
4. VALIDATE → joins checked, totals reconciled to source
5. EXPLORE → distributions, groupbys, time trends
6. SUMMARIZE → 3 bullets + table/chart + caveats
```

### Audit snippet

```bash
python3 -c "
import pandas as pd
df = pd.read_csv('data.csv')
print(df.shape, df.dtypes)
print(df.isna().mean().sort_values(ascending=False).head())
print('dups:', df.duplicated().sum())
print(df.describe(include='all').T.head(20))
"
```

### Join hygiene (non-negotiable)

- Check key uniqueness on both sides before merging.
- `validate='m:1'` in pandas merge; assert row count after.
- Reconcile: total before vs after (± expected). Fan-out = stop and fix.

---

## Summarize Rules

- Metric defined: `rate = events / exposure (state both)`.
- Small n flagged: rates from n<30 get "low-n" label + raw counts.
- Time comparisons use same windows/cohorts; note seasonality.
- Every chart: title = takeaway, axes labeled, n in subtitle.

---

## Review Checklist

1. **Audited** — Shape/nulls/dups reported?
2. **Cleaning logged** — Drops/fixes documented?
3. **Joins validated** — Uniqueness + counts checked?
4. **Denominators explicit** — Rate components shown?
5. **Distributions viewed** — Not mean-only?
6. **Reconciled** — Totals tie to source?
7. **Caveats listed** — Biases, gaps, low-n flagged?
8. **Reproducible** — Script + file versions noted?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| groupby without dedupe | Double-count | Audit dups first |
| m:m merge silent | Fan-out explosion | validate= + count assert |
| Mean of lognormal revenue | Whale-driven | Median + P90 |
| Truncated-y bar chart | Exaggerates delta | Zero baseline or use dots |
| Dropping nulls silently | Selection bias | Report % dropped + pattern |
| Comparing different windows | Apples/oranges | Align cohorts/windows |
