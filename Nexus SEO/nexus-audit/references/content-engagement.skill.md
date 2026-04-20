# SKILL: content/content-engagement.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Editorial Quality
# EXECUTION PRIORITY: POST-DRAFT — Runs after content-generation-engine produces a draft. Audits pacing and inserts visual/interactive break points.
# POSITION: Editorial pacing engine that prevents content fatigue. Scans the draft for text-heavy stretches and inserts engagement elements (code examples, infographics, videos, callout boxes, real-world examples) at optimal intervals.
# DEPENDENCIES: code-generation-preview.skill, infographic-image-engine.skill, real-world-examples.skill, brand-[name].skill.md (fields: mention_frequency, mention_style, features)

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill prevents readers from bouncing due to text fatigue. It scans content for consecutive text-only sections and inserts engagement elements that keep readers scrolling to the end — improving dwell time, reducing bounce rate, and signaling content quality to search engines.

### The Core Rule
> **No more than 4 consecutive paragraphs (~300 words) without a visual or interactive element.**

After 300 words of unbroken text, the skill flags an insertion point and selects the most appropriate engagement element.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — ENGAGEMENT ELEMENT PRIORITY
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

When a break is needed, select the element type based on context:

| Priority | Element | When to Use | Skill That Produces It |
|---|---|---|---|
| 1 | **Code example + screenshot** | Section is technical, explains a how-to | `code-generation-preview.skill` |
| 2 | **Infographic/diagram** | Section explains a concept, flow, or comparison | `infographic-image-engine.skill` |
| 3 | **Real-world example callout** | Section makes a claim that needs evidence | `real-world-examples.skill` |
| 4 | **YouTube video embed** | Relevant video exists on approved channels | `infographic-image-engine.skill` (video section) |
| 5 | **Highlighted callout box** | Lightweight break, tip/warning/insight | WordPress shortcode |
| 6 | **Data table or comparison** | Section compares options or presents data | Inline HTML table |
| 7 | **GIF suggestion** | Section describes a dynamic interaction | Ask user |

### Selection Logic
```
Is the section explaining HOW to do something technically?
  → YES → Priority 1: Code example + screenshot
  → NO ↓

Is the section explaining a CONCEPT, FLOW, or COMPARISON?
  → YES → Priority 2: Infographic/diagram
  → NO ↓

Is the section making a CLAIM that needs EVIDENCE?
  → YES → Priority 3: Real-world example callout
  → NO ↓

Does a RELEVANT VIDEO exist on approved YouTube channels?
  → YES → Priority 4: YouTube embed
  → NO ↓

Is this a quick TIP, WARNING, or KEY INSIGHT?
  → YES → Priority 5: Callout box
  → NO ↓

Does the section present DATA or OPTIONS?
  → YES → Priority 6: Data table
  → NO → Priority 7: Suggest GIF to user
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — CALLOUT BOX TYPES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WordPress shortcode callout boxes available:

| Type | Color | Use Case | Example |
|---|---|---|---|
| **Tip** | Green accent | Best practice, efficiency gain, pro tip | "Tip: Use getByRole over CSS selectors for stable tests" |
| **Warning** | Amber accent | Common mistake, gotcha, deprecation | "Warning: page.waitForTimeout causes flaky tests" |
| **Key Insight** | Blue accent | Important concept, industry data, expert quote | "Key Insight: 73% of flaky tests stem from timing issues" |
| **Brand Feature** | Brand accent (from brand.infographic_style or default) | Product feature that solves the discussed problem | "[brand.brand_name]'s [feature name from brand.features] does [specific action]" |

### Callout Box Rules
- Maximum 3 callout boxes per article section (between H2s)
- Maximum 8 callout boxes per article total
- Never place 2 callout boxes back-to-back — minimum 1 paragraph of text between them
- Keep callout text to 1-3 sentences
- Brand Feature callouts read from brand.features (name + description fields).
  Check maturity label before inserting. Never insert COMING_SOON or UNRELIABLE features.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — EXECUTION STEPS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Step 1: Content Scan
Parse the draft and measure:
- Total word count
- Word count between each visual element (or from start to first visual)
- Identify all text-only stretches exceeding 300 words

### Step 2: Insertion Point Identification
For each text-only stretch exceeding 300 words:
1. Identify the natural break point (between paragraphs, ideally at a topic transition)
2. Determine the section's context (technical how-to, conceptual, data-driven, etc.)
3. Apply the Selection Logic from Section 2 to choose the element type

### Step 3: Element Generation
Dispatch to the appropriate skill:
- Code example → `code-generation-preview.skill`
- Infographic → `infographic-image-engine.skill`
- Real-world example → `real-world-examples.skill`
- Video → `infographic-image-engine.skill` (YouTube section)
- Callout box → Generate inline using WordPress shortcode format
- Table → Generate inline HTML table
- GIF → Flag for user: "Suggest: Record a GIF showing [X]"

### Step 4: Engagement Score
After inserting all elements, calculate the engagement score:

| Metric | Target | Scoring |
|---|---|---|
| Visual-to-text ratio | 1 visual per 250-350 words | 100 if met, -10 per section outside range |
| Content composition | 60-70% text / 30-40% visual-interactive | 100 if met, -15 per 5% deviation |
| Max consecutive text | Under 300 words | 100 if met, -20 per violation |
| Element variety | Minimum 3 different element types per article | 100 if met, -10 per missing type |
| Callout density | Under 8 per article | 100 if met, -5 per excess |

**Target: 100/100.** Report any deductions with specific fix recommendations.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — INTEGRATION WITH CONTENT PIPELINE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Pipeline Position
```
content-generation-engine → [THIS SKILL] → infographic/code/examples skills → humanizer
```

### Input
- Full draft text from content-generation-engine
- Content research brief (for context on what media competitors use)

### Output
- Annotated draft with insertion markers: `[INSERT: code_example | context: "demonstrate getByRole for login"]`
- Engagement score report
- List of all elements to generate (dispatched to respective skills)

---

*Content Engagement Optimization — v1.0.0 | Nexus SEO Operating System*
