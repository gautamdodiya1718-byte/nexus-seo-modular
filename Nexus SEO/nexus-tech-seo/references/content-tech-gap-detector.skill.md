# content-tech-gap-detector.skill

**Role:** Cross-reference content quality with technical SEO health to diagnose whether the bottleneck preventing a page from ranking is content-side or tech-side. Designed for the "my content is great but it's not ranking" scenario.

**Loaded by:** `nexus-tech-seo` SKILL.md in modes: full_audit (final synthesis step), content_tech_bottleneck (primary)

---

## Why this skill exists

A page can fail to rank for many reasons:
- The content is genuinely thin or off-intent (content problem)
- The content is great but technical issues are blocking it from ranking effectively (tech problem)
- The site authority is too low to compete regardless of content/tech (authority problem)
- All three (compound problem)

Most SEO audits jump to "rewrite the content" by default. This skill prevents that mistake by explicitly diagnosing which axis is the bottleneck.

---

## Inputs

Required:
- Output from on-page-tech-checker (tech health per page)
- Output from core-web-vitals-analyzer (page speed health)
- Output from crawlability-checker (indexability)
- Output from gsc-insights-extractor (search behavior — impressions, clicks, position)

Optional but valuable:
- Content quality scores (from nexus-audit if user has run it; or user-provided audit data)
- Domain authority signals (DA / DR estimate; or user-stated estimate)

---

## Diagnostic logic

### Step 1 — Score content health (per page)

If content quality scores are provided (e.g., from blog-post-auditor in nexus-audit):
- Score 80-100: HIGH content quality
- Score 60-79: MEDIUM
- Below 60: LOW

If no content quality scores are provided:
- Infer from available signals: word count (rough), heading hierarchy (proxy for structure), schema presence, internal link count, presence of comparison tables / lists / images, etc.
- Mark confidence as LOWER for inferred content quality

### Step 2 — Score tech health (per page)

Aggregate from upstream skills:

**Crawlability score:**
- 100: indexable, in sitemap, self-canonical, robots-allowed, no blocking issues
- 60-99: indexable but minor issues (missing canonical, sitemap, etc.)
- 0-59: indexability problems (noindex, robots-blocked, canonical to wrong page, etc.)

**On-page tech score:**
- 100: title/meta within range, single H1, valid schema, OG/Twitter present, all images have alt + dimensions
- 60-99: minor on-page issues
- 0-59: major issues (missing H1, malformed schema, 50%+ images missing alt, etc.)

**Core Web Vitals score:**
- 100: All 3 vitals GOOD on mobile + desktop
- 60-99: One or two vitals NEEDS IMPROVEMENT
- 0-59: One or more vitals POOR

Combined tech health = average of the three.

### Step 3 — Score search performance (per page)

From gsc-insights-extractor:
- Impressions in 90 days
- Clicks in 90 days
- Average position
- CTR vs expected for position

### Step 4 — Bottleneck classification

| Content Score | Tech Score | Search Performance | Diagnosis | Fix Priority |
|---|---|---|---|---|
| HIGH | LOW | High impressions, low position | **TECH-BOTTLENECKED** | Fix tech first; content is fine |
| HIGH | MEDIUM | Medium impressions, decent position, low CTR | **TECH-BOTTLENECKED (CTR)** | Fix title/meta + Core Web Vitals |
| HIGH | HIGH | Low impressions, low position | **AUTHORITY-LIMITED** | Tech and content are fine; need backlinks / topical authority |
| HIGH | HIGH | High impressions, good position, low CTR | **SNIPPET ISSUE** | Title/meta rewrite — content is great, tech is great, but the SERP preview isn't compelling |
| MEDIUM | HIGH | Low impressions, low position | **CONTENT-LIMITED** | Improve content depth / E-E-A-T signals; tech is fine |
| MEDIUM | LOW | Anything | **COMPOUND PROBLEM** | Fix critical tech issues first, then improve content |
| LOW | HIGH | Anything | **CONTENT-BOTTLENECKED** | Content is the issue; rewrite/expand significantly |
| LOW | LOW | Anything | **CRITICAL — REBUILD** | Both fundamentally weak; consider full rebuild |

### Step 5 — Specific fix order recommendation

Based on diagnosis, output a prioritized fix list:

For **TECH-BOTTLENECKED:**
```
1. [Critical tech fix from upstream — e.g., "Remove noindex from /this-page"]
2. [Next critical tech fix — e.g., "Fix LCP — currently 4.8s mobile due to unoptimized hero image"]
3. [Next — e.g., "Add valid Article schema with required fields"]
4. After these tech fixes ship, do NOT rewrite content. Wait 4-6 weeks for re-crawl, then re-evaluate.
```

For **CONTENT-BOTTLENECKED:**
```
1. Content audit needed — invoke nexus-audit on this URL for detailed content fix list
2. Specific content signals missing: [list — e.g., "no comparison table where competitors all have one", "0 first-person experience markers", "schema is FAQPage but no FAQ section in content"]
3. Tech is fine — do not over-engineer it; small tech tweaks won't move the needle
```

For **SNIPPET ISSUE:**
```
1. Title rewrite: current "[X]" — suggested "[Y]" — reason: [more specific value prop / number / benefit]
2. Meta description rewrite: current "[X]" — suggested "[Y]" — reason: [stronger CTA / specific outcome]
3. Schema preview check: ensure FAQ/Review/HowTo schema is producing rich snippets in SERP
```

For **AUTHORITY-LIMITED:**
```
1. Tech is fine. Content is fine. Issue is competitive authority.
2. Recommended actions:
   - Build backlinks (specific targets: [from ranking-diagnostics output if available])
   - Increase topical authority by publishing supporting cluster content around this topic
   - Internal link: ensure this page is linked from your highest-authority pages
3. Don't rewrite. Don't re-engineer tech. Build authority.
```

For **COMPOUND PROBLEM:**
```
Order: tech fixes that block ranking → tech fixes that limit ranking → content depth → content quality → snippet
1. [Most critical tech fix — usually crawlability/indexability]
2. [Next critical tech fix]
3. [Content depth gap — most impactful first]
4. [Content quality issue]
5. [Snippet optimization]
```

For **CRITICAL — REBUILD:**
```
This page is failing on both content and tech axes simultaneously.
Rebuild approach:
1. Decide: is this URL worth saving? If it has backlinks, yes. If not, consider 301 to a stronger page.
2. If saving: full rewrite of content + full tech rebuild (new schema, new title/meta, fix Core Web Vitals)
3. If not saving: 301 to the most relevant existing page
```

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: content-tech-gap-detector
Status: COMPLETE
Inputs consumed: [list]

PER-PAGE BOTTLENECK DIAGNOSIS:

Page: [URL]
  Content quality: [HIGH/MEDIUM/LOW] (based on: [source])
  Tech health: [HIGH/MEDIUM/LOW]
    - Crawlability: [score]
    - On-page tech: [score]
    - Core Web Vitals: [score]
  Search performance:
    - Impressions/90d: [N]
    - Clicks/90d: [N]
    - Avg position: [N]
    - CTR vs expected: [%]

  DIAGNOSIS: [bottleneck type from table above]

  WHY: [short explanation citing specific evidence — e.g., "Content scored 92/100 in audit. Tech score is 38 because LCP is 5.2s mobile and the page is noindex due to a stale meta tag from a staging environment."]

  FIX ORDER:
    1. [specific action]
    2. [specific action]
    3. [specific action]

  EXPECTED IMPACT: [if these fixes ship, expected ranking change]

  TIMELINE TO RE-EVALUATE: [4-6 weeks for content changes, 1-2 weeks for tech, after Google re-crawls]

SITE-LEVEL PATTERNS:
  [if multiple pages diagnosed: identify systemic issues — e.g., "12 pages have HIGH content but LOW tech — same root cause: render-blocking script in header template"]

CONFIDENCE: HIGH / MEDIUM / LOW (based on input completeness)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Don't recommend rewriting good content.** This is the core point of the skill. If content scores HIGH and tech is the issue, explicitly tell the user NOT to rewrite.

2. **Be specific about authority limits.** Authority is real. If the content is great and tech is great but you're a DA 20 site competing with DA 80 sites, no amount of optimization moves the needle without backlinks.

3. **Cross-reference systemic issues.** If 10 pages all have the same diagnosis, that's a template/infrastructure issue, not 10 separate page issues. Surface that.

4. **Conservative on rebuild recommendations.** Only recommend full rebuild when both content and tech are LOW. Otherwise, targeted fixes.

5. **Specific timeline expectations.** Tech fixes often show in 1-2 weeks (re-crawl). Content changes take 4-6 weeks. Authority shifts take months. Set expectations.
