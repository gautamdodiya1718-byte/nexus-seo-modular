# live-link-verifier.skill

**Role:** Verify every internal and external link in generated content via HTTP HEAD requests before delivery. Catches broken links, redirected links (recommends updating to direct URLs), and links with wrong destinations.

**Loaded by:** `nexus-content` SKILL.md in all generation modes (mandatory before delivery)

---

## Why this skill exists

The old prompt asked Claude to "verify links are not 404" — but Claude has no way to actually verify URLs without making HTTP requests. Result: in practice, links are assumed-good and often fabricated or stale.

This skill makes verification real by performing HTTP HEAD on every link and reporting exact status.

---

## Inputs

Required:
- Generated content with all links present
- Brand profile (for the verified internal link inventory in `brand.urls`)

Optional:
- Custom timeout for HTTP requests (default 5 seconds per link)
- Whitelist of known-good link patterns (skip verification for explicitly trusted URLs to save time)

---

## Process

### Step 1 — Extract every link from content

Parse generated content (markdown or HTML) and extract:
- Every `[text](URL)` markdown link
- Every `<a href="URL">text</a>` HTML link
- Every bare URL pasted in content (e.g., `https://...` without anchor)

For each link, record:
- The URL
- The link text
- The position in content (which section, which paragraph)
- Whether internal (matches brand.site_url domain) or external

### Step 2 — Verify each link

For each link, make an HTTP HEAD request:

| Status | Meaning | Action |
|---|---|---|
| 200 | OK — link is good | KEEP |
| 301 / 302 / 307 / 308 | Redirect | Follow the redirect; recommend updating link text to the final destination URL |
| 404 | Not found | BROKEN — flag for replacement |
| 403 | Forbidden (may be a bot block, may be real) | Mark UNCERTAIN — try GET request as fallback |
| 410 | Gone — permanently removed | BROKEN — flag for replacement |
| 5xx | Server error | Mark UNCERTAIN — retry once after 5s; if still 5xx, flag as POSSIBLY_BROKEN |
| Timeout | No response within timeout | Mark UNCERTAIN — retry once with longer timeout |
| Connection error | DNS failure, certificate error, etc. | BROKEN — flag for replacement |

For redirects, check redirect chain depth:
- 1 redirect: fine, but recommend updating link
- 2+ redirects: flag as needs-fixing (slow, dilutes link equity)

### Step 3 — Cross-check internal links against brand.urls

For internal links (same domain as brand.site_url):
- Cross-reference with `brand.urls` repository
- If link is in brand.urls: confirmed valid (the brand owns it)
- If link is NOT in brand.urls: flag as NOT_IN_INVENTORY (potentially a hallucinated URL, even if HTTP 200 happens to work)

This catches the case where Claude invents an internal URL that happens to redirect to homepage or a 404 catchall.

### Step 4 — Replacement strategy

For BROKEN internal links:
- Search brand.urls for the closest semantic match using anchor text + context
- If close match found: auto-replace and log
- If no close match: flag with options for user (a) provide alternative URL, (b) remove link entirely, (c) restructure section to not need this link

For BROKEN external links:
- Try web search for the resource the link was pointing to (using anchor text)
- If found: suggest replacement URL
- If not found: flag for removal or restructuring
- Don't auto-replace external links without user confirmation (different sources have different credibility)

For NOT_IN_INVENTORY internal links:
- Auto-replace with the closest match from brand.urls based on anchor text + context
- If no good match exists: remove the link, keep the anchor text plain

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: live-link-verifier
Status: COMPLETE / PARTIAL ([reason — e.g., "HTTP requests unavailable, mark all as UNVERIFIED"])
Inputs consumed: generated content [N words], brand.urls inventory [M URLs]

LINK VERIFICATION SUMMARY:
  Total links checked: [N]
  Internal: [N] | External: [N]

  STATUS DISTRIBUTION:
    OK (200):                  [N]
    REDIRECT (1 hop):          [N — recommend updating]
    REDIRECT CHAIN (2+ hops):  [N — flag for fix]
    BROKEN (404/410/connection): [N]
    UNCERTAIN (403/5xx/timeout): [N]
    NOT IN INVENTORY (internal hallucinated): [N]

BROKEN INTERNAL LINKS:
  1. [URL] — [HTTP status / error]
     Anchor text: "[text]"
     Position: [section X, paragraph Y]
     Suggested replacement: [closest match from brand.urls based on anchor]
     Action: [AUTO_REPLACED / FLAGGED_FOR_REVIEW]

  2. ...

BROKEN EXTERNAL LINKS:
  1. [URL] — [HTTP status / error]
     Anchor text: "[text]"
     Position: [section X, paragraph Y]
     Suggested replacement (from web search): [URL or "not found"]
     Action: FLAGGED — user should review

  2. ...

NOT-IN-INVENTORY INTERNAL LINKS (potentially hallucinated):
  1. [URL] — Brand inventory does not contain this URL
     Anchor text: "[text]"
     Position: [section X]
     Closest brand.urls match: [URL]
     Action: AUTO_REPLACED

  2. ...

REDIRECTED LINKS (recommend updating to direct URLs):
  1. [URL] → [final URL]
     Action: link updated in content to direct URL

  2. ...

DELIVERY DECISION:
  - All BROKEN flagged links auto-replaced where possible: YES/NO
  - Remaining unresolved BROKEN links: [N]
  - If [N] > 0: content needs human review of those specific links before publication

CONFIDENCE: HIGH (all links verified) / MEDIUM (some UNCERTAIN) / LOW (HTTP requests unavailable)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Make actual HTTP requests.** Don't infer link validity from training data. HEAD requests, real responses.

2. **Brand.urls is authoritative for internal.** Even if a fabricated internal link returns HTTP 200 (because the site has a 404 catchall that returns 200), if it's not in brand.urls, treat as hallucinated.

3. **Don't auto-replace external links without confirmation.** External link credibility varies — replacing http://study.example.com/A with http://otherstudy.example.com/B changes what's being cited.

4. **Specific replacements based on context.** Use anchor text + surrounding paragraph context to find the closest brand.urls match — not just topic keywords.

5. **Conservative on UNCERTAIN.** Don't mark links as broken when they might just be temporarily down or bot-blocking. Retry once. If still uncertain, flag for human review.

6. **Block delivery if unresolved breaks remain.** If [N] > 0 unresolved BROKEN links, content shouldn't ship — feeds into confidence-gate.
