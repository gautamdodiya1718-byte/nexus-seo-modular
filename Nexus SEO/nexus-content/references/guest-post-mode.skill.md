# guest-post-mode.skill

**Role:** Override default content generation rules for guest posts on external sites. Different brand-promotion logic, different anchor text rules, no CTAs, weighted toward host brand context.

**Loaded by:** `nexus-content` SKILL.md when user invokes guest_post mode

---

## Why this skill exists

Default content generation assumes you're publishing on your own brand's site. Guest posts have entirely different rules:
- Promoting your brand too much hurts the relationship with the host
- Anchor text rules change (one designated link, often with required text)
- CTAs aren't allowed (the host owns conversion)
- Tone needs to fit the host's editorial style, not yours

This skill makes guest-post-specific rules explicit and active.

---

## Inputs

Required:
- Topic
- Host site URL or name (the site you're guest-posting on)
- Anchor text (specific text the host wants to link)
- Anchor link target (specific URL the anchor links to)
- Host site editorial guidelines (if available — pasted from host's submission guidelines)

Optional:
- Host site's existing top content (for tone matching)
- Brand profile (your brand — used minimally)
- Author profile

---

## The guest post rule overrides

### Override 1 — Brand promotion rules

| Type of mention | Standard blog | Guest post |
|---|---|---|
| Host site mentions | N/A | ~30% of brand-related context — show familiarity with their brand |
| Your brand mentions | ~10-15% | ≤10% — minimal, only where genuinely relevant |
| Educational/informational | ~75% | ~60-65% — bulk of content |
| CTAs | 1-3 explicit CTAs | 0 CTAs — host owns conversion |

The "60% informational, 30% host promotion, 10% your brand" pattern is the default unless host specifies otherwise.

### Override 2 — Link rules

- **Designated anchor:** ONE link to a specific URL with specific anchor text (provided by user). This is the negotiated link.
- **Internal links to your brand:** Generally NOT allowed. Some hosts allow 1, but default to 0 unless explicitly negotiated.
- **External links (non-brand, non-host):** Allowed and encouraged for citations and credibility.
- **Host site internal links:** Encouraged where natural — link to relevant existing host content.

### Override 3 — Anti-promotion rule modification

Standard mode: 50% of content is purely educational, 50% can have brand mentions.
Guest post mode: 100% should feel educational. Your brand mentions should be incidental and value-adding, never promotional.

### Override 4 — Tone matching

Guest posts should sound like host's editorial voice, not yours:
- Read host's recent content (if URL provided) to match tone
- Match host's structural conventions (do they use callouts? Code blocks? Comparison tables?)
- Match host's formatting (sentence length, paragraph length, heading style)

If host's editorial guidelines are provided, honor them strictly. They override default Nexus style rules.

### Override 5 — Author byline rules

- Often the byline is YOU (your author profile)
- Author bio at end may include link back to your site (this is often the negotiated value of guest posting)
- Bio text should be short (50-100 words) and not promotional

### Override 6 — No infographic mandates

Standard mode: minimum 2 infographics. Guest posts: 0 unless host specifically requested.
Hosts typically have their own visual style and may not want SVG infographics in their content.

### Override 7 — Schema markup

Guest posts: no schema. Host owns the page and its schema. Don't generate JSON-LD blocks.

---

## Process

### Step 1 — Confirm guest post details

Verify all required inputs:
- Topic confirmed
- Host site identified
- Anchor text confirmed (exact text)
- Anchor link target confirmed (exact URL)
- Editorial guidelines reviewed (if provided)

If any are missing, ask before proceeding.

### Step 2 — Tone matching pass

If host site URL provided, web_search the host site to find recent content patterns:
- Sentence rhythm
- Paragraph length conventions
- Heading style
- Visual element usage
- Vocabulary tier

Calibrate generation to match.

### Step 3 — Generate with overrides

Run standard content generation pipeline BUT with these guest-post overrides applied:
- Brand mention frequency reduced
- Host brand context elevated
- Designated anchor placed naturally (not forced)
- No CTAs
- No infographics (unless requested)
- No schema markup
- Tone matched to host

### Step 4 — Quality checks

Standard quality checks still apply:
- Substance gates
- Live fact verification
- Live link verification (especially the anchor link)
- E-E-A-T enforcement
- Helpful Content compliance
- Differentiation enforcement

But disable:
- Internal linking checks (no internal links expected)
- CTA generation
- Infographic generation
- Schema generation

### Step 5 — Final delivery format

Guest posts often have specific submission formats:
- Markdown (most common)
- Google Docs with comments
- Plain text with notation

Output in markdown by default; let user request alternative format.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: guest-post-mode
Status: COMPLETE
Inputs consumed: topic, host site, anchor text, anchor link, [optional inputs]

GUEST POST PARAMETERS APPLIED:
  Host site: [name/URL]
  Designated anchor: "[text]" → [URL]
  Brand mention budget: ≤10% (typically 1-2 mentions)
  Host context: ~30% (showing familiarity with host)
  Educational content: ~60%
  CTAs: 0 (per guest post rules)
  Infographics: [0 / N if host requested]
  Schema markup: NONE
  Tone calibration: [matched to host's [recent article URL] / default if no host content provided]

GENERATION OVERRIDES APPLIED:
  ✓ Brand mention rules — guest post mode
  ✓ Link rules — designated anchor only
  ✓ Anti-promotion — 100% educational tone
  ✓ Tone matching — [matched / default]
  ✓ No CTAs
  ✓ No infographics (unless requested)
  ✓ No schema

QUALITY CHECKS RUN:
  ✓ Substance gates
  ✓ Live fact verification
  ✓ Live link verification (especially anchor)
  ✓ E-E-A-T enforcement
  ✓ Helpful Content compliance
  ✓ Differentiation enforcement

QUALITY CHECKS DISABLED (guest-post-mode):
  ✗ Internal linking (no internal links expected)
  ✗ CTA generation
  ✗ Infographic generation
  ✗ Schema generation

DELIVERY:
  Format: Markdown
  Word count: [N — informational]
  Designated anchor placed at: [Section X, paragraph Y]
  Author byline: included
  Author bio: included (50-100 words, link to your site)

CONFIDENCE: HIGH / MEDIUM / LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Host editorial guidelines override Nexus defaults.** If host says "no callouts" but Nexus defaults to callouts, follow host.

2. **Designated anchor is sacred.** Place it naturally but it MUST appear with the exact text and link target provided. No paraphrasing.

3. **Don't over-promote.** Easy mistake. The 10% brand mention budget is a ceiling, not a floor. Most guest posts should have 0-2 brand mentions max.

4. **Tone matching matters.** Generic Nexus voice in a host's editorial environment looks like spam. Match their style.

5. **No schema.** Host owns the page. Don't generate JSON-LD that conflicts with their setup.

6. **Author bio is the value.** The bio link is often the primary SEO value of guest posting. Make it count.
