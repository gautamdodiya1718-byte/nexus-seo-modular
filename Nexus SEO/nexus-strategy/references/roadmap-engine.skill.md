# SKILL: strategy/roadmap-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Strategy / Execution Planning
# EXECUTION PRIORITY: POST-ANALYSIS — Runs after gap-opportunity-engine.skill, opportunity-engine.skill, and authority-engine.skill have all produced their outputs.
# POSITION: Strategy translation layer. Converts all analysis intelligence into a sequenced, phased, dependency-aware content execution roadmap.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **content execution roadmap engine** of the Nexus strategy layer.

Every preceding skill in the Nexus system produces intelligence: cluster architectures, opportunity scores, authority maps, gap analyses, SERP patterns, entity maps, internal link plans. Each of those intelligence outputs answers a question about the content opportunity landscape. But intelligence without sequencing is a pile of correct information that produces no results. A content team that runs all eleven pipeline skills and receives a ranked list of 80 keyword opportunities has not received a strategy — they have received an unsorted backlog.

This skill takes all of that intelligence and converts it into a structured execution roadmap: a phased, month-by-month, dependency-aware, funnel-balanced production plan that tells the content team not just what to build, but what to build first, why to build it in that order, what depends on what existing before it can be built, and what the expected impact of each phase is.

The roadmap is not a priority list. It is a sequenced plan that respects cluster architecture (pillar pages before supporting pages), funnel logic (foundational content before conversion content where needed), dependency chains (a MOFU comparison cannot exist without the TOFU article it bridges from), and realistic production capacity (maximum 4 items per month to prevent planning theatre).

### Primary Responsibilities

| Responsibility                          | Description                                                                                                           |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Cluster Grouping**                    | Organize all content ideas and gap opportunities by their cluster assignment before any sequencing begins             |
| **Pillar-First Sequencing**             | Within every cluster, always sequence the pillar page before its supporting content                                   |
| **Funnel Mapping**                      | Assign every roadmap item its funnel stage and ensure funnel balance across the plan                                  |
| **Timeline Planning**                  | Distribute roadmap items across short-term (M1–2), mid-term (M3–5), and long-term (M6+) phases                      |
| **Dependency Mapping**                  | Define which items must be published before others can be meaningfully created or linked to                           |
| **Sequence Validation**                 | Verify the final sequence has no dependency conflicts, logical gaps, or funnel imbalances                             |
| **Output Table Production**             | Produce the formatted roadmap table with maximum 4 items per month                                                   |
| **Execution Notes**                     | For each phase and each roadmap item, provide specific rationale for its timing and expected impact                   |
| **Cluster Priority Weighting**          | Weight sequencing decisions by cluster strategic strength — highest-value clusters are built out first                |
| **Opportunity-Based Sequencing**        | Use opportunity scores from `opportunity-engine.skill` as the primary sequencing signal within each phase             |
| **Funnel Balancing**                    | Monitor funnel stage distribution across the roadmap to prevent TOFU-heavy or BOFU-light plans                      |
| **Realistic Workload Enforcement**      | Cap monthly items at 4; flag when the backlog exceeds 12-month horizon; surface to user for scope decisions          |

### What This Skill Is NOT

- It is **not** a content generator. It plans what to build — it does not write content.
- It is **not** a keyword researcher. It works with the keyword universe and gap analysis already produced.
- It is **not** a project management tool. It produces a strategic roadmap — not sprint tickets, assignments, or deadlines.
- It is **not** a static document. It should be re-run whenever new content is published or the opportunity landscape shifts.
- It is **not** a guarantor of rankings. It sequences the highest-probability opportunities correctly — outcomes depend on implementation quality.

### The Defining Standard of This Skill

> A roadmap is only as good as the logic it uses to sequence its items.
>
> "Publish the most important content first" is not a strategy.
> A strategy sequences content so that:
> → Each piece published increases the authority of the pieces that follow it.
> → Each pillar creates the hub that supports pages need to pass authority back to.
> → Each TOFU article builds the audience that MOFU and BOFU content will convert.
> → Each cluster is built to a minimum viable depth before the next cluster begins.
> → Dependencies are always resolved before the content that depends on them is scheduled.
>
> Every roadmap item must be specific enough to brief directly.
> Every rationale must explain the sequencing logic — not just describe the topic.
> Every phase must produce a real, measurable shift in the site's authority or conversion architecture.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                             | Required |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------|----------|
| `semantic-clustering.skill`           | Full cluster architecture — cluster names, tier (Pillar/Sub/Micro), keyword lists, funnel balance, cluster strength scores, pillar designations | YES |
| `gap-opportunity-engine.skill`        | Content ideas with target keyword, intent, cluster, gap type, priority, strategic reason                   | YES      |
| `authority-engine.skill`              | Authority scores per cluster (0–5), missing elements per cluster, action priority queue, gap urgency       | YES      |
| `opportunity-engine.skill`            | Opportunity scores (0–10) per keyword, priority label (HIGH/MEDIUM/LOW), reason records                   | YES      |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                    | Used For                                                            |
|---------------------------------------|------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| `query-graph.skill`                   | Hub Scores, node types, dependency chains, expansion paths, global priority ranking | Dependency mapping (Step 5) and cluster weighting (Advanced Logic) |
| `internal-linking.skill`              | Funnel gap register, orphan page list, pillar reinforcement needs                  | Dependency mapping and sequencing priority                         |
| `content-awareness.skill`             | Current coverage state per cluster, published/planned pages                        | Avoiding re-planning existing content; identifying what already exists |
| `content-inventory.skill`             | Full published and planned content list                                            | Deduplication — not re-scheduling what is already published         |
| `production_capacity`                 | Content team's monthly capacity: articles per month (default: 4 max)              | Step 7 workload cap enforcement                                     |
| `horizon`                             | Planning horizon in months (default: 12)                                           | Timeline distribution across phases                                 |
| `priority_clusters`                   | User-designated priority clusters that must be scheduled early regardless of score | Step 4 short-term override                                          |

### Input Pre-Conditions

1. **Are gap opportunities available?** If `gap-opportunity-engine.skill` output is absent → cannot build a meaningful roadmap. Log: "Gap opportunity data required — run gap-opportunity-engine.skill before roadmap generation."
2. **Is the authority map available?** If absent → produce roadmap without cluster authority weighting; flag: "Authority map absent — sequencing based on opportunity scores only."
3. **Is production capacity specified?** If not → default to 4 items per month.
4. **Is the planning horizon specified?** If not → default to 12 months.
5. **Are content inventory and content awareness data available?** If yes → filter out already-published or already-planned items before roadmap items are selected. If no → produce the full roadmap; note that some items may already be in production.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Content Execution Roadmap Package

```
NEXUS CONTENT EXECUTION ROADMAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain                  : [domain]
Planning Horizon        : [N months]
Production Capacity     : [N items/month]
Total Items Roadmapped  : [count]
Total Items in Backlog  : [count — items beyond the planning horizon]
Clusters Addressed      : [count]
  Fully Addressed       : [count — all HIGH gaps within horizon]
  Partially Addressed   : [count — some HIGH gaps within horizon]
  Not Yet Addressed     : [count — below horizon cutoff]
Funnel Distribution:
  TOFU items            : [count] — [%]
  MOFU items            : [count] — [%]
  BOFU items            : [count] — [%]
Generated By            : strategy/roadmap-engine.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — ROADMAP TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Phase       | Month    | #  | Topic                                      | Target Keyword                    | Intent | Cluster           | Priority | Type        | Depends On | Rationale                                     |
|-------------|----------|----|-------------------------------------------|-----------------------------------|--------|-------------------|----------|-------------|------------|-----------------------------------------------|
| SHORT-TERM  | Month 1  | 1  | [specific topic]                          | [keyword]                         | BOFU   | [cluster name]    | HIGH     | NEW         | NONE       | [specific rationale]                          |
| SHORT-TERM  | Month 1  | 2  | [specific topic]                          | [keyword]                         | BOFU   | [cluster name]    | HIGH     | NEW         | NONE       | [specific rationale]                          |
| SHORT-TERM  | Month 1  | 3  | [specific topic]                          | [keyword]                         | MOFU   | [cluster name]    | HIGH     | NEW         | #1         | [specific rationale]                          |
| SHORT-TERM  | Month 1  | 4  | [specific topic]                          | [keyword]                         | MOFU   | [cluster name]    | HIGH     | REFRESH     | NONE       | [specific rationale]                          |
| SHORT-TERM  | Month 2  | 5  | [specific topic]                          | [keyword]                         | MOFU   | [cluster name]    | HIGH     | NEW         | NONE       | [specific rationale]                          |
| SHORT-TERM  | Month 2  | 6  | [specific topic]                          | [keyword]                         | TOFU   | [cluster name]    | HIGH     | NEW (PILLAR)| NONE       | [specific rationale]                          |
| SHORT-TERM  | Month 2  | 7  | [specific topic]                          | [keyword]                         | TOFU   | [cluster name]    | MEDIUM   | NEW         | #6         | [specific rationale]                          |
| SHORT-TERM  | Month 2  | 8  | [specific topic]                          | [keyword]                         | BOFU   | [cluster name]    | HIGH     | NEW         | NONE       | [specific rationale]                          |
| MID-TERM    | Month 3  | 9  | [specific topic]                          | [keyword]                         | TOFU   | [cluster name]    | HIGH     | NEW (PILLAR)| NONE       | [specific rationale]                          |
...

TYPE LEGEND:
  NEW         : New content to be created from scratch
  NEW (PILLAR): New pillar page — must be published before its supporting content
  REFRESH     : Existing content to be optimized using optimization-engine.skill
  EXPANSION   : Adding a new section or page to extend an existing cluster

PHASE LEGEND:
  SHORT-TERM  : Month 1–2 — highest opportunity; quick wins; BOFU/MOFU anchors
  MID-TERM    : Month 3–5 — cluster expansion; supporting content; authority building
  LONG-TERM   : Month 6+   — TOFU scaling; depth expansion; authority compounding

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — DEPENDENCY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[See Dependency Map format in Section 3]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — PHASE EXECUTION NOTES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[See Phase Notes format in Section 3]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — BACKLOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Items beyond the planning horizon — prioritized for the next planning cycle]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : strategist-ai.skill
                         strategy-memory.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Dependency Map Format (Part 2)

```
DEPENDENCY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CLUSTER: [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
#[N] [Pillar Topic]  ← must publish first
  └──→ #[N+1] [Supporting Topic 1]   (depends on #N)
  └──→ #[N+2] [Supporting Topic 2]   (depends on #N)
  └──→ #[N+3] [BOFU Topic]           (depends on #N and #N+1)
        └──→ #[N+4] [Deep Dive]      (depends on #N+3)

CLUSTER: [Cluster 2 Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
#[M] [Pillar Topic]
  └──→ #[M+1] [Supporting Topic]     (depends on #M)
  └──→ #[M+2] [MOFU Comparison]      (depends on #M; also benefits from #N being live)

CROSS-CLUSTER DEPENDENCY:
  #[M+2] also benefits from #[N] being live — links between clusters improve after both are published.

HARD DEPENDENCIES (must not schedule before):
  #[N+1] requires #[N] → cannot schedule in the same month as #[N]
  #[M+2] requires #[M] → cannot schedule in the same month as #[M]

SOFT DEPENDENCIES (prefer after, but not required):
  #[N+3] ideally published after #[N+1] and #[N+2] are live — linking improves
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Phase Execution Notes Format (Part 3)

```
PHASE NOTES: SHORT-TERM (Month 1–2)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Strategic Objective:
  [What this phase is designed to achieve in 1–2 sentences]

Why These Items First:
  [Specific rationale for the short-term selection — opportunity scores, gaps, quick wins]

Expected Impact by End of Phase:
  Authority: [what authority changes are expected]
  Traffic: [what traffic pattern changes to expect]
  Revenue: [what conversion changes to expect if BOFU items are included]

Dependencies Resolved in This Phase:
  [list of items that unlock future items]

Month 1 Notes:
  Item #1: [why this item goes in Month 1 specifically — not Month 2]
  Item #2: [rationale]
  Item #3: [rationale]
  Item #4: [rationale]

Month 2 Notes:
  Item #5: [why Month 2]
  ...

PHASE NOTES: MID-TERM (Month 3–5)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[same format]

PHASE NOTES: LONG-TERM (Month 6+)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[same format]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — GROUP BY CLUSTER

Before any sequencing begins, organize all content opportunities into their cluster architecture. Sequencing across clusters without first understanding the cluster context produces incoherent roadmaps where a site might publish five articles from five different clusters in Month 1 — building no meaningful authority in any of them.

**Phase 1A — Content Pool Construction**

Aggregate all content items from the input sources into a single content pool:

1. Load all gap opportunities from `gap-opportunity-engine.skill` that received a priority of HIGH or MEDIUM.
2. Load all HIGH priority actions from `authority-engine.skill` action priority queue.
3. Load all HIGH and MEDIUM opportunity keywords from `opportunity-engine.skill` scored output.
4. If content inventory is available → remove any item that matches a published or planned page (de-duplicate against existing content).
5. If duplicate items appear from multiple sources (same keyword from both gap engine and opportunity engine) → keep the single record with the highest priority and richest metadata.

**Phase 1B — Cluster Assignment**

For each item in the content pool:
1. Assign the item to its cluster using the `cluster` field from the gap engine output.
2. If an item has no cluster assignment → infer from the keyword's position in the cluster architecture. If still unclear → assign to a provisional "UNCLASSIFIED" group and flag for review.
3. Verify the cluster assignment is consistent with the semantic-clustering architecture.

**Phase 1C — Cluster Content Pool Summary**

Produce a cluster-level summary of the content pool:

```
CLUSTER CONTENT POOL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster: [Name] | Strength: [N/100] | Authority: [N/5]
  HIGH items: [count]
  MEDIUM items: [count]
  Pillar exists: [YES / NO — needs creation]
  Funnel coverage: TOFU [N] / MOFU [N] / BOFU [N]
  Total items: [count]

Cluster: [Name] | Strength: [N/100] | Authority: [N/5]
  ...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total clusters with content items: [count]
Total content pool size           : [count]
```

---

### ▸ STEP 2 — PILLAR-FIRST LOGIC

Apply the pillar-first sequencing rule within every cluster. This rule is absolute: within any given cluster, the pillar page is always scheduled before its supporting content.

**Phase 2A — Pillar Status Check Per Cluster**

For each cluster in the content pool:
1. Does a pillar page for this cluster already exist (published)?
2. If YES → the pillar constraint is already satisfied; supporting content may be scheduled freely.
3. If NO → the pillar page must be the first item scheduled for this cluster, regardless of whether it has the highest individual opportunity score in the cluster.

**Phase 2B — Pillar Page Identification in Content Pool**

If the cluster has no published pillar and a pillar page appears in the content pool (from gap engine or authority engine):

- That pillar item receives **MANDATORY FIRST** status within its cluster.
- No supporting content from this cluster may be scheduled before the pillar item in the roadmap.

If the cluster has no published pillar and no pillar page appears in the content pool:

- Create a pillar page requirement and insert it as Item 0 of the cluster.
- Flag it: "PILLAR CREATION REQUIRED — [Cluster Name] has no pillar page. Schedule this before supporting content."
- This manufactured pillar requirement receives the cluster's highest opportunity score by default.

**Phase 2C — Supporting Content Sequencing Within Cluster**

Once the pillar is accounted for, sequence the supporting content within the cluster:

| Content Type               | Sequencing Position Within Cluster                                     |
|----------------------------|------------------------------------------------------------------------|
| Pillar page (TOFU/mixed)   | Position 1 — always first                                             |
| MOFU supporting pages      | Position 2–3 — after pillar; they link to the pillar and to each other |
| BOFU conversion pages      | Position 3–4 — after at least one MOFU page establishes context       |
| Deep TOFU supporting pages | Position 3+ — after pillar establishes the hub; these expand coverage |
| Micro-cluster niche pages  | Last — after the core cluster structure is established                 |

**Phase 2D — Minimum Viable Cluster Rule**

A cluster achieves Minimum Viable Depth when it has:
- 1 published pillar page.
- 1 published MOFU or supporting page.
- 1 published BOFU or conversion page (or strong CTA within the MOFU page).

A cluster that achieves minimum viable depth before another cluster begins is more valuable than a cluster with 10 pages that are all TOFU with no conversion path. The roadmap prioritizes closing clusters to minimum viable depth before beginning deep expansion of any single cluster.

---

### ▸ STEP 3 — FUNNEL MAPPING

Assign every roadmap item its funnel stage and ensure the overall roadmap maintains a balanced funnel distribution.

**Phase 3A — Funnel Stage Assignment**

For each item in the content pool:
1. Import the funnel stage (TOFU / MOFU / BOFU) from the gap engine or opportunity engine record.
2. If no funnel stage is assigned → infer from the item's intent classification:
   - Informational intent → TOFU.
   - Commercial intent → MOFU.
   - Transactional intent → BOFU.
3. Document the funnel stage for every item before sequencing.

**Phase 3B — Funnel Balance Assessment**

Calculate the funnel distribution across the full content pool:

| Funnel Balance State           | Condition                                          | Implication                                                     |
|--------------------------------|----------------------------------------------------|-----------------------------------------------------------------|
| **HEALTHY**                    | TOFU 40–60% / MOFU 25–40% / BOFU 15–25%          | Site can attract, evaluate, and convert                        |
| **TOFU-HEAVY**                 | TOFU > 65%                                         | Site attracts traffic but cannot convert it                    |
| **BOFU-HEAVY**                 | BOFU > 35%                                         | Site converts well but has limited organic reach               |
| **MOFU-ABSENT**                | MOFU < 15%                                         | Evaluation stage gap — readers jump from awareness to decision |
| **CONVERSION-MISSING**         | BOFU = 0                                           | Site cannot convert any organic traffic to revenue             |

When funnel imbalance is detected → apply funnel balancing corrections in Phase 3C.

**Phase 3C — Funnel Balancing Correction**

| Detected Imbalance       | Correction Action                                                                               |
|--------------------------|-------------------------------------------------------------------------------------------------|
| TOFU-HEAVY               | Elevate MOFU and BOFU items in the sequencing; delay lowest-priority TOFU items to later phases |
| CONVERSION-MISSING       | Immediately flag the absence of BOFU content as a CRITICAL gap; insert BOFU creation into SHORT-TERM phase regardless of cluster position |
| MOFU-ABSENT              | Schedule at least one MOFU item per active cluster in the SHORT-TERM or early MID-TERM phases   |
| BOFU-HEAVY               | Note this is a conversion-focused strategy (acceptable for established sites); flag for user awareness |

**Phase 3D — Funnel Mapping Output**

After funnel balance is assessed and corrections are applied:

```
FUNNEL DISTRIBUTION MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Roadmap total items    : [count]
TOFU items             : [count] ([%]) — [HEALTHY / HIGH / LOW]
MOFU items             : [count] ([%]) — [HEALTHY / HIGH / LOW]
BOFU items             : [count] ([%]) — [HEALTHY / HIGH / LOW]
Overall balance        : [HEALTHY / TOFU-HEAVY / BOFU-HEAVY / MOFU-ABSENT / CONVERSION-MISSING]
Corrections applied    : [list of corrections if any]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 4 — TIMELINE PLANNING

Distribute roadmap items across three planning phases based on priority, opportunity score, and strategic sequencing logic.

**Phase 4A — Phase Definitions**

| Phase        | Month Range | Strategic Purpose                                                                  |
|--------------|-------------|------------------------------------------------------------------------------------|
| SHORT-TERM   | Month 1–2   | Capture the fastest, highest-impact opportunities; establish quick wins; prioritize BOFU and high-gap MOFU content; resolve critical authority gaps |
| MID-TERM     | Month 3–5   | Expand cluster coverage with supporting content; build minimum viable depth in all active clusters; begin TOFU authority building |
| LONG-TERM    | Month 6+    | Scale TOFU content for compounding traffic; deepen authority in clusters at Score 3–4; expand to secondary clusters |

**Phase 4B — SHORT-TERM Item Selection Criteria (Month 1–2)**

Items qualify for the SHORT-TERM phase if they meet the criteria for any of these selection triggers:

| Selection Trigger                                                                    | Priority Given    |
|--------------------------------------------------------------------------------------|-------------------|
| Opportunity Score ≥ 8 (HIGH) from `opportunity-engine.skill`                        | Always SHORT-TERM |
| BOFU content in a cluster with authority score 0–1 (no conversion path exists)     | Always Month 1    |
| Authority engine gap urgency = CRITICAL (BOFU cluster with score 0–1)               | Always Month 1    |
| Gap type = EMERGING TOPIC (first-mover advantage window is closing)                 | Month 1–2         |
| Gap type = MISSING TOPIC in a cluster with strength ≥ 70                           | Month 1–2         |
| User-designated priority cluster item                                               | Month 1–2 override|
| Quick win: HIGH opportunity + EASY SERP + CONTENT_EMPTY                             | Month 1           |

**Phase 4C — SHORT-TERM Overload Handling**

If more items qualify for SHORT-TERM than the production capacity allows (4 items × 2 months = 8 items maximum):

1. Rank all qualifying items by opportunity score (descending).
2. Within the same opportunity score → rank by BOFU > MOFU > TOFU.
3. Fill Month 1 with the top 4; fill Month 2 with the next 4.
4. Move all remaining qualifying items to MID-TERM Month 3.
5. Flag: "SHORT-TERM queue exceeded capacity — [N] HIGH priority items moved to Month 3. Consider expanding capacity."

**Phase 4D — MID-TERM Item Selection Criteria (Month 3–5)**

Items qualify for MID-TERM if:
- They are cluster expansion content that supports Short-Term pillar pages now being indexed.
- They are supporting pages for clusters that achieved minimum viable depth in SHORT-TERM.
- They are TOFU content for clusters where the MOFU/BOFU foundation was built in SHORT-TERM.
- Their opportunity score is MEDIUM (5–7) but they build cluster depth for a HIGH-strength cluster.
- They address the second-tier gap urgency (IMPORTANT from authority-engine).

**Phase 4E — LONG-TERM Item Selection Criteria (Month 6+)**

Items qualify for LONG-TERM if:
- They are TOFU content for clusters that are already adequately served with MOFU/BOFU.
- They are opportunity score LOW (0–4) items that require a cluster foundation to be worth creating.
- They are secondary cluster expansion items — deepening coverage beyond minimum viable depth.
- They are cross-cluster bridging content that benefits from multiple cluster foundations being live.
- They are OPTIONAL gap items that would add value but are not strategic priorities.

**Phase 4F — Monthly Distribution and Workload Cap**

Apply the production capacity cap to every month in the roadmap:

| Capacity Setting          | Items Per Month Cap | Override Condition                                   |
|---------------------------|---------------------|------------------------------------------------------|
| DEFAULT                   | 4                   | No override — this is the realistic production rate  |
| User-specified `production_capacity` | [user value] | Applied instead of default                |
| OVERLOAD CONDITION        | > capacity in queue | Surface excess to BACKLOG (Part 4 of output)        |

**Monthly distribution rule:** No month may contain fewer than 2 items unless there are literally fewer than 2 items remaining in the entire queue. Every month that falls within the planning horizon should be meaningfully productive.

---

### ▸ STEP 5 — DEPENDENCY MAPPING

Define the dependencies between roadmap items — which items must be published before others, and which items create mutual value when they are both live.

**Phase 5A — Hard Dependency Types**

A hard dependency means Item B CANNOT be usefully scheduled before Item A is published.

| Dependency Type                                                                     | Example                                                                    |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| **PILLAR-DEPENDENCY**                                                               | Supporting content cannot link to its pillar if the pillar does not exist |
| **FUNNEL-DEPENDENCY**                                                               | BOFU comparison content cannot bridge FROM a TOFU article that doesn't exist yet |
| **ENTITY-DEPENDENCY**                                                               | An article assuming familiarity with Concept X cannot precede the article that defines Concept X |
| **PREREQUISITE-CHAIN**                                                              | Advanced guide to Topic B requires the foundational guide to Topic A to be live |
| **INTERNAL-LINK-DEPENDENCY**                                                        | A page designed to receive links from a cluster pillar needs that pillar published first for links to flow |

Hard dependencies are marked in the roadmap table's "Depends On" column with the item number.

**Hard Dependency Rule:** Items with hard dependencies cannot be scheduled in the same month as the item they depend on. They must be scheduled at least one month later.

**Phase 5B — Soft Dependency Types**

A soft dependency means Item B can be published before Item A, but the combined value increases significantly when A is live:

| Soft Dependency Type                                                                | Example                                                                    |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| **LINKING-BENEFIT**                                                                 | Item B will link TO Item A once A is live — publishing A later doesn't block B, but delays link-benefit |
| **CLUSTER-DEPTH-BENEFIT**                                                           | Item B ranks better after other cluster content establishes the cluster's topical authority |
| **CROSS-REFERENCE**                                                                 | Items A and B reference each other; value compounds when both are live    |

Soft dependencies are noted in execution notes — they do not constrain scheduling but inform optimal sequencing.

**Phase 5C — Dependency Chain Detection**

Build the full dependency tree from the roadmap items:

1. For each item, identify all items it depends on (hard or soft).
2. Check for circular dependencies (A depends on B; B depends on A) → these are invalid; flag and resolve by identifying which item is more foundational and breaking the circular reference.
3. Build the dependency tree structure for the output dependency map.

**Phase 5D — Critical Path Identification**

The critical path is the sequence of items with the most dependencies — the items that unlock the most other items when published.

Identify the critical path:
1. For each item, count how many other items depend on it (directly or transitively).
2. Items with the highest dependency count are on the critical path.
3. Critical path items are always prioritized over non-critical path items at the same opportunity score level.

---

### ▸ STEP 6 — SEQUENCE VALIDATION

After all items have been assigned to months and dependencies have been mapped, validate the complete sequence for conflicts, gaps, and imbalances.

**Phase 6A — Dependency Conflict Check**

For every item in the roadmap:
1. Is the item scheduled AFTER all its hard dependencies? If NO → move the item forward one month.
2. Is the dependent item scheduled in the same month as its dependency? If YES → move the dependent item to the next month.
3. Re-check after every adjustment — a single move may create a cascade of adjustments.
4. If adjustments cause an item to fall outside the planning horizon → move to BACKLOG.

**Phase 6B — Logical Progression Check**

Verify the roadmap tells a coherent story of authority building:

| Check                                                                        | Pass Condition                                              |
|------------------------------------------------------------------------------|-------------------------------------------------------------|
| No supporting page is scheduled before its pillar page                      | Pillar always precedes its supporting pages in the plan    |
| No BOFU page is scheduled without any preceding MOFU page in the same cluster | At minimum one MOFU page is live or scheduled before BOFU in the same cluster |
| No cluster expansion begins in MID-TERM without any SHORT-TERM foundation   | At least one item per cluster in SHORT-TERM if cluster has HIGH items |
| Critical path items are not in LONG-TERM when there are no dependencies blocking them | Critical path items should be as early as possible |
| No more than 2 items from the same cluster in the same month                | Avoids month-to-month topic repetition and over-concentrating on one cluster |

**Phase 6C — Funnel Balance Final Check**

Apply the final funnel distribution check to the completed roadmap (not just the pool):

Within each phase (SHORT / MID / LONG), verify:
- SHORT-TERM: BOFU + MOFU ≥ 50% of phase items (quick wins must include conversion-path content).
- MID-TERM: Balanced mix (no single funnel stage > 60% of phase items).
- LONG-TERM: TOFU acceptable at higher percentage (authority scaling is the objective here).

If any phase fails the funnel check → apply funnel rebalancing: swap the lowest-priority item of the over-represented funnel stage with the highest-priority item of the under-represented stage from adjacent months.

**Phase 6D — Sequence Validation Log**

After all checks and adjustments:

```
SEQUENCE VALIDATION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Dependency conflicts found      : [count]
Dependency conflicts resolved   : [count]
Items moved due to conflict     : [list]
Logical progression issues      : [count and description]
Funnel balance corrections      : [count and description]
Items flagged NEEDS REVIEW      : [count]
Final validation status         : [PASSED / PASSED WITH FLAGS / REQUIRES REVISION]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 7 — OUTPUT TABLE PRODUCTION

Produce the final formatted roadmap table with all validated and sequenced items.

**Phase 7A — Roadmap Table Assembly**

For each month in the planning horizon:
1. List all items assigned to that month (maximum 4 per month after capacity enforcement).
2. Sort within the month: BOFU before MOFU before TOFU; within same funnel stage, by opportunity score descending.
3. Assign sequential item numbers across the full roadmap (not restarting per month).
4. Record the "Depends On" field using item numbers.

**Phase 7B — Roadmap Table Column Definitions**

| Column            | Content                                                                                          |
|-------------------|--------------------------------------------------------------------------------------------------|
| Phase             | SHORT-TERM / MID-TERM / LONG-TERM                                                               |
| Month             | Month [N] — using planning month numbers (not calendar months unless user specifies)            |
| #                 | Sequential item number across the full roadmap                                                  |
| Topic             | Human-readable editorial topic description (8–15 words max)                                    |
| Target Keyword    | The exact primary keyword this content will target                                              |
| Intent            | TOFU / MOFU / BOFU                                                                              |
| Cluster           | The cluster this item belongs to                                                                |
| Priority          | HIGH / MEDIUM / LOW — from opportunity-engine score                                             |
| Type              | NEW / NEW (PILLAR) / REFRESH / EXPANSION                                                        |
| Depends On        | Item number(s) this item has a hard dependency on; NONE if no dependencies                    |
| Rationale         | One specific sentence explaining why this item is in this month (not a generic priority label) |

**Phase 7C — Rationale Quality Standard**

The rationale field for every item must answer: "Why is THIS item in THIS month specifically?"

**Prohibited rationale forms:**
- "HIGH priority item" — restates the priority column; adds no sequencing logic.
- "Important for SEO" — vague; no specificity.
- "Builds cluster authority" — generic; does not explain why THIS month.
- "Good opportunity" — explains nothing.

**Required rationale forms:**

| Item Context                                     | Required Rationale Content                                                                  |
|--------------------------------------------------|---------------------------------------------------------------------------------------------|
| Month 1 BOFU item                                | "BOFU page in [cluster] with zero existing content and EASY SERP — direct revenue path with no competition." |
| Month 2 pillar after BOFU                        | "Pillar page for [cluster] — publishes AFTER Month 1 BOFU establishes initial authority; enables all subsequent cluster supporting pages to receive internal links." |
| Month 3 supporting MOFU                          | "First supporting page for [cluster] pillar (Item #N) — expands MOFU coverage after pillar is indexed; enables funnel bridge from Item #N's TOFU article." |
| MID-TERM TOFU                                    | "TOFU expansion for [cluster] — cluster foundation complete after Month 1–2; TOFU content now compounds authority on an established hub." |

---

### ▸ STEP 8 — EXECUTION NOTES

Produce phase-level execution notes that explain the strategic logic of each phase and the specific reasoning behind each monthly allocation.

**Phase 8A — Phase-Level Strategic Notes**

For each phase, produce:

1. **Strategic Objective:** What this phase is designed to achieve for the site's authority and traffic profile.
2. **Why These Items First:** The specific reason these items were selected over alternatives.
3. **Expected Impact by End of Phase:** Specific, measurable expected changes in content coverage, authority scores, and traffic/conversion potential.
4. **Dependencies This Phase Resolves:** Which future items are unlocked by completing this phase.

**Phase 8B — Item-Level Execution Notes**

For each month, produce notes for each item that explain:
- Why this item was placed in this specific month.
- What happens if this item is delayed.
- How this item connects to adjacent items (what it links to, what links to it).
- What success looks like for this specific piece.

**Item-Level Note Quality Standard:**

```
ITEM EXECUTION NOTE — Month [N], Item #[N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Topic           : [topic]
Target Keyword  : [keyword]
Why This Month  : [specific explanation — not generic]
If Delayed      : [specific consequence of delaying this item by one month]
Links To        : [which items this page will internally link to when published]
Linked From     : [which items will link to this page — from the internal linking plan]
Success Signal  : [what a successful outcome for this page looks like — specific]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ CLUSTER PRIORITY WEIGHTING

Not all clusters contribute equally to a site's strategic authority. This advanced module weights the priority of cluster items based on the cluster's overall strategic importance.

**Cluster Weight Factors:**

| Factor                                             | Weight Input                                          |
|----------------------------------------------------|-------------------------------------------------------|
| Cluster strength score from `semantic-clustering` | Primary: 0–100 score, normalized to 0–1              |
| Authority score from `authority-engine`            | Secondary: 0–5 score (low score = high urgency)      |
| Hub Score from `query-graph`                       | Tertiary: 0–100 Hub Score (higher = more central)    |
| User priority designation                          | Override: user-designated clusters receive weight 1.0 |

**Cluster Priority Weight Formula:**

`Cluster_Weight = (Cluster_Strength × 0.5) + ((5 - Authority_Score) × 0.3) + (Hub_Score / 100 × 0.2)`

The first term rewards high-strength clusters (they are more valuable to build).
The second term rewards low-authority clusters (they have more room to gain).
The third term rewards central clusters (they influence more of the topic graph).

**Application:** When two items from different clusters have the same opportunity score, the item from the higher-weight cluster is scheduled first.

---

### ▸ OPPORTUNITY-BASED SEQUENCING

Within phases, items are sequenced by their opportunity score from `opportunity-engine.skill`. This module applies the sequencing algorithm.

**Sequencing Algorithm:**

1. **Within Phase — Primary Sort:** Opportunity Score descending (highest first).
2. **Tie-breaking — Level 1:** BOFU > MOFU > TOFU (conversion-stage content produces faster measurable impact).
3. **Tie-breaking — Level 2:** Cluster weight (higher cluster weight items sequence first).
4. **Tie-breaking — Level 3:** Gap type priority: MISSING TOPIC (highest) > MISSING INTENT > EMERGING TOPIC > MISSING DEPTH > MISSING FORMAT > MISSING FRESHNESS.
5. **Tie-breaking — Level 4:** Items on the critical path sequence first.
6. **Tie-breaking — Level 5 (last resort):** Alphabetical by cluster name.

**Opportunity Score Adjustment for Roadmap Use:**

The raw opportunity score from `opportunity-engine.skill` is adjusted for roadmap sequencing when:
- The item has a HARD DEPENDENCY that cannot be met until a prior item is published → reduce effective score by 20% (pushes dependent items later in sequencing without changing their underlying value).
- The item is a REFRESH (existing content to be optimized, not new content) → add 5% to effective score (refreshes produce faster ranking signals on content that already has some authority).
- The item is a PILLAR CREATION item → add 15% to effective score (pillars unlock all cluster content; the multiplier effect justifies prioritization).

---

### ▸ FUNNEL BALANCING (ADVANCED)

The funnel balancing algorithm monitors the rolling funnel distribution as items are assigned to months and makes real-time corrections.

**Rolling Funnel Check:**

After every 4-item monthly assignment is finalized, check the running funnel distribution:

| Running Funnel State                            | Correction                                                                  |
|-------------------------------------------------|-----------------------------------------------------------------------------|
| BOFU items < 15% of items assigned so far       | Next available BOFU item is elevated to the next available month slot       |
| MOFU items < 20% of items assigned so far       | Next available MOFU item is elevated to fill the next available month slot  |
| TOFU items > 65% of items assigned in any phase | Swap one TOFU item for the next available MOFU or BOFU item in the backlog  |

**Funnel Balancing Never Violates:**
- Hard dependencies.
- The pillar-first rule.
- The production capacity cap.

If a balancing correction is not possible without violating one of these rules → the imbalance is flagged and documented in the validation log as a FUNNEL IMBALANCE NOTE rather than forced.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Topics

No two roadmap items may cover the same editorial topic. A topic is considered repeated if the two items would, if both written, produce content with ≥70% semantic overlap. When a duplicate is detected:
- Keep the item with the higher opportunity score.
- Remove the duplicate.
- Note the removed item in the deduplication log.

### Rule 2 — No Duplicate Keywords

No two roadmap items may have the same target keyword. If two items from different sources (gap engine vs. opportunity engine) both target the same keyword → merge them into a single roadmap item using the richer metadata from either source. Log the merge.

### Rule 3 — No Re-Scheduling of Published or Planned Content

If content inventory is available → any roadmap item whose target keyword matches a published or in-pipeline page is removed from the roadmap automatically. Log: "Item '[topic]' targeting '[keyword]' removed — content already exists at [URL]."

### Rule 4 — One Pillar Per Cluster

Each cluster may have only one pillar page in the roadmap. If two items for the same cluster both appear to be pillar-level content → designate the broader (shorter, more general keyword) as the pillar; demote the other to a supporting page.

### Rule 5 — Memory Deduplication

Query `memory-controller.skill` (via `strategy-memory.skill`) for prior roadmap runs. If a prior roadmap exists for this domain → do not re-schedule items that appear in the prior plan with status COMPLETED. Items with status IN PROGRESS → keep in the new plan but note their prior schedule. Items with status CANCELLED → carry forward if they still have HIGH opportunity score; note the prior cancellation.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Roadmap Behaviors

| Prohibited Behavior                                                                              | Reason                                                                            |
|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Using "Build authority" as the rationale for any roadmap item                                   | Generic — every SEO item builds authority; the rationale must be specific        |
| Scheduling a supporting page before its cluster's pillar page                                   | Violates the pillar-first rule — supporting pages need a hub to link to          |
| Producing a roadmap with no BOFU items in the first three months for a site with no conversions | If there is zero BOFU content, BOFU creation must be in SHORT-TERM               |
| Assigning more than 2 items from the same cluster to the same month                             | Over-concentration — authority building is broader than one cluster per month    |
| Leaving "NEEDS REVIEW" items unresolved without a specific decision point                        | Each NEEDS REVIEW item must have a resolution recommendation                     |
| Scheduling items beyond the planning horizon without placing them in BACKLOG                    | Items beyond the horizon are backlog — not invisible deferrals                   |
| Using vague topic descriptions ("SEO guide", "CRM article", "marketing content")               | Every topic must be specific enough to brief a writer without additional research |
| Producing a roadmap where all LONG-TERM items are TOFU without explanation                     | LONG-TERM TOFU-only is valid but must be explicitly noted as a deliberate choice  |
| Generating a rationale that simply restates the priority level                                   | "HIGH priority" is not a rationale — it is the result of scoring                |
| Skipping dependency mapping when items have clear prerequisite relationships                     | Dependencies must always be detected and documented — never assumed to be obvious |

### Required Roadmap Behaviors

- Every roadmap item must be specific enough to brief directly to a writer.
- Every phase must include its strategic objective in the execution notes.
- Every month must have no more than 4 items (or the specified capacity cap).
- Every item with hard dependencies must show those dependencies in the Depends On column.
- The dependency map must visually show the full dependency tree for all clusters.
- The backlog must be populated with all items that couldn't fit within the planning horizon.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary content idea input)                                             |
| **Receives**     | Content ideas with target keyword, intent, cluster, gap type, priority, strategic reason      |
| **Used for**     | Step 1 (content pool), Step 2 (cluster assignment), Step 4 (SHORT-TERM selection)            |

---

### ▸ opportunity-engine.skill

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (keyword opportunity scoring)                                            |
| **Receives**     | Opportunity scores (0–10), priority labels, reason records per keyword                       |
| **Used for**     | Step 4 (timeline selection criteria), Advanced Logic (opportunity-based sequencing)           |

---

### ▸ authority-engine.skill

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (cluster authority state)                                                |
| **Receives**     | Authority scores per cluster, action priority queue, gap urgency, missing elements            |
| **Used for**     | Step 2 (pillar page status), Step 4 (SHORT-TERM selection), Step 3 (funnel gaps)             |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (cluster architecture)                                                   |
| **Receives**     | Cluster names, tier, strength scores, pillar designations                                     |
| **Used for**     | Step 1 (cluster pool), Step 2 (pillar-first logic), Advanced Logic (cluster weighting)        |

---

### ▸ query-graph.skill

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment                                                                  |
| **Receives**     | Hub Scores, dependency chains, expansion paths, global priority ranking                       |
| **Used for**     | Step 5 (dependency mapping), Step 6 (critical path), Advanced Logic (cluster weighting)       |

---

### ▸ internal-linking.skill

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment                                                                  |
| **Receives**     | Funnel gap register, orphan page list, pillar reinforcement needs                            |
| **Used for**     | Step 5 (dependency mapping — orphan pages need links before they can be removed from orphan list) |

---

### ▸ strategist-ai.skill

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | roadmap-engine → strategist-ai (downstream consumer)                                          |
| **Sends**        | Complete roadmap table, dependency map, phase execution notes, backlog                        |
| **Used for**     | Strategic recommendations, executive summaries, scenario analysis, phase adjustments         |

---

### ▸ strategy-memory.skill (via memory-controller)

| Property         | Detail                                                                                         |
|------------------|------------------------------------------------------------------------------------------------|
| **Direction**    | Write (after roadmap finalization)                                                            |
| **Writes**       | Complete roadmap (all 4 parts), item statuses, funnel distribution, cluster coverage state  |
| **Memory layer** | strategy-memory.skill                                                                         |
| **Used for**     | Cross-session continuity — tracking which items are COMPLETED, IN PROGRESS, or PENDING       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                           |
|---------------------------------------------------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Gap opportunity data is absent                                            | Input pre-condition — gap-opportunity-engine null      | Halt. "Gap opportunity data required for roadmap generation. Run gap-opportunity-engine.skill first."     |
| Content pool is empty after deduplication                                 | Step 1 — 0 items after filtering published content    | Flag: "All identified opportunities already exist as published or planned content. Roadmap complete — no new items needed at this time." |
| A cluster has items but no pillar and no pillar in the content pool       | Step 2 Phase 2B — no pillar found                     | Insert a manufactured pillar requirement as Item 0. Flag it as "PILLAR REQUIRED — must be created." Schedule it as the first item for that cluster. |
| All items are HIGH priority — exceeds capacity by >50%                   | Step 4 — HIGH item count > capacity × 3              | Apply strict opportunity score ranking. Log: "HIGH-priority items exceed [N]-month capacity. [N] items moved to BACKLOG. Consider expanding team capacity or extending horizon." |
| A dependency chain is circular (A depends on B; B depends on A)          | Step 5 — circular dependency detected                 | Break the cycle: identify the more foundational item (broader keyword, higher cluster strength). Make the other dependent. Flag: "Circular dependency between #[N] and #[M] — resolved by making #[M] dependent on #[N]." |
| All items fall into LONG-TERM — no SHORT-TERM candidates exist            | Step 4 — 0 items qualify for SHORT-TERM               | Review content pool: are all items LOW priority? Flag and ask user: "No HIGH or MEDIUM items found for SHORT-TERM. This may indicate the content opportunity analysis needs to be re-run with expanded parameters." |
| Funnel balancing cannot be achieved without violating pillar-first rule   | Step 6 Phase 6C — funnel correction conflicts with pillar rule | Log as FUNNEL IMBALANCE NOTE. Do not force the correction. Note: "Funnel balancing correction for [funnel stage] not applied — pillar-first rule prevents reordering. Address by creating [funnel stage] content in a separate new cluster item." |
| Memory controller unavailable for status check                            | Rule 5 — memory query fails                           | Proceed without prior roadmap data. Flag: "Prior roadmap status unavailable — roadmap generated fresh. Verify against existing content and in-progress items before assigning work." |
| An item's cluster cannot be identified                                    | Step 1 Phase 1B — cluster lookup fails                | Assign to UNCLASSIFIED. Flag for user review. Do not schedule in the roadmap until cluster is confirmed. Add to NEEDS REVIEW section. |
| Production capacity is set to 0 or null                                   | Step 4 Phase 4D — capacity is 0 or null              | Default to 4 items/month. Flag: "Production capacity not specified — using default capacity of 4 items per month." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — PHASE TIMELINE QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PHASE TIMELINE QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SHORT-TERM (Month 1–2):
  Objective    : Quick wins + conversion architecture
  What goes here:
    ✓ Opportunity score ≥ 8
    ✓ BOFU in clusters with 0 authority
    ✓ CRITICAL gaps from authority-engine
    ✓ Emerging topics (first-mover window)
    ✓ Quick wins (HIGH opportunity + EASY SERP + CONTENT_EMPTY)
    ✓ User-designated priority cluster items
  Max items    : 4/month × 2 months = 8 items total
  Expected end state: 1–2 clusters with minimum viable depth established

MID-TERM (Month 3–5):
  Objective    : Cluster expansion + supporting content depth
  What goes here:
    ✓ Opportunity score 5–7 (MEDIUM)
    ✓ Supporting pages for SHORT-TERM pillars
    ✓ TOFU content for clusters with MOFU/BOFU foundation built
    ✓ Second-tier gap urgency (IMPORTANT from authority-engine)
    ✓ Cluster expansion content
  Max items    : 4/month × 3 months = 12 items total
  Expected end state: 3–5 clusters at minimum viable depth; TOFU building begun

LONG-TERM (Month 6+):
  Objective    : Authority compounding + TOFU scaling
  What goes here:
    ✓ Opportunity score 0–4 (LOW) — only after HIGH/MEDIUM are complete
    ✓ Deep TOFU expansion for established clusters
    ✓ Secondary cluster development
    ✓ Cross-cluster bridge content
    ✓ OPTIONAL gap items
  Max items    : 4/month
  Expected end state: Deep topical authority across all targeted clusters

BACKLOG (Beyond horizon):
  Items that cannot fit within the planning horizon.
  Re-prioritized at the start of the next planning cycle.
  Reviewed for opportunity score changes at re-plan time.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — PILLAR-FIRST SEQUENCING GUIDE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
PILLAR-FIRST SEQUENCING GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RULE: Within every cluster, the pillar always comes first.
NO EXCEPTIONS regardless of the supporting page's opportunity score.

WHY: A supporting page published before its pillar cannot:
  → Link to its pillar (creating a broken hub-and-spoke model)
  → Receive cluster authority (no hub to aggregate to)
  → Signal topical depth (no hub to contextualize the sub-topic)

CLUSTER SEQUENCING ORDER (within any given cluster):
  1. Pillar page (MANDATORY FIRST)
  2. MOFU supporting page #1 (links to pillar; receives links from pillar)
  3. BOFU conversion page (built on MOFU foundation)
  4. TOFU depth page (expands reach on established hub)
  5. Additional MOFU supporting pages
  6. Micro-cluster niche pages

PILLAR CREATION VS. EXISTING PILLAR:
  Existing pillar (published) → constraint satisfied; go to #2
  Pillar in content pool (planned) → must schedule before all supporting items
  No pillar in content pool → insert PILLAR REQUIRED as manufactured item; schedule first

PILLAR-FIRST DOES NOT MEAN PILLAR-ONLY IN MONTH 1:
  A cluster pillar can share a month with items from OTHER clusters.
  Only the cluster's OWN supporting pages must wait.
  Example: Month 1 = [Cluster A Pillar] + [Cluster B BOFU] + [Cluster C BOFU] + [Cluster D MOFU]
  This is valid — each cluster item respects its own cluster's pillar-first rule.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — ROADMAP ITEM COMPLETENESS CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
ROADMAP ITEM COMPLETENESS CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Every roadmap item must pass these checks before appearing in the output table:

□ Phase is assigned (SHORT-TERM / MID-TERM / LONG-TERM)
□ Month is assigned (Month 1, 2, 3...)
□ Sequential # is assigned (unique across whole roadmap)
□ Topic is specific (≥8 words; no generic labels)
□ Target keyword is exact primary keyword (not a category)
□ Intent is assigned (TOFU / MOFU / BOFU)
□ Cluster is named (matching semantic-clustering cluster name)
□ Priority is assigned (HIGH / MEDIUM / LOW)
□ Type is assigned (NEW / NEW-PILLAR / REFRESH / EXPANSION)
□ Dependencies are listed (item number or NONE)
□ Rationale is specific (references the specific opportunity, gap, or sequencing reason — NOT "HIGH priority item")
□ Item is not a duplicate of a published or planned page
□ Item respects cluster capacity (≤2 items from same cluster in same month)
□ Item respects pillar-first rule (cluster pillar published in same or earlier month)
□ Item respects hard dependencies (dependent items in later month than their dependency)

NEEDS REVIEW items:
  → Opportunity score between HIGH and MEDIUM boundary
  → Cluster unclear (UNCLASSIFIED)
  → No specific gap evidence for the item
  → Dependency chain could not be resolved automatically
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: strategy/roadmap-engine.skill*
*Nexus SEO Operating System — Version 1.0.0*
