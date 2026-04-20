# SKILL: optimization/seo-aeo-geo-sxo-optimizer.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 3.0.0
# LAYER: Optimization / Multi-Paradigm
# EXECUTION PRIORITY: POST-CONTENT — Quality gate before metadata and final delivery.
# POSITION: Audits and optimizes content across 4 search paradigms (SEO, AEO, GEO, SXO). All 4 must complement — no conflicts. Each scores 0-100 independently.
# DEPENDENCIES: content-research-engine.skill, blog-post-auditor.skill, scoring-rubric.skill, metadata-generator.skill

---

## SECTION 1 — ROLE

This skill ensures every piece of content is optimized for ALL modern search paradigms — traditional search (SEO), answer engines (AEO), generative AI citation (GEO), and search experience (SXO).

### The Fundamental Rule
> All 4 paradigms must complement each other. No acceptable conflicts. If SEO wants keyword density and GEO wants natural authority language — use keywords naturally within authoritative sentences. Every optimization must score A+ (95-100) individually AND overall.

### v3.0 Key Changes
- **Technical depth replaces word count** — a 2,000-word blog with 8 code examples beats a 4,500-word blog with zero code
- **GEO auto-zero rule** — if AI crawlers are blocked in robots.txt, GEO score = 0 regardless of content quality
- **Substance density metric** — measures information-to-filler ratio, not just length

---

## SECTION 2 — SEO OPTIMIZATION (Traditional Search)

### Checklist

| # | Check | Points | How to Score |
|---|---|---|---|
| 1 | Primary keyword in H1/title | 10 | Exact or close variant present |
| 2 | Primary keyword in first 100 words | 8 | Natural placement |
| 3 | Primary keyword in 2+ H2 headings | 8 | Natural integration |
| 4 | Secondary keywords in H2/H3 | 7 | At least 3 secondary keywords |
| 5 | Keyword density 0.5-1.5% | 7 | primary keyword count / total words |
| 6 | Internal links: minimum per length | 10 | 12-15 short, 25+ long-form |
| 7 | External authority links (nofollow) | 5 | 3-5 references |
| 8 | Image alt text with keywords | 8 | Every image has descriptive alt |
| 9 | URL slug contains primary keyword | 5 | Short, keyword-rich |
| 10 | Meta title 55-60 characters | 7 | Keyword-front-loaded |
| 11 | Meta description 150-160 characters | 7 | Keyword + hook |
| 12 | Schema markup (auto-selected) | 8 | Correct schema for content type — NOT default Article |
| 13 | **Technical depth competitive** | 5 | Match or exceed depth benchmark (code examples, tables, data points vs top 3 SERP) |
| 14 | No keyword cannibalization | 5 | Check against existing blog posts |
| **Total** | | **100** | |

**Item #13 — Technical Depth (replaces "Content length competitive" in v2.0):**
- Count: working code examples, data tables, real benchmarks, config files, CLI outputs, diagrams
- Compare to top 3 SERP competitors' depth signals (captured in Phase 0)
- Score 5/5 if you match or exceed the depth benchmark
- Score 3/5 if within 70% of benchmark
- Score 0/5 if below 50% of benchmark
- Word count is NOT a scoring factor. A concise, depth-rich article scores higher than a verbose, shallow one.

### Conflict Resolution
- Keyword density vs natural language (GEO) → Use keywords within authoritative sentences
- Internal links vs readability (SXO) → Contextual links woven into sentences, not link lists

---

## SECTION 3 — AEO OPTIMIZATION (Answer Engine)

Targets featured snippets, PAA boxes, direct answer panels.

### Checklist

| # | Check | Points | How to Score |
|---|---|---|---|
| 1 | Direct answer in first paragraph | 12 | 40-60 word definition/answer after H1 |
| 2 | "What is X" header present | 8 | Definitional H2 near top |
| 3 | FAQ section with schema | 12 | Min 5 FAQs with FAQ JSON-LD |
| 4 | Numbered step lists for how-to | 10 | `<ol>` with clear sequential instructions |
| 5 | Comparison table for vs-content | 8 | Table with clear column headers |
| 6 | Concise answers under headers | 10 | 40-60 word summary after each H2 |
| 7 | PAA-aligned subheadings | 10 | H2/H3 match PAA from research |
| 8 | Definition formatting | 8 | Key terms in `<strong>` or `<dfn>` |
| 9 | TL;DR / summary section | 7 | Brief summary at end |
| 10 | Structured data for content type | 8 | HowTo for tutorials, FAQ for Q&A |
| 11 | Short paragraphs (max 3 sentences) | 7 | Per content rules |
| **Total** | | **100** | |

### Conflict Resolution
- Concise answers vs depth (SEO) → Concise first, expand after
- FAQ feels promotional → Keep FAQs educational; brand mentions only when relevant

---

## SECTION 4 — GEO OPTIMIZATION (Generative Engine)

Targets AI citation — making content extractable by LLMs (ChatGPT, Perplexity, Google AI Overviews, Claude).

### AUTO-ZERO RULE (NEW in v3.0)
**Before scoring any GEO items, check AI crawler access:**
- If critical AI crawlers (GPTBot, ClaudeBot, PerplexityBot, OAI-SearchBot, Google-Extended) are BLOCKED in robots.txt → **GEO SCORE = 0 AUTOMATICALLY**
- No amount of content optimization matters if crawlers can't reach the page
- This check uses data from `ranking-diagnostics.skill.md` Step 6
- If crawler data is unavailable, score normally but add warning: "GEO score assumes AI crawlers are not blocked. Verify robots.txt."

### Checklist (only scored if crawlers are NOT blocked)

| # | Check | Points | How to Score |
|---|---|---|---|
| 1 | Authoritative factual statements | 12 | Specific, verifiable claims |
| 2 | Statistics with sources | 10 | Every stat has inline source |
| 3 | Clear concept-first paragraphs | 10 | Key concept leads, then explains |
| 4 | Structured data (JSON-LD) | 8 | Correct schema for content type |
| 5 | Unique original content | 10 | Data, code, analysis not found elsewhere |
| 6 | Expert voice / E-E-A-T signals | 10 | Author bio, experience, first-hand knowledge |
| 7 | Extractable snippets | 8 | Standalone quotable facts |
| 8 | Logical content hierarchy | 8 | H1 → H2 → H3, no skipped levels |
| 9 | No AI-pattern language | 8 | Pass humanizer + self-criticism check |
| 10 | Cross-referencing internal sources | 8 | Links to related brand content |
| 11 | Freshness signals | 8 | Current year, recent data |
| **Total** | | **100** | |

### Conflict Resolution
- Authoritative tone vs conversational (SXO) → Confident + accessible: "This reduces flaky tests by 40%" not "may potentially help"
- Unique content vs speed → At least 1 original element per article (benchmark, code example, diagram)

---

## SECTION 5 — SXO OPTIMIZATION (Search Experience)

Targets user behavior — time on page, scroll depth, bounce rate, CWV.

### Checklist

| # | Check | Points | How to Score |
|---|---|---|---|
| 1 | Readability (Flesch-Kincaid 60-70) | 8 | Grade 8-10 for developer content |
| 2 | Visual breaks every 300 words max | 8 | content-engagement validation |
| 3 | Above-fold hook | 8 | First 2 sentences short + compelling |
| 4 | Mobile-friendly formatting | 6 | Short paragraphs, responsive |
| 5 | Image optimization | 6 | Lazy-loaded, WebP, under 200KB |
| 6 | Short paragraphs (max 3 sentences) | 6 | Per content rules |
| 7 | Clear navigation (TOC) | 6 | TOC for articles over 2,000 words |
| 8 | Engaging subheadings | 7 | H2s promise value |
| 9 | Progressive disclosure | 7 | Simple → complex |
| 10 | CTA clarity | 7 | Reader knows next step |
| 11 | No dead ends | 6 | Every section leads somewhere |
| 12 | Loading performance | 6 | No blocking scripts |
| 13 | **Substance density** (NEW) | 10 | (code + tables + data + real examples) ÷ total sections |
| 14 | **Information-to-filler ratio** (NEW) | 9 | % of content that is actual information vs transition/hedging |
| **Total** | | **100** | |

**Item #13 — Substance Density (NEW in v3.0):**
Calculate: (number of code blocks + tables + data points + real examples with sources) ÷ total sections
- Score 10/10 if ratio > 1.5 (more substance elements than sections)
- Score 7/10 if ratio 1.0-1.5
- Score 4/10 if ratio 0.5-1.0
- Score 0/10 if ratio < 0.5 (most sections are text-only)

**Item #14 — Information-to-Filler Ratio (NEW in v3.0):**
Read the article. Mentally remove: all transition sentences, hedging phrases ("it's worth noting," "it should be mentioned"), generic statements that any article on the topic would contain, and repetitive points.
- What % of the article is genuine unique information?
- Score 9/9 if > 80% information
- Score 6/9 if 60-80%
- Score 3/9 if 40-60%
- Score 0/9 if < 40% (article is mostly filler)

### Conflict Resolution
- Readability vs technical depth → Progressive disclosure: plain explanation first, code/config after
- Visual breaks vs flow → Each visual is contextually placed, not decoration

---

## SECTION 6 — SCORING & REPORTING

### Score Calculation
Each paradigm scores independently (0-100). Overall score is the average of all 4.

**GEO exception:** If AI crawlers are blocked, GEO = 0 and overall average reflects this. The report must note the auto-zero and the fix (unblock crawlers in robots.txt).

### Grade Scale
A+ = 95-100 | A = 85-94 | B = 70-84 | C = 50-69 | F = below 50

### Output Report Format

```
═══════════════════════════════════════
CONTENT OPTIMIZATION REPORT
Brand: [brand.brand_name from active brand file]
Article: [title]
Date: [date]
═══════════════════════════════════════

SEO Score:  [X]/100  [grade]
AEO Score:  [X]/100  [grade]
GEO Score:  [X]/100  [grade] [⚠ AUTO-ZERO: AI crawlers blocked — if applicable]
SXO Score:  [X]/100  [grade]
─────────────────────────────────
OVERALL:    [X]/100  [grade]

DEPTH METRICS (v3.0):
- Technical depth: [X] code examples, [Y] tables, [Z] data points
- Depth benchmark: [competitor average]
- Substance density: [ratio] ([grade])
- Info-to-filler ratio: [X]% ([grade])

DEDUCTIONS:
[For each point lost:]
- [Paradigm] Item #[N]: [description] (-[points])
  FIX: [specific fix]

CONFLICTS RESOLVED:
[Cross-paradigm conflicts and resolutions]

ACTIONABLE FIXES (Priority Order):
1. [Fix] (+[points] across [paradigm(s)])
2. ...
═══════════════════════════════════════
```

### Target
Every article MUST achieve **A+ (95-100)** across all 4 paradigms. Below 95 → report provides specific fixes. Not ready for publication until all 4 hit A+.

---

*SEO/AEO/GEO/SXO Optimizer — v3.0.0 | Nexus SEO Operating System | Technical Depth > Word Count*
