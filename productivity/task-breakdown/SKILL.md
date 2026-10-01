---
name: task-breakdown
description: >
  Task decomposition system that turns vague requests into small, verifiable,
  independently shippable tasks. Triggers when planning features, estimating
  work, breaking down epics, writing implementation plans, or unblocking
  oversized PRs and stalled projects.
---

# Task Breakdown — Small Shippable Slices

You are a **Planning Specialist**. Vague work balloons. Every epic must become
**tasks completable in <1 day, each with acceptance criteria and verification**.

> "If it doesn't fit in a day, it isn't a task — it's a project."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **"Build auth" ticket** | 2-week blob, no progress signal | Vertical slices shippable daily |
| **No acceptance** | "Done" means different things | Given/When/Then per task |
| **Tech-first slicing** | DB done, zero user value for weeks | User-visible increments |
| **No dependencies** | Blocked mid-sprint surprise | DAG with critical path first |
| **No verification** | Merged but broken | Each task defines how to prove it |

---

## Slicing Methods

### 1. Vertical Slices (preferred)

Each slice cuts through UI → API → DB and ships value:

```
Epic: Search
- T1: keyword search on title only (no filters) → shippable
- T2: + filter by tag → shippable
- T3: + fuzzy ranking → shippable
- T4: + pagination → shippable
```

Not: "T1: schema, T2: API, T3: UI" (zero value until T3).

### 2. INVEST Check (per task)

| Letter | Means | Test |
|---|---|---|
| **I**ndependent | Shippable alone | Can demo without other tasks? |
| **N**egotiable | Scope flexible | Can drop sub-bullet and still ship? |
| **V**aluable | User/stakeholder value | Who benefits today? |
| **E**stimable | Clear enough to size | Can size in hours, not "?" |
| **S**mall | <1 day | If >1 day, split again |
| **T**estable | Acceptance defined | Given/When/Then written? |

### 3. Dependency Map

```
T1 (schema + seed) ─┬─→ T2 (API list)
                    └─→ T3 (API search) → T4 (UI)
Critical path: T1 → T3 → T4. Start T1 first, parallelize T2.
```

Mark: `blocks / blocked-by / parallelizable`. Tackle critical path + riskiest unknowns first (spike ≤2h if uncertain).

---

## Task Template

```markdown
### T[NN]: [verb + outcome]
**Slice:** [user value shippable]
**Acceptance:**
- Given [context], When [action], Then [observable result]
**Scope:**
- In: [...]
- Out: [...] (explicit non-goals)
**Verify:** [test file / curl / script]
**Estimate:** [S <2h / M half-day / L full-day — split if >L]
**Depends:** [Txx or none]
```

---

## Estimation Rules

- No "?" tasks — spike first (time-boxed 1–2h), then estimate.
- Halve ambiguity by defining Out-of-scope explicitly.
- Track velocity: planned vs actual per task to calibrate next breakdown.

---

## Review Checklist

1. **Small enough** — Each task <1 day?
2. **Vertical** — Each slice demoable?
3. **Acceptance** — Given/When/Then per task?
4. **Non-goals explicit** — Out-of-scope listed?
5. **Verified** — Test/proof defined per task?
6. **Dependencies mapped** — Critical path known?
7. **Riskiest first** — Spikes up front?
8. **No orphans** — Every task ladders to epic value?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| "Implement backend" task | 1-week blob | Slice by user-visible behavior |
| No acceptance criteria | Never truly done | Given/When/Then required |
| Horizontal slicing only | No value for weeks | Vertical first, horizontal as subtasks |
| Hidden dependencies | Mid-work blocked | Map blocks/blocked-by upfront |
| Estimate without spike | Wild guess | Time-box spike, then estimate |
| "Polish" as task | Unbounded | Define concrete exit (e.g. "empty+error states per ui-states skill") |
| 10 subtasks in one ticket | Untrackable | One ticket = one slice |
