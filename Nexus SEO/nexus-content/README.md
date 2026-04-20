# Nexus Content

Standalone content creation skill. Generates blog posts, landing pages, guest posts, tutorials, comparison posts, pillar pages, and content refreshes with substance gates (no word count enforcement), live fact verification, live link verification, author voice, user research injection, helpful content compliance, AI citation optimization, JSON-LD schema generation, freshness architecture, and decay mapping.

Part of the Nexus SEO family. Works completely independently — no other Nexus skill required.

## What it does

- **Multi-mode generation** — 7 content types: blog, landing page, guest post, tutorial, comparison, pillar page, refresh
- **Substance gates replace word count** — per-section completeness/density/self-sufficiency check; length emerges from substance
- **Live fact verification** — every numeric/feature/comparison/date claim verified against current web sources
- **Live link verification** — HTTP HEAD on every link before delivery; broken links flagged or auto-replaced
- **Author voice** — when author profile loaded, content is generated in that author's signature voice
- **User research injection** — accepts user-provided documents, data, interviews, proprietary benchmarks; injected as Priority 1 evidence
- **Helpful Content compliance** — Google HCU questions as scored gates with revision instructions
- **Inline LLM checklist** — 9-category quality checks DURING generation, not after
- **Schema generation** — complete JSON-LD blocks (Article, FAQPage, BreadcrumbList, HowTo)
- **Freshness architecture** — separates evergreen spine from time-bound sections
- **Decay mapping** — tags decay-sensitive sections with review triggers (30/60/90/180 days)
- **AI citation optimization** — structures content for AI search surface lift
- **Differentiation enforcement** — ≥3 unique angles vs top 10 SERP required before generation
- **Confidence gate** — hard block on <75% accuracy (stricter for BOFU/YMYL)

## Inputs

- **Topic / target keyword** (required)
- **Research package** (recommended — ideally from `nexus-research`; otherwise minimal SERP analysis runs inline)
- **Brand profile** (recommended — drives voice, internal linking, banned words, CTAs)
- **Author profile** (optional — content generated in this author's voice)
- **User preferences** (optional — personal rules across sessions)
- **User research documents/data** (optional but high-leverage — interview transcripts, benchmarks, proprietary data, client docs)
- **URL** (only for `content_refresh` mode)

## Install

1. Download `nexus-content.zip`
2. Unzip
3. Upload to your Claude project (or install as a skill)
4. Invoke with `/nexus-content` or describe what you want in natural language

## Modes

The skill auto-detects which mode you want and shows a heads-up before running.

| Trigger phrase | Mode |
|---|---|
| "write a blog / write an article" | standard blog (DEFAULT) |
| "landing page / vs page / alternatives" | landing page |
| "guest post / write for [site]" | guest post |
| "tutorial / how to / step-by-step" | tutorial |
| "X vs Y / compare X and Y" | comparison post |
| "pillar page / ultimate guide" | pillar page |
| "refresh / update / improve [URL]" | content refresh |

## Output

Either:
- **Full content delivered** — markdown with metadata, schema blocks, internal links, infographics, callouts, fact-verified claims, link-verified URLs, decay tags
- **Blocked with remediation** — if confidence gate fails, content not delivered; specific issues + fixes provided; user can override with UNRELIABLE warning

## Version

v1.0.0 — initial release. Standalone, brand/author/preferences-pluggable, 7 modes, substance-gated, fact-verified, link-verified, schema-generated.
