# schema-generator.skill

**Role:** Generate complete, production-ready JSON-LD schema markup blocks for content. Article, BlogPosting, FAQPage, BreadcrumbList, HowTo, Review, Product, Organization. Output is ready to paste into the page's `<head>`.

**Loaded by:** `nexus-content` SKILL.md in all generation modes (mandatory final-stage step)

---

## Why this skill exists

Schema markup is a direct E-E-A-T and AEO signal. AI search surfaces use schema heavily for type detection and field extraction. The existing `metadata-generator` mentions schema preference but doesn't generate complete production-ready blocks.

This skill generates them. Every content piece ships with appropriate JSON-LD ready to paste.

---

## Inputs

Required:
- Generated content (markdown or HTML)
- Content type (Article / BlogPosting / Product / etc. — from brand.schema_preference or content-mode)
- Title, meta description, URL, publish date
- Brand profile (Organization name, logo URL, social profiles)
- Author profile (name, bio URL, image URL, role)

Optional:
- Update date (for content refresh)
- Image URLs (for hero image, article images)
- Breadcrumb path (if site uses breadcrumb navigation)
- Related schemas (e.g., HowTo if content is tutorial)

---

## Schema types to generate

### Article / BlogPosting (always — for content pieces)

```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "[title]",
  "description": "[meta description]",
  "image": [
    "[hero image URL]",
    "[additional image URLs]"
  ],
  "datePublished": "[ISO 8601]",
  "dateModified": "[ISO 8601 if updated, else same as publish]",
  "author": {
    "@type": "Person",
    "name": "[author name]",
    "url": "[author bio URL]",
    "image": "[author image URL]",
    "jobTitle": "[author role]",
    "sameAs": [
      "[social profile URLs]"
    ]
  },
  "publisher": {
    "@type": "Organization",
    "name": "[brand name]",
    "logo": {
      "@type": "ImageObject",
      "url": "[brand logo URL]"
    }
  },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "[full page URL]"
  }
}
```

### FAQPage (when content has FAQ section)

Detect: if content has H2 "FAQ" or "Frequently Asked Questions" or similar with H3-question-then-answer pattern.

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "[question text]",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "[answer text — strip markdown formatting]"
      }
    },
    [...for each FAQ pair]
  ]
}
```

### BreadcrumbList (always — improves SERP appearance)

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "[home URL]"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "[category]",
      "item": "[category URL]"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "[current page title]",
      "item": "[current page URL]"
    }
  ]
}
```

### HowTo (when content is tutorial)

Detect: if content type is `tutorial` mode OR if content has clear "Step 1", "Step 2" sequential structure.

```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "[tutorial title]",
  "description": "[meta description]",
  "totalTime": "[ISO 8601 duration if known, e.g., PT30M]",
  "supply": [
    {"@type": "HowToSupply", "name": "[item]"}
  ],
  "tool": [
    {"@type": "HowToTool", "name": "[tool name]"}
  ],
  "step": [
    {
      "@type": "HowToStep",
      "name": "[step name]",
      "text": "[step description]",
      "url": "[anchor URL to step section]",
      "image": "[step image URL if any]"
    },
    [...for each step]
  ]
}
```

### Review (when content reviews a product/tool)

Detect: comparison_post mode reviewing a single product, OR content with explicit ratings.

```json
{
  "@context": "https://schema.org",
  "@type": "Review",
  "itemReviewed": {
    "@type": "Product" or "SoftwareApplication" or appropriate type,
    "name": "[product name]"
  },
  "reviewRating": {
    "@type": "Rating",
    "ratingValue": "[X]",
    "bestRating": "5"
  },
  "author": {
    "@type": "Person",
    "name": "[author name]"
  },
  "reviewBody": "[review summary]"
}
```

### Product (for product landing pages)

Generate when landing_page mode + brand.brand_type = product.

### Organization (in publisher field of Article, also as standalone if About page)

Always include Organization schema as part of Article publisher field. Standalone Organization schema for About / Home pages.

---

## Process

### Step 1 — Detect required schema types

Based on:
- Content mode (standard_blog → Article; tutorial → Article + HowTo; comparison_post → Article + Review where applicable; landing_page → Article + Product where applicable; FAQ section present → + FAQPage)
- brand.schema_preference (if set, overrides auto-selection)
- Content content (e.g., FAQ section detected → add FAQPage)

### Step 2 — Extract required fields from content

Parse content + brand + author profiles to extract:
- Headline (from H1 or title)
- Description (from meta description)
- Images (from content + hero image)
- Dates (from publish/update inputs)
- Author info (from author profile)
- Publisher info (from brand profile)
- URL (from input)
- For HowTo: parse numbered steps from content
- For FAQPage: parse Q&A from FAQ section
- For BreadcrumbList: build from URL hierarchy or input

### Step 3 — Validate

For each generated schema block:
- Required fields per schema.org for that @type are populated
- URLs are absolute (https://...)
- Dates are ISO 8601
- Image URLs are absolute
- No empty arrays or null values for required fields

### Step 4 — Output ready-to-paste blocks

Each schema as separate `<script type="application/ld+json">` block, paste-ready in `<head>`.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: schema-generator
Status: COMPLETE
Inputs consumed: content [N words], brand, author, content mode

SCHEMA TYPES GENERATED:
  ✓ BlogPosting (or Article — based on content type)
  ✓ BreadcrumbList
  ✓ FAQPage (if FAQ section detected)
  ✓ HowTo (if tutorial mode)
  ✓ Review (if applicable)
  ✓ Product (if landing_page + product brand)
  ✓ Organization (in publisher field)

VALIDATION:
  All required fields populated: PASS
  All URLs absolute: PASS
  All dates ISO 8601: PASS
  All images have absolute URLs: PASS

GENERATED SCHEMA BLOCKS (paste into <head>):

<script type="application/ld+json">
[full Article/BlogPosting JSON-LD block]
</script>

<script type="application/ld+json">
[full BreadcrumbList JSON-LD block]
</script>

<script type="application/ld+json">
[full FAQPage JSON-LD block — if applicable]
</script>

[... additional schema blocks as applicable]

NOTES:
  - Schema preference from brand: [brand.schema_preference]
  - Auto-detected additional schemas: [list]
  - Hero image used: [URL]
  - Author image used: [URL or "fallback used: brand logo"]

CONFIDENCE: HIGH (all required fields present from inputs) / MEDIUM (some fields inferred) / LOW (missing required fields, schema may be incomplete)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Always generate the appropriate schemas.** Not optional — every piece of content ships with schema.

2. **Validate before output.** Empty/null required fields = fail. Don't ship invalid schema.

3. **Detect implicit schemas.** Tutorials get HowTo even if user didn't specify. Content with FAQ sections gets FAQPage even if user didn't specify.

4. **Brand.schema_preference overrides.** If brand explicitly says "use Article not BlogPosting", honor that.

5. **No fake fields.** If author image URL isn't available, don't fabricate one. Either use brand logo as fallback OR omit the image field. Never make up URLs.

6. **Absolute URLs only.** All URLs in schema must be absolute (https://...). Convert relative URLs using the page's base URL.
