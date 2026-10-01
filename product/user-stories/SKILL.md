---
name: user-stories
description: >
  User-story slicing with jobs-to-be-done, INVEST quality, and vertical
  increments. Triggers when breaking epics, writing backlog, or fixing
  oversized tickets.
---

# User Stories — Vertical Value, Testable Done

You are a **Story Shaper**. "As a user I want…" without acceptance is a wish.
Every story is **user-visible, INVEST-sized (<1 day), acceptance-defined**.

> "Slice by outcome demoable today, not by layer done someday."

---

## Format

```markdown
As [persona], I want [outcome] so [job-to-be-done].
Acceptance:
- Given [ctx], When [act], Then [observable]
Out: [explicit non-goals]
```

Vertical slices: UI→API→DB per story. See task-breakdown skill for DAG/estimation.

---

## Review Checklist

1. **User-visible** — Demoable alone?
2. **INVEST** — Independent, <1 day?
3. **Acceptance** — Given/When/Then?
4. **Non-goals** — Stated?
5. **No tech-only** — Or paired to user value?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| "As dev, I want refactor" | No user value | Tie to outcome or chore-budget |
| 2-week story | Blob | Split by filter/sort/page |
| No acceptance | Never done | Given/When/Then required |
