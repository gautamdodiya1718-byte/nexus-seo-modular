# SKILL: content/internal-linking.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Strategic Architecture
# EXECUTION PRIORITY: POST-INVENTORY AND POST-CLUSTERING — Runs after content-inventory.skill and semantic-clustering.skill are complete. Can be re-run whenever new content is added.
# POSITION: Internal link architecture engine. Converts the content inventory and cluster architecture into a complete, prioritized, placement-specific internal linking system.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **internal link architecture engine** of the Nexus system.

Internal links are not a cosmetic feature. They are the mechanism by which topical authority is distributed across a site's content — the structural plumbing that signals to search engines which pages are the most important, how topic clusters are organized, and how readers should navigate from one piece of content to the next in a coherent journey. A site with excellent content but poor internal linking is a site where authority accumulates in disconnected pockets, pillar pages are under-reinforced, supporting pages are invisible to crawlers, and readers who finish one article have no guided path to the content that would serve them next.

This skill builds the internal linking system that prevents all of those outcomes. It processes the complete content inventory and cluster architecture simultaneously — mapping every page to its cluster, identifying the pillar pages that should receive authority, modeling authority flow across the cluster network, detecting orphaned pages with no incoming links, flagging cannibalization risks where the same anchor text points to competing pages, and producing a complete, placement-specific, anchor-text-assigned linking plan that content teams can implement directly.

### Primary Responsibilities

| Responsibility                        | Description                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Cluster-Based Link Mapping**        | For every cluster, build the hub-and-spoke linking structure connecting pillar pages and supporting pages            |
| **Funnel-Based Link Routing**         | Route links along the reader journey — TOFU → MOFU → BOFU — so each piece of content guides readers toward conversion |
| **Anchor Text Strategy**              | Assign every link an anchor text that follows the 20/60/20 distribution rule and avoids generic anchors              |
| **Contextual Placement Specification**| For every link, specify exactly where in the source page the link should be placed                                   |
| **Orphan Detection**                  | Identify pages with zero or near-zero incoming links and classify their risk level                                   |
| **Orphan Resolution**                 | For every orphan, identify which existing pages should link to it and with what anchor text                          |
| **Pillar Reinforcement Planning**     | Identify the top 3–5 pillar pages; assess their current incoming link count; recommend additional link sources       |
| **Cannibalization Detection**         | Identify same-anchor-different-page conflicts and same-intent link patterns that create ranking confusion             |
| **Authority Flow Modeling**           | Model how PageRank-style authority distributes across the link graph and identify authority concentration and gaps    |
| **Cluster Strength Weighting**        | Weight link priority by cluster strategic strength — high-strength cluster links are prioritized for implementation  |
| **Link Deduplication**                | Ensure no page pair is linked more than once per direction; no anchor text is reused for the same target page        |
| **Implementation Priority Scoring**  | Score every link recommendation by authority impact so content teams implement the highest-value links first         |

### What This Skill Is NOT

- It is **not** a link builder. It builds internal links — not external backlinks.
- It is **not** a content generator. It specifies where links should go — it does not write the anchor text into articles (that is the content team's implementation task).
- It is **not** a live crawler. It works with the content inventory — it does not crawl the live site.
- It is **not** a one-time operation. Every time new content is published, this skill should be re-run or updated to incorporate the new page.
- It is **not** optional for sites with more than 10 pages. Without a linking system, topical authority cannot flow coherently.

### The Foundational Principle of This Skill

> Internal links are authority votes that you control completely.
>
> Every link from a supporting page to its pillar passes authority to the pages that define the cluster.
> Every link from a TOFU article to a MOFU resource guides a reader toward evaluation.
> Every link from a MOFU comparison to a BOFU landing page guides a reader toward conversion.
>
> This skill designs that system intentionally — not accidentally.
> Every link has a reason. Every anchor text has a strategy. Every placement has a context.
> No link exists because "it seemed relevant." Links exist because they serve a specific function in the authority and reader journey architecture.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Input Source                      | Fields Consumed                                                                                        | Required |
|-----------------------------------|--------------------------------------------------------------------------------------------------------|----------|
| `content-inventory.skill`         | All content records — URL, primary keyword, secondary keywords, intent (TOFU/MOFU/BOFU), cluster name, status (published/draft/planned), coverage score | YES |
| `semantic-clustering.skill`       | Full cluster architecture — cluster names, tier (Pillar/Sub/Micro), keyword lists, cluster strength scores, pillar designations, cross-references | YES |

### Optional Inputs

| Input Source                      | Fields Consumed                                                              | Used For                                                     |
|-----------------------------------|------------------------------------------------------------------------------|--------------------------------------------------------------|
| `authority-engine.skill`          | Authority scores per cluster, pillar page URLs, missing element reports     | Pillar reinforcement prioritization (Step 7)                |
| `query-graph.skill`               | Node types (Hub/Connector/Leaf), Hub Scores, dependency chains              | Authority flow modeling (Advanced Logic)                    |
| `existing_link_structure`         | Current internal links already in place (if a site audit was performed)    | Deduplication and avoiding re-recommending existing links   |
| `conversion_pages`                | Landing pages, pricing pages, product pages — BOFU destinations            | Funnel linking terminal point mapping (Step 2)              |
| `domain`                          | Target site domain                                                          | URL normalization and relative link generation              |

### Input Pre-Conditions

1. **Is the content inventory complete?** If inventory only includes published pages → produce the linking plan for published content only. Note: draft and planned pages are included in orphan detection (as future pages that will need links when published).
2. **Is the cluster architecture available?** If not → apply keyword-similarity-based clustering from inventory keywords only. Flag: "Cluster architecture absent — linking plan based on keyword proximity, not formal cluster structure."
3. **Is the existing link structure available?** If yes → exclude already-existing links from new recommendations. If no → produce all recommendations; flag: "Existing links unknown — some recommendations may duplicate existing links. Verify before implementing."
4. **Has this skill been run before for this domain?** → Query `memory-controller.skill`. If a prior linking plan exists → offer delta mode (add links for new pages since last run). If first run or full rebuild requested → proceed completely.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Internal Link Architecture Package

```
NEXUS INTERNAL LINK ARCHITECTURE PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Domain                  : [domain]
Pages Analyzed          : [count — published]
Clusters Processed      : [count]
Total Links Recommended : [count]
  HIGH Priority Links   : [count]
  MEDIUM Priority Links : [count]
  LOW Priority Links    : [count]
Orphan Pages Detected   : [count]
At-Risk Pages Detected  : [count]
Pillar Pages Identified : [count]
Cannibalization Issues  : [count]
Generated By            : content/internal-linking.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — INTERNAL LINKING TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | Source Page                 | Target Page                 | Anchor Text                    | Placement             | Link Type       | Priority | Cluster           |
|---|-----------------------------|-----------------------------|--------------------------------|-----------------------|-----------------|----------|-------------------|
| 1 | /[source-slug]              | /[target-slug]              | [anchor text]                  | [placement]           | CLUSTER-PILLAR  | HIGH     | [cluster name]    |
| 2 | /[source-slug]              | /[target-slug]              | [anchor text]                  | [placement]           | FUNNEL-BRIDGE   | HIGH     | [cluster name]    |
| 3 | /[source-slug]              | /[target-slug]              | [anchor text]                  | [placement]           | CLUSTER-SIBLING | MEDIUM   | [cluster name]    |
| 4 | /[source-slug]              | /[target-slug]              | [anchor text]                  | [placement]           | CROSS-CLUSTER   | MEDIUM   | [cluster name]    |
| 5 | /[source-slug]              | /[target-slug]              | [anchor text]                  | [placement]           | ORPHAN-RESCUE   | HIGH     | [cluster name]    |
...

LINK TYPE LEGEND:
  CLUSTER-PILLAR : Supporting page → its cluster pillar page (authority reinforcement)
  CLUSTER-SIBLING: Sub-page → another sub-page within same cluster (topical completeness)
  FUNNEL-BRIDGE  : TOFU page → MOFU page / MOFU page → BOFU page (reader journey)
  CROSS-CLUSTER  : Page in Cluster A → page in adjacent Cluster B (thematic relationship)
  ORPHAN-RESCUE  : New link to a page that currently has 0 incoming links
  PILLAR-BOOST   : Additional link to a pillar page from a distant but relevant page
  CONVERSION     : BOFU or MOFU page → conversion/landing/pricing page

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — ORPHAN PAGE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ORPHAN PAGES (0 incoming links):
| # | Page URL           | Title / Topic                | Cluster          | Funnel | Fix Source Pages                                  | Suggested Anchor Text         |
|---|--------------------|-----------------------------|------------------|--------|---------------------------------------------------|-------------------------------|
| 1 | /[slug]            | [topic]                     | [cluster name]   | TOFU   | /[page1-slug], /[page2-slug]                     | [anchor text]                 |
| 2 | /[slug]            | [topic]                     | [cluster name]   | MOFU   | /[page1-slug]                                    | [anchor text]                 |

AT-RISK PAGES (only 1 incoming link):
| # | Page URL           | Title / Topic                | Cluster          | Incoming Links | Risk Level | Fix Source Page              |
|---|--------------------|-----------------------------|------------------|----------------|------------|------------------------------|
| 1 | /[slug]            | [topic]                     | [cluster name]   | 1              | AT RISK    | /[additional-source-slug]    |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — LINK BOOST PLAN (Pillar Pages)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Pillar Page            | Cluster          | Current Incoming Links | Target Links | Links to Add | Priority Sources                              |
|------------------------|------------------|------------------------|--------------|--------------|-----------------------------------------------|
| /[pillar-slug]         | [cluster name]   | [N]                    | [N+N]        | [N]          | /[source1], /[source2], /[source3]           |
| /[pillar-slug]         | [cluster name]   | [N]                    | [N+N]        | [N]          | /[source1], /[source2]                       |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — CANNIBALIZATION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | Issue Type              | Anchor Text Used    | Page 1              | Page 2              | Recommendation                           |
|---|-------------------------|---------------------|---------------------|---------------------|------------------------------------------|
| 1 | SAME ANCHOR SPLIT       | "[anchor]"          | /[slug-1]           | /[slug-2]           | Use "[anchor-1]" for page 1; "[anchor-2]" for page 2 |
| 2 | INTENT DUPLICATION      | [topic/intent]      | /[slug-1]           | /[slug-2]           | Designate primary; redirect or differentiate |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — AUTHORITY FLOW SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CURRENT AUTHORITY DISTRIBUTION:
  Pages with 0 incoming internal links: [count] — [%]
  Pages with 1–2 incoming links        : [count] — [%]
  Pages with 3–5 incoming links        : [count] — [%]
  Pages with 6+ incoming links         : [count] — [%]
  Most-linked page                     : [URL] — [N] incoming links
  Least-linked pages (published)       : [count] with 0 links

POST-IMPLEMENTATION PROJECTION (after all HIGH links are added):
  Orphan pages resolved                : [count]
  Pillar pages incoming link gain      : [N per pillar]
  Authority bottlenecks addressed      : [count]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISPATCH READY
Feeds Into             : authority-engine.skill
                         roadmap-engine.skill
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Per-Link Record Format (Supporting Detail)

```
LINK RECORD — #[N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Link Type       : [CLUSTER-PILLAR / CLUSTER-SIBLING / FUNNEL-BRIDGE / CROSS-CLUSTER / ORPHAN-RESCUE / PILLAR-BOOST / CONVERSION]
Priority        : [HIGH / MEDIUM / LOW / OPTIONAL]
Source Page     : [URL] — "[page title]"
Target Page     : [URL] — "[page title]"
Cluster         : [cluster name]
Anchor Text     : [exact recommended anchor text]
Anchor Type     : [EXACT / PARTIAL / SEMANTIC]
Placement       : [INTRO / WITHIN-PARAGRAPH / NEAR-MENTION / CONCLUSION / FAQ-SECTION]
Placement Detail: [specific location description — e.g., "after the paragraph defining CRM in the intro section"]
Rationale       : [one sentence explaining why this link serves the reader and the authority architecture]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — CLUSTER-BASED LINKING

Build the foundational linking architecture for every cluster in the content inventory. The cluster-based model establishes the hub-and-spoke structure that makes topical authority legible to search engines.

**Phase 1A — Cluster Inventory Assembly**

For each cluster in the architecture:
1. Identify all published pages assigned to this cluster.
2. Identify the pillar page (if designated in the cluster architecture or authority engine data).
3. Categorize all other pages as:
   - **Sub-pages:** Direct supporting content for the cluster's core topic.
   - **Micro-pages:** Highly specific, long-tail articles within the cluster.
4. Record the funnel stage (TOFU/MOFU/BOFU) of each page in the cluster.

**Phase 1B — Hub-and-Spoke Link Construction**

For each cluster with a pillar page:

**Rule 1 — Sub-pages link TO the pillar (upward authority flow):**

Every published sub-page and micro-page in the cluster should have at least one link pointing to the cluster's pillar page. This is the fundamental authority aggregation mechanism.

- Link source: every non-pillar page in the cluster.
- Link target: the pillar page.
- Priority: HIGH for the closest sub-pages; MEDIUM for micro-pages further from the pillar's topic.
- Anchor text: Use a partial match or semantic anchor referencing the pillar's primary keyword.

**Rule 2 — The pillar page links TO key sub-pages (downward authority distribution):**

The pillar page should link to its major sub-pages — distributing its authority to the pages that prove the cluster's depth.

- Link source: pillar page.
- Link target: the 3–5 most comprehensive supporting pages in the cluster.
- Priority: HIGH — the pillar's outbound links to supporting pages signal cluster membership.
- Anchor text: Use the sub-page's primary keyword as a partial match anchor.

**Rule 3 — Sub-pages link to related sub-pages within the cluster (peer authority):**

Pages within the same cluster that cover adjacent sub-topics should link to each other where the topic of one is directly relevant to a passage in the other.

- Link source: any sub-page.
- Link target: another sub-page in the same cluster — but only where there is a genuine topical connection (not all-to-all).
- Priority: MEDIUM for natural topical connections; LOW for more distant connections.
- Anchor text: Use the target sub-page's primary keyword or a semantic variant.

**Phase 1C — Pillar Page Identification Protocol**

If no pillar page is formally designated in the cluster data:

Infer the pillar page from these signals (in priority order):

| Signal                                                              | Pillar Indicator     |
|---------------------------------------------------------------------|----------------------|
| URL is shortest in the cluster (fewest path components)            | HIGH confidence      |
| Primary keyword matches the cluster name directly                  | HIGH confidence      |
| Coverage score is highest in the cluster (≥4)                     | MEDIUM confidence    |
| Title contains "Guide," "Complete," "Ultimate," or "Overview"     | MEDIUM confidence    |
| Page has the most H2 sections of any page in the cluster          | MEDIUM confidence    |
| Page targets the broadest (shortest) keyword in the cluster        | MEDIUM confidence    |

If no page meets two or more of these signals → flag the cluster as PILLAR ABSENT. Note in the output: "Cluster [Name] has no clear pillar page — all sub-pages are treated as peers. Recommend creating a pillar page."

**Phase 1D — Cluster Linking Table Assembly**

For each cluster, produce a cluster-level linking summary:

```
CLUSTER LINKING SUMMARY: [Cluster Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Pillar Page       : [URL or ABSENT]
Sub-pages         : [count] — [URL list]
Micro-pages       : [count]
Links to create:
  Sub → Pillar    : [count] links
  Pillar → Subs   : [count] links
  Sibling links   : [count] links
Total new links   : [count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 2 — FUNNEL LINKING LOGIC

Build the reader journey linking layer — connecting pages across funnel stages so a reader entering at awareness can be guided toward evaluation and then toward conversion.

**Phase 2A — Funnel Stage Mapping**

From the content inventory, build the funnel map:

```
FUNNEL MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOFU pages (Informational):
  [URL] — [topic] — [cluster]
  [URL] — [topic] — [cluster]

MOFU pages (Commercial):
  [URL] — [topic] — [cluster]
  [URL] — [topic] — [cluster]

BOFU pages (Transactional):
  [URL] — [topic] — [cluster]
  [URL] — [topic] — [cluster]

Conversion pages (if provided):
  [URL] — [type: pricing / landing / product]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 2B — Funnel Bridge Identification**

A funnel bridge link connects a page at one funnel stage to a page at the next stage within the same topical territory:

**TOFU → MOFU Bridges:**

For each TOFU page:
1. Identify which topic(s) the TOFU page covers.
2. Find the MOFU page that evaluates the options or decisions a reader who finished this TOFU page would naturally face next.
3. If a matching MOFU page exists in the same or adjacent cluster → create a FUNNEL-BRIDGE link.
4. If no MOFU page exists for this TOFU topic → flag as FUNNEL GAP — MOFU MISSING for the cluster.

**MOFU → BOFU Bridges:**

For each MOFU page:
1. Identify the decision the MOFU page is helping the reader make.
2. Find the BOFU page or conversion page that a reader ready to act would naturally go to next.
3. If a matching BOFU or conversion page exists → create a FUNNEL-BRIDGE link.
4. If no BOFU/conversion page exists → flag as FUNNEL GAP — BOFU MISSING.

**BOFU → Conversion:**

For BOFU pages that are not themselves conversion pages:
1. Link to the most relevant pricing page, free trial page, or product landing page.
2. This is typically the highest-value link in the entire internal linking plan — it directly connects organic traffic to conversion.
3. Priority: HIGH for all BOFU-to-conversion links.

**Phase 2C — Funnel Bridge Priority Rules**

| Bridge Type              | Priority | Rationale                                                                |
|--------------------------|----------|--------------------------------------------------------------------------|
| TOFU → MOFU (same cluster) | HIGH   | Keeps reader within the site as they progress from awareness to evaluation |
| MOFU → BOFU (same cluster) | HIGH   | Highest-value funnel progression — evaluation to decision               |
| BOFU → Conversion          | HIGH   | Direct revenue path — never leave this unlinked                         |
| TOFU → BOFU (skip MOFU)    | MEDIUM | Reader bypass — acceptable when no MOFU page exists; lower authority transfer |
| TOFU → MOFU (adjacent cluster) | MEDIUM | Cross-cluster funnel progression                                    |
| MOFU → BOFU (adjacent cluster) | MEDIUM | Cross-cluster conversion path                                       |

**Phase 2D — Funnel Gap Logging**

When funnel gaps are detected (TOFU exists but MOFU is absent for a topic; MOFU exists but BOFU is absent):
1. Log the gap in the funnel gap register.
2. Flag it for `roadmap-engine.skill` as a content creation priority.
3. The funnel bridge link cannot be created until the target page exists — it is marked as PENDING for when the missing page is published.

---

### ▸ STEP 3 — ANCHOR TEXT STRATEGY

Assign anchor text to every link in the plan following the 20/60/20 distribution rule and the specific anchor type guidelines.

**Phase 3A — Anchor Text Type Definitions**

| Type              | Definition                                                                        | Target Distribution |
|-------------------|-----------------------------------------------------------------------------------|---------------------|
| **EXACT MATCH**   | Anchor text is identical to the target page's primary keyword                    | 20% of all links    |
| **PARTIAL MATCH** | Anchor text includes the target's primary keyword within a longer descriptive phrase | 60% of all links   |
| **SEMANTIC**      | Anchor text describes the target page's topic using related terminology — no exact keyword match | 20% of all links |

**Phase 3B — Anchor Text Distribution Enforcement**

After all anchor texts are assigned across the full link plan:
1. Count the distribution of EXACT / PARTIAL / SEMANTIC anchors for links pointing to each high-priority page (pillar pages and top cluster pages).
2. Adjust if the distribution is significantly outside the 20/60/20 guideline:
   - Too many EXACT anchors to a single page → convert the excess to PARTIAL anchors.
   - Too many SEMANTIC anchors to a page that needs stronger keyword signals → convert some to PARTIAL.
3. Apply this enforcement only to pages with 3+ incoming links in the plan. Single-link pages receive whatever anchor type best fits their content context.

**Phase 3C — Anchor Text Generation Rules**

**For EXACT MATCH anchors:**
- Use the target page's exact primary keyword.
- Only use in a sentence where the keyword genuinely serves as a noun or adjective phrase.
- Example: Target keyword = "crm software for startups" → Anchor: `crm software for startups`

**For PARTIAL MATCH anchors:**
- Include the target keyword within a phrase that provides natural sentence context.
- The phrase must read naturally in the source page's context — the link should not interrupt flow.
- Example: Target keyword = "crm software for startups" → Anchor: `the best crm software for startups`, `choosing crm software for your startup`, `CRM software options built for startup sales teams`

**For SEMANTIC anchors:**
- Describe the target page's topic or value using related terms — not the primary keyword itself.
- Example: Target keyword = "crm software for startups" → Anchor: `contact management tools for early-stage companies`, `startup pipeline tools`, `sales tracking solutions for growing teams`

**Phase 3D — Prohibited Anchor Text Patterns**

```
PROHIBITED ANCHOR TEXT — NEVER USE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Generic anchors (provide no topical signal):
  "click here"           "read more"            "learn more"
  "here"                 "this article"         "this page"
  "this post"            "more information"     "find out more"
  "see here"             "view more"            "check this out"

Bare URL anchors:
  "https://example.com/crm-guide"
  "www.example.com/blog/crm"

Brand name as sole anchor for content pages:
  Using only the brand name for an article page (acceptable only for homepage links)

Keyword-stuffed anchors:
  "best crm software best crm tools crm comparison"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 3E — Target Page Anchor Uniqueness Rule**

For every target page, no two source pages in the same linking plan should use the identical anchor text string for that target. If the same target page receives links from multiple sources → vary the anchor text across sources while maintaining the type distribution.

This prevents an unnaturally uniform anchor profile that search engines associate with manipulative linking patterns.

---

### ▸ STEP 4 — CONTEXTUAL PLACEMENT

Specify the exact placement location for every link in the plan. Generic placement guidance ("put it somewhere in the article") is prohibited — every link must have a specific placement.

**Phase 4A — Placement Type Definitions**

| Placement Type        | Definition                                                                                        | Best For                           |
|-----------------------|---------------------------------------------------------------------------------------------------|------------------------------------|
| **INTRO**             | Within the first 200 words of the article, near the topic's definition or framing               | Pillar links; fundamental topic links |
| **WITHIN-PARAGRAPH**  | Embedded within a body paragraph where the target topic is naturally referenced                  | Most sibling and cross-cluster links |
| **NEAR-MENTION**      | Immediately following the first or most prominent mention of the target topic in the body        | Keyword-triggered linking         |
| **SECTION-OPENER**    | At the beginning of an H2 or H3 section where the target topic is the section's subject         | Section-specific deep links        |
| **CONCLUSION**        | In the final section or closing paragraph, near next-step recommendations                        | Funnel bridges; further reading    |
| **FAQ-SECTION**       | Within a FAQ answer where the target page answers the question more completely                   | Pillar links; deep-dive links      |
| **CTA-ADJACENT**      | Near a call-to-action block — linking to the page that supports the CTA's claim                 | Conversion links; trust links      |

**Phase 4B — Placement Selection Protocol**

For each link in the plan, select the placement using this logic:

| Link Type              | Preferred Placement         | Secondary Placement         |
|------------------------|-----------------------------|-----------------------------|
| CLUSTER-PILLAR         | INTRO or NEAR-MENTION       | WITHIN-PARAGRAPH            |
| CLUSTER-SIBLING        | WITHIN-PARAGRAPH            | NEAR-MENTION                |
| FUNNEL-BRIDGE (TOFU→MOFU) | CONCLUSION              | WITHIN-PARAGRAPH            |
| FUNNEL-BRIDGE (MOFU→BOFU) | CTA-ADJACENT or CONCLUSION | WITHIN-PARAGRAPH         |
| CROSS-CLUSTER          | WITHIN-PARAGRAPH            | NEAR-MENTION                |
| ORPHAN-RESCUE          | NEAR-MENTION                | WITHIN-PARAGRAPH            |
| PILLAR-BOOST           | WITHIN-PARAGRAPH            | FAQ-SECTION                 |
| CONVERSION             | CTA-ADJACENT                | CONCLUSION                  |

**Phase 4C — Placement Detail Specification**

For HIGH priority links, the placement detail must be specific enough that a content editor can implement without ambiguity:

**Too vague (prohibited):** `"Place in the body of the article"`

**Required format:** `"Place in the introduction after the sentence that defines [topic] — the link should follow the first mention of '[keyword]' in paragraph 2"`

**Another required format:** `"Place in the Section '[H2 title]' — add a link on the phrase '[partial match anchor]' in the paragraph discussing [specific aspect of the topic]"`

---

### ▸ STEP 5 — ORPHAN DETECTION

Identify pages that are structurally isolated in the content graph — receiving no or minimal incoming internal links.

**Phase 5A — Incoming Link Count Calculation**

For every published page in the inventory:

1. Count how many other published pages link TO this page (incoming internal links).
2. If `existing_link_structure` data is available → use real counts.
3. If not available → assume 0 existing internal links for all pages (worst-case detection mode) and flag: "Existing link structure unknown — orphan detection assumes no current internal links. Verify implementation before adding recommended links."

**Phase 5B — Orphan Classification**

| Incoming Link Count | Classification | Description                                                                                 |
|---------------------|----------------|---------------------------------------------------------------------------------------------|
| 0                   | **ORPHAN**     | Page is completely isolated — search engines and users cannot reach it through internal navigation |
| 1                   | **AT RISK**    | Page is connected to the site by only one path — a single link removal would orphan it     |
| 2–3                 | **LOW LINKED** | Page has minimal connections — below the threshold for meaningful authority accumulation   |
| 4+                  | **CONNECTED**  | Page is adequately connected                                                                 |

**Phase 5C — Orphan Severity Assessment**

Not all orphans carry the same urgency. Score each orphan by strategic importance:

| Orphan Priority Level | Condition                                                                     |
|-----------------------|-------------------------------------------------------------------------------|
| **CRITICAL ORPHAN**   | The page is a pillar page or a BOFU page with 0 incoming links               |
| **HIGH ORPHAN**       | The page is a high-coverage-score (≥4) content asset with 0 incoming links   |
| **MEDIUM ORPHAN**     | The page is a standard cluster sub-page with 0 incoming links                |
| **LOW ORPHAN**        | The page is a thin (coverage score 1–2) or peripheral page with 0 links      |

**Phase 5D — Orphan Report Construction**

For each orphan and at-risk page:

```
ORPHAN RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Page URL        : [URL]
Page Title      : [title]
Cluster         : [cluster name]
Funnel Stage    : [TOFU / MOFU / BOFU]
Coverage Score  : [N/5]
Orphan Priority : [CRITICAL / HIGH / MEDIUM / LOW]
Reason Orphaned : [why this page has no links — new page / cluster gap / was never linked]
Fix 1 (primary) : Link from [source URL] — anchor: "[anchor text]" — placement: [placement]
Fix 2 (backup)  : Link from [source URL] — anchor: "[anchor text]" — placement: [placement]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 6 — FIX ORPHANS

For every orphan and at-risk page, identify the best source pages to link FROM, the best anchor text, and the best placement.

**Phase 6A — Source Page Identification**

For each orphan page, identify which existing published pages are the best candidates to link from:

**Source Page Selection Criteria (in priority order):**

| Criterion                                                                    | Why It Matters                                                      |
|------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Same cluster as the orphan                                                  | Topical relevance is highest within the same cluster               |
| Source page already mentions the orphan page's topic (near-mention opportunity) | Natural linking context exists without content modification    |
| Source page has a high coverage score (≥3) — authoritative within the cluster | High-authority source pages pass more value                    |
| Source page and orphan are in adjacent funnel stages (TOFU → MOFU → BOFU)  | Funnel bridge opportunity — natural reader journey connection      |
| Source page was recently published (fresh content is more likely to be crawled) | Recency advantage for link discovery                           |

**Phase 6B — Source-Target Relationship Validation**

Before finalizing a source page for an orphan fix:

1. Is there a genuine topical connection between the source page and the orphan? → If YES → proceed.
2. Would a reader who finishes the source page logically want to read the orphan page? → If YES → HIGH confidence recommendation.
3. Does the source page already contain a mention of the orphan's topic that makes a link natural? → If YES → NEAR-MENTION placement.
4. Would the link feel forced or artificial in context? → If YES → mark as OPTIONAL; do not force the connection.

**Phase 6C — Orphan Fix Completion**

After all orphan fixes are identified, verify:
- Every CRITICAL and HIGH orphan has at least two fix options.
- Every MEDIUM orphan has at least one fix option.
- LOW orphans have at least one fix option OR are flagged as structurally difficult to link (minimal topical connections across the site — a signal that the page may not belong in the current content strategy).

---

### ▸ STEP 7 — PILLAR REINFORCEMENT

Identify the most strategically important pillar pages in the content inventory and ensure they receive sufficient incoming internal links to function as genuine authority hubs.

**Phase 7A — Top Pillar Page Identification**

Select the top 3–5 pillar pages using these criteria:

| Pillar Selection Signal                                          | Weight |
|------------------------------------------------------------------|--------|
| Formally designated as Pillar in `semantic-clustering.skill`    | HIGH   |
| Highest cluster strength score in the cluster architecture      | HIGH   |
| Highest coverage score in the content inventory (≥4)            | MEDIUM |
| Targets the broadest (most strategic) keyword in the cluster    | MEDIUM |
| Has the most sub-pages in its cluster (hub with most spokes)    | MEDIUM |

Rank all eligible pillar pages by combined signal weight. Select the top 3 (minimum) to 5 (maximum) for the Pillar Reinforcement Plan.

**Phase 7B — Current Link Count Assessment**

For each selected pillar page:
1. Count current incoming internal links (from `existing_link_structure` if available; otherwise assume 0).
2. Calculate the pillar's current link density: `current incoming links / total published pages × 100`.
3. Compare to the target density: `target = cluster sub-page count × 0.7` (70% of sub-pages should link to the pillar).
4. Calculate the link deficit: `target - current = links to add`.

**Phase 7C — Additional Link Source Identification**

For each pillar page with a link deficit:

1. List all pages in the same cluster that do NOT currently link to the pillar → these are the primary link sources.
2. List pages in adjacent clusters that share entities or cross-references with this pillar cluster → these are secondary link sources.
3. Prioritize the primary sources first; add secondary sources if the primary sources cannot close the deficit.

For each identified source:
- Verify the topical connection between the source and the pillar.
- Assign the anchor text type (following Phase 3B distribution rules for this pillar).
- Specify placement.

**Phase 7D — Pillar Boost Plan Output**

```
PILLAR BOOST PLAN: [Pillar Page URL]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cluster         : [cluster name]
Cluster Strength: [N/100]
Current Links   : [N]
Target Links    : [N]
Deficit         : [N] links to add

Priority Sources (add first):
  1. [source URL] — anchor: "[anchor]" — placement: [placement]
  2. [source URL] — anchor: "[anchor]" — placement: [placement]
  3. [source URL] — anchor: "[anchor]" — placement: [placement]

Secondary Sources (add if deficit remains):
  4. [source URL] — anchor: "[anchor]" — placement: [placement]
  5. [source URL] — anchor: "[anchor]" — placement: [placement]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 8 — CANNIBALIZATION DETECTION

Identify link patterns that would create anchor text cannibalization — where the same anchor text points to two different pages, sending mixed signals to search engines about which page should rank for a given keyword.

**Phase 8A — Anchor Text Collision Detection**

After all anchor texts are assigned across the complete link plan:

1. Build an anchor text index: for every anchor text string, list all target pages it is assigned to.
2. If any anchor text string is assigned to TWO DIFFERENT target pages → ANCHOR COLLISION detected.

| Collision Type                                  | Definition                                                                         |
|-------------------------------------------------|------------------------------------------------------------------------------------|
| **SAME ANCHOR SPLIT**                           | Identical anchor text pointing to two different pages (clearest cannibalization signal) |
| **NEAR-DUPLICATE ANCHOR SPLIT**                | Very similar anchor texts (≥80% word overlap) pointing to different pages         |
| **INTENT DUPLICATE**                            | Different anchor texts but both carry the same primary keyword intent, pointing to different pages |

**Phase 8B — Cannibalization Resolution Protocol**

For each detected cannibalization issue:

1. **Identify the primary page** — which of the two competing pages is the correct target for this keyword/intent?
   - The page with the higher coverage score is usually the primary.
   - The page with the pillar designation is always the primary.
   - The page that is more topically central to the keyword is the primary.

2. **Differentiate the anchor texts:**
   - Keep the anchor for the primary page.
   - Replace the anchor for the secondary page with a semantic or partial match anchor that does not share the primary keyword.

3. **Log the resolution:**

```
CANNIBALIZATION RESOLUTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Issue           : [collision type]
Conflicting anchor: "[original anchor text]"
Primary target  : [URL] — keep anchor "[anchor text]"
Secondary target: [URL] — change anchor to "[new differentiated anchor]"
Reason          : [why the primary page owns this keyword territory]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 8C — Same-Intent Link Pattern Detection**

Beyond anchor text, detect when two different pages receive links with the same topical intent from the same section of a third page:

Example: A single article links to two different "CRM comparison" pages in the same paragraph, creating intent confusion about which page is the authority on CRM comparisons.

Detection: For any source page, if two links in the same section point to pages with the same primary keyword cluster → INTENT DUPLICATE flag.

Resolution: Keep only the link to the higher-authority page (higher coverage score / pillar designation). Suggest the second link be moved to a different section or removed.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ AUTHORITY FLOW MODELING

Model how PageRank-style authority distributes across the link graph after all recommended links are implemented. This identifies authority bottlenecks — places where authority is flowing into a page but not distributing to the rest of the cluster.

**Authority Flow Simulation Protocol:**

1. **Initialize:** Assign each page a base authority value equal to its coverage score (1–5, normalized to 0–1).

2. **Apply link structure:** For each link in the full plan (existing + new), model authority flow:
   - Each page distributes a portion of its authority to every page it links to.
   - Authority passed per outbound link = `page_authority / outbound_link_count × 0.85` (the 0.85 is a standard dampening factor).
   - Authority received = sum of all inbound authority transfers.

3. **Run 5 propagation iterations** to allow authority to cascade through the link chain.

4. **Identify authority bottlenecks:** Pages that receive high authority but have low outbound link counts — they accumulate authority without distributing it. These pages should have their outbound links to cluster sub-pages increased.

5. **Identify authority sinks:** Leaf pages with many incoming links but no outbound cluster links. These should link back to their pillar to complete the authority cycle.

**Authority Flow Output:**

| Page          | Initial Authority | After Propagation | Authority Gain | Role          |
|---------------|-------------------|-------------------|----------------|---------------|
| [pillar URL]  | [N]               | [N]               | [+N]           | AUTHORITY HUB |
| [sub URL]     | [N]               | [N]               | [+N]           | RECEIVER      |
| [orphan URL]  | [N]               | [N unchanged]     | [0]            | ISOLATED      |

---

### ▸ CLUSTER STRENGTH WEIGHTING

Apply cluster strength scores from `semantic-clustering.skill` to weight the priority of link recommendations. Links within high-strength clusters deserve more urgent implementation than links within low-strength clusters.

**Cluster Weight Application:**

| Cluster Strength Score | Link Priority Multiplier             |
|------------------------|--------------------------------------|
| 80–100 (CRITICAL)      | All cluster links elevated to HIGH   |
| 60–79 (HIGH)           | Standard priority assignments apply  |
| 40–59 (MEDIUM)         | MEDIUM links may be downgraded to LOW|
| < 40 (LOW)             | LOW links downgraded to OPTIONAL     |

**Application Rule:** The cluster strength multiplier is applied AFTER the standard priority classification in Steps 1–7. It can only elevate priority — not reduce it below the standard determination. A HIGH priority link from a core structural rule (orphan rescue, BOFU-to-conversion) is never downgraded even in a low-strength cluster.

---

### ▸ PAGERANK-STYLE LINK VALUE DISTRIBUTION

Model the relative value of each link based on the source page's authority and the depth of the link in the page structure.

**Link Value Factors:**

| Factor                                             | Value Impact                                   |
|----------------------------------------------------|------------------------------------------------|
| Source page authority score                        | Higher authority source = higher link value    |
| Source page coverage score                         | Higher coverage = more authoritative source    |
| Link depth in source page (intro vs. conclusion)  | Intro links pass more value than footer links  |
| Number of other outbound links on source page     | More outbound links = diluted value per link   |
| Whether source page is in same cluster as target  | Same-cluster links pass more topical relevance |

**Link Value Score:**

`Link_Value = (Source_Authority × Placement_Weight) / Outbound_Link_Count × Cluster_Alignment`

Where:
- `Source_Authority` = coverage score × cluster strength weight
- `Placement_Weight` = INTRO (1.0) / WITHIN-PARAGRAPH (0.85) / CONCLUSION (0.7) / FAQ-SECTION (0.8)
- `Outbound_Link_Count` = total outbound links on source page (estimated from existing + new plan)
- `Cluster_Alignment` = 1.2 if same cluster / 1.0 if adjacent cluster / 0.8 if different cluster

High Link Value Score → higher priority for implementation.

This model is applied to break ties when two links have the same strategic priority — the higher link value link is implemented first.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Duplicate Page Pairs

Each ordered pair (Source Page A → Target Page B) may appear exactly once in the link plan. If the same source-target pair would be generated by two different planning steps (e.g., both cluster-based linking and orphan rescue would create the same link) → keep one instance and note that it serves dual purposes.

### Rule 2 — No Repeated Anchor Text Per Target

For any given target page, no two source pages in the plan may use the identical anchor text string. Every anchor text used for the same target must be distinct. Apply the anchor text variation rules from Step 3 Phase 3E.

### Rule 3 — No Self-Links

No page may link to itself. If the cluster architecture or orphan resolution logic would generate a link where source URL = target URL → reject it automatically and log it as an INVALID LINK.

### Rule 4 — No Duplicate Link Recommendations with Existing Links

If `existing_link_structure` data is provided, no link that already exists should appear as a new recommendation. Before finalizing the plan, cross-reference all recommended links against the existing link structure and remove any duplicates. Log removed duplicates: "Link [source] → [target] already exists — not re-recommended."

### Rule 5 — No Reciprocal Link Overload

If Page A links to Page B AND Page B links to Page A, both links may be valid. However, if THREE pages mutually link to each other in a triangle (A→B, B→C, C→A) with no content justification beyond authority exchange → flag as LINK RING PATTERN. This does not block the links but is noted in the cannibalization report as a pattern to monitor.

### Rule 6 — Memory Deduplication

If a prior linking plan exists in `memory-controller.skill` → use it to prevent re-recommending links that were already implemented. Only recommend new links that were not in the prior plan. In delta mode, the output contains ONLY new recommendations since the last plan run.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Internal Linking Behaviors

| Prohibited Behavior                                                                             | Reason                                                                           |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Recommending a link without specifying the exact anchor text                                    | Anchor text is a strategic decision — not an implementation detail              |
| Using "click here," "read more," or "learn more" as anchor text                                | These generic anchors provide no topical signal to search engines               |
| Recommending a link without specifying the exact placement in the source page                  | Placement determines link value — "put it somewhere" is not actionable          |
| Linking every page to every other page in the same cluster (all-to-all)                       | Over-linking dilutes authority and creates an unnatural link profile             |
| Recommending links between pages that have no genuine topical connection                        | Irrelevant links are detected by search engines and provide no authority benefit |
| Generating more than 10 outbound internal links for a single source page                       | More than 10 internal links on a single page dilutes the value of each link     |
| Flagging a link as HIGH priority without explaining why it matters for authority or reader flow | Every HIGH priority designation needs a specific strategic rationale            |
| Leaving any CRITICAL or HIGH orphan without a fix recommendation                               | These are the most damaging orphans — they must always have a fix path           |
| Producing a link plan that does not resolve any orphan pages                                    | Orphan resolution is a mandatory output of this skill                          |
| Using the same anchor text format for all links from the same source page                      | Monotonous anchor patterns are a quality signal problem and a cannibalization risk |

### Required Internal Linking Behaviors

- Every link recommendation must have: source URL, target URL, anchor text, anchor type, placement type, placement detail, and priority.
- Every orphan page must have at least one fix recommendation.
- Every top-3 pillar page must appear in the Pillar Boost Plan.
- Every detected cannibalization issue must have a specific resolution.
- The authority flow summary must show the pre- and post-implementation state.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-inventory.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (primary page data)                                                    |
| **Receives**     | All content records — URL, keyword, cluster, intent (TOFU/MOFU/BOFU), status, coverage score |
| **Used for**     | Steps 1–8 — all page-level data including funnel mapping, orphan detection, pillar identification |

---

### ▸ semantic-clustering.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream data source (cluster architecture)                                                 |
| **Receives**     | Cluster names, tier designations, keyword lists, strength scores, pillar designations       |
| **Used for**     | Step 1 (cluster-based linking), Step 7 (pillar identification), Advanced Logic (cluster weighting) |

---

### ▸ authority-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment + downstream consumer                                          |
| **Receives from**| Authority scores per cluster, pillar page URLs, missing element reports                    |
| **Sends to**     | Complete link plan including pillar reinforcement data                                     |
| **Used for**     | Step 7 (pillar boost plan), authority flow modeling                                        |

---

### ▸ query-graph.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream optional enrichment                                                                |
| **Receives**     | Node types (Hub/Connector/Leaf), Hub Scores, dependency chains, cross-cluster relationships |
| **Used for**     | Advanced Logic — authority flow modeling, cluster strength weighting                       |

---

### ▸ roadmap-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | internal-linking → roadmap-engine (data consumer)                                          |
| **Sends**        | Funnel gap register (missing MOFU/BOFU pages), pillar reinforcement needs                 |
| **Used for**     | Roadmap engine uses funnel gaps to prioritize new content creation                         |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Bidirectional — READ for delta mode, WRITE after plan finalization                          |
| **Reads**        | Prior linking plan, existing link structure, previously resolved orphans                   |
| **Writes**       | Complete link plan (all 5 parts), anchor text assignments, orphan status updates           |
| **Memory layers**| content-memory.skill (link plan), strategy-memory.skill (authority flow data)             |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Content inventory has fewer than 3 pages                                  | Step 1 — page count < 3                               | Flag: "Internal linking plan requires minimum 3 pages. With [N] pages, the plan will be minimal. Expand content inventory before running a comprehensive linking plan." Proceed with available pages. |
| No cluster architecture available                                         | Input pre-condition — semantic-clustering output null | Apply keyword-proximity clustering: group pages by keyword overlap. Flag: "Cluster architecture absent — links based on keyword proximity only. Run semantic-clustering.skill for a more structured plan." |
| No pillar page identifiable in any cluster                                | Phase 1C — no page meets 2+ pillar signals           | Flag all clusters as PILLAR ABSENT. Recommend pillar page creation for each cluster. Produce linking plan treating highest-coverage-score page as provisional pillar. |
| All published pages are orphans (no existing link structure)              | Step 5 — all pages have 0 incoming links             | Flag: "Site appears to have no internal linking structure. This is a full build — all recommendations are new. Implement HIGH priority links first." Proceed with full plan. |
| Cannibalization detection finds >10 anchor conflicts                     | Step 8 — conflict count > 10                         | Flag: "HIGH CANNIBALIZATION RISK — anchor text architecture needs comprehensive review." Surface all conflicts. Prioritize resolutions by the authority value of the affected pages. |
| A funnel stage is completely absent from the content inventory            | Step 2 Phase 2D — TOFU, MOFU, or BOFU has 0 pages  | Flag the missing funnel stage. Log to roadmap-engine.skill as a content creation priority. Note: "Funnel bridges for [stage] cannot be created until [stage] content is published." |
| Memory controller unavailable for delta mode check                        | READ query returns error                              | Proceed in full rebuild mode. Flag: "Prior link plan unavailable — full link plan generated. Verify against existing site links before implementing to avoid duplicates." |
| Source page identified for orphan fix is itself an orphan                 | Step 6 — source page has 0 incoming links             | Find an alternative source page with at least 1 incoming link. Note: "Original source page [URL] is also an orphan — using alternative source [URL] to avoid an isolated link chain." |
| A cluster has 30+ pages with no outbound limit in place                   | Step 1 — cluster page count exceeds threshold        | Apply outbound link cap: no single source page may receive more than 8 new link recommendations in the plan. Distribute across top priority links only. Flag remaining lower-priority links as BACKLOG. |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — ANCHOR TEXT STRATEGY QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
ANCHOR TEXT STRATEGY — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TARGET DISTRIBUTION:
  EXACT MATCH  (20%): Anchor = target page's exact primary keyword
  PARTIAL MATCH(60%): Anchor includes keyword within a descriptive phrase
  SEMANTIC     (20%): Anchor describes the topic without using the keyword

EXACT MATCH EXAMPLES (target: "crm software for startups"):
  ✓ "crm software for startups"

PARTIAL MATCH EXAMPLES:
  ✓ "the best crm software for startups"
  ✓ "choosing crm software for your startup"
  ✓ "CRM software built for startup sales teams"

SEMANTIC EXAMPLES:
  ✓ "contact management tools for early-stage companies"
  ✓ "startup pipeline tools"
  ✓ "sales tracking for growing teams"

PROHIBITED ANCHOR PATTERNS:
  ✗ "click here"
  ✗ "read more"
  ✗ "learn more"
  ✗ "here"
  ✗ "this article"
  ✗ "this page"
  ✗ Bare URL
  ✗ Brand name only (for content pages)
  ✗ Keyword-stuffed multi-keyword strings

UNIQUENESS RULE:
  No two source pages may use the identical anchor text
  for the same target page.

CANNIBALIZATION RULE:
  No anchor text string should point to two different pages.
  If conflict detected → differentiate one anchor to semantic.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — LINK TYPE AND PRIORITY QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
LINK TYPES AND DEFAULT PRIORITIES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CLUSTER-PILLAR   (Sub → Pillar)
  Default priority: HIGH
  Purpose: Concentrates topical authority in the pillar page

CLUSTER-SIBLING  (Sub → related Sub, same cluster)
  Default priority: MEDIUM
  Purpose: Signals topical depth and cluster completeness

FUNNEL-BRIDGE    (TOFU → MOFU / MOFU → BOFU)
  Default priority: HIGH
  Purpose: Guides reader journey; compounds session depth

CROSS-CLUSTER    (Cluster A → adjacent Cluster B)
  Default priority: MEDIUM
  Purpose: Signals semantic relationship between topic areas

ORPHAN-RESCUE    (any page → orphaned page)
  Default priority: HIGH (if orphan is CRITICAL/HIGH)
               MEDIUM (if orphan is MEDIUM)
               LOW (if orphan is LOW)
  Purpose: Connects isolated pages to the link graph

PILLAR-BOOST     (distant page → pillar page)
  Default priority: MEDIUM
  Purpose: Increases pillar page's incoming link count

CONVERSION       (BOFU/MOFU → conversion/pricing page)
  Default priority: HIGH
  Purpose: Closes the revenue loop — never leave unlinked

CLUSTER STRENGTH OVERRIDE:
  High-strength cluster (80–100) → all links elevated to HIGH
  Low-strength cluster (< 40) → LOW links downgraded to OPTIONAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — ORPHAN CLASSIFICATION AND FIX QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
ORPHAN CLASSIFICATION AND FIX GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CRITICAL ORPHAN (pillar page or BOFU page, 0 links):
  Fix urgency: IMMEDIATE
  Required fixes: Minimum 3 incoming links
  Sources: All sub-pages in cluster + top cross-cluster pages

HIGH ORPHAN (high-coverage page, 0 links):
  Fix urgency: HIGH
  Required fixes: Minimum 2 incoming links
  Sources: Pages in same cluster discussing related sub-topics

MEDIUM ORPHAN (standard sub-page, 0 links):
  Fix urgency: MEDIUM
  Required fixes: Minimum 1 incoming link
  Sources: Pillar page outbound + nearest sibling page

LOW ORPHAN (thin or peripheral page, 0 links):
  Fix urgency: LOW
  Required fixes: 1 incoming link OR review if page belongs in strategy
  Note: If no natural link source exists → page may be out of scope

AT-RISK PAGE (1 incoming link):
  Fix urgency: LOW-MEDIUM
  Required fixes: 1 additional incoming link from different source
  Purpose: Prevents orphaning if current link is removed

ORPHAN FIX VALIDATION:
  Source page must have genuine topical connection to orphan
  Would a reader naturally want to read the orphan after the source?
  If NO → mark link as OPTIONAL, not a fix requirement
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: content/internal-linking.skill*
*Nexus SEO Operating System — Version 1.0.0*

# PATCH: internal-linking.skill — v1.1.0 UPDATE
# APPLY TO: references/internal-linking.skill.md
# INSTRUCTIONS: Append this entire section to the end of the existing internal-linking.skill.md file.
# These additions enforce the new link density rules, front-loading requirements, and nofollow policies.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## ADDENDUM — LINK DENSITY RULES (v1.1.0)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Minimum Link Counts (Scaled by Content Length)

| Content Length | Minimum Internal Links | Minimum Unique Destinations | Minimum Unique Anchors |
|---|---|---|---|
| Under 1,000 words | 8-10 | 8 | 8 |
| 1,000-1,500 words | 10-12 | 10 | 10 |
| 1,500-2,000 words | 12-15 | 12 | 12 |
| 2,000-2,500 words | 15-20 | 15 | 15 |
| 2,500-3,500 words | 20-25 | 20 | 20 |
| 3,500+ words | 25+ | 25+ | 25+ |

**Density target:** 1 internal link per 100-150 words (never denser than 1 per 80 words — that's spammy).

### Front-Loading Requirement

The intro and first 300 words of every article MUST contain 3-4 internal links:

| Position | Link Type | Purpose |
|---|---|---|
| First paragraph (intro) | 1-2 contextual links | Immediately connect to pillar content and related articles |
| Second/third paragraph | 1-2 contextual links | Link to background concepts the reader may need |

**Why front-load:** Google crawls pages top-to-bottom. Links discovered early in the document get more crawl equity. Readers who click early links spend more time on site.

### Link Placement Distribution (After Front-Loading)

For the remaining links beyond the intro 3-4:
| Section | Links | Type |
|---|---|---|
| Per H2 section | 2-3 links | Contextual within paragraph text |
| Conclusion | 1-2 links | "Next steps" or "related reading" |
| Code examples | 1-2 links | Link to related tutorial when showing code patterns |
| Callout boxes | 0-1 per box | Only if naturally relevant |

### Anchor Text Strategy

| Anchor Type | Percentage | Example |
|---|---|---|
| **Partial match keyword** | 50-60% | "Playwright debugging techniques" linking to /blog/playwright-debugging-guide/ |
| **Descriptive/natural** | 25-30% | "this comprehensive comparison" linking to /blog/playwright-vs-cypress/ |
| **Exact match keyword** | 5-10% | "playwright trace viewer" linking to /blog/playwright-trace-viewer/ |
| **Branded** | 5-10% | "[Brand]'s [feature name]" linking to relevant docs or feature page |
| **Generic ("learn more")** | 0% | NEVER use generic anchors |

**Rules:**
- Never repeat the same exact anchor text for the same destination URL
- Never use "click here," "learn more," "read more" as anchor text
- Every anchor must be 2-7 words (not single words, not full sentences)
- Anchor text must accurately describe the destination page's content

### Link Destination Priority

When deciding which pages to link to:

| Priority | Destination Type | When to Link |
|---|---|---|
| 1 | **Pillar/cluster hub pages** | Always — these are your highest-authority targets |
| 2 | **High-traffic blog posts** | When contextually relevant — distributes link equity |
| 3 | **New/recent blog posts** | When relevant — aids crawl discovery |
| 4 | **Docs pages** | When discussing a specific brand feature |
| 5 | **Product pages** (pricing, compare, alternatives) | Only in MOFU/BOFU content |
| 6 | **Case studies** | When discussing real-world results |

### External Link Rules

| Rule | Requirement |
|---|---|
| All external links | `rel="nofollow"` — no exceptions |
| External link targets | Only authoritative sources (official docs, engineering blogs, conferences) |
| External link count | 3-5 per article (provides authority signals without leaking equity) |
| Competitor links | Link to competitor DOCS only (not their marketing pages) — nofollow |

### UTM Parameter Rules

| Link Type | UTM Treatment |
|---|---|
| Contextual internal links (in paragraph text) | Clean URLs — NO UTM parameters |
| CTA button links | Add UTM: `?utm_source=blog&utm_medium=cta&utm_campaign={slug}` |
| CTA destination | ALWAYS `brand.primary_cta_url` with UTM (from active brand file) |
| Callout box feature links | Clean URLs to docs pages |

### URL Source
All internal URLs MUST come from `brand.urls` in the active brand file (`brand-[name].skill.md`). Never hardcode or guess a URL. If no matching URL exists in `brand.urls` for a topic, flag it as `[LINK_NEEDED: {topic}]` for manual resolution. ZERO cross-brand links — never link to any other registered brand's URLs.

---

*Internal Linking Patch — v1.1.0 | Nexus SEO Operating System*
