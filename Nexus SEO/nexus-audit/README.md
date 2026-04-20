# Nexus Audit

Standalone content analysis and audit skill. Audits any blog post, article, landing page, or pasted content against the full Nexus quality framework — 4-paradigm scoring (SEO/AEO/GEO/SXO), 7-section forensic audit, E-E-A-T enforcement, Helpful Content compliance, differentiation vs competitors, AI search readiness, and live fact verification of every claim.

Part of the Nexus SEO family. Works completely independently — no other Nexus skill required.

## What it does

- **7-section forensic audit** — intent match, depth gap, E-E-A-T, tech SEO, authority gap, AEO/GEO, competitive gaps
- **4-paradigm scoring** — SEO, AEO (AI search), GEO (generative search), SXO (experience)
- **Heading audit + rewrites** — every H2/H3 with verdict and rewrite suggestion using rhetorical patterns
- **E-E-A-T enforcement** — first-person markers, practitioner scenarios, opinion claims with reasoning, primary source citations
- **Helpful Content compliance** — scored against Google's published quality questions
- **Differentiation analysis** — uniqueness vs top-ranking competitors (angles you have, angles you don't)
- **AI citation readiness** — whether AI surfaces (Perplexity, ChatGPT search, AI Overviews) will lift this content cleanly
- **Live fact verification** — every numeric/feature/comparison/date claim verified against current web sources
- **Confidence gate** — if verification confidence <75%, content explicitly flagged as unreliable

## Inputs

- **Content** — URL (must be fetchable) OR pasted content OR file path to .md/.html
- **Target keyword** — what the content is targeting (asks if not provided)
- **Brand profile** (optional) — enriches recommendations
- **Author profile** (optional) — checks voice consistency against author signature
- **Top 3 SERP competitors** (optional) — for competitive comparison

## Install

1. Download `nexus-audit.zip`
2. Unzip
3. Upload to your Claude project (or install as a skill)
4. Invoke with `/nexus-audit` or describe what you want in natural language

## Brand and author profiles (optional)

Both are optional. The audit works brand-agnostic with universal quality standards. To get brand-specific recommendations:
- Create a brand profile from `brand-template.skill.md`
- Optionally create an author profile from `author-template.skill.md`
- Load them into your Claude session before invoking

## Modes

The skill auto-detects which mode you want from your prompt and shows a heads-up before running anything. You can change mode or cancel.

| Trigger phrase | Mode |
|---|---|
| "audit this URL / page / blog" | full audit |
| "quick score / just numbers" | quick score |
| "audit my headings / fix H2s" | heading audit |
| "E-E-A-T / experience signals" | E-E-A-T audit |
| "AEO / GEO / AI search readiness" | AEO/GEO scoring |
| "compare to / vs competitors" | competitor compare |
| "LLM checklist / score against rubric" | LLM checklist scoring |
| "fact check / verify claims" | fact verification only |
| "Helpful Content / Google quality" | helpful content check |

## Output

A complete audit report with:
- Executive summary (5 bullets)
- Scores dashboard (8 dimensions out of 100 each + overall)
- 7+ detailed sections (or fewer in single-mode runs)
- Priority fix order (top 10 issues by impact)
- Confidence percentage
- Specific fix instructions (not generic advice)

## Version

v1.0.0 — initial release. Standalone, brand-agnostic-capable, 9 modes.
