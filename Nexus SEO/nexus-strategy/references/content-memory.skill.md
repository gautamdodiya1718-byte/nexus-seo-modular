# SKILL: memory/content-memory.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Memory / Content Intelligence
# EXECUTION PRIORITY: FOUNDATIONAL — Called before every content generation, optimization, and gap analysis operation. The canonical content state lives here.
# POSITION: Content intelligence persistence layer. Every piece of content that exists, is planned, or is in production for a project is registered, tracked, and lifecycle-managed here.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **single source of truth for all content data** in the Nexus SEO Operating System.

In any content-driven SEO program, content sprawl is the default failure mode. Without a centralized content record, the same topic gets written twice. A BOFU comparison article gets created for a keyword that already has a published comparison page. A content refresh is skipped because no one knows the existing article is 18 months old. A gap analysis recommends content that is already in draft. A new keyword is flagged as an opportunity while the published article targeting it is sitting at position 4 and needs only a minor update to reach position 1.

This skill prevents all of those failures by maintaining a complete, normalized, lifecycle-tracked record of every content item associated with a project — published pages, planned articles, active drafts, and archived content. It is the first skill called before any content is generated or planned, and it is the skill that decides whether a new content idea should be created, or whether existing content should be refreshed or optimized instead.

The principle is simple but absolute: **no new content should be created when existing content can be improved, and no topic should be written twice.**

### Primary Responsibilities

| Responsibility                          | Description                                                                                                           |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Content Intake**                      | Accept content data from sitemaps, CSV files, pipeline outputs, and manual inputs                                    |
| **Content Normalization**               | Standardize all URL formats, title casing, keyword assignments, intent labels, and cluster names                    |
| **Deduplication Gate**                  | Before storing any content record, check for URL duplicates, keyword duplicates, and semantic topic overlaps         |
| **Keyword Mapping and Inference**       | When a content item lacks a primary keyword, infer it from slug, title, and cluster data                            |
| **Status Assignment and Tracking**      | Classify and maintain content lifecycle status: published / planned / draft / archived                              |
| **Coverage Scoring**                    | Assign a 1–5 coverage score to every content item reflecting its depth and completeness                             |
| **Refresh Flag Detection**              | Automatically flag content that is stale (12+ months) or thin (coverage score ≤ 2) for refresh                    |
| **Optimization Tracking**              | Flag content with detected issues that need optimization before or independent of a refresh                          |
| **Duplicate Content Prevention**       | Before any content generation begins, check whether the topic, keyword, or intent already has content              |
| **Structured Retrieval**               | Return the full content inventory or filtered subsets by status, cluster, intent, or coverage level                 |
| **Reverse Keyword Mapping**            | Infer the primary keyword of existing content items from their URLs and titles when keyword data is absent          |
| **Content Lifecycle Evolution**        | Track how content progresses from planned → draft → published and how it ages over time                            |
| **Refresh Prioritization**             | Rank stale and thin content by strategic importance to guide refresh sequencing                                     |

### What This Skill Is NOT

- It is **not** a content generator. It stores and manages content records — it does not write content.
- It is **not** a SERP tracker. It stores static content metadata — it does not track live SERP positions.
- It is **not** an analytics tool. It stores content performance signals as flags — it does not pull live analytics data.
- It is **not** a CMS. It is a memory layer — it does not host or publish content.
- It is **not** a content quality grader. Coverage scores are assigned based on structural completeness signals — not writing quality assessments.

### The Defining Standard of This Skill

> Before any content is created, this skill is called.
> If existing content covers the topic → no new content is created.
> If existing content covers the topic but is thin or stale → a refresh is recommended instead.
> If no existing content covers the topic → new content generation proceeds.
>
> Enforce this at every call. No exceptions.
> The cost of a duplicate article is higher than the cost of the extra memory check.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — STORAGE STRUCTURE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Content Item Record Schema

Every content item is stored as a structured record:

```
CONTENT ITEM RECORD SCHEMA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Field                   Type          Required  Description
────────────────────────────────────────────────────────────────────────────
content_id              string        YES       System-generated UUID — unique identifier
project_id              string        YES       Foreign key reference to parent project in project-memory
url                     string        SOFT      Full canonical URL for published/planned content; null for draft with no URL yet
url_normalized          string        SOFT      Normalized URL form for dedup comparison (see Phase 2A)
url_slug                string        SOFT      Path component extracted from URL: /crm-software-for-startups/
title                   string        YES       The page title (H1 or document title)
title_normalized        string        YES       Lowercase, stripped version for dedup comparison
primary_keyword         string        YES       The single keyword this content is optimized to rank for
secondary_keywords      list          SOFT      List of secondary keywords (from metadata-generator or manual)
intent                  enum          YES       TOFU | MOFU | BOFU
cluster                 string        YES       The cluster name from semantic-clustering.skill
cluster_id              string        SOFT      The cluster's ID reference
status                  enum          YES       PUBLISHED | PLANNED | DRAFT | ARCHIVED
content_type            enum          YES       BLOG | LANDING | GUIDE | COMPARISON | TUTORIAL | FAQ | TOOL | CASE-STUDY
coverage_score          number        YES       1–5 integer score (see Coverage Score Definitions)
word_count              number        SOFT      Approximate word count of the content; null if not yet determined
published_date          string        SOFT      ISO 8601 date of initial publication; null if not yet published
last_updated            string        YES       ISO 8601 date of most recent content update OR creation date
last_reviewed           string        SOFT      ISO 8601 date this record was last reviewed for accuracy by a human
optimization_needed     boolean       YES       true if content has detected SEO issues; false otherwise
refresh_flag            boolean       YES       true if content needs a full content refresh; false otherwise
refresh_reason          string        SOFT      Specific reason refresh_flag was set: STALE | THIN | INTENT-MISMATCH | SERP-SHIFT | COMPETITOR-SURPASS
serp_difficulty         string        SOFT      EASY | MEDIUM | HARD — imported from serp-intelligence.skill if available
funnel_balance_notes    string        SOFT      Notes about how this piece fits the cluster's funnel coverage
internal_links_from     list          SOFT      List of URLs that link TO this page internally (from internal-linking.skill)
internal_links_to       list          SOFT      List of URLs this page links TO internally
aeo_optimized           boolean       SOFT      true if content has a direct answer block and AEO sections
faq_count               number        SOFT      Number of FAQ questions in the content
session_created         string        YES       session_id of the session that created this record
session_last_updated    string        YES       session_id of the most recent update to this record
source                  string        YES       How this record was created: SITEMAP | CSV | PIPELINE | MANUAL | CONTENT-GENERATION
notes                   string        SOFT      Free-text notes — optimization findings, strategic observations
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Status Enum Definitions

| Status        | Definition                                                                                  | Who Sets It                                |
|---------------|---------------------------------------------------------------------------------------------|--------------------------------------------|
| **PUBLISHED** | Content is live and accessible at its URL                                                   | User confirmation / content-inventory.skill|
| **PLANNED**   | Content is in the roadmap; not yet written                                                 | `roadmap-engine.skill`                    |
| **DRAFT**     | Content is being written or has been written but not yet published                         | `content-generation-engine.skill`         |
| **ARCHIVED**  | Content has been taken offline or is no longer active                                      | User action / content-inventory.skill     |

### Coverage Score Definitions

| Score | Label            | Definition                                                                                              |
|-------|------------------|---------------------------------------------------------------------------------------------------------|
| **1** | VERY THIN        | Minimal content (< 500 words); no structure; lacks examples, data, or depth; primarily a stub           |
| **2** | PARTIAL          | Has basic structure and some depth (500–1,200 words typical); misses key sub-topics or entities         |
| **3** | DECENT           | Covers the core topic with reasonable structure (1,200–2,500 words typical); most key elements present  |
| **4** | STRONG           | Comprehensive coverage with examples, data, entity depth, good structure (2,000–3,500 words typical)   |
| **5** | AUTHORITY        | Definitive resource; all entities, examples, comparisons, and AEO elements present; regularly updated  |

### Content Type Enum

| Type            | Definition                                                              |
|-----------------|-------------------------------------------------------------------------|
| **BLOG**        | Standard editorial article; informational or editorial format          |
| **LANDING**     | Conversion-focused page; typically BOFU                                |
| **GUIDE**       | Comprehensive, structured, long-form educational resource              |
| **COMPARISON**  | Side-by-side evaluation of multiple options; typically MOFU            |
| **TUTORIAL**    | Step-by-step instructional content                                     |
| **FAQ**         | Question-and-answer format; standalone FAQ page                        |
| **TOOL**        | Interactive or calculable resource                                     |
| **CASE-STUDY**  | Real-world example with outcomes and learnings                        |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Source                                | Data Provided                                                                              | Call Context            |
|---------------------------------------|--------------------------------------------------------------------------------------------|-------------------------|
| `website-analysis.skill`              | URL lists from sitemap discovery; page title extraction                                   | WRITE (bulk intake)     |
| `content-inventory.skill`             | Full normalized content records with all fields where available                           | WRITE (bulk intake)     |
| `content-generation-engine.skill`     | New content records at DRAFT → PUBLISHED transitions                                     | WRITE (status update)   |
| `optimization-engine.skill`           | optimization_needed updates with specific issue notes                                    | WRITE (flag update)     |
| `roadmap-engine.skill`                | New PLANNED content records when roadmap items are created                               | WRITE (new PLANNED)     |
| `user_pipeline_input`                 | Manually provided content lists, CSV uploads, URL lists                                  | WRITE (manual intake)   |
| `memory-controller.skill`             | All operations routed through memory-controller                                           | ALL                     |

### Read Query Types

| Query Type                         | Parameters                                              | Returns                                                    |
|------------------------------------|---------------------------------------------------------|------------------------------------------------------------|
| `GET_ALL`                          | project_id                                              | Complete content inventory for the project                 |
| `GET_BY_STATUS`                    | project_id + status                                     | All content items with the specified status                |
| `GET_BY_CLUSTER`                   | project_id + cluster_name                               | All content items in the specified cluster                 |
| `GET_BY_INTENT`                    | project_id + intent                                     | All content items with the specified funnel stage          |
| `CHECK_CONTENT`                    | project_id + url OR keyword OR topic                    | Duplication check — returns match data or null             |
| `GET_REFRESH_QUEUE`                | project_id                                              | All content items with refresh_flag = true, sorted by priority |
| `GET_OPTIMIZATION_QUEUE`           | project_id                                              | All content items with optimization_needed = true          |
| `GET_COVERAGE_MAP`                 | project_id                                              | Coverage map by cluster showing score distribution         |
| `GET_THIN_CONTENT`                 | project_id                                              | All content items with coverage_score ≤ 2                 |
| `GET_CONTENT_STATS`                | project_id                                              | Aggregate inventory statistics                              |
| `GET_BY_COVERAGE_SCORE`            | project_id + min_score + max_score                      | Content items within the specified score range             |

### Input Pre-Conditions

1. **Is a project context available?** All content records require a `project_id` — content cannot be stored without a parent project.
2. **Is the content source identified?** The `source` field must indicate how the record was created: SITEMAP / CSV / PIPELINE / MANUAL / CONTENT-GENERATION.
3. **Is a URL available for published content?** A PUBLISHED record without a URL is invalid — must provide URL or change status to DRAFT.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Output 1 — Write Confirmation

```
CONTENT WRITE CONFIRMATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
operation           : [STORE_NEW / UPDATE_STATUS / UPDATE_FLAGS / MERGE / ARCHIVE]
items_processed     : [count]
  successfully stored : [count]
  duplicates blocked  : [count]
  merged              : [count]
  errors              : [count]
project_id          : [UUID]
session_id          : [UUID]
timestamp           : [ISO 8601]
status              : SUCCESS / PARTIAL / FAILED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 2 — Structured Content Dataset (GET_ALL or GET_BY_* queries)

```
CONTENT DATASET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id              : [UUID]
total_items             : [count]
as_of                   : [ISO 8601]

STATUS BREAKDOWN:
  PUBLISHED             : [count] ([%])
  PLANNED               : [count] ([%])
  DRAFT                 : [count] ([%])
  ARCHIVED              : [count] ([%])

INTENT BREAKDOWN:
  TOFU                  : [count] ([%])
  MOFU                  : [count] ([%])
  BOFU                  : [count] ([%])

COVERAGE DISTRIBUTION:
  Score 5 (Authority)   : [count] ([%])
  Score 4 (Strong)      : [count] ([%])
  Score 3 (Decent)      : [count] ([%])
  Score 2 (Partial)     : [count] ([%])
  Score 1 (Thin)        : [count] ([%])

FLAGS:
  refresh_flag = true   : [count]
  optimization_needed   : [count]

CLUSTER COVERAGE SUMMARY:
  [cluster_name]        : [N] items | Avg Score: [N.N] | Coverage: [COMPLETE/PARTIAL/MISSING]
  ...

FULL CONTENT LIST: [see content item records]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 3 — Duplication Check Result (CHECK_CONTENT query)

```
DUPLICATION CHECK RESULT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
query_input         : "[URL / keyword / topic]"
check_1_url         : [MATCH — content_id: UUID | NO MATCH]
check_2_keyword     : [MATCH — content_id: UUID, status: [status] | NO MATCH]
check_3_semantic    : [MATCH — "[matched title]" (similarity: N%) | NO MATCH]
check_4_intent_cluster: [MATCH — "[matched content]" at [cluster] + [intent] | NO MATCH]
recommendation      : [ALLOW_NEW / BLOCK_DUPLICATE / REFRESH_EXISTING / OPTIMIZE_EXISTING]
existing_reference  : [content_id + url + title + status + coverage_score — if match found]
reason              : [specific reason for recommendation]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output 4 — Content Coverage Insights (GET_COVERAGE_MAP query)

```
CONTENT COVERAGE MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
project_id              : [UUID]
total_clusters          : [count]
clusters_fully_covered  : [count]
clusters_partial        : [count]
clusters_missing        : [count]

COVERAGE BY CLUSTER:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster: [name]
  Status: [COMPLETE / PARTIAL / MISSING]
  Items: [N published] / [N total]
  TOFU: [Y/N] | MOFU: [Y/N] | BOFU: [Y/N]
  Avg Coverage Score: [N.N]
  Pillar Published: [YES — [url] / NO]
  refresh_flag items: [count]
  optimization_needed: [count]
...

CONTENT GAPS DETECTED:
  Clusters with no BOFU: [list]
  Clusters with no published content: [list]
  Thin content clusters (avg score ≤ 2): [list]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — CONTENT INTAKE

Accept content data from all available input sources and prepare it for normalization and storage.

**Phase 1A — Intake Source Identification**

| Intake Source        | Format                             | Processing Path                                        |
|----------------------|------------------------------------|--------------------------------------------------------|
| **Sitemap**          | XML sitemap with URLs              | Extract all URLs; fetch title and meta from each page  |
| **CSV**              | Structured CSV with known columns  | Map CSV columns to schema fields; infer missing fields |
| **Manual input**     | User-provided URL list or single URL | Parse; apply normalization; flag missing fields      |
| **Content pipeline** | Direct output from content-generation-engine | Full record already structured; validate + store |
| **Content inventory**| Output from content-inventory.skill | Pre-normalized records; validate + store              |

**Phase 1B — Sitemap Intake Protocol**

When a sitemap is provided:
1. Parse all `<loc>` URL entries from the XML.
2. For each URL: extract the path component as `url_slug`.
3. Derive a working title from the slug (hyphen-separated → Title Case) as a provisional `title` until the actual page title is retrieved.
4. Set `source` = SITEMAP.
5. Set `status` = PUBLISHED (URLs in a sitemap are live pages).
6. Set all unmapped fields to null — they will be populated by `content-inventory.skill` enrichment.

**Phase 1C — CSV Intake Protocol**

When a CSV is provided:
1. Detect and map CSV column headers to schema field names (case-insensitive, spaces tolerated).
2. Standard column mappings:

| CSV Column (common variations)           | Schema Field        |
|------------------------------------------|---------------------|
| url / URL / page_url / link              | url                 |
| title / page_title / h1                  | title               |
| keyword / primary_keyword / target_kw    | primary_keyword     |
| status / content_status                  | status              |
| word_count / words                       | word_count          |
| cluster / topic_cluster / topic          | cluster             |
| intent / funnel / funnel_stage           | intent              |
| type / content_type / format             | content_type        |
| score / coverage / quality_score         | coverage_score      |
| updated / last_updated / date            | last_updated        |

3. For any unrecognized column → store as note.
4. For any required field not present in the CSV → mark for inference in Steps 2 and 4.

**Phase 1D — Single URL Intake**

When a single URL is provided manually:
1. Normalize the URL.
2. Extract slug → provisional title.
3. All other fields are flagged as INFERRED or set to null for manual review.
4. Trigger Steps 4 (keyword mapping) and 6 (coverage scoring) to populate missing fields.

**Phase 1E — Pipeline Intake Protocol**

When content arrives from `content-generation-engine.skill`:
1. All mandatory fields should already be populated from the generation pipeline.
2. Verify all required fields are present.
3. Any missing required field → halt the storage of that specific record; return an error identifying the missing field. Do not store a partial record from a pipeline output.

**Phase 1F — Intake Minimum Viability Check**

Before proceeding to normalization, every record must have at minimum:
- `title` OR `url` (at least one of these two is required).
- `project_id`.
- `status` (or a strong signal from which it can be inferred).

If neither title nor URL is present → reject the record; log: `"Record rejected — insufficient intake data: neither title nor URL provided."`

---

### ▸ STEP 2 — NORMALIZATION

Standardize all field values to ensure consistent comparison and storage.

**Phase 2A — URL Normalization**

Apply URL normalization before any URL-based lookup or storage:

1. Strip protocol: remove `https://`, `http://`
2. Strip `www.` subdomain if present.
3. Lowercase the entire URL.
4. Remove trailing slash from the path component.
5. Remove URL parameters (everything after `?`).
6. Remove fragment identifiers (everything after `#`).
7. Store the result as `url_normalized`.

| Raw URL                                          | Normalized                                  |
|--------------------------------------------------|---------------------------------------------|
| `https://www.example.com/crm-guide/`            | `example.com/crm-guide`                    |
| `http://example.com/CRM-Guide?ref=blog#intro`   | `example.com/crm-guide`                    |
| `https://blog.example.com/crm-software/`        | `blog.example.com/crm-software`            |

**Phase 2B — Title Normalization**

1. Strip leading and trailing whitespace.
2. Store the normalized version (`title_normalized`) as lowercase with all punctuation removed.
3. The display `title` retains its original casing.

| Original Title                                          | title_normalized                              |
|---------------------------------------------------------|-----------------------------------------------|
| `"Best CRM Software for Small Business (2024 Guide)"`  | `"best crm software for small business 2024 guide"` |
| `"  How to Choose a CRM  "`                            | `"how to choose a crm"`                       |

**Phase 2C — Status Normalization**

Map all status variants to the defined enum:

| Input Variant                                         | Normalized Status |
|-------------------------------------------------------|-------------------|
| "live", "published", "public", "active", "1"         | PUBLISHED         |
| "planned", "scheduled", "future", "backlog"          | PLANNED           |
| "draft", "wip", "in progress", "writing", "0"        | DRAFT             |
| "archived", "deleted", "deprecated", "unpublished"   | ARCHIVED          |

Unrecognized status → default to PLANNED with a flag: `"STATUS INFERRED: unrecognized value '[value]' → defaulted to PLANNED. Review."`

**Phase 2D — Intent Normalization**

Map all intent variants to the defined enum:

| Input Variant                                         | Normalized Intent |
|-------------------------------------------------------|-------------------|
| "informational", "tofu", "awareness", "tofu"          | TOFU              |
| "commercial", "mofu", "evaluation", "consideration"   | MOFU              |
| "transactional", "bofu", "conversion", "decision"    | BOFU              |

**Phase 2E — Cluster Name Normalization**

Cluster names must match the canonical cluster names from `semantic-clustering.skill`:
1. Load the canonical cluster name list from `keyword-memory.skill` (which tracks cluster names from the clustering output).
2. For each content record's `cluster` field: match against the canonical list (case-insensitive).
3. If a match is found → use the canonical cluster name.
4. If no match → retain the provided cluster name but flag as: `"CLUSTER NOT IN CANONICAL LIST — verify against semantic-clustering output"`.

**Phase 2F — Content Type Normalization**

Map all content type variants to the defined enum:

| Input Variant                                          | Normalized Type   |
|--------------------------------------------------------|-------------------|
| "blog", "post", "article", "blog post"                | BLOG              |
| "landing", "landing page", "product page", "lp"      | LANDING           |
| "guide", "ultimate guide", "complete guide"           | GUIDE             |
| "comparison", "vs", "versus", "compared"              | COMPARISON        |
| "tutorial", "how to", "how-to", "step by step"       | TUTORIAL          |
| "faq", "q&a", "questions"                             | FAQ               |
| "tool", "calculator", "template"                      | TOOL              |
| "case study", "case-study", "study"                   | CASE-STUDY        |

Unknown type → BLOG as default; flag for review.

---

### ▸ STEP 3 — DEDUPLICATION GATE

Before storing any content record, run four checks to detect duplicate or overlapping content.

**Phase 3A — Check 1: URL Match**

1. Query the URL index: `content:index:[project_id]:url:[url_normalized]`.
2. If a match is found → **BLOCK — URL DUPLICATE**.
3. Log: `"Content record blocked — URL '[url_normalized]' already exists as content_id [UUID], status: [status]."`

Exception: If the URL match is for an ARCHIVED record → do NOT block. Flag: `"URL matches an ARCHIVED record. Storing as new active record. Old record ID: [UUID]."`

**Phase 3B — Check 2: Primary Keyword Match**

1. Normalize the incoming `primary_keyword`.
2. Query the keyword index: `content:index:[project_id]:keyword:[normalized_keyword]`.
3. If a match exists for a PUBLISHED or PLANNED or DRAFT record:
   - Return the existing record's details.
   - Recommend action based on its status and coverage_score:

| Existing Record Condition                           | Recommendation                                                              |
|-----------------------------------------------------|-----------------------------------------------------------------------------|
| PUBLISHED + coverage_score ≥ 4                      | BLOCK — content already exists at strong quality. Optimize if needed.      |
| PUBLISHED + coverage_score 3                        | BLOCK — content exists. Consider optimization to improve quality.          |
| PUBLISHED + coverage_score ≤ 2                      | BLOCK — thin content exists. REFRESH recommended over new creation.        |
| PLANNED                                             | BLOCK — content is already in roadmap. Update roadmap item if changes needed.|
| DRAFT                                               | BLOCK — draft exists. Complete and publish the draft rather than starting new. |

**Phase 3C — Check 3: Semantic Topic Match**

When no exact URL or keyword match exists, check for semantic topic similarity:

1. Extract the topic signal from the incoming record: `title_normalized + primary_keyword + cluster`.
2. For each existing record in the same cluster, calculate semantic similarity between their combined topic signals.
3. If similarity ≥ 80% → **HIGH SIMILARITY — flag for review**. The new record is NOT automatically blocked, but the match is surfaced.
4. Present to the calling skill or user:

```
SEMANTIC MATCH DETECTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Incoming topic  : "[incoming title / keyword]"
Similar to      : "[matched title]"
Similarity      : [N%]
Existing URL    : [url]
Existing Status : [status]
Coverage Score  : [N/5]
Recommendation  : [REVIEW — existing content may serve this topic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

5. If the calling skill confirms the new content IS distinct → ALLOW storage.
6. If uncertain → flag incoming record as REVIEW-NEEDED; do not auto-block or auto-allow.

**Phase 3D — Check 4: Intent-Cluster Overlap**

Beyond keyword and topic similarity, check for same-intent content that already serves the same cluster position:

A cluster-intent overlap exists when:
- Incoming content has the same `cluster` as an existing record.
- Incoming content has the same `intent` as the existing record.
- The existing record has `coverage_score ≥ 3`.

If overlap is found AND the existing content is PUBLISHED:
- Recommendation: `OPTIMIZE_EXISTING` — improve the existing piece rather than creating a new one.
- Flag: `"CLUSTER-INTENT OVERLAP: [cluster] already has a [intent] piece at coverage score [N]: '[existing title]' — consider optimizing it instead."`

**Phase 3E — Dedup Gate Summary**

After all four checks:

```
DEDUP GATE SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Check 1 URL match         : [PASS / BLOCK]
Check 2 Keyword match     : [PASS / BLOCK / SOFT-BLOCK — details]
Check 3 Semantic match    : [PASS / FLAG — N% similarity]
Check 4 Cluster-intent    : [PASS / FLAG — existing content found]
Final decision            : [ALLOW_STORE / BLOCK / FLAG_REVIEW / REFRESH_EXISTING / OPTIMIZE_EXISTING]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 4 — KEYWORD MAPPING

When a content record is missing a primary keyword, infer it from available data.

**Phase 4A — Keyword Inference Priority**

Apply these inference methods in priority order:

| Priority | Method                                                    | Accuracy             |
|----------|-----------------------------------------------------------|----------------------|
| 1        | URL slug decomposition                                    | HIGH — slugs are typically keyword-optimized |
| 2        | Title keyword extraction                                  | HIGH — titles contain the primary keyword explicitly |
| 3        | Cluster-based keyword lookup via `keyword-memory.skill`  | MEDIUM — uses the cluster's primary keyword as the default |
| 4        | Secondary keywords list (most prominent)                 | LOW — secondary keywords may not be the primary keyword |

**Phase 4B — URL Slug Decomposition**

1. Extract the slug from the URL: everything after the domain and before query parameters.
2. Split on hyphens: `/best-crm-software-for-startups/` → `["best", "crm", "software", "for", "startups"]`.
3. Remove common stop words: `a, an, the, for, and, or, of, in, to, is, on, by, at, as, with`.
4. Reconstruct: `"best crm software startups"`.
5. Cross-reference this string against `keyword-memory.skill` to find if it exactly matches a stored keyword.
6. If it matches → use the stored keyword as `primary_keyword`.
7. If it does not match → use the reconstructed string as the provisional `primary_keyword`; flag as INFERRED.

**Phase 4C — Title Keyword Extraction**

If slug decomposition produces no match:
1. Take `title_normalized`.
2. Remove generic title qualifiers ("a complete guide to", "everything you need to know about", "the best", year formats).
3. Match the remaining phrase against `keyword-memory.skill` stored keywords.
4. If a match is found → use it as `primary_keyword`.
5. If not → use the most keyword-dense phrase from the title as the provisional `primary_keyword`; flag as INFERRED.

**Phase 4D — Keyword Inference Flag**

All inferred keywords are flagged:

```
> **📌 INFERRED KEYWORD:** primary_keyword = "[inferred keyword]"
> Source: [URL SLUG / TITLE / CLUSTER DEFAULT]
> Confidence: [HIGH / MEDIUM / LOW]
> Review: Verify this is the correct target keyword for this content.
```

---

### ▸ STEP 5 — STATUS ASSIGNMENT

Assign or confirm the content item's lifecycle status.

**Phase 5A — Status Signal Detection**

When a record arrives without an explicit status, infer from available signals:

| Signal                                                             | Inferred Status |
|--------------------------------------------------------------------|-----------------|
| URL is a fully formed live URL (confirmed accessible)             | PUBLISHED       |
| URL exists in the sitemap                                         | PUBLISHED       |
| Record comes from `roadmap-engine.skill` (new roadmap item)       | PLANNED         |
| Record comes from `content-generation-engine.skill` (in writing)  | DRAFT           |
| Record has no URL and was not from a known source                 | PLANNED         |
| Record has a URL but it returns a 301/404                         | ARCHIVED        |

**Phase 5B — Status Validation**

Validate status against available field data:

| Status Assigned | Required Field Validation                                                     |
|-----------------|-------------------------------------------------------------------------------|
| PUBLISHED       | Must have `url` — reject PUBLISHED status without a URL                      |
| PLANNED         | Must have `primary_keyword` or `title` — can exist without URL              |
| DRAFT           | Must have `title` — partially written content must have a working title      |
| ARCHIVED        | No field requirement — archiving can proceed on minimal data                 |

**Phase 5C — Status History Tracking**

Every status transition is logged:
- Prior status and new status are recorded.
- Timestamp of the transition is stored.
- The session_id that triggered the transition is stored.

This history enables lifecycle velocity analysis (how long content spends in each status stage) and is accessible via `GET_CONTENT_STATS`.

---

### ▸ STEP 6 — COVERAGE SCORING

Assign or update the coverage score for each content item based on available quality signals.

**Phase 6A — Coverage Score Assignment Protocol**

Coverage scores are assigned using this priority:

1. **Explicit score from `content-inventory.skill`** → Use directly; most reliable.
2. **Score derived from word count and structural analysis** → Apply the estimation matrix below.
3. **Score inferred from title and type alone** → Apply the weakest default.

**Phase 6B — Word Count-Based Score Estimation**

| Word Count Range     | Content Type Context           | Score Estimate |
|----------------------|--------------------------------|----------------|
| < 500 words          | Any                            | 1 (VERY THIN)  |
| 500–1,199 words      | Blog / Guide                   | 2 (PARTIAL)    |
| 500–1,199 words      | FAQ / Comparison               | 2–3            |
| 1,200–2,499 words    | Blog / Guide                   | 3 (DECENT)     |
| 1,200–2,499 words    | Comparison / GUIDE             | 3–4            |
| 2,500–3,999 words    | Blog / Guide / Comparison      | 4 (STRONG)     |
| ≥ 4,000 words        | Guide / Comparison             | 4–5            |

Word count is a proxy — the actual score must also reflect entity coverage, structure quality, and AEO optimization. When only word count is available, use the estimate with a flag: `"SCORE ESTIMATED from word count — verify with content audit."`

**Phase 6C — Structural Quality Modifiers**

When additional structural data is available, apply modifiers to the base word-count score:

| Positive Signal                                               | Modifier |
|---------------------------------------------------------------|----------|
| AEO direct answer block present (`aeo_optimized = true`)    | +0.5     |
| FAQ section present (`faq_count ≥ 3`)                        | +0.5     |
| Internal links present to/from this content (≥ 3)           | +0.5     |
| Content has a published date within the last 12 months       | +0.5     |

| Negative Signal                                               | Modifier |
|---------------------------------------------------------------|----------|
| Content has no H2 structure detectable                       | -0.5     |
| No examples or statistics detectable (keyword-only analysis) | -0.5     |
| Content was last updated > 18 months ago                     | -1.0     |

Final score = base estimate + Σ(positive modifiers) - Σ(negative modifiers). Cap at 5; floor at 1. Round to nearest integer.

**Phase 6D — Score Inflation Prevention**

The following conditions OVERRIDE any calculated score and force a lower value:

| Override Condition                                          | Forced Score |
|-------------------------------------------------------------|--------------|
| Word count < 500 words                                      | Maximum 1    |
| Word count < 1,000 words + no structural signals           | Maximum 2    |
| Content is a LANDING page type                              | Maximum 3 (landing pages are focused, not comprehensive) |

This prevents a heavily-linked thin page from being scored as AUTHORITY by modifier inflation.

---

### ▸ STEP 7 — REFRESH FLAG DETECTION

Identify content that needs a full content refresh rather than minor optimization.

**Phase 7A — Refresh Triggers**

A content item receives `refresh_flag = true` when ANY of the following conditions are met:

| Trigger                                                           | `refresh_reason` Value  |
|-------------------------------------------------------------------|-------------------------|
| `last_updated` date is > 12 months ago                           | STALE                   |
| `coverage_score` ≤ 2                                             | THIN                    |
| A confirmed SERP intent shift exists for this keyword (from `serp-intelligence.skill`) | INTENT-MISMATCH |
| A competitor has published significantly stronger content for the same keyword | COMPETITOR-SURPASS |
| The content's primary keyword has shifted clusters (re-clustering event) | SERP-SHIFT        |

**Phase 7B — Multi-Trigger Handling**

When multiple refresh triggers fire simultaneously, `refresh_reason` stores all applicable reasons as a comma-separated list:
`"STALE, THIN"` or `"STALE, COMPETITOR-SURPASS"`.

**Phase 7C — Refresh vs. Optimization Decision**

| Condition                                           | Flag                                       |
|-----------------------------------------------------|--------------------------------------------|
| `refresh_flag = true` AND `coverage_score ≤ 2`     | Full rewrite needed (refresh priority)    |
| `refresh_flag = true` AND `coverage_score ≥ 3`     | Substantive update needed (refresh with most content preserved) |
| `refresh_flag = false` AND `optimization_needed = true` | Targeted optimization (no full rewrite)  |
| Both `refresh_flag = false` AND `optimization_needed = false` | No action needed; MAINTAIN           |

**Phase 7D — Automatic Stale Date Calculation**

At every session start, run a stale check across all PUBLISHED content:
1. For every PUBLISHED content item where `refresh_flag = false`:
2. Calculate `days_since_update = today - last_updated`.
3. If `days_since_update > 365` → set `refresh_flag = true`; set `refresh_reason = STALE`.
4. Log each newly flagged item in the stale detection report.

---

### ▸ STEP 8 — OPTIMIZATION TRACKING

Flag content that has SEO issues requiring targeted optimization — without a full rewrite.

**Phase 8A — Optimization Triggers**

A content item receives `optimization_needed = true` when ANY of the following are detected by `optimization-engine.skill`:

| Trigger                                                             | Notes Added                                           |
|---------------------------------------------------------------------|-------------------------------------------------------|
| Primary keyword missing from H1                                     | "H1 keyword gap detected"                            |
| Primary keyword missing from meta title                            | "Meta title keyword gap detected"                    |
| No direct answer block (AEO gap)                                   | "Missing AEO direct answer block"                    |
| Internal links below minimum threshold (< 2 internal links)       | "Insufficient internal links"                        |
| FAQ section absent when SERP pattern requires it                   | "Missing FAQ section"                                |
| Coverage score dropped from prior assessment                       | "Coverage score declined: [old] → [new]"            |
| Title H1 cannibalization detected with another page               | "Keyword cannibalization: [competing URL]"           |

**Phase 8B — optimization_needed Reset**

When `optimization-engine.skill` confirms that optimization issues have been addressed:
1. Set `optimization_needed = false`.
2. Update `last_updated` to current timestamp.
3. Update `last_reviewed` to current timestamp.
4. Clear the `notes` field of optimization flag entries (or append "RESOLVED: [date]").
5. Recalculate `coverage_score` based on current content state.

---

### ▸ STEP 9 — DUPLICATE CONTENT PREVENTION

Before any new content generation is initiated, run the full duplication prevention check.

**Phase 9A — Pre-Generation Check Protocol**

This is NOT the same as the storage dedup gate (Step 3). The pre-generation check runs BEFORE content creation begins — at the point when a skill asks "should I create content for this keyword/topic?" The storage dedup gate runs AFTER content has been produced and is being stored.

The pre-generation check runs the same four checks as Step 3 but in a READ-ONLY context — it returns a recommendation without making any storage changes.

**Phase 9B — Pre-Generation Decision Logic**

```
PRE-GENERATION DECISION LOGIC
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
INPUT: keyword OR topic OR URL

If URL match found (any status except ARCHIVED):
  → BLOCK — content exists: return [url, title, status, coverage_score]

If keyword match found AND status = PUBLISHED:
  AND coverage_score ≥ 3:
    → OPTIMIZE — content is adequate; optimize instead of rewriting
  AND coverage_score ≤ 2:
    → REFRESH — content exists but is thin; refresh instead of creating new
  AND refresh_flag = true:
    → REFRESH — content is stale; refresh is the priority action

If keyword match found AND status = PLANNED:
  → BLOCK — already in roadmap: return roadmap item details

If keyword match found AND status = DRAFT:
  → BLOCK — draft exists: complete the draft rather than starting new

If keyword match found AND status = ARCHIVED:
  → ALLOW WITH NOTICE — prior version exists but was archived; new version acceptable

If semantic topic match ≥ 80% found:
  → REVIEW — similar content exists; confirm new content is distinct before proceeding

If cluster + intent match found AND existing PUBLISHED score ≥ 3:
  → REVIEW — position in cluster already filled; new content may cannibalize

If NO match found across all checks:
  → ALLOW — no existing content detected; proceed with generation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 9C — Refresh Priority Enforcement**

When the pre-generation decision returns REFRESH → the calling skill must communicate to the user:

```
> **📌 REFRESH RECOMMENDED:**
> Existing content found for this topic: **"[existing title]"** ([url])
> Status: PUBLISHED | Coverage Score: [N/5] | Last Updated: [N] months ago
>
> A **content refresh** is recommended over creating new content.
> Refreshing the existing piece is:
> → Faster to impact (existing page has authority signals)
> → Better for SEO (avoids keyword cannibalization)
> → More resource-efficient than building from scratch
>
> Proceed with refresh? (Y to refresh / N to create new anyway)
```

If the user insists on creating new content despite the REFRESH recommendation → allow it but log: `"NEW CONTENT CREATED OVER REFRESH RECOMMENDATION for keyword '[keyword]'. Existing content: [content_id]."`

---

### ▸ STEP 10 — RETRIEVAL

Return structured content data to requesting skills based on the query type.

**Phase 10A — Query Routing**

Route each query to the appropriate retrieval method:

| Query Type               | Index Used                                                             |
|--------------------------|------------------------------------------------------------------------|
| `GET_ALL`                | Load all content records for project                                   |
| `GET_BY_STATUS`          | Query `content:index:[project_id]:status:[status]`                    |
| `GET_BY_CLUSTER`         | Query `content:index:[project_id]:cluster:[cluster_name]`             |
| `GET_BY_INTENT`          | Query `content:index:[project_id]:intent:[intent]`                    |
| `CHECK_CONTENT`          | Run pre-generation check (Phase 9B) — READ-ONLY                       |
| `GET_REFRESH_QUEUE`      | Query status=PUBLISHED + refresh_flag=true; sort by refresh priority  |
| `GET_OPTIMIZATION_QUEUE` | Query optimization_needed=true; sort by coverage_score ascending      |
| `GET_COVERAGE_MAP`       | Aggregate cluster × status × score data                               |
| `GET_THIN_CONTENT`       | Query coverage_score ≤ 2; sort by score ascending                    |
| `GET_CONTENT_STATS`      | Calculate all aggregate statistics                                     |
| `GET_BY_COVERAGE_SCORE`  | Filter by score range; sort by score ascending                        |

**Phase 10B — Refresh Queue Priority Sorting**

When returning the refresh queue, sort by strategic priority:

1. BOFU content with `refresh_flag = true` → highest priority (revenue path).
2. Pillar pages (primary_keyword = true in keyword-memory) with `refresh_flag = true` → second priority.
3. MOFU content with `refresh_flag = true` → third priority.
4. TOFU content with `refresh_flag = true` → fourth priority.
5. Within each tier → sort by `coverage_score` ascending (lowest quality refreshes first for maximum impact).

**Phase 10C — Coverage Insights Generation**

When returning coverage data, generate specific insight statements:

For every cluster with MISSING coverage (no PUBLISHED content): `"CLUSTER GAP: [cluster_name] has [N] PLANNED items but no published content."`

For every cluster with coverage_score average ≤ 2: `"THIN CLUSTER: [cluster_name] has published content but average coverage score is [N.N] — refresh cycle recommended."`

For every cluster with BOFU = null (no BOFU PUBLISHED item): `"NO CONVERSION PATH: [cluster_name] has no published BOFU content — organic traffic cannot convert in this cluster."`

**Phase 10D — Enriched Return Fields**

At retrieval, calculate these derived fields for each content record:

```
DERIVED FIELDS AT RETRIEVAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
days_since_published   : today - published_date (if PUBLISHED)
days_since_updated     : today - last_updated
age_label              : FRESH (<3mo) / CURRENT (3–12mo) / AGING (12–18mo) / STALE (>18mo)
action_recommended     : MAINTAIN / OPTIMIZE / REFRESH / URGENT-REFRESH
urgency_score          : 1–5 (based on refresh_flag, optimization_needed, days_since_updated, coverage_score)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ SEMANTIC SIMILARITY DETECTION

The semantic similarity check (Step 3C and Step 9B) uses a layered text comparison approach.

**Similarity Calculation Method:**

1. **Token overlap ratio:** Shared meaningful words divided by the total unique words across both titles.
2. **Slug overlap:** Extract slugs from both URLs; calculate shared word ratio.
3. **Keyword proximity:** If both content items have primary keywords, compare them using the same method as `keyword-memory.skill`'s semantic check.
4. **Combined similarity:** Weighted average of the three signals (token: 40%, slug: 30%, keyword: 30%).

| Combined Similarity | Classification                                                |
|---------------------|---------------------------------------------------------------|
| ≥ 80%               | SEMANTIC DUPLICATE — surfaces for blocking or review         |
| 60–79%              | HIGH SIMILARITY — surfaces as notice; does not block         |
| < 60%               | DISTINCT — no similarity flag                                |

---

### ▸ REVERSE KEYWORD MAPPING

When content records arrive without a `primary_keyword` field (common in bulk sitemap imports), apply reverse keyword mapping to infer it.

**Reverse Mapping Pipeline:**

1. Slug decomposition (Phase 4B) → provisional keyword.
2. Cross-reference with `keyword-memory.skill` stored keywords for the project.
3. Cross-reference with `semantic-clustering.skill` cluster keyword lists.
4. If a match is found at any stage → use the matched keyword as `primary_keyword`.
5. Store the inference method in `notes`.

**Reverse Mapping Confidence:**

| Match Type                                          | Confidence   | Flag Applied                        |
|-----------------------------------------------------|--------------|-------------------------------------|
| Exact match with keyword in keyword-memory          | HIGH         | No flag — use directly             |
| Partial match (3+ words shared)                     | MEDIUM       | Flag as INFERRED                   |
| Cluster default (no close match found)              | LOW          | Flag as INFERRED — verify          |
| No match at any stage                               | NONE         | primary_keyword = null; flag REVIEW |

---

### ▸ CONTENT LIFECYCLE TRACKING

Track how content moves through its lifecycle and generate velocity metrics.

**Lifecycle Velocity Metrics:**

| Metric                                   | Calculation                                                                        |
|------------------------------------------|------------------------------------------------------------------------------------|
| `avg_time_planned_to_published`         | Average days from PLANNED status to PUBLISHED status across all completed items   |
| `avg_time_draft_to_published`           | Average days from DRAFT to PUBLISHED                                              |
| `avg_score_at_publication`              | Average coverage score at initial publication                                     |
| `avg_days_before_refresh_needed`        | Average days from publication to first refresh_flag = true                       |
| `refresh_backlog_size`                  | Count of PUBLISHED items with refresh_flag = true                                |
| `publication_velocity`                  | Published items per session (total PUBLISHED / sessions_count)                   |

These metrics are returned as part of `GET_CONTENT_STATS` and fed to `strategist-ai.skill` for strategic context.

---

### ▸ REFRESH PRIORITIZATION

Rank refresh candidates by the strategic value of refreshing them.

**Refresh Priority Score Formula:**

`Refresh_Priority = (Intent_Weight × 0.35) + (Coverage_Gap × 0.30) + (Staleness × 0.20) + (Authority_Multiplier × 0.15)`

Where:
- `Intent_Weight`: BOFU = 100, MOFU = 70, TOFU = 40.
- `Coverage_Gap`: `(3 - coverage_score) × 25` — lower score = higher gap = higher priority.
- `Staleness`: `min(days_since_update / 365, 1.0) × 100` — normalized to 0–100.
- `Authority_Multiplier`: `internal_links_from.count × 10` (capped at 100) — pages with more incoming links produce more impact when refreshed.

Higher Refresh Priority Score = should be refreshed sooner.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate URLs

Each normalized URL may have exactly one active (non-ARCHIVED) content record per project. Attempting to store a second record with the same `url_normalized` is blocked. An ARCHIVED record with the same URL does not block new storage — the new version supersedes the archived version.

### Rule 2 — No Duplicate Primary Keywords for Active Content

Each primary keyword may be associated with at most one PUBLISHED or DRAFT content record per project. Two pieces of content cannot both target the same primary keyword. If a new record tries to register the same primary keyword as an existing active record → apply Phase 3B blocking logic.

### Rule 3 — No Duplicate Intent-Topic Combinations for the Same Cluster

Within a single cluster, no two active (PUBLISHED, PLANNED, or DRAFT) records may serve the same intent (TOFU/MOFU/BOFU) with ≥ 80% semantic topic similarity. The cluster-intent-topic combination must be unique.

### Rule 4 — No Duplicate Content IDs

Every content record has a system-generated UUID as its `content_id`. UUID collision is theoretically possible but practically infinitesimal. If a collision is detected → regenerate the UUID for the new record and log the collision.

### Rule 5 — No Content Records Without Project Context

Every content record must reference a valid `project_id`. A record without a project_id is not stored. A record with an invalid project_id (referencing a project that does not exist in project-memory) is rejected with: `"Invalid project_id — project not found. Ensure project-memory.skill has a record for this project."`

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Content Memory Behaviors

| Prohibited Behavior                                                                             | Reason                                                                           |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Storing a record with `primary_keyword = null` without flagging it as INFERRED or REVIEW       | Every content item must have a keyword mapping strategy — null with no flag is invisible |
| Assigning a coverage_score of 4 or 5 based only on word count without structural signals      | High coverage scores must be earned — word count alone is not sufficient evidence |
| Setting `refresh_flag = false` for content that is > 24 months old                            | Content older than 24 months in any non-archived status must be flagged as STALE  |
| Allowing a new content record to overwrite an existing PUBLISHED record's URL without archiving the old one | URL reassignment without archiving destroys historical record integrity |
| Storing a content record in PUBLISHED status without a URL                                     | PUBLISHED content must have a URL — there is no published page without an address|
| Lowering a coverage_score without a documented reason                                           | Score changes must be traceable — unexplained downgrades are not auditable       |
| Assigning cluster = "general" or cluster = "miscellaneous" or any non-specific cluster name   | Vague cluster assignments produce useless coverage maps                          |
| Running the pre-generation check without returning the existing record's URL and coverage score | The check is only useful if it returns the specific content the calling skill should update |
| Auto-clearing `optimization_needed` without confirmation that optimization was actually applied | The flag can only be cleared by confirmation that the issues were addressed       |

### Required Content Memory Behaviors

- Every stored record must have `title`, `primary_keyword` (or INFERRED flag), `cluster`, `intent`, `status`, and `coverage_score`.
- Stale date check runs on every session start for all PUBLISHED content.
- Every duplication block must return the existing record's details.
- Every pre-generation REFRESH recommendation must include the existing URL and coverage score.
- Storage key indexes must be updated synchronously with record writes.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 9 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-awareness.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-memory → content-awareness (primary data source)                                   |
| **Sends**        | Full content dataset (GET_ALL or GET_BY_CLUSTER), coverage map, refresh queue             |
| **Used for**     | content-awareness.skill builds its coverage map from this memory layer's data             |

---

### ▸ deduplication-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-memory → deduplication-engine (query responder)                                    |
| **Receives**     | CHECK_CONTENT queries — keyword, URL, or topic to check                                   |
| **Sends back**   | Deduplication check result with recommendation and existing content reference              |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | content-memory → gap-opportunity-engine (data source)                                      |
| **Sends**        | Full content dataset including coverage map and cluster coverage status                    |
| **Used for**     | Gap analysis compares the cluster keyword universe against this content inventory to detect gaps |

---

### ▸ optimization-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — reads content records; writes optimization flags back                      |
| **Reads**        | GET_OPTIMIZATION_QUEUE — fetches all content where optimization_needed = true             |
| **Writes**       | Sets optimization_needed = true/false; updates coverage_score after optimization           |

---

### ▸ website-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream intake source                                                                      |
| **Sends**        | URL lists from sitemap discovery; provisional content records                              |
| **Protocol**     | Bulk intake — triggers Phase 1B (sitemap) or Phase 1D (URL list)                          |

---

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream intake source AND data enrichment source                                          |
| **Sends**        | Fully normalized content records with all available metadata                               |
| **Protocol**     | Bulk intake — triggers Phase 1E (pipeline intake); enriches records from sitemap intake   |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — queries memory before generation; writes new records after generation      |
| **Queries**      | CHECK_CONTENT — pre-generation duplication check before writing begins                    |
| **Writes**       | New DRAFT records on generation start; PUBLISHED records on publication confirmation      |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — reads content state; writes planned content records                        |
| **Reads**        | GET_BY_STATUS(PUBLISHED) to know what exists; GET_REFRESH_QUEUE for refresh scheduling    |
| **Writes**       | New PLANNED records for scheduled roadmap items                                            |

---

### ▸ keyword-memory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Sibling memory skill — cross-referenced for keyword mapping and status synchronization     |
| **Receives from**| keyword_status updates when content is PUBLISHED (triggers keyword status to PUBLISHED)   |
| **Sends to**     | Keyword records used for reverse mapping (Phase 4B/4C)                                    |

---

### ▸ project-memory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Sibling memory skill — receives aggregate content counts                                   |
| **Sends**        | Delta `content_pieces` count at the end of every session for project aggregate tracking   |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                             |
|---------------------------------------------------------------------------|--------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| No content input is available at intake                                   | Step 1 — all intake sources return null               | Request from user: "To analyze existing content, I need: (1) Your sitemap URL, (2) A CSV content list, or (3) A list of your published URLs. Please provide one of these." |
| A bulk sitemap import produces 0 parseable URLs                           | Phase 1B — XML parse fails or returns empty          | Alert: "Sitemap could not be parsed. Possible causes: invalid XML, blocked by robots.txt, or no <loc> entries. Please provide a CSV or manual URL list instead." |
| A content record arrives without both title AND URL                       | Phase 1F — minimum viability check fails             | Reject the record. Log: "Record rejected — insufficient data. Neither title nor URL provided. Minimum requirement: one of these two fields." |
| Coverage score cannot be calculated (no word count, no structural data)  | Phase 6B — no score input available                  | Assign coverage_score = null with flag: "SCORE UNAVAILABLE — no word count or structure data provided. Score will be set when content is audited." Do not default to 3. |
| URL normalization produces an empty string (malformed URL)               | Phase 2A — normalization result is empty              | Reject the URL. Log: "Malformed URL '[raw_url]' — normalization failed. Check URL format." Store the record without a URL; flag as REVIEW-NEEDED. |
| Pre-generation check returns BLOCK but calling skill bypasses it         | Step 9C — bypass detected from calling skill log     | Log the bypass: "PRE-GENERATION CHECK BYPASSED for keyword '[keyword]'. Duplicate content risk." Alert the user when the session summary is produced. |
| Stale detection run produces > 20 newly stale items in one session       | Phase 7D — high stale count                           | Flag: "STALE CONTENT ALERT: [N] content items were flagged as stale this session. A refresh cycle is recommended before new content creation." Deliver a prioritized refresh queue. |
| A content record's cluster does not match any canonical cluster          | Phase 2E — cluster name not in canonical list        | Store with the provided cluster name. Flag as CLUSTER-UNVERIFIED. Do not block storage. Surface in GET_COVERAGE_MAP as an unverified cluster. |
| Two records attempt to PUBLISH with the same URL simultaneously           | Step 3A — URL collision on concurrent write           | Accept the first. Reject the second with: "URL '[url]' is already registered as content_id [UUID] at PUBLISHED status. Duplicate rejected." |
| A user insists on creating new content when REFRESH is recommended        | Phase 9C — user bypasses refresh recommendation     | Allow with logged override. Do not block indefinitely — user judgment is final. Log the bypass for strategic review. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — STORAGE KEY FORMAT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
STORAGE KEY FORMAT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRIMARY RECORDS:
  content:[project_id]:[content_id]              → Full content item record
  content:cluster:[project_id]:[cluster_id]      → Cluster coverage summary record
  content:stats:[project_id]                      → Aggregate statistics record

INDEXES (lookup acceleration):
  content:index:[project_id]:url:[url_normalized]         → content_id
  content:index:[project_id]:keyword:[normalized_keyword] → content_id
  content:index:[project_id]:status:[status]              → list of content_ids
  content:index:[project_id]:cluster:[cluster_name]       → list of content_ids
  content:index:[project_id]:intent:[intent]              → list of content_ids
  content:index:[project_id]:refresh:queue                → sorted list of content_ids with refresh_flag=true
  content:index:[project_id]:optimization:queue           → sorted list of content_ids with optimization_needed=true

HISTORY:
  content:history:[content_id]                    → Status change history log

NOTES ON KEY DESIGN:
  All keys use [project_id] as namespace — projects are isolated
  Normalized strings used in indexes — enables case-insensitive lookup
  Index keys store lists/sorted sets, not full records
  Full records are fetched by content_id after index lookup
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — COVERAGE SCORE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
COVERAGE SCORE QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SCORE 1 — VERY THIN:
  Typical: < 500 words, stub content, no structure
  Flags: refresh_flag = true (always)
  Action: REFRESH — major content expansion required

SCORE 2 — PARTIAL:
  Typical: 500–1,200 words, basic structure, missing key sub-topics
  Flags: refresh_flag = true (auto)
  Action: REFRESH or MAJOR OPTIMIZATION

SCORE 3 — DECENT:
  Typical: 1,200–2,500 words, covers core topic, adequate structure
  Flags: refresh_flag = false unless STALE
  Action: OPTIMIZE if gaps detected; MAINTAIN if healthy

SCORE 4 — STRONG:
  Typical: 2,000–3,500 words, comprehensive, examples present
  Flags: None unless STALE
  Action: MAINTAIN; light optimization only

SCORE 5 — AUTHORITY:
  Typical: 3,500+ words, all entities, AEO elements, regularly updated
  Flags: None unless competitive landscape shifts
  Action: MAINTAIN; competitive monitoring only

SCORE OVERRIDE RULES:
  Word count < 500 → Maximum score 1 (no exceptions)
  Word count < 1,000 + no structure → Maximum score 2
  LANDING page type → Maximum score 3

REFRESH TRIGGERS:
  Score ≤ 2 → refresh_flag = true (THIN)
  last_updated > 365 days → refresh_flag = true (STALE)
  Both → refresh_flag = true; refresh_reason = "THIN, STALE"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — CONTENT RECORD EXAMPLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CONTENT RECORD EXAMPLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{
  "content_id"            : "f8a2b4c6-3d5e-7f1a-9b0c-2d4e6f8a0b2c",
  "project_id"            : "a4f7c2d1-...",
  "url"                   : "https://crm-insights.com/best-crm-software-for-small-business/",
  "url_normalized"        : "crm-insights.com/best-crm-software-for-small-business",
  "url_slug"              : "/best-crm-software-for-small-business/",
  "title"                 : "Best CRM Software for Small Business in 2024: Top 7 Compared",
  "title_normalized"      : "best crm software for small business in 2024 top 7 compared",
  "primary_keyword"       : "best crm software for small business",
  "secondary_keywords"    : ["crm for small teams", "affordable crm tools", "small business contact management"],
  "intent"                : "MOFU",
  "cluster"               : "CRM Software Selection",
  "cluster_id"            : "C-P01",
  "status"                : "PUBLISHED",
  "content_type"          : "COMPARISON",
  "coverage_score"        : 4,
  "word_count"            : 3200,
  "published_date"        : "2023-06-15T00:00:00Z",
  "last_updated"          : "2024-01-08T00:00:00Z",
  "last_reviewed"         : "2024-03-01T00:00:00Z",
  "optimization_needed"   : false,
  "refresh_flag"          : false,
  "refresh_reason"        : null,
  "serp_difficulty"       : "MEDIUM",
  "funnel_balance_notes"  : "MOFU anchor for CRM Selection cluster — links to 2 BOFU pages",
  "internal_links_from"   : ["/crm-software-overview/", "/crm-for-startups/", "/crm-setup-guide/"],
  "internal_links_to"     : ["/hubspot-crm-review/", "/salesforce-for-small-business/"],
  "aeo_optimized"         : true,
  "faq_count"             : 5,
  "session_created"       : "sess-2024-01-08-003",
  "session_last_updated"  : "sess-2024-03-01-012",
  "source"                : "CONTENT-GENERATION",
  "notes"                 : "Strong performer — ranking #4 for primary keyword. Optimization opportunity: add 2024 pricing table update."
}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: memory/content-memory.skill*
*Nexus SEO Operating System — Version 1.0.0*
