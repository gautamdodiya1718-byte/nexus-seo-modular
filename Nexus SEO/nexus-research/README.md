# Nexus Research

Standalone research and planning skill for SEO content. Discovers keywords, analyzes SERPs, identifies gaps, builds content outlines, generates metadata packages, and produces complete research packages ready to feed any content generator.

Part of the Nexus SEO family. Works completely independently — no other Nexus skill required.

## What it does

- **Keyword research** — 8-dimension keyword universe + clustering + opportunity scoring
- **SERP intelligence** — top 10 analysis, intent classification, difficulty, SERP features
- **Deep competitor analysis** — gap matrix, entity coverage, differentiation angles
- **Content outline (TOC)** — full H1/H2/H3 structure with intent labels and AI-citation-friendly patterns
- **Metadata package** — title (50-60 chars), meta description (140-160 chars), excerpt (180-200 chars), URL slug, schema recommendation
- **Semantic SEO** — topic cluster architecture, entity maps, topical authority planning
- **Live SERP verification** — every SERP claim verified via web search; no SERP fabrication
- **Differentiation enforcement** — guarantees ≥3 unique angles vs top 10 ranking pages
- **AI citation pattern analysis** — structures outlines for AI search surface lift (Perplexity, AI Overviews, ChatGPT search)

## Inputs

- **Topic or keyword** (required)
- **Geo target** (optional, defaults to US)
- **Brand profile** (optional but enriches everything — anchors research to your content pillars, seeds competitor analysis from your competitor list)
- **Content type intent** (optional, defaults to comprehensive guide)

## Install

1. Download `nexus-research.zip`
2. Unzip
3. Upload to your Claude project (or install as a skill)
4. Invoke with `/nexus-research` or describe what you want in natural language

## Modes

The skill auto-detects which mode you want and shows a heads-up before running.

| Trigger phrase | Mode |
|---|---|
| "find keywords for / keyword research" | keyword research |
| "SERP for / analyze SERP / what's ranking for" | SERP analysis |
| "competitor gap / vs top 10 / what am I missing" | competitor gap |
| "outline / TOC / table of contents / structure" | content outline |
| "metadata / meta title / SEO title" | metadata package |
| "topic cluster / entity map / topical authority" | semantic SEO |
| "research [topic] / full research / everything for" | full research package (DEFAULT) |

## Output

Depends on mode. The full research package includes:
- Primary + secondary keywords with volumes
- SERP intelligence (intent, top 10 analysis, PAA)
- Content outline with differentiation angles
- Metadata package
- Entity coverage list
- Internal linking targets (if brand loaded)
- Competitor gap analysis
- AI citation optimization recommendations
- Opportunity scores

The output is structured to feed directly into `nexus-content` for content generation, or to inform manual content creation.

## Version

v1.0.0 — initial release. Standalone, brand-pluggable, 7 modes, live SERP verification.
