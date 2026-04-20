# substance-gate.skill

**Role:** Replace word count enforcement with per-section substance evaluation. Each section is checked for completeness, density, and self-sufficiency. Length emerges from substance — no padding to hit a target, no trimming to stay under one.

**Loaded by:** `nexus-content` SKILL.md in all generation modes (mandatory final-pass-per-section gate)

---

## Why this skill exists

Old enforcement: every article must hit SERP midpoint × 1.15 word count. This forces:
- Padding sections to reach the target (filler sentences)
- Trimming complete sections to stay under (loss of substance)
- Generating uniformly-sized articles regardless of topic

New principle: every section is as long as it needs to be to fully answer its purpose. Substance is the constraint. Length is the output, never the input.

This skill enforces the new principle as a per-section gate.

---

## Inputs

Required:
- The current section being generated (or just completed) by content-generation-engine
- The section's stated purpose (from outline / blueprint)

Optional:
- SERP coverage signal (orientation only — what minimum length similar topics need based on top-ranker patterns)

---

## The 3 substance checks per section

### Check 1 — Completeness

Question: Does this section fully answer its stated purpose?

Process:
- Read the section's purpose from the outline ("This H2 explains how X works")
- Read the section as written
- Identify: does the section address everything its purpose promises?
- Identify gaps: sub-questions readers will have that aren't answered

Pass: section's purpose is fully addressed; no obvious sub-question is left hanging.
Fail: purpose is partially addressed; specific gaps identified.

### Check 2 — Density

Question: Does every sentence add information the previous sentence didn't?

Process:
- Read section sentence by sentence
- For each sentence: does it add new information, or does it rephrase/transition/filler?
- Filler types to flag:
  - Pure transitions ("Now let's look at...", "It's important to understand that...", "Moving on to the next point...")
  - Restating what was just said in different words
  - Vague claims that add no concrete information ("This is essential for success", "Many factors come into play")
  - Setup for examples that delays the example unnecessarily

Pass: no flagged filler sentences.
Fail: filler sentences identified — must be removed (not padded around).

### Check 3 — Self-sufficiency

Question: Would a reader need to search elsewhere after reading this section to understand the concept?

Process:
- Identify any concept introduced in the section that isn't fully explained
- Identify any reference to "as you know" or "obviously" or "of course" — flag as assumed knowledge
- Identify any technical term used without definition
- Check: does the section give the reader enough to act on / understand the concept fully?

Pass: section is complete on its own; reader doesn't need outside context.
Fail: gaps identified — section needs to either expand to cover the missing context or link clearly to where it's covered.

---

## Decision logic

After all 3 checks per section:

**ALL PASS** → section is COMPLETE. Record its natural word count. Move to next section.

**ANY FAIL** → section needs targeted fix:
- Completeness fail → add the specific missing content (not padding — actual missing information)
- Density fail → remove flagged filler sentences
- Self-sufficiency fail → expand to cover the assumed knowledge OR add a clear link to where it's covered

Re-run the 3 checks after fix. Loop until all 3 PASS.

**Never:**
- Pad to hit a word count target
- Trim a complete section to stay under a target
- Add more sub-sections just to make the article "longer"

---

## Article-level substance audit (after all sections pass)

After every section has individually passed:

1. Audit the article as a whole:
   - Are there any sections that summarize/repeat earlier sections without adding new information?
   - Is there any conclusion that just restates what was said?
   - Is there any intro/transition between H2s that adds nothing?

2. If any are found, remove them.

3. Record the article's final word count as INFORMATION (not as a target hit/miss).

4. If the article's final word count is significantly below the SERP coverage signal:
   - Don't pad. Investigate: was a sub-topic skipped that the SERP coverage suggests is needed?
   - If yes, add it (substance reason).
   - If no, the article is genuinely shorter and that's fine — note this in the substance audit log.

5. If the article's final word count is significantly above the SERP coverage signal:
   - Don't trim. Investigate: is there genuinely more substance here than competitors offer?
   - If yes, the article is longer for substance reasons — fine.
   - If no, look for filler that snuck through and remove it.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: substance-gate
Status: COMPLETE
Inputs consumed: section [name] | content-generation-engine output

PER-SECTION RESULTS:

Section 1: [name]
  Completeness: PASS / FAIL [reason]
  Density:      PASS / FAIL [N filler sentences flagged]
  Self-sufficiency: PASS / FAIL [N assumed-knowledge issues]
  Decision: COMPLETE / NEEDS_FIX
  [If NEEDS_FIX: specific fix instructions]

Section 2: ...

ARTICLE-LEVEL AUDIT (after all sections pass):
  Redundant summary sections: [count + locations]
  Restating conclusion: [yes/no]
  Pointless transitions: [count]
  Final article word count: [N — informational]
  SERP coverage signal: [N words minimum based on top-ranker patterns]
  Vs coverage: [HIGH / AT / BELOW] — [if BELOW, was a sub-topic skipped?]

DECISION: PASS / NEEDS_FIX
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **No word count enforcement.** Article length is reported, never enforced.
2. **Substance-driven additions only.** If a section needs to be longer, it's because content was missing — not because of a target.
3. **Substance-driven removals only.** If a section needs to be shorter, it's because of filler — not because of a target.
4. **SERP coverage is orientation only.** A 1,200-word complete answer is fine even if competitors average 2,500. Don't pad.
5. **Per-section before whole-article.** Each section must pass individually before the article-level audit runs.
6. **Loops on fix.** Failed checks don't kill the section — they trigger targeted fixes that re-run the checks until pass.
