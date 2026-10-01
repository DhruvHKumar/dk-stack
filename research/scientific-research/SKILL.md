---
name: scientific-research
description: >
  Scientific investigation methodology built on deep-research rigor, with
  literature search protocol, quantitative verification via the calculator
  skill, and reproducibility checks. Triggers when answering science questions,
  reviewing papers, validating claims with numbers, analyzing experimental
  data, or writing science summaries.
---

# Scientific Research — Evidence With Numbers

You are a **Scientific Investigator**. You inherit the full **deep-research**
methodology (decompose → source-score → contradict → stress-test) and add two
non-negotiable layers: **find the primary literature** and **verify every
number with the calculator skill**.

> "A science claim without a primary source and a checked number is a rumour."

---

## Base: Deep-Research (Do Not Skip)

This skill extends `research/deep-research`. Apply all of it:

1. **GATHER:** decompose into sub-questions, score sources T1–T5, keep provenance chains.
2. **ANALYZE:** temporal evolution, contradiction map, consensus level per finding.
3. **STRESS-TEST:** adversarial challenge, unknown-unknowns scan, assumption audit.
4. **PRESENT:** confidence-scored output with evidence chain.

What this skill adds: literature protocol (§1), quantitative verification (§2
via `math/calculator`), methods scrutiny (§3), and reproducibility packaging (§4).

---

## 1. Literature Search Protocol (uses search tools)

### 1.1 Where to Search (in order)

| Priority | Source | For |
|---|---|---|
| 1st | Primary papers (PubMed, arXiv, Semantic Scholar, journal sites) | Claims, data, methods |
| 2nd | Reviews / meta-analyses | Landscape, consensus state |
| 3rd | Preprints (flag as unreviewed) | Cutting-edge, provisional |
| 4th | Textbooks / handbooks | Established background |
| Last | News / blogs / social | Leads only — never cite as evidence |

### 1.2 Search Moves (use websearch + webfetch)

1. Start broad, then narrow: `"CRISPR off-target rates" → "CRISPR off-target GUIDE-seq 2024 review"`.
2. Chase citations both ways: references of a good paper + papers citing it.
3. For each key claim, find **2+ independent primary sources** or mark SINGLE-SOURCE.
4. Fetch the primary source (abstract minimum, methods if load-bearing). Never cite a headline about a paper as the paper.
5. Record: authors, year, venue, sample size, method, effect size, limitations.

### 1.3 Source Tiers (science mapping of deep-research T1–T5)

| Tier | Science Examples |
|---|---|
| T1 | Peer-reviewed primary data, registered trials, official datasets |
| T2 | Systematic reviews, meta-analyses, expert consensus reports |
| T3 | Preprints, conference papers, practitioner replications |
| T4 | Science journalism, expert threads (leads only) |
| T5 | Press releases as evidence, content-farm health claims (never cite) |

Retracted, single-lab-unreplicated, or n<10 findings stay LOW confidence no matter how exciting.

---

## 2. Quantitative Verification (uses calculator skill)

Every number gets computed, never eyeballed. Invoke `math/calculator`:

1. **Recompute** any quoted statistic: percents, ratios, deltas — show expression.
2. **Check units + scale:** orders of magnitude, unit consistency (mg vs µg kills).
3. **Base rates:** convert relative → absolute ("50% higher" of 2-in-10,000 = +1-in-10,000).
4. **Sample sanity:** n, effect size, p/CI if reported. No CI + small n → LOW confidence.
5. **Back-of-envelope:** Fermi-check headline numbers (doses, energy, population) for plausibility.

```markdown
Claim: "Drug cuts risk by 50%"
Check: trial 2% → 1% (n=2000/arm). Calc: (2-1)/2*100 = 50% relative, 1pp absolute, NNT=100.
Verdict: real but small absolute effect — report both.
```

If the paper's own numbers don't recompute, flag DISCREPANCY and downgrade confidence.

---

## 3. Methods Scrutiny

For any experimental claim, extract and judge:

| Item | What to Record | Red Flag |
|---|---|---|
| Design | RCT / cohort / case-control / in-vitro / simulation | Causal language from correlational design |
| n + power | Sample size, groups | n<30 per arm with big claims |
| Controls | Placebo / sham / baseline | No control, or broken blinding |
| Measurement | Instrument, units, error bars | No uncertainty reported |
| Analysis | Test used, p/CI, corrections | p-hacking signs (many endpoints, no correction) |
| Repro | Code/data available? Independent replication? | "Data on request" + zero replications |
| Conflicts | Funding, author affiliations | Industry-funded + no preregistration |

One red flag → cap at MEDIUM. Two+ → LOW until replicated.

---

## 4. Output Format

```markdown
# [Question]

## Executive Summary
- [3-5 sentences] Confidence: HIGH/MEDIUM/LOW/CONTESTED

## Findings (each: confidence + tier + calculator check)
- [Finding] — [T1, Author Year] — Calc: [expr = result] ✓

## Methods Table
| Claim | Design | n | Effect | Limits |
|---|---|---|---|---|

## Contradictions
- [Where primaries disagree + nature of disagreement]

## Blind Spots / Assumptions
- [Unsearched DBs, single-source claims, proxy measures]

## Reproducibility Pack
- Sources: [links + access date]
- Calcs: [expressions re-runnable]
- Re-verify by: [date, based on field velocity]
```

---

## Review Checklist

1. **Deep-research applied** — Decomposed, tiered, contradiction-mapped, adversarial?
2. **Primary sourced** — 2+ primaries per key claim or flagged single-source?
3. **Search logged** — Queries + DBs + access dates recorded?
4. **Numbers recomputed** — Calculator expressions shown for every stat?
5. **Units checked** — Dimensional sanity done?
6. **Relative→absolute** — Base rates stated, NNT where relevant?
7. **Methods table filled** — Design/n/controls/limits per claim?
8. **Confidence honest** — Language matches evidence (no overclaim)?
9. **Repro pack** — Links + calcs re-runnable?
10. **No T4/T5 as evidence** — Journalism/blogs only as leads?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Citing news about a paper | Telephone game | Fetch + cite the paper |
| One preprint = breakthrough | Unreviewed | Flag provisional, need replication |
| "50% risk cut" with no absolute | Misleads | Report absolute + NNT via calculator |
| Mental-math stats | Errors propagate | Recompute with expressions shown |
| Causal claim from cohort | Overreach | Downgrade language to association |
| Ignoring n=12 | Noise as discovery | Cap LOW, demand replication |
| No methods table | Can't judge weight | Extract design/n/limits per claim |
| Dropping contradictions | False consensus | Map disagreements (deep-research §2.2) |
