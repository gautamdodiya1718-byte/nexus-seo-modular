# SKILL: core/execution-mode.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Core / Execution Controller
# EXECUTION PRIORITY: SECOND — Activates immediately after task-router.skill resolves intent.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **execution controller** of the Nexus system.

Where `task-router.skill` determines *what* to run, `execution-mode.skill` determines *how* to run it — with full precision over sequencing, dependency resolution, scope enforcement, and logic depth protection.

This skill sits between the router and every downstream skill. Nothing executes in Nexus without passing through this controller.

### Primary Responsibilities

| Responsibility                   | Description                                                                                    |
|----------------------------------|------------------------------------------------------------------------------------------------|
| **Execution Type Classification**| Classify every routed request into TYPE 1, TYPE 2, or TYPE 3                                  |
| **Dependency Resolution**        | Detect hidden and explicit upstream dependencies before building any execution plan            |
| **Execution Graph Construction** | Build a fully ordered, dependency-aware execution graph for every request                     |
| **Scope Enforcement**            | Ensure single-task requests stay single-task; prevent scope creep in all directions           |
| **Logic Depth Protection**       | Enforce full internal logic depth for every skill regardless of execution mode                |
| **Circular Execution Prevention**| Detect and break circular dependency chains before they reach any skill                       |
| **Redundancy Elimination**       | Skip skills already completed in-session unless freshness re-run is explicitly requested      |
| **Execution Plan Dispatch**      | Send structured, ordered execution plans to individual skills or `nexus-connector.skill`      |
| **Failsafe Handling**            | Detect broken chains, ambiguous scope, and missing dependencies — recover without data loss   |
| **Completion Signaling**         | Notify `task-router.skill` upon successful execution chain completion                         |

### What This Skill Is NOT

- It is **not** a router. It does not re-detect intent. Intent arrives pre-resolved from `task-router.skill`.
- It is **not** an executor. It does not process SEO data, generate content, or produce analysis.
- It is **not** a simplifier. It never reduces logic depth to fit output size constraints.
- It is **not** an optimizer of skill logic. It optimizes execution *sequence* only — never internal skill steps.
- It is **not** a gatekeeper of user choices. If the user selects a single task, it runs exactly one skill — fully.

### The Single Most Important Rule of This Skill

> **Execution depth is invariant.**
>
> The number of skills that run may change based on user request.
> The depth at which each skill executes must NEVER change.
>
> TYPE 1 runs one skill. TYPE 3 runs the full pipeline.
> In both cases, every skill that runs executes its complete internal logic — no exceptions.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill accepts **only structured execution instruction packets** from `task-router.skill`. It does not accept raw user input directly.

### Primary Input Format

```
NEXUS EXECUTION HANDOFF
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID from memory-controller]
Timestamp           : [ISO 8601 timestamp]
Detected Intent     : [intent label]
Confidence Level    : [HIGH / MEDIUM / LOW]
Primary Subject     : [keyword / URL / topic / domain]
Subject Type        : [keyword / URL / topic / vague]
Modifiers           : [list or NONE]
Scope               : [SINGLE / CHAINED / FULL-SYSTEM]
Target Skill        : [skill-name.skill]
Execution Queue     : [ordered list if CHAINED, else NULL]
Memory Check        : [PASSED / FLAGGED — see note]
Parameters:
  subject           : [value]
  scope             : [value]
  modifiers         : [value]
  constraints       : [any limitations provided by user]
Routing Source      : core/task-router.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Secondary Input — Memory State (from memory-controller.skill)

Before building any execution plan, this skill also receives a memory state snapshot:

```
MEMORY STATE SNAPSHOT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Completed Skills    : [list of skill names + subjects already executed]
Cached Outputs      : [list of available cached results by skill]
Freshness Flags     : [list of results older than threshold]
Duplicate Alerts    : [list of re-run risks detected by memory-controller]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Input Validation Rules

Before processing any input, the execution controller validates the following:

| Validation Check                          | Pass Condition                                              | Fail Action                                       |
|-------------------------------------------|-------------------------------------------------------------|---------------------------------------------------|
| Routing source is task-router.skill       | `Routing Source` field = `core/task-router.skill`          | Reject packet. Log error. Return to router.       |
| Session ID is present                     | Non-null session ID exists                                  | Request session ID from memory-controller.skill   |
| Intent is resolved (not NULL)             | `Detected Intent` field is non-empty                       | Reject. Return to task-router.skill with flag.    |
| Target skill is a valid Nexus skill       | Skill name exists in the Nexus skill registry              | Reject. Notify router of invalid target.          |
| Memory check status is PASSED or FLAGGED  | Field is not empty or corrupted                            | Query memory-controller.skill directly.           |
| Parameters are structurally complete      | Required parameters for target skill are present or NULL-flagged | Proceed with NULL-flagged params — target skill will prompt user |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill produces one of three output types depending on execution classification.

### Output Type A — Single Skill Execution Plan (TYPE 1)

```
NEXUS EXECUTION PLAN — TYPE 1 (SINGLE)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Execution Type      : TYPE 1 — Single Skill
Target Skill        : [skill-name.skill]
Primary Subject     : [value]
Execution Depth     : FULL (all internal logic steps active)
Dependencies Met    : [YES / NO — if NO, see dependency injection below]
Dependency Skills   : [NULL or list with SILENT/VISIBLE flag]
Scope Lock          : ENFORCED — no additional skills will trigger
Memory Check        : [PASSED / SKIP — reason]
Dispatch To         : [skill-name.skill]
Parameters:
  subject           : [value]
  modifiers         : [value]
  constraints       : [value]
Execution Start     : [timestamp]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Type B — Chained Skill Execution Plan (TYPE 2)

```
NEXUS EXECUTION PLAN — TYPE 2 (CHAINED)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Execution Type      : TYPE 2 — Partial Flow
Primary Subject     : [value]
Execution Depth     : FULL (all internal logic steps active for each skill)
Scope               : CHAINED — [N] skills in sequence

Execution Queue:
  [1] → [skill-name.skill]         | Status: PENDING | Depends on: NULL
  [2] → [skill-name.skill]         | Status: PENDING | Depends on: [1]
  [3] → [skill-name.skill]         | Status: PENDING | Depends on: [1],[2]
  ...

Scope Lock          : ENFORCED — no skills outside this queue will trigger
Memory Checks       : [PASSED / SKIP — reason per skill]
Dispatch Mode       : SEQUENTIAL — each skill waits for prior completion
Dispatch To         : execution-mode.skill (self-managed queue)
Execution Start     : [timestamp]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Type C — Full System Execution Plan (TYPE 3)

```
NEXUS EXECUTION PLAN — TYPE 3 (FULL SYSTEM)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Execution Type      : TYPE 3 — Full System Pipeline
Primary Subject     : [value]
Execution Depth     : FULL (all internal logic steps active for all skills)

Full Pipeline Queue:
  [1] → keyword-discovery.skill         | Status: PENDING | Depends on: NULL
  [2] → keyword-clustering.skill        | Status: PENDING | Depends on: [1]
  [3] → serp-analysis.skill             | Status: PENDING | Depends on: [1],[2]
  [4] → deep-serp-analysis.skill        | Status: PENDING | Depends on: [3]
  [5] → website-analysis.skill          | Status: PENDING | Depends on: NULL (parallel-safe)
  [6] → gap-opportunity-engine.skill    | Status: PENDING | Depends on: [1],[2],[5]
  [7] → opportunity-scoring.skill       | Status: PENDING | Depends on: [6]
  [8] → authority-mapping.skill         | Status: PENDING | Depends on: [5]
  [9] → roadmap-engine.skill            | Status: PENDING | Depends on: [2],[6],[7]
  [10]→ article-blueprint.skill         | Status: PENDING | Depends on: [3],[4],[9]
  [11]→ content-generation-engine.skill | Status: PENDING | Depends on: [10]
  [12]→ content-validator.skill         | Status: PENDING | Depends on: [11]
  [13]→ output-formatter.skill          | Status: PENDING | Depends on: [12]

Dispatch To         : nexus-connector.skill
Execution Start     : [timestamp]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — RECEIVE AND VALIDATE ROUTED INTENT

Accept the structured execution instruction packet from `task-router.skill`.

**Validation Sequence:**

1. Confirm `Routing Source` = `core/task-router.skill`. If not, reject the packet immediately and log an error.
2. Confirm `Session ID` is present. If missing, query `memory-controller.skill` to retrieve or generate.
3. Confirm `Detected Intent` is non-null and non-empty. If null, return packet to `task-router.skill` with flag: `INTENT_UNRESOLVED`.
4. Confirm `Target Skill` exists in the Nexus skill registry. If not found, return to `task-router.skill` with flag: `INVALID_SKILL_TARGET`.
5. Confirm `Memory Check` field. If `FLAGGED`, read the flag reason before proceeding to Step 2.
6. Log receipt of packet to `memory-controller.skill` with timestamp.

**Output of Step 1:**

```
INPUT VALIDATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Packet Origin       : VERIFIED — core/task-router.skill
Session ID          : [ID] — CONFIRMED
Intent              : [value] — RESOLVED
Target Skill        : [value] — VALID
Memory Flag         : [PASSED / FLAGGED: reason]
Validation Status   : PASS — proceed to Step 2
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If validation fails on any check, stop here and return control to `task-router.skill` with a structured error packet.

---

### ▸ STEP 2 — CLASSIFY EXECUTION TYPE

Using the validated input packet, classify the request into exactly one of three execution types.

**Classification Logic:**

| Condition                                                                 | Execution Type       |
|---------------------------------------------------------------------------|----------------------|
| `Scope` = SINGLE AND `Execution Queue` = NULL                            | TYPE 1 — Single Skill|
| `Scope` = CHAINED AND `Execution Queue` contains 2–10 skills             | TYPE 2 — Partial Flow|
| `Scope` = FULL-SYSTEM OR `Target Skill` = nexus-connector.skill          | TYPE 3 — Full System |
| `Scope` = CHAINED AND `Execution Queue` contains >10 skills              | TYPE 3 — promote to Full System; notify user |

**Edge Case Handling:**

| Edge Case                                                                 | Resolution                                                        |
|---------------------------------------------------------------------------|-------------------------------------------------------------------|
| Scope = SINGLE but target skill has unmet mandatory upstream dependencies | Reclassify to TYPE 2. Add required upstream skills silently.      |
| Scope = CHAINED but only 1 skill in queue after deduplication             | Reclassify to TYPE 1. Notify no additional skills are needed.     |
| Scope = FULL-SYSTEM but memory shows 8+ skills already completed          | Flag for user: "Most pipeline steps complete. Resume from [skill]?"|
| Scope is NULL or UNCLEAR                                                  | Return to `task-router.skill` with flag: `SCOPE_UNRESOLVED`       |

**Output of Step 2 — Classification Record:**

```
EXECUTION TYPE CLASSIFICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Input Scope         : [SINGLE / CHAINED / FULL-SYSTEM]
Execution Type      : [TYPE 1 / TYPE 2 / TYPE 3]
Reclassification    : [YES — reason / NO]
Skills in Scope     : [count]
Proceed to Step 3   : YES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 3 — DEPENDENCY RESOLUTION

Before building the execution graph, resolve all dependencies for every skill in scope.

**Dependency Resolution Protocol:**

1. Load the **Nexus Dependency Registry** (defined in Advanced Logic, Section 5).
2. For each skill in scope, look up its `required_upstream` list.
3. For each required upstream skill:
   a. Check if it has already been completed this session via `memory-controller.skill`.
   b. If completed → mark as `SATISFIED (cached)`.
   c. If not completed → mark as `REQUIRED — add to execution graph`.
4. For each newly added dependency skill, recursively resolve *its* upstream dependencies as well.
5. Continue recursion until no new unresolved dependencies remain.
6. Flag circular dependency chains immediately (see Advanced Logic).

**Dependency Classification:**

| Dependency Type     | Definition                                                             | Action                             |
|---------------------|------------------------------------------------------------------------|------------------------------------|
| **Hard dependency** | Skill absolutely cannot run without upstream output                    | Must be included in execution graph|
| **Soft dependency** | Skill performs better with upstream output but can run without it      | Include if not already cached; skip if cached |
| **Optional input**  | Upstream output enriches skill but is not required for base execution  | Include only if user requested depth = FULL |
| **Satisfied**       | Upstream skill already completed this session with same subject        | Use cached output; do not re-run   |

**Dependency Resolution Output:**

```
DEPENDENCY RESOLUTION MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Skill        : [skill-name.skill]
  Required Upstream:
    [skill-name.skill]    → Status: REQUIRED   | Type: HARD
    [skill-name.skill]    → Status: SATISFIED  | Type: HARD | Cached from: [timestamp]
    [skill-name.skill]    → Status: REQUIRED   | Type: SOFT
  Recursive Dependencies of REQUIRED skills:
    [skill-name.skill]    → Status: REQUIRED   | Type: HARD
    [skill-name.skill]    → Status: SATISFIED  | Type: SOFT | Cached from: [timestamp]
Total Skills Added to Graph  : [N]
Total Satisfied (Cached)     : [N]
Circular Dependency Detected : [YES — chain / NO]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 4 — BUILD EXECUTION GRAPH

Using the resolved dependency map from Step 3, construct a fully ordered, dependency-aware execution graph.

**Graph Construction Rules:**

1. **Topological ordering**: Skills with no upstream dependencies execute first (depth = 0). Skills dependent on one or more upstream skills execute after all their dependencies are satisfied.
2. **No forward references**: A skill may never be scheduled before any of its hard dependencies.
3. **Parallel-safe identification**: Identify skills that share no dependencies with each other and could theoretically run in parallel. Mark them — but do NOT parallelize execution unless `nexus-connector.skill` explicitly enables it.
4. **Depth labeling**: Assign each skill a `depth_level` integer representing how many dependency layers precede it.
5. **Critical path identification**: Identify the longest sequential dependency chain — this is the critical path. It determines minimum total execution time.

**Graph Node Format (per skill):**

```
GRAPH NODE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Position            : [N]
Skill               : [skill-name.skill]
Depth Level         : [integer — 0 = no dependencies]
Depends On          : [list of position numbers, or NULL]
Parallel-Safe With  : [list of position numbers, or NULL]
Input Source        : [upstream skill output or user-provided]
Output Destination  : [downstream skill(s) or final output]
Memory Skip         : [YES — reason / NO]
Execution Depth     : FULL (invariant)
Status              : PENDING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Execution Graph Integrity Checks:**

Before finalizing the graph, run the following integrity checks:

| Check                                    | Pass Condition                                                    |
|------------------------------------------|-------------------------------------------------------------------|
| No circular paths exist                  | No skill appears as both upstream and downstream of itself        |
| No orphaned nodes exist                  | Every skill in the graph connects to at least one other node      |
| All hard dependencies are present        | No hard dependency is missing from the graph                      |
| All nodes have defined input sources     | No skill in the graph has a NULL input source for a hard dependency|
| All depth levels are correctly assigned  | Depth N+1 never precedes depth N in execution order              |

If any check fails → abort graph construction → rebuild from Step 3 → notify user of delay.

---

### ▸ STEP 5 — DEPENDENCY INJECTION (SILENT VS. VISIBLE)

When the execution controller adds upstream dependency skills that were not explicitly requested by the user, it must decide whether to inject them silently or visibly.

**Injection Mode Decision Rules:**

| Condition                                                                       | Injection Mode  |
|---------------------------------------------------------------------------------|-----------------|
| TYPE 1 request; dependency is mandatory (HARD); user unaware it's needed        | SILENT injection — add to graph without user notification |
| TYPE 1 request; dependency is soft or optional; could affect output meaningfully| VISIBLE injection — notify user, ask to confirm |
| TYPE 2 request; dependency not in original queue                                | VISIBLE injection — update the execution queue and confirm |
| TYPE 3 request; all dependencies are expected by design                         | SILENT injection — no notification needed |

**Visible Injection Notification Format:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Dependency Required
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
To run [target-skill.skill], the following skill must run first:
  → [dependency-skill.skill]

Reason: [one-line explanation of why this dependency is required]

This will be added to your execution plan automatically.
Continue? [YES / NO]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Silent Injection Rules:**

- Never inform the user of a silently injected dependency unless it fails.
- If a silently injected dependency fails, immediately surface it to the user with full context.
- Silent injection is only permitted for HARD dependencies where no user decision is needed.

---

### ▸ STEP 6 — SCOPE ENFORCEMENT (OVER-EXECUTION PREVENTION)

This step is a strict gate that prevents the execution controller from triggering any skill outside the explicitly defined execution scope.

**Scope Lock Enforcement:**

| Scope Type     | Scope Lock Rule                                                                          |
|----------------|------------------------------------------------------------------------------------------|
| TYPE 1         | Only one skill executes. No additional skills may be added after dependency injection is complete, unless they are HARD dependencies. |
| TYPE 2         | Only skills explicitly in the resolved execution queue may execute. No scope expansion. |
| TYPE 3         | Full pipeline executes. No external skills may be added beyond the standard pipeline.   |

**Over-Execution Detection Patterns:**

The following patterns must be detected and blocked:

| Pattern                                                                       | Block Action                                                       |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------|
| A downstream skill attempts to trigger a skill not in the current graph       | Reject the trigger. Log the attempt. Continue with current graph.  |
| A hard dependency resolution adds a skill that cascades into 3+ new additions | Pause. Present updated execution queue to user for confirmation.   |
| A soft dependency resolution would double the size of the execution graph     | Pause. Present soft dependencies as optional. Ask user.           |
| TYPE 1 request begins executing but the target skill internally requests another skill | The internally requested skill receives NULL from this controller if not in scope |

**Scope Lock Confirmation (logged at every execution):**

```
SCOPE LOCK STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Execution Type      : [TYPE 1 / 2 / 3]
Skills in Graph     : [N]
Scope Lock          : ENFORCED
Over-Execution Attempts Blocked : [count]
Scope Expansion Requests Blocked: [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 7 — EXECUTION DEPTH LOCK

Before dispatching any skill, enforce the execution depth invariant.

**Execution Depth Invariant:**

Every skill that executes in the Nexus system — regardless of whether it runs in TYPE 1, TYPE 2, or TYPE 3 mode — must execute its complete internal logic, including:

- All defined internal steps
- All validation checks
- All analysis passes
- All deduplication checks
- All output formatting rules
- All quality gates

**What is permitted to change across execution types:**

| Variable              | TYPE 1        | TYPE 2         | TYPE 3          |
|-----------------------|---------------|----------------|-----------------|
| Number of skills      | 1 (+ deps)    | N (defined)    | Full pipeline   |
| Input parameters      | User-provided | Chained        | Chained         |
| Output destinations   | User / memory | Next skill     | Next skill      |
| **Logic depth**       | **FULL**      | **FULL**       | **FULL**        |
| **Internal steps**    | **ALL**       | **ALL**        | **ALL**         |
| **Output quality**    | **MAXIMUM**   | **MAXIMUM**    | **MAXIMUM**     |

**Depth Lock Enforcement Mechanism:**

When dispatching each skill, the execution plan packet includes a mandatory field:

```
Execution Depth : FULL — ALL INTERNAL STEPS REQUIRED
Logic Compression : PROHIBITED
Output Truncation : PROHIBITED
Step Skipping     : PROHIBITED
```

If any skill returns a completion signal that includes a flag indicating it skipped internal steps, the execution controller must:

1. Reject the output.
2. Log the failure.
3. Re-dispatch the skill with a `FORCE_FULL_EXECUTION` flag.
4. Notify the user of the delay.

---

### ▸ STEP 8 — DISPATCH EXECUTION PLAN

Send the finalized execution plan to the appropriate dispatch target.

**Dispatch Routing:**

| Execution Type | Dispatch Target                          | Dispatch Method                     |
|----------------|------------------------------------------|-------------------------------------|
| TYPE 1         | Target skill directly                    | Single skill execution packet       |
| TYPE 2         | execution-mode.skill self-managed queue  | Sequential dispatch — one skill at a time |
| TYPE 3         | nexus-connector.skill                    | Full pipeline execution plan packet |

**TYPE 2 Self-Managed Queue Protocol:**

For chained execution, this skill does NOT hand off the entire queue at once. It manages the queue sequentially:

1. Dispatch skill at position [1].
2. Wait for completion signal from skill [1].
3. Extract output from skill [1] and inject it as input to skill [2].
4. Dispatch skill [2].
5. Repeat until queue is exhausted.
6. At each step, update `memory-controller.skill` with completion status.
7. If any skill in the queue fails → pause queue → notify user → wait for resolution before continuing.

**Single Skill Execution Packet (dispatched per skill in queue):**

```
NEXUS SKILL DISPATCH PACKET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Dispatch Timestamp  : [ISO 8601]
Target Skill        : [skill-name.skill]
Position in Queue   : [N of M]
Execution Type      : [TYPE 1 / 2 / 3]
Execution Depth     : FULL — ALL INTERNAL STEPS REQUIRED
Logic Compression   : PROHIBITED
Output Truncation   : PROHIBITED
Primary Subject     : [value]
Input From          : [upstream skill output reference or user-provided]
Modifiers           : [value]
Constraints         : [value]
Completion Signal To: core/execution-mode.skill
Memory Log To       : memory-controller.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 9 — INTER-SKILL OUTPUT INJECTION

When a skill completes in a TYPE 2 or TYPE 3 chain, its output must be correctly injected as the input for the next skill in the queue.

**Output Injection Protocol:**

1. Receive completion signal + output reference from completed skill.
2. Identify the next skill in the queue.
3. Look up what input fields the next skill requires (from Nexus Skill Input Registry).
4. Map completed skill's output fields to next skill's input fields.
5. If a required input field cannot be satisfied from upstream output → check if user can provide it → if not, mark skill as BLOCKED and notify user.
6. Inject mapped inputs into next skill's dispatch packet.
7. Dispatch next skill.

**Output-to-Input Field Mapping Table (Core Pipeline):**

| From Skill                     | Output Field              | To Skill                        | Input Field               |
|--------------------------------|---------------------------|---------------------------------|---------------------------|
| keyword-discovery.skill        | keyword_list              | keyword-clustering.skill        | raw_keyword_list          |
| keyword-discovery.skill        | primary_keyword           | serp-analysis.skill             | target_keyword            |
| keyword-clustering.skill       | topic_clusters            | roadmap-engine.skill            | topic_clusters            |
| keyword-clustering.skill       | cluster_labels            | gap-opportunity-engine.skill    | known_topic_clusters      |
| serp-analysis.skill            | serp_snapshot             | deep-serp-analysis.skill        | serp_data                 |
| serp-analysis.skill            | top_ranking_urls          | gap-opportunity-engine.skill    | competitor_urls           |
| deep-serp-analysis.skill       | ranking_signals           | article-blueprint.skill         | serp_signals              |
| website-analysis.skill         | site_profile              | gap-opportunity-engine.skill    | site_data                 |
| website-analysis.skill         | existing_content_map      | authority-mapping.skill         | site_content_reference    |
| gap-opportunity-engine.skill   | gap_list                  | opportunity-scoring.skill       | candidate_opportunities   |
| opportunity-scoring.skill      | scored_opportunities      | roadmap-engine.skill            | opportunity_scores        |
| authority-mapping.skill        | authority_profile         | roadmap-engine.skill            | authority_context         |
| roadmap-engine.skill           | content_roadmap           | article-blueprint.skill         | roadmap_context           |
| article-blueprint.skill        | article_blueprint         | content-generation-engine.skill | blueprint                 |
| content-generation-engine.skill| draft_content             | content-validator.skill         | content_to_validate       |
| content-validator.skill        | validated_content         | output-formatter.skill          | final_content             |

---

### ▸ STEP 10 — COMPLETION SIGNALING AND MEMORY LOGGING

Upon completion of all skills in the execution graph:

1. Send a **completion signal** to `task-router.skill` to trigger the post-task prompt.
2. Log all execution details to `memory-controller.skill`:

```
EXECUTION COMPLETION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Execution Type      : [TYPE 1 / 2 / 3]
Skills Executed     : [list with completion timestamps]
Skills Skipped      : [list with skip reasons]
Dependencies Injected: [list — SILENT / VISIBLE]
Scope Violations Blocked : [count]
Depth Lock Enforced : YES
Total Execution Time: [duration]
Output References   : [list of output locations per skill]
Status              : COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

3. Return control to `task-router.skill`.
4. Do NOT display anything to the user directly. Post-task communication is the router's responsibility.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ NEXUS DEPENDENCY REGISTRY

The definitive reference for all skill dependencies in the Nexus system. This registry is consulted during Step 3 of every execution.

```
NEXUS DEPENDENCY REGISTRY v1.0
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SKILL                             HARD DEPS                          SOFT DEPS
─────────────────────────────────────────────────────────────────────────────
keyword-discovery.skill           NULL                               NULL
keyword-clustering.skill          keyword-discovery.skill            NULL
serp-analysis.skill               keyword-discovery.skill            keyword-clustering.skill
deep-serp-analysis.skill          serp-analysis.skill                NULL
website-analysis.skill            NULL                               NULL
gap-opportunity-engine.skill      keyword-clustering.skill,          deep-serp-analysis.skill
                                  website-analysis.skill
opportunity-scoring.skill         gap-opportunity-engine.skill       keyword-clustering.skill
authority-mapping.skill           website-analysis.skill             NULL
roadmap-engine.skill              keyword-clustering.skill,          authority-mapping.skill
                                  opportunity-scoring.skill
article-blueprint.skill           serp-analysis.skill,               deep-serp-analysis.skill,
                                  roadmap-engine.skill               keyword-clustering.skill
content-generation-engine.skill   article-blueprint.skill            serp-analysis.skill
content-validator.skill           content-generation-engine.skill    NULL
output-formatter.skill            content-validator.skill            NULL
memory-controller.skill           NULL                               NULL
execution-mode.skill              task-router.skill                  memory-controller.skill
nexus-connector.skill             execution-mode.skill               ALL pipeline skills
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ CIRCULAR DEPENDENCY DETECTION

The execution controller must detect and break circular dependency chains before they reach any skill.

**Detection Algorithm:**

1. Represent all skills and dependencies as a directed graph.
2. Perform a depth-first search (DFS) traversal starting from the target skill.
3. Track the current DFS path as a stack.
4. If a skill is encountered that already exists on the current stack → circular dependency detected.
5. Record the full cycle path.
6. Break the cycle by identifying the weakest link:
   - If the cycle contains a SOFT dependency → remove it from the graph.
   - If the cycle contains only HARD dependencies → this is a system error. Log it. Halt execution. Alert user.

**Circular Dependency Alert Format:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Circular Dependency Detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cycle Path   : [skill-A] → [skill-B] → [skill-C] → [skill-A]
Weak Link    : [skill-B → skill-C] (SOFT dependency)
Resolution   : Removing soft dependency to break cycle.
Revised Graph: [updated execution order]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ COMPOUND COMMAND RECOGNITION

Users sometimes issue compound commands that contain implicit execution scope. The execution controller must recognize these patterns and classify them correctly.

**Compound Command Patterns:**

| User Command Pattern                                      | Resolved Execution Type | Notes                                           |
|-----------------------------------------------------------|-------------------------|-------------------------------------------------|
| `"keyword research and SERP analysis"`                    | TYPE 2                  | Two skills, one subject                        |
| `"find keywords, cluster them, and build a roadmap"`      | TYPE 2                  | Three skills with implied dependency order     |
| `"write a full article from scratch"`                     | TYPE 2 (deep chain)     | Implies keyword → serp → blueprint → generate  |
| `"audit my site and find what I'm missing"`               | TYPE 2                  | website-analysis → gap-opportunity-engine      |
| `"build me a complete content strategy"`                  | TYPE 3                  | Full system trigger                            |
| `"just do the keywords"`                                  | TYPE 1                  | Scope limiter detected ("just")                |
| `"only run keyword research"`                             | TYPE 1                  | Scope limiter detected ("only")                |
| `"quick SERP check"`                                      | TYPE 1                  | Scope limiter detected ("quick")               |
| `"do keyword research but don't write anything"`          | TYPE 2 (constrained)    | Negative constraint applied — generation blocked|

**Scope Limiter Detection:**

The following words, when detected in the user's command, enforce a scope ceiling:

| Limiter Word/Phrase           | Effect                                              |
|-------------------------------|-----------------------------------------------------|
| `"just"`, `"only"`, `"quick"` | Force TYPE 1 — no expansion permitted              |
| `"don't"`, `"not"`, `"skip"`  | Apply negative constraint to named skills          |
| `"everything"`, `"all"`       | Confirm TYPE 3 intent — trigger full-system gate   |
| `"first"`, `"start with"`     | Begin TYPE 2 chain at specified skill              |
| `"then"`, `"after that"`      | Extend TYPE 2 chain to next specified skill        |

---

### ▸ EXECUTION RESUMPTION LOGIC

When a TYPE 2 or TYPE 3 execution chain is interrupted (user stops it, skill fails, session ends), the execution controller must support resumption from the point of interruption.

**Resumption Protocol:**

1. Query `memory-controller.skill` for last completed skill in the chain.
2. Identify the next pending skill in the graph.
3. Verify that all inputs required by the next skill are available from completed upstream skills.
4. Present resumption options to user:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Resume Execution?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Previous session found. Progress:
  ✓ [skill-name.skill] — COMPLETE
  ✓ [skill-name.skill] — COMPLETE
  ○ [skill-name.skill] — PENDING (next)
  ○ [skill-name.skill] — PENDING
  ○ [skill-name.skill] — PENDING

Resume from [skill-name.skill]? [YES / Start Over / Select Step]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

5. If user selects `YES` → resume from next pending skill, using cached outputs from completed skills.
6. If user selects `Start Over` → clear relevant session data in `memory-controller.skill` → rebuild full execution graph.
7. If user selects `Select Step` → display numbered list of all skills in the graph → user picks a starting point.

---

### ▸ EXECUTION PERFORMANCE MONITORING

Track and log execution performance metrics for every run. These are stored in `memory-controller.skill` and used by `nexus-connector.skill` to optimize future execution plans.

**Metrics Tracked Per Skill Execution:**

| Metric                  | Description                                                |
|-------------------------|------------------------------------------------------------|
| `dispatch_timestamp`    | When the skill was dispatched                             |
| `completion_timestamp`  | When the skill returned a completion signal               |
| `execution_duration`    | Time delta between dispatch and completion                |
| `depth_lock_enforced`   | Whether full logic depth was enforced and honored         |
| `scope_violations`      | Number of over-execution attempts blocked                 |
| `dependencies_injected` | Count of SILENT + VISIBLE dependencies added              |
| `cache_hits`            | Number of upstream inputs served from cache               |
| `output_size`           | Approximate token count of skill output                   |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Deduplication in `execution-mode.skill` operates at three levels and is enforced before every dispatch.

### Level 1 — Pre-Dispatch Skill Deduplication

Before dispatching any skill in the execution graph:

1. Query `memory-controller.skill`: has this skill already been executed with the same `primary_subject` in the current session?
2. If YES and output is still valid (within freshness threshold):
   - Mark skill as `SKIPPED — CACHED`.
   - Use cached output as if the skill had just executed.
   - Do NOT re-dispatch the skill.
   - Do NOT notify the user unless the cached output is being used in a place where freshness matters.
3. If YES but output is stale (beyond freshness threshold):
   - Flag for user: "This skill ran [X minutes ago]. Use cached result or re-run?"
   - Wait for user confirmation.
4. If NO → dispatch normally.

### Level 2 — Within-Graph Deduplication

When building the execution graph:

1. No skill may appear more than once in a single execution graph.
2. If dependency resolution causes the same skill to be added twice (e.g., two different skills both depend on `keyword-discovery.skill`):
   - Add it once at the earliest required position.
   - Route both downstream skills to use its single output.
3. If a skill in a CHAINED queue appears as a dependency of another skill also in the queue:
   - Remove the duplicate instance.
   - Reorder to satisfy the dependency relationship.

### Level 3 — Cross-Type Deduplication

When execution type is reclassified (e.g., TYPE 1 → TYPE 2 due to dependency injection):

1. Do NOT run the originally requested skill AND its dependency as two separate runs of overlapping logic.
2. Build a clean merged graph where the dependency runs first and feeds into the requested skill.
3. There is no duplication of any logic at any point in the merged graph.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Execution Behaviors

| Prohibited Behavior                                                         | Reason                                                   |
|-----------------------------------------------------------------------------|----------------------------------------------------------|
| Adding skills to an execution graph "just in case they might be useful"     | Scope creep. Not permitted under any condition.          |
| Running a full pipeline because "it will produce better results"            | User did not request it. Scope lock violation.           |
| Simplifying skill internal logic because the execution chain is long        | Depth lock violation. Explicitly prohibited.             |
| Truncating skill output mid-execution to save tokens                        | Output integrity violation. Always continue until complete.|
| Assuming what the user wants from a vague input and executing on that assumption | Input interpretation is task-router.skill's role only.|
| Notifying the user of SILENT dependency injections                          | Defeats the purpose of silent injection.                 |
| Treating TYPE 2 as TYPE 3 because the queue is "almost full pipeline"       | Reclassification requires the defined threshold (>10 skills). |
| Running optional soft dependencies without user confirmation in TYPE 1 mode | Scope violation.                                         |
| Logging execution performance data to any system other than memory-controller.skill | Data routing integrity rule.                       |
| Presenting execution plans with marketing language or enthusiasm             | Execution plans are structured data. No prose additions. |

### Required Execution Behaviors

- Every execution plan must use the exact structured packet formats defined in Section 3.
- Every dispatch must include the `Execution Depth: FULL` field.
- Every completion must log to `memory-controller.skill` before signaling `task-router.skill`.
- Every scope violation attempt must be blocked and logged — not silently ignored.
- Every dependency injection must be recorded with its type (SILENT/VISIBLE) in the completion log.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ task-router.skill

| Property           | Detail                                                                   |
|--------------------|--------------------------------------------------------------------------|
| **Direction**      | Bidirectional (receives from router, sends completion signal back)       |
| **Receives**       | Structured execution instruction packet                                  |
| **Sends back**     | Completion signal (no data — router does not process skill output)       |
| **On failure**     | Returns structured error packet to router with fail reason and recovery suggestion |

---

### ▸ memory-controller.skill

| Property           | Detail                                                                   |
|--------------------|--------------------------------------------------------------------------|
| **Direction**      | Bidirectional (query + write)                                            |
| **Queries**        | Session context, completed skills, cached outputs, freshness status      |
| **Writes**         | Execution start log, per-skill completion entries, full execution summary|
| **Timing**         | Queried at Step 1, Step 3, Step 6. Written at Step 8 and Step 10.       |

---

### ▸ nexus-connector.skill

| Property           | Detail                                                                   |
|--------------------|--------------------------------------------------------------------------|
| **Direction**      | execution-mode → nexus-connector (TYPE 3 only)                           |
| **Sends**          | Full system execution plan packet (Output Type C)                        |
| **nexus-connector manages** | All TYPE 3 inter-skill orchestration after receiving the plan   |
| **Execution-mode role in TYPE 3** | Plan builder and initiator only — not queue manager          |

---

### ▸ All Target Skills (keyword-discovery, serp-analysis, etc.)

| Property           | Detail                                                                   |
|--------------------|--------------------------------------------------------------------------|
| **Direction**      | execution-mode → target skill (one-way dispatch per skill)               |
| **Sends**          | Individual skill dispatch packet (defined in Step 8)                     |
| **Receives back**  | Completion signal + output reference                                     |
| **Execution depth control** | Enforced via mandatory `Execution Depth: FULL` field in every packet |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The execution controller must handle failures gracefully without data loss or silent degradation.

### Failsafe Scenarios and Responses

| Failure Scenario                                      | Detection Method                              | Recovery Action                                                                 |
|-------------------------------------------------------|-----------------------------------------------|---------------------------------------------------------------------------------|
| Input packet from router is malformed                 | Validation failure in Step 1                  | Return structured error to task-router.skill. Do not attempt to parse.          |
| Scope field is NULL or UNCLEAR                        | Step 2 classification fails                   | Return to task-router.skill with flag: `SCOPE_UNRESOLVED`                       |
| Dependency registry lookup fails                      | Step 3 registry query returns error            | Use last known dependency registry version. Log warning. Notify user.           |
| Circular dependency of HARD type detected             | Step 3 DFS traversal finds HARD cycle          | Halt execution. Alert user. Escalate to system diagnostic log.                  |
| Execution graph integrity check fails                 | Step 4 integrity checks fail                  | Discard graph. Rebuild from Step 3. If rebuild also fails, notify user.         |
| A skill in the queue fails mid-execution              | No completion signal received after timeout    | Pause queue. Notify user with fail point and option to retry, skip, or abort.   |
| Output injection mapping cannot be satisfied          | Step 9 mapping fails for a required field      | Mark downstream skill as BLOCKED. Notify user. Offer: provide input manually or skip. |
| memory-controller.skill is unavailable               | Step 1 or Step 3 query returns no response     | Continue without memory check. Flag all dedup checks as UNVERIFIED. Warn user.  |
| User session expires mid-execution chain              | Session ID becomes invalid                    | Save execution graph state to memory-controller.skill. Offer resumption on next session. |
| Execution depth lock violation (skill reports partial logic) | Completion signal includes `PARTIAL_LOGIC` flag | Reject output. Re-dispatch with `FORCE_FULL_EXECUTION`. Log violation.       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — EXECUTION TYPE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
TYPE 1 — SINGLE SKILL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Trigger     : One resolved intent, scope = SINGLE
Skills Run  : 1 (+ HARD dependencies, silently)
Logic Depth : FULL — invariant
Dispatch To : Target skill directly
Use Case    : "run keyword research", "analyze this site", "check SERP for X"

TYPE 2 — PARTIAL FLOW (CHAINED)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Trigger     : Multiple resolved intents, scope = CHAINED, 2–10 skills
Skills Run  : N (defined by intent decomposition + dependency resolution)
Logic Depth : FULL — invariant for every skill in chain
Dispatch To : Self-managed sequential queue
Use Case    : "keyword research + SERP analysis", "audit site and find gaps"

TYPE 3 — FULL SYSTEM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Trigger     : Scope = FULL-SYSTEM, confirmed by user
Skills Run  : Full pipeline (13 skills)
Logic Depth : FULL — invariant for all skills
Dispatch To : nexus-connector.skill
Use Case    : "full SEO strategy", "do everything", "complete analysis"
```

---

*SKILL FILE END: core/execution-mode.skill*
*Nexus SEO Operating System — Version 1.0.0*
