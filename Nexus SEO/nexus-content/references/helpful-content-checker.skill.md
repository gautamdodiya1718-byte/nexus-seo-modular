# helpful-content-checker.skill

**Role:** Score content against Google's published Helpful Content Update (HCU) quality questions. Replaces the vague "is this helpful?" check with explicit, scored, per-question evaluation.

**Loaded by:** `nexus-audit` SKILL.md in modes: full_audit, helpful_content_check
**Also loaded by:** `nexus-content` SKILL.md as a quality gate during generation

---

## Why this skill exists

Google publishes ~30 explicit questions to ask about content quality (the Helpful Content guidance). Most content systems treat these as decoration. This skill turns them into structured, scored checks with specific revision instructions when checks fail.

---

## Inputs

Required:
- Content to evaluate
- Target keyword + intent (asks if not provided)

Optional:
- Top 3 SERP results for the same keyword (for "substantially better than other pages" check)
- Brand profile and author profile (enriches checks)

---

## The Helpful Content question set

Organized into 4 themes per Google's framework. Each question scored on 0-10 scale with structural check.

### Theme 1 — Content and Quality

**Q1.1 — Does the content provide original information, reporting, research, or analysis?**

Check:
- Original information: Are there claims, data, or angles not present in the top 10 SERP results?
- Original reporting: Is there primary research (interviews, original surveys, original benchmarks)?
- Original analysis: Is there interpretation/synthesis beyond aggregation?
- Score: HIGH if ≥3 markers of originality; MEDIUM if 1-2; LOW if zero — content is just summary

**Q1.2 — Does the content provide substantial, complete, or comprehensive description of the topic?**

Check:
- Are key sub-topics covered? (cross-reference with PAA + competitor topics if available)
- Is each section developed to substance threshold (not just surface)?
- Are obvious questions readers will ask answered?
- Score: HIGH if comprehensive; MEDIUM if main topic covered but sub-topics thin; LOW if shallow

**Q1.3 — Does the content provide insightful analysis or interesting information that goes beyond the obvious?**

Check:
- Look for "non-obvious" markers: counter-intuitive points, contrarian positions, surprising findings, edge cases
- Score: HIGH if multiple non-obvious points; MEDIUM if 1; LOW if entirely conventional wisdom

**Q1.4 — If borrowing from sources, does the content add substantial value and originality?**

Check:
- If content cites/references other sources, does it add interpretation, contradiction, or new framing?
- Score: HIGH if cited sources are extended/critiqued/synthesized; LOW if just summarized

**Q1.5 — Is the headline/page title descriptive and not exaggerating or shocking?**

Check:
- Headline accuracy: does the content deliver what the title promises?
- Clickbait patterns: "You won't believe...", excessive use of "X things you must..."
- Score: HIGH if accurate + non-clickbait; LOW if mismatch or clickbait

**Q1.6 — Would users bookmark this, share it, or recommend it?**

Check (proxies):
- Does the content provide actionable tools, templates, or specific decision frameworks readers can return to?
- Specific numerical values or formulas that need re-reference?
- Score: HIGH if reference-worthy; MEDIUM if useful once; LOW if disposable read

### Theme 2 — Expertise

**Q2.1 — Does the content present information so well that you would want to cite it as a reference?**

Check:
- Specificity, citation-worthiness of claims
- Authority of voice (depends on author profile + first-person experience markers)
- Cross-check with eeat-enforcer's Authoritativeness score
- Score: HIGH if AI-citation-scorer also gives HIGH; LOW if generic

**Q2.2 — Is the content clearly being written by an expert or enthusiast who demonstrably knows the topic well?**

Check (delegated mostly to eeat-enforcer):
- First-person experience markers
- Practitioner-only scenarios
- Author credibility match
- Score: copy from eeat-enforcer Experience score

**Q2.3 — Is the content free from easily-verified factual errors?**

Check (delegated to live-fact-verifier):
- All factual claims verified
- Score: copy from live-fact-verifier confidence

**Q2.4 — Would you trust the information for important issues like health, financial, or safety decisions (YMYL)?**

Check:
- Author credentials visible (byline + bio link)
- Citations to authoritative sources for YMYL claims
- Disclosures where appropriate
- Conservative language for uncertain claims
- Score: only applies to YMYL topics; HIGH if all signals present, LOW if any missing

### Theme 3 — Presentation and Production

**Q3.1 — Is the content free of spelling/stylistic issues?**

Check:
- Spelling, punctuation, grammar
- Consistency in capitalization, list formatting, code formatting
- Score: HIGH if clean; LOW if multiple issues

**Q3.2 — Is the content well-produced or does it appear sloppy or hastily-produced?**

Check:
- Visual breaks (headings, lists, images, callouts) every ~300 words
- Code formatting consistent
- Image alt text present
- No broken internal links (delegated to link-verifier)
- Score: HIGH if production quality high; LOW if walls of text or formatting issues

**Q3.3 — Is the content mass-produced by or outsourced to a large number of creators?**

Check:
- Voice consistency across the content (especially in long pieces)
- Author profile match (if loaded)
- Generic language patterns (signs of bulk content)
- Score: HIGH if single coherent voice; LOW if voice drift

**Q3.4 — Is the content excessively automated for many topics?**

Check:
- Compare against other content from the same site if available
- Pattern: many similar-structured articles on disparate topics suggests automated generation
- Score: HIGH if site demonstrates editorial focus; flag if automation suspected

### Theme 4 — Helpful + People-First

**Q4.1 — Does the content have an existing or intended audience that would find it useful if they came directly?**

Check:
- Audience clarity: is it obvious who this content is for?
- Brand profile + audience profile cross-check
- Score: HIGH if audience clear; LOW if generic

**Q4.2 — Does the content demonstrate first-hand expertise and a depth of knowledge?**

Check (delegated to eeat-enforcer): Experience + Expertise scores
- Score: composite of those

**Q4.3 — Does the content leave the reader feeling they've learned enough about the topic to help with their goal?**

Check:
- Does the content end with a complete answer, or does it require additional searches?
- Does it provide actionable next steps?
- Score: HIGH if reader's question is fully answered; LOW if loose ends

**Q4.4 — Does the content provide a satisfying experience to the reader?**

Check:
- Reading flow (sentence rhythm, paragraph length, visual breaks)
- Information density (substance per paragraph)
- Lack of frustration signals (no clickbait, no answers requiring scrolling past ads, no unrelated tangents)
- Score: HIGH if satisfying; LOW if frustrating

**Q4.5 — Are you keeping in mind Google's core updates and the helpful content system?**

Check:
- This is meta; functionally checked through Q1-Q4 above
- Score: composite of others

**Q4.6 — Is the content substantially better than other pages in search results?**

Check:
- Compare against top 3 SERP results (if provided)
- Differentiation factors: more depth, more specifics, better structure, better experience markers, original data, unique angle
- Score: HIGH if substantially better on ≥2 dimensions; MEDIUM if equivalent; LOW if worse

---

## Scoring

Each question 0-10. Themes weighted equally (25% each). Overall HCU score 0-100.

Passing thresholds:
- 85+ = STRONG (likely to perform well in HCU-affected niches)
- 70-84 = ADEQUATE (may pass in lower-competition niches)
- Below 70 = WEAK (high risk of HCU demotion)

For YMYL content, anything below 80 should block delivery.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: helpful-content-checker
Status: COMPLETE
Inputs consumed: content, target keyword, [optional inputs]

HELPFUL CONTENT SCORES:
  Theme 1 (Content & Quality): [X/100]
  Theme 2 (Expertise):         [X/100]
  Theme 3 (Presentation):      [X/100]
  Theme 4 (People-First):      [X/100]
  OVERALL HCU SCORE:           [X/100] — [STRONG / ADEQUATE / WEAK]

PER-QUESTION RESULTS (each scored 0-10):

THEME 1:
  Q1.1 Original information/reporting/analysis: [X/10] — [pass/fail rationale]
  Q1.2 Substantial, complete, comprehensive: [X/10] — [rationale]
  Q1.3 Insightful, beyond the obvious: [X/10] — [rationale]
  Q1.4 Adds value when borrowing from sources: [X/10] — [rationale]
  Q1.5 Headline accurate, not exaggerating: [X/10] — [rationale]
  Q1.6 Bookmark/share-worthy: [X/10] — [rationale]

THEME 2:
  Q2.1 Citation-worthy: [X/10] — [rationale]
  Q2.2 Demonstrably expert author: [X/10] — [rationale]
  Q2.3 Free of factual errors: [X/10] — [rationale]
  Q2.4 Trustworthy for YMYL: [X/10 or N/A] — [rationale]

THEME 3:
  Q3.1 Free of spelling/style issues: [X/10] — [rationale]
  Q3.2 Well-produced (not sloppy): [X/10] — [rationale]
  Q3.3 Single coherent voice (not mass-produced): [X/10] — [rationale]
  Q3.4 Editorially focused (not mass-automated): [X/10] — [rationale]

THEME 4:
  Q4.1 Clear intended audience: [X/10] — [rationale]
  Q4.2 First-hand expertise + depth: [X/10] — [rationale]
  Q4.3 Reader leaves with goal achieved: [X/10] — [rationale]
  Q4.4 Satisfying reader experience: [X/10] — [rationale]
  Q4.6 Substantially better than other pages: [X/10] — [rationale]

CRITICAL FAILURES (questions scored ≤4):
  - [Q]: [specific revision instruction]

HIGH-PRIORITY IMPROVEMENTS (5-7 scores):
  - [Q]: [specific revision instruction]

YMYL ALERT: [if YMYL topic and any T-related score is weak]

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Specific revision instructions per failure.** Not "improve helpfulness" — "rewrite Section 4 to include 2-3 sub-topics it currently skips: X, Y, Z (which are the top 3 PAA questions for this keyword)."
2. **YMYL strict mode.** Auto-detect YMYL topics from keywords (health, finance, legal, safety, etc.) and apply stricter thresholds.
3. **Composite checks delegate to other skills.** Q2.2 reads from eeat-enforcer; Q2.3 from live-fact-verifier. Don't duplicate work.
4. **Comparison-driven scoring needs SERP context.** Q4.6 (substantially better) only meaningful with top 3 results provided. Otherwise mark as MEDIUM with note.
5. **Conservative scoring.** When uncertain, score lower. The point is not to inflate scores but to give honest assessment.
