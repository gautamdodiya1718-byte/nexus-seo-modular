# SKILL: research/real-world-examples.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Research / Authority Building
# EXECUTION PRIORITY: DURING CONTENT GENERATION — Triggered by content-engagement.skill or content-generation-engine when a claim needs evidence.
# POSITION: Searches for and validates real-world evidence — company case studies, published benchmarks, conference talks, GitHub repos — to support article claims. Builds E-E-A-T authority through verified external evidence.
# DEPENDENCIES: official-links-repository.skill

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill finds real-world evidence to back up claims in content for any brand.
Every significant claim should be supported by data, examples, or authoritative references. This transforms articles from "here's what we think" to "here's what the industry proves."

### What This Skill Searches For

| Evidence Type | Credibility Score | Example |
|---|---|---|
| **Company engineering blog** (primary source) | 5/5 | "Netflix's engineering team documented a 40% reduction in flaky tests" |
| **Conference talk / video** | 4/5 | "Debbie O'Brien demonstrated this approach at Playwright Conf 2025" |
| **Published benchmark / research paper** | 5/5 | "The State of Testing report shows 67% of teams use CI pipelines" |
| **GitHub repo with stars** | 4/5 | "The playwright-sharding-example repo (2.3k stars) shows this pattern" |
| **Third-party case study** | 3/5 | "According to a Testsigma case study, migration took 3 weeks" |
| **Forum / Reddit anecdote** | 2/5 | "A senior SDET on r/QualityAssurance reported..." (use sparingly, only as color) |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — SEARCH METHODOLOGY
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Step 1: Identify Claims Needing Evidence
Scan the draft for:
- Statistical claims ("X% of teams experience...")
- Performance claims ("reduces execution time by...")
- Best practice claims ("leading teams use this approach")
- Comparison claims ("faster than alternative Y")
- Trend claims ("adoption is growing because...")

### Step 2: Search for Evidence
For each claim, search in this order:

1. **Engineering blogs:** `"[topic]" site:engineering.*.com OR site:*.engineering.com OR site:tech.*.com 2025 OR 2026`
2. **Conference talks:** `"[topic]" conference talk OR presentation OR keynote 2025 OR 2026`
3. **Benchmarks:** `"[topic]" benchmark data report statistics 2025 OR 2026`
4. **GitHub repos:** Search GitHub for repos demonstrating the concept, sort by stars
5. **Industry reports:** `"[topic]" state of testing report OR survey OR research`
6. **Brand's own case studies:** Check brand.urls.case_studies in the active brand file for existing case study URLs. Use every case study URL present.

### Step 3: Validate Each Example

| Validation Check | Action |
|---|---|
| **Source exists:** URL returns 200 | If 404, discard |
| **Source is credible:** From a recognized company/publication | If unknown, demote to anecdotal |
| **Data is specific:** Contains actual numbers or concrete details | If vague, note as "qualitative only" |
| **Recency check:** Published date | Priority: 12mo → 24mo → 36mo; flag >36mo as dated |
| **Relevance check:** Actually about the specific topic | If tangential, note the limitation |

### Step 4: Format for Insertion

**For inline citations:**
> "Netflix's testing infrastructure team reported a 40% reduction in flaky tests after implementing deterministic test ordering ([source](URL))."

**For callout boxes (real-world example type):**
```
📊 Real-World Example: Netflix
Netflix's engineering team documented their approach to flaky test reduction
in a 2025 blog post. By implementing deterministic ordering and resource
isolation, they reduced their flaky test rate from 12% to under 3% across
their Playwright suite of 8,000+ tests.
Source: Netflix Technology Blog (nofollow link)
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — WHEN NO EXAMPLES EXIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

For frontier topics where no published examples exist:

### Option A: Synthetic Example
Create a realistic but fictional scenario based on common patterns:
- Use a generic company type ("a mid-size fintech with 200 engineers")
- Base numbers on industry averages from available research
- Clearly label as "typical scenario" not a specific company
- Example: "Consider a team running 3,000 Playwright tests across 4 CI environments. With a 5% flaky test rate, that's 150 false signals per day..."

### Option B: Suggest Building a Demo
Recommend the user create original evidence:
- "No published case studies exist for [topic]. Consider: run a real experiment using [brand.brand_name]'s own product/service, document the results with real data, and publish as an original [brand.brand_name] case study. This is the most defensible form of original evidence."
- This creates original content no competitor can replicate

### Always do BOTH: Create a synthetic example for the current article AND suggest building a live demo for future content.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — QUALITY STANDARDS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

- Every example must have a verifiable URL (or be clearly labeled as synthetic)
- Never fabricate company names, specific numbers, or quotes
- All external links in examples get `rel="nofollow"`
- Minimum 2 real-world examples per long-form article (2,500+ words)
- Minimum 1 real-world example per standard article (1,500-2,000 words)
- If fewer than minimum found, supplement with synthetic examples

---

*Real-World Examples & Case Studies — v1.0.0 | Nexus SEO Operating System*
