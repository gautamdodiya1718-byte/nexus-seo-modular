# blog-post-auditor.skill.md

**Role:** Forensic AEO+GEO+SEO audit engine. Runs a structured 7-section analysis on any blog post URL (or draft text). Produces a Scores Dashboard, Priority Fix List, and Rewritten H2 Structure.

**Always load alongside:** `scoring-rubric.skill.md`  
**Load when reaching Section 3 + Phase 3 deliverable:** `heading-rhetoric.skill.md`

---

## Purpose

Run a forensic AEO+GEO+SEO audit on any blog post. Deliver honest, specific, actionable findings. No encouragement. No filler. Every finding must include: what's wrong, why it hurts ranking, and the exact fix.

---

## Inputs Required

Collect before starting (or infer from context):

1. **Blog URL** — The page to audit (required; or draft text for pre-publish QA)
2. **Primary keyword** — The main keyword the blog should rank for (required)
3. **Secondary keywords** — 3-5 additional target keywords (optional but recommended)

If only a URL is provided, infer the primary keyword from the title and H1, then confirm before proceeding.

---

## When This Skill Runs in Nexus

| Context | When | Input |
|---|---|---|
| **Audit Pipeline [04]** | Standalone CONTENT-AUDIT request | Live URL + primary keyword |
| **Content Pipeline [17] — optional QA gate** | User says "QA before publishing" or FULL mode | Generated draft text |
| **Optimization Pipeline [00] — optional entry** | URL provided; findings feed optimization-engine | Live URL + primary keyword |

---

## Phase 1: Data Collection

Collect ALL data BEFORE writing any analysis. Do not start analyzing until all data is gathered.

### Step 1: Fetch the blog content

Use `web_fetch` to load the full page content. Extract:
- H1 (title)
- All H2 and H3 headings (in order)
- Word count (approximate)
- Number of code blocks
- Number of tables
- Number of images
- Internal links (count and destinations)
- External links (count and destinations)
- FAQ section presence (yes/no)
- TL;DR block presence (yes/no)
- Meta description (if visible)
- Author name and bio
- Publication date
- URL structure

### Step 2: Fetch SERP competitors

> **Nexus integration:** If `deep-serp-analysis` already ran in this pipeline session (CACHE-HIT), reuse its output for Steps 2 and 3. Do not re-fetch.

Search for the primary keyword. For the top 5 results:
- URL and title
- Word count (approximate from snippet or fetch)
- Content format (tutorial, listicle, comparison, guide, docs)
- Whether they have FAQ sections
- Whether they have code examples
- Whether they have comparison tables
- Publisher type (vendor blog, personal blog, official docs, media)
- Domain authority estimate (high/medium/low based on publisher)

### Step 3: Check LLM citability

Ask: If someone typed the primary keyword into ChatGPT, Claude, or Perplexity, would this blog appear in the answer?
- Does the blog contain direct, extractable answer blocks?
- Does it have structured data (tables, numbered lists) that LLMs parse?
- Does it have unique claims attributed to a specific brand?

### Step 4: Brand integration checks (all brands)

Read active brand file for:
  brand.mention_frequency → expected mention count range
  brand.features          → list of features that should be referenced
  brand.primary_cta_text  → expected CTA language

Check:
  - Does brand mention count match brand.mention_frequency? (low=1-2, medium=3-5, high=6+)
  - Are brand mentions backed by proof points or just name-drops?
  - Are referenced features matching maturity labels in brand.features?
    (UNRELIABLE or COMING_SOON features should not appear)
  - Is the CTA section present and does it use brand.primary_cta_text?
  - Are internal links sourced exclusively from brand.urls? (zero invented URLs)

---

## Phase 2: Analysis and Scoring

Run all 7 sections. Every finding follows this format:

```
**Finding:** [Specific observation with evidence]
**Impact:** [Which ranking factor or citation signal this affects]
**Fix:** [Exact action — rewrite, add, remove, restructure]
**Priority:** [Critical / High / Medium / Low]
```

---

### SECTION 1: Why This Isn't Ranking Top 3

**1.1 Search Intent Match**  
Determine dominant SERP intent (Navigational / Informational / Commercial / Transactional). Compare blog format against top 3 results. Score: Match / Partial Match / Mismatch.

**1.2 Content Depth Gap**  
Word count comparison. Code examples comparison. Tables/matrices comparison. List 3-5 subtopics competitors cover that this blog misses. Score: Exceeds / Matches / Below / Significantly Below.

**1.3 E-E-A-T Signals**  
Author bio quality (yes/no + what's missing). Original data presence (yes/no). Proof points count (screenshots, CI output, error logs). First-person experience signals count. Score: Strong / Adequate / Weak / Absent.

**1.4 Technical SEO**  
H1/H2/H3 hierarchy issues. Internal link count and quality. Schema markup presence (FAQ, HowTo, Article). URL structure. Image optimization. Score: Clean / Minor Issues / Major Issues.

**1.5 Authority Gap**  
Estimated DA vs top 3 ranking sites. External backlinks to this page (if observable). Brand mentions in developer communities. Score: Competitive / Disadvantaged / Significantly Disadvantaged.

---

### SECTION 2: Scope of Improvement

For every issue from Section 1, apply the full Finding → Impact → Fix → Priority format.

Prioritize fixes in this order:
1. Content gap closures vs top 3 (biggest ranking impact)
2. LLM citability improvements (structured answers, definitions, tables)
3. On-page SEO fixes (headings, internal links, schema)
4. E-E-A-T improvements (proof points, original data, author authority)

---

### SECTION 3: Heading Audit

For EVERY H2 and H3 in the blog, create a row:

| # | Heading (exact text) | Level | Keyword Present? | AEO Answerable? | Creates Tension? | Verdict | Suggested Rewrite |
|---|---|---|---|---|---|---|---|
| 1 | [exact heading] | H2 | Yes/No | Yes/No | Yes/No | Pass/Fail | [rewrite or "OK"] |

Additional checks:
- Flag H2s that should be split into 2 sections
- Flag H3s that are actually H2-level topics
- Flag orphan H3s without a parent H2
- Count generic headings (Prerequisites, Getting Started, Conclusion, etc.)

> **Load `heading-rhetoric.skill.md` now** for rewrite patterns.

---

### SECTION 4: Technical Depth Assessment

**4.1 Technical Level Score** — Rate 1-10 (1-3: Beginner / 4-5: Intermediate / 6-7: Advanced / 8-10: Expert)

**4.2 Target Audience Fit** — "This blog is written for a [job title] with [X years] of experience at a [company type]. They would read this blog because [specific reason]."

**4.3 The Catch-22 Assessment**
- Could the target reader get equivalent value by prompting an LLM? (yes/no + explanation)
- What original insight or artifact does this blog contain that an LLM can't generate? (list them, or say "none")
- If "none": Recommend 3 specific angles that would create differentiation.

**4.4 Code Quality (if code examples present)**
- Are examples copy-pasteable and runnable?
- Do they handle edge cases or just happy paths?
- Are imports and context included?

---

### SECTION 5: AEO Score (Answer Engine Optimization)

Rate each dimension 1-5 using `scoring-rubric.skill.md`:

| Dimension | Score | Evidence |
|---|---|---|
| Direct answer blocks | X/5 | |
| Structured data | X/5 | |
| Unique attribution signals | X/5 | |
| Freshness signals | X/5 | |
| Definition sentences | X/5 | |
| FAQ section | X/5 | |

**Overall AEO Score: X/30**

---

### SECTION 6: GEO Score (Generative Engine Optimization)

**6.1 Citation Worthiness** — "If Perplexity were answering '[primary keyword]', would they cite this blog?" Answer Yes or No with specific reason.

**6.2 Authoritative Framing** — Authority signals vs vendor signals. Be specific.

**6.3 Link Magnetism** — List specific linkable assets (tables, frameworks, templates, data). If none, recommend what to add.

---

### SECTION 7: Competitive Content Gap Analysis

Compare against top 3 ranking results:

| Dimension | This Blog | Competitor 1 | Competitor 2 | Competitor 3 |
|---|---|---|---|---|
| Word count | X | X | X | X |
| Code examples | X | X | X | X |
| Tables | X | X | X | X |
| FAQ section | Y/N | Y/N | Y/N | Y/N |
| TL;DR block | Y/N | Y/N | Y/N | Y/N |
| Original data/insight | Y/N | Y/N | Y/N | Y/N |
| Video/interactive | Y/N | Y/N | Y/N | Y/N |

Topics competitors cover that this blog misses (list 3+). Trust signals competitors have that this blog lacks.

---

## Phase 3: Deliverables

Deliver in this exact order:

### 1. Executive Summary
5 bullets, 1 sentence each. Each = 1 specific problem + 1 specific fix. No filler.

### 2. Scores Dashboard

| Dimension | Score | Grade |
|---|---|---|
| Search Intent Match | Match/Partial/Mismatch | A/B/C/D/F |
| Content Depth vs Competition | Exceeds/Matches/Below | A/B/C/D/F |
| E-E-A-T Signals | Strong/Adequate/Weak | A/B/C/D/F |
| Technical SEO | Clean/Minor/Major | A/B/C/D/F |
| AEO Score | X/30 | A/B/C/D/F |
| Heading Quality | X/Y passing | A/B/C/D/F |
| Catch-22 Assessment | Pass/Fail | Pass/Fail |

Grading: A = 90%+, B = 75–89%, C = 60–74%, D = 40–59%, F = below 40%

### 3. Full Audit
Sections 1–7 above.

### 4. Priority Fix List

Top 10 changes ranked by estimated ranking impact:

| # | Fix | Section | Priority | Estimated Impact |
|---|---|---|---|---|
| 1 | [specific action] | [Section X] | Critical | [why this matters most] |

### 5. Rewritten H2 Structure

> **Load `heading-rhetoric.skill.md`** to apply rhetorical patterns.

```
# [Rewritten Title]

## [Rewritten H2 — Rhetorical Pattern: X]
### [H3 if needed]

## [Rewritten H2 — Rhetorical Pattern: Y]

## [Rewritten H2 — Rhetorical Pattern: Z]

## FAQ: [Primary Keyword]
```

---

## Critical Rules

1. **Forensic, not encouraging.** Mention strengths only when they inform the fix strategy.
2. **Every finding needs evidence.** Quote the specific heading, sentence, or structural element.
3. **Every fix must be actionable.** "Improve the headings" is not a fix. "Rewrite H2 'Prerequisites' to 'What you need before Claude Code writes a single test'" is a fix.
4. **Compare against real competitors.** Fetch and analyze actual SERP results. Do not guess.
5. **Be honest about the Catch-22.** If content is reproducible by prompting an LLM, say so.
6. **Never fabricate data.** If DA or backlink count is not determinable, say so and move on.
7. Apply Step 4 brand integration checks for all brands.
8. **Nexus integration:** If SERP data was already collected upstream (serp-intelligence / deep-serp-analysis), mark those steps as CACHE-HIT and reuse. Do not re-fetch.

---

## Failsafe

If the URL is inaccessible (paywalled, 404, auth-required), ask the user to paste the content directly. Proceed with all sections except Step 2 competitor fetch (which runs independently).
