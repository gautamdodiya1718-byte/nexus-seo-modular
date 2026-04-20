# SKILL ENTRY PATTERN (Universal — every Nexus skill SKILL.md follows this)

**The 5-step entry sequence every skill executes when invoked:**

## Step 1 — Brand Profile Detection

Check the current session context for brand profile files (`brand-*.skill.md`).

```
IF found 1 brand profile:
  Auto-load it. Confirm: "Running under [Brand Name]. Correct?"
  Wait for user confirm or "switch brand."

IF found 2+ brand profiles:
  List them. Ask which to load.

IF no brand profiles:
  IF this skill REQUIRES brand context (most do):
    Tell user: "No brand profile found. Options:
      a) Create one now (I'll guide you through brand-template.skill.md)
      b) Run brand-agnostic (limited output — no brand voice, no internal links)
      c) Cancel"
  IF this skill works fine brand-agnostic (e.g., nexus-tech-seo audit of any site):
    Note "running brand-agnostic" and proceed.
```

## Step 2 — Mode Detection

Apply the mode-detection-pattern.md logic. Auto-detect → suggest → ask.

## Step 3 — Heads-Up Block

Show the heads-up-block.md format. Wait for user confirmation (or auto-proceed if user explicitly stated mode in prompt).

## Step 4 — Pipeline Execution

Run the pipeline for the selected mode. Each step:
- Loads its required inputs (from previous steps' output blocks if chained)
- Executes
- Emits its output-block per output-block-format.md
- The next step verifies the previous output block exists before proceeding

If any required input is missing, that step emits SKIPPED status with reason.

## Step 5 — Accountability Log

Final block per output-block-format.md. Shows what ran, what skipped, what failed, overall confidence.

---

## Mandatory Behaviors (apply to every skill)

1. **Never skip silently.** If you can't run a step, emit SKIPPED block with reason.
2. **Never fabricate.** If a fact, link, or claim can't be verified, mark it as unverified or remove it.
3. **Never measure by word count.** Substance, completeness, and density are the metrics.
4. **Always heads-up first.** No execution until user has seen what's about to happen.
5. **Always emit output blocks.** Pipeline integrity depends on them.
6. **Always emit accountability log.** Even on partial/failed runs.

## Mandatory Files Every Skill Includes

- `SKILL.md` — entry point with mode detection, heads-up logic, pipeline routing
- `README.md` — install instructions for users
- `brand-template.skill.md` — so user can create a brand profile
- `brand-audience-template.skill.md` — extended audience profile template
- `references/` — folder with the actual sub-skills

Skills that need author voice also include `author-template.skill.md`.
Skills that need persistent user preferences also include `user-preferences-template.md`.
