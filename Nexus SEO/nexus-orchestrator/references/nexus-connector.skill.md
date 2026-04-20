# SKILL: core/nexus-connector.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Core / Orchestration
# EXECUTION PRIORITY: ENTRY POINT — This skill is the primary entry point for all Nexus workflows. It receives user input and orchestrates the complete execution pipeline across all other skills.
# POSITION: Central orchestration layer. Every user-initiated request flows through this skill. It determines what to run, in what order, and how to combine the results.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **central orchestration engine** of the Nexus SEO Operating System.

The Nexus system comprises 37 specialized skills — each one an expert in a specific domain of SEO intelligence. Each skill is precise, deep, and production-ready. But a system of 37 independent skills is not a system — it is a collection of tools. What makes those tools into a unified operating system is the orchestration layer that knows which tools to run for which request, in what sequence, with what dependencies, with what data shared between them, and how to combine their outputs into a single coherent, hierarchically structured final deliverable.

This skill is that orchestration layer.

When a user provides a keyword, a domain, a topic, or a content request, this skill activates. It reads the execution mode, checks memory for prior context, classifies the request type, builds the appropriate execution pipeline from the available skills, runs that pipeline in dependency-correct order, deduplicates outputs across skills, merges all results into a unified structured document, appends the strategic synthesis, persists all outputs to memory, and delivers the final package to the output formatter.

The user never needs to know which of the 37 skills ran. They never need to configure the pipeline. They never need to manage dependencies. They provide a request — and this skill produces a complete, unified, multi-layer SEO intelligence output that reflects the work of every relevant skill in the system.

### Primary Responsibilities

| Responsibility                         | Description                                                                                                              |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| **Context Initialization**             | Call `memory-controller.skill` to check for prior project context; offer continue/restart decision                       |
| **Execution Mode Resolution**          | Determine FULL / PARTIAL / SINGLE execution scope from `execution-mode.skill`                                           |
| **Request Classification**             | Classify the user request as keyword-based, domain-based, content-creation, or mixed — and route accordingly            |
| **Dynamic Pipeline Construction**      | Build the exact ordered pipeline of skills required for the classified request type and execution mode                   |
| **Dependency Resolution**              | Ensure every skill in the pipeline receives its required inputs from upstream skills before it executes                  |
| **Ordered Execution Management**       | Run all pipeline skills in their correct dependency order; pass outputs between skills correctly                         |
| **No-Duplicate Execution Enforcement** | Prevent any skill from being called more than once per pipeline run; reuse cached outputs                               |
| **Output Deduplication**               | Remove overlapping insights and duplicate findings before merging skill outputs                                          |
| **Unified Output Construction**        | Merge all skill outputs into a single, hierarchically structured, section-labelled final report                         |
| **Strategic Synthesis Append**         | Call `strategist-ai.skill` to add the executive strategic brief to the final output                                     |
| **Memory Persistence**                 | Store all keywords, content, roadmap items, and strategic decisions to memory via `memory-controller.skill`             |
| **Output Dispatch**                    | Pass the final unified report to `output-formatter.skill` for formatting and delivery                                   |
| **Pipeline Transparency**              | Display pipeline execution progress to the user — which skills are running, which are complete, what was skipped       |
| **Clarification Management**           | When input is ambiguous, ask the user for clarification before executing — never guess at the user's intent             |

### What This Skill Is NOT

- It is **not** a skill that does analysis itself. It orchestrates other skills — it does not perform keyword research, SERP analysis, or content generation directly.
- It is **not** a rigid pipeline executor. It builds the pipeline dynamically based on request type — different inputs produce different pipelines.
- It is **not** a replacement for running individual skills directly. Advanced users can still invoke any skill independently. This connector is for unified full-strategy workflows.
- It is **not** responsible for the quality of individual skill outputs. Each skill is responsible for its own output quality; this connector is responsible for the pipeline architecture.
- It is **not** a single-use tool. It can be re-run at any point to update the strategy as new content is published or the competitive landscape changes.

### The Foundational Principle of This Skill

> The Nexus system is only as useful as its orchestration.
>
> A system where the user must manually determine which of 37 skills to run,
> in what order, with what dependencies, for what type of request
> is a system that requires the user to already be an expert.
>
> This skill removes that requirement.
> The user provides the signal. This skill builds the system.
>
> One request in. One complete, unified SEO intelligence output out.
> Every relevant skill executed. Every output combined.
> Every dependency resolved. Every result persisted.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                       | Description                                                                                      | Required |
|-----------------------------|--------------------------------------------------------------------------------------------------|----------|
| `user_request`              | The user's raw input — a keyword, domain, topic, content idea, or strategy request              | YES      |
| `execution_mode`            | FULL / PARTIAL / SINGLE — from `execution-mode.skill` or user-specified directly               | YES (default: FULL) |
| `memory_context`            | Prior project data from `memory-controller.skill` — if session has prior context               | AUTO-LOADED |

### User Request Format Types

The connector accepts requests in any of these formats — classification happens in Step 3:

| Format Type                        | Example Input                                                                          |
|------------------------------------|----------------------------------------------------------------------------------------|
| **Single keyword**                 | `"best crm software"`                                                                  |
| **Topic / idea**                   | `"write a guide on CRM for startups"`                                                 |
| **Domain analysis request**        | `"analyze my site: example.com"`                                                       |
| **Competitor research**            | `"research my competitors in the CRM space"`                                          |
| **Full strategy request**          | `"build a complete SEO strategy for crm-software.com"`                               |
| **Content creation request**       | `"create an article targeting 'crm software for startups'"`                           |
| **Optimization request**           | `"optimize my article at example.com/crm-guide"`                                     |
| **Gap analysis request**           | `"find content gaps on my site for the CRM topic"`                                   |
| **Roadmap request**                | `"build a 6-month content roadmap for CRM topics"`                                   |
| **Single skill invocation**        | `"run keyword discovery for 'crm software'"` (SINGLE mode)                           |

### Optional Context Inputs

| Field                       | Description                                                                       | Used For                                             |
|-----------------------------|-----------------------------------------------------------------------------------|------------------------------------------------------|
| `target_audience`           | Audience description for content-related requests                                 | Passed to content pipeline skills                   |
| `funnel_stage`              | TOFU / MOFU / BOFU preference                                                     | Passed to relevant pipeline skills                  |
| `word_count`                | Target word count for content generation requests                                 | Passed to `content-generation-engine.skill`         |
| `competitors`               | Competitor URLs for analysis                                                      | Passed to `competitor-analysis.skill`               |
| `source_of_truth`           | URLs or documents for accuracy checking                                           | Passed to `technical-accuracy-checker.skill`        |
| `priority_clusters`         | User-designated clusters to prioritize                                            | Passed to `roadmap-engine.skill`                    |
| `planning_horizon`          | Strategy planning horizon in months                                               | Passed to `roadmap-engine.skill`                    |
| `production_capacity`       | Content items per month                                                           | Passed to `roadmap-engine.skill`                    |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Unified SEO Intelligence Report

```
NEXUS UNIFIED SEO INTELLIGENCE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Request                 : "[user request]"
Request Type            : [KEYWORD / DOMAIN / CONTENT / STRATEGY / OPTIMIZATION / MIXED]
Execution Mode          : [FULL / PARTIAL / SINGLE]
Skills Executed         : [count]
Skills Skipped          : [count — with reason]
Memory Context          : [NEW SESSION / CONTINUED — session N]
Total Runtime           : [processing summary]
Generated By            : core/nexus-connector.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PIPELINE EXECUTION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Transparent skill-by-skill execution record]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 1 — STRATEGIC EXECUTIVE BRIEF
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: strategist-ai.skill — Strategic Intelligence Brief]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 2 — KEYWORD INTELLIGENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: keyword-discovery.skill + semantic-clustering.skill + deduplication-engine.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 3 — SERP INTELLIGENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: serp-intelligence.skill + serp-filter.skill + deep-serp-analysis.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 4 — CONTENT BLUEPRINT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: content-pattern-extractor.skill + serp-blueprint-generator.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 5 — AUTHORITY AND GAP ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: authority-engine.skill + gap-opportunity-engine.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 6 — OPPORTUNITY INTELLIGENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: opportunity-engine.skill + query-graph.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 7 — CONTENT ROADMAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: roadmap-engine.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 8 — CONTENT OUTPUT (if content was generated)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: content-generation-engine.skill → humanizer.skill → technical-accuracy-checker.skill → content-validation.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 9 — METADATA AND KEYWORD PACKAGE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: metadata-generator.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 10 — INTERNAL LINKING PLAN (if applicable)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[From: internal-linking.skill]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MEMORY SAVED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Confirmation of what was persisted to memory]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Pipeline Execution Log Format

```
PIPELINE EXECUTION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Request Type    : [type]
Execution Mode  : [mode]
Pipeline Built  : [count] skills

✓ [01] memory-controller.skill       — Context loaded (prior session: YES/NO)
✓ [02] execution-mode.skill          — Mode: [FULL/PARTIAL/SINGLE]
✓ [03] serp-intelligence.skill       — Intent: [type] | Difficulty: [level]
✓ [04] serp-filter.skill             — Filtered: [N] of [N] results
✓ [05] deep-serp-analysis.skill      — Analyzed [N] pages
✓ [06] content-pattern-extractor.skill — [N] Must-Have patterns detected
✓ [07] keyword-discovery.skill       — [N] keywords discovered
✓ [08] semantic-clustering.skill     — [N] clusters built
✓ [09] entity-extraction.skill       — [N] entities mapped
✓ [10] deduplication-engine.skill    — [N] duplicates removed
✓ [11] serp-blueprint-generator.skill — Blueprint: [N] sections
✓ [12] metadata-generator.skill      — Metadata package complete
  [13] website-analysis.skill        — SKIPPED (no domain input)
  [14] content-inventory.skill       — SKIPPED (no domain input)
  [15] content-awareness.skill       — SKIPPED (no domain input)
✓ [16] gap-opportunity-engine.skill  — [N] gaps detected
✓ [17] authority-engine.skill        — Domain score: [N/5]
✓ [18] query-graph.skill             — Graph: [N] nodes, [N] edges
✓ [19] opportunity-engine.skill      — [N] opportunities scored
✓ [20] roadmap-engine.skill          — [N]-month roadmap built
✓ [21] internal-linking.skill        — [N] links recommended
  [22] content-generation-engine.skill — SKIPPED (not content request)
  [23] humanizer.skill               — SKIPPED (no content generated)
  [24] technical-accuracy-checker.skill — SKIPPED (no content generated)
  [25] content-validation.skill      — SKIPPED (no content generated)
✓ [26] strategist-ai.skill           — Strategic brief complete
✓ [27] memory-controller.skill       — All outputs saved
  [28] output-formatter.skill        — Formatting and delivery
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Skills Executed: [N] | Skills Skipped: [N] | Total: [N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — INITIALIZE CONTEXT

Before any skills are invoked, establish the session context from memory and confirm the user's intent for this session.

**Phase 1A — Memory Check**

Call `memory-controller.skill` with:
- Domain extracted from the user request (if present).
- Keyword extracted from the user request (if present).
- Session lookup: is there prior project context for this domain/keyword?

Memory lookup outcomes:

```
MEMORY CHECK RESULT OPTIONS:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
A) NO PRIOR SESSION:
   → New session. All skills will run fresh.
   → No confirmation needed. Proceed to Step 2.

B) PRIOR SESSION FOUND (within freshness threshold — 14 days):
   → Display summary:
     "Found prior session for [domain/keyword]:
      - Keywords: [N] discovered
      - Clusters: [N] built
      - Roadmap: [N] items scheduled
      - Last run: [date]
      Continue from prior session (C) or start fresh (F)?"
   → Wait for user response.
   → If C: load prior data; use delta mode for all skills
   → If F: clear prior context; run fresh

C) PRIOR SESSION FOUND (stale — beyond 14 days):
   → Display summary with freshness flag:
     "Prior session found for [domain/keyword] but it is [N] days old.
      SERP data and opportunity scores may be outdated.
      Refresh all data (R) or use prior data (U)?"
   → Wait for user response.
   → If R: run fresh with prior memory as reference
   → If U: load prior data; flag all outputs as USING CACHED DATA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 1B — Context Loading**

If continuing a prior session:
1. Load all relevant prior outputs into the session context.
2. Record which data is fresh (within 14 days) vs. potentially stale.
3. Mark stale data sources with CACHE-STALE flag — they will be re-run if in the pipeline.
4. Fresh data sources will be reused without re-running — they are marked CACHE-HIT.

**Phase 1C — Session Record Initialization**

Create the session record:

```
SESSION RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID      : [generated UUID]
Request         : "[raw user input]"
Session Type    : [NEW / CONTINUE / REFRESH]
Domain          : [extracted or null]
Keyword         : [extracted or null]
Prior Context   : [none / loaded — session N, date]
Cache Hits      : [list of fresh data sources reused]
Stale Sources   : [list of data sources marked for refresh]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — DETERMINE EXECUTION MODE

Resolve the execution mode for this session from `execution-mode.skill` or from user-provided mode specification.

**Phase 2A — Mode Source Resolution**

Execution mode is determined in this priority order:
1. User explicitly specifies mode in their request ("run a FULL analysis", "just do SINGLE keyword discovery").
2. `execution-mode.skill` has been called prior to the connector and has set a mode.
3. Request type inference (Phase 3A) suggests a mode.
4. Default: FULL mode.

**Phase 2B — Execution Mode Definitions**

```
EXECUTION MODES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FULL MODE:
  → Run the complete pipeline for the request type
  → All relevant skills execute
  → Full unified report produced
  → Use for: full strategy requests; new domain analysis; complete content planning
  → Typical pipeline: 18–28 skills

PARTIAL MODE:
  → Run a specific subset of the pipeline
  → User specifies which layers to include
  → Partial report produced with clearly labeled scope
  → Use for: quick keyword check; SERP-only analysis; content only
  → Typical pipeline: 5–12 skills
  → Requires user to specify which sections are needed

SINGLE MODE:
  → Run exactly one skill
  → Output from that skill only
  → No merging; no synthesis
  → Use for: specific tool invocation; testing; targeted lookup
  → Pipeline: 1 skill + memory-controller
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 2C — PARTIAL Mode Scope Definition**

When PARTIAL mode is selected, the user must specify which pipeline sections to include. Present the section menu:

```
PARTIAL MODE — SELECT SECTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Which sections do you need? (enter numbers, separated by commas)

[1] Keyword Intelligence     (keyword-discovery + clustering)
[2] SERP Intelligence        (serp-intelligence + deep-serp-analysis)
[3] Content Blueprint        (pattern-extractor + blueprint-generator)
[4] Authority & Gap Analysis (authority-engine + gap-opportunity)
[5] Opportunity Scoring      (opportunity-engine + query-graph)
[6] Content Roadmap          (roadmap-engine)
[7] Content Generation       (full content pipeline)
[8] Metadata Package         (metadata-generator)
[9] Internal Linking         (internal-linking)
[10] Strategic Summary       (strategist-ai — add to any selection)

Enter selection: _
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 2D — SINGLE Mode Skill Selection**

When SINGLE mode is selected, identify which skill the user intends:

| User Intent Signal                                  | Skill to Invoke                          |
|-----------------------------------------------------|------------------------------------------|
| "keyword discovery," "find keywords"               | `keyword-discovery.skill`               |
| "cluster keywords," "group keywords"               | `semantic-clustering.skill`             |
| "check SERP," "SERP analysis"                      | `serp-intelligence.skill`               |
| "analyze this page," "deep SERP"                   | `deep-serp-analysis.skill`              |
| "content blueprint," "build structure"             | `serp-blueprint-generator.skill`        |
| "find opportunities," "score opportunities"        | `opportunity-engine.skill`              |
| "content gaps," "gap analysis"                     | `gap-opportunity-engine.skill`          |
| "authority check," "authority map"                 | `authority-engine.skill`                |
| "build roadmap," "content plan"                    | `roadmap-engine.skill`                  |
| "write content," "create article"                  | `content-generation-engine.skill`       |
| "optimize article," "improve content"              | `optimization-engine.skill`             |
| "internal links," "link architecture"              | `internal-linking.skill`                |
| "strategic summary," "SEO strategy"               | `strategist-ai.skill`                   |

---

### ▸ STEP 3 — INPUT UNDERSTANDING AND REQUEST CLASSIFICATION

Analyze the user's raw input and classify it into a request type that determines which pipeline template to use.

**Phase 3A — Request Type Classification**

Analyze the user_request against these classification rules:

| Request Type              | Signals in Input                                                                          | Primary Pipeline          |
|---------------------------|-------------------------------------------------------------------------------------------|---------------------------|
| **KEYWORD-BASED**         | Input is a keyword phrase (2–6 words, no domain, no URL); OR "find keywords for [topic]" | Keyword pipeline          |
| **DOMAIN-BASED**          | Input contains a domain name (example.com) or URL; OR "analyze my site"                 | Domain pipeline           |
| **CONTENT-CREATION**      | Input asks to "write," "create," "generate," or "draft" specific content                 | Content pipeline          |
| **CONTENT-OPTIMIZATION**  | Input asks to "optimize," "improve," or "refresh" a specific URL or piece               | Optimization pipeline     |
| **STRATEGY-FULL**         | Input asks for "complete strategy," "full SEO strategy," "content roadmap"              | Strategy pipeline (all)   |
| **GAP-ANALYSIS**          | Input asks for "gaps," "what am I missing," "content opportunities"                     | Domain + Gap pipeline     |
| **COMPETITOR-RESEARCH**   | Input mentions competitors explicitly or asks "how do I beat [competitor]"              | Competitor + SERP pipeline|
| **MIXED**                 | Input combines signals (e.g., "analyze my site and write a guide on [keyword]")         | Composite pipeline (built dynamically) |

**Phase 3B — Domain and Keyword Extraction**

Extract the key entities from the request:

```
INPUT EXTRACTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Primary Entity Type  : [keyword / domain / URL / topic]
Keyword(s) extracted : [extracted keywords or null]
Domain extracted     : [extracted domain or null]
URL extracted        : [extracted URL or null]
Topic extracted      : [extracted topic description or null]
Intent signals       : [write / analyze / find / optimize / compare]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3C — Ambiguity Resolution**

If the request type cannot be confidently classified → ask for clarification before building the pipeline:

```
CLARIFICATION NEEDED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
I want to make sure I run the right workflow for your request.
Are you looking to:

[A] Discover and analyze keywords for "[extracted keyword/topic]"
[B] Analyze an existing website or domain
[C] Create new content (article, guide, comparison)
[D] Optimize existing content
[E] Build a complete SEO strategy (keywords + content plan + roadmap)

Type A, B, C, D, or E:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Never guess at the user's intent and run the wrong pipeline. A misclassified request wastes pipeline execution and produces irrelevant output.

---

### ▸ STEP 4 — BUILD EXECUTION PIPELINE

Based on the classified request type and execution mode, construct the ordered list of skills to execute.

**Phase 4A — Pipeline Templates**

**PIPELINE TEMPLATE 1 — KEYWORD-BASED REQUEST:**

```
KEYWORD PIPELINE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAYER 1 — SERP INTELLIGENCE (foundation):
  [01] serp-intelligence.skill          (target keyword → intent + difficulty + SERP features)
  [02] serp-filter.skill                (raw SERP → clean editorial set)
  [03] deep-serp-analysis.skill         (filtered SERP → 8-dimension page analysis)

LAYER 2 — PATTERN EXTRACTION:
  [04] content-pattern-extractor.skill  (SERP analysis → Must-Have patterns)
  [05] entity-extraction.skill          (keyword + SERP → entity map)

LAYER 3 — KEYWORD INTELLIGENCE:
  [06] keyword-discovery.skill          (target keyword → full universe, 8 dimensions)
  [07] semantic-clustering.skill        (keyword universe → cluster architecture)
  [08] deduplication-engine.skill       (cluster output → deduplicated keyword set)

LAYER 4 — BLUEPRINT AND METADATA:
  [09] serp-blueprint-generator.skill   (patterns + SERP + keyword → 9-part blueprint)
  [10] metadata-generator.skill         (keyword + blueprint + SERP → full metadata package)

LAYER 5 — OPPORTUNITY ANALYSIS:
  [11] gap-opportunity-engine.skill     (clusters + SERP + content-awareness → gap ideas)
  [12] opportunity-engine.skill         (clusters + SERP + gaps → scored keywords)
  [13] query-graph.skill                (clusters → topic relationship graph)

LAYER 6 — STRATEGY:
  [14] roadmap-engine.skill             (gaps + opportunities + authority → roadmap)
  [15] strategist-ai.skill              (all outputs → strategic brief)

LAYER 7 — PERSISTENCE:
  [16] memory-controller.skill          (write all outputs to memory)
  [17] output-formatter.skill           (unified output → formatted delivery)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PIPELINE TEMPLATE 2 — DOMAIN-BASED REQUEST:**

```
DOMAIN PIPELINE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAYER 1 — SITE INTELLIGENCE:
  [01] website-analysis.skill           (domain → URL inventory + topic map)
  [02] content-inventory.skill          (site URLs → normalized content records)
  [03] content-awareness.skill          (inventory → coverage map + freshness flags)

LAYER 2 — KEYWORD ARCHITECTURE:
  [04] semantic-clustering.skill        (content topics → cluster architecture)
  [05] entity-extraction.skill          (clusters + content → entity map)
  [06] deduplication-engine.skill       (clusters → deduplicated keyword set)

LAYER 3 — SERP INTELLIGENCE (for top clusters):
  [07] serp-intelligence.skill          (primary cluster keywords → SERP data)
  [08] serp-filter.skill                (SERP → clean editorial set)
  [09] deep-serp-analysis.skill         (filtered SERP → competitive analysis)

LAYER 4 — AUTHORITY AND GAPS:
  [10] authority-engine.skill           (inventory + clusters → authority map)
  [11] gap-opportunity-engine.skill     (coverage + clusters + SERP → gap ideas)
  [12] competitor-analysis.skill        (SERP + domain → competitor intelligence)

LAYER 5 — OPPORTUNITY ANALYSIS:
  [13] opportunity-engine.skill         (clusters + SERP + gaps → scored keywords)
  [14] query-graph.skill                (clusters → topic graph)

LAYER 6 — ARCHITECTURE:
  [15] internal-linking.skill           (inventory + clusters → link architecture)

LAYER 7 — STRATEGY:
  [16] roadmap-engine.skill             (all analysis → content roadmap)
  [17] strategist-ai.skill              (all outputs → strategic brief)

LAYER 8 — PERSISTENCE:
  [18] memory-controller.skill          (write)
  [19] output-formatter.skill           (format + deliver)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PIPELINE TEMPLATE 3 — CONTENT CREATION REQUEST:**

```
CONTENT PIPELINE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAYER 1 — PRE-GENERATION INTELLIGENCE:
  [01] serp-intelligence.skill
  [02] serp-filter.skill
  [03] deep-serp-analysis.skill
  [04] content-pattern-extractor.skill
  [05] keyword-discovery.skill
  [06] semantic-clustering.skill
  [07] entity-extraction.skill
  [08] deduplication-engine.skill
  [09] gap-opportunity-engine.skill
  [10] serp-blueprint-generator.skill
  [11] pre-execution-input.skill         (blueprint + user config → final execution brief)
  [12] metadata-generator.skill

LAYER 2 — CONTENT GENERATION:
  [13] content-generation-engine.skill   (calls all above + generates full article)

LAYER 3 — POST-GENERATION PROCESSING:
  [14] humanizer.skill                   (article → natural language)
  [15] technical-accuracy-checker.skill  (article → accuracy verified)
  [16] content-validation.skill          (article → final quality gate)

LAYER 4 — STRATEGY (optional — if FULL mode):
  [17] roadmap-engine.skill
  [18] strategist-ai.skill

LAYER 5 — PERSISTENCE:
  [19] memory-controller.skill
  [20] output-formatter.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PIPELINE TEMPLATE 4 — CONTENT OPTIMIZATION REQUEST:**

```
OPTIMIZATION PIPELINE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAYER 1 — CONTEXT GATHERING:
  [01] serp-intelligence.skill          (target keyword → current SERP state)
  [02] entity-extraction.skill          (keyword → entity map for comparison)

LAYER 2 — OPTIMIZATION ANALYSIS:
  [03] optimization-engine.skill        (existing content + keyword + SERP → optimization plan)

LAYER 3 — IMPLEMENTATION (if rewrites requested):
  [04] humanizer.skill                  (rewrite suggestions → natural language)
  [05] technical-accuracy-checker.skill (rewrites → accuracy verified)
  [06] content-validation.skill         (rewrites → validated)

LAYER 4 — PERSISTENCE:
  [07] memory-controller.skill
  [08] output-formatter.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PIPELINE TEMPLATE 5 — FULL STRATEGY REQUEST:**

```
FULL STRATEGY PIPELINE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
All layers from DOMAIN PIPELINE (if domain provided)
PLUS all layers from KEYWORD PIPELINE
PLUS optional CONTENT PIPELINE (if user requests a piece)
PLUS roadmap-engine.skill
PLUS strategist-ai.skill
Total range: 20–32 skills depending on what is provided
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 4B — Dynamic Pipeline Modification**

After selecting the base template, modify the pipeline based on:

1. **Cache hits:** If a skill's output is already in memory and is fresh → mark as CACHE-HIT; skip the skill; use cached output.
2. **Missing inputs:** If a skill requires an input that is not available (no domain for website-analysis, no competitor URLs for competitor-analysis) → mark as SKIPPED-NO-INPUT; continue with remaining pipeline.
3. **Execution mode restrictions:** In PARTIAL mode → remove all skills not in the user-selected sections.
4. **Priority cluster constraints:** If user-designated priority clusters exist → ensure roadmap and opportunity skills receive this as a parameter.
5. **Production capacity:** Pass to roadmap-engine if specified.

**Phase 4C — Pipeline Validation**

Before execution begins, validate the pipeline:

1. Are all hard dependencies met? For every skill in the pipeline → verify its required inputs come from an upstream skill also in the pipeline.
2. Are there any circular dependencies? → Detect and flag.
3. Are there skills in the pipeline that overlap in scope for this request? → Remove the lower-specificity one.
4. Display the final pipeline to the user (Pipeline Execution Log header).

---

### ▸ STEP 5 — SKILL EXECUTION

Execute all pipeline skills in the correct dependency order.

**Phase 5A — Execution Order Enforcement**

The pipeline is executed in the layer-by-layer sequence defined in Step 4. Within each layer:
1. Skills within the same layer that have no dependency on each other may be executed in parallel.
2. Skills that depend on outputs from skills in the same layer must wait for those outputs.
3. No skill in Layer N may begin until all skills in Layer N-1 are complete.

**Phase 5B — Output Passing Protocol**

As each skill completes, its output is:
1. Stored in the session output cache, indexed by skill name.
2. Passed as input to the next skill(s) that require it.
3. NOT re-processed or re-generated — the same output object is referenced by all downstream skills.

Output passing is explicit — every skill invocation includes the specific output objects from upstream skills that it requires. Implicit data sharing is prohibited: if a skill needs data from `serp-intelligence.skill`, the `serp-intelligence.skill` output object must be explicitly passed.

**Phase 5C — Execution Status Tracking**

For every skill in the pipeline, track:

| Status           | Meaning                                                          |
|------------------|------------------------------------------------------------------|
| **PENDING**      | Skill is in the pipeline; waiting for upstream skills to complete |
| **RUNNING**      | Skill is currently executing                                     |
| **COMPLETE**     | Skill has finished; output is available                          |
| **CACHE-HIT**    | Skill was skipped; cached output from prior session used        |
| **SKIPPED**      | Skill was removed from pipeline (no input available / PARTIAL mode / not needed) |
| **FAILED**       | Skill encountered an error — failsafe applied                   |

Update the Pipeline Execution Log in real time as each skill transitions through statuses.

**Phase 5D — No-Duplicate Execution Rule**

Each skill may be called AT MOST ONCE per pipeline run. If two different downstream skills would both trigger the same upstream skill → the connector calls it once and passes the single output to both downstream skills.

Example: Both `gap-opportunity-engine.skill` and `opportunity-engine.skill` require `semantic-clustering.skill` output. The connector calls `semantic-clustering.skill` once and passes its output to both downstream callers.

**Phase 5E — Graceful Degradation**

If a skill fails:
1. Apply the skill's documented failsafe (SERP inference, default values, etc.).
2. Mark the skill as FAILED with the fallback noted.
3. Continue pipeline execution — do not halt the entire pipeline for a single skill failure.
4. The final report marks any section affected by a failed skill with: `"[REDUCED CONFIDENCE — [skill] failed; fallback applied]"`.

---

### ▸ STEP 6 — DEDUPLICATION LAYER

After all pipeline skills have completed, apply a cross-skill deduplication pass to the combined output before merging.

**Phase 6A — Cross-Skill Deduplication**

Scan all skill outputs for overlapping content:

| Overlap Type                                                                     | Resolution                                                          |
|----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Same keyword recommendation appears in both `keyword-discovery` and `gap-opportunity-engine` | Keep the version with richer metadata; note dual-source confirmation |
| Same content idea appears in both `gap-opportunity-engine` and `opportunity-engine` | Merge into single record with combined scoring data                |
| Same structural recommendation in both `serp-blueprint-generator` and `deep-serp-analysis` | Merge; the blueprint's version takes precedence (it integrates deep analysis) |
| Same strategic insight in both `authority-engine` and `strategist-ai`           | Strategist-ai version takes precedence (it synthesizes the authority data) |
| Same internal link recommendation in both `serp-blueprint-generator` and `internal-linking` | Internal-linking version takes precedence (it has the full inventory context) |

**Phase 6B — Insight Hierarchy**

When the same finding appears in multiple skills, the higher-layer skill's version is authoritative:

| Layer Priority (Highest → Lowest)                                                         |
|-------------------------------------------------------------------------------------------|
| 1. `strategist-ai.skill` — synthesizes everything; always authoritative for strategy     |
| 2. `roadmap-engine.skill` — authoritative for sequencing and timing                      |
| 3. `authority-engine.skill` — authoritative for cluster authority state                  |
| 4. `opportunity-engine.skill` — authoritative for keyword opportunity scores             |
| 5. `gap-opportunity-engine.skill` — authoritative for gap-specific content ideas         |
| 6. `serp-blueprint-generator.skill` — authoritative for content structure                |
| 7. `deep-serp-analysis.skill` — authoritative for per-page competitive intelligence      |
| 8. `serp-intelligence.skill` — authoritative for SERP-level signals                     |

---

### ▸ STEP 7 — COMBINE OUTPUTS

Merge all deduplicated skill outputs into the unified hierarchical report structure.

**Phase 7A — Section Assignment**

Map every skill's output to its report section:

| Report Section                    | Contributing Skills                                                                    |
|-----------------------------------|----------------------------------------------------------------------------------------|
| Section 1 — Strategic Brief       | `strategist-ai.skill`                                                                 |
| Section 2 — Keyword Intelligence  | `keyword-discovery.skill`, `semantic-clustering.skill`, `deduplication-engine.skill` |
| Section 3 — SERP Intelligence     | `serp-intelligence.skill`, `serp-filter.skill`, `deep-serp-analysis.skill`          |
| Section 4 — Content Blueprint     | `content-pattern-extractor.skill`, `serp-blueprint-generator.skill`                  |
| Section 5 — Authority & Gaps      | `authority-engine.skill`, `gap-opportunity-engine.skill`, `website-analysis.skill`   |
| Section 6 — Opportunity Intel     | `opportunity-engine.skill`, `query-graph.skill`                                       |
| Section 7 — Content Roadmap       | `roadmap-engine.skill`                                                                 |
| Section 8 — Content Output        | `content-generation-engine.skill` → `humanizer.skill` → `technical-accuracy-checker.skill` → `content-validation.skill` |
| Section 9 — Metadata Package      | `metadata-generator.skill`                                                             |
| Section 10 — Internal Linking     | `internal-linking.skill`                                                               |

**Phase 7B — Section Presence Logic**

A section is included in the final report only if the skill(s) that produce it were executed. If a section's contributing skills were all skipped → the section is omitted and a note is added to the report header: `"Section [N] omitted — [skills] not executed in this run."`

**Phase 7C — Hierarchical Structure Enforcement**

The final report follows the section order defined in the output template. Sections are never reordered — the Strategic Brief always leads, even though `strategist-ai.skill` runs last in the pipeline. The connector reorders sections for presentation independently of execution order.

**Phase 7D — Cross-Reference Insertion**

After all sections are assembled, insert cross-references between related findings across sections:

Examples:
- In Section 2 (Keyword Intelligence): `"See Section 6 (Opportunity Scores) for the ranked priority of these keywords."`
- In Section 4 (Content Blueprint): `"This blueprint's keyword targets are scored in Section 6. HIGH-scoring keywords are bolded."`
- In Section 7 (Roadmap): `"Each roadmap item's gap evidence is detailed in Section 5."`

---

### ▸ STEP 8 — STRATEGIC SUMMARY APPEND

Call `strategist-ai.skill` to produce the executive strategic brief and place it as Section 1 of the unified report.

**Phase 8A — Strategist Input Assembly**

Compile the `strategist-ai.skill` input package from all completed pipeline outputs:
- Roadmap output from `roadmap-engine.skill`.
- Opportunity data from `opportunity-engine.skill`.
- Authority map from `authority-engine.skill`.
- Gap analysis from `gap-opportunity-engine.skill`.
- SERP intelligence from `serp-intelligence.skill` (if available).
- Memory context from `memory-controller.skill` (prior session data).

**Phase 8B — Strategist Output Placement**

The Strategic Brief produced by `strategist-ai.skill` is placed at the TOP of the unified report (Section 1) — not the bottom. The strategic brief is what the decision-maker reads first. The detailed analytical sections (2–10) are the supporting evidence.

---

### ▸ STEP 9 — SAVE TO MEMORY

After the unified output is assembled and the strategist brief is appended, persist all outputs to memory via `memory-controller.skill`.

**Phase 9A — What Gets Saved**

| Memory Category              | What Is Saved                                                                    | Memory Layer               |
|------------------------------|----------------------------------------------------------------------------------|----------------------------|
| Keywords                     | Full keyword universe, cluster assignments, opportunity scores                   | keyword-memory.skill       |
| Content                      | Generated articles, metadata packages, validation reports                       | content-memory.skill       |
| Strategy                     | Roadmap, strategic brief, action plans, 90-day projections                      | strategy-memory.skill      |
| Project context              | Domain, session ID, request type, pipeline executed, skills run                 | project-memory.skill       |
| Decisions                    | User inputs to clarification prompts, execution mode choices, configuration     | project-memory.skill       |

**Phase 9B — Memory Status Confirmation**

After saving, display a memory confirmation in the report:

```
MEMORY SAVED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ✓ [N] keywords saved to keyword-memory
  ✓ [N] content records saved to content-memory
  ✓ Roadmap saved to strategy-memory ([N] items)
  ✓ Strategic brief saved to strategy-memory
  ✓ Session context saved to project-memory
  Session available for continuation as: Session [N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 10 — FINAL OUTPUT

Dispatch the completed unified report to `output-formatter.skill` for formatting and delivery.

**Phase 10A — Pre-Dispatch Validation**

Before dispatching to the output formatter:
1. Verify the report has a minimum of Section 1 (Strategic Brief) and at least two analytical sections.
2. Verify all executed skills are represented in the report.
3. Verify the Pipeline Execution Log is complete and accurate.
4. Verify no section has unresolved placeholder text.

**Phase 10B — Output Formatter Invocation**

Pass to `output-formatter.skill`:
- The complete unified report document.
- The session metadata (session ID, request, pipeline executed).
- The output format preference (if user specified: markdown / structured text / clean document).

**Phase 10C — Final Delivery**

`output-formatter.skill` returns the formatted final document. The connector presents it to the user.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ DYNAMIC PIPELINE GENERATION

The connector does not use rigid pipeline templates exclusively. It modifies and extends them dynamically based on the specific inputs and context available.

**Dynamic Pipeline Rules:**

| Condition                                                                              | Dynamic Modification                                                   |
|----------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Domain provided AND keyword provided                                                  | Merge domain pipeline layers 1–2 with keyword pipeline layers 1–3; combine at SERP intelligence layer |
| Content creation requested AND domain provided                                        | Run domain pipeline first to populate content awareness; then full content pipeline |
| Competitor URLs provided                                                               | Insert `competitor-analysis.skill` after `serp-intelligence.skill`    |
| Source of truth provided                                                               | Pass to `technical-accuracy-checker.skill` explicitly                 |
| Multiple keywords provided (batch)                                                    | Run `semantic-clustering.skill` across all; then single `serp-intelligence` per top cluster only |
| SINGLE mode with a skill that has upstream dependencies                              | Add the minimum required upstream skills; do not run the full pipeline, but satisfy the target skill's dependencies |

**Dynamic Pipeline Visualization:**

Before execution, display a visual pipeline summary:

```
DYNAMIC PIPELINE BUILT FOR THIS REQUEST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Request Type    : [type]
Base Template   : [template name]
Modifications   : [list of dynamic additions/removals]
Final Pipeline  : [N] skills in [N] layers
Estimated Cache-Hits: [N] (from prior session)
Ready to execute? (Y to proceed / M to modify)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ DEPENDENCY RESOLUTION

The connector maintains a complete dependency graph of all 37 Nexus skills and uses it to resolve execution order automatically.

**Dependency Graph (simplified):**

```
CORE DEPENDENCIES:
  serp-intelligence → serp-filter → deep-serp-analysis
  deep-serp-analysis → content-pattern-extractor → serp-blueprint-generator
  keyword-discovery → semantic-clustering → deduplication-engine
  semantic-clustering → [entity-extraction | gap-opportunity-engine | authority-engine]
  [all analysis skills] → opportunity-engine → roadmap-engine → strategist-ai

DOMAIN DEPENDENCIES:
  website-analysis → content-inventory → content-awareness → [authority-engine | gap-opportunity-engine]

CONTENT DEPENDENCIES:
  [all research skills] → pre-execution-input → content-generation-engine
  content-generation-engine → humanizer → technical-accuracy-checker → content-validation

MEMORY DEPENDENCIES:
  [any skill] → memory-controller (read/write at start and end)
```

**Dependency Resolution Algorithm:**

1. For each skill in the pipeline, load its declared input requirements.
2. For each required input — is it produced by a skill already in the pipeline?
3. If YES → verify the producing skill is scheduled before the consuming skill.
4. If NO → the producing skill is a missing dependency — add it to the pipeline in the correct position.
5. Re-run the dependency check after every addition.
6. Repeat until no missing dependencies exist.

---

### ▸ EXECUTION OPTIMIZATION

Maximize pipeline efficiency by identifying what can be parallelized and what can be skipped.

**Parallelization Opportunities:**

| Parallel Group                                                             | Skills That Can Run Simultaneously                |
|----------------------------------------------------------------------------|---------------------------------------------------|
| After `serp-intelligence` completes                                        | `serp-filter` and `keyword-discovery` (no dependency between them) |
| After `semantic-clustering` completes                                      | `entity-extraction` and `gap-opportunity-engine` (no dependency between them) |
| After `serp-blueprint-generator` completes                                 | `metadata-generator` and `opportunity-engine` (no dependency between them) |
| After content pipeline completes                                           | `humanizer` and `technical-accuracy-checker` on separate passes (then validate combined) — NOTE: humanizer must complete BEFORE accuracy checker |

**Skip Optimization Rules:**

| Skip Condition                                                              | Skip Action                                                          |
|-----------------------------------------------------------------------------|----------------------------------------------------------------------|
| A skill's output is in the session cache and within freshness threshold    | Skip the skill; use cached output; record as CACHE-HIT               |
| A skill requires a domain but no domain is in the request                 | Skip with SKIPPED-NO-INPUT                                           |
| A skill is in the pipeline but its output section was not selected (PARTIAL mode) | Skip with SKIPPED-PARTIAL-MODE                               |
| A skill would produce output that is entirely deduplicated by a higher-layer skill | Skip with SKIPPED-REDUNDANT (rare; only when skill provides zero unique content) |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Skill Execution

Each skill executes exactly once per pipeline run. If the dynamic pipeline builder adds the same skill twice for different reasons → it is executed once; both triggering flows receive the same output.

### Rule 2 — No Duplicate Result Objects

If two skills produce a result object for the same entity (e.g., both `keyword-discovery` and `gap-opportunity-engine` produce a record for the same keyword) → merge the records into one, combining all fields. The merged record is used in the unified report.

### Rule 3 — No Redundant Sections in Output

If a section would contain only content already fully expressed in Section 1 (Strategic Brief) → suppress the redundant section and add a note: "This section's findings are synthesized in the Strategic Brief (Section 1)."

### Rule 4 — No Re-Running of Cache-Hit Skills

A skill marked CACHE-HIT must not be re-run unless the user explicitly requests a refresh. The connector must distinguish between "I want new analysis" (run the skill) and "I want to continue the session" (use cached data). Ambiguous requests are clarified before execution.

### Rule 5 — No Cross-Session Duplication

Skills that produced output in a prior session and have not had their underlying conditions change (same keyword, same domain, fresh cache) are always CACHE-HIT in the new session unless the user requested a refresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Orchestration Behaviors

| Prohibited Behavior                                                                              | Reason                                                                            |
|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Running skills that are not required for the request type                                        | Over-processing produces slower results and irrelevant sections                  |
| Running `content-generation-engine.skill` without explicit content creation intent from the user | Content generation is a significant resource investment — never inferred          |
| Running the full strategy pipeline (20+ skills) for a SINGLE-mode request                      | Execution mode constraints must be respected                                      |
| Proceeding without clarification when request type is ambiguous                                 | A wrong pipeline wastes all execution resources                                   |
| Including a report section for a skill that was not executed                                    | Empty sections or placeholder sections degrade the report's utility               |
| Calling `strategist-ai.skill` before all other pipeline skills complete                        | Strategist requires all analysis data — partial input produces a partial strategy|
| Saving to memory before the output is validated                                                 | Memory should only contain verified, complete outputs                            |
| Displaying the Pipeline Execution Log after the final report rather than before it             | Users need to see pipeline status before the report — it provides context        |
| Running `deduplication-engine.skill` without passing it the full pipeline output pool         | Deduplication requires the complete output set to do its job                     |
| Truncating any skill's output when merging into the unified report                             | Every skill's output must be fully preserved in its assigned section             |

### Required Orchestration Behaviors

- Always check memory before executing any skills.
- Always display the pipeline before execution begins.
- Always ask for clarification when request type is ambiguous.
- Always apply the no-duplicate-execution rule before finalizing the pipeline.
- Always place the Strategic Brief (Section 1) first in the output, regardless of when `strategist-ai.skill` ran.
- Always confirm memory save completion before dispatching to output formatter.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ memory-controller.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — called twice: READ at Step 1 (context init), WRITE at Step 9 (persistence)  |
| **Step 1 READ**  | Check for prior project context; load fresh data; flag stale data                           |
| **Step 9 WRITE** | Save all pipeline outputs — keywords, content, roadmap, decisions, session metadata         |
| **Critical role**| The memory layer is what makes the Nexus system a persistent intelligence platform, not a one-shot tool |

---

### ▸ execution-mode.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream configuration input                                                                 |
| **Called at**    | Step 2 (before pipeline construction)                                                        |
| **Receives**     | FULL / PARTIAL / SINGLE mode specification                                                   |
| **Pipeline impact** | Mode directly determines which pipeline template is used and which sections are included  |

---

### ▸ task-router.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream routing layer (may call the connector)                                              |
| **Relationship** | `task-router.skill` is the entry point before `nexus-connector.skill` — it decides whether to invoke the connector or invoke a specific skill directly |
| **When bypassed**| SINGLE mode requests may bypass the connector entirely and go directly to the target skill  |

---

### ▸ All 37 Nexus Skills

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | All skills are downstream consumers — called by the connector                               |
| **Relationship** | Every skill in the Nexus system is invocable by this connector                              |
| **Authority**    | The connector manages execution order, dependency resolution, and output routing for all skills |
| **Independence** | Skills may still be called independently for single-skill use cases — the connector does not monopolize them |

---

### ▸ output-formatter.skill

| Property         | Detail                                                                                        |
|------------------|-----------------------------------------------------------------------------------------------|
| **Direction**    | nexus-connector → output-formatter (final dispatch)                                         |
| **Sends**        | Complete unified report + session metadata + format preference                              |
| **Called at**    | Step 10 (final output dispatch — the last action of every pipeline run)                    |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `user_request` is empty or null                                           | Input validation — null field                          | Do not begin pipeline. Display: "Please provide a keyword, domain, topic, or request to begin."         |
| Request type cannot be classified after clarification prompt              | Step 3C — user response is still ambiguous             | Default to KEYWORD-BASED pipeline if a keyword-like string is present; otherwise halt and ask again.    |
| `memory-controller.skill` is unavailable                                  | Step 1 — memory call fails                            | Proceed as NEW SESSION. Flag: "Memory unavailable — session will not be persisted. All outputs will be delivered in this session only." |
| A pipeline skill fails and has no documented failsafe                    | Step 5D — FAILED status with no fallback              | Mark skill as FAILED-NO-FALLBACK. Omit its section from the report with note: "Section omitted — [skill] failed without fallback data." Continue pipeline. |
| All pipeline skills fail (catastrophic failure)                          | Step 5 — all skills FAILED                            | Halt. Report: "Pipeline execution failed entirely — no output can be generated. Check skill configurations and data sources." |
| `strategist-ai.skill` fails to run                                        | Step 8 — strategist fails                             | Produce the report without Section 1. Add prominently: "Strategic Brief unavailable — strategist-ai.skill failed. All analysis sections are present. Run strategist-ai.skill independently for the brief." |
| `output-formatter.skill` fails                                            | Step 10 — formatter fails                             | Deliver the raw unified report directly without formatting. Flag: "Output formatter unavailable — delivering unformatted report." |
| User requests a skill not in the Nexus skill library                     | Step 2D / Step 4A — skill name not recognized         | Flag: "Skill '[name]' not found in the Nexus system. Available skills: [list the 37 skill names]. Please specify a valid skill." |
| Dynamic pipeline builds an empty pipeline (no skills qualify)            | Phase 4C — 0 skills after dynamic modification        | Ask user: "Your request + current filters would result in an empty pipeline. Please confirm: (A) Run the default FULL pipeline; (B) Modify the request." |
| Memory session conflict (same domain being processed by two sessions)    | Step 1 — memory detects concurrent session flag      | Flag: "A session for [domain] is already in progress. Continue this session (C) or create a new independent session (N)?" |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — PIPELINE TEMPLATE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PIPELINE TEMPLATES — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KEYWORD PIPELINE (17 skills):
  → Use for: keyword research, topic analysis, blueprint + content planning
  → Core: serp-intel → filter → deep-serp → pattern → kw-discovery → clustering
    → dedup → blueprint → metadata → gaps → opportunity → graph → roadmap → strategist

DOMAIN PIPELINE (19 skills):
  → Use for: site audit, content gap analysis, authority mapping
  → Core: website-analysis → inventory → awareness → clustering → entity → dedup
    → serp-intel → filter → deep-serp → authority → gaps → competitor → opportunity
    → graph → internal-linking → roadmap → strategist

CONTENT PIPELINE (20 skills):
  → Use for: full content creation with research
  → Core: [all keyword pipeline layers 1–3] + blueprint + pre-execution + metadata
    → content-generation → humanizer → accuracy-checker → validation
    → (optional: roadmap + strategist)

OPTIMIZATION PIPELINE (8 skills):
  → Use for: improving existing content
  → Core: serp-intel → entity → optimization-engine → (humanizer → accuracy → validation)

FULL STRATEGY PIPELINE (20–32 skills):
  → Use for: complete SEO operating strategy
  → Combines domain + keyword + (optional content) + roadmap + strategist

PARTIAL PIPELINE (user-defined):
  → Use for: targeted analysis of specific sections
  → Layers selected by user from the section menu

SINGLE SKILL (1–3 skills):
  → Target skill + minimum dependencies + memory
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — REQUEST CLASSIFICATION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
REQUEST CLASSIFICATION — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KEYWORD-BASED signals:
  → 2–6 word phrase, no domain, no URL
  → "keyword," "topic," "phrase," "search term"
  → "what should I write about [topic]"
  Pipeline: Keyword Pipeline

DOMAIN-BASED signals:
  → Contains .com / .io / .co / domain TLD
  → "my site," "my website," "analyze"
  → URL pattern
  Pipeline: Domain Pipeline

CONTENT-CREATION signals:
  → "write," "create," "draft," "generate," "produce"
  → "article," "guide," "comparison," "post"
  Pipeline: Content Pipeline

OPTIMIZATION signals:
  → "optimize," "improve," "refresh," "update," "fix"
  → URL of an existing page
  Pipeline: Optimization Pipeline

FULL STRATEGY signals:
  → "full strategy," "complete SEO," "content roadmap"
  → "plan," "strategy," "6-month," "annual"
  Pipeline: Full Strategy Pipeline

GAP ANALYSIS signals:
  → "gaps," "missing," "what am I missing," "opportunities"
  → "what should I cover," "content I don't have"
  Pipeline: Domain Pipeline (emphasis on gap layers)

COMPETITOR signals:
  → "competitors," "beat [competitor]," "vs [competitor]"
  → Competitor domain name in input
  Pipeline: Keyword + Competitor extension

AMBIGUOUS — ask before proceeding
  → Any input that matches 2+ signal categories
  → Any input with fewer than 3 words and no context
  → Any input that is a question without a clear topic
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — EXECUTION MODE DECISION GUIDE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
EXECUTION MODE DECISION GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FULL MODE — When to use:
  ✓ First time analyzing a domain or keyword
  ✓ Building a complete content strategy
  ✓ Monthly strategy refresh
  ✓ When the user needs everything in one output
  ✓ When all strategy layers are needed for decision-making
  Skills: 17–32 depending on pipeline template

PARTIAL MODE — When to use:
  ✓ Quick keyword check without full strategy
  ✓ Checking SERP status for a specific keyword
  ✓ Updating only the roadmap after new content is published
  ✓ When the user already has some analysis and needs specific additions
  ✓ Time-constrained analysis
  Skills: 5–12 depending on selected sections

SINGLE MODE — When to use:
  ✓ Testing a specific skill's output
  ✓ Quick targeted lookup (just keyword discovery / just SERP)
  ✓ Re-running one skill after its data becomes stale
  ✓ Power users who know exactly which skill they need
  Skills: 1–3 (target + minimum dependencies + memory)

DEFAULT: FULL MODE
  When mode is not specified → default to FULL MODE.
  Always display the pipeline and allow the user to modify it before execution.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: core/nexus-connector.skill*
*Nexus SEO Operating System — Version 1.0.0*
