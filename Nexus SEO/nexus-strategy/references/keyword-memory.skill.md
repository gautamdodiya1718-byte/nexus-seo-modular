# SKILL: memory/keyword-memory.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Memory / Keyword Intelligence
# EXECUTION PRIORITY: FOUNDATIONAL — Called during keyword discovery, deduplication, roadmap planning, and content generation. Maintains the canonical keyword state for the entire project.
# POSITION: Keyword intelligence persistence layer. Every keyword that enters the Nexus system is registered, tracked, and lifecycle-managed here.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **keyword intelligence persistence and lifecycle management layer** of the Nexus memory system.

In a single-session tool, keyword discovery produces a list that disappears when the session ends. The next time the user runs keyword discovery, the same keywords are generated again — unaware that they were already discovered, already scored, already assigned to content, or already published. This produces redundant analysis, duplicated roadmap items, and wasted editorial resources.

This skill prevents that entirely. Every keyword discovered by `keyword-discovery.skill`, clustered by `semantic-clustering.skill`, scored by `opportunity-engine.skill`, assigned by `roadmap-engine.skill`, or published as content — is registered here with its full lifecycle state. When `keyword-discovery.skill` runs in a new session, it checks this memory first. When `deduplication-engine.skill` needs to know which keywords are already in the system, it queries this memory. When `roadmap-engine.skill` needs to avoid re-scheduling content that has already been written, it checks this memory.

The keyword memory is the canonical source of truth for the project's entire keyword universe — not just keywords from the current session, but every keyword ever discovered for this project, in any session, at any time.

### Primary Responsibilities

| Responsibility                          | Description                                                                                                           |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Keyword Registration**                | Store every new keyword with its complete metadata — intent, cluster, score, and status                              |
| **Deduplication Gate**                  | Before storing any keyword, check for exact matches, semantic duplicates, and same-intent overlaps                   |
| **Status Lifecycle Tracking**          | Track every keyword through its lifecycle: unused → assigned → in-progress → published                              |
| **Reuse Prevention**                    | Block downstream skills from generating content or roadmap items for keywords already in the pipeline               |
| **Cluster Coverage Tracking**          | Maintain cluster-level coverage metrics — keyword count, funnel balance, coverage status                             |
| **Stale Keyword Detection**            | Identify keywords that have been unused for extended periods and flag them for strategic review                      |
| **Keyword Retrieval**                   | Return the full keyword dataset or filtered subsets (by status, cluster, intent, or score) on demand                |
| **Status Update Handling**             | Accept lifecycle status updates from downstream skills as keywords move through the pipeline                         |
| **Content URL Registration**           | Record the published URL for keywords that have been written and published                                           |
| **Semantic Duplicate Resolution**      | When near-duplicate keywords exist, merge or suppress the lower-value variant with documented reasoning             |
| **Cross-Session Continuity**           | Maintain the cumulative keyword universe across all project sessions without loss                                    |

### What This Skill Is NOT

- It is **not** a keyword researcher. It stores and manages keyword data — it does not discover keywords.
- It is **not** a SERP analyzer. It tracks keyword metadata — it does not perform SERP analysis.
- It is **not** a content tracker. Content metadata is managed by `content-memory.skill`; this skill only stores the keyword's content_url as a reference.
- It is **not** an opportunity scorer. Opportunity scores are produced by `opportunity-engine.skill`; this skill stores and retrieves those scores.
- It is **not** a replacement for `deduplication-engine.skill`. This skill provides the memory data that `deduplication-engine.skill` queries — they are complementary, not redundant.

### The Foundational Principle of This Skill

> Every keyword is either new, in-flight, or done.
> No keyword should ever be "new" twice.
>
> The keyword memory layer enforces this rule across every session.
> A keyword discovered in January that produces a published article in March
> is NEVER re-discovered in June and re-added to the roadmap.
>
> The canonical state of every keyword lives here.
> Every skill reads from here before generating new keyword recommendations.
> Every skill writes to here when a keyword's state changes.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — STORAGE STRUCTURE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Keyword Record Schema

Every keyword is stored as a structured record:

```
KEYWORD RECORD SCHEMA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Field                   Type          Required  Description
────────────────────────────────────────────────────────────────────────────
keyword_id              string        YES       System-generated UUID — unique identifier for this keyword record
keyword                 string        YES       The exact keyword string as discovered (lowercase, no special chars)
keyword_normalized      string        YES       Lowercase, whitespace-normalized version for dedup comparison
project_id              string        YES       Foreign key reference to the parent project in project-memory
intent                  enum          YES       TOFU | MOFU | BOFU
funnel_stage            enum          YES       TOFU | MOFU | BOFU (same as intent — duplicate for explicit query support)
cluster                 string        YES       The cluster name from semantic-clustering.skill this keyword belongs to
cluster_id              string        YES       The cluster's ID reference for joins
primary_keyword         boolean       YES       true = this keyword is the primary keyword for its cluster or content; false = secondary/supporting
opportunity_score       number        SOFT      0–10 score from opportunity-engine.skill; null if not yet scored
d1_gap_level            number        SOFT      Dimension 1 score from opportunity-engine.skill (0–2)
d2_serp_weakness        number        SOFT      Dimension 2 score (0–2)
d3_intent_value         number        SOFT      Dimension 3 score (0–2)
d4_cluster_importance   number        SOFT      Dimension 4 score (0–2)
d5_differentiation      number        SOFT      Dimension 5 score (0–2)
status                  enum          YES       UNUSED | ASSIGNED | IN-PROGRESS | PUBLISHED | DEPRIORITIZED | REVIEW-NEEDED | STALE
content_url             string        SOFT      The published URL where this keyword is targeted; null until published
content_title           string        SOFT      The title of the published content piece; null until published
assigned_roadmap_item   string        SOFT      The roadmap item ID this keyword is assigned to; null if not in roadmap
session_discovered      string        YES       The session_id in which this keyword was first discovered
session_last_updated    string        YES       The session_id of the most recent status update
created_date            string        YES       ISO 8601 timestamp of first storage
last_updated            string        YES       ISO 8601 timestamp of most recent status change
days_in_current_status  number        SOFT      Calculated at retrieval: days since last_updated (not stored)
stale_threshold_days    number        SOFT      Default: 90. Days before UNUSED keyword is flagged STALE
keyword_type            enum          SOFT      PRIMARY | SECONDARY | LONG-TAIL | LSI — classification from metadata-generator
serp_difficulty         string        SOFT      EASY | MEDIUM | HARD — from serp-intelligence.skill
gap_type                string        SOFT      MISSING TOPIC | MISSING INTENT | MISSING DEPTH | MISSING FORMAT | MISSING FRESHNESS | EMERGING
source_skill            string        YES       Which skill first produced this keyword (keyword-discovery | gap-opportunity-engine | metadata-generator)
dedup_merged_from       list          SOFT      List of keyword_ids that were merged INTO this record (if this is the canonical version after a merge)
notes                   string        SOFT      Free-text notes from any session
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Cluster Coverage Record Schema

Alongside individual keyword records, this skill maintains a cluster-level summary:

```
CLUSTER COVERAGE RECORD SCHEMA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Field                   Type          Required  Description
────────────────────────────────────────────────────────────────────────────
cluster_memory_id       string        YES       System-generated UUID
cluster_name            string        YES       Matches cluster name in semantic-clustering.skill
cluster_id              string        YES       Matches cluster ID in semantic-clustering.skill
project_id              string        YES       Parent project reference
keyword_count           number        YES       Total keywords in this cluster across all statuses
unused_count            number        YES       Keywords with status UNUSED
assigned_count          number        YES       Keywords with status ASSIGNED
in_progress_count       number        YES       Keywords with status IN-PROGRESS
published_count         number        YES       Keywords with status PUBLISHED
deprioritized_count     number        YES       Keywords with status DEPRIORITIZED
tofu_count              number        YES       Keywords with intent TOFU
mofu_count              number        YES       Keywords with intent MOFU
bofu_count              number        YES       Keywords with intent BOFU
coverage_status         enum          YES       COMPLETE | PARTIAL | MINIMAL | MISSING
primary_keyword         string        SOFT      The single primary keyword for this cluster (if designated)
pillar_page_published   boolean       YES       true if a pillar page URL exists for this cluster's primary keyword
average_opportunity_score number      SOFT      Mean opportunity score across all scored keywords in cluster
highest_score_keyword   string        SOFT      Keyword with the highest opportunity score in this cluster
lowest_score_keyword    string        SOFT      Keyword with the lowest opportunity score
last_updated            string        YES       ISO 8601 timestamp of most recent update
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Status Enum Definitions

| Status           | Definition                                                                                   | Who Sets It                                |
|------------------|----------------------------------------------------------------------------------------------|--------------------------------------------|
| **UNUSED**       | Keyword discovered and stored; not yet assigned to any roadmap item or content              | Default at creation                        |
| **ASSIGNED**     | Keyword has been assigned to a roadmap item; content creation is planned                    | `roadmap-engine.skill` via status update  |
| **IN-PROGRESS**  | Content targeting this keyword is being written (draft exists)                              | `content-generation-engine.skill` updates |
| **PUBLISHED**    | Content has been published; `content_url` is populated                                      | User or `content-memory.skill` confirms   |
| **DEPRIORITIZED**| Keyword was reviewed and explicitly removed from priority — low score, HARD SERP, etc.     | `roadmap-engine.skill` or user decision   |
| **REVIEW-NEEDED**| Keyword flagged as uncertain — scoring unclear, intent ambiguous, or conflicting signals    | `opportunity-engine.skill` or manual      |
| **STALE**        | Keyword has been UNUSED for longer than `stale_threshold_days` (default 90 days)           | Auto-set by stale detection in Step 6     |

### Coverage Status Definitions

| Coverage Status | Condition                                                                            |
|-----------------|--------------------------------------------------------------------------------------|
| **COMPLETE**    | ≥80% of cluster keywords are PUBLISHED; all funnel stages (TOFU/MOFU/BOFU) have ≥1 PUBLISHED keyword |
| **PARTIAL**     | 30–79% of cluster keywords are PUBLISHED; at least one funnel stage has a PUBLISHED keyword |
| **MINIMAL**     | 1–29% of cluster keywords are PUBLISHED; only one funnel stage has any published content |
| **MISSING**     | 0% PUBLISHED keywords in this cluster; no published content exists                   |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Source                            | Data Provided                                                                              | Call Context     |
|-----------------------------------|--------------------------------------------------------------------------------------------|------------------|
| `keyword-discovery.skill`         | New keyword batches — keyword string, intent, category, strategic reason                  | WRITE (store new)|
| `semantic-clustering.skill`       | Cluster assignments — keyword-to-cluster mapping, cluster names and strength scores       | WRITE (assign)   |
| `opportunity-engine.skill`        | Opportunity scores — 0–10 total score + D1–D5 dimension scores per keyword               | WRITE (score)    |
| `roadmap-engine.skill`            | Status update: ASSIGNED — keyword is now in the roadmap                                   | WRITE (status)   |
| `content-generation-engine.skill` | Status update: IN-PROGRESS — content is being written                                    | WRITE (status)   |
| `content-memory.skill`            | Status update: PUBLISHED + content_url + content_title                                   | WRITE (status)   |
| `deduplication-engine.skill`      | Query — check if a keyword already exists before generation proceeds                      | READ (query)     |
| `memory-controller.skill`         | All operations are routed through memory-controller                                       | ALL              |

### Read Query Types

When called with a READ operation, the query type determines what is returned:

| Query Type                        | Parameters                                    | Returns                                                   |
|-----------------------------------|-----------------------------------------------|-----------------------------------------------------------|
| `GET_ALL`                         | project_id                                    | Complete keyword dataset for the project                  |
| `GET_BY_STATUS`                   | project_id + status                           | All keywords matching the specified status                |
| `GET_BY_CLUSTER`                  | project_id + cluster_name                     | All keywords in the specified cluster                     |
| `GET_BY_INTENT`                   | project_id + intent                           | All keywords with the specified intent (TOFU/MOFU/BOFU)  |
| `GET_CLUSTER_COVERAGE`            | project_id                                    | All cluster coverage records                              |
| `CHECK_KEYWORD`                   | project_id + keyword_string                   | Deduplication check — returns match data or null          |
| `GET_STALE`                       | project_id                                    | All keywords with status STALE                            |
| `GET_HIGH_PRIORITY_UNUSED`        | project_id                                    | Unused keywords with opportunity_score ≥ 7               |
| `GET_KEYWORD_STATS`               | project_id                                    | Aggregate counts by status, intent, cluster               |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Output 1 — Write Confirmation

```
KEYWORD WRITE CONFIRMATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
operation           : [STORE_NEW / UPDATE_STATUS / UPDATE_SCORE / MERGE]
keywords_processed  : [count]
  successfully stored : [count]
  duplicates rejected : [count]
  merged              : [count]
  errors              : [count]
project_id          : [UUID]
session_id          : [UUID]
timestamp           : [ISO 8601]
status              : SUCCESS / PARTIAL / FAILED
partial_details     : [if PARTIAL — which keywords failed and why]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 2 — Keyword Dataset (GET_ALL Query)

```
KEYWORD DATASET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id          : [UUID]
total_keywords      : [count]
as_of               : [ISO 8601]

STATUS BREAKDOWN:
  UNUSED            : [count] ([%])
  ASSIGNED          : [count] ([%])
  IN-PROGRESS       : [count] ([%])
  PUBLISHED         : [count] ([%])
  DEPRIORITIZED     : [count] ([%])
  REVIEW-NEEDED     : [count] ([%])
  STALE             : [count] ([%])

INTENT BREAKDOWN:
  TOFU              : [count] ([%])
  MOFU              : [count] ([%])
  BOFU              : [count] ([%])

CLUSTER SUMMARY:
  [cluster_name]    : [keyword_count] keywords | Coverage: [status]
  [cluster_name]    : [keyword_count] keywords | Coverage: [status]
  ...

FULL KEYWORD LIST: [see keyword records below]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 3 — Deduplication Check Result (CHECK_KEYWORD Query)

```
DEDUPLICATION CHECK RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
keyword_queried     : "[keyword string]"
exact_match         : [YES — keyword_id: UUID | NO]
semantic_match      : [YES — "[matched keyword]" (similarity: N%) | NO]
intent_match        : [YES — same intent + same cluster: "[matched keyword]" | NO]
recommendation      : [ALLOW_STORE / REJECT_DUPLICATE / MERGE_WITH / REVIEW]
merge_target_id     : [keyword_id to merge with — if MERGE_WITH]
reason              : [specific reason for recommendation]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 4 — Keyword Statistics (GET_KEYWORD_STATS Query)

```
KEYWORD STATISTICS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id                    : [UUID]
total_keywords                : [count]
total_clusters_tracked        : [count]
keywords_without_scores       : [count]
high_priority_unused (≥7)     : [count]
stale_keywords                : [count]
published_ratio               : [count published / total × 100]%
funnel_coverage:
  TOFU published              : [count] / [total TOFU]
  MOFU published              : [count] / [total MOFU]
  BOFU published              : [count] / [total BOFU]
cluster_with_most_keywords    : [cluster_name] ([count] keywords)
cluster_with_most_published   : [cluster_name] ([count] published)
cluster_coverage_complete     : [count] clusters
cluster_coverage_partial      : [count] clusters
cluster_coverage_missing      : [count] clusters
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — KEYWORD STORAGE

Receive and store new keywords from upstream discovery and analysis skills.

**Phase 1A — Incoming Keyword Batch Processing**

When keywords arrive from `keyword-discovery.skill`, `gap-opportunity-engine.skill`, or `metadata-generator.skill`:

1. Extract all keyword-level fields available in the incoming batch.
2. For each keyword, run the deduplication gate (Step 2) before any storage operation.
3. Only keywords that pass the dedup gate proceed to storage.
4. All rejected keywords are logged in the write confirmation.

**Phase 1B — Keyword String Normalization**

Before any storage or comparison, normalize every keyword string:

1. Lowercase all characters.
2. Trim leading and trailing whitespace.
3. Collapse internal multiple spaces to single space.
4. Remove special characters that do not affect semantic meaning (", ', !, ?). Exception: retain hyphens in hyphenated compounds ("e-commerce", "b2b").
5. Store both the original string (`keyword`) and the normalized version (`keyword_normalized`).

**Normalization examples:**

| Raw Input                          | Normalized                          |
|------------------------------------|-------------------------------------|
| `"Best CRM Software"`              | `"best crm software"`              |
| `"  crm for startups  "`          | `"crm for startups"`               |
| `"B2B CRM Tools"`                  | `"b2b crm tools"`                  |
| `"CRM Software's Features"`        | `"crm softwares features"`          |
| `"E-commerce CRM"`                 | `"e-commerce crm"`                 |

**Phase 1C — Default Field Assignment**

For every new keyword being stored, assign defaults for optional fields that are not yet available:

| Field                   | Default Value at Creation               |
|-------------------------|-----------------------------------------|
| `status`                | UNUSED                                  |
| `opportunity_score`     | null (to be filled by opportunity-engine)|
| `d1_gap_level`          | null                                    |
| `d2_serp_weakness`      | null                                    |
| `d3_intent_value`       | null                                    |
| `d4_cluster_importance` | null                                    |
| `d5_differentiation`    | null                                    |
| `content_url`           | null                                    |
| `content_title`         | null                                    |
| `assigned_roadmap_item` | null                                    |
| `stale_threshold_days`  | 90                                      |
| `dedup_merged_from`     | [] (empty list)                         |
| `notes`                 | null                                    |

**Phase 1D — Batch Storage Execution**

After dedup gate and default assignment:
1. Generate a `keyword_id` (UUID) for each new keyword.
2. Set `created_date` and `last_updated` to current timestamp.
3. Set `session_discovered` to the current `session_id`.
4. Set `session_last_updated` to the current `session_id`.
5. Write the complete keyword record to memory.
6. Update the cluster coverage record for the keyword's assigned cluster (Step 5).

**Phase 1E — Batch Storage Key Format**

```
Storage Key Format:
  keywords:[project_id]:[keyword_id]        → Full keyword record
  keywords:index:[project_id]:exact:[normalized_keyword] → Points to keyword_id
  keywords:index:[project_id]:cluster:[cluster_name]     → List of keyword_ids in cluster
  keywords:index:[project_id]:status:[status]            → List of keyword_ids by status
  keywords:index:[project_id]:intent:[intent]            → List of keyword_ids by intent
  keywords:cluster:[project_id]:[cluster_id]             → Cluster coverage record
  keywords:stats:[project_id]                             → Aggregate statistics record
```

---

### ▸ STEP 2 — DEDUPLICATION GATE

Before storing any keyword, check for existing records that would make the new keyword redundant.

**Phase 2A — Exact Match Check**

1. Take the `keyword_normalized` of the incoming keyword.
2. Query the exact index: `keywords:index:[project_id]:exact:[normalized_keyword]`.
3. If a match is found → **REJECT as EXACT DUPLICATE**.
4. Log: `"Keyword '[keyword]' rejected — exact match found: keyword_id [UUID], status [status]"`

**Exact Match Exception — Refresh Detection:**

If an exact match is found but its status is STALE → do NOT reject. Instead:
- Update the existing keyword record's status from STALE back to UNUSED.
- Update `last_updated` and `session_last_updated`.
- Return: REFRESHED (not REJECT and not a new store).
- Log: `"Keyword '[keyword]' refreshed from STALE to UNUSED — keyword_id [UUID]"`

**Phase 2B — Semantic Match Check**

When no exact match is found, apply semantic similarity comparison:

**Semantic Similarity Detection:**

For each existing keyword in the same cluster as the incoming keyword, calculate semantic overlap:

1. **Shared word ratio:** `shared_words / max(words_in_A, words_in_B)` — what fraction of words are the same?
2. **Root word overlap:** Strip common suffixes (-ing, -er, -s, -ed) and compare root words.
3. **Concept identity:** Do both keywords express the exact same searcher intent with different phrasing?

| Similarity Score | Classification                           |
|------------------|------------------------------------------|
| ≥ 85%            | SEMANTIC DUPLICATE — reject or merge     |
| 70–84%           | HIGH SIMILARITY — flag for review; do not auto-reject |
| 50–69%           | MODERATE SIMILARITY — different enough to store; note relationship |
| < 50%            | DISTINCT — store without concern         |

**Semantic Match Examples:**

| Incoming Keyword                   | Existing Keyword                     | Similarity | Action         |
|------------------------------------|--------------------------------------|------------|----------------|
| `"crm for small business"`         | `"small business crm"`              | 90%        | SEMANTIC DUPLICATE → reject |
| `"best crm software 2024"`         | `"best crm software"`               | 88%        | SEMANTIC DUPLICATE → merge (add year to existing) |
| `"crm implementation guide"`       | `"how to implement a crm"`          | 75%        | HIGH SIMILARITY → flag for review |
| `"crm tools for startups"`         | `"crm software for startups"`       | 65%        | MODERATE → store with relationship note |
| `"crm database management"`        | `"crm contact management"`          | 45%        | DISTINCT → store |

**Phase 2C — Intent-Cluster Duplication Check**

Beyond semantic similarity, check for intent duplication within the same cluster:

An intent-cluster duplicate exists when:
- The incoming keyword targets the exact same searcher intent as an existing keyword.
- Both are in the same cluster.
- Both have the same funnel stage.

This is a stricter check than semantic similarity — it catches cases where the keywords use completely different words but target the exact same query intent.

**Detection method:** Compare the `intent` field and `cluster` field. Then check: would content created for the incoming keyword directly compete with content targeting the existing keyword? If YES → INTENT DUPLICATE.

| Condition                                                                       | Action                                               |
|---------------------------------------------------------------------------------|------------------------------------------------------|
| Same cluster + same intent + ≥ 85% semantic similarity                         | REJECT as SEMANTIC DUPLICATE                        |
| Same cluster + same intent + 70–84% similarity                                 | FLAG as REVIEW-NEEDED; store with flag              |
| Same cluster + same intent + < 70% similarity                                  | ALLOW — different enough to justify separate content|
| Same intent + DIFFERENT cluster                                                 | ALLOW — different topical territory                 |
| Same cluster + DIFFERENT intent                                                 | ALLOW — different funnel stage                      |

**Phase 2D — Dedup Decision Record**

For every keyword processed through the dedup gate, record the decision:

```
DEDUP DECISION RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
incoming_keyword    : "[keyword]"
check_1_exact       : [PASS / REJECT — matched keyword_id]
check_2_semantic    : [PASS / REJECT / FLAG — similarity %]
check_3_intent      : [PASS / REJECT / FLAG]
final_decision      : [ALLOW_STORE / REJECT_EXACT / REJECT_SEMANTIC / MERGE / REVIEW]
action_taken        : [description of what happened]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 2E — Merge Protocol**

When two keywords are near-duplicates where one is clearly more specific than the other (e.g., `"best crm software"` vs. `"best crm software 2024"`):

1. Designate the more specific or more recent keyword as the CANONICAL version.
2. Update the canonical record with any superior metadata from the incoming keyword.
3. Mark the less specific keyword's `dedup_merged_from` list with the absorbed keyword's UUID.
4. Do NOT delete the absorbed keyword — store it as DEPRIORITIZED with a note: `"Merged into [canonical_keyword_id] — [reason]"`.
5. Log the merge.

---

### ▸ STEP 3 — STATUS TRACKING

Update keyword status as it moves through the content production lifecycle.

**Phase 3A — Status Transition Rules**

Status transitions follow strict rules — only valid forward transitions are permitted:

```
STATUS TRANSITION MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
UNUSED ──────────→ ASSIGNED (when added to roadmap)
UNUSED ──────────→ DEPRIORITIZED (when scored LOW and explicitly removed)
UNUSED ──────────→ STALE (when not used for > 90 days — automatic)
UNUSED ──────────→ REVIEW-NEEDED (when scoring is uncertain)

ASSIGNED ────────→ IN-PROGRESS (when content creation begins)
ASSIGNED ────────→ UNUSED (if removed from roadmap — reverse is allowed)
ASSIGNED ────────→ DEPRIORITIZED (if roadmap item cancelled)

IN-PROGRESS ─────→ PUBLISHED (when content is live)
IN-PROGRESS ─────→ ASSIGNED (if writing is paused before completion)

PUBLISHED ───────→ [no further transitions — terminal state]

STALE ───────────→ UNUSED (when re-discovered or user reviews — see Phase 2A)
STALE ───────────→ DEPRIORITIZED (when explicitly removed after stale review)

REVIEW-NEEDED ───→ UNUSED (after review confirms keyword is valid)
REVIEW-NEEDED ───→ DEPRIORITIZED (after review confirms keyword should be dropped)

DEPRIORITIZED ───→ UNUSED (if re-evaluated and reinstated — allowed)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Invalid transitions** (any transition not in the map above) are rejected with:
`"Invalid status transition for keyword_id [UUID]: [current_status] → [requested_status]. Check the status transition map."`

**Phase 3B — Status Update Execution**

When a status update is received from a calling skill:

1. Validate that the transition is permitted (Phase 3A).
2. Load the existing keyword record.
3. Update the `status` field.
4. Update `last_updated` to current timestamp.
5. Update `session_last_updated` to the current session_id.
6. If status = PUBLISHED → also require `content_url` and `content_title`; reject PUBLISHED update without these fields.
7. If status = ASSIGNED → also require `assigned_roadmap_item`; reject ASSIGNED update without this field.
8. Write the updated record.
9. Update the cluster coverage record to reflect the new status count (Step 5).

**Phase 3C — Status Update Confirmation**

```
STATUS UPDATE CONFIRMATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
keyword_id          : [UUID]
keyword             : "[keyword]"
previous_status     : [old status]
new_status          : [new status]
updated_by          : [calling skill]
session_id          : [session_id]
timestamp           : [ISO 8601]
additional_fields   : [any fields updated alongside status — e.g., content_url]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 4 — PREVENT REUSE

Block downstream skills from generating redundant content or roadmap items for keywords that are already in the pipeline.

**Phase 4A — Pre-Generation Cross-Check**

When any skill is about to generate new content recommendations or roadmap items, it must first query this memory for the target keyword. The response determines whether generation should proceed:

| Keyword Status at Query Time | Instruction to Calling Skill                                                      |
|------------------------------|-----------------------------------------------------------------------------------|
| UNUSED                       | ALLOW — keyword is available for use; include in recommendations                 |
| ASSIGNED                     | BLOCK — keyword is already in the roadmap; do not create a duplicate item        |
| IN-PROGRESS                  | BLOCK — content is being written; do not create a duplicate item                 |
| PUBLISHED                    | BLOCK — content exists at [content_url]; do not re-create                        |
| DEPRIORITIZED                | SOFT BLOCK — keyword was explicitly dropped; surface it with a note before proceeding |
| REVIEW-NEEDED                | ALLOW WITH FLAG — keyword can proceed but is flagged as uncertain                |
| STALE                        | ALLOW WITH REFRESH — keyword can be re-used; update status to UNUSED first       |

**Phase 4B — Batch Pre-Generation Block**

When a skill generates a batch of keywords for a new analysis session, this memory returns the cross-check status for the entire batch at once:

```
BATCH CROSS-CHECK RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id          : [UUID]
batch_size          : [count of keywords checked]
  ALLOW             : [count] — new keywords, safe to use
  BLOCK (ASSIGNED)  : [count] — already in roadmap
  BLOCK (IN-PROGRESS): [count] — content being written
  BLOCK (PUBLISHED) : [count] — already published
  SOFT BLOCK        : [count] — deprioritized; review first
  ALLOW WITH FLAG   : [count] — review-needed status
  REFRESHED (STALE) : [count] — stale keywords refreshed to UNUSED

BLOCKED KEYWORDS:
  "[keyword]" → [status] (content_url if PUBLISHED)
  ...

ALLOWED KEYWORDS:
  "[keyword]" → UNUSED / REVIEW-NEEDED / REFRESHED
  ...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 4C — Historical Awareness Mode**

When `keyword-discovery.skill` runs in delta mode (continuing a prior session), the cross-check provides additional context:

- How many prior sessions have run for this project.
- Which clusters have been recently analyzed (within 14 days).
- Which clusters have high UNUSED keyword inventory (suggesting keyword generation should focus elsewhere).

This allows `keyword-discovery.skill` to focus discovery resources on gaps rather than re-discovering what already exists.

---

### ▸ STEP 5 — CLUSTER TRACKING

Maintain cluster-level coverage metrics that provide a real-time picture of where the keyword universe is fully developed vs. sparse.

**Phase 5A — Cluster Coverage Record Updates**

Any time a keyword's status changes, the cluster coverage record for its cluster is updated:

1. Load the cluster coverage record for `cluster_name`.
2. Recalculate all count fields by querying the keyword index for this cluster and counting by status.
3. Recalculate TOFU/MOFU/BOFU counts by querying by intent within the cluster.
4. Recalculate `average_opportunity_score` from all scored keywords in the cluster.
5. Update `coverage_status` using the definitions in Section 2.
6. Update `last_updated`.
7. Write the updated cluster coverage record.

**Phase 5B — Coverage Status Recalculation**

Calculate `coverage_status` as follows:

```
COVERAGE STATUS CALCULATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
published_ratio = published_count / keyword_count

IF published_ratio ≥ 0.80
   AND tofu has ≥1 PUBLISHED keyword
   AND mofu has ≥1 PUBLISHED keyword
   AND bofu has ≥1 PUBLISHED keyword
→ coverage_status = COMPLETE

ELIF published_ratio ≥ 0.30
   AND at least 1 funnel stage has ≥1 PUBLISHED keyword
→ coverage_status = PARTIAL

ELIF published_ratio > 0 (any published)
   AND only 1 funnel stage has published keywords
→ coverage_status = MINIMAL

ELSE (no published keywords)
→ coverage_status = MISSING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 5C — Pillar Page Tracking**

For each cluster, track whether a pillar page has been published:

1. Among all PUBLISHED keywords in the cluster, check if any keyword has `primary_keyword` = true.
2. If yes → set `pillar_page_published` = true and record the pillar keyword's `content_url`.
3. If no published primary keyword exists → `pillar_page_published` = false.

**Phase 5D — Cluster Evolution Tracking**

Track how cluster coverage changes over time:

When `pillar_page_published` changes from false to true → log:
`"CLUSTER MILESTONE: Pillar page published for cluster '[cluster_name]' — cluster now has its hub page."`

When `coverage_status` changes → log:
`"CLUSTER STATUS CHANGE: '[cluster_name]' moved from [old_status] to [new_status]"`

These milestone logs are visible in the cluster coverage record's history and are surfaced to `strategist-ai.skill` for strategic context.

---

### ▸ STEP 6 — STALE KEYWORD DETECTION

Identify keywords that have sat UNUSED for longer than the project's stale threshold and flag them for strategic review.

**Phase 6A — Stale Detection Trigger**

Stale detection runs:
- At the start of every new session (as part of context loading).
- When `GET_STALE` query is received.
- After any bulk status update (to catch newly stale items).

**Phase 6B — Stale Identification Logic**

For every keyword with status UNUSED:

1. Calculate days since `last_updated`: `today - last_updated`.
2. Compare to `stale_threshold_days` for that keyword (default: 90).
3. If `days_since_last_updated ≥ stale_threshold_days` → mark as STALE.
4. Update `status` from UNUSED to STALE.
5. Update `last_updated` to the detection timestamp.
6. Add a note: `"Auto-marked STALE on [date] — unused for [N] days. Review for roadmap inclusion or deprioritization."`

**Phase 6C — Stale Threshold Customization**

The `stale_threshold_days` field is per-keyword, enabling different thresholds for different keyword types:

| Keyword Characteristic                        | Recommended Threshold   |
|-----------------------------------------------|-------------------------|
| BOFU keyword (high conversion intent)         | 60 days                 |
| MOFU keyword (evaluation intent)              | 75 days                 |
| TOFU keyword (informational intent)           | 90 days (default)       |
| EMERGING keyword (first-mover window)         | 30 days                 |
| DEPRIORITIZED keyword                         | No stale threshold — already inactive |

**Phase 6D — Stale Review Surfacing**

When stale keywords are detected, they are surfaced to the user as part of the session context:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️  STALE KEYWORD ALERT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[N] keywords have been UNUSED for more than [threshold] days:

| # | Keyword                    | Cluster          | Score | Days Unused | Recommendation              |
|---|----------------------------|------------------|-------|-------------|----------------------------|
| 1 | [keyword]                  | [cluster]        | [N]   | [N] days    | INCLUDE IN NEXT ROADMAP CYCLE |
| 2 | [keyword]                  | [cluster]        | [N]   | [N] days    | REVIEW — score may be outdated |
| 3 | [keyword]                  | [cluster]        | [N]   | [N] days    | DEPRIORITIZE — SERP may have hardened |

Review these keywords and update their status, or they will remain STALE.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 7 — RETRIEVAL

Return keyword data to requesting skills and to `memory-controller.skill` based on the query type.

**Phase 7A — Query Routing**

Route each incoming query to the appropriate retrieval method:

| Query Type                 | Retrieval Method                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------|
| `GET_ALL`                  | Load all keyword records for project; assemble dataset summary; return                     |
| `GET_BY_STATUS`            | Query `keywords:index:[project_id]:status:[status]`; load each matching record; return     |
| `GET_BY_CLUSTER`           | Query `keywords:index:[project_id]:cluster:[cluster_name]`; load matching records; return  |
| `GET_BY_INTENT`            | Query `keywords:index:[project_id]:intent:[intent]`; load matching records; return         |
| `GET_CLUSTER_COVERAGE`     | Load all cluster coverage records for project; return                                       |
| `CHECK_KEYWORD`            | Run dedup gate (Phase 2A-2C) on query keyword; return decision record without storing      |
| `GET_STALE`                | Query `keywords:index:[project_id]:status:STALE`; load matching records; return            |
| `GET_HIGH_PRIORITY_UNUSED` | Query status:UNUSED; filter where `opportunity_score ≥ 7`; sort by score desc; return      |
| `GET_KEYWORD_STATS`        | Load `keywords:stats:[project_id]`; calculate fresh if stale; return                       |

**Phase 7B — Enrichment at Retrieval**

When returning keyword records, calculate these derived fields at retrieval time (not stored):

```
DERIVED FIELDS CALCULATED AT RETRIEVAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
days_in_current_status:
  Calculate: today - last_updated (in days)
  Purpose: Shows how long keyword has been in its current status

priority_tier:
  HIGH if opportunity_score ≥ 8
  MEDIUM if opportunity_score 5–7
  LOW if opportunity_score < 5
  UNSCORED if opportunity_score is null

stale_risk:
  IMMINENT if status=UNUSED and days_since_update ≥ 75
  AT-RISK if status=UNUSED and days_since_update ≥ 50
  NONE otherwise

funnel_gap_flag:
  TRUE if this keyword fills a missing funnel stage for its cluster
  FALSE if the cluster already has coverage at this funnel stage
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 7C — Sorted Return Order**

Unless the calling skill specifies a sort order, return keyword records in this default order:

1. Status priority: IN-PROGRESS → ASSIGNED → UNUSED (HIGH score) → UNUSED (MEDIUM score) → UNUSED (LOW/unscored) → REVIEW-NEEDED → STALE → DEPRIORITIZED → PUBLISHED (at end)
2. Within the same status: sort by `opportunity_score` descending (highest first).
3. Within the same score: sort by `intent` — BOFU → MOFU → TOFU.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ SEMANTIC DUPLICATION DETECTION (ADVANCED)

The standard semantic match in Step 2B uses word overlap ratios. This advanced module adds a layer of conceptual identity detection.

**Conceptual Identity Detection:**

Two keywords express conceptual identity (even with low word overlap) when:

| Pattern                                           | Example                                              |
|---------------------------------------------------|------------------------------------------------------|
| Question form + declarative form                 | `"what is a crm"` = `"crm definition"`             |
| Verb form + noun form                            | `"implementing crm"` = `"crm implementation"`      |
| Audience-first vs. product-first                 | `"crm for startups"` ≈ `"startup crm software"` (high overlap) |
| Singular vs. plural with same meaning            | `"crm tool"` ≈ `"crm tools"`                       |
| Abbreviation vs. full form                       | `"crm system"` ≈ `"customer relationship management system"` |
| Synonym substitution (same cluster)             | `"best crm platform"` ≈ `"top crm system"` in the same cluster |

When conceptual identity is detected → apply the same logic as semantic similarity ≥ 85%: REJECT or MERGE.

**Conceptual Identity False Positives to Avoid:**

These are NOT conceptual duplicates even though they appear similar:

| Pair                                                | Why NOT a Duplicate                                                     |
|-----------------------------------------------------|-------------------------------------------------------------------------|
| `"crm for small business"` + `"crm for enterprise"` | Different audiences → different content → different intent             |
| `"crm setup guide"` + `"crm implementation guide"`  | "setup" suggests quick start; "implementation" suggests full process — different scope |
| `"free crm software"` + `"crm software"` (general)  | "free" qualifier changes the SERP intent fundamentally                 |

---

### ▸ LIFECYCLE TRACKING

Track keyword lifecycle velocity — how quickly keywords move through the pipeline — and surface patterns to strategy skills.

**Lifecycle Metrics Calculated:**

| Metric                                   | Calculation                                                               |
|------------------------------------------|---------------------------------------------------------------------------|
| `avg_time_to_publish`                   | Average days from `created_date` to PUBLISHED status across all published keywords |
| `avg_time_unused_before_assignment`     | Average days keywords spend UNUSED before moving to ASSIGNED             |
| `assignment_rate`                       | Count ASSIGNED/IN-PROGRESS/PUBLISHED ÷ total non-DEPRIORITIZED keywords  |
| `publication_rate`                      | Count PUBLISHED ÷ total non-DEPRIORITIZED keywords                       |
| `stale_rate`                            | Count STALE ÷ total UNUSED + STALE keywords                             |

These metrics are calculated and returned as part of `GET_KEYWORD_STATS` and are surfaced to `strategist-ai.skill` for strategic assessment.

**Lifecycle Velocity Insight:**

When lifecycle metrics indicate a problem, this skill generates a specific insight:

| Condition                                   | Insight Generated                                                                            |
|---------------------------------------------|----------------------------------------------------------------------------------------------|
| `avg_time_unused_before_assignment` > 60 days | "PIPELINE VELOCITY ISSUE: Keywords are sitting UNUSED for an average of [N] days before being assigned to roadmap items. The discovery-to-execution gap is slowing strategy." |
| `stale_rate` > 40%                         | "HIGH STALE RATE: [N%] of unused keywords have gone stale. Either the keyword discovery is outpacing execution capacity, or many discovered keywords are not viable." |
| `publication_rate` < 20% after 3+ sessions | "LOW PUBLICATION RATE: Only [N%] of tracked keywords have published content after [N] sessions. Consider narrowing the keyword universe to increase focus." |

---

### ▸ CLUSTER EVOLUTION

Track how clusters evolve over time — from empty to partially covered to fully covered — and provide evolution data to `authority-engine.skill` and `strategist-ai.skill`.

**Cluster Evolution Milestones:**

| Milestone Event                                        | Trigger Condition                                      |
|--------------------------------------------------------|--------------------------------------------------------|
| CLUSTER ACTIVATED                                      | First keyword stored in the cluster                   |
| FIRST KEYWORD ASSIGNED                                 | First keyword moves to ASSIGNED                       |
| PILLAR PAGE LIVE                                       | Primary keyword moves to PUBLISHED                    |
| FUNNEL PARTIALLY COMPLETE                              | ≥1 PUBLISHED keyword in 2 of 3 funnel stages         |
| FUNNEL COMPLETE                                        | ≥1 PUBLISHED keyword in all 3 funnel stages           |
| CLUSTER COVERAGE COMPLETE                             | coverage_status transitions to COMPLETE               |

Each milestone is timestamped and stored in the cluster coverage record's milestone log. `strategist-ai.skill` uses milestone data to project how much work remains to complete each cluster.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate Keyword Strings

No two keyword records for the same project may have the same `keyword_normalized` value. The exact index (`keywords:index:[project_id]:exact:[normalized_keyword]`) enforces this. A second attempt to store the same normalized string is blocked at the dedup gate.

### Rule 2 — No Semantic Duplicate Keywords in the Same Cluster

No two active (non-DEPRIORITIZED) keyword records for the same project and same cluster may have semantic similarity ≥ 85%. When such a pair is detected during ingestion, the lower-priority keyword (lower opportunity score; less specific) is MERGED into the higher-priority one.

### Rule 3 — No Same-Intent-Same-Cluster Duplication Beyond Threshold

Within any given cluster, no two keywords should produce content that directly competes for the same searcher intent. The intent-cluster duplication check (Phase 2C) enforces this by requiring < 70% semantic similarity for keywords sharing the same cluster and funnel stage.

### Rule 4 — No Cluster Coverage Record Duplicates

Each cluster for each project has exactly one cluster coverage record. If a second cluster coverage record is created for the same cluster_name + project_id combination → the newer record is merged into the existing one; the newer record is not stored separately.

### Rule 5 — No Status Update Without Previous State Record

Every status update appends to the status change history (not stored in the main record — stored in a separate `keywords:history:[keyword_id]` key). Before applying a new status, the previous status is recorded. This prevents silent overwrites and enables audit trailing. No status change may proceed without a valid previous state being recorded.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Keyword Memory Behaviors

| Prohibited Behavior                                                                             | Reason                                                                           |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Storing a keyword without a cluster assignment                                                  | Every keyword belongs to a cluster; unclustered keywords produce no strategic intelligence |
| Storing a keyword without an intent classification (TOFU/MOFU/BOFU)                           | Intent determines which strategic decisions this keyword informs                 |
| Setting status to PUBLISHED without `content_url`                                              | PUBLISHED is a verifiable state — it requires proof (a URL)                      |
| Setting status to ASSIGNED without `assigned_roadmap_item`                                     | ASSIGNED is only meaningful if it traces to a specific roadmap item               |
| Storing keywords with vague or generic `keyword_type` classifications                         | Keyword type is a lookup field — it must be one of the defined enum values       |
| Running stale detection without updating `last_updated` on the stale record                   | Stale marking is itself a status update — the timestamp must reflect it         |
| Rejecting REVIEW-NEEDED keywords at the dedup gate                                             | REVIEW-NEEDED keywords should still be queryable — they are not invalid keywords |
| Overwriting a PUBLISHED keyword's `content_url` without logging the change                    | Published URLs are canonical — overwrites must be auditable                      |
| Allowing duplicate `keyword_normalized` values by accepting a slightly modified version        | The normalized comparison exists to prevent gaming the dedup gate               |
| Auto-resolving REVIEW-NEEDED status without user confirmation                                  | REVIEW-NEEDED requires human judgment — it cannot be auto-resolved by the system |

### Required Keyword Memory Behaviors

- Every keyword must have `keyword`, `keyword_normalized`, `project_id`, `intent`, `cluster`, `status`, `created_date`, and `session_discovered` populated before storage.
- Stale detection runs on every session start.
- Every status change must produce a status update confirmation.
- Cluster coverage records must be updated after every status change.
- The dedup gate must run before every storage operation — never bypassed.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 9 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | All operations routed through `memory-controller.skill`                                     |
| **Calls handled**| STORE_NEW, UPDATE_STATUS, UPDATE_SCORE, MERGE, GET_* queries, CHECK_KEYWORD                |
| **Priority**     | Called before all analysis runs; called after all content publications                     |

---

### ▸ keyword-discovery.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source — sends new keyword batches                                            |
| **Receives from**| Raw keyword batches with intent, category, strategic reason                                |
| **Sends back**   | BATCH CROSS-CHECK RESULT — which keywords are new vs. already in memory                   |
| **Delta mode**   | When keywords are already in memory, `keyword-discovery.skill` skips re-discovering them  |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream enrichment — sends cluster assignments for stored keywords                        |
| **Sends**        | `cluster_name`, `cluster_id`, `primary_keyword` flag for each keyword                     |
| **Used for**     | Phase 1D (cluster assignment at storage) and Step 5 (cluster coverage tracking)           |

---

### ▸ opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream enrichment — sends opportunity scores                                             |
| **Sends**        | `opportunity_score`, `d1_gap_level` through `d5_differentiation` for each keyword         |
| **Triggers**     | UPDATE_SCORE operation on existing keyword records                                         |

---

### ▸ deduplication-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Calling skill — queries this memory for dedup checks                                       |
| **Query type**   | CHECK_KEYWORD — receives the deduplication check result before proceeding with generation  |
| **Relationship** | `deduplication-engine.skill` applies the broader system-wide dedup; this memory is its authoritative data source for keyword state |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Downstream consumer AND status updater                                                      |
| **Queries**      | `GET_HIGH_PRIORITY_UNUSED` to build roadmap from unactioned keywords                      |
| **Updates**      | Sets status = ASSIGNED + `assigned_roadmap_item` when keywords enter the roadmap           |
| **Blocks**       | Receives batch cross-check to avoid scheduling already-assigned keywords                   |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Status updater                                                                              |
| **Updates**      | Sets status = IN-PROGRESS when content generation begins for a keyword                    |

---

### ▸ content-memory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Sibling memory skill — coordinates PUBLISHED status updates                               |
| **Relationship** | When `content-memory.skill` records a published article → it triggers keyword-memory to update affected keywords to PUBLISHED with the content_url |

---

### ▸ project-memory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Sibling memory skill — receives aggregate keyword counts                                   |
| **Sends**        | Delta `keywords_discovered` count at the end of every session for project record aggregation |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                            |
|---------------------------------------------------------------------------|--------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| A keyword arrives without a cluster assignment                            | Phase 1D — cluster field is null                       | Store with cluster = "UNCLASSIFIED". Flag for semantic-clustering review. Do not block storage.            |
| A keyword arrives without an intent classification                        | Phase 1D — intent field is null                        | Infer intent from keyword pattern (informational = TOFU, commercial = MOFU, transactional = BOFU). Store with flag: "INTENT: INFERRED — verify." |
| Status update to PUBLISHED arrives without content_url                   | Phase 3B — content_url null for PUBLISHED update      | Reject the status update. Return: "PUBLISHED status requires content_url. Provide the URL of the published content." |
| Semantic similarity calculation produces an error                        | Phase 2B — calculation fails                           | Flag the keyword as REVIEW-NEEDED. Store it. Do not block. Note: "Semantic check failed — manual dedup review needed." |
| Cluster coverage record update fails after status change                 | Step 5 Phase 5A — write fails                         | Retry once. If retry fails → store the status change but flag cluster record as STALE-COVERAGE. Alert: "Cluster coverage for [cluster_name] may be stale — manual recalculation recommended." |
| More than 50% of incoming keywords are rejected as duplicates            | Phase 2D — high rejection rate detected                | Flag: "HIGH DUPLICATION RATE: [N%] of incoming keywords already exist in memory. Either the keyword discovery is targeting a mature, well-covered space, or the discovery parameters should be expanded." |
| A keyword is submitted for PUBLISHED update with a URL that is already used by another keyword record | Phase 3B — content_url collision detected | Flag: "URL collision: [URL] is already associated with keyword_id [UUID] ('[existing_keyword]'). A single URL cannot be the canonical URL for two different keywords. Resolve before setting PUBLISHED status." |
| The stale threshold for a keyword is set to 0 or negative              | Phase 6B — invalid threshold                          | Override to minimum threshold of 30 days. Log: "Stale threshold corrected to minimum 30 days for keyword_id [UUID]." |
| Keyword record is requested but keyword_id does not exist                | Phase 7A — ID not found                               | Return: "Keyword record not found for keyword_id [UUID]. It may have been merged or deleted. Query by keyword string instead." |
| A REVIEW-NEEDED keyword is never reviewed after 5 sessions               | Phase 6B — review-needed age check                    | Escalate to STALE regardless of stale threshold. Flag: "REVIEW-NEEDED keyword '[keyword]' has not been reviewed in [N] sessions — auto-escalated to STALE." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — STATUS LIFECYCLE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
STATUS LIFECYCLE QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
UNUSED
  → Set by: keyword-memory.skill at creation (default)
  → Meaning: Keyword discovered; not yet in any pipeline
  → Can transition to: ASSIGNED / DEPRIORITIZED / STALE / REVIEW-NEEDED
  → Stale threshold: 90 days (adjustable per keyword)

ASSIGNED
  → Set by: roadmap-engine.skill
  → Requires: assigned_roadmap_item field
  → Meaning: Keyword has a roadmap item scheduled
  → Can transition to: IN-PROGRESS / UNUSED (if removed from roadmap) / DEPRIORITIZED

IN-PROGRESS
  → Set by: content-generation-engine.skill
  → Meaning: Content for this keyword is being written
  → Can transition to: PUBLISHED / ASSIGNED (if paused)

PUBLISHED
  → Set by: content-memory.skill (via keyword-memory)
  → Requires: content_url + content_title
  → Meaning: Content is live; terminal status
  → Cannot transition to any other status

DEPRIORITIZED
  → Set by: roadmap-engine.skill or user decision
  → Meaning: Keyword explicitly removed from consideration
  → Can transition to: UNUSED (if reinstated)
  → No stale threshold applies

REVIEW-NEEDED
  → Set by: opportunity-engine.skill or dedup gate
  → Meaning: Keyword needs human review before actioning
  → Can transition to: UNUSED (after review) / DEPRIORITIZED
  → Auto-escalates to STALE after 5 unreviewed sessions

STALE
  → Set by: stale detection (auto)
  → Meaning: Keyword has been UNUSED beyond threshold
  → Can transition to: UNUSED (if re-discovered / refreshed) / DEPRIORITIZED
  → Surfaces in stale alert at session start

TERMINAL STATUS: PUBLISHED
  Once PUBLISHED, a keyword's status never changes.
  A new version of the content (refresh) creates a new session update
  but the keyword_id remains at PUBLISHED with the URL updated.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — DEDUPLICATION DECISION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
DEDUPLICATION DECISION QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 1 — EXACT MATCH:
  keyword_normalized matches exactly → REJECT (unless STALE → REFRESH)

STEP 2 — SEMANTIC MATCH (same cluster):
  Similarity ≥ 85% → REJECT or MERGE (merge if one is more specific)
  Similarity 70–84% → FLAG as REVIEW-NEEDED; ALLOW storage with note
  Similarity < 70% → ALLOW storage

STEP 3 — INTENT-CLUSTER MATCH:
  Same cluster + same funnel stage + similarity ≥ 85% → REJECT as INTENT DUPLICATE
  Same cluster + same funnel stage + similarity 70–84% → FLAG; ALLOW with note
  Same cluster + same funnel stage + similarity < 70% → ALLOW (different enough)
  Same cluster + DIFFERENT funnel stage → ALWAYS ALLOW
  DIFFERENT cluster → ALWAYS ALLOW (even if high similarity — different territory)

MERGE PROTOCOL:
  Use when: near-duplicates where one is more specific than other
  Keep: higher opportunity score / more specific / more recent
  Absorb: lower score / more generic / older
  Mark absorbed: DEPRIORITIZED + note "Merged into [canonical_id]"
  Log: dedup_merged_from list on canonical record

ALLOW WITH FLAG = REVIEW-NEEDED:
  Keyword is stored but flagged for human review
  Not blocked from roadmap — can proceed with caution
  Should be resolved within 2 sessions

REFRESH (STALE → UNUSED):
  Applied when exact match exists with STALE status
  Status updated to UNUSED; last_updated refreshed
  Keyword is available for re-use
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — KEYWORD RECORD EXAMPLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
KEYWORD RECORD EXAMPLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{
  "keyword_id"            : "c7d3e1f4-2a5b-4c8d-9e0f-1b2c3d4e5f6a",
  "keyword"               : "best crm software for small business",
  "keyword_normalized"    : "best crm software for small business",
  "project_id"            : "a4f7c2d1-...",
  "intent"                : "MOFU",
  "funnel_stage"          : "MOFU",
  "cluster"               : "CRM Software Selection",
  "cluster_id"            : "C-P01",
  "primary_keyword"       : false,
  "opportunity_score"     : 8,
  "d1_gap_level"          : 2,
  "d2_serp_weakness"      : 2,
  "d3_intent_value"       : 1,
  "d4_cluster_importance" : 2,
  "d5_differentiation"    : 1,
  "status"                : "ASSIGNED",
  "content_url"           : null,
  "content_title"         : null,
  "assigned_roadmap_item" : "RM-003",
  "session_discovered"    : "sess-2024-01-10-001",
  "session_last_updated"  : "sess-2024-02-15-007",
  "created_date"          : "2024-01-10T09:15:00Z",
  "last_updated"          : "2024-02-15T11:30:00Z",
  "stale_threshold_days"  : 75,
  "keyword_type"          : "PRIMARY",
  "serp_difficulty"       : "MEDIUM",
  "gap_type"              : "MISSING DEPTH",
  "source_skill"          : "keyword-discovery.skill",
  "dedup_merged_from"     : [],
  "notes"                 : "Assigned to Month 2 roadmap — MOFU comparison article targeting small business segment"
}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: memory/keyword-memory.skill*
*Nexus SEO Operating System — Version 1.0.0*
