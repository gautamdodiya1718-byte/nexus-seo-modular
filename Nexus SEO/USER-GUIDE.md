# Nexus SEO Modular — User Guide

How to install, configure, and use the 6 standalone Nexus skills — alone or together — to get the best output for any SEO/content workflow.

---

## Table of Contents

1. [What is the Nexus Modular Family](#what-is-the-nexus-modular-family)
2. [Quick Start (5 minutes)](#quick-start-5-minutes)
3. [The 6 Skills — Per-Skill Reference](#the-6-skills--per-skill-reference)
4. [Setup: Brand, Author, and User Preferences](#setup-brand-author-and-user-preferences)
5. [Skill Complementarity Matrix](#skill-complementarity-matrix)
6. [Scenario-Based Workflows](#scenario-based-workflows)
7. [Best Practices](#best-practices)
8. [Common Mistakes to Avoid](#common-mistakes-to-avoid)
9. [Quick-Reference Tables](#quick-reference-tables)
10. [Troubleshooting](#troubleshooting)

---

## What is the Nexus Modular Family

A family of 6 standalone Claude skills for end-to-end SEO and content work:

- **nexus-tech-seo** — technical SEO diagnosis (source code, GSC, PageSpeed)
- **nexus-audit** — content analysis and audit
- **nexus-research** — keyword + SERP + outline + metadata research
- **nexus-content** — content creation (blog, landing, guest, tutorial, comparison, pillar, refresh)
- **nexus-strategy** — strategy + roadmap + portfolio + AI search positioning
- **nexus-orchestrator** — optional automation layer that chains the others

Each skill is independent. Install only what you need. None depend on the others. Optionally install the orchestrator for automated chaining.

**Who's it for:**
- Solo SEO practitioners and content marketers
- In-house SEO/content teams
- Agencies serving multiple brands
- Freelance content auditors
- Founders doing their own SEO

---

## Quick Start (5 minutes)

### 1. Pick the skill you need

Don't install everything. Pick based on what you actually do:
- Just want to audit existing content? → install only **nexus-audit**
- Just want technical SEO diagnosis? → install only **nexus-tech-seo**
- Writing new content? → install **nexus-research** + **nexus-content**
- Doing everything? → install all 6 + orchestrator

### 2. Install

For each skill you want:
1. Download the `.zip` file
2. Unzip
3. Upload to your Claude project, or install via Claude's plugin/skill system

### 3. (Optional) Create a brand profile

If you want brand-specific output:
1. Open `brand-template.skill.md` (in any installed skill folder)
2. Copy it. Rename to `brand-[yourbrand].skill.md`
3. Fill in every section (brand identity, audience, tone, banned words, internal links, CTAs, etc.)
4. Upload to your Claude project — Nexus auto-detects on next session

If you skip this, skills run in brand-agnostic mode (works fine but less specific).

### 4. (Optional) Create an author profile

For voice-specific content:
1. Open `author-template.skill.md` (in nexus-audit or nexus-content folder)
2. Copy. Rename to `author-[name].skill.md`
3. Fill in vocabulary, signature moves, opinion stances, experience markers
4. Upload to your project

### 5. Invoke a skill

Just describe what you want in natural language:
- "Audit this URL: [paste URL]"
- "Write a blog on [topic]"
- "Why isn't my site ranking for [keyword]?"
- "Find keywords for [topic]"

The skill auto-detects which mode you want, shows a heads-up before running anything, executes, and produces a structured output with accountability log.

---

## The 6 Skills — Per-Skill Reference

### 🔧 nexus-tech-seo

**What it does:** Diagnoses technical SEO issues from your website source code, Google Search Console exports, and Google PageSpeed Insights reports. Identifies whether the bottleneck preventing ranking is content-side or tech-side.

**Modes (auto-detected from your prompt):**
| Trigger phrase | Mode |
|---|---|
| "audit my site / tech SEO / why isn't my site ranking" | full_audit (default) |
| "PageSpeed / Web Vitals / LCP / INP / CLS" | core_web_vitals_only |
| "GSC / Search Console / queries" | gsc_insights_only |
| "cannibalization / two pages competing" | cannibalization_only |
| "ranking diagnosis / backlink targets" | ranking_diagnosis |
| "content is good but not ranking" | content_tech_bottleneck |
| "analyze my source code / HTML" | source_code_only |

**Inputs (any combination):**
- Source code: HTML, CSS, JS, .md exports, ZIPs of site source, or pasted content
- GSC reports: queries CSV, pages CSV, performance JSON, coverage report
- PageSpeed Insights: full JSON, screenshot, or pasted summary
- Optional: robots.txt, sitemap.xml, hreflang config

**Output:** Severity-ranked issue list (CRITICAL / HIGH / MEDIUM / LOW) with specific fix instructions for each, cannibalization flags with consolidate-vs-differentiate recommendations, content-vs-tech bottleneck diagnosis, and confidence percentage.

**When to use:** Any time you suspect technical SEO is a bottleneck, want a comprehensive site audit, need to diagnose ranking failures, or want to verify Core Web Vitals.

---

### 🔍 nexus-audit

**What it does:** Audits any blog post, article, landing page, or pasted content against the full quality framework — 7-section forensic audit, E-E-A-T enforcement, Helpful Content compliance, AI citation readiness, differentiation analysis, live fact verification.

**Modes:**
| Trigger phrase | Mode |
|---|---|
| "audit this URL / page / blog" | full_audit (default) |
| "quick score / just numbers" | quick_score |
| "audit my headings / fix H2s" | heading_audit |
| "E-E-A-T / experience signals" | eeat_audit |
| "AEO / GEO / AI search readiness" | aeo_geo_scoring |
| "compare to / vs competitors" | competitor_compare |
| "LLM checklist / score against rubric" | llm_checklist_scoring |
| "fact check / verify claims" | fact_verification_only |
| "Helpful Content / Google quality" | helpful_content_check |

**Inputs:**
- Content (URL, file, or pasted text) — required
- Target keyword — asks if not provided
- Brand profile — optional but enriches recommendations
- Author profile — optional, enables voice consistency check
- Top 3 SERP competitors — optional, enables competitive comparison

**Output:** Audit report with executive summary (5 bullets), scores dashboard (8 dimensions /100), 7-12 detailed sections, priority fix order (top 10 by impact), confidence percentage.

**When to use:** Auditing existing content (yours or others'), evaluating content before publishing, comparing your content to ranking competitors, scoring content for AI search readiness, fact-checking content for hallucinations.

---

### 🔬 nexus-research

**What it does:** Discovers keywords, analyzes SERPs, identifies gaps, builds content outlines, generates metadata packages, and produces complete research packages ready to feed any content generator.

**Modes:**
| Trigger phrase | Mode |
|---|---|
| "find keywords / keyword research" | keyword_research |
| "SERP for / what's ranking for" | serp_analysis |
| "competitor gap / vs top 10" | competitor_gap |
| "outline / TOC / table of contents" | content_outline |
| "metadata / SEO title / meta description" | metadata_package |
| "topic cluster / entity map / topical authority" | semantic_seo |
| "research [topic] / full research" | full_research_package (default) |

**Inputs:**
- Topic or keyword — required
- Geo target — optional (defaults to US)
- Brand profile — optional but enriches output
- Content type intent — optional

**Output:** Per mode. The full research package includes primary + secondary keywords, SERP intelligence (intent, top 10 analysis, PAA), content outline with differentiation angles, metadata package (title + meta + excerpt + slug + schema), entity coverage list, internal linking targets (if brand loaded), competitor gap analysis, AI citation optimization recommendations, opportunity scores. Output ready to chain into nexus-content.

**When to use:** Before writing any new content, researching a new topic, analyzing competitors for a keyword, building topic clusters, generating metadata packages.

---

### ✍️ nexus-content

**What it does:** Generates blog posts, landing pages, guest posts, tutorials, comparison posts, pillar pages, and content refreshes. Substance gates replace word count enforcement. Live fact + link verification. Author voice. User research injection. Schema generation. Decay tracking.

**Modes:**
| Trigger phrase | Mode |
|---|---|
| "write a blog / write an article" | standard_blog (default) |
| "landing page / vs page / alternatives" | landing_page |
| "guest post / write for [site]" | guest_post |
| "tutorial / how to / step-by-step" | tutorial |
| "X vs Y / compare X and Y" | comparison_post |
| "pillar page / ultimate guide" | pillar_page |
| "refresh / update / improve [URL]" | content_refresh |

**Inputs:**
- Topic or target keyword — required
- Research package (from nexus-research) — recommended
- Brand profile — recommended (drives voice, internal linking, banned words, CTAs)
- Author profile — optional
- User preferences — optional
- User research documents/data — optional but high-leverage
- URL — only for content_refresh mode

**Output:** Either:
- Full content with metadata, schema blocks, internal links, infographics, callouts, fact-verified claims, link-verified URLs, decay tags
- OR blocked with specific remediation if confidence gate fails (you can override with UNRELIABLE warning)

**When to use:** Creating any new content, refreshing existing content, generating guest posts, building tutorials with verified working code, creating comparison posts with structured comparison tables.

---

### 🎯 nexus-strategy

**What it does:** Generates SEO strategies, content roadmaps, programmatic SEO setups, AI search positioning strategies, concept ownership analyses, and content portfolio reports. Has cross-session memory.

**Modes:**
| Trigger phrase | Mode |
|---|---|
| "SEO strategy / 6-month plan / full strategy" | full_strategy (default) |
| "topical authority / topic cluster strategy" | topical_authority |
| "programmatic SEO / pages at scale" | programmatic_seo |
| "content roadmap / publication schedule" | content_roadmap |
| "strategic brief / executive summary" | strategic_brief |
| "AI search positioning / Perplexity strategy" | ai_search_positioning |
| "concept ownership / what to own" | concept_ownership |
| "content portfolio status / what to update" | content_portfolio_status |

**Inputs:**
- Brand profile — strongly recommended
- Topic or focus area — optional for full strategy
- Existing data — optional, loaded from memory if present

**Output:** Strategy report with executive brief, topical authority map, content roadmap, AI search positioning recommendations, concept ownership analysis, 7 prioritized next actions.

**When to use:** Building SEO strategy, planning topical authority, scaling content production, positioning for AI search, identifying which concept to own, managing content portfolio maintenance.

---

### 🤖 nexus-orchestrator (optional)

**What it does:** Chains other Nexus skills automatically. Detects which Nexus skills are installed and offers a unified menu only showing options for available skills. Includes mandatory output-block enforcement, drift detection, pipeline accountability log.

**Pre-requisites:** Install at least 2 other Nexus skills first.

**Inputs:** Same as the chained skills.

**Output:** Same as the chained skills, with a pipeline accountability log showing what executed, skipped, and why.

**When to use:** When you want automated chaining (e.g., "write a blog" → orchestrator runs research + content + audit automatically) and you have 2+ Nexus skills installed.

**When NOT to use:** Free users with limited context. Single-skill installations. Quick one-off tasks (invoke the skill directly).

---

## Setup: Brand, Author, and User Preferences

These three profile types are optional but transform output quality.

### Brand Profile (recommended)

**File:** `brand-[yourbrand].skill.md`
**Template:** `brand-template.skill.md` (in every skill folder)

**What's in it:** brand identity, target audience, tone, banned words, content pillars, products with maturity labels (STABLE / BETA / COMING_SOON / UNRELIABLE), internal URL repository, competitors, CTA preferences, mention rules (frequency, style, first mention format), schema preferences, geo markets, GSC property.

**Used by:** every skill that generates or recommends content.

**How to set up:**
1. Copy `brand-template.skill.md`
2. Rename to `brand-[name].skill.md`
3. Fill in EVERY section (don't leave fields blank)
4. Upload to your Claude project alongside any installed Nexus skill
5. On first invocation, Nexus confirms "Running under [Brand Name]"

### Author Profile (optional but high-leverage)

**File:** `author-[name].skill.md`
**Template:** `author-template.skill.md` (in nexus-audit and nexus-content folders)

**What's in it:** author identity, vocabulary tier (beginner/practitioner/expert/mixed), vocabulary quirks (signature phrases), anti-vocabulary (words/phrases NEVER used), sentence rhythm, signature structural moves (opening/section/closing patterns), opinion stances on niche debates, personal experience markers, writing examples (3-5 sentences), topics credible on / not credible on, disclosures.

**Used by:** humanizer, eeat-enforcer, author-voice-applier, differentiation-checker.

**Why high-leverage:** without an author profile, content sounds like polished AI prose. With one, it sounds like a specific experienced person.

**How to set up:** Copy `author-template.skill.md`, rename, fill in (especially the writing examples — those are calibration data), upload.

### User Preferences (optional)

**File:** `user-preferences-[name].md`
**Template:** `user-preferences-template.md` (in nexus-content folder)

**What's in it:** always-include rules, never-include rules, structural preferences, custom quality checks, confidence gate override settings, mode defaults, output format preferences, default brand.

**Used by:** every Nexus skill that has user-facing output.

**Why useful:** if you ALWAYS want a TL;DR block, NEVER want em dashes, ALWAYS want a comparison table on comparison content — set these once and stop re-asking session by session.

---

## Skill Complementarity Matrix

Which skills work well together for which goals:

| Skill | Pairs naturally with | For these goals |
|---|---|---|
| nexus-research | nexus-content | New content creation (research → write) |
| nexus-research | nexus-strategy | Strategy + roadmap (research informs strategy) |
| nexus-research | (alone) | Just need keyword/SERP/outline data |
| nexus-content | nexus-research | New content (uses research as input) |
| nexus-content | nexus-audit | Quality verification post-generation |
| nexus-content | nexus-strategy | Programmatic SEO (strategy + content) |
| nexus-content | (alone) | Quick content creation when research isn't needed |
| nexus-audit | nexus-tech-seo | Diagnosing why content isn't ranking |
| nexus-audit | nexus-content | Optimizing existing content (audit → refresh) |
| nexus-audit | (alone) | Auditing third-party content, scoring competitors |
| nexus-tech-seo | nexus-audit | Comprehensive site diagnosis (tech + content) |
| nexus-tech-seo | (alone) | Pure technical SEO audit |
| nexus-strategy | nexus-research | Strategy informed by research |
| nexus-strategy | nexus-content | Programmatic SEO execution |
| nexus-strategy | (alone) | Strategic planning when execution comes later |
| nexus-orchestrator | (any 2+ Nexus skills) | Automated chaining |

---

## Scenario-Based Workflows

The most-common scenarios with step-by-step instructions, which skills to use, in what order, with what inputs.

### Scenario A — Writing New Content From Scratch

**Goal:** create a new blog post (or landing page / tutorial / comparison post / pillar page) that ranks well and feels human.

**Skills needed:** nexus-research + nexus-content (+ nexus-audit for QA)

**Step-by-step:**

1. **Setup** (one-time):
   - Create brand profile
   - Create author profile (if you want author voice)
   - (Optional) prepare any user research you want to inject (interview transcripts, internal data, proprietary benchmarks)

2. **Research phase:**
   - Invoke `nexus-research` with: "research [topic] for me"
   - Mode auto-detected: `full_research_package`
   - Confirm heads-up
   - Receive: keyword data, SERP analysis, content outline, metadata, differentiation angles, AI citation recommendations
   - Save the research package output

3. **Generation phase:**
   - Invoke `nexus-content` with: "write a blog on [topic], here's the research: [paste research package]"
   - If you have user research: include it as additional input
   - If you have author profile: ensure it's loaded
   - Mode auto-detected: `standard_blog` (or whichever variant)
   - Confirm heads-up
   - Receive: full content with metadata, schema blocks, fact-verified claims, link-verified URLs, decay tags
   - If confidence gate blocks delivery: review remediation, fix issues, re-run

4. **(Optional) QA phase:**
   - Invoke `nexus-audit` with: "audit this content: [paste generated content]"
   - Mode: `full_audit`
   - Receive: forensic audit confirming quality scores, identifying any remaining gaps

**Output expectations:**
- Substance-driven length (no padding)
- Fact-verified claims with linked sources
- Working internal links
- Author voice (if profile loaded)
- AI-citation-friendly structure
- Schema markup ready to paste
- Decay map (when to review)

**Tips for best output:**
- Provide user research if you have any — it's the highest-leverage input for differentiation
- Provide author profile — content quality jumps significantly
- Don't override the confidence gate unless you have a specific reason

**With orchestrator:** Just say "write a blog on [topic]" — orchestrator chains research + content + audit automatically.

---

### Scenario B — Optimizing Existing Content

**Goal:** improve an existing article that's published but not performing well (or that's stale).

**Skills needed:** nexus-audit + nexus-content

**Step-by-step:**

1. **Audit phase:**
   - Invoke `nexus-audit` with: "audit this URL: [URL]"
   - Mode: `full_audit`
   - Receive: 7-section forensic audit + scores dashboard + priority fix list
   - Note: ranking-status-check runs first (PROTECT/SHIELD/RECOVERY/FULL mode determined)

2. **Decide scope:**
   - If audit shows page is at position 1-3: only minor refresh allowed (PROTECT mode — don't disrupt ranking)
   - If position 4-10: targeted improvements (SHIELD mode)
   - If 11-30: aggressive optimization (RECOVERY mode)
   - If beyond 30: full optimization (FULL mode)

3. **Refresh phase:**
   - Invoke `nexus-content` with: "refresh this URL: [URL]" + audit findings
   - Mode auto-detected: `content_refresh`
   - The skill applies improvements within the ranking-status-check constraints
   - Receive: refreshed content respecting ranking protection rules

**Output expectations:**
- Targeted improvements only (won't rewrite content that's ranking well)
- Updated facts (live verified)
- Fixed broken links
- Refreshed decay-sensitive elements
- Updated metadata if needed

**Tips for best output:**
- Always run audit FIRST — you need ranking-status before deciding scope
- Don't override PROTECT mode without explicit reason
- Provide GSC data if you want intent-mismatch insights

---

### Scenario C — Analyzing Existing Content (Just Audit, No Optimization)

**Goal:** evaluate content quality without rewriting (e.g., reviewing third-party content, scoring before purchase, content audit consulting).

**Skills needed:** nexus-audit (alone)

**Step-by-step:**

1. Invoke `nexus-audit` with: "audit this URL: [URL]" or "audit this content: [paste]"
2. Pick mode based on focus:
   - Comprehensive audit: `full_audit`
   - Just want scores: `quick_score`
   - Just headings: `heading_audit`
   - Just E-E-A-T: `eeat_audit`
   - Just AI search readiness: `aeo_geo_scoring`
   - Just fact-check claims: `fact_verification_only`
3. Receive: detailed audit per the chosen mode

**Tips for best output:**
- Provide the target keyword (audit needs to know what intent to evaluate against)
- Provide top 3 SERP competitors if you want comparative analysis
- Brand profile not required for third-party content

---

### Scenario D — Tech SEO Website Analysis

**Goal:** comprehensive technical SEO audit of a website (your own or a client's).

**Skills needed:** nexus-tech-seo (alone)

**Step-by-step:**

1. **Gather inputs:**
   - Page source HTML (for the most important pages — homepage, top blog posts, key landing pages)
   - GSC export from the last 90 days (queries report + pages report — performance data preferred with both Page and Query dimensions for cannibalization detection)
   - PageSpeed Insights JSON (run https://pagespeed.web.dev/ on key URLs, both mobile + desktop, save JSON)
   - Optional: robots.txt, sitemap.xml, hreflang config

2. **Invoke:** `nexus-tech-seo` with: "audit my site, here's the source HTML, GSC export, PageSpeed JSON: [attach]"
3. Mode auto-detected: `full_audit`
4. Confirm heads-up
5. Receive: severity-ranked issues (CRITICAL → HIGH → MEDIUM → LOW), cannibalization flags, content-vs-tech bottleneck diagnosis, ranking diagnostic with backlink targets, specific fix instructions per issue

**Output expectations:**
- Critical blockers identified first (e.g., noindex meta on important pages, robots blocking crawl)
- Core Web Vitals issues with specific elements identified
- Cannibalization cases with consolidate-vs-differentiate recommendations
- Content-vs-tech bottleneck analysis: "your content is good, the issue is X"

**Tips for best output:**
- Provide ALL inputs if possible — confidence increases with completeness
- Even partial inputs work — skill flags what's missing and operates on what's there
- For best cannibalization detection: GSC export must include both Page and Query dimensions
- Run PageSpeed on mobile AND desktop, both JSONs

---

### Scenario E — Diagnosing Why Content Isn't Ranking

**Goal:** specific URL not ranking despite seeming high quality. Want to know exactly what's wrong.

**Skills needed:** nexus-tech-seo + nexus-audit

**Step-by-step:**

1. **Run nexus-audit on the URL** with: "audit this URL: [URL]" → get content quality score
2. **Run nexus-tech-seo on the same URL** with: "ranking diagnosis for [URL], here's the source + GSC + PageSpeed" → get tech health
3. **Synthesize:** invoke nexus-tech-seo with: "content vs tech bottleneck for [URL]" — provides the cross-referenced diagnosis ("Your content scored 92, but tech score is 38 because of X. Fix order: 1, 2, 3.")
4. Apply fixes in priority order
5. Wait 4-6 weeks for re-crawl
6. Re-audit

**Tips for best output:**
- Don't jump to rewriting content — audit + tech first to identify the actual bottleneck
- If content is already great (audit score 85+) and ranking is poor, the issue is almost always tech or authority
- Provide ALL inputs to tech-seo for highest confidence diagnosis

**With orchestrator:** Say "why isn't [URL] ranking?" — orchestrator chains audit + tech-seo + content-tech-bottleneck synthesis automatically.

---

### Scenario F — Building Full SEO Strategy

**Goal:** establish or refresh comprehensive SEO strategy with 6-month roadmap.

**Skills needed:** nexus-research + nexus-strategy

**Step-by-step:**

1. **Brand profile required.** Strategy without brand context produces generic advice.
2. **Research phase:** invoke `nexus-research` with mode `keyword_research` and `competitor_gap` to populate keyword universe and competitive intelligence
3. **Strategy phase:** invoke `nexus-strategy` with: "give me full SEO strategy for [brand]"
4. Mode: `full_strategy`
5. Receive: executive strategic brief, topical authority map, 6-month roadmap, AI search positioning, concept ownership recommendation, 7 prioritized next actions
6. **Memory persists across sessions.** Next time you invoke nexus-strategy for the same brand, it builds on prior session.

**Tips for best output:**
- Run research first — strategy uses research data
- Provide GSC data if available (improves portfolio diagnosis)
- Set time horizon (12 months / 24 months / 5 years) for concept ownership recommendations

---

### Scenario G — Topical Authority / Pillar Page Workflow

**Goal:** establish topical authority by publishing a pillar page + supporting cluster content.

**Skills needed:** nexus-research + nexus-strategy + nexus-content

**Step-by-step:**

1. **Strategy:** invoke `nexus-strategy` with: "plan topical authority for [topic]" → mode `topical_authority` → receive cluster architecture + roadmap
2. **Research the pillar:** invoke `nexus-research` with: "semantic SEO for [topic]" → mode `semantic_seo` → receive topic cluster map + entity relationships
3. **Generate pillar page:** invoke `nexus-content` with: "pillar page for [topic]" → mode `pillar_page` → receive comprehensive pillar with cluster links + ≥7 unique angles
4. **Generate cluster pages:** for each sub-topic, invoke `nexus-content` with: "write a blog on [sub-topic]" → mode `standard_blog` → ensure each cluster page links back to pillar
5. **Internal linking review:** the brand's `urls` repository should now include all pillar + cluster pages so future content links correctly

**Tips for best output:**
- Plan all cluster sub-topics BEFORE writing the pillar — pillar links to cluster, so cluster pages must be planned first
- Pillar page differentiation requirement is strict (≥7 unique angles) — usually means including original data or contrarian positions
- Decay-mapping flags pillar's time-bound sections separately from evergreen spine

---

### Scenario H — Content Portfolio Maintenance

**Goal:** review your content portfolio, identify what to update, prioritize maintenance work.

**Skills needed:** nexus-strategy + nexus-audit + nexus-content

**Step-by-step:**

1. **Portfolio status:** invoke `nexus-strategy` with: "show me content portfolio status" → mode `content_portfolio_status` → receive list of all content with rank, decay signal, audit date, recommended action per piece
2. **Prioritize:** focus on URGENT_AUDIT_DIAGNOSE pieces first (declining rankings)
3. **For each priority piece:**
   - Invoke `nexus-audit` for diagnosis
   - Then `nexus-content` mode `content_refresh` to apply improvements within ranking protection constraints
4. **Update portfolio:** mark pieces as updated; portfolio status will reflect on next run

**Tips for best output:**
- This workflow requires content-memory to be populated (nexus-strategy tracks all generated content)
- If you have GSC export, provide it to nexus-strategy — improves accuracy of decline detection
- Don't refresh STABLE_TOP content without good reason — protect ranking

---

### Scenario I — Guest Post for External Site

**Goal:** write a guest post for a publication outside your brand. Different rules apply.

**Skills needed:** nexus-research + nexus-content

**Step-by-step:**

1. **Gather:** topic, host site URL, designated anchor text, anchor link target, host editorial guidelines (if available)
2. **Light research:** invoke `nexus-research` with mode `serp_analysis` for the topic — confirm angle
3. **Write:** invoke `nexus-content` with: "guest post for [host site] on [topic]"
4. Mode: `guest_post`
5. The skill applies guest-post-mode overrides:
   - ~30% host context, ~10% your brand, ~60% educational
   - One designated anchor with exact text
   - No CTAs
   - Tone calibrated to host's editorial voice (if host URL provided)
   - No infographics (unless host requested)
   - No schema (host owns the page)
6. Receive: guest post in markdown, ready to submit

**Tips for best output:**
- Research host's recent content tone first — provide URL to nexus-content so it can match
- Verify designated anchor placement looks natural
- Don't force brand mentions — most guest posts should have 0-2 max

---

### Scenario J — Programmatic SEO at Scale

**Goal:** generate hundreds or thousands of pages from a template (e.g., "best [tool] for [use case]" pages, "X vs Y" comparison pages).

**Skills needed:** nexus-strategy + nexus-content

**Step-by-step:**

1. **Plan:** invoke `nexus-strategy` with: "programmatic SEO for [pattern]" → mode `programmatic_seo`
2. Receive: template design + keyword matrix + sample pages + quality gates + rollout plan
3. **Approve template** (this is the only checkpoint)
4. **Generate at scale:** invoke `nexus-content` with: "generate page from template [template name] for [keyword combination]" — repeated for each keyword in matrix
5. Each page goes through full nexus-content pipeline with substance gates, fact verification, etc.
6. Quality gates from programmatic-seo skill apply
7. Stage rollout per the plan

**Tips for best output:**
- Don't skip the strategy phase — bad templates produce bad pages at scale
- Quality gates are stricter for programmatic — even more important to apply confidence-gate
- Stage rollout in batches (don't publish 1000 pages at once — Google penalizes sudden mass-publishing)

---

## Best Practices

**Universal best practices for any scenario:**

1. **Always create a brand profile.** It transforms output from generic to specific. 30 minutes of setup, used forever.

2. **Create an author profile when you can.** Especially for content with first-person experience markers. Without it, content feels AI-written.

3. **Provide user research when you have it.** Interviews, internal data, benchmarks — these are the highest-leverage differentiation inputs.

4. **Read the heads-up.** Every skill shows what it's about to do before doing it. If the plan looks wrong, change mode or cancel before context is wasted.

5. **Read the accountability log.** It tells you exactly what executed, what skipped, why, and overall confidence. Don't skip it.

6. **Don't override confidence gate without good reason.** If accuracy is below threshold, content has problems. Override only when you've reviewed the issues and accept them.

7. **Run nexus-audit on your own content before publishing.** Even content you wrote manually benefits from the audit checks.

8. **For ranking content, respect PROTECT mode.** Don't restructure content that's ranking 1-3. Minor updates only.

9. **Cross-skill workflows beat single-skill calls.** Research → Content is much better than just Content. Audit → Content (for refresh) beats Content alone.

10. **Use orchestrator only when you have 2+ skills.** Solo skill installations don't benefit from it.

---

## Common Mistakes to Avoid

1. **Skipping research before content.** Generates content based on training data, not current SERP. Output is generic.

2. **Skipping audit on existing content.** Optimizing without auditing first means you don't know what's actually wrong.

3. **Treating word count as a target.** It's not — it's an output. Don't ask the skill to "make it longer." If it's complete, it's complete.

4. **Overriding confidence gate routinely.** Defeats the entire verification system.

5. **Not creating a brand profile.** Brand-agnostic mode works but produces noticeably less specific output.

6. **Mixing multi-author site without author profiles.** Content reads inconsistently because there's no author calibration.

7. **Refreshing PROTECT-mode content (positions 1-3).** Restructuring content that's ranking well risks losing the ranking. Don't do it without strong reason.

8. **Not providing source code to nexus-tech-seo.** It's required for source-code-based checks. Without it, the audit is partial.

9. **Not providing GSC export to nexus-tech-seo.** Cannibalization detection requires GSC pages × queries data. Without it, this check is skipped.

10. **Using orchestrator when you only have 1 skill.** Wastes context. Just invoke the skill directly.

11. **Treating audit as the final QA gate when refreshing live content.** ranking-status-check should be the gate — it tells you what scope of changes is safe.

12. **Forgetting to provide top 3 SERP competitors to audit's competitor_compare mode.** Without them, comparison is generic.

13. **Generating tutorials without code-generation-preview verification.** Broken code in tutorials damages credibility more than no tutorial.

14. **Generating comparison content where your brand is one of the options without disclosure.** comparison-post-mode requires disclosure when applicable. Don't bypass.

---

## Quick-Reference Tables

### Skills by Goal Type

| Goal | Primary skill | Supporting skills |
|---|---|---|
| Write new content | nexus-content | nexus-research (research first), nexus-audit (QA) |
| Audit existing content | nexus-audit | — (alone) |
| Optimize existing content | nexus-audit + nexus-content | — |
| Tech SEO audit | nexus-tech-seo | — (alone) |
| Diagnose ranking failure | nexus-audit + nexus-tech-seo | — |
| SEO strategy | nexus-strategy | nexus-research (informs strategy) |
| Topical authority | nexus-strategy + nexus-content | nexus-research (semantic SEO) |
| Portfolio maintenance | nexus-strategy + nexus-audit + nexus-content | — |
| Guest post | nexus-content (guest mode) | nexus-research (light) |
| Programmatic SEO | nexus-strategy + nexus-content | — |
| Keyword research only | nexus-research | — |
| Generate metadata | nexus-research (metadata mode) | — |

### Inputs Required by Skill

| Skill | Required | Recommended | Optional |
|---|---|---|---|
| nexus-tech-seo | At least one of: source HTML, GSC, PageSpeed | All three | robots.txt, sitemap, hreflang |
| nexus-audit | Content (URL or text), target keyword | Brand profile, top 3 SERP competitors | Author profile |
| nexus-research | Topic or keyword | Brand profile | Geo target, content type intent |
| nexus-content | Topic, brand profile | Research package, author profile | User research, user preferences |
| nexus-strategy | Brand profile | Existing data (memory) | Topic, time horizon |
| nexus-orchestrator | 2+ Nexus skills installed | Brand profile | Author profile, user preferences |

### Modes Quick Reference

| Skill | Default mode | Other useful modes |
|---|---|---|
| nexus-tech-seo | full_audit | core_web_vitals_only, gsc_insights_only, ranking_diagnosis, content_tech_bottleneck |
| nexus-audit | full_audit | quick_score, eeat_audit, fact_verification_only, helpful_content_check |
| nexus-research | full_research_package | keyword_research, content_outline, metadata_package |
| nexus-content | standard_blog | guest_post, tutorial, comparison_post, pillar_page, content_refresh |
| nexus-strategy | full_strategy | topical_authority, content_portfolio_status, ai_search_positioning |

---

## Troubleshooting

**Q: The skill is running brand-agnostic when I have a brand profile uploaded.**
A: Ensure the brand file is named `brand-[name].skill.md` (with the `.skill.md` extension and the `brand-` prefix). Re-upload to your Claude project.

**Q: Confidence gate is blocking my content. What do I do?**
A: Read the remediation list. Fix the specific issues (usually wrong facts, broken links, or insufficient differentiation). Re-run. Or override per-session: reply "override confidence gate" and content delivers with UNRELIABLE warning.

**Q: nexus-tech-seo says it can't analyze without inputs.**
A: Provide source HTML, GSC export, and/or PageSpeed JSON. The skill works with any combination but needs at least one to do anything useful.

**Q: nexus-content is producing content that doesn't sound like my voice.**
A: Create an author profile. Without one, content uses brand voice only (less specific). Author profile transforms voice quality.

**Q: nexus-audit's score is much lower than I expected.**
A: Read each section carefully. The scoring is honest — content that "feels good" can fail E-E-A-T (no first-person experience), differentiation (identical to top 10), or live-fact-verification (claims that can't be verified). Address the specific failures.

**Q: Pipeline accountability log shows skills were SKIPPED.**
A: Read the reason for each skip. Common reasons: missing required input, dependency on previous skill that didn't produce expected output, web_search unavailable. Provide the missing input and re-run.

**Q: How do I update from one version of a skill to a newer version?**
A: Each skill is independently versioned. Replace the old skill folder with the new one. Brand and author profiles are unaffected.

**Q: Can I use multiple brand profiles in one session?**
A: Yes. Upload both. Nexus detects multiple and asks which to use. Or say "switch brand to [name]" mid-session.

**Q: Does the orchestrator work without other Nexus skills installed?**
A: No. Orchestrator chains other skills — it has no value alone. Install at least 1-2 other Nexus skills first.

---

## Final Notes

The modular Nexus is designed so you only use what you need. Don't feel obligated to install all 6 skills if you only need one or two. A solo content auditor needs nexus-audit. A solo tech SEO consultant needs nexus-tech-seo. A content team writing new posts needs nexus-research + nexus-content. Match installation to actual workflow.

The orchestrator is convenience, not necessity. Manual invocation gives you more control and lighter context use. Automated chaining gives you speed at the cost of context.

Brand profiles are the highest-leverage one-time setup. Author profiles are second. User preferences are third. All optional but increasingly impactful.

Substance over word count. Verification over inference. Heads-up before action. Output blocks for accountability. These are the principles. Every skill follows them.

— End of user guide
