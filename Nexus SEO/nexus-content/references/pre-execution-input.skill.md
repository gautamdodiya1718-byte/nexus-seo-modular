# SKILL: content/pre-execution-input.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Pre-Execution
# EXECUTION PRIORITY: PRE-FLIGHT — Runs before content-generation-engine.skill and all major content workflows.
# TRIGGER: Any request involving content creation, SEO strategy generation, or multi-step content workflows.

---

## BRAND FILE PRE-FILL (runs before any questions)

Before asking the user anything, check whether an active brand file is loaded in session.

If brand file IS loaded:
  Auto-populate these fields from the brand file and SKIP asking for them:
    tone              ← brand.tone
    audience          ← brand.primary_audience.role + brand.primary_audience.seniority
    technical_level   ← brand.technical_level
    writing_style     ← brand.writing_style_notes
    banned_words      ← brand.banned_words
    funnel_stage      ← infer from content type (ask only if ambiguous)
    target_url        ← brand.site_url

  Tell the user which fields were auto-filled:
  "Auto-filled from [brand.brand_name] profile:
    Audience: [brand.primary_audience.role]
    Tone: [brand.tone]
    Technical level: [brand.technical_level]
  Only asking for what the brand profile doesn't cover."

If brand file IS NOT loaded:
  Ask all questions as normal.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **pre-execution input collector and configuration validator** for all content-related workflows in the Nexus system.

No content generation, content strategy, or multi-step content workflow may begin without a fully validated configuration object. This skill is responsible for producing that object — either by collecting answers from the user through structured, guided questions, by intelligently inferring values from available context, or by applying smart defaults when the user explicitly delegates all decisions to the system.

The skill ensures that `content-generation-engine.skill` and all downstream content skills always receive a clean, structured, conflict-free configuration rather than raw, ambiguous, or incomplete user input.

### Primary Responsibilities

| Responsibility                    | Description                                                                                              |
|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| **Input Trigger Detection**       | Determine whether input collection is needed or whether the user has already provided all required values|
| **Guided Question Delivery**      | Ask up to 10 structured questions, each with options, explanations, and a clear recommendation          |
| **Smart Inference**               | Derive missing values from keyword signals, topic type, content type, and user intent                   |
| **Smart Default Application**     | Apply and explain pre-selected defaults when a user skips questions or delegates decisions              |
| **Input Normalization**           | Convert all collected or inferred answers into a standardized configuration object                      |
| **Conflict Detection**            | Identify and resolve incompatible configuration combinations before they reach downstream skills        |
| **Keyword Deduplication Check**   | Verify the selected keyword has not already been used for existing content in this project              |
| **Configuration Output**          | Deliver a fully validated, structured configuration object to `content-generation-engine.skill`          |

### What This Skill Is NOT

- It is **not** a content generator. It prepares the configuration — it does not write.
- It is **not** a keyword researcher. If the keyword is missing and cannot be inferred, it prompts the user — it does not discover keywords itself.
- It is **not** an interrogator. It avoids over-questioning. When inference or defaults can serve accurately, it uses them.
- It is **not** optional. No content execution begins with an undefined configuration. This is an absolute rule.
- It is **not** a SERP analyzer. Word count auto-detection is flagged for `content-generation-engine.skill` — it does not perform SERP analysis here.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill accepts input from two sources.

### Source A — User Request (via task-router.skill)

Any user request that involves content creation, strategy generation, or multi-step content workflows. This may be highly specific or entirely vague.

| Input Form                                  | Example                                                       |
|---------------------------------------------|---------------------------------------------------------------|
| Fully specified request                     | `"Write a 2000-word technical guide on Docker for DevOps engineers, strict accuracy, professional tone"` |
| Partially specified request                 | `"Write a blog post about project management software"`       |
| Vague content request                       | `"Write something about AI tools"`                            |
| Delegated request                           | `"just do everything"` / `"use defaults"` / `"auto"`         |
| Workflow trigger from execution-mode.skill  | Structured execution packet with `target_skill = content-generation-engine.skill` |

### Source B — Memory Context (from memory-controller.skill)

Before asking any question, this skill queries `memory-controller.skill` for existing project context that may pre-fill or constrain configuration options.

**Memory Context Fields Used:**

| Field from Memory                | Usage in This Skill                                                 |
|----------------------------------|---------------------------------------------------------------------|
| `project niche`                  | Infer audience level, accuracy mode, and funnel stage              |
| `target audience` (if set)       | Pre-fill audience field without asking                             |
| `existing keyword list`          | Check for keyword duplication before accepting user keyword         |
| `existing content titles`        | Check for topic duplication before accepting keyword               |
| `published_content lifecycle states` | Identify gaps in funnel coverage for smart funnel recommendation |
| `competitor URLs` (if stored)    | Pre-fill competitor benchmark if manual URLs already exist          |

### Input Pre-Conditions

Before this skill begins question collection, it evaluates three pre-conditions:

1. **Is this a content-related request?**
   → If yes, proceed. If no, return to `task-router.skill` — this skill was triggered incorrectly.

2. **Has the user already provided a complete configuration?**
   → Evaluate completeness against all 10 configuration fields.
   → If all 10 are present and non-conflicting → skip to Step 4 (Normalization).
   → If any field is missing or ambiguous → proceed to Step 2 (Question Collection).

3. **Has the user delegated all decisions?**
   → Detect delegation phrases: `"just do everything"`, `"use defaults"`, `"auto"`, `"recommended"`, `"your choice"`.
   → If yes → skip all questions → apply all recommended defaults → proceed to Step 4.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill produces a single, fully structured configuration object delivered to `content-generation-engine.skill`.

### Primary Output — Structured Configuration Object

```
NEXUS CONTENT CONFIGURATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID          : [ID]
Generated By        : content/pre-execution-input.skill
Timestamp           : [ISO 8601]
Collection Method   : [FULL_COLLECTION / PARTIAL_COLLECTION / FULL_INFERENCE / FULL_DEFAULT]

Configuration:
  content_type      : [Blog Post / Landing Page / Technical Guide / Comparison Article]
  keyword           : [target keyword string]
  word_count        : [integer / AUTO]
  funnel            : [TOFU / MOFU / BOFU]
  audience          : [Beginner / Intermediate / Expert / Business]
  competitors       : [AUTO / list of URLs]
  accuracy_mode     : [Standard / High / Strict]
  tone              : [Professional / Casual / Technical / Persuasive]
  depth             : [Standard / Deep / Skimmable]
  goal              : [Traffic / Leads / Sales / Affiliate]

Inference Log:
  [field]           : [value] — [USER_PROVIDED / INFERRED: reason / DEFAULT: reason]
  ...

Conflict Check      : [PASSED / RESOLVED: description]
Keyword Dedup       : [PASSED / WARNING: description]
Validation Status   : COMPLETE
Dispatch To         : content-generation-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Collection Method Labels

| Method                | Meaning                                                                              |
|-----------------------|--------------------------------------------------------------------------------------|
| `FULL_COLLECTION`     | All 10 fields were answered by the user directly                                     |
| `PARTIAL_COLLECTION`  | Some fields collected from user; remainder inferred or defaulted                    |
| `FULL_INFERENCE`      | All fields inferred from user's original request without asking any questions        |
| `FULL_DEFAULT`        | User delegated all decisions; all recommended defaults applied                       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DETECT IF INPUT COLLECTION IS NEEDED

Before asking any question, evaluate what information is already available and what must be collected.

**Phase 1A — Check for Delegation Phrases**

Scan the user's input for full-delegation signals:

| Delegation Phrase                            | Action                                                |
|----------------------------------------------|-------------------------------------------------------|
| `"just do everything"`                       | Apply all recommended defaults. Skip to Step 3.       |
| `"use defaults"` / `"recommended"`           | Apply all recommended defaults. Skip to Step 3.       |
| `"auto"` / `"automatic"`                     | Apply all recommended defaults. Skip to Step 3.       |
| `"your choice"` / `"you decide"`             | Apply all recommended defaults. Skip to Step 3.       |
| `"don't ask me anything"`                    | Apply all recommended defaults. Skip to Step 3.       |

When delegation is confirmed, display:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Auto-Configuration Selected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
All recommended defaults will be applied.
[Keyword must still be confirmed — see below]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Exception:** Keyword is ALWAYS required. Even under full delegation, if the keyword cannot be confidently inferred from the user's request, ask for it before proceeding.

---

**Phase 1B — Extract Already-Provided Values**

Parse the user's raw input and extract any of the 10 configuration fields that are explicitly stated or strongly implied.

**Extraction Heuristics:**

| User Says                                           | Extracted Field + Value                                     |
|-----------------------------------------------------|-------------------------------------------------------------|
| `"2000 words"` / `"2000-word"`                      | `word_count = 2000`                                         |
| `"blog post"` / `"article"`                         | `content_type = Blog Post`                                  |
| `"landing page"`                                    | `content_type = Landing Page`                               |
| `"technical guide"` / `"technical tutorial"`        | `content_type = Technical Guide`                            |
| `"comparison"` / `"vs"` / `"alternatives"`         | `content_type = Comparison Article`                         |
| `"for beginners"`                                   | `audience = Beginner`                                       |
| `"for developers"` / `"for engineers"`              | `audience = Expert`                                         |
| `"for businesses"` / `"B2B"`                        | `audience = Business`                                       |
| `"professional tone"` / `"formal"`                  | `tone = Professional`                                       |
| `"casual"` / `"conversational"`                     | `tone = Casual`                                             |
| `"technical tone"`                                  | `tone = Technical`                                          |
| `"persuasive"` / `"sales"`                          | `tone = Persuasive`                                         |
| `"strict accuracy"` / `"cite sources"`              | `accuracy_mode = Strict`                                    |
| `"generate leads"` / `"lead generation"`            | `goal = Leads`                                              |
| `"drive traffic"` / `"rank on Google"`              | `goal = Traffic`                                            |
| `"increase sales"` / `"sell"`                       | `goal = Sales`                                              |
| `"affiliate"` / `"review"`                          | `goal = Affiliate`                                          |
| `"deep dive"` / `"comprehensive"`                   | `depth = Deep`                                              |
| `"quick read"` / `"skimmable"`                      | `depth = Skimmable`                                         |

**Phase 1C — Determine Collection Mode**

After extraction, count how many fields are populated:

| Fields Populated | Collection Mode                                              |
|------------------|--------------------------------------------------------------|
| 10 of 10         | Skip directly to Step 4 (Normalization)                      |
| 7–9 of 10        | Collect only the missing fields. Skip answered ones.         |
| 4–6 of 10        | Collect missing fields. Infer where possible before asking.  |
| 0–3 of 10        | Run full guided question sequence (Steps 2A through 2J).     |

**Phase 1D — Query Memory for Pre-Fill**

Before beginning question collection, query `memory-controller.skill` for existing project context:

1. If `target_audience` exists in project memory → pre-fill `audience` without asking.
2. If `competitor_urls` exist in project memory → pre-fill `competitors` without asking.
3. If `project_niche` exists → use for inference in Steps 2D, 2E, 2G.
4. Check existing keyword list → used in Step 5 (Keyword Deduplication).

---

### ▸ STEP 2 — GUIDED QUESTION COLLECTION

Present only the questions for fields that are not yet populated. Do not re-ask for fields that were extracted in Step 1 or pre-filled from memory.

Present questions one at a time in the order defined below. Do NOT present all questions simultaneously. Do NOT combine multiple questions into one prompt.

After each answer, acknowledge it briefly (one line max) and move to the next question.

---

#### ▸ QUESTION 1 — Content Type

**Ask only if:** `content_type` is not yet populated.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Content Type
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What type of content do you want to create?

1  Blog Post
   → Drives organic search traffic via Google rankings
   → Use when: building awareness or targeting informational keywords
   → Recommended for most SEO campaigns ✓

2  Landing Page
   → Conversion-focused, minimal navigation, direct CTA
   → Use when: selling a product or service, running paid campaigns
   → Recommended for BOFU (bottom-of-funnel) goals ✓

3  Technical Guide
   → Deep, precise, documentation-style content
   → Use when: targeting developers, engineers, or expert-level audiences
   → Recommended for authority building and niche trust ✓

4  Comparison Article
   → Evaluates multiple products, tools, or approaches side-by-side
   → Use when: targeting decision-stage buyers with commercial intent
   → Recommended for affiliate and conversion goals ✓

Enter number (1–4) or type your preference:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response       | Action                                                              |
|----------------|---------------------------------------------------------------------|
| `1`            | `content_type = Blog Post`                                          |
| `2`            | `content_type = Landing Page`                                       |
| `3`            | `content_type = Technical Guide`                                    |
| `4`            | `content_type = Comparison Article`                                 |
| Free text      | Normalize to closest match. Confirm: "Setting content type to [X]. Correct?" |
| No response    | Default to `Blog Post`. Note in inference log.                     |

---

#### ▸ QUESTION 2 — Target Keyword

**Ask only if:** `keyword` is not yet populated AND cannot be confidently inferred.

**Inference Before Asking:**

Attempt to infer the keyword from the user's original request:
- If user said `"write about project management software"` → infer `keyword = project management software`
- If user said `"SEO article on Docker for DevOps"` → infer `keyword = Docker for DevOps`
- If inferred with high confidence → use without asking. Log as `INFERRED`.
- If inferred with medium confidence → present inference for confirmation:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Keyword Confirmation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Inferred keyword: "[inferred keyword]"

Use this keyword? [YES / Enter different keyword]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**If keyword cannot be inferred at all, display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Target Keyword
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What keyword or topic should this content target?

This is the primary term your content will be optimized for.
It can be a single keyword, a phrase, or a topic.

Example: "best project management software for small teams"

Enter keyword or topic:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Keyword Validation (immediately after collection):**

1. Keyword must not be empty. If empty after two prompts → halt and notify user.
2. Keyword length: 1–10 words. If longer, ask user to trim to the primary phrase.
3. Run deduplication check against keyword memory (Step 5).

---

#### ▸ QUESTION 3 — Word Count

**Ask only if:** `word_count` is not yet populated.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Word Count
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
How long should the content be?

1  Auto
   → Determines ideal length by analyzing top-ranking pages for your keyword
   → Use when: unsure of ideal length, or optimizing for SERP parity
   → Recommended ✓

2  Custom
   → You specify an exact word count
   → Use when: you have a strict editorial requirement
   → Not recommended unless necessary

Enter 1 or 2, or type a specific number (e.g., "1500"):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response              | Action                                                               |
|-----------------------|----------------------------------------------------------------------|
| `1` or `"auto"`       | `word_count = AUTO`                                                  |
| `2`                   | Display sub-prompt: "Enter your target word count:" → set as integer |
| Integer value         | `word_count = [integer]`                                             |
| No response / skip    | `word_count = AUTO`. Log as DEFAULT.                                 |

**Word Count Validation:**

- Minimum: 300 words (below this, flag as too short for SEO value).
- Maximum: 15,000 words (above this, flag as potentially better split across multiple pieces).
- If out of range → display advisory and ask to confirm or adjust.

---

#### ▸ QUESTION 4 — Funnel Stage

**Ask only if:** `funnel` is not yet populated AND cannot be inferred.

**Inference Before Asking:**

| Signal                                               | Inferred Funnel |
|------------------------------------------------------|-----------------|
| `goal = Traffic` OR `content_type = Blog Post` AND informational keyword | TOFU |
| `goal = Leads` OR `content_type = Comparison Article`| MOFU            |
| `goal = Sales` OR `goal = Affiliate` OR `content_type = Landing Page` | BOFU |
| Keyword contains: `"best"`, `"vs"`, `"review"`, `"buy"`, `"price"` | BOFU |
| Keyword contains: `"how to"`, `"what is"`, `"guide"` | TOFU            |
| Keyword contains: `"alternatives"`, `"compare"`, `"top X"` | MOFU        |

If inference confidence is HIGH → apply without asking. Log as `INFERRED`.
If inference confidence is MEDIUM or LOW → ask.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Funnel Stage
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Which stage of the buyer journey does this content target?

1  TOFU — Top of Funnel
   → Awareness-stage content for cold audiences
   → Use when: building traffic, targeting broad informational keywords
   → Recommended for traffic growth ✓

2  MOFU — Middle of Funnel
   → Comparison and evaluation content for warm audiences
   → Use when: building trust, targeting consideration-stage buyers
   → Recommended for balanced growth

3  BOFU — Bottom of Funnel
   → Conversion content for hot, purchase-ready audiences
   → Use when: driving sales, sign-ups, or affiliate clicks
   → Recommended for revenue goals ✓

Enter number (1–3):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response    | Action                      |
|-------------|-----------------------------|
| `1`         | `funnel = TOFU`             |
| `2`         | `funnel = MOFU`             |
| `3`         | `funnel = BOFU`             |
| No response | Infer from keyword or goal. Default to TOFU if still unclear. Log. |

---

#### ▸ QUESTION 5 — Target Audience

**Ask only if:** `audience` is not yet populated AND not pre-filled from project memory.

**Inference Before Asking:**

| Signal                                                          | Inferred Audience  |
|-----------------------------------------------------------------|--------------------|
| `content_type = Technical Guide` OR keyword contains tech terms | Expert             |
| Keyword contains `"for beginners"`, `"introduction"`, `"basics"` | Beginner         |
| Keyword targets business use cases, ROI, teams, enterprise      | Business           |
| Keyword contains `"advanced"`, `"deep dive"`, `"architecture"`  | Expert             |
| General informational keyword with no strong signal             | Intermediate       |

If inference confidence is HIGH → apply without asking. Log as `INFERRED`.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Target Audience
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Who is the primary audience for this content?

1  Beginner
   → No prior knowledge assumed
   → Use when: keyword targets newcomers or broad educational topics

2  Intermediate
   → Some familiarity assumed; skips basic definitions
   → Use when: most general SEO blog posts and guides

3  Expert
   → Deep technical knowledge assumed; high-detail, no hand-holding
   → Use when: developer docs, engineering guides, advanced tutorials

4  Business
   → Decision-maker perspective; ROI, efficiency, team impact
   → Use when: B2B content, SaaS features, enterprise software

Enter number (1–4):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response    | Action                          |
|-------------|---------------------------------|
| `1`         | `audience = Beginner`           |
| `2`         | `audience = Intermediate`       |
| `3`         | `audience = Expert`             |
| `4`         | `audience = Business`           |
| No response | Infer or default to Intermediate. Log. |

---

#### ▸ QUESTION 6 — Competitor Benchmark

**Ask only if:** `competitors` is not yet populated AND no competitor URLs exist in project memory.

**If competitor URLs exist in project memory:** pre-fill with existing URLs. Skip this question. Notify:
`"Using [N] competitor URLs from your project memory."`

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Competitor Benchmark
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
How should content be benchmarked against competitors?

1  Auto
   → Pulls the top-ranking pages for your keyword from SERP
   → No URLs needed from you
   → Recommended ✓

2  Manual
   → You provide specific competitor URLs to benchmark against
   → Use when: you want to target specific competitors

Enter 1 or 2:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response    | Action                                                                    |
|-------------|---------------------------------------------------------------------------|
| `1`         | `competitors = AUTO`                                                      |
| `2`         | Display: "Enter competitor URLs (one per line, press Enter when done):" → `competitors = [url list]` |
| No response | `competitors = AUTO`. Log as DEFAULT.                                     |

**URL Validation (for Manual mode):**

- Must start with `http://` or `https://`.
- Maximum 10 URLs accepted.
- Remove duplicates.
- Flag URLs that are not standard web pages (e.g., PDF links, login-required pages).

---

#### ▸ QUESTION 7 — Accuracy Mode

**Ask only if:** `accuracy_mode` is not yet populated.

**Inference Before Asking:**

| Signal                                                          | Inferred Accuracy Mode |
|-----------------------------------------------------------------|------------------------|
| `content_type = Technical Guide` OR `audience = Expert`         | Strict                 |
| Topic is medical, legal, financial, or scientific               | Strict                 |
| Topic is general SaaS, marketing, or business                   | High                   |
| Topic is lifestyle, opinion, or trend-based                     | Standard               |
| Keyword contains `"statistics"`, `"data"`, `"research"`, `"study"` | High               |

If inference confidence is HIGH → apply without asking. Log as `INFERRED`.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Accuracy Mode
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What level of factual accuracy is required?

1  Standard
   → General accuracy; statements are plausible and well-reasoned
   → Use when: opinion pieces, trend analysis, soft topics

2  High
   → Claims are grounded in known data; statistics are validated
   → Use when: SaaS, marketing, business topics
   → Recommended for most content ✓

3  Strict
   → All factual claims must be traceable to official documentation,
     peer-reviewed sources, or primary data only
   → Use when: medical, legal, financial, technical documentation topics

Enter number (1–3):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response    | Action                          |
|-------------|---------------------------------|
| `1`         | `accuracy_mode = Standard`      |
| `2`         | `accuracy_mode = High`          |
| `3`         | `accuracy_mode = Strict`        |
| No response | `accuracy_mode = High`. Log as DEFAULT. |

---

#### ▸ QUESTION 8 — Tone

**Ask only if:** `tone` is not yet populated.

**Inference Before Asking:**

| Signal                                                     | Inferred Tone   |
|------------------------------------------------------------|-----------------|
| `audience = Expert` OR `content_type = Technical Guide`    | Technical       |
| `content_type = Landing Page` OR `goal = Sales`            | Persuasive      |
| `audience = Business` AND `goal = Leads`                   | Professional    |
| `audience = Beginner` OR lifestyle/casual topic            | Casual          |
| `audience = Intermediate` AND no strong tone signal        | Professional    |

If inference confidence is HIGH → apply without asking. Log as `INFERRED`.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Content Tone
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What tone should the content use?

1  Professional
   → Clear, authoritative, business-appropriate
   → Use when: B2B content, industry guides, corporate audiences

2  Casual
   → Friendly, approachable, conversational
   → Use when: consumer-facing content, beginner audiences

3  Technical
   → Precise, direct, assumes reader knowledge
   → Use when: developer guides, API docs, engineering content

4  Persuasive
   → Benefit-focused, urgency-driven, action-oriented
   → Use when: landing pages, sales content, conversion-focused articles

Enter number (1–4):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response    | Action                      |
|-------------|-----------------------------|
| `1`         | `tone = Professional`       |
| `2`         | `tone = Casual`             |
| `3`         | `tone = Technical`          |
| `4`         | `tone = Persuasive`         |
| No response | Infer or default to Professional. Log. |

---

#### ▸ QUESTION 9 — Content Depth

**Ask only if:** `depth` is not yet populated.

**Inference Before Asking:**

| Signal                                                        | Inferred Depth |
|---------------------------------------------------------------|----------------|
| `content_type = Technical Guide` OR `audience = Expert`       | Deep           |
| `word_count > 2500` OR keyword suggests comprehensive guide   | Deep           |
| `word_count < 1000` OR `goal = Traffic` with casual topic     | Skimmable      |
| No strong signal                                              | Standard       |

If inference confidence is HIGH → apply without asking. Log as `INFERRED`.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Content Depth
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
How deeply should this content cover the topic?

1  Standard
   → Covers the topic thoroughly without excessive detail
   → Use when: most blog posts and general SEO content

2  Deep
   → Exhaustive coverage with sub-topics, edge cases, and advanced detail
   → Use when: pillar pages, technical guides, comprehensive resources

3  Skimmable
   → Optimized for quick reading with headers, bullets, and short paragraphs
   → Use when: list articles, quick-answer content, mobile-first audiences

Enter number (1–3):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response    | Action                      |
|-------------|-----------------------------|
| `1`         | `depth = Standard`          |
| `2`         | `depth = Deep`              |
| `3`         | `depth = Skimmable`         |
| No response | `depth = Standard`. Log as DEFAULT. |

---

#### ▸ QUESTION 10 — Goal

**Ask only if:** `goal` is not yet populated AND cannot be inferred.

**Inference Before Asking:**

| Signal                                                       | Inferred Goal  |
|--------------------------------------------------------------|----------------|
| `funnel = TOFU` AND informational keyword                    | Traffic        |
| `funnel = MOFU` OR `content_type = Comparison Article`       | Leads          |
| `funnel = BOFU` OR `content_type = Landing Page`             | Sales          |
| `content_type = Comparison Article` AND `audience = Expert`  | Affiliate      |
| Keyword contains `"best"`, `"review"`, `"top"`, `"vs"`       | Affiliate      |

If inference confidence is HIGH → apply without asking. Log as `INFERRED`.

**Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Content Goal
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What is the primary business goal for this content?

1  Traffic
   → Rank on Google and drive organic visitors
   → Use when: building audience, increasing visibility

2  Leads
   → Convert readers into contacts or trial sign-ups
   → Use when: SaaS products, service businesses, B2B

3  Sales
   → Drive direct purchases or bookings
   → Use when: ecommerce, product pages, BOFU content

4  Affiliate
   → Drive clicks to third-party products via affiliate links
   → Use when: review sites, comparison articles, niche blogs

Enter number (1–4):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Input Handling:**

| Response    | Action                  |
|-------------|-------------------------|
| `1`         | `goal = Traffic`        |
| `2`         | `goal = Leads`          |
| `3`         | `goal = Sales`          |
| `4`         | `goal = Affiliate`      |
| No response | Infer or default to Traffic. Log. |

---

### ▸ STEP 3 — SMART DEFAULTS

Applied when:
- User delegates all decisions (detected in Step 1).
- User skips individual questions.
- Inference confidence is too low to auto-populate a field.

**Default Value Table:**

| Field           | Default Value    | Rationale                                                              |
|-----------------|------------------|------------------------------------------------------------------------|
| `content_type`  | Blog Post        | Most common SEO content format; safest universal default               |
| `keyword`       | *(no default)*   | Must always be collected or inferred — no default is valid             |
| `word_count`    | AUTO             | SERP-parity detection produces better results than arbitrary numbers   |
| `funnel`        | TOFU             | Most content serves awareness before conversion                        |
| `audience`      | Intermediate     | Broadest coverage without assumptions about expertise level            |
| `competitors`   | AUTO             | Always pulling SERP top-10 is more comprehensive than user-provided list |
| `accuracy_mode` | High             | Balances credibility with production efficiency                        |
| `tone`          | Professional     | Most versatile tone across business, SaaS, and marketing content       |
| `depth`         | Standard         | Covers topic without over-engineering the content structure            |
| `goal`          | Traffic          | Most common entry-level SEO goal; safe universal default               |

**Default Application Display:**

When defaults are applied, display a summary before proceeding:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Configuration Applied
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
The following settings have been applied:

  Content Type    : Blog Post         [default]
  Word Count      : Auto              [default]
  Funnel          : TOFU              [default]
  Audience        : Intermediate      [default]
  Competitors     : Auto              [default]
  Accuracy Mode   : High              [default]
  Tone            : Professional      [default]
  Depth           : Standard          [default]
  Goal            : Traffic           [default]

You can override any setting by naming it now.
Otherwise, proceeding with this configuration.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Wait 5 seconds (or for user response) before proceeding. If user provides an override → update that field → re-display summary → proceed.

---

### ▸ STEP 4 — NORMALIZE INPUTS

Convert all collected, inferred, and defaulted values into the standardized configuration object format.

**Normalization Rules:**

| Field           | Normalization Action                                                                  |
|-----------------|---------------------------------------------------------------------------------------|
| `content_type`  | Capitalize first letter of each word. Map synonyms: `"article"` → `Blog Post`, `"page"` → `Landing Page` |
| `keyword`       | Lowercase. Trim whitespace. Remove trailing punctuation. Max 100 characters.          |
| `word_count`    | If integer: ensure positive integer. If `"auto"` or `"AUTO"` → set to string `AUTO`. |
| `funnel`        | Uppercase: `TOFU`, `MOFU`, `BOFU`. Map synonyms: `"top"` → `TOFU`, `"bottom"` → `BOFU`. |
| `audience`      | Capitalize first letter: `Beginner`, `Intermediate`, `Expert`, `Business`.           |
| `competitors`   | If AUTO → set string `AUTO`. If URLs → normalize to full URL format with protocol.    |
| `accuracy_mode` | Capitalize first letter: `Standard`, `High`, `Strict`.                               |
| `tone`          | Capitalize first letter: `Professional`, `Casual`, `Technical`, `Persuasive`.        |
| `depth`         | Capitalize first letter: `Standard`, `Deep`, `Skimmable`.                            |
| `goal`          | Capitalize first letter: `Traffic`, `Leads`, `Sales`, `Affiliate`.                   |

**Post-Normalization — Build the Inference Log:**

For every field in the configuration, record how it was populated:

```
Inference Log:
  content_type    : Blog Post        — USER_PROVIDED
  keyword         : [value]          — USER_PROVIDED
  word_count      : AUTO             — DEFAULT (user skipped)
  funnel          : TOFU             — INFERRED (informational keyword detected)
  audience        : Intermediate     — INFERRED (no strong audience signal)
  competitors     : AUTO             — DEFAULT (user skipped)
  accuracy_mode   : High             — DEFAULT (no technical topic detected)
  tone            : Professional     — INFERRED (Business audience + neutral topic)
  depth           : Standard         — DEFAULT (no depth signal detected)
  goal            : Traffic          — INFERRED (TOFU funnel + Blog Post + informational keyword)
```

---

### ▸ STEP 5 — VALIDATION

Run all validation checks against the normalized configuration object before dispatching.

**Validation Check 1 — Keyword Presence**

- `keyword` must not be empty or NULL.
- If empty after all collection and inference attempts → halt. Display:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Keyword Required
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
A target keyword is required to proceed.
Enter a keyword or topic to continue:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Validation Check 2 — Keyword Deduplication**

Query `memory-controller.skill` → `keyword-memory.skill`:
- Has this exact keyword been used for existing content in this project?
- Has a semantically similar keyword been used (≥85% similarity)?

| Dedup Result                    | Action                                                                                       |
|---------------------------------|----------------------------------------------------------------------------------------------|
| No duplicate found              | Proceed.                                                                                     |
| Exact duplicate found           | Display warning: "This keyword already has content in your project. Use anyway? [YES / Choose different keyword]" |
| Semantic duplicate found        | Display warning: "A similar keyword ([existing keyword]) already has content. Use this keyword anyway? [YES / Choose different]" |

**Validation Check 3 — Configuration Conflict Detection**

Check for incompatible field combinations:

| Conflict Combination                                   | Resolution                                                                  |
|--------------------------------------------------------|-----------------------------------------------------------------------------|
| `content_type = Landing Page` AND `depth = Deep`       | Advisory: "Landing pages are typically concise. Confirm Deep depth? [YES / Change to Standard]" |
| `content_type = Landing Page` AND `goal = Traffic`     | Advisory: "Landing pages rarely rank organically. Did you mean Blog Post for traffic? [YES keep / Change type]" |
| `accuracy_mode = Strict` AND `tone = Casual`           | Advisory: "Strict accuracy typically pairs with Professional or Technical tone. Keep Casual? [YES / Change tone]" |
| `depth = Skimmable` AND `word_count > 2500`            | Advisory: "Skimmable depth is unusual for long-form content. Keep? [YES / Change depth to Standard]" |
| `funnel = BOFU` AND `goal = Traffic`                   | Advisory: "BOFU content typically targets conversions, not broad traffic. Keep Traffic goal? [YES / Change to Sales or Leads]" |
| `audience = Beginner` AND `accuracy_mode = Strict`     | Advisory: "Strict accuracy for beginner content may reduce readability. Keep? [YES / Change to High]" |

For each conflict:
- Display advisory as a single-line prompt.
- Wait for user response.
- Apply user's choice.
- Log the resolution in the conflict log.

**Validation Check 4 — Word Count Range**

- If `word_count` is an integer and is below 300 → warn: "Word count below 300 may not provide sufficient SEO value. Continue? [YES / Adjust]"
- If `word_count` is an integer and exceeds 15,000 → warn: "Word count above 15,000 may be better split into a content series. Continue? [YES / Adjust]"

**Validation Check 5 — URL Format (if competitors = manual)**

- Validate each URL starts with `http://` or `https://`.
- Remove duplicates.
- Maximum 10 URLs.
- Flag invalid URLs. Remove them and notify user.

**Validation Pass Output:**

```
VALIDATION RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keyword Presence    : PASS
Keyword Dedup       : [PASS / WARNING — description]
Conflict Check      : [PASS / RESOLVED — description]
Word Count Range    : [PASS / WARNING — description]
URL Validation      : [PASS / N/A]
Overall Status      : [COMPLETE — proceed / BLOCKED — reason]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 6 — OUTPUT DISPATCH

Assemble the final configuration object and dispatch it to `content-generation-engine.skill`.

**Final Configuration Assembly:**

Combine:
- Normalized field values (Step 4)
- Inference log (Step 4)
- Validation result (Step 5)
- Session metadata

Produce the complete structured configuration object as defined in Section 3.

**Dispatch Action:**

1. Send the configuration object to `content-generation-engine.skill` as the first input in its execution packet.
2. Send a write request to `memory-controller.skill` to log the configuration as a strategy record.
3. Update the keyword lifecycle state for the target keyword:
   - If keyword was `DISCOVERED` or `CLUSTERED` → transition to `ASSIGNED`.
   - Log the transition in `keyword-memory.skill` via `memory-controller.skill`.
4. Return a dispatch confirmation:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Configuration Complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keyword             : [keyword]
Content Type        : [type]
Word Count          : [value or AUTO]
Funnel              : [stage]
Audience            : [level]
Tone                : [tone]
Depth               : [depth]
Goal                : [goal]
Accuracy            : [mode]

Passing to content generation...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ KEYWORD INTENT INFERENCE ENGINE

When a keyword is provided, this skill analyzes its semantic and structural signals to infer multiple configuration fields simultaneously. This reduces the number of questions asked and improves configuration accuracy.

**Intent Signal Analysis:**

| Keyword Pattern                                     | Intent Type    | Infers                                           |
|-----------------------------------------------------|----------------|--------------------------------------------------|
| `"what is"`, `"how to"`, `"guide to"`, `"learn"`   | Informational  | `funnel=TOFU`, `goal=Traffic`, `depth=Standard`  |
| `"best"`, `"top"`, `"alternatives"`, `"vs"`         | Commercial     | `funnel=MOFU` or `BOFU`, `goal=Affiliate`        |
| `"buy"`, `"price"`, `"discount"`, `"deal"`          | Transactional  | `funnel=BOFU`, `goal=Sales`                      |
| `"[brand name]"`, `"login"`, `"sign in"`            | Navigational   | Flag as low-value SEO target. Warn user.         |
| `"for developers"`, `"API"`, `"SDK"`, `"CLI"`       | Technical      | `audience=Expert`, `accuracy_mode=Strict`, `tone=Technical` |
| `"for beginners"`, `"101"`, `"introduction to"`     | Beginner       | `audience=Beginner`, `tone=Casual`, `depth=Standard` |
| `"ROI"`, `"for teams"`, `"enterprise"`, `"pricing"` | Business       | `audience=Business`, `tone=Professional`, `funnel=BOFU` |

Apply all inferences from the keyword simultaneously. Each inference updates the field if that field has not already been populated by the user.

---

### ▸ TECHNICAL TOPIC AUTO-DETECTION

When a keyword or content type signals a technical topic, this skill automatically upgrades the accuracy mode recommendation without asking.

**Technical Topic Detection Signals:**

- Keywords containing: `"code"`, `"API"`, `"architecture"`, `"database"`, `"security"`, `"algorithm"`, `"machine learning"`, `"neural"`, `"cloud"`, `"DevOps"`, `"infrastructure"`, `"protocol"`, `"encryption"`
- `content_type = Technical Guide`
- `audience = Expert`

**Auto-Upgrade Logic:**

- If technical signals detected AND `accuracy_mode` has not been explicitly set by user:
  → Set `accuracy_mode = Strict`
  → Log as `INFERRED (technical topic detected)`
  → Display advisory (not a question): `"Technical topic detected — accuracy mode set to Strict."`

---

### ▸ COMMERCIAL INTENT AUTO-DETECTION

When keyword signals show commercial or transactional intent, this skill proactively upgrades funnel and goal recommendations.

**Commercial Intent Detection Signals:**

- Keywords containing: `"best"`, `"top"`, `"review"`, `"vs"`, `"compared to"`, `"buy"`, `"price"`, `"cheap"`, `"affordable"`, `"discount"`, `"deal"`, `"coupon"`

**Auto-Upgrade Logic:**

- If commercial signals detected:
  → If `funnel` not yet set → set `funnel = BOFU`
  → If `goal` not yet set → set `goal = Affiliate` (for review/comparison content) or `goal = Sales` (for transactional content)
  → Log as `INFERRED (commercial intent detected)`

---

### ▸ OVER-QUESTIONING PREVENTION

This skill is designed to ask the minimum number of questions necessary. The following rules enforce this:

1. **Never ask a question that can be confidently inferred.** Confidence threshold: ≥80% certainty based on available signals.
2. **Never ask more than 10 questions total.** If all 10 fields are not resolved after 10 questions, apply defaults for remaining fields.
3. **Never ask the same question twice.** If a user skips a question → apply default → move on.
4. **Never ask for information already in project memory.** Always query memory first.
5. **Batch inference first.** Before asking question N, apply all inferences from answers 1 through N-1. If question N can now be inferred → skip it.
6. **Stop asking after the keyword is confirmed** if the user says `"just write it"` or equivalent → apply all defaults for remaining fields → proceed.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — Keyword-Level Deduplication

Before accepting any keyword, check against `keyword-memory.skill` via `memory-controller.skill`:

- **Exact match:** Same keyword string already exists in project memory with lifecycle state ≥ `ASSIGNED`.
  → Warn user. Require explicit confirmation to proceed.
- **Semantic match (≥85% similarity):** A keyword covering the same topic already exists.
  → Warn user. Show the existing keyword. Ask: "Use this keyword anyway?"
- **Intent match:** Same search intent AND same funnel stage already served by existing content.
  → Warn user. Show the existing content title. Ask: "This topic may already be covered. Continue?"

### Rule 2 — Topic-Level Deduplication

Before finalizing the configuration, check `content-memory.skill` via `memory-controller.skill` for existing articles targeting the same keyword or close semantic match.

- If an article in `DRAFT_GENERATED`, `VALIDATED`, or `PUBLISHED` state covers the same keyword:
  → Display: "Content for [keyword] already exists at [lifecycle state]. Create another piece or update existing?"

### Rule 3 — Funnel Coverage Deduplication

Check whether the selected `funnel` stage is already well-covered in content memory for this project.

- If the project has ≥5 pieces of content in the same funnel stage:
  → Advisory only (not a blocker): "You have [N] [TOFU/MOFU/BOFU] pieces already. Consider filling [other funnel stage] gaps."

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Behaviors

| Prohibited Behavior                                                                   | Reason                                                               |
|---------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| Asking all 10 questions regardless of what is already known                           | Over-questioning damages UX and wastes time                         |
| Displaying option descriptions that exceed two lines per option                       | Keeps questions scannable; prevents overwhelming the user           |
| Asking for the same field more than once in a session                                 | Repetition erodes trust in the system                               |
| Adding editorial commentary to the user's choices ("Great choice!")                   | Filler. Prohibited in all Nexus skill outputs.                       |
| Asking for information already present in project memory                              | Redundant. Memory exists specifically to prevent this.              |
| Presenting configuration conflicts as errors instead of advisories                    | Conflicts are advisory — they inform, they don't block without cause|
| Proceeding to content generation without a confirmed keyword                          | Keyword is the non-negotiable anchor of all content configuration   |
| Producing the structured config object in free prose instead of the defined format    | Structured format is required for machine readability by downstream skills |
| Asking for competitor URLs when AUTO is the preferred and sufficient default          | Unnecessarily burdens the user with data the system can fetch itself|
| Treating the validation step as optional when a keyword conflict is detected          | Deduplication is mandatory — keyword conflicts must always surface  |

### Required Question Display Behaviors

- Every option within a question must include: what it does, when to use it, and a recommendation flag.
- Every question must include an "Enter" prompt with valid input formats.
- Questions must be separated by the `━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━` border — no cramming multiple questions into one display block.
- Acknowledgment of user answers must be a maximum of one line.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ task-router.skill

| Property         | Detail                                                                              |
|------------------|-------------------------------------------------------------------------------------|
| **Direction**    | Router → pre-execution-input (triggers this skill for content requests)             |
| **Receives**     | Routed intent with any pre-extracted subject and parameters                         |
| **Returns**      | Nothing. Pre-execution-input sends its output directly to content-generation-engine.|

---

### ▸ memory-controller.skill

| Property         | Detail                                                                              |
|------------------|-------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional                                                                       |
| **Reads**        | Project niche, target audience, competitor URLs, existing keyword list, existing content titles |
| **Writes**       | Configuration record (strategy layer), keyword lifecycle state update (ASSIGNED)   |
| **Timing**       | READ at Step 1 (pre-fill). WRITE at Step 6 (post-config).                         |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                              |
|------------------|-------------------------------------------------------------------------------------|
| **Direction**    | pre-execution-input → content-generation-engine (one-way dispatch)                 |
| **Sends**        | Complete structured configuration object                                            |
| **Timing**       | Step 6 — after validation passes                                                   |

---

### ▸ keyword-memory.skill (via memory-controller)

| Property         | Detail                                                                              |
|------------------|-------------------------------------------------------------------------------------|
| **Direction**    | Read (dedup check), Write (lifecycle state update)                                 |
| **Used for**     | Keyword deduplication check (Step 5), keyword lifecycle state transition (Step 6)  |

---

### ▸ content-memory.skill (via memory-controller)

| Property         | Detail                                                                              |
|------------------|-------------------------------------------------------------------------------------|
| **Direction**    | Read only                                                                           |
| **Used for**     | Topic-level deduplication check (Section 6, Rule 2)                                |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                          | Detection                                     | Recovery Action                                                                          |
|-----------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| User says `"just do everything"` or full delegation phrase | Step 1 — delegation detection                 | Apply all recommended defaults. Confirm keyword only. Proceed.                           |
| Keyword cannot be collected or inferred after two attempts| Step 2, Question 2 — no response twice        | Halt. Display: "A keyword is required. Please enter a target keyword to proceed."         |
| User skips any non-keyword question                       | No response within question timeout            | Apply default for that field. Log. Continue to next question.                            |
| Memory controller is unavailable                          | READ query returns error                       | Skip pre-fill. Proceed with question collection. Flag: "Memory unavailable — pre-fill skipped." |
| Validation detects unresolvable conflict                  | Step 5 — conflict with no valid resolution     | Halt only if the conflict is keyword-level. For all others, present advisory and proceed on user confirmation. |
| User provides a keyword that exactly duplicates existing content | Step 5 — dedup check                   | Display warning. Require explicit YES to proceed. Do not auto-override.                  |
| All 10 questions answered but configuration still has NULL fields | Step 4 — normalization finds NULL      | Apply defaults for remaining NULL fields. This should not occur if question logic is followed correctly. |
| `content-generation-engine.skill` rejects the configuration | Step 6 — dispatch fails                    | Return the configuration to Step 5. Re-run validation. Identify the rejected field and re-collect. |
| User provides a word count below 300 and confirms         | Step 5 — word count range check, user says YES | Proceed with user-specified count. Add a note in the configuration: `"User confirmed short-form content."` |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — FULL DEFAULT CONFIGURATION (AUTO-MODE)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Applied when user fully delegates all decisions:

```
NEXUS AUTO-CONFIGURATION DEFAULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
content_type        : Blog Post
keyword             : [REQUIRED FROM USER — no default]
word_count          : AUTO
funnel              : TOFU
audience            : Intermediate
competitors         : AUTO
accuracy_mode       : High
tone                : Professional
depth               : Standard
goal                : Traffic
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Collection Method   : FULL_DEFAULT
Inference Log       : All fields set to recommended defaults
Conflict Check      : N/A (compatible default combination)
Keyword Dedup       : Pending user keyword input
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — QUESTION SEQUENCE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
QUESTION SEQUENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Q1   Content Type       → Ask if: not extracted from input
Q2   Keyword            → Ask if: not extracted AND not confidently inferred
Q3   Word Count         → Ask if: not extracted; Default = AUTO
Q4   Funnel Stage       → Ask if: not extracted AND not inferred from keyword/goal
Q5   Target Audience    → Ask if: not extracted AND not in project memory
Q6   Competitors        → Ask if: not extracted AND not in project memory; Default = AUTO
Q7   Accuracy Mode      → Ask if: not extracted AND not inferred from topic type
Q8   Tone               → Ask if: not extracted AND not inferred from audience/goal
Q9   Content Depth      → Ask if: not extracted AND not inferred from type/word count
Q10  Goal               → Ask if: not extracted AND not inferred from funnel/content type

Max questions asked in any single run: 10
Min questions asked (full inference): 0 (keyword confirmation only)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: content/pre-execution-input.skill*
*Nexus SEO Operating System — Version 1.0.0*
