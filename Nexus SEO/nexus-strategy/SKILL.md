---
name: nexus-strategy
description: >-
  Strategy, planning, and programmatic SEO skill. Generates full SEO strategy with 6-month roadmap, topical authority planning, content roadmaps, programmatic SEO at scale (template-based pages with quality gates), AI search positioning strategy, concept ownership analysis, and content portfolio status reports. Includes 5-layer memory system (project, keyword, content, strategy, memory-controller) for cross-session continuity. Brand profile recommended. Trigger on: SEO strategy, content roadmap, 6-month plan, topical authority, programmatic SEO, template pages, pages at scale, AI search positioning, concept ownership, content portfolio, strategic brief, full strategy.
---

# Nexus Strategy — v1.0.0

You are operating the **Nexus Strategy** skill: a self-contained strategy and planning tool. Generates SEO strategies, content roadmaps, programmatic SEO setups, and content portfolio reports with cross-session memory.

This is one skill of the Nexus SEO family. Works completely standalone — no other Nexus skill required.

**Read the relevant files in `references/` BEFORE executing any step. Never guess — always load.**

---

## ENTRY SEQUENCE

### Step 1 — Brand Profile Detection

Standard pattern. Brand profile **strongly recommended** for strategy work — strategist-ai uses brand.content_pillars, brand.competitors, brand.differentiators heavily.

### Step 2 — Memory Detection

Check for prior session memory in this brand's project:
- If found within 14 days: offer "Continue from prior session" vs "Start fresh"
- If older or absent: start fresh, initialize memory layers

### Step 3 — Mode Detection

| User says | Detected mode |
|---|---|
| "full strategy / 6-month plan / SEO strategy" | `full_strategy` (DEFAULT for ambiguous strategy requests) |
| "topical authority / topic cluster strategy" | `topical_authority` |
| "programmatic SEO / template pages / pages at scale" | `programmatic_seo` |
| "content roadmap / content calendar / publication schedule" | `content_roadmap` |
| "strategic brief / executive summary / synthesize" | `strategic_brief` |
| "AI search strategy / position for AI / Perplexity strategy" | `ai_search_positioning` |
| "concept ownership / what concept should I own" | `concept_ownership` |
| "content portfolio / status of all content / what to update" | `content_portfolio_status` |

### Step 4 — Heads-Up Block

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS STRATEGY — HEADS-UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand:           [active OR brand-agnostic — strongly recommended for strategy]
Memory:         [prior session found from N days ago / none]
Detected mode:   [mode name]
Detection basis: [phrase from prompt]

What I'll do:
  [Pipeline steps for the mode]

Inputs you provided:
  ✓ [list]

Inputs needed but missing:
  ✗ [list — e.g., "Existing keyword data (will run keyword-discovery first if absent)"]

Estimated steps: [N]

Proceed? Reply 'go' / 'change mode' / 'cancel'.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 5 — Pipeline Execution by Mode

**Mode: `full_strategy`** (DEFAULT — comprehensive strategy + roadmap)

Pipeline:
```
[01] memory-controller (load brand's prior session state if exists)
[02] project-memory (initialize project context)
[03] keyword-memory (load existing keyword universe if exists)
[04] strategy-memory (load existing strategic briefs)
[05] authority-engine (current topical authority scores per cluster)
[06] query-graph (topic relationships)
[07] roadmap-engine (6-month roadmap structured around brand.content_pillars)
[08] ai-search-positioning (AI search surface strategy)
[09] concept-ownership (which concept this brand should own)
[10] strategist-ai (executive synthesis + 7 next actions)
[11] memory-controller (WRITE session state)
```

**Mode: `topical_authority`**

Pipeline:
```
[01] authority-engine (current authority per cluster)
[02] query-graph (topic relationships)
[03] roadmap-engine (cluster-by-cluster authority-building plan)
[04] strategist-ai (priorities)
```

**Mode: `programmatic_seo`**

Pipeline:
```
[01] programmatic-seo (template design + keyword matrix + sample pages + quality gates + rollout plan)
[02] strategist-ai (commit / iterate decision)
```

**Mode: `content_roadmap`**

Pipeline:
```
[01] roadmap-engine (publication schedule structured around brand.content_pillars)
[02] content-memory (what's been published / drafted / planned)
[03] strategist-ai (sequencing recommendations)
```

**Mode: `strategic_brief`**

Pipeline:
```
[01] memory-controller (load all relevant memory)
[02] strategist-ai (synthesize across all available data — executive brief format)
```

**Mode: `ai_search_positioning`**

Pipeline:
```
[01] ai-search-positioning (analyze brand's positioning for AI search surfaces)
[02] strategist-ai (action plan — content, schema, attribution improvements)
```

**Mode: `concept_ownership`**

Pipeline:
```
[01] concept-ownership (which concept this brand should own based on content_pillars + competitive gaps)
[02] strategist-ai (execution plan to claim ownership)
```

**Mode: `content_portfolio_status`**

Pipeline:
```
[01] content-memory (load all published content with metadata)
[02] content-portfolio-status (rank, decay signals, audit dates, next actions)
[03] strategist-ai (priority maintenance recommendations)
```

### Step 6 — Output Block + Accountability Log

Standard format.

---

## OUTPUT FORMAT

The full_strategy output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS STRATEGY — FULL REPORT
Brand: [name]
Date range: [generation date — valid for ~30 days]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

EXECUTIVE STRATEGIC BRIEF (from strategist-ai):
  Current state: [summary]
  Strategic priorities (top 3): [list]
  Quarterly objectives: [list]

TOPICAL AUTHORITY MAP:
  Cluster [name]: authority score [X/5] — [status: BUILDING/COMPETITIVE/DOMINANT]
  ...

CONTENT ROADMAP (next 6 months):
  Month 1: [pillar + N cluster posts + N landing pages]
  Month 2: ...
  ...

AI SEARCH POSITIONING:
  Current state: [analysis]
  Strategic actions: [list]

CONCEPT OWNERSHIP RECOMMENDATION:
  Recommended concept to own: [concept name]
  Why this brand can win it: [reasoning]
  Execution plan: [steps]

PROGRAMMATIC SEO OPPORTUNITIES (if applicable):
  Template patterns identified: [list]
  Estimated pages at scale: [N]

7 NEXT ACTIONS (from strategist-ai):
  1. [specific action]
  2. ...
  7. ...

CONFIDENCE: HIGH / MEDIUM / LOW
[/full strategy report]
```

---

## REFERENCE FILE MAP

| File | Used in modes |
|---|---|
| `strategist-ai.skill.md` | all modes (final synthesis) |
| `roadmap-engine.skill.md` | full_strategy, content_roadmap, topical_authority |
| `authority-engine.skill.md` | full_strategy, topical_authority |
| `query-graph.skill.md` | full_strategy, topical_authority |
| `programmatic-seo.skill.md` | programmatic_seo |
| `memory-controller.skill.md` | all modes (session start + end) |
| `project-memory.skill.md` | all modes |
| `strategy-memory.skill.md` | full_strategy, strategic_brief |
| `content-memory.skill.md` | content_roadmap, content_portfolio_status |
| `keyword-memory.skill.md` | full_strategy, topical_authority |
| `ai-search-positioning.skill.md` (NEW) | full_strategy, ai_search_positioning |
| `concept-ownership.skill.md` (NEW) | full_strategy, concept_ownership |
| `content-portfolio-status.skill.md` (NEW) | content_portfolio_status |

---

## EXECUTION RULES

1. **Brand profile strongly recommended.** Strategy needs brand context. Brand-agnostic mode produces generic strategies of limited value.
2. **Memory bridges sessions.** First-time strategy on a brand creates memory. Subsequent sessions build on it.
3. **Heads-up before action.** Standard.
4. **Strategy isn't generation.** This skill plans. Use nexus-content for execution. Use nexus-research for research.
5. **AI search positioning matters.** Modern strategy includes AI surface presence, not just Google ranking.
6. **Concept ownership is a real thing.** Brands that own a concept (own the conversation around X) outperform brands that compete on every keyword.
7. **Portfolio view requires content-memory.** If content-memory is empty, content_portfolio_status mode tells user "no published content tracked yet — add content to memory first."

---

## QUICK-INVOKE EXAMPLES

| User says | What runs |
|---|---|
| "Give me a 6-month SEO strategy" | `full_strategy` mode (DEFAULT) |
| "Plan topical authority for [topic]" | `topical_authority` mode |
| "Generate programmatic pages at scale for [pattern]" | `programmatic_seo` mode |
| "What should I publish next quarter?" | `content_roadmap` mode |
| "Synthesize a strategic brief from existing data" | `strategic_brief` mode |
| "How do I rank in Perplexity / AI Overviews?" | `ai_search_positioning` mode |
| "What concept should my brand own?" | `concept_ownership` mode |
| "Show me status of all my published content" | `content_portfolio_status` mode |

---

*Nexus Strategy — v1.0.0 | Standalone skill | Part of Nexus SEO family | 5-layer memory system, AI-search-aware*
