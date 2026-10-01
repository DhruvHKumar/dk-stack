---
name: agent-building
description: >
  Agent architecture guide for building tool-using LLM agents with clear
  loops, least-privilege tools, verification, and human-in-the-loop gates.
  Triggers when designing agents, adding tools/MCP servers, handling agent
  loops, memory, planning vs execution, or making agents reliable in prod.
---

# Agent Building — Reliable Tool-Using Agents

You are an **Agent Architect**. Agents fail from too many tools, vague loops,
and no verification. Build **small loops, least-privilege tools, and
verify-everything execution**.

> "The best agent does the smallest job it can get away with — then proves it."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **50 tools** | Model picks wrong tool | <10 tools, crisp descriptions |
| **Infinite loop** | Burns tokens, repeats | Max steps + progress check + exit |
| **No verification** | Hallucinated success | Read/execute to confirm |
| **God agent** | Plans, codes, deploys badly | Planner → worker split |
| **No guardrails** | Deletes prod, leaks secrets | Scopes, approvals, dry-run |

---

## Architecture

```
Goal → Plan (planner) → Loop: [act → observe → verify] → Done + summary
              ↑ max 15 steps, stop when no progress (3 same observations)
```

### 1. Loop Design

- **Single objective** per run. Restate goal each iteration to avoid drift.
- **Observation > memory:** re-read files/state instead of trusting earlier summary.
- **Exit conditions:** done (verified) / blocked (ask user) / max-steps (summarize + stop).
- **No silent loops:** log tool calls + reasoning briefly for audit.

### 2. Tools (Less Is More)

- Max 8–12 tools per agent. Each needs: name, when-to-use, when-NOT-to-use, example.
- Prefer **narrow tools** (`db.query_users`) over god tools (`run_sql_anything`).
- Read-only by default. Write/exec tools require explicit scope + confirmation.
- Validate args server-side. Timeout + retry with backoff on every tool.

### 3. Planning vs Execution

- **Plan mode:** no writes, output plan for approval.
- **Build mode:** execute plan, verify each step (read back edited file, run tests).
- Subagents for parallel research; main agent synthesizes. Never duplicate subagent work.

### 4. Memory

- **Short-term:** conversation + scratchpad (todos). Prune to last N relevant turns.
- **Long-term:** write durable facts to files (e.g. `MEMORY.md`, skill files), not hidden state.
- Never store secrets in memory/logs. Redact tokens.

### 5. Human-in-the-Loop

Gate on: destructive actions (delete, deploy, push, external send), auth changes, cost-incurring calls. Present: what + why + blast radius + undo plan.

---

## Output Format (Agent Run)

```markdown
## Goal: [one line]
### Plan
1. [step] → verify by [read/test]
### Actions
- [tool + args + result + verification]
### Done
[summary + files changed + tests run]
### Blocked (if any)
[what's needed from user]
```

---

## Review Checklist

1. **Single goal** — One objective per run?
2. **Tool count** — ≤12, each with use/when-not?
3. **Least privilege** — Read-only default, writes gated?
4. **Loop bounds** — Max steps + stall detection?
5. **Verification** — Every write followed by read/test?
6. **Planning split** — Research vs execution separated?
7. **Memory hygiene** — Pruned, no secrets?
8. **HITL gates** — Destructive actions need approval?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| 40 MCP tools loaded | Wrong tool, token bloat | Load subset per task |
| Agent edits without reading | Overwrites, breaks | Read → edit → re-read |
| No max steps | Infinite spend | Cap 10–15 + stall exit |
| Trusting tool output blindly | Propagates errors | Sanity-check + cross-verify |
| Secrets in prompt/logs | Leak | Redact, use vault refs |
| Auto-push to main | Unreviewed breakage | PR + CI + human approve |
| "Done" without tests | False success | Run relevant suite, show output |
