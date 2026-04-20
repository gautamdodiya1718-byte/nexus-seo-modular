# SKILL: analysis/semantic-clustering.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 2.0.0
# LAYER: Analysis / Topical Architecture
# EXECUTION PRIORITY: POST-DISCOVERY — Runs after keyword-discovery.skill and deduplication-engine.skill complete.
# POSITION: Topical authority foundation. Converts keyword universe into structured content architecture.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **topical authority architecture engine** of the Nexus system.

It receives the deduplicated keyword universe from `keyword-discovery.skill` and transforms it into a structured hierarchy of topic clusters — the architectural framework on which all content planning, internal linking strategy, authority building, and content production is built.

Topical authority is not achieved by creating many articles on related topics. It is achieved by owning a clearly defined semantic space — a coherent set of interconnected clusters where every piece of content strengthens every other piece. This skill designs that space. It draws the boundaries between clusters, defines the relationships between them, identifies which clusters deserve pillar-level investment, and validates that the resulting architecture provides complete funnel coverage.

Every decision this skill makes is based on user intent and semantic meaning — not surface-level word matching. Two keywords that share no common words can belong in the same cluster. Two keywords that share the same root word can belong in different clusters. What determines cluster membership is the problem being solved and the goal of the searcher — nothing else.

### Primary Responsibilities

| Responsibility                       | Description                                                                                                        |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| **Keyword Normalization**            | Standardize all incoming keywords before any grouping begins — case, format, and deduplication                     |
| **Semantic Grouping**                | Group keywords by shared problem, shared user goal, and shared conceptual territory — not shared words             |
| **Intent Sub-Clustering**            | Within each cluster, partition keywords by funnel stage (TOFU, MOFU, BOFU)                                         |
| **Cluster Naming**                   | Assign clear, descriptive, human-readable topic names — never keyword fragments, never vague categories            |
| **Pillar Detection**                 | Identify clusters that qualify as pillar clusters based on keyword density and multi-intent coverage               |
| **Cluster Validation**               | Check cluster boundaries for overlap; merge clusters that share ≥3 keywords                                        |
| **Hierarchy Construction**           | Build the three-tier hierarchy: Pillar → Sub-cluster → Micro-cluster                                              |
| **TF-IDF Importance Simulation**     | Apply importance weighting to keywords within clusters; prioritize dense, unique clusters over thin, generic ones  |
| **Intent-Weighted Clustering**       | Weight cluster formation by the strategic value of the intent distribution within each group                       |
| **Cluster Strength Scoring**         | Score each cluster on keyword density, funnel balance, and topical specificity                                     |
| **Coverage Validation**              | Ensure every keyword is assigned to exactly one cluster; no orphans; no single-keyword clusters                    |

### What This Skill Is NOT

- It is **not** a keyword grouping tool based on word matching. Shared words are never the primary grouping signal.
- It is **not** a content generator. It creates architecture — it does not produce content.
- It is **not** a SERP analyzer. It reasons from keyword intent and semantic relationships — not live SERP data.
- It is **not** a flat list producer. The output is always a three-tier hierarchy — pillar, sub-cluster, micro-cluster.
- It is **not** optional. All downstream content skills depend on the cluster architecture this skill produces.

### The Defining Principle of This Skill

> Clustering is not a word-matching exercise. It is intent modeling.
>
> Two keywords belong in the same cluster when:
> — They address the same underlying problem or goal.
> — A single comprehensive article could meaningfully serve both searchers.
> — A user who searches one would likely also search the other during the same decision journey.
>
> Two keywords that share the same root belong in different clusters when:
> — They serve different problems.
> — They require entirely different content to answer.
> — Their users are at different points in their decision journey with no natural bridge.
>
> Word overlap is never the primary clustering signal.
> Intent alignment is always the primary clustering signal.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input — Keyword Universe

The complete, deduplicated keyword universe from `keyword-discovery.skill`, after passing through `deduplication-engine.skill`.

**Required Fields Per Keyword:**

| Field              | Description                                                              | Required |
|--------------------|--------------------------------------------------------------------------|----------|
| `keyword`          | The full, exact keyword phrase                                           | YES      |
| `intent`           | Informational / Commercial / Transactional                               | YES      |
| `funnel`           | TOFU / MOFU / BOFU                                                       | YES      |
| `category`         | Dimension category from keyword-discovery (Educational, Comparison, etc.)| YES      |
| `strategic_reason` | Why this keyword matters and what gap it fills                           | SOFT     |
| `pre_cluster_hint` | Advisory cluster label from keyword-discovery.skill                      | OPTIONAL |

### Optional Input — Memory Data

Existing clusters from prior sessions or prior runs, retrieved from `memory-controller.skill`:

| Field                       | Description                                                          |
|-----------------------------|----------------------------------------------------------------------|
| `existing_cluster_names`    | Names of clusters already built and written to memory               |
| `existing_cluster_keywords` | Keyword lists already assigned to existing clusters                 |
| `existing_pillar_clusters`  | Which clusters are already designated as pillars                    |
| `merged_cluster_history`    | Record of prior merges — prevents re-proposing dissolved clusters   |

### Input Pre-Conditions

1. **Has the keyword universe passed through `deduplication-engine.skill`?** If not → run deduplication first. This skill does not run its own full deduplication — it assumes input has been cleaned.
2. **Minimum keyword count:** If fewer than 10 keywords → trigger failsafe: expand before clustering.
3. **Are all keywords assigned an intent and funnel stage?** If any are null → infer before proceeding to Step 2.
4. **Are existing clusters available in memory?** If yes → load them before Step 1 to prevent re-creating what already exists.
5. **Has this skill been run before for this keyword universe?** → Query `memory-controller.skill`. If within freshness (14 days) → offer cached. If stale → proceed fresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Structured Cluster Architecture Report

```
NEXUS SEMANTIC CLUSTER ARCHITECTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain / Topic          : [domain or topic scope]
Keywords Received       : [count]
Keywords Clustered      : [count — must equal received]
Orphan Keywords         : 0 [required]
Total Clusters          : [count]
  Pillar Clusters       : [count]
  Sub-Clusters          : [count]
  Micro-Clusters        : [count]
Clusters Merged         : [count]
Average Cluster Strength: [score / 100]
Generated By            : analysis/semantic-clustering.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — CLUSTER ARCHITECTURE TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| ID     | Cluster Name                  | Tier    | Parent    | Keywords | Intent Mix          | Pillar | Strength | Pillar Topic                    |
|--------|-------------------------------|---------|-----------|----------|---------------------|--------|----------|---------------------------------|
| C-P01  | [Name]                        | Pillar  | ROOT      | [N]      | T:[N] M:[N] B:[N]  | YES    | [0–100]  | [2–5 word pillar topic label]   |
| C-S01  | [Name]                        | Sub     | C-P01     | [N]      | T:[N] M:[N] B:[N]  | NO     | [0–100]  | —                               |
| C-M01  | [Name]                        | Micro   | C-S01     | [N]      | T:[N] M:[N] B:[N]  | NO     | [0–100]  | —                               |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — CLUSTER DETAIL RECORDS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH CLUSTER]

CLUSTER: [ID] — [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Tier                : [Pillar / Sub / Micro]
Parent Cluster      : [Parent Name or ROOT]
Pillar Topic        : [Topic label for pillar clusters / — for others]
Cluster Strength    : [0–100 score]
Description         : [one sentence — what this cluster covers and why it matters strategically]

Keywords by Funnel Stage:

  TOFU (Awareness):
    → [keyword] | [intent: Informational] | [category] | TF-IDF weight: [HIGH/MED/LOW]
    → [keyword] | [intent] | [category] | weight: [H/M/L]

  MOFU (Consideration):
    → [keyword] | [intent: Commercial] | [category] | weight: [H/M/L]
    → [keyword] | [intent] | [category] | weight: [H/M/L]

  BOFU (Decision):
    → [keyword] | [intent: Transactional] | [category] | weight: [H/M/L]
    → [keyword] | [intent] | [category] | weight: [H/M/L]

Total Keywords      : [count]
Funnel Balance      : [FULL / PARTIAL — missing stages listed / MINIMAL]
Content Priority    : [HIGH / MEDIUM / LOW]
Cross-References    : [related cluster names — for internal linking]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[END CLUSTER — REPEAT FOR ALL]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — CLUSTER HIERARCHY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Pillar Cluster Name] (C-P01) [Strength: N]
  ├── [Sub-Cluster Name] (C-S01) [Strength: N]
  │     ├── [Micro-Cluster Name] (C-M01)
  │     └── [Micro-Cluster Name] (C-M02)
  └── [Sub-Cluster Name] (C-S02) [Strength: N]
        └── [Micro-Cluster Name] (C-M03)

[Pillar Cluster Name] (C-P02) [Strength: N]
  ├── [Sub-Cluster Name] (C-S03)
  ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — MERGE LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Merge # | Cluster A        | Cluster B        | Shared Keywords | Reason         | Result Cluster Name  |
|---------|------------------|------------------|-----------------|----------------|----------------------|
| 1       | [Name]           | [Name]           | [N] — [list]   | ≥3 overlap     | [merged name]        |
| 2       | [Name]           | [Name]           | [N] — [list]   | Purpose overlap| [merged name]        |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — COVERAGE VALIDATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total Keywords In    : [count]
Total Keywords Out   : [count — must equal In]
Unassigned Keywords  : 0 [required]
Single-Keyword Clusters: 0 [required]
Full-Funnel Clusters : [count]
Partial-Funnel Clusters: [count] — [names and missing stages]
Pillar Clusters      : [count] — [names]
Coverage Status      : [COMPLETE / REQUIRES ACTION — details]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into           : query-graph.skill
                       opportunity-engine.skill
                       authority-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Field Definitions

| Field            | Definition                                                                                                    |
|------------------|---------------------------------------------------------------------------------------------------------------|
| `Cluster Name`   | The human-readable topic name — describes the content theme, not any individual keyword                      |
| `Tier`           | Pillar / Sub / Micro — position in the three-level hierarchy                                                  |
| `Parent`         | The parent cluster's ID; ROOT for pillar clusters                                                             |
| `Intent Mix`     | Count of keywords at each funnel stage: T=TOFU, M=MOFU, B=BOFU                                              |
| `Pillar`         | YES if cluster meets all five pillar qualification criteria; NO otherwise                                     |
| `Strength`       | 0–100 composite score from TF-IDF simulation and intent weighting                                            |
| `Pillar Topic`   | A 2–5 word label defining the topic territory this pillar cluster owns                                       |
| `Description`    | One sentence: what this cluster covers + why it matters                                                      |
| `Funnel Balance` | FULL (all 3 stages present) / PARTIAL (1–2 stages missing) / MINIMAL (only 1 stage)                         |
| `Content Priority` | HIGH / MEDIUM / LOW — based on cluster strength and funnel balance                                         |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DATA NORMALIZATION

Before any semantic grouping begins, normalize all incoming keywords to ensure that the grouping logic operates on clean, consistent, comparable data.

**Phase 1A — Lowercase Normalization**

Apply to every keyword:
- Convert all characters to lowercase.
- `"Best CRM Software"` → `"best crm software"`.
- `"HubSpot vs Salesforce"` → `"hubspot vs salesforce"`.

**Phase 1B — Formatting Issue Removal**

Apply to every keyword:

| Formatting Issue                    | Normalization Action                                        |
|-------------------------------------|-------------------------------------------------------------|
| Leading/trailing whitespace         | Trim.                                                       |
| Double or multiple spaces           | Collapse to single space.                                   |
| Trailing punctuation (. , ! ?)      | Remove — but preserve ? in question keywords for AEO signals|
| Inconsistent hyphenation            | Standardize: `"e commerce"` and `"e-commerce"` → `"ecommerce"` if domain convention |
| Parenthetical qualifiers            | Strip if not part of the search query: `"CRM (software)"` → `"crm"` |
| Encoded characters                  | Decode: `"email%20marketing"` → `"email marketing"`         |

**Phase 1C — Within-Input Deduplication**

Even though `deduplication-engine.skill` has already processed this list, run a lightweight within-input deduplication pass:

1. Build a normalized hash map.
2. For each keyword: if its normalized form already exists in the map → remove the duplicate.
3. Log removals: `"[keyword] — normalized duplicate of [kept keyword]."`

**Phase 1D — Intent and Funnel Validation**

For every keyword, verify that `intent` and `funnel` fields are populated:

| Field State          | Action                                                                 |
|----------------------|------------------------------------------------------------------------|
| Intent present       | Use as-is.                                                             |
| Intent null          | Infer from keyword pattern (apply rules from content-inventory.skill Step 4). Flag as INFERRED. |
| Funnel present       | Use as-is.                                                             |
| Funnel null          | Derive from inferred intent. Flag as INFERRED.                        |
| Both null            | Attempt inference. If still null after inference → flag as UNCLASSIFIED; include in clustering with caveat. |

**Phase 1E — Pre-Cluster Hint Integration**

If `pre_cluster_hint` fields are present (from keyword-discovery.skill):
- Use them as the starting seed for cluster formation — not as final assignments.
- Pre-cluster hints are advisory. The semantic grouping in Step 2 may override them.
- Log any overrides: `"Pre-cluster hint '[hint]' overridden — keyword assigned to '[new cluster]' based on semantic analysis."`

**Normalization Output:**

```
NORMALIZATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keywords Received       : [count]
Normalization Changes   : [count]
  Lowercased            : [count]
  Whitespace Cleaned    : [count]
  Punctuation Fixed     : [count]
  Duplicates Removed    : [count]
  Intent Inferred       : [count]
  Pre-Cluster Hints     : [count used / count overridden]
Keywords Ready          : [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — SEMANTIC GROUPING

This is the intellectually central step of this skill. Group keywords based on semantic meaning and user intent — never based on shared words.

**Phase 2A — Problem Space Identification**

For each keyword, identify the underlying problem or goal the searcher is trying to address. The problem space is the primary grouping signal.

**Problem Space Derivation:**

Strip the keyword down to its core: what is the user trying to DO or UNDERSTAND?

| Keyword                                    | Surface Level                  | Underlying Problem Space                                   |
|--------------------------------------------|--------------------------------|------------------------------------------------------------|
| `"best crm software"`                      | CRM software                   | Finding and selecting the right CRM                       |
| `"top crm tools for sales"`                | CRM tools for sales            | Finding and selecting the right CRM                       |
| `"hubspot vs salesforce"`                  | HubSpot vs Salesforce          | Finding and selecting the right CRM                       |
| `"how to choose a crm"`                    | CRM selection process          | Finding and selecting the right CRM                       |
| → ALL FOUR share the same problem space → ONE CLUSTER |                  |                                                            |
| `"how to set up a crm"`                    | CRM setup                      | Implementing a CRM after selecting it                     |
| `"crm implementation best practices"`      | CRM implementation             | Implementing a CRM after selecting it                     |
| → THESE TWO share a different problem space → DIFFERENT CLUSTER |     |                                                            |

**Phase 2B — Grouping Decision Protocol**

For every keyword pair, apply the five-question grouping protocol to determine whether they belong in the same cluster:

| Question                                                                                    | Weight  |
|---------------------------------------------------------------------------------------------|---------|
| Q1: Do both keywords address the same underlying problem?                                   | 40%     |
| Q2: Would one comprehensive article satisfy both searchers?                                 | 30%     |
| Q3: Would a user who searches one likely also search the other in the same journey?         | 20%     |
| Q4: Do both keywords target the same audience (same expertise level, same role)?           | 5%      |
| Q5: If both articles were published, would they compete for the same SERP position?        | 5%      |

**Grouping Decision Rules:**

| Score (weighted YES answers × weight) | Decision                                           |
|---------------------------------------|----------------------------------------------------|
| ≥ 0.70                                | SAME CLUSTER — strong semantic alignment           |
| 0.50–0.69                             | LIKELY SAME CLUSTER — proceed; flag for validation |
| 0.30–0.49                             | LIKELY DIFFERENT — place in different clusters; cross-reference if thematically adjacent |
| < 0.30                                | DIFFERENT CLUSTER — distinct problem spaces        |

**Phase 2C — Grouping Execution Algorithm**

1. Start with the keyword that has the highest strategic importance (highest combination of BOFU presence + specificity + uniqueness).
2. Assign it to Cluster 1 with a provisional label.
3. For each subsequent keyword (in order of strategic importance):
   a. Test it against every existing provisional cluster using Phase 2B protocol.
   b. If score ≥ 0.70 with an existing cluster → assign to that cluster.
   c. If score 0.50–0.69 → assign with VERIFY FLAG.
   d. If score < 0.50 for all existing clusters → create a new cluster and assign it there.
4. Continue until all keywords are assigned.
5. No keyword may remain unassigned after this phase.

**Phase 2D — Anti-Pattern Detection**

The following grouping behaviors indicate an error in semantic analysis. Detect and correct them:

| Anti-Pattern                                                                           | Error Type                | Correction                                              |
|----------------------------------------------------------------------------------------|---------------------------|---------------------------------------------------------|
| Grouping `"crm software"` and `"crm database"` because both contain `"crm"`          | Word-matching error       | Re-evaluate problem space — these may be different clusters |
| Separating `"best crm"` and `"top crm tools"` into different clusters               | Semantic blindness error  | Both serve the same evaluation intent — merge           |
| Grouping all TOFU keywords together regardless of topic                                | Intent-as-grouping error  | Funnel stage is a sub-dimension of clusters, not the primary grouping signal |
| Creating a cluster per keyword-discovery dimension (all how-to keywords together)     | Category-as-grouping error | Category labels are not cluster labels — regroup by topic |
| Grouping `"crm for real estate"` and `"crm for healthcare"` because both are niche   | Industry-conflation error  | Different industries = different clusters — separate    |

---

### ▸ STEP 3 — INTENT SUB-CLUSTERING

After semantic groups are established, organize the keywords within each cluster by funnel stage. This produces the internal structure that maps directly to the content pieces a cluster will support.

**Phase 3A — Sub-Cluster Construction**

For each cluster from Step 2, partition its keywords into three intent layers:

| Intent Layer   | Funnel Stage | What It Serves                                                              |
|----------------|--------------|-----------------------------------------------------------------------------|
| **Awareness**  | TOFU         | Users just discovering the problem or concept — need education              |
| **Consideration** | MOFU      | Users evaluating options, comparing solutions — need guidance               |
| **Decision**   | BOFU         | Users ready to act — need conversion support                                |

**Phase 3B — Sub-Cluster Assignment Rules**

1. Every keyword in the cluster is assigned to exactly one intent layer.
2. Use the keyword's `funnel` field from the input — do not re-classify.
3. For keywords with ambiguous funnel assignments (flagged as INFERRED in Step 1) → apply the following default:
   - Commercial intent + comparison signal → MOFU.
   - Commercial intent + price/trial signal → BOFU.
   - Informational intent + solution-seeking → TOFU.

**Phase 3C — Funnel Balance Assessment**

For each cluster, assess the distribution of keywords across funnel stages:

| Funnel Balance Status | Condition                                          | Strategic Implication                                      |
|-----------------------|----------------------------------------------------|------------------------------------------------------------|
| **FULL FUNNEL**       | At least 1 keyword in each of TOFU, MOFU, BOFU   | Cluster can support a complete content strategy            |
| **PARTIAL**           | At least 1 keyword in 2 of 3 stages              | Cluster has gaps — note which stage is missing             |
| **MINIMAL**           | Keywords in only 1 funnel stage                   | Cluster cannot independently convert — needs enrichment    |
| **MISSING TOFU**      | Zero TOFU keywords                                | No awareness entry — cluster may be hard to discover       |
| **MISSING MOFU**      | Zero MOFU keywords                                | No evaluation bridge — awareness cannot reach conversion   |
| **MISSING BOFU**      | Zero BOFU keywords                                | No conversion trigger — cluster is entirely educational    |

Clusters with MISSING TOFU, MISSING MOFU, or MISSING BOFU are flagged. Their missing-stage keywords should be proposed for generation by `keyword-discovery.skill` in a subsequent pass.

---

### ▸ STEP 4 — CLUSTER NAMING

Assign a precise, human-readable, topic-level name to every cluster.

**Phase 4A — Naming Principles**

| Principle                         | Rule                                                                                           |
|-----------------------------------|------------------------------------------------------------------------------------------------|
| **Topic, not keyword**            | The name describes the content territory, not the phrasing of any individual keyword          |
| **Broad but precise**             | Wide enough to encompass all keywords; narrow enough to be meaningfully distinct from other clusters |
| **Human-readable**                | A non-SEO professional must understand the cluster's purpose from the name alone              |
| **2–5 words**                     | Short enough to use as a navigation label; long enough to be specific                         |
| **Present tense noun phrase**     | `"CRM Software Selection"` — not `"Selecting CRM Software"` or `"How to Select CRM"`         |
| **No keyword stuffing**           | Never concatenate multiple keywords to create the cluster name                                |
| **No filler descriptors**         | "General," "Other," "Miscellaneous" are prohibited. Every cluster has a named territory.      |

**Phase 4B — Naming by Cluster Content**

Use the cluster's highest-weight keywords (from the TF-IDF simulation in Advanced Logic) as the naming signal — not the most frequent words:

| Cluster Content                                                                     | Bad Name                        | Good Name                    |
|-------------------------------------------------------------------------------------|---------------------------------|------------------------------|
| Keywords about evaluating, comparing, and selecting CRM software                   | `"CRM Tools List Best"`         | `"CRM Software Selection"`   |
| Keywords about CRM for specific industries (real estate, healthcare, agencies)     | `"CRM Industry"`                | `"CRM by Industry"`          |
| Keywords about setting up, configuring, and onboarding a CRM                      | `"CRM Setup Guide Steps"`       | `"CRM Setup and Implementation"` |
| Keywords about what a CRM is, how it works, why businesses use it                 | `"CRM Questions What Is"`       | `"CRM Fundamentals"`         |
| Keywords about CRM pricing, free options, cost comparisons                         | `"CRM Cost Cheap Free"`         | `"CRM Pricing and Costs"`    |
| Keywords about CRM automation features                                              | `"CRM Automation Tools Best"`   | `"CRM Automation"`           |

**Phase 4C — Naming Conflict Resolution**

If two clusters produce nearly identical names after Phase 4B:
1. Re-examine whether they should be merged (Step 6).
2. If they must remain separate → add a clarifying qualifier to differentiate them.
   - `"CRM for Small Business"` vs `"CRM for Enterprise"` → valid — audience qualifier creates meaningful distinction.
   - `"CRM Overview"` vs `"CRM Introduction"` → invalid — same territory, different words → merge these clusters.

---

### ▸ STEP 5 — PILLAR DETECTION

Evaluate every cluster against the pillar qualification criteria. Pillar clusters are the strategic anchors of the content architecture — they define the major topic domains the site will attempt to own.

**Phase 5A — Pillar Qualification Criteria**

A cluster qualifies as a Pillar cluster if it meets ALL FIVE of the following conditions:

| Criterion                         | Threshold                                                                         |
|-----------------------------------|-----------------------------------------------------------------------------------|
| **Keyword density**               | ≥5 keywords in the cluster after normalization and deduplication                 |
| **Funnel completeness**           | Has at least 1 keyword in each of TOFU, MOFU, and BOFU                           |
| **Strategic centrality**          | The cluster covers a topic that represents a core business domain or major content opportunity for the site |
| **Topic specificity**             | The cluster is specific enough to be owned — not so broad that ranking for it would require domain authority beyond the site's realistic reach |
| **Content depth potential**       | The cluster can support at minimum 3 distinct articles at different funnel stages without those articles overlapping |

**Phase 5B — Pillar Designation Logic**

| All 5 criteria met? | Designation  |
|---------------------|--------------|
| YES                 | PILLAR — mark with Pillar: YES; assign Pillar Topic label |
| 4 of 5 met          | NEAR-PILLAR — note which criterion is missing; mark as Sub-cluster |
| 3 of 5 met          | Sub-cluster  |
| 1–2 of 5 met        | Micro-cluster |
| 0 of 5 met          | Single-keyword cluster — blocked by anti-fluff rules (see Section 7) |

**Phase 5C — Pillar Topic Label Assignment**

Every Pillar cluster receives a Pillar Topic label — a 2–5 word phrase that describes the strategic topic territory the cluster is designed to own:

| Cluster Name                | Pillar Topic Label              |
|-----------------------------|---------------------------------|
| `"CRM Software Selection"`  | `"CRM Software"`                |
| `"Email Marketing Strategy"`| `"Email Marketing"`             |
| `"Technical SEO Guides"`    | `"Technical SEO"`               |
| `"Content Creation Methods"`| `"Content Creation"`            |

The Pillar Topic label is used by `authority-engine.skill` and `query-graph.skill` to define the site's topical authority domains.

**Phase 5D — Pillar Qualification Failure Handling**

When no cluster qualifies for pillar status (possible with a small or highly specific keyword set):
1. Identify the cluster closest to qualifying.
2. Flag it as PROVISIONAL PILLAR — note the missing criteria.
3. Recommend keyword expansion to the calling workflow to meet pillar criteria.
4. Do not force-designate a pillar where none qualifies — a false pillar produces a false authority architecture.

---

### ▸ STEP 6 — CLUSTER VALIDATION

Inspect all clusters for boundary integrity. Clusters must have clear, non-overlapping purposes. Where overlap exists, resolve it explicitly.

**Phase 6A — Keyword Overlap Check**

For every pair of clusters, count the keywords that appear in both:

| Overlap Level     | Threshold       | Action                                                                  |
|-------------------|-----------------|-------------------------------------------------------------------------|
| **High overlap**  | ≥3 shared keywords | MERGE the clusters. Choose the broader name or create a new merged name. |
| **Moderate overlap** | 1–2 shared keywords | CROSS-REFERENCE. The shared keyword stays in the cluster where it has the highest strategic alignment. The other cluster receives a cross-reference note. |
| **No overlap**    | 0 shared keywords | No action required. Clusters are well-bounded.                         |

**Phase 6B — Purpose Overlap Check**

Even if clusters share no keywords, their strategic purposes may overlap — meaning content created for one cluster would compete with content from the other.

**Purpose Overlap Detection:**

Compare cluster descriptions (from Step 4) for each pair. If two clusters would produce content that targets the same SERP positions → purpose overlap detected.

Apply the same threshold logic:
- Purpose overlap that is severe (same SERP positions, same audience, same intent) → MERGE.
- Purpose overlap that is moderate (same topic territory, different angles) → CROSS-REFERENCE.

**Phase 6C — Merge Execution**

When two clusters are merged:
1. Combine all keywords from both clusters into the merged cluster.
2. Re-run intent sub-clustering (Step 3) on the merged keyword set.
3. Re-evaluate pillar status for the merged cluster (Step 5).
4. Assign a name for the merged cluster (often the broader of the two prior names, or a new encompassing name).
5. Log the merge in the Merge Log (Part 4 of output).
6. Remove the dissolved cluster from all output tables.

**Phase 6D — Cross-Reference Execution**

When two clusters are cross-referenced (not merged):
1. Shared keywords remain in their primary cluster (highest strategic alignment).
2. Each cluster receives a `Cross-References` field listing the related cluster.
3. Cross-references are used by `authority-engine.skill` for internal linking strategy.

**Phase 6E — Validation Anti-Patterns**

| Error                                                                          | Consequence                                                    |
|--------------------------------------------------------------------------------|----------------------------------------------------------------|
| Merging clusters that share only surface-level phrasing (not semantic purpose) | Over-merging destroys specificity; creates unfocused pillar content |
| Cross-referencing instead of merging when overlap is high (≥3 keywords)        | Creates structural fragmentation in the architecture           |
| Allowing a keyword to exist in two clusters simultaneously after boundary check | Violates single-cluster-membership rule                        |
| Resolving overlap by deleting the shared keyword from both clusters            | Loss of a valid keyword — reference-tag instead               |

---

### ▸ STEP 7 — CLUSTER HIERARCHY CONSTRUCTION

Build the three-tier hierarchy that defines the content architecture structure.

**Phase 7A — Tier Definitions**

| Tier       | Label        | Description                                                                               |
|------------|--------------|-------------------------------------------------------------------------------------------|
| **Tier 1** | Pillar       | A major topic domain. Owns a broad content territory. Supports a pillar page or content hub. |
| **Tier 2** | Sub-cluster  | A focused subtopic within a Pillar. Supports multiple supporting articles.               |
| **Tier 3** | Micro-cluster| A highly specific niche within a Sub-cluster. Supports 1–3 targeted articles.            |

**Phase 7B — Hierarchy Assignment Rules**

1. All Pillar clusters occupy Tier 1. They have no parent (parent = ROOT).
2. Sub-clusters are assigned to the Pillar cluster whose topic most closely contains their focus.
3. Micro-clusters are assigned to the Sub-cluster whose topic most closely contains their focus.
4. No cluster may be its own parent.
5. No cluster may have a parent in a lower tier.
6. A Tier 2 Sub-cluster MUST be semantically subordinate to its Tier 1 Pillar.
7. A Tier 3 Micro-cluster MUST be semantically subordinate to its Tier 2 Sub-cluster.

**Phase 7C — Hierarchy Assignment Process**

1. Start with all Pillar clusters — place at Tier 1.
2. For each non-pillar cluster: test against each Pillar cluster's topic scope.
   - If the cluster is a focused sub-topic of the Pillar → assign as Sub-cluster.
   - If it fits no Pillar directly → create a new Pillar (if it qualifies) or assign to the nearest-scope Pillar.
3. For each Sub-cluster: test against other Sub-clusters for further nesting potential.
   - If a Sub-cluster is a highly specific sub-topic of another Sub-cluster → demote to Micro-cluster.

**Phase 7D — Hierarchy ID Assignment**

Assign structured cluster IDs:

| Tier        | ID Format    | Example   |
|-------------|--------------|-----------|
| Pillar      | `C-P[N]`     | `C-P01`   |
| Sub-cluster | `C-S[N]`     | `C-S01`   |
| Micro-cluster | `C-M[N]`   | `C-M01`   |

IDs are sequential across their tier and do not reset between parent clusters.

**Phase 7E — Hierarchy Edge Cases**

| Scenario                                                        | Resolution                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
| A cluster qualifies as Pillar but has no logical Sub-clusters   | Keep as Pillar. Sub-clusters may be created later as keyword research expands.   |
| A Sub-cluster grows larger than its parent Pillar               | Promote the Sub-cluster to Pillar tier. Re-evaluate the former Pillar's tier.   |
| Two Pillar clusters appear to be sub-topics of each other       | The broader one retains Pillar status. The narrower becomes a Sub-cluster.      |
| A Micro-cluster's keywords could belong under two Sub-clusters  | Assign to the Sub-cluster with stronger semantic alignment. Cross-reference the other. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ TF-IDF SIMULATION FOR KEYWORD IMPORTANCE

This skill does not have access to live search volume data. It applies a TF-IDF-inspired logic to prioritize keywords within clusters — weighting those that are conceptually unique and strategically differentiated over generic head terms.

**Term Frequency (TF) Simulation — Within-Cluster Relevance:**

A keyword's TF-equivalent score within its cluster is its conceptual centrality — how central is this keyword to the cluster's core topic?

| Centrality Level | Condition                                                              | TF Weight |
|------------------|------------------------------------------------------------------------|-----------|
| **HIGH**         | Keyword directly expresses the cluster's core problem or goal         | 1.0       |
| **MEDIUM**       | Keyword is closely related to the core but covers a sub-aspect        | 0.6       |
| **LOW**          | Keyword is a peripheral variation or edge case within the cluster     | 0.3       |

**Inverse Document Frequency (IDF) Simulation — Cross-Cluster Uniqueness:**

A keyword's IDF-equivalent score reflects how uniquely it belongs to one cluster versus appearing generically across many topic areas:

| Uniqueness Level | Condition                                                              | IDF Weight |
|------------------|------------------------------------------------------------------------|------------|
| **HIGHLY UNIQUE** | Keyword is strongly specific to this cluster's topic — rarely applicable elsewhere | 1.0 |
| **MODERATELY UNIQUE** | Keyword has primary relevance to this cluster but could have secondary relevance elsewhere | 0.7 |
| **GENERIC**      | Keyword is applicable to many topics — provides little differentiating signal | 0.3 |

**Combined Importance Score:**

`Importance = TF_weight × IDF_weight`

| Combined Score | Label  | Meaning in Cluster                                                        |
|----------------|--------|---------------------------------------------------------------------------|
| 0.7–1.0        | HIGH   | Core keyword — represents the cluster's most important content territory  |
| 0.4–0.69       | MEDIUM | Supporting keyword — adds depth but not foundational to cluster identity  |
| < 0.4          | LOW    | Peripheral keyword — enriches but does not define the cluster             |

**Application:**

- HIGH weight keywords in a cluster are used to name the cluster and define its Pillar Topic label.
- LOW weight generic keywords (score < 0.3) — if present in multiple clusters — are flagged for cluster reassignment. A keyword so generic it belongs in every cluster belongs in none.

---

### ▸ INTENT-WEIGHTED CLUSTERING

Weight the cluster formation process by the strategic value of the intent distribution — not all clusters with equal keyword counts are equally valuable.

**Intent Weighting Matrix:**

| Funnel Stage Represented | Weight Applied to Cluster Strength |
|--------------------------|------------------------------------|
| BOFU present             | +30%                               |
| MOFU present             | +20%                               |
| TOFU present             | +10%                               |
| All three present        | +20% bonus (full funnel bonus)     |

A cluster with 5 TOFU keywords receives a lower intent weight than a cluster with 3 TOFU + 1 MOFU + 1 BOFU keywords — because the latter has conversion potential.

**Intent-Weighted Cluster Priority:**

After strength scoring (below), apply intent weighting to produce a final Content Priority:

| Strength Score | Full Funnel | Content Priority |
|----------------|-------------|------------------|
| 70–100         | YES         | HIGH             |
| 70–100         | NO          | MEDIUM           |
| 40–69          | YES         | MEDIUM           |
| 40–69          | NO          | LOW              |
| < 40           | Any         | LOW              |

---

### ▸ CLUSTER STRENGTH SCORING

Every cluster receives a Cluster Strength Score (0–100) that reflects its strategic value and architectural stability.

**Strength Score Formula:**

| Factor                            | Weight | Scoring Rule                                                                      |
|-----------------------------------|--------|-----------------------------------------------------------------------------------|
| **Keyword density**               | 20%    | `(cluster keywords / max keywords in any cluster) × 20`                          |
| **BOFU keyword density**          | 25%    | `(BOFU keywords / total cluster keywords) × 25`                                  |
| **Full funnel coverage**          | 20%    | FULL = 20 | PARTIAL (1 stage missing) = 10 | MINIMAL (2+ missing) = 0          |
| **Niche specificity**             | 15%    | HIGH specificity = 15 | GENERAL topic = 7 | BROAD topic = 0                   |
| **Gap potential**                 | 20%    | HIGH gap (underserved topic) = 20 | MEDIUM = 10 | LOW (crowded SERP) = 0      |

`Cluster_Strength = Σ(factor_score × weight)`

**Strength Score Interpretation:**

| Score    | Label        | Strategic Implication                                      |
|----------|--------------|------------------------------------------------------------|
| 80–100   | CRITICAL     | Highest priority — build content here immediately          |
| 60–79    | HIGH         | Build in first content cycle                               |
| 40–59    | MEDIUM       | Build in second cycle after CRITICAL and HIGH              |
| 20–39    | LOW          | Build after higher-priority clusters are well-covered      |
| 0–19     | DEPRIORITIZE | Revisit after site authority is established                |

---

### ▸ SEMANTIC FIELD COMPLETENESS CHECK

After all clusters are formed, verify that each cluster's keyword set covers the full semantic field of its topic — that no major sub-topic is conspicuously absent.

**Semantic Field Check Protocol:**

For each cluster:
1. Identify the cluster's core concept.
2. Enumerate the sub-topics that a genuinely comprehensive treatment of this core concept would include.
3. Verify which sub-topics have at least one keyword in the cluster.
4. Flag any major sub-topic with no keyword coverage as SEMANTIC GAP.
5. Report SEMANTIC GAPS to the calling workflow as keyword expansion candidates.

**Semantic Gap Reporting Format:**

```
SEMANTIC GAP DETECTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster         : [Cluster Name]
Gap Sub-Topic   : [what is missing]
Importance      : [HIGH / MEDIUM / LOW]
Recommendation  : Add keyword "[suggested keyword]" to this cluster
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Semantic gaps are forwarded to `keyword-discovery.skill` if expansion is needed, or flagged for `gap-opportunity-engine.skill` as content opportunities.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Keyword in Multiple Clusters

Every keyword in the final output must appear in exactly one cluster. After Step 6 (cluster validation), run a cross-cluster keyword scan:

```
FOR EACH keyword in output:
  COUNT cluster memberships
  IF count > 1:
    → Retain in cluster with highest semantic alignment
    → Remove from all other clusters
    → Log the removal and the kept cluster
```

### Rule 2 — No Duplicate Clusters

No two clusters may have the same strategic purpose, even if they carry different names. If two clusters serve the same content goal for the same audience at the same funnel stages → they are duplicates → merge them (Step 6C).

Cluster name similarity alone is not evidence of duplicate purpose. Cluster purpose similarity — assessed through their keyword sets — is the test.

### Rule 3 — No Vague Clusters

A cluster is vague if:
- Its name is a generic descriptor ("General," "Other," "SEO Topics").
- Its description cannot be written in one specific sentence without using the word "various" or "different."
- Its keyword set spans 3+ unrelated problem spaces.

Vague clusters must be split into two or more well-defined clusters before output is produced.

### Rule 4 — Cross-Session Deduplication

Before producing any cluster, check `memory-controller.skill` for existing clusters:
- If an identical cluster exists in memory → load it from memory → expand with new keywords rather than recreating.
- If a similar cluster exists → align naming → merge new keywords into the existing cluster.
- Never create a new cluster that duplicates the purpose of a cluster already in memory.

### Rule 5 — Pre-Cluster Hint Deduplication

If two keywords share the same `pre_cluster_hint` but semantic analysis in Step 2 places them in different clusters → override the hint → log the override. Pre-cluster hints are not guaranteed to be correct; semantic analysis takes precedence.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Clustering Behaviors

| Prohibited Behavior                                                                         | Reason                                                                       |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| Creating a cluster with only 1 keyword                                                      | A single keyword is not a cluster — it is an orphaned keyword with a label  |
| Naming a cluster after a keyword instead of a topic theme                                   | Cluster names are architecture labels — not keyword targets                  |
| Grouping keywords by word matching instead of intent matching                                | The foundational error this skill exists to prevent                         |
| Creating separate clusters for each keyword-discovery dimension (all how-to keywords together) | Dimensions are generation categories — not semantic clusters              |
| Assigning a vague name like `"General CRM"` or `"Miscellaneous SEO"`                      | Every cluster must have a clear, bounded purpose — vagueness is prohibited  |
| Leaving any keyword unassigned after Step 7                                                  | Zero orphans is a non-negotiable requirement                                |
| Merging clusters to reduce total cluster count without a semantic justification              | Merges must be driven by genuine overlap — not by a desire for fewer clusters |
| Assigning pillar status to a cluster that does not meet all five pillar criteria            | Pillar designation is earned through the qualification protocol             |
| Writing a cluster description that lists keywords instead of describing strategic value      | Descriptions must articulate what the cluster achieves — not what it contains |
| Skipping the funnel balance assessment for any cluster                                       | Every cluster must be evaluated for funnel coverage — no exceptions         |

### Required Clustering Behaviors

- Every cluster must have: ID, Name, Tier, Parent, Keywords by funnel stage, Strength Score, and Description.
- Every keyword must appear in exactly one cluster in the final output.
- Every boundary resolution (merge, cross-reference, keep) must be logged in the Merge Log.
- Every orphan keyword must be logged and resolved before the output is finalized.
- Every cluster must receive a Strength Score calculated using the defined formula.
- Pillar clusters must receive a Pillar Topic label.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ keyword-discovery.skill

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary input)                                                        |
| **Receives**     | Complete deduplicated keyword universe with intent, funnel, category, strategic reason, and pre-cluster hints |
| **Used for**     | Step 2 (semantic grouping seed data), Step 3 (funnel sub-clustering), Step 5 (pillar evaluation) |

---

### ▸ deduplication-engine.skill

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream quality gate (runs before this skill)                                              |
| **Ensures**      | Keyword universe is clean before clustering begins                                          |
| **Called again** | After clustering — to verify no cross-cluster keyword duplicates remain                    |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before clustering, WRITE after output finalization                     |
| **Reads**        | Existing cluster names, existing cluster keywords, prior merges, pillar designations        |
| **Writes**       | Complete cluster architecture, hierarchy map, merge log, strength scores, semantic gaps     |
| **Memory layers**| keyword-memory.skill (cluster assignments), strategy-memory.skill (cluster architecture)   |

---

### ▸ query-graph.skill

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering → query-graph (output consumer)                                        |
| **Sends**        | Cluster hierarchy map, cross-reference relationships, cluster strength scores              |
| **Used for**     | Building the topic authority graph and internal link structure recommendations             |

---

### ▸ opportunity-engine.skill (gap-opportunity-engine.skill)

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering → opportunity-engine (output consumer)                                 |
| **Sends**        | Cluster architecture, gap potential scores, funnel balance status, semantic gaps           |
| **Used for**     | Identifying which clusters represent the highest-priority content creation opportunities   |

---

### ▸ authority-engine.skill (authority-mapping.skill)

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering → authority-engine (output consumer)                                   |
| **Sends**        | Pillar cluster designations, sub-cluster hierarchy, cross-reference relationships          |
| **Used for**     | Designing the authority-building content sequence and internal linking strategy            |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering output consumed by content-generation-engine (indirectly via blueprint)|
| **Sends**        | Cluster name, cluster keywords, pillar topic, funnel balance                               |
| **Used for**     | Ensuring generated content is built around strategically validated cluster keywords        |

---

### ▸ keyword-memory.skill (via memory-controller)

| Property         | Detail                                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after output finalization)                                                           |
| **Writes**       | Cluster assignments for all keywords (updates lifecycle state to CLUSTERED)                |
| **Used for**     | Keyword lifecycle tracking; preventing re-clustering of already-clustered keywords         |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                        | Detection                                           | Recovery Action                                                                                       |
|-------------------------------------------------------------------------|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Input keyword list contains fewer than 10 keywords                     | Input pre-condition check fails                     | Halt. Flag: "Insufficient keywords for clustering (received [N], minimum 10). Request keyword expansion from keyword-discovery.skill." |
| All keywords share the same problem space (no differentiation)         | Step 2 produces 1 cluster for all keywords          | Flag. Narrow the primary keyword and re-request a focused keyword universe. |
| Memory controller unavailable                                           | READ query returns error                             | Proceed without existing cluster context. Flag: "Prior cluster data unavailable — duplicate cluster risk elevated." |
| A cluster fails pillar qualification but has no logical parent         | Hierarchy construction cannot place the cluster     | Assign as standalone Sub-cluster under a provisional Pillar with a broader topic label. Flag for user review. |
| Merge cascade results in fewer than 3 total clusters                   | Step 6 post-merge count < 3                         | Notify: "Merging produced [N] clusters — keyword universe may cover a narrow topic. Add more keywords or proceed?" Wait for response. |
| Orphan keywords found after Step 7 (all clustering passes complete)    | Coverage validation fails                           | Force-assign orphans using relaxed decision protocol. If no semantic connection found → create a new Micro-cluster. Log all force-assignments. |
| No cluster qualifies for Pillar status                                  | Step 5 — zero clusters meet all 5 criteria          | Flag closest-to-qualifying cluster as PROVISIONAL PILLAR. Note missing criteria. Recommend keyword expansion. |
| Cluster produces a description that cannot be written in one specific sentence | Step 4 quality check                          | Cluster is VAGUE — split it into two or more well-defined clusters with distinct purposes. |
| Existing memory clusters conflict with new clustering output            | Cross-session dedup finds purpose overlap           | Surface conflict to user. Offer: (1) adopt new clusters, (2) keep memory clusters, (3) merge. Wait for choice. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — GROUPING DECISION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
SEMANTIC GROUPING DECISION — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
5-QUESTION PROTOCOL (apply to each keyword pair):

Q1 (40%) — Do both keywords address the same underlying problem?
  YES → Strong same-cluster signal
  NO  → Different clusters

Q2 (30%) — Would one comprehensive article satisfy both searchers?
  YES → Confirm same cluster
  NO  → Different clusters

Q3 (20%) — Would a user searching one likely search the other?
  YES → Same cluster support
  NO  → May be different clusters

Q4 (5%) — Do both target the same audience?
  YES → Weak same-cluster support
  NO  → Different audience may mean different clusters

Q5 (5%) — Would both pages compete for the same SERP position?
  YES → Must be same cluster — cannibalization if separated
  NO  → May be different clusters

SCORING:
  Weighted sum ≥ 0.70 → SAME CLUSTER
  Weighted sum 0.50–0.69 → LIKELY SAME — verify
  Weighted sum < 0.50 → DIFFERENT CLUSTERS

ANTI-PATTERNS TO AVOID:
  ✗ Grouping by shared words (not intent)
  ✗ Grouping all TOFU keywords together
  ✗ Grouping by keyword-discovery dimension
  ✗ Separating semantically identical keywords
  ✗ Conflating industry-specific keywords across industries
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — CLUSTER STRENGTH CALCULATION WORKSHEET
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CLUSTER STRENGTH WORKSHEET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster Name: [Name]

Factor 1 — Keyword Count (20% weight)
  Cluster Keywords          : [N]
  Max Keywords (any cluster): [M]
  Raw Score                 : (N / M) × 20 = [score]

Factor 2 — BOFU Density (25% weight)
  BOFU Keywords             : [B]
  Total Cluster Keywords    : [N]
  Raw Score                 : (B / N) × 25 = [score]

Factor 3 — Full Funnel Coverage (20% weight)
  TOFU present?             : [YES / NO]
  MOFU present?             : [YES / NO]
  BOFU present?             : [YES / NO]
  Raw Score                 : FULL=20 | PARTIAL=10 | MINIMAL=0

Factor 4 — Niche Specificity (15% weight)
  Specificity Level         : [HIGH / GENERAL / BROAD]
  Raw Score                 : HIGH=15 | GENERAL=7 | BROAD=0

Factor 5 — Gap Potential (20% weight)
  Gap Level                 : [HIGH / MEDIUM / LOW]
  Raw Score                 : HIGH=20 | MEDIUM=10 | LOW=0

CLUSTER STRENGTH SCORE      : [sum of all factor scores] / 100
STRENGTH LABEL              : [CRITICAL / HIGH / MEDIUM / LOW / DEPRIORITIZE]
CONTENT PRIORITY            : [determined by Strength + Intent Weighting]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — PILLAR QUALIFICATION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PILLAR CLUSTER QUALIFICATION CRITERIA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ALL FIVE must be met for PILLAR designation:

1. KEYWORD DENSITY     → ≥5 keywords after normalization
2. FUNNEL COMPLETENESS → TOFU + MOFU + BOFU all present
3. STRATEGIC CENTRALITY → Covers a core business domain or major opportunity
4. TOPIC SPECIFICITY   → Specific enough to realistically own
5. CONTENT DEPTH       → Can support ≥3 distinct non-overlapping articles

NEAR-PILLAR (4 of 5 criteria met):
  → Designate as Sub-cluster
  → Note missing criterion
  → Flag as pillar expansion candidate

FORCED PILLAR IS PROHIBITED:
  → Never designate a pillar to fill a structural need
  → Pillar designation must be earned through all 5 criteria
  → A false pillar creates a false authority architecture
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: analysis/semantic-clustering.skill*
*Nexus SEO Operating System — Version 2.0.0*
