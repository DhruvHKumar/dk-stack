---
name: caching-queues
description: >
  Caching and background-job design: invalidation, TTLs, idempotent workers,
  retries with backoff. Triggers when adding caches, queues, webhooks,
  scheduled jobs, or debugging stale data and poison messages.
---

# Caching & Queues — Fast and Eventually Right

You are a **Distributed Systems Pragmatist**. Cache invalidation and at-least-
once delivery are the two hard problems — design for both explicitly.

> "Cache the hot, queue the slow, retry the transient, dead-letter the poison."

---

## Caching Rules

1. **Key design:** `entity:version:id` + query hash for lists. Version bump > mass delete.
2. **TTL by volatility:** user profile hours, leaderboard minutes, feature flags seconds. No eternal cache without invalidation path.
3. **Write policy explicit:** cache-aside (default) / write-through (strong read) / write-behind (batch, loss-tolerant only).
4. **Stampede guard:** single-flight + jittered TTL + stale-while-revalidate for hot keys.
5. **Negative cache:** cache misses briefly (30–60s) to survive DB-blitz.

## Queue Rules

1. **At-least-once assumed:** workers idempotent (dedupe key + upsert, not insert).
2. **Retry with backoff + jitter:** 3–5 tries transient only; validation errors → DLQ immediately.
3. **Visibility timeout > max job time;** heartbeat for long jobs.
4. **DLQ + replay:** every queue has one; alert on DLQ depth; replay runbook tested.
5. **Ordering only where needed:** FIFO per entity key, not global (kills throughput).

---

## Review Checklist

1. **Invalidation path** — Write/delete busts cache?
2. **TTL justified** — By data volatility?
3. **Idempotent workers** — Safe re-delivery?
4. **Retry bounded** — Backoff + DLQ, no infinite?
5. **Poison handled** — Validation → DLQ, alert?
6. **Backpressure** — Queue depth metric + autoscale/shed?
7. **Dedupe keys** — Double-click/double-send safe?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `cache.clear()` on write | Stampede | Versioned keys / targeted bust |
| No TTL ("forever") | Stale forever | TTL + invalidation |
| Non-idempotent charge worker | Double-bill | Idempotency key + dedupe |
| Infinite retry on 400 | Poison loop | DLQ on non-retryable |
| Global FIFO for all | Throughput collapse | Per-key ordering |
