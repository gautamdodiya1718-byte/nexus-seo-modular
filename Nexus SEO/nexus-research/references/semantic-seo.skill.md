# SKILL: strategy/semantic-seo.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Strategy / Semantic Intelligence
# EXECUTION PRIORITY: ON-DEMAND — Triggered by menu item 9 or keywords like "topic cluster," "entity optimization," "semantic SEO," "topical authority"
# POSITION: The topical authority architect. Moves Nexus from single-page optimization to whole-topic domination. Produces topic cluster maps, entity maps, semantic gap analysis, and publication sequences that make Google see you as THE authority on a subject.
# DEPENDENCIES: keyword-discovery.skill, semantic-clustering-v2.skill, entity-extraction.skill, serp-intelligence.skill, google-signals-extractor.skill, internal-linking.skill, roadmap-engine.skill, gap-opportunity-engine.skill

---

## SECTION 1 — ROLE

Semantic SEO is NOT about keywords. It's about meaning, context, entities, and relationships.

Google's Knowledge Graph processes 800 billion+ facts about 8 billion+ entities. Modern search algorithms (Hummingbird, RankBrain, BERT, MUM) understand concepts and intent — not just word matching. A single article on "Playwright test automation" won't rank if competitors have 15-20 interconnected articles covering every subtopic. Topical authority is built by coverage depth, not individual page quality.

This skill produces the architecture for topical domination: what to write, in what order, how to connect it, and what entities to optimize for.

### What This Skill Produces
1. **Topic Cluster Map** — pillar page + 10-20 cluster pages with defined relationships
2. **Entity Map** — all entities to optimize for (people, tools, concepts, organizations)
3. **Semantic Gap Analysis** — subtopics your site is missing vs. what SERP leaders cover
4. **Internal Linking Architecture** — how cluster pages connect to each other and the pillar
5. **Publication Sequence** — which pages to write first for maximum topical authority buildup
6. **Semantic Content Brief per Page** — what each page must cover to be topically complete

---

## SECTION 2 — TOPIC CLUSTER ARCHITECTURE

### The 3 Components

**A. Pillar Page**
- Broad, comprehensive coverage of the core topic
- Targets the head keyword (high volume, high competition)
- 3,000-5,000 words — covers everything at overview depth
- Links OUT to every cluster page
- Gets linked TO from every cluster page
- Example: "Playwright Test Automation — The Complete Guide"

**B. Cluster Pages**
- Deep, specific coverage of 1 subtopic each
- Targets long-tail keywords (lower volume, lower competition)
- 1,500-3,000 words — goes deep on the specific angle
- Links TO the pillar page
- Links TO 2-3 related cluster pages
- Example: "Playwright Parallel Testing with Sharding"

**C. Internal Linking Mesh**
- Every cluster → pillar (mandatory)
- Pillar → every cluster (mandatory)
- Cluster → 2-3 related clusters (contextual, semantically relevant)
- Anchor text uses semantic variants, NOT exact-match keywords for every link
- No orphan pages — every page in the cluster must be reachable from at least 3 other pages

### How to Build a Topic Cluster

**Step 1 — Identify the Core Topic**
- What broad topic do you want to own? (e.g., "Playwright test automation")
- Check: does this topic have enough subtopics to support 10-20 cluster pages?
- Check: is this aligned with the brand's expertise?
  Read brand.content_pillars and brand.industry from active brand file.
  Flag any cluster that falls outside these areas.

**Step 2 — Map All Subtopics**
Use these sources to discover subtopics:
- `keyword-discovery.skill.md` — find all keyword variations
- `google-signals-extractor.skill.md` — PAA questions at depth 3-4, autocomplete, related searches
- `serp-intelligence.skill.md` — what subtopics do top 10 results cover?
- Web search for "site:competitor.com [topic]" — what pages do competitors have?
- Also search: forums (Reddit, StackOverflow), YouTube videos, conference talks

**Step 3 — Cluster the Subtopics**
Use `semantic-clustering-v2.skill.md` to group subtopics by semantic similarity.
Each cluster becomes 1 cluster page. Rules:
- Each cluster page targets 1 primary long-tail keyword
- Each covers a specific INTENT (informational, tutorial, comparison, troubleshooting)
- No 2 cluster pages should target the same intent for the same subtopic (cannibalization)
- Minimum 10 cluster pages, maximum 25 per pillar

**Step 4 — Define the Entity Map (Section 3)**

**Step 5 — Build Internal Linking Architecture (Section 5)**

**Step 6 — Determine Publication Sequence (Section 6)**

---

## SECTION 3 — ENTITY OPTIMIZATION

Entities are the building blocks of semantic search. Google doesn't just match keywords — it identifies entities (people, products, organizations, concepts) and their relationships.

### Entity Mapping Process

**Step 1 — Extract Entities from the Topic**
For the core topic, identify ALL entities:

| Entity Type | Examples for "Playwright Test Automation" |
|---|---|
| **Tools/Products** | Read brand.features (brand's own products) + top entities from SERP analysis for this keyword cluster. Never hardcode entities from a specific brand. |
| **Concepts** | Test automation, end-to-end testing, browser automation, test parallelization, flaky tests, CI/CD |
| **People** | Andrey Lushnikov (Playwright creator), relevant thought leaders |
| **Organizations** | Microsoft (Playwright maintainer), testing tool companies |
| **Standards/Specs** | WebDriver protocol, Chrome DevTools Protocol (CDP) |
| **Formats/Languages** | TypeScript, JavaScript, Python, Java, C# |
| **Actions** | Install, configure, debug, shard, retry, screenshot, trace |

**Step 2 — Check Knowledge Graph Presence**
For each entity, search Google and check:
- Does it have a Knowledge Panel? (strong entity recognition)
- Does it appear in Google's autocomplete? (entity awareness)
- Is it linked to related entities in search results? (relationship mapping)

**Step 3 — Schema Markup for Entities**
For each entity mentioned in content, ensure:
- It's wrapped in appropriate schema (SoftwareApplication, Person, Organization)
- Relationships between entities are declared (creator, maintainer, alternative)
- Entity attributes are explicit (version, language, license)

**Step 4 — Entity Consistency Across Content**
- Same entity referenced the same way across all cluster pages
- Entity descriptions are consistent (don't describe Playwright differently in each article)
- Cross-referencing between pages reinforces entity relationships

### Entity Map Output Format

```
ENTITY MAP: [Core Topic]
━━━━━━━━━━━━━━━━━━━━━━━━

Primary Entity: [main tool/concept]
  Type: [SoftwareApplication / Concept / Organization]
  Knowledge Graph: [YES / NO / PARTIAL]
  Schema: [recommended schema type]
  
Related Entities:
  [Entity 1]: [type] — relationship: [how it relates to primary]
  [Entity 2]: [type] — relationship: [how it relates to primary]
  ...

Entity Coverage per Cluster Page:
  Pillar page: ALL entities mentioned, primary entity central
  Cluster 1: [which entities are relevant]
  Cluster 2: [which entities are relevant]
  ...

Missing Entities (competitors have, you don't):
  [Entity]: found on [competitor URL] — recommendation: [add/skip]
```

---

## SECTION 4 — SEMANTIC GAP ANALYSIS

This identifies what subtopics competitors cover that you don't.

### Process

**Step 1 — Crawl Competitor Topic Coverage**
For top 3 SERP competitors in the topic space:
- Fetch their sitemap or crawl their blog
- List ALL pages related to the core topic
- Map their coverage: which subtopics do they have pages for?

**Step 2 — Compare Against Your Coverage**
- Fetch sitemap from brand.site_url/sitemap.xml
- List all pages you have on the topic
- Create a coverage matrix:

```
SEMANTIC GAP MATRIX: [Core Topic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Subtopic              | You | Comp 1 | Comp 2 | Comp 3 | Gap? |
|-----------------------|-----|--------|--------|--------|------|
| [Subtopic 1]          | ✅  | ✅     | ✅     | ✅     | NO   |
| [Subtopic 2]          | ❌  | ✅     | ✅     | ✅     | YES  |
| [Subtopic 3]          | ✅  | ❌     | ✅     | ❌     | NO   |
| [Subtopic 4]          | ❌  | ❌     | ✅     | ✅     | YES  |
| [Subtopic 5]          | ❌  | ✅     | ✅     | ✅     | YES  |

GAPS TO FILL (priority order):
1. [Subtopic 2] — all 3 competitors cover it, you don't. HIGH PRIORITY.
2. [Subtopic 5] — all 3 competitors cover it. HIGH PRIORITY.
3. [Subtopic 4] — 2 of 3 cover it. MEDIUM PRIORITY.

UNIQUE OPPORTUNITIES (you could cover, nobody does):
- [Subtopic X] — discovered via PAA/autocomplete, no competitor coverage
```

**Step 3 — Semantic Completeness Score**
Calculate: (subtopics you cover) / (total subtopics in the space) × 100
- Above 80% = Strong topical authority
- 50-80% = Moderate — fill gaps to strengthen
- Below 50% = Weak — need systematic cluster buildout

---

## SECTION 5 — INTERNAL LINKING ARCHITECTURE

### Hub-and-Spoke + Mesh Model

The internal linking for a topic cluster follows this structure:

```
                    ┌─────────────┐
              ┌─────│ PILLAR PAGE │─────┐
              │     └──────┬──────┘     │
              │            │            │
         ┌────▼────┐  ┌───▼────┐  ┌───▼────┐
         │Cluster 1│──│Cluster2│──│Cluster3│
         └────┬────┘  └───┬────┘  └───┬────┘
              │           │           │
         ┌────▼────┐  ┌───▼────┐  ┌───▼────┐
         │Cluster 4│──│Cluster5│──│Cluster6│
         └─────────┘  └────────┘  └────────┘
```

- Pillar ↔ every cluster (bidirectional)
- Related clusters ↔ each other (2-3 connections per cluster)
- No cluster is an orphan (every page linked from 3+ other pages)

### Anchor Text Rules for Semantic Linking
- Use semantic variants — NOT the same anchor text for every link to the same page
- Pillar → Cluster: use the cluster's specific subtopic phrase
- Cluster → Pillar: use a broader topic reference or brand + topic combination
- Cluster → Cluster: use contextual phrases that describe the relationship

**BAD:** Every link to the pillar says "Playwright test automation guide"
**GOOD:** Links say "complete Playwright guide," "our main Playwright resource," "the full testing walkthrough," "as covered in the parent guide"

### Output: Internal Linking Plan

```
INTERNAL LINKING ARCHITECTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Pillar: [URL] — "[Title]"
Links OUT to: [list all cluster URLs with anchor text]
Links IN from: [list all cluster URLs]

Cluster 1: [URL] — "[Title]"
  → Pillar: [anchor text]
  → Cluster 3: [anchor text + context]
  → Cluster 5: [anchor text + context]
  ← Linked from: Pillar, Cluster 2, Cluster 4

[Repeat for each cluster]

Orphan check: [PASS — all pages linked from 3+ sources / FAIL — list orphans]
```

---

## SECTION 6 — PUBLICATION SEQUENCE

The ORDER you publish cluster pages matters for topical authority buildup.

### Sequencing Rules

**Rule 1 — Pillar First**
Publish the pillar page first. It establishes your topic ownership. Cluster pages are added as they're published, with internal links connecting them.

**Rule 2 — Foundational Clusters Second**
Publish cluster pages that cover the most fundamental subtopics first. These are the ones that OTHER cluster pages will reference.
- "What is [topic]" / "Introduction to [topic]" = foundational
- "Advanced [specific technique]" = build on top of foundation

**Rule 3 — High-Gap Clusters Third**
Publish the subtopics that fill the biggest semantic gaps (from Section 4) — the ones all competitors have and you don't.

**Rule 4 — Unique Angle Clusters Last**
Publish your unique-take content last — these differentiate but don't fill gaps.

**Rule 5 — Cadence**
- Publish 2-3 cluster pages per week for maximum velocity
- Update pillar page with new internal links after each cluster publishes
- Don't publish all at once — Google values steady content velocity

### Output: Publication Sequence

```
PUBLICATION SEQUENCE: [Core Topic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Week 1: [PILLAR] — [Title] — [Primary Keyword]
Week 1: [CLUSTER] — [Title] — [Keyword] — foundational
Week 2: [CLUSTER] — [Title] — [Keyword] — foundational
Week 2: [CLUSTER] — [Title] — [Keyword] — gap-fill (HIGH)
Week 3: [CLUSTER] — [Title] — [Keyword] — gap-fill (HIGH)
Week 3: [CLUSTER] — [Title] — [Keyword] — gap-fill (MEDIUM)
Week 4: [CLUSTER] — [Title] — [Keyword] — unique angle
...

After each publish:
  - Add internal links from pillar → new cluster
  - Add internal links from related existing clusters → new cluster
  - Update sitemap
```

---

## SECTION 7 — SEMANTIC CONTENT BRIEF (PER PAGE)

For each page in the cluster, produce a brief that ensures semantic completeness:

```
SEMANTIC CONTENT BRIEF
━━━━━━━━━━━━━━━━━━━━━━

Page: [Cluster/Pillar]
Title: [Title]
Primary Keyword: [keyword]
Secondary Keywords: [list]
Search Intent: [informational / tutorial / comparison / troubleshooting]

ENTITIES TO INCLUDE:
  Required: [entity 1], [entity 2], [entity 3]
  Optional: [entity 4], [entity 5]

SEMANTIC TERMS TO COVER (LSI/NLP-derived):
  [term 1], [term 2], [term 3], [term 4], [term 5]...
  (These should appear naturally — not stuffed)

QUESTIONS THIS PAGE MUST ANSWER:
  1. [PAA question 1]
  2. [PAA question 2]
  3. [Related question from autocomplete]
  4. [Follow-up question a reader would logically ask]

SUBTOPICS TO COVER (for topical completeness):
  - [Subtopic A] — must cover for semantic score
  - [Subtopic B] — competitors all cover this
  - [Subtopic C] — unique angle opportunity

INTERNAL LINKS TO PLACE:
  → Pillar: [URL] with anchor "[text]"
  → Cluster [X]: [URL] with anchor "[text]"
  → Cluster [Y]: [URL] with anchor "[text]"

SCHEMA:
  [Recommended schema type for this page]

DEPTH TARGET:
  [X] code examples, [Y] tables, [Z] data points
  (Based on competitive depth benchmark)
━━━━━━━━━━━━━━━━━━━━━━
```

---

## SECTION 8 — FULL OUTPUT FORMAT

When the Semantic SEO pipeline runs, deliver ALL of these:

```
═══════════════════════════════════════
SEMANTIC SEO STRATEGY
Brand: [brand.brand_name]
Core Topic: [topic]
Date: [date]
═══════════════════════════════════════

1. TOPIC CLUSTER MAP
   Pillar: [title + keyword]
   Clusters: [N] pages
   [Visual list of all pages with keywords and intents]

2. ENTITY MAP
   [Primary entity + all related entities with types and relationships]

3. SEMANTIC GAP ANALYSIS
   [Coverage matrix — you vs competitors]
   Completeness score: [X]%
   Gaps to fill: [list with priority]

4. INTERNAL LINKING ARCHITECTURE
   [Hub-and-spoke diagram with all connections and anchor text]

5. PUBLICATION SEQUENCE
   [Week-by-week publishing order with rationale]

6. SEMANTIC CONTENT BRIEFS
   [1 brief per page — keyword, entities, questions, subtopics, links]

═══════════════════════════════════════
NEXT STEPS:
- Start with pillar page using Nexus Content Pipeline (menu item 1)
- Each cluster page: use Nexus Content Pipeline with this brief as input
- After each publish: update internal links across existing pages
═══════════════════════════════════════
```

---

## SECTION 9 — INTEGRATION WITH OTHER NEXUS SKILLS

| When Combined With | How It Integrates |
|---|---|
| **Menu 1 (Write blog)** | Semantic brief becomes the input for content-research-engine. Cluster keyword + entities + questions feed directly into the content pipeline. |
| **Menu 3 (Keyword research)** | Keyword discovery feeds into topic cluster mapping. Clusters from keyword research become cluster pages. |
| **Menu 7 (Competitor analysis)** | Competitor data feeds into semantic gap analysis. Competitor topic coverage becomes the benchmark. |
| **Menu 10 (Programmatic SEO)** | If the cluster has a repeating pattern (e.g., "[tool] with Playwright" × 10 tools), use programmatic-seo to generate the cluster pages from a template instead of writing each manually. |
| **Menu 8 (Full strategy)** | Semantic SEO output becomes the content architecture section of the full strategy. |

---

*Semantic SEO — v1.0.0 | Nexus SEO Operating System | Topical Authority Architecture*
