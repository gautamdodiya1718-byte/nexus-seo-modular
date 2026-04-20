# OUTPUT BLOCK FORMAT (Universal — every skill step produces this)

**Purpose:** Make pipeline execution verifiable. Without these blocks, there's no proof a step actually ran. With them, the next step in a chain can verify the previous step's deliverable exists.

**Per-step output block:**

```
[NEXUS-STEP-N OUTPUT]
Skill: [skill name]
Step: [step number] of [total]
Status: COMPLETE / SKIPPED ([reason]) / FAILED ([reason])
Inputs consumed: [list]
Output deliverable:
[the actual structured output of this step — data, findings, recommendations, etc.]
Confidence: [HIGH / MEDIUM / LOW] (used by confidence-gate)
[/NEXUS-STEP-N OUTPUT]
```

**Final accountability log (every skill ends with this):**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS [SKILL-NAME] — ACCOUNTABILITY LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Mode:                   [mode used]
Brand:                  [active brand OR "brand-agnostic"]
Steps planned:          [N]
Steps executed:         [N]
Steps skipped:          [N] (each with reason)
Steps failed:           [N] (each with reason)
Inputs missing:         [list — flagged in heads-up]
Confidence assessment:  [HIGH / MEDIUM / LOW — overall]
Pipeline integrity:     COMPLETE / PARTIAL [reason]
Output delivered:       [summary of what user receives]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Rules:**

1. Every step MUST emit its output block. No exceptions. If a step is skipped, it still emits the block with `Status: SKIPPED (reason)`.

2. The next step in a pipeline MUST verify the previous step's output block exists before consuming its output. If absent, halt and report the gap.

3. The accountability log is mandatory. Even on a failed pipeline, the log shows what completed and what didn't.

4. Confidence assessment in the log is the lowest confidence of any step. If any step was LOW confidence, the whole pipeline is LOW confidence.
