# SKILL: research/serp-intelligence.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / SERP Intelligence
# EXECUTION PRIORITY: POST-CLUSTERING — Runs after semantic-clustering.skill identifies target keywords.
# POSITION: Intent validation layer between keyword architecture and content blueprint generation.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **SERP intelligence and intent validation engine** of the Nexus system.

It does not generate keywords. It does not create content. It reads the competitive landscape of a target keyword's search engine results page and translates observed patterns into actionable intelligence — determining what Google currently rewards for this keyword, why those results rank, and what a new piece of content must do to compete.

This skill operates as the **mandatory validation gate** between keyword selection and content creation. No blueprint is generated, no content is written, and no strategy is finalized for a keyword without first passing it through this skill. The SERP is the ground truth of what works — this skill reads that ground truth systematically.

### Primary Responsibilities

| Responsibility                     | Description                                                                                                       |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Dominant Intent Detection**      | Identify the primary search intent Google's algorithm is satisfying for this keyword across the top results      |
| **Intent Strength Classification** | Determine whether the intent signal is strong (unanimous), mixed, or ambiguous across ranking results           |
| **Content Type Mapping**           | Catalog what types of content formats dominate the SERP for this keyword                                        |
| **SERP Feature Identification**    | Detect all active SERP features and determine what they signal about content opportunity                        |
| **Ranking Pattern Analysis**       | Evaluate domain authority spread, content length trends, freshness requirements, and brand vs. niche dynamics   |
| **Opportunity Signal Scoring**     | Produce a qualified difficulty-opportunity assessment with specific, evidence-based justification               |
| **Content Direction Intelligence** | Translate SERP observations into specific, actionable content format and structure recommendations              |
| **Pattern Weighting**              | Weight repeated signals across multiple results higher than isolated observations — base no conclusion on one page |
| **Outlier Detection and Exclusion**| Identify statistical outliers in the result set and exclude them from pattern conclusions                       |
| **SERP Shift Detection**           | Identify whether the SERP is in a freshness-driven cycle or a stable authority-dominated state                  |

### What This Skill Is NOT

- It is **not** a live web scraper. It reasons about SERP patterns using structural analysis and inference from available signals.
- It is **not** a keyword difficulty scorer. It provides a qualified opportunity signal with evidence — not a single numeric score divorced from reasoning.
- It is **not** a content generator. It provides direction — `article-blueprint.skill` and `content-generation-engine.skill` execute on that direction.
- It is **not** a competitor backlink analyzer. Link profile analysis belongs to `authority-mapping.skill`.
- It is **not** a one-result analysis tool. Every conclusion drawn by this skill must be supported by a pattern observed across multiple results.

### The Defining Standard of This Skill

> No conclusion produced by this skill may rest on a single observed result.
>
> Every insight must be:
> 1. Observed across at minimum 3 of the top-10 results to qualify as a pattern.
> 2. Distinguished from outlier behavior (results that contradict the dominant pattern).
> 3. Justified with a specific explanation tied to what was observed — not a generic SEO principle.
>
> The word "typically" or "generally" never appears in this skill's output.
> Every statement names the specific pattern it observed.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input

| Field              | Description                                                                   | Required |
|--------------------|-------------------------------------------------------------------------------|----------|
| `target_keyword`   | The exact keyword phrase to analyze on the SERP                               | YES      |

### Optional Inputs

| Field                    | Description                                                                | Used For                                               |
|--------------------------|----------------------------------------------------------------------------|--------------------------------------------------------|
| `domain`                 | The site this analysis serves                                              | Contextualizing opportunity signal relative to domain  |
| `cluster_context`        | The semantic cluster this keyword belongs to                               | Cross-cluster SERP pattern comparison                  |
| `funnel_stage`           | TOFU / MOFU / BOFU assignment from keyword-discovery.skill                | Validating or correcting funnel assignment             |
| `content_type_hypothesis`| Preliminary content type assumption from pre-execution-input.skill        | Confirming or overriding prior configuration           |
| `competitor_domains`     | Specific competitor domains to track in the result set                    | Competitor-specific pattern analysis                   |

### Input Pre-Conditions

1. **Is `target_keyword` present and non-null?** If not → halt. Request keyword before proceeding.
2. **Is `target_keyword` a navigational query?** (e.g., brand name only, login page) → Flag as low SERP intelligence value. Warn user. Ask to confirm or provide a different keyword.
3. **Has this keyword been analyzed before?** → Query `memory-controller.skill`. If a SERP analysis exists and is within freshness threshold (7 days) → offer cached result. If stale → proceed with fresh analysis.

### Input Validation

| Check                                              | Pass Condition                                          | Fail Action                                             |
|----------------------------------------------------|----------------------------------------------------------|---------------------------------------------------------|
| Keyword is not empty                               | Non-null, non-empty string                               | Halt. Request keyword.                                  |
| Keyword is not purely navigational                 | No brand-name-only pattern detected                      | Warn user. Proceed on confirmation.                     |
| Keyword length is within analyzable range          | 1–15 words                                              | Flag very long queries as likely conversational/voice   |
| Keyword is not already in stale SERP cache         | Memory check confirms analysis age ≤ 7 days              | Offer cached. Await user choice.                        |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Structured SERP Intelligence Report

```
NEXUS SERP INTELLIGENCE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Keyword          : [exact keyword]
Analysis Timestamp      : [ISO 8601]
Funnel Stage (Input)    : [TOFU / MOFU / BOFU — from input or inferred]
Funnel Stage (Validated): [TOFU / MOFU / BOFU — confirmed or corrected by SERP]
Funnel Correction Flag  : [YES — reason / NO]
Generated By            : research/serp-intelligence.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SERP SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[1] DOMINANT INTENT
  Primary Intent          : [Informational / Commercial / Transactional / Navigational]
  Intent Strength         : [STRONG / MIXED / WEAK]
  Intent Signal Source    : [description of what pattern supports this classification]
  Secondary Intent        : [if mixed — describe secondary intent]

[2] CONTENT TYPES RANKING
  Dominant Format         : [Blog Article / Listicle / Tutorial / Product Page / Landing Page / Video / Forum / Other]
  Format Distribution     :
    [Format A]            : [N of top 10 results] — [brief pattern note]
    [Format B]            : [N of top 10 results] — [brief pattern note]
    [Format C]            : [N of top 10 results] — [brief pattern note]
  Outlier Formats         : [formats present in ≤1 result — excluded from pattern conclusions]

[3] SERP FEATURES ACTIVE
  Featured Snippet        : [PRESENT / ABSENT] — [Type: Paragraph / List / Table / Video if present]
  People Also Ask         : [PRESENT / ABSENT] — [Sample questions if present]
  Video Carousel          : [PRESENT / ABSENT]
  Image Pack              : [PRESENT / ABSENT]
  Knowledge Panel         : [PRESENT / ABSENT]
  Sitelinks               : [PRESENT / ABSENT] — [Triggering domain if known]
  Shopping Results        : [PRESENT / ABSENT]
  Local Pack              : [PRESENT / ABSENT]
  Top Stories             : [PRESENT / ABSENT]
  SERP Feature Implication: [what the active feature set signals about content opportunity]

[4] RANKING PATTERNS
  Domain Authority Spread : [CONCENTRATED (big brands only) / MIXED (brands + niche) / OPEN (niche sites dominant)]
  Content Length Trend    : [SHORT: <1000w / MEDIUM: 1000–2500w / LONG: 2500–5000w / VERY LONG: 5000w+]
  Freshness Requirement   : [HIGH (recent dates dominant) / MODERATE / LOW (old content ranks fine)]
  Brand vs Niche Dominance: [BRAND DOMINATED / NICHE DOMINANT / BALANCED]
  Top Result Pattern      : [specific description of what the #1 and #2 results share]
  Ranking Consistency     : [CONSISTENT (similar results) / VOLATILE (mixed formats and authorities)]

[5] INTENT STRENGTH CLASSIFICATION
  Classification          : [Strongly Informational / Mixed Intent / Commercial Heavy / Transactional Heavy]
  Evidence                : [specific pattern observed across N of top-10 results]
  Confidence Level        : [HIGH / MEDIUM / LOW]

[6] OPPORTUNITY SIGNAL
  Difficulty Rating       : [EASY / MEDIUM / HARD]
  Evidence-Based Reason   : [specific, referenced explanation — not generic]
  Competitive Gap         : [what is missing from existing top results]
  Entry Angle             : [specific angle or approach that gives a new entrant the best opportunity]

[7] CONTENT DIRECTION INTELLIGENCE
  Recommended Format      : [specific format + structure recommendation]
  Recommended Length      : [word count range or AUTO-match with SERP median]
  Recommended Structure   : [key sections or elements that must be present]
  Formats to Avoid        : [what does NOT work on this SERP and why]
  Tone Signal             : [what tone do ranking results use]
  Freshness Strategy      : [evergreen / regular update / date-specific]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PATTERN EVIDENCE LOG

| Pattern Observed               | Result Count | Confidence | Classified As         |
|-------------------------------|--------------|------------|-----------------------|
| [observation]                 | [N/10]       | [H/M/L]   | [Pattern / Outlier]   |
| [observation]                 | [N/10]       | [H/M/L]   | [Pattern / Outlier]   |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FUNNEL VALIDATION RESULT

Input Funnel Stage      : [value from keyword-discovery or pre-execution-input]
SERP-Validated Stage    : [confirmed or corrected value]
Correction Applied      : [YES / NO]
Correction Reason       : [if YES — why the SERP contradicts the prior assignment]
Downstream Impact       : [what this correction changes in the content configuration]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into              : deep-serp-analysis.skill [if HARD difficulty or mixed intent]
                          opportunity-engine.skill [opportunity signal forwarded]
                          blueprint-generator.skill [content direction forwarded]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — INTENT DETECTION

Analyze the SERP to determine the dominant search intent Google is satisfying for the target keyword. This is the most consequential classification in the entire analysis — it determines what type of content must be created, and creating the wrong type for the wrong intent is the primary reason well-written content fails to rank.

**Phase 1A — Intent Signal Extraction**

For each of the top-10 results (or as many as are accessible), extract and record the following intent signals:

| Signal                                     | What It Indicates                                                      |
|--------------------------------------------|------------------------------------------------------------------------|
| **Page type** (blog, product, landing, forum) | The functional purpose of the ranking page                         |
| **Title tag structure**                    | Question-form = Informational; Brand = Navigational; Buy/Get = Transactional; Best/Top = Commercial |
| **URL structure**                          | `/blog/` = Informational; `/product/` or `/shop/` = Transactional; `/compare/` = Commercial |
| **Call-to-action presence**                | Prominent CTA = Transactional or Commercial; No CTA = Informational   |
| **Content structure visible in snippet**   | Definition/explanation = Informational; Comparison table = Commercial; Price/buy button = Transactional |
| **Ad presence above organic results**      | Heavy ads = Transactional or Commercial intent (advertisers pay for commercial intent keywords) |
| **Shopping results presence**              | Confirms Transactional intent for product-type keywords                |
| **SERP features active**                   | Featured snippets and PAA = Informational; Shopping = Transactional; Review snippets = Commercial |

**Phase 1B — Intent Classification**

After extracting signals from all accessible results, tally the dominant intent across the result set:

| Intent Type        | Classification Threshold                                                           |
|--------------------|------------------------------------------------------------------------------------|
| **Informational**  | ≥6 of top-10 results are educational/explanatory content with no transactional CTA |
| **Commercial**     | ≥6 of top-10 results are comparison, review, or listicle pages                    |
| **Transactional**  | ≥6 of top-10 results are product pages, landing pages, or direct purchase paths   |
| **Navigational**   | ≥5 of top-10 results point to the same brand or domain                            |
| **Mixed**          | No single intent type accounts for ≥6 of top-10 results                           |

**Phase 1C — Intent Strength Classification**

| Strength Label | Condition                                                                                    |
|----------------|----------------------------------------------------------------------------------------------|
| **STRONG**     | ≥8 of top-10 results share the same intent classification                                    |
| **MIXED**      | The top-10 results split between 2 intent types, neither reaching the 6-result threshold     |
| **WEAK**       | Results span 3+ intent types with no clear dominant pattern; SERP appears to be in flux     |

**Phase 1D — Funnel Validation**

Compare the detected dominant intent against the funnel stage assigned by `keyword-discovery.skill` or `pre-execution-input.skill`:

| SERP-Detected Intent | Expected Funnel Stage | If Mismatch Detected                                                          |
|----------------------|-----------------------|-------------------------------------------------------------------------------|
| Informational STRONG | TOFU                  | If input said MOFU or BOFU → flag correction → update to TOFU                |
| Commercial STRONG    | MOFU                  | If input said TOFU → flag correction → update to MOFU                        |
| Transactional STRONG | BOFU                  | If input said TOFU or MOFU → flag correction → update to BOFU                |
| Mixed                | No correction         | Mark as Mixed Intent. Downstream skills must account for dual-format content  |

Any funnel correction is logged in the Funnel Validation Result section of the output and propagated to all downstream skills via the dispatch packet.

**Phase 1E — Intent Detection Anti-Patterns (These Are Errors):**

| Error                                                                           | Why It Fails                                                          |
|---------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| Classifying intent based on the keyword phrasing alone (not the SERP)          | Keywords can lie. A keyword that "sounds" informational may have a commercial SERP. |
| Classifying intent from a single result                                         | One result is not a pattern. Minimum 3 results for any pattern claim. |
| Ignoring the SERP features in intent classification                             | Featured snippets and shopping results are direct intent signals.     |
| Calling a SERP "Mixed" because 2 results differ from the other 8               | 2/10 is an outlier level — not a mixed signal. Mixed requires ≥3 results in each of 2+ intent types. |

---

### ▸ STEP 2 — CONTENT TYPE ANALYSIS

Identify the formats of content that appear in the top-10 results and extract the distribution pattern that governs what format a new piece of content should take.

**Phase 2A — Content Type Taxonomy**

Every ranking result is classified into exactly one primary content type:

| Content Type        | Definition                                                                               | Identifying Signals                                                |
|---------------------|------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| **Blog Article**    | Long-form informational prose; single topic; editorial structure                         | /blog/ URL path; article byline; publication date visible         |
| **Listicle**        | Numbered or bulleted list as the primary organizational structure                        | Title contains "X best", "top N", "X ways"; content is list-form  |
| **Tutorial**        | Step-by-step instructional content with a defined end goal                              | Title contains "how to", "step by step"; numbered steps structure  |
| **Product Page**    | Commercial page for a specific product or service with purchase CTA                     | Add to cart / Buy now / Pricing table; product images              |
| **Landing Page**    | Conversion-focused page with a single CTA and minimal navigation                        | Full-page layout; hero section + CTA; no blog structure           |
| **Video**           | Video content ranking (YouTube or embedded)                                              | Video thumbnail in result; YouTube URL                             |
| **Forum / Q&A**     | Community-generated content; question-and-answer format                                 | Reddit, Quora, Stack Exchange, community forums                    |
| **Comparison Page** | Side-by-side evaluation of multiple products, tools, or approaches                     | "vs" or "compared" in title; comparison table visible in snippet  |
| **Tool / Resource** | Interactive tool, template, calculator, or downloadable resource                        | Tool interface visible; "free tool", "template", "calculator"      |
| **News / Press**    | Time-sensitive news article or press release                                            | News publisher domain; date-heavy title; recent timestamp         |

**Phase 2B — Distribution Counting**

For each content type identified across the top-10 results:

1. Count how many of the top-10 results belong to that type.
2. Record the count and position of each instance (position matters — result #1 has more weight than result #9).

**Position Weighting:**

| Position in Results | Weight Multiplier |
|---------------------|-------------------|
| #1                  | 2.0×              |
| #2–3                | 1.5×              |
| #4–6                | 1.0×              |
| #7–10               | 0.5×              |

Apply position weighting to content type counts to produce a **weighted content type distribution**. A listicle at position #1 counts more toward the dominant format conclusion than a listicle at position #9.

**Phase 2C — Pattern Threshold and Outlier Detection**

| Classification | Condition                                                                      |
|----------------|--------------------------------------------------------------------------------|
| **Dominant**   | Weighted count ≥ 40% of total weighted score across all types                  |
| **Secondary**  | Weighted count 15–39% of total weighted score                                  |
| **Tertiary**   | Weighted count 5–14% of total weighted score                                   |
| **Outlier**    | Weighted count < 5% of total weighted score OR raw count = 1 result only       |

Outlier content types are:
- Excluded from content format recommendations.
- Recorded in the output's Outlier Formats field.
- Do NOT influence the dominant format conclusion.

**Phase 2D — Content Type Signal Interpretation**

The dominant content type is not merely descriptive — it carries specific strategic implications:

| Dominant Type     | Strategic Implication                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------|
| **Listicle**      | Google satisfies this query with curated options. New content must be a structured list with ≥ parity in item count. |
| **Tutorial**      | Google satisfies this query with step-by-step instruction. New content must have numbered steps, clear outcomes, and actionable specificity. |
| **Blog Article**  | Google satisfies this query with comprehensive editorial coverage. New content must match or exceed coverage depth. |
| **Product Page**  | Google satisfies this query with commercial intent. Creating a blog article for this keyword is likely futile. |
| **Forum / Q&A**   | Google cannot find good editorial content for this query — UGC fills the gap. Opportunity: build authoritative editorial content that outperforms forum pages. |
| **Comparison**    | Google satisfies this query with side-by-side evaluation. New content must present a structured comparison with clear winner declarations. |
| **Tool / Resource**| Google satisfies this query with utility, not prose. Content strategy must include an interactive or downloadable element to compete. |
| **Video**         | Google satisfies this query visually. Blog content faces an uphill battle. Recommend video-first or video-embedded strategy. |

---

### ▸ STEP 3 — SERP FEATURE DETECTION

Detect all active SERP features for the target keyword and interpret what each feature signals about content opportunity.

**Phase 3A — Feature Detection Checklist**

For each SERP feature, determine: Present or Absent. If present, extract type and trigger details.

**Featured Snippet:**

| Sub-Type         | How to Identify                                                          | Content Implication                                                      |
|------------------|--------------------------------------------------------------------------|--------------------------------------------------------------------------|
| **Paragraph**    | A block of text answering the query directly at the top of the SERP      | Opportunity: write a direct, concise answer to the query in 40–60 words in the content's opening |
| **List**         | A numbered or bulleted list at the top of the SERP                       | Opportunity: structure content with a clearly enumerable answer at the top |
| **Table**        | A data table at the top of the SERP                                      | Opportunity: include a summary comparison table near the top of the content |
| **Video**        | A video snippet at the top of the SERP                                   | Opportunity is video-format specific; prose content unlikely to win snippet |

**People Also Ask (PAA):**

- Record up to 5 PAA questions visible for the keyword.
- PAA questions are direct signals of semantic expansion opportunities:
  - Each PAA question represents a sub-topic the SERP expects comprehensive content to address.
  - PAA questions not already covered by the target keyword's article blueprint are flagged as required subsections.

**Video Carousel:**

- Presence indicates visual intent component.
- If video carousel appears alongside blog results → hybrid content (text + embedded video or video summary) is advantaged.
- If video carousel dominates → content strategy must include video.

**Image Pack:**

- Presence indicates visual search component.
- Content should include optimized images with descriptive alt text.
- For product/commercial keywords → image pack signals high visual decision-making by searchers.

**Knowledge Panel:**

- Presence indicates query has a definitional or entity answer.
- For brand keywords → competitor knowledge panel present signals strong navigational intent.
- For concept keywords → knowledge panel suggests the query is being answered at the entity level; content must go beyond definition to provide depth.

**Sitelinks:**

- Presence for a non-target brand indicates strong brand association with the keyword.
- If the sitelinks belong to a competitor → brand dominance is high; opportunity for niche content is limited.
- If sitelinks belong to target domain → do not target this keyword (already dominated).

**Shopping Results:**

- Unambiguous Transactional intent signal.
- If shopping results appear → informational content is unlikely to rank on page 1.
- Recommend: target long-tail informational variants of this keyword instead.

**Local Pack:**

- Presence indicates geographic intent.
- Non-local content is disadvantaged.
- Recommend: localize content or target non-local variants.

**Top Stories (News Carousel):**

- Presence indicates freshness dependency.
- If top stories appear → SERP rewards recent content.
- Freshness strategy required: content must be structured for regular updating or time-stamped relevance.

**Phase 3B — SERP Feature Opportunity Matrix**

After detecting all active features, synthesize their combined signal:

| Feature Combination                                    | Combined Signal                                                                   |
|--------------------------------------------------------|-----------------------------------------------------------------------------------|
| Featured Snippet (Paragraph) + PAA                     | Strong informational intent with AI/voice optimization opportunity                |
| Featured Snippet (List) + Video Carousel               | Structured + visual; hybrid list-with-video content strongly advantaged          |
| Shopping Results + Knowledge Panel                     | Commercial + entity intent; product comparison or brand review content fits       |
| Top Stories + No Featured Snippet                      | Freshness-dominated SERP; evergreen content must have update mechanism            |
| Knowledge Panel + Sitelinks (competitor)               | Brand-dominated SERP; repositioning to long-tail or niche variant recommended    |
| No SERP features                                       | Open SERP with less optimization pressure; quality and depth are the primary differentiators |
| PAA only (no featured snippet)                         | Conversational query with unanswered sub-questions; FAQ-rich comprehensive content wins |

---

### ▸ STEP 4 — RANKING PATTERN ANALYSIS

Evaluate the structural characteristics of the ranking result set to understand what attributes the current top performers share and what gaps exist.

**Phase 4A — Domain Authority Spread**

Assess the breadth and type of domains ranking in the top 10:

| Authority Pattern      | Definition                                                                 | Opportunity Implication                                                |
|------------------------|----------------------------------------------------------------------------|------------------------------------------------------------------------|
| **CONCENTRATED**       | ≥7 results from high-authority brands (Hubspot, Forbes, Wikipedia, etc.)  | Hard to break in. Niche sites unlikely to rank without exceptional content depth or link authority. |
| **MIXED**              | 4–6 results from authority brands, 4–6 from niche/independent sites       | Moderate difficulty. High-quality, well-structured content from a niche site can compete. |
| **OPEN**               | ≤3 results from authority brands; majority are niche or independent sites  | Strong opportunity. Content quality and specificity are the primary differentiators. |

**Phase 4B — Content Length Trend**

Estimate the dominant content length across top-ranking results using snippet length, heading count, and structural signals visible in the SERP:

| Length Classification | Estimated Word Count  | Identifying Signals                                                    |
|-----------------------|-----------------------|------------------------------------------------------------------------|
| **SHORT**             | < 1,000 words         | Snippet is a direct concise answer; no visible subheadings             |
| **MEDIUM**            | 1,000–2,500 words     | 3–6 visible subheadings; moderate snippet depth                        |
| **LONG**              | 2,500–5,000 words     | 7+ visible subheadings; deep snippet; table of contents likely         |
| **VERY LONG**         | 5,000+ words          | Comprehensive guide indicators; multiple sections visible in PAA       |

**Length Pattern Rules:**

1. Calculate the modal length category (most common across top-10 results).
2. Record the length category of results #1 and #2 specifically (highest weight).
3. If result #1 is SHORT but results #2–5 are LONG → flag: "Leader may be vulnerable to longer, more comprehensive content."
4. If all top-5 results are VERY LONG → flag: "Content length parity is required — short-form content cannot compete here."
5. Never recommend a specific word count based on length trend alone. Length trend informs `word_count = AUTO` calibration in `article-blueprint.skill`.

**Phase 4C — Freshness Requirement**

Evaluate whether the SERP rewards recent content or tolerates older evergreen content:

| Freshness Level   | Signals                                                                                    |
|-------------------|--------------------------------------------------------------------------------------------|
| **HIGH**          | ≥5 of top-10 results published or updated within the last 6 months; Top Stories present  |
| **MODERATE**      | 3–4 of top-10 results recently updated; mix of new and old content ranking                |
| **LOW**           | ≥5 of top-10 results published 2+ years ago; no Top Stories; no date-sensitive signals   |

Freshness Implication:
- HIGH: Content must have a structured update mechanism (date-stamped sections, update logs, versioned content).
- MODERATE: Content should include a clear publication date and commit to annual review.
- LOW: Evergreen content without a date is acceptable. Focus on depth over recency.

**Phase 4D — Brand vs. Niche Dominance**

Classify the competitive dynamic of the result set:

| Classification         | Condition                                                                              |
|------------------------|----------------------------------------------------------------------------------------|
| **BRAND DOMINATED**    | ≥6 results are from well-known brands with homepage domain authority > 70 (estimated)  |
| **NICHE DOMINANT**     | ≥6 results are from small to mid-size niche sites without broad brand recognition      |
| **BALANCED**           | Results split roughly 50/50 between brand and niche domains                            |

Strategic Implication:
- BRAND DOMINATED → Competing on the same broad keyword is high-cost. Recommend targeting niche modifiers or long-tail variants.
- NICHE DOMINANT → The SERP rewards topic expertise over domain authority. Content depth and specificity are the winning strategy.
- BALANCED → Both approaches can work. Content quality and targeting precision determine outcome.

**Phase 4E — Top Result Pattern Analysis**

Examine results #1 and #2 specifically. Identify what they share:

Required observations:
1. What content format are they? (both same format? → format signal confirmed)
2. What is their structural approach? (how do they open, what headings do they use, how do they close?)
3. What signals suggest why Google ranked them #1 and #2 specifically?
4. What do they do that results #6–10 do not?

This is the most critical observation in Phase 4. The gap between result #1 and result #6 reveals what Google is rewarding beyond basic content quality — and that gap is the target for new content.

---

### ▸ STEP 5 — INTENT STRENGTH CLASSIFICATION

Using the data from Steps 1–4, produce the final, synthesized intent strength classification.

**Classification Labels and Definitions:**

| Classification               | Definition                                                                                            |
|------------------------------|-------------------------------------------------------------------------------------------------------|
| **Strongly Informational**   | SERP is dominated (≥8/10) by educational, explanatory content. No shopping, no product pages. The searcher wants to learn. |
| **Mixed Intent**             | SERP contains a split between informational and commercial content (neither dominates). Searcher is simultaneously learning and evaluating. |
| **Commercial Heavy**         | SERP is dominated (≥6/10) by comparison, review, and evaluation content. The searcher is in active vendor evaluation. |
| **Transactional Heavy**      | SERP is dominated (≥6/10) by product pages, shopping results, and landing pages. The searcher is ready to act. |

**Classification Evidence Requirement:**

Every classification must be accompanied by its supporting evidence:

```
Intent Classification   : [classification]
Evidence Pattern        :
  - Result #1: [content type] — [intent signal observed]
  - Result #2: [content type] — [intent signal observed]
  - Result #3: [content type] — [intent signal observed]
  - [Pattern observed in N of 10 results: description]
Confidence Level        : [HIGH / MEDIUM / LOW]
Confidence Justification: [why confidence is at this level]
```

**Confidence Level Rules:**

| Confidence | Condition                                                                                |
|------------|------------------------------------------------------------------------------------------|
| HIGH       | Pattern observed in ≥8 of 10 results with consistent signals                            |
| MEDIUM     | Pattern observed in 5–7 of 10 results; some mixed signals present                       |
| LOW        | Pattern observed in 3–4 of 10 results; significant outlier presence; SERP appears unstable |

If confidence is LOW → note that the SERP may be in flux and recommendations should be treated as provisional.

---

### ▸ STEP 6 — OPPORTUNITY SIGNAL

Produce a qualified assessment of the competitive opportunity available for this keyword, grounded entirely in observed SERP patterns.

**Phase 6A — Difficulty Rating**

| Rating   | Definition                                                                                                       |
|----------|------------------------------------------------------------------------------------------------------------------|
| **EASY** | Current top-ranking content has identifiable weaknesses; niche sites are ranking; content is outdated or shallow |
| **MEDIUM** | Decent competition; a mix of strong and weak content; some niche sites ranking; some gaps exist              |
| **HARD** | Strong authority content throughout; no weak results; freshness and depth requirements both high; brand dominated |

**Difficulty Rating Rules:**

- The rating must be justified with SPECIFIC observations — not generic statements.
- Minimum 2 evidence points are required to support any rating.
- The evidence points must reference actual patterns observed in the result set.

**Evidence-Based Justification Format:**

```
Difficulty Rating       : [EASY / MEDIUM / HARD]

Evidence Point 1        : [specific observation from the result set]
Evidence Point 2        : [specific observation from the result set]
Evidence Point 3        : [specific observation if available]

Generic statements that are NOT acceptable as evidence:
  ✗ "The competition is high."
  ✗ "This is a competitive keyword."
  ✗ "Top results have good content."

Specific statements that ARE acceptable as evidence:
  ✓ "Results #1 and #3 were published in 2020 and have not been updated — freshness gap is exploitable."
  ✓ "5 of 10 results are from niche sites with estimated domain authority below 40, indicating no brand lock-in."
  ✓ "The featured snippet is a short paragraph that does not answer the full question — depth gap exists."
  ✓ "No result in the top 10 addresses the [specific sub-topic] despite PAA showing users ask about it."
```

**Phase 6B — Competitive Gap Identification**

Beyond the difficulty rating, identify the specific gap that a new piece of content can exploit. This is the most actionable part of the opportunity signal.

**Gap Detection Checklist:**

| Gap Type                    | Detection Method                                                                 |
|-----------------------------|----------------------------------------------------------------------------------|
| **Freshness gap**           | Top results published > 18 months ago with no update signal                     |
| **Depth gap**               | Top results are short (MEDIUM or below) for a query that demands comprehensive coverage |
| **Format gap**              | The dominant format is suboptimal for the intent (e.g., listicle for a complex how-to query) |
| **Sub-topic gap**           | PAA questions exist that no top-10 result adequately answers                    |
| **Audience gap**            | Top results target a different audience segment than the keyword implies         |
| **Semantic gap**            | Top results do not cover the full semantic field the keyword implies             |
| **Specificity gap**         | Top results are generic; a niche-specific treatment does not exist               |
| **Structure gap**           | Top results lack organized structure (no table of contents, no clear headings)   |

For each gap detected, record:
1. Gap type
2. Specific evidence from the SERP
3. How a new piece of content can exploit it

**Phase 6C — Entry Angle**

Define the specific content angle that gives a new entrant the highest probability of ranking:

```
Entry Angle             : [specific content angle]
Rationale               : [why this angle exploits the identified gap]
Differentiation         : [what this angle does that the top results do not]
```

Example (not a template — these must be specific to the actual SERP):
- `"Publish a 2024-updated comparison that covers [missing tool] and targets [missing audience segment] not addressed by any current top-10 result, structured with a scoring table that directly answers the featured snippet format."`

---

### ▸ STEP 7 — CONTENT DIRECTION INTELLIGENCE

Translate all SERP observations into specific, actionable content creation directives. This section feeds directly into `article-blueprint.skill` and overrides any conflicting assumptions from `pre-execution-input.skill`.

**Phase 7A — Recommended Format**

Based on dominant content type (Step 2) and intent strength (Step 5):

| SERP Condition                                              | Format Recommendation                                       |
|-------------------------------------------------------------|-------------------------------------------------------------|
| Strongly Informational + Listicle dominant                  | Structured listicle with ≥ parity item count + depth per item |
| Strongly Informational + Tutorial dominant                  | Step-by-step guide with prerequisites, numbered steps, and outcome statement |
| Strongly Informational + Blog Article dominant              | Comprehensive editorial article with full semantic coverage  |
| Mixed Intent + Blog + Comparison split                      | Hybrid: informational primer + embedded comparison section  |
| Commercial Heavy + Comparison dominant                      | Full comparison page with scoring criteria and winner declarations |
| Transactional Heavy + Product Page dominant                 | Landing page format with specific use-case differentiation  |
| Forum/Q&A dominant                                          | Authoritative editorial that directly replaces forum-quality answers |
| Video dominant                                              | Video-embedded article or video-first content strategy      |

**Phase 7B — Recommended Length**

Based on content length trend (Step 4B) and featured snippet analysis (Step 3):

| Length Trend    | Featured Snippet | Recommended Length Approach                                                         |
|-----------------|------------------|-------------------------------------------------------------------------------------|
| SHORT           | Present (Para)   | Match short-form length; optimize opening 60 words for snippet capture              |
| MEDIUM          | Absent           | Target 1,500–2,500 words; quality over length                                       |
| LONG            | Present (List)   | Match long-form; include structured list for snippet; no padding                    |
| VERY LONG       | Absent           | Comprehensive guide format; 4,000+ words; table of contents required               |
| Mixed signals   | Any              | Use AUTO — pass to blueprint-generator for SERP-median calibration                 |

**Phase 7C — Required Structural Elements**

Based on PAA questions, featured snippet type, and top result structure:

List every structural element that the content MUST include to satisfy the SERP's expectations:

```
Required Structural Elements:
  → [Element 1] — [why: tied to specific SERP observation]
  → [Element 2] — [why: tied to specific SERP observation]
  → [Element 3] — [why: tied to specific SERP observation]
  ...
```

Examples of structural elements (must be specific to the SERP, not generic):
- "Direct definition in the first paragraph (paragraph featured snippet is present)"
- "Numbered steps (tutorial format dominates top 3 results)"
- "Comparison table with ≥5 evaluated products (shopping results and comparison pages both present)"
- "FAQ section addressing: [specific PAA question 1], [specific PAA question 2]"
- "Update log or 'last updated' timestamp (freshness requirement is HIGH)"

**Phase 7D — Formats to Avoid**

List every content format that is demonstrably NOT working on this SERP, with evidence:

```
Formats to Avoid:
  → [Format]: [evidence — X of 10 results in this format rank below position Y, or this format is absent entirely]
  → [Format]: [evidence]
```

**Phase 7E — Tone Signal**

Based on content type, audience signals in the ranking results, and intent strength:

| SERP Tone Signal         | Content Tone Recommendation                                          |
|--------------------------|----------------------------------------------------------------------|
| Academic / editorial     | Professional, authoritative, no colloquialisms                      |
| Conversational / forum   | Approachable, direct, practical — avoid corporate stiffness         |
| Technical / developer    | Precise, jargon-appropriate, no over-explanation of basics          |
| Marketing / persuasive   | Benefit-focused, outcome-oriented, clear and confident              |

**Phase 7F — Freshness Strategy**

Based on freshness requirement (Step 4C):

| Freshness Level | Content Strategy                                                              |
|-----------------|-------------------------------------------------------------------------------|
| HIGH            | Include explicit "Last Updated: [Month Year]" marker. Structure content with modular sections that can be updated independently. Plan quarterly review. |
| MODERATE        | Include publication date. Plan annual comprehensive review. Avoid embedding date-specific statistics without citing sources. |
| LOW             | Evergreen format preferred. Avoid date references that will cause the content to appear stale. No update mechanism required. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ PATTERN RECOGNITION AND WEIGHTING

Not all patterns are equal. This skill applies a weighting system to distinguish strong patterns from weak signals and eliminate outlier noise.

**Pattern Strength Classification:**

| Pattern Strength | Condition                                                             | Action                                        |
|------------------|-----------------------------------------------------------------------|-----------------------------------------------|
| **STRONG**       | Observed in ≥7 of top-10 results                                      | Include as primary pattern. Base recommendations on it. |
| **MODERATE**     | Observed in 4–6 of top-10 results                                     | Include as secondary pattern. Note as supporting signal. |
| **WEAK**         | Observed in 2–3 of top-10 results                                     | Note as emerging signal. Do not base primary recommendations on it. |
| **OUTLIER**      | Observed in 1 of top-10 results                                       | Exclude from pattern conclusions. Log separately. |

**Weighting Application Protocol:**

1. For every observation across the result set, apply the position weight multiplier from Step 2B.
2. Aggregate weighted counts per pattern type.
3. Classify patterns into STRONG / MODERATE / WEAK / OUTLIER based on aggregate weighted scores.
4. Base every recommendation in Steps 6 and 7 on STRONG or MODERATE patterns only.
5. Weak patterns are mentioned as supplementary signals but do not drive format, length, or structure decisions.

---

### ▸ SERP SHIFT DETECTION

Identify whether the SERP is in a stable state (authority-driven) or a transitional state (freshness-driven or in algorithmic flux).

**Shift Detection Signals:**

| Signal                                                          | Shift Type                  |
|-----------------------------------------------------------------|-----------------------------|
| Recent publication dates across top-5 results (< 6 months)     | Freshness-driven SERP       |
| Mix of very old and very new content in top 10                  | Algorithmic flux — unstable |
| Top Stories feature active                                      | News-cycle driven           |
| Content of varying quality (some shallow, some deep) ranking    | SERP not yet settled        |
| Consistent high-authority results with stable dates             | Authority-stable SERP       |
| Featured snippet has changed type recently (signals instability)| Snippet instability         |

**Shift Type Implications:**

| Shift Type            | Content Strategy Adjustment                                                      |
|-----------------------|----------------------------------------------------------------------------------|
| Freshness-driven      | Publishing quickly with a solid but not exhaustive piece can capture position while longer content is developed. Speed + update mechanism. |
| Flux / Unstable       | Opportunity is high but unpredictable. Publish a comprehensive piece that covers all observed format types. |
| News-cycle driven     | Evergreen content will be displaced during news cycles. Target adjacent non-news keywords instead. |
| Authority-stable      | Long-term investment required. Quality and depth must exceed current top results. |

---

### ▸ COMPARATIVE PATTERN ANALYSIS (MULTI-RESULT SYNTHESIS)

Ensure no conclusion derives from fewer than 3 results. This is the operational enforcement of the skill's foundational standard.

**Multi-Result Synthesis Protocol:**

1. For every conclusion proposed in Steps 1–7, count the number of results that support it.
2. If support count is < 3 → reclassify as OUTLIER. Do not include in final recommendations.
3. If support count is 3–4 → classify as WEAK signal. Mention as supporting note. Do not use as primary basis for any recommendation.
4. Run a final check before producing output: every recommendation must cite at least one MODERATE or STRONG pattern.
5. If any recommendation cannot cite a MODERATE or STRONG pattern → remove the recommendation. Replace with: "Insufficient pattern evidence — recommend monitoring SERP for [X weeks] before executing."

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Insights

Every insight in the output must be distinct. If the same observation is captured in multiple sections (e.g., "freshness is important" appearing in both the Ranking Patterns and the Content Direction sections), consolidate it. Each insight appears once in the most relevant section and is referenced — not repeated — elsewhere.

### Rule 2 — No Generic Statements

No statement in this skill's output may be applicable to any keyword generically. Every statement must be tied to a specific observation from the target keyword's SERP. If a statement could be published in a generic "SEO guide" without modification → it is generic. Remove it and replace with a specific observation.

**Generic Statement Test:**

> Read the statement. Remove all mentions of the specific keyword. If the statement still makes sense as generic SEO advice → it is generic. Rewrite it with specific SERP evidence.

### Rule 3 — No Cross-Section Contradiction

The output of this skill must be internally consistent. If Step 1 classifies the intent as Strongly Informational, Step 7 cannot recommend a landing page format. All sections must reflect a coherent, non-contradictory analysis.

Cross-section consistency check before output finalization:
- Intent classification (Step 1) must align with recommended format (Step 7).
- Difficulty rating (Step 6) must be supported by ranking patterns (Step 4).
- Opportunity signal (Step 6) must be consistent with domain authority spread (Step 4A).

### Rule 4 — No Duplicate SERP Reports in Memory

Before producing any output, check `memory-controller.skill` for an existing SERP analysis for the same target keyword:
- If found and within freshness threshold (7 days) → offer cached result.
- If found but stale → produce fresh analysis. Update memory record.
- If not found → produce fresh analysis. Write to memory.

Never store two SERP analysis records for the same keyword. Stale records are overwritten — not duplicated.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Output Behaviors

| Prohibited Behavior                                                                         | Reason                                                                     |
|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| Stating `"This is a competitive keyword"` without evidence                                   | Not an insight. Restate with specific authority spread observation.        |
| Stating `"Content should be high quality"` as a recommendation                              | Universal truism. Not a SERP-specific insight. Remove.                     |
| Stating `"Optimize for SEO"` in any form                                                    | Self-referential non-insight. Every statement must be more specific.       |
| Classifying intent based on the keyword phrasing rather than the observed SERP              | The keyword is the hypothesis. The SERP is the evidence. Use the evidence. |
| Recommending a content length without citing the length trend observed in the results        | Length must be SERP-grounded, not assumed.                                 |
| Drawing any conclusion from a single result                                                 | Minimum 3-result pattern threshold is non-negotiable.                      |
| Leaving the Opportunity Signal section without a specific competitive gap                   | "Hard competition" is not a gap analysis. Identify the specific gap.       |
| Ignoring outlier results without documenting them                                           | Outliers must be logged — not silently ignored.                            |
| Producing a Funnel Validation Result without checking against input funnel assignment       | Funnel validation requires comparing input vs. SERP observation.           |
| Marking every SERP as MEDIUM difficulty to avoid commitment                                 | Difficulty ratings must be honest and evidence-based.                      |

### Required Output Behaviors

- Every section of the output must be populated — no section may be left blank.
- Every pattern cited must specify how many of the 10 results it was observed in.
- Every recommendation in Section 7 must reference at least one specific pattern from Sections 1–6.
- The Pattern Evidence Log must contain an entry for every distinct observation made during analysis.
- The Funnel Validation Result must always be included, even if no correction is made.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ memory-controller.skill

| Property         | Detail                                                                                      |
|------------------|---------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before analysis, WRITE after output finalization                       |
| **Reads**        | Prior SERP analyses for this keyword (freshness check), project context, competitor domains |
| **Writes**       | Complete SERP intelligence report, funnel correction flag, opportunity signal               |
| **Timing**       | READ at input pre-condition check. WRITE after Step 7 completes.                           |
| **Memory layer** | strategy-memory.skill                                                                        |

---

### ▸ deep-serp-analysis.skill

| Property         | Detail                                                                                      |
|------------------|---------------------------------------------------------------------------------------------|
| **Direction**    | serp-intelligence → deep-serp-analysis (conditional dispatch)                              |
| **Triggers**     | Dispatched when: Difficulty = HARD, Intent Strength = MIXED, or Confidence = LOW          |
| **Sends**        | Full SERP intelligence report as the input context for deeper pattern analysis             |
| **Used for**     | Granular ranking signal analysis, content gap depth mapping, competitive content audit     |

---

### ▸ opportunity-engine.skill (gap-opportunity-engine.skill)

| Property         | Detail                                                                                      |
|------------------|---------------------------------------------------------------------------------------------|
| **Direction**    | serp-intelligence → opportunity-engine (output consumer)                                   |
| **Sends**        | Opportunity signal (rating + evidence + competitive gap + entry angle)                     |
| **Used for**     | Aggregating opportunity signals across multiple keywords to score and prioritize            |

---

### ▸ article-blueprint.skill (blueprint-generator.skill)

| Property         | Detail                                                                                      |
|------------------|---------------------------------------------------------------------------------------------|
| **Direction**    | serp-intelligence → article-blueprint (primary content direction source)                   |
| **Sends**        | Content direction intelligence (format, length, structure, tone, freshness strategy)       |
| **Used for**     | Generating a content blueprint that is grounded in SERP reality rather than assumptions    |
| **Override rule**| Content direction from this skill overrides any conflicting settings from pre-execution-input.skill |

---

### ▸ keyword-discovery.skill

| Property         | Detail                                                                                      |
|------------------|---------------------------------------------------------------------------------------------|
| **Direction**    | Upstream source (provides target keyword and initial funnel assignment)                    |
| **Receives from**| Target keyword, intent classification, funnel stage assignment                             |
| **Returns**      | Funnel correction flag (if SERP contradicts keyword-discovery's funnel assignment)         |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                      |
|------------------|---------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source                                                                     |
| **Receives from**| Cluster context (which cluster the target keyword belongs to)                              |
| **Returns**      | SERP pattern data that may inform cluster importance re-scoring                            |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                    | Detection                                              | Recovery Action                                                                                       |
|---------------------------------------------------------------------|--------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| `target_keyword` is missing                                         | Input validation check fails                           | Halt. Request keyword before any analysis begins.                                                     |
| Target keyword is navigational (brand-name only)                    | Navigational intent detected with no informational split | Warn: "This keyword targets a specific brand's SERP — ranking opportunity for non-brand sites is minimal." Ask user to confirm or provide alternative. |
| SERP intent is genuinely unclear after full analysis                | Intent strength = WEAK; confidence = LOW               | Mark as MIXED INTENT. Recommend hybrid content strategy (informational primer + comparison section). Log provisional status. |
| Fewer than 3 results are accessible for pattern analysis            | Pattern count < 3                                      | Do not produce difficulty rating or content direction. Mark analysis as INCOMPLETE. Request broader SERP access or recommend manual verification. |
| All top-10 results are from the same domain (monopolized SERP)      | Domain spread = 1 unique domain across ≥7 results     | Flag as NAVIGATIONAL / MONOPOLIZED. Recommend: long-tail variant targeting or adjacent keyword. Do not recommend direct competition. |
| SERP shows extreme freshness volatility (dates all within 2 weeks) | Freshness requirement = HIGH with news-cycle signals   | Mark keyword as NEWS-CYCLE SENSITIVE. Recommend monitoring for 4 weeks before committing to evergreen content strategy. |
| Featured snippet is present but content type is contradictory       | Featured snippet type conflicts with dominant format   | Prioritize featured snippet type for format recommendation. Note the contradiction in the Pattern Evidence Log. |
| Memory controller is unavailable for prior SERP check               | READ query returns error                               | Proceed with fresh analysis. Flag: "Prior SERP cache unavailable — freshness verification skipped." Note risk of duplicate analysis in the output. |
| Confidence level is LOW after all steps complete                    | < 3 MODERATE or STRONG patterns identified             | Flag all recommendations as PROVISIONAL. Add: "SERP appears unstable. Re-run analysis in [X weeks] before finalizing content strategy." |
| Funnel correction contradicts pre-execution-input configuration     | Corrected funnel ≠ configured funnel                   | Apply SERP-validated funnel. Log the correction. Dispatch correction flag to all downstream skills. Display advisory to user explaining the change. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — INTENT CLASSIFICATION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
INTENT CLASSIFICATION QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRIMARY SIGNAL → INTENT TYPE:

Keyword phrasing signals (hypothesis only — verify against SERP):
  "what is", "how to", "guide", "learn"     → Likely Informational
  "best", "top", "vs", "review"             → Likely Commercial
  "buy", "price", "discount", "free trial"  → Likely Transactional
  "[Brand name]" alone                      → Likely Navigational

SERP confirmation signals (override keyword phrasing):
  ≥6 blog/tutorial results                  → Informational CONFIRMED
  ≥6 comparison/review results              → Commercial CONFIRMED
  ≥6 product/landing pages + shopping ads   → Transactional CONFIRMED
  Split across types, no dominant           → MIXED — no single classification
  ≥5 results from same domain               → Navigational CONFIRMED

Intent Strength:
  ≥8/10 results same intent → STRONG
  5-7/10 results same intent → MEDIUM (use MIXED classification)
  3-4/10 results same intent → WEAK
  <3/10 results same intent  → OUTLIER (do not classify as dominant)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — OPPORTUNITY SIGNAL EVIDENCE STANDARDS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
EVIDENCE STANDARDS FOR OPPORTUNITY SIGNAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

EASY — Valid evidence types:
  ✓ "Top [N] results published before [year] with no update signal"
  ✓ "[N] of 10 results are from niche/unknown sites without brand recognition"
  ✓ "No result addresses [specific sub-topic] despite PAA question present"
  ✓ "Featured snippet is a short, incomplete answer — depth gap exploitable"
  ✓ "Dominant format is [Type] but [Type] better serves the stated intent"

MEDIUM — Valid evidence types:
  ✓ "[N] of 10 results are from niche sites; [N] from authority domains"
  ✓ "Content length varies widely — no clear length standard established"
  ✓ "Featured snippet present but no result fully covers the topic"
  ✓ "Some results are recent; some are 2+ years old — mixed freshness"

HARD — Valid evidence types:
  ✓ "[N] of 10 results from high-authority domains with recent update dates"
  ✓ "Featured snippet is comprehensive and well-structured"
  ✓ "All top-5 results exceed [word count] with full semantic coverage"
  ✓ "No niche sites present in top 10 — brand authority dominates"
  ✓ "Content length trend is VERY LONG with high depth across all results"

NEVER acceptable as evidence:
  ✗ "This is a competitive keyword"
  ✗ "High search volume means high competition"
  ✗ "Good content exists on this topic"
  ✗ "The top results are well-optimized"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: research/serp-intelligence.skill*
*Nexus SEO Operating System — Version 1.0.0*
