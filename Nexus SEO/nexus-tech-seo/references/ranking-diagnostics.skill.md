# SKILL: diagnostics/ranking-diagnostics.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 3.0.0
# LAYER: Diagnostics / Technical SEO + Ranking Intelligence
# EXECUTION PRIORITY: MANDATORY — Runs at the end of EVERY content, audit, optimization, and landing page pipeline. Not just "why isn't this ranking" — every pipeline output includes backlink targets and technical improvements.
# POSITION: Universal ranking intelligence. Calculates specific backlink targets with actual numbers. Diagnoses technical issues. Checks AI crawler access. Scores every finding on 3 dimensions. Labels confidence.
# DEPENDENCIES: blog-post-auditor.skill, seo-aeo-geo-sxo-optimizer.skill, brand-[name].skill.md (active brand file)

---

## SECTION 1 — ROLE

This skill serves 2 purposes:

**Purpose 1 — Standalone Diagnosis:** When content scores A+ but doesn't rank, identify the real obstacles — technical SEO, domain authority gaps, backlink deficits, indexation problems, AI crawler access.

**Purpose 2 — Universal Ranking Intelligence:** At the end of every content pipeline, provide specific, quantified ranking targets — how many backlinks needed, from what DA, what technical fixes close the gap. Every article Nexus produces ships with a ranking intelligence report.

### Core Principle
> Be brutally honest. Provide SPECIFIC NUMBERS, not vague advice. "You need approximately 8-12 backlinks from DA 30+ sites" — not "build more backlinks." If you can't rank because DA is 16 and competitors are DA 60+, say so directly. Then provide all fallback strategies with quantified targets.

---

## SECTION 2 — 3-DIMENSION SCORING FRAMEWORK

Every finding is scored on 3 dimensions:

| Dimension | What It Measures | Scale |
|---|---|---|
| **SEO Impact** | How much this issue affects rankings | 1-5 (5 = critical) |
| **Business Impact** | How much this affects conversions/revenue | 1-5 (5 = major revenue impact) |
| **Fix Effort** | How hard and expensive is the fix | 1-5 (5 = months of work) |

**Priority Score = (SEO Impact × 2) + Business Impact - Fix Effort**

Higher priority score = fix this first. This surfaces quick wins (high impact, low effort) and deprioritizes hard-but-low-impact fixes.

### Confidence Labels

Every finding gets a confidence label:

| Label | Meaning | When to Use |
|---|---|---|
| **Confirmed** | Verified with data — PageSpeed result, GSC data, visible in source code | Hard evidence exists |
| **Likely** | Strong signal but not directly verified — DA estimates, backlink counts from web search | Inferred from available data |
| **Hypothesis** | Logical inference without direct data — "competitors likely have better engagement" | No data, but pattern is consistent |

**Never present a Hypothesis as Confirmed.** The user must know what's proven vs. inferred.

---

## SECTION 3 — DIAGNOSTIC FRAMEWORK

### Ranking Factor Checklist (sorted by impact)

| # | Factor | Weight | Category | How to Check |
|---|---|---|---|---|
| 1 | **Indexation** | CRITICAL (blocking) | Technical | `site:{url}` on Google; check GSC |
| 2 | **Search intent mismatch** | 25% | Content | Compare page format vs SERP |
| 3 | **Domain authority gap** | 20% | Authority | Check DA; compare to top 3 |
| 4 | **Backlink deficit** | 18% | Authority | Check backlink count/quality vs competitors |
| 5 | **Content depth/quality gap** | 12% | Content | Run blog-post-auditor; compare depth signals |
| 6 | **Technical SEO (CWV)** | 10% | Technical | PageSpeed Insights LIVE |
| 7 | **AI crawler access** | NEW | Technical | Check robots.txt for 14 AI crawlers |
| 8 | **Internal link equity** | 5% | On-page | Count internal links TO this page |
| 9 | **Content freshness** | 4% | Content | Last modified vs competitors |
| 10 | **User engagement signals** | 3% | Behavioral | CTR from GSC |
| 11 | **Schema markup** | 2% | Technical | JSON-LD types present |
| 12 | **Canonical URL issues** | NEW | Technical | Trailing slashes, www/non-www, query params |
| 13 | **Mobile usability** | 1% | Technical | Mobile score from PageSpeed |

---

## SECTION 4 — EXECUTION STEPS

### Step 1: Indexation Check (BLOCKING)
- Search Google for `site:{exact-url}`
- If NOT indexed → STOP. All other diagnostics are irrelevant.
- Fixes: robots.txt, noindex tags, submit in GSC, canonical tags
- Confidence: **Confirmed** (verifiable)
- SEO Impact: 5 | Business Impact: 5 | Fix Effort: 1-2

### Step 2: Search Intent Analysis
- Search the primary keyword on Google
- Catalog what FORMAT top 5 results use (list, guide, tool, comparison, video, landing page)
- Compare to target page format
- If mismatch → primary ranking obstacle
- Fix: Restructure content to match dominant SERP format
- Confidence: **Confirmed** (SERP visible)

### Step 3: Domain Authority Assessment
- Search for site DA (Moz, Ahrefs, or web search estimates)
- Record DA of top 3 ranking pages
- Calculate gap

| DA Gap | Assessment | Target Action |
|---|---|---|
| 0-10 | Competitive | Content quality can win |
| 11-25 | Challenging | Need 5-10 quality backlinks + superior content |
| 26-40 | Very difficult | Target long-tail first, build DA systematically |
| 41+ | Extremely unlikely short-term | Be honest: cannot win this keyword at current DA |

- Confidence: **Likely** (DA estimates vary by tool)

### Step 4: Backlink Target Calculation (SPECIFIC NUMBERS)
- Fetch backlink data for top 5 SERP competitors (via web search for backlink profiles)
- Record for each competitor: referring domains, page-level backlinks, DA of linking sites
- Calculate averages
- Compare to your current state
- Calculate specific target:

```
BACKLINK TARGET CALCULATION
━━━━━━━━━━━━━━━━━━━━━━━━━━

Top 5 ranking pages for: [keyword]
| Rank | Domain          | DA  | Referring Domains | Page Backlinks |
|------|-----------------|-----|-------------------|----------------|
| 1    | [url]           | [X] | [X]               | [X]            |
| 2    | [url]           | [X] | [X]               | [X]            |
| 3    | [url]           | [X] | [X]               | [X]            |
| 4    | [url]           | [X] | [X]               | [X]            |
| 5    | [url]           | [X] | [X]               | [X]            |

Competitor averages:
- Average DA: [X]
- Average referring domains to page: [X]
- Average page-level backlinks: [X]

Your current state:
- Your DA: [X]
- Your referring domains: [X]
- Your page backlinks: [X]

═══════════════════════════════
TARGET TO REACH PAGE 1:
═══════════════════════════════
- Backlinks needed: [X] from DA [Y]+ sites
- Referring domains needed: [X] unique domains
- DA target: [current] → [needed] (gap: [X] points)
- Estimated difficulty: [EASY / MODERATE / HARD / VERY HARD]

Confidence: [Confirmed/Likely/Hypothesis]
Based on: [data sources used]
━━━━━━━━━━━━━━━━━━━━━━━━━━
```

- NOT DONE UNTIL: Specific numbers are provided. "Build more backlinks" is NEVER acceptable.
- Confidence: **Likely** (backlink data varies by tool, but directionally accurate)

### Step 5: PageSpeed Insights (LIVE CHECK)
Fetch PageSpeed Insights for target URL:
```
URL: https://pagespeed.web.dev/analysis?url={encoded_url}
```
Record:
- Performance score (mobile + desktop)
- Largest Contentful Paint (LCP) — target: under 2.5s
- Interaction to Next Paint (INP) — target: under 200ms (replaced FID March 2024)
- Cumulative Layout Shift (CLS) — target: under 0.1
- First Contentful Paint (FCP)
- Total Blocking Time (TBT)

**Measure at 75th percentile, not average.** Google uses 75th percentile for CWV assessment.

- Confidence: **Confirmed** (live measurement)

### Step 6: AI Crawler Access Check (NEW in v3.0)
- Fetch the site's robots.txt
- Check for blocks on these 14 AI crawlers:
  - GPTBot (OpenAI)
  - OAI-SearchBot (OpenAI Search)
  - ChatGPT-User (ChatGPT browsing)
  - ClaudeBot (Anthropic)
  - PerplexityBot (Perplexity)
  - Amazonbot (Amazon)
  - Google-Extended (Google AI training)
  - Bytespider (TikTok/ByteDance)
  - CCBot (Common Crawl)
  - FacebookBot (Meta)
  - Applebot-Extended (Apple AI)
  - cohere-ai (Cohere)
  - Diffbot (Diffbot)
  - ImagesiftBot (Imagesift)

- If ANY critical crawlers are blocked (GPTBot, ClaudeBot, PerplexityBot, OAI-SearchBot, Google-Extended):
  → Flag: "AI crawlers blocked. Your GEO score should be 0 regardless of content quality. These crawlers cannot index your content for AI search results."
  → SEO Impact: 3 | Business Impact: 4 | Fix Effort: 1
- If all allowed → CLEAR

- Check for llms.txt presence at `/llms.txt` — nice-to-have but low impact
- Confidence: **Confirmed** (robots.txt is directly readable)

### Step 7: Canonical URL Normalization Check (NEW in v3.0)
Check for these common canonical issues:
- **Trailing slash consistency:** Does `site.com/page` return the same as `site.com/page/`? Or do they 200 separately?
- **www/non-www:** Is `www.site.com` properly redirecting to `site.com` (or vice versa)?
- **Protocol:** Is `http://` properly redirecting to `https://`?
- **Query parameter pollution:** Are URLs like `site.com/page?ref=twitter` creating duplicate indexable pages?
- **Canonical tag present:** Does the page have `<link rel="canonical">` pointing to the correct URL?

Each issue found:
- SEO Impact: 2-3 | Business Impact: 1 | Fix Effort: 1-2
- Confidence: **Confirmed** (testable)

### Step 8: Internal Link Equity Check
- Count internal pages linking TO this page
- Is page linked from homepage? Pillar pages? Or only low-traffic pages?
- Compare to competitors' linking patterns
- Confidence: **Likely** (depends on crawl data availability)

### Step 9: Content Freshness Check
- Record last modified date
- Compare to top 3 competitors
- If competitors updated in last 3 months and this page 6+ months stale → flag
- Confidence: **Confirmed** (dates visible)

### Step 10: Schema Markup Audit
- Check current JSON-LD types on page
- Compare to what should be present based on content type:
  - Blog → Article + FAQPage
  - Tutorial → HowTo + Article
  - Comparison → WebPage + ItemList
  - Tool/product → SoftwareApplication
  - Service → Service + Organization
- Missing schema types → flag with specific JSON-LD to add
- Confidence: **Confirmed** (source code visible)

---

## SECTION 5 — OUTPUT: RANKING DIAGNOSTIC REPORT

```
═══════════════════════════════════════
RANKING DIAGNOSTIC REPORT
Brand: [brand.brand_name]
URL: [page URL]
Keyword: [target keyword]
Current Position: [if known]
Date: [date]
═══════════════════════════════════════

## BLOCKING ISSUES
[Issues that prevent ranking entirely]
- Indexation: [INDEXED / NOT INDEXED] — Confidence: Confirmed
  Fix: [specific fix]

## DIAGNOSTIC RESULTS (3-Dimension Scoring)

| # | Factor | Score | SEO Impact | Biz Impact | Fix Effort | Priority | Confidence |
|---|---|---|---|---|---|---|---|
| 1 | Indexation | [PASS/FAIL] | 5 | 5 | [X] | — | Confirmed |
| 2 | Intent match | [0-100] | [X] | [X] | [X] | [X] | Confirmed |
| 3 | Domain authority | DA [X] vs [Y] | [X] | [X] | [X] | [X] | Likely |
| 4 | Backlinks | [X] vs [Y] | [X] | [X] | [X] | [X] | Likely |
| 5 | Content depth | [score] | [X] | [X] | [X] | [X] | Confirmed |
| 6 | Core Web Vitals | [PSI score] | [X] | [X] | [X] | [X] | Confirmed |
| 7 | AI crawler access | [X/14 allowed] | [X] | [X] | [X] | [X] | Confirmed |
| 8 | Internal links | [count] | [X] | [X] | [X] | [X] | Likely |
| 9 | Freshness | [date] | [X] | [X] | [X] | [X] | Confirmed |
| 10 | Schema | [types] | [X] | [X] | [X] | [X] | Confirmed |
| 11 | Canonical URLs | [issues] | [X] | [X] | [X] | [X] | Confirmed |
| 12 | Mobile | [score] | [X] | [X] | [X] | [X] | Confirmed |

## BACKLINK TARGET (SPECIFIC NUMBERS)

[Full backlink target calculation table from Step 4]

## TECHNICAL IMPROVEMENT LIST

Priority-sorted by (SEO Impact × 2) + Business Impact - Fix Effort:

1. [Fix]: SEO [X/5] | Biz [X/5] | Effort [X/5] | Priority: [X] — Confidence: [label]
2. [Fix]: ...
3. ...

## HONEST ASSESSMENT

[Brutally honest paragraph about keyword winnability and what it takes]

## STRATEGY OPTIONS

### Option 1: Quick Technical Wins (fix this week)
[Technical fixes with expected impact, sorted by priority score]

### Option 2: Backlink Building (1-3 months)
[Specific backlink targets with numbers]
- Target: [X] quality backlinks from DA [Y]+ sites
- Focus: [specific link building approaches]

### Option 3: Easier Keywords (pivot if needed)
[Alternative keywords with lower competition]
- "[keyword 1]" — Volume: [X], DA gap: [Y], estimated difficulty: [Z]
- "[keyword 2]" — ...
- "[keyword 3]" — ...

═══════════════════════════════════════
```

---

## SECTION 6 — KEYWORD WINNABILITY MATRIX

| Your DA | Competitor DA | Content Quality | Backlinks | Technical | Verdict |
|---|---|---|---|---|---|
| 16 | 60+ | A+ | 0 | Clean | Cannot win short-term. Build DA + 15-20 backlinks for 6-12 months OR target long-tail |
| 16 | 30-40 | A+ | 0 | Clean | Difficult but possible. Superior depth + 5-10 quality backlinks may close gap |
| 16 | 10-20 | A+ | 0 | Clean | Winnable. Content depth + technical fixes sufficient |
| 16 | 60+ | A+ | 10+ quality | Clean | Long shot for #1. May reach page 1 position 5-8 |
| Any | Any | Any | Any | Blocked crawlers | Fix crawlers FIRST. No ranking in AI search without crawler access |
| Any | Any | Any | Any | Not indexed | Fix indexation FIRST. Everything else is irrelevant |

---

## SECTION 7 — WHEN THIS SKILL RUNS

| Context | Trigger | Mode |
|---|---|---|
| User asks "why isn't this ranking" | Standalone diagnostic | Full report |
| End of content creation pipeline | Mandatory Phase 5 step | Backlink targets + technical improvements |
| End of landing page pipeline | Mandatory Phase 5 step | Backlink targets + technical improvements |
| End of optimization pipeline | Mandatory step | Backlink targets |
| End of audit pipeline | Mandatory step | Backlink targets + technical improvements |
| User asks "how many backlinks" | Single skill | Backlink target calculation only |

**This skill is NEVER optional.** Every content output includes ranking intelligence.

---

*Ranking Diagnostics — v3.0.0 | Nexus SEO Operating System | Universal Ranking Intelligence*
