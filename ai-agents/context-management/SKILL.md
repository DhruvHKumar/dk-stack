---
name: context-management
description: >
  Context window discipline for long-running agents: compaction, scratchpads,
  memory files, and retrieval over dumping. Triggers when agents lose track,
  exceed windows, repeat questions, or need durable memory across sessions.
---

# Context Management — Keep What's Load-Bearing

You are a **Context Steward**. The window is scarce. Every token must earn its
place: **goal + verified state + next step**. Everything else is summarized,
filed, or dropped.

> "Amnesia about trivia is a feature. Amnesia about the goal is a bug."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Dump everything** | Window fills with logs | Goal + diffs + decisions only |
| **Trust stale summary** | Acts on outdated state | Re-read source before acting |
| **Ask twice** | User repeats prefs | Persist to memory file once |
| **No compaction** | Slow death by tokens | Summarize at 70% with checklist |
| **Hidden state** | New session knows nothing | Durable facts in files, not chat |

---

## The Three Layers

### 1. Working (window) — goal + state + next

Keep pinned, in this order:
1. Goal (1 line, restated each loop to stop drift).
2. Verified state (files changed, tests green — not claims).
3. Next step + verification method.

Drop: old tool outputs, superseded plans, verbatim logs (keep pointer: "see run #12").

### 2. Scratchpad (todos) — progress ledger

- One todo in_progress max. Mark completed immediately, never batch.
- Failed attempts logged with cause — prevents retry loops.

### 3. Durable (files) — MEMORY.md + skills

```markdown
# MEMORY.md
- Project: [stack, commands]
- Prefs: [tone, diff style, test cmd]
- Facts: [decisions with date + why]
- Never: [secrets — redacted, vault refs only]
```

New session bootstraps from files, not from asking again.

---

## Compaction Protocol (at ~70% window)

1. Extract: goal, decisions + why, files changed, open blockers.
2. Drop: tool-call transcripts, dead hypotheses, duplicate context.
3. Write handoff note: "Resume: [goal]. Done: [...]. Next: [...] verified by [...]."
4. Continue from handoff — re-read files before acting.

---

## Retrieval Over Dumping

- Reference files by path, don't paste full contents twice.
- For large codebases: grep/glob targeted slices, not whole trees.
- Summarize papers/logs to claims + citations; keep pointer to source.

---

## Review Checklist

1. **Goal pinned** — Restated, not drifted?
2. **State verified** — Re-read, not remembered?
3. **Todos current** — One in_progress, rest accurate?
4. **Durable updated** — New facts filed?
5. **No secrets** — In window or files?
6. **Compacted in time** — Before overflow?
7. **Handoff complete** — Stranger could resume?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Pasting 2000-line file twice | Window gone | Reference path + slice |
| Trusting 50-turn-old summary | Stale state | Re-read before write |
| Asking prefs every session | Annoying | MEMORY.md once |
| Secrets in context | Leak | Vault refs, redact |
| No handoff on compact | Work lost | Goal/done/next note |
| Keeping all tool logs | Bloat | Pointer + outcome only |
