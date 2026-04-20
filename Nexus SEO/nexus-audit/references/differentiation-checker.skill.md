# differentiation-checker.skill

**Role:** Verify that content has at least N angles, claims, data points, or perspectives that don't appear in any of the top 10 ranking pages for the target keyword. Prevents Nexus from producing well-optimized content that's structurally identical to what already ranks.

**Loaded by:** `nexus-audit` SKILL.md in modes: full_audit, competitor_compare
**Also loaded by:** `nexus-content` SKILL.md as a pre-generation gate (extends gap-opportunity-engine)

---

## Why this skill exists

Without enforced differentiation, AI-generated content optimizes itself into a slightly-better version of what already ranks. That's not enough to displace top-rankers; it just becomes #11. To rank, content needs at least some genuinely non-overlapping value.

This skill enforces that minimum at the structural level.

---

## Inputs

Required:
- Content to evaluate (URL, file, or pasted)
- Target keyword

Required for scoring (if absent, skill outputs lower confidence):
- Top 3-10 SERP results for the keyword (URLs + main content of each)

Optional:
- Brand profile (for brand-specific differentiation angles)

---

## Detection logic

### Step 1 — Build "covered angle inventory" from top SERP results

For each top SERP result (top 3 minimum, top 10 ideal):
- Extract H2/H3 headings (the topics they cover)
- Extract claim types (definitions, comparisons, statistics, recommendations, warnings, examples)
- Extract data points (specific numbers, dates, version numbers, prices)
- Extract perspectives/angles (whose POV the content takes — buyer, builder, comparing, evaluating)
- Extract use cases mentioned
- Extract tools/products mentioned

Build a structured inventory: "Top 10 results collectively cover topics A, B, C... claims P, Q, R... data points X, Y, Z..."

### Step 2 — Build "your content's angle inventory"

Same extraction process for the content being audited.

### Step 3 — Compute differentiation

For each item in your content's inventory, classify:
- **DUPLICATE** — same angle/claim/data appears in top 10
- **VARIATION** — similar but not identical (different phrasing, slight angle shift)
- **UNIQUE** — genuinely absent from all top 10

Differentiation score = (UNIQUE items) / (total items in your content's inventory)

### Step 4 — Categorize unique items by type

Sort unique items into:
- **Unique data point** — a specific number/statistic not in top 10
- **Unique angle / perspective** — a POV no top-ranker takes
- **Unique use case** — a use case absent from all competitor pages
- **Unique tool/method mentioned** — something competitors don't reference
- **Contrarian position** — disagrees with the top 10 consensus, with stated reasoning
- **Original example** — specific scenario not in any competitor
- **Unique sub-topic** — H3-level topic absent from all competitors

### Step 5 — Apply differentiation thresholds

**Minimum thresholds for content to clear differentiation:**

| Content type | Minimum unique items required |
|---|---|
| TOFU informational | ≥3 unique items, at least 1 unique data point or original example |
| MOFU comparison/evaluation | ≥4 unique items, at least 1 contrarian position or unique angle |
| BOFU transactional | ≥5 unique items, at least 2 unique data points or original examples |
| Pillar / authority | ≥7 unique items across all categories |

If content fails threshold:
- In **nexus-audit** (review existing content): output specific list of missing differentiation, suggest 3-5 unique angles to add
- In **nexus-content** (generating new content): block the blueprint from proceeding to generation; require differentiation enrichment in research phase

### Step 6 — Identify "covered gaps" (angles you have but aren't currently making prominent)

Sometimes content has differentiation but buries it. Check:
- Are unique items in the conclusion or footnotes when they should be in the intro or main sections?
- Are unique angles only mentioned in passing when they could anchor an H2?
- Recommend restructuring to surface differentiation prominently

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: differentiation-checker
Status: COMPLETE / PARTIAL ([reason — e.g., "no SERP context provided"])
Inputs consumed: content + top [N] SERP results

DIFFERENTIATION SCORE: [X/100]

CONTENT TYPE DETECTED: [TOFU / MOFU / BOFU / pillar]
THRESHOLD REQUIRED: [N unique items minimum, with [specific category requirements]]
THRESHOLD STATUS: [PASS / FAIL]

YOUR CONTENT'S INVENTORY:
  Total angle/claim/data items: [N]
  DUPLICATE (in top 10): [N]
  VARIATION (similar): [N]
  UNIQUE (absent from top 10): [N]

UNIQUE ITEMS YOU HAVE:
  Unique data points: [N]
    1. [item — specific value/stat]
    2. ...
  Unique angles: [N]
    1. [angle description]
    2. ...
  Unique use cases: [N]
    1. ...
  Contrarian positions: [N]
    1. [position + reasoning]
    2. ...
  Original examples: [N]
    1. ...

ANGLES TOP-RANKING COMPETITORS HAVE THAT YOU DON'T:
  [These are not necessarily required, but listed so user knows what's covered]
  - [angle from competitor]: covered by [URL], your content [doesn't cover / covers in passing]

BURIED DIFFERENTIATION (you have it but it's not prominent):
  - [unique item] currently in [section] — recommend moving to [more prominent section]

REMEDIATION (if FAIL):
  Required: add [N] more unique items to clear threshold.
  Suggested differentiation angles based on gap analysis:
    1. [specific angle suggestion + why this is unique]
    2. ...
    3. ...
    4. ...
    5. ...

CONFIDENCE: HIGH (top 10 SERP analyzed) / MEDIUM (top 3 only) / LOW (no SERP context)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **SERP context dramatically improves accuracy.** Without it, can only identify "your content's content" but not what's truly unique. Always recommend providing SERP results.

2. **Variations don't count as unique.** Slight rephrasing of the same point is not differentiation. Only genuinely absent items count.

3. **Quality over quantity.** 3 substantial unique angles is better than 10 trivial differentiations. Score weights deeper items higher.

4. **Don't suggest fabricated differentiation.** When recommending unique angles to add, base them on real gaps from gap-opportunity-engine (in nexus-research) or genuine contrarian positions, not made-up angles.

5. **Threshold by content type.** A 800-word TOFU definition page can't reasonably have 7 unique items. A pillar page should. Calibrate.
