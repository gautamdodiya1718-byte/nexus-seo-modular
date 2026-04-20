# ai-citation-optimizer.skill

**Role:** Apply AI citation optimization patterns DURING content generation (not as post-hoc audit). Companion to `ai-citation-pattern-analyzer` (in nexus-research, which informs outline design) and `ai-citation-scorer` (in nexus-audit, which scores existing content). This skill applies the patterns at write time.

**Loaded by:** `nexus-content` SKILL.md in all generation modes

---

## Why this skill exists

AI citation optimization can't be retrofitted effectively. Content that reads as flowing prose has dependent paragraphs that can't be lifted cleanly. Restructuring after generation often breaks voice and flow.

This skill applies AI citation patterns as the content is being generated, not after.

---

## Inputs

Required:
- Content section being generated (or about to be generated)
- Target keyword + intent
- Output from `ai-citation-pattern-analyzer` if available (from nexus-research)

Optional:
- AI query patterns analysis (which AI queries this content should answer)
- Brand profile

---

## The 7 patterns applied during generation

### Pattern 1 — Self-contained paragraphs

For each paragraph being generated:
- Start with a topic sentence that defines what the paragraph is about
- Avoid pronouns that reference prior context ("this", "that", "the one we discussed")
- Avoid backward references ("as mentioned above", "as we saw")
- Each paragraph should be readable as a standalone answer to a question

Generation rule: every paragraph either (a) introduces a new concept or fact (with topic sentence), or (b) develops the topic sentence with specifics (still self-contained because the topic sentence anchors it).

### Pattern 2 — Dense factual paragraphs

Avoid filler/transition paragraphs entirely:
- No "Now let's look at..." paragraphs
- No "It's worth noting that..." paragraphs
- No "In conclusion..." restating paragraphs
- Information density target: ≥1 specific fact per paragraph (number, name, date, citation)

### Pattern 3 — Definition-first H2 sections

For each H2:
- Start with (or in the second sentence) a clear, citation-friendly definition of the section's topic
- Format: "[Concept] is [definition]. [Optional context sentence.]"
- Even non-definition H2s should have a strong opening that AI can lift as a section summary

### Pattern 4 — Cited claims with linked sources

For factual claims:
- Cite source inline as the claim is made
- Prefer primary sources (official docs, research papers, vendor announcements)
- Link inline: `[claim text](source URL)` — not "according to a study" without link

### Pattern 5 — Numerical specificity

Replace generic quantifiers with specific numbers:
- "Many users" → "Over 5,000 users"
- "Significantly faster" → "47% faster"
- "Some teams report" → "12 of 50 teams surveyed reported"
- "Recently" → "In Q1 2024"

If specific number isn't available, use ranges or rephrase as opinion ("In our experience...") rather than claim it as fact.

### Pattern 6 — Schema-friendly structure

Generate content that matches the schema being used:
- For HowTo: clear numbered steps with name + text per step
- For FAQPage: explicit question-then-answer pattern in FAQ section
- For Article: clear headline + body + author + dates pattern

### Pattern 7 — Attribution-friendly content

Include in content:
- Author byline (visible, with bio link)
- Publish + update dates (visible)
- Brand publisher name (visible, usually in byline area)
- Source citations linked, not just named

---

## Process

### Step 1 — Pre-generation: load AI citation pattern analysis

If `ai-citation-pattern-analyzer` was run upstream (from nexus-research), load its output:
- Likely AI queries this content should answer
- Recommended structural changes to outline
- AI citation readiness score target

### Step 2 — During generation: apply patterns

As content-generation-engine produces each section:
- Run each generated paragraph through the 7 patterns
- Flag any paragraph that violates a pattern; mark for rewrite
- Don't release section to next stage until all paragraphs conform

### Step 3 — Post-generation: AI citation score

After content is fully generated, run AI-citation-style scoring:
- Self-containment score (paragraph-level)
- Density score
- Definition coverage
- Citation coverage
- Numerical specificity
- Schema alignment
- Attribution coverage

Target: AI citation readiness ≥85 for content to pass through.

If <85, identify specific paragraphs/sections that drag score down and trigger targeted rewrite.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: ai-citation-optimizer
Status: COMPLETE
Inputs consumed: content [N words], optional ai-citation-pattern-analyzer output

PATTERN APPLICATION DURING GENERATION:
  Self-contained paragraphs: [N of M paragraphs conform — %]
  Dense factual paragraphs (≥1 fact): [N of M — %]
  Filler paragraphs detected and removed: [N]
  Definition-first H2 sections: [N of M — %]
  Cited claims with linked sources: [N of M factual claims — %]
  Numerical specificity (specific vs generic): [N of M quantitative claims — %]
  Schema alignment: [PASS / partial / FAIL]
  Attribution elements present: [byline ✓, dates ✓, publisher ✓, citations ✓]

AI CITATION READINESS SCORE: [X/100]

PARAGRAPH-LEVEL CITATION READINESS (sample of top 10 paragraphs):
  Paragraph 1: [citation_ready / partial / not_citable] — [reason]
  ...

AI CITATION USE CASES THIS CONTENT CAN SERVE:
  ✓ [Definition lookups / how-to / comparison / list / statistical / opinion]
  ✗ [Use cases this content is NOT well-positioned for]

REWRITE PASSES TRIGGERED:
  [N paragraphs rewritten for self-containment]
  [N paragraphs rewritten for density (filler removal)]
  [N H2 sections restructured for definition-first]

FINAL DECISION: PASS (≥85 score) / NEEDS_FIX (<85)

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Apply during generation, not after.** Restructuring after often breaks voice — better to write self-contained from the start.

2. **Don't sacrifice voice for AI citation.** If a pattern would damage author voice, skip the pattern for that paragraph and note the trade-off.

3. **Specific numbers over generics.** This is the highest-leverage change. Train generation to default to specifics.

4. **Definition-first H2s for AI surfaces.** Even non-definition sections benefit from strong opening sentences.

5. **Schema must match content.** Don't generate FAQPage schema if there's no FAQ section. Don't generate HowTo if not actually how-to.

6. **Score is the gate.** <85 triggers rewrite. Above 85, content passes.
