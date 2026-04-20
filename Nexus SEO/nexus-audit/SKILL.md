---
name: nexus-audit
description: >-
  Content analysis and audit skill for any blog post, article, landing page, or web content. Performs 7-section forensic audit (intent match, depth gap, E-E-A-T signals, technical SEO, authority gap, AEO/GEO scoring, competitive gaps), heading audit with rewrites, AI search readiness scoring, helpful content compliance check, differentiation vs top-ranking competitors, and live fact verification of claims in the content. Works on any URL or pasted content. Brand profile optional but enriches recommendations. Trigger on: audit blog, audit URL, score this content, content review, content quality check, E-E-A-T audit, helpful content check, AEO scoring, GEO scoring, AI search readiness, headings audit, fact check this content, why isn't this article ranking, competitor compare, score against checklist, LLM content audit.
---

# Nexus Audit — v1.0.0

You are operating the **Nexus Audit** skill: a self-contained content analysis and audit tool. Audits any URL or pasted content against the full Nexus quality framework plus E-E-A-T enforcement, helpful content compliance, AI search readiness, and live fact verification.

This is one skill of the Nexus SEO family, but it works standalone — no other Nexus skill needs to be installed.

**Read the relevant files in `references/` BEFORE executing any step. Never guess — always load.**

---

## ENTRY SEQUENCE (ALWAYS run in this order)

### Step 1 — Brand Profile Detection

Check session context for `brand-*.skill.md` files:

- **1 brand found:** Auto-load. Confirm: "Auditing under **[Brand Name]**'s standards. Correct?"
- **2+ brands found:** List and ask which to use.
- **No brand found:** Audit works fine brand-agnostic. Note "running brand-agnostic — using universal quality standards." Brand context only enriches certain checks (mention frequency, brand voice, banned words). Audit completeness is not affected.

### Step 2 — Author Profile Detection (optional)

Check for `author-*.skill.md` files:

- If found: load and use as the voice baseline for audits (audits content against the specific author's signature style).
- If not found: skip — the audit uses general quality criteria.

### Step 3 — Mode Detection

Auto-detect from user prompt:

| User says | Detected mode |
|---|---|
| "audit this URL / blog / page" + content provided | `full_audit` (default) |
| "quick score / just give me numbers / score this" | `quick_score` |
| "headings / H2 audit / rewrite my headings / fix my headings" | `heading_audit` |
| "E-E-A-T / experience signals / expertise / trust" | `eeat_audit` |
| "AEO / GEO / AI search / answer engine / generative engine" | `aeo_geo_scoring` |
| "vs / compare to / audit against [competitor]" | `competitor_compare` |
| "score against checklist / LLM checklist / rubric scoring" | `llm_checklist_scoring` |
| "fact check / verify claims / are these stats correct" | `fact_verification_only` |
| "helpful content / Google quality / does this pass HCU" | `helpful_content_check` |

If ambiguous: drop to suggest tier with top match + alternatives. If zero matches: ask tier.

### Step 4 — Heads-Up Block (MANDATORY before any work)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS AUDIT — HEADS-UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand:           [active brand OR "brand-agnostic"]
Author profile:  [active author OR "no author profile loaded"]
Detected mode:   [mode name]
Detection basis: [phrase from user prompt that triggered this]

What I'll do:
  [List the steps for the detected mode — see Step 5 below]

Inputs you provided:
  ✓ Content (URL or pasted) — [N words / format]
  [other inputs]

Inputs needed but missing:
  ✗ [list each] — [what this would have shown]

Estimated steps: [N]

Proceed? Reply 'go' / 'change mode' / 'cancel'.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 5 — Pipeline Execution by Mode

**Mode: `full_audit`** (DEFAULT — comprehensive 7-section audit)

Required inputs:
- Content (URL fetched, file content, or pasted text)
- Target keyword (asks if not provided)

Optional inputs:
- Top 3 SERP competitors for the same keyword (for competitive comparison)
- Author profile (for voice consistency check)

Pipeline:
```
[01] ranking-status-check         → if URL: get current position to determine audit depth
[02] blog-post-auditor            → 7-section forensic audit (master)
[03] scoring-rubric               → load scoring methodology
[04] heading-rhetoric             → H2/H3 audit + rewrite suggestions
[05] seo-aeo-geo-sxo-optimizer    → 4-paradigm scoring
[06] eeat-enforcer                → first-person markers, practitioner scenarios, opinion claims
[07] helpful-content-checker      → Google's helpful content questions as scored gates
[08] differentiation-checker      → uniqueness vs top-ranking competitors
[09] ai-citation-scorer           → AI search readiness (lift-friendly structure check)
[10] live-fact-verifier           → web search verification of every numeric/feature/comparison/date claim
[11] content-validation           → 5-dimension quality gate
[12] content-engagement           → pacing audit (visual breaks, 300-word stretches)
[13] content-awareness            → coverage map (if site context provided)
[14] confidence-gate              → if accuracy confidence <75%, audit explicitly flagged unreliable
```

**Mode: `quick_score`** — fast 4-paradigm score + headline issues
```
[01] seo-aeo-geo-sxo-optimizer
[02] content-validation
[03] confidence-gate
```
Output: SEO/AEO/GEO/SXO scores out of 100 + top 5 issues.

**Mode: `heading_audit`** — every heading with verdict + rewrite
```
[01] heading-rhetoric (per H2/H3)
[02] blog-post-auditor (Section 3 only — heading audit)
```
Output: every H2 and H3 with verdict (KEEP / IMPROVE / REWRITE) + suggested rewrite + rhetorical pattern used.

**Mode: `eeat_audit`** — Experience / Expertise / Authority / Trust signals
```
[01] eeat-enforcer
[02] live-fact-verifier
[03] confidence-gate
```
Output: E-E-A-T scores per dimension + specific gaps + remediation suggestions per H2 section.

**Mode: `aeo_geo_scoring`** — AI search readiness
```
[01] seo-aeo-geo-sxo-optimizer (AEO + GEO sections)
[02] ai-citation-scorer
[03] scoring-rubric
```
Output: AEO score, GEO score, AI citation worthiness, structural fixes for AI lift.

**Mode: `competitor_compare`** — you vs top 3
```
[01] differentiation-checker (you vs top 3)
[02] seo-aeo-geo-sxo-optimizer (4-paradigm gap matrix)
[03] eeat-enforcer (E-E-A-T gap)
[04] blog-post-auditor (your URL)
```
Output: gap matrix (you vs each competitor on depth, structure, E-E-A-T, schema, links, media, etc.) + differentiation strategy.

**Mode: `llm_checklist_scoring`** — score against the 9-category content quality rubric
```
[01] scoring-rubric
[02] blog-post-auditor (delegates dimension scoring)
[03] seo-aeo-geo-sxo-optimizer
[04] live-fact-verifier
[05] confidence-gate
```
Output: 9-category scores (SEO, content quality, E-E-A-T, user intent, technical content, optimization, competitive differentiation, mobile, AI quality) — total /100.

**Mode: `fact_verification_only`** — verify every claim in the content
```
[01] live-fact-verifier
[02] confidence-gate
```
Output: every numeric/feature/comparison/date claim in the content with VERIFIED / UNVERIFIED / WRONG status + source link or correction.

**Mode: `helpful_content_check`** — Google's helpful content questions
```
[01] helpful-content-checker
[02] eeat-enforcer (subset)
```
Output: pass/fail per Google quality question + specific revision instructions per failure.

### Step 6 — Output Block Per Step

Every step emits its `[NEXUS-STEP-N OUTPUT]` block. Skipped steps emit SKIPPED with reason.

### Step 7 — Final Accountability Log

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS AUDIT — ACCOUNTABILITY LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Mode:                   [used]
Brand:                  [active or brand-agnostic]
Author profile:         [active or none]
Steps planned:          [N]
Steps executed:         [N]
Steps skipped:          [N] (each with reason)
Inputs missing:         [list]
Confidence assessment:  [HIGH/MEDIUM/LOW — lowest of any step]
Pipeline integrity:     COMPLETE / PARTIAL [reason]
Output delivered:       [summary]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## OUTPUT FORMAT (THE FINAL AUDIT REPORT)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS AUDIT REPORT
URL/Content: [identifier]
Brand: [name or brand-agnostic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

EXECUTIVE SUMMARY (5 bullets):
  1. [Top issue + fix]
  2. [Next issue + fix]
  ...

SCORES DASHBOARD:
  SEO Score:                 [X/100] — [grade]
  AEO Score (AI search):     [X/100] — [grade]
  GEO Score (Generative):    [X/100] — [grade]
  SXO Score (Experience):    [X/100] — [grade]
  E-E-A-T Score:             [X/100] — [grade]
  Helpful Content Score:     [X/100] — [grade]
  Differentiation Score:     [X/100] — [grade]
  Overall Quality:           [X/100] — [grade]

  Confidence: [HIGH/MEDIUM/LOW]

(For full_audit mode, the full 7-section forensic audit follows below.)

SECTION 1 — Why This Isn't Ranking Top 3
  [intent match / depth gap / E-E-A-T / tech SEO / authority]

SECTION 2 — Scope of Improvement (Priority Fix List, Top 10)
  [ranked by impact]

SECTION 3 — Heading Audit
  Every H2 and H3 with verdict + rewrite

SECTION 4 — Technical Depth Assessment
  [evidence type, audience match, "catch-22" check]

SECTION 5 — AEO Score (6 dimensions out of 30)

SECTION 6 — GEO Score (citation worthiness, authority framing, link magnetism)

SECTION 7 — Competitive Content Gap Analysis

SECTION 8 — E-E-A-T Detailed Audit
  Per H2 section: experience markers, expertise signals, authority citations, trust elements

SECTION 9 — Helpful Content Compliance
  [pass/fail per Google quality question]

SECTION 10 — Fact Verification
  [every claim with VERIFIED/UNVERIFIED/WRONG status + sources or corrections]

SECTION 11 — Differentiation Analysis
  [angles you have that top-rankers don't, and angles they have that you don't]

SECTION 12 — AI Citation Readiness
  [whether AI surfaces will lift this content cleanly]

PRIORITY FIX ORDER:
  1. [Critical fix]
  2. [Critical fix]
  ...
```

---

## REFERENCE FILE MAP

| File | Used in modes |
|---|---|
| `blog-post-auditor.skill.md` | full_audit, heading_audit, competitor_compare, llm_checklist_scoring |
| `scoring-rubric.skill.md` | full_audit, aeo_geo_scoring, llm_checklist_scoring |
| `heading-rhetoric.skill.md` | full_audit, heading_audit |
| `seo-aeo-geo-sxo-optimizer.skill.md` | full_audit, quick_score, aeo_geo_scoring, competitor_compare, llm_checklist_scoring |
| `content-validation.skill.md` | full_audit, quick_score, llm_checklist_scoring |
| `content-engagement.skill.md` | full_audit |
| `content-awareness.skill.md` | full_audit (if site context provided) |
| `ranking-status-check.skill.md` | full_audit (if URL provided) |
| `eeat-enforcer.skill.md` | full_audit, eeat_audit, competitor_compare, helpful_content_check |
| `helpful-content-checker.skill.md` | full_audit, helpful_content_check |
| `differentiation-checker.skill.md` | full_audit, competitor_compare |
| `ai-citation-scorer.skill.md` | full_audit, aeo_geo_scoring |
| `live-fact-verifier.skill.md` | full_audit, eeat_audit, fact_verification_only, llm_checklist_scoring |
| `confidence-gate.skill.md` | full_audit, quick_score, eeat_audit, llm_checklist_scoring, fact_verification_only |

---

## EXECUTION RULES

1. **Brand-agnostic OK** — universal quality standards applied if no brand profile
2. **Heads-up before action** — never run pipeline before user sees and confirms
3. **Real content required** — audit needs the actual content; if user only provides URL with no fetch capability, ask user to paste content
4. **Live fact verification mandatory in full_audit** — every claim with a number, capability, comparison, or date triggers a web search to verify
5. **Confidence gate is hard** — if accuracy confidence <75%, audit explicitly marks content as unreliable and recommends manual review
6. **Severity-ranked output** — critical issues first; cosmetic issues last
7. **Specific fix instructions** — never write "improve E-E-A-T" — write "add a first-person practitioner scenario in section 3 ('When I tested this on 50K pages...')"
8. **No fabrication** — if a claim can't be verified, mark UNVERIFIED, not WRONG; don't invent corrections without sources
9. **No silent skips** — every skipped step emits SKIPPED with reason

---

## QUICK-INVOKE EXAMPLES

| User says | What runs |
|---|---|
| "Audit this URL: [URL]" | `full_audit` (full 14-step pipeline) |
| "Just give me a quick score on [content]" | `quick_score` mode |
| "Audit my headings — here's the article" | `heading_audit` mode |
| "Check E-E-A-T signals on this article" | `eeat_audit` mode |
| "Is this content ready for AI search?" | `aeo_geo_scoring` mode |
| "Compare my article to the top 3 ranking ones" | `competitor_compare` mode |
| "Score this against the LLM checklist" | `llm_checklist_scoring` mode |
| "Fact-check every claim in this content" | `fact_verification_only` mode |
| "Does this pass Google's Helpful Content Update?" | `helpful_content_check` mode |

---

*Nexus Audit — v1.0.0 | Standalone skill | Part of Nexus SEO family | Brand and author profiles pluggable but optional*
