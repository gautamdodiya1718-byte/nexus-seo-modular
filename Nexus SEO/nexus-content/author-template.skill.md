---
name: author-template
description: Author voice profile template. Copy and rename to author-[name].skill.md, fill in all sections, then save to your project so any Nexus skill can read this author's signature voice for content audits and content generation. Author profile is separate from brand profile — a brand can have multiple authors.
---

# Author Profile Template

Copy this file. Rename to `author-[your-name].skill.md`. Fill in every section. Save to your project. Nexus skills will auto-detect it on next session.

---

## Identity

**Name:** [Author's full name]
**Role / Title:** [e.g., "Senior QA Engineer", "Founding Engineer", "VP of Product"]
**Years of experience in domain:** [N years]
**Domain expertise areas:** [comma-separated list — e.g., "Playwright test automation, CI/CD pipelines, browser testing"]
**Public bio URL or LinkedIn:** [optional, but useful for E-E-A-T enforcement]

---

## Voice Signature

### Vocabulary tier

Pick one — affects which words the humanizer uses and which it flags as off-voice:

- [ ] **Beginner-friendly** (avoids jargon, defines all technical terms inline)
- [ ] **Practitioner** (uses domain jargon naturally, assumes intermediate audience)
- [ ] **Expert / senior** (uses advanced jargon without definition, assumes deep domain knowledge)
- [ ] **Mixed** (per-piece — pick based on the audience of the specific content)

### Vocabulary quirks

List 5-15 words/phrases this author uses naturally and frequently. These should feel "voicey" — they signal who's writing.

Example: "in practice", "the gotcha is", "what actually happens is", "practitioners overlook", "the trap here is"

[Your list:]
1. [phrase]
2. [phrase]
3. ...

### Words/phrases this author NEVER uses

Anti-vocabulary. The humanizer will flag and remove any of these.

Example: "synergy", "leverage", "best-in-class", "deep dive into" (overused), "a wide variety of"

[Your list:]
1. [phrase]
2. [phrase]
3. ...

### Sentence rhythm

Pick the dominant pattern (humanizer calibrates to this):

- [ ] **Short and punchy** — most sentences under 15 words, periods over commas, frequent paragraph breaks
- [ ] **Medium-paced** — average 15-25 words, mix of short and long, paragraph every 3-4 sentences
- [ ] **Flowing / longer-form** — average 25-40 words, complex sentences welcome, paragraphs every 5-6 sentences
- [ ] **Variable** — explicitly varies rhythm for effect (short for emphasis, long for explanation)

---

## Signature Structural Moves

How this author typically structures their writing. Pick all that apply.

### Opening moves
- [ ] Always opens with a counter-intuitive observation
- [ ] Always opens with a specific scenario / story
- [ ] Always opens with a stat or number
- [ ] Always opens by stating the practitioner pain point upfront
- [ ] Always opens with a definition
- [ ] Other: [describe]

### Section structure
- [ ] Always ends each H2 section with a one-sentence takeaway
- [ ] Always includes "what doesn't work" before "what works" in tutorials
- [ ] Always includes a code example before explaining the concept
- [ ] Always includes a comparison table when discussing options
- [ ] Always uses callouts for definitions vs tips vs warnings
- [ ] Other: [describe]

### Closing moves
- [ ] Always ends with a specific next action the reader can take
- [ ] Always ends with a question the reader should ask themselves
- [ ] Always ends with the "common mistake to avoid" summary
- [ ] Other: [describe]

---

## Opinion Stances on Niche Debates

For E-E-A-T, content needs opinion claims with reasoning. List 3-7 specific opinions this author holds on debates in their domain.

Example for a Playwright author:
1. "Page Object Model is overused in Playwright; functional helpers + locator chains are cleaner for most teams"
2. "Snapshot testing is more useful for visual regression than data state assertions"
3. "Parallel execution at the worker level is almost always misconfigured by default"

[Your stances:]
1. [opinion + brief reasoning]
2. [opinion + reasoning]
3. ...

---

## Personal Experience Markers

Domain-specific scenarios this author has actually encountered. Used by E-E-A-T enforcer to detect inauthentic AI-generated experience claims.

Example:
- "Debugged a Playwright timeout that turned out to be a CSS animation triggering on slow CI runners"
- "Migrated a 1200-test suite from Selenium to Playwright over 3 months"
- "Built a CI/CD test analytics dashboard using GitHub Actions + Allure + custom Playwright reporter"

[Your markers:]
1. [specific scenario]
2. [specific scenario]
3. ...

---

## Writing Examples (3-5 sentences in this author's natural voice)

This is the calibration data for the humanizer. Paste 3-5 sentences (not paragraphs) that exemplify this author's voice. Real sentences from real writing this author has done — not paraphrases.

Example:
1. "If your test suite takes 18 minutes to run, you don't have a Playwright problem — you have a parallelization problem disguised as a Playwright problem."
2. "Most teams I've audited have between 30% and 60% redundant assertions across their tests, and removing them cuts runtime nearly linearly."
3. "Don't mock the database in integration tests. We tried that for a quarter and shipped a broken migration because the mocks were too forgiving."

[Your examples:]
1. [sentence]
2. [sentence]
3. ...

---

## Topics This Author Is Credible On (vs Topics They're Not)

E-E-A-T cares about author credibility for the specific topic. List explicitly.

**Credible to write about:**
- [topic 1]
- [topic 2]
- [topic 3]

**NOT credible to write about (humanizer should flag if attempted):**
- [topic outside expertise — e.g., "marketing automation", "frontend animation"]

---

## Disclosure & Bias Statement (optional but recommended for E-E-A-T)

If this author has any commercial relationships that should be disclosed in content (e.g., works at a vendor whose product is being reviewed, has a financial stake), state them here. The eeat-enforcer will check that disclosures are present where relevant.

[Your disclosures:]
- [If applicable]

---

## How Skills Use This Profile

| Skill | What it reads from this profile |
|---|---|
| `humanizer` (in nexus-content) | Vocabulary tier, vocabulary quirks, anti-vocabulary, sentence rhythm, writing examples — for humanization rewrites |
| `eeat-enforcer` (in nexus-audit and nexus-content) | Personal experience markers, opinion stances, topics credible on, disclosures — for E-E-A-T scoring |
| `author-voice-applier` (in nexus-content) | All voice signature fields + structural moves — for first-pass generation in this voice |
| `differentiation-checker` (in nexus-audit) | Opinion stances + experience markers — to detect uniqueness vs generic AI content |

---

## To Add or Remove an Author

- **Add:** Save this file as `author-[name].skill.md` in the same folder as your brand profile.
- **Switch authors:** User says "switch author to [name]" — Nexus unloads current and loads the new one.
- **Multi-author site:** Per article, user can specify which author profile to use. If not specified, Nexus asks.

---

*Author profiles are separate from brand profiles. A brand can have many authors. Each author writes in their own voice within the brand's overall standards.*
