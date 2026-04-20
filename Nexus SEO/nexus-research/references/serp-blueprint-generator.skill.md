# SKILL: serp/serp-blueprint-generator.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: SERP / Blueprint Generation
# EXECUTION PRIORITY: POST-PATTERN-EXTRACTION — Runs after content-pattern-extractor.skill and serp-intelligence.skill are complete.
# POSITION: The bridge between competitive intelligence and content execution. The final pre-writing artifact.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **content blueprint engine** of the Nexus system.

It converts the aggregated intelligence from SERP analysis and competitive pattern extraction into a complete, actionable, execution-ready content blueprint. Every downstream skill — `content-generation-engine.skill`, `metadata-generator.skill`, `humanizer.skill` — receives this blueprint as its primary instruction set. Nothing is written, generated, or optimized without first passing through the blueprint this skill produces.

The blueprint is not a generic content brief. It is a precision instrument derived from real competitive data: every structural decision reflects observed SERP patterns, every keyword recommendation reflects actual semantic gaps, every differentiation strategy reflects documented competitor weaknesses. Nothing in this blueprint is assumed. Everything is justified by upstream analysis.

### Primary Responsibilities

| Responsibility                        | Description                                                                                                            |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| **SEO Brief Construction**            | Define the content's full SEO context — topic, funnel stage, keyword, difficulty, and strategic priority              |
| **Keyword Expansion**                 | Generate secondary, long-tail, and LSI keywords grounded in semantic coverage and free of duplication                 |
| **Content Requirements Specification**| Translate pattern data into precise word count targets, required sections, required media, and required elements       |
| **Structure Blueprint Creation**      | Build the full content outline — H1 through H3 level — aligned with SERP-validated structure patterns                 |
| **AEO Plan Construction**             | Specify the answer engine optimization layer: direct answer, FAQ sections, PAA alignment, snippet targeting           |
| **Content Elements Placement Plan**   | Specify where and what type of media, tables, examples, callouts, and visual elements appear in the content           |
| **Internal Linking Plan**             | Define which internal pages to link to, from which sections, and with which anchor text strategies                    |
| **Differentiation Strategy**          | Identify three specific, competitor-backed advantages the new content must execute to outperform the SERP             |
| **Metadata Generation**               | Produce a click-optimized meta title, meta description, and URL slug                                                  |

### What This Skill Is NOT

- It is **not** a content writer. It produces the blueprint — `content-generation-engine.skill` executes it.
- It is **not** a keyword researcher. Keywords in the blueprint derive from upstream analysis — this skill expands and organizes, not discovers.
- It is **not** a generic template engine. Every blueprint is specific to the target keyword and its SERP. No two blueprints from this skill are identical.
- It is **not** a simplifier. If the SERP demands a 5,000-word comprehensive guide, this skill produces a blueprint for a 5,000-word comprehensive guide — it does not suggest a shorter alternative for convenience.
- It is **not** optional. No content execution begins without a blueprint from this skill.

### The Defining Standard of This Skill

> Every element of every blueprint produced by this skill must trace back to one of three sources:
> 1. A Must-Have or Good-to-Have pattern signal from `content-pattern-extractor.skill`.
> 2. A content direction directive from `serp-intelligence.skill`.
> 3. A differentiation opportunity identified in the deep SERP analysis pipeline.
>
> Any blueprint element that cannot be traced to one of these three sources is generic — and generic blueprint elements are prohibited.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                      | Required |
|---------------------------------------|------------------------------------------------------------------------------------------------------|----------|
| `content-pattern-extractor.skill`     | Pattern Summary, Priority Signal tables (Must-Have / Good-to-Have / Opportunity), structural data, heading patterns, AEO patterns, quality signals, opportunity detection | YES |
| `serp-intelligence.skill`             | Dominant intent, content type recommendation, recommended format, recommended length, required structural elements, SERP features active, PAA questions, funnel stage (validated), difficulty rating | YES |
| `target_keyword`                      | The exact keyword phrase this blueprint is built for                                                 | YES |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                | Used For                                             |
|---------------------------------------|--------------------------------------------------------------------------------|------------------------------------------------------|
| `pre-execution-input.skill` config    | content_type, tone, depth, goal, audience, competitors, accuracy_mode          | Aligning blueprint with user-configured preferences  |
| `keyword-discovery.skill` output      | Secondary keywords, long-tail variants, LSI terms from the keyword universe   | Pre-populating keyword expansion (Step 2)           |
| `semantic-clustering.skill` output    | Cluster context, related cluster names, cross-reference relationships          | Informing internal linking plan (Step 7)             |
| `memory-controller.skill`             | Existing content titles, published articles, assigned keywords                 | Preventing topic duplication and informing internal links |
| `domain`                              | Target site domain                                                             | URL slug construction, internal link context         |

### Input Pre-Conditions

1. **Are both primary SERP inputs present?** Pattern data AND SERP intelligence are both required. If either is missing → request the missing upstream skill be run before proceeding.
2. **Is `target_keyword` confirmed?** If null → halt. Blueprint cannot be generated without a keyword anchor.
3. **Is the funnel stage validated?** If `serp-intelligence.skill` produced a funnel correction → use the corrected funnel stage, not the original.
4. **Has a blueprint already been generated for this keyword?** → Query `memory-controller.skill`. If a blueprint exists within freshness threshold (14 days) → offer cached. If stale → proceed fresh.
5. **Is `pre-execution-input.skill` config available?** If yes → import preferences. If no → derive all configuration from SERP data.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Full Content Blueprint

```
NEXUS CONTENT BLUEPRINT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Keyword          : [exact keyword]
Blueprint Version       : 1.0
Generated By            : serp/serp-blueprint-generator.skill
Timestamp               : [ISO 8601]
Pattern Confidence      : [HIGH / MEDIUM / LOW — inherited from pattern extractor]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — SEO BRIEF
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Blog Topic              : [human-readable topic title — not the H1, but the editorial topic description]
Funnel Stage            : [TOFU / MOFU / BOFU] — [validated by serp-intelligence.skill]
Content Type            : [Blog Article / Listicle / Tutorial / Comparison / Landing Page / Guide]
Strategic Priority      : [CRITICAL / HIGH / MEDIUM / LOW — from cluster score or opportunity signal]
Primary Keyword         : [exact target keyword]
Estimated Search Volume : [LOW (<1K/mo) / MEDIUM (1K–10K/mo) / HIGH (10K–100K/mo) / VERY HIGH (>100K/mo)]
Keyword Difficulty      : [EASY / MEDIUM / HARD — from serp-intelligence opportunity signal]
Audience                : [Beginner / Intermediate / Expert / Business]
Goal                    : [Traffic / Leads / Sales / Affiliate]
Tone                    : [Professional / Casual / Technical / Persuasive]
Accuracy Mode           : [Standard / High / Strict]
Freshness Strategy      : [Evergreen / Regularly Updated / Date-Specific]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — KEYWORD EXPANSION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Primary Keyword         : [target keyword]

Secondary Keywords (6–8):
  1. [keyword] — [intent: INFO/COM/TRANS] — [placement: H2 / intro / conclusion]
  2. [keyword] — [intent] — [placement]
  3. [keyword] — [intent] — [placement]
  4. [keyword] — [intent] — [placement]
  5. [keyword] — [intent] — [placement]
  6. [keyword] — [intent] — [placement]
  7. [keyword] — [intent] — [placement]
  8. [keyword] — [intent] — [placement]

Long-Tail Keywords (5–7):
  1. [keyword] — [target section]
  2. [keyword] — [target section]
  3. [keyword] — [target section]
  4. [keyword] — [target section]
  5. [keyword] — [target section]
  6. [keyword] — [target section]
  7. [keyword] — [target section]

LSI Keywords (10–15):
  [keyword], [keyword], [keyword], [keyword], [keyword],
  [keyword], [keyword], [keyword], [keyword], [keyword],
  [keyword], [keyword], [keyword], [keyword], [keyword]

Keyword Placement Notes : [brief strategic note on semantic coverage priority]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — CONTENT REQUIREMENTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Word Count
  Minimum (SERP parity)     : [word count — matches dominant range from pattern data]
  Recommended (competitive) : [minimum + 10–20%]
  Maximum (avoid bloat)     : [recommended + 20%]
  Basis                     : [pattern data citation — e.g., "7 of 9 analyzed pages: 2,000–3,000 words"]

Required Sections (Must-Have — from pattern data):
  → [Section type / topic] — [pattern evidence: N/N pages]
  → [Section type / topic] — [pattern evidence]
  → [Section type / topic] — [pattern evidence]
  → [Section type / topic] — [pattern evidence]
  → [Section type / topic] — [pattern evidence]

Required Media (Must-Have — from pattern data):
  → [Media type] — [placement: intro / section X / throughout] — [pattern evidence: N/N pages]
  → [Media type] — [placement] — [pattern evidence]
  → [Media type] — [placement] — [pattern evidence]

Required Content Elements (Must-Have — from pattern data):
  → [Element: table / FAQ / callout / list / etc.] — [placement] — [pattern evidence: N/N pages]
  → [Element] — [placement] — [pattern evidence]
  → [Element] — [placement] — [pattern evidence]

Recommended Elements (Good-to-Have — from pattern data):
  → [Element] — [placement] — [pattern evidence: N/N pages]
  → [Element] — [placement] — [pattern evidence]

Depth Requirement        : [MODERATE / DEEP / EXHAUSTIVE — from quality floor/ceiling analysis]
Quality Baseline         : [minimum quality standard — specific to this SERP]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — STRUCTURE BLUEPRINT (FULL OUTLINE)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

H1: [Full proposed H1 — contains primary keyword — human-readable — not a template label]

DIRECT ANSWER BLOCK (immediately below H1):
"[2–3 sentence direct answer to the keyword's implied question — written in snippet-ready format — under 60 words — complete standalone statement]"

TABLE OF CONTENTS:
  1. [Section 1 title]
  2. [Section 2 title]
  3. [Section 3 title]
  4. [Section 4 title]
  5. [Section 5 title]
  6. [Section 6 title — if required]
  7. [Section 7 title — if required]

─────────────────────────
H2: [Section 1 Title — opening section — aligned with dominant intro pattern]
  Purpose    : [what this section accomplishes for the reader and the SERP]
  Word target: [range]
  Keywords   : [secondary / LSI keywords to incorporate here]
  Elements   : [specific elements required in this section]

  H3: [Sub-section 1A title]
    Content note: [what this sub-section must cover]
    Elements    : [elements specific to this sub-section]

  H3: [Sub-section 1B title]
    Content note: [what this sub-section must cover]
    Elements    : [elements specific to this sub-section]

─────────────────────────
H2: [Section 2 Title]
  Purpose    : [what this section accomplishes]
  Word target: [range]
  Keywords   : [keywords to incorporate]
  Elements   : [required elements]

  H3: [Sub-section 2A title]
    Content note: [coverage requirement]
    Elements    : [specific elements]

  H3: [Sub-section 2B title]
    Content note: [coverage requirement]
    Elements    : [specific elements]

  H3: [Sub-section 2C title — if needed]
    Content note: [coverage requirement]

─────────────────────────
H2: [Section 3 Title]
  [same structure repeated]

─────────────────────────
H2: [Section 4 Title]
  [same structure repeated]

─────────────────────────
H2: [Section 5 Title — AEO section 1]
  Purpose    : [AEO / answer engine optimization]
  Word target: [range]
  Keywords   : [PAA-aligned keywords]
  Elements   : [FAQ format, structured list, definition block]
  AEO Note   : [specific snippet or PAA question this section targets]

  H3: [question-format sub-section]
    Content note: [direct answer in 40–60 words]

  H3: [question-format sub-section]
    Content note: [direct answer]

─────────────────────────
H2: [Section 6 Title — if required by pattern or differentiation]
  [same structure]

─────────────────────────
H2: [Section 7 Title — AEO section 2 or closing section]
  Purpose    : [FAQ / summary / next steps / verdict]
  Elements   : [FAQ block, CTA if applicable, summary block]

[END OF STRUCTURE BLUEPRINT]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — AEO PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Direct Answer Block (required immediately under H1):
  Format          : [Paragraph / List / Table — based on active featured snippet type]
  Target Length   : [40–60 words]
  Target Query    : "[the implied question the keyword represents]"
  Draft Answer    : "[written direct answer — snippet-ready — complete and self-contained]"

AEO H2 Section 1:
  Location in Outline : [Section number]
  Target              : [specific PAA question or informational sub-intent]
  Format              : [numbered list / definition / paragraph — based on snippet type]
  Approach            : [how to structure the answer for maximum snippet eligibility]

AEO H2 Section 2:
  Location in Outline : [Section number]
  Target              : [specific PAA question or informational sub-intent]
  Format              : [format type]
  Approach            : [structure approach]

FAQ Block (5 Questions — snippet-ready):

  Q1: [question — natural language, directly matching PAA or common user query]
  A1: [answer — 40–80 words — complete standalone — no "as mentioned above" references]

  Q2: [question]
  A2: [answer]

  Q3: [question]
  A3: [answer]

  Q4: [question]
  A4: [answer]

  Q5: [question]
  A5: [answer]

Schema Recommendation    : [FAQ + Article / HowTo / FAQ only — based on pattern data]
Snippet Eligibility Note : [what specific formatting requirements must be met for snippet capture]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 6 — CONTENT ELEMENTS PLACEMENT PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Each element specifies TYPE, LOCATION in the outline, PURPOSE, and SPECIFICATION]

IMAGES:
  Image 1 — [type: screenshot / diagram / infographic / chart]
    Location    : [Section / H3 reference]
    Purpose     : [why this image is needed here]
    Specification: [what the image must show — specific, not generic]

  Image 2 — [type]
    Location    : [Section / H3 reference]
    Purpose     : [purpose]
    Specification: [specification]

  [Additional images as required by pattern data]

TABLES:
  Table 1 — [type: data / comparison / summary]
    Location    : [Section / H3 reference]
    Purpose     : [purpose]
    Specification: [what data/comparisons the table must contain — column headings if applicable]

  [Additional tables as required]

CALLOUT BOXES:
  Callout 1 — [type: tip / warning / note / key stat / important]
    Location    : [Section / H3 reference]
    Purpose     : [purpose]
    Content     : [what the callout must highlight]

  [Additional callouts as required]

CODE BLOCKS (if technical content):
  Code Block 1 — [language: Python / JavaScript / Bash / etc.]
    Location    : [Section / H3 reference]
    Purpose     : [what this code demonstrates]
    Specification: [what the code example must show]

  [Additional code blocks as required]

EXAMPLES:
  Example 1 — [original / hypothetical / named case]
    Location    : [Section / H3 reference]
    Purpose     : [what concept this example illustrates]
    Specification: [specific scenario or case to cover]

  [Additional examples — minimum count from content requirements]

EXPERT QUOTES / STATISTICS:
  Stat/Quote 1
    Location    : [Section reference]
    Type        : [statistic / expert quote]
    Subject     : [what claim it must support]
    Source Note : [source type required — academic / industry report / named expert]

  [Additional stats/quotes as required]

INFOGRAPHICS:
  Infographic 1 — [type]
    Location    : [Section reference]
    Purpose     : [what it visualizes]
    Specification: [data or process to visualize]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 7 — INTERNAL LINKING PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Linking Strategy        : [Cluster-based / Topic authority / Funnel bridge / Mixed]
Cluster Context         : [name of the semantic cluster this content belongs to]

Recommended Internal Links:

  Link 1
    From Section  : [H2 or H3 reference in this blueprint]
    To Page       : [target page title or topic]
    Anchor Text   : [exact or near-exact anchor text recommendation]
    Link Purpose  : [why this link — topical relevance / funnel flow / authority distribution]

  Link 2
    From Section  : [reference]
    To Page       : [target]
    Anchor Text   : [anchor text]
    Link Purpose  : [purpose]

  Link 3
    From Section  : [reference]
    To Page       : [target]
    Anchor Text   : [anchor text]
    Link Purpose  : [purpose]

  Link 4
    From Section  : [reference]
    To Page       : [target]
    Anchor Text   : [anchor text]
    Link Purpose  : [purpose]

  Link 5
    From Section  : [reference]
    To Page       : [target]
    Anchor Text   : [anchor text]
    Link Purpose  : [purpose]

Anchor Text Rules for This Blueprint:
  → Use [type: descriptive / partial match / exact match] anchors
  → Avoid: [specific anchor patterns to avoid based on topic]
  → Priority pages to receive links from this article: [list]

External Link Recommendations:
  → [Source type 1] — [what claim it should support] — [placement section]
  → [Source type 2] — [what claim] — [placement section]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 8 — DIFFERENTIATION STRATEGY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Competitive Context     : [1-sentence summary of what the top-ranking content looks like]

ADVANTAGE 1 — [What Competitors Miss]
  Gap Identified        : [specific element absent from most or all competing pages — cite N/N pages]
  Execution             : [exactly how new content fills this gap]
  Expected Impact       : [why filling this gap creates ranking or user-experience advantage]

ADVANTAGE 2 — [What to Improve Upon]
  Weakness Identified   : [specific underdevelopment in existing top content — cite pattern evidence]
  Execution             : [exactly how new content improves on this weakness]
  Expected Impact       : [what this improvement delivers]

ADVANTAGE 3 — [What to Add Uniquely]
  Unique Angle          : [something no competitor has — original element, format, or depth level]
  Execution             : [exactly what the unique addition is and how it is produced]
  Expected Impact       : [what competitive separation this creates]

Summary Differentiation Statement:
  "[Single sentence summarizing the content's competitive position — what makes it the best available resource on this topic]"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 9 — METADATA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Meta Title (55–65 characters):
  Primary Option  : [title — character count: XX]
  Backup Option   : [title — character count: XX]
  Title Rules Applied:
    → Primary keyword present: [YES / NO]
    → Click-motivator included: [YES — type: number / power word / question / year]
    → Within character limit: [YES / NO]

Meta Description (150–160 characters):
  Primary Option  : [description — character count: XX]
  Backup Option   : [description — character count: XX]
  Description Rules Applied:
    → Primary keyword present: [YES / NO]
    → Clear value proposition: [YES / NO — description of what's promised]
    → CTA present: [YES / NO — type: Learn / Discover / Find out / See]
    → Within character limit: [YES / NO]

URL Slug:
  Primary Option  : /[slug]/
  Backup Option   : /[slug]/
  Slug Rules Applied:
    → Lowercase and hyphenated: YES
    → Contains primary keyword: YES
    → Length: [X words]
    → No stop words: [YES / NO — note if retained for readability]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BLUEPRINT SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Word Count       : [recommended word count]
H2 Sections             : [count]
H3 Sub-sections         : [count]
FAQ Questions           : [count]
Media Elements          : [count]
Internal Links          : [count]
Must-Have Elements Met  : [count / total must-have signals]
Differentiation Angles  : 3
AEO Coverage            : [FULL / PARTIAL]
Pattern Source          : content-pattern-extractor.skill — [N] pages analyzed
SERP Intelligence Source: serp-intelligence.skill — [keyword]
Blueprint Readiness     : COMPLETE — dispatch to content-generation-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DEFINE SEO BRIEF

Construct the complete SEO context block that frames every decision made in the remaining blueprint steps. The SEO Brief is the single source of truth for what this content piece is, who it serves, and what it must achieve.

**Phase 1A — Blog Topic Definition**

The Blog Topic is NOT the H1. It is a human-readable editorial statement describing the content's subject in plain language that a managing editor would understand without needing SEO context.

Rules:
- Must capture the full scope of the article — not just the primary keyword.
- Must be phrased as a topic, not a headline (no click-bait, no questions, no formatting tricks).
- Must be specific enough that no other article on the site could carry the same topic without being a direct duplicate.

Examples:
- Primary keyword: `"best CRM software"` → Blog Topic: `"A comprehensive buyer's guide to CRM software for sales teams"`
- Primary keyword: `"how to set up a CRM"` → Blog Topic: `"Step-by-step guide to configuring a CRM system from scratch"`

**Phase 1B — Funnel Stage Assignment**

Use the funnel stage validated by `serp-intelligence.skill`. Do not use the original assignment from `keyword-discovery.skill` or `pre-execution-input.skill` if a correction was issued.

- Record the validated funnel stage.
- Record whether a correction was applied.
- If a correction was applied → note the downstream implications for tone, CTA, and structural approach.

**Phase 1C — Content Type Determination**

Use the recommended content type from `serp-intelligence.skill`'s Content Direction Intelligence section. Cross-reference against the dominant heading pattern identified by `content-pattern-extractor.skill`.

If the two sources recommend different content types:
- Use the `serp-intelligence.skill` recommendation as primary (it is based on observed SERP patterns).
- Note the discrepancy and the reason the primary source was chosen.

**Phase 1D — Strategic Priority Assignment**

Assign strategic priority based on the convergence of:
1. Cluster score from `semantic-clustering.skill` (if available).
2. Opportunity signal difficulty from `serp-intelligence.skill`.
3. Keyword funnel stage (BOFU > MOFU > TOFU for revenue priority; reverse for traffic priority).

| Priority Level | Condition                                                                              |
|----------------|----------------------------------------------------------------------------------------|
| CRITICAL       | BOFU or high-opportunity MOFU + EASY or MEDIUM difficulty + high cluster score        |
| HIGH           | Any funnel stage + EASY or MEDIUM difficulty + MOFU or above                          |
| MEDIUM         | TOFU + MEDIUM difficulty + moderate cluster score                                      |
| LOW            | TOFU + HARD difficulty + low cluster score or broad keyword                           |

**Phase 1E — Estimated Search Volume and Keyword Difficulty**

These are qualitative estimates derived from available signals, not live data:

**Search Volume Estimation:**
- Use keyword modifiers, topic breadth, and funnel stage as proxies.
- Head terms (1–2 words, no modifiers) → likely MEDIUM–VERY HIGH.
- Long-tail terms (4+ words with specific modifiers) → likely LOW–MEDIUM.
- Comparison terms with named tools → likely LOW–MEDIUM but high conversion.

**Keyword Difficulty:**
- Inherit directly from `serp-intelligence.skill` opportunity signal rating.
- EASY → difficulty: EASY. MEDIUM → MEDIUM. HARD → HARD.

**Phase 1F — Audience, Goal, Tone, Accuracy, Freshness**

Import from `pre-execution-input.skill` configuration if available. If not available, derive from:
- Audience: from funnel stage + keyword audience signals (defined in `keyword-discovery.skill` anchor).
- Goal: from funnel stage (TOFU → Traffic, MOFU → Leads, BOFU → Sales/Affiliate).
- Tone: from `serp-intelligence.skill` Tone Signal directive.
- Accuracy: from `serp-intelligence.skill` (technical topics → High or Strict).
- Freshness: from `serp-intelligence.skill` Freshness Strategy directive.

---

### ▸ STEP 2 — KEYWORD EXPANSION

Generate the full keyword ecosystem for this content piece — secondary, long-tail, and LSI keywords that create comprehensive semantic coverage without duplication.

**Phase 2A — Secondary Keywords (6–8)**

Secondary keywords are keyword phrases that:
- Directly relate to the primary keyword.
- Target the same core topic from different angles.
- Can realistically be incorporated into section headings (H2 or H3) or major content passages.
- Were identified in `keyword-discovery.skill` output as Dimensions 1–4 (Problem-Based, How-To, Comparison, Alternatives).

**Secondary Keyword Selection Rules:**
1. No secondary keyword may be a substring or reformulation of the primary keyword alone (e.g., if primary is "CRM software," secondary cannot be "CRM" or "software CRM" — these are exact/near-exact matches, not secondary keywords).
2. Every secondary keyword must have a distinct intent angle from the primary.
3. Each secondary keyword must be assigned a placement target (H2, intro, conclusion, specific section) — not left as free-floating.
4. Maximum 1 secondary keyword per major section of the outline — do not concentrate keywords.

**Secondary Keyword Intent Classification:**

Every secondary keyword is labeled with its intent type:
- INFO (Informational): Educational angle on the topic.
- COM (Commercial): Evaluation, comparison, or selection angle.
- TRANS (Transactional): Action-oriented, conversion angle.

**Phase 2B — Long-Tail Keywords (5–7)**

Long-tail keywords are highly specific phrases (4+ words) that:
- Address narrow sub-intents within the topic.
- Are naturally worked into sub-section headings (H3) or body content passages.
- Were identified in `keyword-discovery.skill` Dimension 5 (Long-Tail).

**Long-Tail Keyword Selection Rules:**
1. Every long-tail keyword must represent a genuinely distinct search query — not a padded version of the primary keyword.
2. Each long-tail keyword must be assigned to a specific section of the content outline where it will naturally appear.
3. Long-tail keywords must collectively cover the audience spectrum: at least 1 must target a specific industry, at least 1 must address a specific use case, and at least 1 must address a constraint (budget, team size, technical level).

**Phase 2C — LSI Keywords (10–15)**

LSI (Latent Semantic Indexing) keywords are the conceptually related terms that a comprehensive treatment of the topic would naturally include. They are not targeted directly — they are woven into prose, headings, and supporting content to build topical authority signals.

**LSI Keyword Sources:**
- Semantic Expansion keywords from `keyword-discovery.skill` Dimension 7.
- Semantic neighbors identified by `semantic-clustering.skill` LSI simulation.
- PAA questions from `serp-intelligence.skill` (the core concepts behind those questions become LSI terms).

**LSI Keyword Rules:**
1. No LSI keyword may duplicate any primary, secondary, or long-tail keyword.
2. LSI keywords should span the full semantic field — include tool names, process terms, concept terms, and outcome terms.
3. LSI keywords are listed as a comma-separated group — they do not receive individual placement targets (they are incorporated naturally).

**Phase 2D — Deduplication Check**

After generating all three keyword tiers, run a full deduplication check:
- No keyword appears in more than one tier.
- No two keywords in any tier are semantic duplicates (≥80% similarity).
- All keywords are distinct from any existing content in the project (via `memory-controller.skill` lookup).

---

### ▸ STEP 3 — CONTENT REQUIREMENTS

Translate the pattern data from `content-pattern-extractor.skill` into precise, quantified content requirements.

**Phase 3A — Word Count Targets**

**Minimum Word Count:**
The minimum is the lower boundary of the dominant word count range from the pattern extractor.
`minimum = dominant_range_lower_bound`
This is the floor below which the content cannot compete.

**Recommended Word Count:**
Apply the 10–20% uplift over the dominant range midpoint:
`recommended = dominant_range_midpoint × 1.15` (use 15% as default; increase to 20% if difficulty = HARD)

Round to the nearest 100 words.

**Maximum Word Count:**
Apply 20% over the recommended:
`maximum = recommended × 1.20`

The maximum prevents scope creep and forces deliberate section-level word allocation rather than padding.

**Word Count Basis:**
Always cite the specific pattern data behind the word count targets:
`"Based on [N]/[total] analyzed pages in the [word count range] range"`

**Phase 3B — Required Sections**

Import every MUST-HAVE pattern signal from `content-pattern-extractor.skill` Section 8 that relates to content sections, topics, or structural elements. These become required sections.

For each required section:
1. State what section type it is (definitional, procedural, comparative, FAQ, etc.).
2. Cite the pattern evidence (N/N pages).
3. Note whether it maps to a specific heading pattern position (opening, middle, closing).

**Phase 3C — Required Media**

Import all MUST-HAVE media signals from `content-pattern-extractor.skill` Section 2 (Media Pattern).

For each required media type:
1. State the type specifically (not "images" — "screenshots showing [topic]" or "custom diagram illustrating [concept]").
2. Cite the pattern evidence.
3. Specify placement priority (intro, specific section, throughout).

**Phase 3D — Required Content Elements**

Import all MUST-HAVE element signals from `content-pattern-extractor.skill` Section 8 that relate to content elements (tables, lists, callouts, code blocks, etc.).

For each required element:
1. State the type and sub-type (not "table" — "comparison table with [specific columns]").
2. Cite the pattern evidence.
3. Specify placement.

**Phase 3E — Depth and Quality Baseline**

From `content-pattern-extractor.skill` Section 4 (Quality Signal Patterns):
- State the quality floor identified from the pattern analysis.
- State the quality ceiling.
- Define the quality baseline for new content: `new content must meet or exceed the quality ceiling, not merely the floor`.

---

### ▸ STEP 4 — STRUCTURE BLUEPRINT

Create the full content outline, from H1 through H3, grounded in the dominant heading pattern from `content-pattern-extractor.skill` and aligned with SERP-validated intent from `serp-intelligence.skill`.

**Phase 4A — H1 Construction**

Rules for H1:
1. Must contain the primary keyword — either exact match or close variation that preserves full meaning.
2. Must be specific and descriptive — a reader must know exactly what the article is about from the H1 alone.
3. Must be distinct from the meta title (same keyword coverage; different phrasing; H1 can be longer).
4. Must not be a question (for informational content) unless the SERP is question-dominant.
5. Must not use superlatives (`"Ultimate"`, `"Perfect"`, `"Best Ever"`) unless the SERP demonstrates these rank.
6. Length: 6–12 words preferred.

**Produce two H1 options.** The content generation engine selects the final H1 during writing.

**Phase 4B — Direct Answer Block**

Immediately below the H1, before the table of contents, insert a Direct Answer Block.

Rules:
- 2–3 sentences maximum.
- Under 60 words.
- Must directly answer the implied question of the primary keyword.
- Must be a complete, standalone statement — a reader could read only this block and have a usable answer.
- Must use the primary keyword naturally within the first sentence.
- Format based on active featured snippet type from `serp-intelligence.skill`:
  - Paragraph snippet active → prose block.
  - List snippet active → 3-bullet summary.
  - Table snippet active → 2-column 3-row mini-table.

This block IS the primary featured snippet target.

**Phase 4C — Table of Contents Construction**

Generate the TOC from the H2 sections defined in Phase 4D. TOC rules:
- List all H2 sections by title only (not sub-sections).
- Maximum 7 entries.
- TOC is present only if the pattern analysis shows ≥40% of pages include a TOC.
- If TOC is not warranted by pattern data → omit and note the reason.

**Phase 4D — H2 Section Construction**

Build a maximum of 7 H2 sections. Each section must:
1. Be grounded in the dominant heading pattern (Pattern A/B/C/D) from `content-pattern-extractor.skill`.
2. Serve a defined purpose — what it accomplishes for the reader AND what SEO/AEO signal it serves.
3. Contain a word count target (specific range, not a generic "standard length").
4. List the secondary or LSI keywords to be incorporated.
5. List the specific content elements required within that section.

**H2 Section Ordering Logic:**

The order of H2 sections follows the dominant pattern structure:

| Pattern A Order | Pattern B Order | Pattern C Order | Pattern D Order |
|-----------------|-----------------|-----------------|-----------------|
| Definition/What | Listicle intro  | Problem framing | Overview        |
| How-To/Steps    | Item breakdown  | Solution intro  | Step 1          |
| Examples        | Comparison      | Deep analysis   | Step 2...       |
| Advanced/Tips   | Verdict         | Advanced tactics| Tools/Resources |
| Case Studies    | —               | —               | Mistakes        |
| FAQ (AEO 1)     | FAQ (AEO)       | FAQ (AEO)       | FAQ (AEO)       |
| Summary/Next    | Summary         | Next steps      | Summary         |

If pattern is MIXED → use the hybrid logic defined in the Advanced Logic section.

**Section Naming Rules:**

Each H2 section title must:
1. Be human-readable and topic-specific — never a placeholder like "Section 3" or "How-To".
2. Contain at least one keyword (primary, secondary, or LSI) where natural.
3. Follow the heading pattern's typical phrasing conventions:
   - Pattern A: "What is [X]", "How to [Action]", "[N] Examples of [X]"
   - Pattern B: "[N] Best [Category]", "How [Item] Works", "[A] vs [B]"
   - Pattern C: "The Problem With [X]", "How [Solution] Fixes [Problem]", "Advanced [Topic]"
   - Pattern D: "[Topic] Overview", "Step [N]: [Action]", "Best [Category] for [Task]"
4. NOT be generic (prohibited: "Introduction", "Overview", "Conclusion", "Tips").
5. NOT duplicate another H2 in the same blueprint (each section must have a unique topic scope).

**Phase 4E — H3 Sub-Section Construction**

For each H2 section, build 2–4 H3 sub-sections as required by topic complexity and word count target.

H3 rules:
1. Each H3 must be semantically subordinate to its parent H2 — narrower in scope, not parallel.
2. H3 titles use the same naming rules as H2 but at a more specific level.
3. Each H3 receives a `Content note` — a 1–2 sentence description of what must be covered and how.
4. H3s that house specific content elements (tables, code blocks, examples) are explicitly labeled.
5. Not every H2 requires H3s — short, focused sections (under 300 words) may stand without sub-sections.

---

### ▸ STEP 5 — AEO PLAN

Build the complete answer engine optimization layer of the blueprint.

**Phase 5A — Direct Answer Block Planning**

Already constructed in Step 4B. In the AEO Plan, specify:
- The exact format (paragraph / list / table).
- The exact character/word target.
- The specific query this answer targets.
- A drafted version of the direct answer for `content-generation-engine.skill` to refine.

**Phase 5B — AEO H2 Section 1 Definition**

Identify which section in the blueprint is the primary AEO section — the section specifically structured to capture a featured snippet or PAA answer.

Specification:
- Location: which H2 position in the outline.
- Target: which specific PAA question (from `serp-intelligence.skill` output) this section targets.
- Format: numbered list / definition / paragraph (based on active snippet type for that question).
- Approach: specific instruction on how to structure the answer for maximum eligibility.

**Phase 5C — AEO H2 Section 2 Definition**

Identify the secondary AEO section — typically the FAQ section or a secondary sub-topic that addresses additional PAA questions.

Specification: same format as Phase 5B.

**Phase 5D — FAQ Block Construction (5 Questions)**

Generate 5 FAQ questions and their answers.

**Question Selection Rules:**
1. Every question must align with a PAA question from `serp-intelligence.skill` OR a question-based keyword from `keyword-discovery.skill` Dimension 6.
2. No two questions may address the same sub-intent.
3. Questions must be phrased as natural language queries — exactly as a person would type or speak them.
4. Questions must collectively cover: definition (what is), process (how to), comparison (what's better), suitability (is it right for me), and an advanced or edge-case angle.

**Answer Construction Rules:**
1. Every answer must be 40–80 words.
2. Every answer must be completely self-contained — a reader who reads only the answer without the rest of the article must receive a usable, complete response.
3. Every answer must contain the question's core keyword naturally.
4. No answer may reference other sections of the article (`"as mentioned above"` — prohibited).
5. Every answer should begin with a direct declarative sentence answering the question, followed by supporting detail.

**Phase 5E — Schema Recommendation**

Based on the pattern data (schema signals from `content-pattern-extractor.skill` Section 3):
- Recommend the schema types to implement.
- Prioritize FAQ schema if the blueprint includes 3+ FAQ questions.
- Recommend HowTo schema if the blueprint follows Pattern D or contains a numbered steps section.
- Recommend Article schema universally for all editorial content.
- Specify which schema applies to which section of the content.

---

### ▸ STEP 6 — CONTENT ELEMENTS PLACEMENT PLAN

Specify every non-prose content element — where it appears, what type it is, and what it must contain.

**Phase 6A — Image Placement**

For each image required by the Content Requirements:
1. Specify the type (screenshot / custom diagram / infographic / chart / icon set).
2. Assign a precise location in the outline (H2 section + H3 if applicable).
3. Write a specification that tells the content creator exactly what the image must depict — not just "an image showing the topic."

**Image Specification Examples:**

| Vague (Prohibited)              | Specific (Required)                                                                        |
|---------------------------------|--------------------------------------------------------------------------------------------|
| "Include an image here"         | "Screenshot of [tool] dashboard showing the [specific feature] configuration step"        |
| "Add a diagram"                 | "Custom diagram illustrating the three-stage process: [Stage 1] → [Stage 2] → [Stage 3]" |
| "Use an infographic"            | "Infographic comparing [Tool A] vs [Tool B] vs [Tool C] across [5 criteria]"              |

**Phase 6B — Table Placement**

For each table required:
1. Specify type: data table / comparison table / summary table / pricing table.
2. Assign location.
3. Define column headers and what data each column must contain.
4. State the minimum row count.

**Phase 6C — Callout Box Placement**

For each callout box:
1. Specify type: Tip / Warning / Note / Key Stat / Important / Pro Tip.
2. Assign location.
3. State what the callout must highlight — the specific insight or data point.

**Phase 6D — Code Block Placement (Technical Content Only)**

For each code block (only if content type = Technical Guide, accuracy mode = Strict, or audience = Expert):
1. Specify language.
2. Assign location.
3. Describe exactly what the code must demonstrate.
4. Note whether comments are required within the code.

**Phase 6E — Examples Placement**

For each example:
1. Specify type: original / hypothetical / named case / real-world scenario.
2. Assign location.
3. Describe the scenario or concept the example must illustrate.
4. State whether it should include specific numbers, outcomes, or named entities.

**Phase 6F — Statistics and Expert Quotes Placement**

For each required statistic or quote:
1. Assign location.
2. State what claim it must support.
3. Specify the source type required (academic paper / industry report / named expert / authoritative publication).
4. Note: never invent statistics — flag for sourcing at content generation time.

---

### ▸ STEP 7 — INTERNAL LINKING PLAN

Design the internal linking strategy for this content piece within the site's broader content architecture.

**Phase 7A — Linking Strategy Classification**

Based on the semantic cluster this content belongs to (from `semantic-clustering.skill`) and the site's existing content map (from `memory-controller.skill`), classify the linking strategy:

| Strategy Type         | Description                                                                            |
|-----------------------|----------------------------------------------------------------------------------------|
| **Cluster-based**     | Links primarily to other content within the same semantic cluster                     |
| **Topic authority**   | Links to the pillar page for this cluster + supporting content                        |
| **Funnel bridge**     | Links to content at adjacent funnel stages (TOFU article links to MOFU comparison)   |
| **Mixed**             | Combination of cluster-based and funnel bridge linking                                |

**Phase 7B — Link Recommendations**

Generate 3–5 specific internal link recommendations:

For each link:
1. **From Section:** The specific H2 or H3 in this blueprint where the link will appear.
2. **To Page:** The title or topic of the destination page. If the page already exists (from memory) → use exact title. If it doesn't exist yet → describe the intended topic.
3. **Anchor Text:** The exact anchor text to use. Must be descriptive and keyword-rich. Must not be generic ("click here," "read more," "this article").
4. **Link Purpose:** Why this link exists — topical relevance, funnel flow, authority distribution, or related cluster connection.

**Anchor Text Rules for This Blueprint:**

Define the specific anchor text approach appropriate for this content's topic and competitive context:
- Describe the preferred anchor text type (exact match / partial match / descriptive / branded).
- List any anchor text patterns to avoid for this specific topic.
- Name the priority pages that should receive links from this article.

**Phase 7C — External Link Recommendations**

Generate 2–3 external link recommendations:

For each:
1. State the type of source (academic study / industry report / official documentation / authoritative publication).
2. State what claim it should support.
3. Assign the section where it should appear.
4. Note that specific URLs are sourced at content generation time — this plan specifies source type and purpose only.

---

### ▸ STEP 8 — DIFFERENTIATION STRATEGY

Define three specific, evidence-backed competitive advantages that new content must execute to outperform the current top results.

**Phase 8A — Competitive Context Statement**

Write one sentence summarizing the current state of the top-ranking content on this SERP. This sets the baseline against which differentiation is defined.

The statement must:
- Reference the dominant content type and quality level.
- Reference the most significant pattern that characterizes the competitive set.
- Not use vague language ("good content exists on this topic").

Example (not a template — must be specific): `"The top-ranking results for this keyword are predominantly 2,000-word blog posts following a definition-first structure, with moderate quality (examples present, statistics absent) and minimal AEO optimization — only 2 of 9 analyzed pages have FAQ sections."`

**Phase 8B — Advantage 1: What Competitors Miss**

Drawn from the zero-presence or lowest-frequency opportunity signals in `content-pattern-extractor.skill` Section 7.

Format:
1. Gap identified: Name the specific element that is absent or nearly absent from the competitive set. Cite frequency (N/N pages).
2. Execution: Describe exactly how new content fills this gap — not generically but in the specific context of this topic and keyword.
3. Expected impact: Explain the specific ranking or user-experience benefit.

**Phase 8C — Advantage 2: What to Improve Upon**

Drawn from the universal weakness signals in `content-pattern-extractor.skill` Section 7 (elements present but universally underdeveloped).

Format: same structure as Advantage 1.

Example pattern:
- Statistics are present in 78% of pages but all are unsourced → Advantage: include sourced statistics with attributable data.
- FAQ sections exist in 67% of pages but average only 2 questions → Advantage: provide 5+ FAQs with full snippet-optimized answers.

**Phase 8D — Advantage 3: What to Add Uniquely**

A competitive angle that exists nowhere in the SERP — something genuinely original that no competing page offers.

This cannot be derived from pattern data (because pattern data only shows what exists). It must be derived from:
- The keyword's semantic field and what a first-principles treatment of the topic would add.
- The quality signals from `deep-serp-analysis.skill` (what first-party data, original research, or unique perspectives would add).
- The audience's unmet needs that neither the SERP nor existing content addresses.

Format: same structure as Advantages 1 and 2.

**Phase 8E — Summary Differentiation Statement**

Write a single sentence that encapsulates the content's competitive position:
- What makes it the best available resource on this topic.
- What a reader gains by choosing this content over the current top result.

---

### ▸ STEP 9 — METADATA

Generate all metadata elements required for SERP display.

**Phase 9A — Meta Title Construction**

Rules:
1. Length: 55–65 characters (measure precisely — not a range for approximation).
2. Must contain the primary keyword (exact or close variation).
3. Must contain at least one of: a power word, a number, a current year, or a question format — whichever is most appropriate for the content type and SERP.
4. Must not be identical to the H1 (different phrasing, same keyword coverage).
5. Must be click-motivating — a reader scanning the SERP must find it compelling vs. the competition.

**Power Word Categories (use one):**
- Specificity words: "Exactly", "Precisely", "Step-by-Step", "Complete"
- Value words: "Free", "Proven", "Essential", "Expert"
- Urgency words: "[Year]", "Updated", "New"
- Scope words: "Full Guide", "Everything You Need", "The Only [X] You Need"

Produce two meta title options. Count characters for both. Label each with its click-motivator type.

**Phase 9B — Meta Description Construction**

Rules:
1. Length: 150–160 characters (measure precisely).
2. Must contain the primary keyword (early in the description, ideally first 80 characters).
3. Must include a clear value proposition — what the reader gets from clicking.
4. Must include a CTA — "Learn," "Discover," "Find out," "See," or equivalent.
5. Must not duplicate the meta title phrasing.
6. Must not be generic (prohibited: "This article covers everything about [topic]").

Produce two meta description options. Count characters for both.

**Phase 9C — URL Slug Construction**

Rules:
1. Lowercase only.
2. Hyphens between words (no underscores, no spaces).
3. Contains primary keyword — exact match preferred, close variation acceptable.
4. No stop words (a, the, is, of, for, in) unless their removal makes the slug unreadable or ambiguous.
5. Length: 3–6 words preferred. Maximum 8 words.
6. No trailing slash inconsistency — always includes trailing slash.

Produce two URL slug options. Note whether stop words were included and why if they were.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ PATTERN WEIGHT APPLICATION

All blueprint decisions are weighted by the signal strength of the patterns they derive from.

**Weighting Application Rules:**

| Signal Tier      | Blueprint Application                                                                 |
|------------------|---------------------------------------------------------------------------------------|
| MUST-HAVE (≥60%) | Appears in Required Sections, Required Media, Required Elements — non-negotiable     |
| GOOD-TO-HAVE (30–59%) | Appears in Recommended Elements — included if word count permits               |
| OPPORTUNITY (<30%) | Appears in Differentiation Strategy — targeted as competitive advantages           |

No signal is included in the blueprint at a tier above its pattern strength:
- A GOOD-TO-HAVE element must not appear as a required section.
- An OPPORTUNITY element must not appear as a good-to-have — it is a differentiator, not a standard.

---

### ▸ TOFU/MOFU/BOFU BALANCE WITHIN THE BLUEPRINT

Depending on the validated funnel stage, the blueprint's structural balance must reflect the reader's journey:

**TOFU Blueprint Balance:**
- Opening sections: educational, definitional, awareness-building.
- Middle sections: practical, how-to, process-based.
- Closing sections: FAQ, next steps, further reading.
- CTA: Soft (subscribe, download resource, explore tool) — not hard purchase CTA.

**MOFU Blueprint Balance:**
- Opening sections: context-setting, problem acknowledgment.
- Middle sections: comparison, evaluation, criteria-setting.
- Closing sections: verdict, recommendation, FAQ.
- CTA: Medium (start trial, request demo, see pricing) — indicates evaluation readiness.

**BOFU Blueprint Balance:**
- Opening sections: solution-first, benefit-oriented.
- Middle sections: deep product/service detail, proof (case studies, testimonials).
- Closing sections: objection handling, FAQ, strong CTA.
- CTA: Hard (buy now, get started, schedule a call).

---

### ▸ HYBRID BLUEPRINT LOGIC (MIXED INTENT)

When `serp-intelligence.skill` classifies the intent as MIXED, the blueprint must accommodate two user types simultaneously.

**Hybrid Blueprint Rules:**

1. The opening section serves the informational intent (defines and educates).
2. The middle sections serve both intents (practical + comparative content coexists).
3. The closing section serves the commercial intent (comparison summary, recommendation, or verdict).
4. The direct answer block targets the informational sub-intent (most common entry point).
5. The FAQ section covers questions from both intent types.
6. The meta title targets the dominant intent (informational or commercial — whichever is slightly stronger).
7. The internal linking plan connects to both TOFU content (upstream) and BOFU content (downstream).

---

### ▸ SECTION-LEVEL WORD COUNT ALLOCATION

After defining the total recommended word count, allocate it across H2 sections:

**Allocation Rules:**
1. The opening section (definition/overview) receives 10–15% of total word count.
2. The core content sections (H2 2 through H2 N-2) receive 60–70% total, distributed evenly or weighted toward the deepest sections.
3. The AEO section (FAQ) receives 10–15% — enough for complete, self-contained answers.
4. The closing section receives 5–10%.
5. No single H2 section may receive more than 30% of total word count (prevents a single bloated section).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Keywords Across Tiers

Every keyword in the blueprint (primary, secondary, long-tail, LSI) must be unique. A keyword cannot appear in two tiers. If a keyword from `keyword-discovery.skill` output appears in multiple tier candidates → assign it to the most appropriate tier → exclude from all others.

### Rule 2 — No Repeated H2 Section Topics

Every H2 section in the structure blueprint must cover a distinct topic scope. If two H2s would produce overlapping content → merge them into one H2 with expanded H3s. No two sections may answer the same reader question.

### Rule 3 — No Redundant Ideas Across Blueprint Parts

An insight, recommendation, or requirement stated in Part 3 (Content Requirements) must not be restated in Part 8 (Differentiation Strategy) as an advantage. If an element is already required (Must-Have), it cannot also be a differentiator — differentiators are things the competition does NOT do.

### Rule 4 — No Duplicate FAQ Questions

All 5 FAQ questions must address different sub-intents. No two questions may be paraphrases of each other. Run a semantic check: if two questions share ≥70% semantic overlap → rewrite the less strategically valuable one.

### Rule 5 — No Generic Anchor Text in Internal Linking Plan

Every anchor text recommendation must be unique across the 5 internal links. No two links may use the same anchor text. No anchor text may be generic.

### Rule 6 — Memory Deduplication

Before finalizing the blueprint, verify with `memory-controller.skill` that:
- No existing article on the site already targets the primary keyword.
- No existing article uses the proposed H1 or URL slug.
- No recently published article covers the same topic scope.
If conflicts are found → adjust the unique angle, H1, and URL slug to create differentiation from existing content.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Blueprint Behaviors

| Prohibited Behavior                                                                             | Reason                                                                             |
|-------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| Using placeholder H2 titles like "Introduction", "Overview", "Conclusion"                      | Every heading must be a specific, keyword-informed topic title — not a generic label |
| Generating secondary keywords that are reorderings of the primary keyword                       | Reorderings are not secondary keywords — they are the same keyword in a different form |
| Setting a word count without citing the pattern evidence behind it                             | Word count targets must be SERP-grounded — not assumed or estimated without basis  |
| Including a content element in Required Elements without specifying placement                  | Unplaced requirements are not actionable — every element needs a section location |
| Writing FAQ answers that reference other parts of the article                                  | FAQ answers must be self-contained — referenced answers fail AEO requirements     |
| Listing differentiation advantages that match Must-Have pattern signals                        | Advantages must come from opportunity signals — not from standard requirements    |
| Constructing a meta title that exceeds 65 characters without flagging the violation            | Character limits are precise — violations must be detected, not glossed over      |
| Producing a URL slug longer than 8 words without explicit justification                        | Slug length must be deliberate — long slugs are a technical SEO liability         |
| Placing the same image type in 3+ consecutive sections                                         | Media placement must be distributed — front-loading media creates structural imbalance |
| Creating an internal link plan where all links come from the same H2 section                  | Links must be distributed across sections — concentrated linking is unnatural     |

### Required Blueprint Behaviors

- Every H2 title must be specific, keyword-informed, and unique within the blueprint.
- Every content element specification must include type, location, purpose, and content specification.
- Every differentiation advantage must cite its gap evidence (N/N pages).
- Every FAQ answer must be self-contained (40–80 words) and start with a direct declarative answer.
- Meta title and description character counts must be explicitly stated and verified.
- The Blueprint Summary at the end must accurately reflect all counts from the blueprint body.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-pattern-extractor.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary structural input)                                              |
| **Receives**     | Pattern Summary, Priority Signal tables, structural profiles, media/AEO/quality patterns      |
| **Critical field** | Must-Have signal table — drives all Required Sections, Media, and Elements in Part 3       |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream intelligence source (primary intent and direction input)                            |
| **Receives**     | Dominant intent, validated funnel stage, content direction intelligence, PAA questions, SERP feature data |
| **Override authority** | serp-intelligence.skill overrides pre-execution-input.skill configuration when they conflict |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Blueprint generator → content engine (primary output consumer)                               |
| **Sends**        | Complete blueprint (all 9 parts) as the primary instruction set for content generation       |
| **Critical fields** | Structure Blueprint (Part 4), Content Requirements (Part 3), AEO Plan (Part 5), Differentiation Strategy (Part 8) |
| **Authority**    | Blueprint specifications override content engine defaults                                    |

---

### ▸ metadata-generator.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Blueprint generator → metadata generator (output consumer)                                  |
| **Sends**        | Metadata section (Part 9) — meta titles, meta descriptions, URL slugs (both options each)  |
| **Used for**     | Final metadata optimization and A/B title selection                                         |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ for dedup checks, WRITE after blueprint finalization                    |
| **Reads**        | Existing articles, assigned keywords, published content titles, existing URL slugs           |
| **Writes**       | Complete blueprint, keyword assignments (lifecycle → ASSIGNED), blueprint metadata          |
| **Memory layer** | content-memory.skill (blueprint stored), keyword-memory.skill (lifecycle update)            |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source                                                                       |
| **Receives**     | Cluster context, cross-reference relationships, pillar designation                           |
| **Used for**     | Internal linking plan construction (Phase 7A) and cluster-based link strategy               |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Pattern data and SERP intelligence recommend conflicting content types    | Step 1C finds format mismatch between two sources      | Use serp-intelligence.skill recommendation as primary. Log conflict. Note in blueprint with justification. |
| Pattern data has LOW confidence (fewer than 5 pages analyzed)            | Pattern confidence = LOW from extractor header         | Mark all Required Sections and Media as PROVISIONAL. Add note: "Low pattern confidence — verify against additional SERP research before finalizing." |
| SERP intent is classified as MIXED                                        | Intent strength = MIXED from serp-intelligence         | Build hybrid blueprint using Advanced Logic hybrid rules. Flag blueprint as MIXED INTENT — note that content must serve two reader types. |
| Dominant heading pattern is NOVEL (undefined pattern)                     | Step 5 pattern classification = NOVEL in extractor     | Build blueprint using the Pattern A structure as default (most universal). Note: "Novel SERP structure detected — Pattern A used as baseline. Monitor SERP response." |
| No PAA questions available from serp-intelligence.skill                  | PAA data is absent from SERP intelligence input        | Generate 5 FAQ questions from the semantic field of the keyword using Dimension 6 keywords from keyword-discovery.skill. Flag: "PAA data unavailable — FAQs generated from keyword semantic analysis." |
| Internal linking targets cannot be found in memory (no existing content)  | memory-controller.skill returns empty content map      | Flag internal link recommendations as PLANNED LINKS. Note which articles need to be created to enable the internal linking plan. Proceed with blueprint. |
| Memory controller unavailable for dedup checks                           | READ query returns error                               | Proceed without memory dedup. Flag: "Memory unavailable — verify H1, URL slug, and topic scope manually against existing content before publishing." |
| Word count pattern is MIXED (no dominant range)                          | Pattern extractor reports no dominant word count range | Use the average of all page midpoints as the recommended word count baseline. Apply 15% uplift. Flag: "No dominant word count pattern — baseline derived from set average." |
| Both meta title options exceed 65 characters                             | Step 9A character counting finds both options too long | Rebuild both options. Reduce by removing modifiers. Never truncate mid-word. Ensure primary keyword is preserved in the shortened version. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — HEADING PATTERN QUICK REFERENCE FOR BLUEPRINT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
BLUEPRINT SECTION ORDER BY PATTERN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PATTERN A (Definition-Led Informational):
  H2-1: What is / Definition
  H2-2: How to / Step-by-Step
  H2-3: Types / Categories / Options
  H2-4: Examples / Use Cases
  H2-5: Best Practices / Tips
  H2-6: AEO Section 1 (targeted sub-topic)
  H2-7: FAQ (AEO Section 2)

PATTERN B (Evaluation-Led Commercial):
  H2-1: Overview / Criteria
  H2-2 through H2-N: [Each item] — Review
  H2-N+1: Comparison Summary
  H2-N+2: Verdict / Recommendation
  H2-N+3: FAQ

PATTERN C (Problem-Solution):
  H2-1: The Problem with / Why [X] Fails
  H2-2: What [Solution] Is / How It Works
  H2-3: How to Implement / Step-by-Step
  H2-4: Advanced Strategies / Deep Dive
  H2-5: Real Examples / Case Studies
  H2-6: FAQ (AEO)
  H2-7: Next Steps / Action Plan

PATTERN D (Process-Led Tutorial):
  H2-1: Overview / What You'll Learn
  H2-2: Prerequisites / What You Need
  H2-3: Step 1 — [First Action]
  H2-4: Step 2 — [Second Action]
  H2-5: [Recommended Tools / Resources]
  H2-6: Common Mistakes to Avoid
  H2-7: FAQ (AEO)

HYBRID (Mixed Intent):
  H2-1: Definition / Overview (serves informational)
  H2-2: How It Works (serves informational)
  H2-3: [Topic] Options / Types (bridges both)
  H2-4: Comparison / Evaluation (serves commercial)
  H2-5: How to Choose / Criteria (serves commercial)
  H2-6: FAQ — informational + commercial questions
  H2-7: Recommendation / Verdict
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — META TITLE AND DESCRIPTION FORMULAS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
META TITLE FORMULAS BY CONTENT TYPE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Blog Article (Informational):
  [Primary Keyword]: [Specificity Modifier] Guide [Year]
  The [Complete/Full] [Primary Keyword] Guide for [Audience]

Listicle (Commercial):
  [N] Best [Primary Keyword] for [Audience] [[Year]]
  Top [N] [Primary Keyword]: Expert Picks [[Year]]

Tutorial (How-To):
  How to [Primary Keyword]: Step-by-Step [Guide/Tutorial]
  [Primary Keyword]: [N]-Step [Process/Guide] [[Year]]

Comparison (MOFU):
  [Tool A] vs [Tool B]: Which [Primary Category] Wins [[Year]]?
  [N] [Primary Keyword] Compared: Full Breakdown

CHARACTER COUNT RULE:
  Count every character including spaces.
  55 minimum. 65 maximum. No exceptions.
  If over 65: remove year first, then modifiers, never the keyword.

META DESCRIPTION FORMULA:
  [Value hook — what this article delivers] + [primary keyword]
  + [supporting detail — specific benefit] + [CTA].

  Example structure:
  "Learn [what they'll learn about primary keyword].
   [Specific benefit: e.g., 'Compare X options with
   expert scoring']. [CTA: Discover the best fit today.]"

  150 minimum. 160 maximum. No exceptions.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: serp/serp-blueprint-generator.skill*
*Nexus SEO Operating System — Version 1.0.0*
