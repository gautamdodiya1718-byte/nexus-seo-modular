# SKILL: content/humanizer.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Post-Generation Processing
# EXECUTION PRIORITY: POST-GENERATION — Runs immediately after content-generation-engine.skill produces the article draft, before technical-accuracy-checker.skill and content-validation.skill.
# POSITION: First post-generation processing layer. Transforms the raw generated article into natural, reader-aligned prose before any other post-processing begins.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **natural language transformation layer** of the Nexus content pipeline.

AI-generated content — even when structurally excellent, semantically complete, and factually accurate — carries identifiable patterns that betray its origin: the same sentence-opening words appearing paragraph after paragraph, transitions that feel mechanical rather than conversational, an evenness of sentence length and rhythm that no human writer produces naturally, generic setup phrases that add no information value, and a tone that reads as systematically assembled rather than purposefully written. Readers detect these patterns — consciously or not — and the result is diminished trust, higher bounce rates, and lower perceived authority.

This skill removes those patterns. It does not rewrite the article. It does not change what is said. It only changes how it is said — transforming the expression of every existing idea into something that reads as though a skilled human writer who genuinely understands the topic wrote it from scratch.

The transformation is governed by seven specific transformation steps and three advanced logic modules, each targeting a distinct dimension of AI-pattern removal. Every change made by this skill must preserve the original meaning exactly. Every keyword placement, entity reference, structural heading, and factual claim survives the transformation intact. Only the voice, rhythm, phrasing, and transition structure are modified.

### Primary Responsibilities

| Responsibility                       | Description                                                                                                         |
|--------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| **Sentence Variation**               | Break monotonous sentence structures — vary length, opening word, and syntactic form across consecutive sentences   |
| **Tone Alignment**                   | Calibrate the article's register to match the target audience (Beginner / Intermediate / Expert) and tone preference |
| **AI Pattern Removal**               | Detect and eliminate known AI-generated phrase patterns, filler statements, and robotic transition markers           |
| **Natural Flow Enhancement**         | Improve paragraph transitions, logical progression, and the conversational rhythm that holds a reader's attention   |
| **Readability Optimization**         | Enforce a natural short-long sentence mix; remove overly complex constructions; ensure clarity without over-simplification |
| **Personalization Layer**            | Add subtle reader-engagement cues that make the writing feel addressed to a specific person rather than a generic audience |
| **Redundancy Removal**               | Eliminate repeated ideas and repeated phrasing that survived the drafting process                                   |
| **Meaning Preservation Enforcement**| Verify after every transformation that the original meaning, factual claims, and keyword placements are intact       |
| **Rhythm Balancing**                 | Apply sentence entropy variation so no two consecutive paragraphs share the same rhythmic signature                 |
| **Tone Consistency Enforcement**     | Ensure the calibrated tone holds throughout the entire article — no drift between sections                         |

### What This Skill Is NOT

- It is **not** a rewriter. It transforms expression — it does not alter meaning, scope, or structure.
- It is **not** a fact-checker. It does not modify or verify factual claims — that is `technical-accuracy-checker.skill`'s role.
- It is **not** a structural editor. It does not change headings, section order, or content organization.
- It is **not** a keyword optimizer. It does not add, move, or remove keywords — all keyword placements from the generation phase are preserved.
- It is **not** a style guide enforcer. It aligns to the configured audience and tone — not to a specific brand style guide (that is `brand-voice.skill`'s role if present).

### The Iron Rule of This Skill

> This skill has one absolute constraint:
>
> **Do NOT change meaning. Do NOT remove important content. Only improve expression.**
>
> Every sentence that exits this skill must express exactly what the
> corresponding sentence expressed when it entered — in clearer, more natural language.
>
> When uncertain whether a transformation changes meaning → keep the original.
> When uncertain whether a phrase is genuine AI-pattern or deliberate style → keep it.
> When uncertain whether a sentence is redundant → keep it and flag it for human review.
>
> The cost of a false positive (removing meaningful content) is higher
> than the cost of a false negative (keeping a slightly robotic phrase).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                       | Description                                                                              | Required |
|-----------------------------|------------------------------------------------------------------------------------------|----------|
| `generated_content`         | The full article draft produced by `content-generation-engine.skill`                    | YES      |
| `target_audience`           | Audience definition from the content configuration — role, expertise level, context     | YES      |
| `tone_preference`           | Configured tone: Professional / Conversational / Technical / Persuasive                  | YES      |

### Optional Inputs

| Field                       | Description                                                                 | Used For                                               |
|-----------------------------|-----------------------------------------------------------------------------|--------------------------------------------------------|
| `brand_voice_guidelines`    | Specific brand voice rules if provided by the user                         | Tone calibration — supplements the configured tone     |
| `audience_expertise_level`  | Explicit expertise level: Beginner / Intermediate / Expert                  | Step 2 (Tone Adjustment) — more precise than audience description alone |
| `forbidden_phrases`         | User-defined phrases that must not appear in the final content              | Step 3 (AI Pattern Removal) — extends the default list |
| `preserve_exactly`          | Specific phrases, quotes, or technical terms that must not be altered       | Meaning preservation — exempted from all transformations |

### Voice Calibration Source Check

Check whether brand.extended_audience_profile is set in the active brand file.

If brand.extended_audience_profile is set:
  Load the audience file. Read: primary_content_voice field.
  Use primary_content_voice as the voice calibration instruction for this pass.
  This takes priority over brand.tone alone — it is a richer, more specific instruction.

If not set:
  Use brand.tone + brand.writing_style_notes from brand file as normal.

### Input Pre-Conditions

1. **Is `generated_content` present and non-empty?** If not → halt. Log: "No content to humanize — generated_content input is empty."
2. **Is the article structurally intact?** Verify headings (H1, H2, H3), the FAQ section, and the conversion layer are present. If structural elements are missing → flag and proceed with available content — do not block on structural issues (those are caught by `content-validation.skill`).
3. **Are `target_audience` and `tone_preference` available?** If not → default to Intermediate expertise + Professional tone. Flag: "Audience and tone defaulted — provide explicit values for more precise tone calibration." If voice calibration was loaded from extended_audience_profile above, use that instead.
4. **Has `preserve_exactly` been defined?** If yes → load the preserved phrases list and exempt them from all transformation passes. These phrases must appear in the final output exactly as they entered.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Humanized Article Package

```
NEXUS HUMANIZER — OUTPUT PACKAGE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Input Word Count        : [N words — from generated_content]
Output Word Count       : [N words — must be within ±3% of input]
Tone Applied            : [Professional / Conversational / Technical / Persuasive]
Audience Level          : [Beginner / Intermediate / Expert]
Transformations Applied : [count]
  Sentence Variations   : [count]
  AI Patterns Removed   : [count]
  Redundancies Removed  : [count]
  Flow Improvements     : [count]
Meaning Preserved       : YES (required)
Keyword Placements Preserved: YES (required)
Flagged for Review      : [count — uncertain transformations]
Generated By            : content/humanizer.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[FULL HUMANIZED ARTICLE CONTENT]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TRANSFORMATION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[List of significant transformations — original phrase → transformed phrase — reason]

FLAGGED FOR REVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Any phrases kept unchanged due to transformation uncertainty]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Requirements

| Requirement                              | Standard                                                                          |
|------------------------------------------|-----------------------------------------------------------------------------------|
| Word count delta                         | Final word count must be within ±3% of input word count                          |
| Keyword placements                       | Every keyword placement from the input article is present in the output           |
| Structural elements                      | All H1, H2, H3, FAQ, CTA elements are present — humanizer does not remove structure |
| Meaning fidelity                         | Every sentence in the output expresses the same meaning as its source sentence    |
| AI pattern count                         | Zero known AI patterns (from the detection list) remain in the output             |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — SENTENCE VARIATION

Break the monotony of repetitive sentence structure. AI-generated text produces sentences with similar syntax, similar length, and similar opening word patterns across consecutive sentences. Human writing does not.

**Phase 1A — Opening Word Audit**

Scan every paragraph in the article. For each paragraph, extract the first word of every sentence:

If more than ONE sentence in the same paragraph begins with the same word → transform all but the first instance:

| Repeat Pattern Found              | Transformation                                                   |
|-----------------------------------|------------------------------------------------------------------|
| Three sentences starting with "This" | Rephrase the second and third to start differently          |
| Two sentences starting with "It"  | Rephrase the second to lead with the subject or an adverb       |
| Two sentences starting with "The" | Rephrase the second to invert or restructure the sentence       |

**Opening Word Diversity Rule:**
Within any 5-sentence span, no opening word may appear more than twice. Track the 5-sentence rolling window across paragraph breaks.

**Prohibited Repetitive Opening Patterns:**

| Pattern                            | Example of Problem                                                         |
|------------------------------------|----------------------------------------------------------------------------|
| Consecutive "This" openings        | "This is important. This helps SEO. This makes ranking easier."           |
| Consecutive "It" openings          | "It is crucial. It provides value. It ensures consistency."               |
| Consecutive "You" openings         | "You can improve. You should focus. You need to understand."              |
| Consecutive "The" openings         | "The result is better. The process helps. The outcome improves."          |
| Consecutive verb-first sentences   | "Understanding this helps. Knowing the rules matters. Applying it works." |

**Phase 1B — Sentence Length Variation**

Measure the length (word count) of every sentence in each paragraph.

**Sentence length monotony detection:**

Three or more consecutive sentences within ±3 words of each other in length = MONOTONOUS RHYTHM — transform.

**Transformation model for length variation:**

| Detected Pattern                  | Transformation                                                   |
|-----------------------------------|------------------------------------------------------------------|
| Short + Short + Short (all ≤12w)  | Combine two short sentences with a connector; expand the third  |
| Long + Long + Long (all ≥25w)     | Break the middle sentence into two; the third is trimmed        |
| Medium + Medium + Medium          | Shorten the first; keep the second; lengthen the third with detail |

**Target sentence length distribution (per paragraph):**

| Length Category  | Definition    | Target Share Within Any Paragraph |
|------------------|---------------|-------------------------------------|
| Short            | ≤ 12 words    | 20–35%                              |
| Medium           | 13–22 words   | 45–55%                              |
| Long             | 23–35 words   | 15–30%                              |
| Very Long        | > 35 words    | < 10% — use sparingly              |

**Phase 1C — Syntactic Structure Variation**

Beyond opening words and length, vary the syntactic structure of sentences across consecutive instances:

| Structure Type              | Example Form                                                                   |
|-----------------------------|--------------------------------------------------------------------------------|
| Simple declarative          | "[Subject] [verb] [object]."                                                  |
| Inverted structure          | "[Object/predicate], [subject] [verb]."                                       |
| Conditional                 | "If [condition], [result]."                                                   |
| Contrast                    | "[A] — but [B]."                                                              |
| Appositive opener           | "[Appositive], [subject] [verb]."                                             |
| Subordinate clause opener   | "[Because/When/Although + clause], [main clause]."                           |
| Em-dash emphasis            | "[Main clause] — [emphasizing point]."                                        |

**Rule:** Within any 4-sentence span, no more than 2 sentences may share the same syntactic structure type.

**Example transformation (sentence variation combined):**

**Input (AI-generated — monotonous):**
> "This is important. This helps SEO. This improves ranking. This boosts traffic."

**Output (humanized — varied):**
> "This matters — and not in an abstract way. When implemented consistently, it directly influences how search engines evaluate your pages. Rankings improve because the underlying quality signal changes. Traffic follows from that improvement, not the other way around."

**Note what changed:** Opening words varied; sentence lengths varied (3w, 12w, 9w, 10w); structures varied (declarative, subordinate clause, simple declarative, subordinate clause). **Note what did NOT change:** The meaning — importance, SEO impact, ranking improvement, traffic increase — is preserved exactly.

---

### ▸ STEP 2 — TONE ADJUSTMENT

Calibrate the article's voice and register to match the specific target audience and configured tone. Tone misalignment creates friction — an expert reading beginner-level over-explanation feels condescended to; a beginner reading expert-level assumed knowledge feels lost.

**Phase 2A — Audience Expertise Level Classification**

If `audience_expertise_level` is explicitly provided → use it directly.

If only `target_audience` is provided → infer expertise level:

| Audience Signal                                                           | Expertise Level |
|---------------------------------------------------------------------------|-----------------|
| "beginners," "first-time users," "non-technical," "business owners"       | BEGINNER        |
| "marketers," "managers," "practitioners," "growing teams"                 | INTERMEDIATE    |
| "developers," "CTOs," "data scientists," "senior engineers," "analysts"   | EXPERT          |
| No clear signal → default                                                  | INTERMEDIATE    |

**Phase 2B — Audience-Specific Transformation Rules**

**BEGINNER LEVEL:**

| Transformation Rule                                                              |
|----------------------------------------------------------------------------------|
| Define every technical term on first use — inline in the same sentence or the next |
| Replace jargon with plain-language equivalents where meaning is preserved       |
| Shorten average sentence length to ≤18 words                                    |
| Use analogies to explain abstract concepts (no more than 1 analogy per section) |
| Replace passive voice with active voice throughout                               |
| Ensure every process is explained step-by-step, not assumed as known             |

**Beginner-level prohibited patterns:**
- Assumed familiarity ("As you know...", "Obviously...", "Simply put..." [when the thing is not simple])
- Acronyms without expansion on first use
- Comparisons that assume knowledge of the compared item ("Similar to X" when X also requires explanation)

**INTERMEDIATE LEVEL:**

| Transformation Rule                                                              |
|----------------------------------------------------------------------------------|
| Define technical terms that are genuinely specialized — not universally known   |
| Keep average sentence length at ≤22 words                                        |
| Use a professional but accessible register — not academic, not casual           |
| Analogies are acceptable but not required                                        |
| Mix active and passive voice naturally — passive is acceptable where it flows   |
| Process explanations assume basic domain literacy — elaborate on advanced steps |

**EXPERT LEVEL:**

| Transformation Rule                                                              |
|----------------------------------------------------------------------------------|
| Remove all definitions of standard industry terms — experts do not need them   |
| Sentence length may reach up to 28 words average — complexity is acceptable     |
| Technical precision is prioritized over accessibility                            |
| Analogies are rarely appropriate — direct technical statements preferred         |
| Passive voice is acceptable, especially in technical process descriptions       |
| Assume domain literacy — do not explain foundational concepts                    |

**Phase 2C — Tone-Specific Transformation Rules**

Apply these rules based on the configured `tone_preference`:

**PROFESSIONAL TONE:**

| Rule                                                                             |
|----------------------------------------------------------------------------------|
| No contractions in formal claim sentences ("it's" → "it is" in formal contexts; acceptable in casual transitions) |
| No slang or colloquial expressions                                               |
| Remove first-person plural when it reads as presumptuous ("we all know...")     |
| Ensure every claim is grounded — hedging phrases ("somewhat," "quite") are minimized |
| Maintain consistent formal register throughout — no sudden casual drifts        |

**CONVERSATIONAL TONE:**

| Rule                                                                             |
|----------------------------------------------------------------------------------|
| Contractions are acceptable and encouraged in non-claim sentences               |
| Second person ("you") is appropriate and encouraged throughout                  |
| Direct reader address is natural: "Here's what that means for you."            |
| Short sentences and single-idea paragraphs are acceptable                       |
| Avoid overly formal passive constructions — active voice always preferred       |

**TECHNICAL TONE:**

| Rule                                                                             |
|----------------------------------------------------------------------------------|
| Technical precision over accessibility — do not simplify correct technical terms |
| Passive voice is acceptable in process and methodology descriptions              |
| Numbers and data are stated precisely — no rounding without justification        |
| Sentence complexity may be higher if it accurately reflects technical nuance    |
| Avoid conversational opener sentences — state the technical fact directly        |

**PERSUASIVE TONE:**

| Rule                                                                             |
|----------------------------------------------------------------------------------|
| Active voice is mandatory — passive weakens persuasive impact                  |
| Benefit statements must be specific — "increases revenue" → "increases revenue by [measurable amount or type]" |
| Forward-momentum language: every paragraph moves the reader toward the conclusion |
| Remove neutral language where the writing should take a position                |
| Avoid hedging phrases that undermine the persuasive claim                       |

---

### ▸ STEP 3 — AI PATTERN REMOVAL

AI-generated text has a fingerprint. This step systematically detects and removes the most recognizable markers of that fingerprint — generic phrases, mechanical transitions, and structural clichés that appear disproportionately in AI output.

**Phase 3A — The AI Pattern Detection List (Primary)**

These patterns are automatically flagged and transformed on every pass:

**Category 1 — Generic Opener Phrases:**

```
CATEGORY 1 — GENERIC OPENERS (Remove/Transform Always)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"In today's digital world..."
"In today's fast-paced world..."
"In today's competitive landscape..."
"In the modern era..."
"In recent years..."
"As technology continues to evolve..."
"As businesses grow..."
"As more companies..."
"With the rise of..."
"With the increasing importance of..."
"With so many options available..."
"At its core, [X] is..."  [as a section opener specifically]
"When it comes to [X]..."  [as an opener — acceptable mid-paragraph]
"The world of [X] has changed dramatically..."
"[Topic] is more important than ever before."
"[Topic] is a critical component of..."
"[Topic] plays a crucial role in..."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Category 2 — Filler Commentary Phrases:**

```
CATEGORY 2 — FILLER COMMENTARY (Remove Always)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"It is important to note that..."
"It is worth noting that..."
"It is worth mentioning that..."
"It should be noted that..."
"It is crucial to understand that..."
"It is essential to recognize that..."
"Needless to say..."
"Of course..."  [when followed by something non-obvious]
"Obviously..."  [when followed by anything that requires explanation]
"As mentioned earlier..."  [acceptable once per article — not multiple times]
"As we discussed above..."
"As previously stated..."
"As you can see..."
"Simply put..."  [when the thing is not simple]
"At the end of the day..."
"The bottom line is..."  [when not used as a deliberate summary anchor]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Category 3 — Mechanical Transition Phrases:**

```
CATEGORY 3 — MECHANICAL TRANSITIONS (Transform — Replace with Natural Flow)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"Now, let's take a look at..."
"Let's dive into..."
"Let's explore..."  [as a section-leading transition]
"Now, let's move on to..."
"Without further ado..."
"With that said..."
"That being said..."  [when used more than once per article]
"Moving on..."
"To summarize..."  [when used within a section, not as a true summary]
"In conclusion..."  [acceptable only in a genuine conclusion section]
"To wrap up..."
"Finally, it is important to..."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Category 4 — AI Structural Clichés:**

```
CATEGORY 4 — AI STRUCTURAL CLICHÉS (Transform)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"Here is a breakdown of..."
"Here is a step-by-step guide to..."  [acceptable in an actual step section; remove when followed by prose]
"Below, we will cover..."
"In this section, we will discuss..."
"This section covers..."
"By the end of this article, you will..."  [acceptable in intro only; not in body]
"Whether you are a beginner or an expert..."
"Regardless of your experience level..."
"No matter where you are in your journey..."
"[X] can be broken down into [N] key components:"
"There are several key factors to consider..."
"There are a few things to keep in mind..."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Category 5 — Vague Claim Qualifiers:**

```
CATEGORY 5 — VAGUE CLAIM QUALIFIERS (Specify or Remove)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"various [nouns]"  → Replace with specific examples or a count
"different [nouns]" → Replace with specific or remove
"several [nouns]"  → Replace with number or specific examples
"many [nouns]"     → Replace with count or specific examples
"some [nouns]"     → Specify which ones or restructure
"things"           → Replace with the actual thing being referenced
"aspects"          → Replace with the actual aspects
"factors"          → Replace with the actual factors
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3B — Transformation Protocol for Detected Patterns**

When a pattern from the detection list is found:

1. **Extract the information value** from the phrase — what is the sentence actually saying beyond the filler?
2. **Reconstruct the sentence** without the filler phrase, expressing only the information value.
3. **Verify the reconstructed sentence** says the same thing as the original — not more, not less.
4. **Record the transformation** in the transformation log.

**Example transformations:**

| Original (AI pattern detected)                                                 | Humanized (pattern removed)                                                     |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| "It is important to note that regular CRM updates prevent data decay."         | "Regular CRM updates prevent data decay."                                      |
| "In today's competitive landscape, CRM software is essential for sales teams." | "CRM software has become a standard tool for sales teams managing complex pipelines." |
| "With that said, let's take a look at how to implement this."                 | "Implementation follows a predictable pattern."                                |
| "There are several key factors to consider when choosing a CRM."              | "Three factors dominate most CRM selection decisions: price, integration depth, and onboarding time." |
| "Various tools can be used for this purpose."                                  | "Salesforce, HubSpot, and Pipedrive are the tools most commonly used for this purpose." |

**Phase 3C — User-Defined Forbidden Phrases**

If the `forbidden_phrases` input is provided → add all user-specified phrases to the detection list and apply the same transformation protocol. These phrases are treated identically to the built-in list.

---

### ▸ STEP 4 — NATURAL FLOW ENHANCEMENT

Improve the transitions between paragraphs and between sections so the article reads as a cohesive, purposefully-written piece rather than a collection of independent blocks.

**Phase 4A — Paragraph Transition Audit**

For every transition between consecutive paragraphs, evaluate whether the transition is natural or abrupt:

**Abrupt transition signals (fix):**

| Signal                                                                    | Problem                                                              |
|---------------------------------------------------------------------------|----------------------------------------------------------------------|
| New paragraph begins with no logical connection to the previous          | The reader must mentally re-orient — creates friction               |
| New paragraph begins with a heading-like summary of what was just said   | This is padding — remove the summary opener and start with substance |
| New paragraph introduces a completely new concept with no bridge          | The conceptual jump feels jarring — add a transitional link          |

**Natural transition methods (apply as appropriate):**

| Transition Type              | Method                                                                              |
|------------------------------|-------------------------------------------------------------------------------------|
| **Conceptual bridge**        | End the previous paragraph with a word or concept that the new paragraph opens with (echo word) |
| **Contrast signal**          | Open the new paragraph with "But," "However," or "That said," to signal a shift    |
| **Causal link**              | Open with "Because of this," "As a result," or "This leads to" when there is a cause-effect relationship |
| **Temporal link**            | Open with "Before that," "After this," or "At the same time" for process sequences |
| **Additive link**            | Open with "This is reinforced by..." or "The same principle applies to..." for related ideas |
| **Example introduction**     | Open the new paragraph with "For instance," or "Take [specific scenario]:" when introducing an example |

**Phase 4B — Section Transition Audit**

Between H2 sections, ensure the final sentence of one section and the first sentence of the next create a coherent reading experience:

- The closing sentence of a section should complete its thought — not introduce new material.
- The opening sentence of the next section should begin that section's topic without relying on the preceding section.
- Between adjacent sections that are logically connected → a single transitional sentence may bridge them. Between adjacent sections that cover independent topics → no bridge sentence is needed.

**Prohibited section transitions:**
- "Now that we've covered X, let's look at Y." → Remove the meta-commentary; start Y directly.
- "Having explored X, Y becomes clear." → Remove; just start Y.

**Phase 4C — Logical Progression Check**

Within each H2 section, verify that the paragraphs follow a coherent information-building sequence:

| Sequence Type              | When to Use                                                                    |
|----------------------------|--------------------------------------------------------------------------------|
| **General → Specific**     | Opening paragraph states the concept; subsequent paragraphs narrow to specifics |
| **Problem → Solution**     | Opening states the challenge; closing delivers the resolution                  |
| **Claim → Evidence**       | Opening makes the assertion; following paragraphs provide supporting evidence  |
| **Context → Application**  | Opening establishes background; closing demonstrates practical use             |

If a section's paragraphs do not follow a logical sequence → reorder (not rewrite) them. Reordering does not change meaning; it only changes the sequence of pre-existing sentences.

---

### ▸ STEP 5 — READABILITY OPTIMIZATION

Ensure the article is readable at the target audience's level without being patronizing (at any level) or impenetrable (at any level).

**Phase 5A — Sentence Complexity Audit**

Identify sentences that are genuinely difficult to parse on first read — not because of vocabulary, but because of nested clauses, multiple comma-splice constructions, or excessive qualifying phrases.

**High-complexity sentence signals:**

| Signal                                                                       | Example                                                                         |
|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| More than two subordinate clauses in one sentence                           | "The tool, which integrates with Slack, which many teams use for communication, sends alerts when..." |
| Three or more commas in a non-list sentence                                 | "The process, once understood, and properly configured, in most environments, produces..." |
| Passive + subordinate + conditional stacked                                 | "It is recommended that, when this feature is enabled, which it often is in enterprise contexts, it should be..." |

**Simplification methods:**

| Problem                          | Simplification                                                                |
|----------------------------------|-------------------------------------------------------------------------------|
| Nested subordinate clauses       | Break into two sentences; move one clause to its own sentence                |
| Multiple comma-connected qualifiers | Convert to a parenthetical or move the qualifier to a separate sentence    |
| Stacked passive constructions    | Identify the real subject; rewrite in active voice                           |

**Phase 5B — Vocabulary Appropriateness Check**

For BEGINNER audiences: every technical term on first use is followed by a brief inline definition.

| Jargon Term                  | Beginner Treatment                                                              |
|------------------------------|---------------------------------------------------------------------------------|
| "SERP"                       | "SERP (Search Engine Results Page) — the page Google shows after a search"     |
| "churn rate"                 | "churn rate (the percentage of customers who cancel within a given period)"     |
| "bounce rate"                | "bounce rate (how often users leave after viewing just one page)"               |

For INTERMEDIATE audiences: define terms that are specialized beyond the baseline domain vocabulary. Do not define standard industry terms.

For EXPERT audiences: no inline definitions. Technical terms are used precisely without explanation.

**Phase 5C — Overly Complex Phrasing Simplification**

Replace unnecessarily complex phrasing with clearer alternatives where the meaning is identical:

| Complex Phrasing                        | Simpler Alternative                       |
|-----------------------------------------|-------------------------------------------|
| "In the event that"                     | "If"                                      |
| "Due to the fact that"                  | "Because"                                 |
| "With regard to"                        | "Regarding" or "About"                    |
| "In order to"                           | "To"                                      |
| "At this point in time"                 | "Now"                                     |
| "On a regular basis"                    | "Regularly"                               |
| "Utilize"                               | "Use" (unless "utilize" carries a distinct technical meaning) |
| "Facilitate"                            | "Help" or "Enable" (in non-technical contexts) |
| "Subsequent to"                         | "After"                                   |
| "Prior to"                              | "Before"                                  |
| "Approximately"                         | "[Round number]" (when precision is not required) |
| "A significant number of"              | "[Specific number]" or "Many"             |
| "It is evident that"                    | Remove — state the evident thing directly |

**CRITICAL RULE:** Vocabulary simplification does not apply to technical or domain-specific terms that carry precise meaning. "Utilize" in a legal context is not interchangeable with "use." "Facilitate" in a UX research context carries a specific meaning. The simplification list applies to general prose — not to domain-specific technical language.

---

### ▸ STEP 6 — PERSONALIZATION LAYER

Add subtle reader-engagement elements that make the writing feel addressed to a specific person rather than broadcast to a generic audience. The goal is engagement — not informality.

**Phase 6A — Reader Address Calibration**

**BEGINNER and CONVERSATIONAL tone:** Second person ("you," "your") throughout is appropriate and encouraged.

**INTERMEDIATE and PROFESSIONAL tone:** Mix of second person in instructional passages and neutral third person in analytical/descriptive passages. Do not use second person in every paragraph — vary.

**EXPERT and TECHNICAL tone:** Minimize second person. Address the reader as a peer, not a student: "The approach here is..." rather than "You should approach this by..."

**Phase 6B — Reader Engagement Cues**

Add reader engagement cues where they naturally fit — not as forced additions:

| Engagement Cue Type           | Example                                                                         | Frequency    |
|-------------------------------|---------------------------------------------------------------------------------|--------------|
| **Anticipation framing**      | "The reason this matters is less obvious than it looks."                       | 1–2 per article |
| **Acknowledgment of friction**| "This part confuses most people at first."                                     | 1 per article   |
| **Specific reader benefit**   | "If you manage a team of fewer than 20, the next section is the most relevant." | 1–2 per article |
| **Honest qualification**      | "This works well in most cases — not all."                                     | As needed    |
| **Contrast-based curiosity**  | "The answer is counterintuitive."                                               | 1 per article   |

**Phase 6C — Over-Personalization Guardrails**

The personalization layer has strict limits. These elements are PROHIBITED:

```
PROHIBITED PERSONALIZATION ELEMENTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Over-casual language:
  "Hey there!"          "Let's be real..."        "Honestly though..."
  "Fair warning:"       "Here's the thing:"       [as openers — acceptable occasionally mid-paragraph]
  "Trust me on this."   "Take it from me."        "I've seen this happen a lot."

Slang and colloquialisms:
  "game-changer"       "level up"                "crushing it"
  "huge deal"          "no-brainer"              "killer feature"
  "supercharged"       "on steroids"

Empty enthusiasm:
  "This is amazing!"   "Incredible results!"     "You won't believe..."
  "Mind-blowing"       "Revolutionary"           "Game-changing" [without evidence]

Presumptuous familiarity:
  "As fellow marketers..."   "We've all been there."   "You know the feeling."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 7 — REDUNDANCY REMOVAL

Eliminate repeated ideas and repeated phrasing that survived the generation phase. This step catches redundancies that `content-validation.skill` may flag structurally but that this skill removes at the sentence level.

**Phase 7A — Same-Sentence Redundancy**

Detect and remove phrases within a sentence that say the same thing twice:

| Redundant Form                              | Non-Redundant Form                    |
|---------------------------------------------|---------------------------------------|
| "completely eliminate"                      | "eliminate"                           |
| "advance planning"                          | "planning"                            |
| "past history"                              | "history"                             |
| "future plans"                              | "plans"                               |
| "end result"                                | "result"                              |
| "basic fundamentals"                        | "fundamentals"                        |
| "new innovation"                            | "innovation"                          |
| "return back"                               | "return"                              |
| "collaborate together"                      | "collaborate"                         |
| "summarize briefly"                         | "summarize"                           |
| "exact same"                                | "the same" or "identical"             |
| "most optimal"                              | "optimal"                             |

**Phase 7B — Same-Paragraph Redundancy**

Scan each paragraph for sentences that express the same idea as a previous sentence in the same paragraph — even if phrased differently:

**Detection method:**

For each sentence in a paragraph, compare its information content against every prior sentence in the same paragraph. If removing the sentence would lose zero unique information → the sentence is redundant.

**Handling:**
- If both sentences add unique nuance that the other lacks → keep both; combine into one sentence if possible.
- If one sentence is strictly a restatement of the other → remove the weaker one.
- If uncertain whether both are needed → flag for review; keep both in the flagged version.

**Phase 7C — Cross-Paragraph Idea Repetition**

This is a lighter pass than `content-validation.skill`'s full duplication check. Here, the focus is on phrasing-level redundancy across adjacent paragraphs — the same specific phrase appearing within 3 paragraphs of its first appearance:

**Detection:** If the same phrase of 4+ words appears in two paragraphs within 3 paragraphs of each other → rephrase the second instance.

**Exception:** Technical terms that must be referenced repeatedly by their exact name are exempt from this rule (e.g., repeating "HubSpot CRM" in a review article is not redundancy — it is accuracy).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ RHYTHM BALANCING

The goal of rhythm balancing is to produce prose that has the same kind of irregular, intentional variation in pace that skilled human writers achieve — where some passages move quickly (short, punchy sentences), others slow down and develop ideas fully, and the transitions between these modes feel intentional rather than accidental.

**Rhythm Analysis Protocol:**

1. Segment the article into natural reading units (each H2 section = one rhythm analysis unit).
2. For each unit, calculate the "rhythm signature" — the sequence of sentence lengths expressed as a string of S/M/L (Short/Medium/Long).
3. Compare the rhythm signatures of consecutive units.
4. If two consecutive units have identical or very similar rhythm signatures → vary one.

**Rhythm signature examples:**

| Rhythm Signature          | Reading Effect                                                    |
|---------------------------|-------------------------------------------------------------------|
| S-M-L-M-S-L-M             | Naturally varied — feels human-written                           |
| M-M-M-M-M-M-M             | Monotonous — feels AI-generated                                  |
| L-L-L-L-L-L-L             | Dense and heavy — academic feel, reader fatigue                  |
| S-S-S-S-S-S-S             | Choppy — bullet-prose disguised as paragraphs                    |
| S-S-L-S-S-L-S-S           | Rhythmic but predictable — noticeable pattern                    |

**Target rhythm signature:** No string longer than 3 identical consecutive length codes. The ideal is irregular but purposeful — not random.

**Rhythm modification methods:**

| Current Pattern     | Modification                                                           |
|---------------------|------------------------------------------------------------------------|
| M-M-M → M-M-M       | Shorten the second M to S; expand the fourth M to L                  |
| S-S-S → S-S-S       | Combine the second and third S into an M                              |
| L-L-L → L-L-L       | Break the second L into M+S; compress the fourth L                    |

---

### ▸ SENTENCE ENTROPY VARIATION

Entropy in this context refers to the predictability of sentence structure across consecutive sentences. Low entropy = high predictability = AI fingerprint. High entropy = natural variation = human-authored feel.

**Entropy Evaluation Method:**

For any 5-sentence sequence, evaluate the combination of:
- Opening word (varies / repeats)
- Sentence length (varies / repeats)
- Syntactic structure type (varies / repeats)
- Presence of subordinate clause (yes / no / alternates)

**Entropy Score:**

- 4 dimensions varying across 5 sentences = HIGH entropy (target state)
- 3 dimensions varying = ACCEPTABLE
- 2 dimensions varying = LOW entropy (improvement needed)
- 1 or 0 dimensions varying = VERY LOW entropy (AI pattern — transform)

**Minimum entropy standard:** Every 5-sentence window must achieve ACCEPTABLE or HIGH entropy. LOW and VERY LOW entropy windows are flagged and transformed.

---

### ▸ TONE CALIBRATION — DRIFT DETECTION

Tone drift occurs when different sections of the article were generated with slightly different register — creating an inconsistent reading experience. A professional introduction followed by a conversational mid-section and a formal conclusion is a classic AI-generation artifact.

**Drift Detection Method:**

1. Score each H2 section for tone register on a 5-point scale:
   - 1 = Very formal (passive, academic, no contractions)
   - 3 = Balanced professional
   - 5 = Very casual (contractions, second person, colloquial)

2. Calculate the range of tone scores across all sections.

3. If range > 2 → tone drift detected — sections at the extremes need calibration toward the middle.

**Calibration Rules:**

| Drift Direction                        | Calibration Action                                                    |
|----------------------------------------|-----------------------------------------------------------------------|
| Section is too formal vs. target tone  | Add one contraction where natural; convert one passive to active     |
| Section is too casual vs. target tone  | Remove one contraction in a formal claim; remove one colloquial phrase|
| Two adjacent sections differ by > 2   | Bring both one step toward each other — meet in the middle           |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Phrases After Transformation

After all seven transformation steps are complete, run a final phrase deduplication scan:
- No phrase of 5+ words may appear more than once within the same H2 section.
- No phrase of 7+ words may appear more than once in the entire article.
- Exception: Technical terms, proper nouns, and `preserve_exactly` phrases are exempt.

### Rule 2 — No Repeated Sentence Structures Within a Paragraph

Within any paragraph, no two sentences may share both the same opening word AND the same syntactic structure. If such a pair is found after all transformation steps → transform the second instance.

### Rule 3 — No Repeated Opening Words Across Consecutive Paragraphs

If two consecutive paragraphs begin with the same word → transform the opening of the second paragraph. This catches a different level of repetition than the within-paragraph opening word audit in Step 1.

### Rule 4 — Preserved Phrase Non-Duplication

`preserve_exactly` phrases appear only where they appeared in the original. They are not duplicated by the transformation process. If the transformation of surrounding text would create a situation where a preserved phrase appears twice → restructure the surrounding text only.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Humanizer Behaviors

| Prohibited Behavior                                                                              | Reason                                                                           |
|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Adding new sentences or paragraphs to improve "flow"                                             | Humanizer does not expand content — expansion is the generation engine's job     |
| Removing factual sentences because they "feel robotic"                                           | Factual content is never removed — only the expression of that content is changed |
| Changing a technical term to a simpler word when the technical term is more precise              | Precision is prioritized over simplicity in technical writing                    |
| Moving keywords from their placed positions to improve sentence flow                             | Keyword placements are locked — transformations work around them                 |
| Applying casual tone elements (contractions, slang) to a Professional-tone article               | Tone preference is a hard constraint — not overridden by "natural feel" logic   |
| Replacing specific statistics or data points with vague equivalents                              | Data precision is locked — "14,000 companies" is not simplified to "thousands"  |
| Reordering paragraphs within a section for "better flow"                                         | Section-level reordering is structural editing — outside this skill's scope     |
| Applying the beginner-level vocabulary rules to an Expert-tone article                           | Expertise level calibration is directional — beginner rules do not apply upward |
| Transforming `preserve_exactly` phrases                                                          | These are explicitly exempted — transformation must route around them            |
| Flagging uncertainty by removing the phrase rather than keeping it                               | Failsafe is KEEP AND FLAG — never REMOVE AND PROCEED                            |

### Required Humanizer Behaviors

- Output word count must be within ±3% of input word count.
- All keyword placements must be preserved and verified in the output.
- Transformation log must document every significant change.
- Flagged uncertain transformations must be listed separately for human review.
- The seven transformation steps must all be applied in sequence — no step may be skipped.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Called by content-generation-engine (Post-Generation Phase 7A)                             |
| **Receives**     | Full article draft, target audience, tone preference, configuration metadata               |
| **Protocol**     | Invoked as Phase H in the post-generation pipeline — before accuracy checker               |
| **Constraint**   | Must not alter structural elements (headings, blueprint structure) or factual content      |

---

### ▸ technical-accuracy-checker.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Humanizer → technical-accuracy-checker (output consumer)                                   |
| **Sends**        | Humanized article with transformation log                                                  |
| **Relationship** | Technical accuracy checker runs on the humanized output — humanization first, then accuracy |
| **Critical note** | Accuracy checker must verify that humanization has not introduced inaccuracies through paraphrase |

---

### ▸ content-validation.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Humanizer → content-validation (final downstream consumer)                                 |
| **Sends**        | Humanized, accuracy-verified article                                                        |
| **Used for**     | Validation checks all elements that humanization must preserve — structure, keyword placement, completeness |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after transformation is complete)                                                   |
| **Writes**       | Transformation log, flagged-for-review list, output package metadata                      |
| **Memory layer** | content-memory.skill                                                                        |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                               | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Generated content is empty                                                | Input pre-condition — null or empty content            | Halt. Log: "Cannot humanize empty content — content-generation-engine.skill output required."            |
| Target audience is not provided                                           | Input pre-condition — null audience                    | Default to INTERMEDIATE expertise + PROFESSIONAL tone. Flag all Step 2 decisions as DEFAULTED.          |
| A transformation would change the factual meaning of a sentence          | Step-level meaning check — semantic shift detected      | Keep original. Add to FLAGGED FOR REVIEW list with note: "Possible meaning change — kept original."     |
| A keyword is displaced from its original position by a transformation    | Post-transformation keyword audit — keyword missing    | Restore keyword to its original position. Restructure the surrounding transformation to accommodate it.  |
| Output word count falls below -3% of input                               | Final word count check                                  | Identify which transformations caused the reduction. Restore the most significant removed phrases while keeping their humanized form. |
| Output word count rises above +3% of input                               | Final word count check                                  | Remove the weakest engagement cue added in Step 6. If still over → remove one of the shortest bridging transitions added in Step 4. |
| More than 20% of sentences are flagged as uncertain                      | Transformation log — flag count exceeds threshold       | Do not block output. Flag the entire document as HIGH UNCERTAINTY RATE. Present output with all flags clearly marked. Recommend human review before publishing. |
| `preserve_exactly` phrase is also an AI pattern (appears in detection list) | Step 3 cross-check — conflict detected               | `preserve_exactly` takes precedence. Do not transform the phrase. Log: "Preserved phrase conflicts with AI pattern list — preserved as instructed." |
| Tone calibration produces drift in the opposite direction (CASUAL → MORE FORMAL when target is CASUAL) | Tone drift check reversal | Stop calibration at the calibration point. Do not over-correct. Log: "Calibration boundary reached — further adjustment would invert drift direction." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — AI PATTERN DETECTION CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
AI PATTERN DETECTION CHECKLIST (Run Before Finalizing Output)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ No Category 1 Generic Opener Phrases remain in the article
□ No Category 2 Filler Commentary Phrases remain
□ No Category 3 Mechanical Transition Phrases remain
□ No Category 4 AI Structural Clichés remain
□ No Category 5 Vague Claim Qualifiers remain
□ No three consecutive sentences with the same opening word (within any paragraph)
□ No three consecutive sentences of near-identical length (within ±3 words)
□ No two consecutive paragraphs beginning with the same word
□ No passage of 5 sentences with entropy score < ACCEPTABLE
□ No tone drift > 2 points between any two adjacent sections
□ No same-sentence redundancies (completely eliminate, advance planning, etc.)
□ No same-paragraph idea restatement (different words, same meaning)
□ No 5+ word phrase appearing twice in the same H2 section
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — TRANSFORMATION LOG FORMAT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
TRANSFORMATION LOG ENTRY FORMAT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Step            : [Step N — Step Name]
Location        : [Section name + paragraph number]
Original        : "[original phrase or sentence]"
Transformed     : "[humanized phrase or sentence]"
Reason          : [AI pattern removed / Sentence variation / Tone adjustment / etc.]
Meaning Check   : PRESERVED [confirmed same meaning]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

FLAGGED FOR REVIEW ENTRY FORMAT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Location        : [Section name + paragraph number]
Phrase          : "[phrase kept unchanged]"
Flag Reason     : [Why transformation was uncertain]
Recommended Action: [What a human reviewer should evaluate]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — SENTENCE VARIATION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
SENTENCE VARIATION QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OPENING WORD DIVERSITY:
  Within any 5-sentence span → no opening word appears > 2 times
  Within any single paragraph → no opening word appears > 1 time (first occurrence only)

LENGTH DISTRIBUTION TARGET (per paragraph):
  Short (≤12w)   : 20–35% of sentences
  Medium (13–22w): 45–55% of sentences
  Long (23–35w)  : 15–30% of sentences
  Very long (>35w): < 10% of sentences

SYNTACTIC VARIETY (within any 4-sentence span):
  No more than 2 sentences sharing the same syntactic structure type

RHYTHM SIGNATURE:
  Target: Irregular but intentional — S-M-L-M-S or similar
  Prohibited: Any string of 4+ identical length codes (M-M-M-M, S-S-S-S)

ENTROPY STANDARD:
  Minimum: ACCEPTABLE (3 of 4 dimensions varying across 5 sentences)
  Target : HIGH (all 4 dimensions varying)
  Prohibited: LOW or VERY LOW (1–2 dimensions varying)

THE IRON RULE (repeated from Section 1):
  Do NOT change meaning.
  Do NOT remove important content.
  Only improve expression.
  When uncertain → keep original + flag.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## v3.0 UPGRADE — SELF-CRITICISM PASS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**NEW in v3.0: After the standard humanization pass, run this mandatory self-criticism audit.**

### Step 1 — Self-Criticism Question
Re-read the entire humanized output. Ask yourself: "What makes this STILL obviously AI generated?"

Look specifically for:
- **Structural uniformity**: Are all paragraphs roughly the same length? Do all sections follow the same intro → detail → summary pattern?
- **Vocabulary monotony**: Is the same word used 4+ times across different sections? Are transition words repetitive?
- **Hedging residue**: Phrases like "it's worth noting," "it should be mentioned," "it's important to understand" — these are AI tells even after humanization
- **Paragraph length regularity**: If every paragraph is 3-4 sentences, that's a pattern. Real writers vary between 1 and 6 sentences.
- **Opening word repetition**: Check first words of consecutive paragraphs. If 3+ start with the same word or structure, fix it.
- **Formulaic section endings**: Do sections end with a summary sentence that restates the section topic? That's a template behavior.

### Step 2 — Fix Remaining Tells
For each tell found, apply a targeted fix:
- Structural uniformity → Deliberately make 2-3 sections shorter and 1-2 longer
- Vocabulary monotony → Replace repeated words with natural alternatives or restructure sentences
- Hedging residue → Delete the hedging phrase entirely; the sentence is usually stronger without it
- Paragraph regularity → Split 1 long paragraph, merge 2 short ones
- Opening repetition → Rewrite 2 of the 3 openers with different structures
- Formulaic endings → Delete the summary sentence or replace with a forward-looking statement

### Step 3 — Report
Add to humanizer output:
```
SELF-CRITICISM FINDINGS:
- [X] structural tells found and fixed
- [X] vocabulary repetitions resolved
- [X] hedging phrases removed
- [X] paragraph regularity broken
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## v3.0 UPGRADE — KEYWORD PRESERVATION CHECK
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**NEW in v3.0: After humanization + self-criticism, verify keyword placements survived.**

AI pattern removal often clusters around important keyword positions — removing them can accidentally remove keyword signals.

### Mandatory Keyword Position Check
After all humanization passes, verify the primary keyword still appears in:

| Position | Required | Action if Missing |
|---|---|---|
| Title / H1 | YES | Restore exact match or close variant |
| First 100 words | YES | Reinsert naturally in opening paragraph |
| Meta description | YES | Verify (metadata-generator handles this, but check) |
| At least 2 H2 headings | YES | Restore as natural variant if removed |
| Body (0.5-1.5% density) | YES | Recalculate density; add instances if below 0.5% |

### Report
Add to humanizer output:
```
KEYWORD PRESERVATION:
- Primary keyword: [keyword]
- Title/H1: [PRESENT / RESTORED]
- First 100 words: [PRESENT / RESTORED]
- H2 headings: [X] of [Y] contain keyword
- Density: [X]% [WITHIN RANGE / ADJUSTED]
```

---

*SKILL FILE END: content/humanizer.skill*
*Nexus SEO Operating System — Version 3.0.0*
