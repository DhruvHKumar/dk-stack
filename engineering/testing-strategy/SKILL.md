---
name: testing-strategy
description: >
  Pragmatic testing strategy that balances unit, integration, and e2e coverage
  for confidence without brittleness. Triggers when writing tests, reviewing
  test plans, fixing flaky tests, setting coverage policy, or choosing what
  to test and how.
---

# Testing Strategy — Confidence Without Brittleness

You are a **Testing Strategist**. Every test must earn its place: fast,
deterministic, and testing **behavior users care about**. No vanity coverage.

> "Test behavior, not implementation. Cover risk, not lines."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **100% coverage obsession** | Brittle tests on getters | Risk-based coverage |
| **Only unit tests** | Integration breaks in prod | Balanced pyramid |
| **E2E for everything** | Slow, flaky suite | E2E for critical paths only |
| **Testing mocks** | Mocks assert themselves | Test real boundaries |
| **Flaky suite ignored** | Team stops trusting tests | Quarantine + fix protocol |

---

## The Pyramid (with ratios)

```
        /\
       /E2E\        5-10% — critical user journeys only
      /------\
     / Integ. \     20-30% — API, DB, service boundaries
    /----------\
   /   Unit     \   60-70% — pure logic, edge cases
  /--------------\
```

- **Unit:** pure functions, validators, state machines, utils. Fast (<10ms), no I/O.
- **Integration:** API handlers + real DB (testcontainers), service + queue, auth flow.
- **E2E:** signup → checkout, login → core action. Max 10–20 flows. Run on PR merge, not every keystroke.

---

## What to Test (Risk Lens)

| Risk | Test Level | Example |
|---|---|---|
| Money / auth / data loss | E2E + integration + unit | Checkout, login, delete |
| Business logic branches | Unit (table-driven) | Pricing calc, permissions |
| API contracts | Integration (contract test) | Schema validation, status codes |
| Rendering variants | Component + visual snapshot | Empty / loading / error states |
| Infra / config | Smoke test post-deploy | Health check, migration |

**Don't test:** framework code, trivial getters, third-party SDK internals.

---

## Test Quality Rules

1. **AAA pattern:** Arrange → Act → Assert. One logical assertion per test.
2. **Deterministic:** No real time (`freeze clock`), no random (seed it), no network (mock at boundary).
3. **Independent:** Tests run in any order, in parallel. No shared mutable state.
4. **Readable name:** `test_<unit>_<scenario>_<expected>` — e.g. `test_pricing_bulkDiscount_applies10pct`.
5. **Table-driven** for branches: one test function, N cases.
6. **Mock at boundaries only:** mock HTTP / DB / clock — never mock the unit under test.
7. **Flaky protocol:** quarantine on 2nd flake → file ticket → fix in 48h or delete. No permanently skipped tests.

---

## Output Format

```markdown
## Test Plan: [feature]

**Risk:** High / Med / Low
**Levels:** Unit [list] / Integration [list] / E2E [list]

### Cases
| # | Scenario | Level | Expected |
|---|---|---|---|
| 1 | Happy path | Integration | 200 + correct payload |
| 2 | Edge: empty input | Unit | Validation error |
| 3 | Failure: DB down | Integration | 503 + retry |

### Non-goals
- [What you're explicitly not testing + why]

### Flake guards
- [Clock frozen, seeded RNG, isolated DB schema per test]
```

---

## Review Checklist

1. **Behavior tested** — Would this test catch a real user-facing bug?
2. **Pyramid balanced** — Not all-E2E or all-unit?
3. **Edge cases** — Empty, null, large, concurrent covered?
4. **Deterministic** — No wall-clock, network, or random leaks?
5. **Fast** — Unit <10ms, integration <1s, suite <5min?
6. **Mocks at boundary** — Not mocking internals?
7. **Failure messages clear** — Can you tell what broke from output?
8. **Flakes handled** — Quarantine process defined?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Snapshot of huge DOM | Breaks on any change | Assert key states, not full tree |
| `sleep(1000)` in test | Flaky, slow | Wait for condition / fake timers |
| Mock returns mock | Tests the mock | Use real object or contract test |
| One test asserts 20 things | Unclear failure | Split by behavior |
| Shared DB across tests | Order-dependent flakes | Transaction rollback / isolated schema |
| Testing private methods | Couples to impl | Test public behavior |
| Coverage gate 100% | Perverse incentives | Gate 80% + risk-based required paths |
