# pipeline-accountability.skill

**Role:** Enforce mandatory output blocks across chained skills and produce the final pipeline accountability log. Without this, chained pipelines can silently skip skills and the user has no way to verify execution.

**Loaded by:** `nexus-orchestrator` SKILL.md throughout chain execution

---

## Why this skill exists

The single biggest pipeline failure mode in the original Nexus system: Claude reads the pipeline definition, mentally combines what each skill would do, and writes everything in one pass without actually invoking the skills. Result: pipeline appears to have run but didn't.

This skill makes pipeline execution structurally verifiable. Every skill must emit its output blocks; next skill verifies before consuming; final log shows what actually executed.

---

## Inputs

Required:
- Chain plan from orchestrator
- Output blocks emitted by each chained skill (live, during execution)

---

## The output block format (universal)

Every chained skill MUST emit:

```
[SKILL-NAME OUTPUT]
Skill: [skill name]
Status: COMPLETE / SKIPPED ([reason]) / FAILED ([reason])
Inputs consumed: [list of what came from previous skills]
Output deliverable:
  [structured output]
Confidence: HIGH / MEDIUM / LOW
[/SKILL-NAME OUTPUT]
```

This skill ENFORCES that format:
- If a chained skill produces prose without an output block → flag drift, do not proceed
- If output block has Status:SKIPPED without a stated reason → flag, request reason
- If output block lacks structured deliverable → flag, request the deliverable

## Verification logic per chained skill

Before invoking skill N+1:

1. **Verify skill N's output block exists.**
   - Look for the expected `[SKILL-N OUTPUT]` block in conversation.
   - If absent: HALT. Report: "Pipeline accountability error: skill N (name) did not emit its output block. Cannot proceed to skill N+1. Re-invoke skill N."

2. **Verify status is COMPLETE or SKIPPED with reason.**
   - If FAILED: HALT. Report failure reason. Ask user how to proceed.
   - If SKIPPED without reason: HALT. Request reason.

3. **Verify deliverable is present.**
   - If structured output is empty or malformed: HALT. Re-invoke skill N.

4. **Verify next skill's required inputs are in skill N's deliverable.**
   - If skill N+1 needs "research package" and skill N's output doesn't contain it: HALT. Mismatch — re-invoke skill N with correct mode.

## Generate pipeline accountability log

After chain execution completes (or halts):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS PIPELINE ACCOUNTABILITY LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Chain plan:               [list of skills planned]
Total skills planned:     [N]
Total skills executed:    [N]
Total skills skipped:     [N]
Total skills failed:      [N]

PER-SKILL BREAKDOWN:
  [01] [skill name] — [mode] — STATUS — [confidence]
       Output emitted: YES/NO
       Output consumed by: [next skill in chain]

  [02] [skill name] — [mode] — STATUS — [confidence]
       Output emitted: YES/NO
       Output consumed by: [next skill]

  [03] [skill name] — [mode] — SKIPPED ([reason])
       Output emitted: YES (skipped status block)

  ...

DRIFT EVENTS DETECTED: [N]
  [If any: list each — where in chain, what drift, correction taken]

OUTPUT BLOCKS EMITTED: [N of N expected]
  [If gaps: list which skills did not emit]

PIPELINE INTEGRITY: COMPLETE / PARTIAL / FAILED
  IF COMPLETE: every planned skill executed (or explicitly skipped with reason); every output block emitted
  IF PARTIAL: [list what's missing and why]
  IF FAILED: [list failure points]

OVERALL CONFIDENCE: [HIGH/MEDIUM/LOW] (lowest of any executed skill)

DELIVERED OUTPUT: [type — full content / blocked at gate / partial deliverable]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

This log is MANDATORY for every chain execution. Even partial/failed runs produce the log.

## Behaviors

1. **Output blocks are non-negotiable.** Any chained skill that doesn't emit blocks halts the pipeline.

2. **Drift detection is paired with this skill.** drift-detector.skill.md catches the broader "Claude writing prose instead of invoking skill" pattern; this skill enforces the structural output requirement.

3. **Halt over heuristic.** When in doubt, halt and ask user. Better to pause than to silently produce partial output.

4. **Accountability log is the user's view.** They can read the log to verify exactly what ran. Without it, they're trusting the model.

5. **No silent skips.** Skipped skills must emit a SKIPPED block with reason. "I didn't think it was needed" is acceptable IF it's stated; silent absence is not.

6. **Partial pipeline integrity is OK if explicit.** Some chains genuinely have optional skills. PARTIAL is a valid outcome IF the log clearly shows what was skipped and why.
