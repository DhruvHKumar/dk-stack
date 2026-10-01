---
name: tool-design
description: >
  Design guide for crisp, least-privilege agent tools and MCP servers that
  models actually pick correctly. Triggers when adding tools, writing MCP
  servers, fixing wrong-tool picks, or scoping permissions.
---

# Tool Design — Tools Models Pick Right

You are a **Tool Designer**. Agents fail when tools are many, vague, or
god-powered. Every tool must be **narrow, named by outcome, and safe by
default**.

> "If the model has to guess which tool to use, you designed two tools as one."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **50 tools loaded** | Wrong pick, token bloat | ≤12 per task, subset loading |
| **Vague description** | Misuse, hallucinated args | When-use + when-NOT + example |
| **God tool** | run_sql_anything deletes prod | Narrow verbs, scoped args |
| **No validation** | Bad args explode downstream | Server-side schema + errors |
| **Silent timeout** | Hangs the loop | Timeout + retry + partial result |

---

## Tool Spec Template

```markdown
name: db.query_orders
when: look up orders by id/user within date range
when-not: no bulk exports (use db.export_orders), no writes (use db.update_order)
args: {order_id?: string, user_id?: string, since?: date, limit=50 (max 200)}
returns: [{id, total, status}] + truncated flag
errors: INVALID_RANGE, NOT_FOUND (both actionable)
example: query_orders(user_id=u_42, since=2026-01-01)
side-effects: none (read-only)
timeout: 10s, retry 2x on 5xx only
```

Rules: verb_noun names, required vs optional explicit, enums closed, max
limits enforced server-side.

---

## Scoping

- Read-only by default. Write/exec tools gated (see autonomy-gates).
- Narrow over god: `orders.lookup` + `orders.update_status` beats `db.query`.
- Validate args strictly; return actionable errors naming the fix.
- Idempotency keys on writes; dry-run flag on destructive ops.

---

## Review Checklist

1. **Count sane** — ≤12 loaded for this task?
2. **Spec complete** — Use/when-not/example/errors?
3. **Narrow** — One job per tool?
4. **Validated** — Schema enforced server-side?
5. **Safe default** — Read-only unless gated?
6. **Timeout+retry** — Bounded, backoff?
7. **Observable** — Calls logged with args (redacted)?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| 40 tools always loaded | Mis-picks | Task-scoped subsets |
| "Runs commands" tool | Unbounded blast radius | Narrow verbs + allowlist |
| No example in description | Guessed args | One canonical example |
| Client-side-only validation | Bypassed | Server enforces |
| Unlimited limit/offset | OOM / exfil | Caps + pagination |
| Destructive without dry-run | No preview | dry_run default true |
