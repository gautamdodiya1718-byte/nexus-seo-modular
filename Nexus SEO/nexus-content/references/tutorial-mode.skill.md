# tutorial-mode.skill

**Role:** Override default content generation rules for tutorial / how-to content. Mandatory prerequisites section, ordered numbered steps, working code per step, expected outputs per step, troubleshooting section.

**Loaded by:** `nexus-content` SKILL.md when user invokes tutorial mode

---

## Why this skill exists

Tutorials have specific structural requirements that distinguish them from blog posts:
- Reader needs to know what they need before starting (prerequisites)
- Steps must be ordered and complete (skipping or out-of-order = broken tutorial)
- Code examples must work as-is (not pseudocode)
- Each step needs verification (how does the reader know it worked?)
- Troubleshooting common errors prevents frustration

This skill enforces tutorial-specific structure that standard blog generation doesn't apply.

---

## Inputs

Required:
- Topic (the task to be taught)
- Target audience expertise level (beginner / intermediate / advanced)
- Brand profile (recommended for code examples conditional)

Optional:
- Estimated time to complete (if known)
- Required tools (if known)
- Required prerequisites (if known)

---

## The mandatory tutorial structure

### Required sections (in order)

1. **What you'll build / accomplish** (intro)
   - One paragraph: clear outcome statement
   - Visual/screenshot of end result if possible

2. **Prerequisites**
   - Required knowledge (e.g., "familiarity with JavaScript ES6")
   - Required tools (e.g., "Node.js 18+, VS Code")
   - Required accounts (e.g., "Free GitHub account")
   - Estimated time to complete
   - Difficulty level

3. **Numbered steps**
   - Each step: clear name + clear text
   - Each step: working code (verified) where applicable
   - Each step: expected output (what the user should see if the step worked)
   - Each step: brief explanation of what the code does and why

4. **Verification / testing**
   - How does the reader know the whole thing works?
   - Test cases or success indicators

5. **Troubleshooting common issues**
   - Top 3-5 errors that beginners typically hit
   - Each error: what causes it + how to fix it

6. **Next steps / what to learn next**
   - Where to go after this tutorial
   - Related concepts to explore

### Optional sections

- Background context (why this matters / when to use this approach)
- Variations / alternative approaches (with trade-offs)
- Performance considerations
- Production considerations

---

## Process

### Step 1 — Audience-appropriate calibration

Adjust depth based on audience expertise level:

**Beginner:**
- Define every technical term inline
- Show full code (no `[...other code]` shortcuts)
- Explain WHY at every step, not just HOW
- Include screenshots where possible
- Spell out file paths and commands

**Intermediate:**
- Assume basic technical vocabulary
- Show essential code; can use shortcuts for boilerplate
- Explain non-obvious decisions
- Verify familiarity with prerequisites instead of teaching them

**Advanced:**
- Use domain jargon freely
- Show high-leverage code only
- Skip explanations of standard patterns
- Focus on the novel/non-obvious aspects

### Step 2 — Code generation with verification

For every code example:
- Generated code must be syntactically valid
- Code must do what it claims (delegated to code-generation-preview skill)
- Code must include filename comment as first line per Nexus convention (e.g., `// myComponent.tsx`)
- Code must be self-contained or clearly reference prior steps
- Code style matches brand convention if available

### Step 3 — Step verification

For each step, generate the "expected output" content:
- For CLI commands: show the expected terminal output
- For UI actions: describe the expected visual state
- For code: show the expected console output or visible behavior
- For configuration: describe the expected file content or system state

### Step 4 — Troubleshooting generation

Identify common errors for the topic:
- Use web_search to find common errors associated with the tutorial's task
- For each error: cause + fix
- Include actual error messages where possible (not paraphrased)

### Step 5 — Schema requirement

Generate HowTo schema (mandatory for tutorials):
- name (tutorial title)
- description (intro outcome statement)
- totalTime (estimated time, ISO 8601 duration)
- supply (required tools/items)
- tool (specific tools)
- step (each numbered step with name + text + url)

This is in addition to standard Article/BlogPosting schema.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: tutorial-mode
Status: COMPLETE
Inputs consumed: topic, audience level, [optional inputs]

TUTORIAL STRUCTURE GENERATED:

  ✓ What you'll build (intro with outcome statement)
  ✓ Prerequisites (knowledge, tools, accounts, time, difficulty)
  ✓ Numbered steps: [N steps]
    Each step: name + text + code (verified) + expected output + explanation
  ✓ Verification / testing
  ✓ Troubleshooting common issues: [N issues with cause + fix]
  ✓ Next steps

AUDIENCE CALIBRATION: [beginner / intermediate / advanced]
  Code completeness: [full / essential-only / high-leverage-only]
  Term definitions: [inline / assumed / jargon-OK]
  Explanation depth: [WHY-at-every-step / non-obvious-only / novel-only]

CODE EXAMPLES GENERATED: [N]
  All code verified by code-generation-preview: [PASS / FAIL list]
  Filename comments included: [PASS]
  Style matches brand: [PASS / N/A]

EXPECTED OUTPUTS GENERATED: [for N steps]

TROUBLESHOOTING SECTION: [N issues addressed]
  Top issues sourced from: [web_search of "[topic] common errors"]

SCHEMA GENERATED:
  ✓ BlogPosting (standard)
  ✓ HowTo (tutorial-specific — mandatory)

QUALITY CHECKS APPLIED:
  ✓ Substance gates
  ✓ Live fact verification (especially version numbers, command syntax)
  ✓ Live link verification
  ✓ Code generation preview (every code example tested)
  ✓ E-E-A-T (author should be credible for the tutorial topic)

CONFIDENCE: HIGH (full tutorial structure + verified code) / MEDIUM (some code unverified) / LOW (significant verification gaps)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Mandatory structure is mandatory.** Every tutorial gets all 6 required sections. No skipping prerequisites or troubleshooting.

2. **Code must work.** Tutorials with broken code damage credibility more than no tutorial. Every code example runs through code-generation-preview verification.

3. **Expected outputs are mandatory.** Reader needs to know "if I did this right, this is what I should see."

4. **Troubleshooting from real errors.** Use web_search to find actual common errors, not invented ones.

5. **HowTo schema mandatory.** Every tutorial ships with HowTo schema in addition to Article/BlogPosting.

6. **Audience calibration is hard.** Beginner tutorials must hand-hold; advanced tutorials must respect time. Don't mix levels in one tutorial.
