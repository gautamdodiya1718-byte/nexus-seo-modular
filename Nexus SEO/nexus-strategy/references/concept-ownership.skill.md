# concept-ownership.skill

**Role:** Identify which specific concept a brand should own (be the definitive authority on) based on content pillars, competitive gaps, and brand differentiators. Concept ownership is the strategic alternative to keyword competition.

**Loaded by:** `nexus-strategy` SKILL.md in modes: full_strategy, concept_ownership

---

## Why this skill exists

Most SEO strategy chases existing high-volume keywords against established competitors. That's expensive (DA wars) and crowded (everyone optimizes for the same terms).

Concept ownership is the alternative: identify a specific concept the brand can plausibly own — be THE definitive authority — and build the entire content strategy around that ownership. Brands that own a concept (Salesforce owns "CRM", HubSpot owns "inbound marketing", Notion owns "all-in-one workspace") get cited, recommended, and searched-for as a category.

This skill helps identify which concept this brand could realistically own.

---

## Inputs

Required:
- Brand profile (industry, what we do, differentiators, content pillars, competitors)
- Existing content inventory (if available — what the brand has already published)

Optional:
- Strategic ambition level (how far is the brand willing to invest?)
- Time horizon (12 months / 24 months / 5 years)

---

## Process

### Step 1 — Candidate concept identification

Generate concept candidates from multiple angles:

**Angle 1 — Content pillar synthesis**
- What is the underlying concept that ties brand.content_pillars together?
- Is there a unifying frame that makes the pillars coherent?
- Example: TestDino's pillars (Playwright, test automation, CI/CD, AI testing) might tie into "intelligent test automation" as a unifying concept

**Angle 2 — Differentiator amplification**
- What does the brand do differently than competitors?
- Could that difference become a named concept?
- Example: a brand that "uses AI to analyze test failures" could own "AI test failure analysis" as a concept

**Angle 3 — Underserved space**
- What concept is genuinely underserved (high search interest, low competitive coverage)?
- These are concept-ownership opportunities competitors haven't claimed

**Angle 4 — Industry shift positioning**
- What new concept is emerging that the brand can be early on?
- Examples: "AI-native testing" (early in 2024), "shift-right testing" (re-emerging concept)

**Angle 5 — Inversion of competitor positioning**
- What position do competitors take that the brand could counter?
- Example: if all competitors say "comprehensive testing platform", brand could own "focused test reporting"

For each angle, generate 2-3 concept candidates. Total: 10-15 candidates.

### Step 2 — Score each concept candidate

For each candidate, score on these dimensions:

**Dimension 1 — Concept volume (search interest)**
- Estimated monthly search volume for the concept name + variations
- Trend (growing / flat / declining)
- HIGH / MEDIUM / LOW

**Dimension 2 — Competitive coverage**
- Who else claims to own this concept?
- How established are they?
- HIGH (multiple strong claims) / MEDIUM (1-2 weak claims) / LOW (no clear ownership claim)

**Dimension 3 — Brand fit**
- Does the brand have the substance to own this concept?
- Specifically: existing content depth, product alignment, team expertise
- HIGH (clear fit) / MEDIUM (partial fit) / LOW (would require pivot)

**Dimension 4 — Time-to-ownership**
- Realistic timeline to be considered THE authority
- < 12 months (fast claim possible) / 12-24 months (medium) / 24+ months (long play)

**Dimension 5 — Strategic value if won**
- If the brand owned this concept, what's the impact?
- Pricing power, category creation, defensibility against new entrants
- HIGH / MEDIUM / LOW

**Concept-ownership score = weighted combination:**
- Concept volume: 20%
- Competitive coverage (lower is better): 25%
- Brand fit: 30%
- Time-to-ownership (shorter is better): 15%
- Strategic value: 10%

Top 3 candidates by score = recommended concepts.

### Step 3 — Detailed analysis for top recommendation

For the #1 recommended concept, provide:

**Concept definition:**
- Clear, citation-friendly definition (3-5 sentences)
- Why this concept matters in the market
- Who currently fills this conceptual space (or notes that it's vacant)

**Brand's claim to ownership:**
- Specific differentiators that support owning this concept
- Specific existing assets (content, products, team) that support the claim
- Gaps to fill before claim is credible

**Execution plan:**
- 12-month roadmap to establish ownership:
  - Month 1-3: Foundation content (definitive pillar pages, glossary, methodology docs)
  - Month 4-6: Authority signals (PR, expert content, original research)
  - Month 7-12: Distribution (conferences, podcasts, partnerships, AI surface presence)

**Defensibility:**
- How to maintain ownership once claimed
- Risks (competitor moves, market shifts)
- Defensive moats (proprietary methodology, original research, community)

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: concept-ownership
Status: COMPLETE
Inputs consumed: brand profile, content inventory, [optional inputs]

CONCEPT CANDIDATES GENERATED: [N total]
  From content pillar synthesis: [N]
  From differentiator amplification: [N]
  From underserved space analysis: [N]
  From industry shift positioning: [N]
  From competitor inversion: [N]

TOP 3 CANDIDATES BY SCORE:

1. CONCEPT: "[concept name]"
   Score: [X/100]
   Volume: [HIGH/MEDIUM/LOW] | Competitive coverage: [HIGH/MEDIUM/LOW] | Brand fit: [HIGH/MEDIUM/LOW]
   Time-to-ownership: [estimate] | Strategic value: [HIGH/MEDIUM/LOW]

2. CONCEPT: "[concept name]"
   Score: [X/100]
   ...

3. CONCEPT: "[concept name]"
   Score: [X/100]
   ...

DETAILED ANALYSIS — RECOMMENDED #1 CONCEPT:

CONCEPT: "[name]"

DEFINITION (citation-friendly):
  [3-5 sentence definition]

WHY THIS CONCEPT MATTERS:
  [market context, why it's valuable to own]

CURRENT OWNERSHIP STATE:
  [vacant / partially claimed by X / contested by X+Y]

BRAND'S CLAIM TO OWNERSHIP:
  Differentiators supporting claim: [list]
  Existing assets supporting claim: [list]
  Gaps to fill before claim is credible: [list]

12-MONTH EXECUTION PLAN:

  Months 1-3 (Foundation):
    - [Specific deliverable: pillar page on [concept name]]
    - [Specific deliverable: glossary / methodology doc]
    - [Specific deliverable: original research or proprietary data]

  Months 4-6 (Authority):
    - [Specific deliverable: PR / expert quotes / coverage]
    - [Specific deliverable: thought leadership content with author bylines]
    - [Specific deliverable: cite-worthy reference content]

  Months 7-12 (Distribution):
    - [Specific deliverable: conference speaking / podcast appearances]
    - [Specific deliverable: AI surface presence (per ai-search-positioning)]
    - [Specific deliverable: community / partnership building]

DEFENSIBILITY:
  How to maintain ownership: [strategy]
  Risks: [competitor X may also claim; market may shift to Y]
  Moats: [proprietary methodology / original research / community / etc.]

ALTERNATIVES CONSIDERED:
  [why concepts #2 and #3 weren't recommended despite scoring well]

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Concept ownership > keyword competition.** Frame strategy around owning a concept, not chasing keywords.

2. **Concept must be defendable.** A concept that anyone could claim isn't ownable. Look for concepts where the brand has unique substance.

3. **Realistic time horizons.** Concept ownership takes 12+ months minimum. Don't promise 3-month ownership.

4. **Competitive coverage matters most.** Don't recommend a concept that's already owned by an established player unless brand has a genuine advantage.

5. **Specific execution plans.** Not "build authority" — "publish a 5,000-word definitive pillar page on [concept] in Month 1, sourced from internal benchmark data."

6. **Defensibility is the long game.** Concept ownership only matters if it's defensible. Surface the moats explicitly.
