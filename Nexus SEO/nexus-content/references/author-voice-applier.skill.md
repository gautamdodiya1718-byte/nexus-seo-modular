# author-voice-applier.skill

**Role:** Load author voice profile and calibrate content generation to that author's signature style. Goes beyond brand voice (tone, banned words) to author-specific structural moves, vocabulary quirks, opinion stances, and sentence rhythm. Used during generation, not as post-hoc humanization.

**Loaded by:** `nexus-content` SKILL.md in all generation modes when `author-*.skill.md` profile is present

---

## Why this skill exists

Brand profile defines tone and banned words at the brand level. But within a brand, different authors should sound different. A generic AI-voiced article that conforms to brand tone but lacks personal voice signature reads as polished AI prose, not as a specific experienced person's writing.

This skill is the bridge between author identity and generated content. It doesn't humanize after the fact — it calibrates generation upfront.

---

## Inputs

Required:
- Author profile file (`author-[name].skill.md`)
- Content being generated (or about to be generated)
- Brand profile (for cross-reference — author voice operates within brand standards)

---

## Process

### Step 1 — Load and parse author profile

Extract from author profile:
- Identity (name, role, expertise areas, years experience)
- Voice signature:
  - Vocabulary tier (beginner/practitioner/expert/mixed)
  - Vocabulary quirks (signature phrases this author uses)
  - Anti-vocabulary (words/phrases this author NEVER uses)
  - Sentence rhythm (short-punchy / medium-paced / flowing / variable)
- Signature structural moves (opening, section, closing patterns)
- Opinion stances on niche debates
- Personal experience markers
- Writing examples (3-5 sentences in natural voice — calibration data)
- Topics credible on / not credible on

### Step 2 — Pre-generation calibration

Before content generation begins, set generation parameters:

**Vocabulary tier:** content-generation-engine adjusts complexity to match (beginner = inline definitions, expert = no definitions for advanced terms).

**Vocabulary injection:** ensure 30-50% of "voicey" sentences include at least one signature phrase from author's vocabulary quirks list (not forced, but prompted as natural opportunity).

**Anti-vocabulary scrub:** any sentence that uses an anti-vocabulary phrase gets flagged and rewritten before being considered done.

**Sentence rhythm enforcement:** after each paragraph, check word counts:
- Short-punchy: avg <15 words/sentence; flag any >25
- Medium-paced: avg 15-25; flag any <8 or >35
- Flowing: avg 25-40; flag any <12

**Structural moves application:** apply selected structural moves:
- Opening: if author always opens with X (counter-intuitive observation, specific scenario, stat, etc.), generate intro with that pattern
- Sections: if author always ends each H2 with takeaway, generate that
- Closing: if author always ends with specific next action, generate that

### Step 3 — Opinion stance integration

For BOFU/MOFU content where opinions belong:
- Look for opportunities to express the author's actual opinion stances on niche debates
- These should feel natural in flow — not forced citations of the stance
- Mark stance expressions in generated content so eeat-enforcer can credit them

### Step 4 — Experience marker injection

For sections discussing topics the author has personal experience with:
- Look for opportunities to insert "When I..." / "We saw..." / "In practice..." markers backed by author's stated experience
- Pull from author's "personal experience markers" list — don't fabricate experience
- Cross-check: if author profile lists experience X, content can reference X-related scenarios

If content is on a topic outside author's stated expertise areas:
- Flag this as a credibility issue
- Do NOT inject fake experience markers
- Recommend either (a) get a different author profile that's credible for this topic, OR (b) remove first-person framing entirely and write in third person

### Step 5 — Voice calibration check

After generation, run author voice check:
- Compare generated content's vocabulary to author's signature vocabulary — frequency match expected
- Compare sentence rhythm to author's stated rhythm
- Verify anti-vocabulary scrubbed
- Verify opinion stances expressed where natural
- Verify experience markers present in author's expertise areas

If significant divergence detected, generation gets a rewrite pass focused on voice calibration (not content change).

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: author-voice-applier
Status: COMPLETE
Inputs consumed: author profile [name], content [N words], brand profile

AUTHOR PROFILE LOADED:
  Author: [name]
  Vocabulary tier: [tier]
  Sentence rhythm: [rhythm]
  Signature phrases: [count] loaded
  Anti-vocabulary: [count] loaded
  Opinion stances: [count] loaded
  Personal experience markers: [count] loaded
  Topics credible on: [list]

PRE-GENERATION CALIBRATION:
  Generation parameters set per author profile.

POST-GENERATION VOICE CHECK:
  Vocabulary signature match: [%] of "voicey" sentences include signature phrases (target 30-50%)
  Anti-vocabulary scrub: [N instances found and replaced]
  Sentence rhythm match: [PASS / FAIL — actual avg vs target]
  Opinion stances expressed: [N of N applicable opportunities]
  Experience markers injected: [N — all sourced from author's stated experience]
  Topic credibility match: [PASS / FAIL — content on author's expertise topics?]

REWRITE PASSES TRIGGERED: [N — for voice calibration]

VOICE FIDELITY SCORE: [X/100]

CRITICAL ISSUES (if any):
  - [issue — e.g., "Content covers payments fraud detection but author profile has no expertise in payments. Either change author or remove first-person framing."]

CONFIDENCE: HIGH (full author profile + voice check passed) / MEDIUM (partial profile or some divergence) / LOW (no calibration possible)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Calibrate during generation, not just after.** Author voice should be in the first draft, not retrofitted by humanizer.

2. **Don't fabricate experience.** If author profile doesn't list experience with topic X, don't inject "When I worked with X..." markers. Either use existing experience markers or write in third person.

3. **Topic-credibility check is hard.** If author isn't credible for topic, flag — don't pretend they are.

4. **Anti-vocabulary scrub is automatic.** Any sentence with banned phrase gets rewritten, no exceptions.

5. **Opinion stances feel natural.** Inject stances where they fit the flow — not as forced citations.

6. **Voice fidelity is measurable.** Output a score so user can see how well content matches author's voice. <80 = significant divergence; rewrite triggered.
