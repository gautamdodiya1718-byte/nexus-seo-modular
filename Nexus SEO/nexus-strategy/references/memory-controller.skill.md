# SKILL: core/memory-controller.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Core / Memory Management
# EXECUTION PRIORITY: FOUNDATIONAL — Called by every skill before and after execution.
# POSITION: Middleware between all skills and all memory layers.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **central memory manager** of the Nexus system.

No skill in Nexus reads or writes memory directly. All memory access — whether reading context before execution, writing outputs after execution, updating existing records, or detecting conflicts — is routed exclusively through this controller. It is the single source of truth for all persistent state in the system.

### Primary Responsibilities

| Responsibility                    | Description                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------|
| **Context Identification**        | Extract and normalize project identity signals from any input source                                 |
| **Project Detection**             | Detect whether an existing project matches the current request before any execution begins           |
| **Project Initialization**        | Create structured, fully-formed project records for new projects                                     |
| **Memory Routing**                | Direct every read and write request to the correct specialized memory skill                          |
| **Read Operations**               | Fetch only the precise data slice a requesting skill needs — no full memory dumps                    |
| **Write Operations**              | Format, validate, and safely store all generated data without overwriting existing valid records     |
| **Conflict Detection**            | Identify collisions between new data and existing records at every level before any write occurs     |
| **Deduplication Gating**          | Block all duplicate entries at five distinct levels before they reach any memory layer               |
| **Memory Update & Timestamping**  | Maintain accurate timestamps and session usage tracking on every record                              |
| **Lifecycle Tracking**            | Track keywords, content, and strategies through their complete lifecycle states                      |
| **Staleness Detection**           | Identify aged records and recommend refresh over redundant new creation                              |
| **Cross-Session Continuity**      | Carry forward relevant project context across session boundaries without requiring user re-entry     |
| **Failsafe Memory Management**    | Handle unavailable memory, missing records, and user-skipped decisions safely                        |

### Middleware Architecture

```
ALL NEXUS SKILLS
       │
       ▼
┌──────────────────────────┐
│  memory-controller.skill │  ← This skill
│  (Central Memory Manager)│
└──────────────────────────┘
       │
       ├──→ project-memory.skill
       ├──→ keyword-memory.skill
       ├──→ content-memory.skill
       └──→ strategy-memory.skill
```

Every skill in Nexus interacts with exactly one layer of this stack — the controller. No skill speaks directly to a specialized memory skill.

### What This Skill Is NOT

- It is **not** a storage system. It routes to storage — it does not hold data itself.
- It is **not** a skill executor. It has no SEO logic. It manages state only.
- It is **not** optional. Memory must be checked before and updated after every execution.
- It is **not** a decision-maker for user content. When a conflict requires user input, it presents options and waits — it does not choose.
- It is **not** a full-dump retriever. It always fetches the minimum required data slice, never the full memory state.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill receives input from three distinct sources. Each source triggers a different operational mode.

### Source A — User Input (Direct or via task-router.skill)

Raw input passed from the user, often at session start or when a new project is initiated.

| Field              | Description                                                                | Required |
|--------------------|----------------------------------------------------------------------------|----------|
| `project_name`     | Name or label for the current project (may be inferred from domain)        | Optional |
| `domain`           | Website domain being worked on (e.g., `example.com`)                      | Optional |
| `keyword`          | Seed keyword or topic phrase provided by the user                          | Optional |
| `niche`            | Industry or niche descriptor if provided                                   | Optional |
| `session_id`       | System-generated session identifier from execution-mode.skill              | Required |
| `operation_type`   | What the user wants to do: `new_project`, `continue_project`, `query`     | Inferred |

**Input Normalization Rules:**

- Strip protocol prefixes from domains: `https://example.com` → `example.com`
- Lowercase all domain values before matching
- Trim whitespace from all string fields
- If `project_name` is not provided, attempt to derive it from `domain` (e.g., `example.com` → `Example`)
- If neither `project_name` nor `domain` is provided, mark both as `NULL` and proceed to project detection with keyword alone

---

### Source B — Skill Requests (from any executing Nexus skill)

Memory requests issued programmatically by other skills during their execution. These are structured request packets.

**Memory Request Packet Format:**

```
MEMORY REQUEST PACKET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Requesting Skill    : [skill-name.skill]
Session ID          : [ID]
Operation           : [READ / WRITE / UPDATE / DELETE / CHECK]
Memory Layer        : [project / keyword / content / strategy / ALL]
Record Type         : [specific record type within the layer]
Subject             : [keyword / URL / topic / cluster / article title]
Filters             : [lifecycle_state / date_range / tag / status]
Data Payload        : [for WRITE and UPDATE operations — structured data]
Urgency             : [BLOCKING / NON-BLOCKING]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Urgency Classification:**

| Urgency         | Meaning                                                                         |
|-----------------|---------------------------------------------------------------------------------|
| `BLOCKING`      | Requesting skill cannot proceed until memory response is received               |
| `NON-BLOCKING`  | Requesting skill can proceed; memory operation completes asynchronously         |

All READ operations before skill execution are `BLOCKING`.
All WRITE operations after skill completion are `NON-BLOCKING` unless the write result affects downstream skills.

---

### Source C — Generated Outputs (from completed skills)

Structured data produced by skills upon completion, sent to the memory controller for storage.

**Output Storage Packet Format:**

```
OUTPUT STORAGE PACKET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Source Skill        : [skill-name.skill]
Session ID          : [ID]
Completion Time     : [ISO 8601 timestamp]
Memory Layer        : [project / keyword / content / strategy]
Record Type         : [specific type]
Subject             : [keyword / URL / topic / article]
Lifecycle State     : [GENERATED / ASSIGNED / PUBLISHED / ARCHIVED]
Data               :
  [structured key-value pairs of output data]
Overwrite Policy    : [NEVER / IF_STALE / IF_CONFIRMED]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill produces one of four output types depending on the operation performed.

### Output Type A — Memory Context Response (READ)

Returned to the requesting skill after a successful read operation.

```
MEMORY CONTEXT RESPONSE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Responding To       : [requesting-skill.skill]
Session ID          : [ID]
Operation           : READ
Memory Layer        : [layer]
Record Type         : [type]
Subject             : [value]
Records Returned    : [count]
Data               :
  [structured data slice — only fields relevant to the requesting skill]
Freshness           : [FRESH / STALE — age in days if stale]
Dedup Status        : [VERIFIED — no duplicates in response set]
Response Timestamp  : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Type B — Write/Update Confirmation

Returned after a successful write or update operation.

```
MEMORY WRITE CONFIRMATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Source Skill        : [skill-name.skill]
Session ID          : [ID]
Operation           : [WRITE / UPDATE]
Memory Layer        : [layer]
Record Type         : [type]
Subject             : [value]
Action Taken        : [CREATED / UPDATED / MERGED / SKIPPED]
Timestamp           : [ISO 8601]
Dedup Check         : [PASSED — no duplicate created]
Record ID           : [unique record identifier]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Type C — Conflict Resolution Prompt

Displayed to the user when a write conflict is detected and cannot be resolved automatically.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Memory Conflict Detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Conflict Type       : [EXACT / SEMANTIC / INTENT / CLUSTER / PIPELINE]
Existing Record     : [description of existing entry]
  Created           : [timestamp]
  Layer             : [memory layer]
  Status            : [lifecycle state]
New Data            : [description of incoming data]
  Source Skill      : [skill-name.skill]

Options:
  1  Overwrite    — Replace existing record with new data
  2  Merge        — Combine existing and new data (safe default)
  3  Skip         — Keep existing record; discard new data

Enter choice (1 / 2 / 3):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Rules for Output Type C:**
- Display only when the conflict cannot be resolved automatically.
- Do NOT display for exact duplicates — those are silently blocked (see Deduplication Rules).
- Always wait for user response before proceeding.
- Default to option 2 (Merge) if the user does not respond within session timeout.

### Output Type D — Project Detection Prompt

Displayed to the user when an existing project matching the current input is found.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Existing Project Found
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project Name        : [name]
Domain              : [domain]
Niche               : [niche]
Created             : [date]
Last Active         : [date]
Sessions            : [count]
Completed Tasks     : [list of skill names completed]
Pending Tasks       : [list if any]

Continue this project? [YES / NO / Start New]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Rules for Output Type D:**
- Display only once per session, on first invocation.
- Do NOT re-display if user has already made a project selection this session.
- If user selects `YES` → load project context, proceed.
- If user selects `NO` → treat as new project, archive current project.
- If user selects `Start New` → create new project entry, do NOT archive existing.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — CONTEXT IDENTIFICATION

On every invocation, regardless of source, extract and normalize the core identity signals from the input.

**Extraction Targets:**

| Signal           | Source Options                                                       | Normalization Rule                                      |
|------------------|----------------------------------------------------------------------|---------------------------------------------------------|
| `project_name`   | User input, domain derivation, prior session record                 | Title-case, strip special characters                    |
| `domain`         | User input, URL parsing, prior session record                       | Lowercase, strip protocol and trailing slash            |
| `keyword`        | User input, skill request subject field                             | Lowercase, trim, deduplicate spaces                     |
| `niche`          | User input, inferred from domain/keyword, prior session record      | Lowercase, normalize synonyms (e.g., "ecom" → "ecommerce") |
| `session_id`     | execution-mode.skill packet, task-router.skill packet               | Use as-is; generate if missing                          |
| `operation_type` | Inferred from input structure and request context                   | Classify as: NEW_PROJECT / CONTINUE / READ / WRITE / UPDATE |

**Context Extraction Priority:**

When the same signal is available from multiple sources, use this priority order:

1. Explicit user input (highest trust)
2. Existing session context (already verified this session)
3. Prior session memory record (requires freshness check)
4. Inferred from other available signals (lowest trust — flag as INFERRED)

**Output of Step 1 — Normalized Context Record:**

```
CONTEXT IDENTIFICATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_name        : [value / NULL / INFERRED: value]
domain              : [value / NULL]
keyword             : [value / NULL]
niche               : [value / NULL / INFERRED: value]
session_id          : [value / GENERATED]
operation_type      : [NEW_PROJECT / CONTINUE / READ / WRITE / UPDATE]
Extraction Source   : [USER / SESSION / MEMORY / INFERRED]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — EXISTING PROJECT DETECTION

Using the normalized context from Step 1, query `project-memory.skill` to determine whether a matching project already exists.

**Detection Query Logic:**

1. Query `project-memory.skill` with:
   - `domain` (primary match key)
   - `project_name` (secondary match key)
   - `keyword` (tertiary match key — used only if domain and name are both NULL)

2. Match conditions:

| Match Type              | Condition                                                           | Result              |
|-------------------------|---------------------------------------------------------------------|---------------------|
| **Exact match**         | Domain OR project_name matches an existing record exactly           | PROJECT FOUND       |
| **Fuzzy match**         | Project name differs by ≤2 characters OR domain shares root domain  | POSSIBLE MATCH      |
| **No match**            | No existing record satisfies any match condition                    | NO PROJECT          |

3. Handle Possible Match (fuzzy) separately:

   If a fuzzy match is detected but not an exact match, display:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Possible Project Match
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
You entered         : [user input]
Closest match found : [existing project name / domain]

Is this the same project? [YES / NO]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

4. Based on detection result:

| Result           | Next Action                                                               |
|------------------|---------------------------------------------------------------------------|
| PROJECT FOUND    | Display Output Type D (Project Detection Prompt). Wait for user response. |
| POSSIBLE MATCH   | Display fuzzy match prompt. Wait for user response.                       |
| NO PROJECT       | Proceed directly to Step 3 (Project Initialization).                      |

5. If user selects `YES` (continue existing project):
   - Load full project context from `project-memory.skill`.
   - Set `operation_type` = `CONTINUE`.
   - Pre-populate session context with all existing project data.
   - Notify `execution-mode.skill` that session context is loaded.
   - Skip Step 3.

6. If user selects `NO` or `Start New`:
   - Do NOT load existing project context.
   - Set `operation_type` = `NEW_PROJECT`.
   - If `NO`: archive the detected project in `project-memory.skill` (status → ARCHIVED).
   - If `Start New`: keep existing project as-is, create a new parallel project entry.
   - Proceed to Step 3.

---

### ▸ STEP 3 — PROJECT INITIALIZATION

Executed only when `operation_type` = `NEW_PROJECT`.

**New Project Record Structure:**

```
NEW PROJECT RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
record_id           : [UUID — auto-generated]
project_name        : [value]
domain              : [value / NULL]
niche               : [value / NULL]
target_audience     : [value / NULL — if provided by user]
seed_keyword        : [value / NULL — if provided]
status              : ACTIVE
created_at          : [ISO 8601 timestamp]
last_active         : [ISO 8601 timestamp]
session_count       : 1
sessions            : [[session_id]]
completed_skills    : []
pending_skills      : []
keyword_count       : 0
content_count       : 0
strategy_count      : 0
notes               : []
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Initialization Actions:**

1. Generate a unique `record_id` (UUID format).
2. Populate all available fields from the normalized context record (Step 1 output).
3. Set all lifecycle counters to zero.
4. Send the completed record to `project-memory.skill` for storage.
5. Receive write confirmation from `project-memory.skill`.
6. Update the current session's context with the new project record.
7. Log initialization to the session event log.

**What to do with NULL fields:**

- Do NOT ask the user to fill in NULL fields at initialization time.
- Mark them as NULL and allow downstream skills to populate them as they execute.
- Exception: if `project_name`, `domain`, AND `keyword` are all NULL simultaneously → ask user for at least one identifier before creating the project record.

---

### ▸ STEP 4 — MEMORY ROUTING

Every read and write request must be routed to the correct specialized memory skill. This step determines routing before any operation executes.

**Memory Layer Routing Table:**

| Data Type                                      | Memory Layer              | Target Skill              |
|------------------------------------------------|---------------------------|---------------------------|
| Project metadata, domain, niche, status        | Project layer             | `project-memory.skill`    |
| Keywords, seed terms, keyword clusters         | Keyword layer             | `keyword-memory.skill`    |
| Keyword lifecycle states                       | Keyword layer             | `keyword-memory.skill`    |
| Keyword opportunity scores                     | Keyword layer             | `keyword-memory.skill`    |
| Article titles, blueprints, drafts             | Content layer             | `content-memory.skill`    |
| Published content records                      | Content layer             | `content-memory.skill`    |
| Content lifecycle states                       | Content layer             | `content-memory.skill`    |
| Content performance data                       | Content layer             | `content-memory.skill`    |
| SEO strategies, roadmaps, priority lists       | Strategy layer            | `strategy-memory.skill`   |
| SERP analysis results                          | Strategy layer            | `strategy-memory.skill`   |
| Gap analysis results                           | Strategy layer            | `strategy-memory.skill`   |
| Authority mapping data                         | Strategy layer            | `strategy-memory.skill`   |
| Cross-layer queries (e.g., full project state) | ALL layers                | All four memory skills     |

**Routing Resolution Logic:**

1. Inspect the `record_type` and `data_payload` fields of the incoming request.
2. Match to the routing table above.
3. If a request spans multiple layers (e.g., "get everything for this project"):
   - Split into sub-requests, one per layer.
   - Execute all sub-requests.
   - Merge results before returning to requesting skill.
4. If the layer cannot be determined from the request:
   - Default to querying `project-memory.skill` first.
   - If not found there, cascade to keyword, content, strategy in that order.
   - Return the first match found.

**Routing Output:**

```
ROUTING DECISION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Request From        : [skill-name.skill]
Record Type         : [type]
Routed To           : [memory-skill.skill]
Sub-Requests        : [count — if multi-layer]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 5 — READ OPERATION

When a skill requests existing data from memory before or during execution.

**Read Protocol:**

1. Receive the memory request packet (Source B format).
2. Validate the request:
   - Session ID is present and active.
   - Requesting skill is a valid Nexus skill.
   - `Operation` = `READ`.
3. Apply routing decision from Step 4.
4. Query the target memory skill with:
   - Subject identifier (`keyword`, `domain`, `article_title`, etc.)
   - Record type filter
   - Lifecycle state filter (if specified)
   - Date range filter (if specified)
5. Receive raw records from memory skill.
6. **Data Slicing** — Extract only the fields relevant to the requesting skill:

**Field Relevance Map (examples):**

| Requesting Skill                | Fields Returned                                                           |
|---------------------------------|---------------------------------------------------------------------------|
| `keyword-discovery.skill`       | existing_keywords[], cluster_labels[], keyword_lifecycle_states[]         |
| `serp-analysis.skill`           | previously_analyzed_serps[], serp_timestamps[], competitor_snapshots[]    |
| `content-generation-engine.skill`| article_blueprints[], assigned_keywords[], content_guidelines[]          |
| `roadmap-engine.skill`          | scored_opportunities[], published_content[], pending_content[]            |
| `gap-opportunity-engine.skill`  | known_topic_clusters[], competitor_urls[], existing_content_map[]         |
| `task-router.skill`             | session_context[], completed_skills[], last_intent[]                      |
| `execution-mode.skill`          | completed_skills[], session_id, execution_logs[]                          |

7. Assemble the data slice for the requesting skill.
8. Check freshness of returned records:

**Freshness Thresholds by Record Type:**

| Record Type                | Fresh Threshold  | Stale Action                                        |
|----------------------------|------------------|-----------------------------------------------------|
| SERP analysis results      | 7 days           | Flag as STALE. Recommend re-run.                    |
| Keyword opportunity scores | 14 days          | Flag as STALE. Recommend refresh.                   |
| Competitor snapshots       | 7 days           | Flag as STALE. Recommend re-run.                    |
| Content drafts             | 90 days          | Flag as STALE. Recommend review.                    |
| Project metadata           | No expiry        | Always FRESH.                                       |
| Authority mapping data     | 30 days          | Flag as STALE. Recommend refresh.                   |
| Gap analysis results       | 14 days          | Flag as STALE. Recommend re-run.                    |

9. Return the data slice as Output Type A (Memory Context Response).

**Hard Rule:** Never return more data than the requesting skill needs. No full-memory dumps. No speculative data inclusion.

---

### ▸ STEP 6 — WRITE OPERATION

When a skill sends generated output to be stored after execution.

**Write Protocol:**

1. Receive the output storage packet (Source C format).
2. Validate the packet:
   - Source skill is a valid Nexus skill.
   - Session ID is present.
   - Data payload is not empty.
   - Record type is specified.
3. Run **Deduplication Gate** (Step 8) before any write. If gate blocks the write → stop here.
4. Run **Conflict Detection** (Step 7) before any write. If conflict found → pause and resolve.
5. Apply routing decision from Step 4.
6. Format the data payload for the target memory layer:

**Per-Layer Formatting Rules:**

| Target Layer          | Formatting Requirements                                                   |
|-----------------------|---------------------------------------------------------------------------|
| `project-memory.skill`| Add `last_active` timestamp. Increment relevant counters.                 |
| `keyword-memory.skill`| Assign `lifecycle_state`. Add `session_id` to usage log. Set `created_at`.|
| `content-memory.skill`| Assign `lifecycle_state`. Link to `keyword_id`. Set `created_at`.        |
| `strategy-memory.skill`| Tag with `source_skill`. Link to `project_id`. Set freshness timestamp. |

7. Send formatted record to the target memory skill.
8. Receive write confirmation.
9. Return Output Type B (Write Confirmation) to the source skill.
10. Update the project record in `project-memory.skill`:
    - Increment the relevant counter (`keyword_count`, `content_count`, `strategy_count`).
    - Update `last_active` timestamp.
    - Add new record ID to the appropriate list.

**Overwrite Policy Enforcement:**

| Incoming Overwrite Policy | Condition                                      | Action                              |
|---------------------------|------------------------------------------------|-------------------------------------|
| `NEVER`                   | Always                                         | Do not overwrite. Resolve conflict per Step 7 if collision detected. |
| `IF_STALE`                | Only if existing record is beyond freshness threshold | Overwrite silently. Log the overwrite. |
| `IF_CONFIRMED`            | Always                                         | Run conflict detection. Present to user for confirmation. |

---

### ▸ STEP 7 — CONFLICT DETECTION

Executed before every WRITE and UPDATE operation. Detects collisions between incoming data and existing memory records.

**Conflict Detection Protocol:**

1. Query the target memory layer for records matching the incoming subject.
2. If no matching records found → no conflict. Proceed with write.
3. If matching records found → classify the conflict type:

**Conflict Classification:**

| Conflict Type        | Detection Method                                                                 | Auto-Resolvable |
|----------------------|----------------------------------------------------------------------------------|-----------------|
| **EXACT**            | Incoming data is byte-for-byte identical to existing record                      | YES — block silently |
| **SEMANTIC**         | Incoming data conveys the same meaning as existing record (different wording)    | NO — user decision |
| **INTENT**           | Incoming data targets the same user intent as existing record                    | NO — user decision |
| **CLUSTER**          | Incoming keyword belongs to a cluster already covered by existing keyword records | NO — user decision |
| **PIPELINE**         | Incoming strategy/roadmap step duplicates a step already planned or executed     | NO — user decision |

4. For EXACT conflicts → silently block the write. Return confirmation that existing record was used instead. No user prompt needed.
5. For all other conflict types → display Output Type C (Conflict Resolution Prompt) and wait for user decision.

**User Decision Handling:**

| User Choice  | Action                                                                              |
|--------------|-------------------------------------------------------------------------------------|
| `1 — Overwrite` | Replace the existing record with the incoming data. Log the overwrite with timestamp and source skill. |
| `2 — Merge`     | Combine fields from both records. For list fields: union. For scalar fields: keep existing unless incoming is more recent. |
| `3 — Skip`      | Discard incoming data. Keep existing record unchanged. Return reference to existing record to source skill. |
| *(no response)* | Default to `2 — Merge`. Log the auto-resolution. Notify user in next output. |

**Merge Logic (for Option 2):**

- **List fields** (keywords, URLs, topics): Union merge — add all unique items from both records.
- **Scalar fields** (dates, scores, counts): Keep the more recent value for timestamps; keep the higher value for scores; sum for counts.
- **Text fields** (descriptions, titles, notes): Keep existing as primary. Append incoming as an alternative if materially different.
- **Lifecycle state fields**: Keep the more advanced state (e.g., PUBLISHED > DRAFT > GENERATED).

---

### ▸ STEP 8 — DEDUPLICATION GATE

Executed before every WRITE and UPDATE operation, immediately before Step 7. This is the first line of defense. If a duplicate is caught here, it is blocked before conflict detection even runs.

**Five-Level Deduplication Check:**

**Level 1 — Exact Duplicate Check**

- Compare incoming record's full data payload against all existing records in the target memory layer.
- Use hash comparison for speed.
- If identical hash found → block write. Return reference to existing record. Log.

**Level 2 — Semantic Duplicate Check**

- Apply semantic similarity analysis to text-heavy fields (titles, descriptions, topics).
- Similarity threshold: ≥85% cosine similarity = semantic duplicate.
- If semantic duplicate detected → classify as SEMANTIC conflict (Step 7). Do not auto-block.

**Level 3 — Intent Duplicate Check**

- For keyword and content records: check if the incoming record targets the same search intent as an existing record.
- Intent classification: INFORMATIONAL, COMMERCIAL, TRANSACTIONAL, NAVIGATIONAL.
- If same intent AND same topic category AND same target audience → classify as INTENT conflict (Step 7).

**Level 4 — Cluster Duplicate Check**

- For keyword records: check if the incoming keyword belongs to a cluster already represented in memory.
- Cluster membership is determined by:
  - Shared root topic
  - Shared parent keyword (seed keyword match)
  - Shared SERP overlap (≥50% of top-10 results match)
- If cluster duplicate detected → classify as CLUSTER conflict (Step 7).

**Level 5 — Pipeline Duplicate Check**

- For strategy and roadmap records: check if the incoming pipeline step duplicates a step already planned, in-progress, or completed.
- Match on: step type + target skill + primary subject.
- If pipeline duplicate detected → classify as PIPELINE conflict (Step 7).

**Deduplication Gate Output:**

```
DEDUPLICATION GATE RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Level 1 — Exact        : [PASSED / BLOCKED]
Level 2 — Semantic     : [PASSED / FLAGGED — similarity: X%]
Level 3 — Intent       : [PASSED / FLAGGED — intent type: X]
Level 4 — Cluster      : [PASSED / FLAGGED — cluster: X]
Level 5 — Pipeline     : [PASSED / FLAGGED — step: X]
Overall Gate Result    : [PASS — proceed to write / BLOCK — duplicate / CONFLICT — see Step 7]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Action by Gate Result:**

| Gate Result  | Action                                                                     |
|--------------|----------------------------------------------------------------------------|
| `PASS`       | Proceed to Step 7 (Conflict Detection).                                    |
| `BLOCK`      | Stop write. Return reference to existing record to source skill.           |
| `CONFLICT`   | Proceed to Step 7 with the specific conflict type flag pre-set.            |

---

### ▸ STEP 9 — MEMORY UPDATE AND TIMESTAMPING

Executed after every successful WRITE or UPDATE operation.

**Update Actions:**

1. **Update the written record:**
   - Set `last_modified` = current ISO 8601 timestamp.
   - Increment `version` counter (start at 1, increment on each update).
   - Append `session_id` to the record's `session_history` list.
   - Preserve all prior versions in a version history array (up to 5 prior versions).

2. **Update the project record in `project-memory.skill`:**
   - Update `last_active` = current timestamp.
   - Increment the relevant counter (`keyword_count`, `content_count`, `strategy_count`).
   - Add the new record's ID to the relevant list field in the project record.
   - Append to `sessions` if current session_id is not already present.

3. **Update the session event log in `memory-controller.skill`:**

```
SESSION EVENT LOG ENTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
session_id          : [ID]
event_type          : [WRITE / UPDATE / READ / BLOCK / CONFLICT]
memory_layer        : [layer]
record_type         : [type]
subject             : [value]
action_taken        : [CREATED / UPDATED / MERGED / BLOCKED / SKIPPED]
timestamp           : [ISO 8601]
source_skill        : [skill-name.skill]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

4. **Freshness reset:** Reset the freshness clock for any record that was successfully written or updated.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CROSS-SESSION AWARENESS

The memory controller maintains continuity across sessions for the same project without requiring the user to re-enter context.

**Cross-Session Protocol:**

1. On every new session start, query `project-memory.skill` with any available identity signal.
2. If a matching project is found, pre-load its context into the current session silently.
3. The pre-loaded context includes:
   - All completed skills and their outputs (by reference, not by full data dump)
   - Current keyword lifecycle states
   - Current content lifecycle states
   - Last executed strategy step
   - Any pending tasks that were not completed in the prior session

4. When `task-router.skill` or `execution-mode.skill` queries memory at session start, return this pre-loaded context directly — no re-query needed.

5. Cross-session data is read-only at session start. It becomes writable only after the user confirms project continuation (Step 2).

---

### ▸ KEYWORD LIFECYCLE TRACKING

Every keyword in `keyword-memory.skill` is tracked through a defined lifecycle. The memory controller is responsible for transitioning keywords between states based on signals from other skills.

**Keyword Lifecycle States:**

```
UNUSED
  │
  ▼
DISCOVERED      ← keyword-discovery.skill writes this state
  │
  ▼
CLUSTERED       ← keyword-clustering.skill writes this state
  │
  ▼
ANALYZED        ← serp-analysis.skill writes this state
  │
  ▼
ASSIGNED        ← roadmap-engine.skill writes this state
  │
  ▼
IN_PROGRESS     ← article-blueprint.skill or content-generation-engine.skill writes this state
  │
  ▼
PUBLISHED       ← user or output-formatter.skill writes this state
  │
  ▼
ARCHIVED        ← memory-controller.skill writes this on staleness detection
```

**Lifecycle Transition Rules:**

- A keyword can only move forward in the lifecycle (no regression without explicit user command).
- If a skill attempts to assign a keyword that is already in state `IN_PROGRESS` or `PUBLISHED` → flag as PIPELINE conflict (Step 7).
- If a keyword has been in `DISCOVERED` state for more than 30 days without advancing → flag as STALE in read responses.
- If a keyword has been in `ASSIGNED` state for more than 14 days without advancing → notify user: "Keyword [X] has been assigned but not yet written."

---

### ▸ CONTENT LIFECYCLE TRACKING

Every content record in `content-memory.skill` is tracked through a parallel lifecycle.

**Content Lifecycle States:**

```
BLUEPRINT_CREATED   ← article-blueprint.skill
  │
  ▼
DRAFT_GENERATED     ← content-generation-engine.skill
  │
  ▼
VALIDATED           ← content-validator.skill
  │
  ▼
FORMATTED           ← output-formatter.skill
  │
  ▼
PUBLISHED           ← user action / external signal
  │
  ▼
ARCHIVED            ← memory-controller.skill on staleness / user command
```

**Content Lifecycle Rules:**

- A blueprint cannot be created for a keyword that already has content in `DRAFT_GENERATED` or later state, unless the user explicitly requests a new draft.
- A new DRAFT cannot overwrite a VALIDATED or later record without explicit user confirmation.
- PUBLISHED records are read-only — they can only transition to ARCHIVED, never backward.

---

### ▸ STALENESS DETECTION AND REFRESH SUGGESTIONS

The memory controller proactively detects stale records and surfaces refresh suggestions rather than allowing skills to operate on outdated data.

**Staleness Detection Protocol:**

1. On every READ operation, check the `last_modified` timestamp of every returned record.
2. Compare against the freshness threshold for that record type (defined in Step 5).
3. If a record is stale, append a staleness flag to the data slice:

```
STALENESS FLAG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Record              : [subject]
Last Modified       : [timestamp]
Age                 : [X days]
Threshold           : [Y days]
Staleness Status    : STALE
Recommendation      : Re-run [source-skill.skill] for fresh data
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

4. Do NOT block execution on stale data unless the requesting skill has `require_fresh = TRUE` in its request.
5. Present staleness warnings to the user at the start of the relevant skill's execution — not as a blocker, but as an advisory.
6. If a skill generates new data for a stale record → automatically update the record and reset the freshness clock.

**Suggest Refresh Instead of New Creation:**

When a requesting skill attempts to create a new record for a subject that already has a stale record:
- Do NOT silently create a duplicate.
- Surface the existing stale record with a refresh suggestion:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Existing Record Found (Stale)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Subject             : [subject]
Last Analyzed       : [date — X days ago]
Status              : STALE

Options:
  1  Refresh — Re-run analysis to update existing record
  2  Create New — Add a fresh record alongside the stale one
  3  Use Stale — Continue with existing data as-is

Enter choice:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ MEMORY HEALTH MONITORING

The memory controller maintains an ongoing health score for the memory system.

**Health Metrics Tracked:**

| Metric                        | Description                                                  | Warning Threshold    |
|-------------------------------|--------------------------------------------------------------|----------------------|
| `duplicate_rate`              | % of write attempts blocked as duplicates                    | >20% = investigate   |
| `stale_record_rate`           | % of records beyond freshness threshold                      | >40% = mass refresh  |
| `conflict_rate`               | % of writes triggering conflict detection                    | >15% = review        |
| `orphaned_records`            | Records with no project association                          | Any = clean up       |
| `unresolved_conflicts`        | Conflicts awaiting user decision for >24 hours               | Any = notify user    |
| `lifecycle_stuck_rate`        | Keywords/content stuck in same lifecycle state for >30 days  | >10% = notify user   |

Health monitoring runs passively on every WRITE operation and generates a health summary at session close.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Deduplication in `memory-controller.skill` is enforced at five levels (implemented in Step 8) and at two additional system-wide levels described here.

### System-Wide Rule 1 — Reference Over Storage

When a duplicate is detected at any level, the correct action is to **reference the existing record** — not to create a new one and not to discard the reference. Every skill that receives a "duplicate blocked" response must also receive the record ID and location of the existing record it can use instead.

### System-Wide Rule 2 — Deduplication Before Every Write Without Exception

The deduplication gate (Step 8) is non-bypassable. No skill may include a `bypass_dedup = TRUE` flag in its write request. There are no exceptions to this rule.

### System-Wide Rule 3 — Deduplication Applies to Partial Data

If a write request contains five fields and three of them are duplicates of an existing record, the deduplication gate must still process all five — it blocks at the field level, not only the record level. Fields that are not duplicates are merged into the existing record (Merge behavior from Step 7).

### System-Wide Rule 4 — Semantic Deduplication Is Not Optional

Semantic duplicate checking (Level 2) must always run, even if it requires additional processing time. Allowing semantic duplicates to enter memory is categorically worse than the overhead of checking them.

### System-Wide Rule 5 — Cross-Layer Deduplication

Deduplication is not siloed by memory layer. A keyword stored in `keyword-memory.skill` cannot be duplicated as a topic in `strategy-memory.skill` under a different label. The controller must perform cross-layer checks for records that could reasonably exist across multiple layers.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Memory Behaviors

| Prohibited Behavior                                                              | Reason                                                              |
|----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Writing a new record when an existing valid record covers the same subject        | Creates memory bloat. Duplicates degrade all downstream read quality.|
| Returning full memory dumps to requesting skills                                  | Wastes processing. Creates noise. Violates data minimalism rule.   |
| Overwriting an existing record without first running conflict detection           | Data integrity violation. Can destroy valid prior outputs.          |
| Allowing skills to query memory layers directly (bypassing the controller)        | Architecture violation. All memory access must route through here. |
| Creating vague memory entries without a defined record type and subject           | Produces unqueryable records. Pollutes memory layers.               |
| Storing intermediate computation results as permanent records                     | Memory layers store only finalized, validated outputs.              |
| Prompting the user about every write operation                                    | Only conflict-requiring decisions surface to the user.              |
| Treating memory unavailability as a fatal error                                   | Execution continues. Memory is marked for deferred write.          |
| Running deduplication checks selectively                                          | Deduplication is mandatory for every write. No exceptions.         |
| Surfacing technical memory errors to the user in raw format                       | All errors are translated into user-readable advisory messages.    |

### Required Memory Behaviors

- Every record written to memory must have: `record_id`, `subject`, `record_type`, `lifecycle_state`, `created_at`, `last_modified`, `session_id`, `source_skill`.
- Every read response must include a `freshness` status for every returned record.
- Every blocked write must return a reference to the existing record that caused the block.
- Every conflict resolution must be logged with the user's decision and timestamp.
- Every session must end with an updated project record reflecting all writes during that session.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ task-router.skill

| Property        | Detail                                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| **Direction**   | Bidirectional                                                                       |
| **Router sends**| Session ID, primary subject, detected intent — for pre-routing memory check        |
| **Controller returns** | Prior session context, completed skills list, duplicate alerts             |
| **Timing**      | Called at Step 1 and Step 4 of task-router.skill                                   |

---

### ▸ execution-mode.skill

| Property        | Detail                                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| **Direction**   | Bidirectional                                                                       |
| **Execution sends** | Session ID, skill name, subject — for pre-execution memory state query         |
| **Controller returns** | Completed skills, cached outputs, freshness flags, dedup alerts            |
| **Timing**      | Called at Steps 1, 3, and 10 of execution-mode.skill                               |

---

### ▸ project-memory.skill

| Property        | Detail                                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| **Direction**   | Controller → project-memory (write), project-memory → controller (read)            |
| **Stores**      | Project records, project metadata, session logs, completion tracking               |
| **Called at**   | Step 2 (detection), Step 3 (initialization), Step 9 (post-write update)           |

---

### ▸ keyword-memory.skill

| Property        | Detail                                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| **Direction**   | Controller → keyword-memory (write), keyword-memory → controller (read)            |
| **Stores**      | Keywords, clusters, lifecycle states, opportunity scores, session usage logs       |
| **Called at**   | Step 4 (routing), Step 5 (read), Step 6 (write), Step 9 (update)                  |

---

### ▸ content-memory.skill

| Property        | Detail                                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| **Direction**   | Controller → content-memory (write), content-memory → controller (read)            |
| **Stores**      | Article blueprints, drafts, validated content, published records, content lifecycle |
| **Called at**   | Step 4 (routing), Step 5 (read), Step 6 (write), Step 9 (update)                  |

---

### ▸ strategy-memory.skill

| Property        | Detail                                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| **Direction**   | Controller → strategy-memory (write), strategy-memory → controller (read)          |
| **Stores**      | Roadmaps, SERP snapshots, gap analyses, authority profiles, opportunity reports    |
| **Called at**   | Step 4 (routing), Step 5 (read), Step 6 (write), Step 9 (update)                  |

---

### ▸ All Other Nexus Skills (keyword-discovery, serp-analysis, content-generation-engine, etc.)

| Property        | Detail                                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| **Direction**   | Any skill → controller (pre-execution READ request and post-execution WRITE request)|
| **Pre-execution** | Each skill sends a READ request to check what's already known about its subject  |
| **Post-execution** | Each skill sends a WRITE request with its output data for storage              |
| **Rule**        | No Nexus skill may read from or write to any memory layer directly               |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Failsafe Scenarios and Responses

| Failure Scenario                                       | Detection                                       | Recovery Action                                                                           |
|--------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------|
| `project-memory.skill` is unavailable                  | Write request returns error                     | Continue execution. Mark write as DEFERRED. Retry at session end. Warn user if deferred writes exceed 3. |
| `keyword-memory.skill` is unavailable                  | Write/read request returns error                | Continue execution. Cache data in session memory. Retry at session close.                 |
| `content-memory.skill` is unavailable                  | Write/read request returns error                | Continue execution. Cache output data in session memory. Retry at session close.          |
| `strategy-memory.skill` is unavailable                 | Write/read request returns error                | Continue execution. Cache output in session memory. Retry at session close.               |
| All memory skills unavailable                          | All requests return errors                      | Switch to session-only mode. Warn user: "Memory unavailable. Data will not persist beyond this session." |
| User does not respond to conflict prompt               | Timeout (session-defined threshold)             | Default to Merge (Option 2). Log auto-resolution. Notify user in next output.            |
| User does not respond to project detection prompt      | Timeout                                         | Default to continuing existing project. Log assumption. Notify user.                      |
| Deduplication check times out                          | Level 2 or 4 check exceeds threshold            | Skip the timed-out level. Log the skip. Proceed with remaining levels. Flag record for manual review. |
| Write operation partially completes (some layers fail) | Partial success response from memory skill      | Complete successful layers. Retry failed layers. Flag partial write to source skill.      |
| Session ID is missing from request                     | Step 1 validation fails on session ID           | Generate a temporary session ID. Log. Notify source skill to include session ID in future requests. |
| Record version conflict (two writes to same record simultaneously) | Version mismatch on update | Keep higher-version record. Merge non-conflicting fields. Notify source skills.          |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — MEMORY LAYER QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
LAYER 1 — project-memory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Stores : Project metadata, session history, task completion records
Keys   : project_id, domain, project_name
Access : All skills (via controller)
Expiry : No expiry

LAYER 2 — keyword-memory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Stores : Keywords, clusters, lifecycle states, scores
Keys   : keyword_id, keyword_text, cluster_id, project_id
Access : keyword-discovery, clustering, serp-analysis, roadmap, blueprint (via controller)
Expiry : Opportunity scores: 14 days | Lifecycle states: no expiry

LAYER 3 — content-memory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Stores : Blueprints, drafts, validated content, published records
Keys   : content_id, article_title, keyword_id, project_id
Access : blueprint, generation, validator, formatter (via controller)
Expiry : Drafts: 90 days | Published: no expiry

LAYER 4 — strategy-memory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Stores : SERP snapshots, gap analyses, roadmaps, authority data
Keys   : strategy_id, source_skill, project_id, subject
Access : serp-analysis, deep-serp, gap-engine, opportunity, authority, roadmap (via controller)
Expiry : SERP data: 7 days | Gap analysis: 14 days | Authority: 30 days | Roadmaps: no expiry
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — MANDATORY RECORD FIELDS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Every record written to any memory layer by this controller must include all of the following mandatory fields. Records missing any of these fields are rejected at the write gate.

```
MANDATORY FIELDS (ALL LAYERS)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
record_id           : UUID — auto-generated if not provided
project_id          : From current project record
session_id          : From current session
source_skill        : Name of the skill that generated this data
record_type         : Classification within the memory layer
subject             : Primary subject identifier (keyword, URL, topic, etc.)
lifecycle_state     : Current state in the appropriate lifecycle
created_at          : ISO 8601 timestamp — set at first write
last_modified       : ISO 8601 timestamp — updated on every modification
version             : Integer — starts at 1, increments on each update
freshness_reset_at  : ISO 8601 timestamp — reset on every write/update
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: core/memory-controller.skill*
*Nexus SEO Operating System — Version 1.0.0*
