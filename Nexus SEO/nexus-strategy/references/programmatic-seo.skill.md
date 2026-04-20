# SKILL: strategy/programmatic-seo.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Strategy / Scaled Content Generation
# EXECUTION PRIORITY: ON-DEMAND — Triggered by menu item 10 or keywords like "programmatic SEO," "generate pages at scale," "template pages," "bulk landing pages"
# POSITION: The scale engine. Takes a pattern + data and produces hundreds of optimized pages from a single template. Not content spinning — structured, unique, valuable pages targeting long-tail keywords at volumes impossible to create manually.
# DEPENDENCIES: keyword-discovery.skill, serp-intelligence.skill, metadata-generator.skill, landing-page-engine.skill, ranking-diagnostics.skill, internal-linking.skill

---

## SECTION 1 — ROLE

Programmatic SEO creates large volumes of pages from a single template populated with structured data. Each page targets a specific long-tail keyword with enough unique value to satisfy Google's quality standards.

### Real-World Examples at Scale
- **Zapier:** 25,000+ integration pages ("Connect Slack to Trello") → 16M+ monthly organic visitors
- **Wise:** 8.5M currency converter pages → owns the long-tail of currency search
- **Canva:** Template pages for every design type → 100M+ monthly visitors
- **TripAdvisor:** "Things to Do in [City]" for thousands of locations
- **Example of this pattern in action:** "[Brand] + [Integration]" pages across multiple tools, "[Brand] vs [Competitor]" comparison pages — read brand.content_pillars and brand.competitors to identify your specific patterns

### When to Use Programmatic SEO
- You have a repeating pattern: [Variable A] + [Constant Template] (e.g., "[brand.brand_name] vs {competitor}" — substitute brand.brand_name from active brand file)
- You have structured data that can populate the variable slots
- The target keywords are long-tail with real search intent
- You could NOT reasonably write each page manually (10+ pages minimum)
- Each page can provide genuine unique value beyond variable substitution

### When NOT to Use
- The pages would be thin (just swapping 1 word with no real content difference)
- There's no real search intent for the variations
- You have fewer than 10 variations (just write them individually)
- The data to populate pages doesn't exist or isn't verifiable

---

## SECTION 2 — THE PROGRAMMATIC SEO PROCESS

### Step 1 — Pattern Discovery

Identify the template pattern. Every programmatic SEO play follows this formula:

```
[Head Term] + [Modifier] = Keyword Variation
```

| Pattern Type | Head Term | Modifier | Example Pages |
|---|---|---|---|
| **Versus/Comparison** | "[Brand] vs" | {competitor} | [brand_name] vs [Competitor 1], [brand_name] vs [Competitor 2] |
| **Alternatives** | "{tool} alternatives" | {tool name} | [Competitor 1] alternatives, [Competitor 2] alternatives |
| **Feature × Use Case** | "{feature} for" | {use case} | [Feature 1] for [use case 1], [Feature 2] for [use case 2] |
| **Tool × Integration** | "[Product] +" | {integration tool} | [brand_name] + [Integration 1], [brand_name] + [Integration 2] |
| **Location × Service** | "{service} in" | {location} | [service] in [location 1], [service] in [location 2] |
| **Template × Type** | "{content type}" | {template variant} | [topic] config for [variant 1], [topic] config for [variant 2] |

**Pattern sources:** Read brand.competitors for versus patterns, brand.features for feature patterns, brand.content_pillars for topic patterns, brand.urls.integrations for integration patterns.

**Output from this step:**
```
PATTERN IDENTIFIED:
  Template: "[Brand] vs {competitor}"
  Head term: "[brand.brand_name] vs"
  Modifier source: competitor list (brand.competitors)
  Total variations: [N] pages
  Search volume per variation: [estimated range]
```

### Step 2 — Keyword Matrix Generation

Generate ALL valid keyword combinations from the pattern:

```
KEYWORD MATRIX
━━━━━━━━━━━━━━

Pattern: [brand_name] vs {competitor}
Head term: read from brand.competitors list (competitor.name fields)
Variable: each competitor in brand.competitors generates one page

| # | Page Title | Vol | Difficulty | Intent | Priority |
|---|---|---|---|---|---|
| 1 | [brand_name] vs [Competitor 1] | [vol] | [diff] | Commercial | HIGH |
| 2 | [brand_name] vs [Competitor 2] | [vol] | [diff] | Commercial | HIGH |
| 3 | [brand_name] alternatives | [vol] | [diff] | Commercial | HIGH |

For integration patterns, read brand.content_pillars and brand.features:
  Pattern: [Feature name] + {integration}
  Generate: one page per integration in brand.urls.integrations

TOTAL PAGES: [N]
TOTAL ADDRESSABLE VOLUME: [sum of all volumes]
```

**Use `keyword-discovery.skill.md`** to find additional modifiers you might not have considered.
**Use `google-signals-extractor.skill.md`** to find PAA questions for the pattern.

### Step 3 — Template Design

Design the page template. This is the STRUCTURE that stays the same across all pages — only the DATA changes.

**Template rules:**
- Same section structure across all pages
- Same section order across all pages
- Variable content per section (different data fills each section)
- Minimum 30% unique content per page (Google's quality threshold)
- Each page must standalone as valuable even if the reader doesn't visit other pages

**Example template for "[brand.brand_name] vs {Competitor}":**

```
TEMPLATE STRUCTURE:
━━━━━━━━━━━━━━━━━━

[SECTION 1: HERO]
  H1: "[brand.brand_name] vs {competitor_name}: {year} Comparison"
  Quick verdict: 2-3 sentences (UNIQUE per page)

[SECTION 2: COMPARISON TABLE]
  Side-by-side feature matrix
  Data source: {competitor_features_data}
  Columns: Feature | [brand.brand_name] | {competitor_name}

[SECTION 3: KEY DIFFERENTIATORS]
  3-5 areas where [brand.brand_name] differs — read brand.differentiators
  Content: UNIQUE per page — based on actual competitor analysis
  Each differentiator: what [brand.brand_name] does vs what {competitor} does — read brand.differentiators

[SECTION 4: HONEST ACKNOWLEDGMENT]
  Where {competitor_name} does well
  Content: UNIQUE per page — genuine strengths of the competitor

[SECTION 5: USE CASE MATCHING]
  "Choose [brand.brand_name] if..." / "Choose {competitor} if..."
  Content: UNIQUE per page — honest use case fit

[SECTION 6: MIGRATION GUIDE]
  "Switching from {competitor_name} to [brand.brand_name]"
  Content: Semi-unique — varies by competitor's export/API

[SECTION 7: FAQ]
  5 questions specific to this comparison
  Content: UNIQUE per page — real questions people ask

[SECTION 8: CTA]
  Standard CTA with UTM: ?utm_source=blog&utm_medium=comparison&utm_campaign=vs-{competitor_slug}
```

### Step 4 — Data Collection

For each variable in the template, collect the data:

```
DATA REQUIREMENTS:
━━━━━━━━━━━━━━━━━

Variable: {competitor_name}
Source: Manual list (verified competitors)
Count: [N] values

Variable: {competitor_features_data}
Source: Web research per competitor (features, pricing, limitations)
Collection method: Fetch competitor website, extract feature list
Quality check: Verify each data point — no fabricated features

Variable: {year}
Source: Current year
Auto-populated: YES

Variable: {competitor_slug}
Source: Derived from competitor_name (lowercase, hyphens)
Auto-populated: YES
```

**Critical rule: Data quality = page quality.** If your comparison data is wrong or outdated, the page is worse than useless. Every data point must be verified against the competitor's actual current state.

### Step 5 — Sample Page Generation

Before scaling, generate 3-5 sample pages and validate:

```
SAMPLE PAGES GENERATED:
━━━━━━━━━━━━━━━━━━━━━━

Page 1: "[brand.brand_name] vs [brand.competitors[0].name]"
  Unique content: [X]% (target: 30%+ between pages)
  Word count: [N]
  Sections completed: [all / partial]
  Quality check: [PASS / NEEDS WORK]

Page 2: "[brand.brand_name] vs [brand.competitors[1].name]"
  Unique content: [X]% (vs page 1)
  [...]

Page 3: "[brand.brand_name] vs [brand.competitors[2].name]"
  [...]

DIFFERENTIATION SCORE: [X]% average uniqueness across samples
MINIMUM THRESHOLD: 30% — if below, template needs more unique sections
```

**If differentiation is below 30%:** Add more unique sections, expand the honest acknowledgment section (it's naturally unique per competitor), add more specific FAQ questions, or include unique data tables per competitor.

### Step 6 — Quality Gates (CRITICAL)

Every programmatic page must pass these gates BEFORE publish:

| Gate | Check | Threshold | Action if Fails |
|---|---|---|---|
| **Uniqueness** | % of content unique vs other pages in the set | 30% minimum | Add more unique sections or data |
| **Thin content** | Total unique, valuable words per page | 500 words minimum unique content | Expand sections with real data |
| **Real data** | All comparison data verified against live sources | 100% verified | Re-check competitor features/pricing |
| **No cannibalization** | No 2 pages in the set target the same primary keyword | 0 overlaps | Merge or differentiate keywords |
| **Search intent match** | Page format matches what SERP shows for this keyword | Format matches SERP | Restructure to match expected format |
| **Schema present** | Correct JSON-LD for page type | Schema validated | Add missing schema |
| **Internal links** | Connected to pillar/hub page and 2-3 related pages | 3+ internal links | Add links |
| **CTA present** | Conversion path exists with UTM tracking | CTA + UTM present | Add CTA |
| **Mobile readable** | Short paragraphs, responsive tables, visible CTA | Mobile-friendly | Fix formatting |

### Step 7 — Internal Linking Plan

Programmatic pages need a hub-and-spoke linking structure:

```
INTERNAL LINKING PLAN
━━━━━━━━━━━━━━━━━━━━

Hub page: /comparisons/ (or /alternatives/ or /integrations/)
  Links to: ALL programmatic pages in this set

Each programmatic page:
  → Hub page (mandatory)
  → 2-3 related pages from the same set (e.g., vs-currents links to vs-reportportal)
  → 1-2 blog posts on related topics (from brand link repository)
  → Pillar page if part of a semantic cluster

Cross-set linking (if multiple programmatic sets exist):
  vs-pages → alternatives-pages (where relevant)
  integration-pages → feature-pages (where relevant)
```

### Step 8 — Staged Rollout

**NEVER publish all pages at once.** Google flags sudden bulk page additions.

```
ROLLOUT PLAN
━━━━━━━━━━━━

Total pages: [N]
Rollout cadence: [X] pages per week

Week 1: [3-5 highest priority pages]
  Monitor: indexation, crawl stats, rankings
  If OK → continue
  If issues → pause, diagnose, fix

Week 2: [next 3-5 pages]
  Monitor: same metrics + cannibalization check
  
Week 3-N: [continue at same cadence]

Post-rollout (ongoing):
  - Weekly: check indexation rate in GSC
  - Monthly: audit for thin/duplicate content
  - Quarterly: refresh data (competitor features change)
  - Prune: remove pages that get 0 traffic after 3 months
```

---

## SECTION 3 — WORKED EXAMPLE (FILL WITH YOUR BRAND)

This section shows how to apply the framework above to a specific brand.
Replace every [brand.brand_name] reference with your actual brand name from brand-[name].skill.md.

### Pattern: [brand.brand_name] vs {Competitor}
Source: read brand.competitors list — each competitor generates one page.

| # | Page Title | Source Field |
|---|---|---|
| 1 | [brand.brand_name] vs [brand.competitors[0].name] | brand.competitors[0] |
| 2 | [brand.brand_name] vs [brand.competitors[1].name] | brand.competitors[1] |
| 3 | [brand.brand_name] vs [brand.competitors[2].name] | brand.competitors[2] |

### Pattern: [brand.brand_name] + {Integration}
Source: read brand.urls.integrations — each integration page generates one entry.

| # | Page Title | Source Field |
|---|---|---|
| 1 | [brand.brand_name] + [integration_name] | brand.urls.integrations[0] |
| 2 | [brand.brand_name] + [integration_name] | brand.urls.integrations[1] |

### Pattern: [Feature] — [brand.brand_name]
Source: read brand.features — each STABLE feature generates one page.

| # | Page Title | Maturity Gate |
|---|---|---|
| 1 | [feature.name] — [brand.brand_name] | STABLE only |
| 2 | [feature.name] — [brand.brand_name] | STABLE only |

## SECTION 4 — MULTI-BRAND SETUP

If you have more than one brand registered (multiple brand-[name].skill.md files in references/):
- Run this skill once per brand
- Each brand's keyword matrix is built from its own brand file
- ZERO cross-brand page generation — never mix brand.urls across brands
- Confirm active brand at session start before generating any template

## SECTION 5 — METADATA AND SCHEMA AT SCALE

### Metadata Template
Each programmatic page gets metadata generated from the template:

```
Title: "{Head Term} {Modifier}: {Year} Comparison" (55-60 chars)
Meta: "Compare {Brand} and {Modifier}. Side-by-side features, pricing, and honest verdict. See which is right for your team." (150-160 chars)
Slug: /{category}/{brand}-vs-{modifier-slug}/
```

### Schema per Page Type
Use `metadata-generator.skill.md` auto-selection:
- Comparison pages → `WebPage` + `FAQPage`
- Alternatives pages → `WebPage` + `ItemList` + `FAQPage`
- Integration pages → `SoftwareApplication` + `WebPage`
- Location pages → `Service` + `LocalBusiness`
- Feature pages → `SoftwareApplication` + `WebPage`

### Sitemap Management
- All programmatic pages get their own sitemap segment: `/sitemap-comparisons.xml`
- Do NOT mix programmatic pages with editorial blog sitemap
- Update sitemap with each batch rollout

---

## SECTION 6 — CANNIBALIZATION PREVENTION

When generating pages at scale, cannibalization is the biggest risk.

### Pre-Generation Checks
1. Run `deduplication-engine.skill.md` across ALL keyword variations
2. Verify no 2 pages target the same primary keyword
3. Check existing blog posts — does any existing blog already target this keyword?
4. Check across programmatic sets — does a vs-page cannibalize an alternatives-page for the same competitor?

### Post-Generation Checks
1. After rollout, monitor GSC for pages competing against each other
2. If 2 pages from the same set rank for the same keyword → merge the weaker one or differentiate it further
3. Use canonical tags if variations are intentionally similar (e.g., different currency amounts)

---

## SECTION 7 — FULL OUTPUT FORMAT

```
═══════════════════════════════════════
PROGRAMMATIC SEO PLAN
Brand: [brand.brand_name from active brand file]
Pattern: [head term + modifier]
Date: [date]
═══════════════════════════════════════

1. KEYWORD MATRIX
   [Full table of all keyword variations with volume, difficulty, priority]
   Total pages: [N]
   Total addressable volume: [N]

2. PAGE TEMPLATE
   [Section-by-section template structure]
   [Variable slots identified]
   [Minimum uniqueness rules per section]

3. DATA REQUIREMENTS
   [What data is needed per variable]
   [Collection method per variable]
   [Verification requirements]

4. SAMPLE PAGES (3-5)
   [Full generated samples]
   Differentiation score: [X]%

5. QUALITY GATES
   [All gates with pass/fail per sample]

6. INTERNAL LINKING PLAN
   [Hub page + cross-linking architecture]

7. METADATA + SCHEMA TEMPLATE
   [Title/meta/slug patterns + schema per page type]

8. ROLLOUT PLAN
   [Week-by-week staged deployment]

9. CANNIBALIZATION CHECK
   [Results of deduplication across keyword matrix + existing content]

═══════════════════════════════════════
NEXT STEPS:
- Approve template and keyword matrix
- Collect/verify data for all variations
- Generate first batch (3-5 pages)
- Review, refine, then scale
═══════════════════════════════════════
```

---

## SECTION 8 — INTEGRATION WITH OTHER NEXUS SKILLS

| Combined With | How It Integrates |
|---|---|
| **Menu 1 (Write blog)** | Each programmatic page can be generated using the content pipeline with the template as the brief |
| **Menu 2 (Landing page)** | Programmatic pages ARE landing pages — `landing-page-engine.skill.md` provides CRO scoring |
| **Menu 3 (Keyword research)** | Keyword discovery feeds the modifier list for the keyword matrix |
| **Menu 5 (Ranking diagnosis)** | Run ranking diagnostics per page to get individual backlink targets |
| **Menu 9 (Semantic SEO)** | Programmatic sets can serve as cluster pages within a topic cluster |
| **`ranking-diagnostics`** | After rollout, diagnose which pages aren't ranking and why |

---

*Programmatic SEO — v1.0.0 | Nexus SEO Operating System | Scale Without Sacrifice*
