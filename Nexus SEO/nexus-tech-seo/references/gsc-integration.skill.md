# SKILL: data/gsc-integration.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Data / Analytics
# EXECUTION PRIORITY: ON-DEMAND — Triggered when user asks to analyze search performance, identify content opportunities from GSC data, or diagnose traffic issues.
# POSITION: Connects to Google Search Console API to pull performance data (queries, pages, impressions, clicks, CTR, position). Analyzes the data to identify opportunities, cannibalization, striking distance keywords, and niche targeting recommendations.
# DEPENDENCIES: official-links-repository.skill, ranking-diagnostics.skill

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill connects to Google Search Console to pull real performance data and transform it into actionable content strategy insights. It answers: What's working? What's underperforming? Where should we invest next?

### Data Access Method
Connect via GSC API using credentials provided by the user:
- User provides: Service account JSON key or OAuth credentials
- Property: read brand.gsc_property from active brand file.
  If brand.gsc_property is blank, ask the user: "What is your Google Search Console
  property? (e.g. https://yourdomain.com or sc-domain:yourdomain.com)"
- API endpoint: Google Search Console API v3

### Fallback: CSV Upload
If API connection fails or user prefers:
- User exports CSV from GSC (Performance > Export)
- Upload to Claude for analysis
- Skill processes the CSV identically to API data

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — DATA QUERIES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Query Types

| Query | API Parameters | Purpose |
|---|---|---|
| **Top queries** | dimensions: [query], rowLimit: 1000, last 90 days | See what keywords drive impressions/clicks |
| **Top pages** | dimensions: [page], rowLimit: 500, last 90 days | See which pages perform best |
| **Query + Page** | dimensions: [query, page], rowLimit: 2000 | Detect cannibalization (multiple pages for same query) |
| **Query + Country** | dimensions: [query, country], rowLimit: 500 | Identify geographic opportunities |
| **Query + Device** | dimensions: [query, device], rowLimit: 500 | Mobile vs desktop performance |
| **Date comparison** | Compare last 28 days vs previous 28 days | Trend detection |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — ANALYSIS MODULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Module 1: Striking Distance Analysis
Find keywords close to page 1 that could be won with optimization:

| Position Range | Classification | Action |
|---|---|---|
| 1-3 | **Defending** | Monitor, ensure freshness, protect backlinks |
| 4-10 | **Striking distance (page 1)** | Optimize content depth, add internal links, improve CTR with better title/meta |
| 11-20 | **Striking distance (page 2)** | Significant content update + internal link reinforcement |
| 21-50 | **Visibility** | Assess if worth investing in or if keyword is too competitive |
| 51+ | **Discovery** | Content exists but not competitive; consider rewrite or new approach |

**Output:** Prioritized list of striking distance keywords sorted by (impressions × CTR improvement potential).

### Module 2: Cannibalization Detection
Find cases where multiple [brand.brand_name] pages compete for the same keyword:

1. Group queries by keyword where 2+ different URLs appear
2. For each group, identify which page ranks higher
3. Flag the lower-ranking page as cannibalization risk
4. Recommend: consolidate, differentiate, or redirect

**Output:** Cannibalization report with specific resolution actions.

### Module 3: CTR Optimization Opportunities
Find keywords with high impressions but low CTR (title/meta description problem):

| Position | Expected CTR | If Actual CTR Below | Diagnosis |
|---|---|---|---|
| 1-3 | 15-30% | Below 10% | Title or meta description not compelling |
| 4-10 | 3-10% | Below 2% | Title may not match intent, or SERP features stealing clicks |
| 11-20 | 1-3% | Below 0.5% | Normal for page 2, but optimize title for when it reaches page 1 |

**Output:** Pages ranked by (impressions × CTR gap) — the biggest CTR improvement opportunities first.

### Module 4: Content Gap from GSC
Find queries with impressions but NO dedicated page:

1. List all queries with 50+ impressions in last 90 days
2. Match each query to the ranking URL
3. Identify queries where the ranking URL is NOT specifically about that query (e.g., a broad page ranking for a specific long-tail)
4. These are content creation opportunities

**Output:** New content ideas sorted by impression volume.

### Module 5: Trend Analysis
Compare metrics across time periods:

1. Queries gaining impressions (growing topics)
2. Queries losing impressions (declining topics)
3. Pages gaining clicks (improving content)
4. Pages losing clicks (degrading content — needs refresh)

**Output:** Trend report with arrows (↑ growing, ↓ declining, → stable).

### Module 6: Niche Targeting Recommendations
Based on all GSC data, identify:

1. **Strongest niches:** Topic clusters where [brand.brand_name] already ranks well — double down
2. **Emerging niches:** Topics with growing impressions but no page 1 positions — invest now
3. **Adjacent niches:** Related topics [brand.brand_name] doesn't cover but has authority in nearby topics — expand
4. **Declining niches:** Topics losing impressions — deprioritize or refresh

**Output:** Niche matrix with recommendation per niche.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — OUTPUT: GSC ANALYSIS REPORT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
═══════════════════════════════════════
GSC ANALYSIS REPORT
Property: [brand.gsc_property]
Date range: [start] to [end]
═══════════════════════════════════════

## SUMMARY METRICS
- Total impressions: [N] ([+/-X%] vs previous period)
- Total clicks: [N] ([+/-X%])
- Average CTR: [X%]
- Average position: [X]
- Total indexed pages: [N]
- Unique queries appearing: [N]

## TOP OPPORTUNITIES

### Striking Distance Keywords (Quick Wins)
[Top 10 keywords in positions 4-20 with highest impression × CTR-gap]

### CTR Optimization Targets
[Top 10 pages with highest impression but lowest CTR relative to position]

### Cannibalization Issues
[All detected cannibalization pairs with resolution recommendations]

### Content Gap Opportunities
[Top 10 queries with high impressions but no dedicated page]

## NICHE ANALYSIS
[Matrix of topic clusters with performance and recommendations]

## TREND ALERTS
### Growing (invest more)
[Keywords/pages gaining traction]

### Declining (refresh needed)
[Keywords/pages losing traction]

## RECOMMENDED ACTIONS (Priority Order)
1. [Highest-impact action] — Expected impact: [+X clicks/month]
2. ...
═══════════════════════════════════════
```

---

*GSC Integration — v1.0.0 | Nexus SEO Operating System*
