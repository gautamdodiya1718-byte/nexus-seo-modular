# SKILL: research/website-analysis.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / Site Intelligence
# EXECUTION PRIORITY: FOUNDATIONAL — Runs once per project to establish full site content awareness.
# POSITION: Site intelligence layer. Foundation for gap analysis, deduplication, and roadmap planning.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **site content intelligence engine** of the Nexus system.

It ingests a website — in any form the user provides: a full URL, a bare domain, or even just a company name — and converts it into a fully structured, keyword-mapped, intent-classified, cluster-organized inventory of every piece of content the site has published. This inventory becomes the system's ground truth for what the site already covers, how well it covers it, and where content gaps exist.

Without this skill, the Nexus system operates without content awareness — it cannot prevent duplicate content production, cannot identify genuine gaps versus imagined gaps, and cannot build an intelligent content roadmap. This skill is the prerequisite for all gap analysis, opportunity scoring, and content planning that follows.

### Primary Responsibilities

| Responsibility                      | Description                                                                                                          |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Domain Resolution**               | Convert any user-provided site identifier (URL, domain, company name) into a confirmed canonical domain             |
| **Sitemap Discovery**               | Locate the site's URL inventory through standard sitemap paths and fallback methods                                 |
| **URL Cleaning and Filtering**      | Remove non-content URLs (pagination, utility pages, author pages) to isolate strategically meaningful pages         |
| **Data Extraction**                 | Extract or infer title, H1, slug-based topic, and meta description for every valid URL                              |
| **Topic Inference**                 | Determine the primary topic, primary keyword, and search intent for each page                                       |
| **Cluster Assignment**              | Group all pages into named topic clusters that reflect the site's content architecture                              |
| **Coverage Scoring**                | Score each cluster's content depth — Strong, Medium, Weak, or Missing                                              |
| **Duplicate Detection**             | Identify pages competing for the same keyword or overlapping in topic coverage                                      |
| **Content Inventory Output**        | Produce a complete, structured, per-page analysis table with all classification fields populated                    |
| **Cluster Summary Output**          | Produce a cluster-level summary showing coverage strength and content volume per topic area                         |

### What This Skill Is NOT

- It is **not** a technical SEO auditor. It does not evaluate page speed, Core Web Vitals, crawl errors, or redirect chains.
- It is **not** a backlink analyzer. Link profiles are analyzed by `authority-mapping.skill`.
- It is **not** a keyword rank tracker. It maps existing content to inferred keywords — it does not verify actual ranking positions.
- It is **not** a content quality evaluator. It classifies content by topic and intent — it does not score writing quality or read time.
- It is **not** optional for project initialization. Any project where the user has an existing website must run this skill before gap analysis or roadmap planning begins.

### The Foundational Principle of This Skill

> This skill creates the system's memory of what already exists.
>
> Every keyword it assigns, every cluster it builds, every coverage score it produces
> becomes the reference baseline against which all future content decisions are made.
>
> If a page exists and is not catalogued here, it does not exist to the Nexus system.
> If a topic is covered and is not detected here, it will be treated as a gap — and redundant content will be produced.
>
> Completeness is non-negotiable. Every valid page must be classified.
> Every classification must be specific enough to prevent downstream duplication.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input — Site Identifier (One of Three Forms)

This skill accepts a site identifier in any of three forms. All three are handled with different processing paths:

| Input Form       | Example                              | Processing Path                                         |
|------------------|--------------------------------------|---------------------------------------------------------|
| **Full URL**     | `https://example.com`                | Extract domain → confirm → proceed to sitemap discovery |
| **Bare domain**  | `example.com`                        | Add protocol → confirm → proceed to sitemap discovery   |
| **Company name** | `"Acme Corp"` or `"HubSpot"`        | Infer domain → confirm with user → proceed              |

### Optional Inputs

| Field                     | Description                                                              | Used For                                                     |
|---------------------------|--------------------------------------------------------------------------|--------------------------------------------------------------|
| `sitemap_url`             | Explicit sitemap URL if standard paths fail                             | Bypassing sitemap discovery (Step 2)                         |
| `url_list`                | Manual list of URLs provided by user                                    | Replacing sitemap discovery if unavailable                   |
| `exclude_patterns`        | URL patterns the user wants excluded (e.g., `/news/`, `/events/`)       | Appended to the URL cleaning filter list (Step 3)            |
| `focus_section`           | A specific section of the site to analyze (e.g., `/blog/` only)        | Scoping the analysis to a subdirectory                       |
| `existing_clusters`       | Cluster labels the user has already defined                             | Seeding the cluster assignment (Step 6) with known structure |
| `target_keywords`         | Keywords the user is planning to target                                 | Informing duplicate detection (Step 8)                       |

### Input Pre-Conditions

1. **Is any site identifier provided?** If not → halt. Request at minimum a company name, domain, or URL.
2. **Is the domain already in project memory?** → Query `memory-controller.skill`. If a site analysis exists within freshness threshold (30 days) → offer cached. If stale → ask user: "Your site analysis was last run [N days ago]. Run fresh or use cached?" If no prior analysis → proceed.
3. **Is the focus limited to a specific section?** → If `focus_section` is provided → scope all subsequent steps to that path only.
4. **Has the user provided a URL list directly?** → If yes → skip Steps 1 and 2 → proceed directly to Step 3 (URL Cleaning).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Structured Site Content Intelligence Report

```
NEXUS WEBSITE ANALYSIS REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain Analyzed         : [confirmed domain]
Analysis Scope          : [FULL SITE / SECTION: /path/]
Total URLs Discovered   : [count — from sitemap]
Total URLs After Filter : [count — valid content pages]
Pages Analyzed          : [count]
Clusters Identified     : [count]
Duplicate Alerts        : [count]
Coverage Score (Overall): [STRONG / MEDIUM / WEAK / MIXED]
Analysis Confidence     : [HIGH (sitemap) / MEDIUM (partial) / LOW (manual)]
Generated By            : research/website-analysis.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — PAGE INVENTORY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | URL                         | Page Title                   | H1 (if available) | Primary Topic           | Inferred Keyword               | Intent  | Cluster           | Coverage Notes          |
|---|-----------------------------|------------------------------|--------------------|-------------------------|-------------------------------|---------|-------------------|-------------------------|
| 1 | /blog/[slug]                | [title]                      | [H1 or N/A]       | [topic]                 | [keyword]                     | TOFU    | [cluster name]    | [note if applicable]    |
| 2 | /blog/[slug]                | [title]                      | [H1 or N/A]       | [topic]                 | [keyword]                     | MOFU    | [cluster name]    | [overlap with #N]       |
| 3 | /[page-slug]                | [title]                      | [H1 or N/A]       | [topic]                 | [keyword]                     | BOFU    | [cluster name]    |                         |
...

[Full table — one row per valid analyzed page]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — CLUSTER SUMMARY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Cluster Name             | Page Count | TOFU | MOFU | BOFU | Coverage Strength | Pillar Page | Notes                        |
|--------------------------|------------|------|------|------|-------------------|-------------|------------------------------|
| [Cluster 1]              | [N]        | [N]  | [N]  | [N]  | STRONG            | [URL/None]  | [any coverage observation]   |
| [Cluster 2]              | [N]        | [N]  | [N]  | [N]  | MEDIUM            | [URL/None]  | [note]                       |
| [Cluster 3]              | [N]        | [N]  | [N]  | [N]  | WEAK              | None        | Needs expansion              |
| [Cluster 4]              | [N]        | [N]  | [N]  | [N]  | MISSING           | None        | No content — full gap        |
...

COVERAGE STRENGTH LEGEND:
  STRONG  → 3+ pages in the cluster; at least 2 funnel stages covered
  MEDIUM  → 1–2 pages; at least 1 funnel stage covered
  WEAK    → 1 page; only 1 funnel stage; thin coverage
  MISSING → 0 pages assigned to this cluster

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — DUPLICATE DETECTION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

KEYWORD OVERLAP ALERTS:

| Alert # | URL 1                | URL 2                | Shared Keyword / Topic    | Overlap Type     | Recommendation               |
|---------|----------------------|----------------------|---------------------------|------------------|------------------------------|
| 1       | /[slug-1]            | /[slug-2]            | [shared keyword]          | EXACT KEYWORD    | Merge or differentiate       |
| 2       | /[slug-3]            | /[slug-4]            | [shared topic]            | SEMANTIC OVERLAP | Consolidate or add angle     |
| 3       | /[slug-5]            | /[slug-6]            | [shared intent + topic]   | INTENT CANNIBALIZATION | Designate one primary |
...

DUPLICATE ALERT TYPES:
  EXACT KEYWORD       → Two pages targeting the same primary keyword
  SEMANTIC OVERLAP    → Two pages covering the same topic with different phrasings
  INTENT CANNIBALIZATION → Two pages targeting the same intent at the same funnel stage

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — COVERAGE GAP SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

FUNNEL COVERAGE ANALYSIS:
  TOFU Content     : [count] pages — [% of total]
  MOFU Content     : [count] pages — [% of total]
  BOFU Content     : [count] pages — [% of total]
  Unclassified     : [count] pages — [% of total]

  Funnel Assessment : [one-sentence observation about funnel balance or imbalance]

CLUSTER COVERAGE GAPS:
  MISSING Clusters  : [count] — [names]
  WEAK Clusters     : [count] — [names]
  Total Gap Count   : [count]

PILLAR PAGE Status:
  Clusters with Pillar : [count] — [names]
  Clusters without Pillar: [count] — [names]
  Pillar Recommendation: [which missing clusters most urgently need a pillar page]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — INVENTORY METADATA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Sitemap Source          : [URL used / fallback method / manual input]
Total URLs in Sitemap   : [raw count]
URLs Removed (Filter)   : [count] — Reason breakdown:
  Pagination            : [count]
  Tag/Category pages    : [count]
  Author pages          : [count]
  Utility pages         : [count]
  External links        : [count]
  Duplicates            : [count]
  Other                 : [count]
Final Valid Pages        : [count]
Classification Method   : [EXTRACTED (live data) / INFERRED (slug + title) / MIXED]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into              : content-inventory.skill
                          content-awareness.skill
                          gap-opportunity-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Field Definitions

| Field                | Definition                                                                                                   |
|----------------------|--------------------------------------------------------------------------------------------------------------|
| `URL`                | The canonical relative path of the page (relative to domain, e.g., `/blog/article-title`)                  |
| `Page Title`         | The HTML `<title>` tag value — extracted if available; inferred from slug if not                            |
| `H1`                 | The page's primary H1 heading — extracted if available; marked N/A if not accessible                       |
| `Primary Topic`      | A human-readable description of what the page is about — derived from title + slug + meta description      |
| `Inferred Keyword`   | The primary keyword the page appears to target — derived from topic inference (Step 5)                     |
| `Intent`             | TOFU / MOFU / BOFU — the searcher intent the page appears to serve                                          |
| `Cluster`            | The topic cluster name this page is assigned to (Step 6)                                                   |
| `Coverage Notes`     | Any notable observation: overlap alert, thin content signal, duplicate flag, orphan status                  |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DOMAIN RESOLUTION

Accept the user's site identifier in any of its three forms and resolve it to a confirmed canonical domain before any further processing.

**Phase 1A — Input Form Detection**

Evaluate the user's input to determine which of three forms it takes:

| Form Detection Rule                                                          | Input Form Detected |
|------------------------------------------------------------------------------|---------------------|
| Input starts with `http://` or `https://`                                    | Full URL            |
| Input contains a `.` and matches domain pattern (X.Y or X.Y.Z)              | Bare domain         |
| Input is a word or phrase with no URL characters                             | Company name        |

**Phase 1B — Full URL Processing**

When input is a full URL:
1. Extract the domain from the URL: `https://blog.example.com/article/` → `example.com`.
2. Strip subdomains (blog., app., www.) unless the user explicitly intends to analyze only a subdomain.
3. Add `https://` protocol if not present.
4. Produce the canonical domain: `https://example.com`.

**Subdomain Handling Decision:**

If the URL contains a non-standard subdomain:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Subdomain Detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
You provided: blog.example.com

Should I analyze:
  1  The full domain: example.com (recommended — full site coverage)
  2  Only the subdomain: blog.example.com (narrower scope)

Enter choice:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Wait for user confirmation before proceeding.

**Phase 1C — Bare Domain Processing**

When input is a bare domain:
1. Normalize: strip `www.` if present → `www.example.com` → `example.com`.
2. Add `https://` protocol.
3. Proceed to domain confirmation.

**Phase 1D — Company Name Processing**

When input is a company name:

1. Infer the most likely official domain using these heuristics:

| Heuristic                                                          | Example                                        |
|--------------------------------------------------------------------|------------------------------------------------|
| Lowercase company name + `.com`                                    | `"HubSpot"` → `hubspot.com`                   |
| Remove spaces and special characters + `.com`                      | `"Acme Corp"` → `acmecorp.com`                |
| Common abbreviation + `.com`                                       | `"Search Engine Journal"` → `searchenginejournal.com` |
| Known brand → canonical domain from training knowledge             | `"Ahrefs"` → `ahrefs.com`                     |

2. If the company name is ambiguous or produces multiple plausible domains → list the top 2 candidates.

3. Display confirmation prompt:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Domain Confirmation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Is this the correct domain: [inferred domain]?

  1  Yes — proceed with this domain
  2  No — enter the correct domain

Enter choice:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**WAIT** for user confirmation. Do NOT proceed to Step 2 without confirmation.

If user selects `2` → display: `"Please enter the correct domain:"` → accept user input → re-run Phase 1C/1D with corrected input → re-confirm.

**Phase 1E — Domain Record Initialization**

After domain is confirmed:
1. Record the canonical domain.
2. Record the analysis scope (full site or section).
3. Check `memory-controller.skill` for prior analysis of this domain.
4. Initialize the site analysis record in `project-memory.skill` via `memory-controller.skill`.

---

### ▸ STEP 2 — SITEMAP DISCOVERY

Locate the site's URL inventory through a structured discovery sequence. The sitemap is the most reliable, complete source of all URLs the site has made available for indexing.

**Phase 2A — Standard Sitemap Path Probing**

Attempt each standard sitemap path in order. Stop at the first successful path:

| Attempt | Sitemap Path                          | Notes                                             |
|---------|---------------------------------------|---------------------------------------------------|
| 1       | `[domain]/sitemap.xml`                | Standard XML sitemap — most common                |
| 2       | `[domain]/sitemap_index.xml`          | Sitemap index file — links to multiple sitemaps   |
| 3       | `[domain]/wp-sitemap.xml`             | WordPress native sitemap (WordPress 5.5+)         |
| 4       | `[domain]/news-sitemap.xml`           | Google News sitemap — common for publishers      |
| 5       | `[domain]/page-sitemap.xml`           | Page-specific sitemap — common in WordPress       |
| 6       | `[domain]/post-sitemap.xml`           | Post-specific sitemap — common in WordPress       |
| 7       | `[domain]/robots.txt`                 | Parse `Sitemap:` directives in robots.txt         |
| 8       | `[domain]/sitemap/`                   | Directory-style sitemap — some CMSs              |

**Phase 2B — Sitemap Index Handling**

If a sitemap index file is found (contains links to child sitemaps):
1. Parse the index to extract all child sitemap URLs.
2. Probe each child sitemap.
3. Combine all URLs from all child sitemaps into a single URL pool.
4. Deduplicate URLs across child sitemaps before proceeding.

**Phase 2C — Robots.txt Parsing**

If `robots.txt` is accessible:
1. Extract any `Sitemap:` directives.
2. Treat each declared sitemap URL as a candidate.
3. Probe each candidate sitemap URL.

**Phase 2D — Sitemap Discovery Failure Handling**

If no sitemap is found after all attempts:

**Option A — Request from User:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS — Sitemap Not Found
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Could not locate a sitemap at [domain].

Options:
  1  Enter your sitemap URL directly
  2  Paste a list of URLs to analyze
  3  Proceed with homepage crawl inference (limited)

Enter choice:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

WAIT for user response.

**Option B — Homepage Crawl Inference (if user selects 3):**
- Extract navigation links from the homepage.
- Extract blog/content section links.
- Use these as the candidate URL pool.
- Flag the analysis as `PARTIAL — homepage inference only`.
- Confidence level = MEDIUM.

**Phase 2E — URL Pool Assembly**

After sitemap parsing (or URL list receipt):
1. Collect all discovered URLs into a raw URL pool.
2. Record total URL count (raw).
3. Normalize all URLs:
   - Strip trailing slashes consistently (apply site's convention).
   - Strip UTM parameters and tracking suffixes.
   - Strip fragment identifiers (#section).
   - Convert all to lowercase.
   - Resolve protocol inconsistency (mix of http and https → standardize to https).
4. Record normalized URL count.

---

### ▸ STEP 3 — URL CLEANING AND FILTERING

Filter the raw URL pool to retain only strategically meaningful content pages — the pages that represent actual content assets and warrant SEO classification.

**Phase 3A — Removal Filter Rules**

Remove every URL that matches any of the following patterns:

**Category 1 — Pagination:**

| Pattern                                | Example                                    |
|----------------------------------------|--------------------------------------------|
| `/page/[N]/` or `?page=[N]`            | `/blog/page/2/`                            |
| `?paged=[N]` or `?p=[N]`              | `/articles/?paged=3`                      |
| `?offset=[N]` or `?start=[N]`         | `/news/?offset=20`                        |
| `[any-path]/[N]/` where N is a pure integer at end | `/category/seo/2/`            |

**Category 2 — Tag and Category/Taxonomy Pages:**

| Pattern                                | Example                                    |
|----------------------------------------|--------------------------------------------|
| `/tag/[tag-name]/`                    | `/tag/content-marketing/`                 |
| `/tags/[tag-name]/`                   | `/tags/seo/`                               |
| `/category/[name]/`                   | `/category/marketing/`                    |
| `/categories/[name]/`                 | `/categories/seo/`                        |
| `/topic/[name]/`                      | `/topic/analytics/`                       |
| `/label/[name]/`                      | `/label/growth/`                          |

**Category 3 — Author Pages:**

| Pattern                                | Example                                    |
|----------------------------------------|--------------------------------------------|
| `/author/[name]/`                     | `/author/jane-smith/`                     |
| `/writers/[name]/`                    | `/writers/john-doe/`                      |
| `/contributors/[name]/`               | `/contributors/alex/`                     |
| `/people/[name]/`                     | `/people/sarah-jones/`                    |

**Category 4 — Utility and System Pages:**

| Pattern                                | Example                                    |
|----------------------------------------|--------------------------------------------|
| `/login` or `/sign-in`                | `/login`                                   |
| `/register` or `/signup`              | `/signup`                                  |
| `/cart` or `/checkout`                | `/cart`                                    |
| `/account` or `/profile`              | `/account/settings`                       |
| `/wp-admin/` or `/wp-login.php`       | `/wp-admin/`                              |
| `/feed/` or `/rss`                    | `/feed/`                                   |
| `/search` or `/search-results`        | `/search?q=keyword`                       |
| `/404` or `/not-found`                | `/404`                                    |
| `/sitemap` or `/sitemap.xml`          | `/sitemap.xml`                            |
| `/privacy-policy` or `/terms`         | `/privacy-policy`                         |
| `/contact` or `/about` or `/team`    | `/contact`                                |

**Note on About/Contact/Team exclusion:** These utility pages are excluded from SEO analysis by default. However, if the user's `focus_section` or `existing_clusters` suggest these pages are strategically important (e.g., for E-E-A-T purposes) → keep them and classify them separately.

**Category 5 — Duplicate and Near-Duplicate URLs:**

- Exact URL duplicates (after normalization) → keep one, remove all others.
- URLs that differ only by trailing slash → `example.com/page` and `example.com/page/` → keep the site's canonical form.
- URLs that differ only by protocol → `http://` vs `https://` → keep `https://`.
- URLs with and without `www.` → keep the canonical form declared in the domain confirmation.

**Category 6 — User-Specified Exclusion Patterns:**

Apply any patterns provided in the optional `exclude_patterns` input.

**Phase 3B — Content Page Inclusion Criteria**

After removals, verify retained pages pass at least one inclusion criterion:

| Content Type       | Inclusion Signal                                                               |
|--------------------|--------------------------------------------------------------------------------|
| Blog post          | `/blog/`, `/posts/`, `/articles/`, `/resources/`, `/insights/`, `/news/` path |
| Landing page       | Descriptive slug in root path without utility indicators                       |
| Product page       | `/products/`, `/solutions/`, `/features/`, `/services/` path                  |
| Pillar page        | Long-form topic pages — often at `/[topic]/` or `/guide/[topic]/`             |
| Case study         | `/case-studies/`, `/success-stories/`, `/customers/[name]/`                   |
| Documentation      | `/docs/`, `/documentation/`, `/help/`, `/support/` (if in scope)              |

**Phase 3C — Filter Audit Record**

Document the filter results:
- Total URLs removed by category.
- Total URLs retained.
- List of any borderline URLs that required a judgment call (with the decision made and reasoning).

---

### ▸ STEP 4 — DATA EXTRACTION

For each URL that survived the filter, extract or infer the data fields needed for classification.

**Phase 4A — Extraction Hierarchy**

For each URL, attempt data extraction in this priority order:

| Priority | Data Source                        | Reliability | Notes                                              |
|----------|------------------------------------|-------------|----------------------------------------------------|
| 1        | Live page metadata (title tag, H1, meta description) | HIGHEST | Requires page access — most accurate             |
| 2        | Sitemap `<title>` or `<lastmod>` fields | HIGH | Some sitemaps include title or publication data |
| 3        | Slug reverse-engineering           | MEDIUM      | Always available — minimum data point             |
| 4        | URL structure inference            | MEDIUM      | Path components reveal content type and topic     |

**Phase 4B — Title Extraction and Inference**

**Extraction (if page is accessible):**
- Extract the `<title>` tag value.
- If the title contains a site name suffix (e.g., `"Article Title | Example.com"`): strip the suffix and retain only the article title component.
- Record as: `Extracted`.

**Inference (if page is not accessible):**
- Reverse-engineer from the URL slug:
  - Decode URL-encoded characters.
  - Replace hyphens and underscores with spaces.
  - Title-case the result.
  - Example: `/blog/best-crm-software-for-small-businesses` → `"Best CRM Software for Small Businesses"`.
- Record as: `Inferred from slug`.

**Phase 4C — H1 Extraction**

**Extraction (if page is accessible):**
- Extract the first `<h1>` element's text content.
- If multiple H1s exist → use the first one only.
- Record as: `Extracted`.

**Non-accessible:**
- Record as: `N/A — page not accessible`.
- Proceed using title inference as the primary classification signal.

**Phase 4D — Meta Description Extraction**

**Extraction (if page is accessible):**
- Extract the `<meta name="description">` content attribute.
- Record as: `Extracted`.

**Non-accessible:**
- Record as: `N/A`.
- Meta description is a supplementary signal — its absence does not block classification.

**Phase 4E — Slug Analysis**

For every URL (regardless of whether live data is accessible):
- Extract the URL slug (the path component after the domain).
- Remove path prefixes (`/blog/`, `/resources/`, `/posts/`).
- Parse the slug's semantic components:

| Slug Component Pattern            | Extraction                                   |
|-----------------------------------|----------------------------------------------|
| `[keyword-phrase]-[modifier]`     | Primary keyword: `[keyword-phrase]`, modifier: `[modifier]` |
| `[number]-[keyword]-[noun]`       | List-type content; keyword: `[keyword]`      |
| `how-to-[action]-[object]`        | Tutorial content; keyword: `[action] [object]` |
| `[tool-a]-vs-[tool-b]`           | Comparison content; entities: Tool A, Tool B |
| `best-[category]-for-[audience]`  | Listicle; keyword: `[category]`, audience: `[audience]` |
| `what-is-[concept]`               | Definition content; keyword: `[concept]`    |
| `[topic]-review`                  | Review content; keyword: `[topic]`           |
| `[topic]-guide` or `[topic]-tutorial` | Guide content; keyword: `[topic]`       |
| `[audience]-[topic]`              | Audience-targeted; audience: `[audience]`, topic: `[topic]` |

---

### ▸ STEP 5 — TOPIC INFERENCE

For each page, determine its primary topic, inferred keyword, and search intent. This is the classification core of the skill — every downstream output depends on the accuracy of these three assignments.

**Phase 5A — Primary Topic Determination**

The primary topic is a 3–6 word human-readable description of what the page is about. It must be specific enough that two different topics cannot be confused.

**Topic Derivation Rules:**

| Available Data                    | Topic Derivation Method                                                    |
|-----------------------------------|----------------------------------------------------------------------------|
| Title + H1 available              | Use the more specific of the two; synthesize if they complement each other |
| Title available, H1 unavailable   | Derive from title, cleaned of brand name and filler words                  |
| Slug only                         | Reverse-engineer from slug components (Phase 4E output)                   |
| Slug + meta description           | Use slug for primary topic; use meta description to add specificity        |

**Topic Specificity Rules:**

- The topic must name a specific concept, tool, audience, or use case — not a generic category.
- Prohibited topics: `"SEO"`, `"Marketing"`, `"Content"` (too broad).
- Required topics: `"CRM software for startups"`, `"Email open rate optimization"`, `"HubSpot vs Salesforce comparison"`.

**Phase 5B — Inferred Keyword Determination**

The inferred keyword is the primary search query the page was most likely created to rank for. It should be expressed as a natural language search query — the way a user would type it into Google.

**Keyword Inference Rules:**

| Page Signal                                        | Keyword Inference                                                             |
|----------------------------------------------------|-------------------------------------------------------------------------------|
| Slug starts with `how-to-`                        | `"how to [remainder of slug, spaces]"` — preserve question structure          |
| Slug contains `vs`                                | `"[tool a] vs [tool b]"` — preserve comparison structure                     |
| Slug contains `best-` + category                  | `"best [category]"` — may add `"for [audience]"` if slug contains it         |
| Slug contains `what-is-`                          | `"what is [concept]"` — preserve definitional structure                       |
| Slug contains `review`                            | `"[tool/product] review"` — preserve review structure                         |
| Slug contains `-guide` or `-tutorial`             | `"[topic] guide"` or `"[topic] tutorial"`                                     |
| Slug is a pure topic phrase (no structural signal)| Infer most natural keyword form from topic phrase                             |
| Title provides more specificity than slug         | Use title-derived keyword; note title as source                               |

**Keyword Format Rules:**

- Always lowercase (as users type it).
- Include modifiers if present in slug (audience, industry, use case).
- Do not fabricate specificity that is not evidenced by the URL or title.
- Maximum 7 words.

**Phase 5C — Intent Classification**

Classify each page into one of three funnel stages based on the keyword and topic:

**Intent Signal Detection:**

| Keyword / Slug Pattern                                    | Intent  |
|-----------------------------------------------------------|---------|
| `what is`, `how does`, `guide to`, `introduction to`     | TOFU    |
| `why`, `benefits of`, `vs`, `importance of`              | TOFU    |
| `best`, `top`, `alternatives to`, `comparison`           | MOFU    |
| `review`, `pricing`, `features`, `which is better`       | MOFU    |
| `buy`, `get started`, `free trial`, `pricing plans`      | BOFU    |
| `case study`, `results`, `ROI`, `implementation`         | BOFU    |
| Landing page structure with primary CTA                   | BOFU    |
| Product/service page                                      | BOFU    |

**Intent Ambiguity Resolution:**

When a page's signals are mixed:
- Apply the dominant signal (the one with more evidence).
- If exactly equal → default to TOFU for informational-first pages; MOFU for evaluation-first pages.
- Document ambiguity in the Coverage Notes field.

---

### ▸ STEP 6 — CLUSTER ASSIGNMENT

Group all classified pages into named topic clusters. Each cluster represents a distinct topic area within the site's content architecture.

**Phase 6A — Cluster Seed Identification**

Before assigning individual pages, identify the topic clusters that will organize the inventory:

**Method 1 — Pre-provided Clusters:**
If the user provided `existing_clusters` as input → use those cluster names as the assignment framework. Add any cluster that emerges from the inventory but was not pre-provided.

**Method 2 — Emergent Clustering (primary method):**
Group the page inventory by shared topic territory:
1. Review all primary topics and inferred keywords.
2. Identify groups of pages that address the same core concept from different angles.
3. Name each group with a descriptive cluster label.

**Phase 6B — Cluster Naming Convention**

Cluster names must be:
- 2–5 words describing the topic territory (not the keyword).
- Human-readable and immediately understandable.
- Specific enough that no two clusters have overlapping names.
- Consistent across the inventory (same naming style throughout).

| Prohibited Name            | Required Name                              |
|----------------------------|--------------------------------------------|
| "SEO Topics"               | "On-Page SEO Optimization"                 |
| "Marketing"                | "Email Marketing Strategy"                 |
| "Content Group 1"          | "Content Creation and Planning"            |
| "Blog Posts"               | "Technical SEO Guides"                    |
| "General"                  | [Not allowed — every cluster needs a name]|

**Phase 6C — Page-to-Cluster Assignment Protocol**

For each page in the inventory, assign it to the most appropriate cluster:

1. Compare the page's primary topic against all cluster names.
2. If the topic clearly fits one cluster → assign.
3. If the topic fits two clusters → assign to the cluster with higher thematic alignment; cross-reference in Coverage Notes.
4. If the topic fits no cluster → create a new cluster.
5. No page may remain unassigned after this phase.

**Semantic Grouping Rules:**

- Pages belong in the same cluster if a single pillar page could reasonably serve as the parent for all of them.
- Pages belong in different clusters if their primary topics serve different reader needs, even if they share surface-level vocabulary.
- Cluster membership is defined by topic territory — NOT by URL path (a blog post at `/blog/crm-for-startups` and a landing page at `/crm-software/` may belong to the same cluster).

**Phase 6D — Pillar Page Identification**

Within each cluster, identify whether a pillar page exists:

| Pillar Page Signal                                                             |
|--------------------------------------------------------------------------------|
| URL is at root or shallow path level (e.g., `/crm-software/`)                 |
| Title suggests comprehensive coverage ("Complete Guide to X", "Everything About X") |
| Slug is shorter and broader than supporting pages (e.g., `/crm/` vs `/crm-for-startups/`) |
| Page appears to be a hub with links to more specific supporting content        |

Record: `Pillar Page: [URL]` or `Pillar Page: None` for each cluster.

---

### ▸ STEP 7 — COVERAGE SCORING

Score each cluster's content coverage to identify where the site is strong, thin, or completely absent.

**Phase 7A — Coverage Score Calculation**

For each cluster, calculate coverage score based on two factors:

**Factor 1 — Page Count:**

| Page Count        | Raw Score |
|-------------------|-----------|
| 0 pages           | 0         |
| 1 page            | 1         |
| 2 pages           | 2         |
| 3–5 pages         | 3         |
| 6–10 pages        | 4         |
| 11+ pages         | 5         |

**Factor 2 — Funnel Coverage:**

| Funnel States Present                     | Multiplier |
|-------------------------------------------|------------|
| All three (TOFU + MOFU + BOFU)           | 1.5×       |
| Two stages present                        | 1.2×       |
| One stage present                         | 1.0×       |
| No classifiable intent (utility pages)    | 0.5×       |

**Combined Coverage Score:**
`Combined_score = Raw_score × Multiplier`

**Phase 7B — Coverage Strength Classification**

| Combined Score  | Coverage Strength | Definition                                                         |
|-----------------|-------------------|--------------------------------------------------------------------|
| 0               | MISSING           | No content exists in this cluster                                 |
| 1–2             | WEAK              | Minimal coverage; 1 page or thin presence; major gaps             |
| 2–4             | MEDIUM            | Adequate presence; some funnel coverage; room to expand           |
| 4.5+            | STRONG            | Well-covered; multiple pages; multiple funnel stages present      |

**Phase 7C — Pillar Status Impact**

If a cluster has STRONG page count but no pillar page → add note: "Cluster lacks a pillar page. Content exists but lacks structural anchor." This is a distinct content architecture gap from topic coverage.

If a cluster has a pillar page but only 1–2 supporting pages → classify as MEDIUM regardless of pillar presence. A pillar without support is an unsupported hub.

**Phase 7D — Coverage Score Output**

For each cluster, produce a coverage record:

```
Cluster: [Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Pages           : [count]
TOFU Pages      : [count]
MOFU Pages      : [count]
BOFU Pages      : [count]
Pillar Page     : [URL or None]
Raw Score       : [N]
Funnel Multiplier: [N]×
Combined Score  : [N]
Coverage Strength: [STRONG / MEDIUM / WEAK / MISSING]
Coverage Notes  : [specific observation about this cluster's coverage state]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 8 — DUPLICATE DETECTION

Identify pages within the inventory that are competing for the same keyword or serving the same intent within the same topic — a problem that creates cannibalization risk and dilutes the site's authority signals.

**Phase 8A — Exact Keyword Overlap Detection**

Compare every page's inferred keyword against every other page's inferred keyword:
- If any two pages share the same inferred keyword (case-insensitive, after normalization) → flag as EXACT KEYWORD overlap.

**Normalization for Comparison:**
- Lowercase all keywords.
- Strip trailing filler words ("complete guide", "ultimate guide", "for beginners" if also present in the other).
- Treat `"best CRM software"` and `"best CRM"` as overlapping (one is a substring of the other with the same core intent).

**Phase 8B — Semantic Topic Overlap Detection**

Beyond exact matches, detect pages that cover the same topic even if their keywords are phrased differently:

Semantic overlap is present when:
- Two pages have primary topics that describe the same concept from the same angle.
- Two pages would compete for the same SERP position if both were fully optimized.
- Two pages would logically be merged into one stronger piece if the site were restructuring.

**Semantic Overlap Test:**

Ask: "If a reader searching for [Page A's keyword] arrived at [Page B], would they consider their search satisfied?"
- YES → semantic overlap detected.
- NO → different topics despite surface similarity.

**Phase 8C — Intent Cannibalization Detection**

Beyond keyword and topic overlap, detect cases where two pages target the same intent at the same funnel stage for the same topic — even if their specific keywords differ:

Example: `/blog/crm-software-comparison` (MOFU, Comparison) and `/blog/best-crm-for-small-business` (MOFU, Comparison) — both serve the same "I'm evaluating CRM options" intent in the same cluster. This is intent cannibalization even though the keywords differ.

**Cannibalization Test:**
Same cluster + same funnel stage + same primary user need = intent cannibalization.

**Phase 8D — Duplicate Alert Classification and Recommendation**

For each detected overlap, classify and recommend:

| Overlap Type               | Recommended Action                                                                     |
|----------------------------|----------------------------------------------------------------------------------------|
| EXACT KEYWORD              | Merge the two pages into one comprehensive piece; or differentiate by clearly distinct angle |
| SEMANTIC OVERLAP           | Consolidate into one page; redirect the weaker URL to the stronger; or add a clearly differentiated angle to one |
| INTENT CANNIBALIZATION     | Designate one page as primary; restructure the other to serve a genuinely adjacent intent; or merge |

**Merge vs. Differentiate Decision Framework:**

| Condition                                                       | Recommendation |
|-----------------------------------------------------------------|----------------|
| Both pages are SHORT and cover the same ground                  | MERGE          |
| Both pages are LONG but one is clearly comprehensive            | CONSOLIDATE (redirect weak to strong) |
| Both pages are LONG and cover genuinely different sub-angles    | DIFFERENTIATE (update each to own its distinct sub-topic) |
| Pages are same topic but different audiences                    | DIFFERENTIATE by audience — keep both |
| Pages are same topic, same audience, same length                | MERGE into one definitive piece |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ REVERSE KEYWORD MAPPING

The slug is always available, making it the minimum data point for keyword inference. This skill applies a structured reverse-mapping algorithm to extract maximum signal from slug structure alone.

**Reverse Keyword Mapping Algorithm:**

```
SLUG RECEIVED: [raw slug]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Step 1: Strip path prefix
  Remove: /blog/, /posts/, /articles/, /resources/, /insights/, /news/, /guides/
  Result: [clean slug]

Step 2: Decode URL encoding
  %20 → space, %2F → /, etc.
  Result: [decoded slug]

Step 3: Replace separators
  Hyphens (-) and underscores (_) → spaces
  Result: [word sequence]

Step 4: Detect structural pattern
  → how-to-X → Keyword: "how to X" | Format: Tutorial | Intent: TOFU
  → X-vs-Y → Keyword: "X vs Y" | Format: Comparison | Intent: MOFU
  → best-X-for-Y → Keyword: "best X for Y" | Format: Listicle | Intent: MOFU
  → what-is-X → Keyword: "what is X" | Format: Definitional | Intent: TOFU
  → X-review → Keyword: "X review" | Format: Review | Intent: MOFU-BOFU
  → X-guide → Keyword: "X guide" | Format: Guide | Intent: TOFU
  → X-pricing → Keyword: "X pricing" | Format: Pricing | Intent: BOFU
  → X-alternatives → Keyword: "X alternatives" | Format: Alternative | Intent: MOFU
  → No pattern → Keyword: [full word sequence] | Format: Unknown | Intent: Inferred from context

Step 5: Apply title correction (if available)
  If title provides more specificity than slug → use title-derived keyword
  Record source as: "Title-corrected"

Step 6: Normalize keyword
  Lowercase
  Remove stop words from start and end (a, the, an, is, of, for, in — unless they are part of a keyword phrase like "for beginners")
  Trim to ≤7 words

Output: Inferred Keyword + Source + Confidence (HIGH/MEDIUM/LOW)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ INTENT INFERENCE FROM URL STRUCTURE

Beyond slug reverse-mapping, the URL structure itself carries intent signals that can be used when slug content is ambiguous.

**URL-Level Intent Signals:**

| URL Path Component          | Intent Signal                        |
|-----------------------------|--------------------------------------|
| `/blog/`                    | TOFU (default) — informational section |
| `/guides/` or `/tutorials/` | TOFU — educational content           |
| `/resources/`               | TOFU — reference content             |
| `/compare/` or `/vs/`       | MOFU — comparison content            |
| `/best-of/` or `/roundups/` | MOFU — evaluation content            |
| `/case-studies/`            | BOFU — proof content                 |
| `/pricing/` or `/plans/`    | BOFU — commercial intent             |
| `/features/` or `/solutions/`| MOFU-BOFU — commercial evaluation   |
| `/customers/` or `/success/` | BOFU — social proof content         |
| `/landing/` or `/lp/`       | BOFU — conversion-focused            |

Use path-level signals to confirm or override slug-level intent inference. Path signal wins only when it is more specific than the slug signal.

---

### ▸ CLUSTER EVOLUTION DETECTION

When the user has provided `existing_clusters` from a prior analysis or from `semantic-clustering.skill`:

1. Compare newly inferred clusters from the current inventory against the existing cluster names.
2. If a new cluster emerges that has no match in the existing cluster set → flag as EMERGING CLUSTER: "New topic territory detected — not previously in cluster architecture."
3. If an existing cluster has no pages in the current inventory → flag as DORMANT CLUSTER: "Cluster [name] has no current content — previously identified as part of site architecture."
4. If pages in the current inventory belong to a different cluster than they were previously assigned → flag as CLUSTER MIGRATION: "Page [URL] was previously assigned to [Old Cluster]; current analysis assigns it to [New Cluster]. Review."

---

### ▸ CONTENT FRESHNESS INFERENCE

When sitemap `<lastmod>` dates are available (common in XML sitemaps):

1. Extract the `<lastmod>` date for each URL.
2. Calculate the page's age in months from the current date.
3. Assign a freshness tag:

| Age               | Freshness Tag | Implication                                              |
|-------------------|---------------|----------------------------------------------------------|
| < 3 months        | FRESH         | Recently published or updated — no freshness concern     |
| 3–12 months       | CURRENT       | Relatively current — may benefit from data refreshes     |
| 12–24 months      | AGING         | Beginning to show age — review for outdated data         |
| 24–36 months      | STALE         | Likely contains outdated information — refresh priority  |
| 36+ months        | OUTDATED      | High probability of outdated data — urgent refresh need  |

Record freshness tags in the Coverage Notes field. Roll up freshness data to the cluster level: if ≥50% of a cluster's pages are STALE or OUTDATED → flag the cluster with a FRESHNESS RISK warning in the cluster summary.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate URLs in Inventory

Every URL in the final page inventory must be unique. After normalization (Step 2E), any URL that appears more than once is deduplicated:
- Keep the canonical form (HTTPS, no trailing slash inconsistency, no UTM parameters).
- Record removed duplicates in the Inventory Metadata section.

### Rule 2 — No Duplicate Topic Assignments

Two different URLs should not be assigned the same exact primary topic unless they are intentionally covering the same concept from different angles (different audience, different funnel stage). If two pages share the same topic AND the same funnel stage AND the same audience → flag as DUPLICATE TOPIC in the duplicate detection report.

### Rule 3 — No Duplicate Cluster Names

Every cluster in the inventory must have a unique name. If two cluster names are semantically equivalent → merge the clusters. Keep the name that is more descriptive. Reassign pages from the dissolved cluster to the surviving cluster.

### Rule 4 — No Duplicate Keyword Assignments (First-Pass Alert)

When the same keyword is assigned to two different pages during Step 5 → flag both pages immediately. Do not allow the duplicate to pass silently into the inventory. Every duplicate keyword assignment is a duplicate detection alert (Step 8 catches these systematically — but Step 5 should flag them in real time as well).

### Rule 5 — No Orphaned Pages

Every valid page in the filtered URL pool must be assigned to a cluster. A page with no cluster assignment is an orphaned page — which is a classification failure, not a valid output state. If a page genuinely does not fit any cluster → create a micro-cluster of 1 (temporarily) and flag it: `"ORPHAN — needs cluster review."` Then evaluate whether it belongs in an existing cluster or should seed a new one.

### Rule 6 — Memory Deduplication

Before producing the site analysis report, check `memory-controller.skill` for a prior analysis of this domain. If the domain has been analyzed within the freshness threshold (30 days) → offer cached result. If stale → proceed fresh, but compare new clusters against prior clusters and flag changes (cluster evolution detection).

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Analysis Behaviors

| Prohibited Behavior                                                                           | Reason                                                                            |
|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Leaving the `Inferred Keyword` field blank for any page                                      | Every page must have a keyword assignment — this is the skill's primary output    |
| Assigning a cluster name that is a single generic word ("SEO", "Marketing", "Content")       | Single-word clusters are too broad — they prevent meaningful gap analysis         |
| Using "General" or "Miscellaneous" as a cluster name                                         | Every cluster must be a specific, defined topic territory                         |
| Assigning all pages to 1–2 clusters because it's faster                                      | Under-clustering produces inaccurate gap analysis downstream                     |
| Leaving coverage strength as STRONG for a cluster with only 1 page                          | Coverage strength rules are precise — 1 page is always WEAK regardless of quality|
| Skipping the duplicate detection step because no exact keyword matches are obvious           | Semantic overlap and intent cannibalization require active detection, not passive |
| Removing pages from the inventory without logging them in the filter audit                   | Every removed page must be accounted for in the Inventory Metadata               |
| Assigning BOFU intent to an informational blog post because it has a CTA                     | CTAs do not determine intent — the page's primary purpose and keyword do          |
| Classifying a 10-page cluster as MISSING coverage                                            | MISSING means zero pages — a 10-page cluster is at minimum MEDIUM or STRONG      |
| Leaving the `Coverage Notes` field blank when a duplicate or overlap exists                  | Coverage notes are mandatory whenever an anomaly, overlap, or gap is detected    |

### Required Analysis Behaviors

- Every page in the final inventory must have all 7 output fields populated.
- Every cluster must have a coverage strength score and a pillar page status.
- Every duplicate detection alert must include a recommendation.
- The filter audit must account for every URL removed from the raw pool.
- The intent classification must be TOFU, MOFU, or BOFU — never "Unknown" unless explicitly flagged as unclassifiable with a reason.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before analysis, WRITE after output finalization                       |
| **Reads**        | Prior site analysis (freshness check), existing cluster definitions, project metadata        |
| **Writes**       | Complete site inventory, cluster map, coverage scores, duplicate alerts, freshness data     |
| **Memory layers**| project-memory.skill (site metadata), content-memory.skill (page inventory), strategy-memory.skill (clusters) |
| **Timing**       | READ at pre-conditions + Step 1E. WRITE after Step 8 completes.                            |

---

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | website-analysis → content-inventory (primary output consumer)                              |
| **Sends**        | Complete page inventory table (Part 1), all 7 fields per page                              |
| **Used for**     | Maintaining the persistent, cross-session content inventory for the project                 |
| **Critical field** | Inferred Keyword — used by content-inventory.skill to prevent duplicate content creation |

---

### ▸ content-awareness.skill (gap-opportunity-engine.skill input)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | website-analysis → content-awareness (context provider)                                    |
| **Sends**        | Cluster summary table (Part 2), coverage gap summary (Part 4)                              |
| **Used for**     | Establishing the system's awareness of what the site already covers before gap analysis    |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | website-analysis → gap-opportunity-engine (strategic input)                                |
| **Sends**        | MISSING and WEAK clusters (from Part 2 and Part 4), funnel gap data, duplicate alerts      |
| **Used for**     | Identifying which topic areas represent genuine gaps vs. areas where content exists        |
| **Critical field** | MISSING and WEAK cluster list — these are the primary inputs for gap opportunity scoring |

---

### ▸ keyword-memory.skill (via memory-controller)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after inventory finalization)                                                        |
| **Writes**       | All inferred keywords with their lifecycle state set to `PUBLISHED` (if confirmed existing content) |
| **Used for**     | Preventing keyword-discovery.skill from re-generating keywords for topics already covered  |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional (receives from and sends to)                                                  |
| **Receives from**| Existing cluster definitions (if semantic-clustering has already run)                       |
| **Sends to**     | Emergent clusters discovered during inventory analysis                                      |
| **Used for**     | Cross-referencing site's actual content clusters with the keyword universe's cluster architecture |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | website-analysis → roadmap-engine (coverage context)                                       |
| **Sends**        | Coverage strength per cluster, missing cluster list, weak cluster list, freshness risk data |
| **Used for**     | Prioritizing content creation based on what the site currently lacks                       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                            | Detection                                             | Recovery Action                                                                                              |
|-----------------------------------------------------------------------------|-------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Company name cannot be resolved to a domain                                 | Domain inference fails or produces ambiguous results  | Display up to 3 candidate domains. Ask user to confirm or enter the correct domain. Do not proceed without confirmation. |
| All standard sitemap paths fail and user declines to provide URL list       | Phase 2D — no sitemap found, user selects no option   | Attempt homepage crawl inference. Flag output as PARTIAL — homepage inference only. Reduce confidence to MEDIUM. |
| User provides a URL list but it contains fewer than 10 URLs                 | Phase 3C — final valid page count < 10                | Proceed with available URLs. Flag: "LIMITED INVENTORY — fewer than 10 valid pages. Analysis may not reflect full site." Recommend sitemap access for complete analysis. |
| URL pool contains more than 10,000 URLs after normalization                 | Phase 2E — URL count exceeds threshold               | Confirm with user: "Sitemap contains [N] URLs. Analyze all or focus on a specific section?" If user selects section → scope to that path. If all → proceed with batch processing and note extended analysis time. |
| A page cannot be classified into any funnel stage                           | Step 5C — no intent signal detected                   | Assign to the closest plausible funnel stage based on URL path context. Add Coverage Note: "Intent inferred from path — no keyword signal available." |
| A page cannot be assigned to any cluster after attempting all cluster options | Step 6C — no cluster fits                           | Create a temporary cluster named "UNCATEGORIZED — [page topic]". Flag for user review: "This page could not be assigned to an existing cluster. Please review or suggest a cluster name." |
| Duplicate detection finds more than 20 overlapping page pairs               | Step 8 — high duplication rate                        | Flag site as HIGH CANNIBALIZATION RISK. Produce duplicate alerts for all pairs. Prioritize the top 5 by severity for immediate action. Recommend a site consolidation review before new content is created. |
| Memory controller is unavailable                                            | READ/WRITE queries return errors                      | Proceed with analysis. Flag: "Memory unavailable — results will not be persisted. Copy or export this report before session ends." |
| Sitemap lastmod dates are absent (freshness data unavailable)               | Phase 2B — no lastmod in sitemap                      | Skip freshness tagging. Note in Inventory Metadata: "Freshness data unavailable — sitemap does not include lastmod dates." Proceed with all other steps at full depth. |
| User's site has content in multiple languages                               | URL pool shows language-prefixed paths (/en/, /es/)   | Confirm with user: "Multi-language site detected. Analyze: (1) All languages, (2) English only, (3) Specific language." Proceed with user selection. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — URL FILTER QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
URL REMOVAL FILTER RULES — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REMOVE — Pagination:
  /page/[N]/ | ?page=[N] | ?paged=[N] | ?p=[N]
  ?offset=[N] | ?start=[N] | /[N]/ (terminal integer)

REMOVE — Taxonomy Pages:
  /tag/ | /tags/ | /category/ | /categories/
  /topic/ | /label/ | /archive/ | /type/

REMOVE — Author Pages:
  /author/ | /writers/ | /contributors/ | /people/

REMOVE — Utility Pages:
  /login | /sign-in | /register | /signup
  /cart | /checkout | /account | /profile
  /wp-admin/ | /wp-login.php | /feed/ | /rss
  /search | /search-results | /404 | /not-found
  /sitemap | /robots.txt | /privacy-policy
  /terms | /cookie-policy | /disclaimer

REMOVE — System Files:
  *.xml | *.txt | *.pdf (unless PDF is a content asset)
  *.jpg | *.png | *.gif (media files, not pages)

KEEP — Content Pages:
  /blog/ | /posts/ | /articles/ | /resources/
  /insights/ | /news/ | /guides/ | /tutorials/
  /case-studies/ | /success-stories/ | /customers/
  /products/ | /solutions/ | /features/ | /services/
  Root-level descriptive slugs (e.g., /crm-software/)
  /landing/ | /lp/ (if in scope)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — COVERAGE SCORING QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
COVERAGE SCORING FORMULA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Raw Score (by page count):
  0 pages  = 0
  1 page   = 1
  2 pages  = 2
  3–5 pgs  = 3
  6–10 pgs = 4
  11+ pgs  = 5

Funnel Multiplier:
  TOFU + MOFU + BOFU = 1.5×
  Any two stages     = 1.2×
  One stage only     = 1.0×
  No classifiable    = 0.5×

Combined Score = Raw Score × Multiplier

Classification:
  0      = MISSING  (no content)
  1–2    = WEAK     (minimal, thin, or single-stage)
  2–4    = MEDIUM   (adequate but not comprehensive)
  4.5+   = STRONG   (multi-page, multi-stage coverage)

Additional Flags:
  STRONG pages + no pillar → "Hub missing"
  MEDIUM pages + pillar → "Pillar without support"
  WEAK + STALE freshness → "Priority refresh"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — SITEMAP DISCOVERY PATH SEQUENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
SITEMAP DISCOVERY SEQUENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Attempt 1:  [domain]/sitemap.xml
Attempt 2:  [domain]/sitemap_index.xml
Attempt 3:  [domain]/wp-sitemap.xml
Attempt 4:  [domain]/news-sitemap.xml
Attempt 5:  [domain]/page-sitemap.xml
Attempt 6:  [domain]/post-sitemap.xml
Attempt 7:  [domain]/robots.txt → parse Sitemap: directives
Attempt 8:  [domain]/sitemap/

If all fail → PROMPT USER:
  Option 1: Enter sitemap URL directly
  Option 2: Paste URL list manually
  Option 3: Proceed with homepage crawl inference (limited)

Homepage Crawl Inference (Option 3):
  Extract: navigation links, blog index links, footer links
  Flag: PARTIAL ANALYSIS — homepage inference only
  Confidence: MEDIUM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: research/website-analysis.skill*
*Nexus SEO Operating System — Version 1.0.0*
