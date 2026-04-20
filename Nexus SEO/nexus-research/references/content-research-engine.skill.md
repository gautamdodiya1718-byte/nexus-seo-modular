# SKILL: research/content-research-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / Pre-Writing Intelligence
# EXECUTION PRIORITY: FIRST IN CONTENT PIPELINE — Runs before content-generation-engine. Produces the content brief that all downstream skills consume.
# POSITION: Deep-dive content research engine that analyzes SERP, competitors, media usage, real-world examples, and differentiation opportunities. Produces a comprehensive brief that the content writer, engagement, and optimization skills consume.
# DEPENDENCIES: brand-[name].skill.md (fields: primary_audience, content_pillars, features, urls, extended_audience_profile), serp-intelligence.skill, deep-serp-analysis.skill, google-signals-extractor.skill

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **pre-writing research engine** of the Nexus system. It produces the most comprehensive content brief possible — going far beyond keyword-level analysis into page-level content analysis, media auditing, real-world example sourcing, and differentiation opportunity identification.

### Two Operating Modes

| Mode | Trigger | Description |
|---|---|---|
| **EXISTING-SERP** | Target keyword has 5+ organic results | Analyze top 10 results, find gaps, plan how to beat them all |
| **FRONTIER** | Target keyword has thin/no SERP results | Validate demand via related searches, PAA, forums, GitHub issues; define content from scratch |

### Output
A structured **Content Research Brief** that the user reviews before content generation begins. On approval, it auto-feeds into the content pipeline.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## EXTENDED AUDIENCE PROFILE CHECK

Before reading brand.primary_audience, check whether brand.extended_audience_profile
is set in the active brand file.

If brand.extended_audience_profile is set:
  Load the named audience file from references/.
  Read: primary_persona (P1), content_strategy_by_funnel, primary_content_voice.
  These override the basic brand.primary_audience fields for content research depth.

If brand.extended_audience_profile is blank or not set:
  Read from brand.primary_audience directly as normal.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Required Inputs
| Input | Source | Description |
|---|---|---|
| Target keyword or topic | User | The primary keyword or blog topic to research |
| Content funnel stage | User or inferred | TOFU / MOFU / BOFU |

### Optional Inputs
| Input | Source | Description |
|---|---|---|
| Specific competitor URLs | User | Focus analysis on these competitors |
| Content angle preference | User | E.g., "data-driven report," "practical tutorial," "comparison" |
| Target word count | User | Override default length recommendations |

### Upstream Skill Outputs Consumed
| Skill | Data Consumed |
|---|---|
| `serp-intelligence.skill` | SERP snapshot, intent classification, difficulty score |
| `deep-serp-analysis.skill` | Per-page 8-dimension competitive analysis |
| `google-signals-extractor.skill` | PAA questions, autocomplete, related searches |
| `official-links-repository.skill` | Internal URLs for link planning |
| `brand-[name].skill.md (field: features)` | Feature registry — check maturity labels for relevance assessment |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — EXECUTION STEPS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Step 1: SERP Snapshot & Mode Detection
1. Run `serp-intelligence.skill` for the target keyword
2. Count organic results with real content (exclude forums, Q&A sites, tool pages)
3. If 5+ editorial results → **EXISTING-SERP mode**
4. If fewer than 5 → **FRONTIER mode**
5. Record: search intent (informational/commercial/transactional/navigational), SERP features present (featured snippet, PAA, video carousel, image pack), difficulty score

### Step 2: Competitor Page Analysis (EXISTING-SERP mode)
For each of the top 10 organic results, fetch and analyze:

#### 2a. Content Metrics
| Metric | How to Measure |
|---|---|
| Word count | Total body text |
| Heading count | Number of H2s and H3s |
| Content depth score | (Unique concepts covered) / (Total possible concepts for this topic) |
| Reading level | Flesch-Kincaid grade |
| Freshness | Last updated date if visible |

#### 2b. Media Audit
| Media Type | Count | Details to Record |
|---|---|---|
| Images | How many, what types | Screenshots, custom diagrams, stock photos, infographics, charts |
| Videos | Embedded count, source | YouTube, Vimeo, self-hosted; topics covered |
| Code blocks | Count, language | TypeScript, JavaScript, Python; with or without output |
| Tables | Count, type | Comparison tables, data tables, feature matrices |
| Interactive elements | Count, type | Calculators, code playgrounds, embedded demos |
| Infographics | Count, style | Flowcharts, timelines, process diagrams, checklists |
| GIFs | Count, type | Screen recordings, animated diagrams |

#### 2c. Structural Analysis
| Element | What to Record |
|---|---|
| H2 structure | Full list of all H2 headings (exact text) |
| H3 sub-sections | Under which H2s |
| Content flow | Introduction style, conclusion style, CTA placement |
| FAQ section | Present? How many questions? Schema markup? |
| Schema markup | Types detected (Article, FAQ, HowTo, etc.) |
| Internal links | Count and destination types |
| External links | Count and destinations |

#### 2d. Unique Angles
For each competitor, identify:
- What unique angle or claim do they make that others don't?
- What original data, research, or example do they include?
- What expert quotes or authority signals do they use?
- What is their strongest section? (the one section that's clearly better than everyone else's)

### Step 3: Demand Validation (FRONTIER mode)
When SERP results are thin or nonexistent:
1. Run `google-signals-extractor.skill` for related autocomplete suggestions
2. Search for the topic on:
   - GitHub Issues (are developers discussing this problem?)
   - Stack Overflow (are people asking questions about it?)
   - Reddit (r/QualityAssurance, r/node, r/webdev — is there discussion?)
   - Dev.to / Hashnode (has anyone written about it?)
   - Twitter/X (are practitioners mentioning it?)
3. Check npm download trends if the topic relates to a package
4. Check Google Trends for search interest trajectory
5. Compile demand evidence: "N GitHub issues, M Stack Overflow questions, P Reddit threads in last 12 months"

### Step 4: Content Gap Identification
Compare all competitor pages against each other and identify:

| Gap Type | Description | Example |
|---|---|---|
| **Topic gap** | A subtopic NO competitor covers | "No one explains how to debug MCP connection failures" |
| **Depth gap** | A subtopic all competitors cover superficially | "Everyone mentions sharding but no one shows CI config" |
| **Media gap** | A content type no competitor uses | "No one has a video walkthrough" |
| **Data gap** | No competitor includes quantitative evidence | "No one cites benchmark data for parallel vs sequential" |
| **Freshness gap** | All competitors have outdated information | "All articles reference Playwright v1.40, current is v1.56" |
| **Example gap** | No competitor includes practical code examples | "All articles are theoretical, no runnable code" |
| **Authority gap** | No competitor cites real-world company usage | "No case studies or enterprise examples" |

### Step 5: Real-World Example Search
Search for real-world evidence that supports the article's claims:

1. **Company engineering blogs:** Search "[topic] site:engineering.{company}.com" for major tech companies
2. **Conference talks:** Search "[topic] conference talk 2025 2026" for recent presentations
3. **GitHub repos:** Search GitHub for popular repos demonstrating the concept
4. **Published benchmarks:** Search for quantitative data (performance numbers, adoption stats, cost savings)
5. **Case studies:** Search for "[topic] case study" on corporate blogs

For each example found, record:
| Field | Value |
|---|---|
| Source | Company name or author |
| Type | Blog, talk, repo, benchmark, case study |
| Key claim | What specific data point or insight does it provide? |
| Credibility | Primary (company's own blog) / Secondary (third-party) / Anecdotal (forum) |
| Recency | Date published |
| URL | Source URL |

**Recency priority:** Prefer last 12 months → last 24 months → last 36 months. No hard cutoff, but flag anything older than 36 months with a freshness warning.

**When no examples exist:** Create a realistic synthetic example AND suggest building a live demo. Record both as action items in the brief.

### Step 6: Differentiation Opportunity Assessment
Score each differentiation idea on Effort (1-5) vs Impact (1-5):

| Differentiation Type | Description | Typical Effort | Typical Impact |
|---|---|---|---|
| **Original research/data** | Collect and publish proprietary data | 4-5 | 5 |
| **Live demo or app** | Build a working demo visitors can interact with | 4-5 | 5 |
| **Public GitHub repo** | Code repository readers can clone and run | 3 | 4 |
| **Video walkthrough** | Screen recording or explainer video | 3 | 4 |
| **Downloadable template/cheat sheet** | PDF, config file, or template | 2 | 3 |
| **Interactive tool in the post** | Calculator, code playground, configurator | 4 | 5 |
| **Expert quotes or interviews** | Direct quotes from practitioners | 3 | 4 |
| **Original benchmark/performance test** | Run tests and publish the numbers | 4 | 5 |
| **Comprehensive comparison table** | Side-by-side feature matrix no one else has | 2 | 3 |
| **Step-by-step code + screenshots** | Every step shown with code and output preview | 3 | 4 |

Recommend the **top 3 differentiation opportunities** for this specific article, sorted by Impact/Effort ratio.

### Step 7: Metadata & Technical SEO Pre-Research
| Element | What to Research |
|---|---|
| Primary keyword | Exact match search volume, difficulty, intent |
| Secondary keywords | 5-10 related terms with volume |
| Title patterns | What title formats rank (How-to, Guide, List, Question) |
| Meta description patterns | Common patterns in top 3 results |
| URL slug recommendation | Based on keyword + competitor slugs |
| Schema opportunity | What schema types top results use + what they're missing |
| Featured snippet format | If featured snippet exists, what format (paragraph, list, table) |

### Step 8: Content Brief Assembly
Compile all research into a structured brief:

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — OUTPUT FORMAT: CONTENT RESEARCH BRIEF
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
═══════════════════════════════════════
CONTENT RESEARCH BRIEF
Keyword: [primary keyword]
Mode: [EXISTING-SERP / FRONTIER]
Date: [research date]
═══════════════════════════════════════

## 1. KEYWORD INTELLIGENCE
- Primary keyword: [keyword] | Volume: [vol] | Difficulty: [KD] | Intent: [intent]
- Secondary keywords: [list with volumes]
- Google signals: [PAA questions, autocomplete, related searches]
- Featured snippet opportunity: [Yes/No, format]

## 2. SERP LANDSCAPE
[For each of top 10 results:]
- #[rank]: [Title] | [URL]
  - Word count: [N] | H2s: [N] | Images: [N] | Videos: [N] | Code blocks: [N]
  - Unique angle: [what makes this result different]
  - Strongest section: [their best section]
  - Weakest section: [their biggest gap]

## 3. COMPETITOR MEDIA AUDIT
[Aggregate across all competitors:]
- Average images per article: [N] (Types: [breakdown])
- Articles with video: [N/10]
- Articles with code examples: [N/10]
- Articles with tables: [N/10]
- Articles with infographics: [N/10]
- Articles with interactive elements: [N/10]

## 4. CONTENT GAPS IDENTIFIED
[Numbered list, each with:]
- Gap: [description]
- Type: [Topic/Depth/Media/Data/Freshness/Example/Authority]
- Opportunity: [how we can fill this gap]

## 5. REAL-WORLD EXAMPLES FOUND
[For each example:]
- Source: [company/author] | Type: [blog/talk/repo/benchmark]
- Key insight: [what they said/showed]
- Credibility: [Primary/Secondary/Anecdotal]
- Recency: [date] | URL: [link]

[If no examples found:]
- SYNTHETIC EXAMPLE NEEDED: [describe what to create]
- DEMO SUGGESTION: [describe what demo to build]

## 6. DIFFERENTIATION OPPORTUNITIES (Top 3)
1. [Opportunity] | Impact: [1-5] | Effort: [1-5] | Ratio: [N]
   - What to do: [specific action]
2. [Opportunity] | Impact: [1-5] | Effort: [1-5] | Ratio: [N]
   - What to do: [specific action]
3. [Opportunity] | Impact: [1-5] | Effort: [1-5] | Ratio: [N]
   - What to do: [specific action]

## 7. RECOMMENDED OUTLINE
[H2-level outline with:]
- H2: [heading text]
  - Content: [what to cover]
  - Media: [what visual/interactive element to include]
  - Word budget: [approximate words]
  - Gap filled: [which gap this addresses]

## 8. BRAND RELEVANCE
- Relevant features: [list from brand.features — check maturity labels; only STABLE and BETA features; from product-context skill]
- Mention opportunities: [where brand.brand_name naturally fits — read brand.mention_frequency for count target]
- CTA recommendation: [type + placement]
- Funnel stage: [TOFU/MOFU/BOFU] → mention proportion: [0-2 / 3-5 / unlimited]

## 9. METADATA RECOMMENDATIONS
- Title: [recommended title, 55-60 chars]
- Meta description: [recommended, 150-160 chars]
- URL slug: [recommended slug]
- Schema types: [Article + any additional]
- Featured snippet target: [Yes/No, format to use]

## 10. TECHNICAL REQUIREMENTS
- Estimated word count: [range]
- Code language: [TypeScript/JavaScript based on audience]
- Demo/test target: [read from brand.urls — homepage or docs section; never hardcode a URL]
- Minimum internal links: [based on length]
- External authority links: [suggested sources]
═══════════════════════════════════════
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — USER APPROVAL FLOW
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

After generating the brief:
1. Present the complete brief to the user
2. Ask: "Approve this brief to proceed with content generation, or would you like to adjust anything?"
3. On approval → auto-feed the brief into the Content Creation Pipeline (content-generation-engine + all downstream skills)
4. On revision request → update the specific section and re-present
5. On rejection → ask for clarification on what direction to take instead

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — QUALITY STANDARDS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Minimum Research Depth
| Element | Minimum Requirement |
|---|---|
| Competitor pages analyzed | 10 (or all if fewer than 10 exist) |
| Media types audited per competitor | All 7 types (images, videos, code, tables, interactive, infographics, GIFs) |
| Content gaps identified | Minimum 5 |
| Real-world examples searched | Minimum 5 search queries across different source types |
| Differentiation opportunities scored | Minimum 5, recommend top 3 |
| Google signals captured | PAA (all available), autocomplete (10+), related searches (8+) |

### Accuracy Standards
- All competitor word counts must be fetched, not estimated
- All search volumes must come from web search (no fabrication)
- All real-world examples must have verifiable URLs
- All gap claims must be supported by competitor analysis evidence
- Never claim "no one covers X" without checking all 10 competitor pages

---

*Content Research Engine — v1.0.0 | Nexus SEO Operating System*
