# pillar-page-mode.skill

**Role:** Override default content generation rules for pillar pages — comprehensive authority pieces that anchor a topic cluster. Mandatory comprehensive coverage, table of contents with jump links, internal cluster links to sub-topic pages, ≥7 unique angles required.

**Loaded by:** `nexus-content` SKILL.md when user invokes pillar_page mode

---

## Why this skill exists

Pillar pages serve a different purpose than standard blog posts:
- They anchor a topic cluster (link out to many sub-topic pages)
- They establish topical authority for the brand
- They aim for breadth + depth (cover the full topic universe at substantial depth)
- They typically rank for high-volume head terms
- They have higher differentiation requirements (everyone has a pillar page on common topics)

This skill enforces pillar-specific structure and authority requirements.

---

## Inputs

Required:
- Topic (the cluster's central theme)
- Brand profile (especially brand.urls — pillar pages link extensively to existing cluster content)

Required for full effect:
- Sub-topic pages from the cluster (URLs in brand.urls or provided list)
- If sub-topic pages don't exist yet: pillar page can be written with placeholder internal links + recommendation to publish supporting cluster content

Optional:
- User research (high-leverage for pillar — often the differentiation)
- Author profile (pillar pages benefit from clear author authority)

---

## The mandatory pillar structure

### Required sections (in order)

1. **Hero / opener** with clear value proposition
   - What this guide covers
   - Who it's for
   - Why it matters now

2. **Table of contents** (mandatory — pillar pages are scannable references)
   - Jump links to every H2
   - Visual hierarchy showing H2 + H3 structure

3. **Definition / foundation section** (one or more H2s)
   - Defines the central concept of the cluster
   - Establishes the conceptual framework readers need

4. **Comprehensive topic coverage** (multiple H2s, each significantly developed)
   - Every major sub-topic of the cluster gets an H2
   - Each H2 has substantial development (not surface treatment)
   - Each H2 includes internal link to dedicated sub-topic page (if it exists or planned)

5. **Practical application section** (one or more H2s)
   - How to apply the concepts
   - Common patterns or workflows
   - Case examples

6. **Comparison / decision frameworks** (where applicable)
   - When to use which approach
   - Trade-offs

7. **Common mistakes / anti-patterns** (one H2)
   - What goes wrong and why
   - How to avoid

8. **Tools and resources** (one H2)
   - Curated list with brief descriptions
   - Internal links to deeper coverage of each tool

9. **FAQ** (mandatory — pillar pages aggregate many questions)
   - Top 8-12 PAA questions
   - With FAQPage schema

10. **Next steps / further reading**
    - Links to sub-topic pages in the cluster
    - Recommended order for deeper exploration

### Optional sections

- Glossary (for highly technical pillar pages)
- History / evolution of the topic
- Future trends / what's next
- Author note / methodology

---

## Process

### Step 1 — Cluster mapping

If brand.urls includes sub-topic pages:
- Map them to the pillar's sections (which sub-topic page does each H2 link to?)
- Verify links are still valid (delegated to live-link-verifier)
- If sub-topics are missing, recommend publishing supporting cluster content

If sub-topic pages don't exist:
- Pillar can still be written but with placeholder internal links
- Output recommendation: "Pillar page is published, but topical authority will solidify only when these sub-topic pages exist: [list]"

### Step 2 — Comprehensive coverage check

Cross-reference the pillar's H2 structure with:
- Top 10 SERP results' coverage (from deep-serp-analysis if available)
- PAA questions for the topic
- Related searches
- Competitor coverage gaps (from gap-opportunity-engine)

Pillar should cover topics broader than any single competitor.

### Step 3 — Depth verification

For each H2:
- Substance gate must pass (not surface treatment)
- Each H2 should be the equivalent of a "mini-article" worth of substance
- Total pillar word count is typically 3,000-8,000+ words (substance-driven, not target)

### Step 4 — Differentiation strict mode

Pillar pages compete with established authority pages. Apply STRICTEST differentiation:
- Need ≥7 unique angles vs top 10
- At least 3 unique angles must be original data, contrarian positions, or unique frameworks
- Surface differentiation prominently in opener and TOC

### Step 5 — Internal linking density

Pillar pages should link more than standard blogs:
- Standard blog: 5-10 internal links typically
- Pillar page: 15-30+ internal links (each cluster sub-topic gets at least one link)
- All links from brand.urls (verified valid by live-link-verifier)

### Step 6 — Schema enrichment

Beyond standard Article schema:
- BreadcrumbList (mandatory)
- FAQPage (FAQ section is mandatory)
- TableOfContents schema (where supported)
- Article schema with hasPart array referencing sub-topic pages (advanced — establishes the pillar relationship in schema)

### Step 7 — TOC generation

Generate jump-link TOC:
- Markdown anchor links for every H2 + H3
- Visually formatted as nested list
- Placed after the opener, before the first content section

### Step 8 — Decay mapping for pillars

Pillar pages have unique decay patterns:
- Evergreen sections rarely need updating
- Time-bound sections need scheduled review
- Cluster relationships need periodic verification (sub-topic links still valid?)

Decay-mapping outputs longer review cadences for pillar evergreen content but flags cluster-link-verification on a 90-day cycle.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: pillar-page-mode
Status: COMPLETE
Inputs consumed: topic, brand profile, [optional sub-topic pages, user research, author]

PILLAR STRUCTURE GENERATED:

  ✓ Hero / opener with value proposition
  ✓ Table of contents with jump links: [N H2s + N H3s]
  ✓ Definition / foundation: [N sections]
  ✓ Comprehensive topic coverage: [N H2 sections]
  ✓ Practical application: [N sections]
  ✓ Comparison / decision frameworks
  ✓ Common mistakes / anti-patterns
  ✓ Tools and resources
  ✓ FAQ: [N questions]
  ✓ Next steps / further reading

CLUSTER MAPPING:
  Sub-topic pages identified in brand.urls: [N]
  Internal links to sub-topic pages: [N — each cluster topic gets at least one link]
  Sub-topic pages missing (recommended to publish): [list with topic suggestions]

DIFFERENTIATION (strictest for pillar):
  Required: ≥7 unique angles + ≥3 original data / contrarian / unique frameworks
  Achieved: [N unique angles, N original data, N contrarian positions]
  Differentiation enforcer: PASS / FAIL

INTERNAL LINKING DENSITY:
  Total internal links: [N — target 15-30+]
  All links validated: [PASS / N broken / replaced]
  Cluster coverage: [N of M sub-topics linked]

DEPTH VERIFICATION:
  Each H2 passed substance gate: [PASS / FAIL list]
  Total word count: [N — substance-driven, typically 3000-8000+]
  Coverage breadth vs top 10: [HIGHER / EQUIVALENT / LOWER]

SCHEMA GENERATED:
  ✓ Article (with hasPart array referencing cluster pages)
  ✓ BreadcrumbList
  ✓ FAQPage (FAQ section)
  ✓ TableOfContents (where supported)

QUALITY CHECKS:
  ✓ Substance gates per H2 (extra rigorous)
  ✓ Live fact verification
  ✓ Live link verification (especially internal cluster links)
  ✓ E-E-A-T (author authority matters more for pillar)
  ✓ Helpful Content (especially "substantial value" check)
  ✓ Differentiation enforcer (strictest mode)

DECAY MAPPING:
  Evergreen review cadence: [365+ days for stable concepts]
  Time-bound review cadence: [90-180 days for tools/trends]
  Cluster link verification: every 90 days

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **TOC mandatory.** Pillar pages are scannable references — readers need jump links.

2. **Comprehensive coverage non-negotiable.** Pillar covers the entire topic universe at depth. Surface treatment fails.

3. **Cluster linking is the strategic value.** Pillar pages anchor topic clusters by linking to sub-topic content. Without sub-topic pages, the pillar's authority is incomplete.

4. **Differentiation is hardest for pillar.** Many established pillar pages exist for popular topics. Need genuine unique value to displace.

5. **Schema enrichment matters.** Pillar pages benefit from rich schema — Article + BreadcrumbList + FAQPage + hasPart relationships establish the page as a hub.

6. **Decay strategy is different.** Pillar evergreen content stays valid for years; pillar tools/comparison sections need quarterly review.

7. **Length emerges from substance.** Don't pad pillars to "feel comprehensive." Each section needs real substance. 3,000 words of substance > 8,000 words of fluff.
