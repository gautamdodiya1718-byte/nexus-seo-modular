# live-serp-verifier.skill

**Role:** Verify every SERP-related claim made by upstream skills (serp-intelligence, deep-serp-analysis, content-pattern-extractor, etc.) against current live SERPs via web search. Catches SERP fabrication: claims about "the top result has X" when no actual SERP fetch happened.

**Loaded by:** `nexus-research` SKILL.md in every mode that touches SERPs

---

## Why this skill exists

Without live SERP verification, upstream skills can confidently report SERP intelligence based on:
- Stale training data (positions and content from months/years ago)
- Pattern memory ("results for this kind of keyword usually look like X")
- Plausible-sounding but fabricated specifics ("the top result is by HubSpot at position 1...")

This skill fetches current SERP data via web_search and cross-checks every specific SERP claim made upstream.

---

## Inputs

Required:
- Output from upstream SERP-related skills (serp-intelligence, deep-serp-analysis, content-pattern-extractor, gap-opportunity-engine, etc.)
- Target keyword(s)

Optional:
- Geo target (defaults to US)
- Search engine (defaults to Google)

---

## Process

### Step 1 — Extract SERP claims from upstream output

Parse upstream skills' output blocks and extract every specific SERP claim:
- Specific URLs reportedly in top N
- Specific titles of top results
- Specific page features (PAA boxes, video carousel, featured snippet, knowledge panel)
- Position counts ("X of top 10 are listicles")
- Date ranges or freshness claims
- Domain authority observations
- Specific PAA questions
- Specific autocomplete suggestions
- Specific competitor URL positions

### Step 2 — Sample-verify via web search

For high-impact claims (top 5 positions, primary SERP features), do a fresh web_search for the keyword and compare:

**For positional claims:**
- Verify each claimed top-N URL is actually in top N now
- Note any that have moved (with new position) or dropped
- Note any URLs in current top N that weren't claimed (new entrants)

**For SERP feature claims:**
- Confirm featured snippet exists / doesn't exist
- Confirm PAA box presence
- Confirm video carousel, image pack, knowledge panel, etc.

**For PAA claims:**
- Sample 3-5 PAA questions from the upstream output
- Search and verify they actually appear in current PAA

**For freshness claims:**
- Spot-check publish dates of claimed top results
- Flag if upstream claimed "freshness matters here" but top results are 2+ years old (or vice versa)

### Step 3 — Classify each claim

- **VERIFIED** — claim matches current SERP
- **VERIFIED_OUTDATED** — claim was true at some point but has moved (e.g., URL was position 3, now position 7)
- **WRONG** — claim contradicts current SERP (e.g., claimed URL not present at all)
- **UNVERIFIABLE** — couldn't fetch SERP for this query (web_search unavailable or rate-limited)

### Step 4 — Compute SERP verification confidence

```
SERP confidence = (VERIFIED + VERIFIED_OUTDATED) / (Total verifiable claims)
```

If <80%, the upstream SERP intelligence is unreliable and the entire research package's confidence should drop.

### Step 5 — Provide corrections

For VERIFIED_OUTDATED and WRONG claims, output the corrected current information so downstream skills (content-research-engine, serp-blueprint-generator, etc.) can use accurate data.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: live-serp-verifier
Status: COMPLETE / PARTIAL ([reason — e.g., "web_search unavailable"])
Inputs consumed: upstream SERP-related outputs + target keyword

SERP CLAIM VERIFICATION:
  Total claims extracted: [N]
  VERIFIED:           [N]
  VERIFIED_OUTDATED:  [N]
  WRONG:              [N]
  UNVERIFIABLE:       [N]

SERP VERIFICATION CONFIDENCE: [%]

WRONG CLAIMS (must be replaced before research package ships):
  1. Upstream claim: "Top result for keyword 'X' is [URL] at position 1"
     Current state: [URL] is not in top 10. Actual position 1 is [URL].
     Correction: replace upstream output with current top 10.

  2. ...

OUTDATED CLAIMS (positions/details have shifted):
  1. Upstream claim: "[URL] is at position 3"
     Current position: position 7
     Correction: update position to 7 in downstream consumption.

  2. ...

UNVERIFIABLE CLAIMS (couldn't web_search to verify):
  1. Claim: "[X]"
     Reason: [web_search unavailable / rate-limited / topic too specific]
     Recommendation: mark research package as MEDIUM confidence, flag this claim for manual verification.

  2. ...

CURRENT SERP SNAPSHOT (for keyword "[X]"):
  Top 10 (current):
    1. [URL] — [title] — [meta if visible]
    2. ...
    10. ...

  SERP features detected:
    [list — featured snippet, PAA, video carousel, etc.]

  PAA questions (current — sample of 5):
    1. [question]
    2. ...

DOWNSTREAM IMPACT:
  Skills that should re-process based on corrected SERP data:
    - serp-blueprint-generator (if any top-10 URLs changed)
    - gap-opportunity-engine (if top-10 content changed)
    - competitor-analysis (if top competitors changed)
    - content-pattern-extractor (if dominant patterns changed)

CONFIDENCE: HIGH (most claims verified via web_search) / MEDIUM (partial verification) / LOW (web_search mostly unavailable)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Use web_search for every high-impact claim.** Don't trust upstream skills' SERP claims based on training data alone. Especially position claims, URL claims, SERP feature claims.

2. **Sample-verify, don't exhaustively verify.** For deep-serp-analysis with 10 URLs each having 8-dimension analysis = 80+ claims per keyword. Sample the highest-impact ones (top 5 positions, primary SERP features, top 5 PAA questions).

3. **Don't fabricate corrections.** If a claim is WRONG and the actual value can't be fetched, mark UNVERIFIABLE — don't invent a "probably correct" replacement.

4. **Provide downstream impact analysis.** When claims are wrong/outdated, downstream skills that consumed those claims may need to re-process. Make this explicit.

5. **Conservative on confidence.** When web_search is unavailable for many claims, drop confidence to MEDIUM or LOW. Don't claim HIGH confidence based on partial verification.

6. **Handle rate limits gracefully.** If web_search hits rate limits, do what's possible, mark the rest UNVERIFIABLE with reason, and recommend re-running later.
