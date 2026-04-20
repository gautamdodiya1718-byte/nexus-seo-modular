# differentiation-enforcer.skill

**Role:** Pre-generation gate that enforces ≥3 unique angles, claims, data points, or perspectives in the content outline/blueprint before generation begins. Stops Nexus from producing well-optimized but structurally identical content to what already ranks.

**Loaded by:** `nexus-research` SKILL.md in modes: competitor_gap, content_outline, semantic_seo, full_research_package
**Companion to:** `differentiation-checker.skill.md` (in nexus-audit) — that one audits existing content; this one enforces in research/planning phase

---

## Why this skill exists

Without enforced differentiation at the research phase, generated content will optimize itself toward the SERP average. That produces "slightly better than the top 10" content, which becomes #11. Genuine ranking displacement requires structurally non-overlapping value baked into the blueprint.

This skill blocks blueprints that don't clear the differentiation threshold and requires research enrichment before generation can proceed.

---

## Inputs

Required:
- Content blueprint / outline (in progress, from serp-blueprint-generator)
- Top 10 SERP results for the target keyword (from deep-serp-analysis)
- Target keyword + intent

Optional:
- Brand profile (for brand-specific differentiation angles — proprietary data, unique positioning)
- Author profile (for author-specific opinion stances and experience markers)
- User research (for user-supplied original data, interviews, benchmarks)

---

## Process

### Step 1 — Build covered angle inventory from top 10

For each of the top 10 SERP results (using deep-serp-analysis output):
- Extract H2/H3 topics covered
- Extract claim types (definitions, comparisons, statistics, recommendations, warnings, examples)
- Extract specific data points (numbers, dates, version numbers, prices)
- Extract perspectives/angles (POV taken)
- Extract use cases mentioned
- Extract tools/products referenced

Build a structured inventory: "Top 10 collectively cover [topics A-Z], make claims [P-W], cite data points [X1-Xn], take angles [α-ω], use cases [U1-Un], mention tools [T1-Tn]."

### Step 2 — Build outline angle inventory

Same extraction process for the proposed content outline/blueprint.

### Step 3 — Compute differentiation

For each item in the outline:
- **DUPLICATE** — same angle/claim/data is present in top 10
- **VARIATION** — similar but not identical (different phrasing, slight angle shift)
- **UNIQUE** — genuinely absent from all top 10

Differentiation = count of UNIQUE items in the outline.

### Step 4 — Categorize unique items

Sort unique items into:
- **Unique data points** — specific numbers/statistics not in top 10 (best from user research or brand proprietary data)
- **Unique angles / perspectives** — POV no top-ranker takes
- **Unique use cases** — use cases absent from all competitor pages
- **Unique tools/methods** — references competitors don't make
- **Contrarian positions** — disagrees with top 10 consensus, with stated reasoning
- **Original examples** — specific scenarios not in any competitor
- **Unique sub-topics** — H3-level topics absent from all competitors

### Step 5 — Apply differentiation thresholds

| Content type | Minimum unique items required |
|---|---|
| TOFU informational | ≥3 unique items, at least 1 unique data point or original example |
| MOFU comparison/evaluation | ≥4 unique items, at least 1 contrarian position or unique angle |
| BOFU transactional | ≥5 unique items, at least 2 unique data points or original examples |
| Pillar / authority | ≥7 unique items across all categories |

### Step 6 — Decision

**If outline meets threshold:** PASS. Outline ships to generation.

**If outline fails threshold:** BLOCK. Generation cannot proceed. Output:
- Specific count of unique items vs required
- List of differentiation gaps
- 5+ specific suggestions for unique angles to add (sourced from gap-opportunity-engine output, brand profile if loaded, user research if provided, author opinion stances if profile loaded)
- Path forward: add suggested angles to outline → re-run differentiation-enforcer → on PASS, proceed to generation

### Step 7 — Identify burial risk

Even if outline passes threshold, check that unique items are positioned prominently:
- Unique items should appear in intro, main H2 sections, or prominent callouts
- Flag if unique items are buried in conclusion, footnotes, or "additional reading" sections
- Recommend restructuring to surface differentiation prominently

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: differentiation-enforcer
Status: COMPLETE
Inputs consumed: outline, top [N] SERP results, [optional inputs]

DIFFERENTIATION COUNT: [N unique items]
THRESHOLD REQUIRED: [N for content type X]
DECISION: PASS / BLOCK

OUTLINE INVENTORY:
  Total angle/claim/data items: [N]
  DUPLICATE (in top 10): [N]
  VARIATION (similar): [N]
  UNIQUE (absent from top 10): [N]

UNIQUE ITEMS IN OUTLINE:
  Unique data points: [N]
    1. [item]
  Unique angles: [N]
    1. [angle]
  Unique use cases: [N]
    1. [use case]
  Contrarian positions: [N]
    1. [position + reasoning]
  Original examples: [N]
    1. [example]
  Unique sub-topics: [N]
    1. [sub-topic]

(IF BLOCK)
DIFFERENTIATION GAPS:
  Required: [N more unique items needed]
  Gap categories: [which categories are short]

SUGGESTED UNIQUE ANGLES TO ADD (sourced from gap analysis + brand/author/user data):
  1. [specific suggestion + which source it comes from + why it's unique vs top 10]
  2. ...
  3. ...
  4. ...
  5. ...

NEXT STEP:
  Add ≥[N] of the suggested angles to outline. Re-run differentiation-enforcer to verify PASS before proceeding to generation.

(IF PASS)
BURIAL RISK CHECK:
  Unique items in intro / prominent positions: [N of N — pass]
  Unique items buried in low-prominence sections: [list with recommended repositioning]

CONFIDENCE: HIGH (top 10 SERP fully analyzed) / MEDIUM (top 5 only) / LOW (top 3 or less)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Hard block, not warning.** If outline fails threshold, generation does NOT proceed. The user must enrich the outline.

2. **Specific suggestions sourced from real data.** When recommending unique angles, base them on:
   - gap-opportunity-engine output (real gaps in the SERP)
   - brand profile differentiators (real proprietary positioning)
   - author opinion stances (real author POV)
   - user research (real user-provided data)
   - NEVER fabricate "this would be unique" suggestions without grounding

3. **Variations don't count.** Slight rephrasing of a top-10 point is not a unique angle. Only items genuinely absent from all 10 results.

4. **Threshold by content type.** Calibrated. A 800-word TOFU page can't reasonably have 7 unique items. A pillar page should.

5. **Burial check matters.** Differentiation that exists but is buried in footnotes doesn't help ranking or AI citation. Always check positioning.

6. **Loops cleanly.** When BLOCKED, output gives clear path forward (add angles, re-run). Not "rewrite from scratch."
