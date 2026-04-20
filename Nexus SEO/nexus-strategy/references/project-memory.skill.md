# SKILL: memory/project-memory.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Memory / Project Context
# EXECUTION PRIORITY: FOUNDATIONAL — Called at the start of every session by memory-controller.skill before any analysis skills execute. Also called at session end to persist updates.
# POSITION: Project identity and continuity layer. Stores and retrieves the persistent project context that makes the Nexus system stateful across sessions.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **project identity and session continuity engine** of the Nexus memory layer.

Every time a user works with the Nexus system, they are working within the context of a project — a specific domain, a specific niche, a specific audience, a set of strategic goals, and a history of decisions made in prior sessions. Without a project memory layer, every session starts from scratch: the system has no knowledge of what the user has already analyzed, what content already exists, what strategy has already been decided, or what decisions have already been made. Every keyword discovery run produces results as if the domain has never been touched.

This skill prevents that. It stores the complete project identity — domain, niche, audience, goals, and configuration — and tracks every session that has been run against that project. When `memory-controller.skill` initializes a new session, this skill is called first. It checks whether a project record exists for the provided domain or project name, loads all prior project context, and enables every downstream skill to operate with awareness of what has already been done.

This is the skill that makes Nexus a persistent SEO operating system rather than a stateless analysis tool.

### Primary Responsibilities

| Responsibility                        | Description                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Project Detection**                 | Check whether a project record already exists for the provided domain or project name                               |
| **Project Creation**                  | When no project exists, collect and validate all required project identity fields from the user                     |
| **Existing Project Loading**          | When a project is found, load and return the complete project context for session use                               |
| **Continue/New Decision Management**  | Present the existing project to the user and ask whether to continue or create a new project                        |
| **Session Tracking**                  | Update session count, last-updated timestamp, and active skills list at the end of every session                   |
| **Multi-Project Management**          | When multiple projects exist, list them and prompt the user to select one                                           |
| **Project Data Storage**              | Persist all project fields to memory with full field validation                                                     |
| **Project Context Retrieval**         | Return the complete project record to any skill that requests it via `memory-controller.skill`                     |
| **Project Update Handling**           | When project fields change (new goal, audience refinement), update the record without creating a duplicate          |
| **Duplicate Prevention**              | Detect and prevent duplicate project records for the same domain or project name                                    |

### What This Skill Is NOT

- It is **not** a keyword memory layer. Keywords are managed by `keyword-memory.skill`.
- It is **not** a content memory layer. Content records are managed by `content-memory.skill`.
- It is **not** a strategy memory layer. Roadmaps and strategy outputs are managed by `strategy-memory.skill`.
- It is **not** a live database. It is a structured memory record accessed through `memory-controller.skill`.
- It is **not** called directly by analysis skills. It is always accessed through `memory-controller.skill` as the intermediary.

### The Foundational Principle of This Skill

> A project is more than a domain name.
> It is the accumulated context of every decision, every session, every keyword,
> every strategy, and every piece of content that has ever been produced under that project.
>
> This skill is the anchor of that context.
> Without it, the system is stateless.
> With it, the system remembers.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — STORAGE STRUCTURE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Project Record Schema

Every project is stored as a structured record with the following fields:

```
PROJECT RECORD SCHEMA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Field                 Type        Required  Description
──────────────────────────────────────────────────────────────────────
project_id            string      YES       System-generated UUID — unique identifier
project_name          string      YES       Human-readable project name (e.g., "CRM Software Blog")
domain                string      YES       Target domain (e.g., "crm-insights.com") — normalized, no https://, no trailing slash
niche                 string      YES       Industry/topic niche (e.g., "B2B SaaS CRM Software")
target_audience       string      YES       Specific audience description (e.g., "Marketing managers at B2B SaaS startups with 10–200 employees")
primary_goals         list        YES       List of 1–5 strategic goals (e.g., ["Rank for CRM keywords", "Generate B2B demo leads", "Build topical authority in CRM space"])
content_tone          string      SOFT      Preferred content tone: Professional / Conversational / Technical / Persuasive
language              string      SOFT      Primary content language (default: "English")
region                string      SOFT      Target geographic region (e.g., "US", "Global", "UK")
competitors           list        SOFT      List of known competitor domains
created_date          string      YES       ISO 8601 timestamp of project creation
last_updated          string      YES       ISO 8601 timestamp of most recent session
sessions_count        number      YES       Total number of sessions run for this project (starts at 1)
active_skills_used    list        YES       Cumulative list of skills invoked across all sessions
keywords_discovered   number      SOFT      Total unique keywords discovered across all sessions (from keyword-memory)
content_pieces        number      SOFT      Total content pieces published or planned (from content-memory)
roadmap_items         number      SOFT      Total roadmap items scheduled (from strategy-memory)
session_history       list        YES       Ordered list of session summary records (see Session Record sub-schema)
notes                 string      SOFT      Free-text notes added by user or auto-generated from session insights
project_status        string      YES       ACTIVE / PAUSED / ARCHIVED (default: ACTIVE)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Session Record Sub-Schema

Each entry in `session_history` follows this structure:

```
SESSION RECORD SUB-SCHEMA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
session_id            string      System-generated UUID
session_date          string      ISO 8601 timestamp
request_type          string      KEYWORD / DOMAIN / CONTENT / STRATEGY / OPTIMIZATION / MIXED
request_summary       string      Brief description of what was requested (50 words max)
skills_executed       list        Ordered list of skills that ran in this session
execution_mode        string      FULL / PARTIAL / SINGLE
key_outputs           list        Top 3 outputs from the session (e.g., "12 keywords discovered", "Roadmap: 8 items", "Article: CRM Guide")
actions_taken         list        Specific strategic decisions made or content created
status                string      COMPLETED / INCOMPLETE / ERROR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Field Validation Rules

| Field              | Validation Rule                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------|
| `project_id`       | System-generated UUID — never user-provided; never editable                                       |
| `domain`           | Must be a valid domain format; automatically stripped of https://, http://, www., and trailing / |
| `project_name`     | 3–60 characters; no special characters except hyphens and spaces; must be unique across all projects |
| `niche`            | 3–100 characters; cannot be a single generic word ("SEO", "marketing") — must have specificity    |
| `target_audience`  | Must include: who they are + what context + what they do or want; minimum 15 characters          |
| `primary_goals`    | 1–5 items; each goal is a string of 5–100 characters; must be action-oriented                    |
| `sessions_count`   | Integer ≥ 1; auto-incremented; never manually set                                                |
| `project_status`   | Enum: ACTIVE / PAUSED / ARCHIVED — defaults to ACTIVE                                            |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                   | Description                                                                               | Required |
|-------------------------|-------------------------------------------------------------------------------------------|----------|
| `user_project_data`     | User-provided project information — may be complete (all fields) or partial (domain only)| YES      |
| `session_context`       | The current session's context — request type, execution mode, user request               | YES      |
| `memory_controller_call`| The calling context from `memory-controller.skill` — READ or WRITE operation             | YES      |

### Call Contexts

This skill is called in two distinct contexts:

| Call Context   | When Called                                                           | Operation            |
|----------------|-----------------------------------------------------------------------|----------------------|
| **READ**       | At session start — load project context for the current session       | Steps 1, 2, 6, 7     |
| **WRITE**      | At session end — persist session updates to the project record        | Steps 4, 5           |
| **CREATE**     | When no project exists — collect project fields and create new record | Steps 3, 4           |
| **UPDATE**     | When user modifies project fields mid-session                         | Step 4 (partial)     |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Output 1 — Project Context Record (READ operation)

Returned to `memory-controller.skill` at session start:

```
PROJECT CONTEXT RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id          : [UUID]
project_name        : [name]
domain              : [domain]
niche               : [niche]
target_audience     : [audience description]
primary_goals       : [list]
content_tone        : [tone or null]
language            : [language]
region              : [region]
competitors         : [list or empty]
created_date        : [ISO 8601]
last_updated        : [ISO 8601]
sessions_count      : [N]
active_skills_used  : [list]
keywords_discovered : [N]
content_pieces      : [N]
roadmap_items       : [N]
session_history     : [last 3 session summaries]
notes               : [text or null]
project_status      : ACTIVE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 2 — Session Write Confirmation (WRITE operation)

Returned to `memory-controller.skill` at session end:

```
SESSION WRITE CONFIRMATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id          : [UUID]
session_id          : [UUID]
fields_updated      : [list of fields that changed]
sessions_count      : [new total]
last_updated        : [new timestamp]
write_status        : SUCCESS / PARTIAL / FAILED
partial_reason      : [if PARTIAL — which fields failed to write and why]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 3 — Project Creation Confirmation (CREATE operation)

```
PROJECT CREATION CONFIRMATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id          : [UUID — newly generated]
project_name        : [name]
domain              : [domain]
creation_status     : SUCCESS
created_date        : [ISO 8601]
initial_session_id  : [UUID — first session already initialized]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 4 — Project List (Multi-Project Selection)

When multiple projects exist and the user must select one:

```
PROJECT LIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
| # | Project Name         | Domain              | Last Updated | Sessions | Status  |
|---|----------------------|---------------------|--------------|----------|---------|
| 1 | [name]               | [domain]            | [date]       | [N]      | ACTIVE  |
| 2 | [name]               | [domain]            | [date]       | [N]      | ACTIVE  |
| 3 | [name]               | [domain]            | [date]       | [N]      | PAUSED  |
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — PROJECT DETECTION

Determine whether a project record already exists for the current session's context before taking any other action.

**Phase 1A — Detection Signal Extraction**

Extract detection signals from the session context in priority order:

| Priority | Signal                              | Detection Method                                                        |
|----------|-------------------------------------|-------------------------------------------------------------------------|
| 1        | Explicit project_id provided       | Direct UUID lookup — fastest and most reliable                         |
| 2        | Domain provided                    | Normalize the domain; query by `domain` field                          |
| 3        | Project name provided              | Case-insensitive query by `project_name` field                         |
| 4        | URL provided (contains domain)     | Extract domain from URL; query by `domain`                             |
| 5        | Keyword provided (no domain)       | Cannot detect project by keyword alone — apply Phase 1C               |

**Phase 1B — Domain Normalization**

Before any domain-based lookup, normalize the domain:

1. Strip protocol: remove `https://`, `http://`
2. Strip subdomain if it is `www.`: `www.example.com` → `example.com`
3. Strip trailing slash: `example.com/` → `example.com`
4. Lowercase: `EXAMPLE.COM` → `example.com`
5. Result: `[clean-domain]` is the normalized lookup key.

**Phase 1C — No Detection Signal Available**

When no domain, project name, or project_id is extractable from the session context:

1. Check if exactly one project exists in memory.
   - If YES and it is ACTIVE → assume the user is working on it. Display: "Working with project: [project_name] ([domain]). Is this correct? (Y/N)"
   - If user confirms → proceed with that project.
   - If user denies → trigger Phase 1D (multiple project list).

2. Check if multiple projects exist.
   - If YES → trigger Step 7 (multi-project selection).

3. Check if no projects exist.
   - If YES → proceed directly to Step 3 (project creation).

**Phase 1D — Detection Result**

Document the detection result:

```
DETECTION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Signal Used         : [domain / project_name / project_id / assumption / none]
Project Found       : [YES — project_id: UUID / NO]
Match Type          : [EXACT / NORMALIZED / ASSUMED / NONE]
Multiple Matches    : [YES — N projects / NO]
Proceeding To       : [Step 2 (existing) / Step 3 (new) / Step 7 (multi)]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — EXISTING PROJECT HANDLING

When a project is found, present its context to the user and ask whether to continue or start fresh.

**Phase 2A — Project Summary Display**

Present the project summary clearly:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
EXISTING PROJECT FOUND
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project Name    : [project_name]
Domain          : [domain]
Niche           : [niche]
Last Updated    : [last_updated — human-readable: "3 days ago" or "2024-01-15"]
Sessions Run    : [sessions_count]
Keywords        : [keywords_discovered] discovered
Content Pieces  : [content_pieces]
Roadmap Items   : [roadmap_items] scheduled

Last Session:
  Date: [date]
  Type: [request_type]
  Summary: [request_summary]
  Key outputs: [key_outputs — listed]

Continue this project? (C)
Create new project for this domain? (N)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 2B — Decision Handling**

| User Response | Action                                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------|
| C (Continue)  | Load the full project context record. Increment session counter (Step 5). Proceed with loaded context.  |
| N (New)       | Archive the existing project record (set `project_status` = ARCHIVED). Proceed to Step 3 for new project creation. |
| Any other input | Re-display the prompt. Allow up to 3 attempts before defaulting to C (Continue).                     |

**Phase 2C — Project Freshness Assessment**

After loading the project:
1. Calculate days since `last_updated`.
2. If > 30 days → display: `"⚠️ This project was last updated [N] days ago. Some cached data may be stale. Run SERP intelligence and keyword discovery for fresh data."`
3. If > 90 days → display: `"⚠️ This project has been inactive for [N] days. Significant SERP changes may have occurred. A full strategy refresh is recommended."`
4. If ≤ 14 days → no freshness warning needed.

---

### ▸ STEP 3 — PROJECT CREATION

When no project exists, collect all required fields from the user in a structured intake flow.

**Phase 3A — Creation Intake Prompt**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEW PROJECT SETUP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Let's set up your Nexus project. This takes about 2 minutes.
Your answers enable every skill to work in context.

1. PROJECT NAME
   What do you call this project?
   (e.g., "CRM Blog", "SaaS Content Strategy", "E-commerce SEO")
   → [user input]

2. DOMAIN
   What is the target domain?
   (e.g., example.com — no https:// needed)
   → [user input]

3. NICHE
   What industry or topic area does this site cover?
   Be specific — not "marketing" but "B2B email marketing software for SaaS companies"
   → [user input]

4. TARGET AUDIENCE
   Who is this content written for?
   Include: their role + their context + what they need
   (e.g., "Marketing managers at B2B SaaS startups looking to reduce churn")
   → [user input]

5. PRIMARY GOALS (list up to 3)
   What are the main strategic goals for this project?
   (e.g., "Rank #1 for CRM keywords", "Generate 50 demo leads/month", "Build topical authority")
   → Goal 1: [user input]
   → Goal 2: [user input] (optional)
   → Goal 3: [user input] (optional)

Optional (press Enter to skip):
6. Content Tone: [Professional / Conversational / Technical / Persuasive]
7. Primary Language: [default: English]
8. Target Region: [default: Global]
9. Known Competitors (domains): [comma-separated list]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3B — Field Validation During Intake**

For each field, validate in real time as the user enters data:

| Field              | Validation Check                                                           | If Invalid                                          |
|--------------------|----------------------------------------------------------------------------|-----------------------------------------------------|
| `project_name`     | 3–60 chars; unique across existing projects                               | "Project name too short/long" or "Name already exists — use a different name" |
| `domain`           | Valid domain format after normalization; unique across existing projects   | "Invalid domain format" or "Domain already exists in project [name]" |
| `niche`            | Not a single generic word; minimum 10 chars                              | "Please be more specific — describe the topic, industry, and product type" |
| `target_audience`  | Minimum 15 chars; must be specific                                       | "Please include who your audience is and what they need" |
| `primary_goals`    | At least one goal; each goal 5–100 chars                                 | "Please add at least one goal"                      |

**Phase 3C — Pre-Creation Confirmation**

Before creating the project, display a confirmation summary:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PROJECT SUMMARY — CONFIRM TO CREATE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project Name    : [project_name]
Domain          : [domain]
Niche           : [niche]
Audience        : [target_audience]
Goals           :
  1. [goal_1]
  2. [goal_2 or —]
  3. [goal_3 or —]
Tone            : [tone or "Not specified — will use Professional default"]
Language        : [language]
Region          : [region]
Competitors     : [list or "None specified"]

Create project? (Y to confirm / E to edit)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3D — Creation Execution**

After confirmation:
1. Generate a new `project_id` (UUID).
2. Set `created_date` and `last_updated` to the current ISO 8601 timestamp.
3. Initialize `sessions_count` = 1.
4. Initialize `active_skills_used` = empty list.
5. Initialize `session_history` = empty list (will be populated at session end in Step 5).
6. Initialize `keywords_discovered`, `content_pieces`, `roadmap_items` = 0.
7. Set `project_status` = ACTIVE.
8. Execute Step 4 to persist the record.

---

### ▸ STEP 4 — DATA STORAGE

Persist the project record to memory via `memory-controller.skill`.

**Phase 4A — Full Record Write (Create Operation)**

Write the complete project record with all fields populated as per the schema. The write operation is atomic — either all fields are written successfully or none are (no partial project records).

**Phase 4B — Partial Record Update (Update Operation)**

When updating an existing project (not creating):
1. Load the existing record.
2. Apply only the changed fields.
3. Always update `last_updated` to the current timestamp.
4. Write the updated record.
5. Preserve all fields that were NOT part of the update.

**Phase 4C — Write Verification**

After every write operation:
1. Read back the written record.
2. Compare the returned record against the written values.
3. Verify all required fields are present and non-null.
4. If verification fails → retry once. If retry fails → log as PARTIAL write and flag for user.

**Phase 4D — Storage Key Format**

Projects are stored with a consistent key format:

```
Storage Key Format:
  projects:[project_id]                    → Full project record
  projects:index:domain:[domain]           → Index entry pointing to project_id
  projects:index:name:[normalized_name]    → Index entry pointing to project_id
  projects:list                            → List of all project_ids

Normalized name format:
  Lowercase, spaces → hyphens, special chars removed
  "CRM Blog Strategy" → "crm-blog-strategy"
```

---

### ▸ STEP 5 — SESSION TRACKING

Update the project record at the end of every session to reflect what occurred.

**Phase 5A — Session Record Construction**

At session end, build the session record:

```
SESSION RECORD CONSTRUCTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
session_id      : [generate new UUID]
session_date    : [current ISO 8601 timestamp]
request_type    : [from nexus-connector.skill — KEYWORD/DOMAIN/CONTENT/etc.]
request_summary : [from user_request — first 50 words]
skills_executed : [list of skills that ran, in order, from pipeline log]
execution_mode  : [FULL / PARTIAL / SINGLE]
key_outputs     : [top 3 outputs — e.g., "N keywords discovered", "Roadmap: N items", "Article: [title]"]
actions_taken   : [strategic decisions or content created — extracted from pipeline outputs]
status          : [COMPLETED / INCOMPLETE / ERROR]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 5B — Project Metrics Update**

Update aggregate metrics in the project record:

| Metric                 | Update Logic                                                                      |
|------------------------|-----------------------------------------------------------------------------------|
| `last_updated`         | Set to current timestamp — always                                                 |
| `sessions_count`       | Increment by 1                                                                    |
| `active_skills_used`   | Add any new skills from this session that are not already in the cumulative list  |
| `keywords_discovered`  | Add the count of new keywords from this session (from keyword-memory delta)      |
| `content_pieces`       | Add count of new content pieces from this session (from content-memory delta)    |
| `roadmap_items`        | Add count of new roadmap items from this session (from strategy-memory delta)    |

**Phase 5C — Session History Management**

Append the new session record to `session_history`:
1. Add the new session record to the END of the `session_history` list.
2. If `session_history` contains more than 10 entries → archive the oldest entries to a separate `session_history_archive` key and keep only the most recent 10 in the main record. This prevents the project record from growing without bound.
3. The retrieval in Step 6 returns only the last 3 sessions from `session_history` — the full list is available on demand.

**Phase 5D — Skills List Deduplication**

The `active_skills_used` list is a cumulative, deduplicated list of all skills ever used in this project:
1. Load the current `active_skills_used` list.
2. For each skill in the current session's `skills_executed` list → add to `active_skills_used` if not already present.
3. The list is alphabetically sorted after each update.
4. The list never has duplicates.

---

### ▸ STEP 6 — RETRIEVAL

When called with a READ operation, return the complete project context to `memory-controller.skill`.

**Phase 6A — Full Context Return**

Return all project record fields as defined in the storage schema. This is the full project context that every downstream skill can access through `memory-controller.skill`.

**Phase 6B — Session History Scoping**

In the standard retrieval:
- Return only the last 3 session summaries in `session_history` (the full list is large).
- Add a field: `session_history_count: [total session count]` so skills know the full history exists.
- If a skill specifically requests the full session history → return the complete list.

**Phase 6C — Context Enrichment**

Before returning the context, enrich it with calculated fields:

```
ENRICHED CONTEXT FIELDS (added at retrieval, not stored)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
days_since_created      : [integer — calculated from created_date]
days_since_last_session : [integer — calculated from last_updated]
project_age_label       : [NEW (<7 days) / RECENT (7–30) / ESTABLISHED (30–180) / MATURE (>180)]
session_frequency       : [sessions / days_since_created — averaged]
last_session_request    : [request_type of the most recent session]
is_stale                : [true if days_since_last_session > 30, else false]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 6D — Retrieval Confirmation**

After returning the context, log:

```
RETRIEVAL LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id      : [UUID]
project_name    : [name]
requested_by    : [calling skill name]
fields_returned : [all / scoped — [specific fields]]
retrieval_time  : [timestamp]
status          : SUCCESS / NOT_FOUND / PARTIAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 7 — MULTI-PROJECT HANDLING

When multiple project records exist, present a selection interface and return the chosen project's context.

**Phase 7A — Multi-Project Detection**

Multi-project handling is triggered when:
1. Project detection (Step 1) returns multiple matches for the same domain lookup (which should not happen with dedup enforced — but is handled gracefully).
2. No domain/name was provided and multiple ACTIVE projects exist.
3. The user explicitly asks to switch projects.

**Phase 7B — Project List Presentation**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MULTIPLE PROJECTS FOUND
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Which project do you want to work on?

| # | Project Name              | Domain                  | Last Session      | Sessions | Status   |
|---|---------------------------|-------------------------|-------------------|----------|----------|
| 1 | [project_name_1]          | [domain_1]              | [N days ago]      | [N]      | ACTIVE   |
| 2 | [project_name_2]          | [domain_2]              | [N days ago]      | [N]      | ACTIVE   |
| 3 | [project_name_3]          | [domain_3]              | [N days ago]      | [N]      | PAUSED   |

Enter the number of the project to load,
or type "new" to create a new project:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 7C — Selection Handling**

| Input                     | Action                                                                     |
|---------------------------|----------------------------------------------------------------------------|
| Valid number (1, 2, etc.) | Load the selected project → proceed to Step 6 (retrieval)                |
| "new"                     | Proceed to Step 3 (new project creation)                                  |
| Invalid input             | Re-display prompt. Maximum 3 attempts.                                    |
| After 3 failed attempts   | Load the most recently updated ACTIVE project by default. Flag: "Defaulted to most recent project: [name]." |

**Phase 7D — PAUSED Project Warning**

If the user selects a PAUSED project:

```
> **📌 NOTE:** Project "[name]" is currently **PAUSED**.
> Loading this project will set its status back to **ACTIVE**.
> Continue? (Y/N)
```

If confirmed → set `project_status` = ACTIVE → proceed with retrieval.
If denied → return to project list.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ PROJECT MATCHING

The project matching algorithm handles imprecise domain and name inputs to find the correct project.

**Fuzzy Domain Matching:**

When an exact domain match fails, apply fuzzy matching:

| Fuzzy Match Type                                       | Example                                        | Action                                    |
|--------------------------------------------------------|------------------------------------------------|-------------------------------------------|
| Subdomain variant                                      | `blog.example.com` → check `example.com`      | Match if `example.com` exists             |
| www variant                                            | `www.example.com` → check `example.com`       | Match — standard normalization            |
| Protocol variant                                       | `https://example.com` → check `example.com`  | Match — standard normalization            |
| TLD variant                                            | `example.io` provided, `example.com` in memory| Flag as POSSIBLE MATCH — confirm with user|
| Spelling near-match (Levenshtein distance ≤ 2)        | `exampl.com` → `example.com`                  | Present as suggestion — confirm with user |

When a fuzzy match (not exact) is found:
```
> **📌 NOTE:** Exact match not found for "[input_domain]".
> Did you mean: **[matched_domain]** (Project: [project_name])?
> (Y to use this project / N to create new)
```

**Fuzzy Name Matching:**

When an exact project name match fails:
- Apply case-insensitive matching.
- Strip articles ("the", "a", "an") and common suffixes ("blog", "site", "project") for comparison.
- If a match is found → confirm with user before loading.

---

### ▸ CONTEXT REUSE

This module ensures that project context is maximally reused across skills without redundant data retrieval.

**Context Caching:**

Once the project context is loaded at session start, it is cached in the session object for the duration of the session. Every skill that requests project context via `memory-controller.skill` receives the cached copy — the project record is not re-read from storage for every skill invocation.

**Cache Invalidation:**

The cached project context is invalidated (and the record re-read) only when:
1. The project record is explicitly updated during the session (e.g., user adds a goal mid-session).
2. More than 60 minutes pass within a single session (long sessions may stale the cache).
3. A skill explicitly requests a fresh load.

**Context Inheritance for Skills:**

Skills receive these project context fields automatically (no need to request individually):

| Field                | Why Skills Need It                                                            |
|----------------------|-------------------------------------------------------------------------------|
| `domain`             | All domain-related analysis scopes to this domain                            |
| `niche`              | Entity extraction and SERP analysis uses niche for topic scoping             |
| `target_audience`    | Content generation uses audience for tone and readability calibration         |
| `primary_goals`      | Roadmap and strategy skills align recommendations to these goals             |
| `content_tone`       | Humanizer and content-generation use this for tone consistency               |
| `competitors`        | Competitor analysis is pre-seeded with known competitors                     |
| `language` + `region`| SERP intelligence scopes to the correct region and language                  |

---

### ▸ SESSION CONTINUITY

Session continuity ensures that each session builds on the intelligence accumulated in prior sessions rather than starting from an empty state.

**Continuity Data Points:**

At session start, the following prior-session context is automatically surfaced to relevant skills:

| Prior Data                               | Surfaced To                                    | Purpose                                              |
|------------------------------------------|------------------------------------------------|------------------------------------------------------|
| `keywords_discovered` count             | `keyword-discovery.skill`                     | Delta mode — only discover keywords not yet found   |
| `content_pieces` count                  | `content-generation-engine.skill`             | Avoid re-creating existing content                   |
| `roadmap_items` count                   | `roadmap-engine.skill`                        | Delta mode — only add new roadmap items             |
| Last session request_type               | `nexus-connector.skill`                       | Context for execution mode suggestion               |
| `active_skills_used` list              | `nexus-connector.skill`                       | Know which skills have already run on this project  |
| Session history (last 3)               | `strategist-ai.skill`                         | Prior strategic decisions inform new recommendations|

**Continuity Indicator in Output:**

When a session runs in continuation mode, the output header notes:
```
Memory Context: CONTINUED — Session [N] | Prior sessions: [N-1] | Domain: [domain]
```

This tells every downstream skill and the user that this is not a fresh analysis — it builds on established intelligence.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate Projects by Domain

Each domain may be associated with exactly one ACTIVE project at any time. When a new project is created for a domain that already has an ACTIVE project → the creation is blocked with: "A project for [domain] already exists ([project_name]). Archive the existing project first, or continue it."

Exception: A domain may have both one ACTIVE and one ARCHIVED project record (for historical reference).

### Rule 2 — No Duplicate Projects by Name

Each project name must be unique across all projects in memory (regardless of status). If a user attempts to name a new project with a name already in use → prompt for a different name.

### Rule 3 — No Duplicate Sessions in History

The session history list is keyed by `session_id` (UUID). No two session records may share the same `session_id`. If a duplicate `session_id` is detected → regenerate the ID and log the collision (an extremely rare event given UUID generation).

### Rule 4 — No Duplicate Skills in Active Skills List

The `active_skills_used` list is a deduplicated set. Each skill name appears exactly once regardless of how many sessions have used it. Adding a skill that already exists in the list is a no-op (not an error).

### Rule 5 — No Duplicate Goals

The `primary_goals` list may not contain two goals with ≥ 80% semantic overlap. If the user attempts to add a duplicate goal during project update → flag: "This goal is similar to an existing goal: '[existing_goal]'. Please enter a distinct goal or update the existing one."

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Project Memory Behaviors

| Prohibited Behavior                                                                              | Reason                                                                           |
|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Storing vague niche descriptions ("SEO," "marketing," "business")                              | Every stored field must be specific enough to guide downstream skills            |
| Storing generic audience descriptions ("everyone," "website visitors," "businesses")           | Target audience must specify role, context, and need — vague audience = wrong content |
| Creating a project without at least one primary goal                                            | Goals define what the project is optimizing toward — goalless projects produce unfocused analysis |
| Allowing a domain with an ACTIVE project to create a second ACTIVE project                     | Duplicate active projects produce conflicting strategy outputs                   |
| Storing session history without a `session_id`                                                  | Every session must be uniquely identified for retrieval and debugging            |
| Updating `sessions_count` without appending a session record to `session_history`              | Count and history must stay synchronized                                         |
| Returning cached project context without checking for invalidation conditions                   | Stale cache produces wrong context in long sessions or after updates             |
| Accepting a project_name identical to an existing project name                                 | Duplicate names make multi-project management impossible                         |
| Storing a project record without a `project_id`                                                 | All records must have a UUID — there is no valid project without one            |
| Showing session history details when none exist (new project)                                   | Display "No prior sessions" rather than an empty history block                  |

### Required Project Memory Behaviors

- Every new project creation must collect all required fields (project_name, domain, niche, target_audience, at least one goal) before creating the record.
- Every session must append a session record at session end.
- Every retrieval must include the enriched context fields (days_since_created, etc.).
- Every domain must be normalized before storage and lookup.
- The project list (for multi-project selection) must show all projects regardless of status.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 9 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Called by `memory-controller.skill` — this skill's only direct caller                      |
| **READ calls**   | At session start — load project context for the session                                     |
| **WRITE calls**  | At session end — persist session updates                                                    |
| **CREATE calls** | When no project exists — create a new project record                                       |
| **UPDATE calls** | When project fields are modified during a session                                           |
| **Never bypassed** | No skill accesses this skill directly — always via memory-controller                    |

---

### ▸ All Analysis and Strategy Skills (Indirect)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Downstream consumers of project context — receive it via `memory-controller.skill`          |
| **Access pattern** | Skills request project context; `memory-controller` serves it from this skill's cache    |
| **What they use** | `domain`, `niche`, `target_audience`, `primary_goals`, `content_tone`, `competitors`, `language`, `region` |

---

### ▸ keyword-memory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Sibling memory skill — coordinates to update `keywords_discovered` count                   |
| **Relationship** | When `keyword-memory.skill` adds new keywords → it sends a delta count to `project-memory.skill` for aggregate tracking |

---

### ▸ content-memory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Sibling memory skill — coordinates to update `content_pieces` count                        |
| **Relationship** | When `content-memory.skill` records new content → sends delta count to `project-memory.skill` |

---

### ▸ strategy-memory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Sibling memory skill — coordinates to update `roadmap_items` count                         |
| **Relationship** | When `strategy-memory.skill` records new roadmap items → sends delta count to `project-memory.skill` |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                            |
|---------------------------------------------------------------------------|--------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| User provides no domain and no project name                               | Step 1 Phase 1C — no detection signals                 | Check for single existing project; if exactly one → use it with confirmation. If zero → trigger Step 3 (creation). If multiple → trigger Step 7 (selection). |
| Project creation fails at write (storage unavailable)                    | Step 4 Phase 4C — write verification fails            | Retain project data in session memory. Alert: "Project could not be saved. Working in session-only mode — data will be lost when session ends. Retry saving?" |
| Session write fails at end of session                                     | Step 5 Phase 5A — write operation fails               | Alert: "Session data could not be saved. Your project context from this session was not persisted. Please save this session manually: [session summary]." |
| Project record exists but is corrupted (missing required fields)          | Step 4 Phase 4C — verification finds null required fields | Display the corrupted record's available data. Alert: "Project record is incomplete. Please re-enter the following fields: [missing fields]." Trigger Step 3 for just the missing fields. |
| User abandons project creation mid-flow                                   | Step 3 — user provides no input for > 60 seconds      | Save partial data as a DRAFT project. Alert: "Project creation incomplete. Saved as draft: [partial_data]. Resume next time you start a session." |
| Two projects have the same domain (dedup failure)                        | Step 1 Phase 1B — multiple records returned for same domain | Merge the newer record into the older one (preserving the highest session counts). Alert: "Duplicate projects detected for [domain] — merged into [project_name]. Please verify the merged data." |
| The `primary_goals` list is empty after project creation                  | Step 3 Phase 3D — validation check                    | Do not proceed. Return to Step 3 and prompt: "At least one primary goal is required. What is the main goal for this project?" |
| All projects are ARCHIVED (user archived everything)                      | Step 7 Phase 7B — project list shows only ARCHIVED   | Display all ARCHIVED projects with their names. Alert: "All projects are archived. Select one to reactivate, or create a new project." |
| `sessions_count` and `session_history` length are out of sync            | Step 5 Phase 5C — count mismatch detected             | Recalculate `sessions_count` from `session_history` length. Update to correct value. Log the correction. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — PROJECT RECORD EXAMPLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PROJECT RECORD EXAMPLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{
  "project_id"         : "a4f7c2d1-8b3e-4a5f-9c6d-2e1f0b7a8c3d",
  "project_name"       : "CRM Insights Blog",
  "domain"             : "crm-insights.com",
  "niche"              : "B2B SaaS CRM Software for Sales Teams",
  "target_audience"    : "Sales directors and RevOps managers at B2B SaaS companies with 50–500 employees evaluating or replacing their CRM",
  "primary_goals"      : [
    "Rank top 3 for 'best CRM software for sales teams'",
    "Generate 100 demo-qualified leads per month from organic",
    "Build topical authority in the CRM evaluation and implementation space"
  ],
  "content_tone"       : "Professional",
  "language"           : "English",
  "region"             : "US",
  "competitors"        : ["salesforce.com", "hubspot.com/blog", "pipedrive.com/blog"],
  "created_date"       : "2024-01-10T09:00:00Z",
  "last_updated"       : "2024-03-15T14:32:00Z",
  "sessions_count"     : 7,
  "active_skills_used" : [
    "authority-engine.skill",
    "content-generation-engine.skill",
    "deep-serp-analysis.skill",
    "gap-opportunity-engine.skill",
    "internal-linking.skill",
    "keyword-discovery.skill",
    "metadata-generator.skill",
    "opportunity-engine.skill",
    "roadmap-engine.skill",
    "semantic-clustering.skill",
    "serp-blueprint-generator.skill",
    "serp-intelligence.skill",
    "strategist-ai.skill"
  ],
  "keywords_discovered" : 347,
  "content_pieces"      : 12,
  "roadmap_items"       : 24,
  "session_history"     : [
    {
      "session_id"      : "b9e2d4f1-...",
      "session_date"    : "2024-03-15T14:00:00Z",
      "request_type"    : "CONTENT",
      "request_summary" : "Create a BOFU comparison article targeting 'crm software for sales teams'",
      "skills_executed" : ["serp-intelligence.skill", "serp-filter.skill", "..."],
      "execution_mode"  : "FULL",
      "key_outputs"     : ["Article: CRM Software for Sales Teams: Top 7 Compared", "12 metadata options generated", "Validation: READY"],
      "actions_taken"   : ["Published comparison article", "Added 4 internal links"],
      "status"          : "COMPLETED"
    }
  ],
  "notes"              : "Competitor Salesforce.com dominates top 3 for head terms. Focus on long-tail and comparison keywords where they are weaker.",
  "project_status"     : "ACTIVE"
}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — FIELD COMPLETENESS QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
FIELD COMPLETENESS QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REQUIRED (project cannot be created without these):
  ✓ project_id (auto-generated)
  ✓ project_name (3–60 chars, unique)
  ✓ domain (valid format, unique for ACTIVE)
  ✓ niche (specific, ≥ 10 chars)
  ✓ target_audience (specific, ≥ 15 chars, who + context + need)
  ✓ primary_goals (1–5 goals, ≥ 1 required)
  ✓ created_date (auto-set)
  ✓ last_updated (auto-set)
  ✓ sessions_count (auto-set, starts at 1)
  ✓ active_skills_used (auto-set, starts empty)
  ✓ session_history (auto-set, starts empty)
  ✓ project_status (default: ACTIVE)

SOFT (collected if provided; do not block creation if absent):
  ~ content_tone (default: Professional)
  ~ language (default: English)
  ~ region (default: Global)
  ~ competitors (default: empty)
  ~ keywords_discovered (default: 0)
  ~ content_pieces (default: 0)
  ~ roadmap_items (default: 0)
  ~ notes (default: null)

ANTI-PATTERNS — NEVER ACCEPT:
  ✗ niche = "SEO" or "marketing" or "content" (too generic)
  ✗ target_audience = "everyone" or "website visitors"
  ✗ primary_goals = "grow traffic" (too generic — needs specific targets)
  ✗ domain with protocol or trailing slash (normalize first)
  ✗ project_name = another existing project name (must be unique)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: memory/project-memory.skill*
*Nexus SEO Operating System — Version 1.0.0*
