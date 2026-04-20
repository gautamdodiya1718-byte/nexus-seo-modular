# SKILL: strategy/strategist-ai.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Strategy / Executive Intelligence
# EXECUTION PRIORITY: FINAL SYNTHESIS — Runs after all analysis and planning skills have produced their outputs. The last skill in the strategy layer before output delivery.
# POSITION: Strategic intelligence layer. Synthesizes all Nexus system outputs into a single, clear, decision-ready strategic brief that eliminates confusion and directs action.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **senior SEO strategist intelligence layer** of the Nexus system.

Every other skill in the Nexus system produces one type of output: intelligence about a specific domain of SEO — SERP patterns, keyword clusters, authority gaps, opportunity scores, linking architecture, content roadmaps. Each of those outputs is accurate, detailed, and deeply useful for the specialist who needs to act on it. But when a founder, content director, or head of growth receives fifteen separate analytical outputs and must decide what to do this week, those outputs collectively produce not clarity but paralysis.

This skill resolves that problem. It acts as a senior SEO strategist who has reviewed all of the system's outputs — the roadmap, the authority map, the opportunity scores, the gap analysis, the SERP intelligence — and synthesizes them into the five things a decision-maker actually needs: a clear current state picture, a specific direction, the next seven actions in priority order, the three most dangerous mistakes to avoid, and a realistic projection of what implementing the roadmap will produce in 90 days.

This skill does not produce new analysis. It produces strategic synthesis. The difference is critical: analysis describes what the data shows; synthesis tells you what to do about it and why, given all of the data simultaneously. A strategist who has seen only the opportunity scores and not the authority map will recommend different actions than one who has seen both. This skill has seen everything.

### Primary Responsibilities

| Responsibility                          | Description                                                                                                           |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Current State Analysis**              | Synthesize a SWOT-style current state picture from all available system intelligence — referencing specific data     |
| **Strategic Summary Production**        | Write a 3–5 sentence executive summary of the site's strategic position and the direction it must move               |
| **Next Best Actions**                   | Produce 5–7 specific, time-bound, impact-estimated actions in priority order                                         |
| **Prioritization Logic Application**    | Apply the high-impact-first, BOFU-before-TOFU, cluster-first prioritization framework consistently                   |
| **Avoidance Identification**            | Name the 3 specific mistakes that would waste resources or damage rankings given this site's particular situation     |
| **90-Day Projection**                   | Project realistic Month 1 and Month 3 outcomes based on the roadmap and current authority state                      |
| **Cross-Skill Synthesis**               | Identify patterns that span multiple skill outputs — insights that no single skill can surface alone                 |
| **Priority Compression**                | Compress the full analysis into the minimum necessary action set — removing everything that doesn't move the needle   |
| **Risk Detection**                      | Identify strategic risks that could undermine the roadmap — cannibalization, intent mismatches, dependency failures   |
| **Assumption Documentation**            | When data is insufficient for a strategic conclusion, state the assumption explicitly — never substitute vague hedges |

### What This Skill Is NOT

- It is **not** an analyst. It synthesizes analysis — it does not re-run keyword research or SERP audits.
- It is **not** a content creator. It produces strategic direction — not article briefs or metadata.
- It is **not** a motivational layer. There is no place for phrases like "huge opportunity" or "you've got this" in its output.
- It is **not** a checklist generator. The Next Best Actions are strategic decisions — not implementation task lists.
- It is **not** a replacement for human judgment. It synthesizes data into a clear direction — a human strategist makes the final call.

### The Defining Standard of This Skill

> Every sentence in the output of this skill must be traceable to data.
>
> "Strong topical authority in X" must reference the authority score for X.
> "Prioritize BOFU content" must reference the cluster where BOFU is absent.
> "You are at risk of cannibalization" must name the two keywords competing.
> "By Month 3, expect [outcome]" must explain which roadmap items drive that outcome.
>
> Vague strategic language — "focus on quality content," "build your authority," "target the right keywords" —
> is not strategy. It is noise. It is the output of a system that cannot see the data.
>
> This skill can see all the data. Its output reflects that.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                             | Required |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------|----------|
| `roadmap-engine.skill`                | Full roadmap table, dependency map, phase execution notes, backlog, funnel distribution, clusters addressed | YES      |
| `opportunity-engine.skill`            | Scored keyword table with D1–D5 dimensions, strategic synthesis (highest-opportunity cluster, quick wins, lowest-priority cluster), validation queue | YES |
| `authority-engine.skill`              | Authority scores per cluster (0–5), coverage levels, gap urgency classifications, action priority queue, domain authority score | YES |
| `gap-opportunity-engine.skill`        | Content ideas with gap type, priority, strategic reasons, cluster gap map, validation queue                 | YES      |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                    | Used For                                                            |
|---------------------------------------|------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| `serp-intelligence.skill`             | Dominant intent, difficulty, SERP features, content direction, opportunity signals | Current state competitive context and risk detection               |
| `memory-controller.skill`             | Prior strategic summaries, prior action plans, prior roadmap status               | Cross-session continuity — are prior actions completed?            |
| `content-awareness.skill`             | Coverage map, freshness flags, current published page count                        | Current state — what already exists                                 |
| `content-inventory.skill`             | Published pages, draft pages, planned pages                                        | Quantifying the existing content base in current state             |
| `internal-linking.skill`              | Orphan page count, pillar reinforcement needs, cannibalization issues             | Risk detection and avoidance recommendations                       |
| `query-graph.skill`                   | Hub Scores, orphan clusters, authority flow data                                   | Strategic direction — which clusters are architecturally central   |
| `deduplication-engine.skill`          | Cannibalization alerts, intent overlap flags                                       | What-to-avoid section — cannibalization risk                       |

### Input Pre-Conditions

1. **Is roadmap data available?** If not → strategic synthesis cannot produce a 90-day projection. Flag: "Roadmap absent — projections will be estimated from opportunity data only."
2. **Is authority map available?** If not → current state analysis will be limited. Flag: "Authority map absent — current state based on gap analysis and opportunity scores only."
3. **Is memory data available?** If yes → check whether prior actions have been implemented. Adjust the strategic summary to reflect the current cycle vs. prior cycle differences. If no → produce as first-session synthesis.
4. **Is sufficient data present for all five output sections?** If data is partial → document specific assumptions for each section where data is absent. Never produce vague strategy to hide data gaps.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Strategic Intelligence Brief

```
NEXUS STRATEGIC INTELLIGENCE BRIEF
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain                  : [domain]
Data Sources Active     : [count of upstream skills with data]
Memory Session          : [FIRST RUN / SESSION N — prior actions: N complete / N pending]
Analysis Confidence     : [HIGH / MEDIUM / LOW — based on data completeness]
Generated By            : strategy/strategist-ai.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — CURRENT STATE ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Step 1 output format]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — STRATEGIC SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Step 2 output format]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — NEXT BEST ACTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Step 3 output format]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — WHAT TO AVOID
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Step 5 output format]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — 90-DAY PROJECTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Step 6 output format]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ASSUMPTIONS (if data was partial)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Explicit list of any assumptions made where data was absent]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : strategy-memory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — CURRENT STATE ANALYSIS

Evaluate the site's current strategic position across four dimensions: Strengths, Weaknesses, Opportunities, and Risks. Every statement in this section must be backed by a specific data point from the input sources.

**Phase 1A — Strengths Identification**

Strengths are areas where the site currently has a measurable competitive advantage or established foundation.

Source signals for Strengths:

| Signal Source                         | Strength Signal                                                                              |
|---------------------------------------|----------------------------------------------------------------------------------------------|
| `authority-engine.skill`              | Any cluster with Authority Score ≥ 3 (MEDIUM or above) = existing topical strength          |
| `authority-engine.skill`              | Domain-wide authority score ≥ 3.0 = above-average overall coverage                         |
| `opportunity-engine.skill`            | Clusters where most keywords score OPTIMAL density = already-placed keyword architecture    |
| `content-inventory.skill`             | High coverage-score pages (≥ 4) = proven content assets                                    |
| `gap-opportunity-engine.skill`        | Clusters with PARTIAL gap only (not FULL GAP) = some competitive coverage exists           |
| `content-awareness.skill`             | Coverage Status = COMPLETE or STRONG for any cluster = existing authority territory        |
| Prior memory data                     | Actions completed from prior cycles = real improvements already implemented                |

**Strength Statement Format:**

Each strength must:
- Name the specific cluster, keyword, or metric it refers to.
- State the data point that confirms it is a strength.
- Be one sentence.

```
STRENGTHS:
1. [Cluster Name] has an authority score of [N/5] — [N] published pages provide meaningful coverage depth.
2. [N] content assets scored [N/5] coverage — these are established authority pieces that can anchor internal linking.
3. [specific claim backed by specific data point]
```

**Phase 1B — Weaknesses Identification**

Weaknesses are specific gaps, deficiencies, or misalignments in the current content or strategy.

Source signals for Weaknesses:

| Signal Source                         | Weakness Signal                                                                              |
|---------------------------------------|----------------------------------------------------------------------------------------------|
| `authority-engine.skill`              | Any cluster with Authority Score 0–1 = critical weakness                                    |
| `authority-engine.skill`              | Domain-wide authority score < 2.5 = overall coverage is thin                               |
| `gap-opportunity-engine.skill`        | HIGH priority MISSING TOPIC gaps = entire topic territories not addressed                   |
| `opportunity-engine.skill`            | HIGH-score BOFU keywords with no content = conversion path is missing                      |
| `internal-linking.skill`              | Orphan page count > 10% of inventory = authority is not flowing                            |
| `serp-intelligence.skill`             | Intent mismatch signals = content serves wrong searcher stage                              |
| Funnel distribution                   | CONVERSION-MISSING or MOFU-ABSENT = broken revenue architecture                            |

**Weakness Statement Format:**

Each weakness must:
- Name the specific cluster, keyword, metric, or structural failure.
- State the data point that confirms it.
- State the consequence if not addressed.
- Be one to two sentences.

```
WEAKNESSES:
1. [Cluster Name] has an authority score of [0–1] — [specific consequence: no conversion path / no SERP visibility / etc.].
2. [N] of [N] priority clusters have no BOFU content — the site cannot convert organic traffic to leads in [N] topic areas.
3. [specific weakness with data and consequence]
```

**Phase 1C — Opportunities Identification**

Opportunities are specific, confirmed windows for ranking improvement or traffic capture that the system has identified.

Source signals for Opportunities:

| Signal Source                         | Opportunity Signal                                                                           |
|---------------------------------------|----------------------------------------------------------------------------------------------|
| `opportunity-engine.skill`            | Keywords with score ≥ 8 = highest-confidence opportunity targets                           |
| `gap-opportunity-engine.skill`        | EMERGING TOPIC gaps with EASY SERP = first-mover windows                                   |
| `gap-opportunity-engine.skill`        | MISSING TOPIC with EASY SERP = uncontested territory                                       |
| `roadmap-engine.skill`                | Quick wins identified = fast implementation path to measurable results                     |
| `serp-intelligence.skill`             | SERP opportunity signal EASY for any target keyword = low barrier to ranking               |
| `authority-engine.skill`              | Clusters where one additional piece would elevate score from 2→3 = high-leverage additions |

**Opportunity Statement Format:**

Each opportunity must:
- Name the specific keyword, cluster, or pattern.
- State the data-confirmed opportunity signal (SERP difficulty, gap status, opportunity score).
- State the measurable outcome the opportunity would produce.
- Be one to two sentences.

```
OPPORTUNITIES:
1. "[keyword]" (Opportunity Score: [N]/10) has an EASY SERP and confirmed FULL GAP — a well-executed piece here can rank with minimal competition.
2. [N] emerging topics in the [Cluster Name] territory show EASY SERPs with no established competitors — first-mover advantage window is open.
3. [specific opportunity with data and outcome]
```

**Phase 1D — Risks Identification**

Risks are specific threats to the strategy — patterns or conditions that could undermine the roadmap or damage rankings if not addressed.

Source signals for Risks:

| Signal Source                         | Risk Signal                                                                                  |
|---------------------------------------|----------------------------------------------------------------------------------------------|
| `deduplication-engine.skill`          | Cannibalization alerts — two pages targeting the same keyword                               |
| `internal-linking.skill`              | Cannibalization report — same anchor text pointing to two different pages                  |
| `serp-intelligence.skill`             | Intent mismatch between existing content and current SERP                                  |
| `gap-opportunity-engine.skill`        | MISSING FRESHNESS gaps in high-authority pages — existing rankings at risk                 |
| `authority-engine.skill`              | Clusters with CLUSTER FRESHNESS RISK — content is aging across a whole cluster             |
| `roadmap-engine.skill`                | Long dependency chains — a single missed item blocks multiple subsequent items             |
| `opportunity-engine.skill`            | NEEDS VALIDATION queue — decisions being made on unverified assumptions                    |
| `content-awareness.skill`             | Multiple STALE pages in a high-authority cluster — rankings deteriorating                  |

**Risk Statement Format:**

Each risk must:
- Name the specific threat.
- State the data that identifies it.
- State the consequence if not mitigated.
- Be one to two sentences.

```
RISKS:
1. [N] pages targeting "[keyword]" and "[keyword2]" are creating keyword cannibalization — Google cannot determine which page to rank, and both are likely performing below their potential.
2. [Cluster Name] has [N] pages classified as STALE (18+ months) in a freshness-sensitive SERP — ranking positions are at risk of continuing decline without a refresh cycle.
3. [specific risk with data and consequence]
```

**Phase 1E — Current State Summary Table**

```
CURRENT STATE SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Domain Authority Score  : [N/5] — [STRONG / MODERATE / WEAK]
Clusters with Content   : [N of N total clusters]
  Score ≥ 3 (Viable)    : [count]
  Score 1–2 (Weak)      : [count]
  Score 0 (Empty)       : [count]
BOFU Content Present    : [YES — N pieces / NO — CRITICAL GAP]
Keyword Cannibalization : [NONE DETECTED / N CONFLICTS]
Orphan Pages            : [count]
Freshness Risk          : [NONE / N pages at risk]
TOP STRENGTH            : [single most significant strength — specific]
TOP WEAKNESS            : [single most significant weakness — specific]
TOP OPPORTUNITY         : [single highest-impact opportunity — specific]
TOP RISK                : [single most dangerous risk — specific]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — STRATEGIC SUMMARY

Write a 3–5 sentence executive summary of the site's current strategic position and the direction it must move. This summary synthesizes everything from Step 1 into a clear, direct, actionable direction statement.

**Phase 2A — Strategic Summary Rules**

Every sentence in the strategic summary must:
1. Reference at least one specific data point.
2. Say something that could not be said about every site (i.e., it is specific to this site's situation).
3. Contribute either a current state fact, a strategic direction, or a priority rationale.
4. Contain no motivational language ("great opportunity," "significant potential," "exciting position").
5. Be written as a strategist would brief a CEO — direct, specific, no filler.

**Prohibited Strategic Summary Openers:**

```
PROHIBITED SUMMARY OPENERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
"This site has a great opportunity to..."
"There is significant potential for..."
"You are well-positioned to..."
"By focusing on the right content..."
"SEO is a long-term game and..."
"Building topical authority requires..."
"Content is king and..."
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Strategic Summary Structure:**

| Sentence | Purpose                                                                                        |
|----------|------------------------------------------------------------------------------------------------|
| 1        | Current state: what the site's authority profile currently is — strengths and dominant weakness|
| 2        | Highest urgency: the single most critical action and the data reason it is most critical       |
| 3        | Strategic direction: the overall strategic posture for the next 90 days                       |
| 4 (optional) | Competitive context: how the site sits relative to the SERP landscape for its core targets |
| 5 (optional) | Risk note: the most significant risk that could undermine the strategy if ignored           |

**Example Strategic Summary (format only — not content):**

`"The site has established authority in [Cluster A] (Score 4/5) and [Cluster B] (Score 3/5) but has zero BOFU content in its highest-strength cluster — meaning existing organic traffic has no conversion path. The most urgent action is creating a BOFU comparison page for '[keyword]' (Opportunity Score 9/10, EASY SERP), which directly addresses the only cluster currently driving traffic. For the next 90 days, the strategy is to complete the conversion architecture in the two established clusters before expanding into new territory. The [Cluster C] SERP is currently dominated by weak 2021 content — an emerging window that should be captured in Month 2 before competitors refresh. The primary risk is keyword cannibalization between '[kw1]' and '[kw2]' — resolving this in the first week removes a drag on both pages' ranking potential."`

---

### ▸ STEP 3 — NEXT BEST ACTIONS

List 5–7 specific, time-bound, impact-estimated actions in priority order. These are not tasks — they are strategic decisions with implementation instructions.

**Phase 3A — Action Selection Protocol**

Actions are selected from the following sources, in priority order:

| Source                                | Action Type                                                                         |
|---------------------------------------|-------------------------------------------------------------------------------------|
| Roadmap Month 1 items                 | The highest-priority scheduled content creation actions                            |
| Authority-engine CRITICAL gap urgency | Address clusters with 0 authority and BOFU intent first                           |
| Opportunity-engine quick wins         | HIGH score + EASY SERP + CONTENT_EMPTY = immediate execution candidates            |
| Cannibalization alerts (dedup/linking)| Resolve before any new content is created — cannibalization drags existing performance |
| Orphan resolution (internal-linking)  | Critical orphans (0 incoming links on pillar/BOFU pages) = urgent structural fix  |
| Freshness risk (content-awareness)    | STALE high-authority pages = active ranking risk requiring refresh action          |
| Dependency blockers (roadmap)         | Items that must be completed before the most critical roadmap items can execute    |

**Phase 3B — Action Prioritization Logic**

Apply this decision logic to order the selected actions:

1. **FIRST: Resolve cannibalization** — if any exists. Cannibalization actively degrades current rankings; no new content should be created while existing pages compete with each other.
2. **SECOND: Critical orphan rescue** — if any pillar or BOFU page has 0 incoming links. These pages cannot compound authority until they are connected to the link graph.
3. **THIRD: BOFU gaps in established clusters** — if the site has traffic but no conversion content in its strongest clusters, this is the highest-leverage revenue action.
4. **FOURTH: Dependency blockers** — pillar pages that unlock other cluster content must be published before the content that depends on them.
5. **FIFTH: High-score MOFU content** — evaluation-stage content for keywords with Opportunity Score ≥ 8.
6. **SIXTH: Quick wins** — HIGH opportunity + EASY SERP + CONTENT_EMPTY items not already covered above.
7. **SEVENTH: Freshness refreshes** — STALE high-authority pages in freshness-sensitive SERPs.

**Phase 3C — Action Record Format**

```
NEXT BEST ACTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ACTION #1 (Highest Priority)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Action          : [specific action — not category; names the exact thing to do]
Timeline        : [Week N / Month N — specific, not "soon"]
Expected Impact : [specific, measurable expected change]
Data Basis      : [which data from which skill justifies this action]
How             : [brief implementation note — enough to start without additional research]
If Skipped      : [specific consequence of not taking this action]

ACTION #2
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[same format]

...

ACTION #5–7 (Important but secondary)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[same format — shorter treatment is acceptable for lower-priority items]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3D — Action Quality Standards**

| Quality Check                                         | Pass Condition                                                                          |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------|
| Action is specific enough to assign to a team member | Someone reading it knows exactly what to produce — not "write about X" but "create a BOFU comparison page targeting '[keyword]'" |
| Timeline is specific                                  | Not "this month" — "Week 1" or "Month 1" with a rationale for that timing            |
| Expected impact references a specific metric          | Not "improve SEO performance" — "expected to move [cluster] from Authority Score 1 → 2" |
| Data basis is named                                   | References a specific skill output and the specific signal (opportunity score, authority gap, cannibalization flag) |
| "If Skipped" consequence is specific                  | Not "will harm SEO" — "the Month 2 supporting pages cannot be linked to this pillar, delaying the full cluster launch by at least one month" |

**Prohibited Action Forms:**

```
PROHIBITED ACTION FORMS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✗ "Focus on creating high-quality content"
✗ "Build topical authority in [cluster]"
✗ "Improve your internal linking"
✗ "Create content around [keyword]" (without specifying format, intent, angle)
✗ "Optimize existing pages for SEO"
✗ "Target BOFU keywords" (without naming specific keywords)
✗ "Refresh stale content" (without naming specific pages)
✗ "Fix orphan pages" (without naming which ones and where to link from)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 4 — PRIORITIZATION LOGIC

The prioritization logic applied in Step 3 must be explicitly documented so the decision-maker understands why the actions are ordered as they are.

**Phase 4A — Prioritization Framework Application**

Document the three-part prioritization framework that produced the action ordering:

**Framework 1 — High-Impact First:**
Which action has the highest ratio of impact to effort across all available options? This is determined by:
- Opportunity score × implementation speed (quick win = faster to ranking impact).
- Authority score gap magnitude (Score 0 → 1 is more impactful than Score 4 → 5).
- Business stage relevance (a site with zero conversions benefits more from BOFU than from TOFU depth).

**Framework 2 — BOFU Before TOFU (When Gaps Exist):**
If the site has any cluster with zero BOFU content and existing TOFU/MOFU content in that cluster → BOFU creation takes priority over additional TOFU. The reasoning is precise: TOFU content brings in readers; BOFU content converts them. A site with ten TOFU articles and no BOFU articles is spending editorial resources on audience acquisition while the audience has nowhere to convert. The correct sequence inverts this: close the conversion gap first, then scale the acquisition layer.

**Exceptions to BOFU-first:**
- When no clusters have established TOFU audiences (brand new site with zero authority) → pillar TOFU content must come first to build any audience worth converting.
- When a BOFU keyword has a HARD SERP (all high-authority competitors) → delaying to build cluster authority first is more efficient than publishing BOFU content into an unwinnable SERP.

**Framework 3 — Cluster-First:**
Depth within a cluster is more valuable than breadth across clusters. Publishing the second, third, and fourth article in a cluster where the first article already has some authority compounds faster than publishing one article each in four different clusters. The correct unit of strategic effort is the cluster — achieve minimum viable depth in each priority cluster before beginning new clusters.

**Phase 4B — Prioritization Transparency Block**

```
PRIORITIZATION LOGIC
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Actions #1–2 are prioritized because:
  [specific reason — which framework applies and what data triggers it]

Actions #3–4 are prioritized because:
  [specific reason]

Actions #5–7 are important but secondary because:
  [specific reason — what makes these lower priority than the top actions]

BOFU-BEFORE-TOFU applied: [YES — because [specific cluster/data] / NO — because [specific exception condition]]
CLUSTER-FIRST applied: [YES — focused on [cluster names] / NO — cross-cluster because [reason]]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 5 — WHAT TO AVOID

Name the 3 specific mistakes this site should avoid given its specific strategic situation. These are not generic SEO warnings — they are specific to the patterns detected in this site's data.

**Phase 5A — Avoidance Identification Protocol**

Scan all available data for patterns that would typically lead to common strategic failures. Select the 3 most directly relevant to this site's current state:

**Category 1 — Structural Mistakes (from data patterns):**

| Pattern Detected                                                               | Avoidance Recommendation                                         |
|--------------------------------------------------------------------------------|------------------------------------------------------------------|
| Cannibalization between existing pages                                         | Do not create new content targeting the cannibalized keyword until the existing conflict is resolved |
| Multiple clusters at Score 0 — site wants to address all simultaneously       | Do not spread content creation across too many clusters — minimum viable depth in one cluster before starting another |
| Supporting pages exist but pillar is absent                                    | Do not add more supporting pages to this cluster — the pillar must come first or the authority architecture is inverted |
| TOFU-heavy content pool with no BOFU                                           | Do not continue publishing TOFU content in clusters where no conversion path exists — it compounds an already-imbalanced architecture |

**Category 2 — SERP Mistakes (from opportunity and SERP data):**

| Pattern Detected                                                               | Avoidance Recommendation                                         |
|--------------------------------------------------------------------------------|------------------------------------------------------------------|
| HARD SERP keywords in the roadmap early                                        | Do not prioritize these before authority is established — targeting unwinnable SERPs first wastes editorial resources |
| Intent mismatch in existing content                                            | Do not publish supporting content into a cluster where the pillar page mismatches SERP intent — it compounds the mismatch |
| High-competition MOFU keyword targeted before establishing TOFU foundation    | Do not attempt evaluation-stage content without any awareness-stage content in the same cluster — Google evaluates topical completeness |

**Category 3 — Resource Mistakes (from roadmap and capacity data):**

| Pattern Detected                                                               | Avoidance Recommendation                                         |
|--------------------------------------------------------------------------------|------------------------------------------------------------------|
| Backlog significantly exceeds horizon capacity                                 | Do not try to compress 18 months of work into 6 months by publishing lower-quality content — one high-quality piece at the right opportunity beats three mediocre pieces at average opportunities |
| Freshness risk in high-authority pages is being ignored in favor of new content| Do not create new pages while existing high-authority pages are losing rankings to fresher competitors — a refresh has more immediate impact |
| Dependencies are ignored in scheduling                                         | Do not publish supporting content before its pillar — the authority architecture requires the hub to exist first |

**Phase 5B — Avoidance Record Format**

```
WHAT TO AVOID
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

MISTAKE #1: [specific mistake name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What       : [specific description of the mistake for this site's situation]
Why        : [specific consequence for this site — referencing the data that shows why this is dangerous here]
Data Source: [which skill's output identified this risk]
Instead    : [what to do instead — in one sentence]

MISTAKE #2: [specific mistake name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[same format]

MISTAKE #3: [specific mistake name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[same format]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Quality Check for Avoidance Items:**

Every avoidance item must answer the test: "Would this warning apply to most sites, or specifically to this site based on its data?" If the answer is "most sites" → replace it with a site-specific finding. Generic SEO warnings are prohibited.

---

### ▸ STEP 6 — 90-DAY PROJECTION

Project the realistic expected outcomes of implementing the roadmap at the planned pace. Month 1 and Month 3 outcomes are projected separately.

**Phase 6A — Month 1 Projection**

Month 1 represents the outcome of completing the SHORT-TERM phase Month 1 roadmap items (typically 4 content pieces or actions).

For Month 1, project:

| Projection Dimension         | Source Data                                              | Projection Method                                                |
|------------------------------|----------------------------------------------------------|------------------------------------------------------------------|
| Authority changes            | Authority scores before Month 1 + expected gains from specific pieces | Each new HIGH-authority piece in a cluster adds approximately 0.5–1.0 to that cluster's score |
| Conversion path availability | BOFU content gaps + Month 1 BOFU pieces scheduled       | If Month 1 includes a BOFU piece for a cluster → note that cluster now has a conversion path |
| Cannibalization resolution   | Cannibalization issues + resolution actions in Month 1  | Each resolved cannibalization pair restores ranking competition elimination |
| Orphan resolution            | Critical orphan count + rescue links in Month 1         | Links added to pillar/BOFU orphans immediately improve crawlability |
| Indexing and initial ranking | Typical indexing timeline for new content               | New content typically enters the index within 1–4 weeks; early ranking signals appear in weeks 2–6 |

**Phase 6B — Month 3 Projection**

Month 3 represents the cumulative outcome of all SHORT-TERM phase work (Months 1–2) plus the first month of MID-TERM work.

For Month 3, project:

| Projection Dimension         | Source Data                                              | Projection Method                                                |
|------------------------------|----------------------------------------------------------|------------------------------------------------------------------|
| Cluster authority advancement | Authority scores + all SHORT-TERM pieces published     | Clusters targeted in SHORT-TERM should move from Score [X] to [X+1] or [X+2] |
| Funnel completion            | Funnel gaps + SHORT-TERM funnel bridge content          | Which clusters will have a complete funnel by Month 3            |
| Traffic pattern changes      | TOFU and MOFU pieces indexed by Month 3                 | Organic impressions typically increase 3–8 weeks after indexing for target keywords |
| Competitive positioning      | SERP difficulty + early ranking signals                 | Which target keywords are likely to show position improvements   |
| Internal link architecture   | Link plan implementation progress                       | Pillar pages should have their primary links in place, improving crawl authority |

**Phase 6C — Projection Calibration Rules**

The 90-day projection must be calibrated to be realistic, not aspirational:

| Calibration Rule                                                                               |
|------------------------------------------------------------------------------------------------|
| New content does not rank on publication day. Allow 2–6 weeks for indexing and initial ranking. |
| Pages in new clusters with Score 0 authority will rank slowly — SERP positioning may take 2–3 months for any visibility. |
| Pages adding to established clusters (Score 3+) will benefit from existing topical authority — ranking signals may appear within 3–5 weeks. |
| BOFU pages targeting HARD SERPs will show minimal ranking movement in 90 days — note this explicitly. |
| Freshness refreshes of already-ranking pages typically produce ranking improvements within 2–4 weeks — faster than new content. |
| Cannibalization resolution typically produces ranking improvements for the dominant page within 2–4 weeks. |

**Phase 6D — 90-Day Projection Output Format**

```
90-DAY PROJECTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

BY END OF MONTH 1 (assuming full execution of Month 1 roadmap items):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Authority Changes:
  [Cluster Name]: Score [X] → Score [X+0.5 or X+1] (new piece published targeting this cluster)
  [Cluster Name]: Score [X] (no new content targeted here — no change)

Conversion Architecture:
  [Cluster Name]: BOFU content now live — organic traffic from this cluster has a conversion path for the first time.
  [Cluster Name]: Still no BOFU — conversion path remains absent until Month 2.

Structural Fixes:
  Cannibalization between "[kw1]" and "[kw2]": RESOLVED — expect ranking recovery for [dominant page] within 2–4 weeks.
  Critical orphan [page URL]: NOW LINKED — crawl inclusion expected within 1–2 weeks.

Ranking Signals:
  "[keyword]" (Opportunity Score [N], EASY SERP): New content published in Week 1 — expect indexing by Week 2–3; first ranking signals in Weeks 3–5.
  "[keyword]" (Opportunity Score [N], MEDIUM SERP): New content published in Week 3 — expect indexing by Week 4–5; initial ranking signals in Weeks 5–8.

BY END OF MONTH 3 (assuming full execution of Months 1–2 roadmap):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster Authority State:
  [Cluster Name]: Projected Score [N/5] — [N] new pieces establishing [MOFU + BOFU / full funnel] coverage.
  [Cluster Name]: Projected Score [N/5] — pillar + first supporting page live; cluster foundation established.
  [N] clusters at Minimum Viable Depth (pillar + MOFU + BOFU present).

Funnel Architecture:
  [N] clusters with complete funnel by Month 3.
  [N] clusters still TOFU-only — addressed in MID-TERM phase.

Traffic and Ranking Pattern:
  SHORT-TERM BOFU and MOFU content (published Month 1–2) will be 4–8 weeks indexed by Month 3 — expect first SERP position appearances for [N] target keywords.
  Freshness refresh (if included in SHORT-TERM) should show ranking improvement by Month 3 for [specific keywords].
  HARD-SERP targets: minimal ranking movement expected by Month 3 — cluster authority needs further development.

Strategic State:
  [Specific, data-backed summary of where the site will be strategically by Month 3 — what changed, what still needs work]

PROJECTION CONFIDENCE: [HIGH — all SHORT-TERM items are confirmed EASY SERPs / MEDIUM — mixed difficulty / LOW — most SHORT-TERM items target MEDIUM or HARD SERPs]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CROSS-SKILL SYNTHESIS

The most valuable insights produced by this skill are those that emerge from the intersection of multiple skill outputs — patterns that no single upstream skill can detect alone.

**Cross-Skill Pattern Detection:**

| Pattern Type                                        | Detected From                                       | Synthesis Insight                                                              |
|-----------------------------------------------------|-----------------------------------------------------|---------------------------------------------------------------------------------|
| High opportunity score + low authority cluster     | opportunity-engine + authority-engine               | The site has identified a high-value target but lacks the authority foundation to rank — build the cluster before targeting the keyword |
| Complete funnel in one cluster + zero funnel elsewhere | authority-engine (funnel coverage) + gap analysis | Authority is concentrated; the site is dependent on one cluster for all conversions — strategic diversification needed |
| Large backlog + high SERP difficulty in queue       | roadmap-engine backlog + serp-intelligence          | The backlog is unlikely to produce results — prune it; focus capacity on fewer, higher-probability targets |
| Orphan pages in high-authority cluster             | internal-linking + authority-engine                 | Authority is being lost because pages that should reinforce the cluster have no links — fixing orphans may be faster than creating new content |
| MISSING FRESHNESS in the site's highest-authority cluster | content-awareness + authority-engine          | The site's strongest asset is at risk — refreshing it should rank above creating new content in the priority list |
| Emerging topic aligns with an existing high-authority cluster | gap-opportunity-engine + query-graph        | First-mover advantage is available in a territory where the site already has authority infrastructure — highest-probability ranking action |
| HARD SERP for ALL BOFU keywords                     | serp-intelligence + opportunity-engine (BOFU items) | The site cannot realistically convert organic traffic in the short term — strategy must pivot to authority building before BOFU targeting |

When cross-skill patterns are detected → they are surfaced in the Strategic Summary and in the relevant Next Best Action or Avoidance item. They are labeled as CROSS-SKILL INSIGHT in the output.

---

### ▸ PRIORITY COMPRESSION

This advanced module prevents the Strategic Intelligence Brief from replicating the full analysis with all its detail — compressing multiple findings into the minimum necessary insight set.

**Compression Rules:**

| Situation                                                               | Compression Action                                                    |
|-------------------------------------------------------------------------|-----------------------------------------------------------------------|
| Multiple MISSING TOPIC gaps in the same cluster                        | Compress to one action: "Build out [Cluster Name] — [N] topic gaps confirmed." The roadmap handles the individual items. |
| Multiple LOW-risk items in the gap analysis                             | Summarize in a single note: "[N] LOW-priority gaps exist — defer after Month 3." Do not list them individually. |
| Five clusters all have the same weakness (no BOFU)                     | Compress to one strategic finding: "BOFU is systematically absent across [N] clusters — this is an architecture-level gap, not a cluster-specific one." |
| Multiple canonical strategic recommendations all point in the same direction | Compress to one direction statement. Do not repeat the same recommendation in different words across sections. |

**The Compression Standard:**

> A strategic brief that lists every finding from every upstream skill in detail is not a brief — it is a dump.
> Compression is not losing information. It is finding the signal in the noise.
> Five findings that all say "you need BOFU content" compress to ONE finding with the scale noted.
> The strategist's value is in distillation — not in forwarding the full analysis.

---

### ▸ RISK DETECTION

Beyond the risks identified in Step 1, this advanced module scans for systemic risks that span the full strategy — risks that could undermine the roadmap if not surfaced.

**Systemic Risk Patterns:**

| Risk Pattern                                                                 | Detection Method                                                         | Severity  |
|------------------------------------------------------------------------------|--------------------------------------------------------------------------|-----------|
| **CAPABILITY TRAP**: All HIGH-priority items are in HARD SERPs               | opportunity-engine × serp-intelligence                                   | HIGH      |
| **CLUSTER ISOLATION**: No cross-cluster linking — each cluster is an island | query-graph (disconnected nodes) × internal-linking (no cross-cluster links) | MEDIUM |
| **CONVERSION DESERT**: Site has traffic but zero BOFU across all clusters   | authority-engine (TOFU heavy) × gap analysis (BOFU gaps in all clusters) | CRITICAL  |
| **CANNIBALIZATION CLUSTER**: 3+ pages competing for the same search intent  | deduplication-engine × serp-intelligence                                 | HIGH      |
| **DEPENDENCY CHAIN RISK**: Critical roadmap path has 4+ sequential dependencies | roadmap-engine (critical path length)                                | MEDIUM    |
| **FRESHNESS CLIFF**: 60%+ of existing content is STALE                      | content-awareness (freshness flags across inventory)                     | HIGH      |
| **PILLAR VACUUM**: Multiple high-strength clusters have no pillar pages     | authority-engine × semantic-clustering                                   | MEDIUM    |
| **OVERSCOPE BACKLOG**: Backlog > 2× the planning horizon capacity           | roadmap-engine (backlog count × capacity)                                | MEDIUM    |

When any systemic risk is detected → it is added to the Strategic Summary and escalated in the Avoidance section if it is among the top 3 risks for this site.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Recommendations Across Output Sections

The same recommendation may not appear in both Next Best Actions AND What To Avoid in any form. If a finding drives an action recommendation → it goes in Next Best Actions. If a finding drives an avoidance warning → it goes in What To Avoid. Never both.

### Rule 2 — No Repeated Data Points Across SWOT Items

The same specific data point (e.g., "[Cluster A] Authority Score 0") may be cited once across the four SWOT sections. It most naturally belongs in WEAKNESSES. It must not also appear as a RISK or as a second WEAKNESS item with different phrasing.

### Rule 3 — No Repeated Action Across Timeline

No action may appear in both Month 1 of the 90-Day Projection AND as a standalone Next Best Action item. The projection describes outcomes; the actions describe what to do. Reference the action by number in the projection rather than re-describing it.

### Rule 4 — No Generic Advice After Prior Strategic Sessions

If prior strategy session data is available via `memory-controller.skill` → do not re-recommend actions that were recommended in a prior session and have been marked COMPLETED. Only surface new insights relative to the current session's analysis.

### Rule 5 — Synthesis Deduplication

If three different upstream skills all produce a finding that points to the same conclusion → express it once in the Strategic Summary with a note of its multi-source confirmation: "(confirmed by opportunity-engine, authority-engine, and gap-analysis)." Do not express the same finding three times in three sections.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Strategist Behaviors

| Prohibited Behavior                                                                             | Reason                                                                           |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Writing any SWOT item without a specific data reference                                         | SWOT items are conclusions from data — not general observations                 |
| Using "great opportunity" or "significant potential" anywhere in the output                     | Evaluative language without data is noise — name the opportunity specifically   |
| Using "it is recommended that..." or "you should consider..."                                  | The output is a strategic direction — not a suggestion list                     |
| Producing a 90-day projection that describes activities rather than outcomes                    | "Publish N articles by Month 3" is a plan; "achieve Score 3 in [cluster]" is a projection |
| Writing avoidance items that could apply to any site                                           | Every avoidance item must be specific to this site's detected data patterns     |
| Filling sections where data is absent with vague placeholder language                          | When data is absent → state the assumption explicitly; never obscure the gap    |
| Producing more than 7 Next Best Actions                                                        | More than 7 actions = no compression occurred = a list, not a strategy         |
| Using passive voice for action items ("content should be created")                             | Actions are direct: "Create [specific content] in Week 1"                      |
| Adding a "conclusion" or "summary" section that restates the Strategic Summary                 | The Strategic Summary IS the conclusion — never summarize the summary          |
| Making ranking or traffic guarantees in the 90-Day Projection                                  | Projections are estimates calibrated to realistic SEO timelines — never promises|

### Required Strategist Behaviors

- Every SWOT item traces to a named skill output.
- Every Action names the specific thing, the specific timeline, and the specific expected impact.
- Every Avoidance item is specific to this site's detected pattern — not generic SEO advice.
- The Strategic Summary contains no sentence that could apply to any site without modification.
- The 90-Day Projection distinguishes between activities (what will be done) and outcomes (what will change).
- Every assumption made due to missing data is listed explicitly in the ASSUMPTIONS section.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary action sequencing input)                                      |
| **Receives**     | Full roadmap table, dependency map, phase notes, backlog, funnel distribution               |
| **Used for**     | Step 3 (Next Best Actions from Month 1 items), Step 6 (90-day projection from roadmap sequence), Step 4 (BOFU-first logic from funnel distribution) |

---

### ▸ opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (keyword priority intelligence)                                        |
| **Receives**     | Scored keyword table, strategic synthesis, quick wins, lowest-priority cluster              |
| **Used for**     | Step 1 (Opportunities — high-score keywords), Step 3 (Action selection — quick wins), Step 6 (projection calibration — difficulty levels) |

---

### ▸ authority-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (cluster authority state)                                              |
| **Receives**     | Authority scores, coverage levels, gap urgency, domain authority score, action priority queue |
| **Used for**     | Step 1 (Strengths/Weaknesses), Step 3 (CRITICAL gaps → top actions), Step 6 (authority projection) |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (content gap intelligence)                                             |
| **Receives**     | Content ideas, gap types, cluster gap map, strategic reasons                               |
| **Used for**     | Step 1 (Opportunities — confirmed gaps), Step 5 (Avoidance — structural gap risks)        |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment                                                                |
| **Receives**     | Difficulty, dominant intent, SERP features, opportunity signals                            |
| **Used for**     | Step 1 (Risks — intent mismatches), Step 5 (Avoidance — HARD SERP targeting), Step 6 (projection calibration) |

---

### ▸ internal-linking.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment                                                                |
| **Receives**     | Orphan page count, cannibalization report, pillar reinforcement needs                      |
| **Used for**     | Step 1 (Risks), Step 3 (Action #2 — critical orphan rescue), Step 5 (Avoidance)          |

---

### ▸ strategy-memory.skill (via memory-controller)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after Strategic Intelligence Brief is finalized)                                    |
| **Writes**       | Complete strategic brief, action plan with status tracking, projection targets for future comparison |
| **Used for**     | Cross-session tracking — which actions were completed; how projections compared to actual outcomes |
| **Memory layer** | strategy-memory.skill                                                                       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| All four primary inputs are absent                                        | Input — all primary fields null                        | Halt. "Insufficient data for strategic synthesis. Provide at minimum one of: roadmap, authority map, or opportunity data." |
| Only one primary input is available                                       | Input — single source only                             | Proceed with reduced scope. Flag: "Single-source synthesis — [skill name] only. Recommendations are limited to signals from this source. Run full analysis pipeline for complete strategic brief." |
| No BOFU content exists and no BOFU is in the roadmap                     | Convergent signal from authority-engine + roadmap     | Escalate to CRITICAL finding. State assumption: "No BOFU path exists and none is planned — this site cannot convert organic traffic. This is the highest-priority structural issue." |
| 90-day projection has no clear basis (roadmap absent, SERP data absent)  | Step 6 — projection data insufficient                 | State assumption explicitly: "Month 1 projection assumes: [N] HIGH-priority items scheduled at current pace; MEDIUM SERP difficulty for all targets; no existing domain authority penalty. Actual results will vary." |
| Prior session data conflicts with current analysis                        | Memory comparison — prior action marked COMPLETED but data shows the issue persists | Flag: "Prior action '[action]' was marked complete but [specific data] indicates the underlying issue is unresolved. Re-surfacing as current cycle priority." |
| All opportunity scores are LOW (0–4) across the entire universe           | Convergent signal — no HIGH items in opportunity-engine | State assumption: "All detected opportunities score LOW — this may indicate: (1) the site already has strong coverage; (2) the keyword universe is too competitive; (3) the gap analysis was run on insufficient data. Recommended next step: expand keyword discovery." |
| Strategic summary cannot be written without generic language              | Step 2 — all data is absent or all clusters are unaddressed | State explicitly: "Data is insufficient for a site-specific strategic summary. Below are assumptions applied — verify each before acting on this brief." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — STRATEGIC SUMMARY QUALITY CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
STRATEGIC SUMMARY QUALITY CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Before finalizing the Strategic Summary, verify:

□ Is every sentence traceable to at least one data point?
  If NO → rewrite to include specific data reference.

□ Does any sentence use evaluative language without data?
  ("great opportunity," "strong performance," "promising results")
  If YES → replace with the specific metric and let the data do the evaluation.

□ Could any sentence apply word-for-word to a different site?
  If YES → add the site-specific element that makes this sentence true only for this site.

□ Does the summary name at least one specific:
  □ cluster or keyword
  □ opportunity score or authority score
  □ gap type or SERP difficulty
  If any are missing → the summary is too generic.

□ Is the strategic DIRECTION clear from reading the summary alone?
  A reader who reads only the summary should know:
  → What the site's biggest problem is
  → What to do first
  → What to build toward in 90 days
  If NO → the summary has not synthesized; it has described.

□ Does the summary contain any motivational language?
  ✗ "This is an exciting opportunity"
  ✗ "The site is well-positioned"
  ✗ "With the right strategy"
  If YES → remove and replace with data.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — NEXT BEST ACTIONS QUALITY CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
NEXT BEST ACTIONS QUALITY CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
For each action, verify:

□ Is the action specific enough to assign to a team member without additional briefing?
  If NO → it is a category, not an action. Specify the exact thing to create or fix.

□ Does the action name a specific keyword, page, cluster, or element?
  If NO → it is generic advice, not an action.

□ Is the timeline specific (Week N / Month N)?
  If "this month" or "soon" → specify the month number.

□ Does the expected impact reference a specific metric or outcome?
  If "improve SEO performance" → replace with the specific metric that changes.

□ Is the data basis named (which skill + which signal)?
  If NO → the action cannot be verified or deprioritized by the team.

□ Is the "If Skipped" consequence specific?
  If "will hurt SEO" → name the specific consequence for this action's specific context.

□ Do the 5–7 actions collectively cover:
  □ At least one structural fix (orphan, cannibalization, or dependency)
  □ At least one BOFU or MOFU content action (unless all clusters have BOFU)
  □ At least one quick win (HIGH score + EASY SERP + CONTENT_EMPTY)
  If any category is missing → review whether it should be included.

□ Are the actions in the correct priority order per the framework?
  Priority framework check:
  1. Cannibalization fixes (if any exist)
  2. Critical orphan rescue (if any exist)
  3. BOFU gaps in established clusters
  4. Dependency blockers
  5. High-score MOFU
  6. Quick wins
  7. Freshness refreshes
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — 90-DAY PROJECTION CALIBRATION REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
90-DAY PROJECTION CALIBRATION REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
INDEXING TIMELINES:
  New content (standard pages)  : 1–4 weeks to index
  New content (well-linked site): 3–7 days to index
  Refreshed/updated content     : 3–14 days to re-index
  Orphan page (newly linked)    : 1–2 weeks after link is discovered

RANKING SIGNAL TIMELINES:
  EASY SERP, new content in established cluster  : 3–6 weeks for first position signals
  MEDIUM SERP, new content in established cluster: 6–10 weeks for first signals
  HARD SERP, new content                         : 3+ months; may not move in 90 days
  Refreshed high-authority page                  : 2–4 weeks for ranking improvement
  Cannibalization resolution (dominant page)     : 2–4 weeks for position stabilization
  New pillar page in new cluster (Score 0)       : 6–12 weeks for any ranking signals

AUTHORITY SCORE CHANGES:
  Adding a new comprehensive piece to empty cluster : +0.5 to +1.0 to cluster score
  Adding BOFU to TOFU-only cluster                 : +1.0 to cluster score (funnel completion)
  Full funnel completion (pillar + MOFU + BOFU)    : Cluster reaches Score 3 minimum
  Freshness refresh of pillar                       : May contribute +0.5 to cluster score

CONFIDENCE LEVELS:
  HIGH:  All SHORT-TERM items target EASY SERPs in established clusters
  MEDIUM: Mix of EASY and MEDIUM SERPs; some new clusters
  LOW:   Most SHORT-TERM items target MEDIUM or HARD SERPs; all new clusters

PROJECTION INTEGRITY RULE:
  Never project a specific traffic number — too many variables.
  Always project authority state changes and funnel completeness — these are within the system's control.
  Ranking positions are projectable qualitatively (expect first signals / expect stable improvement) — never numerically.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: strategy/strategist-ai.skill*
*Nexus SEO Operating System — Version 1.0.0*
