# SKILL: content/content-validation.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Final Quality Gate
# EXECUTION PRIORITY: TERMINAL POST-PROCESSING — Runs last in the post-generation pipeline, after humanizer.skill and technical-accuracy-checker.skill. Nothing exits the system without passing this gate.
# POSITION: The final gate. Every piece of content produced by the Nexus system must pass through this skill before delivery to the user or output-formatter.skill.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **final quality gate** of the Nexus content pipeline.

It receives the fully processed article — after generation, humanization, and technical accuracy verification — and applies an eleven-step validation protocol to confirm that the content is complete, structurally correct, free of duplication, keyword-compliant, AEO-optimized, conversion-aligned, and clean for delivery. It validates every commitment made by the upstream pipeline: that the structure matches the blueprint, the word count is within tolerance, the keywords are placed correctly, the AEO sections are properly formed, the conversion layer is present and funnel-aligned, the accuracy corrections were applied, and the humanization removed robotic patterns.

When a validation step finds a fixable issue — a missing FAQ, a structural placeholder, a keyword gap, a word count shortfall — this skill fixes it directly rather than returning the content for regeneration. Only issues that require a complete regeneration or a content-level strategic decision are escalated as BLOCKERS. Everything else is caught, logged, fixed, and documented.

The validation report this skill produces is the article's final quality certificate. It is the record that tells the content team, the output formatter, and the memory controller exactly what was checked, what was fixed, what was flagged, and what the article's overall readiness status is.

### Primary Responsibilities

| Responsibility                          | Description                                                                                                            |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| **Structure Validation**                | Verify all mandatory structural components are present — SEO brief, metadata, blueprint, article, FAQs, AEO sections  |
| **Word Count Validation**               | Measure prose-only word count; compare to target; adjust content (depth, not filler) if within adjustment threshold    |
| **Duplication Detection and Removal**   | Find and remove repeated ideas, repeated paragraphs, and repeated heading labels                                       |
| **Keyword Usage Validation**            | Confirm primary keyword presence in H1, intro, and 1–2 H2 headings; verify no stuffing                               |
| **Readability Check**                   | Flag complex sentences, broken flow, and illogical section progression                                                 |
| **SEO Completeness Verification**       | Confirm internal links, entity coverage, heading structure, and external reference adequacy                            |
| **AEO Validation**                      | Verify direct answer block, 2 AEO sections, and 5 FAQs — each properly formed and self-contained                     |
| **Conversion Layer Verification**       | For MOFU and BOFU content: confirm CTA presence, value proposition clarity, and trust elements                        |
| **Accuracy Corrections Confirmation**   | Verify all corrections from `technical-accuracy-checker.skill` were applied; confirm no unresolved issues remain      |
| **Humanization Verification**           | Confirm robotic tone markers were removed; natural language is present                                                 |
| **Final Cleanup**                       | Remove formatting artifacts, clear placeholder text, standardize structural formatting                                 |
| **Readiness Status Assignment**         | Assign overall readiness status: READY / READY WITH FLAGS / NOT READY                                                 |
| **Completeness Scoring**                | Calculate a 0–100% completeness score across all eleven validation dimensions                                          |

### What This Skill Is NOT

- It is **not** a regeneration engine. It fixes minor issues in place — it does not regenerate sections from scratch.
- It is **not** a content strategist. It validates against the blueprint and user requirements — not against new strategic ideas.
- It is **not** optional. No content produced by the Nexus pipeline reaches `output-formatter.skill` or the user without passing through this gate.
- It is **not** a first-pass quality check. By the time content reaches this skill, it has already been humanized and accuracy-checked. This skill validates that all prior pipeline steps executed correctly and their requirements are met.
- It is **not** a gatekeeper that blocks on minor issues. The goal is to fix what can be fixed and clearly document what cannot.

### The Defining Standard of This Skill

> This skill has one mandate: ensure that what leaves the system is what was asked for.
>
> "What was asked for" means:
> → The structure the blueprint specified.
> → The word count the user requested.
> → The keywords placed as required.
> → The AEO elements that make the content snippet-eligible.
> → The conversion layer appropriate for the funnel stage.
> → The accuracy level the technical checker confirmed.
> → The natural language the humanizer produced.
>
> Every validation step is a specific, measurable check.
> Every finding is either FIXED (documented in the issues log) or FLAGGED (in the validation report).
> Nothing is silently ignored.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                           | Description                                                                                        | Required |
|---------------------------------|----------------------------------------------------------------------------------------------------|----------|
| `article_content`               | The fully processed article — post-humanization and post-accuracy-check                           | YES      |
| `metadata_package`              | The complete metadata output from `metadata-generator.skill` — all parts                          | YES      |
| `blueprint`                     | The annotated content blueprint from `serp-blueprint-generator.skill` — H1–H3 structure, AEO plan, element placements | YES |
| `seo_brief`                     | The SEO brief from `content-generation-engine.skill` Part 1 — funnel, keyword, audience, priority | YES      |
| `user_configuration`            | The five-question configuration from the pre-execution intake — word count, funnel, audience, competitors, source of truth | YES |

### Upstream Pipeline Outputs (Required for Cross-Layer Validation)

| Field                           | Description                                                                                        | Required |
|---------------------------------|----------------------------------------------------------------------------------------------------|----------|
| `accuracy_corrections_log`      | The corrections log from `technical-accuracy-checker.skill`                                       | YES      |
| `accuracy_confidence_score`     | The confidence score (0–100%) from `technical-accuracy-checker.skill`                             | YES      |
| `unverified_claims_list`        | The unverified claims list from `technical-accuracy-checker.skill`                                | YES      |
| `humanizer_transformation_log`  | The transformation log from `humanizer.skill`                                                     | SOFT     |
| `humanizer_flags`               | Any phrases flagged for human review by `humanizer.skill`                                         | SOFT     |

### Optional Inputs

| Field                           | Description                                                                 | Used For                                              |
|---------------------------------|-----------------------------------------------------------------------------|-------------------------------------------------------|
| `entity_map`                    | Entity map from `entity-extraction.skill`                                   | Step 6 (entity coverage validation)                   |
| `cluster_architecture`          | Cluster data from `semantic-clustering.skill`                               | Step 6 (internal linking cluster alignment)           |
| `keyword_intelligence`          | Full keyword set (secondary, long-tail, LSI) from `metadata-generator.skill` | Step 4 (keyword usage validation)                   |
| `internal_linking_plan`         | Internal linking plan from `serp-blueprint-generator.skill`                 | Step 6 (internal link presence check)                |

### Input Pre-Conditions

1. **Is `article_content` present?** If null → halt. Validation requires an article.
2. **Have upstream pipeline skills completed?** Verify `accuracy_corrections_log` and `accuracy_confidence_score` are present. If missing → proceed with article validation only; flag: "Accuracy validation data absent — cross-layer accuracy confirmation skipped."
3. **Is the `blueprint` available?** If missing → perform structure validation against known content standards (H1–H3 hierarchy, FAQ presence, AEO presence) without blueprint-specific comparisons. Flag all blueprint-dependent checks as BLUEPRINT ABSENT.
4. **What is the `accuracy_confidence_score`?** If < 70% → escalate validation strictness for all factual claims. If < 50% → recommend NOT READY status before validation begins unless user explicitly confirmed proceeding.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Validated Final Content Package

```
NEXUS CONTENT VALIDATION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Keyword          : [keyword]
Funnel Stage            : [TOFU / MOFU / BOFU]
Target Word Count       : [N words]
Validated Word Count    : [N words]
Completeness Score      : [N]% — [READY / READY WITH FLAGS / NOT READY]
Accuracy Confidence     : [N]% (from technical-accuracy-checker.skill)
Validation Steps Run    : 11 / 11
Issues Found            : [count]
  Auto-Fixed            : [count]
  Flagged (user action) : [count]
  Blockers              : [count]
Overall Status          : [READY / READY WITH FLAGS / NOT READY]
Generated By            : content/content-validation.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP-BY-STEP VALIDATION RESULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Step 1  — Structure Validation     : [PASS / FIXED / FLAGGED / BLOCKER]
Step 2  — Word Count Validation    : [PASS / FIXED / FLAGGED / BLOCKER]
Step 3  — Duplication Check        : [PASS / FIXED / FLAGGED]
Step 4  — Keyword Validation       : [PASS / FIXED / FLAGGED]
Step 5  — Readability Check        : [PASS / FIXED / FLAGGED]
Step 6  — SEO Completeness         : [PASS / FIXED / FLAGGED]
Step 7  — AEO Validation           : [PASS / FIXED / FLAGGED / BLOCKER]
Step 8  — Conversion Layer Check   : [PASS / FIXED / FLAGGED / N/A-TOFU]
Step 9  — Accuracy Confirmation    : [PASS / FIXED / FLAGGED / BLOCKER]
Step 10 — Humanization Check       : [PASS / FIXED / FLAGGED]
Step 11 — Final Cleanup            : [PASS / FIXED]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ISSUES FIXED LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Issues Fixed Log Format in Section 3]

FLAGS FOR USER ACTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Flags Log Format in Section 3]

BLOCKERS (PREVENT PUBLISHING)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Blocker Log Format in Section 3]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[VALIDATED FINAL ARTICLE CONTENT]
```

### Issues Fixed Log Format

```
ISSUES FIXED LOG ENTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Fix #           : [sequential]
Validation Step : [Step N — Name]
Issue Type      : [MISSING ELEMENT / WORD COUNT / DUPLICATE / KEYWORD GAP / PLACEHOLDER / FORMATTING]
Location        : [Section name + specific position]
Description     : [what was wrong — specific]
Action Taken    : [what this skill did to fix it — specific]
Result          : [what the content looks like after the fix]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Flags Log Format (User Action Required)

```
FLAG ENTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Flag #          : [sequential]
Validation Step : [Step N — Name]
Flag Type       : [UNVERIFIED CLAIM / WORD COUNT NEAR LIMIT / HUMANIZER REVIEW / ACCURACY CONCERN / CONVERSION MISSING]
Location        : [Section name + specific position]
Description     : [what was found — specific]
Risk Level      : [HIGH / MEDIUM / LOW]
Recommended Action: [what the content team should do — specific]
Can Publish     : [YES — with awareness / REVIEW FIRST]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Blocker Log Format (Must Resolve Before Publishing)

```
BLOCKER ENTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Blocker #       : [sequential]
Validation Step : [Step N — Name]
Blocker Type    : [STRUCTURAL FAILURE / ACCURACY FAILURE / MISSING CRITICAL ELEMENT / UNRESOLVED INACCURACY / SEVERE DUPLICATION]
Location        : [Section name or "Whole Article"]
Description     : [what is blocking delivery — specific]
Why Blocker     : [why this cannot be published in its current state]
Resolution Path : [what must happen to resolve this blocker]
Requires        : [CONTENT TEAM ACTION / FULL REGENERATION / USER DECISION]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Overall Readiness Status Definitions

| Status              | Condition                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------|
| **READY**           | All 11 steps PASS or FIXED; zero blockers; zero flags or only LOW-risk flags                       |
| **READY WITH FLAGS** | All 11 steps PASS or FIXED; zero blockers; 1+ MEDIUM or HIGH-risk flags that require review before publishing |
| **NOT READY**       | 1+ BLOCKER exists; OR accuracy confidence score < 50%; OR structural failure cannot be auto-fixed  |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — STRUCTURE VALIDATION

Verify that all mandatory structural components of the content package are present. This step validates the production package as a whole — not just the article body.

**Phase 1A — Package Component Checklist**

Verify presence of each component in the full production package:

| Component                       | Required | Pass Condition                                                          |
|---------------------------------|----------|-------------------------------------------------------------------------|
| SEO Brief (Part 1 of package)   | YES      | All required fields present: blog topic, funnel, priority, audience, primary keyword, secondary keywords, long-tail keywords, LSI keywords |
| Metadata Package (Part 2)       | YES      | Meta title, meta description, URL slug — at minimum one option each    |
| Content Blueprint (Part 3)      | YES      | H1 present; at least 4 H2 sections; TOC present; AEO positions labeled |
| Full Article (Part 4)           | YES      | Article text is present and non-empty                                   |
| Internal Linking Plan (Part 5)  | YES      | At minimum 3 internal link suggestions with section placement          |
| Visual Element Plan (Part 6)    | YES      | At minimum Must-Have visual elements from SERP patterns are specified  |
| Validation Report (Part 7)      | YES      | This document — auto-present when this skill runs                       |

**Phase 1B — Article Structure Validation**

Within the article body itself, verify structural integrity:

**H1 Validation:**
- Exactly one H1 tag present.
- H1 contains the primary keyword (exact or close semantic variation).
- H1 is 6–12 words (ideal range) — flag if outside range but do not block.

**Direct Answer Block Validation:**
- Present immediately after H1 and before the Table of Contents.
- Is 2–3 sentences and ≤60 words.
- Answers the primary keyword's implied question directly.
- Does NOT begin with any of the prohibited openers from `content-generation-engine.skill`.

**Table of Contents Validation:**
- Present and lists all H2 sections.
- TOC entries match the actual H2 headings in the article (no TOC entry without a corresponding H2; no H2 without a TOC entry — unless the H2 was added as a structural expansion during word count adjustment).

**H2 Section Validation:**
- Minimum 4 H2 sections (or the number specified in the blueprint).
- Maximum 7 H2 sections (blueprint maximum).
- No two H2 sections have the same title.
- H2 headings are descriptive and topic-specific — never "Introduction," "Overview," "Conclusion" as the only H2 label.

**H3 Sub-Section Validation:**
- Every H3 is semantically subordinate to its parent H2.
- No H3 appears without a parent H2.
- H3 headings are more specific than their parent H2 headings.

**FAQ Section Validation:**
- Present in the article.
- Contains exactly 5 questions and answers (or flags if count differs).
- Evaluated in detail in Step 7 (AEO Validation).

**Phase 1C — Structure Issue Resolution**

For each structural issue found:

| Issue Type                                  | Auto-Fix                                               | Block if Not Fixed? |
|---------------------------------------------|--------------------------------------------------------|---------------------|
| SEO Brief field missing                     | Flag for content-generation-engine to add             | YES — missing brief = incomplete package |
| Metadata option missing                     | Flag — cannot auto-generate metadata here             | NO — at least one of each element must exist |
| Direct answer block absent                  | Flag as MISSING CRITICAL ELEMENT                      | YES — AEO compliance requires it |
| TOC missing                                 | Generate TOC from existing H2 headings                | NO — auto-fixable |
| TOC entries don't match H2 headings        | Update TOC to match existing H2 headings              | NO — auto-fixable |
| H1 missing primary keyword                 | Flag — cannot change H1 without content team review  | NO — flag only |
| Multiple H1 tags                            | Downgrade extras to H2 or bold text as appropriate   | YES if more than 2 |
| H3 without parent H2                        | Promote to H2 if it has sufficient content; merge with prior H2 if not | NO — auto-fixable |
| FAQ section absent                          | Flag as MISSING AEO ELEMENT — requires generation     | YES |
| Two identical H2 titles                     | Flag both; suggest one rename                         | NO — flag only |

---

### ▸ STEP 2 — WORD COUNT VALIDATION

Measure the article's prose-only word count and compare it to the user-specified target. Adjust if necessary.

**Phase 2A — Word Count Measurement Protocol**

**What to COUNT:**
- All prose sentences and paragraphs.
- H1, H2, H3 heading text.
- Substantive list item text (complete sentences; not single-word labels).
- FAQ question and answer text.
- CTA text.
- Introduction and conclusion text.

**What to EXCLUDE:**
- Image alt text and image captions.
- Table header and cell content.
- Code block contents (any code, CLI, configuration).
- URL text appearing as a standalone link.
- Placeholder markers: `[IMAGE: ...]`, `[TABLE: ...]`, `[CODE: ...]`.
- Navigation or breadcrumb text.
- Any text explicitly marked as non-prose structural element.

**Phase 2B — Count Verification**

Run the measurement on the article after Step 1 structural fixes are applied. Record:
- Section-by-section word count breakdown.
- Total prose word count.
- Delta vs. target: (actual - target) / target × 100 = variance %.

**Phase 2C — Variance Thresholds and Actions**

| Variance             | Status       | Action                                                                                  |
|----------------------|--------------|-----------------------------------------------------------------------------------------|
| ±5% of target        | PASS         | No action required                                                                      |
| -6% to -15%          | EXPAND       | Identify the two shortest H3 sections; add one depth element each (example or mechanism explanation) |
| -16% to -25%         | EXPAND MORE  | Add one new H3 sub-section to the article's broadest H2 section; expand the two weakest H3s |
| > -25% under target  | MAJOR GAP    | Flag as BLOCKER if variance > -30%; flag as FLAG if between -25% and -30%             |
| +6% to +15%          | TRIM         | Apply filler sentence removal pass; remove the weakest concluding sentence in each section |
| +16% to +25%         | TRIM MORE    | Remove the weakest H3 sub-section; condense the two most redundant sections             |
| > +25% over target   | OVER-LENGTH  | Flag as FLAG — content team to decide which sections to condense                        |

**Phase 2D — Expansion Quality Rule**

When expanding to meet word count:

**Allowed expansion methods (in priority order):**
1. Add a specific, concrete example illustrating a claim already in the section.
2. Add a mechanism explanation — how something works, not just what it does.
3. Add a real-world application or use case for the concept being discussed.
4. Add a contrast or comparison that enriches understanding.
5. Expand a step or process explanation with a clarifying sub-step.

**Prohibited expansion methods:**
- Restating the previous paragraph in different words.
- Adding transitional sentences that add no information.
- Repeating statistics or examples already used.
- Adding generic "as mentioned earlier" continuations.
- Padding with "It is important to understand that..." + restatement of existing content.

**The expansion must be validated for quality:** After every auto-expansion, the new content is assessed — if it reads as filler, it is removed and the word count shortfall is flagged instead.

---

### ▸ STEP 3 — DUPLICATION CHECK

Detect and remove repeated content at three levels: idea level, phrase level, and heading level.

**Phase 3A — Heading Duplication Detection**

Compare all H2 titles against each other and all H3 titles within each H2 section:

- **Exact duplicate headings:** Same heading label in two places → rename the duplicate to distinguish them. If they cannot be distinguished (same topic) → merge the sections.
- **Semantically equivalent headings:** Two headings that describe the same content from slightly different angles → evaluate the sections for merge or differentiation.
- **H3 duplicates within same H2:** Two sub-sections under the same H2 covering identical territory → merge.

**Phase 3B — Paragraph-Level Phrase Duplication**

Scan for phrase repetition across the article:

| Duplication Level                                           | Detection Rule                                        | Action                                             |
|-------------------------------------------------------------|-------------------------------------------------------|-----------------------------------------------------|
| Identical paragraph (≥50 words, near-identical)            | Found in two locations                                | Remove the second instance; note the removal        |
| Identical phrase of 7+ words                               | Appears more than once (excluding quotes and proper nouns) | Rephrase the second instance              |
| Same concept explained in full in two different sections   | Two sections both fully explain the same idea         | Keep the fuller explanation; cross-reference from the shorter |
| Same statistic presented twice                              | Same specific number with the same attribution appears twice | Remove the second instance; first attribution stands |

**Phase 3C — Idea-Level Duplication Detection**

Beyond phrasing, detect sections that overlap conceptually:

For each pair of H2 sections, ask: "Do both sections ultimately answer the same reader question?"

| Result        | Action                                                                      |
|---------------|-----------------------------------------------------------------------------|
| YES           | Merge the shorter section into the longer one; create H3 sub-sections if needed |
| PARTIAL       | Identify where the overlap begins; trim the overlapping content from the second section |
| NO            | No action — sections are sufficiently distinct                             |

**Phase 3D — FAQ Duplication Check**

All 5 FAQ questions must address distinct sub-intents:

- Compare every FAQ pair for semantic overlap (do both questions ask the same thing from different angles?).
- If two FAQs overlap ≥70% semantically → merge into one comprehensive question-answer pair.
- If merging reduces the FAQ count below 5 → flag: "FAQ deduplication reduced count to [N] — content team should add [5-N] new questions addressing distinct sub-intents."

---

### ▸ STEP 4 — KEYWORD USAGE VALIDATION

Confirm that keyword usage is correct, complete, and natural throughout the article.

**Phase 4A — Primary Keyword Placement Validation**

Verify the primary keyword appears in every required position:

| Required Position                           | Pass Condition                                                      | Issue if Failing              |
|---------------------------------------------|---------------------------------------------------------------------|-------------------------------|
| H1 heading                                  | Primary keyword (exact or close variation) present in H1           | FLAG — cannot auto-fix H1    |
| Introduction (first 100 words)             | Primary keyword appears within the first 100 words of body text    | AUTO-FIX — add natural instance in intro if absent |
| 1–2 H2 headings                             | At least 1 H2 heading naturally contains the primary keyword       | FLAG — suggest H2 adjustment |
| Meta title                                  | Primary keyword in meta title (validated in metadata package)      | FLAG — cross-reference with metadata |

**Phase 4B — Keyword Density Analysis**

Count the total occurrences of the primary keyword (exact form + close variations) throughout the article:

| Count                          | Status       | Action                                                                   |
|--------------------------------|--------------|--------------------------------------------------------------------------|
| 0 occurrences                  | CRITICAL GAP | Add naturally in intro and one H2 section; flag location                 |
| 1–3 occurrences                | LOW          | Flag as BELOW MINIMUM — suggest adding one natural instance in body     |
| 4–8 occurrences in ≤3000 words | OPTIMAL      | Pass                                                                     |
| 9–12 occurrences               | BORDERLINE   | Flag as APPROACHING STUFFING — review for natural density               |
| >12 occurrences                | STUFFING     | Flag as KEYWORD STUFFING — identify and remove excess instances         |

**Stuffing detection:** Keyword stuffing is not just about count — it is about proximity and naturalness. If the primary keyword appears more than once within a 150-word window (outside of legitimate AEO sections where definition/answer requires it) → flag as local stuffing.

**Phase 4C — Secondary and Long-Tail Keyword Validation**

Using the keyword intelligence from `metadata-generator.skill`:

- Verify at least 4 of the 6–8 secondary keywords appear somewhere in the article body.
- Verify at least 3 of the 5–7 long-tail keywords appear in the article (in H3 headings or body text).
- Verify LSI keywords appear naturally — no specific placement check (LSI presence is a semantic signal, not a placement requirement).
- If secondary keywords are significantly absent (fewer than 3 of 8 present) → flag as SEO COVERAGE GAP.

**Phase 4D — Keyword Context Validation**

Every instance of the primary keyword is checked for context quality:

| Context Quality       | Definition                                                                       | Action                           |
|-----------------------|----------------------------------------------------------------------------------|----------------------------------|
| NATURAL               | Keyword appears in a sentence where it is the genuine subject or object          | Pass                             |
| FORCED                | Keyword appears in a sentence where it reads awkwardly or unnecessarily          | Flag for humanizer review        |
| DEFINITION CONTEXT    | Keyword appears in the direct answer block or AEO section — acceptable higher density | Pass (AEO exemption)       |
| HEADING REPETITION    | Keyword appears in two consecutive headings with no intervening content          | Flag as potentially awkward      |

---

### ▸ STEP 5 — READABILITY CHECK

Ensure the article reads naturally, flows logically, and does not contain sections that are genuinely difficult to parse on first reading.

**Phase 5A — Complex Sentence Detection**

Flag (but do not auto-rewrite) sentences that fail the readability threshold:

| Complexity Signal                                                          | Detection Rule                                              |
|----------------------------------------------------------------------------|-------------------------------------------------------------|
| More than 2 subordinate clauses in one sentence                           | Flag — "COMPLEX SENTENCE: consider breaking into two"      |
| More than 4 commas in a non-list sentence                                 | Flag — "OVERLY QUALIFIED: consider restructuring"          |
| Sentence longer than 45 words with nested qualifiers                      | Flag — "LONG-COMPLEX: review for clarity"                  |
| Three or more consecutive sentences exceeding 30 words                   | Flag — "RHYTHM ISSUE: vary sentence length in this passage"|

**Note:** Complex sentence flags are informational — they are added as inline comments in the article for the content team. They are not auto-rewritten because sentence structure changes can alter meaning (which is outside this skill's correction authority beyond the humanization pass that already ran).

**Phase 5B — Logical Progression Check**

For each H2 section, verify the internal paragraph logic:

Ask: "Does each paragraph build on the previous one in a way that a reader following the section would find coherent?"

Detection of illogical progression:
- A paragraph that introduces a concept assumes in the prior paragraph that the concept was already known.
- A conclusion paragraph precedes the evidence paragraphs.
- A process step appears before the step that should precede it.

When illogical progression is found:
- If the fix requires only reordering (no rewriting) → reorder the paragraphs and log it.
- If the fix requires adding a bridge sentence → add the minimum bridge sentence needed for coherence and log it.
- If the fix requires substantial rewriting → flag as FLOW ISSUE for content team.

**Phase 5C — Article-Level Flow Validation**

Check that the article's sections follow a logical sequence consistent with the content type:

| Content Type         | Expected Sequence Validation                                               |
|----------------------|----------------------------------------------------------------------------|
| Informational Guide  | Foundational concepts before advanced applications                         |
| Comparison           | Criteria before evaluation; individual items before verdict                |
| Tutorial             | Prerequisites before steps; earlier steps before later steps              |
| Case Study           | Context before action; action before results; results before learnings     |
| Listicle             | Ordered by value signal (most important first or numerical order declared) |

If sections are in a demonstrably wrong order → flag as SECTION ORDER ISSUE with the recommended correction.

---

### ▸ STEP 6 — SEO COMPLETENESS VERIFICATION

Validate that all SEO-relevant components are present and properly structured.

**Phase 6A — Internal Links Validation**

Load the internal linking plan from Part 5 of the production package:

| Check                                                 | Pass Condition                                             | Issue if Failing                      |
|-------------------------------------------------------|------------------------------------------------------------|---------------------------------------|
| Minimum 3 internal link suggestions present           | At least 3 distinct link recommendations                  | Flag — add remaining suggestions      |
| Every suggested link has a section placement          | No "place anywhere" suggestions                           | Flag for specificity                  |
| Every suggested link has anchor text                  | No "click here" or URL-only anchor texts                  | Flag — anchor text must be descriptive|
| Anchor texts are all different                        | No two links use identical anchor text                    | Flag — vary anchor text               |
| Internal links placed at natural contextual points   | Links appear where the topic naturally warrants a reference | Flag if links are placed at section breaks |

**Phase 6B — Heading Structure (SEO Hierarchy) Validation**

| Check                                                 | Pass Condition                                             |
|-------------------------------------------------------|------------------------------------------------------------|
| One H1 only                                           | Exactly one H1 in the article                             |
| H2 sections exist                                     | At least 4 H2 sections                                    |
| H3s are subordinate to H2s                            | No H3 exists without a parent H2                          |
| No heading is empty                                   | All H1/H2/H3 have non-empty text                         |
| No H4 or lower used without explicit purpose         | If H4 present → validate it is structurally necessary    |
| Heading hierarchy is unbroken                         | No jump from H1 directly to H3 without an H2             |

**Phase 6C — Entity Coverage Validation**

If `entity_map` is available from `entity-extraction.skill`:

1. Load all Critical entities from the entity map.
2. For each Critical entity: check if it appears in the article with context (at minimum one sentence of explanation alongside the mention).
3. Calculate entity coverage rate: covered / total_critical × 100.

| Entity Coverage Rate | Status                                                    | Action                                         |
|----------------------|-----------------------------------------------------------|------------------------------------------------|
| ≥ 80%                | PASS                                                      | No action                                      |
| 60–79%               | FLAG                                                      | List uncovered Critical entities for content team |
| < 60%                | FLAG — ENTITY GAP                                         | List all uncovered entities; recommend content team adds coverage |

For each uncovered Critical entity → add a note in the validation report: "ENTITY NOT COVERED: [Entity Name] — required for topical authority signal in [Cluster Name]. Add at least one sentence covering this entity in [suggested section]."

**Phase 6D — External References Validation**

For statistics and data points that are cited in the article:
- Verify that statistics with a named source have a reference indicator (inline attribution or footnote marker).
- If a statistic is cited with only a generic attribution ("Studies show...") → flag as UNATTRIBUTED STATISTIC and recommend the source be named.
- Verify that external links (if any are present) point to authoritative sources — not thin aggregator pages.

---

### ▸ STEP 7 — AEO VALIDATION

Validate that all answer engine optimization elements are properly formed and structurally eligible for featured snippet and PAA capture.

**Phase 7A — Direct Answer Block Validation**

The direct answer block appears immediately after the H1 heading, before the Table of Contents.

| Check                                                          | Pass Condition                                              | Auto-Fix?                           |
|----------------------------------------------------------------|-------------------------------------------------------------|-------------------------------------|
| Direct answer block is present                                | Block exists between H1 and first H2                       | NO — flag as BLOCKER if absent     |
| Block is 2–3 sentences                                         | Not a single sentence; not a full paragraph                | FLAG — cannot auto-restructure safely |
| Block is ≤60 words                                             | Word count of block ≤ 60                                  | FLAG if over — suggest trimming     |
| Block answers the primary keyword's implied question           | Reader would have their core question answered by this block alone | FLAG if it doesn't answer directly |
| Block is self-contained                                        | No "as discussed below" or forward references             | AUTO-FIX — remove the forward reference clause |
| Primary keyword appears in the block                           | Keyword is present naturally                               | FLAG if absent                      |
| Block does not start with a prohibited opener                  | None of the banned opener patterns from `content-generation-engine.skill` | AUTO-FIX — rewrite opener |

**Phase 7B — AEO H2 Section Validation**

Two H2 sections in the article are designated as AEO sections in the blueprint.

| Check                                                          | Pass Condition                                              | Auto-Fix?                           |
|----------------------------------------------------------------|-------------------------------------------------------------|-------------------------------------|
| At least 2 AEO H2 sections present                            | Two sections with an AEO/answer-optimized structure        | FLAG as BLOCKER if fewer than 1    |
| First paragraph of each AEO section is ≤60 words             | The direct answer paragraph is concise enough for snippet  | FLAG for trimming if over 60 words |
| First paragraph directly answers the section's heading question | Question in heading → answer in first paragraph           | FLAG if first paragraph doesn't answer |
| First paragraph is self-contained                              | No "see above" or "as mentioned" references               | AUTO-FIX — remove the cross-reference |
| Primary keyword or close variation appears in the answer paragraph | Keyword present naturally                                | FLAG if absent                      |
| Schema recommendation present for AEO sections               | FAQ schema or HowTo schema noted where applicable         | FLAG if absent from visual plan     |

**Phase 7C — FAQ Section Validation**

Validate all 5 FAQ question-answer pairs:

For each FAQ:

| Check                                                 | Pass Condition                                             | Auto-Fix?                          |
|-------------------------------------------------------|------------------------------------------------------------|------------------------------------|
| Question is phrased as natural language query         | Reads as something a person would actually search         | FLAG — cannot rewrite without knowing intent |
| Answer is 40–80 words                                 | Word count within range                                   | FLAG if out of range — suggest adjustment |
| Answer starts with a direct declarative statement     | Not a question, not "It depends," not "There are many..." | AUTO-FIX — rewrite opener to be direct |
| Answer is self-contained                              | No "as mentioned above" or "see section X" references    | AUTO-FIX — remove the cross-reference |
| Answer addresses the question directly                | First sentence resolves the question's core concern       | FLAG if answer circles around the question |
| No two FAQs address the same sub-intent              | Confirmed by Step 3 (FAQ Duplication Check)               | Step 3 handles this                |

**FAQ Count Enforcement:**
- If 5 FAQs are present → PASS.
- If 4 FAQs are present → FLAG as ONE FAQ MISSING — suggest an additional question.
- If 3 or fewer FAQs are present → BLOCKER if this is an AEO-optimized article (which all Nexus articles are).

---

### ▸ STEP 8 — CONVERSION LAYER CHECK

Validate the conversion layer for MOFU and BOFU content. For TOFU content → this step is N/A.

**Phase 8A — Funnel Stage Determination**

Load the validated funnel stage from the SEO Brief. Apply the appropriate conversion layer standard:

| Funnel Stage | Conversion Layer Required                                              |
|--------------|------------------------------------------------------------------------|
| TOFU         | N/A — Step 8 result = N/A-TOFU                                        |
| MOFU         | Minimum 1 CTA, value proposition present, evaluation-stage CTA language |
| BOFU         | Minimum 1 CTA (up to 3), strong value proposition, trust element, action-stage CTA language |

**Phase 8B — CTA Validation (MOFU and BOFU)**

| Check                                                       | Pass Condition                                              | Auto-Fix?                          |
|-------------------------------------------------------------|-------------------------------------------------------------|------------------------------------|
| CTA is present                                              | At least one CTA in the article                            | FLAG as MISSING CTA — cannot auto-add without product/service context |
| CTA type matches funnel stage                               | MOFU: evaluation CTA ("Compare," "See," "Evaluate") / BOFU: action CTA ("Start," "Get," "Try") | FLAG if mismatched |
| CTA is specific (not generic "Learn more")                 | CTA states a specific benefit or action                    | FLAG — "Learn more" is prohibited |
| CTA placement is at a natural trust-peak in the content    | Not in the first 300 words; appears after a value-building section | FLAG if placement is premature |
| BOFU: product/service mention is present                   | For BOFU content: specific tool/service is named and linked | FLAG if absent — required for BOFU |

**Phase 8C — Value Proposition Validation**

For MOFU and BOFU content:

- Is there at least one clear statement of what the reader gains by taking the recommended action?
- Is the value proposition specific (mentions an outcome, not just a general benefit)?
- For BOFU: is a trust element present near the CTA (customer count, case study reference, award, or testimonial)?

| Check                                             | Pass Condition                                           | Auto-Fix?   |
|---------------------------------------------------|----------------------------------------------------------|-------------|
| Value proposition present                         | Clear benefit statement near CTA                        | FLAG if absent |
| Value proposition is specific                     | Names an outcome, not just a vague benefit               | FLAG if vague |
| BOFU: Trust element near CTA                      | Social proof, data point, or testimonial adjacent to CTA | FLAG if absent |

---

### ▸ STEP 9 — ACCURACY CORRECTIONS CONFIRMATION

Verify that all corrections produced by `technical-accuracy-checker.skill` were applied to the article and that no unresolved accuracy issues remain.

**Phase 9A — Corrections Applied Verification**

Load the `accuracy_corrections_log`. For each correction with status CORRECTED:
1. Locate the original incorrect text in the current article.
2. Verify the correction was applied — the original text should no longer be present; the corrected text should be present.
3. If the correction was not applied → apply it now and log.

**Phase 9B — Cannot-Correct Items Review**

Load all items with status CANNOT CORRECT — USER ACTION REQUIRED from the corrections log:
1. Verify each is flagged in the article with a `[VERIFY]` marker.
2. If a `[VERIFY]` marker is missing → add it to the appropriate location.
3. Carry these items forward to the validation report's FLAGS FOR USER ACTION section.

**Phase 9C — Unverified Claims Review**

Load the `unverified_claims_list`:
1. Verify each unverified claim has a `[VERIFY]` marker in the article.
2. For HIGH RISK unverified claims (medical/legal/financial) → escalate to BLOCKER if no expert review was confirmed.
3. Add all unverified claims to the FLAGS FOR USER ACTION section of the report.

**Phase 9D — Confidence Score Cross-Reference**

Cross-reference the `accuracy_confidence_score`:

| Score       | Validation Action                                                                          |
|-------------|--------------------------------------------------------------------------------------------|
| ≥ 90%       | Pass — log the score; add to report header                                                |
| 70–89%      | Flag — note "ACCURACY REVIEW RECOMMENDED" in report                                       |
| 50–69%      | Flag as MEDIUM RISK — add to validation report flags; note sections requiring review      |
| < 50%       | BLOCKER — "NOT READY: Accuracy confidence below publishing threshold. Content team must review all accuracy issues before publishing." |

---

### ▸ STEP 10 — HUMANIZATION CHECK

Verify that `humanizer.skill` successfully removed robotic tone patterns and that the article reads naturally.

**Phase 10A — Remaining AI Pattern Scan**

Run a scan for remaining instances of the AI pattern categories defined in `humanizer.skill`:

| Pattern Category             | Scan Method                                                                             |
|------------------------------|-----------------------------------------------------------------------------------------|
| Generic opener phrases       | Check first sentence of every paragraph against the prohibited opener list             |
| Filler commentary phrases    | Scan for "It is important to note," "It is worth noting," and related patterns         |
| Mechanical transitions       | Scan for "Now let's," "Let's dive into," "With that said"                              |
| AI structural clichés        | Scan for "Here is a breakdown of," "Below, we will cover," and related patterns        |
| Vague claim qualifiers       | Scan for "various," "different," "several," "many" without specificity                 |

For each remaining instance found:
- If it can be transformed without meaning change → auto-fix using the transformation rules from `humanizer.skill`.
- If transformation risk is unclear → flag for humanizer review.

**Phase 10B — Sentence Variation Check**

Sample-check 3 randomly selected paragraphs for sentence variation:
- Do more than 2 consecutive sentences start with the same word? → Flag.
- Do more than 3 consecutive sentences have near-identical length? → Flag.

This is a sample check, not a full re-pass of the humanization step.

**Phase 10C — Humanizer Flags Review**

If `humanizer_flags` were produced by `humanizer.skill` (phrases kept unchanged due to transformation uncertainty):
1. Load all flagged phrases.
2. Review each in the context of the final article.
3. If the phrase is now clearly a robotic pattern → apply the simpler transformation.
4. If the phrase is still uncertain → carry it forward to the FLAGS FOR USER ACTION section.

---

### ▸ STEP 11 — FINAL CLEANUP

Apply cleanup operations to ensure the delivered article is clean, formatted correctly, and free of technical artifacts.

**Phase 11A — Placeholder Removal**

Scan the article for any remaining placeholder markers that were used during the generation process:

| Placeholder Type              | Detection Pattern                                        | Action                                       |
|-------------------------------|----------------------------------------------------------|----------------------------------------------|
| Image placeholders            | `[IMAGE: ...]`, `[INSERT IMAGE]`, `[FIGURE N]`         | Keep the IMAGE specification as a production note; remove bracketed markers from prose |
| Table placeholders            | `[TABLE: ...]`, `[INSERT TABLE]`                       | Keep the TABLE specification as a production note |
| Code placeholders             | `[CODE: ...]`, `[EXAMPLE CODE]`                        | Flag — code should have been generated; add specific BLOCKER if code was specified but absent |
| Video placeholders            | `[VIDEO: ...]`, `[INSERT VIDEO]`                       | Keep as production note; remove from prose  |
| Author placeholders           | `[AUTHOR NAME]`, `[YOUR NAME]`                         | Flag — contact information placeholder found |
| Statistics placeholders       | `[STATISTIC]`, `[DATA POINT]`, `[CITE HERE]`          | BLOCKER — placeholder statistics must be resolved before publishing |
| Link placeholders             | `[LINK]`, `[URL HERE]`, `[ANCHOR TEXT]`               | FLAG — link placeholders require content team to fill |

**Phase 11B — Formatting Consistency**

| Check                                              | Auto-Fix                                             |
|----------------------------------------------------|------------------------------------------------------|
| Inconsistent heading capitalization               | Apply title case consistently to all H2/H3           |
| Double spaces between sentences                  | Collapse to single space                             |
| Trailing whitespace at line ends                 | Remove trailing whitespace                           |
| Inconsistent bullet point styles (mix of - and •) | Standardize to one style                            |
| Inconsistent numbered list formats               | Standardize numbering style                          |
| Extra blank lines between paragraphs (more than 1) | Collapse to single blank line                       |
| En dash vs. em dash inconsistency                | Standardize to em dash for parenthetical emphasis   |
| Inconsistent quote styles (curly vs. straight)   | Note for formatting team — not auto-fixed           |

**Phase 11C — Final Word Count and Delivery Confirmation**

After all cleanup is complete:
1. Run a final word count (using the same measurement rules as Step 2).
2. Confirm the count is within ±5% of the target.
3. Confirm zero placeholder markers remain in the prose.
4. Confirm all structural elements are present (from Step 1).
5. Produce the final completeness score.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CROSS-LAYER VALIDATION

Beyond step-by-step individual checks, cross-layer validation compares the final article against the originating intelligence from multiple upstream skills simultaneously to confirm overall coherence.

**Cross-Layer Consistency Checks:**

| Cross-Layer Check                                                                     | Validation Source                               |
|---------------------------------------------------------------------------------------|--------------------------------------------------|
| Primary keyword in article matches target keyword in SEO Brief                       | SEO Brief vs. Article H1 and intro              |
| Funnel stage tone in article matches validated funnel stage                          | SEO Brief funnel vs. CTA type, tone markers     |
| Blueprint H2 section count matches article H2 count (±1 for expansions)             | Blueprint vs. Article structure                  |
| AEO section positions match blueprint labels                                         | Blueprint AEO positions vs. article AEO section locations |
| Internal linking suggestions reference clusters aligned with the article's cluster  | Cluster architecture vs. internal link targets  |
| Visual element placements in article match the visual element plan                  | Visual element plan vs. article structure       |
| Entity coverage score is consistent with entity map critical entity list            | Entity map vs. article entity mentions          |
| Metadata blog title recommendation matches what was used as the article's H1         | Metadata package vs. article H1                 |

When a cross-layer discrepancy is found:
- If it is a minor alignment issue (H1 differs slightly from recommended title) → FLAG.
- If it is a fundamental mismatch (funnel stage mismatch throughout the article) → BLOCKER.

---

### ▸ CONSISTENCY ENFORCEMENT

The consistency enforcement module detects and corrects internal logical inconsistencies that span multiple sections.

**Consistency Enforcement Checks:**

| Inconsistency Type                                                                    | Detection                                              | Action                                              |
|---------------------------------------------------------------------------------------|--------------------------------------------------------|-----------------------------------------------------|
| A feature described as "free" in one section and "paid" in another                  | Named feature with contradictory access tier claims   | Validate against accuracy log; apply correct version |
| A statistic with the same data point but different numbers in two sections           | Same study/source cited with different values         | Keep the first instance; remove the second; flag discrepancy |
| A process described differently in two sections (different steps, different order)  | Same named process with different step sequences      | Validate against blueprint; keep the fuller description |
| A tool described as "best for X" in one section and "not suitable for X" in another | Contradictory recommendation for the same use case    | Validate against the blueprint differentiation strategy; apply consistent stance |
| An audience level that shifts between sections (expert in intro, beginner mid-article) | Tone drift across sections detected in Step 10      | Flag as TONE CONSISTENCY ISSUE                      |

---

### ▸ COMPLETENESS SCORING

After all 11 steps are complete, calculate the article's completeness score.

**Completeness Score Calculation:**

Each of the 11 validation steps contributes to the completeness score based on its strategic importance:

| Step                          | Weight | PASS points | FIXED points | FLAGGED points | BLOCKER points |
|-------------------------------|--------|-------------|--------------|----------------|----------------|
| Structure Validation          | 15%    | 15          | 12           | 8              | 0              |
| Word Count Validation         | 10%    | 10          | 8            | 5              | 0              |
| Duplication Check             | 8%     | 8           | 7            | 4              | 0              |
| Keyword Validation            | 12%    | 12          | 10           | 6              | 0              |
| Readability Check             | 5%     | 5           | 4            | 3              | 0              |
| SEO Completeness              | 10%    | 10          | 8            | 5              | 0              |
| AEO Validation                | 12%    | 12          | 10           | 6              | 0              |
| Conversion Layer Check        | 8%     | 8           | 7            | 4              | 0              |
| Accuracy Confirmation         | 12%    | 12          | 10           | 6              | 0              |
| Humanization Check            | 5%     | 5           | 4            | 3              | 0              |
| Final Cleanup                 | 3%     | 3           | 2            | 1              | 0              |

`Completeness Score = Σ(step points earned) / 100`

**Score Interpretation:**

| Score       | Status              |
|-------------|---------------------|
| 90–100%     | READY               |
| 75–89%      | READY WITH FLAGS    |
| < 75%       | NOT READY (unless all blockers are zero — then: READY WITH FLAGS if 75–89%) |
| Any Blocker | NOT READY (regardless of score)                                              |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Sections

No two H2 sections may serve the same strategic purpose in the article. Sections are identified as duplicated if they:
- Answer the same primary reader question.
- Cover the same topic territory with different wording.
- Would produce identical content if fully expanded.

Duplicate sections are merged in Step 3. The more comprehensive version is retained; the weaker is dissolved.

### Rule 2 — No Repeated Sentences Across the Article

A sentence — defined as any clause terminated by a period, question mark, or exclamation mark — may not appear more than once in the full article body (excluding intentional repetition for structural emphasis). Any sentence appearing twice has the second instance removed and a note added to the fixes log.

### Rule 3 — No Redundant Issue Logging

If the same issue category is found in multiple locations (e.g., missing `[VERIFY]` markers on 3 unverified claims) → this is logged as ONE issue with all locations listed, not three separate issues. The fixes log and flags log must not contain redundant entries.

### Rule 4 — No Duplicate Flag and Fix Entry

If an issue is AUTO-FIXED → it appears only in the ISSUES FIXED LOG, not in the FLAGS FOR USER ACTION.
If an issue is FLAGGED → it appears only in the FLAGS LOG, not in the Issues Fixed Log.
An issue cannot appear in both logs — its resolution path is one or the other.

### Rule 5 — FAQ Non-Duplication (Cross-Reference with Step 3)

The Step 3 FAQ duplication check ensures all 5 FAQ questions address distinct sub-intents. This rule ensures the dedup log consolidates any FAQ-related findings from both Step 3 and Step 7 into a single FAQ Dedup Note rather than two separate entries.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Validation Behaviors

| Prohibited Behavior                                                                             | Reason                                                                             |
|-------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| Adding filler sentences to reach word count in Step 2                                           | Word count is met with depth — never with padding                                  |
| Marking an issue as FIXED without documenting what specific action was taken                    | Every fix must be traceable — undocumented fixes are not verifiable               |
| Marking a BLOCKER as a FLAG to avoid blocking delivery                                          | Blocker criteria are objective — downgrading a blocker is a quality control failure |
| Flagging complex sentences as ERRORS when they accurately express a complex idea               | Complexity is flagged as informational — not corrected unless clarity is genuinely impaired |
| Passing a validation step without completing all sub-checks within it                          | Every step's phase-level checks must all run — partial step validation is invalid  |
| Adding content to improve SEO scores without confirming the content is accurate                | Validation does not add unverified content — flags are used instead               |
| Removing duplicate content without confirming the removed instance is truly redundant          | Cross-check meaning before removing — same wording ≠ same meaning in all cases    |
| Passing Step 9 (accuracy confirmation) when corrections log items are unapplied               | Applied corrections are verified, not assumed                                      |
| Setting status to READY WITH FLAGS when a BLOCKER exists                                        | A blocker always produces NOT READY — regardless of the completeness score        |
| Adding validation comments directly into the article's publishable prose                        | Validation notes go in the validation report and fixes log — not in the article body |

### Required Validation Behaviors

- All 11 steps must run and receive a result status (PASS / FIXED / FLAGGED / BLOCKER / N/A).
- Every fix must be documented in the Issues Fixed Log with a specific description of what changed.
- Every flag must be documented in the Flags Log with a specific recommended action.
- Every blocker must be documented in the Blocker Log with a specific resolution path.
- The completeness score must be calculated using the defined formula — not estimated.
- The overall status (READY / READY WITH FLAGS / NOT READY) must follow the defined status logic — not editorial judgment.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream source (primary article and all package components)                                |
| **Receives**     | Full 7-part production package — all Parts 1–7                                             |
| **Critical data**| Funnel stage, target word count, primary keyword, blueprint structure                      |

---

### ▸ humanizer.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream processing layer                                                                   |
| **Receives**     | Transformation log, flagged phrases list                                                   |
| **Used for**     | Step 10 (humanization verification), carrying forward humanizer flags to validation report |

---

### ▸ technical-accuracy-checker.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream verification layer                                                                 |
| **Receives**     | Corrections log, unverified claims list, confidence score                                  |
| **Used for**     | Step 9 (accuracy corrections confirmation), confidence score cross-reference               |
| **Critical data**| Confidence score — if < 50% → BLOCKER status; all corrections applied check in Step 9     |

---

### ▸ output-formatter.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-validation → output-formatter (downstream consumer)                                |
| **Sends**        | Validated final article + validation report + issues fixed log                             |
| **Gate**         | output-formatter.skill only receives content with status READY or READY WITH FLAGS         |
| **NOT READY content** | Returns to content team with blockers list; does NOT pass to output-formatter.skill  |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after validation completes)                                                          |
| **Writes**       | Validation report, completeness score, readiness status, issues fixed log                 |
| **Memory layers**| content-memory.skill (validation status), keyword-memory.skill (lifecycle: DRAFT → VALIDATED) |
| **Timing**       | Write after Step 11 cleanup completes and readiness status is assigned.                   |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                           |
|---------------------------------------------------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| `article_content` is empty or null                                        | Input pre-condition — null content                     | Halt. Flag: "No article content received. content-validation.skill cannot run on empty input."            |
| `blueprint` is unavailable                                                | Input pre-condition — null blueprint                   | Proceed without blueprint-specific checks. Apply universal structural standards. Flag all blueprint-dependent steps as BLUEPRINT ABSENT. |
| `accuracy_corrections_log` is unavailable                                 | Step 9 — corrections log null                         | Skip corrections-applied verification. Flag: "Accuracy corrections log unavailable — cannot confirm corrections were applied. Manual review recommended." |
| Accuracy confidence score < 50%                                           | Step 9 — score threshold check                        | Auto-assign BLOCKER: "ACCURACY CONFIDENCE BELOW THRESHOLD." Do not pass content to output-formatter.skill unless user explicitly overrides. |
| Word count variance > 30% under target                                    | Step 2 — severe shortfall                             | Flag as BLOCKER: "SEVERE WORD COUNT SHORTFALL — content is [N]% below target. Auto-expansion would require adding filler. Content team must expand with genuine depth." |
| FAQ section is completely absent AND direct answer block is absent        | Step 1 + Step 7 — two AEO blockers                   | Set status to NOT READY. Report: "Critical AEO elements missing — direct answer block and FAQ section both absent. Article is not AEO-eligible in current state." |
| All 11 steps produce FLAGGED or BLOCKER results (no PASS)                | Completeness scoring — score near 0%                  | Set status to NOT READY. Report: "Systemic quality failure — all validation steps flagged or blocked. Recommend running full content-generation-engine pipeline again with adjusted inputs." |
| A correction from the accuracy log conflicts with a `preserve_exactly` phrase | Step 9 — conflict between accuracy log and preservation instruction | Flag: "ACCURACY-PRESERVATION CONFLICT at [location] — accuracy correction and preserve_exactly instruction conflict. User decision required before publishing." |
| Placeholder statistics (`[STATISTIC]`) found after all cleanup steps     | Step 11 — placeholder scan finds statistics placeholder | Auto-assign BLOCKER: "UNRESOLVED STATISTIC PLACEHOLDER at [location]. Publishing with a placeholder statistic will damage credibility. Content team must provide real data." |
| Memory controller unavailable at write                                    | Step 11 — write operation fails                       | Proceed with delivery. Flag: "Memory write failed — validation results not persisted. Export the validation report manually." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — ELEVEN-STEP VALIDATION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
ELEVEN-STEP VALIDATION QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Step 1  — STRUCTURE VALIDATION
  Check: SEO brief, metadata, blueprint, article, FAQ, AEO sections
  Auto-fix: TOC rebuild, H3 orphan resolution, heading duplication
  Blocker: Missing FAQ, missing direct answer block, >2 H1 tags

Step 2  — WORD COUNT VALIDATION
  Check: Prose-only count vs. target (exclude images, tables, code)
  Auto-fix: Expand weak H3s with depth (not filler); trim filler sentences
  Blocker: >30% under target (auto-expansion would require filler)

Step 3  — DUPLICATION CHECK
  Check: Heading duplication, phrase duplication (7+ words), idea-level overlap, FAQ semantic overlap
  Auto-fix: Remove duplicate paragraphs; merge identical sections; rephrase repeated phrases

Step 4  — KEYWORD VALIDATION
  Check: Primary keyword in H1, intro, 1–2 H2s; density 4–8 instances; secondary keywords present
  Auto-fix: Add natural primary keyword instance to intro if missing
  Blocker: None — flags only (cannot alter H1 without content team)

Step 5  — READABILITY CHECK
  Check: Complex sentences, illogical progression, article-level section order
  Auto-fix: Reorder paragraphs (no rewrite); add minimum bridge sentences
  Blocker: None — flags only for complex sentences

Step 6  — SEO COMPLETENESS
  Check: 3+ internal links with anchor text, heading hierarchy, entity coverage, external reference attribution
  Auto-fix: Flag missing links; add inline attribution markers

Step 7  — AEO VALIDATION
  Check: Direct answer block (≤60w, self-contained), 2 AEO H2 sections (≤60w first para), 5 FAQs (40–80w each, self-contained)
  Auto-fix: Remove "as mentioned above" cross-references; rewrite FAQ openers to be direct
  Blocker: <1 AEO section; <3 FAQs; absent direct answer block

Step 8  — CONVERSION LAYER CHECK (MOFU/BOFU only)
  Check: CTA present and funnel-aligned; value proposition specific; BOFU: trust element near CTA
  Blocker: None — flags only (cannot auto-add CTAs without product context)

Step 9  — ACCURACY CONFIRMATION
  Check: All corrections applied; VERIFY markers present; confidence score ≥70%
  Auto-fix: Apply any unapplied corrections; add missing VERIFY markers
  Blocker: Confidence score <50%; HIGH RISK unverified claims without professional review

Step 10 — HUMANIZATION CHECK
  Check: Remaining AI patterns; sentence variation sample; humanizer flags
  Auto-fix: Remove remaining simple AI patterns; apply safer humanizer transformations

Step 11 — FINAL CLEANUP
  Check: Placeholders, formatting consistency, final word count
  Auto-fix: Remove placeholder markers; standardize formatting; confirm final count
  Blocker: Statistic placeholders remaining; unresolved link placeholders (HIGH severity)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — READINESS STATUS DECISION GUIDE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
READINESS STATUS DECISION GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
READY (Completeness ≥90% + Zero Blockers + Only LOW Flags):
  → Pass to output-formatter.skill immediately.
  → Content team receives the article with a clean validation report.
  → No further action required before publishing.

READY WITH FLAGS (Completeness ≥75% + Zero Blockers + MEDIUM/HIGH Flags):
  → Pass to output-formatter.skill with flags attached.
  → Content team reviews all MEDIUM and HIGH flags before publishing.
  → Article is technically complete and functional — flags are refinement opportunities.
  → Common flag categories at this status: unverified stats, entity coverage gaps, word count near limits.

NOT READY — Blockers Present (Any Number of Blockers):
  → DO NOT pass to output-formatter.skill.
  → Return to content team with Blocker Log.
  → Content team resolves each blocker.
  → Re-run content-validation.skill after blockers are resolved.
  → Common blockers: missing FAQ, absent direct answer block, accuracy confidence <50%,
    statistic placeholders, AEO sections missing, critical structural failures.

NOT READY — Completeness <75% (No Blockers but Severe Flags):
  → Flag as NOT READY with completeness score.
  → Content team reviews all flags and resolves HIGH-risk ones.
  → May re-run content-generation-engine.skill for specific sections.

OVERRIDE RULE:
  Any single BLOCKER = NOT READY.
  No completeness score, however high, overrides a BLOCKER.
  A 99% completeness score with one blocker = NOT READY.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — COMPLETENESS SCORE WORKSHEET
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
COMPLETENESS SCORE WORKSHEET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Step  | Weight | Result    | Points Earned
─────────────────────────────────────────────
1     | 15%    | [P/F/Fl/B]| [15/12/8/0]
2     | 10%    | [P/F/Fl/B]| [10/8/5/0]
3     | 8%     | [P/F/Fl]  | [8/7/4]
4     | 12%    | [P/F/Fl]  | [12/10/6]
5     | 5%     | [P/F/Fl]  | [5/4/3]
6     | 10%    | [P/F/Fl]  | [10/8/5]
7     | 12%    | [P/F/Fl/B]| [12/10/6/0]
8     | 8%     | [P/F/Fl/N/A]| [8/7/4/8*]
9     | 12%    | [P/F/Fl/B]| [12/10/6/0]
10    | 5%     | [P/F/Fl]  | [5/4/3]
11    | 3%     | [P/F]     | [3/2]
─────────────────────────────────────────────
TOTAL POINTS                 : [sum]
COMPLETENESS SCORE           : [total / 100]%
─────────────────────────────────────────────
RESULT KEY:
  P  = PASS       | Full points
  F  = FIXED      | 80% of points
  Fl = FLAGGED    | 50% of points (approx)
  B  = BLOCKER    | 0 points + NOT READY status
  N/A= Not applicable (TOFU for Step 8) | *Full points awarded (not penalized)

BLOCKER OVERRIDE:
  If any step = B → Status = NOT READY
  regardless of total score.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## v3.0 UPGRADE — 5-DIMENSION QUALITY GATE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**NEW in v3.0: After all validation steps pass, run the 5-dimension quality assessment.**

This gate measures writing quality on 5 dimensions that matter for reader trust and engagement. It replaces the vague "humanization pass" with a measurable, scorable system.

### The 5 Dimensions

| Dimension | What It Measures | Score /10 |
|---|---|---|
| **Directness** | Does the writing get to the point? Or does it warm up, hedge, and pad? Count filler phrases per 500 words. Below 2 = 10/10. 2-4 = 7/10. 5+ = 3/10. | /10 |
| **Rhythm** | Does sentence length vary naturally? Check any 10-sentence span for length distribution. Mix of short (under 12 words), medium (13-22), and long (23-35) = high score. Monotonous length = low score. | /10 |
| **Trust** | Does the writing make specific, verifiable claims? Or vague, unsubstantiated statements? Count claims with sources vs claims without. Above 70% sourced = 10/10. 50-70% = 7/10. Below 50% = 3/10. | /10 |
| **Authenticity** | Does it read like a person with real experience wrote it? Check for: opinions stated directly, specific examples from experience, occasional informal language, personality in the writing voice. 3+ signals = 10/10. 1-2 = 6/10. 0 = 2/10. | /10 |
| **Density** | What percentage of the content is actual information vs padding? Remove all transition sentences, hedging, and generic statements. What % remains? Above 80% = 10/10. 60-80% = 7/10. Below 60% = 3/10. | /10 |

### Scoring

**Total: /50**

| Score | Grade | Action |
|---|---|---|
| 45-50 | A+ | Ready to publish |
| 40-44 | A | Minor polish recommended |
| 35-39 | B | Acceptable — publish with noted improvements |
| 25-34 | C | **Mandatory rewrite** — identify weakest dimensions, fix, re-score |
| Below 25 | F | **Full rewrite required** — content fails quality standard |

**Minimum threshold: 35/50.** Below this = mandatory rewrite. The content does not proceed to metadata generation until it scores 35+.

### Report Format

Add to content-validation output:
```
5-DIMENSION QUALITY GATE (v3.0)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Directness:    [X]/10 — [filler count per 500 words]
Rhythm:        [X]/10 — [sentence length distribution]
Trust:         [X]/10 — [sourced claims %]
Authenticity:  [X]/10 — [personality signals found]
Density:       [X]/10 — [information %]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOTAL:         [X]/50 — [grade]
STATUS:        [PASS / REWRITE REQUIRED]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: content/content-validation.skill*
*Nexus SEO Operating System — Version 3.0.0*
