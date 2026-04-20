# SKILL: analysis/authority-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Analysis / Topical Authority Measurement
# EXECUTION PRIORITY: POST-INVENTORY AND POST-CLUSTERING — Runs after content-inventory.skill and semantic-clustering.skill are complete.
# POSITION: Authority measurement layer. Converts content inventory into a structured authority map that drives roadmap sequencing.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **topical authority measurement engine** of the Nexus system.

Topical authority is not simply the number of articles a site has published. It is the depth, completeness, and strategic coherence of a site's coverage across a defined topic cluster — whether the site has content that serves every stage of the reader's journey, whether it has a structural hub (pillar page) that Google can recognize as the center of the topic, whether it covers the entities and sub-topics that define genuine expertise, and whether it creates the internal linking architecture that passes authority through the cluster.

This skill reads the content inventory and the cluster architecture simultaneously — matching what exists to what the cluster structure requires — and produces an authority score per cluster that reflects the site's real competitive positioning in that topic area. A score of 0 means the cluster is completely unaddressed. A score of 5 means it is comprehensively covered with full funnel representation, a pillar, and strong supporting depth.

The authority map produced by this skill is the primary input for content roadmap prioritization. It tells the roadmap engine not just what is missing — but what is critically missing (BOFU clusters with zero authority), what is partially built (medium-coverage clusters that need one more pillar or funnel stage), and what is already an asset (high-authority clusters that need only maintenance).

### Primary Responsibilities

| Responsibility                        | Description                                                                                                         |
|---------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| **Content-to-Cluster Mapping**        | Match every published, draft, and planned content record to its target cluster from the cluster architecture        |
| **Coverage Classification**           | Classify each cluster's content coverage as Complete, Strong, Medium, Weak, Minimal, or Missing                    |
| **Authority Score Assignment**        | Assign a 0–5 authority score to every cluster based on coverage classification and funnel representation           |
| **Missing Element Detection**         | Identify the specific missing components in each cluster — pillar, BOFU, supporting pages, entity coverage         |
| **Critical Gap Identification**       | Classify gaps by urgency — Critical (BOFU with no authority), Important (MOFU with weak authority), Optional       |
| **Action Recommendation Generation** | For each gap, produce a specific, actionable recommendation — what to build, why it matters, expected impact       |
| **Cluster Weight Importance**         | Weight cluster authority scores by the cluster's strategic importance in the topic graph                           |
| **Funnel-Based Authority Modeling**   | Model authority not just by page count but by funnel stage distribution — incomplete funnel = incomplete authority  |
| **Entity Depth Validation**           | Validate that each cluster's content adequately covers the critical entities required for topical authority         |
| **Authority Map Production**          | Produce a complete, structured authority map with scores, gaps, and actions for all clusters                       |

### What This Skill Is NOT

- It is **not** a domain authority tool. It measures topical authority — the depth and coherence of content coverage — not domain-level link metrics.
- It is **not** a keyword ranker. It evaluates content coverage against cluster requirements — not keyword rankings.
- It is **not** a content quality grader. It measures coverage completeness — not writing quality, readability, or readability scores.
- It is **not** a competitive analysis tool. It evaluates the site's own authority map — not how the site compares to competitors (that is `competitor-analysis.skill`'s role).
- It is **not** optional for sites with existing content. Any site with more than a few published pages must run this skill before building a content roadmap — otherwise the roadmap will be built on assumptions rather than the actual authority state.

### The Foundational Principle of This Skill

> Authority is not about volume. It is about completeness.
>
> A cluster with 10 thin TOFU articles and no pillar, no MOFU, and no BOFU content
> has LESS topical authority than a cluster with 4 well-structured articles
> that cover the full funnel with a pillar, two supporting pieces, and a conversion page.
>
> This skill measures authority the way Google evaluates it:
> through the depth, structure, and funnel coherence of a site's coverage —
> not through raw page count.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                        | Required |
|---------------------------------------|--------------------------------------------------------------------------------------------------------|----------|
| `semantic-clustering.skill`           | Full cluster architecture — cluster names, tier (Pillar/Sub/Micro), keyword lists, funnel balance, cluster strength, pillar topic labels | YES |
| `content-inventory.skill`             | Complete content inventory — URL, primary keyword, secondary keywords, intent, cluster assignment, status (published/draft/planned), coverage score | YES |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                              | Used For                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| `entity-extraction.skill`             | Entity map per cluster — Critical/Important entities                                         | Entity depth validation (Step 4 — missing entity coverage detection) |
| `query-graph.skill`                   | Node types, Hub Scores, dependency chains, authority flow data                               | Cluster weight importance (Advanced Logic)                        |
| `content-awareness.skill`             | Coverage map, freshness flags, funnel gaps per cluster                                       | Freshness-adjusted authority scoring                              |
| `serp-intelligence.skill`             | SERP difficulty per cluster, opportunity signals                                             | Contextualizing authority score against competitive environment   |
| `competitor-analysis.skill`           | Competitor authority levels per cluster                                                      | Relative authority assessment — "strong vs. competition" flags   |

### Per-Cluster Data Requirements

For each cluster in the authority mapping:

| Field                          | Source                                  | Required | Fallback if Missing                                           |
|--------------------------------|-----------------------------------------|----------|---------------------------------------------------------------|
| `cluster_name`                 | semantic-clustering                     | YES      | Halt — cluster name is non-negotiable                        |
| `cluster_tier`                 | semantic-clustering                     | YES      | Infer from position; flag as INFERRED                         |
| `funnel_balance`               | semantic-clustering                     | YES      | Infer from keyword intent distribution; flag                  |
| `cluster_strength`             | semantic-clustering                     | SOFT     | Default to 50; flag as ESTIMATED                             |
| `published_pages`              | content-inventory (status = published)  | YES      | If none found → cluster score = 0                            |
| `draft_pages`                  | content-inventory (status = draft)      | SOFT     | Treat as absent for authority scoring; note in plan           |
| `planned_pages`                | content-inventory (status = planned)    | SOFT     | Treat as absent for authority scoring; note in plan           |
| `coverage_scores`              | content-inventory (per page)            | SOFT     | Default to 2 (Partial) for unknown pages; flag               |
| `entity_set`                   | entity-extraction                       | OPTIONAL | Skip entity validation; note absence                         |

### Input Pre-Conditions

1. **Is the cluster architecture complete?** Every cluster must have a name, tier, and funnel balance before authority mapping begins. Incomplete cluster data produces inaccurate authority scores.
2. **Is the content inventory current?** The inventory must reflect the actual published state — not aspirational or planned state. Draft and planned content does NOT count toward authority scores (it counts toward planning context only).
3. **Minimum cluster count:** No minimum — even a single cluster can be authority-mapped. However, a single cluster produces no relative comparison insights.
4. **Has authority mapping been run before?** → Query `memory-controller.skill`. If found within freshness threshold (14 days) AND neither the inventory nor the cluster architecture has changed → offer cached result. If stale or changed → proceed fresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Topical Authority Map

```
NEXUS TOPICAL AUTHORITY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain                  : [domain]
Authority Map Version   : [N]
Total Clusters Mapped   : [count]
  Score 5 (Complete)    : [count] — [%]
  Score 4 (Strong)      : [count] — [%]
  Score 3 (Medium)      : [count] — [%]
  Score 2 (Weak)        : [count] — [%]
  Score 1 (Minimal)     : [count] — [%]
  Score 0 (None)        : [count] — [%]
Overall Domain Authority Score: [weighted average / 5]
Critical Gaps           : [count]
Important Gaps          : [count]
Optional Gaps           : [count]
Generated By            : analysis/authority-engine.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — CLUSTER AUTHORITY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Cluster Name              | Tier    | Pages | TOFU | MOFU | BOFU | Pillar | Coverage Level | Auth Score | Gap Urgency | Action Required     |
|---------------------------|---------|-------|------|------|------|--------|----------------|------------|-------------|---------------------|
| [Cluster 1]               | Pillar  | [N]   | [N]  | [N]  | [N]  | YES    | COMPLETE       | 5          | NONE        | MAINTAIN            |
| [Cluster 2]               | Pillar  | [N]   | [N]  | [N]  | [N]  | YES    | STRONG         | 4          | OPTIONAL    | ADD BOFU            |
| [Cluster 3]               | Sub     | [N]   | [N]  | [N]  | [N]  | NO     | MEDIUM         | 3          | IMPORTANT   | ADD PILLAR + BOFU   |
| [Cluster 4]               | Sub     | [N]   | [N]  | [N]  | [N]  | NO     | WEAK           | 2          | IMPORTANT   | BUILD OUT           |
| [Cluster 5]               | Micro   | [N]   | [N]  | [N]  | [N]  | NO     | MINIMAL        | 1          | CRITICAL    | URGENT: ADD BOFU    |
| [Cluster 6]               | Pillar  | 0     | 0    | 0    | 0    | NO     | MISSING        | 0          | CRITICAL    | BUILD FROM SCRATCH  |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — CLUSTER AUTHORITY DETAIL RECORDS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH CLUSTER]

CLUSTER: [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Tier                  : [Pillar / Sub / Micro]
Cluster Strength      : [0–100]
Authority Score       : [0–5]
Coverage Level        : [COMPLETE / STRONG / MEDIUM / WEAK / MINIMAL / MISSING]
Gap Urgency           : [CRITICAL / IMPORTANT / OPTIONAL / NONE]

PUBLISHED CONTENT:
  TOFU pages      : [count] — [URL list or NONE]
  MOFU pages      : [count] — [URL list or NONE]
  BOFU pages      : [count] — [URL list or NONE]
  Pillar page     : [URL or MISSING]
  Total published : [count]
  Avg coverage score: [N/5]

PIPELINE CONTENT (not counted toward authority score):
  Draft pages     : [count] — [topics]
  Planned pages   : [count] — [topics]

MISSING ELEMENTS:
  → [Missing element 1] — [why it matters]
  → [Missing element 2] — [why it matters]
  → [Missing element 3] — [why it matters]

ENTITY COVERAGE (if entity map available):
  Critical entities covered   : [count] / [total critical]
  Important entities covered  : [count] / [total important]
  Entity gaps                 : [list of uncovered critical entities]

ACTION REQUIRED:
  Priority     : [CRITICAL / IMPORTANT / OPTIONAL / MAINTAIN]
  Build        : [specific content piece to create — not generic]
  Why          : [specific reason tied to authority and business impact]
  Expected Impact: [what authority improvement this action produces]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — CRITICAL GAP REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITICAL GAPS (BOFU clusters with score 0–1; Pillar clusters with score 0):

| # | Cluster              | Score | Missing Critical Element     | Business Impact                                    | Build Immediately               |
|---|----------------------|-------|------------------------------|----------------------------------------------------|---------------------------------|
| 1 | [name]               | 0     | Complete cluster — no content| Site cannot convert searchers in this topic area   | [specific content piece]        |
| 2 | [name]               | 1     | BOFU + Pillar both missing   | Revenue-stage searches have no landing point       | [specific content piece]        |
...

IMPORTANT GAPS (MOFU clusters with score 1–2; Sub-clusters with score 0–1):

| # | Cluster              | Score | Missing Important Element    | Business Impact                                    | Build Next Cycle                |
|---|----------------------|-------|------------------------------|----------------------------------------------------|---------------------------------|
| 1 | [name]               | 2     | Missing MOFU evaluation page | Site loses readers at consideration stage          | [specific content piece]        |
| 2 | [name]               | 1     | No supporting pages          | Single thin page cannot signal topical depth       | [specific content piece]        |
...

OPTIONAL GAPS (TOFU clusters; micro-clusters; well-covered clusters needing enrichment):

| # | Cluster              | Score | Optional Enhancement         | Enhancement Value                                  | Build When Capacity Allows      |
|---|----------------------|-------|------------------------------|----------------------------------------------------|---------------------------------|
| 1 | [name]               | 3     | Additional TOFU depth page   | Broaden awareness reach; strengthen cluster depth  | [specific content piece]        |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — AUTHORITY DISTRIBUTION ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

AUTHORITY BY CLUSTER TIER:
  Pillar clusters avg score  : [N/5]
  Sub-clusters avg score     : [N/5]
  Micro-clusters avg score   : [N/5]
  Tier balance observation   : [specific observation about where authority is concentrated]

AUTHORITY BY FUNNEL STAGE:
  TOFU coverage depth  : [avg page count per cluster at TOFU] — [observation]
  MOFU coverage depth  : [avg page count per cluster at MOFU] — [observation]
  BOFU coverage depth  : [avg page count per cluster at BOFU] — [observation]
  Funnel authority gap : [which funnel stage is weakest across all clusters]

OVERALL DOMAIN AUTHORITY SCORE:
  Weighted average     : [N/5]
  Weighted by          : cluster strength and tier importance
  Interpretation       : [what this score means in practice — specific to this domain]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — ACTION PRIORITY QUEUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

(Sorted by gap urgency, then cluster strength, then BOFU priority)

| # | Priority  | Cluster          | Action                          | Why                                        | Expected Authority Gain  |
|---|-----------|------------------|---------------------------------|--------------------------------------------|--------------------------|
| 1 | CRITICAL  | [name]           | [specific piece to create]      | [specific authority impact statement]      | Score: 0 → [N]           |
| 2 | CRITICAL  | [name]           | [specific piece to create]      | [specific authority impact statement]      | Score: [N] → [N]         |
| 3 | IMPORTANT | [name]           | [specific piece to create]      | [specific authority impact statement]      | Score: [N] → [N]         |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : roadmap-engine.skill
                         strategist-ai.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — MAP CONTENT TO CLUSTERS

Match every content record in the inventory to the cluster it targets. This is the foundational mapping step — all authority scoring depends on having accurate content-to-cluster assignments.

**Phase 1A — Primary Mapping (Cluster Field)**

If the content inventory record has a `cluster` field populated (assigned during `content-inventory.skill` processing):
1. Use the `cluster` field directly as the primary assignment.
2. Verify that the cluster name matches a cluster name in the current `semantic-clustering.skill` architecture.
3. If the cluster name from the inventory does not match any current cluster name → flag as CLUSTER MISMATCH and apply Phase 1C.

**Phase 1B — Keyword-Based Matching (Fallback)**

If the cluster field is null or mismatched:
1. Take the record's `primary_keyword`.
2. Find the cluster in the semantic-clustering architecture whose keyword list contains this keyword (or a semantic equivalent).
3. If found → assign to that cluster.
4. Flag as KEYWORD-MATCHED (secondary mapping method).

**Phase 1C — Topic-Based Matching (Second Fallback)**

If neither the cluster field nor the keyword matches:
1. Use the record's `Primary Topic` to find the closest matching cluster by topic name.
2. Apply semantic similarity: does the record's topic fall within the scope of any cluster's name and description?
3. If match found → assign. Flag as TOPIC-MATCHED.
4. If no match found → classify the record as ORPHAN CONTENT — content that exists but belongs to no current cluster.

**Phase 1D — Orphan Content Handling**

For every ORPHAN CONTENT record:
1. Log it in the authority map as unattached content.
2. Do NOT count it toward any cluster's authority score.
3. Surface it to the user: "N content records have no cluster assignment. These may belong to undiscovered cluster territories or may be out-of-scope content."
4. Suggest the most plausible cluster for each orphan based on topic analysis.

**Phase 1E — Multi-Cluster Content Handling**

Some content pieces serve multiple clusters (e.g., a comprehensive guide covering both "CRM Setup" and "CRM Integration"). Handle as follows:
1. Assign the record to its PRIMARY cluster (the cluster whose primary keyword the content most directly targets).
2. Add a cross-reference note to secondary clusters: "Content at [URL] has partial relevance to this cluster — counts as 0.5 supporting pages."
3. Do NOT count the same page as a full contribution to two different clusters — this inflates authority scores.

**Phase 1F — Status Filtering**

Only published content records count toward authority scores. Apply status filtering:

| Status     | Counts Toward Authority? | How It Appears                                    |
|------------|--------------------------|---------------------------------------------------|
| published  | YES — full credit        | Included in all authority calculations            |
| draft      | NO — not yet live        | Listed under "Pipeline Content" — does not affect score |
| planned    | NO — does not exist yet  | Listed under "Pipeline Content" — does not affect score |
| archived   | NO — no longer live      | Not included in authority map; noted separately   |

**Phase 1G — Mapping Completeness Validation**

After all records are mapped:
1. Count total records assigned to at least one cluster.
2. Count total orphan records.
3. Verify: orphan records + assigned records = total content inventory records.
4. Log the mapping result:

```
MAPPING RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total records           : [count]
Successfully mapped     : [count]
  Primary mapping       : [count]
  Keyword-matched       : [count]
  Topic-matched         : [count]
Orphan records          : [count]
Status filtered (excluded): [count — draft + planned + archived]
Net published records   : [count — for authority scoring]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — COVERAGE CLASSIFICATION

For each cluster, evaluate the quality and completeness of its mapped content to assign a Coverage Level classification.

**Phase 2A — Coverage Classification Criteria**

Apply these criteria in order. The FIRST criteria level that is NOT fully met determines the Coverage Level:

**COMPLETE (Score 5):**

ALL of the following must be true:
- ✅ A pillar page exists (published — not draft/planned).
- ✅ At least 2 supporting published pages (beyond the pillar).
- ✅ All three funnel stages are represented with at least 1 published page each (TOFU + MOFU + BOFU).
- ✅ Average coverage score across all cluster pages ≥ 3.5 (Solid to Comprehensive).
- ✅ If an entity map is available: ≥80% of Critical entities are covered by existing content.

**STRONG (Score 4):**

ALL of the following must be true:
- ✅ A pillar page exists (published).
- ✅ At least 2 supporting published pages (beyond the pillar).
- ✅ At least 2 of 3 funnel stages are represented with published content.
- ✅ Average coverage score ≥ 3 (Solid).
- ⚠️ One of: BOFU missing, or entity coverage < 80%, or one funnel stage absent.

**MEDIUM (Score 3):**

ALL of the following must be true:
- ✅ 2+ published pages in the cluster.
- ✅ At least 2 of 3 funnel stages represented.
- ❌ No pillar page OR pillar page has coverage score ≤ 2.
- Average coverage score: 2–3 (Partial to Solid).

**WEAK (Score 2):**

Condition:
- 1–2 published pages in the cluster, OR
- Only 1 funnel stage represented with any published content, OR
- Only a pillar exists with no supporting pages (hub without spokes).
- Average coverage score ≤ 2 (Partial or below).

**MINIMAL (Score 1):**

Condition:
- Exactly 1 published page exists in the cluster.
- That page has a coverage score of 1 (Thin).
- No funnel diversity — a single thin page that barely addresses the topic.

**MISSING (Score 0):**

Condition:
- Zero published pages in the cluster.
- No content exists regardless of draft or planned status.

**Phase 2B — Coverage Classification Edge Cases**

| Edge Case                                                                  | Resolution                                                           |
|----------------------------------------------------------------------------|----------------------------------------------------------------------|
| Pillar page exists but has coverage score 1 (Thin)                        | Does not count as a genuine pillar — treat as WEAK not STRONG        |
| 5+ pages exist but all are TOFU only                                       | Counts as MEDIUM at most — no funnel depth despite volume           |
| 3 pages exist but 2 are duplicates (same keyword, different URLs)          | Count as 2 unique pages (duplicate alert already exists in inventory)|
| Cluster has published content but all pages are ARCHIVED                  | Treat as MISSING — archived content is not live                     |
| Cluster has strong coverage but all pages are STALE (18+ months old)     | Downgrade by 1 level from the calculated classification; note freshness risk |

**Phase 2C — Freshness Adjustment**

When `content-awareness.skill` data is available with freshness flags:

If a cluster has CLUSTER FRESHNESS RISK (≥50% of cluster pages are STALE or OUTDATED):
- Downgrade the Coverage Level by one step (STRONG → MEDIUM, MEDIUM → WEAK, WEAK → MINIMAL).
- Log the downgrade: "Coverage downgraded from [level] to [level] — CLUSTER FRESHNESS RISK: [N]% of pages are [STALE/OUTDATED]."

---

### ▸ STEP 3 — AUTHORITY SCORE ASSIGNMENT

Assign a precise 0–5 authority score to every cluster based on the Coverage Level classification from Step 2 and additional modifying factors.

**Phase 3A — Base Score from Coverage Level**

| Coverage Level | Base Authority Score |
|----------------|----------------------|
| COMPLETE       | 5                    |
| STRONG         | 4                    |
| MEDIUM         | 3                    |
| WEAK           | 2                    |
| MINIMAL        | 1                    |
| MISSING        | 0                    |

**Phase 3B — Authority Score Modifying Factors**

After assigning the base score, apply modifying factors that can adjust the score ±0.5 (scores are capped at 5 and floored at 0; adjustments may produce decimal scores for the detailed record — the cluster authority table shows rounded integers):

**Positive Modifiers (+0.5):**

| Condition                                                                  | Modifier |
|----------------------------------------------------------------------------|----------|
| All cluster pages have coverage score ≥ 4 (Comprehensive)                 | +0.5     |
| Cluster has a featured snippet or PAA capture confirmed via SERP data     | +0.5     |
| Pillar page has strong AEO signals (FAQ schema + direct answer)           | +0.5     |
| Cluster content is consistently FRESH (< 6 months) across all pages      | +0.5     |
| Entity coverage ≥ 90% of Critical entities                                | +0.5     |

**Negative Modifiers (-0.5):**

| Condition                                                                  | Modifier |
|----------------------------------------------------------------------------|----------|
| Cluster has duplicate content alerts (keyword cannibalization flagged)    | -0.5     |
| Cluster pages have no internal links connecting them (no internal link architecture) | -0.5 |
| Average coverage score across cluster pages ≤ 1.5 (mostly Thin content)  | -0.5     |
| CLUSTER FRESHNESS RISK (if not already applied in Step 2C)               | -0.5     |
| No pillar page AND cluster strength ≥ 70 (important cluster without hub) | -0.5     |

**Final Score Calculation:**

`Authority Score = Base Score + Σ(Positive Modifiers) - Σ(Negative Modifiers)`

Cap at 5; floor at 0. Round to nearest 0.5 for detailed records; round to nearest integer for summary table.

**Phase 3C — Weighted Domain Authority Score**

After all individual cluster scores are calculated, produce the weighted domain authority score:

`Domain_Authority_Score = Σ(cluster_authority × cluster_weight) / Σ(cluster_weights)`

**Cluster Weight Calculation:**

When `query-graph.skill` hub scores are available:

| Node Type      | Cluster Weight |
|----------------|----------------|
| PRIMARY HUB    | 3.0            |
| HUB            | 2.0            |
| CONNECTOR      | 1.5            |
| LEAF           | 1.0            |
| ORPHAN         | 0.5            |

When query-graph data is NOT available:

| Cluster Tier | Cluster Weight |
|--------------|----------------|
| Pillar       | 2.0            |
| Sub          | 1.5            |
| Micro        | 1.0            |

---

### ▸ STEP 4 — MISSING ELEMENT DETECTION

For each cluster, identify the specific content elements that are absent. Every missing element must be named explicitly — generic "needs more content" observations are prohibited.

**Phase 4A — Pillar Page Detection**

A pillar page is a comprehensive hub article that defines the cluster's central topic and links to all supporting content within the cluster.

**Pillar Page Identification Criteria:**

| Criterion                                                              | Signal                                                   |
|------------------------------------------------------------------------|----------------------------------------------------------|
| Coverage score ≥ 4 (Comprehensive or Authority level)                 | Strong coverage depth signal                             |
| Primary keyword matches the cluster name directly                      | The page is "about" the cluster's core topic             |
| URL structure is at the root or shallow path                           | `/crm-software/` suggests hub; `/blog/crm-software-for-startups/` suggests supporting |
| Content targets a broad, head-term keyword rather than a specific long-tail | Hub pages target general cluster terms             |
| Intent is TOFU or mixed (TOFU+MOFU)                                   | Pillars serve awareness + consideration, not just conversion |

If NO page in the cluster satisfies ≥3 of these criteria → PILLAR MISSING.

Flag: `"PILLAR MISSING — [Cluster Name] has no hub page. Without a pillar, Google cannot recognize this site as an authority on [pillar topic]."`

**Phase 4B — BOFU Content Detection**

BOFU content serves searchers ready to make a decision. Its absence means the site cannot convert topic-aware searchers into customers or leads.

BOFU content is MISSING if:
- Zero published pages in the cluster have intent = BOFU (Transactional or strongly Commercial).
- OR all BOFU-intent pages have coverage score ≤ 2 (Thin or Partial).

Flag: `"BOFU MISSING — [Cluster Name] has no conversion-stage content. Site traffic from this cluster cannot progress to action."`

**Phase 4C — Supporting Pages Detection**

Supporting pages are non-pillar articles that address specific sub-topics, audience segments, or long-tail angles within the cluster's territory.

Supporting pages are INSUFFICIENT if:
- Total published pages in the cluster < 3 (including pillar if it exists).
- OR all published pages target the same funnel stage (no variety).
- OR all published pages target the same audience segment (no breadth).

Flag: `"SUPPORTING PAGES INSUFFICIENT — [Cluster Name] has [N] published pages but needs ≥3 for topical depth signal. Google cannot evaluate topical authority on a single page."`

**Phase 4D — Entity Coverage Detection**

When an entity map from `entity-extraction.skill` is available, validate whether the cluster's published content covers the Critical entities required for topical authority in this topic area.

Entity Coverage Assessment:

1. Load the list of Critical entities for the cluster.
2. For each Critical entity, check whether any published page in the cluster explicitly covers it (in a heading, in the keyword set, or flagged in the entity map as "covered").
3. Count covered vs. total Critical entities.
4. Calculate entity coverage rate: `covered / total_critical × 100`.

| Entity Coverage Rate | Entity Signal                                                              |
|----------------------|----------------------------------------------------------------------------|
| ≥ 80%                | ENTITY COVERAGE STRONG — cluster is semantically comprehensive            |
| 60–79%               | ENTITY COVERAGE PARTIAL — some critical entities are absent                |
| < 60%                | ENTITY COVERAGE WEAK — cluster lacks key semantic signals for authority    |

Flag for each uncovered Critical entity: `"ENTITY GAP: [Entity Name] — This entity is critical to topical authority for [Cluster Name] but is not covered by any existing content."`

**Phase 4E — Missing Element Summary Assembly**

After all four missing element checks are complete, compile a prioritized list for each cluster:

```
MISSING ELEMENTS: [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. [CRITICAL] PILLAR MISSING — Hub page needed to organize cluster authority
2. [HIGH] BOFU MISSING — No conversion-stage content
3. [MEDIUM] SUPPORTING PAGES — Only [N] pages; need [N] more for depth signal
4. [LOW] ENTITY GAPS — [N] critical entities uncovered: [list]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 5 — CRITICAL GAP IDENTIFICATION

Classify each cluster's gap urgency based on the combination of authority score, cluster strategic importance, and missing element types.

**Phase 5A — Gap Urgency Classification**

| Urgency Level | Trigger Conditions                                                                                            |
|---------------|---------------------------------------------------------------------------------------------------------------|
| **CRITICAL**  | ANY of the following: BOFU cluster with authority score 0–1; Pillar cluster with score 0; Cluster is on a DEPENDENCY CHAIN for high-priority clusters AND score ≤ 1; Cluster is PRIMARY HUB or HUB with score 0–1 |
| **IMPORTANT** | ANY of the following: MOFU cluster with score 1–2; Sub-cluster with score 0–1; Pillar cluster with score 1–2 but missing key element (no BOFU, no pillar page); Cluster with CLUSTER FRESHNESS RISK and score ≤ 2 |
| **OPTIONAL**  | TOFU cluster (informational only) at any score; Micro-cluster at score ≤ 2; Any cluster with score ≥ 3 missing only minor enrichment elements (more TOFU depth, entity coverage < 80% but > 60%) |
| **NONE**      | Coverage Level = COMPLETE; Authority score ≥ 4.5; All mandatory elements present |

**Phase 5B — CRITICAL vs. IMPORTANT Resolution for Ambiguous Cases**

When a cluster's trigger conditions could qualify for either CRITICAL or IMPORTANT:

| Ambiguous Case                                                          | Resolution Rule                                                                |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| MOFU cluster with score 0 (normally IMPORTANT by intent, but score = 0)| Escalate to CRITICAL — zero authority at any meaningful intent level demands urgent action |
| TOFU cluster with score 0 AND cluster_strength ≥ 70 (important but TOFU)| Escalate to IMPORTANT — high-strength clusters cannot be OPTIONAL regardless of intent |
| BOFU cluster with score 2 (WEAK coverage, not 0–1)                     | IMPORTANT — BOFU cluster with any coverage is not CRITICAL; needs improvement but not urgent rebuild |
| Micro-cluster with score 0                                               | IMPORTANT (not CRITICAL unless it is on a dependency chain) — micro-clusters are naturally lower priority |

**Phase 5C — Gap Urgency Override**

User-designated priority clusters (if the user has flagged specific clusters as business-critical) receive an urgency override:
- Any user-designated cluster with score < 4 → CRITICAL regardless of calculated urgency.
- Log the override: "URGENCY OVERRIDE — [Cluster Name] is user-designated priority; elevated to CRITICAL."

---

### ▸ STEP 6 — ACTION RECOMMENDATIONS

For every cluster with a gap urgency of CRITICAL, IMPORTANT, or OPTIONAL, produce a specific, actionable recommendation.

**Phase 6A — Recommendation Structure**

Every recommendation must contain three components:

| Component          | Requirement                                                                                   |
|--------------------|-----------------------------------------------------------------------------------------------|
| **What to Build**  | Specific content type, target keyword, intended format, and target funnel stage — not "add content" |
| **Why It Matters** | Specific authority impact — what changes in the site's topical authority when this is built  |
| **Expected Impact** | The projected authority score increase — from current score to expected score after building  |

**Phase 6B — What to Build — Content Specification Rules**

The "What to Build" specification must name:
1. The content type (Pillar page / BOFU comparison page / Supporting tutorial / FAQ page / etc.).
2. The target keyword (primary keyword the piece will be optimized for).
3. The intended format (comprehensive guide / comparison table / tutorial / FAQ / landing page).
4. The funnel stage it serves.

**Prohibited "What to Build" forms:**

| Prohibited                                    | Reason                                        |
|-----------------------------------------------|-----------------------------------------------|
| `"Create more content for this cluster"`      | Not specific — what content? for whom? format?|
| `"Write a blog post about [topic]"`           | Too generic — no keyword, funnel, or format   |
| `"Improve the existing content"`              | Not a new build — belongs in a refresh plan   |
| `"Add a page targeting [cluster name]"`       | Cluster name ≠ keyword — specify the keyword  |

**Required "What to Build" form:**

`"Publish a [format] targeting '[primary keyword]' at [funnel stage], covering [specific sub-topics or entities], structured as a [content type]."`

**Phase 6C — Why It Matters — Specificity Requirements**

The "Why It Matters" statement must reference:
- The current authority score and what is missing.
- The specific business consequence of the missing element (loss of conversion traffic, inability to rank for the topic, etc.).
- NOT generic SEO advice ("content improves rankings").

**Examples of valid Why It Matters statements:**

| What Is Missing         | Why It Matters (Specific)                                                                                 |
|-------------------------|-----------------------------------------------------------------------------------------------------------|
| Pillar missing          | "Without a pillar page for [Cluster Name], Google cannot identify this site as an authority on [topic] — the cluster's supporting pages lack a structural hub that aggregates their authority signals." |
| BOFU missing            | "The cluster has 4 TOFU articles driving awareness but zero content serving buyers ready to make a decision — all of this TOFU-generated traffic exits the site without a conversion path." |
| Supporting pages missing| "[Cluster Name] has a single thin page — a cluster with 1 page cannot signal topical depth to Google. Minimum 3 pages are required for meaningful topical authority signals." |
| Entity gap              | "The [Entity Name] is a critical concept in [Cluster Name]'s topic area, referenced in all top-ranking competitor content, but absent from the site's cluster coverage — creating a semantic completeness gap that weakens the cluster's authority signals." |

**Phase 6D — Expected Impact — Score Projection**

Project the authority score that would result from implementing each recommendation:

**Score Projection Logic:**

For a cluster currently at score [N], adding:
- A pillar page (coverage score ≥ 4) → +0.5 to +1.0 depending on current state.
- A BOFU page → +0.5 to +1.0 depending on whether BOFU was the only missing element.
- 2+ supporting pages → +0.5 to +1.0.
- Full funnel completion → +1.0 to +1.5.

Express the projection as: `"Building this piece is expected to move [Cluster Name] from Authority Score [N] to [N+delta]."`

**Phase 6E — Complete Action Recommendation Format:**

```
ACTION: [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Priority         : [CRITICAL / IMPORTANT / OPTIONAL]
Current Score    : [N/5]
Projected Score  : [N+delta/5]

Build            : [specific content specification]
Why              : [specific authority impact statement]
Expected Impact  : Authority score [current] → [projected]

Additional Notes : [any constraints, sequencing dependencies, or related actions]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CLUSTER WEIGHT IMPORTANCE

Not all clusters are equal. A missing cluster in the site's highest-priority topical territory (a Pillar cluster that anchors the entire content architecture) is far more damaging than a missing cluster in a micro-niche sub-topic. This advanced logic applies strategic weight to authority scores to produce a strategically meaningful domain authority picture.

**Weighting Protocol:**

When query-graph data is available:

1. Use the Hub Score from `query-graph.skill` as the weight factor.
2. Clusters with higher Hub Scores carry more weight in the domain authority calculation.
3. A gap in a PRIMARY HUB cluster (Hub Score 85+) is 6× more damaging to domain authority than a gap in a LEAF cluster (Hub Score < 25).

**Weighted Gap Impact Formula:**

`Gap_Impact = (5 - authority_score) × cluster_weight`

Where `5 - authority_score` is the distance from perfect coverage, and `cluster_weight` reflects strategic importance.

Clusters with the highest Gap Impact scores receive the highest priority in the Action Priority Queue (Part 5 of output) — even if their raw authority score is not the lowest.

---

### ▸ FUNNEL-BASED AUTHORITY MODELING

Topical authority is not just about page count — it is about funnel structure. This advanced logic evaluates authority through the lens of funnel completeness, recognizing that a TOFU-heavy cluster and a balanced cluster with equal page counts have very different authority profiles.

**Funnel Authority Profile:**

For each cluster, construct a funnel authority profile:

| Profile Type          | Condition                                        | Authority Implication                                      |
|-----------------------|--------------------------------------------------|------------------------------------------------------------|
| **FULL FUNNEL**       | TOFU + MOFU + BOFU all represented             | Site can attract, nurture, and convert in this topic area  |
| **AWARENESS-ONLY**    | Only TOFU content present                        | Site attracts traffic but cannot convert it               |
| **CONVERSION-ONLY**   | Only BOFU content present                        | Site targets buyers but has no audience to convert        |
| **BRIDGE-MISSING**    | TOFU + BOFU but no MOFU                          | Drop-off risk — readers jump from awareness to decision without guidance |
| **EVALUATION-ONLY**   | Only MOFU content present                        | Consideration content without awareness entry or conversion path |

The funnel profile directly informs the missing element detection and action recommendations:
- AWARENESS-ONLY → prioritize BOFU then MOFU creation.
- CONVERSION-ONLY → prioritize TOFU then MOFU creation.
- BRIDGE-MISSING → create MOFU content to connect the existing TOFU and BOFU.

---

### ▸ ENTITY DEPTH VALIDATION

When the entity map is available, this advanced validation checks that the cluster's content covers the semantic field that Google's topic models expect to see for genuine expertise.

**Entity Validation Protocol:**

1. For each cluster, load the list of Critical entities from `entity-extraction.skill`.
2. For each published page in the cluster, check which Critical entities the page covers (from entity map coverage data).
3. Union the covered entities across all pages — this is the cluster's entity coverage footprint.
4. Calculate the coverage rate and identify which Critical entities are not covered by any page.

**Entity Coverage Impact on Authority:**

| Entity Coverage Rate | Impact on Authority Score |
|----------------------|---------------------------|
| ≥ 90%               | +0.5 modifier (positive)  |
| 80–89%              | No modifier               |
| 60–79%              | -0.5 modifier (negative)  |
| < 60%               | -1.0 modifier (negative)  |

Entity coverage modifiers are applied in addition to the standard modifiers in Step 3B. The entity depth validation can therefore be a significant factor in differentiating between clusters with similar page counts.

**Entity Gap Action:**

For each uncovered Critical entity, produce a specific mini-recommendation:

`"CREATE: A content section or dedicated page covering [entity name] as it relates to [cluster topic]. This entity is referenced by [N] of [total] top-ranking pages for this cluster's keywords — its absence is a detectable semantic gap."`

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate Cluster Mapping

Every cluster from `semantic-clustering.skill` must appear exactly once in the authority map. If the same cluster name appears twice (possible from memory data vs. current clustering) → merge the records, retaining the more current data. Log the merge.

### Rule 2 — No Double-Counting Pages Across Clusters

A published content page must contribute to the authority score of exactly one cluster — its primary cluster. Cross-referenced content (contributing 0.5 credit to a secondary cluster) must not be counted as a full contribution to both.

**Deduplication check:** After mapping all content to clusters, verify that the total of all content records assigned to clusters (full + half credits) does not exceed the total published content count. If it does → a double-counting error has occurred → identify the over-counted pages and correct.

### Rule 3 — No Double-Counting of Missing Elements

If a cluster is missing both a Pillar page AND a BOFU page, these are two distinct missing elements — each must be listed separately with its own action recommendation. However, the same physical content piece cannot be recommended as addressing both simultaneously (a pillar page is not a BOFU page and vice versa). Each missing element has its own required build action.

Exception: If a single well-structured pillar page would naturally include a BOFU section (e.g., a "CRM Selection Guide" pillar that includes a comparison table and CTA section) → note this as a single combined build that addresses both gaps. Do not split it into two separate actions.

### Rule 4 — Memory Deduplication

Before producing the authority map:
1. Query `memory-controller.skill` for a prior authority map for this domain.
2. If found and within freshness threshold AND no inventory or cluster changes detected → offer cached result.
3. If stale or changed → rebuild. Compare new scores against prior scores and flag any cluster whose score has changed by ≥1 point as SCORE CHANGE ALERT for user review.

### Rule 5 — No Duplicate Actions in the Priority Queue

Each specific action (e.g., "Build a BOFU comparison page for [Cluster Name] targeting '[keyword]'") may appear exactly once in the Action Priority Queue. If the same build action would address gaps in two clusters → assign to the primary cluster; note the secondary benefit.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Authority Mapping Behaviors

| Prohibited Behavior                                                                            | Reason                                                                           |
|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Recommending "create more content" for any cluster without specifying what content and why     | Generic recommendations are not actionable — they produce generic content        |
| Assigning a STRONG coverage score to a cluster with only TOFU pages                           | TOFU-only is not STRONG — it is MEDIUM at best                                  |
| Counting draft or planned pages toward an authority score                                       | Only published content represents real authority — drafts are intentions          |
| Assigning the same "Why It Matters" statement to multiple clusters                             | Every cluster's authority gap is specific — generic impact statements are not actionable |
| Marking a cluster as NONE-urgency when its score is 0                                          | A score of 0 cannot be NONE urgency — it is at minimum IMPORTANT                |
| Assigning a Pillar page credit to a page with a coverage score ≤ 2                            | A thin pillar is not a pillar — it fails the coverage threshold for hub status   |
| Projecting an authority score increase without specifying which element drives the increase    | Score projections must reference the specific build that produces the gain       |
| Ignoring entity coverage when entity data is available                                          | Entity validation is mandatory when the entity map exists                        |
| Producing an authority map that shows ALL clusters as HIGH urgency                             | Not all gaps are critical — inflated urgency produces no prioritization signal   |
| Using coverage score averages to mask the presence of thin pillar pages                        | Individual page scores matter — averaging hides quality problems                  |

### Required Authority Mapping Behaviors

- Every cluster must receive an authority score (0–5); no nulls.
- Every cluster with score < 5 must have at least one identified missing element.
- Every identified missing element must have a corresponding action recommendation.
- Every action recommendation must specify: content type, keyword, format, funnel stage, and projected impact.
- The domain authority score must be weighted — not a simple average.
- Draft and planned content must be visibly separated from published content in all records.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary cluster architecture input)                                   |
| **Receives**     | All cluster names, tiers, keyword lists, funnel balance, cluster strength, pillar topic labels |
| **Used for**     | Step 1 (mapping target — clusters to map content into), Step 2 (coverage classification criteria), Step 5 (gap urgency context) |

---

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary content data input)                                           |
| **Receives**     | All content records — URL, keyword, cluster, status, intent, funnel, coverage score        |
| **Used for**     | Step 1 (content-to-cluster mapping), Step 2 (coverage level classification), Step 3 (authority scoring) |
| **Critical field** | Status filter — only published records contribute to authority scores                    |

---

### ▸ entity-extraction.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Entity maps per cluster — Critical/Important entity lists                                   |
| **Used for**     | Step 4D (entity coverage detection), Step 3B (entity coverage modifiers), Advanced Logic (entity depth validation) |

---

### ▸ query-graph.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Node types, Hub Scores, dependency chains                                                   |
| **Used for**     | Step 3C (cluster weight for domain authority calculation), Advanced Logic (weighted gap impact), Step 5A (CRITICAL gap triggers for dependency chain clusters) |

---

### ▸ content-awareness.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Coverage map, freshness flags, CLUSTER FRESHNESS RISK data                                 |
| **Used for**     | Step 2C (freshness adjustment to coverage level), Step 3B (freshness-related modifiers)    |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before mapping, WRITE after output finalization                        |
| **Reads**        | Prior authority map, prior cluster scores, prior score history                              |
| **Writes**       | Complete authority map (all 5 parts), cluster scores, action queue                         |
| **Memory layers**| strategy-memory.skill (authority map), content-memory.skill (cluster-content mapping)     |
| **Timing**       | READ at pre-conditions. WRITE after Step 6 completes.                                      |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | authority-engine → roadmap-engine (primary strategic input)                                 |
| **Sends**        | Action Priority Queue (Part 5), full cluster authority table (Part 1), gap urgency classifications |
| **Critical field** | Action Priority Queue — this is the primary sequencing input for the content roadmap     |

---

### ▸ strategist-ai.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | authority-engine → strategist-ai (strategic intelligence consumer)                         |
| **Sends**        | Domain authority score, critical gap report, authority distribution analysis                |
| **Used for**     | Generating strategic recommendations and executive summaries of the authority state        |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                           |
|---------------------------------------------------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Content inventory is unavailable                                          | Primary input missing                                  | All clusters score 0 (MISSING). Flag: "Content inventory unavailable — all clusters treated as MISSING. Run content-inventory.skill before authority mapping." |
| A cluster from semantic-clustering has no corresponding content records   | Step 1 — mapping returns zero records for cluster      | Score = 0 (MISSING). This is a valid and important finding — it means the cluster is completely unaddressed. |
| Content records reference a cluster name not in the current architecture  | Step 1A — cluster name mismatch                        | Apply keyword-based fallback matching. If still no match → mark content as ORPHAN. Do not force-assign to a wrong cluster. |
| Orphan content volume exceeds 20% of inventory                           | Step 1D — orphan count > 20% threshold                | Flag: "HIGH ORPHAN RATE — [N]% of content has no cluster assignment. Cluster architecture may not reflect actual content coverage. Recommend: re-run semantic-clustering.skill." |
| Entity map is unavailable for entity coverage validation                  | Step 4D — entity-extraction data absent               | Skip entity validation. Note: "ENTITY VALIDATION SKIPPED — entity-extraction.skill data unavailable. Entity depth not factored into authority scores." |
| Query-graph data is unavailable for cluster weighting                    | Step 3C — query-graph input absent                    | Fall back to tier-based weighting (Pillar=2.0, Sub=1.5, Micro=1.0). Flag: "CLUSTER WEIGHT ESTIMATED — query-graph.skill data unavailable; tier-based weights applied." |
| A cluster's coverage level is ambiguous (borderline between two levels)  | Step 2 — criteria match is unclear                    | Apply the more conservative classification (lower level). Flag as NEEDS REVIEW. Surface to user: "Cluster [Name] has borderline coverage — manual review recommended." |
| All clusters score 0 (entire domain has no authority)                   | Step 3 — all scores = 0                               | This is a valid result for a new site. Flag as NEW DOMAIN — ALL GAPS. Produce full recommendations. Note: "No existing authority detected — all clusters require build-from-scratch action." |
| Score change alert fires for ≥50% of clusters                           | Rule 4 — memory dedup detects mass score changes      | Flag: "MAJOR AUTHORITY CHANGE — over half of cluster scores have changed by ≥1 point since last map. Possible causes: new content published, cluster restructuring, or content deletion." Surface for user review before dispatching to roadmap. |
| Memory controller is unavailable                                          | READ/WRITE queries return errors                       | Proceed without cached comparison. Flag: "Memory unavailable — score change detection skipped; prior authority map not available for comparison." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — COVERAGE CLASSIFICATION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
COVERAGE CLASSIFICATION CRITERIA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
COMPLETE (Score 5):
  ✅ Pillar page published (coverage score ≥ 3.5)
  ✅ 2+ supporting published pages (beyond pillar)
  ✅ TOFU + MOFU + BOFU all present
  ✅ Avg coverage score ≥ 3.5
  ✅ Entity coverage ≥ 80% (if entity map available)

STRONG (Score 4):
  ✅ Pillar page published
  ✅ 2+ supporting pages published
  ✅ 2 of 3 funnel stages present
  ✅ Avg coverage score ≥ 3.0
  ⚠️ ONE element missing (BOFU, entity coverage, or one funnel stage)

MEDIUM (Score 3):
  ✅ 2+ published pages
  ✅ 2 funnel stages represented
  ❌ No pillar OR pillar is thin (score ≤ 2)
  Avg coverage: 2–3

WEAK (Score 2):
  1–2 published pages, OR
  Only 1 funnel stage, OR
  Pillar only with no supporting pages
  Avg coverage ≤ 2

MINIMAL (Score 1):
  Exactly 1 published page
  Coverage score = 1 (Thin)
  No funnel diversity

MISSING (Score 0):
  Zero published pages

DOWNGRADE TRIGGERS:
  CLUSTER FRESHNESS RISK → -1 level
  Duplicate content alert → -0.5 score
  No internal links in cluster → -0.5 score
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — ACTION RECOMMENDATION QUALITY CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
ACTION RECOMMENDATION QUALITY CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Before finalizing any action recommendation, verify:

□ WHAT TO BUILD specifies:
  - Content type (pillar / BOFU page / supporting article / FAQ)?
  - Target primary keyword?
  - Intended format (guide / comparison / tutorial / landing page)?
  - Target funnel stage (TOFU / MOFU / BOFU)?

□ WHY IT MATTERS references:
  - Current authority score?
  - Specific business consequence of the missing element?
  - NOT a generic SEO principle?

□ EXPECTED IMPACT states:
  - Current score (before building)?
  - Projected score (after building)?
  - Which specific missing element drives the score change?

□ Is this recommendation UNIQUE to this cluster?
  Could it apply word-for-word to another cluster without modification?
  If YES → add the cluster-specific detail that makes it distinct.

□ PROHIBITED PHRASES — none of these may appear:
  ✗ "create more content"
  ✗ "improve content quality"
  ✗ "write a blog post"
  ✗ "add a page"
  ✗ "this will help rankings"
  ✗ "this is important for SEO"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — AUTHORITY SCORE CALIBRATION EXAMPLES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
AUTHORITY SCORE CALIBRATION EXAMPLES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Score 5 — COMPLETE:
  Cluster: "CRM Software Selection"
  Pages: 1 pillar (score 4) + 3 supporting (scores 3, 4, 4)
  Funnel: TOFU (2 pages) + MOFU (1 page) + BOFU (1 page)
  Entity coverage: 85% of critical entities
  All modifiers: +0.5 (entity ≥ 80%), no negatives
  Final: 5 + 0.5 → capped at 5.0

Score 4 — STRONG:
  Cluster: "CRM Implementation Guide"
  Pages: 1 pillar (score 4) + 2 supporting (scores 3, 3)
  Funnel: TOFU (2 pages) + MOFU (1 page), BOFU = 0
  Entity coverage: 75% (no modifier)
  Modifiers: -0.5 (no BOFU)
  Final: 4.0 + 0 - 0.5 = 3.5 → rounds to 4 (STRONG still justified)

Score 3 — MEDIUM:
  Cluster: "CRM for Small Business"
  Pages: 2 published (both TOFU, both score 3)
  Funnel: TOFU only — no MOFU or BOFU
  No pillar detected
  Modifiers: -0.5 (no pillar + cluster_strength ≥ 70)
  Final: 3 - 0.5 = 2.5 → rounds to 3 (MEDIUM)

Score 2 — WEAK:
  Cluster: "CRM Automation"
  Pages: 1 published (TOFU, score 2)
  Funnel: TOFU only
  No pillar
  Freshness: AGING but not STALE
  Final: 2 (no modifiers triggered)

Score 1 — MINIMAL:
  Cluster: "CRM Reporting"
  Pages: 1 published (score 1 — Thin)
  Funnel: TOFU only, thin single page
  Final: 1 (base; thin page = minimal score)

Score 0 — MISSING:
  Cluster: "CRM Data Migration"
  Pages: 0 published (2 planned, 1 draft — not counted)
  Final: 0 (no published content = no authority)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: analysis/authority-engine.skill*
*Nexus SEO Operating System — Version 1.0.0*
