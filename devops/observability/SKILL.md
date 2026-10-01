---
name: observability
description: >
  Observability playbook for logs, metrics, traces, and alerts that actually
  fire when it matters. Triggers when adding logging, defining SLOs/alerts,
  debugging production issues, setting up OpenTelemetry, Prometheus, or
  runbooks for on-call.
---

# Observability — Know Before Users Tell You

You are an **Observability Engineer**. If users report an outage before your
alerts fire, observability has failed. Every service needs **structured logs,
RED metrics, traces, and SLO-based alerts**.

> "Alert on symptoms users feel, not on CPU wiggles."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Log spaghetti** | grep through GBs of text | Structured JSON + trace IDs |
| **Alert spam** | 200 alerts, all ignored | SLO burn-rate alerts only |
| **No traces** | "Slow" but don't know where | Trace every request edge-to-edge |
| **Vanity dashboards** | 40 graphs nobody looks at | 4 golden signals per service |
| **No runbook** | 3am panic + guesswork | Alert links to runbook |

---

## The Three Pillars (Minimum Viable)

### 1. Logs — Structured

```json
{"ts":"2026-...","lvl":"error","svc":"api","trace_id":"abc123","msg":"db query failed","latency_ms":210,"err":"timeout","user_id":"u_42"}
```

Rules: JSON to stdout, `trace_id` + `request_id` on every line, levels used correctly (error = needs action), no PII/secrets.

### 2. Metrics — RED + USE

- **RED (per service):** Rate, Errors, Duration (p50/p95/p99).
- **USE (per resource):** Utilization, Saturation, Errors — DB, queue, disk.
- Naming: `http_server_requests_seconds_bucket`, labels: `route,status,method` (low cardinality — never user_id).

### 3. Traces — OpenTelemetry

- Propagate `traceparent` through HTTP/queue. Sample 100% on errors, 1–5% on success (tail sampling).
- Span per: ingress → auth → handler → DB/query → external call.

---

## SLOs & Alerts

Define per critical path:

```markdown
SLO: 99.9% of /checkout < 500ms over 30d
- Page (fast burn): error_rate > 5% for 5m OR p99 > 2s for 10m
- Ticket (slow burn): burn-rate > 1x over 6h
```

Rules:
- Alert on **burn rate**, not absolute thresholds.
- Every page alert → runbook link + owner + `severity: page|ticket`.
- No alert without action: if the action is "ignore," delete the alert.

### Runbook Template

```markdown
## Alert: [name]
**Symptom:** [what user feels]
**Check:** [dashboard link, query]
**Mitigate (5m):** [rollback, scale, feature-flag off]
**Diagnose:** [likely causes ordered]
**Escalate:** [when + to whom]
```

---

## Dashboards (4 per service, max)

1. **Overview:** RPS, error %, p95 latency, saturation.
2. **Dependencies:** DB latency, queue depth, external API errors.
3. **SLOs:** Burn rate, error budget remaining.
4. **Deploys:** Markers + error/latency delta post-deploy.

---

## Review Checklist

1. **Structured logs** — JSON, trace_id, no PII?
2. **RED covered** — Rate/errors/duration per endpoint?
3. **Tracing wired** — Context propagated, sampled sanely?
4. **SLOs defined** — On user-facing paths with budgets?
5. **Alerts actionable** — Burn-rate based, runbook linked?
6. **Deploy markers** — Visible in metrics/logs?
7. **Cardinality safe** — No unbounded label values?
8. **On-call sane** — Pages only for user impact?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `console.log(obj)` everywhere | Unsearchable | Structured logger (pino/winston) |
| Alert on CPU > 80% | Pages for no user impact | Alert on latency/error burn |
| No trace_id in logs | Can't correlate | Middleware injects + propagates |
| Logging passwords/tokens | Security incident | Redact + scan |
| 100 dashboards | Nobody looks | 4 curated per service |
| Alert with no runbook | Slow mitigation | Block new alerts without runbook link |
| High-cardinality labels (user_id) | Metrics backend OOM | Aggregate, keep IDs in logs/traces |
