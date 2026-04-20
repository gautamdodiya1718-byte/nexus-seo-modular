# comparison-post-mode.skill

**Role:** Override default content generation rules for X-vs-Y comparison content. Mandatory comparison table with criteria, balanced treatment of all options, criteria-based scoring, recommendation framework, and strict differentiation enforcement.

**Loaded by:** `nexus-content` SKILL.md when user invokes comparison_post mode

---

## Why this skill exists

Comparison content has unique requirements:
- Reader expects a structured comparison table they can scan
- Bias accusations are easy if treatment is unbalanced
- Criteria need to be explicit (what are we comparing on?)
- Recommendation framework needs to acknowledge "it depends"
- Differentiation is harder (most "X vs Y" content already exists)

This skill enforces comparison-specific structure and bias controls.

---

## Inputs

Required:
- Items being compared (typically 2, can be 3-5)
- Comparison context (what is the user trying to decide?)
- Brand profile (if your brand is one of the items being compared, special bias-management rules apply)

Optional:
- User's stated preference (if user is partial to one option, can shape recommendation while maintaining honest comparison)
- Specific criteria to focus on
- Use cases to evaluate against

---

## The mandatory comparison structure

### Required sections (in order)

1. **TL;DR / Quick recommendation**
   - One paragraph: which option for which use case
   - Comparison table summary (top 3-5 criteria)

2. **Comparison criteria explained**
   - What we're comparing on and why
   - Why these criteria matter for the reader's decision

3. **Side-by-side comparison table** (mandatory)
   - Rows: criteria
   - Columns: each option
   - Cells: specific evaluation per criterion (not vague)
   - Visual scoring (e.g., ✓/✗ or 1-5 stars) where applicable

4. **Detailed evaluation per option**
   - One H2 per option being compared
   - Each H2 follows the SAME structure for fairness:
     - Strengths
     - Weaknesses
     - Best for (use cases)
     - Pricing/cost (if applicable)
     - Ecosystem/community (if applicable)

5. **Use case recommendations**
   - "Choose X if..." with specific scenarios
   - "Choose Y if..." with specific scenarios
   - "Consider neither if..." (sometimes neither option fits — be honest)

6. **Decision framework**
   - 3-5 questions reader should ask themselves to decide
   - Each question maps to which option fits

7. **FAQ section**
   - Common questions about the comparison
   - Including "what about [related option]?" if relevant

---

## The bias-management rules

### When your brand IS one of the options

If `brand` profile indicates one of the compared items is your brand:
- Disclosure required at the top: "Note: We make [Brand]. We've tried to evaluate fairly — see methodology below."
- Methodology section required: how we evaluated each option
- Each evaluation must be balanced — your brand's weaknesses are listed alongside competitor weaknesses
- Scoring must be defensible — every score has supporting evidence
- Recommendation can favor your brand IF supported by stated criteria — but must explicitly acknowledge cases where competitor wins

### When your brand is NOT one of the options

Standard balanced evaluation. No special bias rules.

### Evidence requirements (extra strict for comparison)

Every comparative claim needs evidence:
- "X is faster than Y" → benchmark source linked
- "X has more features than Y" → feature comparison source linked
- "X is more popular than Y" → adoption data source linked

Live-fact-verifier runs in extra-strict mode for comparison content.

---

## Process

### Step 1 — Define comparison criteria

Determine which criteria to compare on:
- If user provided criteria: use those
- If brand profile suggests criteria: use them
- If neither: derive from typical buyer questions for the product category (use web_search to find common comparison criteria)

Recommended criteria types:
- Functional (features, capabilities)
- Performance (speed, scale)
- Cost (pricing, total cost of ownership)
- Usability (learning curve, documentation)
- Ecosystem (integrations, community, support)
- Specific use case fit

### Step 2 — Generate side-by-side table

For each criterion × each option, generate:
- Specific evaluation (not vague)
- Source/citation for the evaluation
- Visual indicator (✓ / ✗ / partial / N/A) if binary
- Score (1-5) if scaled

Verify each cell content with live-fact-verifier (especially for capability claims).

### Step 3 — Generate per-option H2 sections

For each option, generate H2 section with parallel structure:
- Strengths (3-5, evidence-backed)
- Weaknesses (3-5, evidence-backed)
- Best for (specific use cases)
- Pricing/cost (current — flagged for decay-mapping as time-sensitive)
- Ecosystem (community, integrations, support)

Same template for every option ensures fairness.

### Step 4 — Generate use case recommendations

For each option, identify scenarios where that option is the best fit. Be specific:
- Not: "Choose X if you value performance"
- Yes: "Choose X if you have >100 concurrent users and need <100ms response times"

Acknowledge cases where neither option fits if true.

### Step 5 — Generate decision framework

3-5 questions readers ask themselves. Each question maps to which option:
- "Do you need X capability?" → if yes, [option A]; if no, doesn't matter
- "What's your budget?" → various tiers map to options
- "Do you have technical resources to maintain X?" → maps to options

### Step 6 — Differentiation strict mode

For comparison posts, differentiation is hardest because most X-vs-Y content already exists. Apply EXTRA STRICT differentiation:
- Need ≥5 unique angles vs top 10 (instead of ≥4 standard for MOFU)
- At least 2 unique angles must be original data (your benchmarks, your test methodology) or contrarian positions
- Surface unique angles in TL;DR, not buried

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: comparison-post-mode
Status: COMPLETE
Inputs consumed: items being compared, context, [optional inputs]

COMPARISON STRUCTURE GENERATED:

  Items compared: [list]
  Comparison criteria: [N criteria — list]
  Brand-disclosure required: YES (your brand is one of the options) / NO

  ✓ TL;DR with quick recommendation
  ✓ Comparison criteria explained
  ✓ Side-by-side comparison table (criteria × options grid)
  ✓ Detailed evaluation per option (parallel H2 structure)
  ✓ Use case recommendations
  ✓ Decision framework (N questions)
  ✓ FAQ section

BIAS-MANAGEMENT (if your brand in comparison):
  ✓ Disclosure included at top
  ✓ Methodology section included
  ✓ Each option evaluated with same structure
  ✓ Brand's weaknesses listed alongside competitor weaknesses
  ✓ Recommendation defensible from stated criteria

EVIDENCE REQUIREMENTS:
  Comparative claims with linked sources: [N of M — %]
  Capability claims live-fact-verified: [N of M — %]
  All performance claims sourced: PASS / FAIL list

DIFFERENTIATION (extra strict for comparison):
  Required: ≥5 unique angles + ≥2 original data / contrarian positions
  Achieved: [N unique angles, N original data points]
  Differentiation enforcer: PASS / FAIL

SCHEMA GENERATED:
  ✓ BlogPosting (standard)
  ✓ Review (if reviewing single primary item)
  ✓ Product (if appropriate)
  ✓ FAQPage (FAQ section)

QUALITY CHECKS:
  ✓ Substance gates
  ✓ Live fact verification (extra strict for comparison)
  ✓ Live link verification
  ✓ E-E-A-T (author credibility for the comparison)
  ✓ Helpful Content (especially "substantially better than other pages" check)

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Comparison table is mandatory.** Every comparison post has a structured side-by-side table. No exceptions.

2. **Parallel H2 structure for fairness.** Same template for every option's deep-dive section.

3. **Disclosure when your brand is in the mix.** Hide-the-bias = lose-credibility. Be upfront.

4. **Evidence per claim is non-negotiable.** "X is faster" requires source. Every. Time.

5. **Specific recommendations.** "Choose X if you value performance" is useless. Specific scenarios with specific thresholds.

6. **Decision framework is the user-value.** Reader came to make a decision. The framework helps them decide based on their context.

7. **Extra-strict differentiation.** Comparison posts compete with many existing comparisons. Need 5+ unique angles to stand out.
