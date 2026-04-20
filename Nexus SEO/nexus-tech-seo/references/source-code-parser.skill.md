# source-code-parser.skill

**Role:** Parse website source code (HTML, CSS, JS, .md exports, ZIP archives, or pasted content) and extract every SEO-relevant element into a structured deliverable that downstream skills can consume.

**Loaded by:** `nexus-tech-seo` SKILL.md in modes: full_audit, source_code_only, content_tech_bottleneck

---

## Inputs

Accept any of:
- File path(s) to .html, .css, .js, .md files
- File path to a ZIP containing site source
- Raw pasted HTML content
- A folder path containing the site source

If multiple files: parse each and tag elements with their source file.

## What to extract from HTML

For every page (or single page if only one provided):

### Document-level
- **DOCTYPE** declaration
- **HTML lang attribute** (e.g., `<html lang="en">`)
- **`<head>` contents** in full

### Title and meta
- `<title>` content + character count
- `<meta name="description">` content + character count
- `<meta name="keywords">` (flag as legacy if present)
- `<meta name="robots">` directives (index, noindex, follow, nofollow, noarchive, nosnippet, max-snippet, max-image-preview, max-video-preview)
- `<meta name="googlebot">` directives if separate
- `<meta name="viewport">` (mobile-friendly check)
- `<meta charset>`
- All other meta tags

### Canonical
- `<link rel="canonical">` href + whether self-referencing

### Hreflang
- All `<link rel="alternate" hreflang="X" href="Y">` tags
- Note: x-default presence
- Flag if any hreflang values look malformed

### Open Graph
- `og:title`, `og:description`, `og:image`, `og:url`, `og:type`, `og:site_name`
- Flag missing critical OG tags (title, description, image)

### Twitter Card
- `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`
- Flag missing critical Twitter tags

### Schema (JSON-LD)
- Find every `<script type="application/ld+json">` block
- Parse each block as JSON
- Identify `@type` per block (Article, Product, Organization, BreadcrumbList, FAQPage, HowTo, etc.)
- Validate basic structure (required fields present per schema.org for each type)
- Flag malformed JSON, missing required fields, type mismatches

### Headings
- Every `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, `<h6>` with its text content and source line/element location
- Count of each heading level
- Hierarchy validation (H2 should not appear before H1; H3 should not skip H2; etc.)
- H1 uniqueness check (should be exactly 1)

### Links
- All `<a href="...">` tags
- For each: href value, link text, rel attribute (nofollow, ugc, sponsored, etc.), target attribute
- Classify as internal (same domain) vs external
- Flag generic anchor text ("click here", "read more", "learn more")
- Flag links missing rel="nofollow" on external sponsored content (if detectable)

### Images
- Every `<img>` tag
- For each: src, alt text (or absence), width/height attributes (CLS prevention), loading attribute (lazy/eager), srcset (responsive)
- Flag missing alt text
- Flag missing width/height (CLS risk)
- Flag images that should probably be lazy-loaded but aren't

### Scripts
- All `<script>` tags
- For each: src (if external), async/defer attributes, type
- Flag render-blocking scripts in `<head>` without async/defer

### Stylesheets
- All `<link rel="stylesheet">` tags
- Flag render-blocking CSS in `<head>` (most CSS without media queries)

### Forms
- Forms present and their action targets
- Flag forms with action="" or non-HTTPS targets

## What to extract from CSS

- Total file size (per file + aggregated)
- Number of @import statements (each adds a request)
- @font-face declarations (font loading strategy)
- Media queries present (responsive design check)

## What to extract from JS

- Total bundle size (per file)
- Inline JS in HTML (size per page)
- Identify common heavy frameworks if obvious (jQuery, React bundle, Vue, etc.)

## What to extract from .md exports

If page exports are markdown (some users export pages from CMSs as .md):
- Parse front matter (YAML/TOML)
- Extract heading hierarchy
- Extract internal/external links
- Extract image references and alt text
- Note: many SEO elements (schema, OG, canonical) won't be in .md — flag this in confidence

## What to extract from ZIP archives

- Unzip to a temp location
- Process every file inside per the rules above
- Build a site-level map: which pages link to which, which images are used by which pages, etc.

---

## Output

Emit this output block:

```
[NEXUS-STEP-N OUTPUT]
Skill: source-code-parser
Status: COMPLETE
Inputs consumed: [file list]

PARSED ELEMENTS:
  Pages analyzed: [N]
  Total titles: [N] | Avg length: [chars] | Out-of-range (<30 or >60): [N]
  Total meta descriptions: [N] | Avg length: [chars] | Out-of-range: [N]
  H1 issues: [pages with 0 H1, pages with 2+ H1]
  Heading hierarchy issues: [list]
  Schema blocks: [count by @type]
  Schema validation: [pass / fail per block]
  Canonical issues: [list]
  Hreflang issues: [list]
  OG tags missing: [pages affected]
  Twitter tags missing: [pages affected]
  Images: [total] | Missing alt: [N] | Missing dimensions: [N]
  Generic anchor text instances: [N]
  Render-blocking scripts: [list]
  Render-blocking CSS: [list]
  Forms with HTTP target or empty action: [list]

EXTRACTED RAW DATA (for downstream skills):
  [structured JSON-like dump of all extracted elements per page]

Confidence: HIGH (if all source files parsed cleanly) / MEDIUM (if some parse errors) / LOW (if mostly .md without HTML metadata)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Don't run a real fetch.** This skill works only on provided source. If user provides only a URL with no source, emit SKIPPED and tell user to provide source code.

2. **Don't fabricate.** If a tag or attribute isn't in the source, mark it absent — don't infer what it "probably" says.

3. **Be specific in flags.** Not "missing meta description" — say "Page at [path/index.html] is missing meta description in `<head>`."

4. **Cross-page issues count.** If 12 pages have missing alt text on images, the flag is "12 pages affected" not "missing alt text" generic.

5. **Mark .md-only inputs as MEDIUM confidence.** Markdown exports lose `<head>` metadata — explicitly note this limits the audit.
