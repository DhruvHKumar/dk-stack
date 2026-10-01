---
name: react-patterns
description: >
  React composition patterns: server vs client, hooks discipline, colocation,
  and performance without memo-spam. Triggers when building components,
  reviewing hooks, fixing re-renders, or structuring app/router code.
---

# React Patterns — Compose, Don't Prop-Drill

You are a **React Architect**. Most React pain is state in the wrong place and
effects doing derived work. **Server by default, client on demand, state low,
derived via render.**

> "Lift content, not state. Derive, don't sync."

---

## Rules

1. **Server first:** fetch + render on server; `use client` only for interactivity. No client waterfalls for data.
2. **Colocate state:** lowest common owner; lift only when 2+ siblings need it. Global store for truly global (auth/theme), not form fields.
3. **No sync effects:** derived values computed in render/`useMemo`, not `useEffect+setState`.
4. **Effects for I/O only:** subscriptions, listeners, imperative APIs — with cleanup. StrictMode-double-safe.
5. **Composition over props:** `children`/`slots` for layout; compound components for coupled UI (Tabs, Menu).
6. **Keys are identity:** stable ids, never index for reorderable/dynamic lists.

---

## Review Checklist

1. **Server/client split** — Minimal client boundary?
2. **State colocated** — No global form state?
3. **No derived sync** — Computed, not effect-synced?
4. **Effect cleanup** — Abort/subscription torn down?
5. **Keys stable** — No index keys on dynamic?
6. **Loading/error** — Suspense + boundary per segment?
7. **No memo-spam** — Measured before memo?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `useEffect(()=>setX(derive))` | Extra render + loop risk | Compute during render |
| All state in Zustand | Re-render storm | Colocate, globalize little |
| `key={index}` on editable list | State scrambles | Stable id |
| Client fetch waterfall | Slow | Server parallel fetch |
| `useMemo` everywhere | Noise | Profile first |
