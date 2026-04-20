# SKILL: analysis/semantic-clustering.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Analysis / Semantic Architecture
# EXECUTION PRIORITY: POST-DISCOVERY — Runs immediately after keyword-discovery.skill output is finalized.
# POSITION: Transforms the raw keyword universe into a structured, hierarchical SEO content architecture.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **semantic architecture engine** of the Nexus system.

It receives a deduplicated, categorized keyword universe from `keyword-discovery.skill` and organizes it into a structured hierarchy of topic clusters — the SEO architecture on which all content planning, internal linking strategy, and authority building will be built.

This skill does not group keywords by word similarity. It does not look for keywords that share the same words and assume they belong together. It groups keywords by **user intent, problem space, and topic context** — the way a human being with domain expertise would intuitively organize research, not the way a search algorithm would tokenize strings.

The output of this skill is the skeleton of the site's content architecture: a set of named, described, hierarchically positioned clusters that tell every downstream skill exactly what to build, in what order, and at what depth.

### Primary Responsibilities

| Responsibility                     | Description                                                                                                 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **Keyword Normalization**          | Clean and standardize all incoming keywords before any grouping logic is applied                           |
| **Semantic Grouping**              | Group keywords by shared problem, shared user goal, and shared topic context — never by shared words        |
| **Intent-Based Sub-Clustering**    | Within each cluster, organize keywords by funnel stage (TOFU, MOFU, BOFU)                                 |
| **Cluster Naming**                 | Assign precise, human-readable, topic-level names to every cluster                                         |
| **Cluster Description**            | Write a single strategic description for each cluster articulating what it covers and why it matters       |
| **Pillar Identification**          | Mark high-value, full-funnel clusters as pillar clusters for content architecture hierarchy                 |
| **Boundary Validation**            | Detect overlap between clusters and resolve via merge or cross-reference                                   |
| **Hierarchy Construction**         | Build the three-tier hierarchy: Pillar → Sub-cluster → Micro-cluster                                      |
| **Coverage Validation**            | Ensure no keyword is orphaned and no cluster is strategically inviable                                    |
| **Cluster Importance Scoring**     | Score every cluster by keyword density, BOFU presence, and gap potential                                  |

### The Foundational Principle of This Skill

> Clustering is not a word-matching exercise. It is a **user intent modeling exercise.**
>
> Two keywords belong in the same cluster if and only if:
> - They address the same underlying problem or goal.
> - A single piece of content could meaningfully serve both search intents simultaneously.
> - A user who searches one would likely search the other in the same decision journey.
>
> Two keywords that share no common words may belong in the same cluster.
> Two keywords that share the same root word may belong in different clusters.
> Word overlap is never the primary clustering signal.

### What This Skill Is NOT

- It is **not** a keyword grouping tool based on string similarity or root word matching.
- It is **not** a content generator. It creates architecture — it does not produce content.
- It is **not** a SERP analyzer. It does not validate clusters against live search results.
- It is **not** an authority builder. It identifies pillar clusters — `authority-mapping.skill` builds on them.
- It is **not** a flat list producer. The output is always a hierarchy — pillar, sub-cluster, micro-cluster.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input — Keyword Universe (from keyword-discovery.skill)

The complete structured output of `keyword-discovery.skill`, including:

| Field                  | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| `keyword`              | The full, exact keyword phrase                                              |
| `intent`               | Informational / Commercial / Transactional                                  |
| `funnel`               | TOFU / MOFU / BOFU                                                          |
| `category`             | Educational / Comparison / Alternative / Problem-Solution / Long-Tail / AEO / Semantic / Niche |
| `strategic_reason`     | Why the keyword matters and what gap it fills                               |
| `pre_cluster_hint`     | Optional cluster label assigned by `keyword-discovery.skill` (advisory only)|

### Optional Input — Existing Clusters (from memory-controller.skill)

If the project has previously generated clusters, they are loaded as context:

| Field                     | Description                                                              |
|---------------------------|--------------------------------------------------------------------------|
| `existing_cluster_names`  | Names of clusters already built in prior sessions                        |
| `existing_cluster_keywords` | Keywords already assigned to existing clusters                         |
| `existing_pillar_clusters`| Which clusters are already designated as pillars                         |
| `merged_cluster_history`  | Record of prior merges (to avoid re-proposing merged clusters)           |

### Input Minimum Requirements

| Requirement                            | Value                                  | Action if Not Met                                    |
|----------------------------------------|----------------------------------------|------------------------------------------------------|
| Minimum keywords in input              | 10 unique keywords                     | Trigger failsafe — request expansion or more input   |
| All keywords must have `intent` field  | Non-null                               | Infer intent before proceeding                       |
| All keywords must have `funnel` field  | Non-null                               | Infer funnel stage before proceeding                 |
| Input must come from keyword-discovery | Verified via source tag or packet field| Warn if source is unverified; proceed with caution   |

### Input Pre-Conditions

1. **Is the keyword universe complete?** Check that all keywords survived deduplication (Step 5 in `keyword-discovery.skill`). If the input appears to be a raw, un-deduplicated list → run normalization Step 1 with extra rigor before proceeding.
2. **Are there existing clusters in memory?** Query `memory-controller.skill` before beginning — existing clusters constrain the merging logic and prevent re-creating architecture that already exists.
3. **Is the input list below 10 keywords?** → Trigger failsafe immediately. Do not attempt to cluster fewer than 10 keywords.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Structured Cluster Architecture

```
NEXUS SEMANTIC CLUSTER ARCHITECTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID            : [ID]
Primary Input Source  : keyword-discovery.skill
Keywords Received     : [count]
Keywords Clustered    : [count]
Orphan Keywords       : [count — must be 0 after coverage check]
Total Clusters        : [count]
  Pillar Clusters     : [count]
  Sub-Clusters        : [count]
  Micro-Clusters      : [count]
Generated By          : analysis/semantic-clustering.skill
Timestamp             : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CLUSTER ARCHITECTURE TABLE

| Cluster ID | Cluster Name              | Tier    | Parent Cluster     | Keywords | Intent Mix         | Pillar | Score | Description                          |
|------------|---------------------------|---------|--------------------|----------|--------------------|--------|-------|--------------------------------------|
| C-01       | [Name]                    | Pillar  | —                  | [N]      | TOFU:N MOFU:N BOFU:N | Yes  | [0-100] | [one-sentence description]         |
| C-02       | [Name]                    | Sub     | C-01               | [N]      | TOFU:N MOFU:N BOFU:N | No   | [0-100] | [one-sentence description]         |
| C-03       | [Name]                    | Micro   | C-02               | [N]      | TOFU:N MOFU:N BOFU:N | No   | [0-100] | [one-sentence description]         |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CLUSTER DETAIL RECORDS

[Repeated for each cluster:]

CLUSTER: [Cluster ID] — [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Tier                : [Pillar / Sub / Micro]
Parent Cluster      : [Parent Cluster Name or ROOT]
Cluster Score       : [0–100]
Pillar Status       : [YES / NO]
Description         : [one sentence — what it covers and why it matters]

Keywords by Funnel Stage:
  TOFU:
    → [keyword] | [intent] | [category]
    → [keyword] | [intent] | [category]
  MOFU:
    → [keyword] | [intent] | [category]
    → [keyword] | [intent] | [category]
  BOFU:
    → [keyword] | [intent] | [category]
    → [keyword] | [intent] | [category]

Total Keywords      : [count]
Funnel Balance      : [FULL / PARTIAL — missing: TOFU/MOFU/BOFU]
Cross-References    : [related cluster names, if any]
Content Priority    : [HIGH / MEDIUM / LOW]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HIERARCHY MAP

[Pillar Cluster Name] (C-01)
  ├── [Sub-Cluster Name] (C-02)
  │     ├── [Micro-Cluster Name] (C-04)
  │     └── [Micro-Cluster Name] (C-05)
  └── [Sub-Cluster Name] (C-03)
        └── [Micro-Cluster Name] (C-06)

[Pillar Cluster Name] (C-07)
  ├── [Sub-Cluster Name] (C-08)
  ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BOUNDARY RESOLUTION LOG

| Action  | Cluster A     | Cluster B     | Reason                                | Resolution                |
|---------|---------------|---------------|---------------------------------------|---------------------------|
| MERGE   | [Name]        | [Name]        | [N] keyword overlap; same user goal   | Combined into [New Name]  |
| X-REF   | [Name]        | [Name]        | Partial overlap; distinct user goals  | Cross-referenced          |
| KEEP    | [Name]        | [Name]        | No meaningful overlap                 | No action needed          |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ORPHAN RESOLUTION LOG

| Keyword          | Reason Orphaned              | Resolution                          |
|------------------|------------------------------|-------------------------------------|
| [keyword]        | [no cluster fit found]       | [assigned to / new micro-cluster]   |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
COVERAGE VALIDATION RESULT

Total Keywords In    : [count]
Total Keywords Out   : [count]
Unassigned Keywords  : 0 [required]
Pillar Clusters      : [count] — [names]
Full-Funnel Clusters : [count]
Partial-Funnel Clusters: [count] — [names and what's missing]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — NORMALIZE KEYWORDS

Before any semantic grouping begins, standardize all incoming keywords to ensure the grouping logic operates on clean, consistent data.

**Normalization Operations:**

| Operation                          | Rule                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------|
| **Lowercase all keywords**         | `"Best CRM Software"` → `"best crm software"`                                            |
| **Trim whitespace**                | Remove leading, trailing, and double-internal spaces                                     |
| **Remove punctuation inconsistencies** | Normalize hyphens, apostrophes, and quotation marks to standard form                |
| **Expand abbreviations**           | `"b2b"` → `"b2b"` (keep), `"crm"` → `"crm"` (keep as known acronym), flag unknown abbreviations |
| **Normalize plural/singular variants** | Track both forms but treat as equivalent for clustering purposes (do not remove either — they may have distinct SERP behavior) |
| **Identify and flag near-identical variants** | `"CRM for small business"` and `"CRM for small businesses"` → flag as near-identical; keep more natural search form |
| **Verify all five fields are present** | keyword, intent, funnel, category, strategic_reason — if any are null, attempt inference before proceeding |
| **Re-run deduplication check**     | Even though `keyword-discovery.skill` deduplicated its output, run a lightweight exact-match check again — if any duplicates arrived in the input, remove them here and log |

**Normalization Output:**

```
NORMALIZATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keywords Received       : [count]
Normalization Changes   : [count]
  Lowercased            : [count]
  Whitespace Cleaned    : [count]
  Near-Identical Flagged: [count — kept one form each]
  Field Gaps Inferred   : [count]
  Residual Duplicates   : [count — removed]
Keywords Ready for Clustering : [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — SEMANTIC GROUPING

This is the core intellectual step of this skill. Group keywords by shared user intent, problem space, and topic context — not by shared words or structural similarity.

**Semantic Grouping Protocol:**

**Phase 2A — Problem Space Identification**

For each keyword, identify the underlying problem or goal the searcher is attempting to address. This is the semantic anchor for grouping.

Derive the problem space from the keyword's `strategic_reason` field and `intent` classification — these encode the deeper meaning of the keyword beyond its surface phrasing.

| Keyword                                       | Surface Topic          | Underlying Problem Space                              |
|-----------------------------------------------|------------------------|-------------------------------------------------------|
| `"best crm software"`                         | CRM software           | Finding and selecting the right CRM tool              |
| `"top crm tools for sales teams"`             | CRM tools              | Finding and selecting the right CRM tool              |
| `"hubspot vs salesforce"`                     | HubSpot vs Salesforce  | Finding and selecting the right CRM tool              |
| `"how to choose a crm"`                       | CRM selection          | Finding and selecting the right CRM tool              |
| → ALL FOUR belong in the same cluster         |                        |                                                       |
| `"how to set up a crm"`                       | CRM setup              | Implementing a CRM after the purchase decision        |
| `"crm implementation guide"`                  | CRM implementation     | Implementing a CRM after the purchase decision        |
| `"crm onboarding best practices"`             | CRM onboarding         | Implementing a CRM after the purchase decision        |
| → THESE THREE belong in a SEPARATE cluster    |                        |                                                       |

**Phase 2B — Grouping Decision Matrix**

For every pair of keywords, apply this decision matrix to determine cluster membership:

| Question                                                                                     | Yes → | No →  |
|----------------------------------------------------------------------------------------------|-------|-------|
| Do both keywords address the same underlying problem?                                        | Same cluster (strong signal) | Different clusters |
| Would a single piece of content comprehensively serve both search intents?                   | Same cluster (confirm)       | Different clusters |
| If a user searches keyword A, would they likely also search keyword B in the same session?   | Same cluster (confirm)       | Possibly different |
| Do both keywords appear at the same or adjacent funnel stages?                               | Same cluster (support)       | May be different sub-clusters within the same parent |
| Do both keywords, if targeted in the same article, create keyword cannibalization risk?      | SAME cluster — do not separate | May be different clusters |

**Cannibalization Logic:** Two keywords that would compete for the same ranking position on the SERP if targeted in separate articles MUST be in the same cluster — creating separate content for them would cause self-cannibalization. This is one of the most important grouping signals.

**Phase 2C — Grouping Execution**

1. Start with the keyword that has the highest strategic importance (highest BOFU presence + most specific intent).
2. Assign it to Cluster 1 with a provisional cluster label.
3. For each subsequent keyword, test it against every existing cluster using the Phase 2B decision matrix.
4. If the keyword fits an existing cluster → assign it there.
5. If the keyword does not fit any existing cluster → create a new cluster and assign it.
6. Continue until every keyword is assigned.
7. No keyword may remain unassigned after this phase.

**Phase 2D — Pre-Cluster Hint Integration**

Use the `pre_cluster_hint` field from `keyword-discovery.skill` as a starting point — not a final answer. The pre-cluster hint is advisory only. If the semantic grouping logic from Phases 2A–2C contradicts a pre-cluster hint, the semantic grouping logic takes precedence. Log any deviation from pre-cluster hints.

**Semantic Grouping Anti-Patterns (These Are Errors):**

| Anti-Pattern                                                                         | Why It Is Wrong                                               |
|--------------------------------------------------------------------------------------|---------------------------------------------------------------|
| Grouping `"crm software"` and `"crm database"` together because both contain `"crm"` | Shared words ≠ shared intent. These serve different problems. |
| Separating `"best crm"` and `"top crm tools"` into different clusters               | Same user goal. Same decision-stage intent. Same cluster.     |
| Grouping `"how to set up crm"` with `"why use a crm"` because both are informational | Different problem spaces. Different content angles. Separate. |
| Creating one cluster for all TOFU keywords regardless of topic                       | Funnel stage is a sub-dimension of clusters, not the primary grouping signal. |
| Grouping `"crm for real estate"` with `"crm for healthcare"` because both are niche  | Different industries = different user contexts = different clusters or sub-clusters. |

---

### ▸ STEP 3 — INTENT-BASED SUB-CLUSTERING

After semantic groups are established, organize the keywords within each cluster by funnel stage. This produces the internal structure of each cluster — a three-layer intent stack that maps directly to the content pieces a cluster will eventually support.

**Sub-Clustering Protocol:**

For each cluster from Step 2, partition its keywords into three intent layers:

| Intent Layer | Funnel Stage | What It Represents                                                                 |
|--------------|--------------|------------------------------------------------------------------------------------|
| **Awareness** | TOFU        | Keywords that target users who are just becoming aware of the problem or topic     |
| **Consideration** | MOFU   | Keywords that target users who are comparing solutions or evaluating options        |
| **Decision** | BOFU        | Keywords that target users who are ready to take action                            |

**Sub-Clustering Rules:**

1. Every keyword in the cluster must be assigned to exactly one intent layer.
2. The intent layer assignment must match the keyword's `funnel` field from the input — do not reassign funnel stages at this step.
3. If a keyword has ambiguous funnel assignment (e.g., `"is CRM worth it"` — Commercial intent but could serve MOFU or BOFU), retain the assignment from `keyword-discovery.skill`. If that assignment was also flagged as ambiguous, apply the following default:
   - Commercial + comparison signal → MOFU
   - Commercial + price/trial signal → BOFU
   - Informational + solution-seeking signal → TOFU

**Funnel Balance Assessment (per cluster):**

| Status               | Condition                                                    | Implication                                                |
|----------------------|--------------------------------------------------------------|------------------------------------------------------------|
| **FULL FUNNEL**      | At least 1 keyword in each of TOFU, MOFU, BOFU              | Cluster can support a complete content strategy            |
| **TOFU-HEAVY**       | ≥70% of keywords are TOFU                                    | Cluster drives traffic but may not convert well            |
| **BOFU-HEAVY**       | ≥70% of keywords are BOFU                                    | Cluster converts well but may not attract new audiences    |
| **MISSING TOFU**     | Zero TOFU keywords                                           | No awareness entry point — cluster may be difficult to rank |
| **MISSING BOFU**     | Zero BOFU keywords                                           | Cluster has no conversion potential — flag for enrichment  |
| **MISSING MOFU**     | Zero MOFU keywords                                           | No consideration bridge — funnel gap between awareness and decision |

Clusters with `MISSING TOFU`, `MISSING MOFU`, or `MISSING BOFU` are flagged and recommendations for enrichment keywords are noted in the cluster detail record.

---

### ▸ STEP 4 — CLUSTER NAMING

Assign a precise, human-readable, topic-level name to every cluster.

**Naming Principles:**

| Principle                           | Rule                                                                                              |
|-------------------------------------|---------------------------------------------------------------------------------------------------|
| **Topic, not keyword**              | The name describes the content theme, not the phrasing of any individual keyword                 |
| **Broad but precise**               | Wide enough to encompass all keywords in the cluster; narrow enough to be meaningfully distinct   |
| **Human-readable**                  | A non-SEO professional could understand the cluster's purpose from the name alone                |
| **2–5 words**                       | Short enough to be a navigation label; long enough to be specific                               |
| **No keyword stuffing**             | The cluster name is not a concatenation of keywords it contains                                  |
| **Present tense, noun phrase form** | `"CRM Software Selection"` not `"Selecting CRM Software"` or `"How to Select CRM"`              |

**Cluster Naming Examples:**

| Keywords in Cluster                                                                  | Bad Cluster Name                        | Good Cluster Name              |
|--------------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
| `"best crm"`, `"top crm tools"`, `"hubspot vs salesforce"`, `"how to choose a crm"` | `"best crm tools comparison list"`      | `"CRM Software Selection"`     |
| `"crm for real estate"`, `"real estate crm software"`, `"best crm for realtors"`    | `"real estate crm tools best"`          | `"CRM for Real Estate"`        |
| `"how to set up crm"`, `"crm implementation guide"`, `"crm onboarding practices"`   | `"crm setup guide implementation step"` | `"CRM Setup and Implementation"` |
| `"what is a crm"`, `"what does crm do"`, `"how does crm work"`                      | `"crm questions what is"`               | `"CRM Fundamentals"`           |
| `"crm without monthly fees"`, `"free crm"`, `"affordable crm for startups"`         | `"cheap free crm options"`              | `"Free and Affordable CRM"`    |

**Naming Conflict Resolution:**

If two clusters in the same hierarchy produce names that are nearly identical:
- Re-examine whether they should be merged (Step 7).
- If they must remain separate → differentiate names with a clarifying modifier.
- Example: `"CRM for Small Business"` and `"CRM for Enterprise"` — both are valid distinct clusters despite sharing the CRM prefix.

---

### ▸ STEP 5 — CLUSTER DESCRIPTION

Write a single-sentence description for every cluster.

**Description Requirements:**

Every description must answer two questions in one sentence:
1. What does this cluster cover? (scope)
2. Why does it matter strategically? (value)

**Description Formula:**

`[This cluster covers] [scope statement], [which/enabling/positioning] [strategic value statement].`

**Description Examples:**

| Cluster Name                  | Poor Description                                    | Good Description                                                                                                                    |
|-------------------------------|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `"CRM Software Selection"`    | `"Keywords about choosing CRM software."`           | `"This cluster covers the full evaluation journey for CRM buyers, capturing high-intent comparison traffic at the moment of maximum purchase readiness."` |
| `"CRM Fundamentals"`          | `"Basic CRM questions for beginners."`              | `"This cluster targets first-touch awareness searchers with no prior CRM knowledge, establishing topical authority at the entry point of the buyer journey."` |
| `"CRM for Real Estate"`       | `"CRM content for the real estate industry."`       | `"This cluster captures a high-conversion niche segment where CRM search volume is underserved and competition is significantly lower than the general CRM market."` |
| `"CRM Setup and Implementation"` | `"Guides for setting up a CRM system."`          | `"This cluster serves post-purchase users seeking implementation success, reducing churn risk while also attracting searchers in active vendor evaluation who want proof of deployability."` |

**Description Anti-Patterns:**

- `"This cluster has keywords about X"` — describes the cluster's contents, not its value.
- `"Important for SEO"` — vague. Not acceptable.
- `"Targets users who want to know about X"` — restates the obvious. Not acceptable.
- Any description exceeding two sentences — must be a single sentence.

---

### ▸ STEP 6 — PILLAR IDENTIFICATION

Evaluate every cluster against the pillar designation criteria. Pillar clusters are the strategic anchors of the content architecture — they define the major topic domains the site will own.

**Pillar Qualification Criteria:**

A cluster qualifies as a Pillar cluster if it meets ALL of the following conditions:

| Criterion                              | Threshold                                                           |
|----------------------------------------|---------------------------------------------------------------------|
| **Keyword density**                    | ≥5 keywords in the cluster after deduplication                      |
| **Funnel completeness**                | Has at least 1 keyword in each of TOFU, MOFU, and BOFU             |
| **Strategic value**                    | The cluster covers a topic that represents a core business domain or high-volume content opportunity |
| **Cluster specificity**                | The cluster is specific enough to be owned — not so broad that ranking for it is implausible |
| **Content depth potential**            | The cluster can support at minimum 3 distinct articles at different funnel stages |

**Pillar Designation Rules:**

- A Pillar cluster always sits at the top of the hierarchy (Tier 1).
- Sub-clusters and Micro-clusters are always children of a Pillar or Sub-cluster.
- There is no minimum or maximum number of Pillar clusters — designate as many as genuinely qualify.
- If no cluster qualifies as a Pillar (uncommon, but possible with very small keyword sets) → flag for expansion and note which clusters are closest to qualifying.
- A Pillar cluster is not necessarily the largest cluster by keyword count. A small, highly strategic cluster with full funnel coverage and high BOFU density can be a stronger Pillar than a large TOFU-heavy cluster.

**Pillar Cluster Marker in Output:**

Each Pillar cluster receives:
- `Pillar: YES` in the architecture table.
- A `Content Priority: HIGH` designation.
- An internal note in the hierarchy map marking it as a root node.

---

### ▸ STEP 7 — BOUNDARY VALIDATION

Inspect all clusters for boundary integrity. Clusters must have clear, non-overlapping purposes. Where overlap exists, resolve it explicitly.

**Overlap Detection Protocol:**

**Phase 7A — Keyword Overlap Check**

For every pair of clusters, count the number of keywords that appear in both.

| Overlap Level         | Threshold         | Action                                                                 |
|-----------------------|-------------------|------------------------------------------------------------------------|
| **High overlap**      | ≥3 shared keywords | MERGE the two clusters into one. Choose the broader cluster name.     |
| **Moderate overlap**  | 1–2 shared keywords | CROSS-REFERENCE the clusters. The shared keyword stays in the cluster where it has the highest strategic alignment. The other cluster receives a cross-reference note. |
| **No overlap**        | 0 shared keywords  | KEEP both clusters as-is. No action required.                         |

**Phase 7B — Purpose Overlap Check**

Even if clusters share no keywords, their strategic purposes may overlap — meaning content created for one cluster would directly compete with content from the other.

Purpose overlap detection:
1. Compare cluster descriptions for each pair.
2. If two clusters would produce content targeting the same SERP positions → purpose overlap detected.
3. Apply the same threshold logic as Phase 7A (MERGE or CROSS-REFERENCE).

**Phase 7C — Merge Execution**

When two clusters are merged:
1. Combine all keywords from both clusters into the merged cluster.
2. Assign the merged cluster the name of the broader cluster (or create a new name that encompasses both).
3. Re-run intent sub-clustering (Step 3) on the merged keyword set.
4. Re-run funnel balance assessment.
5. Re-evaluate pillar status for the merged cluster.
6. Log the merge in the Boundary Resolution Log.
7. Remove the dissolved cluster from all output tables.

**Phase 7D — Cross-Reference Execution**

When two clusters are cross-referenced:
1. Shared keywords remain in their primary cluster (highest strategic alignment).
2. Each cluster receives a `Cross-References` field listing the other cluster.
3. This cross-reference is used by `authority-mapping.skill` to plan internal linking between the two clusters.
4. Cross-references are NOT merges — the clusters remain structurally distinct.

**Boundary Validation Anti-Patterns:**

| Error                                                                             | Consequence                                                    |
|-----------------------------------------------------------------------------------|----------------------------------------------------------------|
| Leaving 3+ keyword overlap unresolved                                             | Two clusters will produce competing content; self-cannibalization risk |
| Merging clusters that share only surface-level phrasing, not semantic purpose     | Over-merging destroys specificity and creates unfocused pillar content |
| Cross-referencing instead of merging when overlap is high (≥3 keywords)           | Creates structural fragmentation in the content architecture   |
| Allowing a keyword to exist in two clusters simultaneously after boundary check   | Violates the single-cluster-membership rule                    |

---

### ▸ STEP 8 — HIERARCHY CREATION

Construct the three-tier cluster hierarchy that defines the content architecture structure.

**Hierarchy Tier Definitions:**

| Tier         | Label        | Definition                                                                                    |
|--------------|--------------|-----------------------------------------------------------------------------------------------|
| **Tier 1**   | Pillar       | A major topic domain. Supports a comprehensive pillar page or content hub.                    |
| **Tier 2**   | Sub-cluster  | A focused subtopic within a Pillar. Supports multiple supporting articles.                    |
| **Tier 3**   | Micro-cluster| A highly specific niche within a Sub-cluster. Supports 1–3 targeted articles.                |

**Hierarchy Construction Rules:**

1. All Pillar clusters occupy Tier 1. They have no parent.
2. Sub-clusters are assigned to the Pillar cluster whose topic most closely contains their focus.
3. Micro-clusters are assigned to the Sub-cluster whose topic most closely contains their focus.
4. No cluster may be its own parent.
5. No cluster may have a parent in a lower tier.
6. A Tier 2 Sub-cluster must be semantically subordinate to its Tier 1 Pillar — meaning the Pillar's topic encompasses the Sub-cluster's topic.
7. A Tier 3 Micro-cluster must be semantically subordinate to its Tier 2 Sub-cluster — same rule applies.

**Hierarchy Assignment Examples (CRM context):**

```
Tier 1 — Pillar: "CRM Software" (root)
  │
  ├── Tier 2 — Sub: "CRM Selection and Evaluation"
  │     ├── Tier 3 — Micro: "CRM Pricing and Value"
  │     └── Tier 3 — Micro: "CRM Vendor Comparison"
  │
  ├── Tier 2 — Sub: "CRM by Industry"
  │     ├── Tier 3 — Micro: "CRM for Real Estate"
  │     ├── Tier 3 — Micro: "CRM for Healthcare"
  │     └── Tier 3 — Micro: "CRM for Marketing Agencies"
  │
  └── Tier 2 — Sub: "CRM Setup and Implementation"
        └── Tier 3 — Micro: "CRM Data Migration"
```

**Hierarchy Edge Cases:**

| Scenario                                                       | Resolution                                                                           |
|----------------------------------------------------------------|--------------------------------------------------------------------------------------|
| A cluster qualifies as Pillar but has no logical Sub-clusters  | Keep as Pillar. Sub-clusters may be created later as keyword research expands.       |
| A Micro-cluster's keywords could belong under two Sub-clusters | Assign to the Sub-cluster with stronger semantic alignment. Cross-reference the other. |
| A Sub-cluster grows larger than its parent Pillar              | Promote the Sub-cluster to Pillar tier. Re-evaluate the former Pillar's tier.        |
| Two Pillar clusters appear to be sub-topics of each other      | The broader one retains Pillar status. The narrower one becomes a Sub-cluster.      |

**Hierarchy ID Assignment:**

Assign cluster IDs using the following convention:
- Pillar: `C-P[N]` (e.g., `C-P1`, `C-P2`)
- Sub-cluster: `C-S[N]` (e.g., `C-S1`, `C-S2`)
- Micro-cluster: `C-M[N]` (e.g., `C-M1`, `C-M2`)

---

### ▸ STEP 9 — COVERAGE CHECK

Validate that the complete keyword universe has been fully processed with no gaps, no orphans, and no structurally weak clusters.

**Coverage Check 1 — Zero Orphan Requirement**

Every keyword from the input must be assigned to exactly one cluster. Count total keywords in all clusters. This number must equal the total keywords received after normalization.

If any keyword is unassigned:
1. Identify the keyword.
2. Test it against all existing clusters using the Phase 2B decision matrix.
3. If it fits a cluster → assign it.
4. If it fits no cluster → create a new Micro-cluster for it (minimum: pair it with at least one semantically adjacent keyword from an existing cluster to avoid a single-keyword cluster).
5. Log in the Orphan Resolution Log.

**Coverage Check 2 — Minimum Cluster Size**

No cluster may contain fewer than 2 keywords. A single-keyword cluster is not a cluster — it is an orphaned keyword with a label.

If a cluster contains only 1 keyword:
- Attempt to merge it into the most semantically adjacent cluster.
- If no adjacent cluster exists → return to `keyword-discovery.skill` equivalent logic and generate companion keywords for that topic.
- If companion keywords cannot be generated → flag for user review. Do not include in final output as a standalone cluster.

**Coverage Check 3 — Structural Soundness**

| Check                                           | Pass Condition                                   | Fail Action                                     |
|-------------------------------------------------|--------------------------------------------------|-------------------------------------------------|
| Every Pillar has at least 1 Sub-cluster         | At least 1 child cluster exists                  | Create a Sub-cluster from the Pillar's BOFU or niche keywords |
| No orphaned Sub-clusters (without a Pillar)     | Every Sub-cluster has a parent Pillar            | Assign to the most relevant Pillar              |
| No orphaned Micro-clusters (without a Sub)      | Every Micro-cluster has a parent Sub-cluster     | Assign to the most relevant Sub-cluster         |
| No circular parent-child relationships          | No cluster is its own ancestor                   | Resolve by reassigning one cluster in the cycle |

**Coverage Check 4 — Deduplication Integrity**

Confirm that no keyword appears in more than one cluster in the final output. Run a cross-cluster keyword scan:

```
FOR EACH keyword in output:
  COUNT occurrences across all clusters
  IF count > 1:
    → flag as cross-cluster duplicate
    → retain in cluster with highest strategic alignment
    → remove from all other clusters
    → log
```

**Coverage Check Output:**

```
COVERAGE CHECK RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keywords In                 : [count]
Keywords Assigned           : [count]
Orphans Resolved            : [count]
Single-Keyword Clusters     : [count — must be 0]
Cross-Cluster Duplicates    : [count — must be 0]
Pillar Clusters             : [count]
Sub-Clusters                : [count]
Micro-Clusters              : [count]
Full-Funnel Clusters        : [count]
Partial-Funnel Clusters     : [count] — [names]
Coverage Status             : [COMPLETE / REQUIRES ACTION — details]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ LSI SIMULATION (LATENT SEMANTIC INDEXING)

This skill simulates LSI — a technique that models the relationships between concepts based on their co-occurrence patterns — to ensure clusters reflect how topics are semantically connected in the real world, not just how they share keywords.

**LSI Simulation Protocol:**

1. For each keyword in the universe, identify its **semantic neighbors** — concepts that frequently co-occur with it in authoritative content about the topic.
2. Use these semantic neighbors to:
   - Confirm cluster assignments (if two keywords share many semantic neighbors → strong signal for same cluster).
   - Identify missing concepts that should be present in a cluster but are not represented by any keyword.
   - Flag missing semantic neighbors as enrichment candidates (notify user or flag for keyword discovery expansion).

3. **Topic Field Construction:** For each cluster, build an implicit semantic field — the full set of concepts a comprehensive piece of content on this cluster's topic would need to address. Use this field to:
   - Validate that the cluster's keywords collectively cover the topic's semantic field.
   - Identify semantic gaps where the cluster has no keyword but where topical coverage is expected.

**LSI Simulation Output (per cluster):**

```
SEMANTIC FIELD ANALYSIS — [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Core Concept           : [value]
Semantic Neighbors     : [list of related concepts]
Semantic Field Coverage: [FULL / PARTIAL — missing: list of uncovered concepts]
Enrichment Suggestion  : [list of concepts the cluster lacks keywords for]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ TF-IDF CLUSTER WEIGHTING

Apply TF-IDF-inspired weighting to prioritize clusters based on their strategic density and uniqueness.

**Cluster Weight Scoring Formula:**

Each cluster receives a **Cluster Score** from 0–100 based on the following weighted factors:

| Factor                                | Weight | Scoring Rule                                                                       |
|---------------------------------------|--------|------------------------------------------------------------------------------------|
| **Keyword count**                     | 20%    | Normalized score: (cluster keywords / max keywords in any cluster) × 20            |
| **BOFU keyword density**              | 25%    | (BOFU keywords in cluster / total cluster keywords) × 25                           |
| **Full funnel coverage**              | 20%    | 20 if FULL, 10 if PARTIAL (1 stage missing), 0 if MINIMAL (2+ stages missing)     |
| **Niche specificity**                 | 15%    | High specificity (niche/industry/use-case focus) = 15; General topic = 7; Broad = 0 |
| **Gap potential**                     | 20%    | If cluster covers a topic with low existing competition (based on strategic_reason fields) = 20; Competitive = 10; Highly competitive = 0 |

**Score Interpretation:**

| Score Range | Priority Label  | Content Strategy Implication                                    |
|-------------|-----------------|------------------------------------------------------------------|
| 80–100      | CRITICAL        | Build content immediately. Highest ROI potential.               |
| 60–79       | HIGH            | Build in first content roadmap cycle.                           |
| 40–59       | MEDIUM          | Build in second cycle after critical and high clusters.         |
| 20–39       | LOW             | Build when higher-priority clusters are well-covered.           |
| 0–19        | DEPRIORITIZE    | Revisit after significant site authority is established.        |

---

### ▸ CLUSTER IMPORTANCE EVALUATION

Beyond the numeric score, evaluate each cluster on three qualitative dimensions:

**Dimension 1 — Keyword Density Analysis**

- Count total keywords in the cluster.
- Count unique intent types represented.
- Identify the dominant intent type.
- A cluster with high keyword count AND high intent diversity is more valuable than one with many keywords of identical intent.

**Dimension 2 — BOFU Presence Analysis**

- Count BOFU keywords in the cluster.
- Calculate BOFU ratio (BOFU keywords / total cluster keywords).
- Clusters with BOFU ratio ≥ 30% are classified as conversion-capable.
- Clusters with zero BOFU keywords are classified as traffic-only — still valuable, but flagged for enrichment.

**Dimension 3 — Gap Potential Analysis**

- Use `strategic_reason` fields of all keywords in the cluster to identify the aggregate gap value.
- If the majority of keywords in a cluster fill gaps not yet covered by the site's existing content → cluster gap potential = HIGH.
- If most keywords in a cluster duplicate topics already covered → cluster gap potential = LOW → flag for review (content updates may be more appropriate than new content creation).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Deduplication in `semantic-clustering.skill` operates at three levels, each enforcing a distinct integrity property.

### Level 1 — Keyword-Level Deduplication (Within and Across Clusters)

**Rule:** No keyword may appear in more than one cluster in the final output.

**Enforcement:**
- After Step 7 (boundary validation), run a full cross-cluster keyword scan.
- If any keyword appears in multiple clusters → retain it in the cluster with the highest strategic alignment → remove from all others → log.
- If strategic alignment is equal → retain in the cluster where it represents the BOFU intent (conversion-stage keywords are higher value in their primary cluster).

### Level 2 — Cluster-Level Deduplication (No Duplicate Clusters)

**Rule:** No two clusters may have the same strategic purpose, even if they have different names.

**Enforcement:**
- Compare cluster descriptions semantically.
- If two descriptions cover the same scope and serve the same strategic function → merge the clusters regardless of keyword overlap count.
- Names alone do not determine cluster identity. Purpose determines identity.

### Level 3 — Purpose-Level Deduplication (No Overlapping Cluster Purpose)

**Rule:** No two clusters may produce content that would compete for the same SERP positions.

**Enforcement:**
- Identify SERP competition risk by comparing the primary keywords of each cluster pair.
- If two clusters' primary keywords would directly compete → this is a purpose-level duplicate → merge.
- Cross-referencing (not merging) is only appropriate when clusters serve adjacent but genuinely distinct audiences or funnel stages.

### Cross-Session Deduplication (via memory-controller.skill)

Before finalizing any cluster, check `memory-controller.skill` for existing clusters from prior sessions:

- If an identical cluster exists in memory → do not recreate it → load it from memory → expand it with new keywords if applicable.
- If a similar cluster exists (same purpose, different name) → align naming → merge new keywords into the existing cluster.
- Never create a new cluster that duplicates the purpose of a cluster already in memory.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Clustering Behaviors

| Prohibited Behavior                                                                          | Reason                                                                      |
|----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Naming a cluster after a keyword instead of a topic theme                                    | Cluster names are architecture labels — they are not keyword targets        |
| Creating a cluster for every individual keyword (N keywords = N clusters)                    | Destroys the clustering logic entirely — grouping is the point              |
| Creating a single cluster for all keywords in the universe (1 mega-cluster)                  | Provides no architectural value — everything is in one undifferentiated pile |
| Grouping keywords by word matching instead of intent matching                                | Produces misleading architecture. This is the foundational error this skill exists to prevent. |
| Assigning a cluster a vague name like `"General CRM"` or `"Miscellaneous SEO"`             | Every cluster must have a clear, bounded purpose. Vague names signal unclear boundaries. |
| Leaving any keyword unassigned after Step 9                                                  | Zero orphans is a non-negotiable requirement                                |
| Creating single-keyword clusters                                                             | A single keyword is not a cluster — it must be paired or merged             |
| Writing a cluster description that describes what the keywords are instead of what the cluster achieves | Descriptions must articulate strategic value, not content inventory  |
| Skipping the pillar evaluation for any cluster                                               | Every cluster must be evaluated against pillar criteria                     |
| Assigning a pillar status to a cluster that does not meet all five pillar criteria           | Pillar designation is earned — not assigned for strategic convenience       |
| Merging clusters to reduce total cluster count without a semantic justification              | Merges must be driven by genuine overlap — not by a desire for fewer clusters |

### Required Clustering Behaviors

- Every cluster must have: Cluster ID, Name, Tier, Parent, Keywords (by funnel stage), Intent Mix, Pillar status, Score, and Description.
- Every keyword must appear in exactly one cluster in the final output.
- Every boundary resolution decision (merge, cross-reference, keep) must be logged.
- Every orphan keyword must be logged and resolved before output is finalized.
- Every cluster must be scored using the TF-IDF weighting formula.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ keyword-discovery.skill

| Property         | Detail                                                                                   |
|------------------|------------------------------------------------------------------------------------------|
| **Direction**    | keyword-discovery → semantic-clustering (primary input source)                           |
| **Receives**     | Complete deduplicated keyword universe with intent, funnel, category, strategic reason, and pre-cluster hints |
| **Used for**     | Semantic grouping seed data, funnel sub-clustering, pillar evaluation                   |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                   |
|------------------|------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before clustering, WRITE after output finalization                  |
| **Reads**        | Existing cluster names, existing cluster keywords, prior merges, pillar designations    |
| **Writes**       | Complete cluster architecture, hierarchy map, boundary resolution log, cluster scores   |
| **Timing**       | READ during input pre-conditions. WRITE after Step 9 passes.                            |
| **Memory layer** | keyword-memory.skill (cluster data), strategy-memory.skill (architecture data)          |

---

### ▸ query-graph.skill

| Property         | Detail                                                                                   |
|------------------|------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering → query-graph (output consumer)                                     |
| **Sends**        | Cluster hierarchy map, cross-reference relationships, cluster scores                    |
| **Used for**     | Building the topic authority graph and internal link structure recommendations          |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                   |
|------------------|------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering → gap-opportunity-engine (output consumer)                          |
| **Sends**        | Cluster architecture with gap potential scores and funnel balance status                 |
| **Used for**     | Identifying which clusters represent the highest-priority content gaps                  |

---

### ▸ authority-mapping.skill

| Property         | Detail                                                                                   |
|------------------|------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering → authority-mapping (output consumer)                               |
| **Sends**        | Pillar cluster designations, sub-cluster hierarchy, cross-reference relationships       |
| **Used for**     | Designing the authority-building content sequence and internal linking strategy         |

---

### ▸ keyword-memory.skill (via memory-controller)

| Property         | Detail                                                                                   |
|------------------|------------------------------------------------------------------------------------------|
| **Direction**    | Write (after output finalization)                                                        |
| **Writes**       | Cluster assignments for all keywords (updates lifecycle state to `CLUSTERED`)           |
| **Used for**     | Keyword lifecycle tracking, preventing re-clustering of already-clustered keywords      |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                   |
|------------------|------------------------------------------------------------------------------------------|
| **Direction**    | semantic-clustering output consumed by roadmap-engine                                   |
| **Sends**        | Cluster scores, content priority labels, pillar designations, funnel balance status     |
| **Used for**     | Determining the content publishing order based on cluster priority                      |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                 | Detection                                            | Recovery Action                                                                           |
|------------------------------------------------------------------|------------------------------------------------------|-------------------------------------------------------------------------------------------|
| Input keyword list contains fewer than 10 keywords              | Input minimum requirement check fails               | Halt. Display: "Insufficient keywords for clustering (received [N], minimum 10). Request keyword expansion from keyword-discovery.skill or provide more keywords." |
| All keywords are in the same semantic group (no differentiation) | Step 2 produces 1 cluster for all keywords          | Flag. Run breadth scoping: prompt user to narrow the primary keyword. Re-request keyword universe for a more focused input. |
| Memory controller unavailable at pre-condition check            | READ query returns error                             | Proceed without existing cluster context. Flag: "Prior cluster data unavailable — duplicate cluster risk elevated." Mark output for manual review against prior sessions. |
| Step 7 boundary validation triggers more merges than expected, resulting in fewer than 3 total clusters | Post-merge cluster count < 3 | Notify user: "Merging produced [N] clusters — this may indicate the keyword universe covers a narrower topic than expected. Add more keywords or proceed?" Wait for response. |
| A cluster fails pillar qualification but has no logical parent  | Hierarchy construction cannot place the cluster     | Assign as standalone Sub-cluster under a provisional Pillar labeled with the broader topic. Flag for user review. |
| Coverage check finds orphan keywords after all clustering passes | Step 9 Coverage Check 1 fails                       | Force-assign orphans using the relaxed decision matrix (any semantic connection qualifies). If no connection found → create a new Micro-cluster. Log all force-assignments. |
| Input keywords have no intent or funnel fields populated        | Step 1 normalization finds null fields across >50% of keywords | Halt full clustering. Run intent inference on all null-field keywords. Re-verify with user before proceeding. |
| Post-merge cluster count drops to 1                             | Extreme merge cascade                               | Halt. Report that the keyword universe has insufficient diversity for meaningful clustering. Request broader keyword input. |
| No cluster qualifies for pillar status                          | Step 6 — zero clusters meet all 5 pillar criteria   | Identify the closest-to-qualifying cluster. Flag it as a provisional pillar. Note the missing criteria. Recommend keyword expansion to meet criteria. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — SEMANTIC GROUPING DECISION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
GROUPING DECISION: SAME CLUSTER?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Ask these questions in order:

1. Do both keywords address the same underlying problem?
   YES → Strong signal for same cluster
   NO  → Likely different clusters

2. Would one piece of content serve both search intents?
   YES → Confirm same cluster
   NO  → Different clusters

3. Would targeting both in separate articles cause cannibalization?
   YES → Must be same cluster (non-negotiable)
   NO  → May be different clusters

4. Do they share at least one adjacent funnel stage?
   YES → Support for same cluster
   NO  → May be Sub-cluster within same Pillar

5. Would a user searching one likely search the other?
   YES → Confirm same cluster
   NO  → Likely different clusters
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
→ 3+ YES: Same cluster
→ 2 YES:  Likely same cluster — check cannibalization
→ 1 YES:  Likely different clusters — check pillar relationship
→ 0 YES:  Different clusters
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — CLUSTER SCORE CALCULATION WORKSHEET
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CLUSTER SCORE WORKSHEET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster Name : [Name]

Factor 1 — Keyword Count (20% weight)
  Cluster Keywords     : [N]
  Max Keywords in Any Cluster : [M]
  Score                : (N / M) × 20 = [score]

Factor 2 — BOFU Density (25% weight)
  BOFU Keywords        : [B]
  Total Keywords       : [N]
  Score                : (B / N) × 25 = [score]

Factor 3 — Full Funnel Coverage (20% weight)
  TOFU present?        : [YES / NO]
  MOFU present?        : [YES / NO]
  BOFU present?        : [YES / NO]
  Score                : FULL=20 | PARTIAL(1 missing)=10 | MINIMAL=0

Factor 4 — Niche Specificity (15% weight)
  Specificity Level    : [HIGH / GENERAL / BROAD]
  Score                : HIGH=15 | GENERAL=7 | BROAD=0

Factor 5 — Gap Potential (20% weight)
  Gap Level            : [HIGH / MEDIUM / LOW]
  Score                : HIGH=20 | MEDIUM=10 | LOW=0

TOTAL SCORE            : [sum of all factor scores] / 100
Priority Label         : [CRITICAL / HIGH / MEDIUM / LOW / DEPRIORITIZE]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: analysis/semantic-clustering.skill*
*Nexus SEO Operating System — Version 1.0.0*
