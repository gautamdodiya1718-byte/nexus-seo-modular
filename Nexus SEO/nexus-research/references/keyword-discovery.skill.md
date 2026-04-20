# SKILL: research/keyword-discovery.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / Keyword Intelligence
# EXECUTION PRIORITY: FOUNDATIONAL RESEARCH — Runs before clustering, SERP analysis, and all content workflows.
# POSITION: First active research skill in all content and strategy pipelines.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **keyword intelligence engine** of the Nexus system.

It does not generate keyword lists. It constructs a **complete, strategic keyword universe** — a structured, multi-dimensional, intent-mapped, deduplicated body of keyword intelligence that reflects every meaningful search opportunity available for a given topic, niche, or domain.

Every keyword produced by this skill must exist for a defined strategic reason. Generic keywords, redundant keywords, and keywords that have already been addressed in existing content are categorically excluded. The output is not volume — it is precision.

### Primary Responsibilities

| Responsibility                      | Description                                                                                               |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **Memory Verification**             | Check what keyword research already exists before generating a single new keyword                        |
| **Anchor Definition**               | Identify the core concept, the problem being solved, and the target audience before expansion begins     |
| **Multi-Dimensional Expansion**     | Generate keywords across 8 distinct semantic and intent-based dimensions without overlap between them    |
| **Intent Classification**           | Classify every keyword by funnel stage — TOFU, MOFU, or BOFU — with explicit reasoning                 |
| **Deduplication**                   | Remove exact, semantic, intent-level, memory-level, and pipeline-level duplicates before output          |
| **Categorization**                  | Assign every keyword a structural category that drives downstream clustering behavior                    |
| **Strategic Justification**         | For every keyword: articulate why it matters, what gap it fills, and what funnel value it provides      |
| **Coverage Validation**             | Ensure every keyword cluster contains representation at all three funnel stages before output            |
| **Breadth Scoping**                 | Detect when a primary keyword is too broad and narrow it into actionable sub-topics                      |

### What This Skill Is NOT

- It is **not** a volume-maximizer. Quality and strategic value outweigh quantity at every step.
- It is **not** a keyword scraper. It does not retrieve live search volume data — it generates intent-mapped keyword intelligence.
- It is **not** a replacement for `semantic-clustering.skill`. It produces pre-clustered groups as a structural hint, not final clusters.
- It is **not** optional in the pipeline. Every content and strategy workflow that touches keyword targeting must begin with this skill's output.
- It is **not** a generic suggestion engine. Every keyword produced must survive the deduplication gate and the strategic justification requirement.

### The Defining Standard of This Skill

> Every keyword in the output must satisfy three conditions simultaneously:
> 1. It has not appeared before in this project's memory.
> 2. It serves a distinct search intent not already covered by another keyword in this output.
> 3. It can be justified with a specific strategic reason tied to the keyword's funnel position and gap value.
>
> If a keyword cannot satisfy all three conditions, it is excluded — regardless of how relevant it seems.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Required Input

| Field             | Description                                                                       | Format                          |
|-------------------|-----------------------------------------------------------------------------------|---------------------------------|
| `primary_input`   | The seed from which the keyword universe is built                                 | Keyword, topic phrase, or niche |

### Optional Inputs

| Field                  | Description                                                                  | Used For                                            |
|------------------------|------------------------------------------------------------------------------|-----------------------------------------------------|
| `domain`               | The website this keyword research serves                                     | Memory lookup, existing content exclusion           |
| `existing_content`     | List of article titles or URLs already published on the domain               | Memory exclusion, gap identification                |
| `target_audience`      | Audience level or persona (Beginner / Expert / Business)                     | Dimension 5 (Long-Tail), Dimension 8 (Niche Mods)  |
| `niche_modifiers`      | Industry, vertical, or use-case constraints                                  | Dimension 8 refinement                              |
| `excluded_keywords`    | Any keywords the user explicitly wants excluded                              | Appended to deduplication exclusion list            |
| `competitor_domains`   | Competitor sites for semantic gap analysis (if provided by user or memory)   | Semantic expansion reference                        |

### Input Forms Accepted

| Input Form                               | Example                                                         |
|------------------------------------------|-----------------------------------------------------------------|
| Single head term                         | `"CRM software"`                                                |
| Topic phrase                             | `"project management for remote teams"`                         |
| Niche descriptor                         | `"B2B SaaS marketing"`                                          |
| Problem statement                        | `"why my website isn't ranking"`                               |
| Broad category                           | `"digital marketing"` (triggers breadth scoping — see Step 2B) |
| Execution packet from execution-mode.skill| Structured packet with `subject` field containing keyword input |

### Input Pre-Conditions

Before proceeding to Step 1, verify:

1. **Is `primary_input` present?** If not → halt and request it.
2. **Is `primary_input` a navigational query?** (e.g., `"[Brand] login"`) → Flag as low SEO value. Warn user. Ask to confirm or provide alternative.
3. **Is `primary_input` excessively broad?** (e.g., `"marketing"`, `"business"`, `"technology"`) → Trigger breadth scoping before expansion (Step 2B).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Structured Keyword Intelligence Table

The primary output is a structured table with one row per keyword. Every row is fully populated — no field may be left empty.

```
NEXUS KEYWORD INTELLIGENCE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Primary Input       : [seed keyword / topic / niche]
Anchor Concept      : [core concept identified in Step 2]
Target Audience     : [audience identified in Step 2]
Total Keywords      : [count]
Deduplication Runs  : [count of duplicates removed]
Coverage Status     : [FULL / PARTIAL — details]
Generated By        : research/keyword-discovery.skill
Timestamp           : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

KEYWORD INTELLIGENCE TABLE

| #  | Keyword                              | Intent        | Funnel | Category          | Strategic Reason                                      |
|----|--------------------------------------|---------------|--------|-------------------|-------------------------------------------------------|
| 1  | [keyword]                            | Informational | TOFU   | Educational       | [why it matters / gap it fills / funnel value]        |
| 2  | [keyword]                            | Commercial    | MOFU   | Comparison        | [why it matters / gap it fills / funnel value]        |
| 3  | [keyword]                            | Transactional | BOFU   | Problem-Solution  | [why it matters / gap it fills / funnel value]        |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DIMENSION COVERAGE SUMMARY

| Dimension                  | Keywords Generated | Notes                              |
|----------------------------|--------------------|------------------------------------|
| 1. Problem-Based           | [count]            |                                    |
| 2. How-To                  | [count]            |                                    |
| 3. Comparison              | [count]            |                                    |
| 4. Alternatives            | [count]            |                                    |
| 5. Long-Tail               | [count]            |                                    |
| 6. Questions (AEO)         | [count]            |                                    |
| 7. Semantic Expansion      | [count]            |                                    |
| 8. Niche Modifiers         | [count]            |                                    |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FUNNEL COVERAGE SUMMARY

| Funnel Stage | Keywords | Coverage Status                    |
|--------------|----------|------------------------------------|
| TOFU         | [count]  | [FULL / THIN — recommendation]     |
| MOFU         | [count]  | [FULL / THIN — recommendation]     |
| BOFU         | [count]  | [FULL / THIN — recommendation]     |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRE-CLUSTER MAP

Cluster 1 — [Cluster Label]
  Keywords: [keyword list]
  Dominant Intent: [TOFU / MOFU / BOFU]
  Funnel Balance: [TOFU: N | MOFU: N | BOFU: N]

Cluster 2 — [Cluster Label]
  Keywords: [keyword list]
  ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
EXCLUDED KEYWORDS LOG

| Excluded Keyword             | Exclusion Reason                          |
|------------------------------|-------------------------------------------|
| [keyword]                    | Memory duplicate — found in session [ID]  |
| [keyword]                    | Semantic duplicate of #[row number]       |
| [keyword]                    | Intent duplicate of #[row number]         |
...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Field Definitions

| Field              | Definition                                                                                   |
|--------------------|----------------------------------------------------------------------------------------------|
| `Keyword`          | The full, exact keyword phrase as it would be searched                                       |
| `Intent`           | Informational / Commercial / Transactional / Navigational                                    |
| `Funnel`           | TOFU / MOFU / BOFU                                                                           |
| `Category`         | Educational / Comparison / Alternative / Problem-Solution / Long-Tail / AEO / Semantic / Niche |
| `Strategic Reason` | Why this keyword matters, what specific gap it fills, and what it contributes to the funnel  |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — MEMORY CHECK

Before generating a single keyword, retrieve all existing keyword intelligence from `memory-controller.skill`.

**Memory Check Protocol:**

1. Send a READ request to `memory-controller.skill` with:
   - `operation = READ`
   - `memory_layer = keyword`
   - `subject = [primary_input]`
   - `session_id = [current session ID]`
   - `filters = [all lifecycle states]`

2. Receive the memory response. Extract:
   - **Existing keyword list:** All keywords previously generated for this project, regardless of lifecycle state.
   - **Existing content titles:** Published articles that signal covered topics.
   - **Existing cluster labels:** Clusters already built in `semantic-clustering.skill`.
   - **Assigned keywords:** Keywords already assigned to content pieces.
   - **Published keywords:** Keywords for which content is live.

3. Build the **Master Exclusion List** from the memory response:
   - All keywords in any lifecycle state ≥ `DISCOVERED`.
   - All article titles parsed into their implied keywords.
   - All cluster labels parsed into their core keyword phrases.
   - Any keywords passed in via the `excluded_keywords` input parameter.

4. Log the Master Exclusion List:

```
MEMORY CHECK RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Existing Keywords Found : [count]
Assigned Keywords       : [count]
Published Keywords      : [count]
Excluded Content Topics : [count]
Total Exclusions Loaded : [count]
Memory Check Status     : COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

5. If the memory check reveals that keyword research has already been fully completed for the `primary_input`:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Existing Keyword Research Found
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keyword research for "[primary_input]" was completed on [date].
Keywords found: [count] | Assigned: [count] | Published: [count]

Options:
  1  Use existing research
  2  Expand existing research (add new dimensions)
  3  Start fresh (archive existing, generate new set)

Enter choice:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Wait for user response before proceeding.

---

### ▸ STEP 2A — DEFINE ANCHOR

Before any keyword expansion can begin, define the three anchor elements that govern the entire expansion process. These anchors determine the boundaries of the keyword universe and ensure all generated keywords remain purposefully connected to the core strategic goal.

**Anchor Element 1 — Core Concept**

Identify the single most precise description of what the `primary_input` is about.

Rules:
- Strip all adjectives, modifiers, and qualifiers from the `primary_input`.
- Identify the root concept (the noun or noun phrase at the center of the topic).
- The core concept should be expressible in 1–4 words.

| Primary Input                               | Core Concept                  |
|---------------------------------------------|-------------------------------|
| `"best CRM software for small businesses"`  | CRM software                  |
| `"how to improve email open rates"`         | email open rates              |
| `"B2B SaaS marketing strategies"`           | B2B SaaS marketing            |
| `"why my website isn't ranking"`            | website ranking               |
| `"project management for remote teams"`     | project management            |

**Anchor Element 2 — Problem Solved**

Identify the primary problem or pain point that searchers using this keyword universe are trying to resolve.

Rules:
- Frame the problem from the searcher's perspective, not the business's.
- Express it as a frustrated statement or a question the searcher might privately ask.
- The problem statement drives Dimension 1 (Problem-Based) and Dimension 6 (AEO) keywords.

| Core Concept            | Problem Solved                                                         |
|-------------------------|------------------------------------------------------------------------|
| CRM software            | "I can't track my customer relationships efficiently"                  |
| email open rates        | "My emails aren't being opened and I don't know why"                   |
| B2B SaaS marketing      | "I don't know how to generate leads for my SaaS product"               |
| website ranking         | "My website doesn't appear on Google despite my effort"                |
| project management      | "My remote team is disorganized and misses deadlines"                  |

**Anchor Element 3 — Target Audience**

Identify who is searching for this keyword universe. If `target_audience` was provided as input, use it. Otherwise, infer from signals in the `primary_input`.

| Signal in Primary Input                          | Inferred Audience    |
|--------------------------------------------------|----------------------|
| `"for beginners"`, `"intro"`, `"basics"`         | Beginner             |
| `"for developers"`, `"API"`, `"technical"`       | Expert / Developer   |
| `"for small businesses"`, `"SMB"`, `"startup"`   | Business (SMB)       |
| `"enterprise"`, `"large teams"`, `"at scale"`    | Business (Enterprise)|
| `"advanced"`, `"optimize"`, `"master"`           | Intermediate–Expert  |
| No strong signal                                  | Intermediate         |

**Anchor Output:**

```
ANCHOR DEFINITION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Core Concept        : [value]
Problem Solved      : [value]
Target Audience     : [value]
Expansion Scope     : [NARROW / STANDARD / BROAD]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Expansion Scope Classification:**

| Scope      | Condition                                                               | Behavior                                          |
|------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `NARROW`   | Core concept is highly specific (4+ words, niche topic)                 | Expand aggressively into all 8 dimensions         |
| `STANDARD` | Core concept is a defined product, service, or practice                 | Expand fully but apply TF-IDF filtering           |
| `BROAD`    | Core concept is a high-level category (1–2 words, massive search space) | Trigger Step 2B before expansion                  |

---

### ▸ STEP 2B — BREADTH SCOPING (TRIGGERED FOR BROAD INPUTS ONLY)

Activated when `Expansion Scope = BROAD`.

When a `primary_input` is too broad (e.g., `"marketing"`, `"software"`, `"finance"`), unrestricted expansion would produce a keyword universe too vast to be strategically actionable. This step narrows the scope before expansion begins.

**Breadth Scoping Protocol:**

1. Identify the 3–5 most strategically relevant sub-topics of the `primary_input` for the user's context.
2. Present them to the user:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Input Scope Too Broad
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"[primary_input]" covers too large a keyword space for a single research run.

Select a sub-topic to focus on, or enter your own:

  1  [Sub-topic A]
  2  [Sub-topic B]
  3  [Sub-topic C]
  4  [Sub-topic D]
  5  [Sub-topic E]
  6  Enter custom sub-topic

Enter number or describe your focus:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

3. If the user selects a sub-topic → update `primary_input` to the sub-topic → recalculate anchors in Step 2A → proceed to Step 3.
4. If the user enters a custom sub-topic → validate it is a meaningful narrowing → proceed.
5. If the user insists on the broad input → proceed with `BROAD` scope but apply strict TF-IDF simulation in Step 3 (priority on unique, specific terms; head terms are downweighted).

---

### ▸ STEP 3 — MULTI-DIMENSIONAL EXPANSION

Generate keywords across all 8 dimensions. Each dimension targets a distinct facet of the keyword space. Dimensions must not overlap — each keyword belongs to exactly one dimension at generation time (cross-dimension duplicates are removed in Step 5).

The anchor elements from Step 2A govern every dimension. Every keyword generated must be traceable to the core concept, the problem solved, and the target audience.

---

#### ▸ DIMENSION 1 — PROBLEM-BASED KEYWORDS

**Purpose:** Capture searchers at the moment of experiencing the problem. These are high-empathy, high-relevance keywords for content that positions the product or service as the solution.

**Funnel Stage:** TOFU to MOFU (problem recognition → solution seeking)

**Generation Templates:**

| Template                                    | Example (CRM context)                              |
|---------------------------------------------|----------------------------------------------------|
| `"why [problem symptom]"`                   | `"why am I losing track of customer follow-ups"`   |
| `"how to fix [problem]"`                    | `"how to fix poor sales pipeline visibility"`      |
| `"[problem] causes"`                        | `"causes of low CRM adoption rates"`               |
| `"signs of [problem]"`                      | `"signs your sales team needs a CRM"`              |
| `"[problem] vs [goal state]"`               | `"disorganized contacts vs CRM"`                   |
| `"[negative outcome] without [solution]"`   | `"losing deals without a CRM system"`              |
| `"what happens when [problem worsens]"`     | `"what happens when sales pipeline has no system"` |
| `"[audience] struggling with [problem]"`    | `"small business struggling with customer data"`   |

**Generation Rules:**

- Generate at minimum 4, maximum 10 problem-based keywords.
- Every keyword must reflect a real, specific pain point — not a generic frustration.
- Do not use the core concept as the subject of the problem (avoid `"CRM software problems"` — this targets existing users, not prospects).
- Problem-based keywords should evoke the pre-solution state of the searcher.

---

#### ▸ DIMENSION 2 — HOW-TO KEYWORDS

**Purpose:** Capture searchers seeking step-by-step guidance. These are educational, high-traffic TOFU keywords that position content as an authoritative guide.

**Funnel Stage:** TOFU (informational, process-seeking)

**Generation Templates:**

| Template                                        | Example (CRM context)                                 |
|-------------------------------------------------|-------------------------------------------------------|
| `"how to [action]"`                             | `"how to set up a CRM for a small business"`          |
| `"how to [action] without [barrier]"`           | `"how to manage contacts without expensive software"` |
| `"how to [action] step by step"`                | `"how to build a sales pipeline step by step"`        |
| `"how to [action] for [audience]"`              | `"how to use a CRM for freelancers"`                  |
| `"how to [action] using [tool/method]"`         | `"how to track leads using a spreadsheet CRM"`        |
| `"step by step [topic]"`                        | `"step by step CRM implementation guide"`             |
| `"guide to [topic]"`                            | `"beginner's guide to CRM for startups"`              |
| `"[action]: complete guide"`                    | `"setting up sales automation: complete guide"`       |

**Generation Rules:**

- Generate at minimum 5, maximum 12 how-to keywords.
- Every keyword must describe a concrete action — not a concept.
- Vary the action verb across keywords (set up, build, create, manage, track, measure, optimize, implement, configure).
- Include at least one `"how to [action] without [barrier]"` variant — these capture reluctant adopters.

---

#### ▸ DIMENSION 3 — COMPARISON KEYWORDS

**Purpose:** Capture searchers in active evaluation mode. These are MOFU/BOFU keywords with high commercial intent and strong conversion potential.

**Funnel Stage:** MOFU to BOFU (evaluation, decision)

**Generation Templates:**

| Template                                       | Example (CRM context)                              |
|------------------------------------------------|----------------------------------------------------|
| `"[A] vs [B]"`                                 | `"HubSpot vs Salesforce for small business"`       |
| `"[topic] comparison"`                         | `"CRM software comparison 2024"`                   |
| `"best [category] for [audience]"`             | `"best CRM for freelancers"`                       |
| `"[A] or [B]: which is better"`                | `"HubSpot or Pipedrive: which is better for SMBs"` |
| `"top [N] [category]"`                         | `"top 10 CRM tools for sales teams"`               |
| `"[category] ranked"`                          | `"CRM software ranked by ease of use"`             |
| `"[A] vs [B] vs [C]"`                          | `"Salesforce vs HubSpot vs Zoho"`                  |
| `"cheap [category] vs premium [category]"`     | `"free CRM vs paid CRM: what's the difference"`    |

**Generation Rules:**

- Generate at minimum 4, maximum 10 comparison keywords.
- Use real named alternatives where known (major players in the niche). Do NOT use placeholder names.
- Include at least one `"best [category] for [specific audience]"` variant.
- Include at least one price-dimension comparison (`"free vs paid"`, `"cheap vs enterprise"`).
- Do NOT generate comparisons for obscure tools that would not realistically appear in SERP results.

---

#### ▸ DIMENSION 4 — ALTERNATIVE KEYWORDS

**Purpose:** Capture searchers who are dissatisfied with an existing solution and actively seeking a replacement. These keywords signal high purchase intent and low loyalty to incumbents.

**Funnel Stage:** MOFU to BOFU (high conversion potential)

**Generation Templates:**

| Template                                          | Example (CRM context)                               |
|---------------------------------------------------|-----------------------------------------------------|
| `"alternatives to [topic/tool]"`                  | `"alternatives to Salesforce for small business"`   |
| `"[tool] alternative"`                            | `"HubSpot alternative"`                             |
| `"[tool] competitors"`                            | `"Pipedrive competitors"`                           |
| `"[tool] replacement"`                            | `"Zoho CRM replacement"`                            |
| `"cheaper alternative to [tool]"`                 | `"cheaper alternative to Salesforce"`               |
| `"[tool] alternative for [audience]"`             | `"Salesforce alternative for startups"`             |
| `"switch from [tool] to [other]"`                 | `"switch from Zoho to HubSpot"`                     |
| `"[tool] too expensive: options"`                 | `"Salesforce too expensive: alternatives"`          |

**Generation Rules:**

- Generate at minimum 4, maximum 8 alternative keywords.
- Focus on named tools and platforms, not generic categories.
- Include at least one price-motivated alternative keyword (`"cheaper"`, `"free"`, `"affordable"`).
- Include at least one migration or switching keyword (`"switch from"`, `"migrate from"`, `"replace"`).

---

#### ▸ DIMENSION 5 — LONG-TAIL KEYWORDS

**Purpose:** Capture highly specific, low-competition search queries with precise user intent. Long-tail keywords often have lower search volume but significantly higher conversion rates due to their specificity.

**Funnel Stage:** Any — long-tail keywords span all funnel stages depending on specificity

**Generation Templates:**

| Template                                             | Example (CRM context)                                    |
|------------------------------------------------------|----------------------------------------------------------|
| `"[topic] for [specific audience]"`                  | `"CRM software for real estate agents"`                  |
| `"[topic] for [use case]"`                           | `"CRM for managing client onboarding"`                   |
| `"[topic] without [barrier]"`                        | `"CRM without monthly fees"`                             |
| `"[topic] for [audience] with [constraint]"`         | `"CRM for solo consultants with limited budget"`         |
| `"[topic] when [condition]"`                         | `"CRM when your team is under 10 people"`                |
| `"[topic] that [specific capability]"`               | `"CRM that integrates with Gmail"`                       |
| `"[topic] under [price point]"`                      | `"CRM under $50 per month"`                              |
| `"[topic] with [specific feature]"`                  | `"CRM with built-in email marketing"`                    |
| `"[topic] for [industry]"`                           | `"CRM for law firms"`                                    |
| `"[topic] [audience] [year]"`                        | `"best CRM for freelancers [current year]"`              |

**Generation Rules:**

- Generate at minimum 8, maximum 20 long-tail keywords.
- Every long-tail keyword must add a specificity layer not present in any other dimension.
- Vary the specificity modifier: audience, industry, use case, constraint, feature, price point, timeline.
- Include multiple industry verticals where the core concept applies (real estate, legal, healthcare, e-commerce, agency, etc.).
- Do NOT generate long-tail variants that are semantic duplicates of each other (e.g., `"CRM for real estate"` and `"real estate CRM"` are duplicates — keep only one).

---

#### ▸ DIMENSION 6 — QUESTION-BASED KEYWORDS (AEO)

**Purpose:** Capture voice search, featured snippet, and Answer Engine Optimization (AEO) opportunities. Question keywords are structured to trigger direct answers in AI-generated results, Google SGE, and People Also Ask sections.

**Funnel Stage:** TOFU (primarily awareness and education)

**Generation Templates:**

| Template                                              | Example (CRM context)                                   |
|-------------------------------------------------------|---------------------------------------------------------|
| `"what is [topic]"`                                   | `"what is a CRM system"`                                |
| `"what does [topic] do"`                              | `"what does CRM software do"`                           |
| `"what is the difference between [A] and [B]"`        | `"what is the difference between CRM and ERP"`          |
| `"when should [audience] use [topic]"`                | `"when should a small business use a CRM"`              |
| `"how does [topic] work"`                             | `"how does CRM software work"`                          |
| `"why do [audience] need [topic]"`                    | `"why do sales teams need a CRM"`                       |
| `"is [topic] worth it"`                               | `"is CRM software worth it for small businesses"`       |
| `"can [audience] use [topic] without [requirement]"`  | `"can a freelancer use a CRM without technical skills"` |
| `"which [category] is best for [audience]"`           | `"which CRM is best for startups"`                      |
| `"does [tool] [specific feature]"`                    | `"does HubSpot CRM have email automation"`              |

**Generation Rules:**

- Generate at minimum 6, maximum 15 question-based keywords.
- Each question must have a specific, answerable form — not open-ended philosophical questions.
- Include at least 2 `"what is the difference between"` variants — these consistently trigger featured snippets.
- Include at least 2 `"is [topic] worth it"` or `"should [audience] use [topic]"` variants — these are high-intent decision validators.
- AEO keywords are formatted as natural language questions exactly as a person would ask them.

---

#### ▸ DIMENSION 7 — SEMANTIC EXPANSION KEYWORDS

**Purpose:** Build topical authority by covering the related concepts, adjacent tools, supporting processes, and contextual terminology that surrounds the core concept. Search engines use semantic relevance to determine topical authority — content that covers the full semantic field around a concept ranks higher than content that only targets isolated keywords.

**Funnel Stage:** Mixed — semantic keywords serve awareness and mid-funnel authority building

**Semantic Expansion Sub-Categories:**

**7A — Related Concepts:**

Keywords representing concepts that are intellectually adjacent to the core concept but distinct enough to merit separate content.

| Example (CRM context)                                    |
|----------------------------------------------------------|
| `"sales pipeline management"`                            |
| `"customer lifecycle management"`                        |
| `"lead nurturing strategies"`                            |
| `"contact management system"`                            |
| `"sales automation tools"`                               |

**7B — Supporting Tools:**

Keywords representing tools, platforms, or technologies that are commonly used alongside or in integration with the core concept.

| Example (CRM context)                                    |
|----------------------------------------------------------|
| `"email marketing integration with CRM"`                 |
| `"CRM and marketing automation"`                         |
| `"CRM with Slack integration"`                           |
| `"CRM reporting dashboard"`                              |
| `"CRM data import from spreadsheet"`                     |

**7C — Supporting Processes:**

Keywords representing workflows, methodologies, or practices that use or relate to the core concept.

| Example (CRM context)                                    |
|----------------------------------------------------------|
| `"sales forecasting process"`                            |
| `"customer segmentation strategy"`                       |
| `"deal tracking workflow"`                               |
| `"client onboarding checklist"`                          |
| `"follow-up email sequence for sales"`                   |

**Generation Rules:**

- Generate at minimum 6, maximum 18 semantic expansion keywords, distributed across all three sub-categories (7A, 7B, 7C).
- Every keyword must be semantically connected to the core concept — not loosely related.
- No keyword in Dimension 7 should be a reformulation of a keyword already generated in Dimensions 1–6.
- Tag each semantic keyword with its sub-category (7A, 7B, or 7C) in the internal generation log for pre-clustering purposes.

---

#### ▸ DIMENSION 8 — NICHE MODIFIER KEYWORDS

**Purpose:** Generate hyper-targeted keywords that apply the core concept to a specific industry, skill level, or use case. These keywords often have very low competition and high conversion rates due to their extreme specificity.

**Funnel Stage:** BOFU to MOFU (high specificity = high intent)

**Niche Modifier Sub-Categories:**

**8A — Industry Modifiers:**

Apply the core concept to specific industries where it is relevant.

| Template                                   | Example (CRM context)                          |
|--------------------------------------------|------------------------------------------------|
| `"[topic] for [industry]"`                 | `"CRM for healthcare providers"`               |
| `"[industry] [topic] software"`            | `"real estate CRM software"`                   |
| `"best [topic] for [industry]"`            | `"best CRM for marketing agencies"`            |

Industries to consider (select the most relevant 3–6):
- Healthcare, Legal, Real Estate, E-commerce, Agency / Marketing, SaaS, Education, Finance, Manufacturing, Non-profit, Consulting

**8B — Skill Level Modifiers:**

Apply the core concept to searchers at different experience levels.

| Template                                   | Example (CRM context)                          |
|--------------------------------------------|------------------------------------------------|
| `"[topic] for beginners"`                  | `"CRM for beginners"`                          |
| `"simple [topic]"`                         | `"simple CRM for small teams"`                 |
| `"advanced [topic] techniques"`            | `"advanced CRM automation techniques"`         |
| `"[topic] without experience"`             | `"how to use a CRM without sales experience"`  |

**8C — Use Case Modifiers:**

Apply the core concept to specific operational scenarios or use cases.

| Template                                   | Example (CRM context)                          |
|--------------------------------------------|------------------------------------------------|
| `"[topic] for [specific task]"`            | `"CRM for cold email outreach"`                |
| `"[topic] for [team size]"`                | `"CRM for teams under 5 people"`               |
| `"[topic] for [business model]"`           | `"CRM for subscription businesses"`            |

**Generation Rules:**

- Generate at minimum 6, maximum 15 niche modifier keywords across all three sub-categories.
- Industry modifiers must be real industries where the core concept has genuine applicability.
- Do NOT generate niche modifiers for industries where the core concept has no realistic use case.
- Skill level modifiers must represent genuinely distinct search intents — `"CRM for beginners"` and `"how to use a CRM without experience"` are distinct; `"CRM for beginners"` and `"beginner CRM guide"` are duplicates.

---

### ▸ STEP 4 — INTENT CLASSIFICATION

After all 8 dimensions have been generated, assign a precise intent classification to every keyword.

**Intent Classification Framework:**

| Intent Type        | Definition                                                                         | Typical Funnel Stage |
|--------------------|------------------------------------------------------------------------------------|----------------------|
| **Informational**  | Searcher wants to learn, understand, or find an answer                            | TOFU                 |
| **Commercial**     | Searcher is researching options and comparing before making a decision             | MOFU                 |
| **Transactional**  | Searcher is ready to take an action (buy, sign up, download, try)                 | BOFU                 |
| **Navigational**   | Searcher is looking for a specific website or brand                               | N/A (flag and exclude unless strategic) |

**Intent Signal Mapping:**

| Signal in Keyword                                    | Intent          | Funnel |
|------------------------------------------------------|-----------------|--------|
| `"what is"`, `"how to"`, `"guide"`, `"learn"`, `"explain"` | Informational | TOFU   |
| `"why"`, `"when should"`, `"do I need"`              | Informational   | TOFU   |
| `"best"`, `"top"`, `"vs"`, `"comparison"`, `"alternatives"`, `"review"` | Commercial | MOFU |
| `"is worth it"`, `"should I"`, `"which is better"`  | Commercial      | MOFU   |
| `"buy"`, `"price"`, `"discount"`, `"free trial"`, `"sign up"`, `"get started"`, `"download"` | Transactional | BOFU |
| `"[brand name]"` alone                               | Navigational    | Flag   |

**Intent Conflict Resolution:**

Some keywords contain signals from multiple intent categories (e.g., `"best CRM software free trial"` contains both Commercial and Transactional signals). Apply the following resolution:

1. Identify all intent signals present.
2. Assign the intent of the **dominant signal** (the signal that most accurately describes the searcher's primary goal at the moment of searching).
3. Log the secondary signal in the internal intent note (used by `semantic-clustering.skill` for nuanced cluster building).

**Funnel Assignment from Intent:**

| Intent          | Funnel Assignment Rule                                                              |
|-----------------|-------------------------------------------------------------------------------------|
| Informational   | TOFU by default. MOFU only if the informational query is clearly comparison-oriented.|
| Commercial      | MOFU by default. BOFU if the commercial signal is accompanied by price or trial intent.|
| Transactional   | BOFU always.                                                                        |
| Navigational    | Exclude from output unless it represents a brand the user owns.                    |

---

### ▸ STEP 5 — DEDUPLICATION (CRITICAL)

This step runs after all 8 dimensions have been generated and before categorization. It is the most critical quality gate in this skill.

**Deduplication Protocol — Five Levels:**

**Level 1 — Exact Duplicate Removal**

- Scan the complete generated keyword list for exact string matches.
- Case-insensitive comparison.
- Remove all but one instance of each exact duplicate.
- Log every removal.

**Level 2 — Semantic Duplicate Removal**

- Apply semantic similarity analysis across all keyword pairs in the generated list.
- Similarity threshold: ≥80% semantic overlap = semantic duplicate.
- Examples of semantic duplicates:
  - `"CRM for real estate"` and `"real estate CRM"` → same meaning, different word order → remove one.
  - `"how to set up a CRM"` and `"CRM setup guide"` → same intent, same topic → remove one.
  - `"best CRM for small businesses"` and `"top CRM tools for SMBs"` → same meaning, different phrasing → remove one.
- When two semantic duplicates are found, keep the one that is more specific, more natural as a search query, or more aligned with a distinct funnel stage.

**Level 3 — Intent Duplicate Removal**

- Two keywords are intent duplicates if they target the same searcher at the same stage of the funnel for the same core information need.
- Intent duplicates may be semantically different but functionally serve the same strategic purpose.
- Examples:
  - `"HubSpot vs Salesforce"` and `"HubSpot or Salesforce: which is better"` → both serve the same comparison intent for the same two tools → remove one.
  - `"CRM software alternatives"` and `"alternatives to CRM platforms"` → same intent, different phrasing → remove one.
- When removing intent duplicates, keep the keyword with the more specific and targeted phrasing.

**Level 4 — Memory Duplicate Removal**

- Cross-reference the complete generated keyword list against the Master Exclusion List built in Step 1.
- Remove every keyword that:
  - Appears verbatim in the Master Exclusion List.
  - Is a semantic duplicate of any keyword in the Master Exclusion List (≥80% similarity).
  - Targets the same intent as a keyword already assigned to published or in-progress content.
- Memory duplicates are never retained — they represent work already done or in progress.

**Level 5 — Pipeline Duplicate Removal**

- Check whether any generated keyword is already present in a planned content pipeline in `strategy-memory.skill`.
- If a keyword is already in a planned piece of content → mark as pipeline duplicate → remove from current output.
- Log pipeline duplicates separately — they confirm the pipeline is on track and do not represent a gap.

**Deduplication Output Log:**

Maintain a complete log of every keyword removed and the reason:

```
DEDUPLICATION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Level 1 — Exact Duplicates Removed    : [count] | Keywords: [list]
Level 2 — Semantic Duplicates Removed : [count] | Keywords: [list with kept variant]
Level 3 — Intent Duplicates Removed   : [count] | Keywords: [list with kept variant]
Level 4 — Memory Duplicates Removed   : [count] | Keywords: [list]
Level 5 — Pipeline Duplicates Removed : [count] | Keywords: [list]
Total Keywords Before Dedup           : [count]
Total Keywords After Dedup            : [count]
Net Deduplication Rate                : [%]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 6 — CATEGORIZATION

Assign a structural category to every keyword that survived deduplication. Categories drive downstream behavior in `semantic-clustering.skill` and `content-generation-engine.skill`.

**Category Definitions and Assignment Rules:**

| Category            | Definition                                                                            | Primary Dimension Source |
|---------------------|---------------------------------------------------------------------------------------|--------------------------|
| `Educational`       | Keyword serves a learning need; content will be instructional                         | Dimensions 2, 6          |
| `Comparison`        | Keyword serves an evaluation need; content will compare options                       | Dimension 3              |
| `Alternative`       | Keyword captures dissatisfied users of a competing solution                           | Dimension 4              |
| `Problem-Solution`  | Keyword captures a pain point; content will frame the solution                        | Dimension 1              |
| `Long-Tail`         | Keyword is highly specific with a narrow, defined audience and use case               | Dimension 5              |
| `AEO`               | Keyword is formatted as a natural language question targeting AI/voice/snippet results | Dimension 6              |
| `Semantic`          | Keyword builds topical authority around adjacent concepts, tools, or processes        | Dimension 7              |
| `Niche`             | Keyword applies the core concept to a specific industry, skill level, or use case     | Dimension 8              |

**Category Assignment Rules:**

1. Every keyword receives exactly one primary category.
2. If a keyword fits two categories, assign it to the category where its strategic value is highest.
3. The category assignment must be consistent with the intent classification — for example, a keyword cannot be categorized as `AEO` if it was classified as `Transactional` intent (AEO keywords are informational by nature).
4. Categories are recorded in the output table and passed to `semantic-clustering.skill` as pre-cluster hints.

---

### ▸ STEP 7 — STRATEGIC REASON

For every keyword that survived deduplication, write a strategic reason statement. This is a mandatory field — no keyword may appear in the output without one.

**Strategic Reason Components:**

Every strategic reason statement must include all three of the following:

| Component          | What It Addresses                                                              |
|--------------------|--------------------------------------------------------------------------------|
| **Why it matters** | What search behavior this keyword represents and why that behavior is valuable  |
| **Gap it fills**   | What content does not yet exist (in this project or on the SERP) that this keyword would create |
| **Funnel value**   | How this keyword moves a user closer to a conversion or desired action         |

**Strategic Reason Quality Standards:**

- Maximum 2 sentences per strategic reason.
- Must be specific to this keyword — not a generic statement applicable to any keyword.
- Must reference either the audience, the funnel stage, or a specific content gap.
- Must NOT be vague filler like `"good for SEO"` or `"high search volume"`.

**Strategic Reason Examples:**

| Keyword                                        | Poor Strategic Reason                 | Good Strategic Reason                                                                                                                 |
|------------------------------------------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `"CRM for law firms"`                          | `"Good for niche traffic"`            | `"Captures a high-intent vertical segment where CRM adoption is low and competition is minimal; converts readers ready to solve a specific legal practice management problem."` |
| `"HubSpot vs Salesforce for small business"`  | `"Comparison content for leads"`      | `"Targets decision-stage buyers comparing the two dominant platforms in the SMB segment; content ranking here intercepts users at maximum purchase intent before they convert elsewhere."` |
| `"why am I losing track of customer follow-ups"` | `"Problem-based, good for TOFU"` | `"Targets a specific, emotionally resonant pain point before the searcher knows they need a CRM; ideal for introducing the solution at the top of the funnel without triggering sales-mode resistance."` |

---

### ▸ STEP 8 — COVERAGE CHECK

Before finalizing the output, validate that the keyword universe provides complete coverage across funnel stages and dimensions.

**Funnel Coverage Validation:**

| Check                                                    | Pass Condition                                | Fail Action                                                   |
|----------------------------------------------------------|-----------------------------------------------|---------------------------------------------------------------|
| TOFU keywords exist                                      | ≥5 TOFU keywords in output                    | Flag as `THIN TOFU` — return to Dimensions 2, 6 and expand   |
| MOFU keywords exist                                      | ≥4 MOFU keywords in output                    | Flag as `THIN MOFU` — return to Dimensions 3, 4 and expand   |
| BOFU keywords exist                                      | ≥3 BOFU keywords in output                    | Flag as `THIN BOFU` — return to Dimensions 4, 8 and expand   |
| No single funnel stage dominates excessively             | No stage contains >60% of total keywords      | Redistribute — expand underrepresented stages                |

**Dimension Coverage Validation:**

| Check                                       | Pass Condition                               |
|---------------------------------------------|----------------------------------------------|
| All 8 dimensions have at least 1 keyword    | No dimension is empty after deduplication    |
| No single dimension contains >40% of output | Balance is maintained across the keyword set |

**Pre-Cluster Balance Validation:**

For every pre-cluster group identified in the output:

| Check                                         | Pass Condition                                             |
|-----------------------------------------------|------------------------------------------------------------|
| Cluster has at least 3 keywords               | Minimum viable cluster for `semantic-clustering.skill`     |
| Cluster has at least 1 TOFU keyword           | Awareness content is possible for this cluster             |
| Cluster has at least 1 MOFU or BOFU keyword   | Conversion content is possible for this cluster            |

If a cluster fails the balance check → add keywords from the appropriate funnel stage before finalizing.

**Coverage Check Output:**

```
COVERAGE VALIDATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOFU Coverage       : [count] keywords — [FULL / THIN]
MOFU Coverage       : [count] keywords — [FULL / THIN]
BOFU Coverage       : [count] keywords — [FULL / THIN]
Dimension Balance   : [BALANCED / SKEWED — dimension X is overrepresented]
Cluster Balance     : [FULL / [N] clusters need expansion]
Overall Status      : [COMPLETE / REQUIRES EXPANSION]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If `REQUIRES EXPANSION` → run additional generation passes for flagged dimensions/funnel stages before producing final output.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ TF-IDF SIMULATION (UNIQUENESS PRIORITIZATION)

The Nexus keyword discovery process does not have access to live search volume data. It uses a simulated TF-IDF logic to prioritize keywords that are conceptually unique and strategically differentiated over generic head terms that are broadly competitive and often already over-served.

**TF-IDF Simulation Rules:**

1. **Downweight high-frequency generic head terms:**
   - If a keyword is a single-word or two-word generic term (e.g., `"CRM software"`, `"project management"`) without a modifier → assign it a `GENERIC` flag.
   - Generic keywords are included in the output only if they are strategically necessary as anchors for clusters.
   - They are deprioritized in the table ordering (listed after specific keywords in their dimension).

2. **Upweight unique, specific, multi-word phrases:**
   - Keywords that combine the core concept with an audience modifier, industry, use case, or comparison element are treated as high-value.
   - These are listed first in the output table within their category.

3. **Penalize common modifier patterns that produce identical content angles:**
   - If 3+ keywords in the output follow the identical structural template (e.g., `"[tool] for [industry]"`) with only the industry name changing, reduce to the 3 highest-value industry variants.
   - Reason: structural repetition = strategic redundancy even when the keywords are technically distinct.

---

### ▸ PRE-CLUSTERING LOGIC

This skill performs a lightweight pre-clustering pass before sending the output to `semantic-clustering.skill`. Pre-clustering reduces the work required by the clustering skill and ensures clusters are logically coherent from the start.

**Pre-Clustering Algorithm:**

1. Group keywords by their core concept sub-topic (the primary noun phrase shared by a group of keywords).
2. Label each group with a descriptive cluster name.
3. Validate each group for funnel balance (at least one TOFU + one MOFU or BOFU keyword).
4. Assign a dominant intent to each cluster.
5. Record the pre-cluster map in the output report.

**Pre-Cluster Naming Convention:**

Cluster names must be descriptive of the content theme, not the keyword template:

- ✅ `"CRM Setup and Implementation"` (describes content theme)
- ✅ `"CRM for Specific Industries"` (describes content theme)
- ❌ `"How-To Keywords"` (describes the template, not the content)
- ❌ `"Long-Tail Group 1"` (generic label with no descriptive value)

---

### ▸ INTENT JOURNEY MAPPING

Beyond funnel stage classification, this skill maps the progression of intent across the keyword universe to identify the optimal path a reader would take from their first search to conversion.

**Intent Journey Construction:**

1. Identify the first-touch entry point for the keyword universe (the TOFU keyword a searcher is most likely to use when they first become aware of the problem).
2. Map the expected next searches in sequence (TOFU → MOFU → BOFU).
3. Identify any gaps in the journey where the keyword universe has no coverage.
4. Flag gap keywords as priority additions.

**Intent Journey Output (included in the report):**

```
INTENT JOURNEY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Entry Point (TOFU)    : [keyword] — "[what the searcher first googles]"
  ↓
Research Stage (MOFU) : [keyword] — "[what they search when comparing]"
  ↓
Decision Stage (BOFU) : [keyword] — "[what they search when ready to act]"

Journey Gaps Detected : [list of missing stage coverage if any]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### System-Level Deduplication Standards

Deduplication in this skill operates at five levels (implemented in Step 5) and enforced by the following system-level rules:

**Rule 1 — One Keyword, One Strategic Purpose**

No two keywords in the final output may serve the same strategic purpose for the same audience at the same funnel stage. If they do, they are intent duplicates regardless of how different their wording appears.

**Rule 2 — Memory Exclusion Is Absolute**

Keywords present in the project's keyword memory in any lifecycle state are unconditionally excluded from the output. There are no exceptions. Previously generated keywords represent investments already made — they are not re-discovered, they are built upon by other skills.

**Rule 3 — Structural Template Duplicates Are Semantic Duplicates**

Two keywords generated from the same template with only a variable substitution (e.g., `"CRM for [industry A]"` and `"CRM for [industry B]"`) are NOT automatically duplicates — the industries make them strategically distinct. However, if the strategic reason for both keywords is identical, they ARE duplicates and only one is retained.

**Rule 4 — Cross-Dimension Duplication Is Resolved at Deduplication, Not at Generation**

Keywords are generated independently across all 8 dimensions. It is expected that Dimension 3 and Dimension 4 keywords may overlap (comparisons and alternatives often share structural patterns). This is acceptable at generation time and resolved at Step 5 — deduplication is not applied during generation.

**Rule 5 — Deduplication Logs Are Permanent**

Every removed keyword and its removal reason is logged in the output's Excluded Keywords Log. This log is written to `keyword-memory.skill` via `memory-controller.skill` so that future runs of this skill know what was considered and discarded — not just what was kept.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Keyword Behaviors

| Prohibited Behavior                                                                 | Reason                                                                    |
|-------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Generating a keyword with no strategic reason                                       | Every keyword must earn its place. No purpose = no inclusion.            |
| Generating generic single-word keywords (`"marketing"`, `"software"`, `"tools"`)    | Too broad. No strategic differentiation. Always dominated by authority sites. |
| Generating keywords that are reformulations of the `primary_input` with no new angle | Adds zero new coverage. Pure repetition.                                 |
| Generating more than 3 keywords from the identical structural template without distinct variables | Structural redundancy = strategic redundancy.               |
| Including navigational keywords (brand-only searches) for brands the user does not own | No SEO value for a site that is not the brand destination.             |
| Writing a strategic reason that applies to any keyword generically                  | Strategic reasons must be specific. Generic reasons are not strategic.   |
| Producing a keyword universe that is 90%+ TOFU                                      | Without MOFU and BOFU coverage, the keyword universe cannot drive conversions. |
| Marking a keyword as both Informational intent AND BOFU funnel stage                | Intent and funnel assignment must be internally consistent.              |
| Producing a dimension with zero keywords after deduplication without triggering expansion | Every dimension must contribute to the final output.                 |
| Skipping the strategic reason field for any keyword, for any reason                 | The strategic reason is mandatory. No keyword is output without it.      |

### Required Output Behaviors

- Every keyword in the table must have all five fields populated: Keyword, Intent, Funnel, Category, Strategic Reason.
- The Dimension Coverage Summary must be included in every output.
- The Funnel Coverage Summary must be included in every output.
- The Excluded Keywords Log must include every removed keyword — not a count only.
- The Pre-Cluster Map must group all output keywords — no orphaned keywords.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ memory-controller.skill

| Property         | Detail                                                                                 |
|------------------|----------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before expansion, WRITE after output finalization                 |
| **Reads**        | Existing keyword list, assigned keywords, published keywords, existing cluster labels  |
| **Writes**       | Complete output keyword table, deduplication log, pre-cluster map                     |
| **Timing**       | READ at Step 1. WRITE after Step 8 (coverage check passes).                           |
| **Layer**        | keyword-memory.skill (via controller)                                                  |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                 |
|------------------|----------------------------------------------------------------------------------------|
| **Direction**    | keyword-discovery → semantic-clustering (one-way output dispatch)                     |
| **Sends**        | Complete deduplicated keyword table + pre-cluster map as clustering seed               |
| **Used for**     | Building final topic clusters from the keyword universe                                |
| **Note**         | Pre-cluster map is advisory — semantic-clustering.skill may revise cluster boundaries  |

---

### ▸ serp-analysis.skill

| Property         | Detail                                                                                 |
|------------------|----------------------------------------------------------------------------------------|
| **Direction**    | keyword-discovery output is consumed by serp-analysis.skill                           |
| **Sends**        | Primary keyword and top strategic keywords (particularly MOFU and BOFU) as analysis targets |
| **Used for**     | SERP analysis of the most strategically valuable keywords in the universe              |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                 |
|------------------|----------------------------------------------------------------------------------------|
| **Direction**    | keyword-discovery output consumed by gap-opportunity-engine.skill                     |
| **Sends**        | Full keyword universe (post-dedup) as the known keyword space for gap analysis        |
| **Used for**     | Identifying which keywords in the universe are not yet covered by the site's content  |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                 |
|------------------|----------------------------------------------------------------------------------------|
| **Direction**    | keyword-discovery output consumed indirectly via pre-execution-input.skill            |
| **Sends**        | Assigned keyword (from this skill's lifecycle output) as the content target keyword   |
| **Used for**     | Ensuring content is built around strategically validated keywords                     |

---

### ▸ keyword-memory.skill (via memory-controller)

| Property         | Detail                                                                                 |
|------------------|----------------------------------------------------------------------------------------|
| **Direction**    | Write (after output finalization)                                                      |
| **Writes**       | All output keywords at lifecycle state `DISCOVERED`, dedup log, pre-cluster hints     |
| **Used for**     | Cross-session deduplication, lifecycle tracking, downstream skill context             |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                          | Detection                                           | Recovery Action                                                                           |
|-----------------------------------------------------------|-----------------------------------------------------|-------------------------------------------------------------------------------------------|
| `primary_input` is missing                                | Input validation pre-condition check fails          | Halt. Request primary keyword, topic, or niche from user.                                 |
| `primary_input` is a navigational query                   | Navigation signal detected (brand name only)        | Warn user. Ask for alternative or confirm intent before proceeding.                       |
| `primary_input` is too broad                              | Expansion scope = BROAD                             | Trigger Step 2B (breadth scoping). Present sub-topic options. Wait for user selection.    |
| Memory controller is unavailable at Step 1               | READ query returns error                            | Proceed without memory check. Flag: "Memory unavailable — exclusions may include previously generated keywords." Log for manual review. |
| Deduplication removes >80% of generated keywords          | Deduplication log shows extreme removal rate        | Flag: "High deduplication rate detected." Analyze which dimensions are most affected. Expand those dimensions with different templates. |
| A dimension produces zero keywords after deduplication    | Dimension coverage validation fails                 | Return to that dimension. Generate with stricter template variation. If still zero after two passes → mark dimension as `EXHAUSTED` and note in output. |
| Funnel coverage check fails (THIN stage detected)         | Coverage check at Step 8                            | Return to the most productive dimension for the thin stage. Generate additional keywords. Re-run coverage check. |
| User insists on a broad input after Step 2B warning       | User rejects sub-topic narrowing                    | Proceed with BROAD scope. Apply strict TF-IDF simulation (aggressive downweighting of generic head terms). Flag output as BROAD SCOPE — recommend running targeted follow-up research by sub-topic. |
| No strategic reason can be written for a generated keyword | Strategic reason quality check flags vague output  | Remove the keyword. A keyword that cannot be strategically justified has no place in the output. |
| Pre-cluster map has clusters with <3 keywords after dedup  | Cluster balance validation fails                   | Merge the thin cluster with the most semantically adjacent cluster. Note the merge in the pre-cluster map. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — DIMENSION QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
DIMENSION QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
D1 — Problem-Based      | TOFU–MOFU | Min: 4  | Max: 10
D2 — How-To             | TOFU      | Min: 5  | Max: 12
D3 — Comparison         | MOFU–BOFU | Min: 4  | Max: 10
D4 — Alternatives       | MOFU–BOFU | Min: 4  | Max: 8
D5 — Long-Tail          | Mixed     | Min: 8  | Max: 20
D6 — Questions (AEO)    | TOFU      | Min: 6  | Max: 15
D7 — Semantic Expansion | Mixed     | Min: 6  | Max: 18
D8 — Niche Modifiers    | MOFU–BOFU | Min: 6  | Max: 15
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total Min Keywords (pre-dedup) : 43
Total Max Keywords (pre-dedup) : 108
Expected Output (post-dedup)   : 30–70 unique strategic keywords
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — CATEGORY × INTENT COMPATIBILITY MATRIX
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
                      Informational  Commercial  Transactional
Educational         :      ✓             ✗             ✗
Comparison          :      ✗             ✓             ✓
Alternative         :      ✗             ✓             ✓
Problem-Solution    :      ✓             ✓             ✗
Long-Tail           :      ✓             ✓             ✓
AEO                 :      ✓             ✗             ✗
Semantic            :      ✓             ✓             ✗
Niche               :      ✓             ✓             ✓

✓ = Compatible assignment
✗ = Incompatible — reassign category or intent if this combination appears
```

---

*SKILL FILE END: research/keyword-discovery.skill*
*Nexus SEO Operating System — Version 1.0.0*
