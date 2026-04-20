# crawlability-checker.skill

**Role:** Diagnose crawlability and indexability issues. Check robots.txt, sitemap.xml, canonical tags, noindex/nofollow misuse, hreflang errors, and redirect chains.

**Loaded by:** `nexus-tech-seo` SKILL.md in modes: full_audit, source_code_only, ranking_diagnosis

---

## Inputs

Accepts any of:
- robots.txt (file path or pasted content)
- sitemap.xml (file path or pasted content)
- Source code parser output (which contains canonical, hreflang, robots meta tags from each page)
- URL of the site (to reference in recommendations only — does not fetch)

If only some inputs provided: run what's possible, mark others SKIPPED.

---

## Checks

### robots.txt analysis

If robots.txt is provided, check for:

1. **Critical paths blocked unintentionally**
   - Disallow rules that block important content directories (e.g., `/blog/`, `/products/`, `/`)
   - Flag any Disallow that affects the homepage or key landing pages

2. **Wildcard misuse**
   - `Disallow: /*` (blocks everything — flag as critical)
   - Wildcards that are too broad (e.g., `/*?*` blocking all parameterized URLs may block legitimate pages)

3. **Blocked CSS/JS**
   - Modern Google needs CSS/JS to render. If robots.txt blocks `*.css`, `*.js`, or directories containing them — flag as critical

4. **AI crawler blocks**
   - Check for User-agent entries for: GPTBot, ChatGPT-User, Google-Extended, OAI-SearchBot, PerplexityBot, ClaudeBot, anthropic-ai, CCBot, Applebot-Extended, FacebookBot, ImagesiftBot, Bytespider, Diffbot, Omgili
   - Note which AI crawlers are blocked and which are allowed
   - If GEO/AI search optimization matters to user, flag blocked AI crawlers as a strategic issue

5. **Sitemap directive**
   - Is `Sitemap:` directive present?
   - If yes, does it point to a valid URL?
   - If absent — recommend adding

6. **Crawl-delay**
   - If present, flag (Google ignores it; for other crawlers, may slow indexing)

7. **Conflicting rules**
   - Allow + Disallow on overlapping paths — note ambiguity

### sitemap.xml analysis

If sitemap.xml is provided, check for:

1. **Validity**
   - Valid XML
   - Conforms to sitemap protocol (urlset namespace, loc + lastmod + changefreq + priority structure)

2. **Size**
   - Under 50,000 URLs per sitemap (split required if over)
   - Under 50MB uncompressed
   - If over either — flag and recommend sitemap index

3. **Critical pages present**
   - If user provided source code, cross-check: are all pages in source represented in sitemap?
   - Flag pages in source missing from sitemap

4. **Stale lastmod**
   - Any lastmod older than 1 year on pages that should be active — flag as stale

5. **noindex pages in sitemap**
   - Cross-check: any URL in sitemap that has noindex meta in source — flag as conflict

6. **HTTPS consistency**
   - All URLs use https://, not http://

7. **Trailing slash consistency**
   - URLs in sitemap use same trailing-slash convention as canonical tags in source

### Canonical tag analysis (from source-code-parser output)

For every page:

1. **Self-canonical correctness**
   - Canonical URL matches the page URL (after normalization)
   - Flag mismatches

2. **Cross-canonical**
   - Canonical points to a different page — note this and flag if unintentional (most pages should self-canonical)

3. **Missing canonical**
   - Pages without canonical tag — flag

4. **Multiple canonical tags**
   - More than one `<link rel="canonical">` on a single page — flag as critical (Google may pick wrong one)

5. **Canonical to noindex page**
   - Canonical destination is itself noindex — flag as critical

### Hreflang analysis (from source-code-parser output)

For multilingual sites:

1. **Return tags missing**
   - If page A has hreflang to page B, page B should have hreflang back to page A — flag missing returns

2. **Invalid language codes**
   - Use ISO 639-1 (e.g., `en`, `es`, `fr-CA`) — flag malformed codes

3. **x-default presence**
   - Recommend x-default for international sites without one

4. **Self-referencing hreflang**
   - Each page should hreflang-reference itself in addition to its alternates

### Robots meta analysis (from source-code-parser output)

1. **noindex on pages that should be indexed**
   - If user provided a list of "important pages" (e.g., from sitemap), cross-check
   - Flag any important page with noindex

2. **Conflicts with canonical**
   - If page has noindex but is canonical-linked from other pages — flag

3. **noarchive misuse**
   - noarchive is fine for time-sensitive content but unnecessary on most pages — note if widespread

### Redirect chain analysis

If user provided redirect data (or HTTP responses), check:

1. **3+ hop chains** — flag (slows crawl, dilutes link equity)

2. **Loops** — flag as critical (page never resolves)

3. **HTTP→HTTPS chains** — recommend updating internal links to direct HTTPS URLs

4. **Trailing slash redirects** — recommend canonicalizing internal links

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: crawlability-checker
Status: COMPLETE / PARTIAL ([reason — e.g., "no robots.txt provided"])
Inputs consumed: [list]

CRAWLABILITY ISSUES BY SEVERITY:

CRITICAL:
  - [issue]
    Impact: [what breaks]
    Fix: [specific action]

HIGH:
  - [issue]
    Impact: [what limits]
    Fix: [specific action]

MEDIUM/LOW:
  - [issue]
    Fix: [specific action]

AI CRAWLER ACCESS STATUS:
  Allowed: [list]
  Blocked: [list]
  Strategic note: [if any major AI crawler blocked]

SITEMAP HEALTH:
  Status: [VALID / INVALID / MISSING]
  URLs: [N] | Stale lastmod: [N] | noindex conflicts: [N]
  HTTPS consistency: [PASS / FAIL]

CANONICAL HEALTH:
  Pages with self-canonical: [N]
  Pages with cross-canonical: [N]
  Pages missing canonical: [N]
  Pages with multiple canonicals: [N — CRITICAL]

HREFLANG HEALTH (if multilingual):
  Pages with hreflang: [N]
  Missing return tags: [list pairs]
  Malformed codes: [list]
  Missing x-default: [yes/no]

CONFIDENCE: HIGH/MEDIUM/LOW
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Don't crawl.** This skill never fetches URLs. It works only on provided files.
2. **Cross-reference inputs.** If both source and sitemap are provided, cross-check them. If only one, do what's possible.
3. **Specific severity.** robots.txt blocking the homepage is CRITICAL. A missing x-default is MEDIUM.
4. **Plain language fixes.** "Add `<link rel='canonical' href='https://yoursite.com/page'>` to the `<head>` of /page.html" — not "add canonical."
