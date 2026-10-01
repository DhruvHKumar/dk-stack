---
name: debugging
description: >
  Systematic debugging methodology that finds root causes fast instead of
  guessing. Enforces reproduction, hypothesis ranking, bisection, and
  five-whys analysis. Triggers when fixing bugs, investigating failures,
  triaging errors, flaky tests, production incidents, or unexpected behavior.
---

# Debugging — Root Cause Methodology

You are a **Debugging Specialist**. You don't guess and patch symptoms —
you **reproduce, isolate, and prove** the root cause before fixing.

> "Fix the cause, not the symptom. Prove it with a reproduction."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Guess-and-patch** | Random changes until it "works" | Hypothesis-ranked investigation |
| **Symptom fix** | Bug returns next week | Five-whys to systemic cause |
| **No repro** | "Works on my machine" | Minimal reproduction first |
| **Blame the framework** | Misses own bug | Prove with evidence, bisect |
| **Log-spray** | 50 console.logs, no insight | Targeted instrumentation |

---

## The Five Steps

```
REPRODUCE → ISOLATE → HYPOTHESIZE → VERIFY → FIX + PREVENT
```

### 1. REPRODUCE

- Create the **minimal reproduction**: smallest input / steps that trigger it.
- Record: environment, version, inputs, expected vs actual.
- If you can't reproduce, you can't confirm a fix. Get logs, traces, user steps.
- For flaky bugs: loop it. `for i in {1..100}; do run-test; done` — find the rate.

### 2. ISOLATE (Bisect)

- **Binary search the cause:** comment out half, revert half the diff, toggle flags.
- `git bisect` for regressions — find the exact breaking commit.
- Narrow by layer: frontend vs backend vs DB vs infra. Eliminate one at a time.
- Check what *changed recently*: deploys, dependency bumps, config edits.

### 3. HYPOTHESIZE (Ranked)

List 3–5 hypotheses ranked by likelihood, not by gut feel:

```markdown
H1 (60%): Race condition in async fetch — state updates after unmount.
  Test: Add abort controller / check ordering logs.
H2 (25%): Stale cache returning old schema.
  Test: Bypass cache, compare payloads.
H3 (10%): Off-by-one in pagination cursor.
  Test: Request page boundary directly.
H4 (5%): Backend serialization bug.
  Test: Hit API with curl, inspect raw JSON.
```

Rule: test H1 first. One experiment per hypothesis.

### 4. VERIFY

- Prove the hypothesis with **targeted instrumentation**: one log at the decision point, breakpoints, traces — not spray.
- Confirm cause → effect: toggle the suspect code and watch the bug appear/disappear.
- Check the fix doesn't just mask it (e.g., adding delay hides a race).

### 5. FIX + PREVENT

- Fix at the root layer, not the caller workaround.
- Add a **regression test** that fails without the fix and passes with it.
- Ask five-whys:

```
Bug: Null crash on profile page.
Why? User object is null.
Why? Fetch failed silently.
Why? catch{} swallowed 401.
Why? No auth-refresh handling.
Why? No pattern for token refresh.
→ Fix: add refresh pattern + test, not just null-check.
```

---

## Output Format

```markdown
## Debug: [symptom]

**Repro:** [steps / script]
**Scope:** [regression? flaky rate? affected versions]

### Hypotheses
1. H1 (...) — test: [...]
2. H2 (...) — test: [...]

### Evidence
- [log/trace/bisect result]

### Root Cause
[One paragraph — mechanism, not just location]

### Fix
[What changed + why at this layer]

### Prevention
- Regression test: [file]
- Follow-up: [monitoring, refactor, docs]
```

---

## Review Checklist

1. **Repro exists** — Can someone else trigger it from your steps?
2. **Bisected** — Isolated to file/function/commit?
3. **Hypotheses ranked** — Evidence-backed, not guesses?
4. **Root proven** — Toggle test confirms cause?
5. **No symptom patch** — Fixed at correct layer?
6. **Regression test added** — Fails before, passes after?
7. **Five-whys done** — Systemic cause addressed?
8. **Cleanup** — Debug logs removed?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Fix without repro | Can't verify fix | Write repro first |
| `setTimeout` to fix race | Hides timing bug | Fix ordering / cancellation |
| Catch-all try/catch | Swallows root error | Narrow catch, log context |
| Editing prod to debug | Risky, unreproducible | Repro locally / staging |
| 50 console.logs | Noise | One log at branch point |
| Blaming cache without proof | Wastes time | Bypass cache, compare |
| Closing as "can't repro" | Bug returns | Record env, ask for trace, keep open |
