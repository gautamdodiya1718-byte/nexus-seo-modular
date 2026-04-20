# gsc-insights-extractor.skill

**Role:** Parse Google Search Console data exports (CSV or JSON) and extract actionable insights: high-impression-low-CTR queries, position decline pages, intent mismatches, queries you rank for that the page doesn't directly target, pages with high impressions but no clicks.

**Loaded by:** `nexus-tech-seo` SKILL.md in modes: full_audit, gsc_insights_only, content_tech_bottleneck

**Note:** This skill is the parser/analyzer. The legacy `gsc-integration.skill.md` (also in references/) is preserved for reference but this skill replaces its analytical role.

---

## Inputs

Accept any of:
- GSC Performance report — Queries CSV (columns: Query, Clicks, Impressions, CTR, Position)
- GSC Performance report — Pages CSV (columns: Page, Clicks, Impressions, CTR, Position)
- GSC Performance report — Queries × Pages CSV (joined export)
- GSC API JSON output
- Coverage report CSV (URL, Coverage status, Last crawl)
- Pasted GSC data table

If multiple reports: cross-reference them. The most powerful insights come from joining queries with pages.

---

## What to extract and analyze

### High-impression / low-CTR queries (title + meta optimization opportunity)

For each query in the queries report:
- Calculate expected CTR for its position (rough benchmarks: pos 1 ≈ 27%, pos 2 ≈ 15%, pos 3 ≈ 11%, pos 4-5 ≈ 8%, pos 6-10 ≈ 4%, pos 11-20 ≈ 1.5%, pos 21+ ≈ <1%)
- If actual CTR is 50% or more below expected for that position, flag as "low CTR opportunity"
- Sort by impressions × CTR gap (biggest opportunity first)

Output: ranked list of queries where rewriting title/meta could capture significantly more clicks at current ranking.

### Position decline detection

If user provided two reports (e.g., last 28 days vs prior 28 days, or month-over-month):
- Compare position per query/page across the two periods
- Flag any page that declined by 3+ positions for a previously-ranking query
- Severity: dropped from page 1 to page 2 = critical; page 2 to page 3 = high; smaller drops = medium

If only one report, skip this analysis (mark partial).

### Intent mismatch detection

For each page in the pages report, look at the queries it ranks for:
- If a page ranks for queries that don't match its declared topic (you provide topic per page, OR we infer from URL slug + H1), flag as intent mismatch
- Common pattern: blog about "how to do X" ranks for "best X tools" — different intent — page won't satisfy clicks effectively

Output: pages with significant intent drift, recommendation to either (a) optimize content for the queries it actually ranks for, or (b) create a new page targeting the original intent.

### High-impression / no-clicks pages (snippet won't convert)

For each page:
- If impressions > 1000 in 90 days but clicks < 5: flag as critical relevance issue
- Likely causes: title/meta clearly off-topic for what users see, or competitive snippet from another result outshines yours

### Query categorization

Group queries into:
- **Branded** (contains brand name) — should have very high CTR; flag low CTR on branded as critical
- **Informational** (how, what, why, guide, tutorial, etc.) — TOFU intent
- **Commercial** (best, vs, alternatives, comparison, review) — MOFU intent
- **Transactional** (buy, pricing, cost, demo, trial, signup, download) — BOFU intent

Note distribution per page: a page with 80% informational queries is a TOFU page; flag if it's structured as BOFU (and vice versa).

### Coverage report cross-reference

If coverage report provided:
- Pages marked "Discovered but not indexed" — list with severity HIGH (high crawl budget waste, no traffic)
- Pages marked "Crawled but not indexed" — list with severity HIGH (Google saw them but rejected)
- Pages with errors — list with severity CRITICAL
- Cross-check: any pages in source code that don't appear in coverage report at all (maybe sitemap-missing or robots-blocked)

### Cross-report joins (most powerful insights)

If queries × pages report provided:

1. **Query cannibalization** — same query has 2+ URLs ranking. (See cannibalization-detector for full analysis.)

2. **Page underperformance** — page ranks for many queries but gets few total clicks (suggests bad title/meta or weak preview snippet).

3. **Query underutilization** — query has high impressions across multiple pages, but the best-positioned page is a tangential one. Recommendation: optimize the better-fit page for that query, or merge tangential pages.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: gsc-insights-extractor
Status: COMPLETE / PARTIAL ([reason])
Inputs consumed: [list of GSC reports provided]

GSC INSIGHTS BY SEVERITY:

CRITICAL:
  Pages with 1000+ impressions and <5 clicks (relevance broken):
    1. [URL] — [N] impressions, [N] clicks, [CTR%], avg position [N]
       Top queries: [list 3]
       Likely issue: [diagnosed cause]
       Fix: [specific action]

  Coverage errors: [count + sample list]

HIGH:
  Low-CTR queries at strong positions (title/meta opportunity):
    1. Query "[X]" — position [N], expected CTR [Y%], actual CTR [Z%]
       Page: [URL]
       Fix: [specific title/meta rewrite suggestion]

  Position decline pages (if 2 periods compared):
    1. [URL] dropped from position [X] to position [Y] for "[query]"
       Possible cause: [SERP change / competitor move / content decay]
       Recommended action: [audit + refresh]

  Intent mismatches:
    1. Page [URL] ranks primarily for [query types] but is structured as [page type]
       Recommendation: [optimize for actual intent OR create separate page for declared intent]

MEDIUM:
  High-impression queries the page doesn't directly target (intent gap)
  Branded queries with below-expected CTR (snippet issue)

QUERY DISTRIBUTION:
  Informational: [%] | Commercial: [%] | Transactional: [%] | Branded: [%]

TOP 20 OPPORTUNITY QUERIES (by impression × CTR gap):
  1. "[query]" — [N] impressions, position [N], current CTR [Z%], potential clicks [N]
  ...

CONFIDENCE: HIGH (full reports + 2 periods) / MEDIUM (1 period) / LOW (partial data)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Don't pull from GSC API directly.** This skill only analyzes user-provided exports.
2. **Two periods are powerful.** Recommend user export 2 time windows for trend analysis. With 1 period, position-decline detection is skipped.
3. **Specific to the data.** Reference actual queries and URLs from the user's data — never generic examples.
4. **Numeric thresholds matter.** A page with 50 impressions and 0 clicks isn't a critical relevance issue (sample too small). 1000+ impressions and <5 clicks is.
5. **CTR benchmarks are rough.** State this; don't claim them as exact. The point is direction not precision.
