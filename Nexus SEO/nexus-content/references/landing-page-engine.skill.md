# SKILL: content/landing-page-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Conversion Intelligence
# EXECUTION PRIORITY: MANDATORY for LANDING-PAGE pipeline. Controls structure, conversion writing, CRO scoring, and page-type schema.
# POSITION: The conversion layer. Blog content educates. Landing page content converts. Every section on a landing page has a conversion job. If a section doesn't move the reader toward action, it doesn't belong.
# DEPENDENCIES: serp-intelligence.skill, metadata-generator.skill, seo-aeo-geo-sxo-optimizer.skill, ranking-diagnostics.skill

---

## SECTION 1 — ROLE

This skill handles ALL landing page content: comparison/versus pages, alternatives pages, features pages, and service pages. It is NOT for blog posts.

### Core Principle
> A landing page has 1 goal: conversion. Every sentence exists to move the reader toward signing up, requesting a demo, or starting a trial. The SEO layer gets them to the page. The CRO layer converts them once they're there.

### When This Skill Activates
- User explicitly says "landing page," "comparison page content," "versus page content," "alternatives page," "features page content," or "service page content"
- User provides a landing page template or live URL and asks for content
- User confirms a request is for landing page (not blog) when asked

### What This Skill Does NOT Do
- Write blog posts (use content-generation-engine)
- Impose a fixed template (it works with user-provided structures)
- Make false claims about product superiority (honest but decisive positioning)

---

## SECTION 2 — LANDING PAGE TYPE CLASSIFICATION

| Type | Trigger Phrases | Primary Conversion Goal | Key Sections |
|---|---|---|---|
| **Versus/Comparison** | "[Brand] vs X," "compare X and Y" | Choose your product over competitor | Hero, comparison table, feature deep-dives, migration section, CTA |
| **Alternatives** | "X alternatives," "best tools for Y" | Choose your product from a category | Hero, category overview, tool list (yours first), comparison matrix, CTA |
| **Features** | "feature page for X," "product feature landing" | Understand and adopt a specific feature | Hero, problem statement, feature explanation, use cases, code/demo, CTA |
| **Service** | "QA consulting services," "testing services page" | Request a consultation or engagement | Hero, pain points, service overview, methodology, case studies, CTA |

---

## SECTION 3 — INPUT MODES

### Mode A — User Provides Template
1. Accept the template structure as-is
2. Audit for conversion gaps using the Conversion Gap Checklist (Section 5)
3. Report gaps: "Your template is missing [X]. Recommendation: [Y]."
4. Write conversion-optimized content for each section in the template
5. Do NOT restructure unless user asks — respect the provided template

### Mode B — User Provides Live URL
1. Fetch the URL content
2. Map existing sections to conversion roles (hero, problem, solution, comparison, proof, CTA)
3. Audit conversion effectiveness per section using CRO Scoring (Section 7)
4. Identify: weak sections, missing sections, structural gaps
5. Present findings: "Current page has [X] strengths and [Y] gaps."
6. Options: (a) Rewrite content within existing structure, (b) Suggest structural changes + rewrite, (c) Full rebuild with new structure
7. Wait for user direction

### Mode C — No Template Provided
1. Determine landing page type from keyword and user context
2. Propose a structure based on the type-specific template (Section 4)
3. Present to user for approval: "Here's the recommended structure for a [type] landing page."
4. After approval, write conversion-optimized content per section

---

## SECTION 4 — TYPE-SPECIFIC SECTION TEMPLATES

These are PROPOSED structures for Mode C only. They are NOT imposed on Mode A or B.

### Versus/Comparison Page
```
[HERO] H1 with primary keyword + clear winner statement
[QUICK VERDICT] 2-3 sentence verdict for readers who want the answer fast
[COMPARISON TABLE] Side-by-side feature matrix — honest, data-backed
[FEATURE DEEP-DIVES] 3-5 key differentiators expanded (your tool wins on these)
[HONEST ACKNOWLEDGMENT] Where the competitor does well (builds trust)
[MIGRATION/SWITCH] How to switch from competitor to your tool
[SOCIAL PROOF] Real customer data, testimonials, case studies
[FAQ] Top 5 buyer objections answered
[CTA] Single clear action
```

### Alternatives Page
```
[HERO] H1 with "X alternatives" keyword + value prop
[WHY PEOPLE LOOK] Why users search for alternatives (acknowledge pain)
[TOOL LIST] Your tool first, then competitors — each with honest mini-review
[COMPARISON MATRIX] Feature/pricing/fit comparison table
[DETAILED WINNER] Expanded section on your tool's advantages
[USE CASE MATCHING] "If you need X, choose Y" — honest recommendations
[FAQ] Category questions + objection handling
[CTA] Start free trial / see demo
```

### Features Page
```
[HERO] H1 with feature name + primary benefit
[PROBLEM] The specific pain this feature solves
[SOLUTION] How the feature works — with real screenshots/code
[USE CASES] 3-4 scenarios where this feature shines
[TECHNICAL DEPTH] Code examples, config, real output
[INTEGRATION] How it fits with existing workflow
[FAQ] Technical questions + setup questions
[CTA] Try this feature
```

### Service Page
```
[HERO] H1 with service keyword + outcome promise
[PAIN POINTS] 3-4 problems the target client faces
[SERVICE OVERVIEW] What you deliver, how it works
[METHODOLOGY] Your approach (builds expertise trust)
[CASE STUDIES] Real clients, real results, real numbers
[DIFFERENTIATORS] Why you, not competitors
[FAQ] Engagement questions + pricing/process questions
[CTA] Request consultation / start engagement
```

---

## SECTION 5 — CONVERSION GAP CHECKLIST

When auditing a template (Mode A) or existing page (Mode B), check for these conversion gaps:

| Gap | Impact | Check |
|---|---|---|
| **No clear hero value prop** | High | Does first section clearly state what this is and why it matters? |
| **No quick verdict/TL;DR** | Medium | Can a scanner get the answer in 10 seconds? |
| **Comparison table missing** | High | For vs/alternatives pages — is there a side-by-side? |
| **No honest competitor acknowledgment** | Medium | Does the page acknowledge where competitors are strong? (Builds trust) |
| **Missing social proof** | High | Are there real customer names, real numbers, real results? |
| **No objection handling** | High | Does the FAQ address why someone might NOT choose this? |
| **Weak or hidden CTA** | Critical | Is there 1 clear action? Is it visible without scrolling on mobile? |
| **CTA buried at bottom only** | Medium | CTA should appear 2-3 times — not just at the end |
| **Generic CTA text** | Medium | "Learn more" is weak. "Start free trial" or "See [brand.brand_name] in action" (use brand.primary_cta_text) is strong. |
| **No migration/switch section** | Medium | For vs pages — how does someone actually switch? |
| **No urgency or scarcity** | Low | If applicable — trial limits, pricing changes, capacity |
| **Missing trust signals** | Medium | Security badges, compliance mentions, customer logos |

---

## SECTION 6 — CONVERSION WRITING RULES

### Rule 1: Every Section Has a Conversion Job
If a section doesn't move the reader toward action, cut it. Landing pages have no room for educational tangents. Every paragraph earns its space by: addressing an objection, proving a claim, creating urgency, or building trust.

### Rule 2: Superiority Framing (Honest but Decisive)
- Acknowledge where competitors are strong (builds credibility)
- Then position your tool as the better choice on dimensions that matter to the buyer
- Never make unverifiable claims ("the best tool" without evidence)
- DO make specific, evidence-backed claims using brand.differentiators — always tie to a concrete, measurable result
- The reader should finish thinking "this is the obvious choice for my situation"

### Rule 3: Buttons and CTAs
- Use action-specific language: "Start free trial" not "Learn more"
- Include the value: "Get your first report in 2 minutes" not "Sign up"
- CTA appears 2-3 times across the page — not buried at bottom
- Primary CTA is always the same action (don't confuse with multiple different CTAs)

### Rule 4: Data Over Claims
- "Reduces test debugging time by 67%" (with source) > "Dramatically reduces debugging time"
- Comparison tables use real feature data, not marketing adjectives
- Pricing/capability comparisons are factually accurate — verifiable
- If you don't have data, use social proof: "Used by 50+ QA teams" > "Trusted by many"

### Rule 5: Format Consistency Across Landing Pages
- The section structure stays consistent across pages of the same type
- A "[Brand] vs [Competitor A]" page uses the same section layout as "[Brand] vs [Competitor B]"
- Only the content (headings, claims, comparison data, keyword targeting) changes per page
- This enables programmatic production at scale

### Rule 6: Reading Level
- Landing pages are even simpler than blogs: Grade 6-8 Flesch-Kincaid
- Shorter sentences, shorter paragraphs
- No jargon without immediate explanation
- Bullet points are OK on landing pages (unlike blogs where prose is preferred)

---

## SECTION 7 — CRO SCORING (CONVERSION RATE OPTIMIZATION)

This is the 5th scoring paradigm for landing pages — used alongside SEO/AEO/GEO/SXO.

### CRO Checklist

| # | Check | Points | How to Score |
|---|---|---|---|
| 1 | **Hero hook** — clear value prop in first sentence | 10 | Reader knows what this is and why in 5 seconds |
| 2 | **Problem agitation** — reader sees themselves | 10 | Specific pain points, not generic |
| 3 | **Superiority framing** — clear winner on key dimensions | 15 | Honest comparison where you win on what matters |
| 4 | **Comparison table** — data-backed, your tool wins | 10 | Real features, real data, checkmarks where you lead |
| 5 | **Social proof** — real numbers, real companies | 10 | Named customers, specific metrics, not generic |
| 6 | **Objection handling** — FAQ covers top 5 buyer concerns | 10 | Pricing, switching cost, reliability, support, security |
| 7 | **Single clear CTA** — 1 action, repeated 2-3 times | 10 | Same CTA, multiple placements |
| 8 | **Trust signals** — security, compliance, logos | 10 | SOC 2, customer logos, uptime stats |
| 9 | **Urgency/scarcity** (if applicable) | 5 | Free trial limits, pricing tiers, capacity |
| 10 | **Mobile conversion path** — CTA visible without scrolling | 10 | Mobile-first CTA placement |
| **Total** | | **100** | |

### Grade Scale
Same as SEO scoring: A+ = 95-100 | A = 85-94 | B = 70-84 | C = 50-69 | F = below 50

### Target
Landing pages must achieve CRO 90+ AND SEO/AEO/GEO/SXO 90+ each.

---

## SECTION 8 — SCHEMA RULES BY PAGE TYPE

Landing pages NEVER use `Article` schema. Auto-select based on type:

| Page Type | Primary Schema | Additional Schema |
|---|---|---|
| Versus/Comparison | `WebPage` | `FAQPage` + `Product` (with reviews if available) |
| Alternatives | `WebPage` + `ItemList` | `FAQPage` |
| Features | `SoftwareApplication` | `WebPage` + `FAQPage` |
| Service | `Service` + `Organization` | `FAQPage` |

Pass the correct schema type to `metadata-generator.skill.md` — do not let it default.

---

## SECTION 9 — OUTPUT FORMAT

Landing page deliverable:

```
═══════════════════════════════════════
LANDING PAGE CONTENT
Brand: [brand.brand_name]
Type: [Versus / Alternatives / Features / Service]
Keyword: [primary keyword]
═══════════════════════════════════════

[Full landing page content in markdown — section by section]

═══════════════════════════════════════
CONVERSION AUDIT
═══════════════════════════════════════
CRO Score: [X]/100 [grade]
Conversion gaps found: [list or "None"]
Improvements applied: [list]

═══════════════════════════════════════
SCORING
═══════════════════════════════════════
SEO: [X]/100 | AEO: [X]/100 | GEO: [X]/100 | SXO: [X]/100 | CRO: [X]/100

═══════════════════════════════════════
SCHEMA
═══════════════════════════════════════
[JSON-LD code — correct type for page type]

═══════════════════════════════════════
METADATA
═══════════════════════════════════════
Title: [55-60 chars]
Meta description: [150-160 chars]
Slug: [keyword-rich]
═══════════════════════════════════════
```

---

*Landing Page Engine — v1.0.0 | Nexus SEO Operating System | Conversion Intelligence*
