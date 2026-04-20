# drift-detector.skill

**Role:** Watch the orchestrator's own output stream during chain execution. Catch the "Claude reads pipeline and skips to writing" failure mode — when the model starts producing prose instead of invoking the next skill in the chain.

**Loaded by:** `nexus-orchestrator` SKILL.md throughout chain execution

---

## Why this skill exists

Even with mandatory output blocks (enforced by pipeline-accountability), Claude can still drift:
- Reads the chain plan
- Mentally combines what each skill would do
- Starts writing the synthesis directly
- Skips invoking the actual sub-skills

The auto-pilot.skill.md from original Nexus diagnoses this pattern explicitly. This skill operationalizes the catch.

---

## Inputs

Required:
- Chain plan (which skills are expected to invoke and in what order)
- Live output stream (what the orchestrator is producing)
- Last-invoked skill (so we know what should come next)

---

## Detection patterns

### Pattern 1 — Prose without skill invocation

After invoking skill N, the next thing in the output stream should be either:
- Skill N+1's output block (if it just ran)
- A clear invocation of skill N+1
- A pre-flight or gate decision

NOT acceptable:
- Free-form prose discussing what skill N+1 "would do"
- Synthesis combining skill N's output with what skill N+1 would have produced
- Final output that incorporates skill N+1's expected deliverable without skill N+1 actually running

### Pattern 2 — Skill summary instead of execution

Drift markers:
- "Skill N+1 would identify the following gaps..."
- "Running skill N+1 would show..."
- "Based on what skill N+1 would do, the gaps are..."
- Direct generation of skill N+1's expected output without the skill having run

### Pattern 3 — Output block emitted without skill execution

A perfectly-formatted output block can be emitted without the underlying skill actually running its logic. Detection:
- Check if the block contains: "Inputs consumed:" — if it lists inputs but the skill has no upstream that produced those inputs, drift suspected
- Check confidence: a HIGH confidence block produced without web_search calls (when web_search is needed) is suspicious
- Check timestamp: if blocks appear in suspiciously rapid sequence, suspect parallel hallucination

### Pattern 4 — Skipping confidence-gate or other hard gates

Hard gates (like confidence-gate in nexus-content) MUST run for content to deliver. Drift marker:
- Final content output produced without confidence-gate output block
- Confidence-gate emitted PASS without prior live-fact-verifier output
- Any hard gate skipped silently

---

## Process

### Step 1 — Maintain expected-next-skill state

After each skill invocation, the orchestrator records:
- Which skill just completed
- Which skill is expected next
- What inputs that next skill needs

drift-detector reads this state.

### Step 2 — Monitor output stream

For each token / sentence the orchestrator produces:
- Is it the start of skill N+1's invocation? (e.g., explicit reference to invoking it, or its output block format)
- Is it a transition / commentary between skills?
- Is it drift (prose synthesizing what skill N+1 would do)?

### Step 3 — Detect and respond to drift

When drift detected:

**Mild drift** (commentary that doesn't replace skill execution):
- Flag in log: "Drift commentary detected — skill N+1 invocation should follow"
- Continue monitoring

**Moderate drift** (prose starting to substitute for skill output):
- HALT current generation
- Tell user: "Drift detected: orchestrator started producing skill N+1's output without invoking it. Re-entering pipeline at skill N+1 invocation."
- Re-invoke skill N+1 properly

**Severe drift** (final synthesis written without multiple skills running):
- HALT
- Report: "Severe drift: orchestrator produced final output skipping skills [list]. Pipeline accountability log will reflect this."
- Either re-run from drift point OR flag pipeline as PARTIAL with clear note about what was skipped

### Step 4 — Drift event logging

Every detected drift event recorded in log:
- Timestamp / sequence position
- What drift pattern
- What skill should have run
- Correction taken (continue / halt / re-enter)

These appear in pipeline accountability log under "DRIFT EVENTS DETECTED."

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: drift-detector
Status: COMPLETE (continuous monitoring across chain)
Inputs consumed: chain plan, live output stream, last-invoked skill state

DRIFT MONITORING SUMMARY:
  Total drift events detected: [N]
  Mild drift (commentary): [N]
  Moderate drift (prose substitution): [N]
  Severe drift (skill execution skipped): [N]

DETECTED EVENTS:
  Event 1:
    Position: after skill [name] completed
    Pattern: [pattern type]
    Detected: "[snippet of drifting text]"
    Expected: invocation of skill [N+1]
    Correction: [continued / halted / re-entered pipeline at skill N+1]

  Event 2: ...

CHAIN EXECUTION INTEGRITY (drift dimension):
  Pipeline ran as planned: [YES / NO with details]
  All skills invoked: [YES / NO with which were skipped]
  All hard gates respected: [YES / NO with which gates were bypassed]

CONFIDENCE: HIGH (no drift) / MEDIUM (mild drift, corrected) / LOW (multiple severe drift events)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Continuous monitoring.** This skill watches throughout chain execution, not as a single check.

2. **Halt over correction.** Severe drift is better halted than retroactively corrected. Re-entering pipeline at the missed point preserves integrity.

3. **Hard gates are sacred.** Confidence-gate, substance-gate, differentiation-enforcer — these MUST run. Drift that skips them = pipeline integrity FAILED.

4. **Mild drift is OK if benign.** Some commentary between skills is fine ("now invoking nexus-research"). Only flag when commentary substitutes for execution.

5. **Drift events visible in log.** User can see in pipeline accountability log exactly where drift happened and what correction was taken.

6. **Pairs with pipeline-accountability.** Output-block enforcement and drift detection are complementary — one structurally verifies, other behaviorally watches.
