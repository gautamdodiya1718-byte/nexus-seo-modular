# ai-citation-scorer.skill

**Role:** Score how readily AI search surfaces (Perplexity, ChatGPT search, AI Overviews, Gemini, Claude) will cite or lift content from a page. AI citation patterns differ from traditional SERP ranking — different structural signals matter.

**Loaded by:** `nexus-audit` SKILL.md in modes: full_audit, aeo_geo_scoring
**Also loaded by:** `nexus-content` SKILL.md (as `ai-citation-optimizer.skill.md` — same logic, generation context)

---

## Why this skill exists

AI search surfaces lift content for citations differently than Google ranks pages. They favor:
- Dense factual paragraphs (not flowery prose)
- Clear definition blocks at section starts
- Cited claims with primary sources
- Specific numerical values
- Self-contained answer chunks (extractable without context)
- Explicit attribution patterns AI can parse

Content optimized for traditional SEO can fail at AI citation if it lacks these patterns. This skill scores on AI-citation criteria specifically.

---

## Inputs

Required:
- Content to evaluate (URL, file, or pasted)
- Target keyword + intent

Optional:
- Brand profile

---

## The 7 AI citation criteria

### Criterion 1 — Self-contained answer chunks (30% weight)

AI surfaces lift paragraphs as standalone answers. Each paragraph that could potentially be cited should be self-contained — meaningful without surrounding context.

Check:
- Each paragraph: can it be read as a complete answer to a specific question?
- Does each paragraph have a clear topic sentence that defines what it's about?
- Are dependencies (pronouns referring to prior context, "as mentioned above", "this") minimized?

Score (0-100):
- HIGH (80+): most paragraphs self-contained, can be lifted cleanly
- MEDIUM (50-79): some paragraphs work, others depend on context
- LOW (<50): content reads as a single flowing piece; AI can't extract clean chunks

### Criterion 2 — Dense factual paragraphs (15% weight)

AI prefers high information density. Filler/transition paragraphs ("Now let's look at...", "It's important to note that...") get skipped.

Check:
- Information-to-words ratio per paragraph
- Filler/transition paragraphs as % of total
- Specific facts (numbers, names, dates, citations) per paragraph

Score:
- HIGH: ≥1 specific fact per paragraph; <10% filler paragraphs
- MEDIUM: ≥0.5 facts per paragraph; 10-25% filler
- LOW: low fact density; >25% filler

### Criterion 3 — Clear definitions at section starts (15% weight)

AI extracts definitions for "what is X" queries. Sections should start with clear definitions of the concept they cover.

Check:
- Does each H2 section start with (or near the start) a clear, citation-friendly definition of the section's topic?
- Are definitions extractable as standalone sentences?

Score:
- HIGH: every H2 has a clear definition near its start
- MEDIUM: some H2s have definitions, others meander
- LOW: H2s start with examples or anecdotes; no clear definitions

### Criterion 4 — Cited claims with primary sources (15% weight)

AI cites content that itself cites authoritative sources. This is meta-citation: "I trust this page because it cites trustworthy sources."

Check:
- % of factual claims with linked source citations
- Source quality: primary sources (official docs, research papers, vendor announcements) vs secondary (blogs citing other blogs)

Score:
- HIGH: ≥80% of factual claims have primary source links
- MEDIUM: 50-79% with mostly secondary sources
- LOW: <50% with citations, or citations are mostly to blogs

### Criterion 5 — Numerical specificity (10% weight)

AI strongly prefers specific numerical values for citation. "47% reduction" beats "significant reduction".

Check:
- % of quantitative claims with specific numbers (vs generic quantifiers)
- Numerical claims with cited sources

Score:
- HIGH: ≥80% of quantitative claims have specific numbers
- MEDIUM: 50-79%
- LOW: <50%; mostly generic quantifiers

### Criterion 6 — Structured data (schema markup) (10% weight)

AI uses schema to understand content type and key fields. FAQPage schema, HowTo schema, Article schema with proper fields all help AI extraction.

Check (delegates to source-code-parser if available):
- Schema markup present
- Schema type matches content type
- Required fields populated
- Schema actually corresponds to content (not fake schema for SEO)

Score:
- HIGH: appropriate schema present, valid, matches content
- MEDIUM: schema present but incomplete or partially mismatched
- LOW: no schema or wrong schema type

### Criterion 7 — Attribution-friendly structure (5% weight)

AI surfaces typically credit content by domain + author. Easy attribution helps.

Check:
- Author byline visible and parseable
- Author bio link present
- Publication name/brand visible
- Publish/update dates visible

Score:
- HIGH: all 4 attribution signals present
- MEDIUM: some present
- LOW: anonymous or unparseable attribution

---

## AI citation type classification

Beyond scoring, identify which AI citation use cases this content is well-positioned for:

- **Definition lookups** ("what is X") — strong if Criterion 3 is HIGH
- **How-to queries** ("how to do X") — strong if HowTo schema + numbered steps
- **Comparison queries** ("X vs Y") — strong if comparison table present + clear pros/cons
- **List queries** ("best X for Y") — strong if list structure + criteria + ranked options
- **Statistical queries** ("how many", "what %") — strong if Criterion 5 is HIGH
- **Opinion queries** ("is X good") — strong if opinion claims with reasoning + author credibility

Output: which use cases this content can serve as AI citation source for, and which it cannot.

---

## Score and overall assessment

Weighted score across 7 criteria. Ranges:
- 85+ = STRONG (likely to be cited frequently in AI surfaces)
- 70-84 = ADEQUATE (may be cited sometimes for specific use cases)
- Below 70 = WEAK (content optimized for traditional SEO but not AI surfaces)

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: ai-citation-scorer
Status: COMPLETE
Inputs consumed: content [N words], target keyword

AI CITATION SCORE: [X/100] — [STRONG / ADEQUATE / WEAK]

CRITERION SCORES:
  Self-contained paragraphs:    [X/100] (weight 30%)
  Dense factual paragraphs:     [X/100] (weight 15%)
  Clear section definitions:    [X/100] (weight 15%)
  Cited primary sources:        [X/100] (weight 15%)
  Numerical specificity:        [X/100] (weight 10%)
  Structured data (schema):     [X/100] (weight 10%)
  Attribution-friendly structure: [X/100] (weight 5%)

AI CITATION USE CASES THIS CONTENT CAN SERVE:
  ✓ [Definition lookups / how-to / comparison / list / statistical / opinion]
  ✗ [Use cases this content is NOT well-positioned for, with reason]

KEY GAPS (with specific fixes):
  1. [Criterion]: [specific gap]
     Fix: [exact action — e.g., "Each paragraph in Section 4 begins with 'For example,' or 'Now,' — these are dependent transitions. Rewrite each paragraph to start with a self-contained topic sentence."]

  2. ...

PARAGRAPH-LEVEL CITATION SCORE (top 10 paragraphs):
  Paragraph 1 (intro): [Citation-ready / Almost / Not citable] — [reason]
  Paragraph 2: ...
  [Identifies which paragraphs AI is most likely to lift, and which are unusable]

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Self-containment is the biggest factor.** Most "good" SEO content fails this. Most paragraphs need topic sentences and minimal pronoun references back.

2. **Specific recommendations.** Not "improve AI readiness" — "rewrite paragraphs 4, 7, 9 to remove 'as mentioned above' references and add topic sentences."

3. **Don't conflate with traditional SEO.** Content can score 85+ on traditional SEO and 40 on AI citation. Make the difference visible to user.

4. **Schema is heavy weight.** AI surfaces increasingly use schema for type detection. Missing or wrong schema kills AI citation chances even with great content.

5. **Numbers matter disproportionately.** AI surfaces favor cite-able specifics. Content full of generic quantifiers ("many", "significantly", "a wide range of") scores low here even when SEO scores high.
