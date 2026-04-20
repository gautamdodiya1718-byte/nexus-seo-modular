# ai-search-positioning.skill

**Role:** Strategy skill for positioning a brand in AI search surfaces (Perplexity, ChatGPT search, AI Overviews / Google SGE, Gemini, Claude). AI search behaves differently from Google ranking — different content patterns get cited, different brand attributes matter, different optimization investments pay off.

**Loaded by:** `nexus-strategy` SKILL.md in modes: full_strategy, ai_search_positioning

---

## Why this skill exists

AI search is a new traffic surface that doesn't behave like Google. Brands that don't position for it lose visibility in increasingly important user journeys (people asking ChatGPT "what's the best X" instead of Googling).

Strategic positioning for AI search is not the same as SEO. It's a parallel discipline that needs explicit strategy.

---

## Inputs

Required:
- Brand profile (industry, what we do, content pillars, differentiators)
- Existing content inventory (from content-memory if available)

Optional:
- Existing AI surface mentions (if user has tracked which queries surface their brand in AI tools)
- Competitor AI surface presence (if known)

---

## Process

### Step 1 — Current AI surface presence baseline

Use web_search to assess current state:

For brand's top 5 priority topics:
- Search the topic in AI tools (or simulate by asking Claude with web context)
- Note: does the brand get cited / mentioned / linked?
- Note: which competitors get cited
- Note: which sources AI tools default to (Wikipedia, official docs, top blogs, specific publications)

Output: baseline — "Currently mentioned in AI for X queries, missing from Y queries."

### Step 2 — Identify high-value AI query categories

For this brand, identify which AI query patterns are most valuable to win:

| Query pattern | Why it matters | Brand fit |
|---|---|---|
| "What is X" | Top-of-funnel awareness | Match if brand defines the category |
| "Best X for Y" | High commercial intent | Match if brand competes on category |
| "X vs Y" | Comparison shopping | Match if brand competes against named alternatives |
| "How to do X" | Tutorial / consideration | Match if brand provides tools/services |
| "Why does X happen" | Education / problem framing | Match if brand owns problem space |
| "Recommended X for Y use case" | Specific recommendation | Match if brand fits use case |

For each pattern + brand fit: prioritize.

### Step 3 — AI surface gap analysis

For each priority query pattern:
- Where is the brand currently?
- Where are competitors?
- What's the path to be cited?

Common gaps:
- **Schema gap** — brand's content doesn't have FAQPage / HowTo / BreadcrumbList that AI surfaces use
- **Self-containment gap** — content is good but paragraphs depend on context, can't be lifted cleanly
- **Authority gap** — content lacks author bylines, dates, citations that AI surfaces use for credibility
- **Source diversity gap** — AI surfaces cite Wikipedia + official docs + 1-2 top blogs; brand isn't in the "top 1-2 blogs" tier yet
- **Definition gap** — content doesn't have clear, citation-friendly definitions that AI lifts

### Step 4 — Strategic recommendations

Based on gaps, recommend strategic actions:

**Content actions:**
- Restructure top content for self-containment
- Add definition-first H2 sections to existing pillar pages
- Generate FAQPage schema across content with FAQ sections
- Strengthen author bylines + bio links (E-E-A-T for AI)
- Cite primary sources more aggressively

**Schema actions:**
- Audit current schema across site (delegate to nexus-tech-seo if user has it)
- Add HowTo schema to tutorials
- Add Article schema with proper hasPart relationships for pillar pages

**Authority actions:**
- Get cited by Wikipedia (if brand is genuinely notable)
- Get cited by top industry publications (PR play)
- Build official docs / comprehensive references (vs blog-only content)
- Establish author authority (publish under named author profiles, not "Team")

**Distribution actions:**
- Be present in Reddit / forums where the topic is discussed (AI surfaces lift from these)
- Be present in YouTube transcripts (AI tools index them)
- Be present in podcast transcripts (AI tools index when transcripts exist)

### Step 5 — Measurement plan

Recommend tracking:
- AI mention monitoring (manual or via tools like AI Overview Tracker, Perplexity citation tracking)
- Branded query volume on AI tools (proxy: branded search volume + brand mention sentiment)
- Direct traffic from AI tools (where attribution exists — some AI tools refer with utm_source)

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: ai-search-positioning
Status: COMPLETE
Inputs consumed: brand profile, content inventory, [web_search results]

CURRENT AI SURFACE PRESENCE:
  Topics where brand is cited: [N — list]
  Topics where brand is missing: [N — list]
  Competitor presence comparison:
    [Competitor]: cited in [N] of [M] priority queries
    [Brand]: cited in [N] of [M] priority queries

PRIORITY AI QUERY CATEGORIES FOR THIS BRAND:
  1. [Pattern type]: [example queries] | Strategic value: [HIGH/MEDIUM/LOW]
  2. ...

AI SURFACE GAPS IDENTIFIED:
  Schema gap: [details — N pages missing required schema]
  Self-containment gap: [details — paragraphs need restructuring]
  Authority gap: [details — author bylines / dates / citations needed]
  Source diversity gap: [details — competitive sources AI prefers]
  Definition gap: [details — pillar pages lack definition-first H2s]

STRATEGIC RECOMMENDATIONS (priority order):

1. Content actions:
   - [Specific action with which existing content + expected AI citation lift]
   - ...

2. Schema actions:
   - [Specific action — which content gets which schema]
   - ...

3. Authority actions:
   - [Specific action — author profiles, Wikipedia, PR, etc.]
   - ...

4. Distribution actions:
   - [Specific action — Reddit, YouTube, podcasts, etc.]
   - ...

MEASUREMENT PLAN:
  - [tracking method 1]
  - [tracking method 2]

ESTIMATED TIMELINE TO IMPACT:
  - Schema improvements: 2-4 weeks (next AI surface re-index)
  - Authority improvements: 3-6 months (slow accumulation)
  - Distribution presence: 1-3 months for forum/social, 6+ months for media

CONFIDENCE: HIGH (web_search baseline + brand analysis) / MEDIUM (limited baseline) / LOW (no current state data)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **AI search ≠ Google search.** Recommendations may diverge from traditional SEO advice. Make trade-offs visible.

2. **Web_search the baseline.** Don't infer current AI surface presence from training data. Search and observe.

3. **Authority is hardest.** AI surfaces favor authoritative sources (Wikipedia, official docs, established publications). New brands need explicit authority-building plans.

4. **Schema matters disproportionately.** AI surfaces use schema for type detection and field extraction. Schema gap is often the highest-leverage fix.

5. **Distribution beyond own site.** AI tools index Reddit, YouTube, podcasts. Brand presence on these platforms feeds AI citations.

6. **Measurement is hard but important.** AI surface attribution is murky. Recommend the best available tracking even if imperfect.
