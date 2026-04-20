---
name: ranking-status-check
description: >-
  Checks the current Google ranking position of a given URL for its target keyword.
  Sets optimization mode to PROTECT (pos 1-3), SHIELD (pos 4-10), RECOVERY (pos 11-30),
  or FULL OPTIMIZATION (not ranking / beyond 30). Every optimization and audit pipeline
  loads this skill before taking any action on existing content.
---

# Ranking Status Check Skill

## PURPOSE

Before any optimization, rewrite, or content improvement begins on an existing URL,
this skill determines the current ranking position and sets the correct operating mode.
This prevents over-optimizing content that is already working.

Load this skill at the START of any pipeline that touches existing content.
The mode it sets governs what is and is not allowed for the rest of that pipeline.

---

## STEP 1 -- DETERMINE TARGET KEYWORD

If the user provided a keyword: use it.
If no keyword provided: fetch the URL, read the H1, and infer the target keyword from it.
Confirm inferred keyword with the user before proceeding: "I'll check ranking for '[inferred keyword]'. Correct?"

---

## STEP 2 -- FETCH CURRENT GOOGLE POSITION

Search: [target keyword] site:[domain]
Check: current position of the URL in Google organic results.

If position is found: record as CURRENT_POSITION.
If URL is not found in top 100: record as NOT_RANKING.
If access to live SERP data is unavailable: tell the user and ask them to provide the current position manually. Do not proceed with PROTECT mode assumptions.

---

## STEP 3 -- SET OPTIMIZATION MODE

Based on CURRENT_POSITION:

POSITION 1-3 -> PROTECT MODE
POSITION 4-10 -> SHIELD MODE
POSITION 11-30 -> RECOVERY MODE
NOT_RANKING or beyond position 30 -> FULL OPTIMIZATION MODE

Output at top of every report:
  Ranking Status: Position [X] for "[keyword]" -- [MODE] ACTIVATED

---

## STEP 4 -- MODE DEFINITIONS AND ENFORCEMENT

### PROTECT MODE (positions 1-3)
Google has validated this content's structure, intent, and depth.
Structural changes at this stage carry significant risk of ranking loss.

ALLOWED CHANGES:
  - Fix factual errors (outdated statistics, wrong version numbers, incorrect pricing)
  - Add 1-2 missing FAQ entries based on new PAA questions
  - Refresh internal links to point to newer/more relevant posts in brand.urls
  - Update CTA text, CTA URLs, and UTM parameters
  - Fix broken internal or external links
  - Update publish date if the content has genuinely been refreshed
  - Minor copy improvements within existing sections (not restructuring)

FORBIDDEN CHANGES:
  - Rewriting or changing the H1
  - Restructuring the outline (adding, removing, renaming, or reordering H2s)
  - Removing any existing sections or merging sections together
  - Changing the URL slug
  - Moving the primary keyword out of its validated positions (H1, first 100 words, key H2s)
  - Rewriting the introduction or conclusion
  - Changing the content format or schema type
  - Removing existing internal links

IF USER REQUESTS MAJOR CHANGES:
  Before doing anything, output this message:

  "PROTECT MODE -- Ranking Position #[X]

  This content is currently ranking #[X] on Google for '[keyword]'. Google has already
  validated this content's structure, depth, and intent signal.

  Major structural changes -- rewriting headings, restructuring sections, changing the
  introduction -- can cause Google to re-crawl and re-evaluate the page. This risks
  losing the current position with no guarantee of recovery.

  Safe options I can do right now:
  (a) Minor refresh: update outdated stats, add 1-2 FAQs, refresh internal links and CTAs
      [Shows you exactly what would change before touching anything]
  (b) Show me a diff: I'll show you which minor changes I'd make vs what I'd leave alone
  (c) Override -- full rewrite anyway [HIGH RISK: may lose position #[X]]

  Which would you like?"

  Wait for explicit user response. Do not begin any work until the user selects an option.
  If user selects (c): acknowledge the risk explicitly, then proceed with the full pipeline.


### SHIELD MODE (positions 4-10)
Page 1 ranking. Content is competitive but not at peak position.
Careful, targeted improvements can improve ranking without disrupting what's working.

ALLOWED CHANGES (in addition to all PROTECT MODE changes):
  - Expand genuinely thin sections (under 100 words) with substantive depth
  - Add missing subtopics identified from PAA and SERP gap analysis
  - Improve internal link density by adding links from brand.urls
  - Optimize one section for featured snippet formatting if SERP shows a snippet
  - Add comparison tables or data tables if top 3 competitors have them and this page does not
  - Improve the introduction's hook and first paragraph if it's clearly weak
  - Add a stronger FAQ section if page lacks one and competitors have one

FORBIDDEN CHANGES:
  - Rewriting the H1
  - Changing the URL slug
  - Removing existing sections or H2s
  - Wholesale restructuring of the outline

REQUIRED BEFORE EXECUTING:
  Show the user an optimization plan listing every proposed change.
  User must confirm before any editing begins.
  Format:
    "SHIELD MODE -- Ranking Position #[X]
    Proposed changes:
      [list each specific change]
    Estimated impact: [your assessment]
    Proceed with these changes? (y/n)"


### RECOVERY MODE (positions 11-30)
Near-page-1. Content has some relevance signal but lacks depth or coverage vs top competitors.
More aggressive improvement is appropriate.

ALLOWED CHANGES:
  All SHIELD MODE changes, plus:
  - Full restructure of outline if SERP shows a significantly different content structure
  - Rewrite of underperforming sections
  - Significant depth expansion to match top 3 competitor depth benchmark
  - H2 renaming if current H2s poorly match PAA and search intent
  - Addition of new sections to cover missing subtopics

STILL FORBIDDEN:
  - Changing the URL slug (risk of losing existing link equity)
  - Removing existing high-authority backlink targets

RECOMMENDED:
  Run full audit pipeline before making changes.
  Show the user what the gap analysis found before rewriting.


### FULL OPTIMIZATION MODE (position 31+ or not ranking)
No existing ranking signal to protect. Standard full pipeline applies with no restrictions.

All changes are permitted.
Run the standard content pipeline from Phase 0 through delivery.
No special warnings needed.

---

## STEP 5 -- OUTPUT FORMAT

Output this block at the top of any pipeline where this skill runs:

  RANKING STATUS
  URL: [url]
  Keyword: [keyword]
  Current Position: [X or NOT RANKING]
  Mode: [PROTECT / SHIELD / RECOVERY / FULL]
  Scope: [one sentence describing what is and is not allowed in this session]

This block is also included in Section 13 of the final NEXUS UNIFIED SEO INTELLIGENCE REPORT.

---

## NOTES FOR PIPELINES

- Every optimization pipeline loads this skill FIRST before any audit or content work begins
- The mode output by this skill is read by optimization-engine.skill.md to scope its work
- The mode output is also read by strategist-ai.skill.md to ensure recommendations match the optimization scope
- If ranking position cannot be determined, default to RECOVERY MODE -- not PROTECT MODE
- PROTECT MODE is only activated on confirmed position data, never assumed
