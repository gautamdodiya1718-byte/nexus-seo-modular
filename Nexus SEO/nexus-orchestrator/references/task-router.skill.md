# SKILL: core/task-router.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Core / Entry Point
# EXECUTION PRIORITY: HIGHEST — This skill activates before any other skill.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **central intelligence gateway** of the Nexus system.

It is the **first and only entry point** through which all user input must pass before any skill is triggered. No other skill in the Nexus system may be activated without first passing through this router, except when explicitly chained by `nexus-connector.skill` during a pre-approved full-system workflow.

### Primary Responsibilities

| Responsibility              | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| **Input Parsing**           | Decompose raw user input into structured intent signals                    |
| **Intent Detection**        | Map parsed intent to the correct target skill                              |
| **Scope Determination**     | Distinguish between single-task, multi-task, and full-system requests      |
| **Clarification Gating**    | Pause execution and prompt user if intent is ambiguous or multi-directional|
| **Execution Delegation**    | Hand off structured task instructions to `execution-mode.skill`            |
| **Repeat Prevention**       | Block redundant executions by querying `memory-controller.skill`           |
| **Post-Task Continuity**    | Ask user for next action after every completed task                        |

### What This Skill Is NOT

- It is **not** an executor. It never processes SEO data directly.
- It is **not** a generator. It never writes content or produces analysis.
- It is **not** a fallback. If routing fails, it asks — it does not guess and run.
- It is **not** optional. Every user interaction begins here, without exception.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill accepts **any raw user message** as input. There are no format requirements imposed on the user. The router must handle all input forms gracefully.

### Accepted Input Types

| Input Type             | Example                                                             |
|------------------------|---------------------------------------------------------------------|
| **Single keyword**     | `"project management software"`                                     |
| **Website URL**        | `"https://example.com"` or `"analyze mysite.com"`                  |
| **Vague SEO request**  | `"I need to improve my SEO"` / `"help me rank better"`             |
| **Direct command**     | `"run keyword research for AI writing tools"`                       |
| **Numbered selection** | `"3"` or `"option 5"` (when responding to the Nexus menu)          |
| **Multi-intent query** | `"analyze my site and find content gaps and write an article"`      |
| **Full system trigger**| `"do everything"` / `"run full SEO strategy"` / `"full analysis"`  |
| **Ambiguous input**    | `"what can you do?"` / `"start"` / `"go"` / `"help"`              |
| **Topic phrase**       | `"I want to write about machine learning for beginners"`            |

### Input Constraints

- Input may arrive with no context (cold start).
- Input may arrive mid-workflow (continuation of a prior session).
- Input may contain a URL, a seed keyword, a topic, or a business domain — all are valid.
- Input may be incomplete or contradictory — the router must handle this gracefully via the clarification gate.

### Input Pre-Conditions (Checked Before Any Routing)

Before routing begins, the following pre-conditions are verified:

1. **Is this a continuation of a previous task?**
   → Query `memory-controller.skill` for session context.

2. **Is there an active workflow already in progress?**
   → If yes, check if the new input overrides or extends it.

3. **Is the input a system command (reset, clear, stop)?**
   → If yes, halt current workflow and reset state.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill produces **one of two possible output types** per invocation. No other output form is valid.

### Output Type A — Routed Execution Instruction

When intent is clearly detected, the router produces a **structured execution instruction packet** and sends it to `execution-mode.skill`.

**Format:**

```
NEXUS ROUTE DECISION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Detected Intent     : [intent label]
Confidence Level    : [HIGH / MEDIUM / LOW]
Primary Subject     : [keyword / URL / topic / domain]
Target Skill        : [skill-name.skill]
Execution Mode      : [SINGLE / CHAINED / FULL-SYSTEM]
Parameters Passed   :
  - subject         : [value]
  - scope           : [value]
  - constraints     : [value if any]
Memory Check        : [PASSED / FLAGGED — reason]
Proceed to          : execution-mode.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Type B — Clarification Menu (Task Selection)

When intent is ambiguous, unclear, or multi-directional, the router displays the **Nexus Task Menu** and halts. No execution occurs until the user responds.

**Format:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — What do you want to do?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1   Keyword Research
2   SERP Analysis
3   Website Analysis
4   Content Gap Analysis
5   Opportunity Analysis
6   Authority Mapping
7   Content Roadmap
8   Article Blueprint
9   Optimization Suggestions
10  Deep SERP Analysis
11  Full SEO Strategy

Enter number or describe your goal:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Rules for Output Type B:**
- Display menu exactly as specified above — no modifications, no additions.
- Do NOT pre-select any option.
- Do NOT suggest what the user "might want."
- Do NOT provide descriptions of each option unless the user explicitly asks.
- Wait in a blocked state until user responds.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — SESSION CONTEXT RETRIEVAL

Before parsing the user's input, retrieve session context from `memory-controller.skill`.

**Actions:**

1. Query `memory-controller.skill` with:
   - Current session ID
   - Timestamp of last interaction
   - Last executed skill (if any)
   - Last resolved intent (if any)

2. Evaluate the response:

   | Memory State             | Router Action                                           |
   |--------------------------|---------------------------------------------------------|
   | **No prior session**     | Treat as cold start. Proceed to Step 2.                |
   | **Active session found** | Load prior context. Check if input extends or overrides.|
   | **Completed session**    | Treat as new request. Proceed to Step 2.               |
   | **Flagged duplicate**    | Notify user. Do not re-execute. Offer alternatives.    |

3. If a prior session context is loaded:
   - Inject it silently into the parsing step.
   - Do NOT inform the user that session data was loaded unless it changes routing behavior visibly.

---

### ▸ STEP 2 — RAW INPUT PARSING

Decompose the raw user input into structured components.

**Parse for the following signals:**

| Signal Type       | What to Extract                                               |
|-------------------|---------------------------------------------------------------|
| **Action verb**   | analyze, research, find, generate, write, build, optimize, audit |
| **Subject noun**  | keyword, site, URL, topic, article, niche, competitor, gap   |
| **Scope marker**  | single, all, full, everything, quick, deep, complete         |
| **Target entity** | Specific keyword string, domain, URL, topic phrase           |
| **Modifier**      | for beginners, B2B, long-tail, high-volume, low-competition  |

**Parsing Rules:**

- Strip filler words: `"can you"`, `"please"`, `"I want to"`, `"help me"`, `"I need"`
- Normalize synonyms:
  - `"audit"` → `"analyze"`
  - `"gaps"` → `"content gap analysis"`
  - `"roadmap"`, `"plan"`, `"strategy"` → evaluate against scope to differentiate
  - `"rank"`, `"rankings"`, `"SERP"` → check if referring to keyword research or SERP analysis
- Preserve specificity: if user provides an exact keyword or domain, lock it in as `primary_subject`.
- Flag multi-intent: if more than one action verb or subject type is detected, mark as `MULTI_INTENT = TRUE`.

**Output of Step 2 — Parsed Input Structure:**

```
PARSED INPUT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Raw Input          : [original user message]
Action Detected    : [verb(s)]
Subject Type       : [keyword / URL / topic / vague]
Primary Subject    : [extracted value or NULL]
Scope              : [SINGLE / MULTI / FULL / UNCLEAR]
Modifiers          : [list or NONE]
Multi-Intent Flag  : [TRUE / FALSE]
Ambiguity Score    : [LOW / MEDIUM / HIGH]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 3 — INTENT DETECTION AND SKILL MAPPING

Using the parsed input structure from Step 2, resolve the user's primary intent and map it to the correct target skill.

**Intent-to-Skill Mapping Table:**

| Detected Intent                                    | Target Skill                          |
|----------------------------------------------------|---------------------------------------|
| Keyword research / keyword ideas / find keywords   | `keyword-discovery.skill`             |
| SERP analysis / who ranks / what's ranking         | `serp-analysis.skill`                 |
| Deep SERP / SERP patterns / ranking signals        | `deep-serp-analysis.skill`            |
| Website analysis / site audit / analyze my site    | `website-analysis.skill`              |
| Content gaps / what am I missing / competitor gaps | `gap-opportunity-engine.skill`        |
| Opportunity analysis / low-competition wins        | `opportunity-scoring.skill`           |
| Authority mapping / link profile / domain authority| `authority-mapping.skill`             |
| Content roadmap / content plan / publishing plan   | `roadmap-engine.skill`                |
| Article blueprint / outline / structure            | `article-blueprint.skill`             |
| Optimization / improve existing content / on-page  | `content-optimizer.skill`             |
| Full SEO strategy / do everything / full analysis  | `nexus-connector.skill`               |

**Confidence Level Rules:**

| Condition                                                       | Confidence |
|-----------------------------------------------------------------|------------|
| Single action verb + single subject + specific value            | HIGH       |
| Single action verb + vague subject OR no primary subject        | MEDIUM     |
| Multiple action verbs OR subject type = unclear OR MULTI_INTENT | LOW        |
| Input is 1-2 words with no context AND no prior session         | LOW        |

**Confidence Routing Decision:**

| Confidence | Router Action                                             |
|------------|-----------------------------------------------------------|
| HIGH       | Proceed directly to Step 4 (Memory Check)                 |
| MEDIUM     | Proceed to Step 4, but flag for user confirmation         |
| LOW        | Skip to Step 6 — display clarification menu              |

---

### ▸ STEP 4 — MEMORY DEDUPLICATION CHECK

Before executing any routing decision, validate against `memory-controller.skill` to ensure this task has not already been completed in the current or recent session.

**Deduplication Check Protocol:**

1. Send to `memory-controller.skill`:
   - Detected intent
   - Primary subject (keyword / URL / topic)
   - Target skill resolved in Step 3

2. Evaluate response:

   | Memory Response                              | Router Action                                                   |
   |----------------------------------------------|-----------------------------------------------------------------|
   | **Not previously executed**                  | Proceed to Step 5                                               |
   | **Executed in current session**              | Notify user: "This task was already completed. See: [output reference]." Offer to re-run or skip. |
   | **Executed with different subject**          | Proceed — treat as new task                                     |
   | **Executed with same subject, older session**| Notify user. Offer: re-run, use cached result, or skip.        |

3. Do NOT silently skip a task. If a duplicate is detected, always notify the user explicitly.

4. Do NOT re-run a task automatically. Always require user confirmation before re-executing a previously completed skill.

---

### ▸ STEP 5 — SCOPE VALIDATION

Determine the execution scope before handing off to `execution-mode.skill`.

**Scope Classification:**

| Scope Type      | Trigger Condition                                              | Execution Path                   |
|-----------------|----------------------------------------------------------------|----------------------------------|
| **SINGLE**      | One intent, one subject, one target skill                      | Route to target skill directly   |
| **CHAINED**     | Multiple intents detected in sequence (e.g., "research then write") | Build a chained execution plan  |
| **FULL-SYSTEM** | "do everything", "full SEO strategy", or "full analysis" detected | Route to `nexus-connector.skill` |

**Chained Execution Rules:**

- When MULTI_INTENT = TRUE and scope = CHAINED:
  1. Extract all intents in order of dependency (e.g., keyword research must precede content generation).
  2. Build a sequential execution queue.
  3. Send the queue to `execution-mode.skill` with a `CHAINED` mode flag.
  4. Do NOT execute all tasks simultaneously.
  5. Prompt user between each task in the chain for confirmation before proceeding to the next.

**Full-System Execution Rules:**

- When scope = FULL-SYSTEM:
  1. Do NOT auto-trigger. Even if the user says "do everything," pause.
  2. Display the following confirmation gate:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Full System Mode Requested
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
This will execute the complete Nexus SEO workflow:
  → Keyword Discovery
  → SERP Analysis
  → Website Analysis
  → Content Gap Analysis
  → Opportunity Scoring
  → Authority Mapping
  → Content Roadmap
  → Article Blueprints

Estimated steps: 8+ sequential skill executions.

Confirm? [YES / NO]
  Or enter a number to run only that task instead.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

  3. Only route to `nexus-connector.skill` upon explicit `YES` confirmation.
  4. If the user selects a number from the menu instead, treat as a SINGLE task request.

---

### ▸ STEP 6 — CLARIFICATION GATE (LOW CONFIDENCE OR AMBIGUITY)

Activated when:
- Confidence level = LOW
- Ambiguity Score = HIGH
- Input is system trigger words with no subject (`"start"`, `"go"`, `"help"`, `"what can you do"`)
- Input is a cold start with no identifiable intent

**Action:**

Display the exact Nexus Task Menu defined in Section 3 — Output Type B.

**Strict Behavioral Rules During Clarification Gate:**

1. Do NOT execute any skill.
2. Do NOT pre-select any option.
3. Do NOT add commentary or suggestions.
4. Do NOT explain what each menu option does unless user asks.
5. Wait. The system is in a **fully blocked state** until user responds.

**Post-Clarification Handling:**

Upon user response to the menu:

| User Response Type           | Action                                             |
|------------------------------|----------------------------------------------------|
| Number (1–11)                | Map to skill per table in Step 3. Proceed to Step 4.|
| Descriptive text             | Re-parse as new input. Return to Step 2.           |
| "What does option X mean?"   | Provide a single-sentence description. Re-display menu. |
| Invalid input / out of range | Display: "Please enter a number between 1 and 11, or describe your goal." Re-display menu. |

---

### ▸ STEP 7 — EXECUTION HANDOFF

When all prior steps have cleared, route to `execution-mode.skill` using a structured execution instruction packet.

**Execution Instruction Packet Format:**

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

**Handoff Rules:**

- The router does NOT wait for the skill to complete before returning control.
- The router does NOT process the output of the target skill.
- Skill output is managed by `execution-mode.skill` and returned to the user directly.
- The router only receives a **completion signal** from `execution-mode.skill` to trigger Step 8.

---

### ▸ STEP 8 — POST-EXECUTION CONTINUITY PROMPT

After receiving a completion signal from `execution-mode.skill`, the router always displays:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Task complete.
Do you want to perform another task?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Post-Execution Rules:**

1. Do NOT auto-suggest next tasks.
2. Do NOT pre-fill any next action.
3. If user responds with a new request → return to Step 1.
4. If user says `"no"` or `"done"` → display session summary from `memory-controller.skill` and close.
5. If user says `"yes"` without specifying → display the Nexus Task Menu (Output Type B).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ MULTI-INTENT QUERY HANDLING

When a user provides a compound request (e.g., `"analyze my site, find content gaps, and write an article"`), the router must:

1. **Decompose** the input into individual intent units.
2. **Classify** each unit using the intent mapping table.
3. **Order** the intents by dependency:

   **Dependency Chain (Sequential Order):**
   ```
   keyword-discovery.skill
       → serp-analysis.skill
           → website-analysis.skill
               → gap-opportunity-engine.skill
                   → opportunity-scoring.skill
                       → roadmap-engine.skill
                           → article-blueprint.skill
                               → content-generation-engine.skill
   ```

4. **Eliminate intents** that are already covered by an upstream skill in the chain (do not run keyword discovery AND SERP analysis AND gap analysis as separate tasks if gap analysis already subsumes keyword and SERP work).
5. **Present the resolved queue** to the user before executing:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Multi-Task Plan Detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Resolved execution queue:
  Step 1 → website-analysis.skill
  Step 2 → gap-opportunity-engine.skill
  Step 3 → article-blueprint.skill

Proceed in this order? [YES / Modify]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

6. Only proceed upon user confirmation.

---

### ▸ DOMINANT INTENT PRIORITIZATION

When a query contains multiple signals but not all are equal weight:

**Priority Rules:**

| Priority | Condition                                                             |
|----------|-----------------------------------------------------------------------|
| 1st      | Explicit direct command (`"write an article about X"`)               |
| 2nd      | Named skill reference (`"run keyword discovery"`)                    |
| 3rd      | Clear action + clear subject (`"analyze example.com"`)               |
| 4th      | Indirect goal statement (`"I want to rank for X"`)                   |
| 5th      | Vague SEO goal (`"help me improve my SEO"`)                          |

- If Priority 1 or 2 are detected, ignore all lower-priority signals.
- If only Priority 4 or 5 are detected → trigger clarification gate.

---

### ▸ FULL-SYSTEM TRIGGER DETECTION

The following input patterns must be detected as full-system triggers, regardless of phrasing variation:

| Pattern                                   | Detected As        |
|-------------------------------------------|--------------------|
| `"do everything"`                         | FULL-SYSTEM        |
| `"full analysis"`                         | FULL-SYSTEM        |
| `"run everything"`                        | FULL-SYSTEM        |
| `"complete SEO strategy"`                 | FULL-SYSTEM        |
| `"full SEO"`                              | FULL-SYSTEM        |
| `"all tasks"`                             | FULL-SYSTEM        |
| `"end to end"`                            | FULL-SYSTEM        |
| `"the whole thing"`                       | FULL-SYSTEM        |
| `"start from scratch and do it all"`      | FULL-SYSTEM        |

Upon detection of any of these:
- Do NOT route immediately.
- Trigger the Full-System Confirmation Gate defined in Step 5.

---

### ▸ SYSTEM COMMAND DETECTION

The router must also detect and handle the following reserved system commands:

| Command Input               | Action                                                             |
|-----------------------------|--------------------------------------------------------------------|
| `"reset"` / `"clear"`       | Clear session state in `memory-controller.skill`. Restart.        |
| `"stop"` / `"cancel"`       | Halt current active workflow. Prompt for next action.             |
| `"undo"`                    | Display: "Cannot undo completed skill executions. Show results?"  |
| `"status"`                  | Query `memory-controller.skill`. Display completed tasks.         |
| `"what have you done?"`     | Same as `"status"`.                                               |
| `"start over"`              | Confirm with user, then clear session and restart.                |

---

### ▸ COLD START BEHAVIOR

When the router is invoked for the very first time in a session with no prior context:

1. Do NOT display a welcome message unless the user's input is completely ambiguous.
2. If input is specific → parse and route immediately.
3. If input is vague → display Nexus Task Menu.
4. Never auto-run any skill on cold start without a specific intent being resolved.

---

### ▸ PARTIAL SUBJECT HANDLING

When a user provides a subject that is incomplete or underspecified:

| Subject State                        | Router Action                                           |
|--------------------------------------|---------------------------------------------------------|
| `"keyword research"` (no keyword)    | Route to `keyword-discovery.skill` but flag: "No seed keyword provided — skill will prompt user." |
| `"analyze my site"` (no URL)         | Route to `website-analysis.skill` but flag: "No URL provided — skill will prompt user." |
| `"write an article"` (no topic)      | Route to `article-blueprint.skill` but flag: "No topic provided — skill will prompt user." |

- Do NOT ask for the missing subject yourself. Let the target skill handle its own input collection.
- The router's job is to route — not to gather all parameters before routing.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Deduplication in `task-router.skill` operates at **four distinct levels**.

### Level 1 — Task-Level Deduplication

- Before routing any task, check if the same skill has already been executed with the same primary subject in the current session.
- If yes: notify user, do not re-execute without explicit confirmation.
- Storage: via `memory-controller.skill` → key: `[skill_name]:[primary_subject]:[session_id]`

### Level 2 — Intent-Level Deduplication

- When a multi-intent query is decomposed, remove any intent that is a semantic subset of another already-resolved intent.
- Example: If `"SERP analysis"` is already in the queue, do not also add `"who ranks for this keyword"` as a separate intent — they resolve to the same skill.

### Level 3 — Routing-Level Deduplication

- Do not route to the same skill twice within a single CHAINED execution, unless the subject is materially different.
- Exception: `memory-controller.skill` may explicitly request a re-run for freshness validation.

### Level 4 — Session-Level Deduplication

- At session close, `memory-controller.skill` receives a complete log of all routed tasks.
- On the next session start, if the user provides the same input as a prior session's primary subject, the router presents the prior result and asks: "Would you like to use the existing result or run a fresh analysis?"
- Do NOT silently use cached results.
- Do NOT silently re-run without informing the user.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The following behaviors are **strictly prohibited** in `task-router.skill`:

### Prohibited Output Patterns

| Prohibited Behavior                                            | Reason                                              |
|----------------------------------------------------------------|-----------------------------------------------------|
| Saying "Great! Let me help you with that."                     | Filler. Wastes tokens. Adds no information.         |
| Saying "SEO is important for…"                                 | Unsolicited generic advice. Not the router's role.  |
| Explaining what SEO is                                         | Not requested. Not relevant. Not the router's role. |
| Suggesting what the user "might want to try"                   | Router does not speculate. It routes or asks.       |
| Adding bullet points of general SEO tips after routing         | Irrelevant. Output scope is routing only.           |
| Prepending "Sure!" or "Of course!" to any response            | Filler. Prohibited in all Nexus skill outputs.      |
| Auto-describing menu options without being asked               | Menu is self-explanatory. Expansion is on-demand only.|
| Producing any SEO analysis output itself                       | The router is not an executor. Forbidden.           |
| Confirming user's input back to them in prose                  | Unnecessary. Route or ask — nothing else.           |
| Adding caveats like "this depends on your industry…"           | Out of scope. Router does not advise.               |

### Required Output Characteristics

- **Every router output must be deterministic**: same input → same routing decision.
- **Every routing message must be structured**: use the defined packet formats exactly.
- **Every clarification must use the exact Nexus menu format**: no improvisation.
- **Every post-task prompt must be the exact two-line format**: no additions.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

`core/task-router.skill` is the **hub** of the Nexus system. All integrations are listed below with their direction, trigger condition, and data contract.

---

### ▸ memory-controller.skill

| Property           | Detail                                                                 |
|--------------------|------------------------------------------------------------------------|
| **Direction**      | Bidirectional (router → memory, memory → router)                       |
| **Trigger**        | On every invocation (Step 1 and Step 4)                                |
| **Router sends**   | Session ID, current intent, primary subject, target skill              |
| **Memory returns** | Prior execution flag, session context, deduplication verdict           |
| **Used for**       | Session retrieval, task-level dedup, session-level dedup, post-task log|

---

### ▸ execution-mode.skill

| Property           | Detail                                                                 |
|--------------------|------------------------------------------------------------------------|
| **Direction**      | Router → execution (one-way handoff)                                   |
| **Trigger**        | After all steps 1–7 clear and scope is confirmed                       |
| **Router sends**   | Full execution instruction packet (defined in Step 7)                  |
| **Execution returns** | Completion signal only (no data passed back through router)         |
| **Used for**       | All single, chained, and full-system skill execution                   |

---

### ▸ nexus-connector.skill

| Property           | Detail                                                                 |
|--------------------|------------------------------------------------------------------------|
| **Direction**      | Router → nexus-connector (conditional routing)                         |
| **Trigger**        | When scope = FULL-SYSTEM and user confirms "YES"                       |
| **Router sends**   | Full-system execution instruction with primary subject and session ID  |
| **Used for**       | Orchestrating the complete 8+ skill sequential SEO workflow            |
| **Note**           | Router does NOT directly control execution in full-system mode — nexus-connector.skill takes over |

---

### ▸ keyword-discovery.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = keyword research                       |
| **Parameters**     | seed_keyword, modifiers, scope                  |

---

### ▸ serp-analysis.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = SERP analysis                          |
| **Parameters**     | target_keyword, depth (standard / deep)         |

---

### ▸ deep-serp-analysis.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = deep SERP / SERP patterns              |
| **Parameters**     | target_keyword, signal_types                    |

---

### ▸ website-analysis.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = site audit / analyze website           |
| **Parameters**     | site_url (may be NULL — skill will prompt)      |

---

### ▸ gap-opportunity-engine.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = content gap analysis                   |
| **Parameters**     | site_url or domain, competitor URLs (optional)  |

---

### ▸ opportunity-scoring.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = opportunity analysis / low-competition wins |
| **Parameters**     | keyword_list or topic_cluster                   |

---

### ▸ authority-mapping.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = link profile / authority / domain strength |
| **Parameters**     | site_url                                        |

---

### ▸ roadmap-engine.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = content roadmap / publishing plan      |
| **Parameters**     | topic_clusters, opportunity_scores, time_horizon|

---

### ▸ article-blueprint.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = article outline / blueprint / structure |
| **Parameters**     | target_keyword, article_type, SERP signals      |

---

### ▸ content-optimizer.skill

| Property           | Detail                                          |
|--------------------|-------------------------------------------------|
| **Direction**      | Routing target only                             |
| **Trigger**        | Intent = optimize existing content / on-page SEO improvements |
| **Parameters**     | existing_content_URL or pasted content          |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — ROUTING DECISION FLOWCHART (TEXT)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
USER INPUT RECEIVED
        │
        ▼
[Step 1] Query memory-controller.skill
        │
        ├── Prior session active? ──── YES ──► Load context ──► Continue
        │
        ▼
[Step 2] Parse raw input
        │
        ├── Multi-intent? ──── TRUE ──► Flag MULTI_INTENT ──► Continue
        │
        ▼
[Step 3] Detect intent + map to skill
        │
        ├── Confidence = HIGH ──────────────────────────────► Step 4
        ├── Confidence = MEDIUM ──► Flag for user confirm ──► Step 4
        └── Confidence = LOW ────► CLARIFICATION GATE ──► Display Menu ──► Wait
                                                                │
                                                          User responds
                                                                │
                                                          Re-enter Step 2
        ▼
[Step 4] Memory deduplication check
        │
        ├── Not duplicate ──────────────────────────────────► Step 5
        └── Duplicate detected ──► Notify user ──► Require confirmation ──► Step 5 or STOP
        │
        ▼
[Step 5] Validate scope
        │
        ├── SINGLE ──────────────────────────────────────────► Step 7
        ├── CHAINED ──► Build queue ──► Present to user ──► Confirm ──► Step 7
        └── FULL-SYSTEM ──► Full-system gate ──► Confirm ──► nexus-connector.skill
        │
        ▼
[Step 7] Build execution packet ──► Send to execution-mode.skill
        │
        ▼
[Step 8] Receive completion signal
        │
        ▼
Display: "Task complete. Do you want to perform another task?"
        │
        ├── YES ──► Return to Step 1
        ├── NO  ──► Display session summary ──► Close
        └── New task described ──► Return to Step 2
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — INTENT SYNONYM NORMALIZATION TABLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| User Says                              | Normalized Intent              | Target Skill                     |
|----------------------------------------|--------------------------------|----------------------------------|
| "find me keywords"                     | keyword research               | keyword-discovery.skill          |
| "what keywords should I target"        | keyword research               | keyword-discovery.skill          |
| "keyword ideas for X"                  | keyword research               | keyword-discovery.skill          |
| "who ranks for X"                      | SERP analysis                  | serp-analysis.skill              |
| "what's on the first page for X"       | SERP analysis                  | serp-analysis.skill              |
| "SERP breakdown"                       | SERP analysis                  | serp-analysis.skill              |
| "ranking signals for X"                | deep SERP analysis             | deep-serp-analysis.skill         |
| "what makes pages rank for X"          | deep SERP analysis             | deep-serp-analysis.skill         |
| "audit my site"                        | website analysis               | website-analysis.skill           |
| "check my website"                     | website analysis               | website-analysis.skill           |
| "what's wrong with my site"            | website analysis               | website-analysis.skill           |
| "what topics am I missing"             | content gap analysis           | gap-opportunity-engine.skill     |
| "competitor content gaps"              | content gap analysis           | gap-opportunity-engine.skill     |
| "what should I write about"            | content gap / opportunity      | gap-opportunity-engine.skill     |
| "best opportunities for me"            | opportunity scoring            | opportunity-scoring.skill        |
| "easy wins"                            | opportunity scoring            | opportunity-scoring.skill        |
| "low competition topics"               | opportunity scoring            | opportunity-scoring.skill        |
| "backlink profile"                     | authority mapping              | authority-mapping.skill          |
| "domain authority check"               | authority mapping              | authority-mapping.skill          |
| "link building opportunities"          | authority mapping              | authority-mapping.skill          |
| "content plan"                         | content roadmap                | roadmap-engine.skill             |
| "publishing schedule"                  | content roadmap                | roadmap-engine.skill             |
| "what should I publish next"           | content roadmap                | roadmap-engine.skill             |
| "outline for an article"              | article blueprint              | article-blueprint.skill          |
| "article structure for X"              | article blueprint              | article-blueprint.skill          |
| "how should I structure this post"     | article blueprint              | article-blueprint.skill          |
| "improve my article"                   | content optimization           | content-optimizer.skill          |
| "on-page SEO for this page"            | content optimization           | content-optimizer.skill          |
| "make this content rank better"        | content optimization           | content-optimizer.skill          |
| "do everything"                        | full system                    | nexus-connector.skill            |
| "full SEO strategy"                    | full system                    | nexus-connector.skill            |
| "complete analysis"                    | full system                    | nexus-connector.skill            |

---

*SKILL FILE END: core/task-router.skill*
*Nexus SEO Operating System — Version 1.0.0*
