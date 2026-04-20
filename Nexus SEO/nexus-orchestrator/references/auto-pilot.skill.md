# SKILL: orchestration/auto-pilot.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 3.0.0
# LAYER: Orchestration / Autonomous Execution Controller
# EXECUTION PRIORITY: MANDATORY — This skill overrides all other orchestration logic. When any content, landing page, optimization, or audit request is detected, this skill takes control.
# POSITION: The autonomous execution engine. It tells you exactly what to do next, checks whether you did it, and blocks you from proceeding until deliverables are met.

---

## WHY THIS SKILL EXISTS

Nexus has 58 skills that produce exceptional content. But skills are useless if they don't run. Claude has a tendency to read pipeline descriptions and then skip to writing plain text. This skill prevents that.

**The rule is simple: you are not done until every mandatory step has produced its deliverable. There are no shortcuts. If a session runs out of context, you generate the handoff block. You never silently skip a step.**

---

## PIPELINE TYPE CLASSIFICATION

Every pipeline is either AUTONOMOUS or APPROVAL-REQUIRED:

| Pipeline Type | Behavior | Which Pipelines |
|---|---|---|
| **AUTONOMOUS** | Zero checkpoints. Run straight through. Deliver complete output. No stops. | Keyword research, competitor analysis, ranking diagnosis, competitor comparison, content improvement, content audit, content optimization |
| **APPROVAL-REQUIRED** | ONE checkpoint only. Pause for approval at the defined gate. Everything after the gate is autonomous. | Content creation (pause after brief), Landing page (pause after structure) |

**For AUTONOMOUS pipelines:** Do not ask the user anything mid-pipeline. Do not say "shall I continue?" Do not wait for input. Run every skill in sequence. Deliver complete output.

**For APPROVAL-REQUIRED pipelines:** Run to the checkpoint. Present the deliverable (brief or structure). Wait for approval. After approval, run everything else autonomously with zero stops.

---

## SESSION AWARENESS — CONTEXT-AWARE HANDOFF

When Nexus detects the context window is getting large mid-pipeline, generate this handoff block:

```
━━━━━━━━━━━━━━━━━━━━━━
NEXUS PIPELINE HANDOFF
━━━━━━━━━━━━━━━━━━━━━━
Brand: [brand.brand_name]
Pipeline: [pipeline name]
Keyword/URL: [target]
Completed phases: [list with key deliverables]
Remaining phases: [list]
All progress saved.

Start a new chat and paste this:
"Resume Nexus pipeline — [pipeline name] for [keyword/URL]
Brand: [brand.brand_name]
Completed: [phases done with deliverable summaries]
Resume from: [exact next step with step number]"
━━━━━━━━━━━━━━━━━━━━━━
```

The user pastes one block into a new chat. Nexus picks up from the exact step. No lost work.

**When to generate handoff:** When you estimate fewer than 20% of available context remains and there are still mandatory steps to complete. Never generate handoff prematurely — maximize what you complete in each session.

---

## PHASE 0 — PRE-FLIGHT CHECK (MANDATORY FOR ALL CONTENT)

**This phase runs BEFORE research. It prevents cannibalization, intent mismatches, and wrong-brand content.**

### Step 0.1 — Brand Classification
- Determine which brand from the active brand file (or ask the user if multiple brand files exist)
- Load the correct brand file and its associated context
- If ambiguous → ASK. Do not guess.
- Deliverable: Brand confirmed. Correct brand file loaded.

### Step 0.2 — Existing Content Check (Cannibalization Prevention)
- Fetch the sitemap or crawl /blog/ page of the target domain
- Fetch sitemap from brand.site_url/sitemap.xml or crawl brand.site_url/blog
- Extract all blog post titles and URLs
- Compare the target keyword against existing titles for semantic overlap
- Deliverable: Cannibalization check result
- If overlap found → FLAG with options: (a) merge, (b) differentiate angle, (c) proceed, (d) different keyword
- If no overlap → CLEAR
- NOT DONE UNTIL: You have fetched real URLs and compared. Do not skip this.

### Step 0.3 — SERP Intent Validation
- Search the target keyword on Google via web search
- Record what FORMAT dominates page 1: blog posts, product pages, tools, videos, comparison tables, landing pages
- Compare to what you're about to write
- Deliverable: Intent match result
- If mismatch → FLAG: "SERP shows [format] but you're writing [different format]."
- If match → CLEAR
- NOT DONE UNTIL: You have done a real web search and categorized the SERP format.

### Step 0.4 — Competitive Depth Assessment
- From top 3 SERP results, count technical depth signals:
  - Working code examples
  - Data tables with real numbers
  - Real benchmarks or performance data
  - Config file examples
  - CLI output snippets
  - Diagrams or infographics
- Record as the "depth benchmark"
- Deliverable: Depth benchmark table
- This is what you need to MATCH OR EXCEED. Not word count — depth.
- NOT DONE UNTIL: You have analyzed at least 3 competitor pages for depth.

**Phase 0 must CLEAR before Phase 1 begins.** If any flag is raised, resolve it with the user first.

---

## CONTENT-CREATE PIPELINE — MANDATORY EXECUTION SEQUENCE

When the user says anything that maps to CONTENT-CREATE ("write a blog on X", "create an article about X"). Execute in order. Do not skip. Do not reorder.

### PHASE 1: RESEARCH (after Phase 0 clears)

**Step 1.1 — SERP research**
- Load: `serp-intelligence.skill.md`
- Action: Web search target keyword. Fetch top 10 organic results. Record: titles, URLs, word counts, SERP features, intent classification, difficulty.
- Deliverable: SERP snapshot table.
- NOT DONE UNTIL: 5+ real SERP results analyzed.

**Step 1.2 — Deep competitor analysis**
- Load: `deep-serp-analysis.skill.md`
- Action: For top 5 results, fetch actual page content. Analyze: heading structure, media types, code examples count, data tables count, unique angles, weakest sections.
- Deliverable: Competitor analysis table with depth signals per competitor.
- NOT DONE UNTIL: 3+ competitor pages fetched and analyzed for technical depth.

**Step 1.3 — Google signals**
- Load: `google-signals-extractor.skill.md`
- Action: Capture PAA questions (depth 3-4 when web search allows, not just first 4), autocomplete, related searches.
- Deliverable: Approved signal list with filter reasoning.
- NOT DONE UNTIL: 5+ PAA questions or autocomplete terms documented.

**Step 1.4 [M1] — Content brief assembly**
- Load: `content-research-engine.skill.md`
- Action: Combine SERP + competitor depth + Google signals into Content Research Brief.
- Deliverable: Brief with keyword intelligence, SERP landscape, content gaps (min 5), differentiation opportunities (top 3), recommended H2 outline with media plan per section, depth targets (code examples needed, tables needed, data points needed).

**PHASE 1 CHECKPOINT (APPROVAL-REQUIRED):**
Present Content Research Brief to user. Ask: "Approve this brief to proceed, or tell me what to adjust." Wait for approval.

**After approval, everything is AUTONOMOUS. No more stops.**

---

### PHASE 2: CONTENT GENERATION (AUTONOMOUS after approval)

**Step 2.1 [M3] — Draft generation**
- Load: `content-generation-engine.skill.md` + brand content writer skill
- Action: Write full draft following approved blueprint. Apply brand voice rules AND these pre-writing constraints:
  1. Cut significance inflation — no "crucial," "essential," "game-changing"
  2. Use plain verbs — "use" not "leverage," "start" not "embark"
  3. End sentences at the fact — no trailing commentary
  4. Vary rhythm — mix short and long sentences deliberately
  5. Earn every adjective — if removing it doesn't change meaning, remove it
- Deliverable: Full article draft in markdown.
- NOT DONE UNTIL: Draft exists with all H2 sections populated.

**Step 2.2 [M4] — Code examples (DEFAULT ON for technical topics)**
- Load: `code-generation-preview.skill.md`
- Action: For EVERY technical section, write working code. Execute it. Capture real output. Include the actual terminal output or result — not "expected output: ..."
- Include realistic data in examples — real config values, real file paths, real test counts
- Deliverable: Working code blocks with actual output embedded in article.
- DEFAULT ON: This step runs for ANY topic involving tools, libraries, CLI commands, configs, or APIs. It is NOT optional. Only skip for purely strategic/non-technical content.

**Step 2.3 [M2] — Real-world examples and data**
- Load: `real-world-examples.skill.md`
- Action: For every claim, statistic, or benchmark — find real evidence. Real company names, real engineering blog posts, real conference talks, real GitHub repos, real performance numbers.
- No "Company A saw 40% improvement." Real: "Shopify's engineering team reduced their Playwright test suite execution from 45 minutes to 12 minutes using sharding across 8 workers."
- Deliverable: All examples verified with source links.
- NOT DONE UNTIL: Every major claim has a real source or is explicitly marked as illustrative.

**Step 2.4 — Tables and comparison data**
- Action: Convert all comparisons, feature lists, and data sets to tables with real data.
- Deliverable: Minimum 2 tables. 3+ for technical topics.

**Step 2.5 [M6] — Infographics**
- Load: `infographic-image-engine.skill.md` + brand design skill
- Action: Generate SVG infographics for major concepts.
- Deliverable: Minimum 2 SVGs with alt text.

**Step 2.6 [M5] — Engagement audit**
- Load: `content-engagement.skill.md`
- Action: Scan for text-heavy stretches. Zero 300+ word blocks without visual break.
- Deliverable: Engagement score + violations fixed.

**Step 2.7 — Callout boxes**
- Action: Insert 4-8 callout boxes (Tips, Warnings, Key Insights) at natural break points.
- Deliverable: Callout boxes placed using blockquote format.

---

### PHASE 3: LINKS, PRODUCT INTEGRATION, AND ACCURACY (AUTONOMOUS)

**Step 3.1 — Internal links**
- Load: `internal-linking.skill.md` + brand.urls from active brand file
- Action: Place internal links per brand.site_size:
  small (under 50 pages): 4-6 links
  medium (50-200 pages): 7-10 links
  large (200+ pages): 12+ links
  Read brand.site_size from active brand file before setting the target.
- CRITICAL: Source all internal links from brand.urls. ZERO invented URLs. ZERO cross-brand links.
- Deliverable: Internal links embedded per brand.site_size target.

**Step 3.2 — External links**
- Action: Add external links to all referenced tools, packages, official docs. Every external link gets `rel="nofollow"`.
- Deliverable: 5-10 external links with nofollow.

**Step 3.3 — Brand product mentions and CTAs**
- Read from active brand file:
    brand.mention_frequency  → sets how many mentions to insert (low=1-2, medium=3-5, high=6+)
    brand.mention_style      → sets how mentions are framed (read the full style instruction)
    brand.first_mention_format → sets how brand name appears on first reference
    brand.features           → check maturity labels before referencing any feature
                               STABLE = mention freely
                               BETA = mention with "currently in beta" qualifier
                               COMING_SOON = do not mention
                               UNRELIABLE = never mention, flag if found in draft
    brand.primary_cta_url + brand.utm_format → construct CTA with UTM parameters
- Action: Determine funnel stage. Insert mentions with specific features per maturity labels.
  Add 1 CTA with UTM params using brand.primary_cta_text and brand.primary_cta_url.
- Deliverable: Brand mentions + CTA embedded.

**Step 3.4 [M8] — Technical accuracy check**
- Load: `technical-accuracy-checker.skill.md`
- Action: Verify every technical claim. Cross-check against official docs.
- Deliverable: Accuracy report.

**Step 3.5 [M7] — Humanizer pass**
- Load: `humanizer.skill.md` + brand content writer 24-pattern framework
- Action: Three-pass humanization:
  1. **First pass:** Standard anti-AI audit — check all 24 patterns, fix violations, banned words, no em dashes
  2. **Self-criticism pass (NEW):** Re-read the output and ask: "What makes this obviously AI generated?" Identify and fix remaining tells — structural uniformity, paragraph length regularity, hedging patterns, vocabulary monotony
  3. **Keyword preservation check (NEW):** Verify primary keyword still appears in: title, H1, first 100 words, meta description. If humanizer removed keyword placements, restore them naturally.
- Deliverable: Humanizer report with pattern count, self-criticism findings, keyword status.

---

### PHASE 4: SCORING AND QUALITY GATES (AUTONOMOUS)

**Step 4.1 — SEO/AEO/GEO/SXO scoring**
- Load: `seo-aeo-geo-sxo-optimizer.skill.md`
- Action: Score across all 4 paradigms. Use technical depth metric (NOT word count). Check GEO auto-zero (AI crawlers blocked = GEO 0).
- Deliverable: 4-paradigm scores table.
- NOT DONE UNTIL: All 4 scores calculated. Below 90 → identify fixes.

**Step 4.2 — Fix loop**
- If any paradigm below 90 → apply fixes → re-score → repeat until 90+.
- Deliverable: Updated scores.

**Step 4.3 [M9] — Content validation**
- Load: `content-validation.skill.md`
- Action: Run 5-dimension quality gate: directness, rhythm, trust, authenticity, density. Each scored /10. Below 35/50 total = mandatory rewrite.
- Deliverable: Quality score + dimension breakdown.

**Step 4.4 — FAQ section**
- Action: If no FAQ exists, add one. Use PAA from Phase 1. Minimum 4 questions. Schema-ready format.
- Deliverable: FAQ section.

**Step 4.5 — Blog audit (optional)**
- Load: `blog-post-auditor.skill.md` + `scoring-rubric.skill.md`
- Action: Run 7-section forensic audit vs top 3 SERP competitors.
- Deliverable: Audit scores + priority fixes.

---

### PHASE 5: RANKING INTELLIGENCE, METADATA, AND DELIVERY (AUTONOMOUS)

**Step 5.1 — Metadata generation**
- Load: `metadata-generator.skill.md`
- Action: Generate title, meta description, slug, OG tags, excerpt.
- Auto-select schema by content type:
  - Blog post → `Article` + `FAQPage`
  - Tutorial/how-to → `HowTo` + `Article`
  - Comparison → `WebPage` + `ItemList`
  - Tool page → `SoftwareApplication`
  - Service page → `Service` + `Organization`
- Generate llms.txt entry as bonus deliverable.
- Deliverable: Complete metadata package with correct schema types.

**Step 5.2 [M10] — Ranking intelligence (MANDATORY — NEW in v3.0)**
- Load: `ranking-diagnostics.skill.md`
- Action: Calculate specific backlink targets:
  - Fetch DA and backlink data for top 5 SERP competitors
  - Calculate: average competitor DA, average referring domains, average page backlinks
  - Calculate: your current DA, your referring domains, your page backlinks
  - Output: specific numbers — "You need X backlinks from DA Y+ sites"
  - Output: technical improvement list — schema gaps, CWV issues, AI crawler access, canonical URLs
  - Output: confidence label on each recommendation (Confirmed/Likely/Hypothesis)
- Deliverable: Backlink Target Report + Technical Improvement List.
- NOT DONE UNTIL: Specific numbers provided, not vague "build more backlinks."

**Step 5.3 — Final content report**
- Load: `master-orchestrator.skill.md`
- Deliverable: Consolidated report with word count, media inventory, link inventory, all scores, backlink targets, remaining action items.

**Step 5.4 — File delivery**
- Save to outputs: blog post .md, SVG infographics, metadata package, backlink target report.
- Present files to user.

**FINAL CHECKPOINT:**
```
━━━━━━━━━━━━━━━━━━━━━━
NEXUS PIPELINE COMPLETE
━━━━━━━━━━━━━━━━━━━━━━
Brand: [brand.brand_name]
Article: [title]
Keyword: [keyword]
Word count: [N]
Depth: [X code examples, Y tables, Z data points]
Scores: SEO [X] | AEO [X] | GEO [X] | SXO [X]
Backlink target: [X] links from DA [Y]+ sites
DA target: [current] → [needed]
Technical fixes: [count]
Files delivered: [list]
Action items: [list or "None"]
━━━━━━━━━━━━━━━━━━━━━━
```

---

## LANDING-PAGE PIPELINE — MANDATORY EXECUTION SEQUENCE

When the user says "landing page" or provides a landing page template/URL.

### PHASE 0: PRE-FLIGHT
Same as content pipeline — brand, cannibalization, SERP intent. Plus:
- Detect landing page type: versus/comparison, alternatives, features, service
- This determines schema selection, section priorities, and superiority framing approach

### PHASE 1: INPUT & STRUCTURE

**Step 1.1 — Input mode detection**
Determine which mode:

**Mode A — User provides template:**
- Accept the template structure
- Load `landing-page-engine.skill.md`
- Audit template for conversion gaps (missing trust signals? no objection handling? weak CTA placement?)
- Present gap findings to user: "Your template is missing [X]. This typically reduces conversion by [Y]%."
- Deliverable: Approved structure + conversion gap notes.

**Mode B — User provides live URL:**
- Fetch the URL content
- Load `landing-page-engine.skill.md`
- Audit conversion effectiveness per section
- Identify: weak sections, missing sections, structural gaps
- Present: "Current structure has [X strengths] and [Y gaps]. Recommended changes: [list]."
- Deliverable: Conversion audit of existing page + improvement recommendations.

**Mode C — No template provided:**
- Load `landing-page-engine.skill.md`
- Propose structure based on landing page type
- Present to user for approval
- Deliverable: Proposed structure.

**PHASE 1 CHECKPOINT (APPROVAL-REQUIRED):**
Present structure (accepted, audited, or proposed) to user. Wait for approval.

**After approval, everything is AUTONOMOUS.**

### PHASE 2: CONVERSION CONTENT (AUTONOMOUS)

**Step 2.1 — SERP intelligence**
- Load: `serp-intelligence.skill.md` + `deep-serp-analysis.skill.md`
- Action: Analyze top 5 competitor landing pages for the target keyword. What sections do they have? Where do they position CTAs? What claims do they make?
- Deliverable: Competitor landing page analysis.

**Step 2.2 — Conversion-focused content writing**
- Load: `landing-page-engine.skill.md`
- Action: Write content for each section of the approved structure. Every section has a conversion job:
  - Hero: Hook with primary keyword + value prop
  - Problem: Agitate the pain the reader has
  - Solution: Position your tool/company as the answer
  - Comparison/features: Show clear superiority on dimensions that matter
  - Social proof: Real numbers, real companies, real results
  - FAQ: Handle top objections
  - CTA: Single clear action, repeated 2-3 times
- Apply superiority framing: honest competitive acknowledgment + decisive positioning on key buyer dimensions
- Deliverable: Complete landing page content in markdown.

### PHASE 3: LINKS, SCHEMA (AUTONOMOUS)

**Step 3.1 — Internal links**
- Brand-specific repository. ZERO cross-brand.
- Landing pages typically have fewer internal links than blogs (5-10 contextual links, not 25).

**Step 3.2 — Schema**
- Load: `metadata-generator.skill.md`
- Auto-select by landing page type:
  - Comparison/versus → `WebPage` + `FAQPage` + `Product` with reviews if applicable
  - Alternatives → `WebPage` + `ItemList` + `FAQPage`
  - Features → `SoftwareApplication` + `WebPage`
  - Service → `Service` + `Organization` + `FAQPage`
- NEVER use `Article` schema for landing pages.

### PHASE 4: SCORING (AUTONOMOUS)

**Step 4.1 — SEO/AEO/GEO/SXO**
- Load: `seo-aeo-geo-sxo-optimizer.skill.md`
- All 4 paradigms scored. Target 90+ each.

**Step 4.2 — CRO scoring**
- Load: `landing-page-engine.skill.md` CRO checklist
- Score conversion effectiveness across 10 dimensions. Target 90+.

### PHASE 5: RANKING INTELLIGENCE & DELIVERY (AUTONOMOUS)

**Step 5.1 — Backlink targets**
- Load: `ranking-diagnostics.skill.md`
- Calculate specific numbers: how many backlinks, from what DA, technical improvements needed.
- NOT DONE UNTIL: Specific numbers, not "build more backlinks."

**Step 5.2 — Delivery**
- Save: landing page content .md, metadata + schema, backlink target report
- Present files.

**FINAL CHECKPOINT:**
```
━━━━━━━━━━━━━━━━━━━━━━
NEXUS LANDING PAGE COMPLETE
━━━━━━━━━━━━━━━━━━━━━━
Brand: [brand.brand_name]
Page: [title]
Type: [versus / alternatives / features / service]
Keyword: [keyword]
Scores: SEO [X] | AEO [X] | GEO [X] | SXO [X] | CRO [X]
Backlink target: [X] links from DA [Y]+ sites
DA target: [current] → [needed]
Technical fixes: [count]
Files delivered: [list]
━━━━━━━━━━━━━━━━━━━━━━
```

---

## CONTENT-OPTIMIZE PIPELINE — MANDATORY EXECUTION SEQUENCE

When user says "optimize X," "improve X," "refresh X" + URL. AUTONOMOUS — zero stops.

1. Fetch and read existing content
2. Determine brand from URL domain
3. Run `blog-post-auditor.skill.md` + `scoring-rubric.skill.md`
4. Run `seo-aeo-geo-sxo-optimizer.skill.md` for current scores
5. Run `serp-intelligence.skill.md` for current SERP landscape
6. Apply fixes: content gaps, missing links, weak headings, AI pattern cleanup, engagement breaks
7. Run `humanizer.skill.md` with self-criticism pass
8. Re-score and verify all scores improved
9. Run `ranking-diagnostics.skill.md` — backlink targets (mandatory)
10. Deliver optimized article + before/after scores + backlink target report

---

## CONTENT-AUDIT PIPELINE — MANDATORY EXECUTION SEQUENCE

When user says "audit X," "score X," "review X," "what's wrong with X." AUTONOMOUS — zero stops.

1. Fetch and read content
2. Determine brand from URL domain
3. Run `serp-intelligence.skill.md` for primary keyword SERP snapshot
4. Run `deep-serp-analysis.skill.md` for competitor benchmarking
5. Run `blog-post-auditor.skill.md` + `scoring-rubric.skill.md` + `heading-rhetoric.skill.md`
6. Run `seo-aeo-geo-sxo-optimizer.skill.md` for 4-paradigm scores
7. Run `ranking-diagnostics.skill.md` — backlink targets + technical fixes (mandatory)
8. Deliver: 7-section audit, scores, priority fixes, rewritten H2s, backlink targets, technical improvements

---

## PROACTIVE DECISION-MAKING

This skill decides what the content needs. It does not wait for instructions.

| Content context | Proactive decision |
|---|---|
| Section explains a process | Flowchart infographic |
| Section compares options | Comparison table with real data |
| Section is technical | Working code example with real output |
| 300+ words without break | Insert appropriate visual element |
| Article references a brand feature | Check maturity, add contextual mention |
| Article references a tool/package | External link with nofollow |
| Article relates to another brand blog | Internal link (brand-specific only) |
| Article claims a fact or statistic | Find real source to back it up |
| Natural FAQ opportunity | FAQ section with schema-ready Q&A |
| Any technical topic | Code example with execution output (DEFAULT ON) |
| Any data claim | Real numbers from real companies with sources |

---

## WHAT "DONE" MEANS

### Blog Content — NOT done until ALL true:
- [ ] Phase 0 pre-flight completed (brand, cannibalization, SERP intent, depth benchmark)
- [ ] SERP research (5+ results)
- [ ] Content Research Brief approved
- [ ] Full draft with brand voice
- [ ] 2+ tables with real data
- [ ] 2+ infographics
- [ ] Working code examples with real output (if technical)
- [ ] Real examples: real companies, real numbers, real sources
- [ ] 4-8 callout boxes
- [ ] Internal links per brand.site_size from brand.urls (ZERO cross-brand, ZERO invented)
- [ ] 5+ external links with nofollow
- [ ] Brand mentions + 1 CTA with UTM
- [ ] Technical claims verified
- [ ] AI patterns cleaned (24-pattern + self-criticism + keyword preservation)
- [ ] 5-dimension quality gate passed (35/50 minimum)
- [ ] All 4 optimization scores 90+
- [ ] Zero 300+ word text stretches
- [ ] FAQ with 4+ questions
- [ ] Metadata with auto-selected schema (NOT default Article)
- [ ] Backlink target report with specific numbers
- [ ] Technical improvement list
- [ ] Files saved and presented

### Landing Page — NOT done until ALL true:
- [ ] Phase 0 pre-flight completed
- [ ] Structure approved (from template, URL audit, or proposed)
- [ ] Conversion intent in every section
- [ ] Tool/company positioned as superior
- [ ] Brand-specific links (ZERO cross-brand)
- [ ] Page-type schema (NOT Article)
- [ ] SEO/AEO/GEO/SXO scores 90+ each
- [ ] CRO score 90+
- [ ] Backlink target report with specific numbers
- [ ] Technical improvement list
- [ ] Files saved and presented

---

*Auto-Pilot Execution Engine — v3.0.0 | Nexus SEO Operating System | Pipelines: Content, Landing Page, Optimize, Audit*
