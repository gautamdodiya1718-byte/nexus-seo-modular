# inline-llm-checklist.skill

**Role:** Implements the 9-category content quality checklist from the Antigravity prompt as in-pipeline gates that run DURING generation (not as post-hoc review). Each category becomes a scored check with revision instructions on failure.

**Loaded by:** `nexus-content` SKILL.md in all generation modes

---

## Why this skill exists

The original Antigravity prompt's "LLM Content Review Checklist" runs AFTER generation as a separate pass. Problem: by then bad content is already written, and the same model that wrote it is now grading itself.

This skill turns the checklist into 9 in-pipeline checks that run during generation. Each check produces a deliverable block. Failed checks block progression to next stage.

---

## The 9 categories (from the original checklist)

### Category 1 — SEO Optimization

**Check 1.1 — Primary keyword integration**
- Keyword in title: YES/NO
- Keyword in first 100 words: YES/NO
- Keyword density: 1-2% target; flag stuffing (>3%) or under-use (<0.5%)
- Variations and synonyms naturally used: count
Pass: all four conditions met without stuffing.

**Check 1.2 — Meta elements**
- Title length: 50-60 chars
- Meta description: 140-160 chars (or 120-160 acceptable)
- Single H1: yes
- Logical heading hierarchy: yes (no skipped levels)
- Subheadings create clear content hierarchy: yes
Pass: all conditions met.

**Check 1.3 — Semantic SEO and topic coverage**
- Comprehensively covers main topic: yes/no (cross-check with serp-blueprint and PAA)
- Related subtopics addressed: count vs SERP requirements
- LSI/synonyms naturally included: count
- PAA-type questions addressed: count
Pass: comprehensive coverage achieved.

### Category 2 — Content Quality and Structure

**Check 2.1 — Readability and UX**
- Paragraphs ≤3 sentences: % of paragraphs that conform
- Sentence length: avg under 20 words; varied
- Active voice predominant: % active vs passive
Pass: ≥85% conformance per metric.

**Check 2.2 — Content organization**
- Intro states what readers will learn: yes/no
- Main points organized logically: yes/no (heading sequence makes sense)
- Clear conclusion with key takeaways: yes/no
- TOC included where helpful: yes/no
- Lists used effectively (not overused): yes/no
Pass: all yes.

**Check 2.3 — Technical accuracy and depth**
- All technical info accurate (delegated to live-fact-verifier): pass/fail
- Code examples correct (delegated to code-generation-preview): pass/fail
- Depth appropriate for target audience: pass/fail
- Complex processes broken into clear steps: pass/fail
- Prerequisites stated: pass/fail
Pass: all pass.

### Category 3 — E-E-A-T Signals (delegates to eeat-enforcer)

**Check 3.1 — Expertise demonstration**
- Subject matter knowledge depth: scored by eeat-enforcer

**Check 3.2 — Authority and trustworthiness**
- Sources, examples, claims with evidence: scored by eeat-enforcer

**Check 3.3 — Experience and first-hand knowledge**
- Personal experiences, case studies, practical tips: scored by eeat-enforcer

Pass: eeat-enforcer overall ≥70.

### Category 4 — User Intent and Value Delivery

**Check 4.1 — Search intent fulfillment**
- Content directly answers implied question: yes/no
- Most important info early: yes/no
- Different skill levels accommodated: yes/no
- Actionable next steps provided: yes/no
- User would feel satisfied: yes/no
Pass: ≥4 of 5 yes.

**Check 4.2 — Practical value and actionability**
- Readers can immediately apply: yes/no
- Specific examples and use cases provided: count
- Tools, resources, templates mentioned: count
- Solves real problems: yes/no
- Clear value beyond competitors: pass/fail (delegated to differentiation-enforcer)
Pass: actionable + clear value.

### Category 5 — Technical Content Specific

**Check 5.1 — Code quality and examples** (only if code present)
- Properly formatted: yes/no
- Includes context and setup: yes/no
- Multiple approaches when relevant: yes/no
- Error handling and edge cases addressed: yes/no
- Tested and functional (delegated to code-generation-preview): pass/fail
Pass: all yes/pass.

**Check 5.2 — Documentation standards** (only if documentation content)
- API endpoints documented: yes/no
- Installation instructions complete: yes/no
- Troubleshooting included: yes/no
- Version compatibility specified: yes/no
- Security considerations mentioned: yes/no
Pass: all relevant items present.

### Category 6 — Content Optimization Opportunities

**Check 6.1 — Internal linking**
- Links to related content: count
- Could support pillar pages or topic clusters: yes/no
- Natural places for contextual links: count
Pass: ≥minimum link count per brand.site_size; all links from brand.urls.

**Check 6.2 — Featured snippet optimization**
- Concise direct answers to common questions: count
- Step-by-step processes outlined: yes/no
- Definitions in snippet-friendly format: count
- Numbered lists for sequential processes: yes/no
- Formatting optimized for rich snippets: yes/no
Pass: ≥3 snippet-optimization opportunities captured.

### Category 7 — Competitive Differentiation (delegates to differentiation-enforcer)

**Check 7.1 — Unique value proposition**
- Different from competitors: pass/fail (delegated)
- Unique insights or perspectives: pass/fail
- Original research or data: pass/fail (boosted by user-research-injector)
- Innovative approaches or solutions: pass/fail
- Worth sharing or citing: pass/fail (delegated to ai-citation-optimizer)

Pass: differentiation-enforcer PASS.

### Category 8 — Mobile and Accessibility

**Check 8.1 — Mobile-friendly**
- Sentences and paragraphs optimized for mobile: yes/no
- Code examples display well on mobile: yes/no
- Images appropriately sized: yes/no
- Content scannable on small screens: yes/no
Pass: all yes.

### Category 9 — AI Content Quality Check

**Check 9.1 — Human value and authenticity**
- Genuine human insight (delegated to author-voice-applier + eeat-enforcer): pass/fail
- Personal anecdotes or observations (delegated to author profile): pass/fail
- Engaging and conversational style: pass/fail
- Avoids generic templated language: pass/fail
Pass: all pass via delegations.

---

## Process

### Step 1 — Run during generation, not after

This skill runs as in-pipeline gates between content-generation-engine output and humanizer. Each section/category's checks run as they become applicable.

### Step 2 — Score each check

Each check produces:
- Score: 0-10
- Pass/Fail (against threshold)
- Specific issues found
- Specific revision instructions

### Step 3 — Aggregate

Each category gets average score of its checks.
Overall content score = average of 9 categories.

### Step 4 — Block progression on critical failures

If any category scores below 5/10, content cannot progress to humanizer until fixed.
If overall score is below 70/100, content cannot reach confidence-gate.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: inline-llm-checklist
Status: COMPLETE
Inputs consumed: generated content [N words], brand profile, author profile

CATEGORY SCORES (each /10):

1. SEO Optimization:
   1.1 Primary keyword integration: [X/10]
   1.2 Meta elements optimization: [X/10]
   1.3 Semantic SEO + topic coverage: [X/10]
   Average: [X/10]

2. Content Quality and Structure:
   2.1 Readability and UX: [X/10]
   2.2 Content organization: [X/10]
   2.3 Technical accuracy and depth: [X/10]
   Average: [X/10]

3. E-E-A-T Signals (delegated to eeat-enforcer):
   3.1 Expertise: [X/10]
   3.2 Authority/trust: [X/10]
   3.3 Experience: [X/10]
   Average: [X/10]

4. User Intent and Value:
   4.1 Search intent fulfillment: [X/10]
   4.2 Practical value and actionability: [X/10]
   Average: [X/10]

5. Technical Content (when applicable):
   5.1 Code quality: [X/10 or N/A]
   5.2 Documentation standards: [X/10 or N/A]

6. Content Optimization:
   6.1 Internal linking: [X/10]
   6.2 Featured snippet optimization: [X/10]
   Average: [X/10]

7. Competitive Differentiation: [X/10]

8. Mobile and Accessibility: [X/10]

9. AI Content Quality: [X/10]

OVERALL CONTENT SCORE: [X/100]

CRITICAL ISSUES (categories scored ≤5):
  - [category]: [specific issues + specific fixes]

HIGH-PRIORITY IMPROVEMENTS (5-7 scores):
  - [category]: [specific improvements]

PROGRESSION DECISION: PASS / BLOCK [reason — which category critical]
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Runs during generation, not after.** Inline gates between generation steps, not post-hoc review.

2. **Specific revision instructions, not vague.** "Improve readability" is useless. "Section 4 has 6 paragraphs over 3 sentences each — split paragraph 2 sentence 3 → 4 → 5; combine paragraph 6 sentences 1+2" is actionable.

3. **Delegate where possible.** E-E-A-T to eeat-enforcer; differentiation to differentiation-enforcer; technical accuracy to live-fact-verifier; code to code-generation-preview. Don't duplicate.

4. **Honest scoring.** Don't inflate scores. Below-threshold scores trigger fixes; passing-by-inflation is the failure mode this skill prevents.

5. **Progression blocks are real.** A 4/10 in any category blocks pipeline progression until fixed.
