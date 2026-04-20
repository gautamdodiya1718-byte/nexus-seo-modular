# SKILL: content/metadata-generator.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Metadata Intelligence
# EXECUTION PRIORITY: POST-BLUEPRINT — Runs after serp-blueprint-generator.skill has produced a complete content blueprint.
# POSITION: Metadata and keyword intelligence layer. Bridges content blueprint and content generation with a complete, SERP-aligned metadata package.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **metadata and keyword intelligence engine** of the Nexus content pipeline.

Metadata is not decoration. A meta title determines whether a searcher clicks. A meta description determines whether a click converts into a reader who stays. A URL slug affects how crawlers index and how users remember the page. Blog titles determine whether readers engage or bounce in the first second. Social metadata determines whether shared content gets clicked when there is no SERP context to provide intent alignment. Every one of these elements is a strategic decision — and this skill makes those decisions systematically, based on SERP intelligence, funnel context, and CTR optimization logic.

This skill does not generate metadata generically. It does not produce one "good enough" title and call the job done. It produces three distinct variants of every element — each with a different strategic angle — so that the content team and `content-generation-engine.skill` have options that reflect different optimization priorities: clarity, specificity, and differentiation. The character counts are precise. The keyword placements are deliberate. The funnel alignment is structural.

The keyword expansion module of this skill is equally important. Secondary keywords, long-tail keywords, and LSI keywords are not afterthoughts — they are the semantic coverage map that tells `content-generation-engine.skill` what to say, how to say it, and what concepts to cover to achieve topical completeness.

### Primary Responsibilities

| Responsibility                        | Description                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **SERP Intent Alignment**             | Detect the dominant search intent for the target keyword and align all metadata decisions to match                   |
| **Blog Title Generation**             | Produce three distinct blog title variants — value-focused, data-driven, and unique-angle — all aligned with intent  |
| **Meta Title Generation**             | Produce three character-counted meta title variants ≤60 characters, each optimized for CTR                          |
| **Meta Description Generation**       | Produce three character-counted meta description variants within the 150–160 character range                        |
| **URL Slug Generation**               | Produce three clean, keyword-rich URL slug options ≤5 words                                                         |
| **Social Metadata Generation**        | Produce OG Title and Twitter Title variants optimized for social context                                            |
| **Keyword Expansion**                 | Generate primary keyword, 6–8 secondary keywords, 5–7 long-tail keywords, and 10–15 LSI keywords                   |
| **Funnel Alignment Validation**       | Verify that all metadata elements reflect the correct funnel stage tone and intent                                   |
| **CTR Optimization Modeling**         | Apply click-through rate optimization logic to title selection — trigger words, specificity, number usage           |
| **SERP Pattern Matching**             | Compare generated titles against dominant SERP title patterns to ensure competitive positioning                      |
| **Deduplication Enforcement**         | Verify no title variants are semantically equivalent; no keyword appears more than intended                          |

### What This Skill Is NOT

- It is **not** a content writer. It produces metadata and keyword scaffolding — not the article body.
- It is **not** a keyword researcher. It expands and organizes keywords within the scope of the target keyword — it does not discover new keyword territories.
- It is **not** a SERP analyzer. It consumes SERP intelligence already produced — it does not query search engines.
- It is **not** optional. Every content piece produced by `content-generation-engine.skill` must have a metadata package from this skill before writing begins.
- It is **not** a copywriter producing "creative" titles. Every title must be strategically grounded — not clever for cleverness's sake.

### The Defining Standard of This Skill

> Every metadata element produced by this skill must satisfy a single test:
>
> "If a searcher saw only this element — this title, this description, this slug —
> would they know exactly what they would find on this page,
> and would they be compelled to click?"
>
> Generic phrasing fails this test. Vague descriptions fail this test.
> Clever titles that obscure the topic fail this test.
>
> The standard is: specific, honest, intent-aligned, and click-worthy.
> In that order.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                             | Required |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------|----------|
| `target_keyword`                      | The exact primary keyword phrase this content will target                                                  | YES      |
| `serp-blueprint-generator.skill`      | SEO Brief (funnel stage, content type, audience, goal, tone), keyword expansion (from blueprint Part 2), metadata draft (from blueprint Part 9), content structure outline | YES |
| `serp-intelligence.skill`             | Dominant intent classification, funnel stage validation, SERP feature data, content direction intelligence, active SERP patterns (title formats, freshness signals) | YES |
| `funnel_stage`                        | TOFU / MOFU / BOFU — validated by serp-intelligence.skill (override blueprint default if correction was issued) | YES |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                    | Used For                                                      |
|---------------------------------------|------------------------------------------------------------------------------------|---------------------------------------------------------------|
| `deep-serp-analysis.skill`            | Title patterns from top-ranking pages, character count distribution                | SERP pattern matching in Advanced Logic                      |
| `content-pattern-extractor.skill`     | Must-Have title patterns, structural patterns from top results                     | Title variant calibration                                     |
| `competitor-analysis.skill`           | Competitor title patterns, overused title structures to avoid                      | Differentiation in title variant 3                           |
| `entity-extraction.skill`             | Critical and Important entities for this cluster                                   | LSI keyword generation (Step 6)                              |
| `existing_content_titles`             | Titles of already-published content for this domain                                | Deduplication — ensuring no title repeats existing titles    |
| `domain`                              | Target site domain                                                                 | URL slug validation and domain-specific slug conventions     |

### Input Pre-Conditions

1. **Is `target_keyword` provided?** If not → halt. All metadata generation is anchored to the primary keyword.
2. **Is `serp-blueprint-generator.skill` output available?** If not → run in limited mode: generate metadata from keyword + SERP intent alone; flag as BLUEPRINT-ABSENT.
3. **Is the funnel stage validated?** If `serp-intelligence.skill` has issued a funnel correction → use the corrected stage, not the original assignment.
4. **Has metadata been generated for this keyword before?** → Query `memory-controller.skill`. If within freshness threshold (14 days) → offer cached. If stale → proceed fresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Complete Metadata Intelligence Package

```
NEXUS METADATA INTELLIGENCE PACKAGE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Keyword          : [keyword]
Funnel Stage            : [TOFU / MOFU / BOFU]
Dominant Intent         : [Informational / Commercial / Transactional]
Content Type            : [Article / Comparison / Tutorial / Guide / Listicle / etc.]
Audience                : [as defined in blueprint SEO Brief]
Tone                    : [Professional / Conversational / Technical / Persuasive]
Generated By            : content/metadata-generator.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — BLOG TITLES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OPTION 1 — CLEAR VALUE TITLE:
  Title   : [full title]
  Type    : Clear Value
  Angle   : [what makes this title work — specific mechanism]
  Keyword : [where keyword appears and how — position + form]
  CTA     : [what action or outcome this title promises]

OPTION 2 — SPECIFIC / DATA-DRIVEN TITLE:
  Title   : [full title — includes number, statistic, or quantified element]
  Type    : Specific / Data-Driven
  Angle   : [specific data element or quantification used and why]
  Keyword : [keyword position + form]
  CTA     : [outcome promised]

OPTION 3 — UNIQUE ANGLE TITLE:
  Title   : [full title — unexpected framing, contrarian angle, or distinctive perspective]
  Type    : Unique Angle
  Angle   : [what makes this framing distinctive and why it stands out in the SERP]
  Keyword : [keyword position + form]
  CTA     : [outcome promised]

RECOMMENDED: [Option 1 / 2 / 3 — with one-sentence rationale tied to SERP pattern]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — META TITLES (≤60 Characters)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OPTION 1 — KEYWORD-FORWARD:
  Meta Title    : [title]
  Character Count: [N] / 60
  Keyword At    : Position [N] (character)
  Structure     : [Primary Keyword] + [Differentiator]
  CTR Signal    : [what in this title drives clicks — named specifically]

OPTION 2 — BENEFIT-FORWARD:
  Meta Title    : [title]
  Character Count: [N] / 60
  Keyword At    : Position [N] (character)
  Structure     : [Benefit/Outcome] + [Primary Keyword]
  CTR Signal    : [CTR mechanism]

OPTION 3 — QUESTION / TRIGGER FORMAT:
  Meta Title    : [title]
  Character Count: [N] / 60
  Keyword At    : Position [N] (character)
  Structure     : [Question or trigger word] + [Keyword] + [Modifier]
  CTR Signal    : [CTR mechanism]

RECOMMENDED: [Option N — one-sentence rationale]

CHARACTER COMPLIANCE:
  All options within 60 character limit: [YES / NO — flag if any exceed]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — META DESCRIPTIONS (150–160 Characters)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OPTION 1 — BENEFIT + KEYWORD + CTA:
  Meta Description  : [description]
  Character Count   : [N] / 160
  Keyword Present   : [YES — at character position N]
  Keyword Variation : [form of keyword used]
  CTA Type          : [Learn / Discover / Find / Compare / See / Get / Start]
  Benefit Lead      : [what benefit opens the description]

OPTION 2 — PROBLEM + SOLUTION + CTA:
  Meta Description  : [description]
  Character Count   : [N] / 160
  Keyword Present   : [YES — at character position N]
  Keyword Variation : [form]
  CTA Type          : [CTA]
  Problem Lead      : [what problem opens the description]

OPTION 3 — SPECIFICITY + SOCIAL PROOF + CTA:
  Meta Description  : [description]
  Character Count   : [N] / 160
  Keyword Present   : [YES — at character position N]
  Keyword Variation : [form]
  CTA Type          : [CTA]
  Specificity Lead  : [specific data point or claim that opens the description]

RECOMMENDED: [Option N — one-sentence rationale]

CHARACTER COMPLIANCE:
  All options within 150–160 range: [YES / NO — flag if any are outside range]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — URL SLUGS (≤5 Words)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OPTION 1 — KEYWORD-DIRECT:
  Slug            : /[slug]/
  Word Count      : [N] words
  Keyword Present : YES
  Stop Words      : NONE
  Rationale       : [why this is the cleanest slug form]

OPTION 2 — KEYWORD + MODIFIER:
  Slug            : /[slug]/
  Word Count      : [N] words
  Keyword Present : YES
  Modifier        : [what the modifier adds and why]
  Stop Words      : [NONE / 1 retained — reason]

OPTION 3 — AUDIENCE / USE-CASE QUALIFIED:
  Slug            : /[slug]/
  Word Count      : [N] words
  Keyword Present : YES
  Qualifier       : [audience or use-case added]
  Stop Words      : [NONE / 1 retained — reason]

RECOMMENDED: [Option N — one-sentence rationale]

SLUG COMPLIANCE:
  All options: lowercase [YES], hyphenated [YES], ≤5 words [YES/NO — flag if any exceed]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — SOCIAL METADATA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OG TITLE (Open Graph — Facebook, LinkedIn):
  Primary   : [title — extended, engaging, benefit-clear]
  Backup    : [title — alternative framing]
  Max chars : 60 (recommended); 95 (Facebook hard limit)
  Char count: [N]
  Strategy  : [why this framing works for social context]

TWITTER TITLE (X/Twitter Card):
  Primary   : [title — punchy, shorter, direct]
  Backup    : [title — alternative]
  Max chars : 70 (recommended for display)
  Char count: [N]
  Strategy  : [why this framing works for Twitter feed context]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 6 — KEYWORD INTELLIGENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PRIMARY KEYWORD:
  Keyword     : [exact primary keyword]
  Intent      : [Informational / Commercial / Transactional]
  Funnel Stage: [TOFU / MOFU / BOFU]
  Placement   : [where it must appear — title, H1, intro, URL]

SECONDARY KEYWORDS (6–8 — semantically related, different intent angles):

| # | Secondary Keyword               | Semantic Relationship         | Intent Angle     | Placement Guidance              |
|---|---------------------------------|-------------------------------|------------------|---------------------------------|
| 1 | [keyword]                       | [how it relates to primary]   | [intent angle]   | [H2 / intro / body / conclusion]|
| 2 | [keyword]                       | [relationship]                | [intent angle]   | [placement]                     |
| 3 | [keyword]                       | [relationship]                | [intent angle]   | [placement]                     |
| 4 | [keyword]                       | [relationship]                | [intent angle]   | [placement]                     |
| 5 | [keyword]                       | [relationship]                | [intent angle]   | [placement]                     |
| 6 | [keyword]                       | [relationship]                | [intent angle]   | [placement]                     |
| 7 | [keyword]                       | [relationship]                | [intent angle]   | [placement]                     |
| 8 | [keyword] — optional            | [relationship]                | [intent angle]   | [placement]                     |

LONG-TAIL KEYWORDS (5–7 — specific queries the content should serve):

| # | Long-Tail Keyword                                      | Specificity Type        | Search Context                    |
|---|--------------------------------------------------------|-------------------------|-----------------------------------|
| 1 | [full long-tail keyword phrase]                        | [audience / use-case / comparison / question] | [who searches this and when] |
| 2 | [keyword]                                              | [type]                  | [context]                        |
| 3 | [keyword]                                              | [type]                  | [context]                        |
| 4 | [keyword]                                              | [type]                  | [context]                        |
| 5 | [keyword]                                              | [type]                  | [context]                        |
| 6 | [keyword]                                              | [type]                  | [context]                        |
| 7 | [keyword] — optional                                   | [type]                  | [context]                        |

LSI KEYWORDS (10–15 — entity-based and concept-based semantic coverage):

Entity-Based LSI:
  [entity keyword], [entity keyword], [entity keyword], [entity keyword], [entity keyword]

Concept-Based LSI:
  [concept keyword], [concept keyword], [concept keyword], [concept keyword], [concept keyword]

Process/Method LSI:
  [process keyword], [process keyword], [process keyword]

Measurement/Metric LSI:
  [metric keyword], [metric keyword]

Total LSI Count: [N] — target: 10–15

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 7 — FUNNEL ALIGNMENT VALIDATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Funnel Stage Validated      : [TOFU / MOFU / BOFU]
Tone Compliance Check       :
  Blog titles reflect [educational/comparative/conversion] tone : [YES / FLAG]
  Meta descriptions avoid [misaligned CTA type for stage]       : [YES / FLAG]
  URL slug reflects [appropriate complexity for stage]           : [YES / FLAG]
  Keywords cover [appropriate intent mix for stage]              : [YES / FLAG]
  Social metadata reflects [appropriate hook for stage]          : [YES / FLAG]

Overall Funnel Alignment    : [ALIGNED / NEEDS REVIEW — specific flag if not aligned]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : content-generation-engine.skill
                         content-inventory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — INTENT ALIGNMENT

Before generating any metadata element, establish the intent model that every subsequent element must comply with. All metadata decisions — title word choice, meta description structure, CTA type, slug length, social framing — are governed by the intent model established in this step.

**Phase 1A — Intent Detection**

Load intent signals from the available inputs in priority order:

| Priority | Source                                | Intent Signal                                            |
|----------|---------------------------------------|----------------------------------------------------------|
| 1        | `serp-intelligence.skill`             | Dominant intent classification (most reliable)          |
| 2        | Blueprint SEO Brief (funnel stage)    | Funnel stage provides coarse intent alignment            |
| 3        | Keyword pattern inference             | Keyword structural signals (how to / best / buy / etc.) |

**Priority rule:** If `serp-intelligence.skill` has issued a funnel stage correction that overrides the blueprint's funnel assignment → use the corrected stage everywhere. All subsequent metadata must reflect the corrected stage.

**Phase 1B — Intent-Specific Metadata Model**

Apply the correct metadata model for the detected intent:

**INFORMATIONAL INTENT MODEL (TOFU):**

| Element          | Guiding Principle                                                                         |
|------------------|-------------------------------------------------------------------------------------------|
| Blog Title       | Clarity first — reader must know exactly what they will learn. Educational framing.       |
| Meta Title       | Educational trigger words: "What Is", "How to", "Guide to", "Understanding", "Complete"  |
| Meta Description | Lead with the learning outcome: "Discover how...", "Learn exactly...", "Find out..."     |
| CTA Type         | Soft: "Learn", "Discover", "Understand", "Find out", "Explore"                           |
| URL Slug         | Concept-first: the subject of learning is the lead element                               |
| Social           | Lead with curiosity or surprising insight hook                                            |

**COMMERCIAL INTENT MODEL (MOFU):**

| Element          | Guiding Principle                                                                         |
|------------------|-------------------------------------------------------------------------------------------|
| Blog Title       | Comparison framing, specificity signals, and evaluation triggers                          |
| Meta Title       | Evaluation trigger words: "Best", "vs", "Comparison", "Review", "Top [N]", "Alternatives" |
| Meta Description | Lead with differentiation: "Compare [N] options...", "See which [X] wins..."             |
| CTA Type         | Evaluation: "Compare", "See", "Find out which", "Decide", "Choose"                      |
| URL Slug         | Comparison-first or evaluation keyword: "best-X", "X-vs-Y", "X-comparison"             |
| Social           | Lead with comparison hook or decision-helping frame                                       |

**TRANSACTIONAL INTENT MODEL (BOFU):**

| Element          | Guiding Principle                                                                         |
|------------------|-------------------------------------------------------------------------------------------|
| Blog Title       | Action and outcome framing — what happens when the reader takes action                   |
| Meta Title       | Action trigger words: "Get Started", "Free", "How to Get", "Start", "Try"                |
| Meta Description | Lead with outcome: "Start [doing X] in [time]...", "Get [specific result]..."            |
| CTA Type         | Action: "Get started", "Try free", "Start", "Access", "Join", "Download"                |
| URL Slug         | Action or outcome focused: "get-X", "start-X", "X-free-trial"                          |
| Social           | Lead with immediate benefit and low-friction action signal                                |

**Phase 1C — SERP Pattern Context**

If `deep-serp-analysis.skill` data is available, load the dominant title patterns from top-ranking pages:
- What trigger words appear most in titles at positions 1–5?
- What content types appear most frequently?
- What differentiates top-3 titles from positions 6–10?

This pattern context informs:
- Which patterns to match (for SERP alignment).
- Which patterns to avoid (for differentiation in Option 3 titles).

---

### ▸ STEP 2 — BLOG TITLE GENERATION

Generate three distinct blog title variants. Each variant serves a different strategic purpose. They must be genuinely different — not the same structure with different words.

**The Three Blog Title Types:**

---

**TITLE TYPE 1 — CLEAR VALUE TITLE**

**Purpose:** The clearest possible expression of what the reader gets from this content. No cleverness. No abstraction. The reader knows immediately: "This is exactly what I need."

**Construction Rules:**
1. Primary keyword must appear — exact match or close variation.
2. The outcome or value must be stated explicitly — not implied.
3. Maximum 70 words (for headline display purposes).
4. No ambiguous pronouns ("this," "it," "these") as the opening word.
5. No clickbait qualifiers ("You Won't Believe," "Shocking," "Secret").
6. Must match the funnel stage tone (from Phase 1B).

**Clear Value Title Formula by Funnel Stage:**

| Funnel Stage | Formula                                                                    | Example                                                            |
|--------------|----------------------------------------------------------------------------|--------------------------------------------------------------------|
| TOFU         | What [concept] Is + [Benefit or scope of coverage]                        | "What Is CRM Software and How Does It Help Sales Teams?"          |
| TOFU         | How to [action] + [Specific Outcome]                                       | "How to Choose a CRM: A Step-by-Step Evaluation Framework"        |
| MOFU         | [N] Best [Category] for [Audience] + [Year or context]                    | "7 Best CRM Software Options for Small Business Teams in 2024"    |
| MOFU         | [Tool A] vs [Tool B]: [Specific Evaluation Angle]                          | "HubSpot vs Salesforce: An Honest Comparison for Growing Startups"|
| BOFU         | How to [Action] with [Tool/Method]: [Specific Outcome]                     | "How to Set Up HubSpot CRM in 30 Minutes: Complete Setup Guide"   |

---

**TITLE TYPE 2 — SPECIFIC / DATA-DRIVEN TITLE**

**Purpose:** Uses a specific number, data point, time estimate, quantity, or measurable outcome to signal credibility and specificity. Data in titles consistently improves CTR because they signal that the content is more thorough than a generic piece.

**Construction Rules:**
1. Must include a number or measurable element — not a vague quantity.
2. The number must be credible — do not manufacture statistics that the content cannot support.
3. Primary keyword must be present.
4. The data element must be meaningful — "10 Tips" is less compelling than "10 CRM Setup Mistakes That Cost Teams 3+ Hours a Week."
5. The specificity must be honest — if the number is approximate, make it clearly approximate.

**Acceptable Data Types for Type 2 Titles:**

| Data Type               | Example Usage                                                             |
|-------------------------|---------------------------------------------------------------------------|
| List count              | "7 CRM Software Options That Actually Work for Agencies"                 |
| Time estimate           | "How to Implement a CRM in 2 Weeks (Without Disrupting Your Team)"       |
| Percentage or stat      | "Why 67% of CRM Implementations Fail (And How to Avoid It)"             |
| Year + recency signal   | "Best CRM Software for 2024: Tested and Ranked"                         |
| Comparison count        | "CRM Comparison: 9 Tools Evaluated on 12 Key Criteria"                  |
| Cost/price data         | "CRM Software Under $30/Month: 5 Options That Actually Deliver"          |

**Prohibited data types:**
- Round numbers that feel invented ("100 Ways to...")
- Exaggerated statistics without source context ("The 1 CRM Feature That Changes Everything")
- Superlatives without quantification ("The Most Powerful CRM Guide Ever Written")

---

**TITLE TYPE 3 — UNIQUE ANGLE TITLE**

**Purpose:** Takes an unexpected, contrarian, specific-perspective, or distinctively framed approach to the topic — one that stands out from the dominant title pattern in the SERP. This title is for differentiation, not for clarity alone.

**Construction Rules:**
1. Must not follow the same structural pattern as the dominant SERP titles (identified from SERP pattern context in Step 1C).
2. Must still include the primary keyword or a clear semantic variant.
3. The uniqueness must be genuine — a real editorial angle, not a fabricated controversy.
4. Must not mislead about content scope or promise more than the content delivers.
5. Works best for TOFU and MOFU content — BOFU content typically benefits more from clarity than uniqueness.

**Unique Angle Strategies:**

| Strategy                  | Description                                                                           | Example                                                                     |
|---------------------------|---------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Contrarian framing        | Challenges the dominant advice or common assumption in the niche                     | "Why Most CRM Implementations Fail (Hint: It's Not the Software)"          |
| Problem-first framing     | Leads with the reader's pain point rather than the solution                          | "CRM Software Is Supposed to Save Time. Here's Why It Usually Doesn't"     |
| Audience-specific angle   | Targets a specific under-served sub-audience the SERP generally ignores             | "CRM for Freelancers: What Solo Operators Actually Need (It's Less Than You Think)" |
| Process-reveal framing    | Promises to reveal how something actually works vs. how it's marketed               | "What CRM Vendors Don't Tell You About Setup Time and Actual Adoption"     |
| Comparison flip           | Compares in the reverse direction from what the SERP typically offers               | "I Tried 6 CRM Tools for 6 Months. Here's What Actually Changed My Process" |
| Narrative/story framing   | Uses a real or hypothetical story as the entry point                                 | "We Tested 7 CRMs With a 20-Person Sales Team. The Winner Surprised Everyone"|

---

**Phase 2A — Title Generation Protocol**

For each of the three title types:
1. Load the intent model from Step 1B.
2. Apply the type-specific construction rules.
3. Draft 2–3 candidates per type.
4. Select the strongest candidate per type based on:
   - Keyword presence and natural placement.
   - Specificity and clarity of the promised value.
   - Differentiation from the other two title types.
   - SERP pattern context fit (for Type 1 and 2) or deliberate deviation (for Type 3).
5. Document the angle, keyword placement, and CTA for each selected title.

**Phase 2B — Title Compliance Check**

After all three titles are generated:

| Check                                               | Pass Condition                                                    |
|-----------------------------------------------------|-------------------------------------------------------------------|
| Primary keyword present in all three titles         | Exact or semantically close variation — not omitted              |
| All three titles are structurally distinct          | No two titles use the same opening word OR same structural template |
| No clickbait elements                               | No "You Won't Believe," "Shocking," "This Changed Everything"    |
| All titles promise something the content delivers   | No overpromising — if title says "complete guide" → content must be complete |
| Character count ≤ 70 (blog title display range)     | Warn if any title exceeds 70 characters                          |
| Funnel alignment verified                           | Title tone matches the intent model from Step 1B                 |

---

### ▸ STEP 3 — META TITLE GENERATION

Generate three meta title variants. Meta titles are the text displayed in Google's search result headers — they are limited to 60 characters and must contain the primary keyword near the start.

**Meta Title Rules (Non-Negotiable):**
1. Maximum 60 characters — strictly enforced. Titles exceeding 60 characters are truncated by Google and lose click-driving value.
2. Primary keyword must appear within the first 35 characters where possible (keyword-early placement improves CTR).
3. Natural phrasing — the title must read as a human-written headline, not as keyword stuffing.
4. Every meta title must include at least one CTR trigger element (see Phase 3A).
5. Meta titles must be distinct from each other — not just reformulations of the same sentence.
6. Meta title ≠ Blog title — the meta title is shorter and more direct; it serves the SERP context specifically.

**Phase 3A — CTR Trigger Elements**

Every meta title must contain one of the following CTR trigger categories:

| Trigger Category         | Examples                                                          | Best For Funnel Stage    |
|--------------------------|-------------------------------------------------------------------|--------------------------|
| **Number/List trigger**  | "7 Best", "Top 5", "9 Ways", "3 Steps"                          | MOFU primarily; TOFU     |
| **Year/Freshness**       | "[2024]", "Updated 2024", "2024 Guide"                           | MOFU, BOFU               |
| **Power word**           | "Complete", "Essential", "Proven", "Definitive", "Ultimate"     | TOFU, MOFU               |
| **Question trigger**     | Starts with "What Is", "How to", "Why", "When to"               | TOFU primarily           |
| **Specificity trigger**  | Named tool, named audience, named outcome, named time           | All stages               |
| **Free/Easy trigger**    | "Free", "Without [pain]", "Easy", "Simple"                      | BOFU, TOFU               |

**Phase 3B — Three Meta Title Structures**

**META TITLE OPTION 1 — KEYWORD-FORWARD STRUCTURE:**

`[Primary Keyword] + [Differentiator/Modifier]`

The keyword leads immediately, establishing relevance in the first words. The differentiator adds specificity.

| Funnel | Example                                         | Chars |
|--------|-------------------------------------------------|-------|
| TOFU   | CRM Software: Complete Beginner's Guide 2024    | 45    |
| MOFU   | Best CRM Software for Small Teams (2024)        | 42    |
| BOFU   | CRM Software Free Trial: Top 5 Options          | 43    |

**META TITLE OPTION 2 — BENEFIT-FORWARD STRUCTURE:**

`[Benefit/Outcome] + [Primary Keyword or Variant]`

The benefit leads, establishing value before the keyword. Works well when the keyword alone is not compelling.

| Funnel | Example                                         | Chars |
|--------|-------------------------------------------------|-------|
| TOFU   | Master CRM Software: Step-by-Step Guide         | 42    |
| MOFU   | Find the Best CRM: Honest Comparison 2024       | 45    |
| BOFU   | Start Selling Smarter with the Right CRM        | 43    |

**META TITLE OPTION 3 — QUESTION OR TRIGGER FORMAT:**

`[Question or strong trigger word] + [Keyword] + [Resolution hint]`

Opens with a curiosity or evaluation trigger — works especially well for TOFU intent where the reader is in discovery mode.

| Funnel | Example                                         | Chars |
|--------|-------------------------------------------------|-------|
| TOFU   | What Is CRM Software? A Plain-English Guide     | 45    |
| MOFU   | Which CRM Software Is Best for You? 2024        | 44    |
| BOFU   | Ready for a CRM? 7 Options to Start Today       | 45    |

**Phase 3C — Character Count Enforcement**

For every meta title generated:
1. Count every character including spaces.
2. Record the count.
3. Flag any title exceeding 60 characters as EXCEEDS LIMIT.
4. If a title exceeds 60 characters → shorten by:
   - Removing the year if present (saves 5–7 characters).
   - Replacing a long word with a shorter synonym.
   - Removing the weakest modifier.
   - Never removing the primary keyword or the CTR trigger element.

---

### ▸ STEP 4 — META DESCRIPTION GENERATION

Generate three meta description variants. Meta descriptions are the text shown beneath the meta title in the search result. They are 150–160 characters and must be benefit-driven with a clear call to action and keyword variation.

**Meta Description Rules (Non-Negotiable):**
1. Length must be 150–160 characters — measured precisely. Shorter = opportunity wasted; longer = truncated in SERP.
2. Primary keyword or a natural variation must appear — preferably in the first 80 characters.
3. Must include a CTA — aligned with the funnel stage intent model from Step 1B.
4. Must lead with a specific benefit, problem, or claim — not with "This article..." or "In this guide..."
5. No keyword stuffing — the keyword appears once, naturally.
6. Each variant must open differently and use a different CTA.

**Phase 4A — Prohibited Meta Description Openers**

The following opening patterns produce generic, CTR-dead meta descriptions. Never use them:

| Prohibited Opener                           | Why It Fails                                               |
|---------------------------------------------|------------------------------------------------------------|
| "This article covers..."                    | Self-referential; no benefit signal; reader-repelling      |
| "In this guide, you'll learn..."            | Weak — every guide says this; zero differentiation        |
| "Learn everything about [topic]..."         | Vague; doesn't specify what "everything" means            |
| "Looking for [keyword]?"                    | Passive; restates the search; no new information           |
| "Are you trying to..."                      | Presumptuous question; low credibility signal              |
| "[Website name] has the best..."            | Self-promotional opener; low trust signal                  |
| "Click here to find out..."                 | The CTA shouldn't be the opener — the benefit should be   |

**Phase 4B — Three Meta Description Structures**

**META DESCRIPTION OPTION 1 — BENEFIT + KEYWORD + CTA:**

Structure: Open with a specific benefit → introduce keyword → close with action-oriented CTA.

Formula: `"[Specific benefit statement]. [Keyword variation + supporting detail]. [CTA verb] today."`

| Funnel | Example (character count)                                                                                                 | Chars |
|--------|---------------------------------------------------------------------------------------------------------------------------|-------|
| TOFU   | "Stop guessing which CRM software fits your team. This complete guide covers features, setup, and what to avoid. Start learning." | 153 |
| MOFU   | "Not all CRM software is created equal. Compare 9 top options on features, pricing, and ease of use — find your match."  | 118 (needs expansion to 150) |
| BOFU   | "Get CRM software running for your team this week. 5 tools with free trials, clear setup guides, and no hidden fees. See them." | 151 |

**META DESCRIPTION OPTION 2 — PROBLEM + SOLUTION + CTA:**

Structure: Open with the reader's specific problem → position the content as the solution → close with CTA.

Formula: `"[Problem statement the reader recognizes]. [Content solves this by: specific approach]. [CTA]."`

| Funnel | Example                                                                                                                           | Chars |
|--------|-----------------------------------------------------------------------------------------------------------------------------------|-------|
| TOFU   | "Most CRM guides are written for IT teams, not business owners. This breakdown explains CRM software in plain terms. Explore it." | 154  |
| MOFU   | "Choosing CRM software with 50+ options available is overwhelming. We cut it to the 7 best for small teams — with honest scores." | 156 |
| BOFU   | "Most free CRM tools hide costs in features you actually need. Here are 5 genuinely free CRM options that work. Compare them now."| 157 |

**META DESCRIPTION OPTION 3 — SPECIFICITY + SOCIAL PROOF + CTA:**

Structure: Open with a specific, credibility-establishing claim → reinforce with detail → close with CTA.

Formula: `"[Specific credibility claim — number, methodology, or unique angle]. [Reinforcing detail]. [CTA]."`

| Funnel | Example                                                                                                                           | Chars |
|--------|-----------------------------------------------------------------------------------------------------------------------------------|-------|
| TOFU   | "Built from 40+ hours of CRM research: a complete guide to CRM software covering types, use cases, and implementation steps."    | 152  |
| MOFU   | "Tested 12 CRM software tools with a real sales team for 6 weeks. Here's the full comparison by price, features, and support."  | 155  |
| BOFU   | "7 CRM tools, 4 weeks of testing, one clear answer for small teams. See our full evaluation and start your free trial today."   | 152  |

**Phase 4C — Character Count Enforcement**

For every meta description generated:
1. Count every character including spaces.
2. Record the count.
3. Flag any description below 150 as TOO SHORT — missing CTR opportunity.
4. Flag any description above 160 as EXCEEDS LIMIT — will be truncated.
5. If a description is outside range → adjust by:
   - Too short: add a secondary benefit clause OR expand the supporting detail.
   - Too long: cut the weakest modifier, replace verbose phrase with concise equivalent, or shorten the CTA.

---

### ▸ STEP 5 — URL SLUG GENERATION

Generate three URL slug options. The slug is the path component of the URL — it affects crawlability, keyword relevance signals, and user trust when the URL is displayed in the SERP.

**URL Slug Rules (Non-Negotiable):**
1. Lowercase only — no uppercase characters.
2. Hyphens between words — no underscores, no spaces, no special characters.
3. Maximum 5 words (after removing stop words).
4. Primary keyword or its core concept must be present.
5. No stop words unless their removal makes the slug meaningless or misleading.
6. No dates in the slug (date-based slugs age and cannot be updated without 301 redirect).
7. No keyword stuffing — the slug is readable as a clean path, not a keyword list.

**Phase 5A — Stop Word Rules**

Remove these stop words by default:

`a, an, the, is, in, on, at, of, for, to, and, or, but, with, by, from, as, into, that, this, these, those, has, have, had, be, been, being, will, was, were, are, do, does, did`

**Retain a stop word ONLY when:**
- Removing it creates an ambiguous or misleading slug: `"how-to-use-crm"` → removing "to" → `"how-use-crm"` (ambiguous) → retain "to."
- The word is part of a named entity or brand name.
- The word is essential for readability in a 2-word slug.

**Phase 5B — Three Slug Structures**

**SLUG OPTION 1 — KEYWORD-DIRECT:**

Uses the primary keyword words with stop words removed. The cleanest, most direct expression.

| Example keyword            | Slug Option 1                     |
|----------------------------|-----------------------------------|
| "best crm software"        | `/best-crm-software/`            |
| "how to choose a crm"      | `/how-to-choose-crm/`            |
| "crm software for startups"| `/crm-software-startups/`        |

**SLUG OPTION 2 — KEYWORD + MODIFIER:**

Adds a specific modifier that increases topical precision and may capture long-tail variants.

| Example keyword            | Slug Option 2                     | Modifier Added         |
|----------------------------|-----------------------------------|------------------------|
| "best crm software"        | `/best-crm-software-guide/`      | "guide" — signals comprehensive resource |
| "how to choose a crm"      | `/how-to-choose-crm-2024/` (if evergreen freshness is a signal) | Year modifier |
| "crm software for startups"| `/crm-software-small-startups/`  | Specificity modifier   |

Note: Year modifiers in slugs are generally discouraged because they require 301 redirects when updated. Use only when freshness is a critical SERP signal for this keyword.

**SLUG OPTION 3 — AUDIENCE OR USE-CASE QUALIFIED:**

Narrows the slug to the specific audience or use case the content serves — useful for highly targeted content.

| Example keyword            | Slug Option 3                                 | Qualifier Type      |
|----------------------------|-----------------------------------------------|---------------------|
| "best crm software"        | `/best-crm-software-small-teams/`            | Audience qualifier  |
| "how to choose a crm"      | `/choose-crm-sales-team/`                    | Use-case qualifier  |
| "crm software for startups"| `/crm-startups-early-stage/`                 | Stage qualifier     |

**Phase 5C — Slug Compliance Check**

| Check                                      | Pass Condition                                           |
|--------------------------------------------|----------------------------------------------------------|
| All lowercase                              | YES for all three options                                |
| Hyphens between words (no underscores)    | YES for all three options                                |
| ≤ 5 words after stop word removal         | YES — flag and shorten if any option exceeds 5 words    |
| Primary keyword concept present            | YES — the keyword's core concept must be recognizable   |
| No dates unless justified                  | Flag if any option includes a year without justification |
| No repetition of words                     | "crm-crm-software" → prohibited                         |

---

### ▸ STEP 6 — KEYWORD EXPANSION

Generate the complete keyword intelligence set: primary keyword confirmation, secondary keywords (6–8), long-tail keywords (5–7), and LSI keywords (10–15).

**Phase 6A — Primary Keyword Confirmation**

Confirm the primary keyword from the input. Do not alter it. Document its intent, funnel stage, and required placements:

- **H1:** Primary keyword must appear in the H1 heading.
- **Meta title:** Must appear in the meta title (verified in Step 3).
- **URL slug:** Must be core to the slug (verified in Step 5).
- **First 100 words of body:** Primary keyword must appear within the first 100 words.
- **Intro paragraph:** Ideally appears in the first sentence or second sentence.

**Phase 6B — Secondary Keyword Generation (6–8)**

Secondary keywords are semantically related to the primary keyword but cover different intent angles, different audience segments, or different sub-topics within the same content piece.

**Secondary Keyword Selection Rules:**
1. No secondary keyword may be the same as the primary keyword (exact or near-exact match).
2. Each secondary keyword must have a distinct intent angle from the primary.
3. Each secondary keyword must have a designated placement — it must be planned for a specific H2, H3, or content zone.
4. Secondary keywords must cover the semantic neighborhood of the primary — the concepts, tools, processes, and outcomes that a comprehensive treatment of the topic would naturally address.
5. At least one secondary keyword should represent a different funnel stage angle (e.g., if primary is MOFU, include one BOFU-leaning secondary and one TOFU-leaning secondary).

**Secondary Keyword Generation by Dimension:**

Derive secondary keywords from these dimensions:

| Dimension                          | Method                                                                 |
|------------------------------------|------------------------------------------------------------------------|
| Audience modifier                  | Primary keyword + specific audience segment not in primary keyword    |
| Use-case modifier                  | Primary keyword + specific use case or application                    |
| Comparison angle                   | Primary keyword in a "vs" or comparison framing                       |
| Process angle                      | Primary keyword in an action/how-to framing                           |
| Evaluation angle                   | Primary keyword in a selection or criteria framing                    |
| Outcome angle                      | Primary keyword framed around the result or benefit                   |
| Industry-specific angle            | Primary keyword qualified for a specific industry                      |
| Feature/attribute angle            | Primary keyword + specific feature or capability                      |

**Phase 6C — Long-Tail Keyword Generation (5–7)**

Long-tail keywords are highly specific, multi-word queries (typically 4+ words) that represent the specific sub-questions a reader might search while in the same content journey as the primary keyword.

**Long-Tail Keyword Selection Rules:**
1. Every long-tail must be a complete search query — phrased as a reader would type it.
2. Long-tails must represent genuinely distinct search intents — not padding variations of the same query.
3. Each long-tail must be assignable to a specific section of the content (an H2 or H3 that addresses that specific question).
4. Cover at least three of these long-tail types:
   - **Question-based:** "what is [concept]," "how does [process] work," "why is [X] important"
   - **Audience-qualified:** "[concept] for [specific audience type]"
   - **Use-case specific:** "[concept] for [specific application]"
   - **Comparison-specific:** "[option A] vs [option B] for [specific context]"
   - **Constraint-qualified:** "[concept] [price/time/size constraint]"

**Phase 6D — LSI Keyword Generation (10–15)**

LSI (Latent Semantic Indexing) keywords are the conceptually related terms that appear naturally in genuinely comprehensive coverage of the primary keyword's topic. They signal topical completeness to search engines.

**LSI Keyword Generation by Type:**

**Entity-Based LSI (3–5 keywords):**
Named tools, platforms, people, organizations, or standards that belong in the semantic field of the topic.
Source: Entity map from `entity-extraction.skill` if available; otherwise derive from topic knowledge.

**Concept-Based LSI (3–5 keywords):**
Foundational concepts, processes, or methodologies that the topic area involves. Not the primary keyword's sub-topics — its conceptual infrastructure.

**Process/Method LSI (2–3 keywords):**
Named processes, workflows, or methods that the topic requires or produces.

**Measurement/Metric LSI (1–3 keywords):**
KPIs, metrics, scores, or outcomes associated with the topic area.

**LSI Quality Rules:**
1. No LSI keyword may be the same as a primary, secondary, or long-tail keyword.
2. LSI keywords are unlabeled in content — they are woven naturally into prose, not forced as visible keyword targets.
3. A keyword too generic to be distinctively associated with this topic (e.g., "software," "business," "team") is not a valid LSI keyword.

**Phase 6E — Keyword Set Deduplication**

After all keyword tiers are generated:
1. Verify no keyword appears in more than one tier.
2. Verify no two keywords in any tier are semantic duplicates (≥80% semantic overlap).
3. Remove any keyword that fails either check — log the removal.

---

### ▸ STEP 7 — SOCIAL METADATA GENERATION

Generate OG Title and Twitter Title optimized for social sharing contexts — where the SERP intent model is absent and social feed context governs whether someone clicks.

**Phase 7A — OG Title (Open Graph — Facebook, LinkedIn)**

Open Graph titles appear when content is shared on Facebook, LinkedIn, Slack, and most messaging apps. They display alongside the OG image, which means the title does not need to carry the full context burden.

**OG Title Rules:**
1. Recommended length: 40–60 characters for optimal display. Hard Facebook limit: 95 characters.
2. Must include primary keyword or a strong semantic variant.
3. Should be more engaging and benefit-forward than the meta title — social audiences have lower information-seeking intent and higher scroll-through behavior than SERP searchers.
4. Use more vivid language than the meta title — but never misleading.
5. The OG Title and meta title should be distinct but cover the same topic.

**OG Title Intent Modifiers:**

| Social Platform Context        | OG Title Adjustment                                                      |
|--------------------------------|--------------------------------------------------------------------------|
| LinkedIn (professional)        | Professional framing, outcome-focused, credibility signals              |
| Facebook (broader audience)    | Benefit-clear, relatable framing, slightly more casual                  |
| Slack/Messaging (team sharing) | Descriptive, immediately clear topic and value                          |

Produce two OG Title options: one that matches LinkedIn professional tone, one that works for broader social.

**Phase 7B — Twitter Title (X/Twitter Card)**

Twitter titles appear in Twitter card previews. The environment is fast-paced, highly competitive for attention, and character-constrained.

**Twitter Title Rules:**
1. Recommended length: 50–70 characters for display clarity.
2. Must be punchier and more direct than the OG title — Twitter readers scroll faster.
3. Use strong action words, numbers, or contrast statements.
4. Avoid full questions — statements with an implied question perform better.
5. Must include the primary keyword or a strong semantic variant.

**Twitter Title vs. Meta Title Distinction:**

| Element        | Context                       | Tone            | Purpose                         |
|----------------|-------------------------------|-----------------|----------------------------------|
| Meta Title     | SERP — high intent searcher   | Direct, keyword-forward | Get the click from searcher |
| Twitter Title  | Social feed — low intent scroller | Punchy, provocative | Stop the scroll, trigger curiosity |

Produce two Twitter Title options — one clear and direct, one with a hook or contrast element.

---

### ▸ STEP 8 — FUNNEL ALIGNMENT VALIDATION

Verify that every metadata element produced across Steps 2–7 is consistent with the validated funnel stage and intent model.

**Phase 8A — Funnel Compliance Matrix**

Apply this compliance matrix across all generated metadata:

| Element               | TOFU Compliance                                        | MOFU Compliance                                       | BOFU Compliance                                      |
|-----------------------|--------------------------------------------------------|-------------------------------------------------------|------------------------------------------------------|
| Blog Title tone       | Educational, curiosity-building, learning-outcome framing | Comparison, evaluation, selection framing         | Action, outcome, results framing                    |
| Meta Title triggers   | "What Is", "How to", "Guide", "Complete"              | "Best", "vs", "Comparison", "Top [N]"                | "Free", "Start", "Get", "Try"                        |
| Meta Description CTA  | "Learn", "Discover", "Find out", "Explore"            | "Compare", "See which", "Evaluate", "Choose"          | "Get started", "Try free", "Start today"             |
| URL Slug pattern      | Concept-led ("/what-is-crm-software/")                | Evaluation-led ("/best-crm-software-comparison/")    | Action-led ("/get-started-crm-free/")               |
| Keyword tone balance  | Informational intent dominant                          | Commercial intent dominant                            | Transactional intent dominant                        |
| Social hook type      | Educational revelation / surprising insight           | Comparison / decision-help frame                     | Immediate benefit / low-friction action frame        |

**Phase 8B — Funnel Misalignment Detection**

When any element is flagged as misaligned:

| Misalignment Detected                                                       | Correction Action                                                      |
|-----------------------------------------------------------------------------|------------------------------------------------------------------------|
| BOFU blog title with educational framing (TOFU tone)                       | Rewrite title to use action/outcome framing — not educational tone     |
| TOFU meta description with "Get started" CTA (BOFU CTA)                   | Replace CTA with "Learn", "Discover", or "Find out" — educational CTA |
| MOFU URL slug with educational pattern ("/what-is-X/")                     | Revise slug to use comparison pattern ("/best-X-comparison/")         |
| LSI keywords dominated by transaction-intent terms in TOFU content         | Replace transactional LSI keywords with informational concept terms    |

**Phase 8C — Funnel Alignment Report**

Produce a one-line compliance status for each element, culminating in an overall alignment verdict:

- **ALIGNED:** All elements are consistent with the validated funnel stage.
- **NEEDS REVIEW:** 1–2 elements have misalignment flags; identified and correctable.
- **MISALIGNED:** Multiple elements conflict with the funnel stage; metadata package should be regenerated with corrected intent model.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CTR OPTIMIZATION MODELING

Beyond the basic CTR trigger elements in Step 3A, this skill applies a more nuanced CTR optimization model based on observed search behavior patterns.

**CTR Factors by Title Element Position:**

| Position in Meta Title    | CTR Contribution                                                         |
|---------------------------|--------------------------------------------------------------------------|
| Characters 1–15           | Highest CTR contribution — what appears before a mobile screen truncation |
| Characters 16–35          | Strong CTR zone — keyword placement target                               |
| Characters 36–55          | Secondary CTR zone — modifier and differentiator placement               |
| Characters 56–60          | Trailing zone — closing word, often year or "guide" / "review"           |

**Optimal Keyword Placement Model:**

For informational queries: keyword ideally in characters 1–20.
For commercial queries: benefit or list number ideally in characters 1–15; keyword in 16–30.
For transactional queries: action word in characters 1–10; keyword in 11–30.

**CTR Multiplier Elements (Apply in order of impact):**

| Element                    | Average CTR Impact (observed across industry) | Application Rule                        |
|----------------------------|-----------------------------------------------|-----------------------------------------|
| Number in title            | +35% average CTR lift                         | Use when content is genuinely a list   |
| Year/freshness signal      | +20% for time-sensitive queries              | Use when SERP rewards freshness        |
| Question format            | +15% for informational queries               | Use Option 3 title for maximum effect  |
| Brackets/parentheses       | +10–15% (signals bonus content)             | Use sparingly — e.g., "[2024 Update]"  |
| Power words                | +5–10% when used authentically               | "Complete," "Proven," "Essential"      |
| First-person perspective   | +10% for reviews/experience-based content    | "I Tested 7 CRMs — Here's What Worked"|

---

### ▸ SERP TITLE PATTERN MATCHING

When `deep-serp-analysis.skill` data is available, apply competitive title pattern analysis to calibrate the three blog title variants.

**Pattern Matching Protocol:**

1. Load the titles of the top 5 ranking pages for the target keyword.
2. Identify the dominant structural pattern:
   - What trigger word appears most frequently? ("Best", "How to", "What Is", number)
   - What title length dominates? (short ≤50 chars vs. medium 50–70 chars)
   - What differentiators appear in titles that perform well vs. poorly?

3. Apply pattern decisions:
   - **Title Option 1 (Clear Value):** Match the dominant pattern's structural elements — be in the conversation.
   - **Title Option 2 (Specific/Data):** Apply the dominant pattern + add a specific number or data element the SERP currently lacks.
   - **Title Option 3 (Unique Angle):** Deliberately deviate from the dominant pattern — use a structure that is absent from the top 5 titles.

4. Flag if the dominant SERP pattern is already very saturated (all top-5 titles use the exact same structure) → Title Option 3 receives higher recommended weight as differentiation is more important.

---

### ▸ KEYWORD POSITIONING LOGIC

When distributing keywords across the content, apply the following positioning framework:

**Primary Keyword Placement Hierarchy:**

| Position              | Priority | Rule                                                           |
|-----------------------|----------|----------------------------------------------------------------|
| URL slug              | 1        | Must appear — core concept only (no stop words)               |
| H1 heading            | 2        | Must appear — exact match or close variation                  |
| Meta title            | 3        | Must appear — within first 35 characters                      |
| First 100 words       | 4        | Must appear — ideally in first 2 sentences                    |
| Meta description      | 5        | Must appear — natural variation acceptable                    |
| 2–3 H2 headings       | 6        | Secondary and long-tail keywords drive H2 headings            |
| Body text frequency   | 7        | Approximately once per 200–300 words — natural density        |

**Secondary Keyword Distribution:**

Map each secondary keyword to a specific H2 section. No two secondary keywords share the same target section (to avoid within-section keyword concentration):

| Secondary Keyword     | Target Section                            | Natural Density Target    |
|-----------------------|-------------------------------------------|---------------------------|
| [KW 1]                | Introduction                              | 1–2 natural appearances   |
| [KW 2]                | H2 Section 1                             | 1–2 appearances           |
| [KW 3]                | H2 Section 2                             | 1–2 appearances           |
...

**LSI Keyword Distribution:**
LSI keywords are not targeted for specific sections. They are distributed naturally throughout the body at whatever density the content naturally produces — forcing LSI keywords into specific positions creates unnatural prose.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Titles Across Options

The three Blog Title options must be structurally distinct — not reformulations of the same sentence. If any two options share both the same opening structure and the same modifier → rewrite the less unique option.

Test: Could a reader see all three titles and recognize them as three distinct angles? If not → rebuild the weakest option.

### Rule 2 — No Meta Title Duplicates Within the Set

The three meta title options must each be distinctly different. No two options may share the same first 30 characters. No two options may use the same CTR trigger category (e.g., two "question format" titles in the same set).

### Rule 3 — No Meta Description Structural Duplicates

Each of the three meta descriptions must open with a different sentence structure. Specifically: Option 1 (benefit lead), Option 2 (problem lead), and Option 3 (specificity/social proof lead) must maintain their distinct structures. No two descriptions may open with the same first word.

### Rule 4 — No Keyword Stuffing

No meta title may contain the primary keyword more than once. No meta description may contain the primary keyword more than once (the keyword variation in the description counts as the single keyword use). No slug may repeat any word from the keyword phrase.

### Rule 5 — No Same Phrasing Across Metadata Types

The same phrase must not appear identically in both the meta title and the meta description. If the meta title says "Best CRM Software for Small Teams" → the meta description should not open with "The best CRM software for small teams..." — it must rephrase. Exact phrase repetition between metadata types signals low-effort generation.

### Rule 6 — Keyword Set Non-Duplication

No keyword may appear in more than one tier (primary, secondary, long-tail, LSI). If a semantic check reveals a keyword in both the secondary and long-tail sets → assign it to the more appropriate tier and remove it from the other. All four tiers must have zero cross-tier duplicates.

### Rule 7 — No Duplication with Existing Content Titles

When `existing_content_titles` is available → verify no generated blog title matches or closely echoes an existing published title on the same domain. Exact and near-exact title matches are rejected and regenerated.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Metadata Behaviors

| Prohibited Behavior                                                                              | Reason                                                                            |
|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Generating a meta title that exactly duplicates the blog title (different lengths, different purpose) | Meta titles are shorter and SERP-specific — they must not duplicate blog titles |
| Using "Learn more" as a CTA in any meta description                                              | "Learn more" is the most generic, lowest-CTR CTA possible — always replace       |
| Opening a meta description with "This article explains..." or "In this guide..."                 | Self-referential openers suppress CTR — always lead with benefit or problem      |
| Generating a URL slug longer than 5 words without flagging the violation                         | Slug length is monitored — violations must surface, not be silently accepted     |
| Producing a meta title without checking and recording its character count                        | Character counts are mandatory for every meta title — not optional               |
| Generating LSI keywords that are too generic to be topic-specific                               | "software," "business," "team" are not LSI keywords — they have no topical signal |
| Assigning all secondary keywords the same placement target (e.g., all in body text)            | Secondary keywords must be distributed across different content zones             |
| Producing Option 3 (Unique Angle) blog title that is misleading or overpromises the content    | Unique framing must be honest — not clickbait dressed as creativity               |
| Generating social metadata that is identical to the meta title                                   | OG and Twitter titles serve a different context — they must be adapted, not copied|
| Skipping funnel alignment validation when funnel stage has been corrected by SERP intelligence  | Corrected funnel stages change the entire tone model — validation is mandatory    |

### Required Metadata Behaviors

- Every meta title must have its character count recorded and verified as ≤60.
- Every meta description must have its character count recorded and verified as 150–160.
- Every URL slug must have its word count verified as ≤5 words.
- Every secondary keyword must have a specific placement guideline.
- Every title option must document its angle, keyword position, and CTA.
- The recommended option must include a specific rationale tied to SERP pattern or funnel context.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ serp-blueprint-generator.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary blueprint input)                                              |
| **Receives**     | SEO Brief, keyword expansion, metadata draft (Part 9 of blueprint), content structure, audience, tone, goal |
| **Used for**     | Steps 1–8 — blueprint data seeds all metadata generation and provides keyword foundation   |
| **Critical field** | Part 9 (Metadata draft) — provides starting keyword and initial title/description candidates for refinement |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream intelligence source                                                                |
| **Receives**     | Dominant intent, funnel stage correction, content direction, SERP feature data             |
| **Used for**     | Step 1 (intent model), Step 8 (funnel alignment validation), Advanced Logic (SERP pattern matching) |
| **Override authority** | serp-intelligence.skill funnel correction overrides blueprint funnel stage           |

---

### ▸ deep-serp-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment                                                                |
| **Receives**     | Title patterns from top-ranking pages, character count distribution, CTR signals           |
| **Used for**     | Advanced Logic — SERP title pattern matching, calibration of Option 1 vs. Option 3 titles  |

---

### ▸ entity-extraction.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment                                                                |
| **Receives**     | Critical and Important entity list for the target cluster                                  |
| **Used for**     | Step 6 Phase 6D — entity-based LSI keyword generation                                      |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | metadata-generator → content-generation-engine (primary downstream consumer)               |
| **Sends**        | Complete metadata package — selected titles, meta titles, meta descriptions, URL slug, keyword intelligence, funnel alignment report |
| **Used for**     | Content generation begins with this metadata package as its structural anchor              |
| **Critical fields** | Primary keyword + keyword expansion set + recommended blog title + recommended meta title |

---

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | metadata-generator → content-inventory (lifecycle update)                                  |
| **Sends**        | Primary keyword, secondary keywords, URL slug, cluster assignment, funnel stage            |
| **Used for**     | Recording the content piece in the inventory at PLANNED status with its keyword assignments |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ at pre-conditions, WRITE after output finalization                     |
| **Reads**        | Prior metadata packages for this keyword, existing content titles for dedup checking       |
| **Writes**       | Complete metadata package, keyword lifecycle updates (PRIMARY + SECONDARY = ASSIGNED)     |
| **Memory layers**| keyword-memory.skill, content-memory.skill (metadata record)                              |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Target keyword is not provided                                            | Input pre-condition — keyword null                     | Halt. Flag: "Primary keyword required. All metadata generation is anchored to a specific target keyword." |
| Blueprint is unavailable                                                  | serp-blueprint-generator data absent                   | Operate in KEYWORD-ONLY MODE. Generate all metadata from keyword + SERP intelligence alone. Flag every output element as BASELINE VERSION — may need refinement when blueprint is available. |
| SERP intelligence is unavailable                                          | serp-intelligence data absent                          | Default intent model from keyword pattern. Flag all Step 1 intent decisions as INFERRED. All generated metadata is ESTIMATED — verify before publishing. |
| A generated meta title exceeds 60 characters                              | Step 3C character count check fails                    | Apply shortening protocol (remove year → replace verbose phrase → remove weakest modifier). Log each shortening action. Produce corrected title alongside original. |
| A generated meta description is outside 150–160 range                    | Step 4C character count check fails                    | Apply expansion (too short) or trimming (too long) protocol. Log adjustment. Produce corrected version. Never publish without character compliance. |
| Generated slug exceeds 5 words                                            | Step 5C word count check fails                         | Remove additional stop words. If still >5 words → shorten keyword phrase to its core noun phrase. Flag: "Slug shortened — verify keyword presence retained." |
| No CTR trigger element can be found for a meta title                     | Step 3A — trigger category check                       | Default to adding the year if content is time-sensitive. Add "Complete" or "Guide" as power word for evergreen. Never produce a trigger-free meta title. |
| Three blog title options converge on the same structure                   | Step 2B — structural distinctness check fails          | Rebuild the least unique option from scratch using a different title type formula. Each option type must produce a structurally distinct result. |
| LSI keyword generation produces <10 keywords                              | Step 6D — LSI count < target                           | Expand LSI generation to include process terms, industry terms, and related concept terms. If genuinely limited by topic narrowness → note: "LSI keywords limited due to narrow topic scope." |
| Funnel alignment check flags multiple elements as misaligned             | Step 8B — >2 compliance failures                       | Flag entire package as FUNNEL MISALIGNMENT — NEEDS REGENERATION. Identify the most likely cause (incorrect intent model in Step 1) and propose correction before regenerating. |
| Memory controller unavailable                                             | READ/WRITE returns error                               | Proceed without dedup check for existing titles. Flag: "Existing title deduplication skipped — verify manually against published content before publishing." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — CHARACTER COUNT COMPLIANCE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CHARACTER COUNT LIMITS — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
META TITLE:
  Maximum     : 60 characters
  Sweet spot  : 50–58 characters
  Minimum     : 30 characters (below = waste of available space)
  Rule        : Keyword in first 35 characters where possible
  Truncation  : Google truncates at ~60 chars on desktop, ~70 on mobile

META DESCRIPTION:
  Maximum     : 160 characters
  Minimum     : 150 characters
  Sweet spot  : 155–160 characters
  Rule        : Keyword in first 80 characters; CTA in last 20–25 characters
  Truncation  : Google truncates at ~160 chars desktop, ~130 mobile

BLOG TITLE (display):
  Maximum     : ~70 characters for most platforms
  Rule        : No hard limit — but >70 chars may truncate in social shares

URL SLUG:
  Maximum     : 5 words after stop word removal
  Rule        : Lowercase, hyphens, no dates, no stop words
  Best practice: 3–4 words (cleanest form)

OG TITLE (Open Graph):
  Recommended : 40–60 characters
  Hard limit  : 95 characters (Facebook)
  Sweet spot  : 55 characters (works across platforms)

TWITTER TITLE:
  Recommended : 50–70 characters
  Hard limit  : 70 characters for clean display

COUNTING RULE:
  Count every character including spaces.
  "Best CRM Software" = 18 characters (B-e-s-t-[space]-C-R-M-[space]-S-o-f-t-w-a-r-e)
  Always count programmatically, never estimate visually.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — FUNNEL ALIGNMENT TONE GUIDE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
FUNNEL ALIGNMENT TONE GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOFU — INFORMATIONAL:
  Reader state  : Exploring, learning, discovering
  Title tone    : Educational, clear, curious-satisfying
  CTA type      : Soft educational — "Learn", "Discover", "Find out", "Understand"
  Triggers      : "What Is", "How to", "Guide", "Explained", "Complete"
  Avoid         : Urgency language, pricing signals, comparison CTAs
  Slug pattern  : Concept-first, question-led

MOFU — COMMERCIAL:
  Reader state  : Evaluating, comparing, narrowing choices
  Title tone    : Comparative, authoritative, trust-building
  CTA type      : Evaluation — "Compare", "See which", "Choose", "Decide", "Find"
  Triggers      : "Best", "vs", "Review", "Top [N]", "Alternatives", "Comparison"
  Avoid         : Pure educational tone, heavy urgency, "Get started" conversion CTAs
  Slug pattern  : Evaluation-led, comparison keyword, "best-X" format

BOFU — TRANSACTIONAL:
  Reader state  : Ready to act, looking for confirmation and path to action
  Title tone    : Action-focused, outcome-clear, low-friction
  CTA type      : Action — "Get started", "Try free", "Start today", "Access"
  Triggers      : "Free", "Start", "Get", "Try", "Today", "Now", "Immediately"
  Avoid         : Educational framing, comparison without verdict, soft CTAs
  Slug pattern  : Action-led, outcome-focused, or tool-specific

MIXED INTENT:
  Lead with the primary intent (as validated by serp-intelligence)
  Use secondary intent signals only in meta description and social metadata
  Slug follows primary intent pattern
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — KEYWORD TIER DEFINITIONS QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
KEYWORD TIER DEFINITIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRIMARY KEYWORD (1):
  The exact keyword phrase this content ranks for.
  Must appear in: URL slug, H1, meta title, first 100 words, meta description.
  Never altered — this is the ranking anchor.

SECONDARY KEYWORDS (6–8):
  Semantically related keywords at different intent angles.
  Each has a designated placement target (H2, H3, intro, conclusion).
  At least one at a different funnel stage than primary.
  May appear in headings — help reinforce topical breadth.

LONG-TAIL KEYWORDS (5–7):
  Highly specific 4+ word queries within the same content journey.
  Assigned to specific content sections where they naturally appear.
  Cover: question-based, audience-qualified, comparison-specific, constraint-qualified types.
  Appear naturally in H3s, sub-sections, and body text.

LSI KEYWORDS (10–15):
  Entity-based + Concept-based + Process/Method + Measurement/Metric.
  Not targeted for specific placements — woven naturally into prose.
  Never appear in headings as targeted keywords.
  Signal topical depth and semantic completeness to search algorithms.

CROSS-TIER DEDUP RULE:
  No keyword appears in more than one tier.
  No two keywords in any tier are semantic duplicates (≥80% overlap).
  Primary keyword is not a secondary, long-tail, or LSI keyword.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: content/metadata-generator.skill*
*Nexus SEO Operating System — Version 1.0.0*

# PATCH: metadata-generator.skill — v1.1.0 UPDATE
# APPLY TO: references/metadata-generator.skill.md
# INSTRUCTIONS: Append this entire section to the end of the existing metadata-generator.skill.md file.
# These additions add excerpt generation, social metadata (OG), and auto-trigger rules.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## ADDENDUM — EXCERPT & SOCIAL METADATA (v1.1.0)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### WordPress Excerpt Generation

Generate 3 excerpt options for every blog post. The user selects their preferred style.

| Style | Format | Character Limit | Example |
|---|---|---|---|
| **Question-based** | Hooks curiosity with a question | 150-160 chars | "Is your Playwright CI pipeline hiding flaky tests? Learn how to detect, track, and fix them with proven strategies from real engineering teams." |
| **Benefit-based** | States what the reader will learn | 150-160 chars | "Cut Playwright debugging time by 70% with trace viewer integration, error grouping, and AI-powered failure analysis for your CI pipeline." |
| **Stat-based** | Leads with a compelling number | 150-160 chars | "73% of flaky Playwright tests stem from timing issues. This guide covers the 8 root causes and proven fixes used by teams running 5,000+ tests." |

**Rules:**
- 150-160 characters maximum (WordPress default)
- Must contain the primary keyword naturally
- Must NOT repeat the meta description verbatim
- Must stand alone as a compelling summary
- No em dashes (use --)
- All numbers in numeric format (3 not three)

### Social Metadata (Open Graph + Twitter Cards)

Generate for every blog post:

| Meta Tag | Specification |
|---|---|
| `og:title` | Same as SEO title OR a more engaging variant (max 60 chars) |
| `og:description` | Same as meta description OR a more social-friendly variant (max 160 chars) |
| `og:image` | Suggest the hero image or most compelling infographic from the article |
| `og:type` | `article` |
| `og:url` | Canonical URL of the article |
| `twitter:card` | `summary_large_image` |
| `twitter:title` | Same as og:title |
| `twitter:description` | Same as og:description |

### Schema Markup Generation

Generate JSON-LD structured data for every blog post:

#### Always Include: Article Schema
```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "{title}",
  "description": "{meta description}",
  "author": {
    "@type": "Organization",
    "name": "[brand.brand_name]"
  },
  "publisher": {
    "@type": "Organization",
    "name": "[brand.brand_name]",
    "url": "[brand.site_url]"
  },
  "datePublished": "{date}",
  "dateModified": "{date}",
  "mainEntityOfPage": "{canonical URL}"
}
```

#### Conditional Schema (add when present in content)
| Content Element | Schema Type | Trigger |
|---|---|---|
| FAQ section exists | `FAQPage` | Article contains FAQ section with Q&A pairs |
| Step-by-step tutorial | `HowTo` | Article has numbered steps/instructions |
| Comparison/list | `ItemList` | Article lists tools, methods, or options |
| Code examples | `SoftwareSourceCode` | Article contains code blocks |

### Auto-Trigger Rule

This skill (with these additions) MUST be triggered automatically whenever:
1. `content-generation-engine` completes a new article
2. User asks to "generate metadata" or "create meta tags"
3. User asks to "rewrite" or "optimize" existing content
4. `master-orchestrator` reaches Phase 4 (Quality Assurance)

The skill produces the COMPLETE metadata package: title + meta description + 3 excerpts + OG tags + Twitter card + JSON-LD schema.

### Output Format
```
═══════════════════════════════════════
METADATA PACKAGE
Article: [title]
═══════════════════════════════════════

## SEO METADATA (Yoast Format)
- Title: [title] ([N] chars)
- Meta description: [description] ([N] chars)
- URL slug: /blog/[slug]/
- Focus keyword: [primary keyword]

## EXCERPT OPTIONS
1. [Question-based] ([N] chars)
2. [Benefit-based] ([N] chars)
3. [Stat-based] ([N] chars)

## SOCIAL METADATA
- OG Title: [title]
- OG Description: [description]
- OG Image suggestion: [image description or filename]
- Twitter Card: summary_large_image

## SCHEMA MARKUP
[Full JSON-LD block(s)]
═══════════════════════════════════════
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## v3.0 UPGRADE — SCHEMA AUTO-SELECTION BY CONTENT TYPE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**NEW in v3.0: NEVER default to Article schema. Analyze the content type and select the correct schema combination.**

### Auto-Selection Rules

Before generating the JSON-LD, classify the content and select schema accordingly:

| Content Type | Detection Signals | Primary Schema | Additional Schema |
|---|---|---|---|
| **Blog post** | Standard article, educational, informational | `Article` | `FAQPage` (if FAQ section exists) |
| **Tutorial / How-to** | Step-by-step instructions, numbered processes, "how to" in title | `HowTo` | `Article` + `FAQPage` |
| **Comparison / Versus** | "vs," "versus," "compared to," side-by-side table | `WebPage` | `FAQPage` + `ItemList` (if listing features) |
| **Alternatives page** | "alternatives," "best tools for," tool listing | `WebPage` + `ItemList` | `FAQPage` |
| **Tool / Product page** | Feature page, product description, SaaS tool | `SoftwareApplication` | `WebPage` + `FAQPage` |
| **Service page** | QA services, consulting, agency offering | `Service` + `Organization` | `FAQPage` |
| **List / Roundup** | "Top 10," "best X," curated list | `Article` + `ItemList` | `FAQPage` |
| **Landing page (generic)** | Conversion-focused, CTA-heavy | `WebPage` | Page-type-specific (see landing-page-engine) |

### SoftwareApplication Schema (for product/tool pages)

When `brand.brand_type = product` or content is about a specific brand feature, use `SoftwareApplication`:

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "[brand.brand_name]",
  "applicationCategory": "DeveloperApplication",
  "operatingSystem": "Web",
  "description": "[Feature-specific description]",
  "url": "[brand.site_url]",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD",
    "description": "Free tier available"
  }
}
```

### BreadcrumbList Schema (add to ALL pages)

Every page should include `BreadcrumbList` alongside its primary schema:

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://[domain]/"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Blog",
      "item": "https://[domain]/blog/"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "[Article Title]",
      "item": "https://[domain]/blog/[slug]/"
    }
  ]
}
```

### Organization Schema (for Service pages and About pages)

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "[Brand Name]",
  "url": "https://[domain]",
  "description": "[Brand description]"
}
```

### Auto-Selection Process

1. Read the content and classify its type from the table above
2. Select the primary + additional schema types
3. Always add `BreadcrumbList`
4. If FAQ section exists in the content, always add `FAQPage` regardless of content type
5. Generate all selected JSON-LD blocks
6. Validate: no duplicate `@type` declarations, all required fields populated
7. Include in metadata package output

### What to NEVER Do
- Never default to `Article` without checking content type first
- Never use `Article` for landing pages, service pages, or product pages
- Never omit `FAQPage` when a FAQ section exists in the content
- Never omit `BreadcrumbList` — it goes on every page

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## v3.0 UPGRADE — llms.txt GENERATION
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**NEW in v3.0: Generate an llms.txt entry as a bonus deliverable.**

### What Is llms.txt
A proposed standard file (served at `/llms.txt`) that helps LLMs understand site structure and content. Similar to robots.txt but designed for AI comprehension.

### Honest Assessment
As of early 2026, no major AI lab has committed to honoring llms.txt. A study of 300,000 domains found no statistical correlation between having llms.txt and being cited by LLMs. However: the file is trivial to generate, Google has started probing for it, and it's a zero-cost bet on a standard that could gain adoption.

### llms.txt Entry Format
For each piece of content, generate an llms.txt entry:

```
# [Site Name]

> [One-line site description]

## Blog Posts

- [Article Title](https://[domain]/blog/[slug]/): [One-line description of what this article covers]
```

### How to Deliver
- Include llms.txt entry at the bottom of the metadata package
- Label it: "BONUS: llms.txt entry (add to your site's /llms.txt file)"
- Do NOT oversell it — present as "low-effort, speculative benefit"

---

### Updated Output Format (v3.0)

```
═══════════════════════════════════════
METADATA PACKAGE
Brand: [brand.brand_name]
Article: [title]
Content Type: [detected type]
═══════════════════════════════════════

## SEO METADATA
- Title: [title] ([N] chars)
- Meta description: [description] ([N] chars)
- URL slug: /blog/[slug]/
- Focus keyword: [primary keyword]

## EXCERPT OPTIONS
1. [Question-based] ([N] chars)
2. [Benefit-based] ([N] chars)
3. [Stat-based] ([N] chars)

## SOCIAL METADATA
- OG Title: [title]
- OG Description: [description]
- OG Image suggestion: [description]
- Twitter Card: summary_large_image

## SCHEMA MARKUP (Auto-Selected: [types])
[JSON-LD block 1 — primary schema]
[JSON-LD block 2 — additional schema]
[JSON-LD block 3 — BreadcrumbList]
[JSON-LD block 4 — FAQPage (if applicable)]

## BONUS: llms.txt Entry
[llms.txt formatted entry]
═══════════════════════════════════════
```

---

*Metadata Generator — v3.0.0 | Nexus SEO Operating System | Auto-Selected Schema + llms.txt*
