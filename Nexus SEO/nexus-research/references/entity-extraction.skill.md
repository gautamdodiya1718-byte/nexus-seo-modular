# SKILL: research/entity-extraction.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / Semantic Intelligence
# EXECUTION PRIORITY: PARALLEL OR POST-SERP — Runs alongside or after deep-serp-analysis.skill.
# POSITION: Semantic backbone builder between SERP intelligence and content generation.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **semantic entity intelligence engine** of the Nexus system.

It extracts, maps, classifies, scores, and prioritizes the named entities and conceptual units that define a topic's semantic landscape — the entities that Google's Knowledge Graph, topical authority algorithms, and answer engines use to determine whether a piece of content genuinely understands its subject matter at depth.

Modern search ranking is not merely about keyword presence. Google evaluates content through entity co-occurrence patterns: does this content mention the tools, experts, organizations, processes, metrics, and concepts that belong to the topic's semantic field? When these entities are present, correctly contextualized, and related to each other appropriately, the content signals topical authority. When they are absent or incorrectly framed, the content signals surface-level treatment — regardless of word count or structure.

This skill builds the **semantic backbone** that downstream content skills use to ensure every piece of content contains the full entity coverage required to compete as a topically authoritative resource.

### Primary Responsibilities

| Responsibility                      | Description                                                                                                          |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Entity Category Definition**      | Identify and maintain the 7 entity categories that structure all extraction                                          |
| **SERP Entity Extraction**          | Extract all named entities appearing in titles, headings, and intro sections of top-ranking pages                   |
| **Frequency Scoring**               | Apply TF-IDF-simulated scoring to determine each entity's relevance weight across the result set                    |
| **Entity Classification**           | Assign every extracted entity a type and a relevance level                                                          |
| **Entity Gap Detection**            | Compare SERP entity set against user's existing content to identify missing and underused entities                  |
| **Relationship Mapping**            | Define semantic relationships between entities — how they depend on, enable, or measure each other                  |
| **AEO Entity Tagging**              | Mark entities suitable for FAQs, definitions, and featured snippet targeting                                        |
| **Entity Prioritization**           | Classify every entity as Critical, Important, or Optional for content inclusion purposes                            |
| **Entity Cluster Mapping**          | Group related entities into semantic clusters that reflect the topic's conceptual architecture                      |
| **Inference Handling**              | When data is limited, use keyword and SERP inference to complete the entity map — always flagged as inferred        |

### What This Skill Is NOT

- It is **not** a keyword extractor. Keywords and entities are distinct. A keyword is what users search for. An entity is a named thing — a concept, tool, person, organization, or process — that the topic requires understanding.
- It is **not** a content generator. It produces entity intelligence — `content-generation-engine.skill` uses that intelligence to write.
- It is **not** a SERP analyzer. It receives SERP data from upstream skills — it does not perform page analysis itself.
- It is **not** a link builder. Entity relationship mapping is semantic, not structural — it does not describe hyperlink relationships.
- It is **not** optional for technical or expert-level content. For any content targeting audiences with domain expertise, entity coverage is as important as keyword targeting.

### The Foundational Principle of This Skill

> An entity is not relevant because it appears in the content.
> An entity is relevant because it belongs to the semantic field of the topic — because a genuine expert writing about this topic would naturally include it.
>
> This skill's job is to identify all such entities from the SERP evidence and user content gaps,
> then classify each by how essential it is to topical authority.
>
> Every entity in the output must be there because it serves the reader's understanding
> or the content's semantic completeness — not because it appeared once in a search result.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                     | Fields Consumed                                                                              | Required |
|----------------------------------|----------------------------------------------------------------------------------------------|----------|
| `deep-serp-analysis.skill`       | Per-page dimension data (D4 Semantic signals, D6 Quality signals), page titles, H2/H3 headings, intro content excerpts, PAA questions | YES |
| `keyword-discovery.skill`        | Full keyword universe (all 8 dimensions), pre-cluster hints, semantic neighbor data          | YES |

### Optional Inputs

| Input Source                     | Fields Consumed                                                                 | Used For                                              |
|----------------------------------|---------------------------------------------------------------------------------|-------------------------------------------------------|
| `serp-intelligence.skill`        | PAA questions, dominant intent, SERP feature data                               | Enriching AEO entity tagging (Step 7)                 |
| `semantic-clustering.skill`      | Cluster labels, cluster descriptions, keyword-to-cluster assignments            | Organizing entities into cluster-aligned groups       |
| `competitor content` (raw)       | Any textual content from competitor pages if accessible                         | Direct entity extraction from competitor body content |
| `user existing content`          | Titles, headings, and body content of the user's existing published articles    | Entity gap detection (Step 5)                         |
| `domain`                         | Target site domain                                                               | Scoping entity gap analysis to site-specific context  |

### Input Confidence Levels

The quality and completeness of entity extraction directly depends on input richness. Track and report input confidence:

| Input State                                                              | Confidence Level | Impact                                                        |
|--------------------------------------------------------------------------|------------------|---------------------------------------------------------------|
| Full deep-SERP data + keyword universe + competitor content              | HIGH             | All 8 extraction steps run at full depth                     |
| Deep-SERP data + keyword universe (no competitor content)                | MEDIUM-HIGH      | Steps 1–4, 6–8 fully; Step 5 partial (SERP only comparison) |
| Keyword universe only (no SERP analysis data)                            | MEDIUM           | Steps 1–4 inference-based; Steps 5–7 flagged as INFERRED     |
| Limited input (keyword or topic only)                                    | LOW              | All steps inference-based; all entities flagged as INFERRED  |

### Input Pre-Conditions

1. **Is `deep-serp-analysis.skill` data available?** If yes → use as primary entity source. If no → operate in inference mode; flag all output.
2. **Is `keyword-discovery.skill` output available?** If yes → load full keyword universe for semantic neighbor expansion. If no → derive entity candidates from topic alone.
3. **Is user existing content available for gap analysis?** If yes → run full gap detection in Step 5. If no → run SERP-only entity map with note that gap analysis was skipped.
4. **Has entity extraction been run before for this keyword/topic?** → Query `memory-controller.skill`. If within freshness threshold (14 days) → offer cached. If stale → proceed fresh.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Structured Entity Intelligence Report

```
NEXUS ENTITY INTELLIGENCE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Target Topic / Keyword  : [keyword or topic]
Input Confidence        : [HIGH / MEDIUM-HIGH / MEDIUM / LOW]
Total Entities Extracted: [count]
  Critical              : [count]
  Important             : [count]
  Optional              : [count]
SERP Pages Referenced   : [count]
Gap Analysis Run        : [YES / NO — reason if no]
Inference Flags         : [count of inferred entities]
Generated By            : research/entity-extraction.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — FULL ENTITY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Grouped by Entity Type:

─────────────────────────────────────────────────────────────────────
CATEGORY 1: TOOLS / SOFTWARE
─────────────────────────────────────────────────────────────────────

| # | Entity             | Type     | Relevance    | Frequency | Priority  | AEO Tag | Why It Matters                                          |
|---|--------------------|----------|--------------|-----------|-----------|---------|----------------------------------------------------------|
| 1 | [Entity Name]      | Tool     | High         | [N/N pgs] | Critical  | DEF     | [specific reason tied to topic and audience]            |
| 2 | [Entity Name]      | Software | Medium       | [N/N pgs] | Important | FAQ     | [specific reason]                                       |
| 3 | [Entity Name]      | Tool     | Low          | [N/N pgs] | Optional  | —       | [specific reason]                                       |
...

─────────────────────────────────────────────────────────────────────
CATEGORY 2: TECHNOLOGIES
─────────────────────────────────────────────────────────────────────

| # | Entity             | Type       | Relevance | Frequency | Priority  | AEO Tag | Why It Matters                                         |
|---|--------------------|------------|-----------|-----------|-----------|---------|--------------------------------------------------------|
| 1 | [Entity Name]      | Technology | High      | [N/N pgs] | Critical  | DEF     | [reason]                                               |
...

─────────────────────────────────────────────────────────────────────
CATEGORY 3: CONCEPTS / METHODS
─────────────────────────────────────────────────────────────────────

| # | Entity             | Type    | Relevance | Frequency | Priority  | AEO Tag | Why It Matters                                         |
|---|--------------------|---------|-----------|-----------|-----------|---------|--------------------------------------------------------|
| 1 | [Entity Name]      | Concept | High      | [N/N pgs] | Critical  | DEF+FAQ | [reason]                                               |
...

─────────────────────────────────────────────────────────────────────
CATEGORY 4: PEOPLE / EXPERTS
─────────────────────────────────────────────────────────────────────

| # | Entity             | Type   | Relevance | Frequency | Priority  | AEO Tag | Why It Matters                                         |
|---|--------------------|--------|-----------|-----------|-----------|---------|--------------------------------------------------------|
| 1 | [Entity Name]      | Expert | High      | [N/N pgs] | Important | QUOTE   | [reason]                                               |
...

─────────────────────────────────────────────────────────────────────
CATEGORY 5: ORGANIZATIONS
─────────────────────────────────────────────────────────────────────

| # | Entity             | Type         | Relevance | Frequency | Priority  | AEO Tag | Why It Matters                                     |
|---|--------------------|--------------|-----------|-----------|-----------|---------|-----------------------------------------------------|
| 1 | [Entity Name]      | Organization | High      | [N/N pgs] | Critical  | REF     | [reason]                                            |
...

─────────────────────────────────────────────────────────────────────
CATEGORY 6: PROCESSES / WORKFLOWS
─────────────────────────────────────────────────────────────────────

| # | Entity             | Type     | Relevance | Frequency | Priority  | AEO Tag | Why It Matters                                     |
|---|--------------------|----------|-----------|-----------|-----------|---------|-----------------------------------------------------|
| 1 | [Entity Name]      | Process  | High      | [N/N pgs] | Critical  | HOW     | [reason]                                            |
...

─────────────────────────────────────────────────────────────────────
CATEGORY 7: METRICS / KPIs
─────────────────────────────────────────────────────────────────────

| # | Entity             | Type   | Relevance | Frequency | Priority  | AEO Tag | Why It Matters                                     |
|---|--------------------|--------|-----------|-----------|-----------|---------|-----------------------------------------------------|
| 1 | [Entity Name]      | Metric | High      | [N/N pgs] | Critical  | DEF     | [reason]                                            |
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — ENTITY GAP ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Only produced if user existing content is available]

MISSING ENTITIES (present in SERP / absent in user content):
| Entity             | Type     | Priority  | SERP Frequency | Gap Severity   |
|--------------------|----------|-----------|----------------|----------------|
| [Entity Name]      | [type]   | Critical  | [N/N pgs]      | CRITICAL GAP   |
| [Entity Name]      | [type]   | Important | [N/N pgs]      | SIGNIFICANT    |
| [Entity Name]      | [type]   | Optional  | [N/N pgs]      | MINOR          |

UNDERUSED ENTITIES (present in user content but insufficiently developed):
| Entity             | Type     | Priority  | User Content Depth | Required Depth |
|--------------------|----------|-----------|--------------------|----------------|
| [Entity Name]      | [type]   | Important | MENTIONED          | DEFINED        |
| [Entity Name]      | [type]   | Critical  | DEFINED            | EXPLAINED+EXAMPLE |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — RELATIONSHIP MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Entity relationship chains — structured by relationship type]

DEPENDENCY RELATIONSHIPS (A depends on / requires B):
  → [Entity A] — requires → [Entity B]
    Reason: [why A cannot function without B]
  → [Entity A] — requires → [Entity B]
  ...

FUNCTIONAL RELATIONSHIPS (Tool used in Process):
  → [Entity: Tool] — used in → [Entity: Process]
    Context: [how the tool is used in this process]
  → [Entity: Tool] — used in → [Entity: Process]
  ...

MEASUREMENT RELATIONSHIPS (Metric measures Outcome):
  → [Entity: Metric] — measures → [Entity: Outcome/Concept]
    Context: [what this metric quantifies and why it matters]
  ...

HIERARCHICAL RELATIONSHIPS (A is a type of / subcategory of B):
  → [Entity: Specific] — is a type of → [Entity: General]
  ...

ENABLING RELATIONSHIPS (A enables / produces B):
  → [Entity: Process/Method] — produces → [Entity: Outcome/Metric]
  ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — AEO ENTITY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DEFINITION TARGETS (DEF — suitable for featured snippet paragraph):
  | Entity           | Recommended Definition Format | Target Query                          |
  |------------------|-------------------------------|---------------------------------------|
  | [Entity Name]    | [Paragraph / 2-sentence]      | "What is [entity]?"                   |
  | [Entity Name]    | [Paragraph / 2-sentence]      | "What does [entity] mean?"            |

FAQ TARGETS (FAQ — suitable for FAQ schema and PAA alignment):
  | Entity           | Recommended Question                     | Answer Format     |
  |------------------|------------------------------------------|-------------------|
  | [Entity Name]    | "[question phrasing]"                    | [list / paragraph]|
  | [Entity Name]    | "[question phrasing]"                    | [format]          |

HOW-TO TARGETS (HOW — suitable for HowTo schema and process snippet):
  | Entity           | Recommended Question                     | Steps Required    |
  |------------------|------------------------------------------|-------------------|
  | [Entity Name]    | "How to use/implement [entity]?"         | [N] steps         |

REFERENCE TARGETS (REF — suitable for authority signal, cited as source):
  | Entity           | Reference Context                        | Citation Type     |
  |------------------|------------------------------------------|-------------------|
  | [Entity Name]    | [where this entity should be cited]      | [stat / report / standard] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — ENTITY CLUSTER MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Grouping entities by semantic proximity across categories]

Entity Cluster 1 — [Cluster Label]:
  Core Entity   : [entity]
  Related       : [entity], [entity], [entity]
  Cluster Purpose: [what semantic territory this cluster covers]
  Content Section: [which blueprint section should cover this cluster]

Entity Cluster 2 — [Cluster Label]:
  Core Entity   : [entity]
  Related       : [entity], [entity]
  Cluster Purpose: [purpose]
  Content Section: [section reference]

...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 6 — PRIORITY SUMMARY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITICAL ENTITIES (must include — failure to cover signals shallow content):
  [Entity 1] ([Type]) | [Entity 2] ([Type]) | [Entity 3] ([Type]) | ...

IMPORTANT ENTITIES (should include — strengthens topical authority):
  [Entity 1] ([Type]) | [Entity 2] ([Type]) | ...

OPTIONAL ENTITIES (enhancement — adds depth for expert audiences):
  [Entity 1] ([Type]) | [Entity 2] ([Type]) | ...

INFERRED ENTITIES (marked as inferred — confirm before including):
  [Entity 1] ([Type]) — INFERRED: [reason for inference]
  [Entity 2] ([Type]) — INFERRED: [reason]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into              : semantic-clustering.skill
                          content-generation-engine.skill
                          optimization-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Output Field Definitions

| Field         | Definition                                                                                                  |
|---------------|-------------------------------------------------------------------------------------------------------------|
| `Entity`      | The canonical name of the entity — the most common, recognized form of its name                            |
| `Type`        | The sub-type within its category (Tool / Software / Technology / Concept / Method / Expert / Organization / Process / Workflow / Metric / KPI) |
| `Relevance`   | HIGH / MEDIUM / LOW — TF-IDF-simulated relevance score                                                      |
| `Frequency`   | How many of the analyzed SERP pages mention this entity (N/N pages)                                        |
| `Priority`    | CRITICAL / IMPORTANT / OPTIONAL — content inclusion priority                                               |
| `AEO Tag`     | DEF (definition target) / FAQ (FAQ target) / HOW (how-to target) / QUOTE (expert quote source) / REF (reference/citation target) / — (no AEO targeting) |
| `Why It Matters` | Specific reason this entity matters to the topic — not generic                                         |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — DEFINE ENTITY CATEGORIES

Before any extraction begins, establish the full taxonomy of entity types this skill will search for across all data sources. This taxonomy ensures no entity category is missed during extraction.

**The 7 Entity Categories:**

---

**Category 1 — Tools / Software**

Definition: Named products, platforms, applications, or digital tools that are directly relevant to the topic.

| Sub-Type      | Description                                                                   | Examples (generic)                          |
|---------------|-------------------------------------------------------------------------------|---------------------------------------------|
| Tool          | A standalone utility or feature used to perform a specific task              | Any named software utility                  |
| Software      | A full platform or application suite                                         | CRM platforms, analytics suites             |
| Plugin        | An extension or add-on that extends another software's functionality         | Browser extensions, CMS plugins             |
| API           | A programmatic interface used to connect systems                             | Named APIs relevant to the topic            |
| Framework     | A structured system for building or implementing something                   | Development frameworks, methodology tools   |

Detection Signals: Named with capital letters; appear in comparison sections; have version numbers; have official brand names.

---

**Category 2 — Technologies**

Definition: Underlying technical standards, architectures, protocols, or technology types that form the infrastructure of the topic's domain.

| Sub-Type       | Description                                                                  | Examples (generic)                           |
|----------------|------------------------------------------------------------------------------|----------------------------------------------|
| Protocol       | A defined standard for communication or data exchange                       | Named communication or data protocols       |
| Architecture   | A structural design pattern for building systems                            | Named system design patterns                |
| Language       | A programming, markup, or query language                                    | Programming and query languages             |
| Standard       | An industry-defined specification or format                                 | File formats, data standards, specifications|
| Infrastructure | A foundational technology layer                                             | Cloud platforms, database types             |

Detection Signals: Abbreviated names (all-caps or camelCase); appear in technical sections; often co-occur with "based on" or "built on."

---

**Category 3 — Concepts / Methods**

Definition: Named theories, methodologies, frameworks, approaches, or conceptual constructs that are central to understanding the topic.

| Sub-Type      | Description                                                                   |
|---------------|-------------------------------------------------------------------------------|
| Concept       | An abstract idea or named principle                                           |
| Method        | A named approach or technique for accomplishing something                     |
| Framework     | A conceptual structure for organizing thought or action (distinct from software frameworks) |
| Model         | A named theoretical representation of how something works                    |
| Strategy      | A named strategic approach                                                    |

Detection Signals: Appear in educational/definitional sections; frequently appear as H2 or H3 headings; often followed by "is a" or "involves."

---

**Category 4 — People / Experts**

Definition: Named individuals who are recognized authorities on the topic — researchers, practitioners, thought leaders, or experts whose work or opinions are relevant.

| Sub-Type      | Description                                                                   |
|---------------|-------------------------------------------------------------------------------|
| Researcher    | Academic or industry researcher who has published relevant work              |
| Practitioner  | Named professional known for applying the topic's methods in practice        |
| Thought Leader| Industry figure whose perspectives define or shape the topic's direction     |
| Author        | Named author of relevant books, studies, or foundational content             |

Detection Signals: Full names with titles; appear in quote sections; cited by name with professional context.

---

**Category 5 — Organizations**

Definition: Named companies, institutions, standards bodies, associations, regulatory bodies, or research organizations with a significant relationship to the topic.

| Sub-Type          | Description                                                               |
|-------------------|---------------------------------------------------------------------------|
| Company           | A named business that creates products or services relevant to the topic  |
| Institution       | An academic or research institution relevant to the topic                 |
| Standards Body    | An organization that defines industry standards                          |
| Association       | An industry group or professional association                            |
| Regulatory Body   | A government or regulatory organization with jurisdiction over the topic |

Detection Signals: Preceded by articles like "the" or "by"; appear in authority or reference sections; co-occur with "published by," "according to," "certified by."

---

**Category 6 — Processes / Workflows**

Definition: Named sequences of steps, operations, or procedures that describe how something is done within the topic's domain.

| Sub-Type      | Description                                                                   |
|---------------|-------------------------------------------------------------------------------|
| Process       | A defined sequence of operations with a specific start and end               |
| Workflow      | A structured sequence of tasks often involving multiple actors or systems     |
| Procedure     | A standardized method for performing a specific task                         |
| Pipeline      | A chained sequence of processes where output of one feeds into the next      |
| Lifecycle     | A recurring process with defined stages from inception to completion         |

Detection Signals: Often contain verbs or action words in their names; appear in "how to" and step-by-step sections; frequently described with phase or stage terminology.

---

**Category 7 — Metrics / KPIs**

Definition: Named quantitative measurements, performance indicators, or scoring systems used to evaluate outcomes within the topic's domain.

| Sub-Type      | Description                                                                   |
|---------------|-------------------------------------------------------------------------------|
| Metric        | A specific measurable value                                                   |
| KPI           | A key performance indicator used to track progress toward a goal             |
| Score         | A composite or indexed measurement                                           |
| Rate          | A ratio-based measurement (e.g., conversion rate, churn rate)                |
| Index         | A normalized or weighted composite measurement                               |

Detection Signals: Appear in measurement or performance sections; co-occur with numbers and percentages; often abbreviated with acronyms.

---

**Category Inventory Initialization:**

Before extraction begins, build a Category Inventory — an empty structured container for each of the 7 categories. All entities extracted in Steps 2 and 3 are deposited into the appropriate category container before classification occurs.

---

### ▸ STEP 2 — SERP ENTITY EXTRACTION

Extract all candidate entities from the SERP data provided by `deep-serp-analysis.skill`. Extraction focuses on three content zones where entities carry the strongest topical authority signal.

**Phase 2A — Extraction Zones and Their Entity Signals**

**Zone 1 — Page Titles and H1 Headings:**

Entities appearing in page titles and H1 headings are the highest-confidence entities — they were important enough to be used as the defining label for the entire page.

Extraction method:
- Collect all page titles from the analyzed SERP pages.
- Identify named entities (capitalized nouns, named concepts, product names, technical terms).
- Record entity name + page count (how many titles contain this entity).
- Entities in ≥3 titles → classify as HIGH relevance candidates.

**Zone 2 — H2 and H3 Headings:**

Entities in subheadings reveal the topic's structural semantic field — what sub-topics and named concepts are treated as important enough to warrant their own sections.

Extraction method:
- Collect all H2 and H3 headings from all analyzed pages (from `deep-serp-analysis.skill` Dimension 1).
- Identify all named entities across the heading set.
- Record entity name + heading count (how many headings contain this entity across all pages).
- Note position: H2 entities carry more weight than H3 entities.

**Zone 3 — Intro Sections (First 200 Words):**

Entities in introductory sections are the ones authors judge essential to establish from the start — they are the definitional anchors of the topic.

Extraction method:
- From snippet data and intro analysis (where available from `deep-serp-analysis.skill`).
- Extract entities that appear in the opening content of ranking pages.
- Record entity name + appearance count.

**Zone 4 — PAA Questions (from serp-intelligence.skill if available):**

PAA questions reveal which entities Google considers relevant enough to surface as follow-up queries. Every named entity in a PAA question is a semantically significant entity for the topic.

Extraction method:
- Parse all PAA questions for named entities.
- Every entity in a PAA question → classify as at minimum MEDIUM relevance.

**Phase 2B — Keyword Universe Entity Extraction**

Beyond SERP page analysis, extract entities from the keyword universe produced by `keyword-discovery.skill`.

Extraction method:
- Scan all 8 keyword dimensions for named entities.
- Dimension 3 (Comparison): Named tools and products in "A vs B" keywords are entities.
- Dimension 4 (Alternatives): Named tools in "alternatives to X" keywords are entities.
- Dimension 7 (Semantic Expansion): Tools, processes, and concepts in semantic expansion keywords are entities.
- Dimension 8 (Niche Modifiers): Industry and use-case names become Organization or Process entities.

**Phase 2C — Competitor Content Entity Extraction (Optional)**

If competitor content is available as a raw text input:
- Parse headings, bold text, and first paragraphs for named entities.
- Entities appearing in multiple competitor pieces are higher-relevance candidates.
- Flag all competitor-derived entities with source: `COMPETITOR CONTENT`.

**Phase 2D — Entity Candidate Pool Assembly**

After all three zones are processed:
1. Combine all extracted entity candidates into a single pool.
2. Remove clearly non-entity items (generic adjectives, stop words, numerical values without named context).
3. Group all instances of the same entity (accounting for synonyms and abbreviations — e.g., "Customer Relationship Management software" and "CRM software" → same entity: "CRM").
4. Count total mentions across all zones for each unique entity.
5. Deposit each entity candidate into its Category Inventory container.

---

### ▸ STEP 3 — FREQUENCY SCORING (TF-IDF SIMULATION)

Apply a TF-IDF-simulated scoring model to every entity candidate. This model assigns each entity a Relevance level based on how frequently and prominently it appears across the analyzed SERP pages — prioritizing entities that are consistently important over entities that appear sporadically.

**Frequency Scoring Framework:**

**Term Frequency Component (TF):**

Count how many of the analyzed SERP pages mention each entity, and in what zone:

| Zone                     | Weight per Mention |
|--------------------------|--------------------|
| Page title / H1          | 3.0×               |
| H2 heading               | 2.0×               |
| H3 heading               | 1.5×               |
| Intro (first 200 words)  | 1.5×               |
| PAA question             | 2.0×               |
| Keyword universe         | 1.0×               |
| Competitor content       | 1.0×               |

**Weighted TF Calculation per Entity:**

`TF_score = Σ(zone_weight × zone_mention_count) across all pages`

**Inverse Document Frequency Component (IDF — Simulated):**

The IDF component penalizes entities that are too generic — entities that would appear in nearly any content on any topic (e.g., "content," "users," "data"). These are not semantically distinctive.

Apply a penalization factor:

| Entity Specificity                                           | IDF Multiplier |
|--------------------------------------------------------------|----------------|
| Highly specific (named tool, named person, named metric)     | 1.0× (no penalty) |
| Moderately specific (named concept, named process)           | 0.9×           |
| Generic (common noun with no named reference)                | 0.5×           |
| Universal (applicable to all topics, e.g., "best practices") | 0.2×           |

**Combined Relevance Score:**

`Relevance_score = TF_score × IDF_multiplier`

**Relevance Level Classification:**

| Relevance Score Range    | Relevance Level |
|--------------------------|-----------------|
| Top 33% of all scores    | HIGH            |
| Middle 33% of all scores | MEDIUM          |
| Bottom 33% of all scores | LOW             |

Note: The score boundaries are relative to the extracted entity set — not absolute values. The top third of this specific entity pool is HIGH, regardless of raw score.

**Frequency Documentation:**

For every entity, record:
- Raw mention count (total across all pages and zones).
- Page count (how many unique pages mention the entity — reported as N/N pages).
- Weighted TF score.
- IDF multiplier applied.
- Final Relevance Level.

---

### ▸ STEP 4 — ENTITY CLASSIFICATION

After frequency scoring, formally classify every entity in the candidate pool.

**Phase 4A — Entity Type Assignment**

Assign every entity to its most precise sub-type within its category:

| Entity                  | Category  | Sub-Type     | Classification Rationale                              |
|-------------------------|-----------|--------------|-------------------------------------------------------|
| [Named CRM Platform]    | Tool      | Software     | Full application suite — not a single-purpose utility |
| [Named Communication Protocol] | Technology | Protocol | Defines rules for data exchange — not a product    |
| [Named Framework]       | Concept   | Framework    | Conceptual structure — not software                   |
| [Named Researcher]      | Person    | Researcher   | Published academic or industry research               |
| [Named Standards Body]  | Org       | Standards Body| Defines industry specifications                      |
| [Named Process]         | Process   | Workflow     | Multi-step sequence involving multiple actors         |
| [Named Rate]            | Metric    | Rate         | Ratio-based quantitative measurement                  |

**Phase 4B — Canonical Name Normalization**

For every classified entity, establish its canonical name — the single, authoritative form of the entity's name to be used consistently throughout all output:

Rules:
1. Use the entity's official or most widely recognized name.
2. For tools and software → use the official product name (exact capitalization).
3. For acronyms → include the full form alongside the abbreviation on first use: `"Customer Acquisition Cost (CAC)"`.
4. For concepts → use the most established academic or industry term.
5. For people → use full name with professional identifier.
6. Never use informal nicknames as the canonical name.

**Phase 4C — Relevance Validation**

After classification, validate that every HIGH relevance entity genuinely belongs to the topic's semantic field:

Validation question: "Would a recognized expert writing about [topic] naturally include this entity in their content without being prompted?"
- YES → confirmed as HIGH relevance.
- UNCERTAIN → downgrade to MEDIUM.
- NO → downgrade to LOW or remove entirely.

This validation prevents frequency bias — an entity that appears in many pages due to coincidental co-occurrence rather than genuine semantic relevance.

---

### ▸ STEP 5 — ENTITY GAP DETECTION

Compare the SERP entity set against the user's existing content to identify which entities are missing from the user's coverage and which are present but underdeveloped.

**Phase 5A — User Content Entity Extraction**

If user existing content is available:
1. Extract entities from the user's published articles (titles, headings, and body content).
2. Apply the same 7-category taxonomy.
3. Classify user-present entities by depth of treatment:

| Depth Level     | Definition                                                                            |
|-----------------|---------------------------------------------------------------------------------------|
| MENTIONED       | Entity name appears once or twice with no explanation                                 |
| DEFINED         | Entity is given a brief definition or explanation (1–2 sentences)                    |
| EXPLAINED       | Entity is covered with context, use cases, or examples (1 paragraph)                |
| DEEP            | Entity is given dedicated coverage with examples, comparisons, or data               |

**Phase 5B — Gap Identification**

For each entity in the SERP entity set:
1. Is it present in the user's content? → YES / NO.
2. If YES → at what depth level?
3. If NO → record as MISSING entity.
4. If YES but depth < required level (determined by entity priority) → record as UNDERUSED entity.

**Required Depth by Priority:**

| Entity Priority | Required Depth Level | Gap Threshold                                          |
|-----------------|---------------------|--------------------------------------------------------|
| CRITICAL        | EXPLAINED or DEEP   | Missing or MENTIONED or DEFINED = CRITICAL GAP         |
| IMPORTANT       | DEFINED or EXPLAINED| Missing or MENTIONED = SIGNIFICANT GAP                |
| OPTIONAL        | MENTIONED           | Missing = MINOR GAP                                    |

**Phase 5C — Gap Severity Classification**

| Gap Type         | Severity      | Implication                                                                       |
|------------------|---------------|-----------------------------------------------------------------------------------|
| CRITICAL GAP     | CRITICAL      | The user's content is missing an entity that nearly all top competitors include. This is a topical authority deficit. |
| SIGNIFICANT GAP  | HIGH          | The user's content mentions or misses an entity that most competitors cover at greater depth. |
| MINOR GAP        | LOW           | The user's content does not include an optional entity that a few competitors cover. |
| UNDERUSE GAP     | MEDIUM        | The entity is present but not covered at the depth required for competitive parity. |

**Phase 5D — Gap Prioritization**

Rank all gaps by:
1. Entity priority (CRITICAL > IMPORTANT > OPTIONAL).
2. SERP frequency (higher-frequency entities = higher-priority gaps).
3. Gap severity (CRITICAL GAP > UNDERUSE GAP).

The highest-ranking gaps feed directly into the `content-generation-engine.skill` as the first entities to address.

---

### ▸ STEP 6 — RELATIONSHIP MAPPING

Define the semantic relationships between entities. This relationship map is the connective tissue of the topic's semantic architecture — it tells `content-generation-engine.skill` not just which entities to include, but how they relate to each other in the content.

**Phase 6A — Relationship Type Taxonomy**

| Relationship Type     | Symbol    | Meaning                                                                   |
|-----------------------|-----------|---------------------------------------------------------------------------|
| Dependency            | requires  | Entity A cannot function or be understood without Entity B               |
| Functional            | used in   | Entity A (tool/method) is used within Entity B (process/workflow)        |
| Measurement           | measures  | Entity A (metric) quantifies performance of Entity B (outcome/concept)  |
| Hierarchical          | is a type of | Entity A is a specific instance or subcategory of Entity B           |
| Enabling              | produces  | Entity A (process/method) creates or generates Entity B (outcome/metric)|
| Contrasting           | differs from | Entity A and Entity B serve similar purposes but in distinct ways    |
| Sequential            | precedes  | Entity A occurs before Entity B in a process or workflow                |
| Compositional         | is part of | Entity A is a component or element of Entity B                        |

**Phase 6B — Relationship Identification Protocol**

For every pair of entities where a meaningful relationship exists:

1. Identify the relationship type from the taxonomy.
2. Express it as: `[Entity A] — [relationship] → [Entity B]`.
3. Add a one-sentence context note explaining why this relationship matters for content.
4. Record the relationship in the Relationship Map.

**Relationship Identification Guidance by Category:**

| Entity Pair Type                        | Likely Relationship          | Example Pattern                               |
|-----------------------------------------|------------------------------|-----------------------------------------------|
| Tool + Process                          | used in                      | [Tool] — used in → [Process name]            |
| Concept + Concept (foundational)        | requires                     | [Advanced concept] — requires → [Basic concept] |
| Metric + Outcome                        | measures                     | [Metric name] — measures → [Outcome/Goal]     |
| Specific Method + General Method        | is a type of                 | [Specific] — is a type of → [General]         |
| Process + Output                        | produces                     | [Process] — produces → [Entity/Artifact]      |
| Sequential Processes                    | precedes                     | [Step 1 entity] — precedes → [Step 2 entity] |
| Tool + More Advanced Tool               | contrasts with               | [Basic tool] — differs from → [Advanced tool] |
| Tool + Platform (it runs on)            | is part of                   | [Feature] — is part of → [Platform name]     |

**Phase 6C — Relationship Cluster Formation**

After all individual relationships are mapped, identify relationship clusters — groups of entities that are densely interconnected:

1. A relationship cluster exists when 3+ entities are all linked to each other through at least one relationship each.
2. Name each cluster with a descriptive label.
3. Note which content section in the blueprint should cover each relationship cluster.

**Relationship Density Rule:**

No relationship should be manufactured. Every relationship documented must reflect a genuine semantic or functional connection. If two entities appear in the same topic domain but have no meaningful relationship to each other → do not create a relationship entry.

---

### ▸ STEP 7 — AEO ENTITY TAGGING

Identify which entities are suitable targets for answer engine optimization — the structural content elements that Google's featured snippets, AI Overviews, and PAA boxes extract.

**Phase 7A — AEO Tag Definitions**

| AEO Tag | Definition                                                                               | Best Format               | Snippet Type Targeted           |
|---------|------------------------------------------------------------------------------------------|---------------------------|---------------------------------|
| `DEF`   | Entity is a named concept or term that users commonly search for a definition of        | 2–3 sentence paragraph     | Featured snippet (Paragraph)   |
| `FAQ`   | Entity is the subject of a common question that appears in PAA or question-based searches| Question + 50-80w answer  | PAA / FAQ schema               |
| `HOW`   | Entity is a named process or method where users search for implementation guidance      | Numbered steps             | Featured snippet (List/HowTo)  |
| `QUOTE` | Entity is a person or organization whose statements are frequently referenced as evidence | Attribution block          | Knowledge Panel / authority signal |
| `REF`   | Entity is an organization or study that is cited as a primary source of authority       | Inline citation + link     | E-E-A-T / authority signal     |
| `—`     | Entity is present for topical coverage but does not have a specific AEO targeting angle | Any format                 | N/A                             |

**Phase 7B — AEO Tagging Protocol**

For every entity in the full entity table, evaluate it against each AEO tag:

**DEF qualification:**
- Is this a named concept, technology, or method that has a clear, searchable definition?
- Does the SERP for `"what is [entity]"` likely produce a featured snippet?
- Would defining this entity add genuine value to readers at this topic's funnel stage?
- If YES to all three → assign DEF tag.

**FAQ qualification:**
- Does this entity appear as the subject of a PAA question in `serp-intelligence.skill` data?
- Is this entity likely to generate question-based searches ("how to use [entity]", "when to use [entity]", "is [entity] worth it")?
- If YES to either → assign FAQ tag.

**HOW qualification:**
- Is this entity a process, workflow, or method?
- Do users commonly search for implementation guidance for this entity?
- If YES to both → assign HOW tag.

**QUOTE qualification:**
- Is this entity a named person or organization whose published statements are widely cited in the topic domain?
- Would quoting this entity improve the content's E-E-A-T signals?
- If YES to both → assign QUOTE tag.

**REF qualification:**
- Is this entity an organization, institution, or study that is recognized as a primary data or authority source for the topic?
- If YES → assign REF tag.

**Multi-Tag Assignment:**
An entity may receive multiple AEO tags if it qualifies for more than one (e.g., a concept that is both worth defining AND worth asking a FAQ question about → DEF + FAQ).

**Phase 7C — AEO Entity Integration Guidance**

For every AEO-tagged entity, produce a brief integration note:

```
Entity: [Name]
AEO Tag: [DEF / FAQ / HOW / QUOTE / REF]
Target Query: "[specific search query this entity targets in AEO context]"
Content Placement: [which section of the blueprint should handle this entity's AEO content]
Format Recommendation: [paragraph / numbered list / attribution / table]
```

---

### ▸ STEP 8 — PRIORITIZATION

Assign every entity a final priority classification that drives content inclusion decisions in `content-generation-engine.skill`.

**Phase 8A — Priority Classification Criteria**

| Priority Level | Definition                                                                                      | Inclusion Requirement                                |
|----------------|-------------------------------------------------------------------------------------------------|------------------------------------------------------|
| **CRITICAL**   | Entity is present in HIGH frequency across the SERP AND belongs to the core semantic field of the topic. A content piece that omits this entity signals shallow topical understanding. | Must be explicitly covered in the content. Missing = competitive deficit. |
| **IMPORTANT**  | Entity is present in MEDIUM frequency OR is in HIGH frequency but slightly peripheral to the core semantic field. A content piece that omits this entity is less complete but not critically deficient. | Should be included where relevant. Omission weakens but does not disqualify. |
| **OPTIONAL**   | Entity is LOW frequency OR is a niche/specialist element relevant only to specific audience segments or use cases. | Include if the target audience is expert-level or if the content aims for exhaustive coverage. |

**Phase 8B — Priority Assignment Rules**

Priority is determined by the combination of Relevance Level (from Step 3) and Entity Category weight:

| Relevance Level | Category Weight | Priority Result |
|-----------------|-----------------|-----------------|
| HIGH            | Core category*  | CRITICAL        |
| HIGH            | Secondary category** | IMPORTANT  |
| MEDIUM          | Core category   | IMPORTANT       |
| MEDIUM          | Secondary category | OPTIONAL     |
| LOW             | Any category    | OPTIONAL        |
| Any             | (Entity is inferred, not SERP-confirmed) | OPTIONAL + INFERRED flag |

*Core categories for most topics: Concepts/Methods, Processes/Workflows, Metrics/KPIs
**Secondary categories: People/Experts, Organizations, Tools/Software, Technologies (may be core for technical topics)

**Note:** Category weight is topic-dependent. For a technical programming topic, Technologies is a core category. For a marketing strategy topic, Concepts/Methods and Metrics/KPIs are core. Adjust category weighting based on topic domain before applying the priority matrix.

**Phase 8C — CRITICAL Entity Validation**

Before finalizing CRITICAL designations, run a validation check:

For each CRITICAL entity, ask:
1. Does omitting this entity produce a content piece that is clearly incomplete compared to the top-ranking pages? → YES → confirm CRITICAL.
2. Would a domain expert reading the content be surprised that this entity is not mentioned? → YES → confirm CRITICAL.
3. Does the entity appear in the titles or H2 headings of ≥50% of analyzed SERP pages? → YES → confirm CRITICAL.

If an entity does not satisfy at least 2 of the 3 validation checks → downgrade to IMPORTANT.

**Phase 8D — Priority Summary Construction**

After all entities are assigned priorities:
1. List all CRITICAL entities together (Part 6 of output).
2. List all IMPORTANT entities together.
3. List all OPTIONAL entities together.
4. List all INFERRED entities separately with their inference basis.
5. Calculate counts for the output header.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ SEMANTIC RELEVANCE SIMULATION

Beyond frequency scoring, this skill simulates semantic relevance by evaluating each entity against the conceptual field of the topic — not just how often it appears, but how central it is to the topic's meaning.

**Semantic Centrality Test:**

For each entity, evaluate:

| Question                                                                             | Signal                           |
|--------------------------------------------------------------------------------------|----------------------------------|
| Is this entity typically taught in introductory coverage of the topic?              | HIGH semantic centrality         |
| Does understanding this topic require understanding this entity?                    | HIGH semantic centrality         |
| Is this entity unique to this topic domain (not shared with many other domains)?    | HIGH topical specificity         |
| Does this entity appear in the topic's Wikipedia article (if applicable)?           | CONFIRMED as semantically central|
| Is this entity referenced in the definitions of other entities in the same topic?   | HIGH semantic dependency         |
| Could this entity appear equally naturally in content about a different topic?      | LOW topical specificity          |

Entities with HIGH semantic centrality AND HIGH topical specificity → elevated to CRITICAL regardless of frequency score.

Entities with LOW topical specificity (generic entities that could belong to any topic) → downgraded by one priority level regardless of frequency.

---

### ▸ ENTITY CLUSTER MAPPING

Group entities into semantic clusters — not based on their category type, but based on how they are conceptually related in the content.

**Cluster Formation Rules:**

1. A cluster forms when ≥3 entities share a primary relationship (e.g., all relate to the same process, or all are tools used in the same workflow).
2. Every entity in the full entity table must be assigned to at least one cluster.
3. An entity may belong to two clusters if it connects two semantic areas of the topic.
4. Clusters should align with content sections in the blueprint — each cluster maps to approximately one H2 section.

**Cluster Naming Convention:**

Cluster names describe the conceptual territory, not the entity list:
- ✅ "CRM Data Management" (describes the territory)
- ✅ "Sales Performance Measurement" (describes the territory)
- ❌ "Group 1: Metrics and Tools" (generic label)
- ❌ "Cluster A" (no descriptive value)

---

### ▸ CONTEXTUAL IMPORTANCE OVERRIDE

Frequency is the primary relevance signal, but contextual importance can override it in specific circumstances.

**Override Conditions:**

| Condition                                                                           | Override Action                                                     |
|-------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Entity appears in only 1–2 SERP pages but is the most cited entity in PAA questions | Override to HIGH relevance                                         |
| Entity is a foundational concept that other HIGH-relevance entities depend on (per relationship map) | Override to CRITICAL regardless of frequency          |
| Entity appears rarely in the SERP but is universally known as essential by domain experts | Override to IMPORTANT with "expert knowledge" note            |
| Entity is an emerging tool or concept not yet widely covered but highly relevant   | Flag as EMERGING — keep at OPTIONAL but note growth trajectory     |
| Entity dominates one page that occupies position #1 but rarely appears elsewhere  | Keep at frequency-determined level; flag as "position-1 signal"   |

---

### ▸ LOW-VALUE ENTITY FILTERING

Prevent the entity table from being diluted by generic, non-informative, or irrelevant entities.

**Low-Value Entity Identification:**

An entity is low-value if it meets ANY of the following conditions:
- It is a generic noun that could appear in content about any topic (e.g., "users," "teams," "data," "content").
- It is a placeholder name with no recognized canonical form in the domain.
- Its IDF multiplier is 0.2 (universal penalty applied in Step 3).
- It has no meaningful relationship to any other entity in the map (isolated entity with no connections).
- Its "Why It Matters" reason cannot be written in a specific, topic-grounded way.

Low-value entities are:
- Removed from the entity table entirely.
- Logged in the extraction deduplication log.
- Not counted in the output total.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — Canonical Name Deduplication

Every entity must appear exactly once in the output under its canonical name. If the same entity is extracted multiple times under different names (full name, abbreviation, alias, informal name):
1. Identify all name variants.
2. Select the canonical name (most formal, most widely recognized).
3. Merge all variant mentions into the single canonical entity record.
4. Sum all variant mention counts into the merged total.
5. Log all aliases: "CRM = Customer Relationship Management software = Customer Relationship Manager."

### Rule 2 — Semantic Entity Deduplication

Two entities that describe the same real-world thing but from different angles are semantic duplicates:
- `"Sales Pipeline"` and `"Sales Funnel"` — not identical, but semantically overlapping.
- Detection: if two entities have the same core meaning, the same relationship partners, and the same AEO treatment → merge into the more established term; note the alternative as an alias.

Semantic duplicate test: "Would including both entities in the same sentence feel redundant?" → YES → merge.

### Rule 3 — Category Assignment Deduplication

Each entity belongs to exactly one category. If an entity could be classified in two categories (e.g., a named framework that is both a Concept AND a Tool):
- Assign to the category that best represents its primary function in the topic domain.
- Note the secondary classification in the entity's record.
- Do not list the entity in two category sections.

### Rule 4 — Relationship Deduplication

Each entity pair has at most one primary relationship in the map. If multiple relationship types could describe the same pair:
- Select the most specific relationship type.
- Note secondary relationship as a parenthetical if meaningful.
- Do not create two separate relationship entries for the same pair.

### Rule 5 — Priority Deduplication

Each entity receives exactly one priority classification. Priority cannot be both CRITICAL and IMPORTANT. If scoring produces a borderline result (on the boundary between two levels) → assign the higher priority and document the borderline note.

### Rule 6 — Memory Deduplication

Query `memory-controller.skill` before producing the entity table. If an entity extraction report for this keyword exists within freshness threshold (14 days):
- Load the cached report.
- Present to user with timestamp.
- Ask: "Use existing entity map or refresh?"
- Never store two entity maps for the same keyword in the same freshness window.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Entity Reporting Behaviors

| Prohibited Behavior                                                                             | Reason                                                                          |
|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Including a generic noun (e.g., "data," "process," "users") as a named entity                  | Generic nouns are not entities — they are background vocabulary                |
| Writing a "Why It Matters" reason that applies to any entity in that category                  | Every reason must be specific to that entity and its role in THIS topic        |
| Assigning CRITICAL priority without citing at minimum 2 validation checks (Phase 8C)           | CRITICAL is earned through validation — not assigned by default                |
| Listing the same entity twice under different names without merging                             | Duplication inflates entity counts and corrupts downstream skill inputs        |
| Creating a relationship between two entities that have no documented semantic connection        | Relationships must be real — not manufactured to appear comprehensive          |
| Including an entity with a LOW IDF multiplier (generic) without explicit contextual justification | Generic entities dilute the entity map's quality                            |
| Assigning AEO tags to entities that are not genuinely AEO-targetable                          | AEO tags must reflect actual snippet or schema opportunities — not aspirations  |
| Omitting the "Why It Matters" field for any entity, for any reason                             | Every entity requires a specific reason — no entity is self-evidently important |
| Recording a frequency of N/N pages without specifying which zone the entity appeared in        | Zone matters for weighting — a title-zone entity and a body-zone entity have different signals |
| Including more OPTIONAL entities than CRITICAL + IMPORTANT entities combined                   | The entity table should be dense with high-value entities, not padded with optionals |

### Required Entity Reporting Behaviors

- Every entity record must have all fields populated: Entity, Type, Relevance, Frequency (with zone), Priority, AEO Tag, Why It Matters.
- Every CRITICAL entity must be validated against Phase 8C criteria.
- Every relationship in the map must include a context note.
- Every inferred entity must be explicitly flagged as INFERRED with its inference basis.
- The entity table must be organized by category — not by priority or alphabetical order.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ deep-serp-analysis.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary entity extraction source)                                     |
| **Receives**     | Per-page titles, H2/H3 headings, intro excerpts, semantic keyword signals (D4), PAA data   |
| **Extraction zones** | Titles, headings, intro content, PAA questions                                          |

---

### ▸ keyword-discovery.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (secondary entity extraction source)                                   |
| **Receives**     | Full keyword universe (all 8 dimensions), semantic neighbor data                            |
| **Used for**     | Extracting named entities from comparison, alternative, and semantic expansion keywords     |

---

### ▸ serp-intelligence.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream context source (optional)                                                          |
| **Receives**     | PAA questions, SERP feature data, dominant intent                                           |
| **Used for**     | Enriching AEO entity tagging (Step 7) and PAA-based entity extraction (Phase 2A Zone 4)    |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | entity-extraction → semantic-clustering (output consumer)                                   |
| **Sends**        | Entity cluster map, entity-to-cluster assignments, relationship map                         |
| **Used for**     | Enriching cluster definitions with entity coverage requirements per cluster                 |

---

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | entity-extraction → content-generation-engine (primary downstream consumer)                |
| **Sends**        | Full entity table, priority summary, AEO entity map, relationship map                      |
| **Used for**     | Ensuring generated content includes all Critical entities, correctly contextualized and related |
| **Critical field** | Priority Summary (Part 6) — tells the generation engine which entities are non-negotiable |

---

### ▸ optimization-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | entity-extraction → optimization-engine (output consumer)                                   |
| **Sends**        | Full entity table, entity gap analysis (Part 2), relationship map                          |
| **Used for**     | Post-generation entity coverage validation and content optimization scoring                 |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ before extraction, WRITE after output finalization                     |
| **Reads**        | Prior entity extraction reports for this keyword (freshness check)                          |
| **Writes**       | Complete entity intelligence report, entity-to-keyword lifecycle updates                   |
| **Memory layer** | strategy-memory.skill                                                                        |
| **Timing**       | READ at pre-conditions. WRITE after Step 8 completes.                                       |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                               | Recovery Action                                                                                              |
|---------------------------------------------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| `deep-serp-analysis.skill` data is unavailable                           | Primary input missing                                   | Switch to inference mode. Extract entities from keyword universe alone. Flag all entities as INFERRED. Reduce priority of all CRITICAL designations to IMPORTANT pending SERP validation. |
| `keyword-discovery.skill` output is unavailable                          | Secondary input missing                                 | Extract entities from topic name and any available SERP data only. Flag knowledge gaps. Note: semantic expansion entities (Dimension 7) will be missing. |
| Both primary inputs are unavailable                                       | No SERP data, no keyword universe                       | Operate in keyword-only inference mode. Extract entities from topic inference alone. Flag ALL entities as INFERRED. Output confidence = LOW. Recommend running upstream skills before accepting this output. |
| Entity candidate pool is smaller than 10 entities after extraction        | Low candidate count                                     | Expand extraction to include body content signals (not just titles and headings). If still <10 after expansion → proceed with available entities; flag output as THIN ENTITY MAP. |
| All extracted entities fall into the same category                        | Category diversity check fails (only 1-2 categories)    | Flag as CATEGORY IMBALANCE. Explicitly note which categories are absent. Recommend: expand keyword research or target a more specific sub-topic where fuller entity coverage exists. |
| No relationship can be established between entities                       | Relationship map is empty after Phase 6B               | Flag as ISOLATED ENTITIES. Produce entity table without relationship map. Note: content generation will lack semantic connection guidance. Recommend manual expert review of relationships. |
| User existing content is unavailable for gap analysis                     | Step 5 cannot run                                       | Produce SERP-only entity map. Note: "Gap analysis skipped — no user content available." Recommend user provides existing content for gap detection before content generation. |
| Entity extraction produces more than 80 entities                          | Output size exceeds practical downstream processing     | Apply strict low-value entity filter. Remove all LOW relevance entities that are not AEO-tagged. Target final entity count of 30–60. Flag the reduction: "Entity count reduced from [N] to [N] via quality filtering." |
| Memory controller is unavailable                                          | READ query returns error                                | Proceed without freshness check. Flag: "Memory unavailable — prior entity extraction may be duplicated." |
| An entity appears in contradictory zones (high title frequency, zero heading frequency) | Zone distribution is anomalous | Accept extraction. Flag the anomaly in the entity's record. Analyze whether it may be an outlier result signal. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — ENTITY CATEGORY QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
7-CATEGORY ENTITY TAXONOMY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CAT 1 — TOOLS / SOFTWARE
  Sub-types: Tool | Software | Plugin | API | Framework(software)
  Signals: Named products; brand names; version numbers

CAT 2 — TECHNOLOGIES
  Sub-types: Protocol | Architecture | Language | Standard | Infrastructure
  Signals: All-caps abbreviations; "based on" co-occurrence; technical specifications

CAT 3 — CONCEPTS / METHODS
  Sub-types: Concept | Method | Framework(conceptual) | Model | Strategy
  Signals: Definitional sections; "what is"; appears as H2 topic headings

CAT 4 — PEOPLE / EXPERTS
  Sub-types: Researcher | Practitioner | Thought Leader | Author
  Signals: Full names; co-occurrence with quotes or attribution; "according to"

CAT 5 — ORGANIZATIONS
  Sub-types: Company | Institution | Standards Body | Association | Regulatory Body
  Signals: "The"; "by"; "according to"; "published by"; "certified by"

CAT 6 — PROCESSES / WORKFLOWS
  Sub-types: Process | Workflow | Procedure | Pipeline | Lifecycle
  Signals: Action-verb names; "stage"; "phase"; "step"; process-flow headings

CAT 7 — METRICS / KPIS
  Sub-types: Metric | KPI | Score | Rate | Index
  Signals: Paired with numbers/percentages; measurement sections; performance headings
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — AEO TAG DECISION TREE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
AEO TAGGING DECISION TREE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
For each entity, answer in order:

1. Is this entity a named concept, technology, or method?
   AND is "what is [entity]" a plausible search query?
   → YES: Add DEF tag. Continue to next question.
   → NO: Skip DEF.

2. Does this entity appear in a PAA question?
   OR is "how/when/why [entity]" a plausible search query?
   → YES: Add FAQ tag. Continue.
   → NO: Skip FAQ.

3. Is this entity a named process, workflow, or method?
   AND do users search for HOW to implement it?
   → YES: Add HOW tag. Continue.
   → NO: Skip HOW.

4. Is this entity a named person or organization?
   AND are their statements widely cited for authority?
   → YES: Add QUOTE tag. Continue.
   → NO: Skip QUOTE.

5. Is this entity an organization, institution, or study?
   AND is it commonly cited as a primary source for data or standards?
   → YES: Add REF tag.
   → NO: Skip REF.

If no tags were assigned → assign "—" (no AEO targeting).
Multiple tags are permitted if multiple questions answer YES.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — RELATIONSHIP MAPPING QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
RELATIONSHIP TYPE REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DEPENDENCY:   [A] — requires → [B]
  Use when: A cannot be implemented or understood without B

FUNCTIONAL:   [A] — used in → [B]
  Use when: A (tool/method) is applied within B (process/workflow)

MEASUREMENT:  [A] — measures → [B]
  Use when: A (metric) quantifies the performance of B (outcome)

HIERARCHICAL: [A] — is a type of → [B]
  Use when: A is a specific instance or subcategory of the broader B

ENABLING:     [A] — produces → [B]
  Use when: Performing A (process) generates or creates B (output)

CONTRASTING:  [A] — differs from → [B]
  Use when: A and B serve similar purposes but in meaningfully distinct ways

SEQUENTIAL:   [A] — precedes → [B]
  Use when: A always occurs before B in a defined sequence or workflow

COMPOSITIONAL:[A] — is part of → [B]
  Use when: A is a component, feature, or element of the larger B

RULE: One relationship per entity pair.
RULE: Never manufacture relationships — only document genuine connections.
RULE: Every relationship requires a one-sentence context note.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: research/entity-extraction.skill*
*Nexus SEO Operating System — Version 1.0.0*
