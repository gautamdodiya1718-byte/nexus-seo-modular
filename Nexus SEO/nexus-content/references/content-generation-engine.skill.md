# SKILL: content/content-generation-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Central Execution Engine
# EXECUTION PRIORITY: TERMINAL — The final execution layer of the Nexus pipeline. Calls all upstream skills before producing output.
# POSITION: Central hub skill. Every research, analysis, and intelligence skill feeds into this engine. Nothing bypasses it.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **central execution engine** of the Nexus SEO Operating System.

It is not a writer. It is not a template filler. It is not a simple content generator that accepts a topic and returns prose. It is a multi-layer execution engine that orchestrates the complete intelligence pipeline of the Nexus system — invoking eleven upstream skills in a mandatory sequence, synthesizing their outputs into a unified production package, and then generating a full SEO-optimized article that reflects every strategic signal the system has gathered.

When this skill runs, it does not guess at what a good article looks like. It knows exactly what the SERP rewards, what patterns dominate the top-ranking pages, which entities belong in the semantic field, which keywords expand the coverage, which gaps the content must fill, and what the exact H1–H3 structure should be — because each of those decisions has been made by the dedicated skill responsible for it. This engine's job is to receive all of that intelligence and translate it into a finished piece that outperforms the competition because it was built on a foundation no competitor can replicate manually.

The final article produced by this skill is a complete production artifact — not a draft. It includes the SEO brief, complete metadata, a structured blueprint, a full article with all content depth requirements met, AEO sections, a conversion layer, internal linking suggestions, visual element placement notes, and humanization — all verified by validation and accuracy checks before delivery.

### Primary Responsibilities

| Responsibility                             | Description                                                                                                           |
|--------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Pre-Execution User Intake**              | Ask five mandatory questions before any pipeline skill is invoked — confirm configuration before committing resources |
| **Mandatory Skill Pipeline Execution**     | Invoke all eleven upstream skills in the defined sequence — no skipping, no shortcuts                                 |
| **SEO Brief Production**                   | Generate a complete SEO brief with all strategic context consolidated from the pipeline                               |
| **Metadata Package Assembly**              | Produce meta title, meta description, and URL slug via `metadata-generator.skill`                                    |
| **Content Blueprint Construction**         | Produce a full H1–H3 outline with AEO sections and FAQ plan from `serp-blueprint-generator.skill`                  |
| **Full Article Generation**                | Write the complete article following all structural, depth, keyword, and AEO requirements                            |
| **Word Count Enforcement**                 | Meet the target word count exactly — excluding images, tables, code blocks, and link text                            |
| **Visual Element Placement**               | Specify where every image, infographic, table, video, code block, and highlight box should appear                    |
| **Conversion Layer Integration**           | Inject CTA, product mentions (BOFU), and trust elements at appropriate structural positions                          |
| **Internal Linking Suggestion**            | Produce 3–5 contextually placed internal link suggestions within the article                                         |
| **Humanization Layer Invocation**          | Call `humanizer.skill` to remove robotic tone, simplify phrasing, and vary sentence length                          |
| **Technical Accuracy Verification**        | Call `technical-accuracy-checker.skill` to verify facts and cross-check official sources                             |
| **Validation Layer Invocation**            | Call `content-validation.skill` to check structure, duplication, and completeness                                    |
| **Entity Coverage Enforcement**            | Ensure all Critical and Important entities from `entity-extraction.skill` are incorporated                           |
| **Deduplication Enforcement**              | Call `deduplication-engine.skill` before finalizing keyword assignments and content ideas                            |

### What This Skill Is NOT

- It is **not** a standalone writer. Every structural and strategic decision it implements was produced by upstream skills.
- It is **not** a template engine. It does not fill placeholders — it synthesizes intelligence into original prose.
- It is **not** a shortcut path. Users who want to skip the pipeline must be explicitly told this engine runs the full pipeline — there is no partial mode.
- It is **not** a revision tool. Revision, optimization, and refreshing are handled by `optimization-engine.skill`. This skill produces the initial article.
- It is **not** optional in the Nexus pipeline. Any content production attempt that bypasses this engine is not using the Nexus system.

### The Defining Standard of This Skill

> This engine produces one output: the best available piece of content on this topic
> that can be created with the intelligence the system has gathered.
>
> "Best available" is defined as:
> → More comprehensive than any top-10 competitor.
> → More structurally aligned with SERP patterns than any top-10 competitor.
> → More semantically complete (entity coverage) than any top-10 competitor.
> → More AEO-optimized than any top-10 competitor.
> → Calibrated to the exact funnel stage the searcher is in.
>
> If the pipeline produces weak intelligence, the article will be flagged accordingly.
> If the pipeline produces strong intelligence, this engine will turn it into a definitive resource.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — MANDATORY SKILL CALL ORDER
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This engine MUST invoke the following skills in the defined sequence before content generation begins. No skill may be skipped. No skill may be reordered except where noted. Each skill's output is passed as input to subsequent skills.

```
MANDATORY PIPELINE EXECUTION SEQUENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1 — SERP INTELLIGENCE (Run First — Foundation of All Decisions)
  [1] serp-intelligence.skill
      Input  : target keyword + funnel stage
      Output : dominant intent, content type rec, difficulty, PAA, freshness signals
      Feeds  : Steps [2], [3], [4], [10], [11]

  [2] deep-serp-analysis.skill
      Input  : target keyword + serp-intelligence output
      Output : per-page analysis across 8 dimensions for top-ranking pages
      Feeds  : Steps [3], [4], [10]

PHASE 2 — PATTERN AND ENTITY EXTRACTION
  [3] content-pattern-extractor.skill
      Input  : deep-serp-analysis output
      Output : Must-Have / Good-to-Have / Opportunity signals, heading patterns, word count range
      Feeds  : Steps [4], [10]

  [7] entity-extraction.skill
      Input  : keyword clusters + serp-intelligence output
      Output : entity map — Critical/Important/Optional entities + AEO entity tags
      Feeds  : Article body (entity coverage), Step [10]

  NOTE: Steps [3] and [7] may run in parallel — they share no dependency.

PHASE 3 — KEYWORD INTELLIGENCE
  [5] keyword-discovery.skill
      Input  : target keyword + serp-intelligence output
      Output : full keyword universe (8 dimensions)
      Feeds  : Steps [6], [9], [11]

  [6] semantic-clustering.skill
      Input  : keyword-discovery output
      Output : topic cluster architecture with tier, strength, funnel balance
      Feeds  : Steps [8], [9], [11]

PHASE 4 — GAP AND DEDUPLICATION
  [8] gap-opportunity-engine.skill
      Input  : content-awareness data + semantic-clustering output + serp-intelligence
      Output : confirmed content gaps — what this article must address that competitors miss
      Feeds  : Step [10]

  [9] deduplication-engine.skill
      Input  : keyword universe + content pipeline data
      Output : deduplicated keyword set — clean primary + secondary + long-tail for this article
      Feeds  : Steps [10], [11]

PHASE 5 — BLUEPRINT AND METADATA GENERATION
  [4] serp-blueprint-generator.skill
      Input  : content-pattern-extractor + serp-intelligence + target keyword
      Output : full 9-part content blueprint (structure, AEO plan, differentiation strategy, metadata draft)
      Feeds  : Steps [10], [11], article body

  [10] blueprint-generator.skill (pre-execution-input.skill)
      Input  : serp-blueprint + keyword intelligence + entity map + gap data + user configuration
      Output : final execution blueprint — the complete production instruction set
      Feeds  : Article body, Step [11]

  [11] metadata-generator.skill
      Input  : target keyword + blueprint + serp-intelligence + funnel stage
      Output : blog titles (3 options), meta titles (3), meta descriptions (3), URL slugs (3), keyword expansion
      Feeds  : SEO Brief, Metadata section of output

PHASE 6 — CONTENT GENERATION (This Skill's Core)
  → Article generation begins ONLY after all 11 upstream skills complete.

PHASE 7 — POST-GENERATION PROCESSING
  [H] humanizer.skill
      Input  : drafted article
      Output : humanized article (simplified phrasing, varied sentence length, robotic tone removed)

  [T] technical-accuracy-checker.skill
      Input  : humanized article + source of truth (if provided)
      Output : accuracy-verified article + fact flags

  [V] content-validation.skill
      Input  : accuracy-verified article + blueprint
      Output : validated article + validation report (structure, duplication, completeness checks)

EXECUTION HIERARCHY:
  SERP blueprint > content patterns > entities > keywords > gaps
  Every content decision that conflicts with a higher-priority signal
  is resolved in favor of the higher-priority signal.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input — Content Trigger

| Field              | Description                                                                                | Required |
|--------------------|--------------------------------------------------------------------------------------------|----------|
| `topic`            | The topic, keyword, working title, or idea that seeds the content production process      | YES      |

This is the only required field. All other parameters are gathered via Pre-Execution User Questions (Section 4) or auto-configured when the user selects "use best."

### Optional Pre-Provided Parameters

If the user provides any of the following alongside the topic, they are imported directly and the corresponding Pre-Execution User Question is skipped:

| Field                  | Description                                                                               |
|------------------------|-------------------------------------------------------------------------------------------|
| `target_audience`      | Who this content is for — role, seniority, industry, or persona description              |
| `funnel_stage`         | TOFU / MOFU / BOFU — overrides SERP-inferred funnel if provided                         |
| `word_count`           | Exact target word count                                                                    |
| `competitors_to_beat`  | Up to 5 URLs of competitor pages to specifically outperform                               |
| `source_of_truth`      | URLs, documents, or data sources to use for factual accuracy                             |
| `tone`                 | Professional / Conversational / Technical / Persuasive                                   |
| `content_type`         | Blog Article / Comparison / Tutorial / Guide / Listicle / Case Study                    |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — PRE-EXECUTION USER QUESTIONS (MANDATORY)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Before invoking any upstream skill, this engine MUST ask the user five configuration questions. These questions cannot be skipped. They cannot be auto-answered without user input UNLESS the user responds with "use best," "auto," "default," or equivalent — in which case the system applies its recommended default for that question.

**Presentation Format:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS CONTENT ENGINE — CONFIGURATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Before generating your content, I need 5 quick inputs.
Type "best" for any question to use the system's recommendation.

1. WORD COUNT
   How long should this content be?
   → Recommendation: Based on SERP average for this keyword.
     I'll calculate this after running SERP analysis.
   → Options: 800 / 1,200 / 1,500 / 2,000 / 2,500 / 3,000+ / Custom: [N]
   → Type your choice or "best":

2. FUNNEL STAGE
   What stage of the buyer journey does this content serve?
   → TOFU — Educational / awareness-building (recommended for traffic)
   → MOFU — Comparison / evaluation (recommended for lead conversion)
   → BOFU — Decision / action (recommended for revenue)
   → Type TOFU / MOFU / BOFU or "best":

3. TARGET AUDIENCE
   Who is this content written for?
   → Be specific: role + seniority + industry (e.g., "marketing managers at B2B SaaS startups")
   → Or type "best" to have the system infer from the keyword and SERP
   → Your audience:

4. COMPETITORS TO BEAT
   Which competitor pages should this content outperform?
   → Recommendation: Top 3 pages currently ranking for your target keyword
   → Paste 1–5 URLs, or type "best" to use the top-ranking SERP results
   → Your competitors:

5. SOURCE OF TRUTH
   Are there specific official sources or documents for factual accuracy?
   → Recommendation: Link to official documentation, product pages, or data sources
   → Examples: official docs, company website, authoritative studies, prior research
   → Your sources (or "none"):

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**WAIT** for all five responses before proceeding.

### "Use Best" Auto-Configuration Logic

When the user responds with "best," "auto," "use best," "default," or "recommended" for any question:

| Question           | Auto-Decision Logic                                                                                    |
|--------------------|--------------------------------------------------------------------------------------------------------|
| Word Count         | After SERP analysis: use the dominant word count range from `content-pattern-extractor.skill` × 1.15 (15% above midpoint for competitive advantage) |
| Funnel Stage       | Use the funnel stage validated by `serp-intelligence.skill` from SERP intent analysis                 |
| Target Audience    | Infer from keyword structure and cluster context — document the inference and display for user confirmation |
| Competitors        | Use the top 3 editorially relevant results from `serp-filter.skill` output                            |
| Source of Truth    | Default to no external source; flag if technical accuracy layer detects unverifiable claims            |

**All auto-decisions are displayed to the user** before pipeline execution begins:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Auto-Configuration Applied
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Word Count      : ~[N] words (SERP average: [N] × 1.15)
Funnel Stage    : [TOFU / MOFU / BOFU] (from SERP intent analysis)
Target Audience : [inferred audience description]
Competitors     : [URL 1], [URL 2], [URL 3]
Source of Truth : [sources or "None — technical accuracy layer will flag uncertain claims"]

Proceeding with pipeline execution. Type STOP to modify.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**WAIT** 5 seconds (or for user input) before proceeding.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — OUTPUT STRUCTURE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The complete output of this engine is a single unified production package containing seven components. Every component is mandatory. No component may be omitted.

### Complete Output Package

```
NEXUS CONTENT PRODUCTION PACKAGE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Topic / Keyword         : [input topic]
Target Keyword          : [primary keyword — confirmed after pipeline]
Word Count Target       : [N words]
Word Count Achieved     : [N words — prose only]
Funnel Stage            : [TOFU / MOFU / BOFU]
Pipeline Status         : COMPLETE — All 11 upstream skills executed
Validation Status       : [PASSED / PASSED WITH FLAGS — see Part 7]
Generated By            : content/content-generation-engine.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — SEO BRIEF
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Blog Topic              : [human-readable editorial topic description]
Funnel Stage            : [TOFU / MOFU / BOFU]
Priority                : [CRITICAL / HIGH / MEDIUM / LOW — from opportunity-engine]
Target Audience         : [specific audience description]
Primary Keyword         : [exact keyword]
Estimated Search Volume : [LOW / MEDIUM / HIGH / VERY HIGH]
Estimated Keyword Difficulty: [EASY / MEDIUM / HARD]
Content Type            : [Blog Article / Guide / Comparison / Tutorial / etc.]
Tone                    : [Professional / Conversational / Technical / Persuasive]
Goal                    : [Traffic / Leads / Conversions / Authority]

Secondary Keywords (6–8):
  [#1] [keyword] — [intent angle] — [placement target]
  [#2] [keyword] — [intent angle] — [placement target]
  [#3–8] [...]

Long-Tail Keywords (5–7):
  [#1] [long-tail phrase] — [search context]
  [#2–7] [...]

LSI Keywords (10–15):
  [comma-separated list of LSI terms]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — METADATA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Recommended Blog Title  : [selected title — with angle rationale]
Alt Blog Titles         : [Option 2] | [Option 3]

Recommended Meta Title  : [title] ([N] chars)
Alt Meta Titles         : [Option 2] ([N] chars) | [Option 3] ([N] chars)

Recommended Meta Desc.  : [description] ([N] chars)
Alt Meta Descs          : [Option 2] ([N] chars) | [Option 3] ([N] chars)

Recommended URL Slug    : /[slug]/
Alt URL Slugs           : /[slug-2]/ | /[slug-3]/

OG Title                : [OG title]
Twitter Title           : [Twitter title]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — CONTENT BLUEPRINT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Full H1–H3 structured outline with:
 - Direct answer block
 - Table of contents
 - All H2 sections with purpose notes
 - All H3 subsections with content notes
 - AEO section positions labeled
 - FAQ plan (5 questions + draft answers)
 - Visual element placements mapped to each section]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — FULL ARTICLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Complete article body — see Step-by-Step Logic for full generation rules]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — INTERNAL LINKING PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[3–5 internal link suggestions with section placement and anchor text]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 6 — VISUAL ELEMENT PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[All visual and structural element placements — images, tables, infographics, code blocks, callouts]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 7 — VALIDATION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Output from content-validation.skill — structure, completeness, deduplication, entity coverage]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — PRE-EXECUTION INTAKE AND CONFIGURATION

Present the five mandatory user questions (Section 4). Wait for responses. Apply auto-configuration for "best" responses. Display configuration summary. Receive confirmation or modifications. Proceed only when configuration is locked.

**Configuration Lock Record:**

```
CONFIGURATION LOCKED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Word Count Target   : [N words] — [user / auto]
Funnel Stage        : [stage] — [user / auto]
Target Audience     : [description] — [user / inferred]
Competitors         : [N URLs confirmed] — [user-provided / SERP-derived]
Source of Truth     : [sources or "none"] — [user / default]
Pipeline Ready      : YES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — PIPELINE EXECUTION

Invoke all 11 upstream skills in the mandatory sequence defined in Section 2. For each skill invocation:

1. Confirm the skill's input dependencies are met (prior skill outputs are available).
2. Invoke the skill with the appropriate inputs.
3. Verify the skill's output is complete and non-empty.
4. Pass the output to the appropriate downstream skills.
5. Log completion.

**Pipeline Progress Log:**

```
PIPELINE EXECUTION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[1] serp-intelligence.skill        ✓ Complete
[2] deep-serp-analysis.skill       ✓ Complete
[3] content-pattern-extractor.skill✓ Complete
[7] entity-extraction.skill        ✓ Complete [parallel with 3]
[5] keyword-discovery.skill        ✓ Complete
[6] semantic-clustering.skill      ✓ Complete
[8] gap-opportunity-engine.skill   ✓ Complete
[9] deduplication-engine.skill     ✓ Complete
[4] serp-blueprint-generator.skill ✓ Complete
[10] blueprint-generator.skill     ✓ Complete
[11] metadata-generator.skill      ✓ Complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
All pipeline skills complete. Content generation beginning.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Pipeline Failure Handling:**

If any upstream skill fails or returns incomplete output:
- Attempt re-invocation once with the same inputs.
- If re-invocation fails → apply the skill's documented failsafe (SERP inference for data-absent scenarios).
- Log the failure and the fallback applied.
- Flag the relevant output section in the final package: `"[SECTION] — REDUCED CONFIDENCE: [skill] returned incomplete data; fallback applied."`
- Continue pipeline — do not halt on a single skill failure.

---

### ▸ STEP 3 — SEO BRIEF CONSTRUCTION (Part 1)

Consolidate the pipeline intelligence into the complete SEO Brief.

**Phase 3A — Blog Topic**

The blog topic is a human-readable editorial description of what this content covers — distinct from the target keyword. It frames the content for an editor, not for a search engine.

Rules:
- 8–15 words.
- Does not duplicate the target keyword.
- Captures the full scope of the article, not just the keyword.
- Clear enough that a writer with no SEO context understands the article's purpose.

**Phase 3B — Priority Assignment**

Import from `opportunity-engine.skill` if available. If not → derive from:
- Funnel stage: BOFU = CRITICAL; MOFU = HIGH; TOFU = MEDIUM.
- Cluster strength: ≥70 = CRITICAL; 40–69 = HIGH; <40 = MEDIUM.
- Combine signals and assign.

**Phase 3C — Keyword Intelligence Assembly**

Import from `metadata-generator.skill` output:
- Primary keyword (confirmed from pipeline).
- Secondary keywords (6–8 with intent angles and placement targets).
- Long-tail keywords (5–7 with search contexts).
- LSI keywords (10–15 across entity/concept/process/metric categories).

**Phase 3D — Search Volume and Difficulty**

Import estimated signals from `serp-intelligence.skill`:
- Search volume tier: LOW / MEDIUM / HIGH / VERY HIGH.
- Keyword difficulty: EASY / MEDIUM / HARD.
- These are qualitative estimates — they are not live data.

---

### ▸ STEP 4 — METADATA ASSEMBLY (Part 2)

Import the complete metadata package from `metadata-generator.skill`. Present all three options for each element with their character counts and rationale. Flag the recommended option for each.

No metadata is generated by this engine directly — all metadata comes from `metadata-generator.skill`. This engine's role is to select and present the recommended options clearly.

**Recommendation Selection Logic:**

For each metadata element, the recommended option is selected based on:
1. SERP pattern alignment (does it match or deliberately differ from the dominant SERP title pattern?).
2. CTR optimization (does it contain the strongest CTR trigger for this funnel stage?).
3. Funnel alignment (does its tone match TOFU/MOFU/BOFU requirements?).
4. Keyword placement (is the primary keyword in the optimal position?).

The selection rationale must be stated in one sentence for each recommended element.

---

### ▸ STEP 5 — CONTENT BLUEPRINT PRODUCTION (Part 3)

Import the structural blueprint from `serp-blueprint-generator.skill` and enhance it with the entity map, gap data, and keyword assignments. The blueprint defines the complete content structure before a single prose word is written.

**Phase 5A — Blueprint Import**

Load the full 9-part blueprint from `serp-blueprint-generator.skill`:
- H1 options (use the recommended option).
- Direct answer block.
- Table of contents structure.
- H2 sections (up to 7) with word targets, keywords, and elements.
- H3 sub-sections with content notes.
- AEO plan (2 AEO H2 sections + 5 FAQs).
- Content elements placement plan.
- Internal linking plan.
- Differentiation strategy.

**Phase 5B — Blueprint Enhancement**

Layer the following intelligence onto the imported blueprint:

| Enhancement                              | Source Skill                        | Where Applied                              |
|------------------------------------------|-------------------------------------|---------------------------------------------|
| Entity coverage requirements             | entity-extraction.skill             | Added to relevant H2/H3 sections           |
| Gap-filling content requirements         | gap-opportunity-engine.skill        | Added as required content in specific sections |
| Secondary keyword assignments            | metadata-generator.skill            | Mapped to specific H2 sections             |
| Long-tail keyword assignments            | metadata-generator.skill            | Mapped to specific H3 sections             |
| Visual element placements                | content-pattern-extractor.skill     | Confirmed per section                       |
| AEO entity tagging                       | entity-extraction.skill             | Applied to AEO sections                    |

**Phase 5C — Annotated Blueprint Output**

The blueprint presented in the output package is fully annotated — every H2 and H3 section includes:
- Its word count target (range).
- Its assigned secondary or long-tail keyword.
- Its required content elements (tables, callouts, examples, etc.).
- Its assigned visual element (if any).
- Its entity coverage requirements.
- Any gap-filling obligation from gap-opportunity-engine.

---

### ▸ STEP 6 — FULL ARTICLE GENERATION (Part 4)

Generate the complete article following all rules defined across the pipeline intelligence. This is the core generation step — every rule below is non-negotiable.

**Phase 6A — Article Generation Framework**

The article is generated section by section, following the annotated blueprint exactly. Each section is generated to its word count target, with its assigned keyword, its required elements, and its entity coverage obligations.

**Section generation order:**
1. H1 heading.
2. Direct answer block (pre-TOC).
3. Table of Contents.
4. Introduction / opening section.
5. H2 sections 1–N in blueprint order.
6. AEO H2 sections.
7. FAQ section.
8. Closing section / CTA.

**Phase 6B — Introduction Rules**

The introduction is the highest-traffic zone of the article — it determines whether a reader stays or bounces. These rules are absolute:

| Rule                                    | Requirement                                                                                    |
|-----------------------------------------|------------------------------------------------------------------------------------------------|
| **Answer-first structure**              | The first paragraph must answer the primary search query directly — not build up to it        |
| **No generic openers**                  | "In today's digital world...", "Have you ever wondered...", "Many people struggle with..." → ALL PROHIBITED |
| **Keyword in first sentence or second** | Primary keyword must appear within the first two sentences                                    |
| **Problem statement**                   | First or second paragraph must acknowledge the specific problem the reader has                |
| **Value promise**                       | By end of paragraph 3, the reader knows exactly what they will learn from this article        |
| **Length**                              | Introduction = 150–250 words (not a section header — it appears before the first H2)         |
| **No list opener**                      | The introduction is prose — not a bulleted list                                               |
| **No "In this article" as the opener**  | Self-referential opening kills engagement — start with the answer, the problem, or the insight |

**Prohibited intro openers (never use):**

```
PROHIBITED INTRODUCTION OPENERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"In today's [adjective] world..."
"Have you ever wondered about..."
"Many businesses struggle with..."
"Did you know that [generic stat]?"
"In this article, we will cover..."
"This comprehensive guide will..."
"Whether you're a beginner or expert..."
"[Topic] is more important than ever."
"If you're looking for [keyword], you've come to the right place."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Required introduction structure (choose one of three models based on funnel stage):**

**TOFU Intro Model — Insight-First:**
Sentence 1–2: A specific insight or clarifying fact about the topic that reframes or answers the question immediately.
Sentence 3–4: Acknowledge what readers typically don't know or get wrong.
Sentence 5–7: Set up what this article will provide specifically.

**MOFU Intro Model — Comparison Context:**
Sentence 1–2: Establish the reader's evaluation context — what they're deciding between and why it matters.
Sentence 3–4: Identify the specific decision challenge this article resolves.
Sentence 5–7: Explain the methodology or criteria this article uses to evaluate.

**BOFU Intro Model — Outcome-First:**
Sentence 1–2: Describe the specific outcome the reader wants — position the article as the path to it.
Sentence 3–4: Name the specific barrier standing between the reader and that outcome.
Sentence 5–7: Explain exactly how this article clears that barrier.

**Phase 6C — Section-Level Content Generation Rules**

For each H2 and H3 section:

**Rule 1 — Word Count Compliance:**
Generate each section to its word count target range (from the annotated blueprint). Prose only — images, tables, code blocks, and list item text that is purely structural do not count.

**Rule 2 — Keyword Natural Placement:**
- Primary keyword: appears in H1, introduction, and 1–2 H2 headings (only where genuinely natural).
- Secondary keyword: appears in its assigned H2 section — in the heading if natural; in the first paragraph of the section if not natural in the heading.
- Long-tail keyword: appears in its assigned H3 sub-section — in body text, not forced into headings.
- LSI keywords: woven naturally throughout the article — no forced placement; density follows natural writing.

**Rule 3 — No Stuffing:**
No keyword appears more than once per 200 words of prose. No phrase identical to the primary keyword appears more than 4 times total in the article. If a keyword must appear, it must be in a form that reads naturally — variations and synonyms are preferred over exact repetition.

**Rule 4 — Entity Coverage:**
Every Critical entity from `entity-extraction.skill` must appear in the article. Important entities should appear where naturally relevant. Optional entities are woven in where genuinely useful.

For each Critical entity, the first appearance must be accompanied by at least one sentence of context — a bare mention with no explanation does not satisfy entity coverage.

**Rule 5 — Content Depth Requirements:**
Every H2 section must include at least ONE of the following depth elements:
- **Example:** A real-world or concrete illustrative example (not hypothetical without context).
- **Statistic:** A relevant data point with its source type noted.
- **Comparison:** A side-by-side evaluation of two or more approaches, tools, or options.
- **Explanation:** A mechanism explanation — not just what, but how and why.

H3 sub-sections must include at least ONE depth element unless they are demonstrably too short for one (< 80 words).

**Rule 6 — No Fluff:**
Every sentence must add information value. Sentences that can be removed without loss of meaning are filler and are prohibited.

Filler sentence patterns (prohibited in every section):
- Transition sentences that merely restate what the previous paragraph said.
- "As mentioned above..." constructions.
- "It's important to note that..." followed by something obvious.
- Closing sentences that say "Now let's look at the next section."
- Padding summaries at the end of sections that repeat the section's content.

**Rule 7 — No Repetition:**
No idea, claim, or explanation may appear in more than one section of the article. If an idea is introduced in Section 2, it is not reintroduced in Section 5. Cross-references are allowed ("as we established in [section]") but the full explanation is never duplicated.

**Phase 6D — AEO Section Generation Rules**

Two H2 sections in the article are designated as AEO sections in the blueprint. These sections are structured specifically to capture featured snippets, answer PAA questions, and maximize answer engine visibility.

**AEO H2 Section Structure:**

```
H2: [Question-format heading or direct topic heading]

[Direct answer paragraph — 40–60 words — complete standalone answer to the implied question]

[Supporting explanation — 100–200 words — context, nuance, application]

[Optional: numbered list or definition block if the answer type supports it]
```

**AEO Paragraph Rules:**
- The first paragraph of every AEO section must answer the heading's question completely in 40–60 words.
- The answer must be self-contained — a reader who reads only this paragraph gets a usable, complete response.
- No "as discussed earlier" or "we'll cover this later" references in AEO paragraphs.
- The primary keyword or a close variant must appear in the AEO answer paragraph.

**Phase 6E — FAQ Section Generation Rules**

The FAQ section contains 5 question-and-answer pairs. Each FAQ is independently AEO-optimized:

**FAQ Rules:**
1. Every question must be phrased as a natural language query — the way a person types it.
2. Questions must be derived from PAA data (from `serp-intelligence.skill`) or from long-tail keyword questions.
3. No two questions may address the same sub-intent.
4. Every answer must be 40–80 words.
5. Every answer must be completely self-contained — no cross-references to other FAQs or article sections.
6. Every answer must begin with a direct declarative response (not a question, not "It depends").
7. FAQ schema markup is recommended — note for the content team.

**FAQ Assembly Order:**
- FAQ 1: Definitional question ("What is...").
- FAQ 2: Process question ("How to...").
- FAQ 3: Comparison or selection question ("Which..." or "What's the difference between...").
- FAQ 4: Constraint or qualification question ("When should...", "Who needs...").
- FAQ 5: Advanced or edge-case question (a question more experienced readers would ask).

**Phase 6F — Conversion Layer Generation**

The conversion layer is integrated into the article — not bolted on at the end. Placement and intensity depend on the funnel stage:

**TOFU Conversion Layer:**
- CTA type: Soft — resource offer, newsletter, "related guide" recommendation.
- Placement: End of article, after FAQ section.
- Intensity: ONE CTA — no more. Hard sell at TOFU = trust destruction.
- Format: Inline text with a link — not a button block (buttons feel out of place in educational content).

**MOFU Conversion Layer:**
- CTA type: Medium — free trial, comparison tool, case study, product page link.
- Placement: End of article AND mid-article (after the comparison or evaluation section).
- Intensity: TWO CTAs maximum — one mid-article (contextual), one closing (direct).
- Format: Contextual inline CTA in the comparison section + a highlighted CTA block at the end.

**BOFU Conversion Layer:**
- CTA type: Strong — "Get started," "Start free trial," "Book a demo," "Request a quote."
- Placement: After introduction, after key proof section (case study/results), at article end.
- Intensity: THREE CTAs maximum — strategic placement at trust peaks in the content.
- Product/service mention: Specific tool, service, or solution mention with direct link.
- Trust elements: Add social proof signal (customer count, case study reference, award, testimonial quote) near the conversion CTA.
- Format: Highlighted CTA block with supporting trust signal.

**Conversion Layer Writing Rules (All Funnel Stages):**
- CTAs must be benefit-specific — not generic. "Get started" alone = weak. "Get started with [specific outcome]" = strong.
- CTAs must not interrupt the article's information flow — they appear at natural breakpoints.
- Trust elements (reviews, case studies, customer counts) must be specific — not vague ("thousands of users" is weak; "14,000+ marketing teams" is specific).

---

### ▸ STEP 7 — WORD COUNT CONTROL

After the full article draft is complete, perform a precise word count verification.

**Phase 7A — Word Count Counting Rules**

**Include in word count:**
- All prose sentences and paragraphs.
- Heading text (H1, H2, H3).
- List item text that is substantive (full sentences).
- FAQ questions and answers.
- CTA text.

**Exclude from word count:**
- Images (the image itself — alt text counts).
- Tables (table data — the surrounding prose counts).
- Code blocks.
- URLs and link text that is a URL.
- "[IMAGE: ...]" placeholder text.
- "[TABLE: ...]" placeholder text.
- "[CODE BLOCK: ...]" placeholder text.

**Phase 7B — Word Count Adjustment**

After counting:

| Count vs. Target   | Action                                                                              |
|--------------------|-------------------------------------------------------------------------------------|
| Within ±5%         | PASS — no adjustment needed                                                         |
| Short by 6–15%     | Add depth: expand the two weakest H3 sections with an additional example each       |
| Short by >15%      | Add a new H3 sub-section to the section most likely to benefit from deeper coverage |
| Over by 6–15%      | Remove the weakest sentence in each section (filler identification pass)            |
| Over by >15%       | Remove the entire weakest H3 sub-section; condense two adjacent sections             |

**CRITICAL RULE:** Word count is NEVER adjusted by adding filler. "Expand with depth, not filler" means:
- Add a new example.
- Add a deeper explanation of a mechanism.
- Add a comparison that was not in the blueprint but is genuinely useful.
- Add a relevant statistic with context.
- NEVER add sentences that restate existing content.

---

### ▸ STEP 8 — INTERNAL LINKING PLAN (Part 5)

Generate 3–5 internal link suggestions. Internal links must be contextually placed — not inserted at the end of the article as an afterthought.

**Phase 8A — Internal Link Source**

Import internal link recommendations from `serp-blueprint-generator.skill` (Part 7 of blueprint). The blueprint-level plan specifies which sections should link to which pages. This step confirms and contextualizes those recommendations within the actual generated article text.

**Phase 8B — Contextual Placement Protocol**

For each internal link:

```
INTERNAL LINK RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
From Section    : [H2 or H3 reference in the generated article]
From Context    : "[quote the sentence or phrase where the link should appear]"
To Page         : [destination page title or topic]
Anchor Text     : [exact anchor text recommendation — descriptive, not "click here"]
Link Purpose    : [topical relevance / funnel flow / authority distribution]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Anchor text rules:**
- Descriptive — the reader understands where they're going from the anchor text alone.
- Keyword-enriched where natural — not generic ("this article", "here", "click here").
- 3–6 words preferred.
- No exact-match anchor stuffing — every internal link uses a different anchor text.

---

### ▸ STEP 9 — VISUAL ELEMENT PLAN (Part 6)

Specify where every visual and structural content element should be placed throughout the article. These placements are derived from `content-pattern-extractor.skill` Must-Have element signals and `serp-blueprint-generator.skill` Content Elements Placement Plan.

**Visual Elements Specification Format:**

```
VISUAL ELEMENT: [Element Type]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Location    : [Section — after paragraph N of H2 "[section name]"]
Type        : [Screenshot / Diagram / Infographic / Chart / Table / Code Block / Callout Box]
Purpose     : [What this element adds that prose cannot — specific]
Specification: [What exactly should be shown — precise, actionable brief for designer/team]
Alt Text    : [Recommended alt text — keyword-aware, descriptive]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Visual Element Types and When to Use Each:**

| Element Type      | Use When                                                                                   |
|-------------------|--------------------------------------------------------------------------------------------|
| **Image**         | Concept that benefits from visual illustration; tool screenshots that show the "what"      |
| **Diagram**       | Process flows, architectures, relationships that text describes but visuals clarify        |
| **Infographic**   | Multi-dimensional comparisons, statistical summaries, step summaries                      |
| **Table**         | Feature comparisons, pricing comparisons, option evaluations with 3+ dimensions           |
| **Code Block**    | Technical content — CLI commands, code examples, configuration samples                    |
| **Callout Box**   | Key insights, warnings, tips, important notes that must not be lost in body prose         |
| **Video**         | Complex processes where animation or walkthrough adds significant clarity over text        |

**Note on word count:** All visual elements are excluded from word count. The article must meet its word count target in prose alone.

---

### ▸ STEP 10 — POST-GENERATION PROCESSING

After the full article is drafted, invoke three post-processing skills in sequence.

**Phase 10A — Humanizer Invocation**

Call `humanizer.skill` with the complete article draft.

Humanizer targets:
- Replace robotic, formal-academic phrasing with natural, conversational alternatives.
- Vary sentence length — no three consecutive sentences of similar length.
- Replace passive voice where active voice is more direct.
- Remove transitional phrases that feel AI-written ("It is worth noting that...", "Furthermore, it is important to...").
- Preserve all technical accuracy — humanization does not change meaning.
- Preserve all keyword placements — humanization does not remove or relocate keywords.

**Phase 10B — Technical Accuracy Checker Invocation**

Call `technical-accuracy-checker.skill` with the humanized article and the source of truth (if provided).

Technical accuracy checker targets:
- Verify all statistics are plausibly accurate (flagging any that appear unverifiable).
- Verify all named entity references (tools, organizations, people) are accurate as of the knowledge cutoff.
- Verify all process descriptions are technically correct.
- Cross-check against source of truth URLs if provided.
- Flag any claim that requires external verification before publishing.

**Phase 10C — Content Validation Invocation**

Call `content-validation.skill` with the accuracy-verified article and the annotated blueprint.

Content validation targets:
- **Structure check:** Every required H2 and H3 from the blueprint is present in the article.
- **Completeness check:** Every required content element (examples, statistics, FAQs, CTAs) is present.
- **Duplication check:** No idea, claim, or phrase is meaningfully repeated.
- **Entity coverage check:** All Critical entities from `entity-extraction.skill` are present.
- **Keyword placement check:** Primary keyword appears in H1, introduction, and 1–2 H2 headings.
- **AEO check:** Both AEO H2 sections have complete direct-answer paragraphs (40–60 words, self-contained).
- **Funnel alignment check:** Tone, CTA type, and content framing match the validated funnel stage.

**Validation Output:**

```
CONTENT VALIDATION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Structure Check     : [PASS / FAIL — issues listed]
Completeness Check  : [PASS / FAIL — missing elements listed]
Duplication Check   : [PASS / FAIL — duplicate phrases listed]
Entity Coverage     : [PASS / PARTIAL — uncovered Critical entities listed]
Keyword Placement   : [PASS / FAIL — placement issues listed]
AEO Check           : [PASS / FAIL — AEO section issues listed]
Funnel Alignment    : [PASS / FAIL — alignment issues listed]
Overall Status      : [PASSED / PASSED WITH FLAGS / REQUIRES REVISION]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If validation produces REQUIRES REVISION → the specific issues are corrected before the article is presented to the user. PASSED WITH FLAGS → the article is presented with the flag list clearly documented.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — CONTENT DEPTH REQUIREMENTS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

These are the minimum depth requirements for every article produced by this engine. They are non-negotiable and are verified by `content-validation.skill`.

### Requirement 1 — Examples

Every H2 section must contain at least one example. An example must:
- Be specific — it names a concrete scenario, tool, company, or situation.
- Illustrate the concept being explained — it is not a restatement of the concept.
- Be relevant to the target audience as defined in the configuration.

**Example quality check:**

| Weak Example (Prohibited)                                    | Strong Example (Required)                                                         |
|--------------------------------------------------------------|-----------------------------------------------------------------------------------|
| "For example, you can use a CRM to manage contacts."         | "A 10-person sales team at a B2B software company might use HubSpot to track 200 active deals across three pipeline stages, with automated follow-up sequences triggered when a lead moves from 'demo scheduled' to 'proposal sent.'" |
| "Statistics show that CRM adoption increases revenue."       | "According to Nucleus Research, every $1 invested in CRM implementation returns an average of $8.71 in revenue — a finding replicated across companies with 10–500 employees in their 2024 ROI report." |

### Requirement 2 — Real-World Use Cases

At minimum ONE H2 section per article must address a real-world use case — a specific scenario showing the topic's application in practice. Use cases differ from examples:
- An **example** illustrates a concept.
- A **use case** demonstrates a complete workflow or application context.

### Requirement 3 — Statistics and Data

For MOFU and BOFU content: minimum two statistics in the article, each with its source type identified (academic study / industry report / official data / platform-published research).

For TOFU content: minimum one statistic where relevant.

Statistics must:
- Be presented in context — not dropped as a bare number.
- Have their source or source type named.
- Be flagged if they cannot be verified during the technical accuracy check.

### Requirement 4 — Comparisons

For MOFU content: at least one formal comparison (side-by-side evaluation of two or more options on defined criteria) must be present — in prose, table, or structured list format.

For TOFU and BOFU: informal comparisons (comparing approaches, outcomes, or conditions) are sufficient and encouraged.

### Requirement 5 — Explanations of Mechanism

At least two H2 sections per article must explain the mechanism of a concept or process — not just state what it is, but explain how and why it works the way it does. Mechanism explanations are the primary signal of genuine expertise and are a key entity depth requirement.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ PARAGRAPH VARIATION

No three consecutive paragraphs in the article may have the same approximate length. Apply the following variation model:

| Paragraph Pattern       | Acceptable                                                          |
|-------------------------|---------------------------------------------------------------------|
| Long + Medium + Short   | ✅ Maximum variation                                                |
| Medium + Short + Long   | ✅ Acceptable variation                                             |
| Short + Medium + Short  | ✅ Acceptable                                                       |
| Long + Long + Long      | ❌ Prohibited — monotonous rhythm kills readability                |
| Short + Short + Short   | ❌ Prohibited — bullets disguised as paragraphs                    |

**Paragraph length guide:**
- Short paragraph: 1–2 sentences (30–60 words).
- Medium paragraph: 3–4 sentences (60–120 words).
- Long paragraph: 5–7 sentences (120–180 words).

### ▸ TONE ALIGNMENT

The tone configured in Step 1 (Professional / Conversational / Technical / Persuasive) must be consistent throughout the article. Tone drift — where sections of the article change register from others — undermines coherence.

**Tone Consistency Checks:**

| Tone                 | Prohibited in this tone                                                    |
|----------------------|----------------------------------------------------------------------------|
| Professional         | Slang, contractions in formal claims, first-person plural ("we think")    |
| Conversational       | Overly formal passive constructions; jargon without explanation            |
| Technical            | Oversimplified analogies; assumed non-technical reader                    |
| Persuasive           | Neutral language that fails to build toward a conclusion; passive verbs    |

### ▸ READABILITY CONTROL

The article must be readable at the target audience's comprehension level. Apply these controls:

| Audience Level    | Sentence Length     | Jargon Level     | Analogy Use     |
|-------------------|---------------------|------------------|-----------------|
| Beginner          | Max avg 18 words    | Define all terms | Use liberally   |
| Intermediate      | Max avg 22 words    | Define new terms | Use selectively |
| Expert            | Max avg 28 words    | No definitions   | Rare            |

### ▸ ENTITY COVERAGE ENFORCEMENT

After the article is drafted, run a systematic entity coverage check:

1. Load the entity map from `entity-extraction.skill`.
2. For every Critical entity: verify it appears in the article with context. Record its location.
3. For every Important entity: verify it appears or note why it was excluded.
4. For Optional entities: note which were incorporated naturally.
5. If a Critical entity is missing → identify the most appropriate section and insert it with a minimum of one contextual sentence.
6. Report entity coverage in the Validation Report.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 9 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Ideas

Every concept, claim, or insight appears exactly once in the article. If a concept is first introduced in the Introduction, it is not re-explained in a later section — it may be referenced, but not re-explained. If the deduplication check finds the same idea in two places, the weaker instance is removed.

### Rule 2 — No Repeated Phrasing

No exact phrase of five or more words may appear more than once in the article. If the same phrase is genuinely useful in two locations, rephrase one instance. `content-validation.skill` runs a phrase duplication check.

### Rule 3 — No Duplicate Section Purposes

No two H2 sections may serve the same reader purpose. If two sections answer the same question from slightly different angles → merge them into one comprehensive section with H3 sub-sections. Section boundary validation is part of the blueprint review in Step 5.

### Rule 4 — No Keyword Recycling from Prior Session

Keywords assigned to this article by `deduplication-engine.skill` must not have been used as primary keywords for any other article in the same project. `deduplication-engine.skill` enforces this — this rule confirms it is respected in the generation output.

### Rule 5 — Article-Level Deduplication Against Content Inventory

The primary keyword and article topic are cross-checked against `content-inventory.skill` before generation begins. If a highly similar article already exists → this is surfaced to the user in the pre-execution phase as a potential duplication risk. User confirms or redirects before pipeline runs.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 10 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Content Behaviors

| Prohibited Behavior                                                                             | Reason                                                                           |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Any of the prohibited introduction openers listed in Phase 6B                                  | These openers guarantee a generic, low-engagement introduction                  |
| Using "It's important to note that..." before stating something obvious                         | Meta-commentary about importance signals that the content itself is weak         |
| Closing a section with "Now let's look at..." transition sentences                              | Section transitions via headings — no prose transitions needed                  |
| Writing a section that summarizes the section just written ("In summary...")                    | Summaries at the end of individual sections are padding                         |
| Using "various," "different," "several," or "many" without specificity                         | Vague quantifiers replace real information — use specific counts or examples    |
| Writing a statistic without a source context                                                    | An unsourced statistic is weaker than no statistic — always attribute           |
| Describing a feature or capability without explaining its mechanism                             | Feature lists without mechanism explanations produce shallow content             |
| Using rhetorical questions as section openers                                                   | "Have you ever wondered why X works?" = weak engagement strategy                |
| Padding to reach word count with additional synonym variations of already-stated claims         | Word count is reached with depth — never with paraphrase inflation              |
| Including a conversion CTA in the first 300 words                                              | CTAs in TOFU introductions destroy trust — readers came to learn, not be sold to |

### Required Content Behaviors

- Every section must add new information not present in any prior section.
- Every claim must be followed by a supporting reason, example, or data point.
- Every process step must be explained — not just listed.
- Every comparison must have named criteria — not just "A is better than B."
- The direct answer block must answer the title's implied question in ≤60 words.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 11 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ Upstream Skills (Pipeline — All Mandatory)

| Skill                            | Invocation Phase | What It Provides                                                                |
|----------------------------------|------------------|---------------------------------------------------------------------------------|
| `serp-intelligence.skill`        | Phase 1          | Dominant intent, content type, SERP features, difficulty, PAA, freshness       |
| `deep-serp-analysis.skill`       | Phase 1          | Per-page 8-dimension analysis of top results                                   |
| `content-pattern-extractor.skill`| Phase 2          | Must-Have/Good-to-Have/Opportunity signals; heading patterns; word count ranges |
| `entity-extraction.skill`        | Phase 2          | Entity map — Critical/Important/Optional + AEO tags                           |
| `keyword-discovery.skill`        | Phase 3          | Full 8-dimension keyword universe                                               |
| `semantic-clustering.skill`      | Phase 3          | Topic cluster architecture + strength scores                                   |
| `gap-opportunity-engine.skill`   | Phase 4          | Confirmed content gaps to fill                                                  |
| `deduplication-engine.skill`     | Phase 4          | Deduplicated keyword set + rejection log                                        |
| `serp-blueprint-generator.skill` | Phase 5          | 9-part content blueprint (structure, AEO, differentiation, metadata draft)    |
| `blueprint-generator.skill`      | Phase 5          | Final production instruction set (pre-execution-input enhanced)                |
| `metadata-generator.skill`       | Phase 5          | Blog titles, meta titles, meta descriptions, URL slugs, keyword expansion      |

### ▸ Post-Processing Skills (Phase 7 — All Mandatory)

| Skill                               | Purpose                                                                        |
|-------------------------------------|--------------------------------------------------------------------------------|
| `humanizer.skill`                   | Remove robotic tone, vary sentence length, simplify phrasing                  |
| `technical-accuracy-checker.skill`  | Verify facts, cross-check sources, flag unverifiable claims                   |
| `content-validation.skill`          | Structure, completeness, duplication, entity, keyword, AEO, funnel checks     |

### ▸ Downstream Skills (Consumers of This Engine's Output)

| Skill                     | Consumes                                                                              |
|---------------------------|---------------------------------------------------------------------------------------|
| `content-inventory.skill` | Primary keyword, cluster, URL slug, status (planned → draft → published lifecycle)   |
| `internal-linking.skill`  | Internal linking plan (Part 5), anchor text recommendations                          |
| `optimization-engine.skill` | Full article for future optimization passes                                         |
| `memory-controller.skill` | Complete production package — all 7 parts written to content-memory and keyword-memory |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                           |
|---------------------------------------------------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Topic is too vague to produce a keyword (e.g., "write about SEO")        | Step 1 — keyword resolution fails                      | Ask clarifying question: "Can you be more specific? For example: 'best CRM software for startups' rather than 'CRM tools'." Do not proceed until a specific keyword is confirmed. |
| An upstream pipeline skill fails to return output                        | Step 2 — pipeline log shows skill failure              | Retry once. If still fails → apply the skill's documented failsafe (SERP-based inference). Flag in output: "[Skill Name] returned incomplete data — fallback applied; confidence MEDIUM." |
| SERP data is unavailable for the target keyword                          | serp-intelligence returns null                         | Run in INFERENCE MODE — use keyword pattern and topic context to estimate SERP signals. Flag entire output as SERP DATA ABSENT — all structural decisions are estimated. |
| Word count target cannot be met without filler                           | Step 7 — word count check fails after all adjustments | Report actual achievable word count. Explain: "This topic can be comprehensively covered in [N] words. Increasing to [target] would require padding. Recommend accepting [N] as the natural target." |
| Validation produces REQUIRES REVISION                                    | Step 10C — validation status = REQUIRES REVISION      | Fix all flagged issues before presenting to user. Re-run validation. Present only after status = PASSED or PASSED WITH FLAGS. |
| The user's source of truth contains information that conflicts with SERP data | Technical accuracy checker detects conflict      | Flag the conflict explicitly: "Source of truth [URL] states X. SERP consensus suggests Y. Using source of truth as authoritative — recommend noting the discrepancy in the article." |
| Deduplication engine blocks the primary keyword (already published)      | Step 4 — deduplication returns primary keyword as REJECTED | Halt content generation. Alert user: "This keyword is already assigned to an existing piece: [URL]. Generate new content for this keyword would create duplication. Options: (1) Update the existing piece, (2) Choose a different angle or keyword." |
| Configuration intake is abandoned (user stops responding)                | Step 1 — no response within session context           | Apply all "best" auto-configurations. Display the auto-configured settings. Proceed with 15-second window before starting. Allow override at any point. |
| Technical accuracy checker flags >5 unverifiable claims                  | Phase 10B — >5 flags produced                         | Present the article with all flags clearly marked. Do not auto-remove flagged claims. Recommend: "Verify these [N] claims before publishing. Claims are marked [VERIFY] in the article." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — EXECUTION INTEGRITY CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
EXECUTION INTEGRITY CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Run before delivering the final output:

□ All 11 upstream pipeline skills completed (or failsafe documented)
□ All 3 post-processing skills completed (humanizer, accuracy, validation)
□ SEO Brief contains all required fields (Part 1)
□ Metadata contains 3 options for each element with character counts (Part 2)
□ Blueprint contains full H1–H3 outline with AEO positions labeled (Part 3)
□ Article intro follows answer-first structure (no prohibited openers)
□ Primary keyword appears in H1, introduction, and 1–2 H2 headings
□ All Critical entities from entity-extraction are present in article
□ Both AEO H2 sections have 40–60 word self-contained direct answers
□ All 5 FAQs are self-contained with direct declarative answers
□ Conversion layer matches funnel stage (TOFU: soft | MOFU: medium | BOFU: strong)
□ Word count is within ±5% of target (prose only)
□ Visual element plan covers all Must-Have SERP elements (Part 6)
□ 3–5 internal link suggestions with contextual placement (Part 5)
□ Validation report is present and status is not REQUIRES REVISION
□ No prohibited introduction openers used
□ No repeated ideas or phrasing across sections
□ No keyword stuffing (primary keyword ≤4 times total)
□ Post-processing: robotic tone removed, sentence length varied
□ All facts are either verifiable or flagged as VERIFY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — FUNNEL-SPECIFIC ARTICLE BEHAVIOR GUIDE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
FUNNEL-SPECIFIC BEHAVIOR GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TOFU — INFORMATIONAL:
  Intro model          : Insight-First (educate and clarify)
  Section sequence     : Definition → How it works → Types/Examples → Best practices → FAQ
  Depth focus          : Explanation of mechanisms; real-world examples
  Tone                 : Educational, helpful, non-commercial
  CTA type             : Soft — "Learn more about X", "Explore our guide to Y"
  CTA count            : 1 maximum — end of article only
  Entity priority      : Concept entities > Tool entities
  Keyword density      : Lower — emphasis on natural writing over keyword placement

MOFU — COMMERCIAL:
  Intro model          : Comparison Context (validate their evaluation problem)
  Section sequence     : Criteria → Comparison → Options evaluated → Recommendation → FAQ
  Depth focus          : Side-by-side comparison; evidence for criteria; specific strengths/weaknesses
  Tone                 : Authoritative, balanced, trustworthy
  CTA type             : Medium — "Compare plans", "See full breakdown", "Try free"
  CTA count            : 2 maximum — mid-article after comparison + end of article
  Entity priority      : Tool entities > Concept entities; organization entities
  Keyword density      : Moderate — secondary keywords target evaluation language

BOFU — TRANSACTIONAL:
  Intro model          : Outcome-First (confirm their decision, remove barriers)
  Section sequence     : Solution overview → Key proof → How to start → FAQ → Strong CTA
  Depth focus          : Concrete outcomes, specific results, trust signals
  Tone                 : Action-oriented, confident, benefit-forward
  CTA type             : Strong — "Get started", "Start free trial", "Book a demo"
  CTA count            : 3 maximum — after intro, after proof section, end of article
  Trust elements       : Required — customer count, case study reference, or testimonial near CTA
  Entity priority      : Tool entities; organization entities; metric entities
  Keyword density      : Higher — transactional keywords appear near CTAs naturally
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — PIPELINE EXECUTION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PIPELINE EXECUTION ORDER — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1 — SERP (Run First — All Other Phases Depend On This):
  [1] serp-intelligence.skill       → intent + difficulty + PAA + freshness
  [2] deep-serp-analysis.skill      → per-page 8-dimension analysis

PHASE 2 — PATTERN + ENTITY (Parallel OK):
  [3] content-pattern-extractor.skill → Must-Have signals + heading patterns
  [7] entity-extraction.skill         → entity map + AEO tags

PHASE 3 — KEYWORD:
  [5] keyword-discovery.skill         → keyword universe (8 dimensions)
  [6] semantic-clustering.skill       → cluster architecture

PHASE 4 — GAP + DEDUP:
  [8] gap-opportunity-engine.skill    → confirmed content gaps
  [9] deduplication-engine.skill      → clean keyword set

PHASE 5 — BLUEPRINT + METADATA:
  [4] serp-blueprint-generator.skill  → 9-part content blueprint
  [10] blueprint-generator.skill      → final execution blueprint
  [11] metadata-generator.skill       → titles + descriptions + keywords

PHASE 6 — GENERATION:
  → Article generation (this engine)

PHASE 7 — POST-PROCESSING:
  [H] humanizer.skill                 → natural tone
  [T] technical-accuracy-checker.skill → fact verification
  [V] content-validation.skill        → completeness + structure check

EXECUTION HIERARCHY:
  SERP blueprint > Content patterns > Entities > Keywords > Gaps
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## v3.0 UPGRADE — 5 PRE-WRITING CONSTRAINTS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**NEW in v3.0: Apply these 5 constraints BEFORE writing any draft. These are not post-processing fixes — they are writing rules that shape the draft from the first sentence.**

### Constraint 1 — Cut Significance Inflation
Never use words that inflate importance beyond what the sentence actually conveys:
- BANNED: "crucial," "essential," "game-changing," "revolutionary," "transformative," "vital," "critical" (unless something is literally critical, like a critical error), "groundbreaking," "pivotal"
- INSTEAD: State the fact. Let the reader decide if it's important. "This reduces test execution time by 67%" is more compelling than "This is a game-changing approach to test execution."

### Constraint 2 — Use Plain Verbs
Choose the simplest verb that conveys the meaning:
- "use" not "leverage" or "utilize"
- "start" not "embark on"
- "help" not "empower" or "enable"
- "show" not "demonstrate" or "showcase"
- "improve" not "enhance" or "elevate"
- "build" not "architect" or "engineer" (unless literally engineering)
- "find" not "uncover" or "discover" (unless actual discovery)

### Constraint 3 — End Sentences at the Fact
Do not add trailing commentary after the information:
- BAD: "Playwright supports parallel execution, which is something that can significantly improve your testing workflow."
- GOOD: "Playwright supports parallel execution."
- The trailing clause adds no information. Cut it.

### Constraint 4 — Vary Rhythm Deliberately
Before writing each section, consciously plan sentence length variation:
- Start a section with a short sentence (under 10 words)
- Follow with a medium sentence (15-25 words)
- Then a longer one with detail (25-35 words)
- Then short again
- Never write 4+ consecutive sentences of similar length

### Constraint 5 — Earn Every Adjective
Before including an adjective, test: does removing it change the meaning?
- "robust framework" → does "robust" add information? Usually no. → "framework"
- "comprehensive guide" → does "comprehensive" add information? Only if you're distinguishing from a quick overview. Usually → "guide"
- "powerful features" → "powerful" says nothing specific. → "features" or better: name the specific features

**These 5 constraints apply to EVERY draft. The humanizer catches remaining AI patterns after the draft, but these constraints prevent the worst patterns from entering the draft in the first place.**

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## v3.0 UPGRADE — REAL EXAMPLE MANDATE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**NEW in v3.0: Every technical claim must be backed by a real, working example. No placeholders. No fake data.**

### Rule 1 — Code Examples Are DEFAULT ON

For ANY topic involving tools, libraries, CLI commands, configs, APIs, or code:
- Write actual runnable code (not pseudocode, not "example-style" snippets)
- Execute the code using `code-generation-preview.skill.md`
- Capture the REAL terminal output or result
- Include the actual output in the article — not "Expected output: ..."
- Use realistic data in examples: real file paths, real test counts, real config values

**This is NOT optional.** The auto-pilot Phase 2 Step 2.2 makes this DEFAULT ON. Only skip for purely strategic, non-technical content where zero code, tools, or technical configuration is involved.

### Rule 2 — Real Data in Non-Code Examples

For statistics, benchmarks, and performance claims:
- Search for real company engineering blogs, conference talks, GitHub repos, case studies
- Use `real-world-examples.skill.md` to find and validate sources
- Include real company names, real team sizes, real metric improvements

**BAD examples (never produce these):**
- "Company A saw a 40% improvement in test execution time"
- "Many organizations have reported significant improvements"
- "Studies show that this approach can reduce costs by up to 50%"

**GOOD examples (always produce these):**
- "Shopify's engineering team reduced their CI pipeline from 45 minutes to 12 minutes by implementing Playwright sharding across 8 workers (Source: Shopify Engineering Blog, 2024)"
- "Netflix runs over 500,000 Playwright tests daily across 200+ microservices (Source: Netflix Tech Blog)"
- When real data is unavailable: "Based on typical enterprise test suites of 2,000-5,000 tests, parallel execution across 4 workers reduces total run time from ~30 minutes to ~8 minutes. (Illustrative — actual results vary by test complexity.)"

### Rule 3 — Mark Illustrative Data Explicitly

If you cannot find a real source for a claim:
- Mark it: "(Illustrative — based on common patterns)" or "(Estimated from typical configurations)"
- NEVER present fabricated data as factual
- NEVER invent company names or attribution

### Rule 4 — Minimum Depth Requirements

For technical blog posts, the draft must contain AT MINIMUM:
- 3+ working code examples with real output (for tool/framework topics)
- 2+ real data points with sources (company names, metrics, blog posts)
- 2+ tables with actual comparison data (not placeholder rows)
- 1+ config file or setup example that a reader could copy-paste and use

For non-technical blog posts:
- 3+ real examples from named companies or individuals with sources
- 2+ data points from verifiable sources
- Specific numbers, not ranges or hedged estimates

### Quality Check

Before passing the draft to the next pipeline step, verify:
```
REAL EXAMPLE AUDIT:
- Code examples: [X] (minimum 3 for technical topics)
- Code executed and output captured: [YES / NO]
- Real company examples: [X] with sources
- Illustrative data marked: [X] instances
- Tables with real data: [X]
- Fake/placeholder data: [should be 0]
```

---

*SKILL FILE END: content/content-generation-engine.skill*
*Nexus SEO Operating System — Version 3.0.0*
