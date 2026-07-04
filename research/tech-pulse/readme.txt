# How to Modify the Tech Pulse Skill
# ====================================
#
# This file explains how to customize the tech-pulse SKILL.md for your
# specific interests, domain, and workflow.
#
#
# 1. DEFINE YOUR FOCUS DOMAINS
# ----------------------------
# The most important customization. Add a "Focus Domains" section
# to the top of SKILL.md (after the YAML frontmatter) to tell the
# agent what you care about:
#
#   ## Focus Domains
#   1. AI/ML — Large language models, inference engines, fine-tuning tools
#   2. Frontend — React ecosystem, build tools, CSS frameworks
#   3. Databases — PostgreSQL ecosystem, edge databases, vector stores
#   4. DevOps — CI/CD, containerization, infrastructure-as-code
#
# Without focus domains, the agent scans too broadly and everything
# looks relevant. 3-5 domains is the sweet spot.
#
# Examples for different roles:
#
#   AI Engineer:
#     1. LLM releases (open and closed source)
#     2. Inference and serving frameworks
#     3. Fine-tuning and training tools
#     4. AI developer tools (copilots, agents, IDEs)
#
#   Frontend Developer:
#     1. JavaScript frameworks and meta-frameworks
#     2. Build and bundling tools
#     3. CSS and styling solutions
#     4. Testing and quality tools
#
#   Startup Founder:
#     1. Developer tools and platforms
#     2. AI capabilities that affect my product
#     3. Competitor product launches
#     4. Funding and market signals in my space
#
#   Data Engineer:
#     1. Data processing frameworks
#     2. Cloud data platforms and warehouses
#     3. Orchestration and pipeline tools
#     4. Data quality and observability
#
#
# 2. CUSTOMIZE YOUR SOURCE LIST
# -----------------------------
# The default source tiers cover general technology. Add sources
# specific to your focus domains:
#
#   AI/ML specific:
#     - Tier 1: arxiv.org (cs.CL, cs.AI), HuggingFace model hub
#     - Tier 2: The Batch (deeplearning.ai), Import AI newsletter
#     - Tier 3: r/LocalLLaMA, r/MachineLearning
#
#   Frontend specific:
#     - Tier 1: Official framework blogs (react.dev, svelte.dev)
#     - Tier 2: JavaScript Weekly, CSS-Tricks, Smashing Magazine
#     - Tier 3: r/reactjs, r/webdev, Twitter frontend community
#
#   Cloud/Infra specific:
#     - Tier 1: AWS/GCP/Azure what's new pages
#     - Tier 2: Last Week in AWS, The New Stack
#     - Tier 3: r/aws, r/devops, CNCF Slack
#
# Add these under a "## Domain-Specific Sources" section in SKILL.md.
#
#
# 3. SET YOUR SCAN CADENCE
# ------------------------
# Edit the "Scan Cadence" section to match your needs:
#
#   If you're in AI (extreme velocity):
#     - Daily 5-min scan of Tier 1 sources
#     - Daily digest of top 3 signals
#
#   If you're in web development (fast velocity):
#     - Twice-weekly scan
#     - Weekly pulse digest
#
#   If you're tracking a stable domain:
#     - Weekly or bi-weekly scan
#     - Monthly trend report
#
#   If you're a founder monitoring everything:
#     - Daily quick scan (AI + competitor signals)
#     - Weekly deep scan (broader ecosystem)
#     - Monthly trend analysis
#
#
# 4. ADJUST NOISE FILTER SENSITIVITY
# -----------------------------------
# The default 5-step noise filter may be too strict or too loose
# for your needs:
#
#   More aggressive filtering (enterprise / stable stack):
#     - Add a "minimum community adoption" filter
#       "Ignore unless 1000+ GitHub stars or 3+ production case studies"
#     - Add a "minimum maturity" filter
#       "Ignore pre-1.0 releases unless from a known vendor"
#
#   Less aggressive filtering (early adopter / researcher):
#     - Relax the Availability Test to include betas and previews
#     - Lower the Magnitude Test threshold — evaluate "significant" too
#     - Include academic papers as valid signals even without code
#
#
# 5. CUSTOMIZE QUICK ASSESSMENT FORMAT
# -------------------------------------
# The default quick assessment format has 4 quick scores:
#   - Novelty, Maturity, Relevance, Urgency
#
# You can add domain-specific scores:
#
#   For AI models:
#     - Benchmark quality: 🟢 Independent / 🟡 Vendor / 🔴 No benchmarks
#     - Openness: 🟢 Open weights / 🟡 API only / 🔴 Closed
#     - License: 🟢 Permissive / 🟡 Restricted / 🔴 Proprietary
#
#   For developer tools:
#     - Integration: 🟢 Works with our stack / 🟡 Partial / 🔴 Incompatible
#     - Migration: 🟢 Drop-in replacement / 🟡 Moderate effort / 🔴 Rewrite
#
#
# 6. SET UP AUTOMATED MONITORING
# -------------------------------
# For high-velocity domains, you can supplement manual scanning
# with automated monitoring. Add a "references/" directory:
#
#   tech-pulse/
#   ├── SKILL.md
#   ├── readme.txt
#   └── references/
#       ├── signal-log.md          <- Running log of all detected signals
#       ├── watch-sources.md       <- URLs and RSS feeds to check
#       ├── archived-signals.md    <- Signals that were filtered out
#       └── trend-tracker.md       <- Emerging patterns being monitored
#
# The signal-log.md becomes valuable over time — you can search it
# to identify trends retrospectively.
#
#
# 7. CONNECT TO TECH RADAR
# -------------------------
# Define explicit escalation rules that map to YOUR radar setup:
#
#   ## Escalation Rules
#   - Any new launch in [Focus Domain 1] → auto-ASSESS on radar
#   - Security alert for any ADOPT-ring technology → immediate ALERT
#   - 3+ signals about same technology in one week → escalate for review
#   - Competitor adopts a technology we have in HOLD → re-evaluate
#
# This ensures the pulse-to-radar pipeline is consistent and not
# dependent on subjective judgment each time.
#
#
# 8. DIGEST DISTRIBUTION
# ----------------------
# If you're creating pulse digests for a team, customize the
# digest format:
#
#   For executives:
#     - 3-bullet summary, no technical details
#     - Focus on strategic implications and competitive signals
#
#   For engineers:
#     - Technical details, benchmarks, code availability
#     - Link to quick assessments for anything relevant
#
#   For product:
#     - Focus on capability unlocks and user-facing implications
#     - Connect signals to roadmap and feature opportunities
#
#
# 9. FILE STRUCTURE EXAMPLE
# -------------------------
# A fully set up tech-pulse directory:
#
#   tech-pulse/
#   ├── SKILL.md                    <- Core skill (edit focus domains, sources)
#   ├── readme.txt                  <- This file
#   └── references/
#       ├── signal-log.md           <- Chronological signal log
#       ├── watch-sources.md        <- Monitored URLs and feeds
#       ├── archived-signals.md     <- Filtered-out signals (for trend mining)
#       ├── trend-tracker.md        <- Active trends being watched
#       └── digests/
#           ├── 2025-w27.md         <- Weekly digest
#           ├── 2025-w28.md
#           └── 2025-07-trends.md   <- Monthly trend report
#
