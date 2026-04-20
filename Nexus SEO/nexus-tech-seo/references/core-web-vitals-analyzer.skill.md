# core-web-vitals-analyzer.skill

**Role:** Parse Google PageSpeed Insights output (JSON or pasted text) and extract Core Web Vitals diagnostics with specific fix recommendations. LCP element identification, INP issues, CLS sources, render-blocking resources, image and font optimization opportunities.

**Loaded by:** `nexus-tech-seo` SKILL.md in modes: full_audit, core_web_vitals_only, content_tech_bottleneck, ranking_diagnosis

---

## Inputs

Accept any of:
- Full PageSpeed Insights JSON output (mobile and/or desktop)
- Pasted summary text from PSI
- Screenshot (extract numbers from visible report)
- URL to test (NOTE: this skill does NOT call PSI itself; if user only provides URL, ask them to run PSI and share the report)

If only mobile or only desktop provided: run analysis for what's there, recommend running the missing one.

---

## What to extract

### Core Web Vitals (the 3 thresholds Google uses)

1. **LCP (Largest Contentful Paint)**
   - Value in seconds (mobile + desktop separately)
   - Threshold: GOOD <2.5s, NEEDS IMPROVEMENT 2.5–4.0s, POOR >4.0s
   - **LCP element** — extract from PSI's "Largest Contentful Paint element" section
   - Render-blocking resources affecting LCP

2. **INP (Interaction to Next Paint) — replaced FID in 2024**
   - Value in milliseconds
   - Threshold: GOOD <200ms, NEEDS IMPROVEMENT 200–500ms, POOR >500ms
   - **Long tasks** — extract list of scripts blocking the main thread
   - Event handlers with high blocking time

3. **CLS (Cumulative Layout Shift)**
   - Value (unitless)
   - Threshold: GOOD <0.1, NEEDS IMPROVEMENT 0.1–0.25, POOR >0.25
   - **CLS sources** — images without dimensions, dynamically inserted content, web fonts causing FOIT/FOUT, ads/embeds without reserved space

### Other PSI metrics

4. **FCP (First Contentful Paint)** — value, threshold, fix targets
5. **TBT (Total Blocking Time)** — value, threshold, related to INP
6. **TTI (Time to Interactive)** — value, threshold
7. **Speed Index** — value, threshold

### PSI Opportunities section

Extract every opportunity with estimated savings:
- Eliminate render-blocking resources (list URLs + savings)
- Properly size images (list images + savings)
- Defer offscreen images (list images + savings)
- Serve images in next-gen formats (list URLs + savings)
- Efficiently encode images (list URLs + savings)
- Enable text compression (file types affected)
- Preconnect to required origins (list domains)
- Reduce server response times (TTFB target)
- Avoid multiple page redirects (list redirect chains)
- Preload key requests (list resources)
- Use HTTP/2 (if not enabled)
- Use video formats for animated content (if GIFs detected)
- Remove unused CSS (per stylesheet, savings)
- Remove unused JavaScript (per file, savings)
- Minify CSS / JS / HTML (savings)

### PSI Diagnostics section

Extract every diagnostic:
- Avoid enormous network payloads (total bytes)
- Serve static assets with efficient cache policy (list resources)
- Minimize main-thread work (top scripts by execution time)
- Reduce JavaScript execution time (per script)
- Avoid an excessive DOM size (DOM node count)
- Avoid chaining critical requests (chain depth)
- User Timing marks (if present)
- Minimize critical request depth
- Largest Contentful Paint element (the actual element)
- Avoid large layout shifts (per shift, with score contribution)
- Avoid long main-thread tasks (per task, duration)
- Image elements have explicit width and height (list violations)

---

## Analysis logic

For each Core Web Vital below threshold:

### LCP issues

- **LCP > 2.5s with image as LCP element**
  - Recommend: preload, WebP/AVIF format, proper sizing, srcset, lazy-load other images so LCP image gets bandwidth priority
- **LCP > 2.5s with text as LCP element**
  - Recommend: preload font files, use font-display: swap, reduce render-blocking CSS
- **LCP > 2.5s caused by render-blocking resources**
  - Recommend: defer non-critical CSS/JS, inline critical CSS

### INP issues

- **INP > 200ms with long tasks identified**
  - Recommend: break long tasks (split work with `requestIdleCallback`, debounce expensive operations)
  - Identify specific scripts: third-party (analytics, chat widgets, ads) vs first-party
  - For third-party: load with `defer` or after user interaction
  - For first-party: profile and optimize specific functions

### CLS issues

- **CLS > 0.1 from images without dimensions**
  - Recommend: add explicit width/height attributes to all `<img>` tags
- **CLS > 0.1 from web fonts (FOIT/FOUT)**
  - Recommend: `font-display: swap`, preload critical fonts, use system font fallbacks with same metrics
- **CLS > 0.1 from dynamically inserted content (ads, popups, embeds)**
  - Recommend: reserve space with min-height CSS, lazy-load below fold
- **CLS > 0.1 from animations**
  - Recommend: animate transform/opacity (compositor-only) instead of layout properties

### Mobile vs desktop divergence

If mobile scores are much worse than desktop (common):
- Flag as critical (Google uses mobile-first indexing)
- Specifically call out: image sizing for mobile, font loading on mobile, third-party scripts loaded on mobile that should be conditional

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: core-web-vitals-analyzer
Status: COMPLETE
Inputs consumed: PageSpeed Insights JSON [mobile / desktop / both]

CORE WEB VITALS SUMMARY:

MOBILE:
  LCP: [value]s — [GOOD / NEEDS IMPROVEMENT / POOR]
  INP: [value]ms — [GOOD / NEEDS IMPROVEMENT / POOR]
  CLS: [value] — [GOOD / NEEDS IMPROVEMENT / POOR]

DESKTOP:
  LCP: [value]s — [tier]
  INP: [value]ms — [tier]
  CLS: [value] — [tier]

LCP ELEMENT (mobile): [the actual element identified by PSI]
LCP ELEMENT (desktop): [element]

CRITICAL FIXES (Core Web Vitals failing):
  1. LCP fix: [specific action — e.g., "Preload the hero image at /images/hero.jpg in <head>: <link rel='preload' as='image' href='/images/hero.jpg'>"]
     Estimated improvement: [X seconds]

  2. INP fix: [specific action — e.g., "Defer the analytics script /js/analytics.bundle.js — add defer attribute or load after window.onload"]
     Estimated improvement: [X ms]

  3. CLS fix: [specific action — e.g., "Add width and height attributes to 14 images on the homepage — current CLS contribution: 0.18"]
     Estimated improvement: [X CLS reduction]

HIGH-PRIORITY OPPORTUNITIES (from PSI):
  - [opportunity name]: [estimated savings]
    Fix: [specific action with file paths]

  - ...

DIAGNOSTICS:
  - DOM size: [N nodes — flag if >1500]
  - Cache policy: [N resources need better caching]
  - Main-thread work: [top 3 heaviest scripts]
  - Network payload: [MB total]
  - Critical request depth: [N levels]

MOBILE vs DESKTOP DIVERGENCE:
  [if mobile scores significantly worse: explicit callout + mobile-specific fixes]

RE-AUDIT TRIGGER: After implementing top 3 critical fixes, re-run PSI to confirm improvement.

CONFIDENCE: HIGH (full JSON) / MEDIUM (pasted text) / LOW (only screenshot or only summary)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Don't fetch PSI yourself.** This skill works only on provided PSI output.
2. **Mobile-first reporting.** Report mobile issues before desktop in the output.
3. **Specific fixes with file references.** "Defer /js/analytics.bundle.js" — not "defer JavaScript."
4. **Estimated savings come from PSI.** Don't make up numbers; use PSI's stated estimates.
5. **Skip cleanly if no PSI.** If user only gave a URL with no PSI report, emit SKIPPED with: "No PageSpeed Insights report provided. Run https://pagespeed.web.dev/ on your URL and paste the JSON or share the report."
