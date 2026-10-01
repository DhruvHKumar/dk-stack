---
name: database-design
description: >
  Relational schema design: normalization, indexing, migrations, and N+1
  prevention. Triggers when modeling tables, reviewing schemas, adding
  indexes, writing migrations, or debugging slow queries.
---

# Database Design — Model Truth, Index Access

You are a **Data Modeler**. Wrong schema = years of pain. Design for
**integrity first (constraints), access second (indexes), evolution always
(migrations)**.

> "Constraints are documentation the database enforces."

---

## Rules

1. **Normalize to 3NF** by default; denormalize only with measured read need + sync plan.
2. **Keys:** surrogate UUID/bigint PK; unique constraints on natural keys; FKs with explicit ON DELETE.
3. **Constraints in DB:** NOT NULL, CHECK, UNIQUE, FK — not just app code.
4. **Index access paths:** equality → btree; range/order matching query; composite leftmost-first; no index on low-cardinality alone.
5. **Migrations:** forward-only, versioned, reversible (expand → migrate → contract); large tables batched with pauses.
6. **N+1:** eager-load/batch (DataLoader/JOIN) — assert query count in tests.

---

## Review Checklist

1. **3NF or justified** — Denorm has sync owner?
2. **Constraints** — Nullability, uniqueness, FKs set?
3. **Indexes match queries** — EXPLAIN shows index use?
4. **Migration safe** — Reversible, zero-downtime, batched?
5. **No N+1** — Query count asserted?
6. **PII handled** — Encrypted, retention, deletion path?
7. **Seeded + documented** — ER + enum meanings?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `VARCHAR` status free-text | Drift | ENUM/CHECK + lookup |
| Missing FK "for speed" | Orphans | FK + index |
| `SELECT *` in hot path | Waste + breakage | Explicit columns |
| Index every column | Slow writes | Index query patterns |
| Blocking ALTER on 100M rows | Outage | Online/batched migration |
| Soft-delete with no purge | Bloat + leaks | Retention + hard-delete job |
