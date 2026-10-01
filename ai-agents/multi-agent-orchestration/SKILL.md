---
name: multi-agent-orchestration
description: >
  Planner/worker orchestration for parallel agents: fan-out research, bounded
  workers, deduped synthesis. Triggers when splitting work across subagents,
  running parallel investigations, or merging multi-source findings.
---

# Multi-Agent Orchestration — Fan Out, Synthesize Once

You are an **Orchestrator**. One god-agent doing everything is slow and
sloppy. **Planner decomposes, workers execute in parallel, orchestrator
synthesizes once** — never redoes worker output.

> "Workers gather. Orchestrator judges. Nobody duplicates."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Sequential research** | 5 topics × 3 min = 15 min | Parallel fan-out, one round |
| **Overlapping briefs** | 3 workers read same files | Partitioned scopes, named owners |
| **Redoing work** | Orchestrator re-searches all | Trust-but-verify samples |
| **Merge mush** | Contradictions averaged away | Preserve + adjudicate |
| **No bounds** | Runaway token spend | Caps per worker + global |

---

## Protocol

```
1. DECOMPOSE → 2-5 non-overlapping briefs (owner + scope + deliverable)
2. FAN-OUT → workers run in parallel, max 10 steps each
3. COLLECT → claims + sources + confidence per worker
4. SYNTHESIZE → dedupe, contradiction-map, confidence-weight
5. VERIFY → spot-check 20% of load-bearing claims, re-read sources
```

### Brief Template

```markdown
Worker B: pricing signals
Scope: ONLY pricing pages + Wayback (not features)
Deliver: table [plan, price, limits, changed-since] + sources
Bounds: 10 steps, stop on 2 empty rounds
```

---

## Synthesis Rules

- Dedupe identical claims, keep strongest source.
- Contradictions preserved with sides + tiers (see deep-research §2.2).
- Confidence = weakest supporting link, not average enthusiasm.
- Gaps assigned as follow-up briefs, not silently filled.

---

## Review Checklist

1. **Partitioned** — No overlapping scopes?
2. **Bounded** — Steps + tokens capped per worker?
3. **Deliverables typed** — Tables/claims, not essays?
4. **No duplication** — Orchestrator didn't redo?
5. **Contradictions kept** — Not averaged?
6. **Spot-verified** — 20% re-checked?
7. **Within budget** — Global cap respected?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| 8 workers, same prompt | 8x same answer | Partition scopes |
| Unbounded workers | Cost explosion | Steps + token caps |
| Orchestrator re-does all | Wasted, second-class check | Sample-verify only |
| Merging by vibes | Loudest wins | Confidence-weighted + cited |
| No synthesis of gaps | Silent holes | Follow-up briefs listed |
