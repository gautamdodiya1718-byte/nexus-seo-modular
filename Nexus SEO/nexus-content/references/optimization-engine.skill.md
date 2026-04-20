# SKILL: content/optimization-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Optimization and Improvement
# EXECUTION PRIORITY: INDEPENDENT — Runs on-demand for existing content optimization, outside the main generation pipeline. Can also run as a post-publication improvement cycle.
# POSITION: Existing content improvement engine. Operates on content that already exists — either published or recently generated — to identify gaps and deliver specific, prioritized improvements.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **content improvement and optimization engine** of the Nexus system.

Where `content-generation-engine.skill` creates new content from scratch, this skill takes existing content — published articles, recently written pieces, or inherited content from before the Nexus system was in use — and transforms them into better-performing, more competitive resources. It analyzes what the content currently does, compares it against what the SERP and the target keyword actually demand, identifies specific gaps in structure, depth, keywords, AEO coverage, and entity completeness, and delivers a prioritized, actionable optimization plan with specific rewrite suggestions for the most critical issues.

This skill does not deliver generic SEO advice. It does not recommend "adding more content" or "improving readability." It delivers exact, specific, traceable findings — what is wrong with this specific content, why it is wrong relative to what the SERP rewards, and exactly what the content team should do to fix it. Every recommendation is ranked by business impact so the content team knows where to invest their optimization effort first.

The optimization engine is the primary tool for content refresh cycles — when existing content is aging, when rankings have dropped, when a keyword's intent has shifted, or when a content audit reveals that a piece is underperforming relative to its topic's potential.

### Primary Responsibilities

| Responsibility                        | Description                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Content Intake**                    | Accept content in any available format: URL, pasted text, or outline — and extract all analyzable elements           |
| **Intent Alignment Verification**     | Determine whether the content's structure and framing match the dominant SERP intent for the target keyword           |
| **Structural Audit**                  | Audit heading hierarchy, introduction quality, logical flow, FAQ presence, and section completeness                  |
| **Keyword Coverage Analysis**         | Verify primary keyword placement, semantic keyword coverage, and entity coverage against what the SERP requires       |
| **Quality Gap Detection**             | Identify missing content depth elements — examples, statistics, comparisons, case studies, actionable steps          |
| **AEO Audit**                         | Evaluate the content's eligibility for featured snippets, PAA capture, and answer engine optimization               |
| **SERP Comparison**                   | Compare the content's depth, structure, and coverage against the current top-ranking pages for this keyword          |
| **Issue Prioritization**              | Classify every identified issue as HIGH, MEDIUM, or LOW priority based on ranking impact                             |
| **Optimization Checklist Production** | Deliver a structured, prioritized checklist of all issues with specific fixes and expected impact                    |
| **Rewrite Suggestions**               | For HIGH-priority issues, provide specific rewritten versions of problematic sections                                |
| **Entity Gap Detection**              | Identify Critical entities missing from the content that competitors' content includes                               |
| **Readability Optimization**          | Detect readability issues: overly complex sentences, monotonous rhythm, and missing engagement elements              |

### What This Skill Is NOT

- It is **not** a content generator. It produces optimization recommendations and rewrite suggestions — not complete new articles.
- It is **not** a keyword researcher. It validates keyword usage against an existing target — it does not discover new keywords unless the user explicitly provides an updated keyword.
- It is **not** a technical SEO auditor. It focuses on content-level optimization — not page speed, crawlability, or structured data implementation.
- It is **not** a publisher. It delivers optimization plans — implementation is a content team responsibility.
- It is **not** a subjective reviewer. Every finding is traceable to a specific structural rule, SERP pattern, or entity gap.

### The Defining Standard of This Skill

> Every recommendation produced by this skill must be specific enough to implement without additional research.
>
> "Add more examples" is not a recommendation. It is noise.
> "Add a real-world example in Section 3 showing how [specific use case] applies to [specific audience] — no competitor covers this angle" is a recommendation.
>
> Every issue must be traceable to a specific element (or absence of an element) in the content.
> Every fix must describe the exact action required — not the category of action.
> Every expected impact must explain what changes in the content's performance when the fix is implemented.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                   | Description                                                                              | Required |
|-------------------------|------------------------------------------------------------------------------------------|----------|
| `content`               | The existing content — one of: a URL, pasted text, or a structured outline               | YES      |
| `target_keyword`        | The keyword this content is targeting or should be targeting                             | YES      |

### Optional Inputs

| Field                   | Description                                                                              | Used For                                              |
|-------------------------|------------------------------------------------------------------------------------------|-------------------------------------------------------|
| `updated_keyword`       | A new or revised target keyword if the optimization is being done with a keyword update  | Replaces `target_keyword` for intent alignment and placement checks |
| `funnel_stage`          | TOFU / MOFU / BOFU — the intended funnel stage                                          | Intent alignment check and conversion layer audit     |
| `competitor_urls`       | Specific competitor URLs to compare against in the SERP comparison step                 | Step 7 (SERP Comparison) — more targeted analysis     |
| `serp_intelligence`     | SERP data from `serp-intelligence.skill` if already available                           | Step 2 (Intent Alignment), Step 7 (SERP Comparison)  |
| `entity_map`            | Entity map from `entity-extraction.skill` if already available                          | Step 4 (Entity Coverage) and Step 7 (Advanced Logic — entity gap) |
| `blueprint`             | A content blueprint for this keyword, if available                                      | Step 3 (Structural Audit against blueprint) and Step 7 |
| `optimization_mode`     | FULL (all 10 steps) / QUICK (Steps 1–5 + 8–9 only) / REWRITE FOCUSED (Steps 1–4 + 10)  | Scopes the analysis depth |
| `word_count_target`     | If the optimization also requires hitting a specific word count                          | Step 5 (Quality Gap — guides depth additions)         |

### Content Input Format Handling

**If URL is provided:**
- Extract the page's title, H1, H2s, H3s, meta description, and visible body text.
- Note: this skill assumes it has access to the page's textual content — it does not perform live web crawling. If the URL's content cannot be accessed, request pasted text.

**If pasted text is provided:**
- Accept and process directly.
- Identify structural markers (##, ###, or explicit H1/H2/H3 labels).
- If no structural markers exist → treat all text as body content and note: "No heading structure detected in input — structural audit will flag H1/H2 absence."

**If outline is provided:**
- Accept the outline structure.
- Note that quality gap detection (Step 5) and readability checks (Advanced Logic) will be limited — outline analysis only reveals structural gaps, not depth gaps.
- Flag all depth-related checks as "OUTLINE ONLY — verify against full article if available."

### Input Clarification Protocol

When the content input is ambiguous or insufficient:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS OPTIMIZATION ENGINE — INPUT NEEDED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
To begin content optimization, I need:

  1. The content to analyze:
     → Paste the article text, or
     → Provide the URL, or
     → Paste the content outline

  2. The target keyword:
     → What keyword should this content rank for?

  3. (Optional) Is the keyword changing?
     → If optimizing for a new keyword, provide it separately.

Please provide at least items 1 and 2 to proceed.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Optimization Intelligence Package

```
NEXUS OPTIMIZATION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Content Reference       : [URL / Title / "Pasted Content"]
Target Keyword          : [keyword]
Updated Keyword         : [new keyword or SAME]
Funnel Stage            : [TOFU / MOFU / BOFU / INFERRED]
Content Word Count      : [N words — approximate]
Optimization Mode       : [FULL / QUICK / REWRITE FOCUSED]
Total Issues Found      : [count]
  HIGH Priority         : [count]
  MEDIUM Priority       : [count]
  LOW Priority          : [count]
SERP Comparison Ran     : [YES / NO — reason if NO]
Entity Gap Analysis     : [YES (N entities missing) / NO]
Rewrite Suggestions     : [count]
Overall Content Grade   : [A / B / C / D — based on issue severity]
Generated By            : content/optimization-engine.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — OPTIMIZATION CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | Priority | Issue                           | Fix Required                          | Section Affected   | Expected Impact              |
|---|----------|---------------------------------|---------------------------------------|--------------------|------------------------------|
| 1 | HIGH     | [specific issue]                | [specific, actionable fix]            | [section or global]| [specific ranking impact]    |
| 2 | HIGH     | [specific issue]                | [specific fix]                        | [section]          | [impact]                     |
| 3 | MEDIUM   | [specific issue]                | [specific fix]                        | [section]          | [impact]                     |
| 4 | MEDIUM   | [specific issue]                | [specific fix]                        | [section]          | [impact]                     |
| 5 | LOW      | [specific issue]                | [specific fix]                        | [section]          | [impact]                     |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — ISSUES REPORT (Detailed)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Per-Issue Detail format in Section 3]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — RECOMMENDED IMPROVEMENTS SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Summary of all recommended improvements organized by improvement category — not by issue — for editorial planning]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — REWRITE SUGGESTIONS (HIGH Issues Only)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Specific rewritten sections for HIGH priority issues]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : content-memory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Per-Issue Detail Format (Part 2)

```
ISSUE DETAIL RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Issue #         : [sequential]
Priority        : [HIGH / MEDIUM / LOW]
Category        : [INTENT / STRUCTURE / KEYWORD / DEPTH / AEO / SERP GAP / ENTITY / READABILITY / CONVERSION]
Location        : [Section name or "Global" or "Introduction"]
Issue           : [specific description of what is wrong — not vague]
Evidence        : [what data, pattern, or SERP signal confirms this is an issue]
Fix             : [specific, actionable fix description — one or two sentences]
Effort Level    : [QUICK (< 30 min) / MODERATE (1–3 hours) / MAJOR (3+ hours)]
Expected Impact : [specific description of what changes in ranking or user experience]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Content Grade Definitions

| Grade | Condition                                                                                          |
|-------|----------------------------------------------------------------------------------------------------|
| **A** | 0 HIGH issues; ≤ 3 MEDIUM issues; article is competitive with top-ranking pages                   |
| **B** | 1–2 HIGH issues; 4–7 MEDIUM issues; article is functional but has clear improvement areas         |
| **C** | 3–5 HIGH issues; content has meaningful structural or depth gaps; ranking potential is limited     |
| **D** | 6+ HIGH issues; content has fundamental problems — intent mismatch, structural failure, major gaps |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — CONTENT INTAKE

Accept and process the content input into an analyzable structure.

**Phase 1A — Content Format Identification**

Determine the format of the provided content:

| Input Signal                                            | Format Identified  | Processing Path          |
|---------------------------------------------------------|--------------------|--------------------------|
| Starts with `http://` or `https://`                    | URL                | Extract textual content from the URL |
| Contains heading markers (##, ###, H1:, H2:, [H1])     | Structured text    | Parse structure and body directly |
| Plain text without obvious heading markers              | Unstructured text  | Treat as body text; flag absent heading structure |
| Bulleted/numbered list with short items                 | Outline            | Process as outline; limit depth analysis |

**Phase 1B — Content Structure Extraction**

From the content, extract:

1. **H1:** The page's primary heading.
2. **Meta description:** If available from URL or provided alongside pasted content.
3. **Introduction:** The first 200 words of body text (before the first H2).
4. **H2 sections:** All H2 headings and the body text within each section.
5. **H3 sub-sections:** All H3 headings and their corresponding body content.
6. **FAQ section:** Any FAQ block or Q&A structured content.
7. **Total word count:** Approximate count of body text (excluding structural metadata).
8. **CTA presence:** Any call-to-action elements visible in the text.

**Phase 1C — Content Inventory Summary**

Produce a quick inventory of what was found:

```
CONTENT INVENTORY SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Format Detected     : [URL / Structured Text / Unstructured / Outline]
H1 Present          : [YES — "[H1 text]" / NO]
Meta Description    : [Present — "[text]" / Not available]
Introduction Words  : [N words]
H2 Sections         : [count] — [list of H2 titles]
H3 Sub-sections     : [count]
FAQ Present         : [YES — N questions / NO]
CTA Present         : [YES — "[CTA text]" / NO]
Estimated Word Count: [N words]
Target Keyword Found: [YES — N times / NO]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — INTENT ALIGNMENT

Determine whether the content's framing, structure, and primary angle match the dominant search intent for the target keyword.

**Phase 2A — Target Keyword Intent Classification**

If `serp-intelligence.skill` output is available → use its dominant intent classification.

If not available → infer from keyword pattern:

| Keyword Pattern                                         | Inferred Intent    | Funnel Stage  |
|---------------------------------------------------------|--------------------|---------------|
| "how to," "guide to," "what is," "explained"           | Informational      | TOFU          |
| "best," "vs," "comparison," "alternatives," "review"   | Commercial         | MOFU          |
| "pricing," "buy," "free trial," "get started"          | Transactional      | BOFU          |
| Mixed signals                                           | Mixed              | Flag for verification |

**Phase 2B — Content Intent Assessment**

Determine what intent the current content actually serves:

| Content Signal                                                          | Content Intent     |
|-------------------------------------------------------------------------|---------------------|
| Introduction focuses on defining and educating                         | Informational       |
| Majority of H2 sections evaluate, compare, or rank options             | Commercial          |
| Strong CTA, product features, pricing discussion dominates             | Transactional       |
| Mixed — some educational, some evaluative, some promotional            | Mixed               |

**Phase 2C — Intent Alignment Determination**

Compare keyword intent against content intent:

| Keyword Intent | Content Intent | Alignment Status     | Priority |
|----------------|----------------|----------------------|----------|
| Informational  | Informational  | ALIGNED              | No issue |
| Commercial     | Commercial     | ALIGNED              | No issue |
| Transactional  | Transactional  | ALIGNED              | No issue |
| Informational  | Commercial     | MISMATCH             | HIGH     |
| Commercial     | Informational  | MISMATCH             | HIGH     |
| Commercial     | Transactional  | PARTIAL MISMATCH     | MEDIUM   |
| Informational  | Transactional  | MISMATCH             | HIGH     |
| Mixed Keyword  | Informational  | PARTIAL              | MEDIUM   |

**HIGH PRIORITY ISSUE: INTENT MISMATCH**

When intent mismatch is detected:

```
HIGH PRIORITY ISSUE — INTENT MISMATCH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keyword intent   : [Informational / Commercial / Transactional]
Content intent   : [what the content currently serves]
Mismatch type    : [FULL / PARTIAL]
Impact           : Google's intent-matching system penalizes content that serves
                   a different purpose than what the SERP expects. This is the
                   highest-impact optimization target in this article.
Specific issue   : [describe exactly where/how the content misserves the intent]
Fix required     : [describe specifically what structural or framing change is needed]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 2D — Funnel Stage Alignment**

If `funnel_stage` is provided → verify the content's tone, CTA type, and framing match:

| Funnel | Expected Content Characteristics                                            |
|--------|-----------------------------------------------------------------------------|
| TOFU   | Educational tone; definitional sections; no hard sell; soft CTA            |
| MOFU   | Evaluation tone; comparison sections; criteria-based analysis; medium CTA  |
| BOFU   | Action tone; outcome-focused; trust signals; specific product/service; hard CTA |

Funnel misalignment is classified as HIGH if the mismatch is between TOFU and BOFU; MEDIUM if between adjacent stages.

---

### ▸ STEP 3 — STRUCTURAL AUDIT

Audit the content's structural integrity against the requirements for competitive ranking content.

**Phase 3A — H1 Audit**

| Check                                                          | Pass Condition                                          | Issue if Failing          |
|----------------------------------------------------------------|----------------------------------------------------------|---------------------------|
| H1 is present                                                  | Exactly one H1 exists                                   | HIGH — missing H1          |
| H1 contains the target keyword                                | Exact or close semantic variation present               | HIGH — keyword absent from H1 |
| H1 is descriptive (not just the keyword)                      | H1 is a complete phrase, not a bare keyword             | MEDIUM — H1 is too thin    |
| H1 length is appropriate (6–12 words)                         | Within range                                            | LOW — flagged for adjustment |
| H1 is unique (not a duplicate of the meta title)              | H1 ≠ exact meta title                                  | LOW — differentiate H1 and meta title |

**Phase 3B — Introduction Audit**

| Check                                                          | Pass Condition                                          | Issue if Failing          |
|----------------------------------------------------------------|----------------------------------------------------------|---------------------------|
| Introduction answers the keyword's implied question           | First paragraph directly addresses what the keyword implies | HIGH — intro does not answer the query |
| Introduction does not start with a prohibited opener          | Not one of the banned AI-generated opener patterns       | MEDIUM — replace with direct opening |
| Primary keyword in first two sentences                        | Keyword present within the first 100 words              | MEDIUM — keyword placement in intro |
| Introduction sets up article scope clearly                    | By end of intro, reader knows what they will learn      | MEDIUM — vague intro      |
| Introduction is 150–250 words                                 | Not too brief (< 100) or excessive (> 300)              | LOW — flag if outside range |

**Phase 3C — Heading Structure Audit**

| Check                                                          | Pass Condition                                          | Issue if Failing          |
|----------------------------------------------------------------|----------------------------------------------------------|---------------------------|
| H2 sections present (minimum 3)                               | At least 3 H2 sections in the article                  | HIGH — article lacks sections |
| H2 sections are logically ordered                              | Each H2 builds on the previous or covers a distinct sub-topic | MEDIUM — reorder sections |
| H2 headings are descriptive                                    | Each H2 heading tells the reader what the section covers | MEDIUM — generic headings |
| H3 sub-sections present (where applicable)                    | Complex sections have H3 breakdown                      | LOW — flat structure      |
| No heading hierarchy gaps                                     | No H3 without parent H2; no jump from H1 to H3          | MEDIUM — hierarchy issue  |
| Section sequence matches content type best practice            | Informational: definition → explanation → examples → tips; Comparison: criteria → options → verdict | MEDIUM — wrong sequence |

**Phase 3D — Logical Flow Assessment**

For each H2 section:
1. Does the section's opening paragraph state what the section covers?
2. Do subsequent paragraphs build on the opening rather than introduce unrelated content?
3. Does the section close with a conclusion or transition that relates to what follows?

If a section fails all three → flag as FLOW ISSUE — MEDIUM priority.
If the entire article's section sequence does not follow a coherent reader journey → flag as STRUCTURAL FLOW — HIGH priority.

**Phase 3E — FAQ Presence Audit**

| Check                                                          | Pass Condition                                          | Issue if Failing          |
|----------------------------------------------------------------|----------------------------------------------------------|---------------------------|
| FAQ section present                                            | At least one FAQ block in the article                   | MEDIUM — missing AEO element |
| FAQ has minimum 3 questions                                    | At least 3 Q&A pairs                                   | LOW — expand FAQ          |
| FAQ questions are phrased as natural language queries          | Questions read as real search queries                   | LOW — rephrase for AEO    |
| FAQ answers are concise (40–80 words each)                    | Each answer within range                                | MEDIUM if over 100 words  |
| FAQ answers are self-contained                                 | No "as mentioned above" references in FAQ answers       | MEDIUM — AEO compliance   |

---

### ▸ STEP 4 — KEYWORD COVERAGE

Analyze keyword usage across the content for placement, density, and semantic completeness.

**Phase 4A — Primary Keyword Placement Audit**

| Required Position                                 | Current Status                             | Issue if Missing      |
|---------------------------------------------------|--------------------------------------------|-----------------------|
| H1 heading                                        | Present / Not present                      | HIGH                  |
| Introduction (first 100 words)                   | Present / Not present                      | HIGH                  |
| At least 1 H2 heading                             | Present / Not present                      | MEDIUM                |
| Meta description                                  | Present / Not present (if meta available) | MEDIUM                |

**Phase 4B — Keyword Density Assessment**

Count the primary keyword (exact + close variations) across the entire article:

| Density Level                          | Status                                                                 |
|----------------------------------------|------------------------------------------------------------------------|
| 0 occurrences                          | CRITICAL — keyword never appears despite being the target             |
| 1–3 occurrences in a 1,500+ word article | UNDER-OPTIMIZED — insufficient signal                               |
| Appropriate range (4–8 per 2,500 words) | OPTIMAL                                                             |
| Near-stuffing (9–12 in ≤2,500 words)  | BORDERLINE — review for naturalness                                   |
| >12 in ≤3,000 words                   | KEYWORD STUFFING — remove excess instances                            |

**Phase 4C — Semantic Keyword Coverage**

If `entity_map` or `keyword_intelligence` is available → check which secondary and LSI keywords are currently covered vs. missing:

1. List all secondary keywords from the keyword universe.
2. For each: is it present in the article?
3. Identify the 3 most impactful missing secondary keywords.
4. Flag them as MEDIUM priority improvement opportunities with specific placement suggestions.

If keyword intelligence is not available → apply the semantic coverage inference method:
- What are the 5 most topically related concepts that a comprehensive article on this keyword should cover?
- Which of those 5 concepts are absent from the article?
- Flag each absent concept as a SEMANTIC COVERAGE GAP — MEDIUM priority.

**Phase 4D — Entity Coverage Audit**

If `entity_map` from `entity-extraction.skill` is available:

1. Load Critical entities for this keyword's cluster.
2. For each Critical entity — is it present in the article with context?
3. Missing Critical entities → HIGH or MEDIUM priority issue depending on the entity's centrality.
4. Missing Important entities → LOW or MEDIUM priority.

Entity gap output:

```
ENTITY COVERAGE ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Critical entities required : [count]
Critical entities present  : [count] — [%]
Critical entities MISSING  : [list]
Important entities present : [count] — [%]
Important entities MISSING : [list — top 5]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

For each missing Critical entity → issue record with specific placement recommendation.

---

### ▸ STEP 5 — QUALITY GAP DETECTION

Identify missing content depth elements that prevent the article from competing with more comprehensive top-ranking pages.

**Phase 5A — Depth Element Inventory**

Check the presence of each content depth element:

| Depth Element     | Definition                                                                     | Minimum Expected    |
|-------------------|--------------------------------------------------------------------------------|---------------------|
| **Examples**      | Specific, concrete scenarios illustrating a concept                           | ≥2 in the article  |
| **Case Studies**  | Real-world outcomes, named scenarios, or measurable results                   | ≥1 for MOFU/BOFU   |
| **Statistics**    | Numerical data points attributed to a source type                             | ≥2 sourced stats   |
| **Comparisons**   | Side-by-side evaluation of two or more options on defined criteria            | ≥1 for MOFU        |
| **Actionable Steps** | Numbered or clearly-sequenced instructions for completing a task           | ≥1 how-to section for TOFU tutorials |
| **Mechanism Explanations** | "How and why" explanations — not just "what"                         | ≥2 sections        |

For each element that is ABSENT or INSUFFICIENT → create an issue record.

**Phase 5B — Example Quality Assessment**

For articles that DO have examples: assess their quality:

| Example Quality       | Condition                                                            | Issue Type                          |
|-----------------------|----------------------------------------------------------------------|-------------------------------------|
| SPECIFIC and USEFUL   | Names a concrete scenario with an audience and outcome              | No issue                            |
| GENERIC               | "For example, a company could use X to improve Y"                  | LOW — improve specificity           |
| HYPOTHETICAL without context | "Imagine you are trying to..."                               | MEDIUM — replace with concrete case |
| ABSENT                | No examples in the section                                          | MEDIUM or HIGH depending on section |

**Phase 5C — Statistics Assessment**

For articles that DO cite statistics:

| Statistic Quality     | Condition                                                            | Issue Type                          |
|-----------------------|----------------------------------------------------------------------|-------------------------------------|
| SOURCED with type     | "According to [Type of Source]..." or specific named report         | No issue                            |
| GENERIC attribution   | "Studies show..." or "Research indicates..."                        | LOW — add source type               |
| NO attribution        | Bare statistic with no attribution                                  | MEDIUM — add attribution or verify  |
| POTENTIALLY OUTDATED  | Statistic references a year more than 2 years ago                  | MEDIUM — update or replace          |

**Phase 5D — Comparison Element Assessment**

For MOFU content, a comparison element is required:

| Comparison Type       | Condition                                                            | Status                              |
|-----------------------|----------------------------------------------------------------------|-------------------------------------|
| Formal comparison table | Structured table comparing options on named criteria               | BEST — pass                        |
| Side-by-side prose    | Structured evaluation paragraphs with clear criteria                | ACCEPTABLE — pass                  |
| Mention of alternatives | Brief mentions without real comparison criteria                    | INSUFFICIENT — flag as MEDIUM      |
| No comparison         | MOFU content with no comparison element                             | HIGH issue                         |

**Phase 5E — Actionable Steps Assessment**

For tutorial or how-to content:

| Steps Quality         | Condition                                                            | Status                              |
|-----------------------|----------------------------------------------------------------------|-------------------------------------|
| Numbered steps with detail | Each step is clear, independent, and instructional              | PASS                               |
| Unnumbered steps      | Process described in prose without step structure                   | MEDIUM — restructure as steps       |
| Steps without context | Step listed but not explained                                       | HIGH — expand step detail           |
| No steps in how-to content | Tutorial content without actionable steps                       | HIGH — fundamental gap             |

---

### ▸ STEP 6 — AEO AUDIT

Evaluate the content's eligibility for answer engine optimization — featured snippets, PAA captures, and voice search answers.

**Phase 6A — Direct Answer Block Audit**

| Check                                                          | Pass Condition                                          | Issue if Failing              |
|----------------------------------------------------------------|----------------------------------------------------------|-------------------------------|
| Direct answer block present (before TOC or in intro)          | A 2–3 sentence answer to the keyword query appears early | HIGH — critical AEO element  |
| Answer block is ≤60 words                                     | Concise enough for snippet eligibility                   | MEDIUM — trim to ≤60 words    |
| Answer block is self-contained                                 | No "as discussed below" references                      | MEDIUM — AEO compliance issue |
| Primary keyword in answer block                               | Keyword present naturally                               | MEDIUM — add naturally        |
| Answer block directly answers the keyword's core question      | First reading resolves the primary search query         | HIGH — answer is indirect     |

**Phase 6B — AEO Section Coverage**

Check for AEO-structured sections in the article:

| AEO Check                                                     | Issue if Failing              |
|---------------------------------------------------------------|-------------------------------|
| At least 1 H2 section with question-format heading            | MEDIUM — no AEO-targeted section |
| First paragraph of question-headed H2 gives a direct answer  | MEDIUM — section isn't AEO-structured |
| FAQ section exists with ≥3 questions                          | MEDIUM — missing or thin FAQ |
| FAQ answers begin with direct statements (not hedges)         | MEDIUM — answers not snippet-eligible |
| FAQ answers are 40–80 words                                   | LOW — outside snippet-optimal range |

**Phase 6C — Answer Format Assessment**

For the keyword's most likely snippet type:

| Snippet Type            | Expected Format in Content                                             | Issue if Wrong Format          |
|-------------------------|------------------------------------------------------------------------|--------------------------------|
| Paragraph snippet       | 2–3 sentence direct answer to the question                           | HIGH if using a list instead   |
| List snippet            | Numbered or bulleted list of items/steps                              | HIGH if using prose instead    |
| Table snippet           | Structured comparison or data table                                   | MEDIUM if missing table        |
| Definition snippet      | First sentence defines the term directly                              | HIGH if definition is buried   |

Infer the expected snippet type from the keyword's structure:
- "How to..." → List snippet (steps).
- "What is..." → Definition snippet (paragraph).
- "Best X for Y" → Table snippet (comparison).
- "Why..." → Paragraph snippet (explanation).

---

### ▸ STEP 7 — SERP COMPARISON

Compare the analyzed content against the current competitive landscape for the target keyword.

**Phase 7A — SERP Intelligence Loading**

If `serp-intelligence.skill` data is available:
- Load dominant intent, content type patterns, SERP features, recommended format, recommended length, difficulty.

If SERP data is not available:
- Apply SERP inference from keyword pattern (same as Step 2 Phase 2A).
- Flag all SERP comparison findings as INFERRED — reduced confidence.

**Phase 7B — Depth Comparison**

Compare the analyzed content's word count and section count against the SERP average:

| Comparison                                          | Issue Type                                                      |
|-----------------------------------------------------|-----------------------------------------------------------------|
| Content word count < SERP average × 0.8             | HIGH — content is significantly shorter than competitive set    |
| Content word count ≥ SERP average × 1.0             | PASS — at parity or above                                       |
| Content section count < SERP average section count  | MEDIUM — content has fewer sections than competitors           |
| Content has no tables when SERP average shows ≥60% of pages have tables | MEDIUM — missing format element |
| Content has no FAQ when SERP average shows ≥60% of pages have FAQs | MEDIUM — missing AEO element              |

**Phase 7C — Structure Pattern Comparison**

If `content-pattern-extractor.skill` data or `blueprint` is available — compare content structure against Must-Have SERP patterns:

| Must-Have Pattern                               | Present in Content? | Issue if Absent                |
|-------------------------------------------------|---------------------|--------------------------------|
| [Pattern type — from SERP data]                 | YES / NO            | HIGH if ≥60% SERP pages have it |
| [Pattern type]                                  | YES / NO            | MEDIUM if 30–59% SERP pages have it |
| [Pattern type]                                  | YES / NO            | LOW if <30% SERP pages have it (opportunity) |

When SERP pattern data is not available → apply the universal Must-Have content elements check:

| Universal Must-Have Element                    | Present in Content? | Issue if Absent |
|------------------------------------------------|---------------------|-----------------|
| Definition/explanation section                | YES / NO            | HIGH for informational content |
| How-to or step section                         | YES / NO            | HIGH for tutorial/guide content |
| Comparison element                             | YES / NO            | HIGH for commercial/evaluation content |
| Examples (≥2)                                  | YES / NO            | MEDIUM always   |
| FAQ section                                    | YES / NO            | MEDIUM always   |
| Statistics with attribution                   | YES / NO            | MEDIUM always   |

**Phase 7D — Differentiation Gap Detection**

Beyond matching what competitors have — identify what competitors miss that this content also misses:

Using `gap-opportunity-engine.skill` data (if available) OR SERP inference:
- Are there topics related to this keyword that no top-ranking page covers well?
- Does this content also miss those topics?
- If yes → flag as OPPORTUNITY — low competition angle that could drive differentiation.

Differentiation opportunities are labeled as OPPORTUNITY in the optimization checklist — they are separate from standard MEDIUM/LOW issues.

---

### ▸ STEP 8 — PRIORITIZATION

Classify every identified issue by priority based on its ranking impact.

**Phase 8A — Priority Classification Criteria**

**HIGH PRIORITY — Must fix before this content can rank competitively:**

| Condition                                                                                           |
|-----------------------------------------------------------------------------------------------------|
| Intent mismatch between content and keyword SERP                                                    |
| Primary keyword completely absent from H1 or introduction                                           |
| Article significantly shorter than SERP average (< 80% of SERP word count average)                |
| Missing core section that every top-ranking competitor has                                          |
| Direct answer block absent (for AEO-eligible keyword)                                              |
| No FAQ section when SERP shows ≥60% of pages have FAQs                                             |
| Introduction does not address the keyword's core question                                           |
| No comparison element in evaluation/MOFU content                                                   |
| How-to content with no numbered actionable steps                                                   |
| Critical entity (from entity map) completely absent from article                                    |
| Internal contradiction between two claims in the article                                           |

**MEDIUM PRIORITY — Significant improvements that would noticeably improve rankings:**

| Condition                                                                                           |
|-----------------------------------------------------------------------------------------------------|
| Keyword present but at low density (< 4 instances in 1,500+ word article)                          |
| FAQ section present but answers are over 100 words each (not snippet-eligible)                     |
| Missing secondary keyword coverage (fewer than half the expected secondary keywords present)        |
| Structural flow issues — sections in wrong order for content type                                  |
| Missing statistics or entirely unattributed statistics throughout                                  |
| Missing examples or only generic/hypothetical examples                                             |
| Heading structure is flat (no H3s where sub-structure would help)                                  |
| Direct answer block present but over 80 words (not concise enough for featured snippet)           |
| Important entities missing from entity map coverage                                               |
| AEO sections present but first paragraph doesn't answer the heading question directly             |
| Missing comparison table when SERP shows tables are a significant pattern                          |
| Funnel stage partial mismatch (adjacent stages)                                                    |

**LOW PRIORITY — Minor improvements that polish and strengthen the content:**

| Condition                                                                                           |
|-----------------------------------------------------------------------------------------------------|
| H1 is technically correct but could be more compelling                                             |
| FAQ has 3 questions where 5 would be optimal                                                       |
| Some examples are generic and could be made more specific                                          |
| Some statistics are slightly dated (1–2 years) but not critically so                              |
| LSI keyword coverage is thin                                                                       |
| Some H2 headings are functional but not particularly engaging                                      |
| Minor readability issues (complex sentences in isolated passages)                                   |
| Internal linking opportunities not yet implemented                                                 |
| Social metadata could be more engaging than current version                                        |

**Phase 8B — Effort-Impact Scoring**

For each issue, assign both a priority level AND an effort level:

| Effort Level  | Definition                                                                 |
|---------------|----------------------------------------------------------------------------|
| QUICK         | The fix takes under 30 minutes: add a sentence, rephrase a heading, add attribution |
| MODERATE      | The fix takes 1–3 hours: add an FAQ section, restructure a section, add 2–3 examples |
| MAJOR         | The fix takes 3+ hours: full section rewrite, fundamental restructuring, adding a case study |

The optimization checklist sorts within each priority level by EFFORT ascending — quick wins first within each priority tier.

---

### ▸ STEP 9 — OPTIMIZATION CHECKLIST PRODUCTION

Consolidate all findings into the structured optimization checklist (Part 1 of output).

**Phase 9A — Checklist Assembly**

For every issue identified across Steps 2–8:
1. Assign its priority (HIGH / MEDIUM / LOW / OPPORTUNITY).
2. Assign its effort level.
3. Write the specific issue description.
4. Write the specific fix (one or two sentences — actionable without additional research).
5. Identify the affected section.
6. State the expected impact.

**Phase 9B — Checklist Ordering**

Sort the checklist:
1. HIGH priority issues first.
2. Within HIGH: QUICK effort before MODERATE before MAJOR.
3. MEDIUM priority issues second.
4. Within MEDIUM: same effort ordering.
5. LOW priority issues third.
6. OPPORTUNITY items last (labeled separately).

**Phase 9C — Recommendations Summary (Part 3)**

Beyond the checklist, produce a thematic improvements summary organized by editorial category:

```
RECOMMENDED IMPROVEMENTS SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CONTENT DEPTH IMPROVEMENTS ([N] recommendations):
  → [Summary of depth-related improvements]

STRUCTURAL IMPROVEMENTS ([N] recommendations):
  → [Summary of structural fixes]

KEYWORD OPTIMIZATION ([N] recommendations):
  → [Summary of keyword placements and coverage]

AEO IMPROVEMENTS ([N] recommendations):
  → [Summary of AEO-related enhancements]

SERP ALIGNMENT ([N] recommendations):
  → [Summary of SERP comparison gaps to close]

ENTITY COVERAGE ([N] recommendations):
  → [Summary of entity gaps to fill]

QUICK WINS (Can be implemented today):
  → [List of all QUICK effort fixes regardless of priority]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 10 — REWRITE SUGGESTIONS

For every HIGH priority issue that requires writing (not just structural changes), provide a specific rewritten version of the affected section.

**Rewrite trigger conditions:** Rewrite suggestions are produced for HIGH priority issues where:
- The current text can be shown verbatim (original) alongside the improved version.
- The improvement requires phrasing changes — not just structural changes.
- The rewrite demonstrates specifically how to fix the issue.

**Rewrite suggestion format:**

```
REWRITE SUGGESTION — Issue #[N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Issue           : [issue description]
Affected        : [Introduction / H2 "[section name]" / Direct Answer Block / FAQ #N]

ORIGINAL TEXT:
"[exact original text from the content]"

IMPROVED VERSION:
"[specific rewritten version that directly fixes the issue]"

What changed    : [specific description of what was improved and why]
Keywords added  : [any keywords added in the rewrite — none removed]
Entities added  : [any entities added — none removed]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Rewrite Quality Rules:**

1. Every rewrite must fix the identified issue — not introduce new issues.
2. Rewrites preserve the original factual content — they do not change information.
3. Rewrites do not fabricate statistics — if a statistic is missing, the rewrite notes where one should go with `[STATISTIC: type of data needed here]`.
4. Rewrites for the introduction follow the funnel-specific intro models from `content-generation-engine.skill`.
5. Rewrites for AEO sections must produce a self-contained answer in ≤60 words.
6. Rewrites for missing sections provide a structural outline with content guidance — they do not write the full section unless the section is short (< 200 words).

**Missing Section Suggestions:**

When a HIGH priority issue identifies a missing section that the article should have:

```
MISSING SECTION SUGGESTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Section Topic   : [what this section should cover]
Where to Insert : [after H2 "[section name]" / before FAQ]
Suggested H2    : "[specific heading for the new section]"
Required Content: [specific list of what this section must include]
  → [Sub-topic 1: what to cover]
  → [Sub-topic 2: what to cover]
  → [Visual element: if applicable]
Keyword to Target: [secondary keyword this section should incorporate]
Estimated Words : [target word range]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ SERP PATTERN COMPARISON (ADVANCED)

When `content-pattern-extractor.skill` output is available, apply a structured SERP pattern comparison.

**Pattern Comparison Protocol:**

1. Load the Must-Have pattern signals from `content-pattern-extractor.skill`.
2. For each Must-Have signal (≥60% of SERP pages) → check if the analyzed content has it.
3. For each Good-to-Have signal (30–59%) → check and recommend if absent.
4. For each Opportunity signal (<30%) → flag as DIFFERENTIATION OPPORTUNITY.

**Pattern Comparison Output Format:**

| SERP Pattern Signal                      | Frequency | Present in Content | Action Required     |
|------------------------------------------|-----------|--------------------|---------------------|
| [Pattern type]                           | [N%]      | YES / NO           | [none / add / flag] |

Patterns absent from the content that are Must-Have in the SERP → automatically classify as MEDIUM or HIGH priority issues depending on frequency.

---

### ▸ ENTITY GAP DETECTION (ADVANCED)

Beyond the basic entity coverage check in Step 4, the entity gap detection module compares entity coverage against competitor entity density.

**Entity Gap Detection Method:**

1. Load the entity map for this keyword's cluster.
2. For each Critical entity → is it present in the content AND in the competitors' content?
3. For each entity present in competitors but absent from the analyzed content → flag as COMPETITIVE ENTITY GAP.
4. For each entity absent from BOTH the analyzed content AND competitors → flag as DIFFERENTIATION ENTITY OPPORTUNITY (add it and own it).

**Entity Gap Priority:**

| Entity Type          | Present in Competitors | Absent from Content | Priority             |
|----------------------|------------------------|---------------------|----------------------|
| Critical entity      | YES                    | YES                 | HIGH — competitive gap |
| Critical entity      | NO                     | YES                 | MEDIUM — differentiation opportunity |
| Important entity     | YES                    | YES                 | MEDIUM — competitive gap |
| Important entity     | NO                     | YES                 | LOW — optional differentiation |

---

### ▸ READABILITY OPTIMIZATION (ADVANCED)

Detect and flag readability issues beyond the basic sentence complexity check in Step 3.

**Readability Detection Checks:**

| Check                                                             | Issue                                    | Priority |
|-------------------------------------------------------------------|------------------------------------------|---------|
| Three+ consecutive paragraphs of identical length (~words)       | Rhythm monotony                          | LOW     |
| More than 3 sections with no H3 sub-structure                    | Flat structure — hard to scan            | LOW     |
| More than 2 adjacent paragraphs starting with the same word      | Opening word repetition                  | LOW     |
| More than one sentence exceeding 45 words in the same section    | Complexity clusters                      | MEDIUM  |
| Any section with 500+ words and no visual element                | Reading fatigue risk                     | LOW     |
| Generic transition phrases (from humanizer.skill prohibited list) | AI pattern residue                       | MEDIUM  |
| Introduction uses a prohibited opener pattern                    | Low-engagement opening                   | MEDIUM  |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Recommendations

If the same fix would address two separate issues (e.g., adding a direct answer block would fix both "missing AEO section" and "intro does not answer query") → create ONE issue entry with both root causes documented. Do not create two separate checklist entries that recommend the same action.

### Rule 2 — Merge Similar Issues

Issues that share the same root cause are merged into one compound issue record. Example: "Missing statistic in Section 2" and "Missing statistic in Section 4" → merged into "Missing statistics in sections 2 and 4 — add at least one sourced data point to each."

Merging is applied when:
- Same issue type.
- Same fix category.
- Same expected impact.
- Affects multiple locations but requires the same type of content addition.

### Rule 3 — No Duplicate Rewrite Suggestions

If a rewrite suggestion for Issue #1 already incorporates the fix needed for Issue #3 (because they affect the same passage) → the rewrite for Issue #1 is annotated to note it also addresses Issue #3. No separate rewrite is produced for Issue #3.

### Rule 4 — No Generic + Specific Redundancy

If the checklist contains a specific issue ("Primary keyword absent from Section 2 H2 heading") AND would also generate a general issue ("Low keyword density"), do not include the general issue separately. The specific issue subsumes it. Specificity always wins over generality in issue reporting.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Optimization Behaviors

| Prohibited Behavior                                                                             | Reason                                                                            |
|-------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Recommending "add more content" without specifying what content and where                       | This is noise — every recommendation must be specific                            |
| Recommending "improve your introduction" without showing the specific problem                   | Vague structure advice is not actionable                                          |
| Flagging a section as "could be improved" without identifying what is wrong                     | Uncertainty is not a finding — skip if you cannot be specific                    |
| Recommending "add keywords naturally" without naming specific placements                        | Keyword recommendations must identify the exact section and opportunity           |
| Providing a LOW priority recommendation before all HIGH and MEDIUM issues are listed           | Checklist ordering is strict — HIGH comes before MEDIUM comes before LOW         |
| Generating a rewrite suggestion for a LOW priority issue                                        | Rewrite suggestions are for HIGH issues only                                     |
| Providing a rewrite suggestion that changes the factual content                                 | Rewrites fix expression — not factual claims (accuracy is `technical-accuracy-checker.skill`'s domain) |
| Fabricating statistics in rewrite suggestions                                                   | Rewrites use `[STATISTIC: type needed]` placeholders — never invented data       |
| Producing optimization recommendations for a content piece that has not been fully analyzed    | All 10 steps must run before the checklist is finalized                          |
| Labeling a change as HIGH priority because it "feels important" without a specific ranking reason | Every HIGH priority designation must trace to a specific ranking impact signal  |

### Required Optimization Behaviors

- Every issue must have a specific fix that can be implemented without additional research.
- Every fix must state the expected impact in terms of content performance.
- Every rewrite suggestion must show both the original and the improved version.
- Missing sections must include structural outlines, not blank recommendations.
- The content grade must trace to specific issue counts — not subjective assessment.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source (can provide the original content when this skill is used for post-generation optimization) |
| **Receives**     | Generated article + SEO brief + blueprint                                                  |
| **Used for**     | Step 3 (structural audit against blueprint), Step 4 (keyword coverage from brief)          |

---

### ▸ entity-extraction.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream enrichment source                                                                  |
| **Receives**     | Entity map — Critical/Important/Optional entities for the keyword's cluster                |
| **Used for**     | Step 4 (entity coverage audit), Advanced Logic (entity gap detection)                      |

---

### ▸ serp-blueprint-generator.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream reference source                                                                   |
| **Receives**     | Content blueprint — expected structure, AEO plan, content elements plan                    |
| **Used for**     | Step 3 (structural audit against expected blueprint), Step 7 (SERP pattern comparison)    |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream intelligence source                                                                |
| **Receives**     | Dominant intent, SERP features, content direction, difficulty, PAA questions               |
| **Used for**     | Step 2 (intent alignment), Step 7 (SERP comparison depth and structure benchmarks)        |

---

### ▸ content-pattern-extractor.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream enrichment source (optional)                                                       |
| **Receives**     | Must-Have/Good-to-Have/Opportunity pattern signals from SERP                               |
| **Used for**     | Advanced Logic — SERP pattern comparison                                                   |

---

### ▸ content-memory.skill (via memory-controller)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after optimization report is complete)                                              |
| **Writes**       | Optimization report, issues list, content grade, recommendations                          |
| **Used for**     | Recording the optimization state of this content piece for future sessions                 |
| **Lifecycle update** | Content status updated to OPTIMIZED or OPTIMIZATION PENDING (based on user action)   |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                            |
|---------------------------------------------------------------------------|--------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| No content provided                                                       | Input — content field is null                          | Display clarification prompt (Section 2 Input Clarification Protocol). Halt until content is provided.    |
| URL provided but content cannot be extracted                              | Phase 1A — URL returns no extractable text             | Ask user: "I cannot access the content at this URL. Please paste the article text directly."              |
| Target keyword not provided                                               | Input — target_keyword is null                        | Ask user: "What keyword is this content targeting? I need the target keyword to evaluate intent alignment and keyword coverage." |
| Content is extremely short (< 300 words)                                  | Phase 1C — word count below minimum                   | Flag: "Content is too short to produce a meaningful optimization analysis. Provide the complete article or a more complete outline." |
| SERP data is completely unavailable for this keyword                     | Step 7 — all sources return null                       | Apply SERP inference from keyword pattern. Flag all SERP comparison findings as INFERRED. Note: "SERP comparison based on keyword pattern inference — reduced confidence. Run serp-intelligence.skill for confirmed signals." |
| Entity map is unavailable                                                  | Step 4D — entity_map is null                          | Skip entity coverage audit. Flag: "Entity coverage analysis skipped — entity-extraction.skill data unavailable. Run entity-extraction.skill for this keyword to add entity coverage to this optimization." |
| Optimization produces zero HIGH priority issues                           | Step 8 — no HIGH issues detected                       | This is a valid result — the content may be well-optimized. Report: "No HIGH priority issues detected. Content is generally well-aligned with the keyword. See MEDIUM and LOW improvements for polishing." Grade = A or B. |
| Content is in a language other than the analysis language                 | Phase 1A — character patterns indicate non-English    | Flag: "Content appears to be in a language other than English. Optimization analysis may be limited. Confirm language and keyword alignment before proceeding." |
| All issues detected are in the OPPORTUNITY category (no standard issues)  | Step 8 — all issues are differentiation gaps         | This is positive — content is competitive but has room for first-mover differentiation. Report: "Content matches SERP patterns well. Opportunities to differentiate are outlined below." |
| Content is an outline with no body text                                   | Phase 1A — outline format detected                    | Limit analysis to structural checks (Steps 2, 3, 6–9). Skip depth analysis (Step 5) and readability checks. Flag all depth-related checks as "OUTLINE ONLY." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — OPTIMIZATION CHECKLIST ITEM QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
OPTIMIZATION CHECKLIST STRUCTURE — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Every checklist item must contain:

1. PRIORITY (one of): HIGH / MEDIUM / LOW / OPPORTUNITY
2. EFFORT (one of): QUICK (<30min) / MODERATE (1-3hr) / MAJOR (3hr+)
3. ISSUE: Specific description — names the element and what is wrong
4. FIX: Specific action — one or two sentences that tell a writer exactly what to do
5. SECTION: Where in the article this fix applies
6. EXPECTED IMPACT: What changes in ranking or user experience when this is fixed

PROHIBITED ISSUE DESCRIPTIONS:
  ✗ "Add more content"
  ✗ "Improve the introduction"
  ✗ "Better keyword usage"
  ✗ "Could be improved"
  ✗ "Consider adding..."

REQUIRED ISSUE DESCRIPTIONS (examples):
  ✓ "Primary keyword absent from H1 — H1 currently reads '[text]' with no keyword variant"
  ✓ "Direct answer block missing — no ≤60-word answer appears before the Table of Contents"
  ✓ "Section 3 has no examples — only abstract claims without concrete scenarios"
  ✓ "FAQ answers average 120 words — too long for featured snippet eligibility (max 80w)"
  ✓ "[Entity Name] not mentioned — Critical entity for this keyword's cluster"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — CONTENT GRADE SCORING GUIDE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CONTENT GRADE SCORING GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GRADE A:
  HIGH issues: 0
  MEDIUM issues: ≤ 3
  Interpretation: Content is well-optimized and competitive.
  Publishing recommendation: Implement LOW improvements when capacity allows.

GRADE B:
  HIGH issues: 1–2
  MEDIUM issues: 4–7
  Interpretation: Content is functional but has clear improvement areas.
  Publishing recommendation: Fix HIGH issues first; then top MEDIUM issues.

GRADE C:
  HIGH issues: 3–5
  Interpretation: Content has meaningful gaps that limit ranking potential.
  Publishing recommendation: Fix all HIGH issues before expecting competitive rankings.

GRADE D:
  HIGH issues: ≥ 6
  Interpretation: Content has fundamental problems — structural, intent, or depth failures.
  Publishing recommendation: Major revision required. Consider regeneration with content-generation-engine.skill.

SPECIAL CONDITIONS:
  Intent mismatch (any) → Maximum grade is C, even with few other issues.
  Missing direct answer block (for AEO keyword) → Automatically MEDIUM or lower.
  Word count < 50% of SERP average → Maximum grade is C.

GRADE OVERRIDE RULE:
  If the content scores B by issue count but has a FULL intent mismatch → grade is C.
  Grade reflects the single most impactful limiting factor when override conditions apply.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — TEN-STEP ANALYSIS QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
TEN-STEP ANALYSIS QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Step 1  — CONTENT INTAKE
  Extract: H1, intro, H2/H3 structure, FAQ, CTA, word count
  Output: Content inventory summary

Step 2  — INTENT ALIGNMENT
  Check: Keyword intent vs. content intent
  Mismatch: HIGH priority issue — critical ranking blocker

Step 3  — STRUCTURAL AUDIT
  Check: H1, intro, heading structure, logical flow, FAQ presence
  Issues: H1 missing (HIGH); intro doesn't answer query (HIGH); missing H2s (HIGH)

Step 4  — KEYWORD COVERAGE
  Check: Primary keyword placement, density, semantic coverage, entity coverage
  Issues: Missing from H1 (HIGH); low density (MEDIUM); missing entities (HIGH/MEDIUM)

Step 5  — QUALITY GAP DETECTION
  Check: Examples, case studies, statistics, comparisons, actionable steps
  Issues: Missing all depth elements (HIGH); generic examples (MEDIUM)

Step 6  — AEO AUDIT
  Check: Direct answer block, AEO sections, FAQ quality
  Issues: Missing answer block (HIGH); FAQ too long (MEDIUM); no FAQ (MEDIUM)

Step 7  — SERP COMPARISON
  Check: Word count vs. SERP average, structure patterns, differentiation gaps
  Issues: Significantly shorter (HIGH); missing Must-Have patterns (MEDIUM)

Step 8  — PRIORITIZATION
  Classify: HIGH / MEDIUM / LOW / OPPORTUNITY
  Sort: Within each priority — QUICK effort before MODERATE before MAJOR

Step 9  — OPTIMIZATION CHECKLIST
  Produce: Prioritized table + improvement summary + quick wins
  Format: Priority | Issue | Fix | Section | Expected Impact

Step 10 — REWRITE SUGGESTIONS
  Produce: Specific rewrites for HIGH issues only
  Format: Original text → Improved version + what changed
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: content/optimization-engine.skill*
*Nexus SEO Operating System — Version 1.0.0*
