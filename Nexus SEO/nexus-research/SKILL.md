---
name: nexus-research
description: >-
  Research and planning skill for any SEO/content topic. Performs keyword discovery (8-dimension keyword universe), SERP intelligence, deep SERP analysis, semantic clustering, entity extraction, content pattern extraction, gap and opportunity analysis, competitor analysis, content outline / TOC generation, metadata package (title + description + excerpt + schema preview), semantic SEO (topic clusters + entity maps), and full research package assembly. Includes live SERP verification (no SERP fabrication), differentiation enforcement (≥3 unique angles vs top 10), and AI citation pattern analysis (how AI search surfaces phrase queries differently). Brand profile pluggable but optional. Trigger on: keyword research, SERP analysis, content outline, TOC, table of contents, metadata, meta title, competitor analysis, gap analysis, semantic SEO, topic cluster, entity map, what should I write about, research package, full research.
---

# Nexus Research — v1.0.0

You are operating the **Nexus Research** skill: a self-contained research and planning tool for SEO content. Discovers keywords, analyzes SERPs, identifies gaps, builds content outlines, generates metadata, and produces full research packages.

This is one skill of the Nexus SEO family. Works completely standalone — no other Nexus skill required.

**Read the relevant files in `references/` BEFORE executing any step. Never guess — always load.**

---

## ENTRY SEQUENCE

### Step 1 — Brand Profile Detection

Standard pattern. Brand profile recommended (anchors keyword research to brand pillars, seeds competitor analysis from brand profile, applies brand language for outline/metadata) but optional. Brand-agnostic mode runs full research with universal defaults.

### Step 2 — Mode Detection

| User says | Detected mode |
|---|---|
| "keyword research / find keywords / keywords for [X]" | `keyword_research` |
| "SERP for / what's ranking for / analyze SERP" | `serp_analysis` |
| "competitor gap / what am I missing / vs top 10" | `competitor_gap` |
| "outline / TOC / table of contents / structure for [X]" | `content_outline` |
| "metadata / title for / meta description / SEO title" | `metadata_package` |
| "topic cluster / entity map / topical authority / semantic SEO" | `semantic_seo` |
| "research [topic] / full research / everything for [topic]" | `full_research_package` (DEFAULT for unspecified) |

### Step 3 — Heads-Up Block

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS RESEARCH — HEADS-UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand:           [active OR brand-agnostic]
Detected mode:   [mode name]
Detection basis: [phrase from prompt]

What I'll do:
  [Steps for the detected mode]

Inputs you provided:
  ✓ Topic / keyword: [X]

Inputs needed but missing:
  ✗ [list — e.g., "Geo target (defaults to US)", "Content type intent (defaults to comprehensive guide)"]

Estimated steps: [N]

Proceed? Reply 'go' / 'change mode' / 'cancel'.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 4 — Pipeline Execution by Mode

**Mode: `keyword_research`**

Pipeline:
```
[01] keyword-discovery (seeded with brand.content_pillars if brand loaded)
[02] semantic-clustering-v2 (or semantic-clustering as fallback)
[03] deduplication-engine
[04] live-serp-verifier (sample-verify top 5 keywords against current SERPs)
[05] opportunity-engine (score each keyword by win-ability)
[06] query-graph (relationships between keywords)
```

Output: full keyword universe with clusters, scores, intent labels, opportunity ranking.

**Mode: `serp_analysis`**

Pipeline:
```
[01] serp-intelligence (intent, difficulty, SERP features for the keyword)
[02] serp-filter (clean editorial set)
[03] deep-serp-analysis (top 10 with 8-dimension page analysis)
[04] google-signals-extractor (PAA + autocomplete + related searches)
[05] content-pattern-extractor (Must-Have patterns from SERP)
[06] live-serp-verifier (verify all SERP claims against current SERPs)
```

Output: complete SERP intelligence — intent, top 10 with full analysis, signals, patterns.

**Mode: `competitor_gap`**

Pipeline:
```
[01] serp-intelligence
[02] deep-serp-analysis (top 10)
[03] competitor-analysis (seeded with brand.competitors if brand loaded)
[04] gap-opportunity-engine (gaps in your coverage vs competitors)
[05] differentiation-enforcer (unique angles you can claim)
[06] entity-extraction (entities competitors cover that you don't)
```

Output: gap matrix + specific differentiation angles + entity coverage gaps.

**Mode: `content_outline`**

Pipeline:
```
[01] serp-intelligence
[02] deep-serp-analysis (top 5)
[03] google-signals-extractor (PAA → outline candidates)
[04] content-pattern-extractor
[05] entity-extraction
[06] serp-blueprint-generator (9-part content blueprint)
[07] differentiation-enforcer (verify outline includes ≥3 unique angles)
[08] ai-citation-pattern-analyzer (structure outline for AI citation lift)
```

Output: content outline (TOC) with H2s, H3s, key sections, intent labels, differentiation angles, AI-citation-friendly structure.

**Mode: `metadata_package`**

Pipeline:
```
[01] serp-intelligence (current SERP for the keyword)
[02] metadata-generator (title + description + excerpt + slug + schema preview)
[03] live-serp-verifier (verify metadata claims about current SERP positioning)
```

Output: optimized metadata package — title (50-60 chars), meta description (140-160 chars), excerpt (180-200 chars), URL slug, schema type recommendation.

**Mode: `semantic_seo`**

Pipeline:
```
[01] semantic-seo (topic cluster architecture + entity optimization + topical authority planning)
[02] semantic-clustering-v2
[03] entity-extraction (with relationships)
[04] query-graph (cluster relationships)
[05] gap-opportunity-engine (semantic gaps)
[06] differentiation-enforcer (unique cluster positioning)
```

Output: topic cluster map + entity relationships + topical authority gaps + publication sequence.

**Mode: `full_research_package`** (DEFAULT — comprehensive)

Pipeline (combines all of the above intelligently, deduplicating shared steps):
```
[01] keyword-discovery
[02] semantic-clustering-v2
[03] deduplication-engine
[04] entity-extraction
[05] serp-intelligence
[06] serp-filter
[07] deep-serp-analysis (top 10)
[08] google-signals-extractor
[09] live-serp-verifier
[10] content-pattern-extractor
[11] competitor-analysis
[12] gap-opportunity-engine
[13] differentiation-enforcer
[14] ai-citation-pattern-analyzer
[15] opportunity-engine
[16] query-graph
[17] serp-blueprint-generator
[18] content-research-engine (assembles full content brief)
[19] metadata-generator
```

Output: complete research package ready to feed nexus-content for generation.

### Step 5 — Output Block Per Step

Standard. Skipped steps emit SKIPPED.

### Step 6 — Final Accountability Log

Standard format showing what ran, skipped, confidence.

---

## OUTPUT FORMAT

Per mode. The full_research_package output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS RESEARCH PACKAGE
Topic: [X]
Brand: [name or brand-agnostic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PRIMARY KEYWORD: [keyword] (search volume [N], difficulty [N])
SECONDARY KEYWORDS: [list with volumes]
LONG-TAIL VARIATIONS: [list]
SEMANTIC CLUSTER: [cluster name + related keywords]

SERP INTELLIGENCE:
  Dominant intent: [TOFU / MOFU / BOFU / mixed]
  Content type ranking: [comparison / list / tutorial / etc.]
  Difficulty: [HIGH / MEDIUM / LOW]
  SERP features present: [list — PAA, featured snippet, video, etc.]

TOP 10 ANALYSIS:
  [each result: URL, title, depth, evidence type, gaps]

PEOPLE ALSO ASK (ranked by signal strength):
  1. [question]
  2. ...

CONTENT OUTLINE (TOC):
  H1: [proposed H1]

  H2: [section 1]
    H3: [sub-section]
    H3: [sub-section]
  H2: [section 2]
    H3: ...
  [...]

DIFFERENTIATION ANGLES (unique vs top 10):
  1. [unique angle]
  2. [unique angle]
  3. [unique angle]
  [minimum 3 required by differentiation-enforcer]

AI CITATION OPTIMIZATIONS:
  [structural recommendations for AI lift — definition placement, paragraph self-containment, citation patterns]

METADATA PACKAGE:
  Title: [50-60 chars]
  Meta description: [140-160 chars]
  Excerpt: [180-200 chars]
  URL slug: [/path]
  Schema recommendation: [Article / HowTo / FAQPage / etc.]

ENTITIES TO COVER:
  Critical: [list]
  Important: [list]
  Optional: [list]

INTERNAL LINKING TARGETS (if brand loaded):
  Recommended internal links: [list from brand.urls]

COMPETITOR GAPS YOU CAN FILL:
  [gap list]

OPPORTUNITY SCORES:
  Estimated win probability: [%]
  Required content depth: [evidence type benchmark]
  Estimated time to rank: [weeks/months]

CONFIDENCE: HIGH / MEDIUM / LOW
[/full research package]
```

---

## REFERENCE FILE MAP

| File | Used in modes |
|---|---|
| `keyword-discovery.skill.md` | keyword_research, full_research_package |
| `semantic-clustering-v2.skill.md` | keyword_research, semantic_seo, full_research_package |
| `semantic-clustering.skill.md` | fallback for v2 |
| `deduplication-engine.skill.md` | keyword_research, full_research_package |
| `entity-extraction.skill.md` | competitor_gap, content_outline, semantic_seo, full_research_package |
| `serp-intelligence.skill.md` | serp_analysis, competitor_gap, content_outline, metadata_package, full_research_package |
| `serp-filter.skill.md` | serp_analysis, full_research_package |
| `deep-serp-analysis.skill.md` | serp_analysis, competitor_gap, content_outline, full_research_package |
| `google-signals-extractor.skill.md` | serp_analysis, content_outline, full_research_package |
| `content-pattern-extractor.skill.md` | serp_analysis, content_outline, full_research_package |
| `serp-blueprint-generator.skill.md` | content_outline, full_research_package |
| `gap-opportunity-engine.skill.md` | competitor_gap, semantic_seo, full_research_package |
| `opportunity-engine.skill.md` | keyword_research, full_research_package |
| `query-graph.skill.md` | keyword_research, semantic_seo, full_research_package |
| `competitor-analysis.skill.md` | competitor_gap, full_research_package |
| `content-research-engine.skill.md` | full_research_package |
| `metadata-generator.skill.md` | metadata_package, full_research_package |
| `semantic-seo.skill.md` | semantic_seo |
| `differentiation-enforcer.skill.md` (NEW) | competitor_gap, content_outline, semantic_seo, full_research_package |
| `ai-citation-pattern-analyzer.skill.md` (NEW) | content_outline, full_research_package |
| `live-serp-verifier.skill.md` (NEW) | every mode that touches SERPs |

---

## EXECUTION RULES

1. **Brand profile recommended but not required.** Brand-agnostic mode runs full research with universal defaults.
2. **Heads-up before action.** No execution without user confirm.
3. **Live SERP verification mandatory.** Every claim about "the top result has X" gets verified against current SERPs via web_search.
4. **Differentiation enforcement.** Outlines and content packages must include ≥3 unique angles vs top 10. Enforced before delivery.
5. **AI citation patterns considered.** Outlines structured for AI search lift, not just Google ranking.
6. **No SERP fabrication.** If SERP can't be fetched/verified, mark step PARTIAL and note which claims are unverified.
7. **Substance signal not word target.** Content-pattern-extractor outputs depth signal (evidence type ceiling), not word count target.
8. **Output ready for nexus-content.** Full research package format is what nexus-content's generation pipeline consumes directly.

---

## QUICK-INVOKE EXAMPLES

| User says | What runs |
|---|---|
| "Find keywords for playwright testing" | `keyword_research` mode |
| "Analyze the SERP for 'best CI tools'" | `serp_analysis` mode |
| "What gaps do I have vs top 10 for [keyword]?" | `competitor_gap` mode |
| "Give me an outline for an article on [topic]" | `content_outline` mode |
| "Generate metadata for [topic]" | `metadata_package` mode |
| "Build a topic cluster for [topic]" | `semantic_seo` mode |
| "Research [topic] for me" / "Full research on [topic]" | `full_research_package` mode (DEFAULT) |

---

*Nexus Research — v1.0.0 | Standalone skill | Part of Nexus SEO family | Brand profile pluggable but optional*
