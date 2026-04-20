# on-page-tech-checker.skill

**Role:** Audit on-page technical foundation: titles, metas, headings, schema validity, image accessibility, link structure, OG/Twitter cards. Operates on the structured output of source-code-parser.

**Loaded by:** `nexus-tech-seo` SKILL.md in modes: full_audit, source_code_only, content_tech_bottleneck

---

## Inputs

Required:
- Source code parser output (extracted elements per page)

Optional:
- Brand profile (for brand voice / link inventory cross-checks)
- Target keyword(s) per page (for keyword placement analysis)

---

## Checks

### Title tag

Per page:
1. Length: 30–60 characters ideal. Flag <30 (likely too thin) or >60 (likely truncated in SERPs).
2. Uniqueness across pages: every title should be unique. Flag duplicates.
3. Brand suffix consistency: if brand profile provided, check if titles end with " | Brand" or similar — flag inconsistency.
4. Keyword presence (if target keyword provided per page): keyword should appear in title, ideally in first half.
5. Pipe vs dash separators: note convention used; recommend consistency.
6. ALL CAPS or excessive special chars: flag as poor UX in SERPs.

### Meta description

Per page:
1. Presence: every page should have one. Flag absent.
2. Length: 120–160 characters ideal. Flag <120 (likely thin) or >160 (likely truncated).
3. Uniqueness across pages: every description should be unique. Flag duplicates.
4. CTR-friendliness: should include action verb, value proposition, or specific number. Flag generic descriptions ("learn more about X").
5. Keyword presence: target keyword should appear naturally.

### Heading hierarchy

Per page:
1. **Exactly 1 H1** required. Flag 0 or 2+.
2. H1 should be unique per page (not the brand name across all pages).
3. **No skipped levels** — H1 → H2 → H3 (not H1 → H3).
4. **No more than 1 H1**, but multiple H2/H3 expected.
5. **H1 length** — under 70 characters ideal for readability.
6. **Heading text quality** — flag generic headings ("Introduction", "Overview", "Conclusion" without specificity).

### Schema markup (JSON-LD)

For every schema block found by source-code-parser:

1. **Validity** — block must be valid JSON.
2. **Required fields per @type:**
   - `Article` / `BlogPosting`: headline, image, datePublished, author, publisher
   - `Product`: name, image, description, offers (with price + priceCurrency + availability)
   - `Organization`: name, url, logo
   - `BreadcrumbList`: itemListElement with position + name + item
   - `FAQPage`: mainEntity with Question name + acceptedAnswer text
   - `HowTo`: name, step (each with name + text)
   - `Review`: itemReviewed, reviewRating, author
3. **Field consistency** — schema headline matches `<title>` (or close variant); image URLs are absolute; dates are ISO 8601.
4. **Schema type appropriateness** — FAQPage on a page with no FAQ section is misleading and may be penalized. Flag mismatches.
5. **Multiple schema blocks** — note if multiple types on one page (e.g., Article + BreadcrumbList + FAQPage is fine; Article + Product on same page is suspicious).

### Image accessibility

For every image found by source-code-parser:

1. **Alt text presence** — every meaningful image should have alt. Decorative images can have alt="" but the attribute should still be present.
2. **Alt text quality** — flag stuffed alt ("keyword keyword keyword"), useless alt ("image", "img1.jpg"), or generic ("photo").
3. **Width/height attributes** — required to prevent CLS. Flag absent.
4. **Loading attribute** — recommend `loading="lazy"` for below-fold images. Flag if all images use eager loading.
5. **Srcset for responsive** — flag large hero images without srcset (mobile users get desktop-sized assets).
6. **File size hints** — if file paths suggest large unoptimized images (e.g., `.png` for photos, large dimensions), recommend WebP/AVIF.

### Link structure

For every link found by source-code-parser:

1. **Generic anchor text** — flag "click here", "read more", "learn more", "this article", "here". Recommend descriptive text.
2. **Internal vs external** — count and report distribution.
3. **External link rel attributes** — external links should have `rel="nofollow"` (or `rel="ugc"` / `rel="sponsored"`). Flag missing.
4. **Internal link depth** — if navigation is provided, check that important pages are within 3 clicks of homepage. Flag pages 4+ clicks deep (orphaned-ish).
5. **Broken link risk** — flag `href="#"` placeholder links, `href="javascript:void(0)"`, and obvious typos.

### Open Graph + Twitter Card

Per page:
1. **OG required tags:** og:title, og:description, og:image, og:url, og:type — flag missing.
2. **OG image** — should be 1200x630 ideal. If dimensions provided in tag and don't match, flag.
3. **Twitter card type** — `summary` or `summary_large_image` — if absent, recommend `summary_large_image` for content pages.
4. **Twitter description length** — under 200 chars.

### Mobile-friendliness signals

1. **Viewport meta** — should be `<meta name="viewport" content="width=device-width, initial-scale=1">`. Flag missing or restrictive (e.g., `user-scalable=no` blocks pinch-zoom — accessibility issue).
2. **Tap target spacing** — only inferable from CSS (button/link min-height 48px) — note if obvious issues.
3. **Mobile-specific CSS** — at least one `@media (max-width:...)` query expected for responsive design. Flag if absent.

### Accessibility basics

1. **HTML lang attribute** — should be set. Flag absent.
2. **Skip-to-content link** — common accessibility pattern; note if absent.
3. **Form labels** — every input should have a `<label>`. Flag inputs without.
4. **Color contrast** — only inferable from CSS analysis; note if extreme issues obvious (e.g., light gray text on white background in inline style).

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: on-page-tech-checker
Status: COMPLETE
Inputs consumed: source-code-parser output for [N] pages

ON-PAGE TECH ISSUES BY SEVERITY:

CRITICAL:
  - [pages with 0 or 2+ H1] — affects [N] pages
    Fix: [specific HTML change]

HIGH:
  - [missing meta description on N pages]
  - [malformed schema blocks on N pages]
  - [generic anchor text on N links across N pages]
  - ...

MEDIUM:
  - [titles outside ideal length range]
  - [images missing dimensions — CLS risk]
  - ...

LOW:
  - [missing twitter card type]
  - [non-descriptive alt text]
  - ...

PER-PAGE BREAKDOWN (top 10 issues per page):
  Page [path]:
    - [issue 1] | Fix: [action]
    - [issue 2] | Fix: [action]

KEYWORD PLACEMENT (if target keywords provided):
  Page [path] | Keyword "X":
    - In title: YES/NO
    - In meta description: YES/NO
    - In H1: YES/NO
    - In first 100 words: YES/NO (if body content available)

CONFIDENCE: HIGH (if source-code-parser had complete data) / MEDIUM (if partial)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Specific to source.** Reference the actual file path / line where the issue exists.
2. **Group by severity, not by check type.** Critical issues across all checks come first.
3. **Fix instructions are real HTML.** Show the actual tag to add/change, not "fix the title."
4. **No fabrication.** If a check requires data the parser didn't extract (e.g., body content for keyword density), mark that check SKIPPED.
