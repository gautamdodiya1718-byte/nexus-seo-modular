# SKILL: research/competitor-analysis.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / Competitive Intelligence
# EXECUTION PRIORITY: POST-SERP — Runs after serp-intelligence.skill and deep-serp-analysis.skill are available.
# POSITION: Content strategy intelligence layer between SERP analysis and gap/opportunity scoring.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **content strategy intelligence engine** of the Nexus competitive analysis pipeline.

It does not compare domain authority scores. It does not count backlinks. It does not rank sites by technical metrics. Those analyses belong to `authority-mapping.skill`. This skill performs a fundamentally different and more strategically direct form of analysis: it examines how competitors have structured, covered, and positioned their content — and it identifies precisely where their strategy is weak, outdated, or incomplete in ways that new content can exploit.

The output of this skill is not a ranking of who is strongest. It is a **strategic opportunity map** — a structured assessment of where each competitor dominates, where they fail, and where the total competitive set has collectively left a content gap that new, purposeful content can fill.

### Primary Responsibilities

| Responsibility                      | Description                                                                                                          |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Competitor Identification**       | Identify the 5 most strategically relevant competitors across three competitor types                                 |
| **Coverage Depth Analysis**         | Evaluate each competitor's topic depth, cluster coverage, funnel coverage, and content freshness                     |
| **Content Approach Analysis**       | Analyze each competitor's format strategy, breadth vs. depth approach, and update behavior                          |
| **Strength Mapping**                | Identify specifically what each competitor does well — with evidence, not vague impressions                         |
| **Weakness Detection**              | Find outdated content, missing funnel stages, thin coverage, absent AEO signals, and entity gaps                    |
| **Gap Matrix Construction**         | Build a cross-competitor gap matrix showing which topic clusters are covered, contested, or open                    |
| **Opportunity Identification**      | Highlight under-served topics, weakened competitor positions, and outdated content areas                            |
| **Pattern of Dominance Detection**  | Identify strategic patterns that define who controls which topic areas and why                                      |
| **Strategic Blind Spot Detection**  | Find areas where ALL competitors have systematically failed to provide adequate coverage                            |

### What This Skill Is NOT

- It is **not** a domain authority comparison. No DA/DR scores, no backlink counts, no PageRank proxies.
- It is **not** a technical SEO audit. Page speed, Core Web Vitals, and structured data errors are not its scope.
- It is **not** a traffic estimation tool. Search volume and traffic estimates belong to other systems.
- It is **not** a "who is biggest" ranking. A low-authority site with exceptional content strategy deserves the same analytical attention as a high-authority site with weak content.
- It is **not** a surface observation. Every insight produced by this skill must be traceable to a specific content behavior, a specific gap, or a specific pattern — not to a general impression.

### The Defining Standard of This Skill

> This skill analyzes content strategy — not site strength.
>
> Every strength identified must describe what a competitor does with their content that works.
> Every weakness identified must describe a specific content failure that can be exploited.
> Every gap identified must be a specific topic, cluster, or funnel stage that is not adequately served.
>
> Vague observations — "Competitor A has good content" or "Competitor B is a big site" —
> are prohibited outputs. Every insight must be specific enough to drive a content decision.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Input

| Field                  | Description                                                                    | Required |
|------------------------|--------------------------------------------------------------------------------|----------|
| `target_keyword`       | The keyword or niche for which competitive analysis is being performed         | YES      |

### Optional Inputs

| Field                       | Description                                                              | Used For                                                        |
|-----------------------------|--------------------------------------------------------------------------|-----------------------------------------------------------------|
| `competitor_urls`           | Explicit competitor URLs provided by the user                           | Pre-populating competitor identification (Step 1)               |
| `serp_data`                 | Output from `serp-intelligence.skill` and `deep-serp-analysis.skill`   | Pre-loading ranking pages, intent data, content type patterns   |
| `keyword_clusters`          | Cluster output from `semantic-clustering.skill`                         | Gap matrix construction (Step 6)                               |
| `entity_map`                | Entity output from `entity-extraction.skill`                            | Entity depth weakness detection (Step 5)                        |
| `domain`                    | Target site's domain                                                    | Excluding the user's own site from competitor identification     |
| `existing_content`          | User's existing published articles                                      | Gap detection relative to user's actual position               |

### Input Confidence Framework

| Input State                                                                        | Confidence | Impact on Analysis                                              |
|------------------------------------------------------------------------------------|------------|-----------------------------------------------------------------|
| Full SERP data + keyword clusters + entity map + competitor URLs                   | HIGH       | All 7 steps at full depth; gap matrix fully populated          |
| SERP data + keyword clusters (no entity map, no explicit URLs)                     | MEDIUM-HIGH| Steps 1–5 full; entity weakness in Step 5 is SERP-inferred    |
| SERP data only (no clusters, no entity map, no URLs)                               | MEDIUM     | Steps 1–4 full; Steps 5–7 partially inferred                   |
| Keyword/niche only (no SERP data, no URLs)                                         | LOW        | All steps inference-based; all insights flagged as INFERRED    |

### Input Pre-Conditions

1. **Is `target_keyword` present?** If not → halt. Competitor analysis requires a keyword anchor.
2. **Is SERP data available?** If from `serp-intelligence.skill` → inherit dominant intent, content types, and top-ranking pages. If from `deep-serp-analysis.skill` → inherit per-page dimension data.
3. **Are explicit competitor URLs provided?** If yes → use as primary candidate pool and supplement with SERP inference. If no → derive competitors entirely from SERP data.
4. **Is the user's own domain provided?** If yes → exclude from competitor pool. If no → flag potential self-inclusion risk.
5. **Has a competitor analysis already been run for this keyword?** → Query `memory-controller.skill`. If within freshness threshold (14 days) → offer cached result. If stale → proceed fresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Structured Competitor Intelligence Report

```
NEXUS COMPETITOR ANALYSIS REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Keyword / Niche  : [keyword]
Competitors Analyzed    : [count]
Clusters in Gap Matrix  : [count]
Input Confidence        : [HIGH / MEDIUM-HIGH / MEDIUM / LOW]
SERP Data Loaded        : [YES / NO]
Generated By            : research/competitor-analysis.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — COMPETITOR PROFILES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH COMPETITOR — UP TO 5]

COMPETITOR [N]: [Domain / Site Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Competitor Type     : [SERP / Semantic / Audience]
Primary URL         : [URL of most relevant ranking page for target keyword]
Content Focus       : [what this site primarily covers within the topic space]
Funnel Dominance    : [TOFU / MOFU / BOFU / MIXED]

COVERAGE DEPTH PROFILE:
  Topic Coverage      : [SHALLOW / MEDIUM / DEEP]
  Cluster Coverage    : [which topic clusters they dominate — list]
  Funnel Coverage     : TOFU [STRONG/WEAK/ABSENT] | MOFU [STRONG/WEAK/ABSENT] | BOFU [STRONG/WEAK/ABSENT]
  Content Freshness   : [CURRENT (<6mo) / AGING (6–18mo) / OUTDATED (18mo+) / UNKNOWN]
  Content Volume      : [estimated number of pages/articles covering this topic space]

CONTENT APPROACH PROFILE:
  Primary Format      : [Blog Article / Listicle / Tutorial / Comparison / Landing Page / Mixed]
  Depth Strategy      : [BROAD (many topics, shallow) / DEEP (few topics, exhaustive) / BALANCED]
  Update Behavior     : [ACTIVE (regular updates) / PASSIVE (rare updates) / STATIC (no updates detected)]
  AEO Approach        : [STRONG (FAQ + direct answers) / MODERATE / WEAK / ABSENT]
  Entity Coverage     : [RICH / MODERATE / SPARSE — relative to entity map]

STRENGTH SUMMARY:
  Primary Strength    : [specific, evidence-backed strength statement]
  Supporting Evidence : [which content behavior, pattern, or SERP signal supports this]
  Secondary Strength  : [specific secondary strength — only if genuinely present]
  Cluster Dominance   : [name the clusters where this competitor is hardest to displace]

WEAKNESS SUMMARY:
  Primary Weakness    : [specific, exploitable weakness statement]
  Evidence            : [what content behavior or absence supports this]
  Secondary Weakness  : [specific secondary weakness — only if genuinely present]
  Exploitability      : [HIGH / MEDIUM / LOW — how actionable is this weakness for new content]
  Weakness Type       : [FRESHNESS / DEPTH / FUNNEL GAP / AEO / ENTITY / FORMAT / COVERAGE]

COMPETITIVE THREAT LEVEL:
  Overall Threat      : [HIGH / MEDIUM / LOW]
  Threat Basis        : [what makes this competitor difficult to displace — specific]
  Threat Ceiling      : [where this competitor's dominance ends — where can they be beaten]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[END COMPETITOR [N] — REPEAT FOR ALL 5]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — COMPETITOR COMPARISON TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Dimension                  | Comp A         | Comp B         | Comp C         | Comp D         | Comp E         |
|----------------------------|----------------|----------------|----------------|----------------|----------------|
| Topic Coverage             | [S/M/D]        | [S/M/D]        | [S/M/D]        | [S/M/D]        | [S/M/D]        |
| Funnel: TOFU               | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   |
| Funnel: MOFU               | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   |
| Funnel: BOFU               | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   | [STR/WK/ABS]   |
| Content Freshness          | [C/A/O]        | [C/A/O]        | [C/A/O]        | [C/A/O]        | [C/A/O]        |
| AEO Optimization           | [STR/MOD/WK]   | [STR/MOD/WK]   | [STR/MOD/WK]   | [STR/MOD/WK]   | [STR/MOD/WK]   |
| Entity Coverage            | [R/M/S]        | [R/M/S]        | [R/M/S]        | [R/M/S]        | [R/M/S]        |
| Format Diversity           | [HIGH/MED/LOW] | [HIGH/MED/LOW] | [HIGH/MED/LOW] | [HIGH/MED/LOW] | [HIGH/MED/LOW] |
| Update Behavior            | [ACT/PAS/STAT] | [ACT/PAS/STAT] | [ACT/PAS/STAT] | [ACT/PAS/STAT] | [ACT/PAS/STAT] |
| Overall Threat             | [H/M/L]        | [H/M/L]        | [H/M/L]        | [H/M/L]        | [H/M/L]        |

Legend: S=Shallow, M=Medium, D=Deep | STR=Strong, WK=Weak, ABS=Absent | C=Current, A=Aging, O=Outdated
        R=Rich, M=Moderate, S=Sparse | ACT=Active, PAS=Passive, STAT=Static | H=High, M=Medium, L=Low

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — GAP MATRIX
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TOPIC CLUSTER GAP MATRIX

| Cluster / Topic Area       | Comp A    | Comp B    | Comp C    | Comp D    | Comp E    | Gap Status    | Priority   |
|----------------------------|-----------|-----------|-----------|-----------|-----------|---------------|------------|
| [Cluster 1 Name]           | COVERED   | COVERED   | COVERED   | COVERED   | COVERED   | CONTESTED     | LOW        |
| [Cluster 2 Name]           | COVERED   | COVERED   | WEAK      | ABSENT    | ABSENT    | PARTIAL       | MEDIUM     |
| [Cluster 3 Name]           | ABSENT    | WEAK      | ABSENT    | ABSENT    | ABSENT    | GAP           | HIGH       |
| [Cluster 4 Name]           | WEAK      | ABSENT    | ABSENT    | ABSENT    | ABSENT    | GAP           | CRITICAL   |
| [Cluster 5 Name]           | COVERED   | ABSENT    | COVERED   | WEAK      | ABSENT    | CONTESTED     | MEDIUM     |
| [Cluster N Name]           | ...       | ...       | ...       | ...       | ...       | ...           | ...        |

FUNNEL GAP MATRIX

| Funnel Stage / Cluster     | Comp A    | Comp B    | Comp C    | Comp D    | Comp E    | Funnel Gap    | Impact     |
|----------------------------|-----------|-----------|-----------|-----------|-----------|---------------|------------|
| TOFU — [Cluster Name]      | COVERED   | COVERED   | ABSENT    | ABSENT    | ABSENT    | PARTIAL       | MEDIUM     |
| MOFU — [Cluster Name]      | ABSENT    | ABSENT    | ABSENT    | ABSENT    | ABSENT    | FULL GAP      | CRITICAL   |
| BOFU — [Cluster Name]      | COVERED   | WEAK      | COVERED   | ABSENT    | ABSENT    | CONTESTED     | MEDIUM     |

GAP STATUS LEGEND:
  COVERED   → Competitor has strong, current, comprehensive content on this cluster
  WEAK      → Competitor has thin, outdated, or narrow content on this cluster
  ABSENT    → Competitor has no content targeting this cluster
  PARTIAL   → Some competitors covered, others not — opportunity exists in the gap
  CONTESTED → Most competitors have strong coverage — difficult to enter without differentiation
  GAP       → No or minimal coverage across all competitors — open opportunity
  FULL GAP  → Zero adequate coverage across all competitors — highest-priority opportunity

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — OPPORTUNITY INTELLIGENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITICAL GAPS (0–1 competitors with adequate coverage):
  Gap 1: [Topic / Cluster / Funnel Stage]
    Coverage state   : [description of what exists vs. what's needed]
    Exploitability   : [why this gap is actionable right now]
    Target approach  : [what type of content captures this gap]
    Urgency          : [HIGH / MEDIUM — why now]

  Gap 2: [Topic / Cluster / Funnel Stage]
    [same structure]

CONTESTED OPPORTUNITIES (2–3 competitors with weak/aging coverage):
  Opportunity 1: [Topic / Cluster]
    Weakest competitor : [name + what makes their coverage weak]
    Attack angle       : [what to do differently to displace them]
    Content advantage  : [specific content approach that outperforms what exists]

  Opportunity 2: [Topic / Cluster]
    [same structure]

FRESHNESS OPPORTUNITIES (areas where competitor content is outdated):
  Freshness Gap 1: [Topic]
    Aging competitors : [which competitors have outdated content here]
    Content age       : [estimated staleness]
    Update opportunity: [what a fresh, updated treatment covers that old content cannot]

  Freshness Gap 2: [Topic]
    [same structure]

UNDER-SERVED AUDIENCE SEGMENTS:
  Segment 1: [Audience segment description]
    Current coverage  : [how poorly this audience is served by existing content]
    Capture approach  : [how new content specifically addresses this audience]

  Segment 2: [Audience segment]
    [same structure]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — STRATEGIC INSIGHTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DOMINANT PATTERN ANALYSIS:
  Who dominates TOFU      : [competitor + why — specific content behavior]
  Who dominates MOFU      : [competitor + why]
  Who dominates BOFU      : [competitor + why]
  Who dominates entity depth: [competitor + why]
  Who dominates freshness : [competitor + why]

STRATEGIC BLIND SPOTS (ALL competitors fail here):
  Blind Spot 1: [description — what no competitor adequately addresses]
    Evidence    : [how many competitors miss this and in what way]
    Opportunity : [what content capturing this blind spot would look like]

  Blind Spot 2: [description]
    [same structure]

WEAKEST COMPETITOR (easiest to displace):
  Competitor    : [name]
  Why weakest   : [specific evidence — what content failures make them vulnerable]
  Best attack angle: [what content strategy most directly displaces them]

STRONGEST COMPETITOR (hardest to displace):
  Competitor    : [name]
  Why strongest : [specific content evidence — what they do that is difficult to replicate]
  How to compete: [what angle gives new content a chance despite their strength]

RECOMMENDED ENTRY SEQUENCE:
  Phase 1 (Quick wins)    : [cluster/topic + approach + why it's achievable now]
  Phase 2 (Build authority): [cluster/topic + approach + what Phase 1 enables]
  Phase 3 (Contest dominance): [cluster/topic + approach + what Phase 2 enables]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into              : gap-opportunity-engine.skill
                          authority-mapping.skill
                          roadmap-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — COMPETITOR IDENTIFICATION

Identify the 5 most strategically relevant competitors for the target keyword. Not all competitors are equal — this step distinguishes between three types of competitive threat and selects the most analytically valuable set.

**Phase 1A — Competitor Type Definitions**

**Type 1 — SERP Competitors:**
Sites whose pages rank in the top-10 organic results for the target keyword. These are the most direct competitors — they are literally the content pieces the new content must displace.

- Source: Top-10 organic results from `serp-intelligence.skill` or `deep-serp-analysis.skill`.
- Priority: HIGHEST — always include if available.
- Count: Minimum 2–3 SERP competitors in the final 5.

**Type 2 — Semantic Competitors:**
Sites that do not rank for the specific target keyword but have comprehensive, high-quality content coverage of the surrounding topic space — the topic cluster and related clusters. These competitors represent the broader topical authority landscape a new site must navigate.

- Source: Identified from keyword universe Dimension 7 (Semantic Expansion) and cluster analysis.
- Priority: MEDIUM — include if SERP competitor count is below 5.
- Identifying signal: These sites rank for multiple keywords in the same cluster but not the specific target keyword.

**Type 3 — Audience Competitors:**
Sites that serve the same target audience and funnel stage as the new content, even if they cover slightly different topics. An audience competitor may not rank for the target keyword but competes for the same reader's attention and trust.

- Source: Identified from audience signals in the keyword's niche and the content type landscape.
- Priority: LOWER — include only if filling the 5-slot after SERP and semantic competitors are selected.
- Identifying signal: Same audience + same funnel stage + different but adjacent topic.

**Phase 1B — Competitor Candidate Pool Construction**

Build the candidate pool:

1. Start with all top-10 organic SERP results as Type 1 candidates.
2. Exclude the user's own domain if provided.
3. Exclude result types that are not content competitors (e-commerce product listings, Wikipedia, government sites, tool homepages without editorial content).
4. Add Type 2 candidates from semantic cluster analysis if SERP candidates number fewer than 5.
5. Add Type 3 candidates only if the pool still needs filling.

**Phase 1C — Final 5 Selection**

From the candidate pool, select exactly 5 competitors using this priority ranking:

| Priority | Selection Criterion                                                                    |
|----------|----------------------------------------------------------------------------------------|
| 1        | SERP competitor at position #1–3 (highest SERP authority for this keyword)            |
| 2        | SERP competitor with the most different content approach from Priority 1               |
| 3        | SERP competitor with the most exploitable weakness signal                              |
| 4        | Semantic competitor with the broadest cluster coverage of the topic space             |
| 5        | The competitor (SERP, semantic, or audience) with the most strategically distinct profile |

**Tie-Breaking Rule:** When two competitors are equally strong candidates for the same slot → prefer the one whose weaknesses are more clearly exploitable. Analytical value is more important than competitive strength.

**Phase 1D — Competitor Profile Initialization**

For each of the 5 selected competitors, initialize a competitor record:
- Domain / site name.
- Competitor type (SERP / Semantic / Audience).
- Primary URL (the most relevant ranking page for the target keyword, or the site's topic hub page for semantic competitors).
- Content focus (what this site primarily covers within the topic space).

---

### ▸ STEP 2 — COVERAGE DEPTH ANALYSIS

For each of the 5 competitors, evaluate the depth, breadth, and completeness of their content coverage across the topic space.

**Phase 2A — Topic Coverage Depth Assessment**

Evaluate how deeply each competitor covers the target topic:

| Coverage Level | Definition                                                                              | Identifying Signals                                                   |
|----------------|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| **SHALLOW**    | Surface-level treatment — key concept mentioned but not meaningfully explained          | Short content, few headings, no examples, no data, minimal structure |
| **MEDIUM**     | Adequate treatment — key concepts explained with some supporting detail                 | Moderate length, some examples, basic structure, limited depth       |
| **DEEP**       | Comprehensive treatment — thorough coverage with evidence, examples, and sub-topic depth | Long-form, rich structure, original examples, data, full entity coverage |

**Phase 2B — Cluster Coverage Mapping**

Using the keyword clusters from `semantic-clustering.skill` (if available) or the topic's known sub-topic structure:

For each competitor, identify:
1. Which clusters they have content targeting.
2. For each covered cluster — what depth level (SHALLOW / MEDIUM / DEEP).
3. Which clusters they have completely absent coverage for.

**Cluster Dominance Classification:**

| Status    | Definition                                                                          |
|-----------|-------------------------------------------------------------------------------------|
| DOMINATES | Has deep, comprehensive, current content covering this cluster                      |
| COVERS    | Has adequate but not exceptional content in this cluster                            |
| WEAK      | Has thin, outdated, or narrow content attempting to cover this cluster              |
| ABSENT    | Has no meaningful content targeting this cluster                                    |

**Phase 2C — Funnel Coverage Assessment**

For each competitor, evaluate their coverage at each funnel stage:

| Funnel Stage | Coverage Signal                                                                        |
|--------------|----------------------------------------------------------------------------------------|
| TOFU         | Informational content, educational guides, what-is articles, how-to content            |
| MOFU         | Comparison articles, evaluation guides, best-of lists, review content                 |
| BOFU         | Landing pages, product pages, trial pages, pricing comparisons, case studies           |

**Funnel Coverage Rating per stage:**

| Rating   | Definition                                                             |
|----------|------------------------------------------------------------------------|
| STRONG   | Multiple pieces of content at this stage; comprehensive and current   |
| WEAK     | 1–2 pieces of content at this stage; thin or outdated                 |
| ABSENT   | No content detected at this funnel stage                              |

**Phase 2D — Content Freshness Assessment**

Evaluate the age and update status of each competitor's content:

| Freshness Label | Date Signal                                                          |
|-----------------|----------------------------------------------------------------------|
| CURRENT         | Content published or updated within the last 6 months               |
| AGING           | Content published 6–18 months ago with no visible update            |
| OUTDATED        | Content published 18+ months ago with no update signal              |
| UNKNOWN         | No date signal detectable in available data                         |

**Freshness Assessment Method:**
- Primary: Use `last_modified` or `published` dates from page metadata if available from `deep-serp-analysis.skill`.
- Secondary: Infer from date-sensitive content signals (references to specific years, outdated tool versions, obsolete data).
- Tertiary: Check if the result's SERP snippet includes a date.

**Phase 2E — Content Volume Estimation**

Estimate how many pages/articles each competitor has on the target topic space:
- Use site:domain queries conceptually (where data is available).
- Cross-reference with cluster coverage — a competitor that covers 5 clusters likely has 10–30 topic-related pages.
- Classify: SPARSE (1–5 pages) / MODERATE (5–20 pages) / EXTENSIVE (20+ pages).

---

### ▸ STEP 3 — CONTENT APPROACH ANALYSIS

Analyze HOW each competitor has approached their content strategy — not just what they cover, but how they structure, format, and maintain it.

**Phase 3A — Primary Format Assessment**

Identify the dominant content format each competitor uses for this topic space:

| Format Type         | Description                                                             |
|---------------------|-------------------------------------------------------------------------|
| Blog Article        | Standard editorial content with prose structure                        |
| Listicle            | Number-led, list-first organization                                    |
| Tutorial            | Step-by-step instructional content                                     |
| Comparison          | Side-by-side evaluation structure                                      |
| Landing Page        | Conversion-focused, minimal navigation                                 |
| Mixed               | Uses multiple formats across different pieces                          |

**Phase 3B — Depth Strategy Assessment**

Evaluate whether the competitor favors breadth (covering many topics at moderate depth) or depth (covering few topics exhaustively):

| Strategy Type | Definition                                                                                  | Signal                                                             |
|---------------|---------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| BROAD         | Many articles covering the topic space; each at moderate depth; wide cluster coverage      | High volume, medium depth per page, many cluster claims           |
| DEEP          | Few articles; each comprehensive and exhaustive; dominate the clusters they target         | Low volume, very high depth per page, strong entity coverage      |
| BALANCED      | Moderate volume with consistent depth; neither wide-and-shallow nor narrow-and-exhaustive  | Medium volume, consistent depth level across pieces               |

**Phase 3C — Update Behavior Assessment**

Evaluate how actively each competitor maintains their content:

| Behavior     | Definition                                                                     | Detecting Signal                                               |
|--------------|--------------------------------------------------------------------------------|----------------------------------------------------------------|
| ACTIVE       | Regular content updates, new articles published frequently, clear editorial cadence | Recent publication dates, "updated" markers, current data references |
| PASSIVE      | Occasional updates; some content refreshed but not systematically             | Mixed dates, some current references, some outdated sections  |
| STATIC       | No detected update behavior; content appears as originally published           | Old dates, outdated tool references, no recency markers       |

**Phase 3D — AEO Approach Assessment**

Evaluate how deliberately each competitor targets answer engine optimization:

| AEO Level | Definition                                                                             |
|-----------|----------------------------------------------------------------------------------------|
| STRONG    | Consistent direct answers in intros, FAQ sections, structured question-answer format, schema markup signals |
| MODERATE  | Some FAQ elements or direct answers but not systematically applied across content      |
| WEAK      | Minimal AEO optimization; content is primarily prose without AEO structure            |
| ABSENT    | No detectable AEO optimization; no FAQ sections, no direct answers, no structured answers |

**Phase 3E — Entity Coverage Assessment**

Using the entity map from `entity-extraction.skill` (if available), evaluate how well each competitor covers the topic's critical entities:

| Coverage Level | Definition                                                               |
|----------------|--------------------------------------------------------------------------|
| RICH           | Most or all Critical and Important entities are present and well-covered |
| MODERATE       | Critical entities present; Important entities partially covered          |
| SPARSE         | Only the most obvious entities covered; significant entity gaps          |

If entity map is not available → infer from semantic keyword signals and heading structure from `deep-serp-analysis.skill` data.

---

### ▸ STEP 4 — STRENGTH MAPPING

For each competitor, identify and precisely articulate their specific content strengths. Every strength statement must be specific, evidence-backed, and actionable.

**Phase 4A — Strength Identification Protocol**

For each competitor, evaluate these specific strength dimensions:

**Dimension 1 — Structural Strength:**
Does this competitor consistently produce well-structured, navigable content that guides the reader through complex topics?
- Evidence: TOC presence, H2/H3 depth from SERP analysis, logical section flow.

**Dimension 2 — Evidence Quality Strength:**
Does this competitor back their claims with data, case studies, original examples, or expert citations?
- Evidence: Quality signals from `deep-serp-analysis.skill` Dimension 6 (examples, stats, quotes, depth level).

**Dimension 3 — Comprehensiveness Strength:**
Does this competitor cover the topic more completely than alternatives — fewer gaps, more sub-topics, better entity coverage?
- Evidence: Word count range, H2/H3 count, cluster coverage breadth.

**Dimension 4 — Freshness Strength:**
Does this competitor consistently produce and update current content in a way that others do not?
- Evidence: Publication and update dates, current data references.

**Dimension 5 — Format Execution Strength:**
Does this competitor use their chosen format more effectively than alternatives — better listicles, clearer tutorials, more useful comparisons?
- Evidence: Media richness, element density, user experience signals.

**Dimension 6 — Cluster Dominance Strength:**
Does this competitor so thoroughly cover one or more clusters that their position there is nearly unassailable?
- Evidence: Multiple deep pieces covering the same cluster from different angles.

**Dimension 7 — AEO Strength:**
Does this competitor win featured snippets, PAA positions, or own the answer engine landscape for this topic?
- Evidence: AEO signals from `deep-serp-analysis.skill` Dimension 5.

**Phase 4B — Primary and Secondary Strength Selection**

For each competitor:
1. Evaluate all 7 strength dimensions.
2. Select the 1 dimension where the competitor is strongest → label as Primary Strength.
3. If a second genuine strength exists → label as Secondary Strength.
4. Do NOT manufacture a secondary strength if the competitor is one-dimensionally strong.

**Phase 4C — Cluster Dominance Identification**

For each competitor, identify the specific clusters where their position is strongest — where new content would face the most resistance:

```
Cluster Dominance Record:
  Competitor   : [name]
  Dominant Cluster: [cluster name]
  Dominance Evidence: [what content behaviors make this cluster difficult to enter against them]
  Dominance Ceiling: [where their dominance ends — what sub-angle or audience they don't serve]
```

---

### ▸ STEP 5 — WEAKNESS DETECTION

For each competitor, identify and precisely articulate their specific content weaknesses. Every weakness must be a specific, exploitable content failure — not a vague observation about site quality.

**Phase 5A — Weakness Category Taxonomy**

| Weakness Type    | Definition                                                                                    | Exploitability |
|------------------|-----------------------------------------------------------------------------------------------|----------------|
| **FRESHNESS**    | Content is outdated — references old tools, old data, old approaches that have been superseded | HIGH — a fresh treatment immediately has a quality advantage |
| **DEPTH**        | Content is thin — the topic is mentioned but not meaningfully covered                        | HIGH — a comprehensive treatment immediately outperforms |
| **FUNNEL GAP**   | Missing entire funnel stages — TOFU-only, BOFU-only, or no MOFU                             | HIGH — content filling the missing stage has no competition |
| **AEO**          | No featured snippet optimization, no FAQ structure, no direct answers                        | MEDIUM-HIGH — AEO-optimized content competes for snippet position |
| **ENTITY**       | Missing critical entities that belong to the topic's semantic field                          | MEDIUM — richer entity coverage signals deeper expertise |
| **FORMAT**       | Using a suboptimal format for the intent — e.g., prose for a step-by-step process           | MEDIUM — better format execution improves user experience and signals |
| **COVERAGE**     | Missing entire topic clusters that the audience needs                                         | HIGH — uncovered clusters are open territory |
| **EVIDENCE**     | Claims without data, examples without context, assertions without proof                      | MEDIUM — evidence-backed content earns more trust and authority |
| **AUDIENCE**     | Targeting the wrong audience level — too broad for experts, too technical for beginners      | MEDIUM — audience-aligned content serves readers better |

**Phase 5B — Per-Competitor Weakness Assessment**

For each competitor, evaluate every weakness category:

1. **Freshness:** Is any of their content AGING or OUTDATED? Which specific pieces or topics?
2. **Depth:** Is any cluster or topic area covered at SHALLOW depth when the audience needs DEEP?
3. **Funnel Gap:** Which funnel stages are WEAK or ABSENT for this competitor?
4. **AEO:** Is their AEO approach WEAK or ABSENT? Which specific question formats are they missing?
5. **Entity:** Are Critical entities from the entity map missing from their content? Which ones?
6. **Format:** Is their chosen format misaligned with the SERP's dominant intent or audience expectation?
7. **Coverage:** Which topic clusters are ABSENT or WEAK in their coverage?
8. **Evidence:** Are their claims unsourced, their examples absent, or their data outdated?
9. **Audience:** Is their content miscalibrated for the audience the keyword implies?

**Phase 5C — Weakness Exploitability Scoring**

For each identified weakness, score its exploitability:

| Exploitability | Condition                                                                                  |
|----------------|--------------------------------------------------------------------------------------------|
| HIGH           | The weakness creates a content gap that new content can directly fill; easy to execute    |
| MEDIUM         | The weakness exists but requires moderate effort to exploit; competitive benefit is clear |
| LOW            | The weakness is minor or addressing it requires significant resources relative to benefit |

**Phase 5D — Primary Weakness Selection**

For each competitor, from all identified weaknesses:
1. Select the Primary Weakness — the most exploitable, highest-impact content failure.
2. Select a Secondary Weakness only if it is genuinely distinct and actionable.
3. If a competitor has no meaningful weaknesses → record: "No significant exploitable weakness detected. Competitor is strong across all dimensions. Focus on differentiation rather than gap-filling for this competitor."

---

### ▸ STEP 6 — GAP MATRIX CONSTRUCTION

Build the cross-competitor gap matrices that reveal which topic clusters and funnel stages are adequately covered and which represent open opportunities.

**Phase 6A — Topic Cluster Gap Matrix**

Using the topic clusters from `semantic-clustering.skill` (or inferred from keyword universe if clustering data is unavailable):

For each cluster × competitor combination, assign a status:

| Status     | Definition                                                                               |
|------------|------------------------------------------------------------------------------------------|
| COVERED    | Competitor has strong, current content covering this cluster's core topics              |
| WEAK       | Competitor has attempted coverage but it is thin, outdated, or incomplete               |
| ABSENT     | Competitor has no content targeting this cluster                                         |

After all competitor-cluster combinations are populated, derive the Gap Status for each cluster:

| Gap Status  | Derivation Rule                                                                                     |
|-------------|------------------------------------------------------------------------------------------------------|
| CONTESTED   | ≥4 of 5 competitors have COVERED status for this cluster                                           |
| PARTIAL     | 2–3 competitors have COVERED; remaining have WEAK or ABSENT                                        |
| GAP         | ≤1 competitor has COVERED; 2–4 have WEAK or ABSENT                                                |
| FULL GAP    | 0 competitors have COVERED; all have WEAK or ABSENT                                                |

**Gap Priority Assignment:**

| Gap Status + Cluster Importance       | Priority Assignment  |
|---------------------------------------|----------------------|
| FULL GAP + Critical cluster           | CRITICAL             |
| GAP + Critical or Important cluster   | HIGH                 |
| PARTIAL + Critical cluster            | HIGH                 |
| GAP + Optional cluster                | MEDIUM               |
| PARTIAL + Important cluster           | MEDIUM               |
| CONTESTED + any cluster               | LOW (differentiation required) |

**Phase 6B — Funnel Stage Gap Matrix**

Build a second matrix crossing funnel stages against topic clusters:

For each Funnel Stage × Cluster combination:
- Aggregate the funnel coverage ratings across all 5 competitors for that cluster.
- Derive the Funnel Gap Status:

| Funnel Gap Status | Derivation Rule                                                           |
|-------------------|---------------------------------------------------------------------------|
| COVERED           | ≥3 competitors have STRONG coverage at this funnel stage for this cluster |
| PARTIAL           | 1–2 competitors have STRONG; others WEAK or ABSENT                       |
| FULL GAP          | 0 competitors have STRONG coverage at this funnel stage for this cluster  |

**Phase 6C — Gap Matrix Priority Synthesis**

After both matrices are complete:
1. Cross-reference the Topic Cluster gap status with the Funnel Stage gap status.
2. A cluster with a GAP status AND a FULL GAP at BOFU level = highest priority opportunity (captures topical gap AND conversion opportunity simultaneously).
3. A cluster with CONTESTED status at TOFU but a FULL GAP at MOFU = medium opportunity (enter at MOFU to avoid TOFU competition).
4. Document the top 3 highest-priority gaps for forwarding to `gap-opportunity-engine.skill`.

---

### ▸ STEP 7 — OPPORTUNITY IDENTIFICATION

Synthesize all competitor data, weakness findings, and gap matrix results into a structured opportunity intelligence report.

**Phase 7A — Critical Gap Opportunities**

Identify topics, clusters, or funnel stages where ≤1 competitor has adequate coverage:

For each critical gap:
1. Describe the coverage state precisely — what exists and what is needed.
2. Explain why this gap is exploitable now — is it a growing topic, a format innovation opportunity, a freshness gap?
3. Recommend the content approach most likely to capture this gap.
4. Assign urgency (HIGH / MEDIUM) based on whether competitors are likely to address this gap soon.

**Phase 7B — Contested Opportunities**

Identify clusters where 2–3 competitors have weak or aging coverage — not fully open, but not firmly closed:

For each contested opportunity:
1. Identify which competitors are weakest in this area.
2. Define the specific attack angle — what approach gives new content the best chance of displacing the weak competitor.
3. Describe the content advantage that makes the attack viable.

**Phase 7C — Freshness Opportunities**

Identify areas where existing competitor content is AGING or OUTDATED:

For each freshness opportunity:
1. Name which competitors have aging content in this area.
2. Estimate the content's age (months).
3. Describe specifically what has changed since the content was published — what new data, tools, methods, or approaches make the old content incomplete.
4. Describe the update advantage — what a fresh treatment covers that the aged content cannot.

**Phase 7D — Under-Served Audience Segments**

Identify specific audience segments that are not well-served by any competitor's existing content:

Detection method:
- Cross-reference the topic's known audience (from keyword signals and cluster context) against the actual audience targeting of competitor content.
- If competitor content consistently targets Intermediate audiences but the keyword universe shows Beginner and Expert demand → these segments are under-served.

For each under-served segment:
1. Describe the segment specifically (who they are, what they need).
2. Explain how current competitor content fails them.
3. Describe what content specifically designed for this segment would look like.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CROSS-COMPETITOR PATTERN OF DOMINANCE DETECTION

Beyond individual competitor profiles, identify patterns that explain WHO dominates WHICH areas and WHY — and what those patterns reveal about the competitive landscape's structure.

**Pattern Detection Protocol:**

1. After all 5 competitor profiles are complete, scan across all profiles for consistent patterns:

| Pattern Type                    | Description                                                                              |
|---------------------------------|------------------------------------------------------------------------------------------|
| **Funnel Monopoly**             | One competitor dominates an entire funnel stage across multiple clusters                |
| **Format Lock**                 | One format type (e.g., listicle) dominates so heavily that alternative formats have no SERP presence |
| **Freshness Leader**            | One competitor updates more actively than all others — making freshness competition irrelevant against them |
| **Cluster Specialist**          | One competitor so deeply covers one cluster that entry there is extremely high-cost     |
| **AEO Absent**                  | No competitor has strong AEO optimization — the entire competitive set is vulnerable to AEO-optimized content |
| **Entity Uniformity**           | All competitors have the same entity gaps — missing the same concepts from the semantic field |
| **Audience Mismatch**           | All competitors target the same audience level, leaving other audience levels unserved  |

2. For each detected pattern → document:
   - Which competitors exhibit it.
   - What it reveals about the competitive landscape.
   - How new content can exploit the pattern.

---

### ▸ STRATEGIC BLIND SPOT DETECTION

A strategic blind spot is an area where ALL competitors have systematically failed — not a random gap, but a consistent, collective failure of content strategy across the entire competitive set.

**Blind Spot Identification Method:**

1. Identify any weakness type (from Step 5's taxonomy) that appears across ALL 5 competitors.
2. If ≥4 of 5 competitors share the same weakness type → that is a collective blind spot.
3. Analyze why: Is it an industry convention? A format convention? A topic that's assumed but not covered? A new development that predates current content?

**Blind Spot Strategic Value:**

Blind spots are the highest-value opportunities because:
- New content addressing a blind spot has ZERO competition.
- Addressing a blind spot creates a unique resource position that is difficult to replicate quickly.
- Blind spots often become featured snippet targets because no existing content answers the implicit question.

**Blind Spot Reporting Format:**

```
Strategic Blind Spot: [Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Description      : [what no competitor adequately addresses]
Evidence         : [how many competitors miss this and what their failure looks like]
Root Cause       : [why the competitive set has missed this — convention, recency, difficulty]
Opportunity      : [what content capturing this blind spot would look like]
Strategic Impact : [what owning this blind spot provides in terms of authority and traffic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ COMPETITIVE THREAT CALIBRATION

Assign each competitor a Competitive Threat Level (HIGH / MEDIUM / LOW) with a specific basis and ceiling.

**Threat Level Definitions:**

| Threat Level | Definition                                                                                      |
|--------------|-------------------------------------------------------------------------------------------------|
| HIGH         | Competitor is strong across multiple strength dimensions; their content is current, deep, and well-structured; displacing them requires significant content investment |
| MEDIUM       | Competitor has strengths in some areas but clear exploitable weaknesses; their content is beatable with targeted, superior content |
| LOW          | Competitor has significant, exploitable weaknesses; their content is thin, outdated, or narrowly focused; new content can immediately outperform |

**Threat Ceiling Concept:**

Every competitor — even HIGH threat competitors — has a "threat ceiling" — the boundary where their strength ends and their vulnerability begins. The threat ceiling is the specific topic area, funnel stage, or audience segment where their dominance does not extend.

Recording the threat ceiling is as important as recording the threat level — it identifies where competition is viable.

---

### ▸ RECOMMENDED ENTRY SEQUENCE

Based on the gap matrix, opportunity intelligence, and competitor threat levels, synthesize a 3-phase recommended entry sequence:

**Phase 1 — Quick Wins (0–3 months):**
- Target FULL GAP or CRITICAL GAP clusters.
- Target LOW-threat competitors' weakest areas.
- Target freshness gaps where current content is OUTDATED.
- These are immediate content opportunities where new content can rank with moderate investment.

**Phase 2 — Authority Building (3–9 months):**
- Target PARTIAL gaps where 1–2 competitors are WEAK.
- Target the blind spots detected in the advanced logic step.
- Target under-served audience segments.
- These build topical authority and enable Phase 3.

**Phase 3 — Contest Dominance (9–18 months):**
- Target CONTESTED clusters where Phase 1 and Phase 2 content has built enough authority to compete.
- Target HIGH-threat competitors at their threat ceiling — the specific areas where even strong competitors are vulnerable.
- These are long-term authority-building plays that become viable only after Phases 1 and 2 establish domain credibility.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Insights Across Competitor Profiles

If the same insight (e.g., "all competitors have thin BOFU coverage") is applicable to multiple competitors, it is stated once as a Pattern Finding in Part 5 (Strategic Insights) — not repeated in every competitor's weakness summary. Each competitor's weakness section contains only competitor-specific observations.

### Rule 2 — Merge Overlapping Gaps

When two or more clusters or funnel stages produce identical or nearly identical gap opportunities:
- Merge them into a single gap entry with a combined description.
- Note both cluster names in the merged entry.
- Do not list them as separate opportunities when their strategic implication is identical.

### Rule 3 — No Strength and Weakness Double-Listing

A competitor's behavior cannot appear as both a strength and a weakness in the same profile. If a behavior is a strength → it is listed as a strength only. If it creates a gap → the gap is listed in the Gap Matrix. No insight may appear in both the strength and weakness sections.

### Rule 4 — No Vague Macro-Level Observations in Competitor Profiles

The competitor comparison table (Part 2) contains the macro-level summary. Competitor profiles (Part 1) must contain only specific, evidence-backed statements. Any observation already captured in the comparison table is not restated in full prose within the competitor profile.

### Rule 5 — Gap Matrix Non-Duplication

The Topic Cluster Gap Matrix and the Funnel Stage Gap Matrix are distinct. A gap appearing in the Topic Cluster matrix is NOT automatically duplicated in the Funnel matrix. The Funnel matrix adds the funnel-stage dimension to the cluster-level analysis — it does not restate the same gaps at a different level of abstraction.

### Rule 6 — Opportunity Sections Are Non-Overlapping

Critical Gaps, Contested Opportunities, Freshness Opportunities, and Under-Served Audience Segments are distinct opportunity categories. A topic or cluster may only appear in one of the four opportunity categories — the one that best describes its primary exploitability characteristic. Do not list the same opportunity in multiple categories.

### Rule 7 — Memory Deduplication

Before producing output, query `memory-controller.skill` for a prior competitor analysis for this keyword. If within freshness threshold (14 days) → present cached result. If stale → proceed fresh. Never store two analyses for the same keyword in the same freshness window.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Competitor Analysis Behaviors

| Prohibited Behavior                                                                           | Reason                                                                            |
|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Stating "[Competitor] has high domain authority" as a strength                                | Domain authority is not a content strategy strength — it is a technical signal    |
| Stating "[Competitor] is well-known in the industry" as a strength                           | Brand recognition is not content quality — it explains historical ranking, not content merit |
| Stating "[Competitor] has many backlinks" as an insight                                       | Link analysis belongs to authority-mapping.skill — not content competitor analysis |
| Recording a strength without citing a specific content behavior as evidence                  | Every strength must be traceable to a specific observed behavior                  |
| Recording a weakness without rating its exploitability                                        | An unexploitable weakness is not an actionable insight                            |
| Leaving any competitor profile field blank (including Secondary Weakness if absent)          | All fields must be populated — including "none detected" when genuinely absent    |
| Listing "high quality content" as a strength without specifying what makes it high quality   | "High quality" is not an observation — depth level, evidence type, and format are |
| Assigning CRITICAL priority to a gap without verifying ≤1 competitor covers it adequately   | Priority classification must be derived from the gap matrix status                |
| Recommending an entry sequence phase without grounding it in the gap matrix and threat levels | Recommendations must derive from the analysis — not from general strategy advice  |
| Identifying more than 5 competitors without justification                                    | The analysis is calibrated for 5 — more dilutes analytical depth                 |

### Required Analysis Behaviors

- Every strength statement must cite a specific content behavior or SERP signal.
- Every weakness statement must include an exploitability rating.
- Every gap in the matrix must have a gap status and a priority.
- Every opportunity must describe a specific content approach to capture it.
- Every strategic insight in Part 5 must be grounded in cross-competitor pattern data — not single-competitor observations.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source                                                                      |
| **Receives**     | Top-ranking pages, dominant intent, content type patterns, PAA questions                    |
| **Used for**     | Competitor identification (Phase 1A), content type assessment (Phase 3A)                   |

---

### ▸ deep-serp-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary competitor content intelligence)                              |
| **Receives**     | Per-page dimension data across all 8 dimensions for top-ranking pages                      |
| **Used for**     | Coverage depth (Phase 2A), AEO weakness (Phase 5B), freshness (Phase 2D), entity coverage (Phase 3E) |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source                                                                      |
| **Receives**     | Keyword clusters, cluster labels, cluster importance scores                                 |
| **Used for**     | Gap matrix cluster population (Phase 6A), cluster coverage mapping (Phase 2B)              |

---

### ▸ entity-extraction.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source (optional)                                                          |
| **Receives**     | Critical and Important entity list, entity gap data                                         |
| **Used for**     | Entity weakness detection (Phase 5B), entity coverage assessment (Phase 3E)               |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | competitor-analysis → gap-opportunity-engine (primary output consumer)                     |
| **Sends**        | Gap matrix (both matrices), opportunity intelligence (Part 4), strategic insights (Part 5) |
| **Used for**     | Scoring and prioritizing content opportunities from the gap data                           |
| **Critical field** | Gap Matrix priority assignments — these drive opportunity scoring directly               |

---

### ▸ authority-mapping.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | competitor-analysis → authority-mapping (context consumer)                                 |
| **Sends**        | Competitor threat levels, cluster dominance data, competitive threat ceilings               |
| **Used for**     | Calibrating authority-building strategy to match the competitive landscape                 |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | competitor-analysis → roadmap-engine (strategic input)                                     |
| **Sends**        | Recommended entry sequence (Phase 1/2/3), gap priority data, opportunity intelligence      |
| **Used for**     | Building the content roadmap in a sequence that reflects competitive reality               |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before analysis, WRITE after output finalization                       |
| **Reads**        | Prior competitor analysis for this keyword (freshness check)                               |
| **Writes**       | Complete competitor analysis report, gap matrix, competitor profiles                        |
| **Memory layer** | strategy-memory.skill                                                                        |
| **Timing**       | READ at pre-conditions. WRITE after Step 7 completes.                                       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                           |
|---------------------------------------------------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Fewer than 3 competitors can be identified                               | Candidate pool < 3 after all three types exhausted    | Broaden analysis: expand to semantic competitors from adjacent keyword clusters. If still < 3 → proceed with available competitors; flag output as LIMITED COMPETITOR SET. |
| No SERP data is available                                                | `serp_data` input is null; no deep-SERP inheritance   | Switch to inference mode using keyword signals only. Derive competitor candidates from topic's known major players. Flag ALL competitor insights as INFERRED. |
| Competitor URLs provided but inaccessible                                | Input URLs return no analyzable data                  | Remove inaccessible URLs from competitor pool. Expand to SERP-identified competitors. Flag removed URLs in output. |
| Cluster data unavailable for Gap Matrix                                  | `keyword_clusters` input is null                      | Build Gap Matrix using inferred sub-topics from keyword universe Dimensions 1–4. Label matrix: "GAP MATRIX — Inferred Clusters — Verify against semantic-clustering.skill output." |
| Entity map unavailable for entity weakness detection                     | `entity_map` input is null                            | Conduct entity weakness assessment from semantic keyword signals in `deep-serp-analysis.skill` D4 only. Flag: "Entity weakness analysis limited — entity-extraction.skill data unavailable." |
| All 5 competitors are HIGH threat                                        | All threat levels = HIGH after calibration            | Do not downgrade for strategic convenience. Instead: focus opportunities on threat ceilings and strategic blind spots exclusively. Flag: "High-competition niche — entry requires differentiation, not gap-filling." |
| No weaknesses detected in any competitor                                 | Step 5 finds no exploitable weaknesses across all 5   | Flag: "No significant exploitable weaknesses detected. Recommend differentiation strategy over gap strategy." Focus Part 4 entirely on blind spots and audience segments. |
| All clusters are CONTESTED (no gaps detected)                            | Gap Matrix shows CONTESTED for all clusters           | Flag: "Highly competitive niche — all clusters are contested." Shift opportunity analysis to funnel gap matrix. Report freshness and AEO opportunities exclusively. |
| Memory controller unavailable for freshness check                        | READ query returns error                              | Proceed without cache check. Flag: "Memory unavailable — prior analysis may be duplicated." |
| Competitor content is in a different language than target keyword         | Language mismatch detected in competitor profile      | Exclude multilingual competitors from pattern analysis. Keep them in the comparison table but flag: "Non-target-language content — patterns may not apply directly." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — WEAKNESS TYPE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
WEAKNESS TYPE REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FRESHNESS   → Outdated content; superseded data, tools, or methods
              Detection: publication/update dates; old tool references; stale statistics
              Exploitability: HIGH — freshness is immediately advantageous

DEPTH       → Thin coverage; topic mentioned but not meaningfully explained
              Detection: short content; few headings; no examples; surface treatment
              Exploitability: HIGH — comprehensive content immediately outranks thin content

FUNNEL GAP  → Missing entire funnel stages
              Detection: TOFU only, BOFU only, or no MOFU content on this topic
              Exploitability: HIGH — uncovered funnel stages have no competition

AEO         → No FAQ structure, no direct answers, no snippet optimization
              Detection: no FAQ sections, no structured answers, no schema signals
              Exploitability: MEDIUM-HIGH — AEO content competes for snippet positions

ENTITY      → Missing critical entities from the topic's semantic field
              Detection: entity map comparison; missing tool names, concepts, metrics
              Exploitability: MEDIUM — richer entity coverage signals deeper expertise

FORMAT      → Wrong format for the intent; prose for process, no list for comparison
              Detection: format mismatch between content type and search intent
              Exploitability: MEDIUM — better format execution improves user signals

COVERAGE    → Missing entire topic clusters the audience needs
              Detection: cluster gap matrix; ABSENT status for important clusters
              Exploitability: HIGH — absent coverage = no competition

EVIDENCE    → Unsourced claims, absent examples, assertions without data
              Detection: quality signal analysis from D6; unsourced statistics
              Exploitability: MEDIUM — evidence-backed content builds more trust

AUDIENCE    → Wrong audience level; too broad for experts, too technical for beginners
              Detection: keyword audience signal vs. competitor content audience targeting
              Exploitability: MEDIUM — audience-aligned content serves readers better
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — GAP STATUS AND PRIORITY QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
GAP MATRIX STATUS AND PRIORITY REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FULL GAP    → 0 competitors adequately cover this area
              Priority: CRITICAL (if cluster is important) → IMMEDIATE ENTRY

GAP         → ≤1 competitor has strong coverage
              Priority: HIGH → NEAR-TERM ENTRY

PARTIAL     → 2–3 competitors weakly cover this area
              Priority: MEDIUM → PLANNED ENTRY WITH DIFFERENTIATION

CONTESTED   → ≥4 competitors have strong coverage
              Priority: LOW → LONG-TERM ENTRY ONLY (after authority builds)

PRIORITY DERIVATION RULE:
  Gap Status × Cluster Importance = Priority
  FULL GAP + Critical Cluster    = CRITICAL priority
  FULL GAP + Important Cluster   = HIGH priority
  GAP + Critical Cluster         = HIGH priority
  GAP + Important Cluster        = MEDIUM priority
  PARTIAL + Critical Cluster     = HIGH priority
  PARTIAL + Important Cluster    = MEDIUM priority
  CONTESTED + Any Cluster        = LOW priority
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — COMPETITOR SELECTION PRIORITY REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
COMPETITOR SELECTION CRITERIA (IN PRIORITY ORDER)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SLOT 1 → SERP competitor at position #1–3
  Rationale: Highest current SERP authority — must be understood and contextualized

SLOT 2 → SERP competitor with most different content approach from Slot 1
  Rationale: Ensures strategic diversity in the analysis; reveals format alternatives

SLOT 3 → SERP competitor with most exploitable weakness
  Rationale: Identifies the most immediately actionable displacement target

SLOT 4 → Semantic competitor with broadest cluster coverage
  Rationale: Reveals the topical authority landscape beyond this specific keyword

SLOT 5 → Competitor with most strategically distinct profile (SERP, semantic, or audience)
  Rationale: Ensures the analysis covers the full competitive context

EXCLUSION RULES:
  → Always exclude the user's own domain
  → Exclude e-commerce product listings without editorial content
  → Exclude Wikipedia and government sites (not content strategy competitors)
  → Exclude tool homepages without editorial/blog content
  → Exclude non-English competitors if the target keyword is English

TIE-BREAKING:
  When two candidates qualify equally for the same slot →
  Prefer the candidate whose WEAKNESSES are more exploitable.
  Analytical value > competitive strength.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: research/competitor-analysis.skill*
*Nexus SEO Operating System — Version 1.0.0*
