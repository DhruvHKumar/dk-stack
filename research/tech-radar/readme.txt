DELETE THIS FILE BEFORE USING THIS WITH YOUR OPENCLAW/HERMES AGENT

# How to Modify the Tech Radar Skill
# ====================================
#
# This file explains how to customize the tech-radar SKILL.md for your
# team, organization, or domain.
#
#
# 1. CUSTOMIZE THE QUADRANTS
# --------------------------
# The default quadrants are:
#   - Languages & Frameworks
#   - Tools
#   - Platforms & Infrastructure
#   - Techniques & Patterns
#
# To change them, edit the "The Four Quadrants" table in SKILL.md.
# Keep exactly 4 quadrants — the radar model works best with 4.
#
# Examples for different domains:
#
#   Data Engineering:
#     - Languages & Libraries (Python, Spark, dbt)
#     - Data Platforms (Snowflake, Databricks, BigQuery)
#     - Orchestration & Ops (Airflow, Dagster, Terraform)
#     - Patterns & Architecture (Medallion, event-driven, feature stores)
#
#   Mobile Development:
#     - Languages & Frameworks (Swift, Kotlin, Flutter, React Native)
#     - Tools & SDKs (Fastlane, Firebase, Sentry, RevenueCat)
#     - Backend & Services (Supabase, AWS Amplify, CloudKit)
#     - Patterns & Practices (Modularization, CI/CD pipelines, A/B testing)
#
#   Design / Creative:
#     - Design Tools (Figma, Framer, Sketch, Penpot)
#     - Prototyping & Motion (ProtoPie, Rive, Lottie, After Effects)
#     - Design Systems (Material, Radix, Shadcn, custom tokens)
#     - Methodologies (Design sprints, user testing, accessibility audits)
#
#
# 2. ADJUST THE EVALUATION DIMENSIONS
# ------------------------------------
# The default 8 dimensions are:
#   1. Maturity
#   2. Ecosystem Health
#   3. Learning Curve
#   4. Performance Characteristics
#   5. Operational Complexity
#   6. Migration Cost (In and Out)
#   7. Vendor & Maintainer Risk
#   8. Team Fit
#
# You can:
#   - ADD dimensions relevant to your domain
#     Example: "Regulatory Compliance" for healthcare/fintech
#     Example: "Accessibility Support" for consumer-facing apps
#     Example: "Offline Capability" for mobile apps
#
#   - WEIGHT dimensions differently
#     Add a "Weight" column to the scoring table:
#     | Dimension | Weight | Score | Notes |
#     For a startup: Team Fit and Learning Curve may be weighted highest.
#     For enterprise: Vendor Risk and Ops Complexity may dominate.
#
#   - REMOVE dimensions that aren't relevant
#     Example: A solo developer may skip "Team Fit"
#     But document WHY you removed it — don't silently skip.
#
#
# 3. CUSTOMIZE THE RINGS
# ----------------------
# The 4 rings (ADOPT, TRIAL, ASSESS, HOLD) are standard and should
# generally NOT be modified — they provide a shared vocabulary.
#
# However, you CAN adjust the criteria for ring placement:
#   - Stricter ADOPT: "Must have 12+ months production usage" (enterprise)
#   - Looser TRIAL: "Successful spike is enough" (startup)
#   - Add a "RETIRE" state alongside HOLD for technologies actively
#     being migrated away from.
#
#
# 4. SET YOUR REVIEW CADENCE
# --------------------------
# Edit the "Radar Review Cadence" section to match your team:
#
#   Solo / Indie:     Every 6 months (you have fewer technologies)
#   Startup (< 10):   Quarterly full review
#   Growth (10-50):   Monthly rotating quadrant review
#   Enterprise (50+): Bi-weekly targeted reviews + quarterly full
#
#
# 5. ADD TEAM-SPECIFIC CONTEXT
# ----------------------------
# At the top of SKILL.md, after the YAML frontmatter, you can add
# a "Team Context" section:
#
#   ## Team Context
#   - Team size: 5 engineers
#   - Primary stack: TypeScript, React, Node.js, PostgreSQL
#   - Deployment: Vercel + Supabase
#   - Constraints: Small team, must minimize operational overhead
#   - Philosophy: Prefer managed services, avoid self-hosting
#
# This context will influence how the agent scores Team Fit and
# Operational Complexity for any technology evaluation.
#
#
# 6. ADD DOMAIN-SPECIFIC SIGNALS
# ------------------------------
# In the "Ecosystem Health" section, add signals specific to your
# domain. For example:
#
#   Frontend tools: Check npm download trends (npmtrends.com)
#   DevOps tools: Check CNCF landscape status
#   AI/ML tools: Check HuggingFace activity, paper citations
#   Mobile: Check app store SDK adoption data
#
#
# 7. INTEGRATE WITH YOUR EXISTING RADAR
# --------------------------------------
# If your team already maintains a technology radar (e.g., in a
# spreadsheet, Notion, or a dedicated tool like Thoughtworks' build-
# your-own-radar), you can add a "references/" directory next to
# this SKILL.md with:
#
#   tech-radar/
#     SKILL.md           <- The skill instructions
#     readme.txt          <- This file
#     references/
#       current-radar.md  <- Your current radar state
#       radar-history.md  <- Movement log
#       evaluation-log/   <- Individual technology evaluations
#
# The agent will use these reference files as context when evaluating
# new technologies or updating existing entries.
#
#
# 8. FILE STRUCTURE EXAMPLE
# -------------------------
# A fully customized tech-radar directory might look like:
#
#   tech-radar/
#   ├── SKILL.md                    <- Core skill (edit quadrants, dimensions)
#   ├── readme.txt                  <- This file
#   └── references/
#       ├── current-radar.md        <- Current state of all blips
#       ├── radar-history.md        <- Movement log over time
#       └── evaluations/
#           ├── supabase.md         <- Individual evaluation
#           ├── bun-runtime.md      <- Individual evaluation
#           └── htmx.md             <- Individual evaluation
#
