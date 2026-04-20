# decay-mapping.skill

**Role:** Tag every piece of content with decay-sensitive sections and assign review triggers (30/60/90/180/365 days) so content can be refreshed before it silently goes stale and loses ranking.

**Loaded by:** `nexus-content` SKILL.md in all generation modes (final tagging step)

---

## Why this skill exists

Content that ranked well silently starts decaying as the world changes around it. Tools update features. Pricing shifts. Statistics get superseded. Best practices evolve. Without explicit decay tracking, content owners don't know what to update or when. They discover the issue only when ranking drops or readers complain.

This skill builds the decay map at content creation time so the system knows exactly when each piece needs review and what to look at.

---

## Inputs

Required:
- Generated content (or content outline if pre-generation)
- Output from freshness-architecture (which sections are time-bound)
- Topic + content type

Optional:
- Brand profile (industry context affects decay velocity)
- User preferences (some users want more frequent review cadences)

---

## Process

### Step 1 — Identify decay-sensitive elements within content

Beyond the section-level separation that freshness-architecture does, decay-mapping identifies specific decay-sensitive elements:

**Highest decay risk:**
- Specific pricing ("$X/month", "starts at $Y")
- Specific version numbers ("v3.2.1", "iOS 17", "Node 20")
- Recent statistics ("As of [recent date]: X% of users...")
- Vendor feature claims ("Tool X supports Y feature")
- Tool/vendor names ("the leading X is Y")
- Recent dates ("In 2024...", "last quarter...")
- Recent events references ("after the recent X update...")
- Real-time data references ("currently...", "right now...")
- Hot trends ("AI search is taking over...")

**Medium decay risk:**
- Industry standards ("most teams use X for Y")
- Best practices ("the recommended approach is X")
- Tool comparisons ("X vs Y in 2024")
- Recommended workflows
- Comparison tables (if comparing tools that update)

**Low decay risk:**
- Conceptual explanations
- Definitions of stable concepts
- Architectural principles
- Methodology explanations
- Historical context (already past)
- Conceptual code examples (not version-specific)

### Step 2 — Per-element review trigger assignment

For each decay-sensitive element, assign a review trigger based on type:

| Element type | Default review cadence | Trigger event |
|---|---|---|
| Specific pricing | 90 days | Date-based |
| Version numbers | 60 days | Date-based + when new major version releases |
| Recent statistics | 180 days | Date-based + when newer data is available |
| Vendor feature claims | 90 days | Date-based + when vendor announces changes |
| Tool/vendor names | 365 days | Date-based + when major industry shifts |
| Recent dates | 365 days | Always update on next refresh |
| Hot trends | 30 days | Date-based |
| Best practices | 365 days | Date-based + when industry guidance changes |
| Tool comparisons | 180 days | Date-based + when any compared tool updates significantly |

### Step 3 — Build decay map

Output a structured decay map:

```
DECAY MAP for content: [title]
Created: [creation date]

DECAY-SENSITIVE ELEMENTS:
  1. Element: "Pricing: starts at $99/month for X plan"
     Location: Section 3, paragraph 4
     Type: pricing
     Review by: [creation date + 90 days]
     What to check: current pricing on vendor's official page
     Update action: replace with current pricing if changed

  2. Element: "Playwright v1.42 supports the new feature X"
     Location: Section 5, paragraph 2
     Type: version_number + feature_claim
     Review by: [creation date + 60 days]
     What to check: current Playwright version + whether feature is still v1.42 specific
     Update action: update version reference; verify feature still exists

  ...
```

### Step 4 — Aggregate review schedule

Per content piece, output the next-review date as the EARLIEST of all element review-bys:

```
NEXT REVIEW DUE: [earliest review-by date — e.g., "2026-07-19 (90 days from publish, due to pricing element)"]
```

This means content owner knows: "by this date, I need to check at least this content."

### Step 5 — Optional: integrate with content portfolio (if user has nexus-strategy installed)

If `nexus-strategy` is installed, the decay map feeds into the content portfolio status report so user can see all content with upcoming reviews in one view.

If not installed, the decay map is delivered as part of the content output for user to manage manually.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: decay-mapping
Status: COMPLETE
Inputs consumed: content [N words], freshness-architecture output

DECAY MAP GENERATED:
  Total decay-sensitive elements: [N]
  Highest-risk elements (review ≤90 days): [N]
  Medium-risk elements (review 90-180 days): [N]
  Low-risk elements (review 180+ days): [N]

DETAILED ELEMENT MAP:
  1. Element: "[exact text from content]"
     Location: Section [X], paragraph [Y]
     Type: [type]
     Review by: [date]
     What to check: [specific verification action]
     Update action: [what to do if stale]

  2. ...

NEXT REVIEW DUE FOR THIS CONTENT: [earliest review-by date]

REVIEW REMINDER:
  Set a calendar reminder for [next review date].
  At that time, run: /nexus-audit on this URL with mode=fact_verification_only to check decay-sensitive elements.

DELIVERY FORMAT:
  - Decay map appended to content output as inline HTML comments (invisible to readers but parseable):
    <!-- DECAY: element="pricing" review_by="2026-07-19" -->
  - Decay map also delivered as separate JSON file for portfolio tracking (if user has nexus-strategy)

EVERGREEN STABILITY:
  This content's evergreen percentage: [X%] (from freshness-architecture)
  Decay velocity: [HIGH / MEDIUM / LOW] (based on element distribution)

CONFIDENCE: HIGH (full content analysis) / MEDIUM (limited topic context) / LOW (no topic velocity data)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Tag at creation, not after.** Decay-mapping happens at content generation time, not retroactively. The map is part of the deliverable.

2. **Element-level granularity.** Don't just tag sections — tag specific decay-sensitive elements within sections. A 5-section article might have 12 decay-sensitive elements with different review cadences.

3. **Aggressive on pricing and version numbers.** These decay fast and frequently. Default 60-90 day reviews.

4. **Conservative on conceptual content.** Don't over-tag stable explanations. Most paragraphs are not decay-sensitive.

5. **Integrate with portfolio if nexus-strategy installed.** Cross-skill data sharing (via files user owns) — doesn't create dependency, just enriches when both installed.

6. **Specific update actions, not vague.** Each element has explicit "what to check" and "what to do if stale" — content owner doesn't have to figure it out at review time.

7. **HTML comment annotations.** Invisible to readers, parseable by automation, persists in the content for future processing.
