# live-fact-verifier.skill

**Role:** Verify every factual claim in content against live web sources. Replaces "trust the model's training data" with "search the web and confirm." Catches hallucinated statistics, outdated version numbers, wrong feature claims, fabricated company names, and misattributed quotes.

**Loaded by:** `nexus-audit` SKILL.md in modes: full_audit, eeat_audit, fact_verification_only, llm_checklist_scoring
**Also loaded by:** `nexus-content` SKILL.md (per-claim, during generation)

---

## Why this skill exists

The single biggest source of AI content failure: confidently-stated facts that are wrong because the model assembled them from pattern memory rather than verified knowledge. A confidence-sounding claim like "Playwright is 30% faster than Cypress for cross-browser tests" might be entirely fabricated. Without live verification, the system has no way to know.

This skill systematically extracts every verifiable claim and checks each one.

---

## Inputs

Required:
- Content to verify (URL, file, or pasted text)

Optional:
- Brand profile (for brand-specific claim verification — features, pricing, etc.)
- Topic context (helps the verifier prioritize which claims need verification first)

---

## Claim extraction

Step 1: Parse the content and extract every claim that fits one of these categories:

### Category A — Numerical claims
- Statistics ("X% of users...", "the average is Y", "studies show Z%")
- Performance numbers ("X is N times faster than Y", "reduces by Z%")
- Counts ("over 1000 companies use X", "N teams have adopted")
- Dates and years ("released in 2024", "since 2019")
- Prices ("$X/month", "starts at $Y")
- Sizes ("N MB", "N GB", "N requests/second")

### Category B — Feature/capability claims
- "X tool supports Y feature"
- "X has built-in Y"
- "X integrates with Y"
- "X can do Y" (specific capability claims)

### Category C — Comparison claims
- "X is better than Y at Z"
- "X outperforms Y on Z metric"
- "X has more Z than Y"

### Category D — Existence claims
- "X company exists / was founded in Y"
- "X tool was released by Y"
- "X person said Y"
- "X organization recommends Y"

### Category E — Causal claims
- "X causes Y"
- "Y happens because of X"
- "X is the reason for Y"

### Category F — Attribution claims
- "According to X, Y"
- "X study found Y"
- "X reports that Y"

---

## Verification process

For each extracted claim:

### Step 1 — Determine verifiability tier

- **TIER 1: Highly verifiable** — single objective fact (price, version number, release date, official feature)
- **TIER 2: Moderately verifiable** — multi-source check needed (statistics, comparisons)
- **TIER 3: Low verifiability** — opinions, projections, subjective claims (mark as opinion, don't try to verify as fact)

### Step 2 — Web search verification (Tier 1 + 2 only)

For each Tier 1/2 claim:
- Search the web for the specific claim using web_search tool
- Look for primary sources first (official docs, vendor announcements, original research)
- Cross-check across multiple sources for statistical claims
- Compare against the claim in the content

### Step 3 — Classify result per claim

- **VERIFIED** — claim matches what current authoritative sources say
- **VERIFIED_OUTDATED** — claim was true at some point but is now outdated; provide current value
- **WRONG** — claim contradicts current authoritative sources; provide correct value if findable
- **UNVERIFIABLE** — couldn't find authoritative sources either way; do NOT mark as wrong, mark as unverifiable
- **OPINION** — Tier 3 claim that's stated as fact but is actually opinion; flag for rephrasing
- **NEEDS_SOURCE** — verifiable claim that's correct but lacks a citation; recommend adding source link

### Step 4 — Aggregate confidence

Overall confidence = function of:
- % VERIFIED + VERIFIED_OUTDATED (both are factually correct, latter just needs update)
- Number of WRONG claims (each WRONG claim drops confidence sharply)
- Number of UNVERIFIABLE claims (drops confidence moderately)

Compute claim accuracy %:
```
Claim accuracy = (VERIFIED + VERIFIED_OUTDATED) / (Total Tier 1 + 2 claims)
```

If claim accuracy <75%, this skill outputs a strong recommendation to confidence-gate to block delivery.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: live-fact-verifier
Status: COMPLETE / PARTIAL ([reason — e.g., "web_search unavailable for this claim"])
Inputs consumed: content [N words]

CLAIM VERIFICATION SUMMARY:
  Total claims extracted: [N]
    Tier 1 (highly verifiable): [N]
    Tier 2 (moderately): [N]
    Tier 3 (opinions): [N]

  VERIFIED:           [N]
  VERIFIED_OUTDATED:  [N]
  WRONG:              [N]
  UNVERIFIABLE:       [N]
  OPINION:            [N] (need rephrasing)
  NEEDS_SOURCE:       [N]

CLAIM ACCURACY: [%]

CONFIDENCE GATE RECOMMENDATION: PASS (>=75%) / BLOCK (<75%)

WRONG CLAIMS (highest priority — must fix):
  1. Claim: "[exact text from content]"
     Status: WRONG
     What's wrong: [specific issue]
     Correct value (if findable): [correction]
     Source: [URL]
     Fix: [exact rewrite]

  2. ...

OUTDATED CLAIMS (high priority — should update):
  1. Claim: "[exact text]"
     Status: VERIFIED_OUTDATED
     Was correct as of: [date if findable]
     Current value: [updated value]
     Source: [URL]
     Fix: [updated text]

  2. ...

UNVERIFIABLE CLAIMS (consider removing or marking as opinion):
  1. Claim: "[exact text]"
     Status: UNVERIFIABLE
     Searched: [what was searched]
     Recommendation: [remove / rephrase as opinion / find authoritative source]

  2. ...

OPINIONS STATED AS FACT (should be rephrased):
  1. Claim: "[exact text]"
     Recommendation: rephrase as opinion (e.g., "In my experience..." or "Many practitioners find...")
     Why: [why this is opinion not fact]

  2. ...

NEEDS SOURCE (claim is correct but unsourced):
  1. Claim: "[exact text]"
     Suggested source link: [URL]
     Add citation: [specific markdown for citation]

  2. ...

CONFIDENCE: HIGH (most claims web-search verified) / MEDIUM (some web_search calls failed) / LOW (significant gaps in verification)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Use web_search tool for every Tier 1 + 2 claim.** Don't rely on training data. Don't infer "what's probably true." Search.

2. **Cross-check statistical claims across multiple sources.** A single source citing a stat doesn't verify it. Need 2+ independent sources for VERIFIED status, or 1 primary source (the original research/study).

3. **Don't fabricate corrections.** If a claim is WRONG and the correct value isn't findable in the search, mark WRONG with "correct value not findable in current sources" — don't invent a "probably correct" value.

4. **Conservative on UNVERIFIABLE.** Better to mark UNVERIFIABLE than to declare WRONG without evidence. Some claims are real but obscure.

5. **Hard 75% confidence threshold.** This skill explicitly recommends confidence-gate to BLOCK delivery if claim accuracy is below 75%. Not a warning — an explicit BLOCK recommendation.

6. **Source links are mandatory in output.** Every VERIFIED, WRONG, OUTDATED claim should have the source URL where the verifier found the truth.

7. **Brand claim verification.** If brand profile is loaded and content makes claims about the brand's own features/pricing, verify against brand profile — flag any divergence (likely outdated content or wrong-brand confusion).
