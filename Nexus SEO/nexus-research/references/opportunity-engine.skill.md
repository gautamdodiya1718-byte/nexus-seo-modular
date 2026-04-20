# SKILL: analysis/opportunity-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Analysis / Strategic Prioritization
# EXECUTION PRIORITY: POST-CLUSTERING AND POST-SERP — Runs after semantic-clustering.skill and serp-intelligence.skill are complete.
# POSITION: Strategic filter layer. Determines which keywords deserve execution investment before any content is created.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **keyword opportunity evaluation and prioritization engine** of the Nexus system.

It receives the organized keyword universe and evaluates every keyword against five strategic dimensions to determine which keywords represent genuine, high-impact opportunities — and which would waste editorial resources on low-probability, high-competition, or low-value targets.

Every keyword in a site's opportunity set is not equal. Some fill a confirmed gap with weak competition and high conversion intent. Others exist in saturated SERPs with strong brand dominance and no realistic differentiator. Building content for both at equal priority is the most common failure mode in SEO strategy. This skill prevents that failure by producing a scored, ranked, reason-backed priority list that the roadmap engine can execute against directly.

The opportunity score is not a search volume estimate. It is not a keyword difficulty score. It is a composite strategic signal — one that weights what matters: whether the SERP is weak, whether a gap exists, whether conversion intent is present, whether the cluster is architecturally critical, and whether a differentiation angle exists that competitors have missed.

### Primary Responsibilities

| Responsibility                        | Description                                                                                                         |
|---------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| **Scoring Dimension Definition**      | Apply five precisely defined scoring dimensions to every keyword in the input universe                              |
| **Per-Keyword Score Calculation**     | Calculate a total opportunity score (0–10) for every keyword based on the five dimension scores                    |
| **Priority Assignment**               | Classify every keyword as HIGH, MEDIUM, or LOW priority based on total score                                       |
| **Reason Generation**                 | Produce a specific, data-tied reason statement for every keyword's score — no generic explanations permitted        |
| **Sorted Output Production**          | Sort the complete keyword list by score descending, then by intent priority (BOFU > MOFU > TOFU)                   |
| **SERP Weakness Simulation**          | Evaluate SERP quality signals to determine how weak or strong the competitive landscape is for each keyword         |
| **Cluster Dependency Weighting**      | Apply additional weight to keywords in clusters that are architecturally critical to the site's topical authority  |
| **Strategic Prioritization Synthesis**| Produce cross-keyword strategic insights about where the highest-density opportunity clusters exist                 |
| **Memory Cross-Check**               | Verify no keyword in the scored output has already been actioned in prior sessions                                 |
| **Validation Flagging**               | When scoring confidence is low, mark keywords for human validation rather than assigning arbitrary scores           |

### What This Skill Is NOT

- It is **not** a keyword researcher. It scores what was discovered — it does not discover new keywords.
- It is **not** a live competition tracker. It works with SERP intelligence provided by `serp-intelligence.skill` — it does not perform live SERP queries.
- It is **not** a traffic estimator. Search volume is not a scoring dimension. Opportunity is about quality of the competitive landscape and strategic fit — not volume.
- It is **not** a final execution planner. It produces a scored priority list — `roadmap-engine.skill` converts that into a publishing sequence.
- It is **not** optional. No content roadmap may be built without first running opportunity scoring across the target keyword set.

### The Defining Standard of This Skill

> Every score must be justified.
> Every justification must reference specific signals — not general principles.
>
> It is better to assign a MEDIUM score with a clear explanation
> than a HIGH score with a vague rationale.
>
> Inflated scores waste editorial resources on low-probability targets.
> Deflated scores abandon winnable opportunities.
> Accurate scores — grounded in specific, evidence-backed reasoning — are the only acceptable output.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                          | Fields Consumed                                                                                      | Required |
|---------------------------------------|------------------------------------------------------------------------------------------------------|----------|
| `semantic-clustering.skill`           | Full cluster architecture — cluster names, keyword lists with intent/funnel, cluster strength scores, funnel balance, pillar designations | YES |
| `serp-intelligence.skill`             | Dominant intent per keyword, content type patterns, difficulty ratings, opportunity signals, SERP feature data, ranking pattern analysis | YES |
| `gap-opportunity-engine.skill`        | Gap matrix data — which clusters/topics are MISSING, GAP, PARTIAL, or CONTESTED                     | SOFT     |

### Optional Inputs

| Input Source                          | Fields Consumed                                                                    | Used For                                                          |
|---------------------------------------|------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| `deep-serp-analysis.skill`            | Per-page quality scores, parity requirements, differentiation angles               | Dimension 2 (SERP Weakness) and Dimension 5 (Differentiation Potential) enrichment |
| `competitor-analysis.skill`           | Competitor strength per cluster, weakness patterns, competitive blind spots         | Dimension 1 (Gap Level) and Dimension 4 (Cluster Importance) enrichment |
| `content-awareness.skill`             | Coverage status per cluster, freshness flags, funnel gap data                      | Dimension 4 (Cluster Importance) enrichment                       |
| `entity-extraction.skill`             | Critical entities per cluster                                                       | Dimension 5 (Differentiation Potential) — entity coverage angle  |
| `keyword-memory`                      | Prior session keyword assignments and scoring history                               | Memory cross-check (Step deduplication)                           |

### Per-Keyword Data Requirements

For every keyword entering the scoring engine, the following minimum data must be available:

| Field                          | Source                              | Required | Fallback if Missing                                           |
|--------------------------------|-------------------------------------|----------|---------------------------------------------------------------|
| `keyword`                      | Input keyword list                  | YES      | Halt — keyword string is non-negotiable                      |
| `intent`                       | semantic-clustering / serp-intel    | YES      | Infer from keyword pattern; flag as INFERRED                  |
| `funnel`                       | semantic-clustering / serp-intel    | YES      | Derive from intent; flag as INFERRED                          |
| `cluster_name`                 | semantic-clustering                 | YES      | Assign "UNCLASSIFIED"; flag for review                        |
| `cluster_strength`             | semantic-clustering                 | SOFT     | Default to 50 (medium); flag as ESTIMATED                    |
| `serp_difficulty`              | serp-intelligence                   | SOFT     | Default to MEDIUM; flag as UNVALIDATED                       |
| `gap_status`                   | gap-opportunity-engine              | SOFT     | Infer from serp_difficulty; flag as INFERRED                 |
| `serp_opportunity_signal`      | serp-intelligence                   | SOFT     | Derive from difficulty; flag as INFERRED                      |

### Input Pre-Conditions

1. **Has the keyword universe been clustered?** If not → request `semantic-clustering.skill` output first. Scoring without cluster context produces incomplete Dimension 4 scores.
2. **Is SERP intelligence available?** If not → proceed with structure-based scoring only; Dimensions 1 and 2 will be partially inferred; flag entire output as PARTIAL SERP DATA.
3. **Minimum keyword count:** No minimum — even a single keyword can be scored. However, if fewer than 5 keywords are provided, the strategic synthesis output (Part 3) will note limited scope.
4. **Has any keyword in the input been previously scored this session?** → Query `memory-controller.skill`. If found in current session → return cached score. Do not re-score within the same session unless the user explicitly requests a refresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Opportunity Intelligence Report

```
NEXUS OPPORTUNITY INTELLIGENCE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain / Topic Scope    : [domain or keyword universe scope]
Keywords Scored         : [count]
  HIGH Priority         : [count] — [%]
  MEDIUM Priority       : [count] — [%]
  LOW Priority          : [count] — [%]
Average Opportunity Score: [N/10]
SERP Data Status        : [FULL / PARTIAL / ABSENT]
Gap Data Status         : [FULL / PARTIAL / ABSENT]
Generated By            : analysis/opportunity-engine.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — SCORED KEYWORD TABLE (Sorted by Score Descending)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | Keyword                         | Cluster              | Intent  | D1  | D2  | D3  | D4  | D5  | Total | Priority | Flags         |
|---|---------------------------------|----------------------|---------|-----|-----|-----|-----|-----|-------|----------|---------------|
| 1 | [keyword]                       | [cluster name]       | BOFU    | 2   | 2   | 2   | 2   | 2   | 10    | HIGH     |               |
| 2 | [keyword]                       | [cluster name]       | BOFU    | 2   | 1   | 2   | 2   | 2   | 9     | HIGH     |               |
| 3 | [keyword]                       | [cluster name]       | MOFU    | 2   | 2   | 1   | 2   | 1   | 8     | HIGH     | INFERRED:D1   |
| 4 | [keyword]                       | [cluster name]       | TOFU    | 1   | 2   | 0   | 2   | 2   | 7     | MEDIUM   |               |
| 5 | [keyword]                       | [cluster name]       | MOFU    | 1   | 1   | 1   | 1   | 1   | 5     | MEDIUM   | NEEDS VALID.  |
| 6 | [keyword]                       | [cluster name]       | TOFU    | 0   | 1   | 0   | 0   | 1   | 2     | LOW      |               |
...

DIMENSION KEY:
  D1 = Gap Level (0–2)
  D2 = SERP Weakness (0–2)
  D3 = Intent Value (0–2)
  D4 = Cluster Importance (0–2)
  D5 = Differentiation Potential (0–2)
  Max Score = 10

FLAG LEGEND:
  INFERRED:D[N]   → Dimension [N] score was inferred (not directly from data)
  NEEDS VALID.    → Overall score confidence is LOW — manual validation recommended
  ESTIMATED       → One or more input fields were estimated due to missing data
  MEM-CONFLICT    → This keyword was scored differently in a prior session

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — PER-KEYWORD REASON RECORDS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[REPEATED FOR EACH KEYWORD — SORTED BY SCORE DESCENDING]

KEYWORD: [keyword] | Score: [N/10] | Priority: [HIGH/MEDIUM/LOW]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster             : [cluster name]
Intent / Funnel     : [intent] / [TOFU/MOFU/BOFU]

D1 — Gap Level      : [0/1/2] — [specific gap evidence]
  Evidence          : [which competitor data, gap matrix status, or SERP signal supports this score]

D2 — SERP Weakness  : [0/1/2] — [specific SERP weakness evidence]
  Evidence          : [what patterns in the SERP support this score — naming specific signals]

D3 — Intent Value   : [0/1/2] — [BOFU/MOFU/TOFU assignment with rationale]
  Evidence          : [why this intent classification is justified for this keyword]

D4 — Cluster Importance: [0/1/2] — [cluster strategic role]
  Evidence          : [cluster strength score, gap status, pillar designation if relevant]

D5 — Differentiation: [0/1/2] — [specific differentiation angle or absence]
  Evidence          : [what unique angle exists (or doesn't) for this keyword]

Opportunity Reason  : "[specific, non-generic one-sentence justification for the total score]"
Confidence Level    : [HIGH / MEDIUM / LOW]
Validation Flag     : [VALIDATED / NEEDS VALIDATION — reason]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — STRATEGIC SYNTHESIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

HIGHEST-OPPORTUNITY CLUSTER:
  Cluster             : [name]
  Avg Score           : [N/10]
  HIGH priority count : [N]
  Strategic Note      : [specific insight about this cluster's opportunity]

MOST CRITICAL GAP (Cluster with highest-scoring unaddressed keywords):
  Cluster             : [name]
  Top Keyword         : [keyword] — Score: [N]
  Gap Type            : [FULL GAP / SERP WEAKNESS / FUNNEL GAP]
  Recommendation      : [specific action]

HIGHEST BOFU DENSITY:
  Cluster             : [name]
  BOFU Keywords (HIGH+MEDIUM): [count]
  Revenue Impact      : [specific note on conversion potential]

QUICK WINS (HIGH priority + EASY difficulty + CONTENT_EMPTY):
  → [keyword] | Score: [N] | Cluster: [name] | Why: [reason]
  → [keyword] | Score: [N] | Cluster: [name] | Why: [reason]
  → [keyword] | Score: [N] | Cluster: [name] | Why: [reason]

LOWEST PRIORITY CLUSTER (resources better spent elsewhere):
  Cluster             : [name]
  Avg Score           : [N/10]
  Reason to Deprioritize: [specific evidence — saturated SERP, no gap, low differentiation]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — VALIDATION QUEUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Keywords marked NEEDS VALIDATION (score confidence LOW):

| # | Keyword                  | Score | Priority | Reason for Low Confidence                  | Recommended Action                      |
|---|--------------------------|-------|----------|--------------------------------------------|------------------------------------------|
| 1 | [keyword]                | [N]   | MEDIUM   | SERP data unavailable for this keyword     | Run serp-intelligence.skill for this KW |
| 2 | [keyword]                | [N]   | MEDIUM   | Gap status inferred — not confirmed        | Verify with gap-opportunity-engine.skill |
| 3 | [keyword]                | [N]   | LOW      | Cluster importance estimated               | Re-score after content-awareness.skill run|

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : roadmap-engine.skill
                         strategist-ai.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DEFINE SCORING DIMENSIONS

Before any keyword is scored, fully define and document the five scoring dimensions. Every scoring decision made in Step 2 must trace back to these definitions. No dimensional score may be assigned without consulting these exact criteria.

---

#### DIMENSION 1 — GAP LEVEL (0–2)

**What it measures:** The degree to which this keyword is under-served by existing content in the search results. A keyword with a high gap level has either no strong content or very weak content competing for it — making entry easier and first-mover advantage more achievable.

**Score Definitions:**

| Score | Label                    | Definition                                                                                         |
|-------|--------------------------|-----------------------------------------------------------------------------------------------------|
| **2** | NO STRONG CONTENT EXISTS | The SERP has no high-quality, authoritative result for this keyword. Either top results are outdated, thin, or misaligned with the true search intent. Alternatively, no result in the top-5 is definitively "owning" this keyword. |
| **1** | WEAK CONTENT EXISTS      | Some content exists targeting this keyword, but it is thin (low coverage score equivalent), outdated (STALE or AGING), or poorly structured (mismatched format for intent). A new, well-built piece could outperform. |
| **0** | STRONG COMPETITION       | Authoritative, comprehensive, recent content already exists from multiple strong domains. The SERP is well-served. Entering this keyword without a strong differentiation advantage would produce minimal results. |

**Scoring Signals for Dimension 1:**

| Signal Source                        | Score 2 Signal                                    | Score 1 Signal                              | Score 0 Signal                               |
|--------------------------------------|---------------------------------------------------|---------------------------------------------|----------------------------------------------|
| `gap-opportunity-engine.skill`       | Gap Status = FULL GAP or GAP                      | Gap Status = PARTIAL                        | Gap Status = CONTESTED                       |
| `serp-intelligence.skill`            | Difficulty = EASY                                 | Difficulty = MEDIUM                         | Difficulty = HARD                            |
| `deep-serp-analysis.skill`           | All page scores ≤3/5; multiple thin results       | Mixed page scores (1–4); some thin content  | Most pages scored ≥4/5; strong uniform quality |
| `competitor-analysis.skill`          | Competitor blind spot detected for this topic     | Competitors have weak coverage              | All competitors have strong coverage         |

**Gap Level Scoring Protocol:**

1. Check `gap-opportunity-engine.skill` gap status for the keyword's cluster and topic.
2. If gap status is available → apply directly using the signal table above.
3. If gap status is unavailable → use `serp-intelligence.skill` difficulty rating as the primary signal.
4. If both are unavailable → use keyword pattern and cluster position to infer:
   - Keyword is in a MISSING cluster → Score: 2 (inferred)
   - Keyword is in a GAP cluster → Score: 2 (inferred)
   - Keyword is in a PARTIAL cluster → Score: 1 (inferred)
   - Keyword is in a CONTESTED cluster → Score: 0 (inferred)
   - Flag inference in the output.

---

#### DIMENSION 2 — SERP WEAKNESS (0–2)

**What it measures:** The quality and strength of the current top-ranking content. A weak SERP is one where the existing results are poor — outdated, thin, structured incorrectly, not AEO-optimized, or failing to serve the true user intent. A strong SERP has well-built, comprehensive, authoritative content that would be genuinely difficult to outperform.

**Score Definitions:**

| Score | Label           | Definition                                                                                                    |
|-------|-----------------|---------------------------------------------------------------------------------------------------------------|
| **2** | POOR RESULTS    | The current top results have significant identifiable weaknesses: outdated content (18+ months with no update), thin coverage (under 1,000 words for a complex topic), format mismatch (e.g., a forum thread ranking for a how-to query), or missing AEO signals. A well-built piece can outperform these results. |
| **1** | AVERAGE RESULTS | Current results are adequate but not exceptional. Some content is well-written but lacks depth, freshness, or AEO optimization. Outperforming is possible with superior execution. |
| **0** | STRONG RESULTS  | Current top results are comprehensive, recent, well-structured, AEO-optimized, and authoritative. Outperforming them requires a genuine content innovation — not just doing the same thing better. |

**Scoring Signals for Dimension 2:**

| Signal Source                      | Score 2 Signal                                           | Score 1 Signal                               | Score 0 Signal                                   |
|------------------------------------|----------------------------------------------------------|----------------------------------------------|---------------------------------------------------|
| `serp-intelligence.skill`          | Opportunity Signal = EASY (specific evidence)            | Opportunity Signal = MEDIUM                  | Opportunity Signal = HARD                        |
| `serp-intelligence.skill` patterns | Content type: forum dominant; outdated dates in snippets | Mixed content types; some aging results      | Dominant content type: comprehensive guides, authority brands |
| `deep-serp-analysis.skill`         | Average page score ≤2.5/5; SERP has <3 strong pages     | Average page score 2.5–3.5/5                 | Average page score ≥4/5; 5+ strong pages         |
| Freshness signals                  | ≥5 of top-10 results are STALE or OUTDATED               | Mix of fresh and aging content               | ≥7 of top-10 are CURRENT                         |
| AEO signals                        | No featured snippet; no FAQ schema; no PAA coverage      | Partial AEO present                          | Strong featured snippet; FAQ schema; full PAA     |

**SERP Weakness Scoring Protocol:**

1. Check `serp-intelligence.skill` for the keyword's SERP opportunity signal.
2. Cross-reference with any per-page analysis from `deep-serp-analysis.skill` if available.
3. If only `serp-intelligence.skill` data is available → use opportunity signal as the primary signal.
4. If neither is available → infer from cluster gap status:
   - Gap Status = FULL GAP → SERP Weakness likely = 2 (no strong content exists).
   - Gap Status = PARTIAL → Score: 1 (inferred).
   - Gap Status = CONTESTED → Score: 0 (inferred).
   - Flag all inferences.

---

#### DIMENSION 3 — INTENT VALUE (0–2)

**What it measures:** The strategic value of the keyword's primary search intent in terms of business impact. Keywords that target users ready to make a decision (BOFU) carry higher strategic value than keywords targeting users in early awareness (TOFU), because BOFU content produces direct revenue or leads rather than traffic that must be converted through additional steps.

**Score Definitions:**

| Score | Label | Definition                                                                                                    |
|-------|-------|---------------------------------------------------------------------------------------------------------------|
| **2** | BOFU  | The keyword's dominant intent is Transactional or strongly Commercial — the searcher is ready to act, purchase, sign up, request a demo, or make a final selection decision. Content for this keyword is directly in the conversion path. |
| **1** | MOFU  | The keyword's dominant intent is Commercial or evaluative — the searcher is comparing options, researching vendors, or evaluating alternatives. Content can capture a reader mid-journey and influence their final decision. |
| **0** | TOFU  | The keyword's dominant intent is Informational — the searcher is in early awareness or learning mode. Content can attract traffic but requires multiple additional touchpoints to convert. This is not a low-value keyword — it is lower strategic urgency relative to BOFU/MOFU. |

**Intent Value Scoring Protocol:**

1. Use the keyword's `funnel` field from the clustering input as the primary source.
2. If the SERP intelligence provides a funnel correction → apply the corrected funnel stage.
3. For mixed-intent keywords (e.g., a keyword with both informational and commercial signals):
   - If BOFU signals are present alongside others → Score: 2 (BOFU wins in mixed intent).
   - If MOFU signals are present with TOFU → Score: 1.
   - Document the mixed-intent nature in the reason record.
4. Do NOT downgrade TOFU keywords' scores in this dimension just because TOFU is lower strategic urgency — TOFU scores 0, not -1. A zero in this dimension is not a penalty — it is a factual classification.

**Intent Nuance Notes:**

- A TOFU keyword that fills a FULL GAP (D1=2) and has a POOR SERP (D2=2) can still reach a total of 7 (MEDIUM priority) — meaning it warrants action even without conversion intent.
- A BOFU keyword with strong competition (D1=0) and a strong SERP (D2=0) scores only 4 (LOW priority) — meaning the conversion intent does not save a keyword from a saturated competitive landscape.

---

#### DIMENSION 4 — CLUSTER IMPORTANCE (0–2)

**What it measures:** The strategic importance of the cluster this keyword belongs to within the site's topical authority architecture. Keywords in clusters that are critical gaps in the site's content strategy — especially pillar clusters with full funnel potential — deserve higher prioritization than keywords in standalone clusters with no architectural connection to the rest of the site's content.

**Score Definitions:**

| Score | Label                   | Definition                                                                                                      |
|-------|-------------------------|------------------------------------------------------------------------------------------------------------------|
| **2** | CRITICAL CLUSTER GAP    | The keyword belongs to a cluster that is either a Pillar cluster with weak/no current coverage, or a cluster marked as MISSING or GAP in the gap analysis. Building this keyword produces content for an architecturally critical territory. |
| **1** | SUPPORTING CLUSTER      | The keyword belongs to a cluster that exists in the architecture with some content, but has coverage gaps. It is part of the strategy but not the most urgent territory. |
| **0** | STANDALONE              | The keyword belongs to a cluster with no architectural connections to the rest of the site's content strategy, OR the cluster is a micro-cluster with very limited topical authority impact, OR the cluster is already well-covered (CONTENT_RICH). |

**Scoring Signals for Dimension 4:**

| Signal Source                        | Score 2 Signal                                                | Score 1 Signal                             | Score 0 Signal                                    |
|--------------------------------------|---------------------------------------------------------------|--------------------------------------------|---------------------------------------------------|
| `semantic-clustering.skill`          | Cluster tier = PILLAR AND funnel_balance ≠ FULL              | Cluster tier = SUB; partial funnel balance | Cluster tier = MICRO; or cluster has CONTENT_RICH |
| `content-awareness.skill`            | Coverage Status = MISSING or GAP                             | Coverage Status = PARTIAL or INCOMPLETE    | Coverage Status = COMPLETE                        |
| `query-graph.skill`                  | Node Type = PRIMARY HUB or HUB; cluster is CONTENT_EMPTY    | Node Type = CONNECTOR                      | Node Type = LEAF or ORPHAN                        |
| `cluster_strength` score             | Cluster strength ≥ 70 AND content is absent                  | Cluster strength 40–69                     | Cluster strength < 40 OR content already present  |

**Cluster Importance Scoring Protocol:**

1. Identify the keyword's cluster from the clustering input.
2. Check cluster tier (Pillar / Sub / Micro) and coverage status.
3. Apply the signal table above — multiple signals confirming the same score = high confidence.
4. If cluster data is incomplete → use cluster strength as the primary proxy:
   - Strength ≥ 70 and no content → Score: 2.
   - Strength 40–69 → Score: 1.
   - Strength < 40 → Score: 0.
   - Flag as ESTIMATED.

---

#### DIMENSION 5 — DIFFERENTIATION POTENTIAL (0–2)

**What it measures:** The degree to which a new piece of content targeting this keyword can establish a genuinely distinct position — an angle, format, audience, depth level, or coverage approach that existing competitors have not taken. Differentiation potential is what converts a keyword from "possible" to "winnable" even when some competition exists.

**Score Definitions:**

| Score | Label                      | Definition                                                                                                        |
|-------|----------------------------|-------------------------------------------------------------------------------------------------------------------|
| **2** | CLEAR UNIQUE ANGLE POSSIBLE | A specific, executable differentiation opportunity exists for this keyword: a format the SERP doesn't have, an audience segment not served, an entity or sub-topic no competitor covers, an AEO gap competitors miss, or a fresher angle on an outdated SERP. The differentiation opportunity can be named specifically. |
| **1** | LIMITED DIFFERENTIATION     | Some differentiation is possible but not clearly defined. The SERP has reasonable content but with exploitable weaknesses in specific areas (e.g., all competitors skip the FAQ section; all use a list when a tutorial would serve better). Differentiation requires skill but is achievable. |
| **0** | COMMODITIZED               | The keyword's SERP is so well-covered, with so many high-quality results from diverse angles, that no practical differentiation opportunity exists without a resource investment that would not justify the return. Competing here means producing better-than-best content on an already-well-served topic. |

**Scoring Signals for Dimension 5:**

| Signal Source                        | Score 2 Signal                                                     | Score 1 Signal                              | Score 0 Signal                                  |
|--------------------------------------|--------------------------------------------------------------------|---------------------------------------------|-------------------------------------------------|
| `deep-serp-analysis.skill`           | Zero-presence opportunities detected (0% of pages have element)    | Low-presence opportunities (1–2/N pages)    | No zero or low-presence opportunities           |
| `content-pattern-extractor.skill`    | Dominant format is mismatched with intent; format gap detected     | Secondary format alternative exists         | All major formats well-represented              |
| `entity-extraction.skill`            | Critical entities not covered by any top result                    | Some entity gaps exist                      | All critical entities are well-covered          |
| `competitor-analysis.skill`          | Blind spot detected across all competitors for this topic          | Weakness detected in 1–2 competitors        | All competitors cover this topic comprehensively|
| SERP AEO signals                     | No featured snippet; no FAQ schema; PAA unanswered                 | Some AEO present; not optimized              | Full AEO coverage by existing results           |
| Freshness                            | All top results STALE/OUTDATED — freshness is itself a differentiator | Mix of fresh and aging                   | All recent results — freshness not a differentiator |

**Differentiation Potential Scoring Protocol:**

1. Check `deep-serp-analysis.skill` output for zero-presence and low-presence opportunities.
2. Check `competitor-analysis.skill` for detected blind spots.
3. Check entity map for entity coverage gaps in the SERP.
4. If no deep analysis is available → assess based on:
   - SERP difficulty EASY + Gap Status GAP → likely Score 2 (differentiation usually exists when competition is weak).
   - SERP difficulty MEDIUM → Score 1 (some differentiation possible).
   - SERP difficulty HARD → Score 0 (commoditized by default without specific evidence).
   - Flag as INFERRED.

---

### ▸ STEP 2 — SCORE CALCULATION

Calculate the total opportunity score for every keyword by summing the five dimension scores.

**Score Calculation Protocol:**

```
OPPORTUNITY SCORE CALCULATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Keyword         : [keyword]

D1 (Gap Level)            : [0/1/2] × weight = [weighted score]
D2 (SERP Weakness)        : [0/1/2] × weight = [weighted score]
D3 (Intent Value)         : [0/1/2] × weight = [weighted score]
D4 (Cluster Importance)   : [0/1/2] × weight = [weighted score]
D5 (Differentiation)      : [0/1/2] × weight = [weighted score]

Total Score               : [sum] / 10
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Standard (Equal-Weight) Scoring:**

By default, all five dimensions are weighted equally. Each dimension scores 0, 1, or 2. The total is the raw sum (maximum 10):

`Total = D1 + D2 + D3 + D4 + D5 (max: 2+2+2+2+2 = 10)`

**Cluster Dependency Weighting (Advanced — Applied When Gap Data Available):**

When `query-graph.skill` output is available and shows that a keyword's cluster is a prerequisite dependency for other high-priority clusters, apply a cluster dependency multiplier:

| Dependency Status                                          | Multiplier Applied to D4 |
|------------------------------------------------------------|--------------------------|
| Cluster is a PREREQUISITE dependency for ≥2 other clusters | ×1.5 (cap D4 at 2)       |
| Cluster is a PREREQUISITE dependency for 1 other cluster   | ×1.25 (cap D4 at 2)      |
| No dependency relationship                                  | ×1.0 (no change)         |

After applying the multiplier, if D4 would exceed 2 → cap it at 2. This prevents artificial score inflation while still boosting strategically critical cluster keywords.

Log any multiplier application in the reason record: `"D4 boosted — cluster is a prerequisite dependency for [cluster B name]."`

**Score Confidence Assessment:**

After calculating the total, assess overall confidence in the score:

| Confidence Level | Condition                                                                                  |
|------------------|--------------------------------------------------------------------------------------------|
| HIGH             | All 5 dimensions scored from direct data (no inferences); SERP data confirmed            |
| MEDIUM           | 1–2 dimensions inferred; overall data quality is adequate for strategic decisions         |
| LOW              | 3+ dimensions inferred; SERP data absent; significant uncertainty in scoring              |

LOW confidence scores → add `NEEDS VALIDATION` flag → add to Validation Queue (Part 4 of output).

---

### ▸ STEP 3 — PRIORITY ASSIGNMENT

Assign a priority label to every keyword based on its total opportunity score.

**Priority Threshold Definitions:**

| Score Range | Priority Label | Definition                                                                                               |
|-------------|----------------|----------------------------------------------------------------------------------------------------------|
| **8–10**    | **HIGH**       | This keyword represents a genuine, high-probability, high-impact content opportunity. Build this content in the current or next content cycle. |
| **5–7**     | **MEDIUM**     | This keyword has meaningful opportunity but may require more effort, better differentiation, or specific cluster context to be worth pursuing now. Schedule for the next planning horizon. |
| **0–4**     | **LOW**        | This keyword either lacks strategic urgency, faces overwhelming competition, or fails to offer differentiation potential. Deprioritize. Revisit after higher-priority opportunities are addressed. |

**Priority Assignment Boundary Rules:**

- A score of exactly 7 → MEDIUM (boundary goes to the lower tier — MEDIUM is more honest than HIGH for borderline cases).
- A score of exactly 8 → HIGH.
- A score of exactly 5 → MEDIUM.
- A score of exactly 4 → LOW.

**Priority Override Conditions:**

Certain conditions can force a manual priority review regardless of the calculated score:

| Override Condition                                                             | Action                                              |
|--------------------------------------------------------------------------------|-----------------------------------------------------|
| Keyword is a MEMORY CONFLICT (scored differently in prior session)             | Flag as MEM-CONFLICT; surface both scores for human review |
| Keyword is NEEDS VALIDATION (confidence = LOW)                                 | Display MEDIUM regardless of calculated score until validated |
| Keyword is in an explicitly user-flagged priority cluster                      | Note the user flag; display score as calculated; add "USER PRIORITY" flag |
| Keyword D1 = 0 AND D2 = 0 (no gap + strong SERP) — even if D3+D4+D5 are high | Maximum effective priority is MEDIUM — a well-served SERP with no gap rarely justifies HIGH |

---

### ▸ STEP 4 — REASON GENERATION

For every keyword, produce a specific, non-generic, data-tied reason statement that explains the total score in one sentence.

**Reason Quality Standards:**

Every reason statement MUST:

1. **Reference at least two scoring dimensions** — a one-dimension reason is too simple and suggests the score is not fully justified.
2. **Name a specific signal** — not a category of signal (not "the SERP is weak" but "the SERP shows outdated forum results from 2021 with no featured snippet").
3. **Be unique per keyword** — a reason that could apply to any keyword is not a valid reason.
4. **Be one sentence** — concise enough to be read in a priority table; specific enough to be acted upon.

**Prohibited Reason Forms:**

| Prohibited Reason                                | Problem                                           |
|--------------------------------------------------|---------------------------------------------------|
| `"This keyword has good opportunity."`           | Restates the score without explaining it          |
| `"This is a high-value BOFU keyword."`           | Generic — applies to any BOFU keyword             |
| `"The SERP is weak and there is a gap."`         | Non-specific — needs named signals                |
| `"This is important for the cluster."`           | Vague — does not name why or how                  |
| `"This keyword should be targeted soon."`        | Recommendation, not explanation                   |

**Required Reason Template:**

`"[Dimension signal 1] combined with [dimension signal 2], supported by [dimension signal 3 if applicable], makes this keyword [specific strategic value statement]."`

**Reason Examples (non-generic — these illustrate the standard, not templates to copy):**

| Score | Reason Example                                                                                                                                                            |
|-------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 10    | `"The SERP has no authoritative results (all forum threads and a 2019 blog post), the keyword targets buyers at the final decision stage, and no competitor covers the CRM-for-law-firms angle — making this a first-mover BOFU opportunity in an empty cluster."`  |
| 8     | `"The cluster is a confirmed gap (gap-engine: MISSING), SERP results are STALE (3 of 5 top results pre-2022), and a fresh comparison format with AEO optimization would immediately differentiate from the weak prose-only results currently ranking."` |
| 6     | `"This MOFU evaluation keyword fills a supporting cluster gap but faces moderate competition from two mid-strength domains — differentiation is possible through a structured comparison format that no current result uses."` |
| 3     | `"This TOFU keyword exists in an already CONTENT_RICH cluster, competes against comprehensive brand-owned guides, and offers no differentiation angle that hasn't been executed by existing results."` |

**Reason Generation Protocol:**

1. Review the five dimension scores and their evidence for the keyword.
2. Identify the 2–3 most significant factors that drive the score (both positively and negatively).
3. Construct the one-sentence reason referencing those factors and the specific signals behind them.
4. Verify the reason is unique to this keyword — it should not describe any other keyword in the dataset without modification.

---

### ▸ STEP 5 — SORTING AND OUTPUT FINALIZATION

Sort the complete scored keyword list and produce the final output.

**Phase 5A — Sort Order**

Apply a two-level sort:

**Primary sort:** Score descending (highest score first).
**Secondary sort (tie-breaking):** Intent priority descending within the same score:
- BOFU (D3=2) beats MOFU (D3=1) beats TOFU (D3=0) at equal total scores.
- Within the same score AND same intent → sort alphabetically by keyword (for deterministic output).

**Phase 5B — Output Assembly**

After sorting:
1. Populate the Scored Keyword Table (Part 1) with all keywords in sorted order.
2. Populate the Per-Keyword Reason Records (Part 2) in the same sorted order.
3. Compute and populate the Strategic Synthesis (Part 3).
4. Populate the Validation Queue (Part 4) with all NEEDS VALIDATION keywords.

**Phase 5C — Strategic Synthesis Calculation**

For Part 3:
1. **Highest-Opportunity Cluster:** Average score per cluster. Identify the cluster with the highest average score and the most HIGH-priority keywords.
2. **Most Critical Gap:** Identify the cluster with the highest-scoring keywords that are CONTENT_EMPTY or MISSING.
3. **Highest BOFU Density:** Count keywords with D3=2 (BOFU) and priority HIGH or MEDIUM per cluster.
4. **Quick Wins:** Keywords that are HIGH priority AND scored from a MISSING/GAP cluster AND are CONTENT_EMPTY. These are the highest-probability, lowest-effort targets.
5. **Lowest Priority Cluster:** The cluster with the lowest average score across all its keywords.

**Phase 5D — Memory Write**

After all output is produced:
1. Write all scored keywords to `keyword-memory.skill` via `memory-controller.skill`.
2. Update each keyword's lifecycle state:
   - HIGH priority → `SCORED_HIGH`
   - MEDIUM priority → `SCORED_MEDIUM`
   - LOW priority → `SCORED_LOW`
   - NEEDS VALIDATION → `SCORED_PENDING_VALIDATION`
3. Write the complete scored output to `strategy-memory.skill` for cross-session continuity.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ SERP WEAKNESS SIMULATION

When live SERP intelligence is absent or incomplete, this skill simulates SERP weakness assessment using structural and contextual signals.

**Simulation Protocol:**

1. **Keyword pattern signal:** Does the keyword follow a pattern that typically produces weak SERPs?

| Pattern                                              | Simulated SERP Weakness |
|------------------------------------------------------|-------------------------|
| Question format + highly specific modifier           | Likely POOR (Score 2)   |
| Long-tail (4+ words) with niche audience modifier    | Likely POOR–AVERAGE (Score 2–1) |
| Head term (1–2 words) with no modifier               | Likely STRONG (Score 0) |
| Comparison keyword with named brand tools            | AVERAGE to STRONG (Score 1–0) |
| Niche keyword with industry + use case modifiers     | POOR to AVERAGE (Score 2–1) |
| Evergreen how-to keyword with no year modifier       | Variable — default AVERAGE (Score 1) |

2. **Cluster gap signal:** If a keyword's cluster is marked as FULL GAP or MISSING in any gap analysis data → simulate SERP weakness as POOR (Score 2). Rationale: a FULL GAP cluster by definition lacks strong content.

3. **Prior session signal:** If the same or semantically similar keyword was scored in a prior session → use the prior D2 score as a baseline and note: "D2 inherited from prior session — verify for freshness."

All simulated SERP weakness scores are flagged with `INFERRED:D2` in the output table.

---

### ▸ CLUSTER DEPENDENCY WEIGHTING

Some keywords are more strategically valuable not because of their own scores, but because the cluster they are in is a prerequisite dependency for other high-priority clusters in the topic graph.

**Implementation:**

1. Check `query-graph.skill` dependency chain data for each keyword's cluster.
2. If the cluster appears as a PREREQUISITE node in any dependency chain → apply the dependency multiplier to D4 (as defined in Step 2).
3. Log the dependency boost: `"Cluster [name] is required before [dependent cluster] can produce maximum-quality content — D4 boosted by [multiplier]."`

**Dependency Cascade Effect:**

When a keyword scores LOW on individual dimensions but its cluster dependency multiplier boosts D4 to 2 → re-evaluate. A keyword that is the gateway to building a HIGH-value cluster may deserve MEDIUM priority even if the keyword itself has a modest score. The strategic value is systemic, not isolated.

---

### ▸ STRATEGIC PRIORITIZATION SYNTHESIS

Beyond individual keyword scoring, this skill produces a cluster-level strategic synthesis that reveals where the highest-density opportunity exists across the entire keyword universe.

**Synthesis Metrics:**

| Metric                              | Calculation                                                                    |
|-------------------------------------|--------------------------------------------------------------------------------|
| Cluster average score               | Sum of all keyword scores in cluster / keyword count                           |
| Cluster HIGH density                | Count of HIGH priority keywords / total keywords in cluster                    |
| Cluster BOFU density                | Count of D3=2 keywords / total keywords in cluster                             |
| Cluster conversion readiness        | Combined metric: BOFU density × cluster strength × (1 / competition level)   |
| Quick win concentration             | Count of HIGH priority + CONTENT_EMPTY + EASY difficulty keywords per cluster  |

**Synthesis Output Rules:**

- Every synthesis insight must reference a specific metric — not a general impression.
- The "Lowest Priority Cluster" must include a specific reason for deprioritization.
- Quick wins must be limited to ≤5 keywords — listing more than 5 dilutes the "quick win" signal.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate Keywords in the Scored Output

Every keyword must appear exactly once in the scored output, regardless of how many clusters it was found in. If a keyword appears in two clusters (a cross-cluster assignment from `semantic-clustering.skill`) → score it once using the primary cluster assignment; note the secondary cluster in the reason record.

### Rule 2 — No Repeated Scoring Within a Session

If `memory-controller.skill` shows that a keyword was already scored in the current session with the same input data → return the cached score. Do not re-score. Log: `"Score returned from session cache — [keyword] scored at [score] at [timestamp]."`

Only re-score if: the user explicitly requests a refresh, new SERP data has become available for this keyword, or the cluster assignment has changed.

### Rule 3 — No Repeated Score Justifications

Every reason statement (Step 4) must be unique to its keyword. If the automated reason generation produces an identical reason statement for two different keywords → manually differentiate by adding the specific signal that makes each keyword's situation distinct from the other's.

### Rule 4 — Memory Conflict Resolution

When a keyword's score differs from a prior session's score by ≥3 points:
1. Flag as MEM-CONFLICT.
2. Record both scores (prior and current) in the reason record.
3. Provide the reason for the change: new SERP data, different cluster assignment, updated gap status.
4. Do NOT silently override the prior score — surface the conflict for human review.

### Rule 5 — Cross-Check Completeness

After scoring all keywords, verify the total scored count equals the total input keyword count. If any keywords are missing from the output → identify them, score them (using inference if necessary), and add them with appropriate flags. No keyword may leave this skill unscored.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Scoring Behaviors

| Prohibited Behavior                                                                            | Reason                                                                           |
|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Assigning a HIGH score based on "gut feel" without calculating all five dimensions             | Every score must trace back to the five-dimension formula                        |
| Scoring a keyword without evidence for Dimension 1 OR Dimension 2                             | D1 and D2 are the most data-dependent dimensions — both require at least an inference signal |
| Assigning D5 = 2 (clear differentiation angle) without naming the specific angle              | A differentiation potential of 2 requires naming what the angle IS              |
| Giving all keywords in the same cluster the same scores                                        | Keywords within a cluster can and often do have different SERP weaknesses and differentiation potentials |
| Inflating scores for keywords in a client's preferred topic to "make the roadmap look stronger"| Inflated scores produce false priorities and waste editorial resources            |
| Deflating scores for difficult keywords to "avoid hard work"                                   | Deflated scores abandon winnable opportunities based on difficulty aversion      |
| Producing a reason statement that does not reference at least two specific dimensional signals  | One-dimensional reasons indicate the score was not fully justified               |
| Marking more than 25% of the keyword universe as NEEDS VALIDATION without specific reasons     | Excessive validation flags signal inference over-use — improve data sourcing     |
| Skipping the strategic synthesis because the keyword set is "too small"                        | The synthesis produces useful insight for even a 5-keyword set                   |
| Assigning priority without considering the sort order logic (BOFU > MOFU > TOFU tie-break)    | Sort order must reflect the intent priority tie-breaking rule                    |

### Required Scoring Behaviors

- Every keyword must have all five dimensions scored (0, 1, or 2 — no nulls).
- Every dimension score of 2 must have specific evidence named in the reason record.
- Every NEEDS VALIDATION flag must explain specifically why the confidence is LOW.
- The strategic synthesis must reference specific metrics — not general impressions.
- Memory write-back must occur after output is finalized.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary keyword and cluster input)                                    |
| **Receives**     | Full cluster architecture: keyword lists with intent/funnel, cluster strength, funnel balance, pillar designations |
| **Used for**     | Step 1 Dimension 4 (Cluster Importance), Step 2 (cluster dependency weighting)             |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream intelligence source                                                                |
| **Receives**     | Difficulty rating, opportunity signal, SERP pattern data, freshness signals, content direction |
| **Used for**     | Step 1 Dimension 1 (Gap Level), Step 1 Dimension 2 (SERP Weakness)                        |

---

### ▸ gap-opportunity-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source                                                                     |
| **Receives**     | Gap matrix (MISSING / GAP / PARTIAL / CONTESTED per cluster and topic)                     |
| **Used for**     | Step 1 Dimension 1 (Gap Level) — primary signal for gap scoring                           |

---

### ▸ deep-serp-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Per-page quality scores, zero-presence and low-presence opportunity detection              |
| **Used for**     | Step 1 Dimension 2 (SERP Weakness enrichment), Dimension 5 (Differentiation Potential enrichment) |

---

### ▸ competitor-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Blind spots, weakness patterns, competitor coverage per cluster                            |
| **Used for**     | Step 1 Dimension 1 (Gap Level) and Dimension 5 (Differentiation Potential) enrichment     |

---

### ▸ content-awareness.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Coverage status per cluster (COMPLETE / PARTIAL / INCOMPLETE / GAP), content status per node |
| **Used for**     | Step 1 Dimension 4 (Cluster Importance), Step 5C (quick win identification)               |

---

### ▸ query-graph.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment source                                                         |
| **Receives**     | Node types (PRIMARY HUB / HUB / CONNECTOR / LEAF), dependency chains                      |
| **Used for**     | Step 2 cluster dependency weighting applied to D4                                          |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before scoring (cache check), WRITE after output finalization          |
| **Reads**        | Prior session keyword scores, lifecycle states, scoring history                             |
| **Writes**       | All scored keywords with priority labels, score breakdowns, reason records                 |
| **Memory layers**| keyword-memory.skill (scores + lifecycle), strategy-memory.skill (full output)            |
| **Timing**       | READ at pre-conditions (Step 4 cache check). WRITE at Step 5D.                            |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | opportunity-engine → roadmap-engine (primary strategic input)                              |
| **Sends**        | Complete scored keyword table, strategic synthesis, quick wins, cluster opportunity rankings |
| **Critical fields** | Sorted scored keyword table (Part 1) — this is the primary input for roadmap sequencing  |

---

### ▸ strategist-ai.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | opportunity-engine → strategist-ai (strategic intelligence consumer)                       |
| **Sends**        | Strategic synthesis (Part 3), validation queue (Part 4), full scored dataset               |
| **Used for**     | Generating strategic recommendations, identifying patterns, advising on editorial priority  |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| SERP intelligence unavailable for any keywords                            | Step 1 D2 — no serp-intelligence data                  | Apply SERP weakness simulation (Advanced Logic). Flag all D2 scores as INFERRED. Add note: "SERP data absent — SERP scores simulated from structural signals." |
| Gap data unavailable for all keywords                                     | Step 1 D1 — no gap-opportunity-engine data             | Infer gap level from serp_difficulty (EASY=2, MEDIUM=1, HARD=0). Flag all D1 scores as INFERRED.       |
| Both SERP data AND gap data are unavailable                               | D1 and D2 both must be inferred                        | Proceed with structure-based scoring only. Flag entire output as PARTIAL DATA — REDUCED CONFIDENCE. Maximum effective score = 6 (D3+D4+D5 only at full confidence). |
| A keyword has no cluster assignment                                       | D4 — cluster lookup fails                              | Assign cluster = UNCLASSIFIED; D4 = 0 (standalone). Flag as CLUSTER UNKNOWN. Score all other dimensions normally. |
| Score confidence = LOW for >25% of input keywords                        | Step 2 — confidence assessment                         | Flag output: "HIGH INFERENCE RATE — recommend running serp-intelligence.skill before acting on these scores." |
| Memory conflict detected on >10 keywords                                  | Step 3 — MEM-CONFLICT flag count > 10                  | Surface a batch conflict notice: "N keywords scored differently than prior session. Possible SERP changes or cluster restructuring. Review before routing to roadmap." |
| All keywords score LOW (total output is entirely LOW priority)            | Step 5A — no HIGH or MEDIUM keywords                   | Flag: "No high-priority opportunities detected in this keyword set. Possible causes: saturated niche, wrong keyword universe, or missing gap data. Recommend: expand keyword discovery or run gap analysis." |
| A keyword cannot receive a specific differentiation angle (D5 is uncertain)| Step 1 D5 — no deep SERP or competitor data           | Score D5 = 1 (LIMITED DIFFERENTIATION as default when differentiation is unknown). Flag as INFERRED:D5. Do not assign 0 without specific evidence of commoditization. |
| Keyword universe is fewer than 5 keywords                                 | Phase 5C — synthesis on very small set                 | Produce per-keyword records at full depth. Note in strategic synthesis: "Dataset too small for cluster-level pattern analysis — strategic synthesis limited to individual keyword observations." |
| Memory controller unavailable                                             | READ/WRITE queries return errors                       | Proceed without cache check or write-back. Flag: "Memory unavailable — prior session conflict detection skipped; scores will not persist across sessions." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — DIMENSION SCORING QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
FIVE-DIMENSION SCORING QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

D1 — GAP LEVEL (0–2)
  2 = No strong content exists / Gap Status: FULL GAP or GAP
  1 = Weak content exists / Gap Status: PARTIAL / SERP: MEDIUM
  0 = Strong competition / Gap Status: CONTESTED / SERP: HARD

D2 — SERP WEAKNESS (0–2)
  2 = Poor results — outdated / thin / format mismatch / no AEO
  1 = Average results — adequate but beatable with superior execution
  0 = Strong results — comprehensive, fresh, well-structured, authoritative

D3 — INTENT VALUE (0–2)
  2 = BOFU — Transactional / final decision intent
  1 = MOFU — Commercial / evaluation intent
  0 = TOFU — Informational / awareness intent

D4 — CLUSTER IMPORTANCE (0–2)
  2 = Critical cluster gap — Pillar cluster MISSING/GAP; no current content
  1 = Supporting cluster — sub-cluster with partial coverage
  0 = Standalone — micro-cluster, no connections, or cluster is CONTENT_RICH

D5 — DIFFERENTIATION POTENTIAL (0–2)
  2 = Clear unique angle possible — zero-presence opportunity / entity gap / AEO gap / freshness advantage
  1 = Limited differentiation — some angle exists; requires skill to execute
  0 = Commoditized — all angles covered; no practical entry point

TOTAL SCORE = D1 + D2 + D3 + D4 + D5 (max 10)

PRIORITY LABELS:
  8–10 → HIGH
  5–7  → MEDIUM
  0–4  → LOW

TIE-BREAKING:
  Equal scores → BOFU > MOFU > TOFU
  Equal intent → Alphabetical by keyword

FAILSAFE:
  Uncertain score → assign 1 per uncertain dimension
  → mark NEEDS VALIDATION
  → do not guess at 0 or 2 without evidence
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — REASON GENERATION QUALITY CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
REASON STATEMENT QUALITY CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Before finalizing any reason statement, verify:

□ Does the reason reference at least TWO scoring dimensions?
  If NO → rewrite to include a second dimension's signal.

□ Does the reason name a SPECIFIC signal (not a category)?
  If NO → replace generic reference with the specific data point.
  BAD: "the SERP is weak"
  GOOD: "the SERP shows three 2020 forum threads and no featured snippet"

□ Is the reason UNIQUE to this keyword?
  Could it apply word-for-word to another keyword in the same cluster?
  If YES → add the differentiating detail that makes this keyword's situation distinct.

□ Is the reason ONE SENTENCE?
  If NO → trim to the most impactful facts.

□ Does the reason reflect the actual SCORE?
  A high-score reason should describe why opportunity is high.
  A low-score reason should describe why opportunity is limited.
  If the reason seems to argue for a different score → re-evaluate the score.

□ Does the reason avoid prohibited phrases?
  ("good opportunity", "high value", "should be targeted", "important keyword")
  If any of the above appear → rewrite with specific signals.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — PRIORITY SCORE EXAMPLES (CALIBRATION REFERENCE)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
SCORE CALIBRATION EXAMPLES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Score 10 (Perfect opportunity):
  D1=2: FULL GAP — no strong competitor content
  D2=2: POOR SERP — forum threads and a 2019 article rank top-3
  D3=2: BOFU intent — buyer ready to decide
  D4=2: CRITICAL cluster gap — pillar cluster with zero content
  D5=2: Clear angle — comparison format + AEO optimization not used by anyone

Score 8 (High opportunity, one constraint):
  D1=2: GAP status confirmed
  D2=2: SERP outdated — 4 of 5 results are 18+ months old
  D3=2: BOFU intent
  D4=1: Supporting cluster — has some content already
  D5=1: Some differentiation — deeper entity coverage angle possible

Score 6 (Medium — meaningful but not urgent):
  D1=1: Weak content exists — one thin competitor piece
  D2=1: Average SERP — decent results but beatable
  D3=1: MOFU intent — evaluation stage
  D4=2: Critical pillar cluster with no MOFU content
  D5=1: Limited differentiation — some room to improve depth

Score 4 (Low — not worth prioritizing now):
  D1=0: No gap — strong competition
  D2=0: Strong SERP — comprehensive, fresh, authoritative results
  D3=2: BOFU intent (still a 2 — intent doesn't save a saturated keyword)
  D4=1: Supporting cluster
  D5=1: Some differentiation theoretically possible

Note: BOFU intent (D3=2) does NOT automatically make a keyword HIGH priority.
The gap and SERP dimensions are equally weighted.
A D3=2 keyword with D1=0 and D2=0 maxes out at 6 (MEDIUM).
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: analysis/opportunity-engine.skill*
*Nexus SEO Operating System — Version 1.0.0*
