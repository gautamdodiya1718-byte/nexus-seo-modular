# ai-citation-pattern-analyzer.skill

**Role:** Analyze how AI search surfaces (Perplexity, ChatGPT search, AI Overviews, Gemini, Claude) phrase queries on a given topic differently from traditional Google search, and structure content outlines to win AI citations specifically.

**Loaded by:** `nexus-research` SKILL.md in modes: content_outline, full_research_package
**Companion to:** `ai-citation-scorer.skill.md` (in nexus-audit) — that one scores existing content; this one informs blueprint design

---

## Why this skill exists

AI search surfaces don't return a list of links — they return synthesized answers with citations. Being cited requires content structured for clean extraction. Traditional SEO optimizes for rank-and-click; AI citation optimizes for rank-and-lift.

This skill makes those optimization patterns explicit at the research stage so blueprints are built with AI citation in mind, not retrofitted later.

---

## Inputs

Required:
- Target keyword + intent
- Content outline (from serp-blueprint-generator) OR working draft of outline

Optional:
- Top 10 SERP results (to compare AI patterns vs Google patterns)

---

## Process

### Step 1 — AI query pattern analysis

For the target topic, identify how users phrase queries to AI surfaces vs Google:

**Google search patterns** (typically):
- Short, keyword-optimized: "playwright vs cypress speed"
- Single-concept queries
- User clicks results to find their answer

**AI search patterns** (typically):
- Longer, more conversational: "Is Playwright faster than Cypress for browser testing?"
- Multi-part queries: "What's the difference between Playwright and Cypress in CI/CD performance, and which one should I use for React apps?"
- Includes context and constraints

Use web_search if available to identify how this specific topic is queried in AI vs Google contexts. Otherwise, infer from:
- PAA questions (which are typically AI-style queries Google has indexed)
- Long-tail variants of the keyword
- Forum/Reddit phrasings of the topic

### Step 2 — Define answerable chunks

For each likely AI query about this topic, identify what chunk of content would directly answer it. Each chunk must be:
- 1-3 paragraphs maximum
- Self-contained (no "as mentioned above" dependencies)
- Ends with a clear conclusion or recommendation

Output: list of "AI-answerable questions" with their corresponding content chunks.

### Step 3 — Structural recommendations for the outline

Based on AI citation patterns, recommend:

**A) Definition-first sections**
- Each H2 should start with (or near the start) a clear, citation-friendly definition
- Format: "[Concept] is [one-sentence definition]. [Optional second sentence with key context.]"
- Why: AI surfaces extract these definitions for "what is X" queries

**B) Self-contained answer paragraphs**
- Mark which paragraphs in the outline should be designed as standalone answer chunks
- These paragraphs must minimize dependencies on prior context
- Why: AI surfaces lift paragraphs for citation; dependency-heavy paragraphs get skipped

**C) Comparison tables for vs/comparison queries**
- If keyword has comparison intent, recommend a structured comparison table near the top
- Why: AI surfaces extract comparison data wholesale from tables

**D) Numbered steps for how-to queries**
- If keyword has tutorial intent, recommend numbered steps with clear inputs/outputs
- Why: AI surfaces use numbered structure to extract complete procedures

**E) Statistical claims with sources**
- Mark which sections need statistical claims with linked sources
- Why: AI surfaces favor citation-worthy sources for stat queries

**F) FAQ section or FAQPage schema candidates**
- Mark questions that should be in an FAQ section + flagged for FAQPage schema
- Why: FAQPage schema is heavily used by AI surfaces for question extraction

**G) Definition cards / callouts**
- Mark concepts that should be in definition-style callouts (visually distinct blocks)
- Why: AI surfaces favor visually-distinct definition blocks

### Step 4 — Citation-readiness checklist for the outline

Apply checklist to the proposed outline:
- [ ] Every H2 has a definition near its start
- [ ] At least 60% of paragraphs are designed as self-contained answer chunks
- [ ] Comparison content (if applicable) has a structured table
- [ ] How-to content (if applicable) has numbered steps
- [ ] Statistical claims have source citations planned
- [ ] FAQ section present + FAQPage schema planned
- [ ] Schema markup planned (matches content type)
- [ ] Author byline + bio link planned
- [ ] Publish + update dates planned

Score the outline 0-100 on AI citation readiness.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: ai-citation-pattern-analyzer
Status: COMPLETE
Inputs consumed: outline, target keyword [+ optional inputs]

AI QUERY PATTERN ANALYSIS:
  Likely AI queries on this topic:
    1. "[example AI query]" — answers found in [outline section]
    2. "[example AI query]" — answers found in [outline section]
    3. ...

  Note: AI queries are typically longer and more conversational than Google queries.

ANSWERABLE CHUNKS IDENTIFIED:
  1. Question: "[question]"
     Chunk source: [outline section X]
     Citation-friendly: [YES — paragraph is self-contained / NO — needs restructuring]

  2. ...

STRUCTURAL RECOMMENDATIONS FOR OUTLINE:
  Definition-first sections:
    - H2 "[section]" should start with definition: "[suggested definition format]"

  Self-contained answer paragraphs:
    - Paragraphs in [section] need to remove dependencies on prior context
    - Specifically: avoid "as mentioned above", "this", "the one we discussed"

  Comparison tables (if applicable):
    - Recommended placement: [section]
    - Suggested columns: [list]
    - Suggested rows: [list]

  Numbered steps (if applicable):
    - Recommended placement: [section]
    - Step structure: [Input → Action → Expected Output per step]

  Statistical claims with sources:
    - Section [X]: needs claims with linked sources
    - Suggested source quality: primary sources (official docs, original studies)

  FAQ section + schema:
    - Recommended FAQ placement: [section]
    - Suggested FAQ questions (from AI query analysis): [list]
    - FAQPage schema: PLANNED

  Definition callouts:
    - Concepts that should be in callout blocks: [list]

CITATION-READINESS CHECKLIST:
  ☐ Every H2 has a definition near start
  ☐ ≥60% paragraphs designed as self-contained
  ☐ Comparison table (if applicable)
  ☐ Numbered steps (if applicable)
  ☐ Statistical claims have source citations planned
  ☐ FAQ section + FAQPage schema
  ☐ Article/HowTo schema planned
  ☐ Author byline + bio link planned
  ☐ Publish + update dates planned

OUTLINE AI CITATION READINESS SCORE: [X/100]

REVISION RECOMMENDATIONS (priority order):
  1. [highest-impact change to outline]
  2. ...
  3. ...

CONFIDENCE: HIGH (web_search available + SERP context) / MEDIUM (limited context) / LOW (no SERP, no web_search)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Distinct from traditional SEO.** AI citation optimization can recommend changes that look "less SEO-optimal" but are better for AI lift. Make the trade-off visible to the user.

2. **Web search informs query patterns.** When available, use web_search to find how the topic is actually queried in AI vs Google contexts.

3. **Schema is a major lever.** AI surfaces use schema heavily for type detection. Always recommend appropriate schema in the outline.

4. **Self-contained paragraphs is the biggest factor.** Most "good" SEO content has dependent paragraphs. Recommending standalone topic sentences is the highest-impact change.

5. **FAQ section + FAQPage schema is high ROI.** Even for non-question-led content, an FAQ section with FAQPage schema captures AI citation opportunities for related questions.

6. **Brand-agnostic.** AI citation patterns don't depend on brand context. Works fine without brand profile.
