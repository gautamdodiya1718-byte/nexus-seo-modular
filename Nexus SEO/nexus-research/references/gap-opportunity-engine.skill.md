# SKILL: analysis/gap-opportunity-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Analysis / Content Gap Intelligence
# EXECUTION PRIORITY: POST-AWARENESS AND POST-CLUSTERING — Runs after content-awareness.skill, semantic-clustering.skill, and serp-intelligence.skill.
# POSITION: Content gap intelligence engine. Converts the difference between what exists and what should exist into specific, actionable content ideas.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **content gap intelligence engine** of the Nexus system.

It does not generate content ideas arbitrarily. It does not produce lists of topics that could theoretically be useful. It performs a structured three-layer comparison — between what the site already has, what the SERP is currently serving, and what the keyword universe defines as the full scope of opportunity — and generates content ideas only where a real, confirmed, specific gap exists in that comparison.

The output of this skill is not a brainstorm. It is a **gap-driven content brief list** — every idea is anchored to a specific gap type, tied to a real cluster, matched to a target keyword, and justified with a strategic reason that references the exact data behind it. The roadmap engine consumes this output directly, which means every idea must be production-ready: specific enough to be briefed to a writer, strategic enough to justify resource allocation.

A generic gap analysis produces generic content ideas. Generic content ideas produce content that Google ignores. This skill is designed from the ground up to prevent exactly that outcome — by enforcing a deduplication gate, requiring gap type classification for every idea, mandating a strategic reason for every recommendation, and running every generated idea through `deduplication-engine.skill` before it is added to the output.

### Primary Responsibilities

| Responsibility                         | Description                                                                                                          |
|----------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Three-Layer Comparison**             | Compare the site's existing content against the SERP landscape and the full keyword universe simultaneously          |
| **Gap Detection**                      | Identify keywords, intents, formats, depths, and topic areas that exist in the opportunity set but not in the site's content |
| **Gap Type Classification**            | Classify every detected gap into one of six defined gap types — no undefined or vague gap categories                 |
| **Deduplication Gate**                 | Call `deduplication-engine.skill` before finalizing any content idea — blocking covered topics, planned topics, and semantic duplicates |
| **Specific Idea Generation**           | For every confirmed gap, generate a specific, unique, actionable content idea — never a generic topic category       |
| **Priority Assignment**                | Score every gap-based idea by urgency — BOFU + high gap + critical cluster = HIGH priority                          |
| **Strategic Reason Production**        | For every idea, produce a reason statement tied to specific data from the three-layer comparison                     |
| **SERP Gap Simulation**                | When live SERP data is partial, simulate SERP gap likelihood from keyword patterns and cluster analysis              |
| **Cluster Gap Mapping**                | Map every detected gap to the cluster it belongs in — no gap may float without cluster context                       |
| **Emerging Topic Detection**           | Identify topics that are growing in the keyword universe but not yet well-covered by any SERP result                 |
| **Memory Cross-Check**                 | Verify that generated ideas have not been proposed or executed in prior sessions                                      |

### What This Skill Is NOT

- It is **not** a brainstorming tool. Every output must be grounded in real gap evidence — not creativity.
- It is **not** a keyword researcher. It works with the keyword universe produced by `keyword-discovery.skill` — it does not expand keywords.
- It is **not** a content generator. It produces content ideas (specific briefs) — not written content.
- It is **not** a SERP analyzer. It consumes SERP intelligence — it does not query search engines.
- It is **not** a generic content calendar filler. Every idea must solve a specific, confirmed gap or it is excluded.

### The Defining Standard of This Skill

> A content idea produced by this skill must satisfy all three of the following conditions simultaneously:
>
> 1. A real gap exists — the site has no adequate content targeting this keyword/topic.
> 2. An opportunity exists — the SERP is weak, the competition is beatable, or the topic is emerging.
> 3. A specific angle exists — the idea is specific enough to be briefed directly without further research.
>
> Any idea that cannot satisfy all three conditions is not included in the output.
> It is either rejected silently (if it is a duplicate) or queued for validation (if it is uncertain).
>
> Generic topics, vague directions, and "write more about X" recommendations are prohibited outputs.
> Every idea in this skill's output is a finished brief seed — not a starting point for more thinking.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                           | Required |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------|----------|
| `content-awareness.skill`             | Coverage map per cluster (covered/partially covered/missing topics), overlap alerts, freshness flags, funnel gap data | YES |
| `semantic-clustering.skill`           | Full keyword universe organized by cluster — all keywords with intent, funnel, category, and strategic reason | YES |
| `serp-intelligence.skill`             | SERP opportunity signal per keyword, content type analysis, SERP feature data, difficulty rating, content direction intelligence, PAA questions | YES |
| `content-inventory.skill`             | Complete published, draft, and planned content records with keyword assignments and coverage scores        | YES |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                    | Used For                                                            |
|---------------------------------------|------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| `competitor-analysis.skill`           | Gap matrix, blind spots, competitor weaknesses, contested vs. open topics          | Enriching Gap Level scoring and emerging topic detection            |
| `deep-serp-analysis.skill`            | Zero-presence and low-presence opportunities, differentiation angles               | Confirming specific gap angles for idea generation                  |
| `entity-extraction.skill`             | Entity gaps per cluster                                                             | Identifying missing entity coverage as a specific gap type         |
| `authority-engine.skill`              | Authority scores and missing element lists per cluster                              | Prioritizing gaps in low-authority clusters                        |
| `opportunity-engine.skill`            | Scored keyword priority list                                                        | Cross-referencing gap ideas with keyword opportunity scores        |

### Per-Gap Data Requirements

For every gap that reaches the idea generation phase:

| Field Required               | Source                                 | If Missing                                              |
|------------------------------|----------------------------------------|---------------------------------------------------------|
| Target keyword               | keyword universe (semantic-clustering) | Infer from topic; flag as INFERRED                      |
| Intent / Funnel              | keyword universe / serp-intelligence   | Derive from keyword pattern; flag as INFERRED           |
| Cluster name                 | semantic-clustering                    | Required — gap with no cluster cannot be scored correctly |
| Gap type                     | three-layer comparison (Step 1–3)      | Classify manually; flag as LOW CONFIDENCE if uncertain  |
| Gap confirmation signal      | content-awareness + serp-intelligence  | Mark as NEEDS VALIDATION if no confirmation signal     |

### Input Pre-Conditions

1. **Is the content awareness report available?** If not → cannot confirm what is covered vs. missing → all ideas would be unvalidated → halt and request `content-awareness.skill` output.
2. **Is the full keyword universe available?** If not → gaps cannot be mapped to keywords → downgrade confidence to LOW → flag entire output as PARTIAL.
3. **Is SERP intelligence available?** If not → SERP gap dimension cannot be assessed directly → apply SERP gap simulation (Advanced Logic) → flag all D2 signals as INFERRED.
4. **Has this skill been run before for this domain?** → Query `memory-controller.skill`. If a gap analysis exists within freshness threshold (14 days) → offer delta mode (only new gaps since last run). If stale → full rebuild.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Gap Opportunity Intelligence Report

```
NEXUS GAP OPPORTUNITY INTELLIGENCE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain / Scope          : [domain]
Gaps Detected           : [count — before deduplication]
Gaps After Deduplication: [count — final actionable ideas]
  HIGH Priority         : [count]
  MEDIUM Priority       : [count]
  LOW Priority          : [count]
  Needs Validation      : [count]
Gap Types Breakdown:
  Missing Topic         : [count]
  Missing Intent        : [count]
  Missing Depth         : [count]
  Missing Format        : [count]
  Missing Freshness     : [count]
  Emerging Topic        : [count]
SERP Data Status        : [FULL / PARTIAL — N/A fields simulated]
Generated By            : analysis/gap-opportunity-engine.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — GAP OPPORTUNITY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

(Sorted by Priority descending, then BOFU > MOFU > TOFU)

| # | Content Idea                                            | Target Keyword                      | Intent | Cluster          | Gap Type          | Priority | Flags         |
|---|---------------------------------------------------------|-------------------------------------|--------|------------------|-------------------|----------|---------------|
| 1 | [specific actionable content idea]                      | [primary keyword]                   | BOFU   | [cluster name]   | Missing Topic     | HIGH     |               |
| 2 | [specific actionable content idea]                      | [primary keyword]                   | MOFU   | [cluster name]   | Missing Intent    | HIGH     | INFERRED:KW   |
| 3 | [specific actionable content idea]                      | [primary keyword]                   | BOFU   | [cluster name]   | Missing Depth     | HIGH     |               |
| 4 | [specific actionable content idea]                      | [primary keyword]                   | MOFU   | [cluster name]   | Missing Format    | MEDIUM   |               |
| 5 | [specific actionable content idea]                      | [primary keyword]                   | TOFU   | [cluster name]   | Emerging Topic    | MEDIUM   | NEEDS VALID.  |
| 6 | [specific actionable content idea]                      | [primary keyword]                   | TOFU   | [cluster name]   | Missing Freshness | LOW      |               |
...

FLAG LEGEND:
  INFERRED:KW   → Target keyword was inferred from topic (not from keyword universe)
  INFERRED:INT  → Intent classification was inferred (not from SERP data)
  NEEDS VALID.  → Gap confidence is LOW — verify before actioning
  COMPETITOR    → Gap identified from competitor blind spot data
  EMERGING      → Topic signal is growing but not yet confirmed in keyword universe
  MEM-CONFLICT  → A similar idea was proposed in a prior session with different priority

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — PER-IDEA DETAIL RECORDS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH CONTENT IDEA — SORTED BY PRIORITY]

IDEA #[N]: [Content Idea Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Content Idea      : [full specific idea description — written as a content brief seed]
Target Keyword    : [primary keyword this content will target]
Secondary Keywords: [2–3 related keywords this content should incorporate]
Intent / Funnel   : [Informational/Commercial/Transactional] / [TOFU/MOFU/BOFU]
Cluster           : [cluster name]
Gap Type          : [gap type classification]
Priority          : [HIGH / MEDIUM / LOW]
Confidence        : [HIGH / MEDIUM / LOW]

GAP EVIDENCE:
  Layer 1 (Site):  [what the site currently has or lacks for this topic]
  Layer 2 (SERP):  [what the SERP shows — weak/strong/missing/format issue]
  Layer 3 (KW):    [where this keyword exists in the keyword universe]

STRATEGIC REASON:
  "[specific one-to-two sentence reason why this gap matters — tied to cluster, intent, and evidence]"

RECOMMENDED FORMAT    : [Blog Article / Comparison Page / Tutorial / Listicle / Landing Page / FAQ]
RECOMMENDED LENGTH    : [word count range — based on SERP pattern or content pattern extractor]
AEO OPPORTUNITY       : [YES — describes which snippet/PAA opportunity / NO]
Validation Flag       : [VALIDATED / NEEDS VALIDATION — reason if latter]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — GAP TYPE BREAKDOWN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

MISSING TOPIC ([count] ideas):
  Clusters affected   : [list]
  Most critical       : [top missing topic idea — idea #N]
  Pattern observation : [what common theme runs through missing topic gaps]

MISSING INTENT ([count] ideas):
  Clusters affected   : [list]
  Most critical       : [top missing intent idea]
  Funnel observation  : [which funnel stage is most absent across clusters]

MISSING DEPTH ([count] ideas):
  Clusters affected   : [list]
  Most critical       : [top missing depth idea]
  Depth observation   : [what type of depth is most commonly missing]

MISSING FORMAT ([count] ideas):
  Clusters affected   : [list]
  Most critical       : [top missing format idea]
  Format observation  : [which format type is absent from most SERPs]

MISSING FRESHNESS ([count] ideas):
  Clusters affected   : [list]
  Most critical       : [top missing freshness idea]
  Freshness pattern   : [which cluster has the most severe freshness gap]

EMERGING TOPIC ([count] ideas):
  Clusters affected   : [list]
  Most promising      : [top emerging topic idea]
  Emergence signals   : [what signals indicate this topic is growing]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — CLUSTER GAP MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Cluster Name              | Ideas Generated | HIGH | MED | LOW | Primary Gap Type    | Cluster Priority for Action |
|---------------------------|-----------------|------|-----|-----|---------------------|-----------------------------|
| [Cluster 1]               | [N]             | [N]  | [N] | [N] | Missing Topic       | CRITICAL                    |
| [Cluster 2]               | [N]             | [N]  | [N] | [N] | Missing Intent      | IMPORTANT                   |
| [Cluster 3]               | [N]             | [N]  | [N] | [N] | Missing Freshness   | MODERATE                    |
| [Cluster 4]               | 0               | 0    | 0   | 0   | NONE (well-covered) | MAINTAIN                    |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — VALIDATION QUEUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ideas marked NEEDS VALIDATION (confidence LOW):

| # | Content Idea             | Gap Type        | Confidence Issue                          | Recommended Verification                    |
|---|--------------------------|-----------------|-------------------------------------------|---------------------------------------------|
| 1 | [idea]                   | Emerging Topic  | No keyword in universe yet — topic inferred| Run keyword-discovery.skill for this topic |
| 2 | [idea]                   | Missing Format  | SERP format data inferred not confirmed   | Run serp-intelligence.skill for [keyword]  |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : opportunity-engine.skill
                         roadmap-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — THREE-LAYER COMPARISON

Before any gap can be identified, the three information layers must be constructed and organized for comparison. Each layer represents a distinct view of the content landscape — together they reveal the gap between what exists, what the SERP rewards, and what the opportunity space contains.

**Phase 1A — Layer 1 Construction: The Site's Existing Content**

Build a comprehensive map of what the site currently covers:

1. Load all published content records from `content-inventory.skill`.
2. Organize by cluster — for each cluster, list:
   - All published pages with their primary keyword.
   - The funnel stage (TOFU/MOFU/BOFU) of each published page.
   - The coverage score of each published page.
   - Whether a pillar page exists.
3. Add the coverage state from `content-awareness.skill`:
   - Covered topics (with their coverage depth).
   - Partially covered topics (with their gap reason).
   - Missing topics (confirmed as absent from all records).
4. Load pipeline content (draft + planned) separately — these do NOT count as covered but they DO prevent duplicate gap recommendations.

**Layer 1 Output Structure:**

```
LAYER 1 — SITE CONTENT MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster: [Name]
  Covered (published): [topic list with keyword + funnel stage]
  Partially covered : [topic list with reason for partial]
  Pipeline (not live): [planned/draft topics — to block duplicate gaps]
  Missing           : [topics confirmed absent from all records]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 1B — Layer 2 Construction: The SERP Landscape**

Build a map of what the SERP is currently serving for each cluster's keywords:

1. Load SERP intelligence from `serp-intelligence.skill` for all available keywords.
2. For each keyword with SERP data, record:
   - Dominant intent classification.
   - Difficulty rating (EASY / MEDIUM / HARD).
   - Opportunity signal (with evidence).
   - Dominant content type (what format currently ranks).
   - Freshness level (CURRENT / AGING / STALE / OUTDATED).
   - AEO signals (featured snippet presence, PAA questions).
3. For keywords with NO SERP data → apply Layer 2 inference (Phase 1E).
4. Organize by cluster — each cluster's keywords form a SERP profile.

**Layer 2 Output Structure:**

```
LAYER 2 — SERP LANDSCAPE MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster: [Name]
  Keyword: [keyword]
    Difficulty   : [EASY/MEDIUM/HARD]
    Dominant type: [format]
    Freshness    : [CURRENT/AGING/STALE/OUTDATED]
    AEO signal   : [presence/type or ABSENT]
    Opportunity  : [signal description]
    Data source  : [CONFIRMED / INFERRED]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 1C — Layer 3 Construction: The Keyword Universe**

Build a map of the full opportunity scope — every keyword that represents a potential content target regardless of whether content exists or what the SERP currently shows:

1. Load the full keyword universe from `semantic-clustering.skill`.
2. Organize by cluster — every keyword in every cluster forms Layer 3.
3. For each keyword, record:
   - Intent / funnel stage.
   - Category (from keyword-discovery dimensions).
   - Strategic reason (why this keyword matters).
   - TF-IDF weight (keyword's importance within its cluster).
4. This is the complete opportunity map — the set of all positions the site could theoretically target.

**Layer 3 Output Structure:**

```
LAYER 3 — KEYWORD UNIVERSE MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster: [Name]
  [keyword] | [intent] | [funnel] | [category] | weight: [H/M/L]
  [keyword] | [intent] | [funnel] | [category] | weight: [H/M/L]
  ...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 1D — Cross-Layer Alignment**

For each cluster, align the three layers:

Create a three-column matrix per cluster where each row is a keyword from Layer 3:

| Keyword (L3)           | Site Has Content (L1)     | SERP Status (L2)           | Gap Signal |
|------------------------|---------------------------|----------------------------|------------|
| [keyword]              | YES — published [URL]     | STRONG competition          | NONE       |
| [keyword]              | NO                        | WEAK — outdated SERP        | GAP        |
| [keyword]              | PARTIAL — draft only      | MEDIUM                      | PARTIAL GAP|
| [keyword]              | NO                        | SERP data unavailable       | INFERRED GAP|

This matrix is the primary input for Step 2 gap detection.

**Phase 1E — Layer 2 Inference (When SERP Data Is Absent)**

When `serp-intelligence.skill` data is not available for a keyword:

Apply SERP status inference based on available signals:

| Available Signal                                             | Inferred SERP Status     |
|--------------------------------------------------------------|--------------------------|
| Gap status = FULL GAP or MISSING from content-awareness     | SERP likely WEAK → Infer EASY opportunity |
| Cluster is a TOFU-only cluster with no BOFU                | SERP difficulty unknown → Infer MEDIUM |
| Keyword has 4+ words with audience + use case modifiers    | SERP likely WEAK → Infer EASY |
| Competitor analysis shows blind spot for this topic         | SERP likely WEAK for this angle |
| No information available                                    | Infer MEDIUM → flag as INFERRED |

All inferred SERP statuses are flagged in Layer 2 as `INFERRED` and carry lower confidence in gap scoring.

---

### ▸ STEP 2 — GAP DETECTION

Using the three-layer comparison matrix from Step 1, systematically detect every type of gap across every cluster.

**Phase 2A — Primary Gap Detection Protocol**

A gap exists at any matrix row where:

| Condition                                                                    | Gap Signal Detected |
|------------------------------------------------------------------------------|---------------------|
| L1 = NO (no published content) AND L3 = keyword exists in universe          | TOPIC GAP           |
| L1 = YES (content exists) AND L2 = WEAK SERP (existing content can be beaten) | DEPTH GAP         |
| L1 = YES (content exists) AND L3 shows a different funnel stage is missing  | INTENT GAP          |
| L1 = YES (content exists) AND L2 shows different format dominates SERP      | FORMAT GAP          |
| L1 = YES (content exists) AND L2 shows STALE/OUTDATED content dominates    | FRESHNESS GAP       |
| L3 has keywords in a growing category AND L1 = NO AND L2 = WEAK/ABSENT     | EMERGING TOPIC GAP  |

**Phase 2B — Cluster-Level Gap Aggregation**

After per-keyword gap detection, aggregate to the cluster level:

For each cluster:
1. Count how many keywords have each gap type.
2. Identify the cluster's dominant gap type (the most frequent gap signal across its keywords).
3. Flag clusters where ALL keywords show the same gap type → likely a systemic coverage failure, not individual keyword gaps.
4. Flag clusters where gap signals conflict across keywords → cluster may need refinement.

**Phase 2C — Gap Confirmation Protocol**

Before a gap proceeds to idea generation, it must be confirmed by at least TWO signals from different layers:

| Gap Type          | Layer 1 Confirmation                           | Layer 2 OR Layer 3 Confirmation                     |
|-------------------|-------------------------------------------------|------------------------------------------------------|
| TOPIC GAP         | No published content (L1)                       | Keyword exists in universe (L3) AND/OR SERP is weak (L2) |
| INTENT GAP        | Published content exists but wrong funnel stage | Same keyword at different funnel in universe (L3)    |
| DEPTH GAP         | Published content at coverage score ≤ 2         | SERP shows competitors have higher depth (L2)        |
| FORMAT GAP        | Published content in wrong format for intent    | SERP dominant format differs from site's format (L2) |
| FRESHNESS GAP     | Published content age > 12 months               | SERP shows STALE results dominate (L2)               |
| EMERGING TOPIC    | No content exists (L1)                          | Keyword shows growth signals in universe (L3) AND SERP is open (L2) |

If a gap can only be confirmed by ONE signal → mark as NEEDS VALIDATION. Do not exclude it — but flag it for human review before actioning.

---

### ▸ STEP 3 — GAP TYPE CLASSIFICATION

Classify every confirmed gap into one of six defined gap types. Every gap must receive exactly one primary gap type classification.

---

**GAP TYPE 1 — MISSING TOPIC**

**Definition:** A topic exists in the keyword universe and represents a real search demand, but the site has zero published content targeting it. This is the most obvious and most actionable gap type.

**Confirmation Requirements:**
- L3: Keyword present in the universe.
- L1: No published page with this primary or secondary keyword.
- No draft or planned content targeting this keyword.

**Detection Method:**
Compare every keyword in Layer 3 against Layer 1's keyword list. Any keyword in L3 not present in L1's published content = MISSING TOPIC candidate (subject to confirmation).

**Why It Matters:** Missing topics mean the site is invisible to searchers actively looking for this content. The site has no opportunity to rank, engage, or convert these searchers regardless of how strong the rest of its content is.

**Typical Action:** Create new content targeting the missing keyword.

---

**GAP TYPE 2 — MISSING INTENT**

**Definition:** Content exists for a topic but it targets the wrong funnel stage. The site has a TOFU article on a topic where BOFU or MOFU content is what the SERP rewards. Or the site has BOFU content on a topic where the SERP is informational and no TOFU article builds the audience.

**Confirmation Requirements:**
- L1: Published content exists for this topic.
- L3: Additional keywords at different funnel stages exist in the universe.
- L2: The missing funnel stage has SERP presence (demand exists at that stage).

**Detection Method:**
For each cluster, identify which funnel stages are represented in L1 and which are in L3 but not L1. The gap is between the funnel stages present in the universe and those present in the site's content.

**Why It Matters:** Missing intent content creates funnel holes — the site attracts visitors but cannot serve them at the stage they are actually in. A TOFU-only cluster drives traffic that has nowhere to go.

**Typical Action:** Create content specifically targeting the missing funnel stage for the existing topic.

---

**GAP TYPE 3 — MISSING DEPTH**

**Definition:** Content exists targeting the right keyword and funnel stage, but the existing content is too thin — coverage score 1 or 2 — to compete with the depth of content in the SERP. The site's content is present but inadequate.

**Confirmation Requirements:**
- L1: Published content exists with coverage score ≤ 2.
- L2: SERP shows competing content with higher depth (score 3+ equivalent).
- L3: Keyword is valid and strategically important.

**Detection Method:**
For every published page with coverage score ≤ 2, check L2 to determine whether the SERP's existing results are stronger. If SERP results consistently show greater depth → MISSING DEPTH gap.

**Why It Matters:** Thin content ranks poorly and poorly serves readers. A site with a keyword coverage score of 1 competing against a SERP full of comprehensive guides will not rank. Addressing depth gaps can improve existing rankings more efficiently than creating entirely new content.

**Typical Action:** Expand and rewrite the existing thin page, or create a new comprehensive piece that replaces the thin one.

---

**GAP TYPE 4 — MISSING FORMAT**

**Definition:** Content exists targeting the right keyword at the right funnel stage with adequate depth, but the format used by the site does not match what the SERP rewards for this keyword. The site has a blog post where the SERP rewards a comparison table. Or a tutorial where the SERP rewards a listicle.

**Confirmation Requirements:**
- L1: Published content exists for this topic.
- L2: SERP dominant format is different from the site's content format.
- The format difference is strategic — not cosmetic (a listicle vs. numbered tutorial for a "how to" query = strategic mismatch).

**Detection Method:**
For each published page, compare the page's format (from content-inventory) against the dominant format identified by `serp-intelligence.skill` for the target keyword. A mismatch where the site uses a format that accounts for ≤20% of SERP results = FORMAT GAP.

**Why It Matters:** Google's intent matching system strongly penalizes format mismatches. A blog article for a "how to" query that the SERP serves with tutorial content will consistently underperform regardless of quality.

**Typical Action:** Create a new piece in the correct format (the existing piece may remain as supplementary content, or be restructured).

---

**GAP TYPE 5 — MISSING FRESHNESS**

**Definition:** Content exists targeting the right keyword at the right funnel stage, but the content is outdated — either because the publication date is STALE (12+ months) and the topic is time-sensitive, or because the SERP shows that recent content is being rewarded and the site's content is being outranked by newer results.

**Confirmation Requirements:**
- L1: Published content exists with publication age ≥ 12 months.
- L2: SERP freshness level is HIGH (recent content dominates) OR content references outdated data/tools/approaches.
- The topic is NOT purely evergreen — it has time-sensitive components.

**Detection Method:**
Load freshness flags from `content-awareness.skill`. For every STALE or OUTDATED page, check the SERP freshness level in L2. If the SERP shows recent results dominanting → FRESHNESS GAP.

**Why It Matters:** Outdated content loses rankings over time to newer, fresher competitors. Freshness gaps are often the easiest wins — a comprehensive update of an existing piece that already has some authority can produce rapid ranking improvements.

**Typical Action:** Refresh and republish the existing page with updated data, new examples, and structural improvements. A full freshness update counts as a new content production effort.

---

**GAP TYPE 6 — EMERGING TOPIC**

**Definition:** A topic is growing in the keyword universe and shows increasing search demand, but the SERP has not yet been captured by authoritative content. These are first-mover opportunities — the kind of gaps where a site can establish authority before competition solidifies.

**Confirmation Requirements:**
- L3: Keyword or topic is present in the universe with signals of growth.
- L1: No published content exists.
- L2: SERP is OPEN (low competition) or dominated by very recent content (< 3 months old), suggesting the topic is newly emerging.

**Detection Method:**
Identify keywords in the universe flagged as EMERGING by `keyword-discovery.skill` or by competitor analysis as a blind spot. Cross-reference with SERP to confirm low competition. If SERP is open AND no site content exists AND topic shows growth signals → EMERGING TOPIC gap.

**Why It Matters:** Emerging topics offer the highest first-mover advantage. A site that publishes authoritative content before competition solidifies can own a keyword before anyone else establishes rankings.

**Typical Action:** Prioritize for immediate creation — the window for first-mover advantage closes as competitors detect the same emerging signal.

---

### ▸ STEP 4 — DEDUPLICATION GATE

Before any gap proceeds to idea generation, it must pass through `deduplication-engine.skill` to ensure no duplicate ideas are generated.

**Phase 4A — Deduplication Engine Invocation**

Call `deduplication-engine.skill` with:
- **Invocation context:** `Calling Skill = gap-opportunity-engine.skill`
- **Candidate type:** CONTENT_IDEAS
- **Strictness level:** STRICT (production-ready gap output requires maximum deduplication)
- **Candidate pool:** All confirmed gaps (as keyword + topic pairs)

**Phase 4B — What Deduplication Blocks**

| What Is Blocked                                                                  | Why                                                                 |
|----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Gaps where the keyword already exists in published content                       | Topic is covered — not a gap                                        |
| Gaps where the keyword is assigned to a draft or planned piece                  | Topic is in pipeline — already being addressed                      |
| Gaps that are semantic duplicates of another gap in the same batch              | Duplicate gaps produce duplicate content ideas                      |
| Gaps that were proposed in a prior session and rejected or actioned             | Memory cross-check — prevents recycling old ideas                   |
| Gaps where the keyword is a memory duplicate (generated in a prior session)     | Cross-session deduplication                                          |

**Phase 4C — Refresh Exception Application**

When a gap is blocked because content already exists, the deduplication engine applies the Refresh Exception (Step 6 of `deduplication-engine.skill`):
- If the existing content is STALE or OUTDATED → block the gap from generating a NEW CONTENT idea, but route it to the MISSING FRESHNESS gap type instead.
- If the existing content has coverage score ≤ 2 → block the MISSING TOPIC idea, but route it to MISSING DEPTH gap type instead.

This ensures that thin or stale existing content is not silently excluded — it becomes a different gap type rather than being dropped entirely.

**Phase 4D — Post-Deduplication Gap Count**

After deduplication:
1. Record gaps removed by each deduplication type.
2. Record gaps redirected to different gap types (refresh exceptions).
3. Proceed to idea generation only with the cleaned, deduplicated gap set.

```
DEDUPLICATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Pre-dedup gaps         : [count]
Blocked (covered)      : [count]
Blocked (pipeline)     : [count]
Blocked (semantic dup) : [count]
Blocked (memory)       : [count]
Redirected to REFRESH  : [count]
Redirected to DEPTH    : [count]
Gaps after dedup       : [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 5 — IDEA GENERATION

For every gap that passed the deduplication gate, generate a specific, unique, actionable content idea.

**Phase 5A — Idea Specificity Requirements**

Every content idea must be specific enough to brief a writer directly without additional research. It must specify:

| Element                | Requirement                                                                  |
|------------------------|------------------------------------------------------------------------------|
| **Topic angle**        | The specific sub-angle, audience, use case, or comparison that differentiates this idea from generic treatment of the keyword |
| **Target audience**    | Who this content is for — specific enough that "everyone" is never a valid answer |
| **Content purpose**    | What the content achieves for the reader (makes a decision, learns a process, finds a tool, compares options) |
| **Primary keyword**    | The exact keyword this content will be optimized to rank for               |
| **Format**             | What type of content this will be (guide, comparison, tutorial, FAQ, etc.) |

**The Specificity Test:**

Apply this test to every generated idea:

`"Could a writer start writing this content without asking a single clarifying question?"`

- YES → The idea is specific enough. Include it.
- NO → The idea is still vague. Add more specificity before including.

**Phase 5B — Prohibited Idea Forms**

| Prohibited Form                                        | Problem                                           | Required Alternative                               |
|--------------------------------------------------------|---------------------------------------------------|----------------------------------------------------|
| `"Write about CRM software"`                           | Generic topic, no angle, no audience              | `"Comparison of CRM tools under $50/month for freelancers: features vs. setup time"` |
| `"Create SEO content for startups"`                    | No keyword, no format, no specific gap            | `"How early-stage SaaS startups should prioritize technical SEO before launching to their first 1,000 users"` |
| `"Write a guide on email marketing"`                   | Entire topic category — not a specific gap        | `"Email open rate optimization: a/b testing subject lines for B2B SaaS audiences with < 10% open rates"` |
| `"Cover missing BOFU content"`                         | Describes the gap type — not the idea             | Name the actual content piece with its keyword and angle |
| `"Update the CRM article"`                             | Vague — which article? update to what standard?  | `"Refresh [specific URL] — update CRM comparison table, add 2024 pricing data, add FAQ section targeting 3 new PAA questions"` |

**Phase 5C — Idea Construction Protocol**

For each confirmed gap, construct the idea using this assembly:

1. **Identify the core keyword** (from Layer 3 keyword universe).
2. **Identify the gap type** (from Step 3).
3. **Identify the differentiation angle** (from Layer 2 SERP analysis — what is missing from current results? What angle would a new piece take that existing SERP results do not?).
4. **Identify the audience** (from the keyword's modifiers and cluster context).
5. **Assemble the idea** as: `"[Format]: [Specific angle for the audience] — [keyword context] — [differentiating element]"`

**Examples of correctly-assembled ideas by gap type:**

| Gap Type          | Keyword                                        | Assembled Idea                                                                                                     |
|-------------------|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| Missing Topic     | `"crm for law firms"`                          | `"Best CRM for Law Firms in 2024: Feature comparison for case management, client billing, and compliance needs"` |
| Missing Intent    | `"hubspot crm"` (BOFU missing, TOFU only)     | `"HubSpot CRM Free Trial vs. Paid: What you actually get access to and when to upgrade"` |
| Missing Depth     | `"crm implementation"` (thin article exists)  | `"CRM Implementation: Complete step-by-step guide covering data migration, team onboarding, and the first 90 days"` |
| Missing Format    | `"best crm for remote teams"` (blog, SERP = comparison table) | `"Remote Team CRM Comparison: Head-to-head scoring of 7 CRMs on remote collaboration features, pricing, and integrations"` |
| Missing Freshness | `"crm market share 2021"` (outdated page)     | `"CRM Market Share 2024: Updated analysis of the top 10 platforms by industry, company size, and use case"` |
| Emerging Topic    | `"ai crm"` (new, SERP open)                   | `"AI-Powered CRM: What the first generation of AI CRM features actually does (and doesn't do) for sales teams"` |

**Phase 5D — Secondary Keyword Assignment**

For each content idea, assign 2–3 secondary keywords that the content should naturally incorporate:

1. Load the keyword cluster containing the target keyword.
2. Select 2–3 keywords from the same cluster that represent sub-topics or related angles the content should cover.
3. Verify no secondary keyword duplicates the primary keyword.
4. Record secondary keywords in the idea record.

**Phase 5E — Format and Length Recommendation**

For each idea, recommend:

**Format** (based on Layer 2 SERP dominant format and gap type):
- MISSING TOPIC: Match the SERP dominant format.
- MISSING INTENT (BOFU): Comparison page or landing page format.
- MISSING DEPTH: Comprehensive guide format.
- MISSING FORMAT: The format that is missing from the SERP (the opposite of what currently ranks).
- MISSING FRESHNESS: Same format as existing content — update, not rebuild.
- EMERGING TOPIC: First-mover format — comprehensive guide to establish authority before competition defines the format.

**Length** (based on content-pattern-extractor data if available, otherwise SERP estimate):
- Reference the dominant word count range for similar keywords in the SERP.
- If no data: TOFU = 1,500–2,500 words; MOFU = 2,000–3,500 words; BOFU = 1,500–2,000 words (focused).
- Emerging topics: 2,000–3,000 words (comprehensive early authority piece).

---

### ▸ STEP 6 — PRIORITY ASSIGNMENT

Score every gap-generated idea by urgency to determine which ideas deserve immediate execution.

**Phase 6A — Priority Scoring Factors**

| Factor                                | HIGH Signal                                | MEDIUM Signal                        | LOW Signal                          |
|---------------------------------------|--------------------------------------------|--------------------------------------|-------------------------------------|
| Intent value                          | BOFU                                       | MOFU                                 | TOFU                                |
| Gap confirmation strength             | Both L1 and L2 confirm gap               | Only L1 or only L2 confirms          | Inferred — needs validation         |
| Cluster strategic importance          | Pillar cluster / CRITICAL gap urgency     | Sub-cluster / IMPORTANT gap urgency  | Micro-cluster / OPTIONAL            |
| SERP competition                      | EASY (weak competition)                   | MEDIUM (beatable competition)        | HARD (strong competition)           |
| Gap type urgency                      | Missing Topic (BOFU) / Emerging Topic    | Missing Intent / Missing Format      | Missing Freshness / Missing Depth   |
| Cluster authority score               | Score 0–1 (no authority)                  | Score 2–3 (partial authority)        | Score 4–5 (strong authority)        |

**Phase 6B — Priority Decision Rules**

An idea receives HIGH priority if:
- Intent is BOFU AND at least 2 other HIGH signals are present.
- OR Cluster authority score = 0 AND gap is confirmed by both layers.
- OR Gap type is EMERGING TOPIC AND SERP is EASY AND cluster is a Pillar or high-strength Sub.

An idea receives MEDIUM priority if:
- Intent is MOFU AND at least 1 HIGH signal is present.
- OR Intent is BOFU but competition is MEDIUM or HARD.
- OR Cluster authority score is 2–3 AND gap type is Missing Topic or Missing Intent.

An idea receives LOW priority if:
- Intent is TOFU AND SERP competition is MEDIUM or HARD.
- OR Gap confirmation strength is LOW (inferred, needs validation).
- OR Cluster authority score is 4–5 (minor enrichment, not strategic urgency).

**Phase 6C — Priority Override Conditions**

| Override Condition                                                            | Override Action                                    |
|-------------------------------------------------------------------------------|----------------------------------------------------|
| Idea is in a cluster flagged as CRITICAL by `authority-engine.skill`          | Minimum priority = MEDIUM (not LOW)                |
| Idea is for an Emerging Topic with EASY SERP (time-sensitive first-mover)   | Minimum priority = MEDIUM; recommend immediate action note |
| Idea is in a user-designated priority cluster                                  | Minimum priority = HIGH regardless of signal strength |
| Idea's cluster is a PREREQUISITE dependency for other HIGH-priority clusters  | Elevate priority by one level (LOW→MEDIUM, MED→HIGH) |

---

### ▸ STEP 7 — STRATEGIC REASON GENERATION

For every content idea, produce a strategic reason statement that explains why this gap matters and ties the recommendation to specific data.

**Phase 7A — Strategic Reason Quality Standards**

Every strategic reason must:
1. Reference the specific gap type and the evidence behind it.
2. Name the cluster and its strategic importance.
3. Explain the business consequence of the gap continuing unfilled.
4. Be 1–2 sentences — concise and specific.

**Prohibited Strategic Reason Forms:**

| Prohibited                                               | Problem                                              |
|----------------------------------------------------------|------------------------------------------------------|
| `"This gap matters for SEO"`                             | Generic — could apply to any keyword                 |
| `"This will improve rankings"`                           | Vague — no specific mechanism or evidence            |
| `"We should cover this topic"`                           | Not a reason — just a statement                      |
| `"This keyword has high search volume"`                  | Volume is not in this skill's scoring — not evidence |
| `"Competitors are covering this"`                        | Insufficient alone — needs to name the specific opportunity |

**Required Strategic Reason Template:**

`"[Gap type] for '[keyword]' in the [Cluster Name] cluster [gap evidence]. [Business consequence of gap remaining unfilled]."`

**Strategic Reason Examples:**

| Gap Type          | Strategic Reason                                                                                                                                                               |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Missing Topic     | `"Missing topic for 'crm for law firms' in the CRM by Industry cluster — no published content exists and the SERP shows only outdated forum results with EASY difficulty. Every buyer from the legal sector searching this keyword cannot find the site, and the topic represents confirmed BOFU intent with no current coverage."` |
| Missing Intent    | `"Missing BOFU content for 'hubspot crm comparison' in the CRM Selection cluster — the site has 3 TOFU articles building awareness but no content serving users ready to make a vendor decision. The evaluation-stage audience generated by these TOFU articles has nowhere to convert."` |
| Missing Depth     | `"Missing depth for 'crm implementation guide' in the CRM Setup cluster — the site's existing article has a coverage score of 2 (Partial) while the SERP's top 3 results average 4,500+ words with step-by-step structured guides. The thin existing page cannot compete for the keyword it targets."` |
| Missing Format    | `"Missing comparison format for 'best crm under $50' in the CRM Pricing cluster — the site's blog article format is mismatched against a SERP dominated by structured comparison tables (7 of 9 analyzed pages use tabular format). Google is rewarding structured evaluation content, not editorial prose."` |
| Missing Freshness | `"Missing freshness for 'crm market trends' in the CRM Strategy cluster — the existing article was published in 2021 and the SERP now rewards content from the last 6 months with a HIGH freshness requirement. The page is actively losing rankings to newer competitors."` |
| Emerging Topic    | `"Emerging topic opportunity for 'ai crm' in the CRM Automation cluster — the keyword is in the universe with EASY SERP difficulty and only 3 months of SERP results competing. First-mover advantage is available for a site that publishes a comprehensive guide before competition solidifies."` |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ SERP GAP SIMULATION

When `serp-intelligence.skill` data is unavailable for specific keywords, this skill simulates the SERP gap likelihood based on structural and contextual signals.

**Simulation Protocol:**

**Signal 1 — Content-Awareness Gap Status:**

| Content-Awareness Status                  | Simulated SERP Gap         | Confidence |
|-------------------------------------------|----------------------------|------------|
| Coverage Status = MISSING                 | SERP likely WEAK → EASY    | MEDIUM     |
| Coverage Status = GAP                     | SERP likely WEAK → EASY    | MEDIUM     |
| Coverage Status = PARTIAL                 | SERP likely MEDIUM          | LOW        |
| Coverage Status = COMPLETE                | SERP likely STRONG → HARD  | LOW        |

**Signal 2 — Keyword Pattern:**

| Keyword Pattern                                             | Simulated SERP Gap    | Confidence |
|-------------------------------------------------------------|----------------------|------------|
| 4+ words + niche industry modifier                         | Likely EASY           | MEDIUM     |
| 2-word head term                                           | Likely HARD           | LOW        |
| Question form + specific audience                          | Likely EASY–MEDIUM    | MEDIUM     |
| Comparison keyword with two named tools                    | MEDIUM                | LOW        |

**Signal 3 — Cluster Tier and Strength:**

| Condition                                                   | Simulated Gap          |
|-------------------------------------------------------------|------------------------|
| Pillar cluster + score 0 (no content)                      | HIGH gap likelihood    |
| Micro-cluster + any score                                  | LOW gap (likely niche) |
| Sub-cluster + score 1–2                                    | MEDIUM gap likelihood  |

All simulation-derived signals are flagged: `INFERRED:SERP — simulated from structural signals.`

---

### ▸ CLUSTER GAP MAPPING

Beyond individual keyword gaps, this skill identifies cluster-level patterns — where the same type of gap affects an entire cluster rather than individual keywords.

**Cluster-Level Gap Pattern Detection:**

| Pattern                                                                    | Cluster Gap Type             | Strategic Implication                                        |
|----------------------------------------------------------------------------|------------------------------|--------------------------------------------------------------|
| ≥70% of cluster keywords show MISSING TOPIC gap                           | CLUSTER TOPIC GAP            | Entire cluster territory is uncovered — high first-mover potential |
| ≥70% of cluster keywords show MISSING INTENT gap (same funnel stage absent)| CLUSTER FUNNEL GAP           | Entire cluster funnel is broken — all traffic leaks at missing stage |
| ≥70% of cluster keywords show MISSING DEPTH gap                            | CLUSTER DEPTH GAP            | Cluster has content presence but insufficient depth — mass update needed |
| ≥60% of cluster keywords show MISSING FRESHNESS gap                        | CLUSTER FRESHNESS GAP        | Cluster content base is aging — systematic refresh needed    |
| ≥3 cluster keywords show EMERGING gap                                      | CLUSTER EMERGING OPPORTUNITY | Cluster territory has multiple growing topics — build out aggressively |

When a cluster-level gap pattern is detected → add a cluster-level recommendation to the output as a STRATEGIC NOTE:

`"CLUSTER [Name]: [N]% of keywords show [gap type] — this is a systemic coverage issue, not isolated keyword gaps. Recommend: [cluster-level build strategy]."`

---

### ▸ EMERGING TOPIC DETECTION

Identify topics that are growing in the keyword universe before they become competitive — the highest-value first-mover opportunities.

**Emerging Topic Signals:**

| Signal                                                                    | Emerging Topic Indicator |
|---------------------------------------------------------------------------|--------------------------|
| Keyword is in the universe but has very recent SERP results (< 3 months) | HIGH emerging signal      |
| Competitor analysis shows blind spot for this topic                        | HIGH emerging signal      |
| Keyword matches a pattern common in growing niches (AI + existing tool)  | MEDIUM emerging signal    |
| SERP difficulty is EASY but topic is clearly relevant to the cluster      | MEDIUM emerging signal    |
| Keyword has no established SERP feature targeting (no featured snippet)  | LOW emerging signal       |

**Emerging Topic Validation:**

Emerging topics require higher scrutiny before actioning — they may be genuinely emerging OR they may simply be niches that never developed an audience.

Validate emerging topics by checking:
1. Is the keyword present in the keyword universe (L3)? If yes → CONFIRMED emerging topic.
2. Is the SERP EASY? If yes → CONFIRMED emerging opportunity.
3. Does any competitor content exist (even weak)? If yes → not first-mover but still early.
4. Is the topic a logical extension of a STRONG existing cluster? If yes → CONFIRMED cluster expansion.

If fewer than 2 of the 4 validation checks pass → mark as NEEDS VALIDATION with reason: "Emerging topic signal weak — verify demand before creating content."

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate Ideas in the Same Output

Every content idea in the output must be distinct from every other idea. If two gaps produce ideas that are semantically equivalent (same target keyword, same angle, same funnel stage) → keep only the one with higher priority. Log the removed duplicate.

### Rule 2 — No Repeated Gap Detections

If the same keyword-level gap is detected through multiple detection methods (e.g., once through content-awareness missing topics AND once through keyword universe comparison) → record it as a single gap. Do not generate two ideas for the same gap. The gap type should reflect the primary reason for the gap.

### Rule 3 — Cross-Check with Memory — No Recycled Ideas

Before finalizing any content idea, query `memory-controller.skill` for:
- Prior gap analyses that proposed this same idea (or semantic equivalent).
- Prior roadmap entries that already include this topic.
- Prior content creation that was completed for this keyword.

If any match is found:
- If the prior idea was ACTIONED → the content should now exist. Query content inventory to verify. If not found → the prior action was abandoned; the gap is real; re-surface the idea.
- If the prior idea was REJECTED → note the prior rejection reason. If the reason is still valid → exclude. If the reason is no longer valid (e.g., prior rejection was "SERP too competitive" — now SERP has weakened) → re-include with note: "MEM-CONFLICT — prior rejection no longer valid: [reason]."
- If the prior idea is still PENDING → flag as MEM-CONFLICT: "This gap was already proposed in session [ID] and not yet actioned."

### Rule 4 — No Idea Without a Cluster

Every content idea must be assigned to exactly one cluster. A content idea with no cluster cannot be prioritized correctly, cannot be tracked in the gap map, and cannot be integrated into the roadmap. If an idea cannot be clustered → flag as ORPHAN IDEA; do not include in the main output.

### Rule 5 — No Idea Without a Strategic Reason

Every content idea must have a strategic reason statement. An idea without a reason is a generic suggestion — it provides no decision-making value. If a reason cannot be written for an idea (because the gap evidence is too weak) → the idea should be in the NEEDS VALIDATION queue, not the main output.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Gap Analysis Behaviors

| Prohibited Behavior                                                                              | Reason                                                                              |
|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Generating a content idea without confirming the gap exists via at least two layers              | Unconfirmed gaps produce invalid content ideas — waste editorial resources           |
| Generating ideas as topic categories (`"write about SEO"`) instead of specific pieces           | Category ideas cannot be briefed — they produce more vague ideas instead of content |
| Generating multiple ideas for the same keyword with different phrasings                          | This is idea duplication — blocked by Rule 1                                        |
| Skipping the deduplication gate for any idea regardless of how obvious the gap seems            | Deduplication is mandatory — "obvious" gaps can still duplicate pipeline content    |
| Marking an idea as HIGH priority because the keyword is BOFU without checking gap confirmation  | BOFU intent is one factor — not a sufficient reason alone for HIGH priority         |
| Generating an idea for a cluster without checking Layer 1 (site content) first                  | Content may already exist — generating an idea without Layer 1 = potential duplication |
| Using the same strategic reason statement for two different ideas                                | Every strategic reason must be unique to its specific gap evidence                  |
| Including emerging topic ideas without validating at least 2 of the 4 emerging topic checks    | Unvalidated emerging topics may be niche topics with no audience — high risk        |
| Assigning the wrong gap type to save classification effort                                       | Gap types determine action recommendations — miscategorizing a gap = wrong action   |
| Producing a NEEDS VALIDATION queue with no specific reason per item                             | Every validation flag must explain what specifically needs validating               |

### Required Output Behaviors

- Every idea must have: Content Idea (specific), Target Keyword, Intent/Funnel, Cluster, Gap Type, Priority, and Strategic Reason.
- Every idea must have passed the deduplication gate before appearing in the main output.
- Every HIGH priority idea must have its priority supported by at least two HIGH signals from Phase 6A.
- Emerging topic ideas require 2+ validation checks passed before appearing in the main output.
- The NEEDS VALIDATION queue must include a specific recommended verification action per item.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-awareness.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (Layer 1 + coverage confirmation)                                      |
| **Receives**     | Coverage map per cluster, confirmed missing topics, partially covered topics, pipeline content, freshness flags |
| **Used for**     | Phase 1A (Layer 1 construction), Step 2 (gap confirmation), Gap Type 5 (MISSING FRESHNESS)  |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (Layer 3 + cluster context)                                            |
| **Receives**     | Full keyword universe organized by cluster with intent, funnel, category, strategic reasons |
| **Used for**     | Phase 1C (Layer 3 construction), Step 3 gap type classification, Step 5B idea construction |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream intelligence source (Layer 2)                                                      |
| **Receives**     | SERP difficulty, opportunity signal, dominant format, freshness level, AEO data, PAA questions |
| **Used for**     | Phase 1B (Layer 2 construction), Gap Types 2–6 SERP confirmation, Step 5E (format recommendation) |

---

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (Layer 1 detail)                                                       |
| **Receives**     | All content records — URL, keyword, cluster, status, intent, coverage score                |
| **Used for**     | Phase 1A (Layer 1), Phase 4B deduplication (pipeline content blocking), Gap Type 3 (MISSING DEPTH) |

---

### ▸ deduplication-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Called service (mandatory gate before idea finalization)                                    |
| **Sends**        | All confirmed gaps as content idea candidates — strictness = STRICT                        |
| **Receives back**| Clean list (deduplicated ideas) + rejection log + refresh candidates                       |
| **Timing**       | Step 4 — after gap confirmation, before idea generation                                    |

---

### ▸ competitor-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Gap matrix, competitor blind spots, contested vs. open topics                              |
| **Used for**     | Enriching Layer 2 (Phase 1B), Gap Type 6 (EMERGING TOPIC), Priority scoring (Phase 6A)    |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before gap detection, WRITE after output finalization                  |
| **Reads**        | Prior gap analyses, prior roadmap entries, prior content for this domain                   |
| **Writes**       | Complete gap opportunity report, all content ideas with priority and gap type              |
| **Memory layers**| strategy-memory.skill (gap intelligence), content-memory.skill (idea records)             |

---

### ▸ opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | gap-opportunity-engine → opportunity-engine (downstream consumer)                          |
| **Sends**        | Full gap opportunity table — all ideas with keywords, clusters, gap types, and priorities  |
| **Used for**     | Scoring and enriching gap ideas with the 5-dimension opportunity score                    |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | gap-opportunity-engine → roadmap-engine (primary content idea source)                      |
| **Sends**        | Full gap opportunity table sorted by priority, cluster gap map, action queue               |
| **Critical fields** | Content Idea + Target Keyword + Priority — these three fields drive roadmap entry creation |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                            | Detection                                             | Recovery Action                                                                                              |
|-----------------------------------------------------------------------------|-------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Content-awareness.skill data is unavailable                                 | Layer 1 cannot be constructed                         | Halt. Flag: "Layer 1 unavailable — content-awareness.skill output required before gap detection. Cannot confirm which topics are already covered." |
| Keyword universe is unavailable                                              | Layer 3 cannot be constructed                         | Proceed with Layer 1 and Layer 2 only. Flag: "PARTIAL GAP ANALYSIS — keyword universe absent. Gaps based on SERP data only; universe-based gaps not detectable." |
| SERP intelligence is unavailable for all clusters                           | Layer 2 cannot be constructed                         | Apply SERP gap simulation for all keywords. Flag entire output: "SIMULATED SERP DATA — all Layer 2 signals inferred. Verify with serp-intelligence.skill before actioning." |
| Deduplication gate blocks >80% of detected gaps                             | Phase 4D — post-dedup gap count < 20% of pre-dedup   | Flag: "HIGH DEDUPLICATION RATE — most gaps are already covered or in pipeline. Gap analysis shows the content strategy is well-aligned with the keyword universe. Focus on freshness and depth updates." |
| No gaps detected after all three layers are compared                        | Step 2 — gap matrix shows NONE for all keywords      | Flag: "ZERO GAPS DETECTED — all keywords in the universe have adequate coverage OR SERP is too competitive for all targets. Recommend: expand keyword universe or review cluster definitions." |
| An idea cannot be assigned to any cluster                                   | Step 5 — cluster lookup fails                         | Mark as ORPHAN IDEA. Do not include in main output. Suggest the most plausible cluster to the user for manual assignment. |
| More than 50% of ideas are NEEDS VALIDATION                                 | Step 6 — confidence assessment                        | Flag: "HIGH UNCERTAINTY RATE — more than half of gap ideas need validation. Recommend running serp-intelligence.skill and competitor-analysis.skill before actioning any ideas." |
| Memory controller unavailable for cross-check                               | Step 4 — memory query returns error                   | Proceed without memory cross-check. Flag: "MEMORY CROSS-CHECK SKIPPED — prior proposals and actioned ideas not verified. Duplicate ideas may exist in roadmap." |
| A gap is confirmed by Layer 1 and Layer 3 but Layer 2 shows HARD SERP      | Step 2 — gap exists but SERP is very competitive      | Keep the gap but assign LOW priority and add note: "GAP EXISTS but SERP is HARD — competition is strong. Only pursue with exceptional differentiation angle." |
| All detected gaps are EMERGING TOPIC type with no confirmed keyword data   | Step 3 — all gaps are type 6 with LOW validation      | Flag: "EMERGING MARKET SIGNAL — keyword universe may be ahead of SERP. Validate demand with keyword-discovery.skill expansion before committing editorial resources." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — GAP TYPE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
SIX GAP TYPES — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TYPE 1 — MISSING TOPIC
  Site: NO content
  SERP: Any (opportunity exists)
  Universe: Keyword present
  Action: Create new content
  Example: No page targeting "crm for agencies"

TYPE 2 — MISSING INTENT
  Site: Content exists but WRONG funnel stage
  SERP: Different funnel stage has demand
  Universe: Multiple funnel keywords exist
  Action: Create content at missing funnel stage
  Example: TOFU content only; BOFU keyword in universe

TYPE 3 — MISSING DEPTH
  Site: Content exists at coverage score ≤ 2
  SERP: Competitors rank with deeper content
  Universe: Keyword is valid
  Action: Expand/rewrite existing or create superior piece
  Example: 500-word overview article vs. 3000-word guide in SERP

TYPE 4 — MISSING FORMAT
  Site: Content exists in wrong format for SERP
  SERP: Different format dominates (e.g., comparison vs. blog)
  Universe: Keyword valid
  Action: Create in correct format
  Example: Blog article where SERP rewards comparison table

TYPE 5 — MISSING FRESHNESS
  Site: Content exists but age > 12mo on time-sensitive topic
  SERP: STALE or recent content dominates
  Action: Comprehensive refresh and republish
  Example: "crm pricing 2021" page losing to 2024 results

TYPE 6 — EMERGING TOPIC
  Site: NO content
  SERP: EASY / open / very recent results
  Universe: Keyword showing growth signals
  Action: IMMEDIATE — first-mover advantage window
  Example: "ai crm" before established results emerge

CONFIRMATION REQUIREMENT: Every gap type needs 2+ signals.
DEDUP GATE: All gap types pass through deduplication-engine.skill.
VALIDATION: Low-confidence gaps → NEEDS VALIDATION queue.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — CONTENT IDEA SPECIFICITY CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CONTENT IDEA SPECIFICITY CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Before including any idea in the output, verify:

□ Does the idea name a SPECIFIC TOPIC ANGLE?
  (Not "write about CRM" — but "compare CRM pricing for freelancers under $50")

□ Does the idea identify a TARGET AUDIENCE?
  (Not "for users" — but "for solopreneurs" or "for enterprise sales teams")

□ Does the idea have a TARGET KEYWORD assigned?
  (Not "something about CRM pricing" — but "crm software under $50/month")

□ Does the idea specify a FORMAT?
  (Not "create content" — but "comparison table" or "step-by-step guide")

□ Could a writer start writing WITHOUT asking a clarifying question?
  If NO → idea is not specific enough. Add the missing element.

□ Is the idea UNIQUE in the output?
  Does any other idea target the same keyword with the same angle?
  If YES → remove the lower-priority duplicate.

□ Is the strategic reason tied to SPECIFIC DATA?
  (Not "this gap matters for SEO" — but "BOFU intent in a cluster scored 0 by authority-engine")

□ PROHIBITED PHRASES CHECK — none of these appear:
  ✗ "write about [topic]"
  ✗ "cover [topic area]"
  ✗ "create content for [keyword]"
  ✗ "this will help SEO"
  ✗ "this is a common search query"
  ✗ "there is demand for this"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — THREE-LAYER COMPARISON QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
THREE-LAYER COMPARISON — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

LAYER 1 — SITE CONTENT (What the site has):
  Source: content-inventory + content-awareness
  Contains: published pages, their keywords, funnel stages, coverage scores
  Tells us: What topics are covered, partially covered, or missing

LAYER 2 — SERP LANDSCAPE (What search engines show):
  Source: serp-intelligence
  Contains: difficulty, dominant format, freshness, AEO signals
  Tells us: Whether the current SERP can be beaten; what format is rewarded

LAYER 3 — KEYWORD UNIVERSE (What the opportunity set contains):
  Source: semantic-clustering → keyword-discovery
  Contains: all target keywords with intent, funnel, strategic reason
  Tells us: The full scope of topics the site should be targeting

GAP MATRIX (Per keyword, three columns):
  L3 keyword | L1 status | L2 status | → Gap signal

  L3: exists | L1: NO     | L2: WEAK  → HIGH-confidence MISSING TOPIC
  L3: exists | L1: YES    | L2: WEAK  → DEPTH or FORMAT gap
  L3: exists | L1: PARTIAL| L2: ANY   → MISSING INTENT or MISSING DEPTH
  L3: exists | L1: YES    | L2: STALE → MISSING FRESHNESS
  L3: emerging| L1: NO    | L2: OPEN  → EMERGING TOPIC

CONFIRMATION RULE:
  Gap must be confirmed by ≥2 layers.
  Single-layer gap → NEEDS VALIDATION (not excluded; flagged).
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: analysis/gap-opportunity-engine.skill*
*Nexus SEO Operating System — Version 1.0.0*
