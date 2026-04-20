# SKILL: serp/serp-filter.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: SERP / Quality Control Gate
# EXECUTION PRIORITY: PRE-ANALYSIS — Runs before deep-serp-analysis.skill on every SERP result set.
# POSITION: SERP quality gate. The first and mandatory filter that every raw SERP result must pass before any competitive analysis begins.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **SERP quality control gate** of the Nexus system.

Before any SERP analysis begins — before content patterns are extracted, before blueprints are generated, before competitive intelligence is assembled — this skill processes the raw set of URLs returned for a target keyword and removes every result that would distort the competitive intelligence produced downstream. The goal is to ensure that every skill in the SERP analysis pipeline operates on a clean, high-quality, editorially coherent result set — not on a mix of documentation pages, product listings, homepages, and thin affiliate spam that happen to rank but represent no meaningful content strategy signal.

SERP analysis is only as useful as the pages being analyzed. If product pages, Wikipedia articles, and API documentation are included in the competitive analysis, the resulting content patterns, format recommendations, and differentiation strategies will be based on content types that the target audience does not read and that do not compete in the editorial content landscape. A guide-writer competing for a content keyword is not competing against a Wikipedia page or a SaaS product homepage — including those in the analysis produces misleading signals and, ultimately, misleading content recommendations.

This skill also enforces the minimum sample rule — ensuring that over-aggressive filtering does not reduce the usable result set below the threshold required for statistically meaningful pattern analysis. When the filtered set is too small, the skill relaxes its criteria and flags the output accordingly, so downstream skills can calibrate their confidence in the analysis.

### Primary Responsibilities

| Responsibility                         | Description                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| **Exclusion Category Enforcement**     | Apply the full list of excluded content types — removing documentation, product pages, directories, and thin content  |
| **Inclusion Category Validation**      | Verify that retained results are genuinely editorial content — guides, articles, comparisons, tutorials, and reviews   |
| **URL Pattern Filtering**              | Detect non-editorial URLs by path patterns — /docs/, /wiki/, /product/, /support/, and related patterns               |
| **Title and Intent Classification**    | Evaluate each result's title for editorial vs. transactional vs. navigational intent                                  |
| **Domain Type Detection**              | Identify the domain type (documentation host, news site, corporate site, editorial publisher) and apply domain-level rules |
| **Edge Case Handling**                 | Apply special handling for high-quality Reddit threads, official company blogs, and hybrid documentation-guides        |
| **Minimum Sample Enforcement**         | When filtered results drop below 5, relax exclusion criteria and add borderline results; flag limited competition      |
| **Deduplication**                      | Remove duplicate URLs and limit same-domain overrepresentation                                                         |
| **Rejection Log Production**           | For every removed URL, record the URL and the specific reason for removal                                              |
| **Borderline Flagging**                | When uncertain about a result's inclusion, retain it and mark as BORDERLINE rather than silently removing it          |

### What This Skill Is NOT

- It is **not** a content quality evaluator. It filters by content TYPE and INTENT — not by writing quality or topical depth (those are assessed by `deep-serp-analysis.skill`).
- It is **not** a ranking validator. It does not check whether results should rank — it only determines whether they are relevant editorial competition.
- It is **not** a keyword intent classifier. It uses intent signals to filter transactional pages — but it does not produce intent classifications for downstream use.
- It is **not** optional. No SERP result set proceeds to `deep-serp-analysis.skill` without passing through this filter first.
- It is **not** aggressive by design. When uncertain, it keeps the result. Over-filtering is worse than under-filtering — a borderline-included result is flagged and can be re-evaluated; a silently removed result is gone.

### The Defining Standard of This Skill

> The question this skill answers for every SERP result is:
>
> "Is this page part of the editorial content landscape for this keyword —
> meaning: is it the kind of content a human writer would be expected to compete with?"
>
> If YES → keep it.
> If NO → remove it and log exactly why.
> If UNCERTAIN → keep it and mark it BORDERLINE.
>
> The goal is a clean, analysis-ready result set —
> not the smallest possible set.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                  | Description                                                                    | Required |
|------------------------|--------------------------------------------------------------------------------|----------|
| `serp_results`         | Raw list of SERP URLs — up to 20 results — as returned for the target keyword  | YES      |
| `target_keyword`       | The exact keyword phrase for which these SERP results were retrieved           | YES      |

### Per-Result Fields

For each URL in the raw SERP result set, the following fields are used if available:

| Field                  | Description                                                                    | Source              | Required |
|------------------------|--------------------------------------------------------------------------------|---------------------|----------|
| `url`                  | Full URL of the result                                                         | SERP data           | YES      |
| `title`                | The page's SERP title (the text shown in the search result)                   | SERP data           | SOFT     |
| `snippet`              | The SERP meta description or auto-generated snippet shown beneath the title    | SERP data           | SOFT     |
| `domain`               | The base domain of the result (extracted from URL)                             | Extracted from URL  | DERIVED  |
| `position`             | The ranking position of the result (1–20)                                      | SERP data           | SOFT     |

### Optional Enrichment Inputs

| Field                  | Description                                                                    | Used For                                              |
|------------------------|--------------------------------------------------------------------------------|-------------------------------------------------------|
| `serp_features`        | List of active SERP features (featured snippet, PAA, image pack, etc.)        | Intent classification cross-reference (Step 4)       |
| `dominant_intent`      | Intent classification from `serp-intelligence.skill` if already run           | Step 4 title/intent check context                     |
| `niche_context`        | The topic domain/niche (e.g., "B2B SaaS", "Medical", "Legal", "Technical")   | Step 5 edge case handling — some niches retain academic/official sources |

### Input Pre-Conditions

1. **Is `serp_results` non-empty?** If the raw result list is empty → halt. Log: "No SERP results received — filter cannot run on empty input."
2. **Is `target_keyword` provided?** If not → title and intent filtering (Step 4) cannot be performed accurately → proceed with URL and domain filtering only; flag as KEYWORD-ABSENT.
3. **What is the initial result count?** Record the raw count. This is the denominator for the minimum sample check in Step 6.
4. **Has this keyword's SERP been filtered before in this session?** → Check session cache. If a filtered SERP set exists for this keyword → return cached result unless the user has explicitly requested a refresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Two Simultaneous Outputs

This skill produces two outputs simultaneously for every run. Both are mandatory.

**Output 1 — Filtered SERP List:**

```
SERP FILTER — CLEAN RESULT SET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Keyword          : [keyword]
Raw Results Received    : [count]
Results After Filter    : [count]
Borderline Results      : [count]
Rejection Rate          : [%]
Minimum Sample Triggered: [YES / NO]
Filter Confidence       : [HIGH / MEDIUM / LOW]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

INCLUDED RESULTS (Ready for Analysis):

| # | Original Rank | URL                                | Domain                | Content Type        | Status     | Notes              |
|---|---------------|------------------------------------|-----------------------|---------------------|------------|--------------------|
| 1 | 1             | [URL]                              | [domain]              | Blog Article        | INCLUDED   |                    |
| 2 | 3             | [URL]                              | [domain]              | Comprehensive Guide | INCLUDED   |                    |
| 3 | 4             | [URL]                              | [domain]              | Comparison Article  | INCLUDED   |                    |
| 4 | 7             | [URL]                              | [domain]              | Tutorial            | INCLUDED   |                    |
| 5 | 9             | [URL]                              | [domain]              | Editorial Review    | BORDERLINE | Hybrid doc/guide   |
| 6 | 11            | [URL]                              | [domain]              | Case Study          | INCLUDED   |                    |
...

CONTENT TYPE DISTRIBUTION:
  Blog Article       : [count]
  Guide (Long-form)  : [count]
  Comparison         : [count]
  Tutorial           : [count]
  Case Study         : [count]
  Editorial Review   : [count]
  Borderline         : [count]

DOMAIN REPRESENTATION:
  Unique domains     : [count]
  Same-domain entries: [N/A / flagged — see dedup notes]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output 2 — Rejection Log:**

```
SERP FILTER — REJECTION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Target Keyword          : [keyword]
Total Rejections        : [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | Original Rank | URL                    | Domain          | Rejection Reason            | Category              |
|---|---------------|------------------------|-----------------|-----------------------------|-----------------------|
| 1 | 2             | [URL]                  | [domain]        | Official documentation      | EXCLUSION CAT 1       |
| 2 | 5             | [URL]                  | [domain]        | Product page (/pricing/)    | EXCLUSION CAT 2       |
| 3 | 6             | [URL]                  | [domain]        | Wikipedia                   | EXCLUSION CAT 6       |
| 4 | 8             | [URL]                  | [domain]        | Thin affiliate page (<500w) | EXCLUSION CAT 9       |
| 5 | 10            | [URL]                  | [domain]        | Transactional title — no editorial value | INTENT FILTER |
| 6 | 14            | [URL]                  | [domain]        | Same-domain duplicate       | DEDUP RULE            |
| 7 | 17            | [URL]                  | [domain]        | Short forum thread (<500w)  | EXCLUSION CAT 10      |
...

REJECTION SUMMARY BY CATEGORY:
  Exclusion Cat 1 (Documentation)   : [count]
  Exclusion Cat 2 (Product pages)   : [count]
  Exclusion Cat 3 (Landing pages)   : [count]
  Exclusion Cat 4 (Homepages)        : [count]
  Exclusion Cat 5 (Wikipedia)        : [count]
  Exclusion Cat 6 (Government)       : [count]
  Exclusion Cat 7 (Academic)         : [count]
  Exclusion Cat 8 (Directories)      : [count]
  Exclusion Cat 9 (Thin affiliate)   : [count]
  Exclusion Cat 10 (Forum threads)   : [count]
  Exclusion Cat 11 (Outdated news)   : [count]
  Intent Filter (Transactional)      : [count]
  Deduplication                      : [count]
  Total Rejected                     : [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Field Definitions

| Field               | Definition                                                                                          |
|---------------------|-----------------------------------------------------------------------------------------------------|
| `Original Rank`     | The position (1–20) the result held in the raw SERP                                                |
| `Content Type`      | The editorial content type classification of the retained result                                   |
| `Status`            | INCLUDED (clean keep) / BORDERLINE (kept with flag) / RELAXED (kept due to minimum sample rule)   |
| `Rejection Reason`  | Specific reason the result was removed — never vague                                                |
| `Category`          | Which exclusion category or rule triggered the rejection                                           |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DEFINE AND APPLY EXCLUSION CATEGORIES

Apply eleven defined exclusion categories to every result. A result that matches any exclusion category is removed unless an edge case exception (Step 5) overrides the exclusion.

---

**EXCLUSION CATEGORY 1 — Official Documentation**

**Definition:** Pages produced by software companies, platforms, or standards organizations as technical reference documentation for their products, APIs, or specifications. These pages are designed to serve existing users of a product — not to educate or persuade potential readers.

**Signals:**

| Signal Type         | Specific Pattern                                                                          |
|---------------------|-------------------------------------------------------------------------------------------|
| Subdomain           | `docs.*`, `developer.*`, `dev.*`, `api.*`, `developers.*`, `reference.*`                |
| URL path            | `/docs/`, `/documentation/`, `/api-reference/`, `/sdk/`, `/reference/`, `/developers/`  |
| Domain pattern      | `docs.[platform].com`, `developer.[brand].com`, `api.[brand].com`                       |
| Title signals       | "[Product] Documentation", "[Feature] API Reference", "[Tool] SDK Guide"                |
| Content signal      | Structured reference content (parameters, response codes, method signatures)            |

**Rejection log entry:** `"Official documentation — [URL] is a reference documentation page serving existing product users, not editorial readers."`

**Edge case:** If the documentation page is structured as a conceptual guide (e.g., "Introduction to [Concept]" on a developer blog that teaches the concept rather than listing API methods) → evaluate in Step 5 as a potential hybrid.

---

**EXCLUSION CATEGORY 2 — Product Pages**

**Definition:** Pages whose primary purpose is to describe, showcase, or sell a specific product or service. These pages serve commercial conversion intent — they are not editorial competition.

**Signals:**

| Signal Type         | Specific Pattern                                                                          |
|---------------------|-------------------------------------------------------------------------------------------|
| URL path            | `/product/`, `/products/`, `/pricing/`, `/plans/`, `/buy/`, `/shop/`, `/store/`, `/purchase/`, `/checkout/` |
| URL path continued  | `/solutions/`, `/platform/`, `/features/[specific-feature]/`, `/enterprise/`            |
| Title signals       | "[Product Name] — Pricing & Plans", "[Tool] Features", "Buy [Product]", "[Service] Plans" |
| Content signals     | Pricing tables, feature comparison matrices between plans, "Get Started" as primary CTA  |
| Domain signals      | The URL is on the SaaS product's own domain AND the path is within the product section    |

**Rejection log entry:** `"Product page — [URL] is a commercial product/service page targeting buyers, not an editorial piece educating readers."`

**Edge case:** A product's blog or resources section (e.g., `[tool].com/blog/[article]`) is NOT a product page — it is potentially editorial content and must be evaluated by Step 2 and Step 4.

---

**EXCLUSION CATEGORY 3 — Landing Pages**

**Definition:** Marketing-focused pages designed for conversion — typically with minimal body content, no navigational structure, a single dominant CTA, and purpose-built to convert a specific traffic source. These do not represent editorial content competition.

**Signals:**

| Signal Type         | Specific Pattern                                                                          |
|---------------------|-------------------------------------------------------------------------------------------|
| URL path            | `/lp/`, `/landing/`, `/campaign/`, `/offer/`, `/promo/`, `/free-trial/`, `/demo/`        |
| Content signals     | Very short word count relative to topic; single dominant CTA; no table of contents      |
| Title signals       | Urgency language ("Get [X] Free", "Start Your [Trial]", "Limited Time Offer")           |
| Structural signal   | No navigational links to rest of site; standalone page with form as primary element      |

**Rejection log entry:** `"Landing page — [URL] is a conversion-focused marketing page with no substantive editorial content."`

---

**EXCLUSION CATEGORY 4 — Company Homepages**

**Definition:** The root domain home page of a company or organization (e.g., `https://example.com/` with no path). Homepages serve navigational intent — they introduce the brand, not teach a topic.

**Signals:**

| Signal Type         | Specific Pattern                                                                          |
|---------------------|-------------------------------------------------------------------------------------------|
| URL structure       | URL has no path component beyond `/`, OR path is only `/home` or `/index`                |
| Title signals       | "[Company Name] — [Tagline]", "[Brand] | [Short Brand Description]"                      |
| Content signals     | Service overview sections, "About Us", hero section with brand pitch as primary content  |

**Rejection log entry:** `"Company homepage — [URL] is the brand's root domain page serving navigational intent, not editorial content."`

**Edge case:** A homepage of a media publication (e.g., a news magazine's home) that happens to appear in the SERP for a topic may retain editorial value — evaluate via Step 4.

---

**EXCLUSION CATEGORY 5 — Wikipedia**

**Definition:** Any page on the Wikipedia network (en.wikipedia.org, any language variant, or any Wikipedia mirror).

**Signals:**

| Signal Type         | Specific Pattern                                                          |
|---------------------|---------------------------------------------------------------------------|
| Domain              | `en.wikipedia.org`, `*.wikipedia.org`, `wikipedia.org`                   |
| URL path            | `/wiki/[Article_Title]`                                                   |

**Rejection log entry:** `"Wikipedia — [URL] is a Wikipedia reference article. Not editorial content competition; provides definitional coverage that new editorial content cannot realistically target."`

**Rationale for unconditional exclusion:** Wikipedia almost never represents achievable content competition — a site cannot replicate Wikipedia's authority model. Including it distorts format and depth analysis by introducing an encyclopedic reference format that editorial content cannot emulate.

---

**EXCLUSION CATEGORY 6 — Government and Official Institution Sites**

**Definition:** Pages on government domains (.gov, .gov.uk, .gc.ca, etc.) or official regulatory/standards body websites. Relevant only when the target keyword is specifically about a government program, legal regulation, or official standard.

**Signals:**

| Signal Type         | Specific Pattern                                                          |
|---------------------|---------------------------------------------------------------------------|
| Domain TLD          | `.gov`, `.gov.uk`, `.gc.ca`, `.gov.au`, `.edu` (for institutions)        |
| Domain pattern      | `[agency].gov`, `[ministry].gov.[country]`                               |

**Default action:** REMOVE.

**Exception:** If the `niche_context` is Legal, Healthcare, Financial Regulation, or Tax — retain government pages IF the target keyword is directly regulatory in nature (e.g., "GDPR compliance requirements" → retain ICO.org.uk). Flag as RETAINED — NICHE EXCEPTION.

**Rejection log entry:** `"Government/official institution site — [URL] is a government domain. Removed as non-editorial competition unless keyword is specifically regulatory."`

---

**EXCLUSION CATEGORY 7 — Academic Papers and Research Studies**

**Definition:** Peer-reviewed academic publications, research papers, academic conference proceedings, and university-produced research content. These serve an academic audience with different reading patterns and content standards than editorial readers.

**Signals:**

| Signal Type         | Specific Pattern                                                          |
|---------------------|---------------------------------------------------------------------------|
| Domain              | `scholar.google.com`, `jstor.org`, `pubmed.ncbi.nlm.nih.gov`, `arxiv.org`, `researchgate.net`, `academia.edu` |
| URL path            | `/paper/`, `/article/`, `/abstract/`, `/doi/`                            |
| Title signals       | Citation format ("Author et al., Year"), abstract-style titles            |
| Content signals     | Abstract + methodology + references structure                             |

**Default action:** REMOVE.

**Exception:** If the `niche_context` is explicitly Academic, Scientific, or Medical Research — retain papers relevant to the keyword if they represent material that the target audience actively cites. Flag as RETAINED — ACADEMIC NICHE.

**Rejection log entry:** `"Academic paper — [URL] is a research publication targeted at academic/scientific audiences, not editorial readers in this space."`

---

**EXCLUSION CATEGORY 8 — Directory Listings and Aggregators**

**Definition:** Pages whose primary content is a structured list of other resources, businesses, services, or products — where the page itself provides little original editorial content and primarily aggregates external links or entries.

**Signals:**

| Signal Type         | Specific Pattern                                                                             |
|---------------------|----------------------------------------------------------------------------------------------|
| Domain pattern      | Yelp, G2, Capterra, Clutch, Product Hunt, AngelList, Crunchbase, Yellow Pages, and similar  |
| URL path            | `/listings/`, `/directory/`, `/software/`, `/category/` on aggregator domains               |
| Content signals     | The page is a list of 50+ entries with brief descriptions and external links; no editorial narrative |
| Title signals       | "Best [Category] Tools/Software/Services/Apps" on a pure directory site                     |

**Distinction note:** G2's individual product review page ≠ G2's category listing page. Individual reviews may have editorial value. Category directory pages do not.

**Rejection log entry:** `"Directory listing/aggregator — [URL] is a structured directory page that aggregates other resources without providing original editorial content."`

---

**EXCLUSION CATEGORY 9 — Thin Affiliate Pages**

**Definition:** Pages that exist primarily to drive affiliate commission by linking to products — characterized by thin original content, heavy use of affiliate disclaimers, and lists of "best" products with minimal analytical depth.

**Signals:**

| Signal Type         | Specific Pattern                                                                             |
|---------------------|----------------------------------------------------------------------------------------------|
| Content signals     | Predominantly "Buy on Amazon" / "Visit Site" buttons with minimal supporting text per item  |
| Word count proxy    | Estimated < 500 words of original content (excluding affiliate link blocks)                 |
| Title signals       | "Best [Product Type] on Amazon", "[N] Best [Products] We Reviewed"                         |
| Domain signals      | Known thin affiliate domains with no editorial reputation                                    |
| Structural signal   | No author attribution; no original research claims; no identifiable editorial perspective   |

**Important distinction:** A well-researched "Best X" article with 3,000+ words, original testing notes, methodology disclosure, and genuine comparison criteria is NOT a thin affiliate page — it is editorial content. The thin/affiliate disqualification is about content DEPTH and ORIGINALITY, not about the presence of affiliate links.

**Rejection log entry:** `"Thin affiliate page — [URL] has minimal original content (estimated < 500 words) with primary purpose being affiliate link monetization rather than editorial value."`

---

**EXCLUSION CATEGORY 10 — Short Forum Threads**

**Definition:** Forum or community threads (Reddit, Quora, Stack Overflow, niche forums) where the total content is below the 500-word threshold and where there is no substantive editorial answer — only short, conversational replies.

**Signals:**

| Signal Type         | Specific Pattern                                                          |
|---------------------|---------------------------------------------------------------------------|
| Domain              | `reddit.com`, `quora.com`, `stackoverflow.com`, niche forum domains       |
| Content threshold   | Thread total word count estimated < 500 words                             |
| Content quality     | No extended answer with practical depth; conversation fragments only      |

**Important distinction:** A long Reddit thread with 2,000+ words of detailed community advice, multiple highly-upvoted comprehensive responses, and genuine topical depth is NOT excluded — it is handled as an edge case in Step 5.

**Rejection log entry:** `"Short forum thread — [URL] is a forum thread below the 500-word threshold with no substantive editorial content."`

---

**EXCLUSION CATEGORY 11 — Outdated News Articles**

**Definition:** News articles that were timely when published but are now significantly outdated — specifically, news content that reports on events, announcements, or data that has since been superseded. Applies to articles older than 24 months that cover time-sensitive news topics.

**Signals:**

| Signal Type         | Specific Pattern                                                                             |
|---------------------|----------------------------------------------------------------------------------------------|
| Publication date    | Article published > 24 months ago AND topic is time-sensitive (not evergreen)               |
| Domain pattern      | News publication domains (techcrunch, forbes, reuters, etc.) with date-stamped URLs        |
| Title signals       | "Breaking:", "[Company] Announces", "[Year] Results", specific event references            |
| Content signals     | Reporting on a past event with no evergreen educational value; purely time-stamped          |

**Important distinction:** A Forbes article from 2021 titled "What Is CRM Software and Why Does It Matter?" is evergreen editorial content — NOT outdated news. A TechCrunch article from 2021 titled "Salesforce Acquires Slack in $27.7B Deal" IS outdated news — it reported an event; it provides no ongoing educational value.

**Rejection log entry:** `"Outdated news — [URL] is a time-stamped news article (published [date]) reporting on a past event with no remaining evergreen editorial value."`

---

### ▸ STEP 2 — DEFINE AND VERIFY INCLUSION CATEGORIES

After exclusion categories are applied, verify that remaining results belong to at least one inclusion category. Inclusion categories represent the content types that provide meaningful editorial intelligence for SERP pattern analysis.

**The Seven Inclusion Categories:**

| Category              | Definition                                                                                         | Characteristic Signals                                                    |
|-----------------------|----------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| **Blog Article**      | An editorial piece published on a site's blog section — informational, educational, or opinion-based | `/blog/`, `/posts/`, `/articles/` in URL; editorial byline; conversational or authoritative prose |
| **Guide**             | A structured, long-form educational piece designed to comprehensively cover a topic                 | Title contains "Guide", "Ultimate", "Complete", "How to"; has navigable sections; 1,500+ words |
| **Tutorial**          | A step-by-step instructional piece for completing a specific task or process                       | Numbered steps; process-oriented structure; practical application focus   |
| **Comparison**        | An editorial piece that compares two or more options, tools, or approaches side-by-side            | Title contains "vs", "versus", "compared", "comparison"; structured comparison format |
| **Case Study**        | An in-depth examination of a specific real-world implementation, outcome, or example               | Real-world scenario; outcome data; narrative structure; typically 1,000+ words |
| **Editorial Review**  | A first-person, experience-based assessment of a tool, product, approach, or service               | Author perspective; pros/cons structure; verdict or recommendation        |
| **Long-Form Article** | Any substantive editorial article that does not fit the above categories but provides genuine educational or analytical value | 1,000+ words; original perspective; designed for readers, not buyers    |

**Inclusion Verification Protocol:**

For each result that survived exclusion filtering:
1. Does it match at least one inclusion category?
2. Is the match genuine — based on the actual page content/structure signals — or superficial (a thin page with a tutorial-style title but no tutorial content)?
3. If match is genuine → classify and include.
4. If match is superficial → evaluate as BORDERLINE (Step 5 failsafe applies).

---

### ▸ STEP 3 — URL PATTERN FILTERING

Apply automated pattern-based filtering to URLs before engaging in title or content analysis. URL patterns are the fastest, most reliable first-pass filter.

**Phase 3A — Exclusion URL Path Patterns**

Remove any URL containing these path components:

```
URL PATH EXCLUSION PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Documentation:
  /docs/          /documentation/     /api/           /api-reference/
  /reference/     /sdk/               /developer/     /developers/
  /changelog/     /release-notes/     /version/       /getting-started/ (on doc sites)
  /tutorial/ (on doc sites — distinguish from editorial tutorial sites)

Product & Commercial:
  /product/       /products/          /pricing/       /plans/
  /buy/           /shop/              /store/         /purchase/
  /checkout/      /cart/              /order/         /subscription/
  /upgrade/       /enterprise/        /business/ (when on a product's main domain)

Landing Pages:
  /lp/            /landing/           /campaign/      /offer/
  /promo/         /free-trial/        /demo/ (when it's a demo signup page)
  /signup/        /register/ (as the full page purpose)

Support & Utility:
  /support/       /help/ (on product domains)    /faq/ (on product domains)
  /status/        /login/             /account/

Homepages (by URL structure):
  URL = https://[domain]/ with NO path component
  URL = https://[domain]/home
  URL = https://[domain]/index
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3B — Exclusion Domain Patterns**

Apply domain-level exclusions regardless of path:

```
DOMAIN EXCLUSION PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Wikipedia Network:
  en.wikipedia.org    *.wikipedia.org     simple.wikipedia.org

Government TLDs (unless niche exception applies):
  *.gov               *.gov.uk            *.gc.ca             *.gov.au
  *.gov.[country]     *.mil               *.edu (for institutional pages)

Academic Databases:
  scholar.google.com  jstor.org           pubmed.ncbi.nlm.nih.gov
  arxiv.org           researchgate.net    academia.edu
  semanticscholar.org sciencedirect.com   springer.com (journal pages)

Directory Aggregators:
  g2.com/categories/* capterra.com/software/* getapp.com/[category]/*
  softwareadvice.com  trustradius.com     producthunt.com (category pages)
  clutch.co/[directory]/* yelp.com/biz/*  yellowpages.com/*
  crunchbase.com/organization/* angelist.co
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3C — Subdomain Exclusion Patterns**

Apply subdomain-level exclusions:

```
SUBDOMAIN EXCLUSION PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Documentation subdomains:
  docs.*              developer.*         developers.*
  api.*               dev.*               reference.*
  support.* (on product domains)         help.* (on product domains)
  status.*            changelog.*         releases.*

Exception — Do NOT apply to:
  blog.*              learn.*             academy.*
  university.*        resources.*         insights.*
  (These may be editorial content and should be evaluated by Step 4)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3D — Ambiguous Path Handling**

Some paths are ambiguous — they could be editorial OR non-editorial depending on context:

| Ambiguous Path             | Ambiguity                                                           | Resolution                                                             |
|----------------------------|---------------------------------------------------------------------|------------------------------------------------------------------------|
| `/tutorial/` or `/tutorials/` | Could be on an editorial site (legit tutorial) OR doc site        | Check domain type — editorial publisher → KEEP; SaaS product main site → evaluate |
| `/guide/` or `/guides/`    | Could be editorial or doc-style                                     | Check title — educational topic title → KEEP; product usage title → evaluate |
| `/resources/`              | Could be editorial resource library or thin directory              | Check for article-style titles → KEEP; check for PDF/file lists → BORDERLINE |
| `/knowledge-base/` or `/kb/` | Support knowledge base vs. editorial knowledge hub               | Check title and content signals → support answer format → BORDERLINE or remove |
| `/learn/`                  | Could be a learning hub (editorial) or product onboarding          | Title and content check required → if topic-educational → KEEP        |

---

### ▸ STEP 4 — TITLE AND INTENT CHECK

After URL and domain filtering, evaluate each remaining result's SERP title to confirm editorial intent and reject pages with purely transactional or navigational intent that slipped through URL filtering.

**Phase 4A — Intent Classification from Title**

Classify each result's title into one of three intent categories:

| Intent Category   | Signals in Title                                                                           | Action        |
|-------------------|--------------------------------------------------------------------------------------------|---------------|
| **Informational** | Starts with "How", "What", "Why", "When", "Guide to", "Introduction to", "Understanding"  | KEEP          |
| **Editorial**     | Contains "vs", "Review", "Best", "Comparison", "Analysis", "Explained", numbered lists    | KEEP          |
| **Commercial**    | Contains pricing language, CTAs, trial offers, "Buy", "Get Started", "Free Trial", "[Price]" | REJECT (transactional) |
| **Navigational**  | Brand name + product name only; "[Product] Home"; "[Company] — [Tagline]"                | REJECT (navigational) |
| **Ambiguous**     | None of the above are clearly dominant; mixed signals                                     | BORDERLINE    |

**Phase 4B — Title Red Flags (Reject Immediately)**

Remove any result whose SERP title contains these patterns:

```
TITLE REJECTION PATTERNS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Commercial intent signals:
  "Buy [X]"           "Get Started"       "Free Trial"
  "Starting at $[N]"  "Plans & Pricing"   "[X]% Off"
  "Sign Up"           "Download Now"      "Request Demo"
  "Shop [X]"          "Order [X]"         "Book Now"

Navigation intent signals:
  "[Brand] | [Short Brand Description]" (homepage title pattern)
  "[Product Name] — Official Site"
  "[Company] — [Tagline only]"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 4C — Title Green Flags (Retain with Higher Confidence)**

Results whose titles contain these patterns receive a CONFIDENT INCLUSION flag — their editorial nature is clearly signaled:

```
TITLE RETENTION SIGNALS (Confident Inclusion)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  "How to [Action]"       "What Is [Concept]"         "Guide to [Topic]"
  "[N] Best [X] for [Y]" "[X] vs [Y]"                "Complete Guide"
  "[Topic] Explained"     "[Topic] Tutorial"          "Understanding [Concept]"
  "Review: [X]"           "[X] Compared"              "Case Study: [X]"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 4D — Snippet Validation (When Available)**

When the SERP snippet is available, use it to validate the title's intent classification:

1. If title is BORDERLINE but snippet describes educational content → upgrade to INFORMATIONAL/EDITORIAL → KEEP.
2. If title appears informational but snippet has pricing/conversion language → downgrade to COMMERCIAL → REJECT.
3. If snippet is a product feature list → REJECT regardless of title.
4. If snippet describes a step-by-step process → KEEP (tutorial confirmation).

**Phase 4E — Keyword Relevance Check**

Beyond intent, verify that the page is actually relevant to the target keyword — not just accidentally ranking for it:

1. Does the title contain the target keyword or a close semantic variant? If not → check snippet.
2. If neither title nor snippet references the target keyword or any closely related term → flag as POTENTIALLY IRRELEVANT.
3. POTENTIALLY IRRELEVANT pages are BORDERLINE — they are kept but flagged. The SERP position system placed them here; there may be a reason not visible from title/snippet alone.

---

### ▸ STEP 5 — EDGE CASE HANDLING

Apply specific handling logic for content types that are not clearly in or out of the editorial content set. These edge cases prevent over-filtering.

**EDGE CASE 1 — Official Company Blogs**

An official blog published by a software company or brand (e.g., `blog.hubspot.com`, `ahrefs.com/blog/`, `buffer.com/resources/`) is EDITORIAL content even though it is published by a commercial entity.

**Distinction rule:**
- `hubspot.com/products/crm` → PRODUCT PAGE → REJECT.
- `blog.hubspot.com/marketing/crm-guide` → EDITORIAL BLOG → KEEP.
- `hubspot.com/blog/crm-software-comparison` → EDITORIAL BLOG (on main domain's blog) → KEEP.

Detection: The URL path explicitly includes `/blog/`, `/resources/`, `/insights/`, `/learn/`, OR the subdomain is `blog.*`, `resources.*`, `learn.*`, `academy.*` → treat as editorial.

**Log entry:** `KEPT — Official company blog: [URL] is an editorial article published on [Brand]'s blog section."`

---

**EDGE CASE 2 — High-Quality Reddit Threads**

A Reddit thread is NOT automatically excluded. It is excluded if it is a short thread (< 500 words — Category 10). A long, substantive Reddit thread with highly-upvoted comprehensive answers can provide meaningful editorial intelligence.

**Inclusion criteria for Reddit threads:**
1. Thread total content (post + top answers) estimated ≥ 500 words.
2. At least one response has substantial depth (≥ 200 words in a single reply).
3. The thread is NOT exclusively question-with-no-good-answers format.
4. The keyword is directly addressed in the discussion.

**Classification when kept:** `"High-quality Reddit thread"` — classified separately from other editorial content. Noted in the clean list.

**Log entry:** `"KEPT — High-quality Reddit thread: [URL] has substantial community discussion (est. 800+ words) with detailed practical answers directly addressing the keyword."`

---

**EDGE CASE 3 — Hybrid Documentation Guides**

Some documentation pages are genuinely educational — they teach a concept from first principles rather than listing API parameters. These hybrid doc-guides can provide meaningful editorial signals.

**Inclusion criteria for hybrid documentation:**
1. The page teaches a concept rather than listing product-specific parameters.
2. The title is concept-educational rather than product-operational.
3. The content would be useful to a reader who does NOT use the product.
4. The page has a narrative structure (introduction → explanation → examples) rather than a reference structure (parameter → type → description).

**Example:** A Cloudflare blog post titled "What Is HTTPS and How Does It Work?" is a hybrid doc-guide that teaches the concept independently of Cloudflare's product — KEEP.

**Example:** `developers.google.com/search/docs/fundamentals/how-search-works` teaches how search indexing works conceptually — potentially KEEP if relevant; flag as BORDERLINE if very product-specific.

**Classification when kept:** `"Hybrid documentation guide — BORDERLINE"` → included with flag.

---

**EDGE CASE 4 — Substack and Newsletter Pages**

Substack articles, Ghost-published newsletters, and similar newsletter-as-blog content are EDITORIAL if they contain substantive editorial content and are not behind a paywall.

**Inclusion criteria:**
1. The page is publicly accessible (no paywall).
2. Content has genuine editorial depth (≥ 500 words).
3. The topic is relevant to the target keyword.

**Classification when kept:** `"Newsletter / editorial publication — Substack/equivalent"`.

---

**EDGE CASE 5 — Quora Answers**

Quora is treated similarly to Reddit — excluded if thin, kept if substantive.

**Inclusion criteria:**
1. The top answer(s) are estimated ≥ 300 words.
2. The answer provides genuine practical guidance (not just "it depends").
3. The question is directly relevant to the target keyword.

**Classification when kept:** `"Quora — detailed expert answer"` — BORDERLINE status.

---

### ▸ STEP 6 — MINIMUM SAMPLE RULE

After all exclusion and inclusion filtering is complete, verify that the retained result set meets the minimum threshold for meaningful SERP pattern analysis.

**Phase 6A — Minimum Sample Check**

| Filtered Result Count | Status            | Action                                                                    |
|-----------------------|-------------------|---------------------------------------------------------------------------|
| ≥ 8 results           | OPTIMAL           | No action required. Proceed.                                             |
| 5–7 results           | ADEQUATE          | Proceed. Note lower-than-ideal sample size in output.                    |
| < 5 results           | BELOW MINIMUM → TRIGGER RELAXED FILTERING |                          |

**Phase 6B — Relaxed Filtering Protocol (Triggered When < 5 Results)**

When fewer than 5 results survive standard filtering:

1. **Step 1 — Relax Category 11 (Outdated News):** Re-evaluate all news articles that were excluded for being outdated. If they are within 36 months (not 24 months), re-include as RELAXED status.

2. **Step 2 — Relax Category 8 (Directories) selectively:** Re-evaluate directory pages from quality aggregators (G2, Capterra) that have substantial editorial review content on individual category pages. If the page has editorial narrative alongside the listing structure → re-include as BORDERLINE.

3. **Step 3 — Relax Category 10 (Forum Threads):** Reduce the forum threshold from 500 words to 300 words. Any forum thread with ≥ 300 words of content is re-evaluated and included as RELAXED status if it provides relevant discussion.

4. **Step 4 — Include borderline rejections from Step 4 (Intent Check):** Any result previously classified as BORDERLINE in the title/intent check is re-evaluated. If the snippet provides any editorial signal → re-include as BORDERLINE.

5. **Stop when ≥ 5 results are retained.** Do not continue relaxing if the minimum is already met.

**Phase 6C — Minimum Sample Flag**

When relaxed filtering is triggered:

Add to the clean output header:
```
⚠️ LIMITED EDITORIAL COMPETITION DETECTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Only [N] editorial results found after standard filtering.
Relaxed filtering applied — [N relaxed results] added with RELAXED status.
Downstream analysis confidence: MEDIUM (limited SERP sample).
Possible causes:
  → Niche keyword with limited editorial coverage
  → Keyword dominated by product/documentation content
  → Emerging topic with few published editorial pieces
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Downstream Impact Note:**

`deep-serp-analysis.skill` and `content-pattern-extractor.skill` will both receive the LIMITED COMPETITION flag. They must calibrate their confidence levels accordingly:
- Pattern frequencies may not be reliable with < 5 results.
- Blueprint recommendations based on < 5 pages are PROVISIONAL.

---

### ▸ STEP 7 — REJECTION LOG PRODUCTION

For every URL removed at any step, produce a complete rejection log entry. This is not optional — every removal must be documented.

**Phase 7A — Rejection Log Entry Format**

For each rejected URL:

```
REJECTION LOG ENTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Original Rank   : [position in raw SERP]
URL             : [full URL]
Domain          : [base domain]
Title           : [SERP title — if available]
Rejection Reason: [specific, non-vague reason]
Category        : [Exclusion Category N / Intent Filter / Dedup Rule / Pattern Match]
Step Triggered  : [Step 1 – 6 where the rejection occurred]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 7B — Rejection Reason Specificity Requirements**

Every rejection reason must:
1. Name the specific type of content or the specific pattern that triggered exclusion.
2. Reference the URL or title element that identified it.
3. NOT use vague language ("not relevant", "doesn't fit", "wrong type").

**Required reason examples:**

| What Was Removed         | Specific Reason                                                                              |
|--------------------------|----------------------------------------------------------------------------------------------|
| Documentation page       | `"Official documentation — URL path /docs/ identifies this as a reference documentation page"` |
| Product page             | `"Product page — URL path /pricing/ and title 'Plans & Pricing' confirm commercial conversion intent"` |
| Wikipedia                | `"Wikipedia — en.wikipedia.org domain is unconditionally excluded from editorial competition analysis"` |
| Short forum thread       | `"Short forum thread — reddit.com URL with estimated content < 500 words; no substantive editorial answers detected"` |
| Transactional intent     | `"Transactional intent — title contains 'Get Started Free' indicating conversion-focused content, not editorial"` |
| Same-domain duplicate    | `"Same-domain duplicate — [domain] already has [N] results in the clean set; this lower-ranked result creates domain overrepresentation"` |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ DOMAIN TYPE DETECTION

Before applying URL pattern filtering, this skill performs a domain type assessment for each result's domain — identifying what category of site the domain belongs to. This prevents misclassification of results where the URL path alone is ambiguous.

**Domain Type Classification:**

| Domain Type              | Definition                                                                                       | Filtering Behavior                                              |
|--------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| **Editorial Publisher**  | A site whose primary purpose is producing editorial content — blog, magazine, independent review | Apply inclusion logic; most results from editorial publishers are KEPT |
| **SaaS Product Domain**  | A site primarily serving as a SaaS product's marketing and product pages                        | Apply strict URL pattern filtering — only blog/learn sections are kept |
| **Documentation Host**   | A site dedicated to technical reference documentation                                           | All results REJECTED regardless of URL path                    |
| **News Publication**     | A site that primarily publishes news — timely, event-based content                               | Apply Category 11 (outdated news) filtering strictly            |
| **Community Platform**   | Reddit, Quora, Stack Overflow, specialized forums                                               | Apply edge case rules (Step 5); minimum word count thresholds  |
| **Directory/Aggregator** | Sites whose primary purpose is listing other resources or businesses                            | Apply Category 8 filtering; individual review pages may be exempt |
| **Mixed Domain**         | A site that has both product/commercial sections AND editorial sections                          | Apply per-URL filtering — do not blanket-exclude the whole domain |

**Domain Type Detection Method:**

1. Check the domain against known domain type lists (SaaS tools, news publications, documentation hosts, community platforms, directories).
2. If not in a known list → infer from URL structure and title patterns.
3. Assign domain type and record it in the result's metadata.
4. Apply the domain type's corresponding filtering behavior.

---

### ▸ INTENT CLASSIFICATION REFINEMENT

This skill's intent classification (Step 4) goes beyond simple title-based keyword detection. It applies a multi-signal intent model:

**Multi-Signal Intent Classification:**

| Signal                    | Weight | How It's Evaluated                                                     |
|---------------------------|--------|------------------------------------------------------------------------|
| Title intent signals      | 40%    | Title keyword patterns — informational, editorial, commercial, navigational |
| Snippet content signals   | 30%    | Snippet describes process (editorial) vs. features/pricing (commercial) |
| URL structure signals     | 20%    | Path indicates article (/blog/, /guide/) vs. product (/pricing/, /product/) |
| Domain type signal        | 10%    | Editorial publisher → editorial intent assumed; SaaS domain → commercial intent assumed |

**Combined Intent Score:**

Sum the weighted signals. If combined score favors INFORMATIONAL or EDITORIAL → KEEP. If combined score favors COMMERCIAL → REJECT. If scores are balanced → BORDERLINE.

---

### ▸ HYBRID CONTENT DETECTION

Some pages blend characteristics of excluded and included content types. The hybrid content detection logic identifies these cases and applies nuanced filtering rather than blanket exclusion or inclusion.

**Hybrid Content Types:**

| Hybrid Type                                                              | Classification Approach                                                   |
|--------------------------------------------------------------------------|---------------------------------------------------------------------------|
| SaaS brand's editorial blog (product context + educational content)      | Evaluate on content quality — if editorial depth > commercial pitch → KEEP |
| Developer documentation with conceptual teaching (not just API reference)| Evaluate on audience — if teaches concept independently of the product → BORDERLINE |
| Forum thread with expert-level substantive answers                        | Evaluate on word count and depth → apply edge case rules (Step 5)        |
| News article with evergreen educational value                             | Evaluate on topic type — if it teaches rather than reports → KEEP         |
| Product review on an aggregator (not just a directory listing)           | Evaluate on review depth — if individual product review with editorial content → BORDERLINE |

Hybrid content is always assigned BORDERLINE status when retained — it is never assigned INCLUDED status regardless of how strong its editorial content appears.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate URLs

After all filtering is complete, the clean SERP list must contain each URL exactly once. If the same URL appears at multiple SERP positions (unusual but possible due to SERP personalization or data duplication) → keep the highest-ranked instance; remove all duplicates. Log each removal as `"DEDUP — duplicate URL at position [N]; retained at position [M] (higher rank)."`

### Rule 2 — Same-Domain Representation Cap

If the same domain appears more than three times in the filtered result set → flag the overrepresentation and apply domain capping:

**Domain Capping Protocol:**

1. Count the number of results from each unique domain in the filtered set.
2. If any domain appears ≥ 4 times → this domain is over-represented.
3. Keep the top 3 results from that domain (by original SERP rank).
4. Remove any additional results from that domain.
5. Log each removed result: `"DEDUP — same-domain overrepresentation: [domain] appeared N times; capped at 3; this result at position [P] removed."`

**Exception to the cap:** If the minimum sample rule (Step 6) has been triggered → do not apply the domain cap. When results are scarce, domain diversity is secondary to minimum sample compliance.

**Why this rule exists:** If one domain owns 6 of 15 SERP positions, including all 6 in the analysis would skew all content pattern analysis toward that domain's style, format, and approach. The analysis should reflect the competitive landscape, not a single dominant publisher.

### Rule 3 — No Borderline-Only Output

The clean result set must contain at least one INCLUDED (non-borderline) result. If every result in the filtered set is BORDERLINE → escalate: reduce the threshold further, include more RELAXED results, or flag the output as INSUFFICIENT EDITORIAL COMPETITION and alert the calling skill.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Filtering Behaviors

| Prohibited Behavior                                                                             | Reason                                                                          |
|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Removing a result without adding it to the rejection log                                        | Every removal must be documented — silent removal destroys the audit trail      |
| Using "not relevant" as a rejection reason without specifying what makes it irrelevant          | Vague rejection reasons are non-auditable and non-actionable                   |
| Removing results because they rank lower than position 10 (position alone is not a filter)     | SERP position is not an editorial quality signal — a highly relevant article at position 18 may outweigh a product page at position 2 |
| Keeping product pages because they have a high domain authority                                  | Domain authority is not a filter criterion — content type is                   |
| Filtering Reddit or Quora results categorically without checking word count                     | Community content is filtered by quality, not by platform                      |
| Treating "company blog" and "product page" interchangeably                                     | These are fundamentally different content types — editorial vs. commercial     |
| Triggering the minimum sample rule before completing all standard filtering steps               | Minimum sample is a LAST RESORT — all standard filtering must run first        |
| Applying the domain cap to BORDERLINE results when they represent the only editorial coverage  | Domain cap protects against overrepresentation — not against editorial scarcity |
| Accepting a thin affiliate page as editorial because it has 501 words                           | 500 words is a minimum threshold, not a content quality guarantee — content quality signals must also be present |
| Removing a borderline result rather than keeping and flagging it                                | When uncertain → KEEP AND FLAG. Removing uncertain results loses potentially valid competitive intelligence. |

### Required Filtering Behaviors

- Every removed URL must appear in the rejection log with a specific reason and category.
- Every BORDERLINE result must appear in the clean list with its BORDERLINE status clearly labeled.
- The minimum sample check must run after every filter pass — not just at the end.
- The rejection summary by category must accurately reflect all removals.
- Filter confidence level must be stated in the output header.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ deep-serp-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | serp-filter is called by deep-serp-analysis (pre-processing gate)                          |
| **Sends to**     | Clean filtered SERP list (all INCLUDED + BORDERLINE results with status flags)             |
| **Sends to (2)** | Rejection log (for record-keeping and confidence calibration)                              |
| **Protocol**     | deep-serp-analysis must not begin page analysis until serp-filter completes               |
| **Critical flag**| LIMITED EDITORIAL COMPETITION flag — deep-serp-analysis reduces confidence when triggered |

---

### ▸ content-pattern-extractor.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Downstream consumer (via deep-serp-analysis output chain)                                  |
| **Receives**     | Filtered result set (INCLUDED results only — BORDERLINE passed separately)                 |
| **Impact of LIMITED flag** | Pattern frequencies calculated from < 5 results are marked PROVISIONAL        |

---

### ▸ serp-blueprint-generator.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Downstream consumer (via content-pattern-extractor output chain)                           |
| **Receives**     | Pattern data derived from the filtered SERP set                                            |
| **Impact**       | Blueprint confidence level reflects filter confidence (HIGH/MEDIUM/LOW)                   |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Optional upstream context source                                                           |
| **Receives from**| Dominant intent classification — used in Step 4 title/intent check to validate edge cases  |
| **Relationship** | If serp-intelligence has already classified intent → serp-filter uses it as a validation signal for ambiguous titles |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                               | Recovery Action                                                                                             |
|---------------------------------------------------------------------------|---------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Raw SERP result list is empty                                             | Input pre-condition — 0 results received               | Halt. Log: "No SERP results received. Cannot filter an empty result set. Request SERP data from the calling skill before proceeding." |
| All 20 results are excluded by standard filtering                        | Step 6 — result count = 0 after standard filtering     | Apply full relaxed filtering protocol. If still 0 → flag: "ZERO EDITORIAL RESULTS — this keyword's SERP may be entirely dominated by non-editorial content. This is itself a strategic insight: the editorial landscape is wide open." |
| Target keyword is not provided                                             | Input pre-condition — target_keyword is null           | Proceed with URL and domain filtering only. Skip Step 4 (title intent check). Flag: "KEYWORD-ABSENT — title intent filtering skipped; URL and domain pattern filtering applied only." |
| A result's URL is unparseable (malformed, encoded, or broken)             | Step 3 — URL pattern matching fails                    | Skip URL pattern filtering for that result. Apply only domain and title filtering. Flag result as BORDERLINE with note: "Malformed URL — URL pattern filtering skipped." |
| All retained results are from the same domain                             | Step 6 / Dedup Rule 2                                  | Apply domain cap (max 3 from same domain). If only 1 domain is available → keep top 3 from that domain; flag: "SINGLE-DOMAIN SERP — all editorial results originate from [domain]. Pattern analysis confidence: LOW." |
| A result has no title and no snippet (URL only)                           | Step 4 — title and snippet both null                   | Classify as BORDERLINE. Apply URL and domain filtering only. Log: "No title or snippet available — BORDERLINE inclusion based on URL/domain analysis only." |
| Minimum sample rule triggers but relaxed filtering adds no new results   | Step 6 — relaxed filtering produces 0 additions        | Flag output as CRITICALLY LIMITED: "Fewer than 5 editorial results available even after relaxed filtering. Analysis confidence: MINIMAL. Results may not be representative of the editorial competitive landscape." |
| BORDERLINE results exceed INCLUDED results after filtering                | Step 7 — borderline count > included count             | Flag: "HIGH BORDERLINE RATIO — more borderline than clearly included results. Analysis confidence: MEDIUM. Consider manual review of borderline inclusions." |
| The same domain is excluded by both URL pattern AND domain list           | Multiple triggers for same result                      | Log only the most specific reason (domain list takes precedence over path pattern). Single rejection log entry per URL — no duplicate entries. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — EXCLUSION CATEGORY QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
ELEVEN EXCLUSION CATEGORIES — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CAT 1 — OFFICIAL DOCUMENTATION
  Signals: docs.*, developer.*, /docs/, /api/, /reference/, /sdk/
  Exception: Conceptual teaching pages that explain topics independently of the product

CAT 2 — PRODUCT PAGES
  Signals: /product/, /pricing/, /plans/, /features/, /buy/
  Exception: Brand's blog or resources section (evaluated separately)

CAT 3 — LANDING PAGES
  Signals: /lp/, /landing/, /campaign/, /offer/, /promo/, /free-trial/
  No exceptions

CAT 4 — COMPANY HOMEPAGES
  Signals: URL = https://[domain]/ with no meaningful path
  Exception: Media publication home with editorial aggregation

CAT 5 — WIKIPEDIA
  Signals: *.wikipedia.org, /wiki/[article]
  No exceptions — unconditional exclusion

CAT 6 — GOVERNMENT/OFFICIAL INSTITUTION
  Signals: .gov, .gov.uk, .edu (institutional), .mil
  Exception: Niche-specific regulatory content (GDPR, healthcare, legal)

CAT 7 — ACADEMIC PAPERS
  Signals: scholar.google, jstor, pubmed, arxiv, researchgate
  Exception: Academic niches where papers are the relevant content type

CAT 8 — DIRECTORY LISTINGS
  Signals: G2 category pages, Capterra listings, Clutch directories, Yelp, YP
  Exception: Individual product reviews with substantive editorial content

CAT 9 — THIN AFFILIATE PAGES
  Signals: <500w original content, primarily affiliate links, no editorial depth
  Exception: Well-researched "Best X" articles with 1,500+ words and methodology

CAT 10 — SHORT FORUM THREADS
  Signals: Reddit/Quora/forums with <500w total thread content
  Exception: Long threads ≥500w with substantive answers (Step 5 Edge Case 2/5)

CAT 11 — OUTDATED NEWS
  Signals: News articles >24 months old on time-sensitive events
  Exception: Evergreen educational pieces that happen to be published by news sites

MINIMUM SAMPLE RULE:
  <5 results after filtering → relax Categories 11, 8, 10, borderline intent
  Still flag as LIMITED EDITORIAL COMPETITION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — EDGE CASE DECISION TREE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
EDGE CASE DECISION TREE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
For any result that was triggered for exclusion but may have editorial value:

Q1: Is this a company blog or brand editorial section?
  → URL path contains /blog/, /resources/, /learn/, /insights/ on a product domain
  YES → KEEP as "Official company blog" — evaluate by Step 2 (inclusion)
  NO → Continue

Q2: Is this a Reddit or Quora thread?
  YES →
    Q2a: Does the thread have ≥500 words of total content?
      YES → KEEP as "High-quality community thread" — BORDERLINE status
      NO → REJECT as "Short forum thread" (Category 10)
  NO → Continue

Q3: Is this a documentation page with conceptual educational content?
  YES →
    Q3a: Does the content teach the concept independently of the product?
      YES → KEEP as "Hybrid documentation guide" — BORDERLINE status
      NO → REJECT as "Official documentation" (Category 1)
  NO → Continue

Q4: Is this a news article?
  YES →
    Q4a: Is it evergreen educational content (teaches rather than reports)?
      YES → KEEP as "Evergreen editorial" — evaluate by Step 2
      Q4b: Is it > 24 months old AND purely event-reporting?
        YES → REJECT as "Outdated news" (Category 11)
        NO → KEEP if ≤24 months; BORDERLINE if 18–24 months

Q5: Is this an aggregator/directory with individual product review content?
  YES →
    Q5a: Does the individual page have ≥500 words of original editorial review?
      YES → KEEP as "Individual editorial review" — BORDERLINE status
      NO → REJECT as "Directory listing" (Category 8)
  NO → Continue

Q6: Still uncertain after all questions?
  → KEEP with BORDERLINE flag
  → Log: "Retained under uncertainty — BORDERLINE; requires manual review"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — FILTER CONFIDENCE LEVEL DEFINITIONS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
FILTER CONFIDENCE LEVELS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HIGH (≥8 INCLUDED results, ≤20% BORDERLINE):
  The filtered SERP set is reliable for pattern analysis.
  Downstream skills may use pattern frequencies at full confidence.
  Pattern thresholds apply at stated levels.

MEDIUM (5–7 results, OR ≥30% BORDERLINE, OR relaxed filtering applied):
  The filtered set is adequate but below optimal.
  Downstream skills should note reduced sample size.
  Pattern frequencies may be less representative — calibrate thresholds downward.
  Must-Have threshold: ≥50% (not ≥60%) due to small sample.

LOW (<5 results even after relaxed filtering, OR majority BORDERLINE):
  The SERP has critically limited editorial competition.
  All pattern-based analysis is PROVISIONAL.
  Downstream skills must flag all blueprint recommendations as PROVISIONAL.
  This is not a failure — it means the editorial competitive landscape is thin,
  which IS itself a strategic signal: the opportunity may be wide open.

DOMINANT CONFIDENCE LEVEL INHERITANCE:
  All downstream skills (deep-serp-analysis, content-pattern-extractor,
  serp-blueprint-generator) inherit the filter confidence level.
  They must calibrate their output confidence accordingly.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: serp/serp-filter.skill*
*Nexus SEO Operating System — Version 1.0.0*
