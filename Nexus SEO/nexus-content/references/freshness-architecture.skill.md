# freshness-architecture.skill

**Role:** Build content with an evergreen structural spine separated from time-sensitive sections, so the article stays ranked through algorithm updates and minor edits keep it fresh without rewrites.

**Loaded by:** `nexus-content` SKILL.md in modes: standard_blog, pillar_page, content_refresh

---

## Why this skill exists

Content that mixes evergreen and time-bound information silently decays. A "Best CI Tools 2024" article includes both timeless explanations (what CI is, why it matters, key features) and time-bound specifics (current pricing, version numbers, vendor positioning). Six months later, the time-bound parts are wrong, the evergreen parts are still right, and the whole article looks stale.

This skill explicitly separates the two — evergreen sections that stay valid without changes, and time-bound sections clearly scoped for periodic updates.

---

## Inputs

Required:
- Content outline (from serp-blueprint-generator) OR content draft
- Content topic + intent

Optional:
- Brand profile (some industries have faster-moving topics)
- Topic velocity classification (if user specifies; otherwise auto-detect)

---

## Process

### Step 1 — Topic velocity classification

Auto-detect how fast this topic changes:

| Velocity | Examples | Update cadence |
|---|---|---|
| **STATIC** | Math concepts, programming language fundamentals, history | 2+ years |
| **SLOW** | Best practices, design patterns, established methodologies | 12-18 months |
| **MODERATE** | Tool comparisons, framework usage, industry standards | 6-12 months |
| **FAST** | Pricing, version numbers, vendor positioning, current trends | 3-6 months |
| **VERY_FAST** | Breaking news, current events, daily-changing data | 1-3 months |

Combined velocity = highest velocity of any section in the content.

### Step 2 — Identify evergreen sections (the spine)

Within the content outline, identify which sections are evergreen (don't change with time):

Examples of evergreen content:
- Definitions of fundamental concepts
- Explanations of why a problem exists
- Architectural principles
- Methodology explanations
- Historical context
- Conceptual examples (not real-company examples)
- "Why this matters" explanations

Mark these sections as EVERGREEN_SPINE. They form the article's stable structure.

### Step 3 — Identify time-bound sections

Identify sections that contain time-sensitive information:

Examples of time-bound content:
- Specific tool features (vendors update features)
- Pricing
- Version numbers
- Recent statistics ("as of 2024", "in the last quarter")
- Current vendor positioning
- Comparison rankings
- Recent examples (real company examples that may become stale)
- "Latest" or "newest" anything

Mark these sections as TIME_BOUND. They will need refresh on a known cadence.

### Step 4 — Apply structural separation

Restructure or annotate content so evergreen and time-bound are clearly separated:

**Option A — Section-level separation:**
- Evergreen sections form the main body
- Time-bound sections grouped into "Current Landscape" or "What's Happening Now" sections that are easier to update wholesale

**Option B — Inline annotation:**
- Time-bound passages within sections marked with `<!-- FRESHNESS: REVIEW BY [date] -->` comments (HTML comments invisible to readers but parseable by automation)

**Option C — Sidebar/callout extraction:**
- Time-bound facts extracted into callouts ("As of [month/year]: pricing starts at $X") that visually separate from main flow

Choose option based on:
- A: works best for content with clear time-bound vs evergreen sections
- B: works best for content with time-bound facts scattered throughout
- C: works best for landing pages and content where current state is important to surface

### Step 5 — Date stamping for time-bound sections

For each TIME_BOUND section, add a visible "Last verified: [date]" stamp:
- Helps readers know how current the information is
- Helps SEO (Google rewards visible freshness signals on time-sensitive topics)
- Helps maintenance (easy to spot what needs updating)

### Step 6 — Assign review triggers (feeds into decay-mapping)

For each TIME_BOUND section:
- Assign next-review-by date based on topic velocity
- VERY_FAST: 30 days
- FAST: 90 days
- MODERATE: 180 days
- SLOW: 365 days

Output to decay-mapping skill so it can manage the review schedule.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: freshness-architecture
Status: COMPLETE
Inputs consumed: content outline / draft, topic, brand

TOPIC VELOCITY CLASSIFICATION: [STATIC / SLOW / MODERATE / FAST / VERY_FAST]
RATIONALE: [why this topic moves at this velocity]

EVERGREEN_SPINE SECTIONS (stable, no scheduled review):
  - Section [name]: [why evergreen]
  - Section [name]: [why evergreen]
  ...

TIME_BOUND SECTIONS (need scheduled review):
  - Section [name]: [why time-bound] | Velocity: [tier] | Next review: [date]
  - Section [name]: [why time-bound] | Velocity: [tier] | Next review: [date]
  ...

STRUCTURAL SEPARATION APPLIED:
  Strategy: [A: section-level / B: inline annotation / C: callout extraction]
  Specific changes:
    1. [change description]
    2. [change description]
    ...

DATE STAMPS ADDED:
  - Section [name]: "Last verified: [date]" inserted at [location]
  - ...

REVIEW SCHEDULE GENERATED (passed to decay-mapping):
  Section [name]: review by [date] (every [cadence])
  Section [name]: review by [date] (every [cadence])
  ...

TOTAL TIME-BOUND SECTIONS: [N]
TOTAL EVERGREEN SECTIONS: [N]
EVERGREEN STABILITY RATIO: [%] (higher = more stable through time/algorithm changes)

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Auto-detect topic velocity from content topic + keywords.** Don't ask user every time. Get it right based on patterns.

2. **Separation happens during generation, not post-hoc.** Better to design freshness architecture into the outline than retrofit it.

3. **Visible date stamps on time-bound sections.** Helps readers, helps SEO, helps maintenance.

4. **Review triggers feed decay-mapping.** This skill defines what needs updating; decay-mapping schedules and tracks updates.

5. **Higher evergreen ratio = more stable.** Aim for 60%+ evergreen content where possible. Pure-trend content (100% time-bound) decays fast and is harder to maintain.

6. **Conservative on velocity assignment.** When in doubt, err on the FASTER side (more frequent reviews) — under-reviewing is worse than over-reviewing.
