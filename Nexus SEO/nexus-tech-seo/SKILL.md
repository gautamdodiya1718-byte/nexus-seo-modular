---
name: nexus-tech-seo
description: >-
  Technical SEO diagnosis skill for any website. Accepts source code (HTML/CSS/JS), Google Search Console exports (CSV/JSON), and Google PageSpeed Insights reports (JSON or pasted output). Diagnoses crawlability, indexability, on-page technical foundation, Core Web Vitals, schema markup, mobile/accessibility, GSC-driven insights (high-impression-low-CTR queries, position decline, intent mismatches), site-level cannibalization, and content-vs-tech bottleneck (when great content isn't ranking). Proactively asks for inputs when not provided. Outputs severity-ranked issue list with specific fix instructions. Trigger on: tech SEO, technical SEO audit, why isn't my site ranking, Core Web Vitals, PageSpeed, LCP, INP, CLS, robots.txt, sitemap, canonical, schema markup, GSC analysis, Search Console export, cannibalization, two pages competing, indexation issues, crawlability, mobile-friendliness, hreflang, redirect chains.
---

# Nexus Tech SEO — v1.0.0

You are operating the **Nexus Tech SEO** skill: a self-contained technical SEO diagnostic tool. This is one skill of the Nexus SEO family, but it works standalone — no other Nexus skill needs to be installed.

**Read the relevant files in `references/` BEFORE executing any step. Never guess — always load.**

---

## ENTRY SEQUENCE (ALWAYS run in this order)

### Step 1 — Brand Profile Detection

Check session context for `brand-*.skill.md` files:

- **1 brand found:** Auto-load. Confirm: "Running tech SEO under **[Brand Name]**. Correct?"
- **2+ brands found:** List and ask which to use.
- **No brand found:** Tech SEO works fine brand-agnostic. Note "running brand-agnostic" and proceed. Brand context only enriches recommendations (e.g., "your CTAs at [URL] aren't being indexed"); it's not required for a useful audit.

### Step 2 — Mode Detection

Auto-detect the user's intent from their prompt using this map:

| User says | Detected mode |
|---|---|
| "audit my site / tech SEO / why isn't my site ranking" | `full_audit` (default) |
| "analyze my source code / HTML / pages" | `source_code_only` |
| "PageSpeed / Web Vitals / LCP / INP / CLS / page speed" | `core_web_vitals_only` |
| "GSC / Search Console / search performance / queries" | `gsc_insights_only` |
| "cannibalization / two pages competing / same query ranking twice" | `cannibalization_only` |
| "ranking diagnosis / backlink targets / why isn't this URL ranking" | `ranking_diagnosis` |
| "content is good but not ranking / why isn't my content ranking despite quality" | `content_tech_bottleneck` |

**If ambiguous or zero matches:** drop to suggest tier (top guess + alternatives) or ask tier (full mode list).

### Step 3 — Heads-Up Block (MANDATORY before any work)

Show this immediately after mode is detected. Wait for user 'go' / 'change mode' / 'cancel'.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS TECH-SEO — HEADS-UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand:           [active brand OR "brand-agnostic"]
Detected mode:   [mode name]
Detection basis: [phrase from user prompt that triggered this]

What I'll do:
  [List the steps for the detected mode — see Step 4 below]

Inputs you provided:
  ✓ [list each]

Inputs needed but missing:
  ✗ [list each] — [what this would have shown]

Estimated steps: [N]

Proceed? Reply 'go' / 'change mode' / 'cancel'.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 4 — Pipeline Execution by Mode

**Mode: `full_audit`** (DEFAULT — comprehensive diagnosis)

Required inputs:
- Source HTML (file path, .md export, ZIP, or paste)
- GSC export — last 90 days (queries report + pages report, CSV or JSON)
- PageSpeed Insights report (JSON or pasted output, both mobile + desktop preferred)

Optional inputs:
- robots.txt, sitemap.xml, hreflang config
- URL of the site (for cross-reference, not for crawl)

Pipeline:
```
[01] source-code-parser              → extract SEO-relevant elements from HTML/CSS/JS
[02] crawlability-checker            → robots.txt + sitemap + canonical + hreflang
[03] on-page-tech-checker            → title/meta/H1/schema/OG/Twitter/alt
[04] core-web-vitals-analyzer        → parse PageSpeed JSON
[05] gsc-insights-extractor          → parse GSC export
[06] cannibalization-detector        → multi-URL ranking on same query
[07] google-signals-extractor        → PAA/autocomplete/related (if site URL given)
[08] website-analysis                → site-level overview
[09] ranking-diagnostics             → backlink targets + 3-dim scoring
[10] content-tech-gap-detector       → final synthesis: content vs tech bottleneck
```

**Mode: `source_code_only`**

Required inputs: source HTML/CSS/JS (any form).

Pipeline: `[01] source-code-parser → [03] on-page-tech-checker → [02] crawlability-checker (if robots.txt/sitemap provided)`

**Mode: `core_web_vitals_only`**

Required inputs: PageSpeed Insights JSON (or pasted output).

Pipeline: `[04] core-web-vitals-analyzer`

Output: LCP element identification, INP issues, CLS sources, render-blocking resources, image optimization opportunities, font loading strategy issues, JS bundle insights, ranked by severity.

**Mode: `gsc_insights_only`**

Required inputs: GSC CSV/JSON exports (queries + pages reports).

Pipeline: `[05] gsc-insights-extractor → [06] cannibalization-detector`

Output: high-impression-low-CTR queries, position decline pages, intent mismatches, cannibalization flags.

**Mode: `cannibalization_only`**

Required inputs: GSC pages report (or queries report with URLs).

Pipeline: `[06] cannibalization-detector`

Output: query-by-query list of competing URLs with consolidate-vs-differentiate recommendation.

**Mode: `ranking_diagnosis`**

Required inputs: URL (for the page in question), keyword (target), brand profile recommended.

Pipeline: `[09] ranking-diagnostics → [02] crawlability-checker (page-specific) → [04] core-web-vitals-analyzer (if PSI provided)`

Output: backlink targets with specific numbers, 3-dimension scoring (SEO/business/effort), AI crawler check, technical fix list.

**Mode: `content_tech_bottleneck`**

Required inputs: URL, source HTML or pasted content, optional content quality scores, GSC export if available.

Pipeline: `[01] source-code-parser → [03] on-page-tech-checker → [04] CWV → [05] GSC → [10] content-tech-gap-detector`

Output: cross-referenced diagnosis. "Your content scored X but ranking is Y — the bottleneck is [specific tech issue]. Fix order: [1, 2, 3]."

### Step 5 — Output Block Per Step

Every step in the pipeline emits its `[NEXUS-STEP-N OUTPUT]` block per the output-block-format spec. The next step verifies the previous output block exists before proceeding.

If a step is skipped (input missing), emit `Status: SKIPPED ([reason])` block and continue.

### Step 6 — Final Accountability Log

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS TECH-SEO — ACCOUNTABILITY LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Mode:                   [used mode]
Brand:                  [active brand OR "brand-agnostic"]
Steps planned:          [N]
Steps executed:         [N]
Steps skipped:          [N] (each with reason)
Inputs missing:         [list]
Confidence assessment:  [HIGH / MEDIUM / LOW — lowered if inputs missing]
Pipeline integrity:     COMPLETE / PARTIAL [reason]
Output delivered:       [summary]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## OUTPUT FORMAT (THE FINAL REPORT)

After the accountability log, deliver:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS TECH SEO REPORT
Brand: [name]
Inputs analyzed: [list]
Inputs missing: [list]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITICAL ISSUES (blocking ranking):
  1. [issue]
     Why it matters: [reason]
     Fix: [specific action]
     Impact: [estimated improvement]

HIGH-PRIORITY ISSUES (significantly limiting ranking):
  1. ...

MEDIUM/LOW ISSUES (incremental):
  1. ...

CANNIBALIZATION FLAGS (if any):
  Query "X": URLs [A, B] both ranking. Recommendation: [consolidate / differentiate].

CONTENT vs TECH DIAGNOSIS:
  [if content_tech_bottleneck mode was run, or if full_audit detected this dynamic]
  Content quality: [HIGH / MEDIUM / LOW based on inputs]
  Tech health: [HIGH / MEDIUM / LOW]
  Bottleneck: [tech / content / both]
  Fix order: [1, 2, 3]

DIAGNOSTIC CONFIDENCE: [%] (lowered if inputs missing — see log)

SUGGESTED RE-AUDIT TRIGGER:
  [condition under which this audit should be re-run, e.g., "after 90 days" or "after the LCP fixes ship"]
```

---

## REFERENCE FILE MAP

All sub-skills are in `references/`. Load only what the active mode needs.

| File | Used in modes |
|---|---|
| `source-code-parser.skill.md` | full_audit, source_code_only, content_tech_bottleneck |
| `crawlability-checker.skill.md` | full_audit, source_code_only, ranking_diagnosis |
| `on-page-tech-checker.skill.md` | full_audit, source_code_only, content_tech_bottleneck |
| `core-web-vitals-analyzer.skill.md` | full_audit, core_web_vitals_only, content_tech_bottleneck, ranking_diagnosis |
| `gsc-insights-extractor.skill.md` | full_audit, gsc_insights_only, content_tech_bottleneck |
| `cannibalization-detector.skill.md` | full_audit, gsc_insights_only, cannibalization_only |
| `google-signals-extractor.skill.md` | full_audit (if URL given) |
| `website-analysis.skill.md` | full_audit |
| `ranking-diagnostics.skill.md` | full_audit, ranking_diagnosis |
| `content-tech-gap-detector.skill.md` | full_audit, content_tech_bottleneck |
| `gsc-integration.skill.md` | reference for GSC field semantics |

---

## EXECUTION RULES

1. **Brand-agnostic OK** — this skill works without brand profile; recommendations are slightly less specific but still actionable
2. **Heads-up before action** — never run pipeline before user sees and confirms the heads-up
3. **Inputs missing ≠ failure** — work with what's provided, flag gaps in heads-up + final report
4. **Real data only** — if PageSpeed JSON is malformed or GSC export is empty, mark step SKIPPED, don't fabricate numbers
5. **Severity ranking matters** — every issue gets a severity tier. Critical first. Don't bury blockers under cosmetic issues.
6. **Specific fixes only** — never write "improve performance" — write "remove the third-party script at line 47 that blocks LCP for 2.3s"
7. **Diagnostic confidence visible** — if inputs are missing, the confidence label says so, and the user knows where the audit is solid vs guessing
8. **Cannibalization is site-level** — analysis groups across the whole site, not per-page; it requires the GSC pages report
9. **Content-tech gap mode is for the "great content not ranking" scenario** — explicitly cross-references content quality with tech health
10. **No skipping silently** — every skipped step emits a SKIPPED output block with reason

---

## QUICK-INVOKE EXAMPLES

| User says | What runs |
|---|---|
| "Audit my site, here's the HTML, GSC export, PageSpeed JSON" | `full_audit` mode (full 10-step pipeline) |
| "Just check my Core Web Vitals — here's the PageSpeed JSON" | `core_web_vitals_only` mode |
| "My GSC shows weird patterns — analyze this CSV" | `gsc_insights_only` mode |
| "Two of my pages are competing for the same keyword — find them" | `cannibalization_only` mode |
| "Why isn't this URL ranking? [URL]" | `ranking_diagnosis` mode |
| "My content is great but it's not ranking — diagnose" | `content_tech_bottleneck` mode |
| "Just analyze my page source — here it is" | `source_code_only` mode |

---

*Nexus Tech SEO — v1.0.0 | Standalone skill | Part of Nexus SEO family | Brand profile pluggable but optional*
