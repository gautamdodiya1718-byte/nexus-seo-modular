---
name: nexus-content
description: >-
  Content creation skill for blog posts, landing pages, guest posts, tutorials, comparison posts, pillar pages, and content refresh. Substance-driven generation (no word count enforcement), live fact verification of every claim, live link verification before delivery, author voice application, user research injection (interviews, proprietary data, benchmarks), helpful content compliance, inline LLM quality checklist, JSON-LD schema generation, freshness architecture (evergreen vs time-bound sections), decay mapping (review triggers), AI citation optimization, differentiation enforcement (≥3 unique angles), confidence gate (blocks <75% accuracy delivery). Brand profile, author profile, and user preferences pluggable. Trigger on: write a blog, write content, create article, blog post, landing page, guest post, tutorial, vs comparison, pillar page, ultimate guide, refresh content, update content, optimize content, generate content.
---

# Nexus Content — v1.0.0

You are operating the **Nexus Content** skill: a self-contained content creation tool. Generates blog posts, landing pages, guest posts, tutorials, comparison posts, pillar pages, and content refreshes with substance gates, live fact verification, author voice, and full quality enforcement.

This is one skill of the Nexus SEO family. Works completely standalone — no other Nexus skill required (though pairs naturally with `nexus-research` for research input).

**Read the relevant files in `references/` BEFORE executing any step. Never guess — always load.**

---

## ENTRY SEQUENCE

### Step 1 — Brand Profile Detection

Standard pattern. Brand profile **strongly recommended** for content generation (drives voice, internal linking, banned words, CTAs, mention frequency). Brand-agnostic mode works but produces generic-voice content with no internal linking.

### Step 2 — Author Profile Detection

Check for `author-*.skill.md` files. If present, content is generated in that author's voice. If absent, content uses brand-default voice (less specific).

### Step 3 — User Preferences Detection

Check for `user-preferences-*.md` files. If present, applies user-specific rules (always-include, never-include, structural preferences) on top of brand and author.

### Step 4 — Mode Detection

| User says | Detected mode |
|---|---|
| "write a blog on / write an article / blog post" | `standard_blog` (DEFAULT) |
| "landing page / vs page / alternatives page / features page / comparison page" | `landing_page` |
| "guest post / write for [external site]" | `guest_post` |
| "tutorial / how to / step-by-step guide" | `tutorial` |
| "X vs Y / compare X and Y / comparison post" | `comparison_post` |
| "pillar page / ultimate guide / comprehensive guide" | `pillar_page` |
| "refresh / update / improve / optimize" + URL | `content_refresh` |

### Step 5 — Heads-Up Block

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS CONTENT — HEADS-UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand:           [active OR brand-agnostic]
Author profile:  [active OR none]
User preferences: [active OR none]
Detected mode:   [mode name]
Detection basis: [phrase from prompt]

What I'll do:
  [Pipeline steps for the detected mode]

Inputs you provided:
  ✓ Topic / target keyword: [X]
  [other inputs]

Inputs needed but missing:
  ✗ [list — e.g., "Research package (will run minimal SERP analysis if absent)", "User research documents (skipping injection)"]

Estimated steps: [N]

Proceed? Reply 'go' / 'change mode' / 'cancel'.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 6 — Pipeline Execution by Mode

**Mode: `standard_blog`** (DEFAULT — full content pipeline)

Pipeline:
```
[01] ranking-status-check (if URL provided — sets PROTECT/SHIELD/RECOVERY/FULL mode)
[02] pre-execution-input (research + user config)
[03] user-research-injector (ingests user-provided documents/data/interviews if any)
[04] differentiation-enforcer (verifies outline has ≥3 unique angles before generation)
[05] author-voice-applier (loads author profile if present, calibrates generation)
[06] content-generation-engine (multi-layer generation with substance gates instead of word count)
[07] real-world-examples (every claim: real company, real number, real source)
[08] code-generation-preview (if brand.code_examples=ON AND SERP shows code)
[09] infographic-image-engine (minimum 2 SVG infographics)
[10] internal-linking (from brand.urls — count by brand.site_size)
[11] humanizer (full pass — author profile + brand voice + banned words)
[12] technical-accuracy-checker (existing skill — pairs with live-fact-verifier)
[13] live-fact-verifier (web search per claim — VERIFIED/WRONG/UNVERIFIABLE/OPINION)
[14] live-link-verifier (HTTP HEAD on every internal/external link)
[15] substance-gate (per-section completeness/density/self-sufficiency check — replaces word count)
[16] inline-llm-checklist (9-category quality checks during generation, not after)
[17] helpful-content-checker (Google HCU questions as scored gates)
[18] ai-citation-optimizer (structures content for AI surface lift)
[19] schema-generator (JSON-LD: Article + FAQPage + BreadcrumbList + HowTo if applicable)
[20] freshness-architecture (separates evergreen spine from time-bound sections)
[21] decay-mapping (tags decay-sensitive sections with review triggers — 30/60/90/180 days)
[22] content-validation (5-dimension quality gate)
[23] content-engagement (visual breaks every 300 words)
[24] confidence-gate (HARD BLOCK if accuracy <75% for content type)
[25] metadata-generator (title + meta + excerpt + slug + schema)
```

**Mode: `landing_page`**

Pipeline (same backbone, with landing-page-engine at center):
```
[01] ranking-status-check
[02] landing-page-engine (replaces content-generation-engine — conversion-focused)
[03] real-world-examples (proof points, case studies, metrics)
[04] internal-linking
[05] humanizer
[06] live-fact-verifier
[07] live-link-verifier
[08] substance-gate
[09] schema-generator (page-type schema from brand.schema_preference)
[10] confidence-gate
[11] metadata-generator
```

**Mode: `guest_post`**

Loads `guest-post-mode.skill.md` which overrides default brand-promotion rules:
```
[01] guest-post-mode (sets host-brand context: ~30% host promotion, 10% your-brand, 60% informational; no CTAs; different anchor rules)
[02] content-generation-engine (with guest-post-mode overrides)
[03] real-world-examples
[04] live-fact-verifier
[05] live-link-verifier (verify every link)
[06] humanizer
[07] substance-gate
[08] confidence-gate
```

**Mode: `tutorial`**

Loads `tutorial-mode.skill.md`:
```
[01] tutorial-mode (mandatory: prerequisites + ordered steps + working code + expected outputs per step)
[02] content-generation-engine (tutorial structure)
[03] code-generation-preview (mandatory in tutorials)
[04] real-world-examples
[05] live-fact-verifier
[06] live-link-verifier
[07] humanizer
[08] substance-gate
[09] schema-generator (HowTo schema mandatory)
[10] confidence-gate
```

**Mode: `comparison_post`**

Loads `comparison-post-mode.skill.md`:
```
[01] comparison-post-mode (mandatory comparison table + balanced treatment + criteria-based scoring)
[02] content-generation-engine (comparison structure)
[03] real-world-examples
[04] live-fact-verifier (extra strict — comparison content has more fact claims)
[05] live-link-verifier
[06] humanizer
[07] substance-gate
[08] differentiation-enforcer (extra strict — comparison content needs unique angles to differentiate from other comparisons)
[09] confidence-gate
```

**Mode: `pillar_page`**

Loads `pillar-page-mode.skill.md`:
```
[01] pillar-page-mode (mandatory: comprehensive coverage of topic universe, internal cluster links to sub-topic pages, table of contents, jump-to navigation)
[02] content-generation-engine (pillar structure)
[03] internal-linking (extra emphasis — pillar should link to many cluster pages)
[04] real-world-examples
[05] live-fact-verifier
[06] live-link-verifier
[07] humanizer
[08] substance-gate
[09] differentiation-enforcer (≥7 unique items required for pillar)
[10] schema-generator (Article schema + comprehensive sub-schemas)
[11] freshness-architecture
[12] decay-mapping
[13] confidence-gate
```

**Mode: `content_refresh`**

```
[01] ranking-status-check (DETERMINES SCOPE — PROTECT/SHIELD/RECOVERY/FULL)
[02] optimization-engine (scope limited by ranking status mode)
[03] real-world-examples (refresh stale examples)
[04] live-fact-verifier (verify all claims still current)
[05] live-link-verifier (catch broken links from time)
[06] humanizer (light pass — voice consistency)
[07] substance-gate (only on changed sections)
[08] decay-mapping (re-tag with new review triggers)
[09] confidence-gate
[10] metadata-generator (refresh if brand.schema_preference or language changed)
```

### Step 7 — Output Block Per Step

Standard. Skipped steps emit SKIPPED with reason.

### Step 8 — Final Accountability Log

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS CONTENT — ACCOUNTABILITY LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Mode:                  [used]
Brand:                 [active OR brand-agnostic]
Author profile:        [active OR none]
User preferences:      [active OR none]
Steps planned:         [N]
Steps executed:        [N]
Steps skipped:         [N] (reasons)
Inputs missing:        [list]
Confidence:            [HIGH/MEDIUM/LOW]
Substance gate:        [PASS / FAIL — sections flagged]
Live fact verification: [N% claims verified — pass/fail vs threshold]
Link verification:     [N broken — pass/fail]
Confidence gate:       [PASS / BLOCK — reason]
Pipeline integrity:    COMPLETE / PARTIAL [reason]
Output delivered:      [type — full content / blocked with remediation list]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## OUTPUT FORMAT

### When confidence-gate PASSES — full content delivered:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS CONTENT — DELIVERED
Brand: [name] | Author: [name OR none]
Mode: [used] | Word count: [N — informational, not a target]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[FULL CONTENT in Markdown]

---

CONTENT METADATA:
  Title: [...]
  Meta description: [...]
  Excerpt: [...]
  URL slug: [...]
  Schema type: [...]

JSON-LD SCHEMA BLOCKS:
  [Article / BlogPosting block]
  [FAQPage block if FAQ section present]
  [BreadcrumbList block]
  [HowTo block if tutorial]

INTERNAL LINKS PLACED: [N — from brand.urls]
EXTERNAL LINKS WITH rel="nofollow": [N]
INFOGRAPHICS GENERATED: [N — paths to SVG files or inline SVG]
CODE EXAMPLES: [N — verified working]
CALLOUTS: [N — Definition / Tip / Note distribution]

QUALITY SCORES:
  Substance gate: PASS (every section met completeness/density/self-sufficiency)
  Live fact verification: [N%] verified
  Link verification: [N] links checked, [N] broken (replaced from brand inventory)
  Helpful Content score: [X/100]
  AI citation readiness: [X/100]
  Differentiation: [N unique angles vs top 10]
  E-E-A-T (when nexus-audit run on this content): [delegated]

DECAY MAPPING:
  Sections flagged for review:
    - Section [X]: review by [date] (reason: contains version-specific claims)
    - ...

REVIEW TRIGGER: [date — when to re-audit this content]
```

### When confidence-gate BLOCKS:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS CONTENT — BLOCKED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Content not delivered due to confidence-gate failure.

REASONS:
  [from confidence-gate output — specific failed thresholds + remediation]

REMEDIATION:
  [from upstream skills — what needs fixing + which skills to re-run after fixes]

OPTION: Reply 'override confidence gate' to receive content with prominent UNRELIABLE warning header.
```

---

## REFERENCE FILE MAP

| File | Used in modes |
|---|---|
| `pre-execution-input.skill.md` | all generation modes |
| `content-generation-engine.skill.md` | standard_blog, tutorial, comparison_post, pillar_page, guest_post |
| `real-world-examples.skill.md` | all modes |
| `code-generation-preview.skill.md` | tutorial (mandatory), standard_blog (conditional) |
| `infographic-image-engine.skill.md` | standard_blog, pillar_page, tutorial |
| `content-engagement.skill.md` | all modes |
| `internal-linking.skill.md` | all modes |
| `humanizer.skill.md` | all modes (mandatory) |
| `technical-accuracy-checker.skill.md` | all modes (paired with live-fact-verifier) |
| `content-validation.skill.md` | all modes |
| `landing-page-engine.skill.md` | landing_page (replaces content-generation-engine) |
| `optimization-engine.skill.md` | content_refresh |
| `ranking-status-check.skill.md` | content_refresh, when URL provided |
| `metadata-generator.skill.md` | all modes (final step) |
| `substance-gate.skill.md` (NEW) | all modes (replaces word count enforcement) |
| `live-fact-verifier.skill.md` (shared with audit) | all modes (mandatory) |
| `live-link-verifier.skill.md` (NEW) | all modes (mandatory) |
| `author-voice-applier.skill.md` (NEW) | all modes (when author profile loaded) |
| `user-research-injector.skill.md` (NEW) | all modes (when user research provided) |
| `helpful-content-checker.skill.md` (shared with audit) | all modes |
| `inline-llm-checklist.skill.md` (NEW) | all modes |
| `schema-generator.skill.md` (NEW) | all modes (mandatory) |
| `freshness-architecture.skill.md` (NEW) | standard_blog, pillar_page, content_refresh |
| `decay-mapping.skill.md` (NEW) | all modes |
| `ai-citation-optimizer.skill.md` (NEW) | all modes |
| `differentiation-enforcer.skill.md` (shared with research) | all modes |
| `confidence-gate.skill.md` (shared with audit) | all modes (mandatory final gate) |
| `guest-post-mode.skill.md` (NEW) | guest_post |
| `tutorial-mode.skill.md` (NEW) | tutorial |
| `comparison-post-mode.skill.md` (NEW) | comparison_post |
| `pillar-page-mode.skill.md` (NEW) | pillar_page |

---

## EXECUTION RULES (the new content principles, replacing old enforcement)

1. **Substance over word count.** Per-section completeness/density/self-sufficiency check. SERP word count is orientation signal, not target. Article length emerges from substance.

2. **Live fact verification mandatory.** Every claim with a number/feature/comparison/date triggers web search before sentence is finalized. WRONG claims removed/corrected. UNVERIFIABLE claims marked or rephrased as opinion.

3. **Live link verification before delivery.** HTTP HEAD on every link. Broken links flagged or auto-replaced from brand link inventory.

4. **Author voice when profile loaded.** Vocabulary tier, signature moves, opinion stances applied during generation — not just humanization.

5. **User research as Priority 1 evidence.** When user provides documents/data/interviews, those are the primary evidence. Generic claims that contradict user data get flagged.

6. **Differentiation enforced before generation.** Outline must have ≥3 unique angles vs top 10 SERP. Otherwise blueprint blocked, requires enrichment.

7. **Inline LLM checklist DURING generation.** 9-category quality checks run as in-pipeline gates, not post-hoc review.

8. **Helpful Content compliance scored.** Google HCU questions evaluated as scored gates with revision instructions on failure.

9. **AI citation optimization built in.** Content structured for AI surface lift (Perplexity, ChatGPT search, AI Overviews) — not just Google ranking.

10. **Schema generated automatically.** Complete JSON-LD blocks for Article + FAQPage + BreadcrumbList + HowTo (if applicable). Not optional.

11. **Freshness architecture.** Evergreen spine separated from time-bound sections. Each section tagged for what changes over time.

12. **Decay mapping.** Every piece tagged with decay-sensitive sections + review triggers (30/60/90/180 days based on topic velocity).

13. **Confidence gate is hard.** If accuracy <75% (TOFU) or higher thresholds for MOFU/BOFU/YMYL, content is BLOCKED. User can override but content carries UNRELIABLE warning.

14. **Brand-pluggable, author-pluggable, user-preferences-pluggable.** All three are optional; system degrades gracefully when absent.

15. **No skipping silently.** Every skipped step emits SKIPPED with reason.

---

## QUICK-INVOKE EXAMPLES

| User says | What runs |
|---|---|
| "Write a blog on [topic]" | `standard_blog` mode (full pipeline) |
| "Create a landing page for [product]" | `landing_page` mode |
| "Write a guest post for [site] on [topic]" | `guest_post` mode |
| "Tutorial: how to [task]" | `tutorial` mode |
| "Compare [X] vs [Y]" | `comparison_post` mode |
| "Pillar page for [topic cluster]" | `pillar_page` mode |
| "Refresh / update / improve [URL]" | `content_refresh` mode |

---

*Nexus Content — v1.0.0 | Standalone skill | Part of Nexus SEO family | Brand, author, user preferences all pluggable*
