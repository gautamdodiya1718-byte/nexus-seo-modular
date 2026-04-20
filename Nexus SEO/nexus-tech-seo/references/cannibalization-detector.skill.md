# cannibalization-detector.skill

**Role:** Detect site-level keyword cannibalization — when 2+ URLs from the same site rank for the same query — and recommend consolidation or differentiation per detected case.

**Loaded by:** `nexus-tech-seo` SKILL.md in modes: full_audit, gsc_insights_only, cannibalization_only

---

## Inputs

Required (one of):
- GSC Pages × Queries joined export (CSV) — best for cannibalization analysis
- GSC Pages report (CSV) + GSC Queries report (CSV) — second best (need to join manually)
- GSC API JSON with both query and page dimensions

Optional:
- Source code parser output (for cross-checking page intent and avoiding false positives where 2 URLs are intentionally separate)

If user only provides one report (queries-only or pages-only), inform them: "Cannibalization detection requires the page+query join. Export the GSC Performance report with both Page and Query dimensions selected."

---

## Detection logic

### Step 1 — Group queries by URLs ranking

For each query in the data:
- List all URLs from the user's site that rank for that query
- Filter: only count URLs ranking in top 50 positions (deeper rankings are noise)

### Step 2 — Flag candidate cannibalization cases

A cannibalization case is flagged when:
- 2+ URLs from the same site rank for the same query in top 50
- AND the URLs are not in a clear hierarchy (e.g., a category page + its child product page is fine; two equivalent blog posts both ranking is not)

### Step 3 — Severity classification

For each candidate case:

**CRITICAL severity:**
- 2+ URLs both ranking in top 10 for the same query (both visible to users, splitting clicks)
- Combined impressions > 500 in 90 days

**HIGH severity:**
- 2+ URLs ranking in top 30 with at least one in top 10
- Combined impressions > 200

**MEDIUM severity:**
- 2+ URLs ranking in top 50 with the lower-ranking URL above position 30
- Combined impressions > 50

**LOW severity / not flagged:**
- One URL in top 50 plus another URL in 50-100 — usually not a real cannibalization issue

### Step 4 — Diagnose intent overlap

For each flagged case, determine if the URLs are:

**A) True cannibalization** (same intent, same audience, same answer)
- Recommendation: **CONSOLIDATE** — merge into one stronger page, 301 redirect the weaker one
- Pick the URL to keep based on: higher position currently, more backlinks (if known), more comprehensive content (if source available), older URL (more authority)

**B) Adjacent intent overlap** (related but distinct angles)
- Recommendation: **DIFFERENTIATE** — rewrite one to cover a clearly different angle / sub-intent
- Provide specific differentiation suggestion (one URL keeps original intent, other targets a more specific sub-query)

**C) False positive** (legitimately different pages that share a keyword)
- Common cases: brand homepage + product page both ranking for brand name; category page + featured product page; pillar page + cluster page
- Recommendation: **NO ACTION** — document why this isn't real cannibalization

If source code parser output is available, use H1 + title + meta description from each URL to assess intent overlap automatically. Otherwise, surface the case and ask the user to assess intent.

### Step 5 — Recommendation per case

For consolidate cases:
```
URLs: [A] (position X), [B] (position Y)
Query: "[Q]"
Combined monthly impressions: [N]
Recommendation: CONSOLIDATE
  Keep: [URL with higher position OR stronger backlinks OR more comprehensive content]
  Remove: [other URL] — 301 redirect to kept URL
  Migration steps:
    1. Identify unique content on [removed URL] worth preserving — merge into [kept URL]
    2. Implement 301 redirect from [removed URL] to [kept URL]
    3. Update internal links pointing to [removed URL]
    4. Submit updated sitemap to GSC
    5. Monitor position of kept URL for 4-6 weeks
  Expected outcome: kept URL consolidates ranking signals; total impressions for query likely increase
```

For differentiate cases:
```
URLs: [A] (position X), [B] (position Y)
Query: "[Q]"
Recommendation: DIFFERENTIATE
  Keep [A] as primary target for "[Q]" — current intent: [intent description]
  Reposition [B] to target sub-query: "[suggested sub-query]"
    Specific changes to [B]:
      - Update title to: "[suggested title]"
      - Update H1 to: "[suggested H1]"
      - Rewrite intro to focus on [sub-intent]
      - Add internal link from [B] to [A] (not vice versa) signaling [A] is the canonical for "[Q]"
  Expected outcome: [A] strengthens for primary query, [B] captures additional traffic from sub-query
```

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: cannibalization-detector
Status: COMPLETE
Inputs consumed: GSC report(s)

CANNIBALIZATION SUMMARY:
  Total queries analyzed: [N]
  Cannibalization cases detected: [N]
    CRITICAL: [N]
    HIGH: [N]
    MEDIUM: [N]

CRITICAL CASES (2+ URLs in top 10 for same query):
  1. Query "[Q]" — [N] impressions/90d
     URL A: [path] — position [X], CTR [Y%]
     URL B: [path] — position [X], CTR [Y%]
     Diagnosed intent: [TRUE / ADJACENT / FALSE_POSITIVE]
     Recommendation: [CONSOLIDATE / DIFFERENTIATE / NO_ACTION]
     Migration plan: [steps]

  2. ...

HIGH CASES:
  1. ...

MEDIUM CASES:
  1. ...

CONSOLIDATION CANDIDATES SUMMARY:
  Total URLs recommended for 301 redirect: [N]
  Total monthly impressions affected: [N]
  Estimated traffic impact (post-consolidation): [N% increase on consolidated URLs]

DIFFERENTIATION CANDIDATES SUMMARY:
  Total URLs to reposition: [N]
  Sub-queries identified for repositioning: [list]

NO-ACTION CASES (false positives):
  [List with reasons — useful so user doesn't worry about them]

CONFIDENCE: HIGH (page+query join provided) / MEDIUM (separate reports joined manually) / LOW (single report)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Be conservative on false positives.** Two pages ranking for "TestDino" (homepage + product page) is NOT cannibalization. Pillar + cluster ranking together is NOT cannibalization. Brand-related and hierarchical relationships are usually intentional.

2. **Specific migration plans.** "Consolidate" is not enough. Show: which URL to keep, why, the redirect to set up, internal links to update, sitemap update step, monitoring period.

3. **Differentiation suggestions must be concrete.** Don't say "make them different." Suggest the actual sub-query the second URL should target and the specific title/H1/intro changes.

4. **Cross-reference with source code if available.** If page intents are clear from H1/title/meta, classify automatically. Otherwise, surface the case and let user confirm.

5. **One pass.** Don't re-flag the same URL pair multiple times if they share several queries — group by URL pair, list all shared queries together.
