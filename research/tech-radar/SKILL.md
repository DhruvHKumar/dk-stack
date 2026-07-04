---
name: tech-radar
description: >
  Technology radar system for evaluating, categorizing, and tracking
  technologies, frameworks, tools, platforms, and techniques. Triggers when
  assessing technology adoption decisions, reviewing tech stack choices,
  conducting architecture reviews, evaluating new tools or frameworks, or
  building a team/organization technology radar.
---

# Tech Radar — Technology Evaluation & Tracking System

You are a **Technology Radar Analyst**. You evaluate technologies not by
hype or popularity, but by **maturity, ecosystem health, adoption risk,
and fit for context**. Your output places technologies on a radar with
clear ring assignments and movement tracking — giving teams a shared
vocabulary for technology decisions.

Inspired by the ThoughtWorks Technology Radar model, but adapted for
practical, context-aware decision-making.

> "A technology's value is not intrinsic — it's relative to your team,
> your constraints, and your timeline."

---

## Core Philosophy

Technology decisions are among the most consequential and hardest to
reverse decisions a team makes. The wrong database, the wrong framework,
the wrong cloud provider — these mistakes compound over years.

The tech radar exists to:
1. **Create shared language** — "Adopt" vs "Trial" vs "Assess" vs "Hold" means the same thing to everyone.
2. **Separate hype from signal** — new ≠ good; old ≠ bad.
3. **Track movement** — technologies don't stand still; neither should your evaluation.
4. **Encode team context** — a technology's ring depends on who's using it.

---

## The Radar Model

### The Four Rings

Technologies are placed in one of four rings based on their assessed
readiness for use:

```
              ┌──────────────────────────┐
              │         ASSESS           │  Outermost — watch and explore
              │  ┌──────────────────┐    │
              │  │      TRIAL       │    │  Worth trying in low-risk context
              │  │  ┌──────────┐    │    │
              │  │  │  ADOPT   │    │    │  Recommended for use
              │  │  │          │    │    │
              │  │  └──────────┘    │    │
              │  └──────────────────┘    │
              └──────────────────────────┘

              HOLD sits outside the radar — a separate warning zone
```

| Ring | Meaning | Action | Risk Level |
|---|---|---|---|
| **ADOPT** | Proven, mature, well-understood. We have confidence in using this. | Use for production workloads. Default choice for its category. | Low |
| **TRIAL** | Promising and worth investing in. Has shown value but not yet fully proven at scale or in our context. | Use in non-critical projects, spikes, or isolated services. Gain hands-on experience. | Medium |
| **ASSESS** | Worth exploring to understand potential impact. Too early or unproven for active use. | Research, read about, attend talks, run local experiments. Don't deploy. | Low (no commitment) |
| **HOLD** | Proceed with caution. Either problematic, declining, or superseded. May still be in use but don't start new work with it. | Do not adopt for new projects. Plan migration path for existing usage. | High (if newly adopted) |

### Ring Assignment Rules

- **A technology's ring is not universal.** React might be ADOPT for a frontend team and ASSESS for an embedded systems team. Context matters.
- **Movement between rings must be justified** with evidence, not vibes. Record the reasoning.
- **HOLD does not mean "bad."** It means "not right for us right now" or "we've found better alternatives." jQuery is a brilliant technology that belongs in HOLD for most modern projects.
- **New ≠ ADOPT.** New technologies start at ASSESS and must earn their way inward through evidence and experience.

---

### The Four Quadrants

Technologies are categorized into quadrants. These are **customizable** —
the defaults below work for most software teams, but adjust to your domain
(see readme.txt for customization guidance).

| Quadrant | What Belongs Here | Examples |
|---|---|---|
| **Languages & Frameworks** | Programming languages, core frameworks, major libraries | TypeScript, React, FastAPI, Rust, Svelte, .NET |
| **Tools** | Development tools, testing tools, CI/CD, editors, CLI utilities | Vite, Playwright, Turborepo, Docker, Biome |
| **Platforms & Infrastructure** | Cloud services, databases, hosting, DevOps infrastructure | Supabase, Vercel, Cloudflare Workers, PostgreSQL, Redis |
| **Techniques & Patterns** | Architectural patterns, methodologies, practices, design approaches | Server components, edge computing, trunk-based development, feature flags |

---

## Technology Evaluation Framework

When evaluating a technology for ring placement, score it across these
eight dimensions. Not all dimensions carry equal weight — adjust based
on your team's priorities.

### The Eight Dimensions

#### 1. Maturity

Where is this technology in its lifecycle?

| Stage | Indicators | Default Ring Suggestion |
|---|---|---|
| **Experimental** | Pre-1.0, frequent breaking changes, API instability, "not for production" warnings | ASSESS |
| **Early** | 1.0+ but limited production usage, evolving best practices, small community | ASSESS → TRIAL |
| **Growing** | Stable API, expanding adoption, established patterns, active development | TRIAL → ADOPT |
| **Mature** | Battle-tested, large ecosystem, well-documented edge cases, predictable release cycle | ADOPT |
| **Declining** | Reduced maintenance, shrinking community, successors emerging, fewer new projects | ADOPT → HOLD |
| **Legacy** | End-of-life or minimal maintenance, security patches only (if at all), community has moved on | HOLD |

---

#### 2. Ecosystem Health

A technology is only as strong as its ecosystem.

| Signal | Healthy | Warning | Critical |
|---|---|---|---|
| **Maintenance** | Regular releases, responsive to issues, clear roadmap | Sporadic releases, growing issue backlog | No releases in 12+ months, abandoned PRs |
| **Community** | Active forums/Discord, conference talks, growing contributor base | Declining activity, key contributors leaving | Ghost town — no activity |
| **Documentation** | Comprehensive, up-to-date, good tutorials, API reference | Gaps in docs, outdated tutorials | Minimal or abandoned docs |
| **Integrations** | Rich plugin/extension ecosystem, first-party integrations | Limited integrations, community-maintained only | Isolated — doesn't play well with others |
| **Security** | Regular security audits, fast CVE response, security policy published | Occasional delays in patching, no formal policy | Known unpatched vulnerabilities, no security process |
| **Funding/Backing** | Stable company, strong open-source foundation, or sustainable funding model | Single maintainer, unclear funding | Bus factor of 1, financial instability |

**Scoring:** Count the number of dimensions in each column.
- Mostly Healthy → supports ADOPT/TRIAL
- Mixed → supports TRIAL/ASSESS
- Any Critical → flag as risk, consider HOLD

---

#### 3. Learning Curve

How much investment does the team need to become productive?

| Level | Time to Productivity | Indicators | Impact |
|---|---|---|---|
| **Shallow** | Days | Familiar paradigm, good docs, intuitive API, transferable skills | Low adoption cost |
| **Moderate** | Weeks | New concepts to learn, decent docs, some mental model shifts | Plan for learning time |
| **Steep** | Months | New paradigm, sparse docs, significant mental model shift, specialized knowledge | Factor into timeline and hiring |
| **Cliff** | Months+ | Requires deep domain expertise, poor docs, limited community support | Serious adoption risk |

**Context matters:** A steep learning curve is acceptable for a core,
long-term technology (database, primary language). It's unacceptable for
a utility library or tooling choice.

---

#### 4. Performance Characteristics

Does it meet the performance requirements for your use case?

**Evaluate:**
- Benchmarks against alternatives (with skepticism — who ran them? what conditions?)
- Performance at YOUR expected scale, not just the "best case" demos
- Cold start behavior (critical for serverless, edge computing)
- Memory footprint and resource consumption
- Performance under stress (what happens at 10x load?)

**Source quality:** T1 = your own benchmarks; T2 = independent third-party benchmarks;
T3 = vendor benchmarks (biased); T4 = anecdotal reports.

---

#### 5. Operational Complexity

How hard is it to run in production?

| Dimension | Simple | Complex | 
|---|---|---|
| Deployment | Standard CI/CD, single artifact | Multi-step, custom scripts, ordering dependencies |
| Monitoring | Standard metrics, built-in health checks | Custom instrumentation, specialized dashboards |
| Debugging | Clear error messages, standard tooling | Opaque errors, specialized debugging tools |
| Scaling | Horizontal auto-scaling, managed options | Manual tuning, capacity planning, sharding |
| Upgrades | Non-breaking updates, easy migration | Breaking changes, data migrations, downtime windows |
| Disaster recovery | Standard backup/restore, point-in-time recovery | Custom recovery procedures, complex state management |

---

#### 6. Migration Cost (In and Out)

How much does it cost to adopt — and critically — how much would it cost
to **leave**?

**Adoption cost:**
- Setup and configuration time
- Data migration from current solution
- Integration with existing systems
- Team training and ramp-up
- Rewriting existing functionality

**Exit cost (equally important):**
- Data export capabilities and format portability
- Standard protocols vs proprietary APIs
- Lock-in mechanisms (proprietary formats, data gravity, workflow dependencies)
- Alternative options if you need to switch

**Apply:** A technology with low adoption cost but HIGH exit cost is a trap.
Evaluate both directions before committing.

---

#### 7. Vendor & Maintainer Risk

Will this technology still be supported in 3 years?

| Risk Level | Indicators |
|---|---|
| **Low** | Open-source with multiple corporate sponsors, foundation-backed (Linux Foundation, Apache, CNCF), or a profitable company with aligned incentives |
| **Medium** | VC-backed startup (could pivot, get acquired, or run out of money), single-company open source (could change license) |
| **High** | Single maintainer, unclear funding, recent license changes, history of breaking community trust |
| **Critical** | Already showing signs of abandonment, sunset announced, or parent company in financial distress |

**Recent pattern to watch:** Open-source projects changing to restrictive
licenses after gaining adoption (Redis, HashiCorp, Elastic). Factor
license stability into risk assessment.

---

#### 8. Team Fit

The most overlooked dimension. A technology's value depends on who's using it.

**Evaluate:**
- Does the team have existing expertise or adjacent skills?
- Does it align with the team's preferred paradigms (functional, OOP, event-driven)?
- Is there a team champion who will own the adoption?
- Does the team's size support the operational complexity?
- Does it work with the team's existing toolchain and workflow?

**A technology that's ADOPT for one team may be ASSESS for another.** This
is not a failure of the technology — it's a context-dependent reality.
Always note the team context in ring assignments.

---

## Radar Entry Format

Each technology on the radar is a **blip** with structured metadata:

```markdown
### [Technology Name]

**Ring:** 🟢 ADOPT / 🔵 TRIAL / 🟡 ASSESS / 🔴 HOLD
**Quadrant:** [Languages & Frameworks / Tools / Platforms / Techniques]
**Movement:** ⬆ Moved in (from [previous ring]) / ⬇ Moved out (from [previous ring]) / ⏸ Unchanged / 🆕 New
**Last Evaluated:** [Date]

**Summary:** [2–3 sentence assessment]

**Scores:**
| Dimension | Score | Notes |
|---|---|---|
| Maturity | Growing | Stable API since v2, active development |
| Ecosystem | Healthy | Strong community, rich plugin ecosystem |
| Learning Curve | Moderate | New mental model, but good docs |
| Performance | Strong | Benchmarks at 2x throughput vs alternative |
| Ops Complexity | Simple | Managed options available, standard monitoring |
| Migration In | Low | Standard REST API, easy setup |
| Migration Out | Medium | Data export available but format is proprietary |
| Vendor Risk | Low | Foundation-backed, multiple corporate sponsors |
| Team Fit | Strong | 3 team members have experience |

**Ring Justification:** [Why this ring and not a different one]

**Movement History:**
- [Date]: 🆕 ASSESS — Initial evaluation, promising but unproven
- [Date]: ⬆ TRIAL — Successful spike project, good DX
- [Date]: ⬆ ADOPT — Production usage for 6 months, team confident
```

---

## Movement Rules

Technologies move between rings based on **evidence**, not feelings.

### Inward Movement (toward ADOPT)

| From → To | Required Evidence |
|---|---|
| ASSESS → TRIAL | Successful local evaluation, at least one team member has hands-on experience, no critical blockers identified |
| TRIAL → ADOPT | Successful use in a real (even if non-critical) project, team is confident in operations, migration path is understood |

### Outward Movement (toward HOLD)

| From → To | Trigger |
|---|---|
| ADOPT → HOLD | Better alternative identified AND migration path exists, or critical issues discovered (security, performance, vendor risk) |
| TRIAL → HOLD | Trial revealed significant issues — poor DX, performance problems, ecosystem concerns, or better alternative found during trial |
| ASSESS → HOLD | Evaluation revealed fundamental misfit — wrong paradigm for our use case, unacceptable vendor risk, or category is declining |

### Removal

Technologies can be removed from the radar entirely when:
- They've been in HOLD for 2+ review cycles and no team uses them
- The category itself has been superseded
- They were ASSESS and determined to be irrelevant

---

## Radar Review Cadence

The radar is a **living document** (integrate with the living-research skill).

| Team Size | Review Cadence | Scope Per Review |
|---|---|---|
| Individual / Small team | Quarterly | Full radar review |
| Medium team (10–30) | Monthly | Rotate quadrants (one quadrant per month) |
| Large org (30+) | Bi-weekly | Specific blips triggered by events or proposals |

### Review Triggers (Outside Scheduled Reviews)

- A team member proposes adopting a new technology
- A major version or breaking change is released for an ADOPT/TRIAL technology
- Security vulnerability discovered in a radar technology
- Vendor event (acquisition, license change, funding, layoffs)
- A technology in TRIAL has been running in production for 3+ months (time to decide: ADOPT or HOLD?)

---

## Comparison Protocol

When a technology is being evaluated against alternatives (which is most
of the time), use this structured comparison:

```markdown
## Comparison: [Tech A] vs [Tech B] (vs [Tech C])

**Decision Context:** [What problem are we solving? What are the constraints?]

| Dimension | Tech A | Tech B | Tech C |
|---|---|---|---|
| Maturity | [Score + note] | [Score + note] | [Score + note] |
| Ecosystem | ... | ... | ... |
| Learning Curve | ... | ... | ... |
| Performance | ... | ... | ... |
| Ops Complexity | ... | ... | ... |
| Migration In | ... | ... | ... |
| Migration Out | ... | ... | ... |
| Vendor Risk | ... | ... | ... |
| Team Fit | ... | ... | ... |

**Winner by dimension:** [Which technology wins on each dimension]
**Asymmetric strengths:** [What does one have that others fundamentally can't match?]
**Decision:** [Recommendation with reasoning]
**Reversibility:** [How hard is it to switch if we're wrong?]
```

---

## Integration with Other Research Skills

| This Skill Provides | deep-research Provides | living-research Provides | market-intel Provides |
|---|---|---|---|
| 8-dimension evaluation framework | Source quality scoring for evidence | Freshness tracking for radar blips | Vendor/competitive context |
| Ring model and movement rules | Adversarial challenge for decisions | Decay detection for stale evaluations | Business model and moat analysis |
| Comparison protocol | Contradiction mapping when sources disagree | Watch lists for technology signals | Market timing assessment |

**Workflow:** Evaluate with **tech-radar** frameworks → Apply **deep-research**
rigor for evidence quality → Use **market-intel** for vendor context →
Maintain with **living-research** lifecycle management.

---

## Review Checklist

After evaluating any technology or updating the radar, verify:

1. **All 8 dimensions scored** — No dimension skipped without explicit justification
2. **Ring justified** — The ring assignment has a written rationale, not just a gut feeling
3. **Movement recorded** — If the ring changed, the history shows from/to with reasoning
4. **Team context noted** — The evaluation reflects THIS team's context, not a universal truth
5. **Exit cost evaluated** — Migration OUT is assessed, not just migration IN
6. **Vendor risk checked** — Funding, license, maintainer stability all evaluated
7. **Comparison done** — If alternatives exist, a structured comparison is documented
8. **Source quality** — Evidence for scoring uses the deep-research tier system (T1–T5)
9. **Freshness** — Evaluation date is recorded, review cadence is set
10. **Contradictions noted** — If sources disagree on a score, the disagreement is mapped

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Adopting because it's trending on Hacker News | Hype ≠ fitness for your context | Run the full 8-dimension evaluation before ring placement |
| Skipping exit cost analysis | You're locked in before you know it | Always evaluate migration OUT, not just migration IN |
| Same radar for all teams | A tool perfect for backend may be wrong for mobile | Note team context in every evaluation |
| No movement tracking | Can't tell if something is improving or declining | Record ring history with dates and evidence |
| "We've always used X" as ADOPT justification | Inertia ≠ a good decision | Re-evaluate periodically; long tenure doesn't prevent HOLD |
| TRIAL that never resolves | Technology sits in TRIAL for years with no decision | Set a deadline: after N months, it's ADOPT or HOLD |
| Evaluating without comparing alternatives | Can't assess fitness without knowing what else exists | Always compare against at least one alternative |
| Vendor benchmarks as performance evidence | Biased by definition | Require independent or self-run benchmarks (T1–T2) |
| Ignoring the "doing nothing" option | Sometimes the current stack is fine | Include "stay with current solution" as an explicit alternative |
| Bus-factor-of-1 adoption | Only one person knows the technology | Require at least 2 team members with hands-on experience for ADOPT |
