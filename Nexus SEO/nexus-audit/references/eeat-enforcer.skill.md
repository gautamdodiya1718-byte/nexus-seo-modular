# eeat-enforcer.skill

**Role:** Formalize Google's E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) framework as scored, enforceable checks. Goes beyond qualitative "demonstrates expertise" to specific structural signals that prove first-hand experience and not just AI-assembled summary.

**Loaded by:** `nexus-audit` SKILL.md in modes: full_audit, eeat_audit, competitor_compare, helpful_content_check
**Also loaded by:** `nexus-content` SKILL.md as a quality gate during generation

---

## Why this skill exists

Google's E-E-A-T framework is the single biggest quality lens in search ranking, especially after the Helpful Content Update. Most "expert-sounding" AI content fails E-E-A-T because:
- It demonstrates Expertise (technical accuracy) but lacks Experience (first-hand encounter)
- It cites authoritative sources but doesn't make authoritative claims of its own
- It writes confidently but trust signals (disclosure, dates, author bio) are missing

This skill checks for the *structural signals* of E-E-A-T, not just the surface tone.

---

## Inputs

Required:
- Content to evaluate (URL, file, or pasted text)
- Author profile (if available — significantly improves accuracy)

Optional:
- Brand profile (for trust signal cross-checks like author bio, disclosure standards)
- Topic the content covers (for credibility check against author's stated expertise areas)

---

## The 4 dimensions and their checks

### Dimension 1 — Experience (first-hand encounter)

For BOFU/MOFU content, content needs to demonstrate that someone with hands-on experience wrote it. Check:

1. **First-person experience markers per H2 section**
   - Look for: "When I tested...", "We saw...", "In my experience...", "We deployed... and noticed...", "I've shipped this for..."
   - Required: at least 1 first-person experience marker per H2 in BOFU content; at least 50% of H2s in MOFU
   - Score: pass if hit, fail if absent

2. **Practitioner-only scenarios**
   - Specific scenarios that only someone who's done the thing would know about
   - Example for testing content: "Debugging a flaky test that turned out to be a CSS animation triggering on slow CI runners" — too specific to be generated without practitioner context
   - Required: at least 1 practitioner-only scenario in any content claiming hands-on knowledge
   - Score: pass if at least 1, fail if 0
   - Cross-check with author profile's "personal experience markers" — if author claims it but content has none, flag as inauthentic

3. **Specific (not generic) examples**
   - "TestDino reduced our test suite from 18 minutes to 6 minutes after migrating to parallel workers" — specific
   - "Many companies reduce test runtime with parallel execution" — generic
   - Required: ≥3 specific examples in any 1500+ word content; flag generic examples for replacement

4. **Numeric specificity in claims**
   - "Reduced bundle size by 47%" — specific
   - "Significantly reduced bundle size" — generic
   - Required: ≥80% of quantitative claims should have specific numbers; generic quantifiers flagged

### Dimension 2 — Expertise (deep subject knowledge)

1. **Technical accuracy and current state**
   - Cross-check claims against current state of tools/APIs (delegated to live-fact-verifier)
   - Pass if all technical claims verified current; fail if any outdated/wrong claims

2. **Nuance acknowledgment**
   - Does the content acknowledge edge cases, exceptions, or "it depends" scenarios?
   - Pure surface-level content treats topics as binary
   - Required: ≥2 nuance acknowledgments per 1500 words

3. **Advanced concepts explained with authority**
   - Does the content go beyond the basics?
   - Score: count of advanced concepts addressed vs surface-only treatment
   - Pass if content goes meaningfully beyond top-3-SERP-result depth; fail if it stays at "what a quick search would tell you"

4. **Industry-specific terminology used correctly**
   - Cross-check terminology against author profile's vocabulary tier
   - Flag misuse (e.g., "API" used to mean "endpoint" when they're different concepts)

### Dimension 3 — Authoritativeness (citations + claims)

1. **External citations to primary sources**
   - Required: ≥3 external links to primary sources per 1500 words
   - Primary sources = official docs, research papers, vendor announcements, original studies — not blog posts that cite other blogs
   - Flag: links only to other blogs, no primary sources

2. **Opinion claims with reasoning**
   - "X is better than Y" claims should be backed by stated reasoning, not just asserted
   - Required: every opinion claim has a "because [reason]" or comparable reasoning
   - Score: % of opinion claims with reasoning

3. **Original positions, not aggregated summary**
   - Does the content make any claim that's not just a summary of other content?
   - This is hard to detect automatically — proxy: contrarian positions, specific predictions, original examples
   - Score: count of "original" markers vs "summary" content

4. **Author credibility match**
   - If author profile is loaded: cross-check that the content's topic is in the author's "credible to write about" list
   - Flag if topic is outside author's stated expertise

### Dimension 4 — Trustworthiness (transparency + accuracy)

1. **Author byline + bio link**
   - Required: visible author byline with link to author bio (or About page)
   - Score: pass if present, fail if absent
   - Flag missing byline as critical for YMYL content

2. **Publish/update dates visible**
   - Required: visible publish date, update date if updated
   - Score: pass if both visible (update date especially important for evolving topics)

3. **Source citations linked, not just named**
   - "According to a Google study" → flag (no link)
   - "According to [Google's 2024 study](https://...)" → pass
   - Score: % of source citations with actual links

4. **Disclosures present where applicable**
   - If author profile lists disclosures, verify they appear in content where relevant
   - If brand profile is the brand being reviewed, flag absence of disclosure
   - Score: pass if applicable disclosures present, fail if missing

5. **Conservative on speculation**
   - Speculation about future products, performance projections, or unverified claims should be marked as such
   - Flag: confident-sounding statements about uncertain future events

6. **No unverifiable specific claims**
   - "Companies report 35% faster deployment with X" without linked source — flag as unverifiable
   - Cross-check with live-fact-verifier output

---

## Scoring

Each dimension scored 0-100:
- Experience: weight 30% (most often fails on AI content)
- Expertise: weight 25%
- Authoritativeness: weight 25%
- Trustworthiness: weight 20%

Overall E-E-A-T score = weighted average.

Passing thresholds:
- 85+ = STRONG E-E-A-T (Google quality bar)
- 70-84 = ADEQUATE
- Below 70 = WEAK (will struggle to rank in competitive niches)

For BOFU and YMYL content, anything below 80 should block delivery in nexus-content's pipeline.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: eeat-enforcer
Status: COMPLETE
Inputs consumed: content [N words], author profile [yes/no], brand profile [yes/no]

E-E-A-T SCORES:
  Experience:        [X/100]
  Expertise:         [X/100]
  Authoritativeness: [X/100]
  Trustworthiness:   [X/100]
  OVERALL E-E-A-T:   [X/100] — [STRONG / ADEQUATE / WEAK]

EXPERIENCE DIMENSION:
  First-person markers per H2: [N of M H2s have markers]
    Missing markers in: [section names]
  Practitioner-only scenarios: [N detected — pass/fail]
  Specific examples: [N specific, M generic — flag list of generics]
  Numeric specificity: [%]

EXPERTISE DIMENSION:
  Technical accuracy: [delegated to live-fact-verifier — pass/fail]
  Nuance acknowledgments: [N detected]
  Beyond-surface depth: [PASS / FAIL — vs SERP comparison]
  Terminology correctness: [N misused terms flagged]

AUTHORITATIVENESS DIMENSION:
  External primary citations: [N — pass if >=3]
  Opinion claims with reasoning: [%]
  Original positions detected: [N]
  Author credibility match: [PASS / FAIL — topic vs author expertise]

TRUSTWORTHINESS DIMENSION:
  Author byline present: [YES/NO]
  Publish date visible: [YES/NO]
  Update date visible: [YES/NO]
  Source citations linked: [%]
  Disclosures present where applicable: [PASS/FAIL/N/A]
  Unverifiable claims: [N flagged]

TOP REMEDIATION ACTIONS:
  1. [Specific E-E-A-T gap] — Fix: [exact action — e.g., "Add first-person experience marker to Section 3 ('Implementing X in Production'); current text is fully impersonal"]
  2. ...

CONFIDENCE: HIGH (with author profile loaded) / MEDIUM (no author profile) / LOW (content lacks visible structure to evaluate)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Hard checks, not vibes.** Every dimension has structural checks, not "feels expert." If a check fails, the score drops; the user can see exactly why.
2. **Author profile dramatically improves accuracy.** Without it, can't cross-check authenticity of experience claims. Always recommend loading one.
3. **BOFU vs TOFU calibration.** First-person experience required is mandatory in BOFU, encouraged in MOFU, optional in TOFU. The skill auto-detects funnel stage from content keywords.
4. **No guess-corrections.** If a claim looks wrong but can't be verified, flag UNVERIFIED — don't propose a correction without a source.
5. **YMYL alert.** Health, finance, legal, and other YMYL topics get stricter thresholds and explicit warning if T scores are weak.
