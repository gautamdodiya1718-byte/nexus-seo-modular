# SKILL: research/content-awareness.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / Content Intelligence
# EXECUTION PRIORITY: POST-INVENTORY — Runs after content-inventory.skill and before gap analysis.
# POSITION: The system's content awareness layer. The bridge between what exists and what needs to be created.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **content awareness layer** of the Nexus system.

It does not create content. It does not discover keywords. It does not analyze competitors. Its single, essential function is to build a complete, accurate, cross-session understanding of everything the system knows about a site's content — what is published, what is planned, what is in draft, what was created in prior sessions, and how all of it maps to the site's topic clusters, funnel stages, and SEO objectives.

Without this skill, the Nexus system operates blind. Gap analysis produces false gaps for topics already covered. The roadmap recommends content that already exists. Content creation tools duplicate work already done or planned. The content awareness layer prevents all of these failures by maintaining a continuously updated, semantically coherent map of the site's full content reality.

### Primary Responsibilities

| Responsibility                          | Description                                                                                                           |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Data Merge**                          | Combine published, planned, draft, and memory content into a single, deduplicated, unified content dataset            |
| **Coverage Map Construction**           | For every cluster, identify which topics are covered, which are partially covered, and which are entirely absent       |
| **Overlap Detection**                   | Detect keyword overlap, intent overlap, and semantic overlap across all content — not just exact matches              |
| **Freshness Auditing**                  | Flag content older than 12 months or demonstrably outdated relative to current SERP standards                        |
| **Funnel Coverage Assessment**          | Verify TOFU, MOFU, and BOFU presence for every cluster; flag missing funnel stages                                    |
| **Coverage Scoring**                    | Assign Complete / Partial / Incomplete / Gap status to every cluster                                                  |
| **Recommendation Layer**                | Produce specific, actionable recommendations for each cluster: what to fix, create, or merge                          |
| **Cross-Session Awareness**             | Load and integrate knowledge from prior sessions to prevent recommending previously completed or abandoned work        |
| **Hidden Duplication Detection**        | Find semantic duplicates that share no exact keywords but serve the same user intent and would compete for the same SERP position |
| **Awareness State Maintenance**         | Write the complete awareness state to `memory-controller.skill` for future session continuity                         |

### What This Skill Is NOT

- It is **not** a gap scorer. It identifies gaps — `gap-opportunity-engine.skill` scores and prioritizes them.
- It is **not** a keyword researcher. It classifies existing keyword assignments — it does not discover new ones.
- It is **not** a competitor analyzer. It is focused entirely on the user's own content landscape.
- It is **not** a one-time operation. It must be re-run whenever new content is published, a draft is completed, or significant time has passed since the last awareness update.
- It is **not** a content quality evaluator. It evaluates coverage completeness — not writing quality.

### The Foundational Principle of This Skill

> The system can only avoid duplication if it knows what already exists.
> The system can only identify real gaps if it knows what is already covered.
> The system can only build an intelligent roadmap if it can see the full content landscape.
>
> This skill creates and maintains that visibility.
>
> Every coverage assessment must reflect REALITY — not assumptions.
> A topic that is "partially covered" must be distinguished from one that is "not covered."
> A gap must be confirmed as a genuine absence — not a false negative from incomplete data.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                          | Required |
|---------------------------------------|----------------------------------------------------------------------------------------------------------|----------|
| `content-inventory.skill`             | Full inventory table (all 5 parts), cluster summary, duplicate alerts, under-optimization report         | YES      |
| `memory-controller.skill`             | Prior awareness states, prior session content records, cross-session keyword assignments                 | YES      |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                   | Used For                                                          |
|---------------------------------------|-----------------------------------------------------------------------------------|-------------------------------------------------------------------|
| `sitemap data` (raw)                  | Raw URL list with lastmod dates                                                   | Freshness audit — provides publication/update dates               |
| `user content pipeline`               | Draft titles, planned pieces, in-progress articles                                | Supplementing inventory with pipeline context                     |
| `serp-intelligence.skill` output      | Dominant intent, SERP features, content direction per keyword                    | Validating freshness flags against SERP standards                 |
| `semantic-clustering.skill` output    | Full cluster taxonomy, cluster descriptions, cross-references                     | Anchoring coverage map to the canonical cluster architecture      |
| `keyword-memory.skill` data           | All keyword assignments across all content records                               | Cross-session keyword deduplication                               |
| `entity-extraction.skill` output      | Entity map for the topic                                                          | Detecting hidden duplication through entity co-occurrence patterns|

### Input Pre-Conditions

1. **Is content-inventory.skill output available?** If not → attempt to construct a partial inventory from available data (sitemap + memory). Flag as PARTIAL AWARENESS. Continue with limited confidence.
2. **Is memory-controller.skill accessible?** If not → flag: CROSS-SESSION AWARENESS UNAVAILABLE. Proceed with current-session data only. Prior session content will not be considered.
3. **Has an awareness state been built before for this domain?** → Load prior state from memory. Identify what has changed since the last awareness run.
4. **What is the date of the most recent awareness run?** → If within 14 days → offer to use cached awareness and update only changed records. If older than 14 days → rebuild full awareness from current inventory.

### Input Confidence Framework

| Input State                                                                    | Confidence Level | Awareness Quality                                              |
|--------------------------------------------------------------------------------|------------------|----------------------------------------------------------------|
| Full inventory + memory + SERP context + clustering data                       | HIGH             | Full awareness — all 7 steps at complete depth                |
| Full inventory + memory (no SERP context)                                      | MEDIUM-HIGH      | Strong awareness — freshness audit uses age only (no SERP comparison) |
| Inventory only (no memory, no SERP context)                                    | MEDIUM           | Current-session awareness — no cross-session context          |
| Memory only (no inventory)                                                     | LOW              | Prior-session awareness only — current state unknown          |
| No inventory, no memory                                                        | MINIMAL          | Cannot build awareness — request data before proceeding       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Content Awareness Report

```
NEXUS CONTENT AWARENESS REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain                  : [domain]
Awareness Version       : [N — increments on each run]
Awareness Confidence    : [HIGH / MEDIUM-HIGH / MEDIUM / LOW / MINIMAL]
Total Clusters Analyzed : [count]
Total Content Records   : [count — published + draft + planned]
  Published             : [count]
  Draft                 : [count]
  Planned               : [count]
Cross-Session Records   : [count — from prior sessions]
Overlap Alerts          : [count]
Freshness Flags         : [count]
Funnel Gaps Detected    : [count]
Real Gaps Confirmed     : [count — verified as genuinely absent]
Generated By            : research/content-awareness.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — CLUSTER COVERAGE MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH CLUSTER]

CLUSTER: [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Coverage Status         : [COMPLETE / PARTIAL / INCOMPLETE / GAP]
Funnel Coverage         : TOFU [✓/✗] | MOFU [✓/✗] | BOFU [✓/✗]
Content Count           : Published: [N] | Draft: [N] | Planned: [N]
Pillar Page             : [URL / None / Planned]

COVERED TOPICS:
  → [Topic] — [URL/Slug] — [Status: published/draft/planned] — [Intent: TOFU/MOFU/BOFU] — [Freshness: FRESH/AGING/STALE/OUTDATED]
  → [Topic] — [URL/Slug] — [Status] — [Intent] — [Freshness]
  ...

PARTIALLY COVERED TOPICS:
  → [Topic] — [URL/Slug] — [Reason: coverage score 2, missing MOFU angle, no FAQ, etc.]
  → [Topic] — [URL/Slug] — [Reason]
  ...

MISSING TOPICS:
  → [Topic] — [Intent needed: TOFU/MOFU/BOFU] — [Gap type: CONFIRMED / INFERRED]
  → [Topic] — [Intent needed] — [Gap type]
  ...

OVERLAP ALERTS IN THIS CLUSTER:
  → [Alert type] — [URL 1] conflicts with [URL 2] — [Resolution]

FRESHNESS ALERTS IN THIS CLUSTER:
  → [URL] — [Age] — [Refresh Required: HIGH/MEDIUM/LOW priority]

RECOMMENDATION SUMMARY:
  Fix     : [specific fix with reasoning — not generic]
  Create  : [specific content piece to create — not generic]
  Merge   : [specific merge recommendation if overlap exists]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[END CLUSTER — REPEAT FOR ALL]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — COVERAGE SUMMARY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Cluster Name              | Coverage Status | TOFU | MOFU | BOFU | Overlap | Freshness Risk | Rec Priority |
|---------------------------|-----------------|------|------|------|---------|----------------|--------------|
| [Cluster 1]               | COMPLETE        | ✓    | ✓    | ✓    | NONE    | LOW            | MAINTAIN     |
| [Cluster 2]               | PARTIAL         | ✓    | ✓    | ✗    | 1 alert | MEDIUM         | CREATE BOFU  |
| [Cluster 3]               | INCOMPLETE      | ✓    | ✗    | ✗    | NONE    | HIGH           | REFRESH+CREATE|
| [Cluster 4]               | GAP             | ✗    | ✗    | ✗    | NONE    | N/A            | CREATE ALL   |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — OVERLAP DETECTION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | Type                  | Record 1              | Record 2              | Overlap Signal           | Severity | Resolution                    |
|---|-----------------------|-----------------------|-----------------------|--------------------------|----------|-------------------------------|
| 1 | EXACT KEYWORD         | /[slug-1]             | /[slug-2]             | "[keyword]"              | HIGH     | Merge into one piece          |
| 2 | SEMANTIC OVERLAP      | /[slug-3]             | /[slug-4]             | Same user intent         | MEDIUM   | Differentiate with angle      |
| 3 | INTENT CANNIBALIZATION| /[slug-5]             | /[slug-6]             | MOFU + [cluster]         | HIGH     | Designate primary; redirect   |
| 4 | HIDDEN DUPLICATE      | /[slug-7]             | /[slug-8]             | Entity co-occurrence     | MEDIUM   | Review and consolidate        |
| 5 | CROSS-CLUSTER OVERLAP | /[slug-9]             | /[slug-10]            | Crosses [Cluster A/B]    | LOW      | Reassign cluster              |
| 6 | PLANNED CONFLICT      | /[slug-11]            | [planned: title]      | Same keyword targeted    | MEDIUM   | Cancel planned / differentiate|

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — FRESHNESS AUDIT REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

FRESHNESS FLAG LEGEND:
  FRESH     → Published/updated < 6 months ago — no action needed
  AGING     → Published 6–12 months ago — monitor; minor refresh may help
  STALE     → Published 12–24 months ago — refresh recommended
  OUTDATED  → Published 24+ months ago OR SERP signals indicate content is behind — urgent refresh

| # | URL                  | Title                | Age         | Freshness   | Refresh Priority | Specific Reason                             |
|---|----------------------|----------------------|-------------|-------------|------------------|---------------------------------------------|
| 1 | /[slug]              | [Title]              | [N months]  | OUTDATED    | HIGH             | [specific outdating signal]                 |
| 2 | /[slug]              | [Title]              | [N months]  | STALE       | MEDIUM           | [specific staleness signal]                 |
| 3 | /[slug]              | [Title]              | [N months]  | AGING       | LOW              | [monitoring note]                           |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — FUNNEL COVERAGE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OVERALL FUNNEL DISTRIBUTION:
  TOFU   : [count] pages — [%] of total
  MOFU   : [count] pages — [%] of total
  BOFU   : [count] pages — [%] of total
  Funnel Balance: [observation about balance — specific to this site's distribution]

FUNNEL GAPS BY CLUSTER:

| Cluster                   | TOFU Gap | MOFU Gap | BOFU Gap | Gap Impact           |
|---------------------------|----------|----------|----------|----------------------|
| [Cluster 1]               | —        | —        | MISSING  | No conversion content|
| [Cluster 2]               | —        | MISSING  | —        | No evaluation content|
| [Cluster 3]               | MISSING  | MISSING  | —        | No awareness content |
| [Cluster 4]               | MISSING  | MISSING  | MISSING  | FULL GAP             |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 6 — AWARENESS RECOMMENDATION SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

IMMEDIATE ACTIONS (Critical gaps + High-severity overlaps):
  1. [Specific action — cluster — expected outcome]
  2. [Specific action — cluster — expected outcome]
  3. [Specific action — cluster — expected outcome]

SHORT-TERM ACTIONS (Freshness + Partial coverage):
  1. [Specific action — cluster — content piece or refresh]
  2. [Specific action — cluster — content piece or refresh]

LONG-TERM ACTIONS (Complete coverage + Authority building):
  1. [Specific action — cluster — strategic rationale]
  2. [Specific action — cluster — strategic rationale]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into              : gap-opportunity-engine.skill
                          roadmap-engine.skill
                          deduplication-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DATA MERGE

Combine all content data sources into a single, unified, deduplicated content dataset. This unified dataset is the authoritative source of truth for the awareness layer — all subsequent steps operate on it exclusively.

**Phase 1A — Source Loading**

Load each available data source:

**Source 1 — Content Inventory (from content-inventory.skill):**

Load all records from the content inventory — published, draft, and planned — with all 7 fields populated.

Fields used:
- URL / Slug (unique identifier for merge deduplication)
- Primary Keyword
- Secondary Keywords
- Intent (TOFU/MOFU/BOFU)
- Cluster
- Status (published/draft/planned/archived)
- Coverage Score

Flag each record: `SOURCE: INVENTORY`.

**Source 2 — Memory Content (from memory-controller.skill):**

Query `memory-controller.skill` for:
- All content records from prior sessions for this domain.
- Any content records written directly to keyword-memory or content-memory in prior sessions.
- Any awareness state snapshots from prior awareness runs.

Fields extracted from memory:
- URL / Slug
- Primary Keyword
- Status at time of last session
- Cluster assignment at time of last session
- Any freshness flags from prior awareness runs
- Any overlap alerts from prior awareness runs

Flag each record: `SOURCE: MEMORY`.

**Source 3 — User-Provided Pipeline (if available):**

Load any draft or planned content provided directly by the user in the current session (not already in the inventory).

Fields extracted:
- Working title (used as Slug proxy)
- Target keyword (if provided)
- Status (draft/planned)

Flag each record: `SOURCE: PIPELINE`.

**Phase 1B — Cross-Source Record Matching**

Before merging, match records across sources to identify which records exist in multiple sources:

**Matching Logic:**

| Match Type                                    | Match Signal                                         | Action                                             |
|-----------------------------------------------|------------------------------------------------------|----------------------------------------------------|
| Exact URL match                               | Same normalized URL in inventory AND memory          | Keep INVENTORY version; absorb MEMORY metadata     |
| Same primary keyword, different URL           | Inventory keyword = memory keyword, URLs differ      | Flag as POTENTIAL DUPLICATE — both records kept pending review |
| Memory record with no inventory counterpart   | URL in memory but not in current inventory           | Add as MEMORY-ONLY record; flag status as UNKNOWN  |
| Pipeline record already in inventory          | Working title matches inventory title closely        | Merge; use inventory URL; mark as CONFIRMED PLANNED|

**Phase 1C — Unified Dataset Construction**

After matching, construct the unified dataset:

1. Start with the full content inventory as the base.
2. Add MEMORY-ONLY records (present in memory but not in current inventory) — flag each as `MEMORY-ONLY`.
3. Add PIPELINE records that have no inventory counterpart — flag each as `PIPELINE-ONLY`.
4. For matched records — retain the most complete version; absorb unique metadata from the secondary source.
5. Assign a unified record ID to every entry.
6. Record the final count: Published, Draft, Planned, Memory-Only, Archived.

**Phase 1D — Unified Dataset Deduplication**

After construction, run a final deduplication pass on the unified dataset:

1. **URL deduplication:** Same normalized URL appearing twice → keep one; log the removed duplicate.
2. **Keyword deduplication (within the merge):** Same primary keyword in two records from different sources → merge records if the URL is the same OR flag as POTENTIAL DUPLICATE if the URLs differ.
3. **Status reconciliation:** If the same URL appears as `published` in the inventory but `planned` in memory → use `published` (inventory is more current). Log the reconciliation.

**Phase 1E — Memory-Only Record Status Assessment**

For every MEMORY-ONLY record (in memory but not in current inventory):

| Reason It May Be Memory-Only               | Assessment                                                          |
|--------------------------------------------|---------------------------------------------------------------------|
| Content was deleted or unpublished          | Mark status: POTENTIALLY ARCHIVED — verify with user               |
| Content is on a subdomain not in inventory scope | Mark status: OUT OF SCOPE — keep in awareness for dedup purposes |
| Memory was populated from a previous, partial analysis | Mark status: UNVERIFIED — do not use as a confirmed gap signal |
| Content was planned but never created      | Mark status: ABANDONED PLAN — free keyword for reassignment         |

Display a summary of MEMORY-ONLY records for user review before proceeding:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Memory Records Not Found in Current Inventory
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[N] records from prior sessions were not found in the current content inventory:

  [URL / Title] — Last seen: [date] — Status: [assessed status]
  ...

These may be deleted pages, out-of-scope content, or abandoned plans.
Proceed with current inventory? [YES / Review list first]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

WAIT for user response before proceeding to Step 2.

---

### ▸ STEP 2 — BUILD COVERAGE MAP

Using the unified dataset from Step 1, construct a comprehensive coverage map for every topic cluster. The coverage map defines what the site covers, how completely, and what is absent.

**Phase 2A — Cluster Enumeration**

Identify all clusters that should be represented in the coverage map:

1. **From inventory:** All cluster names assigned in the content inventory.
2. **From clustering output:** All cluster names from `semantic-clustering.skill` output (if available).
3. **From memory:** Any cluster names from prior awareness states.
4. **Union:** The complete cluster list is the union of all three sources — no cluster is omitted.

For clusters that appear in the semantic clustering output but have NO content in the inventory → record them as GAP clusters with zero content. These are expected gaps — the site knows about this topic territory but has not created content for it.

**Phase 2B — Per-Cluster Topic Mapping**

For each cluster, map all records in the unified dataset to one of three coverage states:

**Coverage State 1 — COVERED TOPICS:**

A topic is considered COVERED if:
- At least one record in the cluster targets this topic as its primary keyword.
- The record has a coverage score of 3 or above (Solid, Comprehensive, or Authority).
- The record status is `published` (draft and planned do NOT qualify as covered — they are in-progress).

For each covered topic, record:
- Topic description (from primary keyword or content title).
- URL.
- Status (published).
- Intent (TOFU/MOFU/BOFU).
- Freshness indicator (from Phase 4A calculation or last-known date).

**Coverage State 2 — PARTIALLY COVERED TOPICS:**

A topic is partially covered if:
- A record exists targeting this topic BUT has a coverage score of 1 or 2.
- A record exists targeting this topic BUT it is only in `draft` status (not yet live).
- A record exists targeting this topic BUT it covers only one of two or more expected sub-topics.
- A record covers the topic at the wrong funnel stage (e.g., a TOFU article exists but MOFU is needed).

For each partially covered topic, record:
- Topic description.
- URL.
- Status.
- Reason for partial classification.
- What would make it fully covered.

**Coverage State 3 — MISSING TOPICS:**

A topic is MISSING if:
- No record in any status (published, draft, planned) targets this topic.
- The topic is expected based on the cluster's semantic scope.
- No memory record from prior sessions covers this topic.

**Missing Topic Identification Method:**

Missing topics are identified from two sources:

1. **Cluster scope inference:** Based on the cluster name and its known semantic field, what topics would a comprehensive coverage of this cluster include? Topics that should be present but have no corresponding record are MISSING.

2. **SERP gap signals (if SERP data is available):** Topics that appear in SERP PAA questions or competing content for this cluster's keywords but have no corresponding record are MISSING.

**Gap Confirmation Protocol:**

Before marking any topic as MISSING, run a confirmation check:

| Check                                                                   | Action if True                                      |
|-------------------------------------------------------------------------|-----------------------------------------------------|
| Does any record with a similar keyword cover this topic?               | NOT MISSING — covered under a different keyword     |
| Is this topic covered as a sub-section of an existing article?          | NOT MISSING — covered within another piece          |
| Was this topic planned in a prior session but never executed?           | MISSING + flag: ABANDONED PLAN                      |
| Is this topic outside the site's intended scope?                        | NOT MISSING — out of scope; note and exclude        |
| Is this topic genuinely absent from all records and memory?             | CONFIRMED MISSING GAP                               |

Flag each missing topic as CONFIRMED or INFERRED:
- CONFIRMED: The topic's absence is verified across all records and memory.
- INFERRED: The topic is expected based on cluster scope but cannot be confirmed without fuller data.

**Phase 2C — Coverage Map Assembly**

After classifying all topics per cluster, assemble the coverage map:

For each cluster:
1. List all COVERED topics in order of coverage score (highest first).
2. List all PARTIALLY COVERED topics with their partial reason.
3. List all MISSING topics with their confirmation status.
4. Record content counts: Published, Draft, Planned.
5. Note pillar page presence or absence.

---

### ▸ STEP 3 — OVERLAP DETECTION

Detect all forms of content overlap within the unified dataset. Overlap detection in this skill goes beyond the keyword-level detection done in `content-inventory.skill` — it surfaces hidden semantic and intent-level overlaps that exact-match keyword comparison cannot find.

**Phase 3A — Exact Keyword Overlap Detection**

Identical to the deduplication step in `content-inventory.skill`, but now applied across the full unified dataset including memory records and pipeline content:

1. Compare every record's primary keyword against every other record's primary keyword.
2. Exact match (case-insensitive, after normalization) → EXACT KEYWORD overlap.
3. One keyword is a substring of another → PARTIAL KEYWORD overlap (lower severity).
4. Flag both records; record the alert in Part 3 of the output.

This step catches any overlaps not yet flagged in the inventory — particularly cases where planned or memory content duplicates existing published content.

**Phase 3B — Semantic Overlap Detection**

Semantic overlap exists when two records cover the same topic with different keyword phrasings. Exact keyword matching cannot catch these — this requires semantic analysis.

**Semantic Overlap Test Protocol:**

For each pair of records in the same cluster, ask three questions:

| Question                                                                               | Signal                         |
|----------------------------------------------------------------------------------------|--------------------------------|
| Would both records satisfy the same searcher if they were searching for either keyword? | YES → Strong semantic overlap  |
| Do both records share ≥3 of the same secondary keywords?                              | YES → High semantic similarity |
| Would both records compete for the same SERP ranking position?                        | YES → Semantic overlap confirmed |

**Semantic Overlap Classification:**

| Overlap Strength | Condition                                              | Action                                          |
|------------------|--------------------------------------------------------|-------------------------------------------------|
| STRONG           | ≥2 of 3 test questions answered YES                   | Flag as SEMANTIC OVERLAP — HIGH severity        |
| MODERATE         | 1 of 3 test questions answered YES                    | Flag as SEMANTIC OVERLAP — MEDIUM severity      |
| WEAK             | Topically adjacent but test questions answered NO     | No flag — healthy cluster coverage              |

**Phase 3C — Intent Cannibalization Detection**

Intent cannibalization occurs when two records in the same cluster target the same funnel stage and serve the same user need — even if their keywords are not semantically equivalent.

**Cannibalization Detection Logic:**

Two records are cannibalized when:
- Same cluster assignment.
- Same intent (TOFU/MOFU/BOFU).
- Both records address the same primary user question or decision point.

**Cannibalization Confirmation Test:**

"If a searcher at [funnel stage] for [cluster topic] visited either page, would they be satisfied?"
- YES to both → Cannibalization confirmed.
- YES to one only → Not cannibalized — they serve different sub-intents despite same funnel stage.

**Phase 3D — Hidden Duplication Detection**

Hidden duplication is the most subtle overlap form: two records that share no common keywords and may be in different clusters, but are semantically so close that they effectively serve the same reader and compete for the same SERP positions.

**Hidden Duplication Detection Method:**

1. Build a **topic fingerprint** for every record:
   - Primary keyword (core concept extracted).
   - Secondary keywords (semantic neighbors).
   - Intent.
   - Cluster.

2. Compare topic fingerprints across all records:
   - If two records' semantic neighbor sets overlap by ≥50% AND they share the same intent → HIDDEN DUPLICATE.
   - If two records from different clusters would produce almost identical content when executed → HIDDEN DUPLICATE.

3. Cross-cluster hidden duplicates are flagged with type: CROSS-CLUSTER OVERLAP.

**Entity-Based Hidden Duplication (if entity map available):**

Using the entity map from `entity-extraction.skill`:
- If two records share ≥70% of the same Critical and Important entities AND share the same intent → HIDDEN DUPLICATE (entity-based).
- Entity co-occurrence pattern analysis reveals thematic equivalence that keyword analysis misses.

**Phase 3E — Planned Content Conflict Detection**

Planned and draft content can conflict with published content — creating future duplication problems before any new content is created.

For every PLANNED or DRAFT record:
1. Compare its target keyword against all PUBLISHED records.
2. If exact or semantic overlap is detected → flag as PLANNED CONFLICT with the existing published piece.
3. Present to user:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Planned Content Conflict Detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Planned piece   : "[working title]" — keyword: "[keyword]"
Conflicts with  : "[existing title]" — [URL] — published [date]

This planned piece would duplicate existing content.

Options:
  1  Cancel planned piece — existing content is sufficient
  2  Differentiate planned piece — change angle or audience
  3  Update existing piece instead of creating new content
  4  Proceed anyway — planned piece serves a distinct sub-intent

Enter choice:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

WAIT for user response. Apply choice before finalizing overlap report.

**Phase 3F — Overlap Resolution Recording**

For every detected overlap (regardless of type), record:
1. The two conflicting record IDs.
2. The overlap type.
3. The overlap signal (what created the overlap).
4. The severity.
5. The recommended resolution.
6. Whether the user has already acted on this overlap (from memory).

Do NOT automatically resolve any overlap. All resolutions require user confirmation.

---

### ▸ STEP 4 — FRESHNESS AUDIT

Assess the age and currency of every published content record and flag those requiring refresh.

**Phase 4A — Age Calculation**

For each published record:
1. Determine the publication or last-update date.

**Date Source Priority:**

| Priority | Date Source                                                    | Reliability |
|----------|----------------------------------------------------------------|-------------|
| 1        | Sitemap `<lastmod>` date                                       | HIGH        |
| 2        | Meta date (extracted from content-inventory.skill if available)| HIGH        |
| 3        | URL date signal (e.g., `/blog/2022/01/article`)               | MEDIUM      |
| 4        | Memory record date (from prior session analysis)               | MEDIUM      |
| 5        | No date available                                             | LOW — flag as UNKNOWN AGE |

2. Calculate the record's age in months from the current date.
3. Assign a freshness label:

| Age Range         | Freshness Label | Indicator |
|-------------------|-----------------|-----------|
| < 6 months        | FRESH           | No action needed |
| 6–12 months       | AGING           | Monitor; minor refresh may help |
| 12–24 months      | STALE           | "Refresh Required" — MEDIUM priority |
| 24–36 months      | OUTDATED        | "Refresh Required" — HIGH priority |
| 36+ months        | CRITICALLY OUTDATED | "Refresh Required" — CRITICAL priority |

**Phase 4B — Freshness Override Signals**

Beyond age, apply freshness override signals that can upgrade a FRESH or AGING record to STALE or OUTDATED regardless of publication date:

| Override Signal                                                              | Freshness Override Action                          |
|------------------------------------------------------------------------------|----------------------------------------------------|
| Record's primary keyword is a time-sensitive topic (annual data, tools, rates) | If AGING → upgrade to STALE                      |
| SERP shows top results all have recent dates (< 6 months)                    | If STALE → upgrade to OUTDATED                    |
| Record references a specific year in its title that is now past              | Automatic STALE regardless of actual publication date |
| SERP context shows the topic has changed significantly since publication      | Automatic OUTDATED regardless of age              |
| Coverage score is 1 or 2 AND age > 12 months                                | STALE — both thin AND aging                       |

**Phase 4C — Refresh Priority Assignment**

Combine age and override signals to produce a final refresh priority:

| Freshness Label | Coverage Score | Topic Sensitivity | Refresh Priority |
|-----------------|----------------|-------------------|------------------|
| OUTDATED        | Any            | Any               | CRITICAL         |
| STALE           | 1–2            | Any               | HIGH             |
| STALE           | 3–5            | Time-sensitive    | HIGH             |
| STALE           | 3–5            | Evergreen         | MEDIUM           |
| AGING           | 1–2            | Any               | MEDIUM           |
| AGING           | 3–5            | Time-sensitive    | LOW              |
| AGING           | 3–5            | Evergreen         | MONITOR          |
| FRESH           | Any            | Any               | NONE             |

**Phase 4D — Freshness Audit Aggregation**

After individual records are assessed:
1. Count records by freshness label.
2. Identify clusters where ≥50% of records are STALE or OUTDATED → flag as CLUSTER FRESHNESS RISK.
3. Produce the Freshness Audit Report (Part 4 of output).

**Freshness Flag Propagation:**

When a cluster has CLUSTER FRESHNESS RISK:
- Its coverage status is degraded by one level (COMPLETE → PARTIAL; PARTIAL → INCOMPLETE).
- Even "covered" topics in a stale cluster are treated as PARTIALLY COVERED in the coverage map.
- This prevents the gap engine from treating stale-covered topics as genuinely complete.

---

### ▸ STEP 5 — FUNNEL COVERAGE ASSESSMENT

For every cluster, verify that all three funnel stages are represented. Missing funnel stages are flagged as structural content gaps — distinct from topic gaps.

**Phase 5A — Per-Cluster Funnel Inventory**

For each cluster, count published (not draft/planned) records at each funnel stage:

```
Cluster: [Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOFU (published)  : [count] — [list of URLs targeting TOFU]
MOFU (published)  : [count] — [list of URLs targeting MOFU]
BOFU (published)  : [count] — [list of URLs targeting BOFU]

TOFU (in pipeline): [count] — [draft/planned pieces]
MOFU (in pipeline): [count]
BOFU (in pipeline): [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 5B — Funnel Gap Classification**

For each cluster, classify funnel coverage:

| TOFU Published | MOFU Published | BOFU Published | Funnel Status                                  |
|----------------|----------------|----------------|------------------------------------------------|
| ≥1             | ≥1             | ≥1             | FULL FUNNEL — all stages present               |
| ≥1             | ≥1             | 0              | BOFU MISSING — no conversion content          |
| ≥1             | 0              | ≥1             | MOFU MISSING — no evaluation bridge            |
| 0              | ≥1             | ≥1             | TOFU MISSING — no awareness entry point        |
| ≥1             | 0              | 0              | TOFU ONLY — risk: drives traffic, cannot convert |
| 0              | 0              | ≥1             | BOFU ONLY — risk: no audience to convert       |
| 0              | ≥1             | 0              | MOFU ONLY — isolated evaluation content        |
| 0              | 0              | 0              | NO CONTENT — full cluster gap                  |

**Phase 5C — Pipeline Consideration**

Draft and planned content is considered for funnel coverage ONLY in the context of identifying whether the gap is being actively addressed — not as a substitute for published coverage:

```
TOFU Published : 0 → TOFU MISSING
TOFU In Pipeline: 1 → TOFU MISSING — but in progress (pipeline piece: [title])
```

The gap is still a gap. The pipeline indicator prevents the gap from being recommended again (it is already being addressed). This distinction is critical for the recommendation layer.

**Phase 5D — Funnel Imbalance Detection**

Beyond missing stages, detect funnel imbalance — where one stage is over-represented relative to others:

| Imbalance Pattern                                      | Flag                                                      |
|--------------------------------------------------------|-----------------------------------------------------------|
| TOFU > 70% of all cluster content                     | TOFU HEAVY — site is building awareness but not converting |
| BOFU > 60% of all cluster content                     | BOFU HEAVY — site is targeting buyers but not educating  |
| MOFU = 0 AND both TOFU and BOFU present               | MOFU GAP — no evaluation bridge between awareness and decision |

Funnel imbalance is recorded in the cluster coverage map with a strategic observation about its business impact.

**Phase 5E — Site-Wide Funnel Distribution**

After per-cluster assessment, aggregate funnel distribution across the entire site:

1. Calculate total TOFU, MOFU, BOFU published page counts.
2. Calculate percentages.
3. Produce the overall funnel distribution observation.
4. Flag site-wide imbalances that affect the recommendation layer:
   - If BOFU % < 10% of total content → flag: "Site is significantly underweighted in conversion content."
   - If TOFU % > 70% of total content → flag: "Site is heavily TOFU-weighted — conversion pipeline may be weak."

---

### ▸ STEP 6 — COVERAGE SCORING

Assign a single Coverage Status to every cluster in the awareness map.

**Phase 6A — Coverage Status Definitions**

| Status       | Definition                                                                                                   |
|--------------|--------------------------------------------------------------------------------------------------------------|
| **COMPLETE** | The cluster has strong published content across all three funnel stages; no significant gaps; freshness is acceptable; no unresolved overlap. |
| **PARTIAL**  | The cluster has content in some funnel stages but is missing at least one; OR has content with coverage score 1–2 across most of the cluster; OR has freshness risk on key pieces. |
| **INCOMPLETE** | The cluster has minimal content — only 1–2 pages with thin coverage; at least 2 funnel stages missing; may have overlap issues not yet resolved. |
| **GAP**      | The cluster has no published content at all — or so little coverage (1 thin page, single funnel stage, stale) that it effectively functions as a gap. |

**Phase 6B — Coverage Status Assignment Logic**

Apply this decision tree for each cluster:

```
COVERAGE STATUS DECISION TREE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Q1: Does the cluster have ZERO published content?
  YES → GAP. Stop.
  NO → Continue.

Q2: Does the cluster have published content in ALL THREE funnel stages?
  NO → At minimum PARTIAL. Continue to Q3.
  YES → Continue to Q3.

Q3: Are the majority of published pieces in this cluster at coverage score 3+?
  NO (most are score 1–2) → INCOMPLETE or PARTIAL (downgrade by missing stages).
  YES → Continue to Q4.

Q4: Does the cluster have a CLUSTER FRESHNESS RISK (≥50% STALE/OUTDATED)?
  YES → Downgrade by one level (COMPLETE → PARTIAL; PARTIAL → INCOMPLETE).
  NO → Continue to Q5.

Q5: Does the cluster have 2+ unresolved HIGH-severity overlap alerts?
  YES → Downgrade by one level.
  NO → Continue to Q6.

Q6: All stages present + good scores + acceptable freshness + minimal overlap:
  → COMPLETE.

If downgraded more than once from COMPLETE → INCOMPLETE.
If GAP criteria apply regardless of other factors → GAP.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 6C — Coverage Status Prioritization**

After scoring all clusters, rank them by action priority:

| Priority Level | Condition                                                                    |
|----------------|------------------------------------------------------------------------------|
| CRITICAL       | GAP cluster — no content; OR INCOMPLETE cluster with HIGH-priority funnel    |
| HIGH           | PARTIAL cluster with BOFU missing — no conversion capability in this cluster |
| MEDIUM         | PARTIAL cluster with MOFU or TOFU missing                                    |
| LOW            | INCOMPLETE cluster with only freshness issues                               |
| MAINTAIN       | COMPLETE cluster — no new content needed; monitor freshness                  |

---

### ▸ STEP 7 — RECOMMENDATION LAYER

For each cluster, produce specific, evidence-backed, non-generic recommendations. Every recommendation must address a real gap, a real overlap, or a real freshness issue identified in the preceding steps.

**Phase 7A — Recommendation Types**

| Rec Type   | When Applied                                                                        | What It Specifies                                                         |
|------------|-------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| **FIX**    | Partial coverage, thin content, under-optimized pages, freshness issues            | Which specific page to update and what specifically to add or refresh     |
| **CREATE** | Missing topics, missing funnel stages, confirmed gaps                              | What type of content to create, for which funnel stage, targeting which keyword |
| **MERGE**  | Exact keyword overlap, semantic overlap, intent cannibalization                     | Which pages to consolidate, which URL to keep, what to do with the other  |
| **REDIRECT** | Duplicate content with one strong and one thin page                              | Which page to redirect to which; redirect type (301)                     |
| **CANCEL** | Planned content that duplicates published content                                   | Which planned piece to cancel; alternative direction if needed           |
| **MONITOR** | Aging content not yet requiring action; clusters at risk but not yet critical     | What to watch for; when to reassess                                      |

**Phase 7B — Recommendation Construction Rules**

Every recommendation must satisfy all of the following:

1. **Specific, not generic:** The recommendation must name a specific cluster, specific URL, specific keyword, or specific content type — not a general principle.
2. **Evidence-backed:** The recommendation must cite which gap, overlap, or freshness signal triggered it.
3. **Actionable:** The recommendation must be completable without additional analysis.
4. **Non-duplicating:** The same recommendation must not appear twice (for two overlapping alerts on the same content).
5. **Pipeline-aware:** Do not recommend creating content that is already in draft or planned status — reference the in-progress piece instead.

**Prohibited Recommendation Forms:**

| Prohibited                                    | Required Alternative                                                           |
|-----------------------------------------------|--------------------------------------------------------------------------------|
| "Create more TOFU content."                   | "Create a [how-to guide / what-is article] targeting [keyword] at TOFU for [Cluster Name]." |
| "Improve content quality."                    | "Update [URL] by adding sourced statistics and a FAQ section — coverage score is currently 2." |
| "Fix duplicate content issues."               | "Merge /[slug-1] and /[slug-2] — both target '[keyword]' at MOFU in [Cluster Name]. Keep /[slug-1]; redirect /[slug-2]." |
| "Refresh old content."                        | "Refresh /[slug] — published [N months] ago; all SERP top results are from [recent period]; content references [outdated element]." |
| "Consider adding a pillar page."              | "Create a pillar page for [Cluster Name] — cluster has [N] supporting pages but no central hub. Suggested URL: /[cluster-topic]/." |

**Phase 7C — Recommendation Priority Ordering**

Within each cluster, order recommendations by priority:
1. HIGH-severity overlaps that create cannibalization (MERGE or REDIRECT first).
2. CRITICAL freshness issues (REFRESH critical pieces before creating new).
3. Missing funnel stages (CREATE — critical funnel gaps addressed before optional coverage).
4. Thin coverage updates (FIX — improving existing before expanding).
5. Optional coverage additions (CREATE — filling in secondary topics).
6. Monitoring items (MONITOR — lowest priority).

**Phase 7D — Global Recommendation Deduplication**

Before finalizing the recommendation layer:
1. Scan all recommendations across all clusters.
2. If the same recommendation appears for two different clusters (unlikely but possible for cross-cluster overlaps) → consolidate into a single entry that references both clusters.
3. If a recommendation in Cluster A would be superseded by a prior recommendation in Cluster B → mark it as a dependency: "Complete [Cluster B recommendation] first."

**Phase 7E — Awareness Recommendation Summary Construction**

After all cluster recommendations are finalized, produce the Awareness Recommendation Summary (Part 6 of output):

**Immediate Actions** (must be addressed before new content creation):
- All HIGH-severity overlap resolutions.
- All CRITICAL freshness refreshes.
- All GAP cluster actions for Critical-priority clusters.

**Short-Term Actions** (complete within next content cycle):
- Missing funnel stage creations for PARTIAL clusters.
- Medium-priority freshness refreshes.
- INCOMPLETE cluster improvement plans.

**Long-Term Actions** (build after immediate and short-term are addressed):
- Optional coverage additions for COMPLETE clusters.
- Pillar page creation for clusters with many supporting pages but no hub.
- Authority-level content creation for clusters with good coverage but no score-5 piece.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CROSS-SESSION AWARENESS

The most critical advanced capability of this skill is its ability to maintain content awareness across multiple sessions — preventing the system from losing knowledge of what was created, planned, or resolved in prior work.

**Cross-Session Protocol:**

1. **Load prior awareness state:** At session start, query `memory-controller.skill` for the most recent awareness state snapshot for this domain. This snapshot includes:
   - All content records as of the last session.
   - All overlap alerts and their resolution statuses.
   - All gap recommendations and whether they were acted upon.
   - All freshness flags and whether refreshes were completed.

2. **Delta detection:** Compare the current inventory against the prior awareness state to identify what has changed:
   - New published content (in current inventory, not in prior state) → add to unified dataset as new records.
   - Removed content (in prior state, not in current inventory) → flag as POTENTIALLY ARCHIVED.
   - Changed status (planned → published, draft → published) → update lifecycle state.
   - Resolved overlaps (overlap was flagged in prior state; duplicate no longer exists) → mark as RESOLVED.

3. **Gap progression tracking:** If a gap was identified in a prior session and recommended content was created → verify the recommendation was fulfilled:
   - If the new published page fills the gap → mark the gap as CLOSED.
   - If the new page was published but doesn't fully address the gap → mark as PARTIALLY ADDRESSED.
   - If no new page was created → gap remains OPEN; do not re-recommend unless context has changed.

4. **Awareness state write-back:** After this session's awareness run is complete, write the full awareness state to `memory-controller.skill` for the next session.

---

### ▸ SEMANTIC OVERLAP DETECTION — ADVANCED IMPLEMENTATION

Beyond the basic semantic overlap tests in Step 3, this advanced implementation applies entity-based and cluster-boundary overlap detection.

**Cluster Boundary Overlap:**

When content in Cluster A semantically overlaps with content in Cluster B — suggesting the cluster boundaries themselves may be wrong:

1. Identify records that have a high semantic fingerprint overlap with records in a DIFFERENT cluster.
2. If ≥3 records cross cluster boundaries in the same direction → flag: CLUSTER BOUNDARY ISSUE — clusters [A] and [B] may need to be merged or reorganized.
3. Surface this to the user as a structural recommendation, not a content recommendation.

**Secondary Keyword Cross-Pollution:**

When the secondary keywords of one record directly overlap with the primary keyword of another record in a different cluster:

1. Record A: Primary keyword `"CRM software for startups"` — Secondary: `"startup growth tools"`.
2. Record B: Primary keyword `"startup growth tools"` — in a different cluster.
3. This is secondary → primary cross-pollution. Flag Record A's secondary keyword as conflicted.
4. Recommendation: Remove the conflicted secondary keyword from Record A; it would create cross-cluster competition.

---

### ▸ COVERAGE INFERENCE (FOR PARTIAL DATA)

When the inventory is incomplete (confidence = MEDIUM or LOW), the skill applies structured inference to estimate coverage status rather than defaulting to "unknown" for all uncategorized topics.

**Coverage Inference Rules:**

| Available Signal                                                              | Inferred Coverage                                        |
|-------------------------------------------------------------------------------|----------------------------------------------------------|
| Cluster has 3+ pages but no intent data                                       | Infer PARTIAL — likely has some funnel coverage          |
| URL slug strongly signals TOFU but no intent data                             | Infer TOFU coverage — mark as INFERRED                  |
| Memory shows a completed recommendation for this cluster from prior session    | Infer topic is COVERED — mark as INFERRED from memory   |
| Keyword is in keyword-memory with PUBLISHED lifecycle state                   | Infer the page exists — coverage PARTIAL until confirmed |
| No data at all for this cluster                                               | Mark as GAP — cannot infer coverage without signals      |

Every inferred coverage classification is flagged as `INFERRED` and not used as a confirmed signal for gap analysis. Gaps confirmed through inference are flagged as INFERRED GAPS — and the recommendation produced is: "Verify coverage before creating new content for this topic."

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — Do Not Mark Covered Topics as Missing

Before any topic is classified as MISSING in the coverage map, it must pass the Gap Confirmation Protocol (Phase 2B). Any topic that is genuinely covered — even at partial depth or in draft — must NOT be listed as a missing gap. False gaps waste roadmap capacity and create unnecessary content.

### Rule 2 — Do Not Duplicate Gap Suggestions Across Steps

The same gap must not appear as a recommendation in both the Coverage Map (Part 1) and the Funnel Coverage Report (Part 5) as if they were two separate action items. Each gap appears once — in the most relevant section — and is cross-referenced in other sections, not duplicated.

### Rule 3 — Do Not Repeat Overlap Alerts Already Resolved

If `memory-controller.skill` shows that an overlap alert was flagged in a prior session AND the user took a resolution action → do not re-surface that alert in the current session. It is marked RESOLVED in the overlap report. If the resolution was not fully completed → mark PARTIALLY RESOLVED and explain what remains.

### Rule 4 — Do Not Recommend Content Already in the Pipeline

Before recommending the creation of any content piece, verify that the same or equivalent piece is not already in draft or planned status. If a pipeline piece addresses the same gap → reference the pipeline piece instead of recommending new creation: "Gap being addressed by planned piece: [title]."

### Rule 5 — Do Not Recommend Merging Content That Was Already Merged

If two records were merged in a prior session and the resolution is reflected in the current inventory → do not re-flag them as an overlap. Prior resolutions are preserved in memory and consulted before any overlap alert is raised.

### Rule 6 — Recommendations Are Not Repeated Within a Session

If the same recommendation would logically apply to two different clusters (e.g., "add a pillar page"), it is produced once as a global recommendation — not once per cluster. Cluster-specific recommendations remain per-cluster. Cross-cluster pattern recommendations are aggregated.

### Rule 7 — Memory Deduplication of Awareness State

When writing the awareness state to memory at session end, do not create a new snapshot if nothing has changed since the last snapshot. Only write a new state if at minimum one of the following changed: new content published, overlap resolved, gap closed, freshness status changed, or cluster structure modified.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Awareness Behaviors

| Prohibited Behavior                                                                           | Reason                                                                             |
|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| Marking a topic as MISSING without running the Gap Confirmation Protocol                     | Missing status must be confirmed — not assumed from absence of keyword match       |
| Producing a recommendation to "improve content quality" without specifying what to improve   | Every fix recommendation must name a specific element to add or update             |
| Flagging every record older than 12 months as requiring urgent refresh                      | Freshness priority is nuanced — evergreen content at score 4+ does not urgently need refreshing |
| Reporting that a cluster has "good coverage" without specifying which topics are covered     | Coverage assessments must list the specific topics covered, not describe them vaguely |
| Producing identical recommendations for multiple clusters                                   | Cluster recommendations must be specific to that cluster's content situation       |
| Using the word "consider" in any recommendation                                              | Recommendations are specific and actionable — not tentative suggestions            |
| Marking pipeline content as fulfilling a coverage gap                                       | Planned/draft content does not constitute coverage — it signals in-progress work   |
| Surfacing an overlap that was already resolved in a prior session                            | Memory must be checked before raising any alert from prior sessions                |
| Recommending creation of content that already exists under a different keyword variant       | The Gap Confirmation Protocol must be run before any CREATE recommendation is made |
| Assigning COMPLETE status to a cluster with a CRITICAL freshness flag                       | COMPLETE requires acceptable freshness — stale clusters cannot be complete         |

### Required Awareness Behaviors

- Every cluster's coverage map must list specific topics (by keyword or title) — not generic descriptions.
- Every freshness flag must cite the specific age signal or override signal that triggered it.
- Every overlap alert must name both conflicting records by URL or slug.
- Every recommendation must be specific enough to execute without additional analysis.
- The awareness state must be written to memory at session close.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary input)                                                        |
| **Receives**     | Full 5-part inventory dataset: all content records, cluster summary, duplicates, under-optimization report |
| **Critical fields** | Primary keyword, Cluster, Intent, Status, Coverage Score — all consumed in every step   |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ at session start, WRITE at session close                               |
| **Reads**        | Prior awareness state, prior content records, prior overlap resolutions, prior gap statuses |
| **Writes**       | Complete current awareness state (all 6 parts), updated overlap statuses, updated gap statuses |
| **Memory layers**| content-memory.skill (content records), strategy-memory.skill (awareness state, cluster coverage) |
| **Timing**       | READ at Step 1. WRITE after Step 7 completes.                                               |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-awareness → gap-opportunity-engine (primary input)                                 |
| **Sends**        | Confirmed gap list (by cluster and funnel stage), coverage status per cluster, funnel gaps |
| **Used for**     | Scoring and prioritizing confirmed gaps for the content roadmap                            |
| **Critical fields** | Confirmed gaps (not inferred) — only CONFIRMED gaps are scored by the gap engine       |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-awareness → roadmap-engine (strategic context)                                     |
| **Sends**        | Awareness Recommendation Summary (Part 6), coverage status per cluster, pipeline status    |
| **Used for**     | Building a content roadmap sequenced by actual awareness state — not theoretical gaps       |
| **Critical fields** | Immediate Actions list — directly informs the first phase of the roadmap                |

---

### ▸ deduplication-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-awareness → deduplication-engine (overlap data)                                    |
| **Sends**        | Complete Overlap Detection Report (Part 3), hidden duplication alerts                      |
| **Used for**     | Deep deduplication analysis and producing concrete merge/redirect execution plans          |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source (optional)                                                          |
| **Receives**     | SERP freshness signals, dominant intent, content direction recommendations                  |
| **Used for**     | Freshness override signals in Phase 4B — validating whether content is outdated vs. SERP  |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source (optional but strongly recommended)                                 |
| **Receives**     | Canonical cluster taxonomy, cluster descriptions, cross-cluster relationships               |
| **Used for**     | Anchoring the coverage map to the canonical cluster architecture (Step 2A)                 |

---

### ▸ entity-extraction.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source (optional)                                                          |
| **Receives**     | Entity map with Critical, Important, Optional entity classifications                        |
| **Used for**     | Entity-based hidden duplication detection (Phase 3D)                                       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                              |
|---------------------------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Content inventory is unavailable                                          | Primary input missing                                  | Attempt to construct partial inventory from memory. Flag output as PARTIAL AWARENESS — confidence LOW. All coverage classifications marked INFERRED. |
| Memory controller is unavailable                                          | READ query returns error                               | Proceed without cross-session context. Flag: "CROSS-SESSION AWARENESS UNAVAILABLE — current session data only." Note that prior gap resolutions cannot be verified. |
| No cluster data is available (no clustering output, no inventory clusters)| Step 2A — cluster enumeration produces 0 clusters     | Attempt to create emergent clusters from keyword groupings in the inventory. If that fails → create one cluster: "[Domain] — All Topics." Proceed with single-cluster awareness. Flag for user review. |
| All records in the inventory are PLANNED/DRAFT — no published content     | Step 2B — COVERED TOPICS list is empty for all clusters | Flag: "No published content detected. Coverage map reflects only pipeline intent." Treat all clusters as GAP with in-progress pipeline. Recommend publishing priority. |
| Freshness dates are unavailable for all records                           | Phase 4A — no date sources available for any record   | Skip age-based freshness labels. Apply keyword-based freshness detection only (time-sensitive topic detection). Flag: "Freshness audit limited — no publication dates available." |
| Overlap detection produces >50 alerts across all clusters                 | Step 3 — alert count exceeds threshold                | Flag site as HIGH DUPLICATION RISK. Present top 10 alerts by severity. Group remaining alerts by cluster. Recommend deduplication-engine.skill as the next step before any content creation. |
| A cluster's coverage status cannot be determined (decision tree produces no result) | Phase 6B — decision tree reaches no conclusion  | Default to PARTIAL. Flag: "Coverage status inconclusive — manual review recommended." |
| User's content pipeline contains only planned content with no keyword assignments | Step 1C — pipeline records have no keyword data   | Add pipeline records to the unified dataset as KEYWORD-MISSING entries. Do not block their inclusion. Flag each: "Keyword needed for this planned piece before gap analysis." |
| A freshness override signal conflicts with a recent publication date       | Phase 4B — override attempts to STALE a FRESH record  | Apply override only if the override signal strength is HIGH (e.g., confirmed outdated reference in content). If override signal is MEDIUM — flag as AGING instead. Do not force-STALE a FRESH record on weak signals. |
| Memory shows a gap was closed in a prior session but the corresponding URL is not in the current inventory | Step 1B — matched memory record shows gap CLOSED but no URL in inventory | Mark the gap as POSSIBLY CLOSED — UNVERIFIED. Do not re-open as a gap. Recommend verifying the URL's status manually. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — COVERAGE STATUS DECISION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
COVERAGE STATUS QUICK DECISION GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GAP:
  → 0 published content records in cluster
  → OR so minimal (1 thin page, score 1) that no real coverage exists
  → Action: CREATE ALL missing funnel stages

INCOMPLETE:
  → 1–2 pages published; coverage scores mostly 1–2
  → At least 2 funnel stages missing
  → May have overlap issues
  → Action: FIX existing thin pages; CREATE critical missing stages

PARTIAL:
  → Content exists in some but not all funnel stages
  → OR most content is at score 2–3 but missing depth
  → OR freshness risk on key pieces
  → Pipeline may be filling some gaps
  → Action: CREATE missing funnel stage; FIX/REFRESH weak pieces

COMPLETE:
  → Published content in all 3 funnel stages
  → Majority of records at score 3+
  → No CLUSTER FRESHNESS RISK
  → Minimal/no unresolved overlaps
  → Action: MONITOR; no new content required

DOWNGRADE TRIGGERS:
  CLUSTER FRESHNESS RISK   → -1 level
  2+ HIGH-severity overlaps → -1 level
  Pillar absent + 5+ pages  → -0.5 (note, not always a full level)

UPGRADE BLOCKS:
  Cannot be COMPLETE if any funnel stage is MISSING
  Cannot be PARTIAL if 0 published pages exist
  Cannot be INCOMPLETE without at least 1 published page
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — OVERLAP TYPE REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
OVERLAP TYPE REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
EXACT KEYWORD
  Definition : Same primary keyword in two records
  Severity   : HIGH
  Detection  : Exact string match after normalization
  Resolution : MERGE (if similar depth) or REDIRECT (if one is stronger)

SEMANTIC OVERLAP
  Definition : Different keywords; same user intent; would compete on SERP
  Severity   : MEDIUM
  Detection  : 2+ of 3 semantic overlap test questions = YES
  Resolution : DIFFERENTIATE (add distinct angle) or MERGE

INTENT CANNIBALIZATION
  Definition : Same cluster + same funnel stage + same user need
  Severity   : HIGH
  Detection  : Cluster + intent match + same primary user question
  Resolution : Designate primary; redirect secondary; or differentiate

HIDDEN DUPLICATE
  Definition : No shared keywords; no shared intent label; but semantically equivalent
  Severity   : MEDIUM
  Detection  : Topic fingerprint overlap ≥50% OR entity co-occurrence ≥70%
  Resolution : REVIEW and CONSOLIDATE if confirmed equivalent

CROSS-CLUSTER OVERLAP
  Definition : Two records in different clusters competing for same SERP position
  Severity   : LOW to MEDIUM
  Detection  : Cluster boundary analysis; secondary → primary keyword conflict
  Resolution : REASSIGN cluster; or remove conflicted secondary keyword

PLANNED CONFLICT
  Definition : Planned/draft piece targets same keyword as published piece
  Severity   : MEDIUM
  Detection  : Pipeline keyword vs. inventory keyword comparison
  Resolution : CANCEL planned; or DIFFERENTIATE by angle; or UPDATE existing instead

RESOLUTION PRIORITY:
  EXACT KEYWORD → INTENT CANNIBALIZATION → SEMANTIC OVERLAP →
  PLANNED CONFLICT → HIDDEN DUPLICATE → CROSS-CLUSTER OVERLAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — FRESHNESS SCORING REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
FRESHNESS SCORING QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AGE-BASED LABELS:
  < 6 months        → FRESH    (no action)
  6–12 months       → AGING    (monitor)
  12–24 months      → STALE    (refresh recommended)
  24–36 months      → OUTDATED (refresh required)
  36+ months        → CRITICALLY OUTDATED (urgent)

OVERRIDE SIGNALS (can upgrade to STALE/OUTDATED regardless of age):
  → Title contains a past year → minimum STALE
  → Topic is time-sensitive (rates, tools, rankings) + AGING → STALE
  → SERP top results all recent (<6mo) → upgrade to OUTDATED
  → Content references superseded tools/data → OUTDATED
  → Coverage score 1–2 AND age > 12 months → STALE minimum

REFRESH PRIORITY:
  CRITICALLY OUTDATED          → CRITICAL
  OUTDATED (any score)         → HIGH
  STALE + score 1–2            → HIGH
  STALE + score 3+ + time-sensitive → HIGH
  STALE + score 3+ + evergreen → MEDIUM
  AGING + score 1–2            → MEDIUM
  AGING + score 3+ + any       → MONITOR

CLUSTER FRESHNESS RISK:
  Triggered when ≥50% of cluster's pages are STALE or OUTDATED
  Effect: Cluster coverage status downgraded by one level
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: research/content-awareness.skill*
*Nexus SEO Operating System — Version 1.0.0*
