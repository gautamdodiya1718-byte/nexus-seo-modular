# Nexus Strategy

Standalone strategy and planning skill. Generates SEO strategies, content roadmaps, programmatic SEO setups, AI search positioning strategies, concept ownership analyses, and content portfolio reports with cross-session memory continuity.

Part of the Nexus SEO family. Works completely independently — no other Nexus skill required.

## What it does

- **Full SEO strategy** with 6-month roadmap
- **Topical authority planning** — cluster-by-cluster authority-building
- **Programmatic SEO** — template-based pages at scale with quality gates
- **Content roadmap** — publication schedule structured around content pillars
- **Strategic brief synthesis** — executive summary across all available data
- **AI search positioning** — strategy for AI surfaces (Perplexity, ChatGPT search, AI Overviews, Gemini)
- **Concept ownership** — identify which concept this brand should own
- **Content portfolio status** — list all published content with rank, decay signals, next actions
- **5-layer memory** — project, keyword, content, strategy, controller; bridges sessions

## Inputs

- **Brand profile** (strongly recommended — strategy needs brand context)
- **Topic / focus area** (optional for full strategy; required for some modes)
- **Existing data** (optional — keyword universe, published content list; loaded from memory if present)

## Install

1. Download `nexus-strategy.zip`
2. Unzip
3. Upload to your Claude project
4. Invoke with `/nexus-strategy` or describe what you want in natural language

## Modes

| Trigger phrase | Mode |
|---|---|
| "SEO strategy / 6-month plan / full strategy" | full strategy (DEFAULT) |
| "topical authority / topic cluster strategy" | topical authority |
| "programmatic SEO / pages at scale" | programmatic SEO |
| "content roadmap / publication schedule" | content roadmap |
| "strategic brief / executive summary" | strategic brief |
| "AI search positioning / Perplexity strategy" | AI search positioning |
| "concept ownership / what to own" | concept ownership |
| "content portfolio status / what to update" | content portfolio status |

## Output

Strategy reports with executive briefs, topical authority maps, content roadmaps, AI search positioning recommendations, concept ownership analyses, and 7 prioritized next actions.

## Cross-session memory

5 memory layers persist across sessions:
- **project-memory** — domain, session state, decisions
- **keyword-memory** — full keyword universe + lifecycle state
- **content-memory** — published content + metadata + validation
- **strategy-memory** — roadmaps, strategic briefs, action plans
- **memory-controller** — orchestrates all layers

First session creates memory. Subsequent sessions build on it (within 14 days = automatic continuity; older = offers refresh).

## Version

v1.0.0 — initial release. Standalone, brand-pluggable, 8 modes, AI-search-aware, memory-enabled.
