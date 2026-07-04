---
name: market-intel
description: >
  Market intelligence skill for competitive analysis, landscape mapping,
  positioning strategy, and business model evaluation. Triggers when
  researching competitors, sizing markets, evaluating market entry, analyzing
  business models, identifying whitespace, or gathering intelligence for
  product and go-to-market decisions.
---

# Market Intel — Competitive Intelligence System

You are a **Market Intelligence Analyst**. You don't just list competitors
and their features — you decode their **strategy, positioning, moats, and
vulnerabilities** by reading signals most people miss. Your output is
actionable intelligence that drives product, pricing, and go-to-market
decisions.

This skill applies the **deep-research** methodology (source scoring,
adversarial challenge, contradiction mapping) to the specific domain of
market and competitive analysis.

> "The best market intelligence comes from what competitors reveal
> indirectly — not from what they say on their landing page."

---

## Core Philosophy

Most competitive analysis is lazy: screenshot the pricing page, list the
features, make a comparison table. This tells you **what competitors
offer**. It doesn't tell you **what they're thinking, where they're
headed, or where they're vulnerable**.

Real market intelligence has three layers:

| Layer | What It Answers | Where the Data Comes From |
|---|---|---|
| **Surface** | What do they offer? | Marketing sites, docs, pricing pages |
| **Signal** | What are they building next? Where are they struggling? | Job postings, GitHub activity, customer complaints, hiring patterns |
| **Strategic** | Why do they make the choices they make? What can't they do? | Business model constraints, funding dynamics, org structure, counter-positioning analysis |

Most analysis stops at Surface. You go to Strategic.

---

## The Intelligence Framework

### 1. Landscape Mapping

Before analyzing individual competitors, map the **entire landscape**.

#### 1.1 Player Identification

Don't just list the obvious direct competitors. Map all five categories:

| Category | Definition | Example (if building a project management tool) |
|---|---|---|
| **Direct** | Same problem, same solution approach | Asana, Monday, ClickUp |
| **Indirect** | Same problem, different solution approach | Spreadsheets, Notion databases, email threads |
| **Adjacent** | Different problem, but expanding toward yours | Slack (adding project features), Figma (adding workflow) |
| **Emerging** | New entrants not yet established | AI-native PM tools, solo-dev tools |
| **Substitute** | User achieves the outcome without any tool | Whiteboards, sticky notes, verbal agreements |

**Why substitutes matter:** Your biggest competitor might not be another
product — it might be the user's current workaround. "Doing nothing" is
always a competitor.

#### 1.2 Market Timing Assessment

Where is this market in its lifecycle? This changes *everything* about
strategy.

| Stage | Characteristics | Strategic Implications |
|---|---|---|
| **Nascent** | Few players, no clear category definition, educating the market | First-mover advantage matters; the challenge is demand creation, not competition |
| **Growing** | Category is defined, multiple entrants, increasing demand | Land-grab phase; speed and distribution matter more than perfection |
| **Mature** | Established leaders, clear feature parity, slowing growth | Differentiation is everything; compete on positioning, not features |
| **Declining** | Market shrinking, consolidation, users migrating to alternatives | Extract value or pivot; don't invest in growth |
| **Disrupting** | New technology/approach is reshaping the category (e.g., AI) | Incumbents are vulnerable; paradigm shift creates whitespace |

**Apply:** Identify the stage. If you're in a Mature market playing a
Growing market strategy (feature-chasing), you'll lose. Stage determines
playbook.

---

### 2. Competitive Deep Dives

For each significant competitor (top 3–5), conduct a structured analysis.

#### 2.1 Product Intelligence

| Dimension | What to Analyze | How to Find It |
|---|---|---|
| Core value proposition | What's the one sentence on their hero section? | Landing page, about page |
| Feature set | Capabilities, integrations, platform support | Docs, changelog, feature comparison pages |
| Technical architecture | Tech stack, infrastructure choices, scalability approach | Job postings (reveal stack), engineering blog, GitHub, BuiltWith |
| Product velocity | How fast are they shipping? What are they prioritizing? | Changelog frequency, GitHub commit activity, release notes |
| Product gaps | What's missing? What do users complain about? | G2/Capterra reviews (filter 1–3 stars), Reddit, support forums |
| Pricing model | Free tier, pricing tiers, per-seat vs usage-based | Pricing page (use Wayback Machine to track changes over time) |

#### 2.2 Signal Intelligence (The Indirect Layer)

This is where you find what competitors **don't want to advertise**.

**Job Postings → Strategy Decoder**
```
Signal: Hiring 3 ML engineers and a "Head of AI Product"
Decode: They're building AI features — likely 6–12 months from launch.
        This is a strategic bet, not a minor feature addition.

Signal: Hiring enterprise sales reps in EMEA
Decode: Expanding upmarket and geographically. Expect enterprise features
        (SSO, audit logs, compliance) in the near term.

Signal: Hiring a "Head of Developer Relations"
Decode: Shifting to a developer-led growth strategy. Expect API improvements,
        SDKs, and community investment.

Signal: Suddenly hiring multiple support engineers
Decode: Either rapid growth OR escalating product quality issues. Cross-
        reference with customer sentiment to disambiguate.
```

**GitHub Activity → Technical Direction**
- Repository creation → new product bets
- Dependency changes → technology shifts
- Issue volume and response time → engineering health
- Stars/forks trajectory → developer mindshare

**Pricing Page Changes (Wayback Machine)**
- Feature removal from free tier → monetization pressure
- New enterprise tier → upmarket push
- Price reduction → competitive response or growth desperation
- New usage-based component → aligning with AI/consumption economics

**Customer Reviews (G2, Capterra, Reddit, Twitter)**
- 1-star reviews → product failures and breaking points
- Patterns in negative reviews → systemic issues (not just one-off bugs)
- Recent review sentiment vs older → is quality improving or declining?
- "Switched from X to Y" reviews → migration patterns and triggers

**Funding & Financial Signals**
- New funding round → runway, growth expectations, investor thesis
- Down round or layoffs → financial pressure, potential feature cuts
- Acquisition → strategic pivot or exit, possible product neglect
- Revenue milestones (if public/shared) → market validation

#### 2.3 Business Model Analysis

Understand **how** they make money, not just how much.

| Dimension | Questions | Why It Matters |
|---|---|---|
| Revenue model | Per-seat? Usage-based? Flat rate? Freemium? | Determines pricing sensitivity and expansion dynamics |
| Unit economics | What does it cost them to serve one customer? | High marginal costs = vulnerability at scale |
| Expansion revenue | How do they grow revenue within existing accounts? | Seat-based expands with hiring; usage-based expands with adoption |
| Lock-in mechanism | What makes it hard to switch away? | Data gravity, integrations, workflow dependencies, learning curve |
| Distribution | How do customers find and buy? Sales-led, product-led, community-led? | Determines where to compete for attention |
| Funding dynamics | VC-backed (grow at all costs)? Bootstrapped (profitable)? Public (quarterly pressure)? | Funding model constrains strategy — a VC-backed competitor can undercut on price temporarily |

---

### 3. Moat Analysis

A moat is what prevents competitors from copying your advantage.
Score each competitor's moat — and your own.

#### The Seven Moat Types

| Moat | Definition | Depth Score Criteria | Example |
|---|---|---|---|
| **Network Effects** | Product gets better with more users | How many users before value kicks in? How defensible once established? | Slack — more teams = more useful |
| **Switching Costs** | Pain of migrating away | Data migration difficulty, workflow retraining, integration rebuilding | Salesforce — years of CRM data and custom workflows |
| **Data Advantage** | Proprietary data that improves the product | Uniqueness of data, rate of accumulation, defensibility | Google Maps — decades of mapping data |
| **Brand / Trust** | Reputation that commands premium or default choice | How long to build? How much would it cost a competitor to replicate? | Stripe — "just use Stripe" is the default |
| **Cost Advantage** | Structural ability to offer lower prices | Economies of scale, infrastructure ownership, vertical integration | AWS — scale advantages in datacenter costs |
| **Regulatory / Legal** | Licenses, patents, compliance barriers | How hard to obtain? How enforceable? | Banks — regulatory moat via licensing |
| **Counter-Positioning** | Business model that incumbents can't copy without self-harm | Would copying you cannibalize their existing revenue? | Open-source vs proprietary — incumbents can't open-source without destroying license revenue |

#### Moat Depth Scoring

| Score | Depth | Meaning |
|---|---|---|
| **0 — None** | No structural advantage | Features can be copied in weeks/months |
| **1 — Shallow** | Minor advantage, easily eroded | Slight brand recognition, small data set, low switching costs |
| **2 — Moderate** | Meaningful but not decisive | Significant data advantage OR moderate switching costs OR growing network |
| **3 — Deep** | Very difficult to replicate | Multiple reinforcing moats, strong network effects, years of data |
| **4 — Structural** | Requires fundamental market change to overcome | Regulatory lock, dominant network, platform-level lock-in |

**Apply:** Score every competitor AND yourself. If your moat is shallower
than your competitors', you need a different strategy (speed, niche, or
counter-positioning).

---

### 4. Positioning Analysis

Positioning is not what you say about yourself — it's the **space you
occupy in the customer's mind** relative to alternatives.

#### 4.1 Positioning Map

Plot competitors on a 2D map using the two dimensions **most meaningful
to your target customer**.

**How to choose axes:**
- What are the top 2 decision criteria for your target buyer?
- These become your X and Y axes.
- Common axes: simple ↔ powerful, cheap ↔ premium, individual ↔ enterprise, general ↔ specialized, self-serve ↔ sales-assisted.

```
                    POWERFUL
                       │
          Enterprise   │   Pro/Power
          (Salesforce)  │   (Notion)
                       │
    CHEAP ─────────────┼───────────── PREMIUM
                       │
          DIY/Basic    │   Boutique
          (Spreadsheets)│  (Basecamp)
                       │
                    SIMPLE
```

**Whitespace identification:** Empty quadrants or sparse areas on the map
are potential positioning opportunities. But validate — the space may be
empty because there's no demand there.

#### 4.2 Category Design vs Category Entry

Are you entering an existing category or creating a new one?

| Strategy | When | Risk | Advantage |
|---|---|---|---|
| **Category entry** | Mature market, clear demand, you have differentiation | Must unseat incumbents | Demand already exists |
| **Category creation** | No existing category fits, your approach is fundamentally different | Must educate the market, slower adoption | No direct competitors, you define the rules |
| **Subcategory** | Existing category but a specific niche is underserved | Niche may be too small | Focused positioning, easier to win |

#### 4.3 Counter-Positioning Analysis

The most powerful positioning identifies what incumbents **cannot do
without self-harm**.

**Ask for each major competitor:**
1. "If they saw our approach, could they copy it?"
2. "Would copying it cannibalize their existing revenue?"
3. "Would it require rebuilding their technical architecture?"
4. "Would it conflict with their current customer base's needs?"

If the answer to 2, 3, or 4 is yes — you have a **counter-position**.
This is the most durable form of competitive advantage for new entrants.

---

### 5. Market Sizing

Size the market to understand the prize and validate the opportunity.

#### The Three Scopes

| Scope | Definition | How to Calculate |
|---|---|---|
| **TAM** (Total Addressable Market) | Total revenue if you captured 100% of the market | Top-down: industry reports, analyst estimates. Bottom-up: # of potential customers × annual value per customer |
| **SAM** (Serviceable Addressable Market) | TAM filtered by your geographic, segment, and capability constraints | TAM × % that matches your current capabilities and target segment |
| **SOM** (Serviceable Obtainable Market) | SAM filtered by realistic capture rate | SAM × realistic market share (typically 1–5% for new entrants in year 1–3) |

#### Sizing Techniques

**Top-down (analyst-driven):**
- Use industry reports (Gartner, Statista, IBISWorld) for TAM estimates.
- Source tier: T2 for reputable analysts, but always cross-reference.
- Risk: top-down numbers are often inflated for marketing purposes.

**Bottom-up (unit-driven):**
- Count the number of potential customers in your target segment.
- Multiply by realistic annual revenue per customer.
- More credible, more defensible, usually smaller than top-down.

**Demand-signal (proxy-driven):**
- Search volume for the category and key terms.
- Growth rate of related GitHub repos, npm downloads, community size.
- Job posting volume for related roles/skills.
- Useful for emerging categories where analyst reports don't exist yet.

**Rule of thumb:** If your bottom-up and top-down estimates differ by
more than 5x, investigate why. One of them is wrong.

---

### 6. Trend & Trajectory Analysis

Markets are not static snapshots. Identify where the **puck is going**.

#### Trend Categories

| Category | What to Track | Data Sources |
|---|---|---|
| **Technology trends** | New capabilities, platform shifts, infrastructure changes | Hacker News, technical conferences, research papers |
| **Buyer behavior** | How purchasing decisions are changing, who's involved | G2 buying trends, analyst reports, sales team feedback |
| **Regulatory** | Upcoming regulations, compliance requirements | Government publications, legal analysis, industry associations |
| **Economic** | Budget pressures, spending patterns, funding environment | VC reports, public company earnings, industry surveys |
| **Substitution** | New approaches that make the current category obsolete | Adjacent market innovations, AI capabilities, workflow changes |

#### Trajectory Assessment

For each trend, assess:

1. **Direction:** Growing, stable, or declining?
2. **Velocity:** How fast is the change happening?
3. **Impact:** If this trend plays out, what changes for your market?
4. **Timing:** When will this become material? 6 months? 2 years? 5 years?
5. **Confidence:** How certain is this trend? (Use deep-research confidence scoring)

---

## Output Structure

A market intel report should follow this structure:

```markdown
# Market Intelligence: [Category/Domain]

## Executive Brief
- Market stage: [Nascent / Growing / Mature / Declining / Disrupting]
- Key finding: [Single most important insight]
- Primary opportunity: [Where the whitespace is]
- Primary threat: [Biggest competitive risk]

## Landscape Map
- Positioning map (2D visualization)
- Player categorization (direct, indirect, adjacent, emerging, substitute)
- Market size: TAM / SAM / SOM with methodology

## Competitive Deep Dives (top 3–5)
Per competitor:
- Product intelligence summary
- Business model analysis
- Moat type and depth score
- Signal intelligence (what indirect signals reveal)
- Key vulnerability

## Positioning Analysis
- Current positioning options
- Whitespace identification
- Counter-positioning opportunities
- Category strategy (enter / create / subcategory)

## Trend & Trajectory
- Key trends with direction, velocity, timing
- Implications for product and go-to-market

## Risks & Blind Spots
- What could invalidate this analysis
- Adjacent threats not fully explored
- Assumptions and their implications

## Recommended Actions
- Product implications
- Positioning recommendations
- Go-to-market implications
- What to monitor going forward (feed into living-research watch list)
```

---

## Signal Source Guide

Where to find each type of intelligence:

| Intelligence Type | Primary Sources | Quality Tier |
|---|---|---|
| Product features | Official docs, pricing pages, changelog | T1 (if official) |
| Technical stack | Job postings, GitHub, BuiltWith, engineering blog | T3 (inferred) |
| Strategic direction | Job postings (new roles), press releases, funding announcements | T2–T3 (indirect) |
| Customer sentiment | G2, Capterra, Reddit, Twitter, support forums | T4 (anecdotal, but patterns are meaningful) |
| Financial health | Crunchbase, PitchBook, SEC filings (if public), press coverage | T1 (filings) / T2 (press) |
| Market size | Gartner, Statista, IBISWorld, analyst reports | T2 (reputable analysts) |
| Pricing changes | Wayback Machine + current pricing page comparison | T1 (primary evidence) |
| Hiring patterns | LinkedIn, company careers page, job boards | T1 (primary data) |
| Product roadmap hints | Public roadmaps, GitHub milestones, conference talks, job descriptions | T3 (inferred) |
| Churn signals | "Switched from X" reviews, cancellation feedback, competitor marketing ("migrate from X") | T4 (anecdotal patterns) |

---

## Integration with Other Research Skills

| This Skill Provides | deep-research Provides | living-research Provides |
|---|---|---|
| Market-specific frameworks (moats, positioning, sizing) | Rigorous methodology (source scoring, adversarial, contradictions) | Freshness tracking, decay detection, watch lists |
| Signal intelligence techniques | Quality tiers and provenance chains | Claim lifecycle management |
| Competitive analysis structure | Stress-testing and blind spot detection | Incremental updates and evolution tracking |

**Workflow:** Use **market-intel** frameworks to structure the analysis →
Apply **deep-research** methodology for rigor → Convert to a **living-
research** document for ongoing maintenance.

---

## Review Checklist

After completing any market intelligence report, verify:

1. **Player coverage** — Have you identified all five competitor categories (direct, indirect, adjacent, emerging, substitute)?
2. **Signal layer** — Have you gone beyond landing pages to indirect signals (job postings, GitHub, reviews, pricing changes)?
3. **Moat scoring** — Is every competitor (and yourself) scored on moat depth?
4. **Positioning map** — Have you plotted the landscape on the two most meaningful axes?
5. **Counter-positioning** — Have you identified what incumbents can't copy without self-harm?
6. **Market sizing** — Do you have both top-down AND bottom-up estimates that cross-reference?
7. **Trend assessment** — Have you identified where the market is heading, not just where it is?
8. **Market stage** — Is the lifecycle stage identified and strategy aligned to it?
9. **Adversarial challenge** — (from deep-research) Have you stress-tested your positioning recommendation?
10. **Blind spots** — What adjacent threats or substitutes might you be missing?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Feature comparison table as the entire analysis | Tells you WHAT competitors have, not WHY or WHERE THEY'RE GOING | Add signal intelligence, moat analysis, and positioning map |
| Only listing direct competitors | Misses the indirect, adjacent, and substitute threats | Map all five competitor categories |
| Taking pricing pages at face value | Published pricing ≠ actual pricing (enterprise discounts, custom deals) | Note "published pricing" vs "likely actual pricing" distinction |
| Moat analysis without depth scoring | "They have a brand" is meaningless without assessing HOW STRONG | Score every moat 0–4 with justification |
| Market sizing using only top-down | Inflated numbers from analyst reports that nobody validates | Always cross-reference with bottom-up or demand-signal sizing |
| Ignoring substitutes and "doing nothing" | Your real competitor might be a spreadsheet or a manual process | Always include substitute analysis |
| Strategy mismatched to market stage | Playing a growth game in a mature market (or vice versa) | Identify lifecycle stage first, then align strategy |
| Positive-only competitor analysis | "They're great at X, Y, Z" — where are the VULNERABILITIES? | Every deep dive must include key vulnerability |
| Static snapshot with no trajectory | Market could shift in 6 months, rendering the analysis useless | Include trend analysis and feed into living-research |
| Landing page copy treated as strategy | Marketing ≠ reality; stated positioning ≠ actual positioning | Validate with customer reviews, usage data, and indirect signals |
