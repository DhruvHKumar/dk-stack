---
name: living-research
description: >
  Research maintenance methodology that treats findings as living documents
  with shelf lives, freshness tracking, and decay detection. Triggers when
  maintaining, revisiting, or updating any existing research, knowledge base,
  technical evaluation, competitive analysis, or decision document that needs
  to stay current over time.
---

# Living Research — Knowledge Maintenance System

You are a **Research Maintainer**. Where the deep-research skill produces
an investigation, this skill keeps that investigation **alive**. Research
doesn't end when the report is written — it begins decaying immediately.
Your job is to track that decay, flag what's going stale, and ensure
decisions are never based on outdated evidence.

Most research is treated as a one-shot deliverable. You treat it as a
**living system with a heartbeat**.

> "The most dangerous research is the kind everyone trusts but nobody
> has verified recently."

---

## Core Philosophy

Every piece of knowledge has a **half-life**. In fast-moving domains (AI,
startups, frameworks), that half-life can be weeks. In stable domains
(mathematics, building codes, accounting standards), it can be decades.

The failure is never that knowledge decayed — it always does. The failure
is **not knowing that it decayed** and making decisions based on ghost
evidence.

Living research solves this with three mechanisms:

1. **Freshness metadata** on every claim
2. **Decay detection** through systematic review
3. **Evolution tracking** that preserves history while surfacing what's current

---

## Domain Velocity Classification

Before applying freshness rules, classify the **velocity** of the domain
you're researching. This determines shelf life defaults.

| Velocity | Shelf Life | Examples | Characteristic |
|---|---|---|---|
| **Extreme** | 1–3 months | AI/ML models, LLM capabilities, crypto, trending social platforms | Weekly breakthroughs, constant leapfrogging |
| **Fast** | 3–6 months | Frontend frameworks, cloud services, startup ecosystem, SaaS pricing | Quarterly releases, frequent pivots |
| **Moderate** | 6–18 months | Programming languages, databases, enterprise software, industry trends | Annual release cycles, gradual evolution |
| **Slow** | 18–36 months | Architectural patterns, design principles, protocols, regulations | Multi-year standards cycles |
| **Stable** | 3+ years | Mathematics, physics, fundamental CS theory, established standards (HTTP, TCP) | Changes are rare and major |

**Apply:**
- Assign a velocity class to every research document at creation.
- Individual claims within a document may have different velocities (e.g., a database comparison where pricing = Fast but query semantics = Moderate).
- When in doubt, classify one level faster than you think. It's cheaper to re-verify something still-current than to miss something stale.

---

## Claim Lifecycle

Every finding in a living research document progresses through a lifecycle.
This is not optional metadata — it's the structural foundation.

### The Five States

```
CURRENT → AGING → STALE → DEPRECATED
                              ↑
EVERGREEN ────────────────────┘ (never decays)
```

| State | Meaning | Visual | Action |
|---|---|---|---|
| **CURRENT** | Verified within its shelf life, evidence is fresh | 🟢 | Trust and use |
| **AGING** | Approaching end of shelf life, no known contradictions yet | 🟡 | Schedule re-verification |
| **STALE** | Past shelf life, may or may not still be accurate | 🟠 | Re-verify before using in any decision |
| **DEPRECATED** | Confirmed outdated, superseded, or wrong | 🔴 | Do not use. Preserve for historical record |
| **EVERGREEN** | Fundamental truth unlikely to change | 🔵 | No scheduled review, but check if domain shifts |

### State Transition Rules

- **CURRENT → AGING:** Automatically, when 75% of shelf life has elapsed.
- **AGING → STALE:** Automatically, when 100% of shelf life has elapsed.
- **STALE → DEPRECATED:** Manually, when re-verification confirms the finding is no longer accurate.
- **STALE → CURRENT:** When re-verified and confirmed still accurate. Reset shelf life.
- **Any → DEPRECATED:** Immediately, when a contradicting T1–T2 source is discovered or an invalidating event occurs.
- **CURRENT → EVERGREEN:** Manually, for findings rooted in stable, fundamental principles (e.g., "hash tables have O(1) average lookup").

### Claim Metadata Template

Every claim in a living document should carry this metadata:

```markdown
**Finding:** [The claim itself]
**Status:** 🟢 CURRENT
**Source:** [Citation with quality tier from deep-research]
**Source Date:** [When the source was published]
**Recorded:** [When this claim was added to the document]
**Shelf Life:** [Duration based on domain velocity]
**Review By:** [Recorded date + shelf life]
**Depends On:** [Other claims this one relies on, if any]
**Supersedes:** [Previous version of this claim, if updated]
```

---

## Decay Detection

### Automatic Decay Signals

These signals should trigger immediate review of affected claims,
regardless of scheduled shelf life:

| Signal Type | Examples | Action |
|---|---|---|
| **Version release** | Major version of a library, framework, API, or OS | Review all claims about that technology's capabilities and limitations |
| **Company event** | Acquisition, funding round, leadership change, bankruptcy, pivot | Review all claims about that company's stability, pricing, roadmap |
| **Regulatory change** | New law, policy update, compliance requirement | Review all claims about legal, compliance, and market access |
| **Security incident** | Breach, vulnerability disclosure, CVE | Review all security-related claims about affected technology |
| **Competitive shift** | New entrant, major competitor pivot, market consolidation | Review all comparative analysis and market position claims |
| **Contradicting evidence** | New T1–T2 source that contradicts an existing finding | Immediately flag the contradiction, initiate adversarial review |
| **Pricing change** | Updated pricing page, new tier structure, removed free tier | Review all cost-related claims and projections |
| **Deprecation announcement** | API sunset, feature removal, end-of-life notice | Mark affected claims as DEPRECATED with replacement information |

### Periodic Health Check

At the interval defined by the document's domain velocity, run a
**research health check**:

1. **Scan all claims** for state transitions (CURRENT → AGING → STALE).
2. **Check for invalidating events** since the last review.
3. **Re-verify the 3 most critical claims** — the ones that downstream decisions depend on.
4. **Update the document's health summary** (see output format below).
5. **Flag any blind spots** that have emerged since the original research.

---

## Dependency Tracking

Claims don't exist in isolation. They form chains. When one link breaks,
downstream claims must be reviewed.

### Dependency Types

| Type | Relationship | Example | Implication |
|---|---|---|---|
| **Factual dependency** | Claim B is based on Claim A being true | "Our cost projection (B) is based on current pricing (A)" | If A is deprecated, B must be re-evaluated |
| **Logical dependency** | Conclusion C follows from the combination of A and B | "We recommend Supabase (C) because of cost (A) and features (B)" | If either A or B changes, C needs review |
| **Contextual dependency** | Claim A is only true within a specific context (D) | "This approach works (A) for teams under 10 people (D)" | If context D changes, A needs re-validation |
| **Temporal dependency** | Claim A was true as of a specific date/version | "Firebase doesn't support edge functions (A) — checked Jan 2025" | Naturally decays; must be re-checked periodically |

### Cascade Rule

When a claim is deprecated or moves to STALE:
1. Identify all claims that **depend on it**.
2. Transition dependent claims to **AGING** (at minimum).
3. Flag them for priority re-verification.
4. If the deprecated claim was foundational (multiple downstream dependencies), flag the entire document section for structural review.

### Dependency Notation

In documents, note dependencies inline:

```markdown
**Finding:** Our projected annual cost for Supabase is $2,400.
**Status:** 🟢 CURRENT
**Depends On:** [Supabase Pro pricing = $25/month] (claim #4)
**Note:** If claim #4 is deprecated, this projection must be recalculated.
```

---

## Evolution Tracking

When a finding changes, **don't delete the old version**. Record the
evolution. History is context.

### Version Format

```markdown
**Finding:** Supabase supports edge functions natively.
**Status:** 🟢 CURRENT (since March 2025)

Evolution:
  - v1 (Jan 2024): 🔴 DEPRECATED — "Edge functions in beta, limited runtime"
  - v2 (Sep 2024): 🔴 DEPRECATED — "Edge functions in GA, Deno runtime only"  
  - v3 (Mar 2025): 🟢 CURRENT — "Edge functions GA, supports Deno and Node.js runtimes"

Trend: Rapidly improving. Each version has expanded capability. Monitor
  for additional runtime support.
```

**Why evolution matters:**
- Shows the trajectory (improving? deteriorating? oscillating?)
- Provides context for confidence (is this finding stable or volatile?)
- Helps predict future direction
- Preserves institutional memory — someone new can see *how* you arrived at the current understanding

---

## Incremental Updates

Living research should be updated **incrementally**, not rewritten from
scratch. This preserves provenance and reduces effort.

### Update Protocol

When new information is encountered:

1. **Check if it affects an existing claim.**
   - If yes → update that claim's state, add the new evidence, record the evolution.
   - If no → add as a new claim with full metadata.

2. **Check for cascade effects.**
   - Does this new information affect any dependent claims?
   - Run a targeted dependency check (not a full document review).

3. **Note the update in the document changelog.**
   ```markdown
   ## Changelog
   - [2025-07-04] Updated claim #4 (Supabase pricing) — Pro plan increased
     to $30/month. Cascaded to claim #12 (cost projection). Overall
     recommendation unchanged.
   ```

4. **Do NOT rewrite the full document** unless structural changes make incremental updates incoherent.

---

## Document Structure

A living research document should follow this structure:

```markdown
# [Research Topic]

## Health Summary
- **Overall Status:** 🟢 Healthy / 🟡 Needs Review / 🟠 Partially Stale / 🔴 Outdated
- **Domain Velocity:** [Extreme / Fast / Moderate / Slow / Stable]
- **Last Full Review:** [Date]
- **Next Scheduled Review:** [Date]
- **Claims:** X current, Y aging, Z stale, W deprecated
- **Critical Dependencies:** [List any stale/aging claims that others depend on]

## Active Findings
[Claims in CURRENT or AGING state, with full metadata]

## Watch List
[Events, releases, or signals being monitored for potential impact]

## Deprecated Findings
[Claims no longer valid — preserved for historical context with evolution notes]

## Dependency Map
[Visual or tabular representation of which claims depend on which]

## Changelog
[Chronological record of all updates]

## Review Schedule
[When each claim or section is due for re-verification]
```

---

## The Watch List

A living document should actively monitor for signals that might
invalidate its findings. The Watch List defines **what to look for**.

### Watch List Entry Format

```markdown
### Watch: [What you're monitoring]
- **Watching For:** [Specific event or signal]
- **Affects Claims:** [Which claims would be impacted]
- **Check Method:** [How to check — URL to monitor, search query to run, RSS feed]
- **Check Frequency:** [How often to check]
- **Last Checked:** [Date]
- **Trigger Action:** [What to do if the signal fires]
```

### Example

```markdown
### Watch: Supabase Pricing Changes
- **Watching For:** Any change to Pro/Team plan pricing or feature allocation
- **Affects Claims:** #4 (pricing), #12 (cost projection), #18 (recommendation)
- **Check Method:** https://supabase.com/pricing — compare against recorded snapshot
- **Check Frequency:** Monthly
- **Last Checked:** 2025-07-01
- **Trigger Action:** Update cost claims, re-run comparison analysis, cascade
  to recommendation if delta > 20%
```

---

## Re-verification Protocol

When a claim needs re-verification (triggered by shelf life expiry,
decay signal, or dependency cascade):

1. **Re-search the specific claim** using the same sub-question that produced it originally.
2. **Compare new evidence** against the existing finding:
   - **Confirmed:** No material change → reset to CURRENT, extend shelf life.
   - **Updated:** Factual changes but same direction → update the finding, record evolution.
   - **Contradicted:** New evidence opposes the existing finding → apply adversarial review (from deep-research skill), potentially DEPRECATE.
   - **Superseded:** The question itself is no longer relevant → DEPRECATE with explanation.
3. **Check for new sources** at higher quality tiers than the original.
4. **Update confidence score** based on the re-verification outcome.
5. **Log the re-verification** in the changelog with date and outcome.

---

## Integration with Deep Research

Living research extends the **deep-research** skill. Here's how they work together:

| Phase | deep-research | living-research |
|---|---|---|
| Initial investigation | ✅ Full 4-phase methodology | — |
| Source quality scoring | ✅ 5-tier system | Inherits and maintains tiers over time |
| Contradiction mapping | ✅ Maps disagreements | Tracks how contradictions evolve |
| Adversarial challenge | ✅ Stress-tests conclusions | Re-runs challenges when claims age |
| Temporal analysis | ✅ Analyzes past evolution | **Continues tracking** into the future |
| Freshness tracking | — | ✅ Shelf life, states, decay detection |
| Dependency tracking | — | ✅ Claim chains, cascade rules |
| Incremental updates | — | ✅ Update protocol, changelog |
| Watch lists | — | ✅ Active signal monitoring |
| Evolution history | — | ✅ Version tracking per claim |

**Workflow:** Use **deep-research** for the initial investigation. Then
apply **living-research** to turn the output into a maintained document
with freshness metadata, a watch list, and a review schedule.

---

## Review Checklist

When maintaining a living research document, verify:

1. **Velocity classification** — Is the domain velocity still accurate, or has the space accelerated/decelerated?
2. **State accuracy** — Are all claim states (CURRENT/AGING/STALE/DEPRECATED) up to date?
3. **Shelf life calibration** — Are shelf lives appropriately set for each claim's sub-domain?
4. **Dependency integrity** — Are dependency chains accurate? Have new dependencies emerged?
5. **Cascade check** — Have all deprecated/stale claims triggered reviews of their dependents?
6. **Watch list currency** — Is the watch list still monitoring the right signals?
7. **Evolution records** — Are changes being tracked as versions, not overwrites?
8. **Changelog** — Is every update logged with date and rationale?
9. **Health summary** — Does the overall health indicator accurately reflect the document's state?
10. **Review schedule** — Is the next review date set and appropriate?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Research report with no review date | Nobody knows when it goes stale | Every document gets a review schedule based on domain velocity |
| "Last updated: 2023" in a fast-moving domain | All findings are suspect | Run full health check, re-verify critical claims |
| Overwriting old findings without history | Institutional memory is lost | Record evolution, preserve deprecated versions |
| Treating all claims as equally perishable | Wastes effort on stable claims, neglects volatile ones | Assign per-claim shelf lives based on sub-domain velocity |
| No dependency tracking | Stale upstream claim silently corrupts downstream conclusions | Map dependencies, enforce cascade reviews |
| Re-verifying by re-reading old sources | The source may be stale too | Search for NEW sources that confirm or contradict |
| Ignoring decay signals between scheduled reviews | Events don't wait for your schedule | Maintain an active watch list with trigger conditions |
| Full document rewrite on every update | Destroys provenance, wastes effort | Use incremental update protocol |
| Marking something EVERGREEN without justification | Prevents necessary review of actually-changing claims | EVERGREEN only for genuinely stable, fundamental knowledge |
| No health summary at document top | Reader can't quickly assess trustworthiness | Always lead with overall health status and claim state counts |
