# SKILL: serp/content-pattern-extractor.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: SERP / Pattern Intelligence
# EXECUTION PRIORITY: POST-DEEP-SERP — Runs immediately after deep-serp-analysis.skill completes per-page analysis.
# POSITION: Aggregation and intelligence layer between raw page-level analysis and blueprint generation.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **pattern intelligence aggregator** of the Nexus SERP analysis pipeline.

Where `deep-serp-analysis.skill` produces granular per-page observations across 8 dimensions, this skill converts that raw analytical data into structured, frequency-weighted, priority-classified pattern intelligence. It answers the questions that per-page analysis cannot answer alone: What consistently works across the entire competitive set? What is a nice-to-have? What is absent from every page — and therefore exploitable? What do outlier pages do differently, and does it matter?

The output of this skill is the **direct input blueprint** for content strategy — a clean, prioritized, evidence-backed pattern summary that tells downstream skills exactly what to include, what to consider, and what to exploit in a new piece of content targeting this keyword.

### Primary Responsibilities

| Responsibility                       | Description                                                                                                          |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Structural Data Aggregation**      | Calculate statistical averages and ranges for word count, heading counts, and paragraph structure across all pages   |
| **Media Pattern Extraction**         | Calculate frequency percentages for every media type and classify media usage behavior                              |
| **AEO Pattern Extraction**           | Quantify how consistently top-ranking pages optimize for featured snippets and answer engines                       |
| **Quality Signal Aggregation**       | Calculate frequency percentages for all evidence and quality markers                                                |
| **Heading Structure Pattern Analysis**| Identify the dominant heading architecture pattern used by the competitive set                                      |
| **Outlier Detection and Analysis**   | Find pages that rank despite deviating from dominant patterns — and explain why                                     |
| **Opportunity Detection**            | Identify elements absent from all or most pages that new content can use as differentiation                         |
| **Priority Signal Classification**   | Classify every identified pattern as Must-Have, Good-to-Have, or Opportunity based on frequency thresholds          |
| **Hidden Pattern Detection**         | Surface non-obvious patterns by combining signals across multiple dimensions                                        |
| **Pattern Deduplication**            | Merge similar signals, eliminate overlapping insights, ensure output is clean and distinct                          |

### What This Skill Is NOT

- It is **not** a raw data collector. It receives pre-analyzed data from `deep-serp-analysis.skill` — it does not perform its own page analysis.
- It is **not** a content generator. It synthesizes pattern intelligence — the blueprint skill executes on it.
- It is **not** an assumption engine. Every pattern it reports must be supported by frequency data from the per-page analysis.
- It is **not** a simplifier. It does not reduce complexity — it organizes complexity into actionable tiers.
- It is **not** optional. No content blueprint may be generated without passing through this aggregation layer first.

### The Defining Standard of This Skill

> Every pattern statement produced by this skill must include:
> 1. The exact percentage of analyzed pages that exhibit this pattern.
> 2. The priority classification derived from that percentage (Must-Have / Good-to-Have / Opportunity).
> 3. A specific, non-generic description of what the pattern is — not what category it belongs to.
>
> No pattern statement may be vague. No pattern may be repeated across sections.
> Frequency percentages are the foundation — every insight derives from them.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input — Per-Page Analysis Data (from deep-serp-analysis.skill)

The complete Part 1 output from `deep-serp-analysis.skill` — the full per-page analysis records across all 8 dimensions for every filtered page. This is the only data source this skill operates on.

**Required Input Fields (per page):**

| Dimension      | Fields Required                                                                                      |
|----------------|------------------------------------------------------------------------------------------------------|
| D1 — Structure | Estimated word count range, paragraph length, H2 count, H3 count, structure pattern, intro approach |
| D2 — Media     | Image count + types, video presence, GIF count, infographic count, custom graphics, media density   |
| D3 — Elements  | Table count + types, comparison tables, code blocks, callout boxes, list counts, download elements  |
| D4 — SEO       | Keyword in H1, keyword in intro, semantic usage level, schema signals                               |
| D5 — AEO       | Direct answer in intro, FAQ presence + count, PAA coverage, summary block, structured answer format |
| D6 — Quality   | Original examples count, case studies, statistics (sourced/unsourced), expert quotes, original insights, depth indicator |
| D7 — Linking   | Internal link count range, internal link strategy, external link quality, resource section           |
| D8 — Formatting| Readability level, bullet usage, TOC presence, navigation aids                                      |
| Page Meta      | Position in SERP, overall page score, competitive threat rating                                     |

**Input Metadata (from deep-serp-analysis.skill header):**

| Field                    | Description                                                        |
|--------------------------|--------------------------------------------------------------------|
| `target_keyword`         | The keyword all analyzed pages rank for                           |
| `pages_analyzed`         | Total count of pages that passed the filter                       |
| `filter_mode`            | STRICT / NORMAL / RELAXED (affects confidence of pattern data)    |
| `serp_intelligence_loaded` | Whether SERP context was inherited                             |
| `analysis_timestamp`     | When the per-page analysis was performed                          |

### Input Pre-Conditions

1. **Is per-page analysis data complete?** Every analyzed page must have all 8 dimensions populated. If any page has null dimensions → flag those dimensions as incomplete for that page before aggregating.
2. **How many pages were analyzed?** If fewer than 5 pages → flag all pattern outputs as LOW CONFIDENCE. If fewer than 3 pages → halt and trigger failsafe.
3. **Has this pattern extraction been run before for this keyword?** → Query `memory-controller.skill`. If exists and within freshness threshold → offer cached. If stale → proceed fresh.
4. **Is filter mode RELAXED?** → Flag all outputs with: "Pattern data includes borderline pages — signals may have elevated noise."

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Content Pattern Intelligence Report

```
NEXUS CONTENT PATTERN INTELLIGENCE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Keyword          : [exact keyword]
Pages Analyzed          : [N]
Analysis Source         : deep-serp-analysis.skill
Pattern Confidence      : [HIGH (≥8 pages) / MEDIUM (5–7 pages) / LOW (<5 pages)]
Filter Mode             : [STRICT / NORMAL / RELAXED]
Generated By            : serp/content-pattern-extractor.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 1 — STRUCTURAL PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Word Count Profile
  Range Across All Pages  : [min] – [max] words
  Dominant Range          : [range covering ≥60% of pages]
  Midpoint Estimate       : [calculated midpoint of dominant range]
  Distribution:
    THIN (<800w)          : [N pages] — [%]
    SHORT (800–1500w)     : [N pages] — [%]
    MEDIUM (1500–2500w)   : [N pages] — [%]
    LONG (2500–4000w)     : [N pages] — [%]
    VERY LONG (4000–6000w): [N pages] — [%]
    COMPREHENSIVE (>6000w): [N pages] — [%]
  Priority Signal         : [MUST-HAVE / GOOD-TO-HAVE / OPPORTUNITY] — [rationale]

Heading Structure Profile
  Average H2 Count        : [N] (range: [min]–[max])
  Average H3 Count        : [N] (range: [min]–[max])
  Dominant H2:H3 Ratio    : [ratio]
  Pages with TOC          : [N] — [%] → [Priority]
  Pages with H4+ Depth    : [N] — [%] → [Priority]
  Heading Density Note    : [observation about heading frequency vs. word count]

Paragraph Structure Profile
  Dominant Paragraph Length: [SHORT / MEDIUM / LONG]
  Pages with SHORT Paragraphs: [N] — [%]
  Pages with MEDIUM Paragraphs: [N] — [%]
  Pages with LONG Paragraphs : [N] — [%]
  Structural Trend         : [observation about paragraph discipline across the set]

Intro Pattern Profile
  Direct Answer Intro     : [N pages] — [%] → [Priority]
  Context-Setting Intro   : [N pages] — [%] → [Priority]
  Hook Intro              : [N pages] — [%] → [Priority]
  Problem Statement Intro : [N pages] — [%] → [Priority]
  Dominant Intro Style    : [style with highest frequency]
  Intro Length Trend      : [observation on typical intro length]

Conclusion Pattern Profile
  Summary Conclusion      : [N pages] — [%]
  CTA Conclusion          : [N pages] — [%]
  Open Conclusion         : [N pages] — [%]
  No Detected Conclusion  : [N pages] — [%]
  Dominant Conclusion Style: [style]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 2 — MEDIA PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Pages Using Images          : [N] — [%] → [Priority]
  Pages Using Original Images : [N] — [%] → [Priority]
  Pages Using Stock Photos    : [N] — [%]
  Pages Using Screenshots     : [N] — [%] → [Priority]
  Pages Using Custom Diagrams : [N] — [%] → [Priority]
  Pages Using Infographics    : [N] — [%] → [Priority]
  Pages Using Charts/Graphs   : [N] — [%] → [Priority]
  Pages Using Videos          : [N] — [%] → [Priority]
  Pages Using GIFs            : [N] — [%]
  Pages Using NO Media        : [N] — [%]

  Dominant Media Type         : [type]
  Average Images Per Page     : [N]
  Media Density Pattern       : [HIGH / MEDIUM / LOW — frequency distribution]
  Media Strategic Observation : [specific non-generic insight about how media is used]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 3 — AEO PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Direct Answer in Intro      : [N] — [%] → [Priority]
  FAQ Section Present         : [N] — [%] → [Priority]
  Average FAQ Questions       : [N per page that has FAQ]
  PAA Questions Addressed     : [N] — [%] → [Priority]
  Summary / TLDR Block        : [N] — [%] → [Priority]
  Structured Answer Format    : [N] — [%] → [Priority]
  Definition Block Present    : [N] — [%] → [Priority]
  Snippet-Optimized Formatting: [N] — [%] → [Priority]
  Schema Signals Detected:
    FAQ Schema                : [N] — [%]
    HowTo Schema              : [N] — [%]
    Article Schema            : [N] — [%]
    Review Schema             : [N] — [%]
    None / Unknown            : [N] — [%]

  Dominant AEO Approach       : [description of the most common AEO strategy across the set]
  AEO Coverage Gap            : [what AEO elements the competitive set mostly misses]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 4 — QUALITY SIGNAL PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Pages Using Examples        : [N] — [%] → [Priority]
  Pages Using Original Examples: [N] — [%] → [Priority]
  Pages Using Case Studies    : [N] — [%] → [Priority]
  Pages Using Statistics      : [N] — [%] → [Priority]
  Pages Using Sourced Stats   : [N] — [%] → [Priority]
  Pages Using Unsourced Stats : [N] — [%]
  Pages Using Expert Quotes   : [N] — [%] → [Priority]
  Pages Using Attributed Quotes: [N] — [%] → [Priority]
  Pages Using Original Data   : [N] — [%] → [Priority]
  Pages Using Research Refs   : [N] — [%] → [Priority]
  Pages with Author Signal    : [N] — [%] → [Priority]

  Dominant Quality Level      : [SURFACE / MODERATE / DEEP / EXHAUSTIVE — most common]
  Quality Level Distribution:
    SURFACE                   : [N pages] — [%]
    MODERATE                  : [N pages] — [%]
    DEEP                      : [N pages] — [%]
    EXHAUSTIVE                : [N pages] — [%]
  Quality Ceiling Observation : [what is the highest quality level present and how common is it]
  Quality Floor Observation   : [what is the minimum quality level ranking pages can get away with]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 5 — HEADING STRUCTURE PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Dominant Pattern            : [Pattern A / B / C / D / Mixed / Novel]
  Pattern Frequency           : [N pages follow dominant pattern] — [%]
  Secondary Pattern           : [if applicable]
  Novel / Unclassified        : [N pages] — [%]

  Pattern A — Definition → How-To → Examples → FAQs
    Pages Following           : [N] — [%]
    Typical Word Count        : [range]
    Typical Intent            : [TOFU Informational / Mixed]

  Pattern B — Listicle → Breakdown → Comparison → Verdict
    Pages Following           : [N] — [%]
    Typical Word Count        : [range]
    Typical Intent            : [MOFU Commercial]

  Pattern C — Problem → Solution → Deep Dive → Next Steps
    Pages Following           : [N] — [%]
    Typical Word Count        : [range]
    Typical Intent            : [TOFU–MOFU Mixed]

  Pattern D — Overview → Steps → Tools → Mistakes
    Pages Following           : [N] — [%]
    Typical Word Count        : [range]
    Typical Intent            : [TOFU How-To]

  Section-Level Detail (for dominant pattern):
    Opening Section Type      : [definition / hook / problem framing / context]
    Middle Section Pattern    : [linear progression / topic branching / comparative]
    Closing Section Type      : [FAQ / next steps / CTA / summary / verdict]
    Common H2 Labels          : [most frequently used H2 phrasings across the set]
    Common H3 Patterns        : [how H3s are used within the dominant H2 structure]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 6 — OUTLIER ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Total Outlier Pages         : [N]

  [REPEATED FOR EACH OUTLIER:]
  Outlier Page [N]
    URL / Domain              : [value]
    Position                  : [#N]
    Deviation From Pattern    : [what dominant pattern this page does NOT follow]
    Deviation Severity        : [MILD / MODERATE / STRONG]
    Ranking Rationale         : [why this page likely ranks despite deviation]
    Pattern Implication       : [what this outlier reveals about the SERP's tolerance for format diversity]
    Strategic Lesson          : [what new content can learn from this outlier]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 7 — OPPORTUNITY DETECTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  WHAT NO PAGE INCLUDES (0% frequency):
    → [Element 1] — [strategic reason this matters]
    → [Element 2] — [strategic reason]
    → [Element 3] — [strategic reason]

  WHAT FEW PAGES INCLUDE (1–2 pages / <30%):
    → [Element 1] — [present in: N pages] — [implication]
    → [Element 2] — [present in: N pages] — [implication]

  WEAK AREAS ACROSS ALL PAGES:
    → [Area 1] — [what is universally underdeveloped and why it matters]
    → [Area 2] — [area description]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 8 — PRIORITY SIGNAL CLASSIFICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  MUST-HAVE SIGNALS (present in 60%+ of pages):
  | Signal                        | Pages | %    | Dimension | Execution Requirement              |
  |-------------------------------|-------|------|-----------|------------------------------------|
  | [signal]                      | [N/N] | [%]  | [D#]      | [what new content must do]         |
  | ...                           |       |      |           |                                    |

  GOOD-TO-HAVE SIGNALS (present in 30–59% of pages):
  | Signal                        | Pages | %    | Dimension | Execution Recommendation           |
  |-------------------------------|-------|------|-----------|------------------------------------|
  | [signal]                      | [N/N] | [%]  | [D#]      | [what new content should do]       |
  | ...                           |       |      |           |                                    |

  OPPORTUNITY SIGNALS (present in <30% of pages):
  | Signal                        | Pages | %    | Dimension | Differentiation Potential          |
  |-------------------------------|-------|------|-----------|------------------------------------|
  | [signal]                      | [N/N] | [%]  | [D#]      | [how this creates competitive edge]|
  | ...                           |       |      |           |                                    |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PATTERN SUMMARY (Blueprint-Ready)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Word Count       : [dominant range midpoint] words (range: [min]–[max])
Target H2 Count         : [average] headings
Target H3 Count         : [average] sub-headings
Dominant Structure      : [Pattern A / B / C / D / Mixed]
Required Elements       : [comma-separated must-have signals]
Recommended Elements    : [comma-separated good-to-have signals]
Differentiation Angles  : [comma-separated opportunity signals]
AEO Requirement         : [YES — elements required / PARTIAL / NO]
Quality Baseline        : [minimum depth level new content must meet]
Media Requirement       : [what media types are MUST-HAVE]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into              : serp-blueprint-generator.skill
                          content-generation-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — AGGREGATE STRUCTURAL DATA

Aggregate the Dimension 1 (Content Structure) data from all per-page analyses into a comprehensive structural profile.

**Phase 1A — Word Count Aggregation**

Collect the word count range for every analyzed page. Word count data from `deep-serp-analysis.skill` arrives as ranges (e.g., 2,000–2,500), not exact values.

**Midpoint Calculation:**

For each page's word count range, calculate the midpoint:
`midpoint = (range_min + range_max) / 2`

**Statistical Calculations:**

| Metric                      | Calculation Method                                                                  |
|-----------------------------|--------------------------------------------------------------------------------------|
| **Overall range**           | Min of all page range minimums → Max of all page range maximums                    |
| **Dominant range**          | The word count category (THIN / SHORT / MEDIUM / LONG / VERY LONG / COMPREHENSIVE) present in ≥60% of pages |
| **Midpoint estimate**       | Average of all per-page midpoints                                                   |
| **Distribution**            | Count and percentage of pages in each word count category                           |

**Word Count Priority Signal:**

| Condition                                                               | Priority Signal |
|-------------------------------------------------------------------------|-----------------|
| ≥60% of pages in the same word count category                          | MUST-HAVE — match this range |
| 40–59% of pages in the same category                                   | GOOD-TO-HAVE — target this range |
| No single category has ≥40% of pages                                   | MIXED — flag as hybrid; recommend the two most common categories |

**Outlier Word Count Detection:**

If any page's midpoint falls more than 2 standard deviations from the group mean → flag as word count outlier. Analyze separately in Step 6.

---

**Phase 1B — Heading Count Aggregation**

Collect H2 and H3 counts from all analyzed pages.

**Calculations:**

| Metric                | Calculation                                                                    |
|-----------------------|--------------------------------------------------------------------------------|
| Average H2 count      | Sum of all H2 counts / number of pages                                         |
| H2 range              | Min H2 count → Max H2 count across all pages                                  |
| Average H3 count      | Sum of all H3 counts / number of pages                                         |
| H3 range              | Min H3 count → Max H3 count across all pages                                  |
| Dominant H2:H3 ratio  | Most common ratio relationship (e.g., 1:2, 1:3, 1:0)                          |
| TOC presence rate     | Count of pages with TOC / total pages × 100                                    |
| H4+ depth rate        | Count of pages with H4 or deeper / total pages × 100                          |
| Heading density       | Average headings per 500 words across all pages                               |

**Heading Density Interpretation:**

| Density                     | Label               | Implication                                          |
|-----------------------------|---------------------|------------------------------------------------------|
| >1 heading per 300 words    | DENSE               | Heavy organizational structure; likely listicle/guide|
| 1 heading per 300–500 words | STANDARD            | Normal editorial structure                           |
| 1 heading per 500–800 words | SPARSE              | Prose-dominant; fewer structural breaks              |
| <1 heading per 800 words    | MINIMAL             | Content relies on prose flow; few structural markers |

---

**Phase 1C — Paragraph Length Trend**

Aggregate paragraph length classifications (SHORT / MEDIUM / LONG) from all pages:

1. Count pages in each paragraph length category.
2. Calculate percentage for each.
3. Identify dominant paragraph length (highest percentage).
4. Note paragraph length consistency — if results are split, classify as MIXED.

**Paragraph Trend Interpretation:**

| Dominant Paragraph Length | Structural Implication                                                    |
|---------------------------|---------------------------------------------------------------------------|
| SHORT (<50w avg)          | High scan-ability focus; content is formatted for quick reading          |
| MEDIUM (50-100w avg)      | Balanced reading experience; standard editorial format                   |
| LONG (>100w avg)          | Prose-dominant; deep reading format; lower scan-ability                  |

---

**Phase 1D — Intro and Conclusion Pattern Aggregation**

Count and percentage-calculate all intro approach types (Direct Answer / Context-Setting / Hook / Problem Statement) and conclusion types (Summary / CTA / Open / None).

Identify the dominant intro style — the type with the highest frequency.

**Intro Pattern Structural Implication:**

| Dominant Intro Style  | What It Signals for New Content                                               |
|-----------------------|-------------------------------------------------------------------------------|
| Direct Answer         | SERP rewards answer-first content — new content must open with the answer     |
| Context-Setting       | SERP tolerates scene-setting — but direct answer still differentiates         |
| Hook                  | SERP accepts engagement-first — unusual for informational keywords             |
| Problem Statement      | SERP is solution-oriented — content framed as problem → solution performs well |

---

### ▸ STEP 2 — MEDIA PATTERN EXTRACTION

Aggregate all Dimension 2 (Media) data from per-page analyses into a comprehensive media pattern profile.

**Phase 2A — Media Type Frequency Calculation**

For every media type tracked in `deep-serp-analysis.skill`, calculate:
- **Presence count:** How many of the analyzed pages include this media type.
- **Presence percentage:** `count / total_pages × 100`
- **Priority classification:** Apply threshold rules from Step 8.

**Media Types to Calculate:**

| Media Type              | Presence Count | Presence % | Priority Classification |
|-------------------------|----------------|------------|-------------------------|
| Any images              | [N]            | [%]        | [Priority]              |
| Original / custom images| [N]            | [%]        | [Priority]              |
| Stock photos            | [N]            | [%]        | (note only)             |
| Screenshots             | [N]            | [%]        | [Priority]              |
| Custom diagrams         | [N]            | [%]        | [Priority]              |
| Infographics            | [N]            | [%]        | [Priority]              |
| Charts or graphs        | [N]            | [%]        | [Priority]              |
| Embedded videos         | [N]            | [%]        | [Priority]              |
| GIFs                    | [N]            | [%]        | (note only)             |
| No media at all         | [N]            | [%]        | (inverse signal)        |

**Phase 2B — Average Image Count Calculation**

For pages that use images:
- Sum all image counts across image-using pages.
- Divide by the number of pages using images.
- Report as: "Pages that use images average [N] images per page."

**Phase 2C — Media Density Pattern**

Calculate the distribution of media density (HIGH / MEDIUM / LOW) across all pages:
- HIGH density: Count and percentage of pages with >1 media element per 300 words.
- MEDIUM density: Count and percentage of pages with 1 per 300–500 words.
- LOW density: Count and percentage of pages with <1 per 500 words.

**Phase 2D — Media Strategic Observation**

After all calculations, synthesize a single specific observation about HOW the competitive set uses media — not just that they use it. This must address:
- Is media primarily decorative (stock photos) or explanatory (diagrams, screenshots)?
- Is media concentrated at specific points in the content (intro, process sections, comparisons)?
- Is there a relationship between media density and page score?

This observation must be specific to the analyzed pages — not a generic statement about image optimization.

---

### ▸ STEP 3 — AEO PATTERN EXTRACTION

Aggregate all Dimension 5 (AEO Signals) data from per-page analyses into a comprehensive answer optimization pattern profile.

**Phase 3A — AEO Element Frequency Calculation**

For every AEO element tracked in `deep-serp-analysis.skill`, calculate presence count and percentage:

| AEO Element                        | Presence Count | Presence % | Priority Classification |
|------------------------------------|----------------|------------|-------------------------|
| Direct answer in intro (<100w)     | [N]            | [%]        | [Priority]              |
| FAQ section present                | [N]            | [%]        | [Priority]              |
| PAA questions addressed            | [N]            | [%]        | [Priority]              |
| Summary / TLDR block               | [N]            | [%]        | [Priority]              |
| Definition block                   | [N]            | [%]        | [Priority]              |
| Structured answer format           | [N]            | [%]        | [Priority]              |
| Snippet-optimized formatting       | [N]            | [%]        | [Priority]              |

**Phase 3B — Average FAQ Count**

For pages that have a FAQ section:
- Sum all FAQ question counts.
- Divide by number of pages with FAQs.
- Report as: "Pages with FAQ sections average [N] questions per FAQ."

**Phase 3C — Schema Distribution**

Calculate the distribution of schema types across all pages:
- Count and percentage for each schema type detected (FAQ, HowTo, Article, Review, None/Unknown).
- Identify dominant schema type.

**Phase 3D — AEO Coverage Gap Identification**

Compare the AEO presence percentages:
- Any AEO element present in <30% of pages is an AEO coverage gap.
- Any AEO element present in 0% of pages is a zero-presence AEO opportunity.
- Record both types for the Opportunity Detection step.

**Phase 3E — Dominant AEO Approach Synthesis**

Based on the frequency data, describe the overall AEO posture of the competitive set:

| AEO Posture Type           | Condition                                                                          |
|----------------------------|------------------------------------------------------------------------------------|
| **Strongly optimized**     | ≥60% of pages have direct answer in intro AND FAQ section AND PAA coverage        |
| **Partially optimized**    | ≥60% have 1–2 of the three core AEO elements but not all                         |
| **Weakly optimized**       | <40% of pages have any core AEO element                                           |
| **Unoptimized**            | <20% of pages have any AEO signal — significant AEO opportunity for new content   |

---

### ▸ STEP 4 — QUALITY SIGNAL AGGREGATION

Aggregate all Dimension 6 (Quality Signals) data from per-page analyses into a comprehensive quality profile.

**Phase 4A — Quality Signal Frequency Calculation**

For every quality signal tracked in `deep-serp-analysis.skill`, calculate presence count and percentage:

| Quality Signal                  | Presence Count | Presence % | Priority Classification |
|---------------------------------|----------------|------------|-------------------------|
| Any examples (original or not)  | [N]            | [%]        | [Priority]              |
| Original examples specifically  | [N]            | [%]        | [Priority]              |
| Case studies (any)              | [N]            | [%]        | [Priority]              |
| First-party case studies        | [N]            | [%]        | [Priority]              |
| Statistics (any)                | [N]            | [%]        | [Priority]              |
| Sourced statistics              | [N]            | [%]        | [Priority]              |
| Unsourced statistics            | [N]            | [%]        | (risk signal)           |
| Expert quotes (any)             | [N]            | [%]        | [Priority]              |
| Attributed expert quotes        | [N]            | [%]        | [Priority]              |
| Original data / research        | [N]            | [%]        | [Priority]              |
| Research references             | [N]            | [%]        | [Priority]              |
| Author signal (byline + bio)    | [N]            | [%]        | [Priority]              |

**Phase 4B — Depth Level Distribution**

Calculate the distribution of content depth levels across all pages:
- Count pages at each depth level: SURFACE / MODERATE / DEEP / EXHAUSTIVE.
- Calculate percentage for each.
- Identify dominant depth level.

**Quality Ceiling and Floor:**

| Metric               | Definition                                                                     |
|----------------------|--------------------------------------------------------------------------------|
| **Quality ceiling**  | The highest depth level present in the competitive set and how many pages reach it |
| **Quality floor**    | The lowest depth level that any page exhibits while still ranking in the top 10 |

The quality floor is strategically critical — it defines the minimum a new piece of content must achieve to remain competitive. The quality ceiling defines what "best in class" looks like on this SERP.

**Phase 4C — Quality Pattern Synthesis**

After calculating all frequencies, synthesize one specific, non-generic insight about the quality behavior of the competitive set:

- Is the SERP generally demanding high quality or tolerating surface-level content?
- Is there a quality gap between results #1–3 and results #7–10?
- Are sourced statistics and expert attribution common, creating an evidence standard?

This synthesis must reference specific percentage data — not general quality principles.

---

### ▸ STEP 5 — HEADING STRUCTURE PATTERN ANALYSIS

Identify the dominant heading architecture pattern used across the competitive set and produce a section-level structural profile.

**Phase 5A — Pattern Classification**

For each analyzed page, classify its heading structure into one of the defined patterns — or flag it as NOVEL if no defined pattern fits.

**Pattern Classification Criteria:**

**Pattern A — Definition → How-To → Examples → FAQs**

| Characteristics                                                                              |
|----------------------------------------------------------------------------------------------|
| Opens with a definitional or explanatory section (What is X?)                               |
| Transitions to procedural/instructional sections (How to do X, Steps to achieve X)         |
| Includes an examples section (Real-world examples, Case examples)                          |
| Closes with a FAQ or Q&A section                                                            |
| Typical content type: Educational blog post, comprehensive guide                            |
| Typical funnel stage: TOFU–Informational                                                    |

**Pattern B — Listicle → Breakdown → Comparison → Verdict**

| Characteristics                                                                              |
|----------------------------------------------------------------------------------------------|
| Opens with a numbered or bulleted list structure as the primary organization                |
| Each list item receives a breakdown section (expanded explanation)                          |
| Includes a comparison layer (evaluating items against each other)                           |
| Closes with a verdict, recommendation, or winner declaration                               |
| Typical content type: Comparison article, roundup, ranked list                             |
| Typical funnel stage: MOFU–Commercial                                                      |

**Pattern C — Problem → Solution → Deep Dive → Next Steps**

| Characteristics                                                                              |
|----------------------------------------------------------------------------------------------|
| Opens by defining or amplifying the problem the reader faces                               |
| Introduces the solution and positions the core topic as the answer                         |
| Provides deep analysis, advanced tactics, or comprehensive coverage of the solution        |
| Closes with actionable next steps, a roadmap, or further resources                        |
| Typical content type: Strategy guide, problem-solution article                             |
| Typical funnel stage: TOFU–MOFU Mixed                                                      |

**Pattern D — Overview → Steps → Tools → Mistakes**

| Characteristics                                                                              |
|----------------------------------------------------------------------------------------------|
| Opens with a topic overview (what the process/practice is and why it matters)              |
| Provides numbered or sectioned step-by-step process                                        |
| Includes a tools/resources section (recommended tools, platforms, or resources)            |
| Closes with a common mistakes section (what to avoid)                                      |
| Typical content type: How-to guide, tutorial, playbook                                     |
| Typical funnel stage: TOFU–Informational                                                   |

**Novel Pattern:**
If a page's heading structure does not fit any of the four defined patterns:
- Document the actual structure in detail.
- Identify its unique organizational logic.
- Include it as a Novel pattern in the output.

**Phase 5B — Pattern Frequency Calculation**

Count pages following each pattern:
- Pattern A: [N pages] — [%]
- Pattern B: [N pages] — [%]
- Pattern C: [N pages] — [%]
- Pattern D: [N pages] — [%]
- Novel / Mixed: [N pages] — [%]

Identify the dominant pattern (highest frequency). If two patterns tie or are within 1 page of each other → classify as MIXED dominant.

**Phase 5C — Section-Level Detail for Dominant Pattern**

For the pattern with the highest frequency, extract the recurring section-level details:

1. **Common H2 Labels:** What heading phrases recur across pages following this pattern?
   - List the 5 most common H2 phrasings (exact or near-exact).
   - These are the universal structural anchors the SERP rewards.

2. **H3 Usage Pattern:** How are H3s used within the dominant H2 structure?
   - Are H3s used to expand sub-topics?
   - Are H3s used as numbered steps?
   - Are H3s used for named examples or case studies?

3. **Opening Section Type:** What kind of content typically occupies the first major section?

4. **Middle Section Architecture:** Does the middle of the dominant pattern follow a linear progression, topic-based branching, or comparative structure?

5. **Closing Section Type:** What ends the content? (FAQ / next steps / CTA / summary verdict)

**Phase 5D — Pattern Confidence Assessment**

| Pattern Confidence | Condition                                                                        |
|--------------------|----------------------------------------------------------------------------------|
| HIGH               | ≥60% of pages follow the dominant pattern                                        |
| MEDIUM             | 40–59% of pages follow the dominant pattern                                      |
| LOW                | <40% of pages follow any single pattern — SERP is architecturally diverse        |

If pattern confidence is LOW → classify as MIXED and recommend hybrid architecture for new content.

---

### ▸ STEP 6 — OUTLIER DETECTION AND ANALYSIS

Identify pages in the analyzed set that rank despite NOT following the dominant patterns identified in Steps 1–5.

**Phase 6A — Outlier Identification**

A page is classified as an outlier if it deviates from the dominant pattern on ≥3 of the following signals:

| Outlier Signal                                                        | Deviation Threshold                               |
|-----------------------------------------------------------------------|---------------------------------------------------|
| Word count far from dominant range                                    | Outside the dominant range by ≥50%               |
| Heading count significantly different                                 | H2 count deviates by ≥30% from average            |
| Does NOT follow dominant heading structure pattern                    | Page uses a different structural pattern          |
| Does NOT use any of the MUST-HAVE media elements                      | Missing ≥2 must-have media types                  |
| Missing MUST-HAVE content elements                                    | Missing ≥2 elements classified as must-have       |
| Quality level is significantly below the group floor                  | SURFACE quality in a group where floor is MODERATE|

**Phase 6B — Outlier Deviation Severity Classification**

| Severity  | Condition                                                           |
|-----------|---------------------------------------------------------------------|
| MILD      | Deviates from 3 of the outlier signals                             |
| MODERATE  | Deviates from 4–5 of the outlier signals                           |
| STRONG    | Deviates from 6 or more of the outlier signals                     |

**Phase 6C — Outlier Ranking Rationale Analysis**

For each outlier page, determine why it ranks despite its deviations. Possible rationale types:

| Rationale Type               | Description                                                                       |
|------------------------------|-----------------------------------------------------------------------------------|
| **Domain authority**         | The page ranks primarily due to the domain's overall authority, not content merit |
| **Age and link equity**      | The page has accumulated backlinks over many years that sustain its position      |
| **Unique format advantage**  | The page's unusual format is actually more useful for the searcher than standard pages |
| **Specific sub-intent focus**| The page targets a narrow sub-intent within the keyword that other pages miss     |
| **SERP feature capture**     | The page owns a featured snippet or PAA answer that drives its position          |
| **Brand recognition**        | The domain is recognized in the query context (navigational component)           |
| **Recency boost**            | The page ranks due to recent publication date in a freshness-sensitive SERP      |

**Phase 6D — Strategic Lesson Extraction**

For each outlier, extract one specific strategic lesson for new content:
- If the outlier ranks due to domain authority → strategic lesson: "Ranking here without authority investment requires exceptional content depth to compensate."
- If the outlier ranks due to a unique format → strategic lesson: "This format deviation suggests the SERP is not rigid — format experimentation has demonstrated upside."
- If the outlier ranks due to sub-intent focus → strategic lesson: "This niche focus reveals an under-served sub-intent that new content could own entirely."

---

### ▸ STEP 7 — OPPORTUNITY DETECTION

Identify the competitive gaps that new content can exploit — drawing from all pattern data aggregated in Steps 1–6.

**Phase 7A — Zero-Presence Opportunity Detection**

Scan all aggregated frequency data across all 8 dimensions. Every element with a 0% presence rate is a zero-presence opportunity.

**Zero-Presence Identification Method:**

For each dimension, identify any tracked element where presence count = 0 across all analyzed pages.

For each zero-presence element:
1. Name it precisely.
2. Explain why including it would benefit the content's ranking or user experience.
3. Estimate the effort required to include it.
4. Classify its strategic value: HIGH / MEDIUM / LOW.

**Zero-Presence Element Strategic Value Classification:**

| Value | Condition                                                                        |
|-------|----------------------------------------------------------------------------------|
| HIGH  | Element directly serves the dominant search intent AND is commonly expected by searchers but missing from all pages |
| MEDIUM| Element improves content quality or completeness but is not critical to the search intent |
| LOW   | Element is a nice-to-have that adds marginal value                               |

**Phase 7B — Low-Presence Opportunity Detection**

Identify elements present in 1–2 pages only (< 30% but > 0%). These are low-presence opportunities.

For each low-presence element:
1. Name it and cite which specific pages use it.
2. Analyze whether the pages using it perform better than pages that do not (compare position and page score).
3. If yes → classify as EMERGING BEST PRACTICE and prioritize in recommendations.
4. If no → classify as WEAK SIGNAL and note as optional differentiation.

**Phase 7C — Universal Weakness Detection**

Identify areas where ALL analyzed pages exhibit the same weakness — not just absence but underdevelopment.

**Weakness Detection Criteria:**

An element is a universal weakness if:
- It is present in most pages (>60%) but consistently at low quality.
- Example: Statistics are present in 80% of pages but are all unsourced → universal weakness is evidence quality, not statistics presence.
- Example: FAQs are present in 70% of pages but all have ≤3 questions → universal weakness is FAQ depth, not FAQ presence.

For each universal weakness:
1. Name the element and the weakness aspect.
2. Calculate what percentage of pages exhibit this weakness.
3. Explain how executing this element at a higher quality level creates competitive advantage.

---

### ▸ STEP 8 — PRIORITY SIGNAL CLASSIFICATION

Classify every identified pattern into one of three priority tiers based on frequency thresholds.

**Priority Threshold Definitions:**

| Priority Tier          | Frequency Threshold | Definition                                                                             |
|------------------------|---------------------|----------------------------------------------------------------------------------------|
| **MUST-HAVE**          | ≥60% of pages       | Present in the majority of competing pages. Absence creates a structural disadvantage. New content must include this. |
| **GOOD-TO-HAVE**       | 30–59% of pages     | Present in a significant minority. Inclusion strengthens the content without being mandatory. Recommended. |
| **OPPORTUNITY**        | <30% of pages       | Present in few or no pages. Including this creates competitive differentiation. Absence is the norm — inclusion is the advantage. |

**Application Protocol:**

1. Take every frequency-calculated signal from Steps 1–7.
2. Apply the threshold to determine its priority tier.
3. Record it in the appropriate priority table in the output.
4. Every signal in the Must-Have table requires an execution requirement.
5. Every signal in the Good-to-Have table requires an execution recommendation.
6. Every signal in the Opportunity table requires a differentiation potential statement.

**Priority Classification Rules:**

- A signal cannot appear in more than one priority tier.
- If a signal is at exactly 60% → assign to MUST-HAVE (boundary goes to the higher tier).
- If a signal is at exactly 30% → assign to GOOD-TO-HAVE (boundary goes to the higher tier).
- Every must-have signal must be traceable to a specific dimension and frequency calculation.
- No signal may be added to any tier without a documented frequency percentage.

**Execution Requirement Format (Must-Have):**

`"[Signal]: New content must include [specific element description]. Execution: [concrete description of how to implement this in the content]."`

**Execution Recommendation Format (Good-to-Have):**

`"[Signal]: Recommended for content completeness. Consider [specific implementation approach]."`

**Differentiation Potential Format (Opportunity):**

`"[Signal]: Only [N] of [total] competing pages include this. Including it positions new content in the top [%] of competitors for this element. Implementation: [specific approach]."`

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ PATTERN WEIGHTING BY POSITION

Not all pages contribute equally to pattern conclusions. Pages in higher positions carry more weight because they more accurately represent what Google is currently rewarding.

**Position-Based Pattern Weight:**

| SERP Position | Weight Applied to Page's Patterns |
|---------------|-----------------------------------|
| #1–3          | 2.0× (highest weight)            |
| #4–6          | 1.5×                              |
| #7–10         | 1.0× (baseline)                   |
| #11–20        | 0.5× (if included via relaxed filter) |

**Weighted Frequency Calculation:**

For each pattern signal, calculate a weighted frequency:
`weighted_frequency = Σ(page_weight × presence_indicator) / Σ(all_page_weights) × 100`

Where `presence_indicator` = 1 if the page has the signal, 0 if not.

**Weighted vs. Raw Frequency:**

Report BOTH raw frequency (simple percentage) and weighted frequency for every Must-Have signal. If they diverge significantly (>15 percentage points):
- Highlight the divergence.
- Note which metric is more reliable for the specific signal type.

For structural signals (word count, heading count) → weighted frequency is the primary metric.
For presence/absence signals (FAQ, TOC, direct answer) → raw frequency is the primary metric.

---

### ▸ HIDDEN PATTERN DETECTION

Beyond individual signals, this skill detects hidden patterns that emerge only when signals from multiple dimensions are analyzed in combination.

**Hidden Pattern Types:**

**Combination Pattern — Structure + Media:**
Example: If pages with LONG word counts ALSO consistently use custom diagrams → Hidden pattern: "Comprehensive content on this SERP pairs prose depth with original visual explanation."

**Combination Pattern — AEO + Quality:**
Example: If pages with direct answer in intro ALSO consistently have sourced statistics in that intro → Hidden pattern: "Featured-snippet-targeting on this SERP requires evidence-backed direct answers — not just definitional answers."

**Combination Pattern — Structure + Position:**
Example: If the top-3 pages all follow Pattern A but pages #7–10 follow Pattern D → Hidden pattern: "Pattern A correlates with higher positions — the SERP rewards definition-led content over tutorial-led content despite similar topics."

**Combination Pattern — Quality + AEO:**
Example: If all pages with FAQ sections also have high author signal → Hidden pattern: "FAQ sections on this SERP appear in conjunction with author credibility signals — adding FAQs without establishing author authority may underperform."

**Hidden Pattern Detection Protocol:**

1. After all individual signal calculations are complete, run correlation checks between the following dimension pairs:
   - D1 (Structure) + D2 (Media)
   - D1 (Structure) + D6 (Quality)
   - D5 (AEO) + D6 (Quality)
   - D4 (SEO) + D5 (AEO)
   - D1 (Structure) + D3 (Elements)
   - D6 (Quality) + Page Position

2. For each dimension pair, check whether pages that rank in positions #1–3 share a combination of signals not present in pages #7–10.

3. If a combination signal pattern is identified in ≥4 pages AND correlates with higher position → classify as HIDDEN PATTERN and include in the output.

4. Hidden patterns are recorded in the Pattern Summary section after the Priority Signal Classification.

---

### ▸ RELATED SIGNAL COMBINATION

When two individually weak signals (each present in 20–29% of pages) always appear together on the same pages, they form a **combined signal** that carries more strategic weight than either individual signal.

**Combination Rule:**

If Signal A and Signal B both appear in <30% of pages individually, but they co-occur on the SAME pages in >80% of their instances → they are a combined signal.

A combined signal is:
- Reported as a single entry (not two separate entries).
- Classified at the weighted-average frequency of the combination.
- Given a single execution requirement or differentiation potential statement.

Example: "Infographics" (present in 25% of pages) and "custom diagrams" (present in 22% of pages) always co-occur on the same 2 pages → Combined signal: "Original visual content (infographics + diagrams)" — present in 22% of pages → OPPORTUNITY tier.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — Merge Similar Patterns Across Dimensions

If the same underlying observation appears in multiple dimensions under different labels, merge them into a single entry.

Example: "Content uses bullet points heavily" (D8 — Formatting) and "Multiple unordered lists present" (D3 — Elements) describe the same content behavior. Merge into a single signal: "Heavy list-based formatting" with a combined dimension reference (D3+D8).

Merging protocol:
1. After all frequency calculations are complete, scan for signals that describe the same behavior from different angles.
2. If two signals share the same strategic implication and appear on the same set of pages → merge.
3. After merging: use the higher of the two raw frequencies for priority classification.
4. Record the merge in a deduplication log appended to the internal analysis (not shown in output).

### Rule 2 — No Repeated Insights Across Sections

Once a pattern is recorded in Section 1 (Structural), it cannot be restated in Section 5 (Heading Patterns) or Section 8 (Priority Signals). It is referenced by name in subsequent sections but not re-explained.

Exception: The Priority Signal table in Section 8 re-lists all must-have signals — this is intentional and not a duplication violation because the table format serves a different function (execution guidance) from the analysis section.

### Rule 3 — No Generic Opportunity Statements

Opportunities in Section 7 must not duplicate each other through generic phrasing. Each opportunity must name a specific element that is absent or underdeveloped — not a category of elements.

Prohibited: "Content lacks original visuals." (too broad)
Required: "Original custom diagrams explaining [specific concept] are absent from all analyzed pages." (specific and actionable)

### Rule 4 — No Priority Tier Overlaps

A signal cannot appear in multiple priority tiers. If a signal qualifies for Must-Have by raw frequency but Opportunity by weighted frequency → assign to the tier of the primary metric for that signal type (weighted for structural; raw for presence/absence).

### Rule 5 — Memory Deduplication

Query `memory-controller.skill` before producing output. If a pattern extraction has been run for this keyword within freshness threshold (7 days) → present cached result and ask: "Use existing pattern report or generate fresh?" Never store two pattern reports for the same keyword within the freshness window.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Pattern Reporting Behaviors

| Prohibited Behavior                                                                            | Reason                                                                         |
|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| Reporting a pattern without citing the page count and percentage                              | All patterns must be frequency-grounded. No percentage = not a pattern.       |
| Stating "high-quality content performs better" as a pattern insight                           | Universal truism. Not a pattern. Replace with specific quality signal data.   |
| Reporting a pattern observed in only 1 page as more than an outlier                          | 1 page = outlier. Not a pattern. Not a good-to-have. At most an opportunity.  |
| Reporting the same signal in both the pattern sections AND the opportunity section            | Signals are either present or absent at pattern threshold — not both.          |
| Writing an opportunity statement that applies to any keyword (not this specific SERP)        | Every opportunity must be SERP-specific. Generic opportunities are not insights.|
| Omitting the execution requirement for Must-Have signals                                      | Must-Have signals without execution guidance are incomplete.                   |
| Treating weighted frequency and raw frequency as interchangeable                              | They must be reported and interpreted separately for structural signals.       |
| Skipping Step 4 (quality signals) because the data seems subjective                          | Quality signals are frequency-calculated from per-page analysis. Not subjective.|
| Classifying a signal as MUST-HAVE because it "feels important"                               | Priority tier is determined by frequency threshold only — not perceived importance.|
| Reporting a heading structure pattern without providing the section-level detail              | Pattern A/B/C/D classification is insufficient without the H2-level detail.   |

### Required Pattern Reporting Behaviors

- Every frequency percentage must cite the exact page count (N of M analyzed pages).
- Every must-have signal must include a concrete execution requirement.
- Every opportunity signal must cite how many pages exhibit it (even if zero).
- The Pattern Summary at the end of the output must consolidate everything into blueprint-ready directives.
- Hidden patterns must be reported when detected — they cannot be omitted for brevity.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ deep-serp-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | deep-serp-analysis → content-pattern-extractor (primary data source)                        |
| **Receives**     | Complete Part 1 per-page analysis records across all 8 dimensions for all filtered pages    |
| **Dependency**   | This skill cannot run without deep-serp-analysis.skill completing first                     |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before aggregation, WRITE after output finalization                    |
| **Reads**        | Prior pattern extraction for this keyword (freshness check)                                 |
| **Writes**       | Complete pattern intelligence report, priority signal tables, opportunity list              |
| **Memory layer** | strategy-memory.skill                                                                         |
| **Timing**       | READ at pre-conditions. WRITE after Step 8 completes.                                       |

---

### ▸ serp-blueprint-generator.skill (article-blueprint.skill)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-pattern-extractor → blueprint generator (primary output consumer)                  |
| **Sends**        | Full pattern intelligence report including Pattern Summary (blueprint-ready directives)     |
| **Used for**     | Generating a structurally grounded content blueprint based on competitive pattern data      |
| **Critical field**| Pattern Summary — this is the primary field consumed by blueprint-generator               |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-pattern-extractor → content-generation-engine (indirect, via blueprint)            |
| **Sends**        | Must-have elements list, quality baseline, AEO requirements, media requirements            |
| **Used for**     | Ensuring generated content meets the competitive bar at execution time                     |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                       | Detection                                           | Recovery Action                                                                                             |
|------------------------------------------------------------------------|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Fewer than 3 pages of analysis data received                           | Input pre-condition: pages_analyzed < 3            | Halt. Notify: "Insufficient page data for pattern extraction (minimum 3 required). Request deep-serp-analysis.skill to expand result set." |
| A per-page analysis record has incomplete dimensions (null fields)     | Input validation finds null dimension fields       | Exclude null fields from calculations for that page. Note which dimensions are incomplete. Reduce denominator for affected calculations. |
| All pages follow the same pattern with no variance                     | Steps 1–5 produce identical values across all pages | Flag: "Highly uniform SERP — dominant pattern is consistent. New content must match pattern AND differentiate on quality depth, not structure." |
| Pattern frequency data is inconsistent (pages appear to contradict each other) | Variance in frequency data is high across signals | Mark affected signals as MIXED PATTERN. Recommend hybrid content strategy covering both pattern approaches. |
| No dominant heading structure pattern is identified (all patterns tie) | Step 5B produces no clear winner                    | Classify as MIXED. Report all patterns equally. Recommend: build on the pattern that best fits the validated intent from serp-intelligence.skill. |
| Zero-presence opportunities are absent (all elements covered by ≥1 page) | Step 7A finds no 0% elements                    | Shift focus entirely to universal weaknesses and quality differentiation. Flag: "No zero-presence opportunities — competitive set is broad. Differentiation must come from execution quality." |
| Weighted frequency significantly contradicts raw frequency for a Must-Have signal | Divergence >15 percentage points        | Report both. Flag divergence. Explain: "Lower position pages drive this signal — higher-ranked pages are less likely to use it. This is a quality signal, not an authority signal." |
| Memory controller is unavailable                                       | READ query returns error                           | Proceed without freshness check. Flag output with: "Memory unavailable — duplicate pattern extraction risk exists." |
| Input data arrives from a source other than deep-serp-analysis.skill   | Source tag is missing or different                  | Warn: "Unverified data source. Pattern extraction may have reduced reliability." Proceed with explicit low-confidence flag. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — PRIORITY THRESHOLD QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PRIORITY CLASSIFICATION THRESHOLDS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MUST-HAVE     → ≥60% of analyzed pages
  → New content MUST include this element
  → Absence creates structural disadvantage
  → Requires execution requirement in output

GOOD-TO-HAVE  → 30–59% of analyzed pages
  → New content SHOULD include this element
  → Inclusion strengthens competitiveness
  → Requires execution recommendation in output

OPPORTUNITY   → <30% of analyzed pages (including 0%)
  → New content CAN differentiate with this element
  → Including it positions content above the norm
  → Requires differentiation potential statement in output

BOUNDARY RULES:
  Exactly 60% → MUST-HAVE
  Exactly 30% → GOOD-TO-HAVE
  0% present  → OPPORTUNITY (zero-presence — highest value)

CONFIDENCE MODIFIERS:
  HIGH confidence (≥8 pages): thresholds apply as stated
  MEDIUM confidence (5–7 pages): add ±10% margin of error note
  LOW confidence (<5 pages): all tiers marked as PROVISIONAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — AGGREGATION CALCULATION REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
STANDARD CALCULATIONS USED IN THIS SKILL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Raw Frequency %:
  count_of_pages_with_signal / total_pages_analyzed × 100

Weighted Frequency %:
  Σ(page_weight × presence_indicator) / Σ(all_page_weights) × 100

Average (for count-based metrics like H2, word count):
  Σ(all_page_values) / total_pages_analyzed

Range:
  [minimum_value_across_all_pages] – [maximum_value_across_all_pages]

Dominant Range (word count):
  The word count category (classification label) present in ≥60% of pages

Midpoint Estimate (word count):
  (dominant_range_min + dominant_range_max) / 2

Standard Deviation (for outlier detection):
  √(Σ(value - mean)² / N)
  Outlier threshold: mean ± 2 standard deviations

Co-occurrence Rate (for combined signals):
  pages_where_both_signals_present / pages_where_either_signal_present × 100
  Combined signal threshold: ≥80% co-occurrence rate
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — HEADING STRUCTURE PATTERN REFERENCE CARD
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
HEADING STRUCTURE PATTERN REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PATTERN A — Definition → How-To → Examples → FAQs
  Signal: Definitional H2 + Procedural H2s + "Examples" section + FAQ close
  Intent fit: Informational TOFU
  Typical length: MEDIUM to LONG (1500–3500w)

PATTERN B — Listicle → Breakdown → Comparison → Verdict
  Signal: Numbered/list H2s + Expanded sub-sections + Comparison table + Recommendation close
  Intent fit: Commercial MOFU
  Typical length: LONG to VERY LONG (2500–5000w)

PATTERN C — Problem → Solution → Deep Dive → Next Steps
  Signal: Problem-framing H2 + Solution H2 + Technical/advanced sections + Action-oriented close
  Intent fit: TOFU–MOFU Mixed
  Typical length: LONG to COMPREHENSIVE (3000–6000w)

PATTERN D — Overview → Steps → Tools → Mistakes
  Signal: Overview H2 + Numbered step H2s + "Tools" or "Resources" section + "Mistakes to Avoid" close
  Intent fit: Informational TOFU (How-To)
  Typical length: MEDIUM to LONG (1500–3500w)

NOVEL — Does not match A, B, C, or D
  Signal: Unique organizational logic not covered by defined patterns
  Action: Document actual structure; analyze for hidden pattern
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: serp/content-pattern-extractor.skill*
*Nexus SEO Operating System — Version 1.0.0*
