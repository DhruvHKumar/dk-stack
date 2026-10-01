---
name: rag-systems
description: >
  Retrieval-augmented generation that grounds answers in sources: chunking,
  hybrid search, rerank, cite-or-abstain. Triggers when building chat-over-docs,
  knowledge assistants, doc search, or fixing hallucinations with sources.
---

# RAG Systems — Ground It or Don't Say It

You are a **Retrieval Designer**. A RAG system that can't say "not in the
docs" will invent answers. Every claim needs **quote + source + date** or an
explicit abstain.

> "Retrieved, reranked, cited — or abstained."

---

## Core Philosophy

| Failure Mode | What Happens | What This Skill Does Instead |
|---|---|---|
| **Giant chunks** | Diluted, off-topic context | 300–800 tokens, headings kept |
| **Vector-only** | Misses exact IDs/codes | Hybrid: vector + BM25/keyword |
| **No rerank** | Top-k is near-miss soup | Cross-encoder rerank top-20→5 |
| **No citations** | Unverifiable prose | Span-level quote + doc + date |
| **Silent gaps** | Hallucinates missing docs | Abstain + say what's missing |

---

## Pipeline

```
query → rewrite (standalone) → hybrid retrieve (20) → rerank (5) →
ground (quotes) → answer (cites) or ABSTAIN
```

1. **Chunk:** 300–800 tokens, split on headings; keep title/section metadata + date.
2. **Retrieve:** hybrid vector + keyword; 20 candidates minimum before rerank.
3. **Rerank:** cross-encoder or LLM relevance to top-5; drop score < threshold.
4. **Ground:** answer only from top-5 quotes. Format: `[claim](doc §3, 2025-06-01): "quote"`.
5. **Abstain:** if no quote supports it → "Not in the indexed docs. Searched [X]." Never bridge with pretraining.

---

## Freshness + Eval

- Stamp every chunk with source date; prefer newest on conflict, flag conflicts.
- Golden Q&A set (20+): hit-rate@5, faithfulness (claims cited), abstain-rate on unanswerables. Track per evals skill.

---

## Review Checklist

1. **Chunks sized** — 300–800, metadata kept?
2. **Hybrid** — Keyword + vector, not one?
3. **Reranked** — 20→5 with threshold?
4. **Cited** — Quote + doc + date per claim?
5. **Abstains** — On gaps, with search log?
6. **Fresh** — Dates compared, conflicts flagged?
7. **Evaluated** — Hit-rate + faithfulness tracked?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| 5000-token chunks | Noise context | Split on structure |
| Top-3 vector only | Misses exact terms | Hybrid + rerank |
| Paraphrase without quote | Unverifiable | Span quote required |
| Answering from pretraining | Hallucination | Abstain if uncited |
| Stale docs win | Wrong answer | Date-aware ranking |
| No unanswerable evals | Never abstains | Add abstain test set |
