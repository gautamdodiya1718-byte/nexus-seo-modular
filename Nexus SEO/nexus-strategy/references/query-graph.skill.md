# SKILL: analysis/query-graph.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Analysis / Strategic Architecture
# EXECUTION PRIORITY: POST-CLUSTERING — Runs after semantic-clustering.skill finalizes all cluster assignments.
# POSITION: Strategic map layer. Transforms cluster architecture into a connected topic graph that drives content sequencing, authority flow, and expansion strategy.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **strategic topic map engine** of the Nexus system.

Where `semantic-clustering.skill` organizes keywords into named topic clusters, this skill builds the graph that connects those clusters into a living strategic architecture — a system of relationships that reveals which topics are central to topical authority, how content should flow from awareness to conversion, which clusters are isolated and need bridging, and which expansion paths offer the highest strategic return.

A cluster architecture without a relationship graph is a list. A relationship graph transforms that list into a system — one where building content in the right clusters, in the right order, along the right paths, compounds the authority of every piece already created. This skill designs those paths.

The output of this skill is the **strategic map** that every downstream planning and execution skill consults. The roadmap engine uses it to sequence content production. The authority engine uses it to design the internal linking architecture. The internal linking skill uses it to identify which pages should connect to which. No content strategy is complete without understanding how its clusters relate to each other.

### Primary Responsibilities

| Responsibility                         | Description                                                                                                           |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Node Creation**                      | Convert every cluster into a structured graph node with full metadata                                                 |
| **Edge Building**                      | Define relationships between cluster nodes — based on shared entities, related problems, user journey progression     |
| **Edge Strength Classification**       | Score every connection as STRONG, MODERATE, or WEAK — no connection may be undefined                                 |
| **Hub Detection**                      | Identify the most strategically central clusters using a PageRank-style connectivity and funnel weight calculation     |
| **Orphan Detection**                   | Find clusters with no connections — flag as standalone or mis-clustered                                               |
| **Expansion Path Definition**          | For every pillar node, define what topics should branch from it, what is logically next, and what is currently missing |
| **Priority Scoring**                   | Score every expansion path by intent stage, gap existence, and competition level                                      |
| **Funnel Progression Modeling**        | Map the searcher journey through the graph — from TOFU entry to BOFU conversion — cluster by cluster                  |
| **Semantic Dependency Mapping**        | Identify which clusters REQUIRE other clusters to exist before they can be meaningfully built                         |
| **Topic Graph Production**             | Produce a complete, structured text-format topic graph with all nodes, edges, hubs, and paths                        |

### What This Skill Is NOT

- It is **not** a keyword researcher. It works with clusters — not individual keywords.
- It is **not** a content generator. It designs the strategic architecture — it does not produce content.
- It is **not** a link builder. It defines the structural authority flow — `internal-linking.skill` implements the actual link recommendations.
- It is **not** a flat list of topics. The output is always a connected graph — relationships are as important as nodes.
- It is **not** a competitor analyzer. It focuses entirely on the strategic relationship between the site's own topic clusters.

### The Foundational Principle of This Skill

> A topic graph is not a visualization exercise.
> It is a **dependency map, an authority flow model, and a content sequencing engine** simultaneously.
>
> Every edge in the graph represents a real relationship between two topics —
> a relationship that a reader would naturally traverse, that Google's algorithms use to evaluate topical relevance,
> and that the content team must understand to produce work that compounds rather than competes.
>
> No edge is drawn for aesthetic reasons.
> No connection is implied — every edge has a named relationship type and a strength score.
> An undefined connection between two nodes is never assumed to exist.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input — Cluster Architecture

The complete cluster architecture output from `semantic-clustering.skill`, including all parts:

| Field                      | Description                                                                 | Required |
|----------------------------|-----------------------------------------------------------------------------|----------|
| `cluster_id`               | Unique identifier (C-P01, C-S01, C-M01, etc.)                             | YES      |
| `cluster_name`             | Human-readable topic name                                                   | YES      |
| `tier`                     | Pillar / Sub / Micro                                                         | YES      |
| `parent_cluster`           | Parent cluster ID (ROOT for pillar clusters)                               | YES      |
| `keywords`                 | Full keyword list with intent, funnel, and TF-IDF weight per keyword       | YES      |
| `dominant_intent`          | The most prevalent intent type in the cluster                              | YES      |
| `funnel_balance`           | FULL / PARTIAL / MINIMAL — with specific missing stages noted              | YES      |
| `cluster_strength`         | 0–100 strength score from semantic-clustering.skill                        | YES      |
| `pillar_topic`             | 2–5 word pillar topic label (for pillar clusters)                          | SOFT     |
| `description`              | One-sentence cluster description                                            | YES      |
| `cross_references`         | Related clusters identified during boundary validation                     | OPTIONAL |

### Optional Inputs

| Field                          | Description                                                             | Used For                                                        |
|--------------------------------|-------------------------------------------------------------------------|-----------------------------------------------------------------|
| `entity_map`                   | Entity intelligence from `entity-extraction.skill`                     | Edge building — shared entity detection (Step 2)               |
| `competitor_analysis`          | Gap and opportunity data from `competitor-analysis.skill`              | Priority scoring — gap existence and competition level (Step 6) |
| `content_inventory`            | Existing content records from `content-inventory.skill`               | Identifying which nodes already have content vs. which are empty |
| `serp_intelligence`            | SERP context per keyword/cluster from `serp-intelligence.skill`       | Competition level assessment for priority scoring               |
| `existing_graph`               | Prior graph state from `memory-controller.skill`                      | Delta-updating existing graph rather than rebuilding from scratch |

### Input Pre-Conditions

1. **Are all clusters fully formed?** Every cluster must have all required fields populated. If any cluster is missing `keywords`, `funnel_balance`, or `dominant_intent` → request completion from `semantic-clustering.skill` before proceeding.
2. **Minimum cluster count:** If fewer than 3 clusters → halt. A graph with fewer than 3 nodes cannot produce meaningful relationships.
3. **Is an entity map available?** Entity data significantly enriches edge building. If absent → proceed with structure-based edge detection only; flag output as ENTITY-ENRICHMENT ABSENT.
4. **Is an existing graph in memory?** → Query `memory-controller.skill`. If found and within freshness threshold (14 days) and cluster architecture has not changed → offer delta-update mode rather than full rebuild.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Topic Graph Report

```
NEXUS TOPIC RELATIONSHIP GRAPH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain / Topic Scope    : [domain or topic]
Total Nodes             : [count — must equal total clusters]
Total Edges             : [count]
  Strong Edges          : [count]
  Moderate Edges        : [count]
  Weak Edges            : [count]
Hub Nodes               : [count]
Orphan Nodes            : [count — target: 0]
Expansion Paths         : [count]
Entity Enrichment       : [PRESENT / ABSENT]
Generated By            : analysis/query-graph.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — NODE REGISTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Node ID | Cluster Name                | Tier    | Keywords | Dominant Intent | Funnel Balance | Strength | Hub Score | Node Type         |
|---------|-----------------------------|---------|----------|-----------------|----------------|----------|-----------|-------------------|
| C-P01   | [Name]                      | Pillar  | [N]      | Commercial      | FULL           | [0–100]  | [0–100]   | PRIMARY HUB       |
| C-P02   | [Name]                      | Pillar  | [N]      | Informational   | PARTIAL        | [0–100]  | [0–100]   | HUB               |
| C-S01   | [Name]                      | Sub     | [N]      | Informational   | FULL           | [0–100]  | [0–100]   | CONNECTOR         |
| C-M01   | [Name]                      | Micro   | [N]      | Transactional   | MINIMAL        | [0–100]  | [0–100]   | LEAF              |
| C-S03   | [Name]                      | Sub     | [N]      | Commercial      | PARTIAL        | [0–100]  | [0–100]   | ORPHAN — FLAG     |
...

NODE TYPE LEGEND:
  PRIMARY HUB : Highest hub score; central authority anchor; pillar of pillars
  HUB         : High connectivity + high funnel coverage; pillar cluster
  CONNECTOR   : Links pillar nodes to micro nodes; bridges topic areas
  LEAF        : Terminal node; no outgoing connections; highly specific topic
  ORPHAN      : No connections detected; requires review

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — EDGE REGISTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Edge ID | From Node | To Node | Relationship Type        | Strength  | Direction    | Rationale                                          |
|---------|-----------|---------|--------------------------|-----------|--------------|-----------------------------------------------------|
| E-001   | C-P01     | C-S01   | PARENT-CHILD             | STRONG    | BIDIRECTIONAL| C-S01 is a focused sub-topic of C-P01              |
| E-002   | C-P01     | C-P02   | PREREQUISITE             | STRONG    | DIRECTED →   | Users must understand C-P01 before C-P02           |
| E-003   | C-S01     | C-S02   | SIBLING                  | MODERATE  | BIDIRECTIONAL| Both are sub-clusters of same pillar; related problems |
| E-004   | C-P01     | C-P03   | FUNNEL FLOW              | STRONG    | DIRECTED →   | C-P01 TOFU content feeds C-P03 MOFU content        |
| E-005   | C-M01     | C-M02   | ENTITY-SHARED            | MODERATE  | BIDIRECTIONAL| Both clusters share [entity name] as critical entity|
| E-006   | C-S01     | C-P02   | CONTEXTUAL               | WEAK      | BIDIRECTIONAL| Adjacent topic areas; user journey may cross over   |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — TOPIC GRAPH (TEXT FORMAT)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[PRIMARY HUB] [C-P01] [Name] (Hub Score: N | Strength: N)
  │
  ├──STRONG──▶ [C-S01] [Name] (Connector | Funnel: FULL)
  │               │
  │               ├──MODERATE──▶ [C-M01] [Name] (Leaf | BOFU)
  │               └──MODERATE──▶ [C-M02] [Name] (Leaf | BOFU)
  │
  ├──STRONG──▶ [C-S02] [Name] (Connector | Funnel: PARTIAL — BOFU missing)
  │               └──WEAK──▶ [C-M03] [Name] (Leaf | TOFU)
  │
  └──FUNNEL──▶ [C-P02] [Name] (Hub | Funnel: FULL)
                  │
                  └──STRONG──▶ [C-S03] [Name] (Connector | Funnel: FULL)
                                  └──MODERATE──▶ [C-M04] [Name] (Leaf | MOFU)

[ORPHAN] [C-S04] [Name] — !! NO CONNECTIONS DETECTED !! — Review cluster placement

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — HUB ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Rank | Node ID | Cluster Name         | Hub Score | Connections | Funnel | Authority Role                                     |
|------|---------|----------------------|-----------|-------------|--------|----------------------------------------------------|
| 1    | C-P01   | [Name]               | [0–100]   | [N edges]   | FULL   | PRIMARY HUB — central authority anchor             |
| 2    | C-P02   | [Name]               | [0–100]   | [N edges]   | FULL   | SECONDARY HUB — major content territory            |
| 3    | C-S01   | [Name]               | [0–100]   | [N edges]   | FULL   | CONNECTOR HUB — bridges pillar territories         |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — EXPANSION PATHS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH PILLAR NODE]

PILLAR: [Cluster Name] (C-P0N)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Existing Branches     : [list of directly connected clusters]
Missing Branches      : [list of topics that logically should branch from this pillar but don't exist yet]
Logical Next Topics   : [what should be built next — in priority order]
Funnel Gaps in Tree   : [which funnel stages are absent from this pillar's entire subtree]

Expansion Recommendations (in priority order):
  1. [PRIORITY: HIGH] — [Topic] — [Reason: gap + intent + connection logic]
  2. [PRIORITY: HIGH] — [Topic] — [Reason]
  3. [PRIORITY: MEDIUM] — [Topic] — [Reason]
  4. [PRIORITY: LOW] — [Topic] — [Reason]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 6 — PRIORITY SCORING TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Path ID | From Node | Expansion Topic        | Intent | Gap Exists | Competition | Priority Score | Priority Label |
|---------|-----------|------------------------|--------|------------|-------------|----------------|----------------|
| P-001   | C-P01     | [topic]                | BOFU   | YES        | LOW         | [0–100]        | HIGH           |
| P-002   | C-P01     | [topic]                | MOFU   | YES        | MEDIUM      | [0–100]        | HIGH           |
| P-003   | C-P02     | [topic]                | MOFU   | PARTIAL    | MEDIUM      | [0–100]        | MEDIUM         |
| P-004   | C-P01     | [topic]                | TOFU   | NO         | HIGH        | [0–100]        | LOW            |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 7 — FUNNEL PROGRESSION MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

AWARENESS → CONSIDERATION → DECISION
(TOFU Entry)  (MOFU Bridge)  (BOFU Conversion)

Searcher Journey Path 1:
  [TOFU Cluster] → [MOFU Cluster] → [BOFU Cluster]
  Entry point: "[TOFU keyword example]"
  Consideration: "[MOFU keyword example]"
  Decision: "[BOFU keyword example]"
  Path Completeness: [COMPLETE / PARTIAL — gap at: stage]

Searcher Journey Path 2:
  [TOFU Cluster] → [MOFU Cluster] → !! BOFU GAP !!
  Missing node: [what BOFU cluster should be created]

...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 8 — ORPHAN RESOLUTION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Orphan Node | Cluster Name         | Reason Isolated                    | Resolution Recommendation                         |
|-------------|----------------------|------------------------------------|---------------------------------------------------|
| C-S04       | [Name]               | No shared entities; no funnel flow | Re-evaluate: may belong under [suggested parent]  |
| C-M05       | [Name]               | Too specific; no natural bridge    | Merge with [adjacent cluster] or expand keywords  |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : roadmap-engine.skill
                         authority-engine.skill
                         internal-linking.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — NODE CREATION

Convert every cluster from the semantic clustering output into a structured graph node. A node is the fundamental unit of the topic graph — it represents a topic territory and carries the metadata needed for all subsequent graph operations.

**Phase 1A — Node Data Structure**

For each cluster, construct a node record with the following fields:

```
NODE RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
node_id             : [cluster_id — carried from semantic-clustering.skill]
cluster_name        : [cluster name]
tier                : [Pillar / Sub / Micro]
parent_node         : [parent cluster_id or ROOT]
keyword_count       : [total keywords in cluster]
dominant_intent     : [Informational / Commercial / Transactional]
funnel_coverage     : [FULL / PARTIAL / MINIMAL]
funnel_stages_present: [TOFU: Y/N | MOFU: Y/N | BOFU: Y/N]
cluster_strength    : [0–100 score from semantic-clustering]
pillar_topic        : [2–5 word label or — for non-pillars]
description         : [one-sentence cluster description]
entity_set          : [list of Critical/Important entities if entity map available]
existing_content    : [Y/N — does published content exist for this cluster?]
connections         : [] [empty list — populated in Step 2]
hub_score           : 0 [initialized; calculated in Step 3]
node_type           : UNCLASSIFIED [assigned in Step 3]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 1B — Node Content Status Flag**

If `content_inventory` data is available, flag each node with its content status:

| Content Status         | Condition                                                                  |
|------------------------|----------------------------------------------------------------------------|
| `CONTENT_RICH`         | 3+ published pages targeting this cluster with coverage score ≥3          |
| `CONTENT_PRESENT`      | 1–2 published pages OR pages with coverage score 1–2                      |
| `CONTENT_PLANNED`      | Draft or planned content exists; no published content yet                 |
| `CONTENT_EMPTY`        | No published, draft, or planned content for this cluster                   |

Content status influences expansion path priority scoring in Step 6 — CONTENT_EMPTY nodes with strategic connections receive the highest expansion priority.

**Phase 1C — Node Registry Validation**

After all nodes are created:
1. Verify total node count equals total cluster count from `semantic-clustering.skill`.
2. Verify every node has a unique `node_id`.
3. Verify every Sub or Micro node has a valid `parent_node` reference.
4. Flag any node missing required fields for reconstruction before proceeding.

**Phase 1D — Entity Set Loading**

If `entity-extraction.skill` output is available:
- Load the entity map for each cluster.
- Populate each node's `entity_set` with all Critical and Important entities that belong to that cluster's topic.
- Entity sets are used in Step 2D for entity-based edge building.

---

### ▸ STEP 2 — EDGE BUILDING

Define the relationships between nodes. Edges are the most strategically important output of this skill — they represent real semantic, structural, or journey-based connections between topic territories.

**The Fundamental Edge Rule:**

> An edge is only drawn when a real relationship can be named.
> The relationship type must be stated explicitly for every edge.
> "These topics feel related" is not a valid reason to create an edge.
> Every edge must have: FROM node, TO node, RELATIONSHIP TYPE, STRENGTH, DIRECTION, RATIONALE.

**Phase 2A — Relationship Type Taxonomy**

| Relationship Type  | Symbol | Definition                                                                                   |
|--------------------|--------|----------------------------------------------------------------------------------------------|
| **PARENT-CHILD**   | P→C    | A hierarchical relationship where one cluster is a sub-topic of another                     |
| **PREREQUISITE**   | A→B    | Content in A must exist (or be understood) before content in B makes full sense             |
| **FUNNEL FLOW**    | T→M→B  | A TOFU cluster naturally feeds into a MOFU cluster; MOFU feeds into BOFU                   |
| **SIBLING**        | A↔B    | Two clusters at the same tier under the same parent — related but distinct problem spaces    |
| **ENTITY-SHARED**  | A↔B    | Both clusters share one or more Critical entities from the entity map                       |
| **SEQUENTIAL**     | A→B    | Topic B logically follows topic A in a learner's journey or process                        |
| **COMPLEMENTARY**  | A↔B    | Two topics that solve related but distinct problems; often consumed together                |
| **CONTEXTUAL**     | A↔B    | Topics that frequently appear together in the same SERP or content ecosystem               |
| **COMPETITIVE**    | A↔B    | Two clusters that target overlapping SERPs — flagged for differentiation strategy         |
| **AUTHORITY-FLOW** | A→B    | A high-authority cluster can pass link equity to a weaker but strategically important cluster |

**Phase 2B — Structural Edge Building (Hierarchy-Derived)**

The first set of edges is derived directly from the cluster hierarchy:

1. **For every Sub-cluster:** draw a PARENT-CHILD edge to its Pillar parent.
   - Direction: BIDIRECTIONAL (the pillar gives the sub-cluster context; the sub-cluster proves the pillar's depth).
   - Strength: STRONG (structural edges are always STRONG).

2. **For every Micro-cluster:** draw a PARENT-CHILD edge to its Sub-cluster parent.
   - Direction: BIDIRECTIONAL.
   - Strength: STRONG.

3. **For every pair of Sub-clusters under the same Pillar:** draw a SIBLING edge.
   - Direction: BIDIRECTIONAL.
   - Strength: MODERATE (siblings are related through their shared parent, not through their own topic overlap).

4. **For clusters identified as cross-references in `semantic-clustering.skill`:** draw a CONTEXTUAL edge.
   - Direction: BIDIRECTIONAL.
   - Strength: MODERATE.

**Phase 2C — Funnel Flow Edge Building**

Funnel flow edges represent the reader journey — they connect clusters that a user would naturally traverse as they move from awareness to conversion.

**Funnel Flow Detection Protocol:**

For each pair of clusters in the same thematic territory:

1. **TOFU → MOFU:** If Cluster A has dominant TOFU intent and Cluster B has dominant MOFU intent AND both cover the same broad topic territory → draw a FUNNEL FLOW edge from A to B.
   - Direction: DIRECTED → (information flows from awareness to evaluation).
   - Strength: STRONG if the clusters share the same pillar; MODERATE if they are in adjacent pillars.

2. **MOFU → BOFU:** If Cluster A has dominant MOFU intent and Cluster B has dominant BOFU intent AND both cover the same topic territory → draw a FUNNEL FLOW edge from A to B.
   - Direction: DIRECTED →.
   - Strength: STRONG if same pillar; MODERATE if adjacent.

3. **TOFU → BOFU (Skip):** If a site has TOFU and BOFU coverage of a topic but no MOFU → draw a DIRECTED edge from TOFU to BOFU but flag it as: FUNNEL GAP — MOFU MISSING. This edge marks where a missing cluster should be created.

**Funnel Flow Edge Rationale Format:**

`"[Cluster A] serves searchers discovering [topic]; [Cluster B] serves the same audience when they are ready to evaluate solutions. Natural progression from awareness to evaluation."`

**Phase 2D — Entity-Based Edge Building**

If the entity map is available from `entity-extraction.skill`:

1. For each pair of clusters, compare their `entity_set` fields.
2. If two clusters share ≥1 Critical entity OR ≥2 Important entities → draw an ENTITY-SHARED edge.
3. Strength classification by entity overlap:

| Shared Entities                              | Edge Strength |
|----------------------------------------------|---------------|
| ≥2 Critical entities shared                  | STRONG        |
| 1 Critical entity OR ≥3 Important entities   | MODERATE      |
| 1–2 Important entities shared                | WEAK          |
| No entity overlap                            | No edge        |

**Entity Edge Rationale Format:**

`"Both clusters require understanding of [entity name] — content in either cluster should reference and link to the other for semantic coherence."`

**Phase 2E — Prerequisite Edge Building**

Identify clusters where understanding the content of one cluster is prerequisite to understanding the content of another:

**Prerequisite Detection Rules:**

| Condition                                                                              | Prerequisite Edge                                     |
|----------------------------------------------------------------------------------------|-------------------------------------------------------|
| Cluster B's keywords imply knowledge of Cluster A (advanced topics require basics)    | A → B (PREREQUISITE, DIRECTED)                       |
| Cluster B's content would cite Cluster A's content as background or foundation        | A → B (PREREQUISITE, DIRECTED)                       |
| Cluster A defines a term that Cluster B uses extensively                               | A → B (PREREQUISITE, DIRECTED)                       |

Example: A cluster covering "CRM data migration" has a prerequisite on a cluster covering "CRM implementation basics." You cannot understand data migration without first understanding CRM setup.

**Phase 2F — Competitive Edge Detection**

Identify clusters that would compete for overlapping SERP positions — not to remove them, but to flag them for differentiation planning:

1. If two clusters have ≥50% keyword intent overlap AND target the same audience → draw a COMPETITIVE edge.
2. COMPETITIVE edges are always WEAK (this is not a content flow relationship — it is a risk flag).
3. Competitive edges are reported to `deduplication-engine.skill` and `authority-engine.skill` for resolution.

**Phase 2G — Edge Deduplication**

After all edge types are built:
1. Remove exact duplicate edges (same FROM, same TO, same relationship type).
2. If two different relationship types exist between the same pair → keep the more specific one; note the other as secondary.
3. No circular meaningless connections: if A→B and B→A both exist as DIRECTED edges of the same type → convert to BIDIRECTIONAL. If the bidirectionality is meaningful only in one direction → keep the directed form.

---

### ▸ STEP 3 — HUB DETECTION

Identify the most strategically central nodes — the clusters that function as the structural anchors of the entire topic graph.

**Phase 3A — Hub Score Calculation**

The Hub Score is a composite metric that reflects a node's centrality in the graph — how much it influences the overall topic territory and how valuable it is as an authority anchor.

**Hub Score Formula:**

```
Hub_Score = (Connection_Score × 0.40)
          + (Funnel_Score × 0.25)
          + (Cluster_Strength × 0.20)
          + (Content_Potential_Score × 0.15)
```

**Connection Score (40% weight):**

| Metric                                    | Scoring Rule                                              |
|-------------------------------------------|-----------------------------------------------------------|
| Total edges (any type, any strength)      | `(node_edges / max_edges_any_node) × 50`                 |
| STRONG edges specifically                 | `(strong_edges / total_edges) × 50`                      |
| Combined Connection Score                 | `(above two scores summed) / 2, normalized to 0–100`     |

**Funnel Score (25% weight):**

| Funnel Coverage at Node and Its Subtree  | Score |
|------------------------------------------|-------|
| FULL funnel coverage at node AND subtree | 100   |
| FULL funnel at node; gaps in subtree     | 75    |
| PARTIAL funnel at node; full in subtree  | 60    |
| PARTIAL funnel at node; gaps in subtree  | 40    |
| MINIMAL funnel at node                   | 20    |

**Cluster Strength (20% weight):**
Direct import of the `cluster_strength` score from `semantic-clustering.skill` (already on 0–100 scale).

**Content Potential Score (15% weight):**

| Condition                                              | Score |
|--------------------------------------------------------|-------|
| Node is CONTENT_EMPTY with HIGH cluster strength       | 100 (highest potential — maximum impact if built) |
| Node is CONTENT_PLANNED                                | 70   |
| Node is CONTENT_PRESENT with coverage gaps             | 50   |
| Node is CONTENT_RICH with full coverage                | 20 (low potential — already built)   |

**Phase 3B — Node Type Assignment**

After Hub Scores are calculated for all nodes, assign node types:

| Hub Score  | Tier       | Node Type     |
|------------|------------|---------------|
| 85–100     | Pillar     | PRIMARY HUB   |
| 65–84      | Pillar     | HUB           |
| 45–64      | Sub        | CONNECTOR     |
| 25–44      | Any        | LEAF          |
| Any score  | Any tier, 0 connections | ORPHAN |

**Phase 3C — Primary Hub Identification**

The Primary Hub is the single most strategically central node in the entire graph — the topic territory that should be built first, that serves as the anchor for all other content, and that provides the most authority compounding when fully covered.

There must be exactly one PRIMARY HUB per graph (or one per disconnected sub-graph, if the graph has disconnected components).

If two nodes tie for the highest Hub Score → prefer the node with FULL funnel coverage. If still tied → prefer the node in the broader topic territory. If still tied → flag both as PRIMARY HUB candidates and request user selection.

**Phase 3D — Hub Score Output**

Report every node's Hub Score in the Node Registry (Part 1 of output) and rank all nodes by Hub Score in the Hub Analysis (Part 4 of output).

---

### ▸ STEP 4 — ORPHAN DETECTION

Identify clusters that have no connections to the rest of the graph. Orphan nodes are either genuinely standalone topics with no relationship to the rest of the site's content territory, or they are mis-clustered keywords that belong elsewhere.

**Phase 4A — Orphan Identification**

A node is an orphan if:
- It has zero edges of any type after all edge-building phases are complete (Steps 2A–2F).
- OR it has only WEAK edges AND those weak edges connect only to other low-strength nodes (isolated weak sub-graph).

**Phase 4B — Orphan Cause Analysis**

For each orphan node, diagnose the likely cause:

| Cause Type                                | Condition                                                                      | Recommended Action                                                |
|-------------------------------------------|--------------------------------------------------------------------------------|-------------------------------------------------------------------|
| **SCOPE MISMATCH**                        | The cluster covers a topic genuinely outside the site's core domain           | Flag as OUT OF SCOPE — recommend user review; may need to be excluded from the content strategy |
| **MIS-CLUSTERED KEYWORDS**               | Keywords in this cluster share more semantic alignment with another cluster    | Flag as MIS-CLUSTERED — recommend merging with the most semantically adjacent cluster |
| **BRIDGE CLUSTER NEEDED**                | The cluster covers a valid topic but lacks a connecting cluster to the rest   | Flag as BRIDGE NEEDED — recommend creating an intermediate cluster to connect this orphan to the graph |
| **HIGHLY SPECIFIC STANDALONE**           | The cluster is a valid, specific niche topic with no natural graph connections | Flag as STANDALONE — keep in architecture; plan as an isolated authority piece; no internal links expected |
| **UNDERDEVELOPED NODE**                  | The cluster has too few keywords (< 3) to establish meaningful connections    | Flag as UNDERDEVELOPED — recommend keyword expansion before re-running graph |

**Phase 4C — Orphan Resolution Protocol**

For each orphan, produce a resolution recommendation:

```
ORPHAN RESOLUTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Orphan Node     : [Node ID] — [Cluster Name]
Cause           : [cause type]
Most Adjacent   : [nearest non-orphan node by topic analysis]
Similarity Score: [0–100 — how close is the orphan to the adjacent node?]
Recommendation  :
  Option A: [primary resolution action]
  Option B: [alternative resolution action]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

After recommendations are produced — do NOT automatically merge or move orphans. Surface the options to `roadmap-engine.skill` and the user for decision. Orphans remain in the graph as flagged ORPHAN nodes until resolved.

---

### ▸ STEP 5 — EXPANSION PATHS

For every Pillar node (Hub or Primary Hub), define the strategic expansion paths — the topics that should branch from this pillar, the content that is logically missing, and the sequence in which to build out the pillar's subtree.

**Phase 5A — Existing Branch Audit**

For each Pillar node:
1. List all nodes currently connected to this pillar (directly or through its sub-cluster chain).
2. List all funnel stages currently covered in this pillar's entire subtree.
3. Identify which funnel stages are missing from the subtree.
4. Identify which thematic sub-topics of this pillar's domain have no current node.

**Phase 5B — Missing Branch Identification**

Missing branches are thematic sub-topics of the pillar's topic territory that have no current cluster in the architecture. They are identified through:

1. **Semantic field analysis:** What sub-topics would a comprehensive coverage of this pillar's Pillar Topic naturally include? Compare this semantic field against the existing sub-cluster and micro-cluster list. Absent sub-topics are MISSING BRANCHES.

2. **Entity gap signals:** If the entity map shows Critical entities for this pillar's topic area that have no current cluster targeting them → those entities represent MISSING BRANCH candidates.

3. **Funnel gap analysis:** If the pillar's subtree has no BOFU nodes → at least one MISSING BRANCH is the BOFU conversion topic for this pillar. If no MOFU exists → the comparison/evaluation topic is missing.

**Missing Branch Format:**

```
MISSING BRANCH DETECTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Pillar          : [Cluster Name]
Missing Sub-topic: [topic description]
Missing Intent  : [TOFU / MOFU / BOFU]
Gap Type        : [SEMANTIC GAP / ENTITY GAP / FUNNEL GAP]
Evidence        : [specific evidence for why this branch is expected]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 5C — Logical Next Topics Identification**

Beyond missing branches, identify what should be built NEXT in the pillar's development:

**Logical Next Topic Criteria:**

| Criterion                                                               | Weight |
|-------------------------------------------------------------------------|--------|
| Topic is connected to the pillar's highest-strength existing node       | HIGH   |
| Topic fills a missing funnel stage in the pillar's tree                 | HIGH   |
| Topic is a prerequisite for other planned topics in the pillar          | MEDIUM |
| Topic creates a new BOFU or MOFU node where only TOFU exists            | HIGH   |
| Topic enables a new internal linking opportunity to a high-value node   | MEDIUM |

Order logical next topics by the sum of criteria weights that apply.

**Phase 5D — Expansion Path Sequencing**

For each pillar, produce an ordered expansion sequence:

```
EXPANSION SEQUENCE: [Pillar Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Current State   : [N sub-clusters | N micro-clusters | Funnel: FULL/PARTIAL]
Priority 1      : [topic] — [reason: gap type + intent + connection logic]
Priority 2      : [topic] — [reason]
Priority 3      : [topic] — [reason]
Priority 4      : [topic] — [reason]
...
After Priority N: [topic] — [reason — lower priority; build after high-priority items complete]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 6 — PRIORITY SCORING

Score every expansion path produced in Step 5 to produce a cross-pillar ranked list of what to build next, ordered by strategic return.

**Phase 6A — Priority Scoring Formula**

```
Priority_Score = (Intent_Score × 0.35)
               + (Gap_Score × 0.35)
               + (Competition_Score × 0.20)
               + (Connection_Value × 0.10)
```

**Intent Score (35% weight):**

| Intent Stage of Expansion Topic | Intent Score |
|---------------------------------|--------------|
| BOFU (Transactional)            | 100          |
| MOFU (Commercial)               | 70           |
| TOFU (Informational)            | 40           |
| Mixed / Unclear                 | 30           |

**Gap Score (35% weight):**

| Gap Existence                                   | Gap Score |
|-------------------------------------------------|-----------|
| Confirmed gap — no competitor coverage          | 100       |
| Confirmed gap — weak competitor coverage        | 80        |
| Partial gap — some competitors cover this weakly| 60        |
| No gap — but existing content is thin (<score 2)| 40        |
| No gap — topic is well-covered by competitors   | 10        |

If `competitor-analysis.skill` data is unavailable → use `content_inventory` to assess whether the site's own coverage leaves a gap:
- CONTENT_EMPTY → Gap Score: 90 (presumed gap without competitor data).
- CONTENT_PRESENT (thin) → Gap Score: 50.
- CONTENT_RICH → Gap Score: 10.

**Competition Score (20% weight):**

| Competition Level           | Competition Score |
|-----------------------------|-------------------|
| LOW (niche sites dominating SERP) | 100         |
| MEDIUM (mix of brands and niches) | 60          |
| HIGH (established brands dominate) | 20         |
| Unknown                     | 50 (neutral assumption)|

**Connection Value (10% weight):**

| Connection Type                                              | Connection Score |
|--------------------------------------------------------------|------------------|
| Expansion topic connects to PRIMARY HUB                      | 100              |
| Expansion topic connects to HUB node                         | 75               |
| Expansion topic connects to CONNECTOR node                   | 50               |
| Expansion topic connects only to LEAF nodes                  | 25               |
| Expansion topic connects to an ORPHAN node                   | 10               |

**Phase 6B — Priority Label Assignment**

| Priority Score | Priority Label |
|----------------|----------------|
| 75–100         | HIGH           |
| 50–74          | MEDIUM         |
| 25–49          | LOW            |
| < 25           | DEPRIORITIZE   |

**Phase 6C — Cross-Path Priority Ranking**

After all individual expansion paths are scored:
1. Rank ALL expansion paths from all pillars by Priority Score (descending).
2. This produces a global content priority queue — not just per-pillar priorities.
3. The global queue is forwarded to `roadmap-engine.skill` as the primary input for content sequencing.

**Phase 6D — Priority Score Tie-Breaking**

When two expansion paths have the same Priority Score:
1. BOFU beats MOFU beats TOFU.
2. Larger cluster strength in the connecting node wins.
3. Alphabetical order (last resort only).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ PAGERANK-STYLE AUTHORITY FLOW MODEL

Beyond the Hub Score, this skill models how topical authority flows through the graph — the way that a highly authoritative piece of content in one cluster can pass authority to connected clusters, increasing their ranking potential even before those clusters have their own content built.

**Authority Flow Simulation:**

**Initial Authority Values:**
- Each node starts with an authority value equal to its `cluster_strength` score.
- Nodes with CONTENT_RICH receive a 20% authority bonus (existing content provides base authority).
- Nodes with CONTENT_EMPTY receive no bonus but are not penalized.

**Authority Propagation:**

At each iteration of the propagation:
1. Each node distributes 15% of its current authority value across its outgoing edges.
2. STRONG edges pass authority at 100% of the distributed amount.
3. MODERATE edges pass authority at 60%.
4. WEAK edges pass authority at 30%.
5. Authority received from incoming edges is added to the receiving node's total.
6. Run 5 propagation iterations.

**After Propagation:**
- Nodes that receive high authority from many STRONG connections have high `authority_potential`.
- Nodes with high `authority_potential` but low `cluster_strength` are called LATENT AUTHORITY NODES — they represent clusters that would gain disproportionate ranking benefit from internal link investment.
- Latent Authority Nodes are flagged for priority in `authority-engine.skill` recommendations.

**Authority Flow Output:**

```
AUTHORITY FLOW ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Node                     | Initial Authority | Post-Propagation | Authority Gain | Type
[Cluster Name]           | [base score]      | [propagated]     | [+N]           | LATENT AUTHORITY NODE
[Cluster Name]           | [base score]      | [propagated]     | [+N]           | AUTHORITY ANCHOR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ SEMANTIC DEPENDENCY MAPPING

Some clusters can only be meaningfully built after other clusters exist. Semantic dependency mapping identifies these prerequisite chains — and uses them to inform the expansion path sequencing in Step 5.

**Dependency Chain Detection:**

For every PREREQUISITE edge (A → B) in the edge registry:
- A is a DEPENDENCY of B.
- B cannot produce its highest-quality content without A existing first.
- B's priority score is reduced by 20% in the global priority ranking if A is CONTENT_EMPTY.
- When A moves to CONTENT_PRESENT → B's priority score is restored to its original calculated value.

**Dependency Chain Format:**

```
DEPENDENCY CHAIN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Topic B: [Cluster B Name]
  → Requires: [Cluster A Name] (A must exist first)
  → A Status: CONTENT_EMPTY → B Priority Reduced by 20%

Topic C: [Cluster C Name]
  → Requires: [Cluster B Name]
  → Requires: [Cluster A Name] (transitively)
  → Full chain: A → B → C
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ FUNNEL PROGRESSION MODELING

Map the specific paths a searcher would take through the graph — from their first TOFU search through to their eventual BOFU conversion. This model identifies complete searcher journeys and incomplete journeys (where a gap in the graph means the site loses a reader before conversion).

**Journey Path Construction:**

1. Identify all TOFU nodes — these are entry points into the graph.
2. From each TOFU node, follow FUNNEL FLOW edges to connected MOFU nodes.
3. From each MOFU node, follow FUNNEL FLOW edges to connected BOFU nodes.
4. If a FUNNEL FLOW edge is missing at any stage → mark the path as INCOMPLETE with the gap labeled.

**Journey Completeness Assessment:**

| Journey Path Status    | Condition                                                         | Implication                                     |
|------------------------|-------------------------------------------------------------------|-------------------------------------------------|
| **COMPLETE**           | TOFU → MOFU → BOFU all present with FUNNEL FLOW edges            | Site can capture and convert readers in this topic area |
| **PARTIAL (BOFU MISSING)** | TOFU → MOFU → !! NO BOFU !!                                | Site builds awareness and consideration but loses readers at decision stage |
| **PARTIAL (MOFU MISSING)** | TOFU → !! NO MOFU !! → BOFU                               | Site has awareness and conversion content but no evaluation bridge — high drop-off risk |
| **ENTRY ONLY**         | TOFU exists; no MOFU or BOFU in the connected graph             | Site can attract readers but has no conversion path in this topic area |
| **CONVERSION ONLY**    | BOFU exists; no connected TOFU or MOFU                          | Site has conversion content but no audience to convert |

Each INCOMPLETE or PARTIAL journey produces a specific gap recommendation forwarded to `roadmap-engine.skill`.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate Nodes

Every cluster from `semantic-clustering.skill` must appear exactly once in the node registry. If a cluster appears twice (possible if memory data introduces a duplicate) → merge the records, keeping the richer data version. Log the merge.

### Rule 2 — No Duplicate Edges

No two edges may share the same FROM node, TO node, AND relationship type simultaneously. If an edge-building phase would produce a duplicate → merge the duplicate into the existing edge by taking the higher strength rating and combining the rationale. Log the merge.

### Rule 3 — No Circular Meaningless Connections

A→B→A (circular reference) is valid if the relationship is genuinely bidirectional (e.g., SIBLING, COMPLEMENTARY). A circular reference is invalid when it is caused by misapplied DIRECTED relationships (e.g., A is a PREREQUISITE of B AND B is a PREREQUISITE of A). Detect and resolve all circular DIRECTED edges:

If A→B (PREREQUISITE) AND B→A (PREREQUISITE) → flag as CIRCULAR DEPENDENCY. Analyze which direction is correct. Convert to BIDIRECTIONAL or keep only the correct direction. Log the resolution.

### Rule 4 — No Redundant Edge Types Between the Same Node Pair

When the same two nodes are connected by both a PARENT-CHILD edge AND a SIBLING edge → the PARENT-CHILD edge takes precedence (it is more specific). Remove the SIBLING edge. Log.

### Rule 5 — No Self-Referencing Edges

A node cannot have an edge to itself. If any edge is detected with FROM = TO → remove immediately. Log as INVALID EDGE.

### Rule 6 — Memory Deduplication

Before producing output, query `memory-controller.skill` for an existing graph for this domain. If the graph exists and the cluster architecture has not changed → offer delta-update (add new nodes and edges for new clusters; do not rebuild unchanged portions of the graph). Never overwrite a prior graph without user confirmation.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Graph Behaviors

| Prohibited Behavior                                                                            | Reason                                                                        |
|-----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| Drawing an edge between two nodes without naming the relationship type                        | Every edge must have a named relationship — "related" is not a relationship type |
| Marking two nodes as connected because they are both in the SEO or marketing space            | Domain proximity is not a graph relationship — problem-space proximity is     |
| Assigning PRIMARY HUB status to the node with the most keywords (not the highest Hub Score)  | Hub status derives from the formula — not from keyword count alone            |
| Leaving any cluster as an ORPHAN without a resolution recommendation                          | Every orphan must receive a specific, actionable resolution option            |
| Scoring expansion paths based on "general importance" without applying the formula            | Priority scores are calculated — not intuited                                 |
| Creating FUNNEL FLOW edges between clusters in unrelated topic territories                    | Funnel flow follows topic territory, not just intent stage                    |
| Drawing a COMPETITIVE edge between clusters that should instead be MERGED or DIFFERENTIATED   | Competitive edges are risk flags — they should trigger a deduplication resolution |
| Producing expansion paths that duplicate existing clusters already in the architecture         | Expansion paths are for MISSING branches — not for recreating what exists     |
| Assigning STRONG strength to a CONTEXTUAL or COMPETITIVE edge                                 | Edge strength must reflect relationship specificity — contextual is MODERATE at best |
| Ignoring the funnel progression model and producing only a structural graph                   | The funnel progression model is mandatory — it reveals journey completeness   |

### Required Graph Behaviors

- Every edge must have: FROM, TO, relationship type, strength, direction, and rationale.
- Every node must have a Hub Score calculated using the defined formula.
- Every ORPHAN node must appear in the Orphan Resolution Report (Part 8).
- Every Pillar node must have at least one expansion path defined.
- Every expansion path must have a Priority Score calculated using the defined formula.
- The global priority ranking (cross-pillar) must be produced.
- The funnel progression model must identify every complete and incomplete reader journey.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary input)                                                        |
| **Receives**     | Complete cluster architecture (all parts) including keywords, funnel balance, strength scores, cross-references |
| **Used for**     | Step 1 (node creation), Step 2B (structural edges from hierarchy), Step 5 (expansion path analysis) |

---

### ▸ entity-extraction.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional source                                                                    |
| **Receives**     | Entity map with Critical/Important entities per cluster topic                               |
| **Used for**     | Step 2D (entity-based edge building), Step 5B (entity gap in missing branch identification) |

---

### ▸ competitor-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional source                                                                    |
| **Receives**     | Gap matrix, opportunity intelligence, competition levels per cluster topic                  |
| **Used for**     | Step 6 (priority scoring — gap existence and competition level factors)                    |

---

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional source                                                                    |
| **Receives**     | Content status per cluster (published, planned, draft, empty)                              |
| **Used for**     | Step 1B (node content status flagging), Step 6 (content potential score in Hub formula)   |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before graph build, WRITE after output finalization                   |
| **Reads**        | Prior graph state, prior node records, prior edge records                                  |
| **Writes**       | Complete topic graph (all 8 parts), authority flow analysis, dependency chains             |
| **Memory layers**| strategy-memory.skill (graph state), content-memory.skill (node content status)           |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | query-graph → roadmap-engine (primary strategic input)                                     |
| **Sends**        | Global priority ranking, expansion paths, funnel progression model, dependency chains      |
| **Critical fields** | Global priority ranking (cross-pillar) — this is the primary input for roadmap sequencing |

---

### ▸ authority-engine.skill (authority-mapping.skill)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | query-graph → authority-engine (architectural input)                                       |
| **Sends**        | Hub analysis, authority flow model, LATENT AUTHORITY NODE flags, edge registry             |
| **Used for**     | Designing the internal link structure that maximizes authority propagation through the graph |

---

### ▸ internal-linking.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | query-graph → internal-linking (structural reference)                                      |
| **Sends**        | Edge registry (specifically FUNNEL FLOW, ENTITY-SHARED, and AUTHORITY-FLOW edges), cross-reference tags |
| **Used for**     | Determining which pages should link to which based on graph relationships                  |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                            | Detection                                             | Recovery Action                                                                                             |
|-----------------------------------------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Fewer than 3 clusters in input                                              | Phase 1C — node count < 3                             | Halt. Flag: "Graph requires minimum 3 nodes for meaningful relationships. Request additional clusters from semantic-clustering.skill." |
| All nodes are orphans after edge building                                   | Step 4 — orphan count = total node count              | Flag: "Graph has no connections — cluster architecture may be too fragmented. Consider broadening clusters or reviewing semantic groupings." |
| No Pillar nodes exist in the cluster architecture                           | Step 3 — no node qualifies for HUB or PRIMARY HUB    | Promote the highest-scoring non-pillar node to provisional HUB. Flag: "No pillar clusters detected — using provisional hub." |
| Entity map unavailable for entity edge building                             | Phase 2D — entity map input null                      | Skip entity-based edge building. Use only structural and funnel-based edges. Flag: "ENTITY ENRICHMENT ABSENT — entity-based edges not built." |
| Two nodes produce identical Hub Scores                                      | Phase 3C — tie at top position                        | Apply tie-breaking logic. If tie persists → flag both as PRIMARY HUB CANDIDATES and surface to user for selection. |
| A dependency chain creates a circular lock (A requires B; B requires A)    | Advanced Logic — circular PREREQUISITE detection      | Flag as CIRCULAR DEPENDENCY. Force-break the cycle by analyzing which prerequisite is stronger. Keep the stronger directed edge. Remove the weaker. Log. |
| Expansion path scoring produces all LOW or DEPRIORITIZE scores              | Step 6B — no HIGH or MEDIUM paths detected            | Flag: "All expansion paths scored LOW or DEPRIORITIZE — topic space may be saturated or clusters may be too broad. Review cluster definitions or expand into adjacent topic territories." |
| More than 50% of edges are WEAK after all edge building phases              | Phase 2G — weak edge ratio > 0.50                     | Flag: "Graph is weakly connected — clusters may not form a coherent topical authority strategy. Review cluster relationships and consider consolidation." |
| Memory controller unavailable for graph state retrieval                     | READ query returns error                               | Proceed with fresh graph build. Flag: "Prior graph state unavailable — building from scratch. Delta-update not possible." |
| Connection unclear between two nodes after all five edge-building phases    | No edge type fires for a pair                         | Mark as NO DIRECT CONNECTION. Do not force an edge. Log the pair as REVIEWED — NO EDGE. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — RELATIONSHIP TYPE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
EDGE RELATIONSHIP TYPES — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PARENT-CHILD   → Hierarchical; one is a sub-topic of the other
                 Direction: BIDIRECTIONAL | Strength: STRONG

PREREQUISITE   → A must exist before B can be fully understood
                 Direction: DIRECTED A→B | Strength: STRONG

FUNNEL FLOW    → TOFU feeds MOFU; MOFU feeds BOFU within a topic territory
                 Direction: DIRECTED (awareness→consideration→decision)
                 Strength: STRONG (same pillar) | MODERATE (adjacent pillar)

SIBLING        → Same tier; same parent; related but distinct problems
                 Direction: BIDIRECTIONAL | Strength: MODERATE

ENTITY-SHARED  → Both clusters share Critical/Important entities
                 Direction: BIDIRECTIONAL | Strength: STRONG/MODERATE/WEAK by overlap count

SEQUENTIAL     → Topic B logically follows Topic A in learner journey
                 Direction: DIRECTED A→B | Strength: MODERATE

COMPLEMENTARY  → Related problems; often consumed together
                 Direction: BIDIRECTIONAL | Strength: MODERATE

CONTEXTUAL     → Adjacent topics; cross-reference in SERP ecosystem
                 Direction: BIDIRECTIONAL | Strength: WEAK

COMPETITIVE    → Overlapping SERP positions; differentiation needed
                 Direction: BIDIRECTIONAL | Strength: WEAK (risk flag only)

AUTHORITY-FLOW → High-authority cluster can pass equity to strategic node
                 Direction: DIRECTED | Strength: as defined by authority model

INVALID TYPES — Never use these:
  ✗ "Related" — too vague
  ✗ "Similar" — too vague
  ✗ "In the same space" — proximity is not a relationship
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — HUB SCORE CALCULATION WORKSHEET
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
HUB SCORE WORKSHEET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Node: [Cluster Name]

Factor 1 — Connection Score (40% weight)
  Total edges                 : [N]
  Max edges in any node       : [M]
  Proportion score            : (N/M) × 50 = [A]
  Strong edges                : [S]
  Strong edge ratio           : (S/N) × 50 = [B]
  Connection Score            : (A + B) / 2 = [0–100]
  Weighted                    : × 0.40 = [score]

Factor 2 — Funnel Score (25% weight)
  TOFU in node + subtree      : [Y/N]
  MOFU in node + subtree      : [Y/N]
  BOFU in node + subtree      : [Y/N]
  Funnel Score                : [100/75/60/40/20]
  Weighted                    : × 0.25 = [score]

Factor 3 — Cluster Strength (20% weight)
  Strength (from clustering)  : [0–100]
  Weighted                    : × 0.20 = [score]

Factor 4 — Content Potential Score (15% weight)
  Content Status              : [EMPTY/PLANNED/PRESENT/RICH]
  Content Potential Score     : [100/70/50/20]
  Weighted                    : × 0.15 = [score]

HUB SCORE TOTAL             : [sum of 4 weighted scores]
NODE TYPE                   : [PRIMARY HUB / HUB / CONNECTOR / LEAF / ORPHAN]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — PRIORITY SCORE CALCULATION WORKSHEET
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
EXPANSION PATH PRIORITY SCORE WORKSHEET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Path: [From Node] → [Expansion Topic]

Factor 1 — Intent Score (35% weight)
  Intent Stage of Topic       : [BOFU / MOFU / TOFU / Mixed]
  Intent Score                : [100 / 70 / 40 / 30]
  Weighted                    : × 0.35 = [score]

Factor 2 — Gap Score (35% weight)
  Gap Existence               : [Confirmed/Partial/No/Unknown]
  Gap Score                   : [100/80/60/40/10] or [90/50/10] without competitor data
  Weighted                    : × 0.35 = [score]

Factor 3 — Competition Score (20% weight)
  Competition Level           : [LOW / MEDIUM / HIGH / Unknown]
  Competition Score           : [100 / 60 / 20 / 50]
  Weighted                    : × 0.20 = [score]

Factor 4 — Connection Value (10% weight)
  Best connection to          : [PRIMARY HUB / HUB / CONNECTOR / LEAF / ORPHAN]
  Connection Score            : [100 / 75 / 50 / 25 / 10]
  Weighted                    : × 0.10 = [score]

PRIORITY SCORE TOTAL        : [sum of 4 weighted scores]
PRIORITY LABEL              : [HIGH ≥75 / MEDIUM 50–74 / LOW 25–49 / DEPRIORITIZE <25]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: analysis/query-graph.skill*
*Nexus SEO Operating System — Version 1.0.0*
