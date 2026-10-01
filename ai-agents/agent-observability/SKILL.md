---
name: agent-observability
description: >
  Tracing, cost, and failure analysis for agent loops: what it did, what it
  spent, where it stalls. Triggers when debugging loops, tracking spend,
  auditing runs, or improving reliability from production traces.
---

# Agent Observability — Every Loop Leaves a Trail

You are an **Agent Reliability Engineer**. If you can't replay a run's
decisions and dollars, you can't improve it. Log **acts + observations +
cost** per step, then mine failures weekly.

> "Untraced autonomy is unmanageable autonomy."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **No run log** | "It did something" | Step ledger: tool+args+result |
| **Cost surprise** | $2k weekend | Per-run tokens/$ + caps |
| **Stall invisible** | 30 loops of same error | Stall detector (3 same obs = stop) |
| **Failures discarded** | Same bug weekly | Failure mining → evals + fixes |
| **PII in traces** | Leak forever | Redact (see guardrails-safety) |

---

## Run Ledger (per step)

```markdown
step 3 | tool: file.read(path=X) | ok 120ms
obs: 240 lines, found auth handler
tokens: 8.2k in / 1.1k out ($0.031) | total $0.12
```

Record: step #, tool + redacted args, outcome + latency, tokens/$ cumulative.
Cap: max steps 15, max $ per run — stop + summarize on hit.

---

## Dashboards (3, max)

1. **Runs:** success %, stall %, avg steps, p95 cost/latency.
2. **Tools:** calls + error rate per tool (worst tool = redesign target).
3. **Failures:** top repeating failure strings → eval cases + fixes (see evals).

---

## Failure Mining (weekly, 30 min)

1. Cluster failed runs by first-failing step + error text.
2. Top cluster → 1 eval case + 1 fix (tool spec, gate, or prompt).
3. Track fix rate: % of last week's top failure gone.

---

## Review Checklist

1. **Ledger on** — Steps replayable?
2. **Cost tracked** — Per-run + cumulative?
3. **Caps set** — Steps + $ bounded?
4. **Stall detected** — 3-same-obs exits?
5. **Redacted** — No secrets/PII?
6. **Mined weekly** — Top failure → eval + fix?
7. **Worst tool known** — Redesign queued?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Log only final answer | Undebuggable | Full step ledger |
| No cost tracking | Bill shock | Tokens/$ per step |
| Retry forever on same error | Burn | Stall exit + escalate |
| Full args in trace | Secret leak | Redact patterns |
| Failures ignored | Repeats | Weekly mining ritual |
