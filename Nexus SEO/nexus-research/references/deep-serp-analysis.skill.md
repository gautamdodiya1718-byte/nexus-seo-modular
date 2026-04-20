# SKILL: serp/deep-serp-analysis.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: SERP / Competitive Intelligence
# EXECUTION PRIORITY: POST-SERP-INTELLIGENCE — Runs after serp-intelligence.skill when difficulty is HARD, intent is MIXED, or confidence is LOW.
# POSITION: Page-level reverse engineering layer between SERP surface analysis and blueprint generation.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill performs **page-level reverse engineering** of the top SERP results for a target keyword.

Where `serp-intelligence.skill` reads the SERP from the outside — intent, format, feature landscape — this skill goes inside each ranking page. It systematically dissects the structure, content patterns, SEO signals, media usage, quality indicators, linking behavior, and formatting choices of every high-quality result in the result set. It then aggregates those observations into cross-page patterns and extracts differentiation opportunities that no individual page's analysis could reveal.

This is not surface-level analysis. This is **competitive content reverse engineering** — the process of understanding, at the page level, exactly what Google is rewarding and why, so that new content can be built to exceed those standards with precision.

### Primary Responsibilities

| Responsibility                       | Description                                                                                                         |
|--------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| **Result Collection**                | Gather top-20 SERP results with URL, title, domain, and position data                                              |
| **Result Filtering**                 | Remove low-value results (docs, product pages, thin content) to isolate 8–12 high-quality editorial pages          |
| **Per-Page Analysis**                | For every filtered page, analyze across 8 defined dimensions — no dimension skipped, no page skipped               |
| **Missing Element Detection**        | For each page, identify what is absent that a stronger version would include                                        |
| **Cross-Page Pattern Synthesis**     | Aggregate per-page observations into repeating patterns with frequency counts                                       |
| **Opportunity Extraction**           | Identify what zero or few competing pages are doing — these are the differentiation angles                         |
| **Pattern Weighting**                | Weight signals by frequency (≥7/10 = strong signal; ≤2/10 = opportunity signal; outliers excluded)                 |
| **Aggregated Insight Production**    | Synthesize cross-page data into actionable intelligence for downstream blueprint generation                        |

### What This Skill Is NOT

- It is **not** a SERP surface scanner. It analyzes individual pages — their internal structure, not just their external SERP appearance.
- It is **not** a backlink analyzer. Internal and external link counting is part of the analysis but domain authority assessment belongs to `authority-mapping.skill`.
- It is **not** a content generator. It extracts and synthesizes what exists — `blueprint-generator.skill` translates that into a content plan.
- It is **not** a replacement for `serp-intelligence.skill`. It runs after it, going deeper. The two skills are sequential layers, not alternatives.
- It is **not** optional when triggered. If this skill is dispatched, it analyzes ALL filtered pages across ALL 8 dimensions — no exceptions, no depth reduction.

### The Defining Standard of This Skill

> Every insight produced by this skill must be traceable to a specific page, a specific dimension, and a specific observation.
>
> No insight may be stated as a general principle or assumption.
> Every observation is tagged with how many of the analyzed pages share it.
> Every opportunity is defined as something the current top results are NOT doing — not something they are doing poorly.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input

| Field               | Description                                                                 | Required |
|---------------------|-----------------------------------------------------------------------------|----------|
| `target_keyword`    | The exact keyword phrase whose SERP is being reverse engineered             | YES      |

### Optional Input

| Field                       | Description                                                             | Used For                                                         |
|-----------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| `serp_intelligence_context` | Full output from `serp-intelligence.skill` for this keyword            | Pre-loading intent classification, feature map, opportunity signal to avoid redundant analysis |
| `domain`                    | The site this analysis serves                                           | Contextualizing link patterns relative to target domain          |
| `funnel_stage`              | TOFU / MOFU / BOFU (validated by serp-intelligence.skill)              | Filtering out pages that target a different funnel stage         |
| `competitor_domains`        | Specific competitor domains to track                                   | Flagging when a competitor appears and extracting their specific patterns |
| `filter_strictness`         | STRICT / NORMAL / RELAXED                                               | Controls the filter aggressiveness in Step 2                     |

### Input Pre-Conditions

1. **Is `target_keyword` present?** If not → halt. No analysis can proceed without a target keyword.
2. **Is `serp_intelligence_context` available?** If yes → load it. Do not re-run intent classification — inherit from serp-intelligence output. If no → run lightweight intent check before filtering (Step 1).
3. **Has this keyword been deep-analyzed before?** → Query `memory-controller.skill`. If analysis exists and is within freshness threshold (7 days) → offer cached result. If stale → proceed fresh.
4. **Is `filter_strictness` set?** If not set → default to NORMAL.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Deep SERP Analysis Report

```
NEXUS DEEP SERP ANALYSIS REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Keyword          : [exact keyword]
Analysis Timestamp      : [ISO 8601]
Pages Collected         : [count — from top 20]
Pages After Filter      : [count — 8–12 target]
Filter Mode             : [STRICT / NORMAL / RELAXED]
SERP Intelligence Loaded: [YES / NO]
Generated By            : serp/deep-serp-analysis.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — PER-PAGE ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH ANALYZED PAGE]

PAGE [N] — [Position #X]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
URL                     : [full URL]
Domain                  : [domain]
Title                   : [page title]
Position                : [#N in SERP]
Estimated Word Count    : [range: e.g., 2,000–2,500]

DIMENSION 1 — CONTENT STRUCTURE
  Estimated Word Count  : [range]
  Paragraph Length      : [SHORT (<50w avg) / MEDIUM (50-100w avg) / LONG (>100w avg)]
  H2 Count              : [N]
  H3 Count              : [N]
  H2:H3 Ratio           : [ratio]
  Structure Pattern     : [Linear / Hub-and-Spoke / Layered / FAQ-Dominant / Mixed]
  Intro Approach        : [Direct Answer / Context-Setting / Hook / Problem Statement]
  Conclusion Approach   : [Summary / CTA / Open / None Detected]
  Notable Structural Feature: [specific observation]

DIMENSION 2 — MEDIA
  Images                : [count] — Types: [icon / screenshot / infographic / photo / diagram]
  Videos                : [count] — Type: [embedded YouTube / native / none]
  GIFs                  : [count]
  Infographics          : [count]
  Custom Graphics       : [YES / NO]
  Media Density         : [LOW (<1 per 500w) / MEDIUM / HIGH (>1 per 300w)]
  Media Strategic Note  : [specific observation about how media is used]

DIMENSION 3 — CONTENT ELEMENTS
  Tables                : [count] — Type: [data / comparison / summary]
  Comparison Tables     : [count]
  Code Blocks           : [count]
  Callout Boxes         : [count] — Type: [tip / warning / note / stat highlight]
  Ordered Lists         : [count]
  Unordered Lists       : [count]
  Stat / Data Blocks    : [count]
  Download Elements     : [YES / NO — type if yes]
  Notable Element       : [specific observation]

DIMENSION 4 — SEO SIGNALS
  Keyword in H1         : [YES / NO / PARTIAL]
  Keyword in Intro      : [YES (first 100w) / YES (100-200w) / NO]
  Keyword in Meta Title : [YES / NO / PARTIAL]
  Semantic Keyword Usage: [RICH / MODERATE / SPARSE]
  Semantic Signals      : [list of related terms observed in headings or prominent text]
  Schema Signals        : [FAQ / HowTo / Article / Review / None / Unknown]
  Alt Text Usage        : [PRESENT / ABSENT / PARTIAL]
  Internal Anchor Text  : [Descriptive / Generic / Mixed]
  Notable SEO Signal    : [specific observation]

DIMENSION 5 — AEO SIGNALS
  Direct Answer in Intro: [YES — within 100 words / PARTIAL — indirect / NO]
  FAQ Section           : [YES / NO] — [count of questions if YES]
  PAA Coverage          : [FULL / PARTIAL / NONE] — [which PAA questions addressed]
  Summary / TLDR Block  : [YES / NO]
  Definition Block      : [YES / NO]
  Structured Answer Format: [YES (numbered/bulleted answer) / NO]
  Featured Snippet Optimization: [LIKELY TARGETING / PASSIVE / NOT TARGETING]
  Notable AEO Signal    : [specific observation]

DIMENSION 6 — QUALITY SIGNALS
  Original Examples     : [count] — [brief description of type]
  Case Studies          : [count]
  Statistics / Data     : [count] — [sourced / unsourced]
  Expert Quotes         : [count] — [attributed / unattributed]
  Original Insights     : [YES / NO] — [description if YES]
  Research References   : [count]
  Author Signal         : [Byline present / Authority bio / Generic / None]
  Trust Signals         : [Publisher badge / Verified sources / None]
  Depth Indicator       : [SURFACE / MODERATE / DEEP / EXHAUSTIVE]
  Notable Quality Signal: [specific observation]

DIMENSION 7 — LINKING
  Estimated Internal Links: [range: LOW <5 / MEDIUM 5-15 / HIGH >15]
  Internal Link Strategy: [Cluster-based / Random / Sidebar-only / Navigation-only]
  Estimated External Links: [count range]
  External Link Quality : [Authority sources / Mixed / Weak / None]
  Resource Section      : [YES / NO]
  Outbound Link Pattern : [Supporting evidence / Competitor linking / Affiliate / None notable]
  Notable Linking Signal: [specific observation]

DIMENSION 8 — FORMATTING
  Readability Level     : [HIGH (short sentences, clear) / MEDIUM / LOW (dense, complex)]
  Bullet Usage          : [HEAVY / MODERATE / LIGHT / NONE]
  Table of Contents     : [YES (sticky) / YES (static) / NO]
  Navigation Aids       : [Jump links / Progress bar / Breadcrumbs / None]
  Mobile Formatting     : [OPTIMIZED / STANDARD / POOR — signal only]
  White Space Usage     : [GENEROUS / AVERAGE / DENSE]
  Pull Quotes           : [YES / NO]
  Notable Formatting    : [specific observation]

MISSING ELEMENT ANALYSIS
  Missing vs. Topic Requirements:
    → [Element 1 missing — explanation of what gap this creates]
    → [Element 2 missing — explanation]
    → [Element 3 missing — explanation]
  Missing vs. User Intent:
    → [Intent need not served — description]
  Missing vs. AEO Requirements:
    → [AEO element absent — description]
  Missing vs. Quality Threshold:
    → [Quality gap — description]
  Improvement Opportunities:
    → [Specific improvement 1]
    → [Specific improvement 2]

PAGE SUMMARY SCORE
  Structure Strength    : [1–5]
  Media Richness        : [1–5]
  Content Elements      : [1–5]
  SEO Optimization      : [1–5]
  AEO Optimization      : [1–5]
  Content Quality       : [1–5]
  Link Profile          : [1–5]
  Formatting Quality    : [1–5]
  Overall Page Score    : [average of 8 dimensions, 1–5]
  Competitive Threat    : [HIGH / MEDIUM / LOW]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[END PAGE [N] — REPEAT FOR ALL FILTERED PAGES]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — AGGREGATED INSIGHTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CROSS-PAGE PATTERN TABLE

| Pattern                          | Pages Showing | Frequency | Signal Strength   | Classification    |
|----------------------------------|---------------|-----------|-------------------|-------------------|
| [observation]                    | [N/analyzed]  | [%]       | [HIGH/MED/LOW]   | [Standard/Opp]    |
| ...                              |               |           |                   |                   |

DOMINANT STRUCTURE PROFILE
  Modal Word Count Range  : [range]
  Modal H2 Count          : [N]
  Modal H3 Count          : [N]
  Dominant Structure Type : [pattern]
  Dominant Intro Style    : [style]
  Dominant Media Type     : [type]
  Dominant Content Element: [element]
  Universal SEO Signals   : [signals present in ≥8 of analyzed pages]
  Dominant Quality Level  : [surface / moderate / deep / exhaustive]
  Dominant Linking Style  : [style]
  Dominant Formatting     : [style]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — OPPORTUNITY EXTRACTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WHAT NO PAGE IS DOING (0/[N] pages)
  → [Opportunity 1] — [why this matters for ranking / user satisfaction]
  → [Opportunity 2] — [why this matters]
  → [Opportunity 3] — [why this matters]

WHAT FEW PAGES ARE DOING (1–2/[N] pages)
  → [Signal 1] — [present in: Page X, Page Y] — [strategic implication]
  → [Signal 2] — [present in: Page X] — [strategic implication]

DIFFERENTIATION ANGLES
  Primary Angle           : [specific differentiation strategy with rationale]
  Secondary Angle         : [specific differentiation strategy with rationale]
  Format Differentiation  : [what format shift creates competitive separation]
  Depth Differentiation   : [where going deeper creates competitive advantage]
  AEO Differentiation     : [answer optimization gaps competitors are missing]

CONTENT SUPERIORITY REQUIREMENTS
  To match the competition   : [minimum requirements for competitive parity]
  To beat the competition    : [what must be done beyond parity]
  Highest-value gaps to fill : [ranked list of missing elements by strategic value]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into              : content-pattern-extractor.skill
                          serp-blueprint-generator.skill
                          content-generation-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — COLLECT SERP RESULTS

Gather the top-20 organic results for the target keyword. These 20 results form the raw candidate pool from which the final analysis set is filtered.

**Data Extraction Per Result:**

For each of the top-20 positions, extract:

| Field          | Description                                                                     |
|----------------|---------------------------------------------------------------------------------|
| `position`     | Organic result position (1–20)                                                  |
| `url`          | Full URL of the result                                                           |
| `domain`       | Root domain (e.g., `hubspot.com`, not `blog.hubspot.com/article/path`)         |
| `title`        | The HTML title tag as it appears in the SERP                                    |
| `snippet`      | The meta description or auto-generated snippet visible in the result            |
| `result_type`  | Preliminary type classification: blog / product / docs / forum / landing / video |
| `estimated_age`| If date is visible in the snippet — record it. If not — note as UNKNOWN.       |

**Pre-Filter Annotations:**

Before calling `serp-filter.skill`, annotate each result with a preliminary flag:

| Flag            | Condition                                                                     |
|-----------------|-------------------------------------------------------------------------------|
| `KEEP_LIKELY`   | Blog, guide, tutorial, listicle, editorial content                            |
| `REMOVE_LIKELY` | Official docs, direct product pages, thin pages, login-required               |
| `BORDERLINE`    | Forum threads, community answers, aggregator pages, wiki pages                |
| `COMPETITOR`    | Matches a domain in the `competitor_domains` input — flag for priority tracking |

**SERP Context Inheritance:**

If `serp_intelligence_context` was provided as input:
- Do NOT re-run intent classification.
- Import the following directly from the context:
  - Dominant intent type
  - Active SERP features
  - Funnel stage (validated)
  - Opportunity signal rating
- These values inform the filter and opportunity extraction steps — they do not need to be re-derived.

---

### ▸ STEP 2 — FILTER RESULTS (via serp-filter.skill logic)

Apply structured filtering to reduce the 20-result candidate pool to 8–12 high-quality, analytically valuable pages. The goal is to isolate editorial content that Google is genuinely ranking for content quality — not for brand authority, navigational intent, or default resource inclusion.

**Filter Logic — Strict Removal Criteria:**

Remove any result that meets one or more of the following conditions:

| Removal Condition                                     | Rationale                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------|
| Official documentation (docs.*, developer.*, api.*)  | Ranks for brand trust and technical authority — not content quality signals        |
| Direct product or category pages (ecommerce or SaaS) | Ranks for transactional intent — different competitive dynamic                    |
| Thin content (estimated < 400 words based on snippet) | Insufficient content depth for meaningful dimension analysis                      |
| Login-required or paywalled content                  | Content structure cannot be analyzed                                              |
| Pure video pages (YouTube, Vimeo without transcript)  | Text dimension analysis is not applicable                                         |
| Auto-generated or scraped pages                      | No strategic content signal available                                             |
| Pages in a different language than the keyword        | Language mismatch — content patterns are not comparable                           |
| Pages at position 15–20 with no distinguishing signal | Distant positions with no clear quality merit add noise, not signal               |

**Filter Logic — Borderline Handling:**

Pages flagged as `BORDERLINE` (forums, wiki, aggregators) are handled based on filter strictness:

| Filter Mode   | Borderline Handling                                                                |
|---------------|------------------------------------------------------------------------------------|
| `STRICT`      | Remove all borderline pages unless they are the only available examples            |
| `NORMAL`      | Include borderline pages if they appear in positions 1–10; exclude if 11–20       |
| `RELAXED`     | Include all borderline pages; note their type in the per-page analysis             |

**Minimum Result Threshold:**

After filtering, if the remaining page count falls below 5:
- Relax filter to NORMAL (if currently STRICT).
- Relax filter to RELAXED (if currently NORMAL).
- Re-evaluate removed borderline pages and include the top-scoring ones.
- If still below 5 after relaxation → flag for failsafe handling (see Appendix A).

**Filter Output:**

```
FILTER RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Pages Received          : 20
  Removed — Docs        : [count]
  Removed — Product     : [count]
  Removed — Thin        : [count]
  Removed — Other       : [count]
  Borderline Included   : [count] — Mode: [STRICT/NORMAL/RELAXED]
  Borderline Excluded   : [count]
Pages for Analysis      : [count] (target: 8–12)
Filter Mode Applied     : [STRICT / NORMAL / RELAXED]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 3 — PER-PAGE ANALYSIS

For every page that survived the filter, execute a full 8-dimension analysis. No page is skipped. No dimension is skipped within any page. Analysis depth is invariant across all pages.

The following sub-sections define every dimension with complete rigor.

---

#### ▸ DIMENSION 1 — CONTENT STRUCTURE

**Purpose:** Understand the architectural organization of the page — how content is partitioned, at what depth, and with what structural logic.

**Data Points Extracted:**

**Word Count Estimation:**
Estimate word count range from:
- Snippet length and density signals
- Number of visible headings
- Page load indicators
- Structural breadth (breadth of topic coverage visible in title + snippet)

| Estimated Word Count Range | Label         |
|----------------------------|---------------|
| < 800 words                | THIN          |
| 800–1,500 words            | SHORT         |
| 1,500–2,500 words          | MEDIUM        |
| 2,500–4,000 words          | LONG          |
| 4,000–6,000 words          | VERY LONG     |
| > 6,000 words              | COMPREHENSIVE |

**Paragraph Length Assessment:**
Estimate average paragraph length from snippet structure:
- If snippet text runs in dense blocks → LONG paragraphs (>100w avg)
- If snippet text shows frequent breaks → SHORT paragraphs (<50w avg)
- Default → MEDIUM (50–100w avg)

**Heading Counts:**
Estimate H2 and H3 counts from:
- Visible subheadings in snippet
- Structural signals in title + PAA coverage
- Page depth indicators from snippet length vs. topic breadth

**Structure Pattern Classification:**

| Pattern Type         | Definition                                                                           |
|----------------------|--------------------------------------------------------------------------------------|
| **Linear**           | Single narrative flow — one section leads directly to the next without branching    |
| **Hub-and-Spoke**    | Central topic with radiating sub-topics, each covered at consistent depth           |
| **Layered**          | Hierarchical depth — top-level overview followed by progressively deeper sub-sections |
| **FAQ-Dominant**     | Primary structure is a series of questions with answers                             |
| **Mixed**            | Combines two or more patterns in the same piece                                     |

**Intro Approach Classification:**

| Approach              | Signal                                                                     |
|-----------------------|----------------------------------------------------------------------------|
| **Direct Answer**     | First 1–2 sentences directly answer the query                             |
| **Context-Setting**   | First paragraph establishes context before addressing the query           |
| **Hook**              | Opens with a surprising stat, question, or claim to engage before answering |
| **Problem Statement** | Opens by defining the problem the reader faces before offering the solution |

---

#### ▸ DIMENSION 2 — MEDIA

**Purpose:** Identify how the page uses visual and multimedia elements to enhance comprehension, engagement, and ranking signal.

**Data Points Extracted:**

**Image Count and Type:**

| Image Type          | What It Signals                                                            |
|---------------------|----------------------------------------------------------------------------|
| **Screenshot**      | Practical proof; tutorial or how-to content                               |
| **Infographic**     | Data visualization; high backlink-earning potential                       |
| **Custom Diagram**  | Original visual explanation; strong authority signal                      |
| **Stock Photo**     | Low content signal; decorative only                                       |
| **Icon / Bullet Art**| Light visual organization; common in listicles                          |
| **Chart / Graph**   | Data-backed content; high credibility signal                              |

**Media Density:**
- HIGH: More than 1 media element per 300 words
- MEDIUM: 1 per 300–500 words
- LOW: Fewer than 1 per 500 words

**Strategic Media Note:**
Record how media is used — is it decorative (low value) or explanatory (high value)? Is it custom-created or stock? Does it appear at key conceptual transitions?

---

#### ▸ DIMENSION 3 — CONTENT ELEMENTS

**Purpose:** Catalog the structural content elements beyond prose — the formatting constructs that add scan value, reference value, and SERP feature eligibility.

**Data Points Extracted:**

**Tables:**
- Count all tables.
- Classify type: data table (rows of facts), comparison table (product A vs. B vs. C), summary table (condensed section recap).
- Tables are the most powerful content element for featured snippet capture in SERP positions that reward structured data.

**Comparison Tables:**
- Track separately from general tables.
- Comparison tables are high-value signals for Commercial intent SERPs.
- Record column count and structure (feature-by-feature, score-based, binary yes/no).

**Code Blocks:**
- Track presence and count.
- Code blocks signal Technical intent and Developer audience.
- High code block count → Technical Guide format confirmed.

**Callout Boxes:**
- Classify type: Tip, Warning, Note, Important, Stat Highlight, Quote Highlight.
- Callout boxes increase readability score and break information density.

**Lists:**
- Count ordered (numbered) and unordered (bulleted) lists separately.
- High ordered list count → Tutorial / How-To structure signal.
- High unordered list count → Listicle or Reference structure signal.

**Download Elements:**
- Track presence of downloadable templates, checklists, PDFs, or tools.
- Downloads are strong engagement signals and common in high-performing resource pages.

---

#### ▸ DIMENSION 4 — SEO SIGNALS

**Purpose:** Evaluate how deliberately and effectively the page has been optimized for the target keyword and its semantic field.

**Data Points Extracted:**

**Keyword in H1:**
- YES: Exact keyword present in H1.
- PARTIAL: Keyword partially present (some words from keyword, not all).
- NO: H1 does not contain the keyword.

**Keyword in Intro:**
- YES (first 100 words): Strong optimization signal.
- YES (100–200 words): Moderate optimization signal.
- NO: Keyword not in the intro — may be targeting a variation.

**Semantic Keyword Usage:**
Assess the density and variety of topically related terms in the visible content:
- RICH: Many related terms visible in headings and snippet text — strong topical coverage.
- MODERATE: Some related terms present.
- SPARSE: Content appears to rely on exact keyword repetition with limited semantic variety.

**Schema Signals:**
Infer likely schema from visible SERP features and page type:
- FAQ schema → FAQ section visible, PAA populated by this page.
- HowTo schema → Steps and outcomes visible, structured tutorial.
- Article schema → Article byline, publication date, author visible.
- Review schema → Star rating, review count visible in result.

**Internal Anchor Text Quality:**
- Descriptive: Anchor text describes the destination page's topic.
- Generic: Anchor text is "click here," "read more," or similar.
- Mixed: Combination of both.

---

#### ▸ DIMENSION 5 — AEO SIGNALS

**Purpose:** Determine how well the page is optimized for Answer Engine Optimization — the capacity to be selected as a direct answer by Google's featured snippets, AI overviews, and voice search engines.

**Data Points Extracted:**

**Direct Answer in Intro:**
- YES (within 100 words): Page opens with a concise, direct answer to the implied question of the keyword. This is the single strongest signal for featured snippet capture.
- PARTIAL: Answer is present but buried or indirect.
- NO: Page does not lead with a direct answer.

**FAQ Section:**
- Record presence, position on page (above fold / below fold / at end), and count of questions.
- Record whether FAQ questions align with PAA questions for this keyword.

**PAA Coverage:**
Using the PAA questions from `serp-intelligence.skill` context (if available):
- FULL: Page addresses all visible PAA questions.
- PARTIAL: Page addresses some PAA questions.
- NONE: Page does not address PAA questions.
Record which specific PAA questions are addressed.

**Summary / TLDR Block:**
- Track presence of a summary box, TLDR section, or key points block at the top or bottom of the article.
- Summary blocks are strong featured snippet and AI overview targets.

**Structured Answer Format:**
- YES: The answer is delivered in a numbered list, bulleted list, or table — formats that Google's algorithms can directly extract for featured snippets.
- NO: Answer is delivered in prose only.

**Featured Snippet Optimization Assessment:**
- LIKELY TARGETING: Direct answer in intro + structured format + FAQ section present.
- PASSIVE: Has some elements but not deliberately optimized.
- NOT TARGETING: No apparent featured snippet optimization.

---

#### ▸ DIMENSION 6 — QUALITY SIGNALS

**Purpose:** Assess the intellectual and evidential depth of the content — what separates generic content from genuinely authoritative, trustworthy, and original work.

**Data Points Extracted:**

**Original Examples:**
- Count and describe type (product screenshots, use case scenarios, worked examples, analogies).
- Original examples signal first-hand experience or deep domain expertise.

**Case Studies:**
- Count. Note whether they are first-party (the publisher's own cases) or third-party (referenced external cases).
- First-party case studies are the highest quality signal.

**Statistics and Data:**
- Count data points used.
- Classify as sourced (linked to a research paper, study, or authoritative source) or unsourced (stated without attribution).
- Sourced statistics signal trustworthiness; unsourced may be fabricated.

**Expert Quotes:**
- Count. Classify as attributed (named source, role, organization) or unattributed.
- Attributed expert quotes signal editorial process and authority.

**Original Insights:**
- YES: The page contains observations, conclusions, or frameworks not found in other ranking pages.
- NO: Content is a synthesis or paraphrase of commonly available information.
- Original insights are the rarest and most valuable quality signal.

**Depth Indicator:**
Based on the aggregate of the above quality signals:
- SURFACE: Content is generic, no original evidence, no depth signals.
- MODERATE: Some statistics, some examples, reasonable coverage.
- DEEP: Original examples, sourced data, expert input, comprehensive coverage.
- EXHAUSTIVE: First-party research, original frameworks, case studies, expert interviews.

---

#### ▸ DIMENSION 7 — LINKING

**Purpose:** Understand how the page uses internal and external links to build topical authority signals and serve user navigation.

**Data Points Extracted:**

**Internal Link Count:**
Estimate from snippet signals and page structure. Classify:
- LOW: < 5 internal links estimated.
- MEDIUM: 5–15 internal links.
- HIGH: > 15 internal links.

**Internal Link Strategy:**
- Cluster-based: Links connect to thematically related content within the same topic cluster.
- Random: Links appear unrelated to the page's topic.
- Sidebar-only: Links appear in navigation sidebar, not within body content.
- Navigation-only: Only header/footer navigation links visible.

**External Link Count and Quality:**
- Count estimated external links.
- Classify quality: Authority sources (academic, government, major publications) / Mixed / Weak sources / None.

**Resource Section:**
- YES: Dedicated section listing external resources, tools, or further reading.
- NO: External links embedded inline without a dedicated section.
- Resource sections are strong authority signals and common in high-quality educational content.

**Outbound Link Pattern:**
Classify the purpose of external links:
- Supporting Evidence: Links to sources that back up claims.
- Competitor Linking: Links to competing tools or products (rare; signals objectivity).
- Affiliate: Links with affiliate tracking parameters.
- None Notable: No significant outbound link pattern.

---

#### ▸ DIMENSION 8 — FORMATTING

**Purpose:** Evaluate how well the page's visual presentation and organization serve readability, user experience, and scanning behavior.

**Data Points Extracted:**

**Readability Level:**
- HIGH: Short sentences, clear language, generous paragraph breaks, consistent formatting.
- MEDIUM: Mix of simple and complex sentences; some long paragraphs; inconsistent formatting.
- LOW: Dense prose, long paragraphs, complex sentence structure throughout.

**Bullet Usage:**
- HEAVY: >5 bullet list sections in the page.
- MODERATE: 2–5 bullet list sections.
- LIGHT: 1 bullet list section.
- NONE: No bullet lists.

**Table of Contents:**
- YES (sticky): TOC remains visible as the user scrolls (high UX signal).
- YES (static): TOC present at top of page but does not follow scroll.
- NO: No table of contents.

**Navigation Aids:**
- Jump links: In-page anchor links from TOC headings.
- Progress bar: Scroll progress indicator.
- Breadcrumbs: Hierarchical site navigation above the article.
- None: No navigation aids beyond standard header.

**Pull Quotes:**
- YES: Text callouts that highlight key statements for scanners.
- NO: No pull quotes.

**White Space Usage:**
- GENEROUS: Clear space between sections; breathing room between paragraphs.
- AVERAGE: Standard spacing.
- DENSE: Tight spacing; content feels compressed.

---

### ▸ STEP 4 — MISSING ELEMENT DETECTION

For each page that has been analyzed across all 8 dimensions, identify what is absent relative to:

1. **Topic requirements:** What elements would a genuinely comprehensive treatment of this topic include?
2. **User intent requirements:** What does the user need to fully satisfy their search intent that this page does not provide?
3. **AEO requirements:** What answer optimization elements are missing that would make this page eligible for featured snippets or AI overviews?
4. **Quality threshold:** What quality signals are present in other top-ranking pages that this page lacks?

**Missing Element Classification:**

| Missing Element Type       | Detection Method                                                                  |
|----------------------------|-----------------------------------------------------------------------------------|
| **Structural gap**         | Topic requires a section that is absent from the page's heading structure        |
| **Format gap**             | Intent requires a format element (table, checklist) that is not present          |
| **Evidence gap**           | Claim is made without supporting data, example, or source                        |
| **AEO gap**                | No direct answer in intro, no FAQ, no structured format for snippet capture      |
| **Audience gap**           | Content doesn't address the specific audience segment the keyword implies         |
| **Depth gap**              | Coverage is present but shallow — topic is touched but not explored              |
| **Update gap**             | Content appears outdated relative to the topic's current state                   |

For each missing element, record:
1. What is missing (specific, named element).
2. Why it matters (what user need or ranking signal it would serve).
3. How it could be addressed (what form the missing element would take).

---

### ▸ STEP 5 — CROSS-PAGE PATTERN SYNTHESIS

After all individual pages have been analyzed, aggregate the per-page data into cross-page patterns. This is the step where individual observations become strategic intelligence.

**Synthesis Protocol:**

**Phase 5A — Pattern Frequency Counting**

For every data point extracted across all 8 dimensions, count how many of the analyzed pages share it:

| Pattern Example                      | Count Out of [N] Analyzed | Frequency % |
|--------------------------------------|--------------------------|-------------|
| Direct answer in intro               | 7/9                      | 78%         |
| Table of contents present            | 5/9                      | 56%         |
| FAQ section present                  | 8/9                      | 89%         |
| Word count > 2,500                   | 6/9                      | 67%         |
| Custom images present                | 3/9                      | 33%         |

**Phase 5B — Signal Strength Classification**

Apply the pattern weighting system:

| Frequency                           | Signal Label        | Action                                               |
|-------------------------------------|---------------------|------------------------------------------------------|
| ≥ 7/10 pages (or ≥70% of analyzed) | **STRONG signal**   | Must be present in new content — non-negotiable      |
| 4–6/10 pages (40–69%)              | **MODERATE signal** | Should be present — competitive parity element       |
| 2–3/10 pages (20–39%)              | **WEAK signal**     | Consider including — emerging differentiator         |
| 1/10 pages (< 20%)                 | **OUTLIER**         | Exclude from pattern conclusions; log separately     |

**Phase 5C — Dominant Structure Profile Construction**

From the frequency-weighted data, identify the modal (most common) value for each key structural attribute:

- Modal word count range (most common among STRONG signal pages)
- Modal H2 count
- Modal H3 count
- Dominant structure type (Linear / Hub-and-Spoke / Layered / FAQ-Dominant)
- Dominant intro style
- Dominant media type
- Dominant content element (table / list / callout)
- Universal SEO signals (present in ≥8 of analyzed pages)
- Dominant quality level
- Dominant formatting style

**Phase 5D — Outlier Documentation**

For every data point that appears in only 1 of the analyzed pages:
- Record it in the Outlier Log.
- Do NOT include it in pattern conclusions.
- Note it in the opportunity extraction step if the outlier represents a differentiation angle.

---

### ▸ STEP 6 — OPPORTUNITY EXTRACTION

Using the cross-page pattern data from Step 5, identify what the current competitive set is NOT doing — the gaps that a new, superior piece of content can exploit.

**Phase 6A — Zero-Presence Opportunities**

Identify elements, structures, formats, or quality signals that appear in ZERO of the analyzed pages.

For each zero-presence opportunity:
- Name it precisely.
- Explain why it matters for the topic and the user.
- Explain why its absence is exploitable — why including it creates competitive advantage.

**Zero-Presence Detection:** Scan all 8 dimension data points. Any attribute with a page count of 0 is a zero-presence opportunity.

**Phase 6B — Low-Presence Opportunities**

Identify elements present in 1–2 of the analyzed pages.

For each low-presence opportunity:
- Name it and identify which page(s) use it.
- Explain the strategic implication of including it in new content.
- Assess whether it is a weak signal or an emerging competitive differentiator.

**Phase 6C — Differentiation Angle Synthesis**

From zero-presence and low-presence opportunities, synthesize 3–5 concrete differentiation angles — specific strategies a new piece of content can employ to create genuine competitive separation:

**Differentiation Angle Format:**

```
Differentiation Angle [N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Angle Type          : [Format / Depth / AEO / Audience / Evidence / Structure]
Description         : [what the angle is — specific and concrete]
Gap Evidence        : [how many competing pages have this — 0/N or 1-2/N]
Competitive Benefit : [what ranking or user benefit this angle provides]
Execution Requirement: [what the content team must produce to implement this angle]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 6D — Content Superiority Requirements**

Define two tiers of requirements for new content:

**Tier 1 — Parity Requirements (Must Match):**
Everything with a STRONG signal (≥7/10 pages) is a parity requirement. New content that fails to include these elements will be at a structural disadvantage from the start. List every STRONG signal as a parity requirement.

**Tier 2 — Superiority Requirements (Must Exceed):**
The specific zero-presence and low-presence opportunities that, if executed well, create a content piece that demonstrably outperforms the current competition. List every differentiation angle as a superiority requirement.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ PATTERN WEIGHTING SYSTEM

The analysis must apply weighted evaluation to all cross-page patterns. Not all signals carry equal weight.

**Two-Axis Weighting:**

**Axis 1 — Frequency Weight:**

| Frequency               | Weight Multiplier |
|-------------------------|-------------------|
| ≥7 of analyzed pages   | 2.0×              |
| 4–6 of analyzed pages  | 1.5×              |
| 2–3 of analyzed pages  | 1.0×              |
| 1 of analyzed pages    | 0.0× (outlier)    |

**Axis 2 — Position Weight:**

| Page Position in SERP   | Weight Multiplier |
|-------------------------|-------------------|
| #1–3                    | 2.0×              |
| #4–6                    | 1.5×              |
| #7–10                   | 1.0×              |
| #11–20 (if included)    | 0.5×              |

**Combined Weight:**
For each observed pattern, the combined weight = Frequency Weight × Position Weight of the highest-ranking page displaying the pattern.

Patterns with combined weight ≥ 2.0 → REQUIRED for new content (parity minimum).
Patterns with combined weight 1.0–1.9 → RECOMMENDED for new content.
Patterns with combined weight < 1.0 → OPTIONAL differentiation signal.

---

### ▸ OUTLIER ISOLATION PROTOCOL

Outlier results — pages that appear in the SERP for reasons unrelated to content quality (brand authority, age, backlink equity) — must be identified and isolated so they do not corrupt the pattern data.

**Outlier Detection Signals:**

| Signal                                                            | Action                                            |
|-------------------------------------------------------------------|---------------------------------------------------|
| Domain authority significantly higher than all other results      | Analyze page but weight at 0.5× across all dimensions |
| Page is significantly older than all other results (5+ years)     | Analyze page but flag all signals as DATED         |
| Page is dramatically shorter or longer than all other results      | Analyze but flag as FORMAT OUTLIER                |
| Page covers a significantly different sub-topic (keyword mismatch) | Exclude entirely after filtering                  |
| Page's ranking appears to rely entirely on brand name in query    | Exclude from pattern analysis; keep in coverage count |

Outlier pages are analyzed (not skipped) but their dimension data is weighted down before entering the cross-page pattern synthesis.

---

### ▸ SERP SHIFT DETECTION AT PAGE LEVEL

Extend the SERP shift detection from `serp-intelligence.skill` down to the page level.

**Page-Level Shift Signals:**

| Signal                                                              | Implication                                                            |
|---------------------------------------------------------------------|------------------------------------------------------------------------|
| Multiple pages with very recent publication dates (< 3 months)      | SERP is rewarding freshness — new content has timing advantage         |
| Top-ranked pages have not been updated recently despite high age     | Stale incumbents — depth and freshness are the primary attack vector   |
| Page-level quality signals vary widely across the result set        | SERP is not settled — quality is the primary differentiator right now  |
| Top 3 results share the same structural template                    | Google has a preferred format for this keyword — deviation carries risk |
| Top 3 results have widely different structures                      | No clear format winner — room for format innovation                    |

Record page-level shift signals in the aggregated insights section.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Per-Page Observations in Aggregated Insights

If the same observation is made about multiple individual pages during per-page analysis, it is recorded once per page in Part 1. In Part 2 (aggregated insights), it appears once as a pattern with a frequency count. It must not be repeated in both the pattern table AND the dominant structure profile as separate entries.

### Rule 2 — No Duplicate Dimension Entries Within a Page

Each data point within a page's analysis belongs to exactly one dimension. If the same element (e.g., a comparison table) could be classified under Dimension 3 (Content Elements) and mentioned in Dimension 4 (SEO Signals), it is recorded in its primary dimension (Dimension 3) and referenced by name in the other.

### Rule 3 — No Overlapping Opportunity Signals

Zero-presence opportunities and low-presence opportunities in Part 3 must be distinct from each other. If the same gap appears at both levels (0/N pages for one aspect, 1/N for another aspect of the same element), consolidate into a single opportunity entry with the combined observation.

### Rule 4 — No Generic Insight Duplication Across Dimensions

If a high-level insight (e.g., "content depth is insufficient") can be derived from multiple dimensions (low quality signals in D6 AND thin structure in D1 AND sparse elements in D3), it is stated once in the aggregated insights as a synthesis conclusion — not repeated as a separate finding in each dimension's summary.

### Rule 5 — Memory Deduplication

Query `memory-controller.skill` before producing output. If any deep SERP analysis has been run for this keyword within the freshness threshold (7 days):
- Load the cached analysis.
- Present it to the user with its timestamp.
- Ask: "Use existing analysis or run fresh?" Wait for response.
- Never store two analyses for the same keyword in the same freshness window.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Analysis Behaviors

| Prohibited Behavior                                                                          | Reason                                                                          |
|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Stating "this page is well-optimized" without dimension-level evidence                       | Not an insight. Every quality statement must cite a specific dimension finding. |
| Recording "content is comprehensive" without citing word count range, H count, and element count | "Comprehensive" is not an observation. Numbers are observations.             |
| Skipping any dimension for any page                                                          | All 8 dimensions are mandatory for every analyzed page — no exceptions.        |
| Skipping any page that passed the filter                                                     | All filtered pages are analyzed — no page is too low in position to skip.      |
| Counting a pattern as STRONG if it appears in fewer than 7 of the analyzed pages            | Pattern weighting rules are precise — do not inflate signal strength.          |
| Identifying an "opportunity" that multiple competitors are already doing well               | An opportunity is what the competition is NOT doing — not what they're doing poorly. |
| Making an assumption about content that is not inferable from available signals             | Every insight must trace to observable signals — not assumptions about unseen content. |
| Producing the aggregated insights before completing all per-page analyses                    | Aggregation is a post-analysis step — never pre-aggregate.                     |
| Using the same phrasing for missing elements across multiple pages                          | Missing elements are page-specific — each page's gap description must be unique. |
| Recording SERP feature data in per-page analysis (it belongs in serp-intelligence.skill)    | Per-page analysis covers page-level signals — SERP features are result-set signals. |

### Required Analysis Behaviors

- Every per-page dimension section must have all labeled fields populated — no field may be left blank.
- Every pattern in the cross-page pattern table must specify the exact page count.
- Every differentiation angle must cite its gap evidence (how many pages — 0 or 1–2).
- Every parity requirement must be traceable to a STRONG signal (≥7/10 pages) in the pattern table.
- Every superiority requirement must be traceable to a zero-presence or low-presence opportunity.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source (feeds into this skill)                                              |
| **Receives**     | SERP intelligence context (intent, features, opportunity signal, funnel validation)          |
| **Inheritance**  | Intent classification, funnel stage, PAA questions — imported directly, not re-derived       |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before analysis, WRITE after completion                                |
| **Reads**        | Prior deep SERP analyses for this keyword (freshness check), competitor domain list          |
| **Writes**       | Complete deep SERP analysis report, per-page scores, opportunity extraction                  |
| **Memory layer** | strategy-memory.skill                                                                         |
| **Timing**       | READ at input pre-condition. WRITE after Step 6 completes.                                  |

---

### ▸ content-pattern-extractor.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | deep-serp-analysis → content-pattern-extractor (primary output consumer)                    |
| **Sends**        | Per-page dimension data, cross-page pattern table, dominant structure profile               |
| **Used for**     | Extracting recurring content patterns for structural blueprint generation                   |

---

### ▸ serp-blueprint-generator.skill (article-blueprint.skill)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | deep-serp-analysis → blueprint generator (output consumer)                                  |
| **Sends**        | Parity requirements, superiority requirements, differentiation angles, dominant structure   |
| **Used for**     | Building a content blueprint that directly reflects the competitive reality of the SERP     |
| **Override rule**| Deep SERP parity requirements override any conflicting blueprint assumptions                |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | deep-serp-analysis → content-generation-engine (indirect, via blueprint)                   |
| **Sends**        | Structural requirements, quality signal targets, AEO requirements, missing element list     |
| **Used for**     | Ensuring generated content meets or exceeds the competitive bar set by the SERP             |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | deep-serp-analysis → gap-opportunity-engine (opportunity data consumer)                     |
| **Sends**        | Zero-presence opportunities, low-presence opportunities, page-level competitive gaps         |
| **Used for**     | Enriching opportunity scoring with page-level gap intelligence                              |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Fewer than 5 pages remain after STRICT filter                             | Post-filter count < 5                                  | Relax to NORMAL filter. Re-evaluate borderline pages. Include top-scoring borderlines.                   |
| Fewer than 5 pages remain after NORMAL filter                             | Post-filter count < 5 after relaxation                 | Relax to RELAXED filter. Include all borderline pages. Flag output as RELAXED FILTER — results may include lower-quality signals. |
| Fewer than 3 pages remain after RELAXED filter                            | Post-filter count < 3                                  | Halt full analysis. Notify user: "Insufficient high-quality results for deep analysis. SERP may be too thin for this keyword. Recommend: (1) target a broader keyword variant, or (2) proceed with 3-page limited analysis with explicit caveat." |
| A page's content is inaccessible (login-required discovered post-filter)  | Dimension analysis returns no signals for a page       | Remove page from analysis set. Expand to next-highest result. Log removal.                              |
| SERP intelligence context not provided — no intent data                  | `serp_intelligence_context` = NULL                     | Run lightweight intent check using keyword signal analysis before filtering. Log: "SERP context not inherited — intent inferred from keyword." |
| Cross-page synthesis produces fewer than 3 STRONG signals                | Phase 5B frequency counting yields < 3 patterns ≥7/10 | Flag: "SERP lacks dominant structural patterns — this may indicate high content diversity or algorithmic flux." Proceed with MODERATE signals as primary guidance. |
| No zero-presence opportunities found                                      | Phase 6A scan finds all elements present in ≥1 page   | Shift to low-presence opportunities as the primary differentiation source. Flag: "No zero-presence opportunities detected — competition covers most angles. Differentiation must come from depth and quality, not presence/absence of elements." |
| Memory controller unavailable                                             | READ query at pre-condition returns error              | Proceed without freshness check. Flag: "Memory unavailable — duplicate analysis risk exists." Log for manual review. |
| Per-page score averages are all HIGH (≥4.0/5.0)                          | Post-analysis scoring reveals uniformly strong field   | Flag: "High-quality uniform SERP detected — competitive barrier is high." Elevate superiority requirements. Explicitly state that content superiority requires original research or first-party data. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — DIMENSION ANALYSIS QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
8-DIMENSION ANALYSIS CHECKLIST (PER PAGE)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
D1 — Content Structure
  □ Word count range
  □ Paragraph length
  □ H2 count
  □ H3 count
  □ Structure pattern
  □ Intro approach
  □ Conclusion approach

D2 — Media
  □ Image count + types
  □ Video presence
  □ GIF presence
  □ Infographic presence
  □ Custom graphics
  □ Media density

D3 — Content Elements
  □ Table count + types
  □ Comparison tables
  □ Code blocks
  □ Callout boxes
  □ Ordered list count
  □ Unordered list count
  □ Download elements

D4 — SEO Signals
  □ Keyword in H1
  □ Keyword in intro
  □ Keyword in meta title
  □ Semantic keyword usage
  □ Schema signals
  □ Alt text usage
  □ Internal anchor text

D5 — AEO Signals
  □ Direct answer in intro
  □ FAQ section
  □ PAA coverage
  □ Summary / TLDR block
  □ Structured answer format
  □ Featured snippet optimization

D6 — Quality Signals
  □ Original examples
  □ Case studies
  □ Statistics (sourced / unsourced)
  □ Expert quotes
  □ Original insights
  □ Research references
  □ Author signal

D7 — Linking
  □ Internal link count
  □ Internal link strategy
  □ External link count + quality
  □ Resource section
  □ Outbound link pattern

D8 — Formatting
  □ Readability level
  □ Bullet usage
  □ Table of contents
  □ Navigation aids
  □ Pull quotes
  □ White space usage
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — PATTERN SIGNAL CLASSIFICATION REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PATTERN SIGNAL CLASSIFICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STRONG SIGNAL (≥7/10 pages):
  → REQUIRED in new content — non-negotiable parity minimum
  → Include in blueprint as mandatory structural element
  → Absence creates competitive disadvantage

MODERATE SIGNAL (4–6/10 pages):
  → SHOULD be present in new content
  → Include in blueprint as recommended element
  → Absence is acceptable if outweighed by superiority elements

WEAK SIGNAL (2–3/10 pages):
  → CONSIDER including — emerging differentiator
  → Note as optional in blueprint
  → Presence creates minor differentiation advantage

OUTLIER (1/10 pages):
  → EXCLUDE from pattern conclusions
  → Log separately in Outlier Log
  → Evaluate independently as a potential differentiation angle

ZERO-PRESENCE OPPORTUNITY (0/10 pages):
  → HIGH-VALUE differentiation opportunity
  → Include in superiority requirements
  → Executing this well creates clear competitive separation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: serp/deep-serp-analysis.skill*
*Nexus SEO Operating System — Version 1.0.0*
