# confidence-gate.skill

**Role:** Hard structural gate that blocks low-confidence content from delivery. Reads aggregated confidence from upstream verification skills (live-fact-verifier, technical-accuracy-checker, eeat-enforcer, helpful-content-checker) and either passes content through or blocks it with specific remediation steps.

**Loaded by:** `nexus-audit` SKILL.md in modes: full_audit, quick_score, eeat_audit, llm_checklist_scoring, fact_verification_only
**Also loaded by:** `nexus-content` SKILL.md as the final gate before delivery

---

## Why this skill exists

Without a hard gate, low-confidence content gets delivered as if it were reliable. Users then publish content that contains unverified claims, fabricated statistics, or weak E-E-A-T — and discover the issue only when ranking suffers or readers point it out.

This skill stops that. It's not a warning system; it's a stop sign.

---

## Inputs

Required:
- Output from one or more verification skills (live-fact-verifier is primary; eeat-enforcer, helpful-content-checker, technical-accuracy-checker also feed in)

Optional:
- Content type (TOFU/MOFU/BOFU/YMYL — affects threshold strictness)
- User override (advanced users can pre-acknowledge low confidence and proceed)

---

## Confidence aggregation

### Step 1 — Read confidence inputs

From each verification skill that's run:

| Skill | Confidence input |
|---|---|
| `live-fact-verifier` | Claim accuracy % |
| `eeat-enforcer` | E-E-A-T overall score |
| `helpful-content-checker` | HCU overall score |
| `technical-accuracy-checker` | Technical accuracy score |
| `differentiation-checker` | Differentiation pass/fail |
| `link-verifier` (in nexus-content) | Link verification pass/fail |

### Step 2 — Apply thresholds per content type

**TOFU informational content:**
- Live fact verification: ≥75%
- E-E-A-T: ≥70
- HCU: ≥70
- Technical accuracy: ≥75
- Differentiation: PASS or content is 100% original/primary research
- Link verification: 100% PASS (no broken links)

**MOFU comparison/evaluation content:**
- Live fact verification: ≥80%
- E-E-A-T: ≥75
- HCU: ≥75
- Technical accuracy: ≥80
- Differentiation: PASS
- Link verification: 100% PASS

**BOFU transactional content:**
- Live fact verification: ≥85%
- E-E-A-T: ≥80
- HCU: ≥80
- Technical accuracy: ≥85
- Differentiation: PASS
- Link verification: 100% PASS

**YMYL content (health, finance, legal, safety):**
- Live fact verification: ≥90%
- E-E-A-T: ≥85
- HCU: ≥85
- Technical accuracy: ≥90
- Differentiation: PASS
- Link verification: 100% PASS
- Author credentials: REQUIRED (must be visible in content)
- Disclosures: REQUIRED where applicable

### Step 3 — Pass/Block decision

Content PASSES the gate if ALL applicable thresholds are met.
Content is BLOCKED if ANY threshold is missed.

When BLOCKED, the skill outputs:
- Which thresholds were missed
- Specific remediation steps from each upstream skill
- A re-run instruction (which skills need to run again after fixes)

### Step 4 — User override path

If the user has set `confidence_gate_override = true` in their preferences:
- Even when blocked, content is delivered
- BUT delivery includes a prominent UNRELIABLE warning at the top
- Audit log records the override

This option exists for users who want to ship despite warnings — but with explicit accountability.

---

## Output

### When content PASSES:

```
[NEXUS-STEP-N OUTPUT]
Skill: confidence-gate
Status: COMPLETE
Decision: PASS

CONFIDENCE SUMMARY:
  Content type detected: [TOFU/MOFU/BOFU/YMYL]
  Thresholds applied: [content-type-specific table]

  Live fact verification:    [X%] vs threshold [Y%] — PASS
  E-E-A-T score:             [X/100] vs threshold [Y] — PASS
  HCU score:                 [X/100] vs threshold [Y] — PASS
  Technical accuracy:        [X/100] vs threshold [Y] — PASS
  Differentiation:           PASS
  Link verification:         100% PASS
  [YMYL author credentials]: PRESENT (if applicable)

GATE DECISION: PASS — content cleared for delivery.
[/NEXUS-STEP-N OUTPUT]
```

### When content is BLOCKED:

```
[NEXUS-STEP-N OUTPUT]
Skill: confidence-gate
Status: COMPLETE
Decision: BLOCK

CONFIDENCE SUMMARY:
  Content type detected: [TOFU/MOFU/BOFU/YMYL]
  Thresholds applied: [content-type-specific table]

  Live fact verification:    [X%] vs threshold [Y%] — [PASS / FAIL by Z%]
  E-E-A-T score:             [X/100] vs threshold [Y] — [PASS / FAIL by Z]
  HCU score:                 [X/100] vs threshold [Y] — [PASS / FAIL by Z]
  Technical accuracy:        [X/100] vs threshold [Y] — [PASS / FAIL by Z]
  Differentiation:           [PASS / FAIL]
  Link verification:         [N broken links — FAIL]

GATE DECISION: BLOCK — content cannot be delivered as-is.

REASONS FOR BLOCK:
  1. [Specific failed threshold]
     What needs fixing: [from upstream skill]
     How to fix: [specific actions]
     Re-run: [which skill to re-run after fix]

  2. ...

REMEDIATION ORDER:
  1. [Highest-priority fix — typically WRONG facts first]
  2. [Next fix]
  3. ...

AFTER FIXES, RE-RUN: [list of skills that need to re-execute]

USER OVERRIDE OPTION:
  If you want to deliver despite low confidence, reply 'override confidence gate' and the content will be delivered with an UNRELIABLE warning header. This is logged in the audit trail.
[/NEXUS-STEP-N OUTPUT]
```

### When user overrides:

```
[NEXUS-STEP-N OUTPUT]
Skill: confidence-gate
Status: COMPLETE
Decision: OVERRIDE_GRANTED

User explicitly chose to deliver content despite gate failure.
Content will include UNRELIABLE warning header.
Override logged: [timestamp, content identifier, failed thresholds]

DELIVERED CONTENT INCLUDES THIS HEADER:
  ⚠ This content was delivered with active confidence-gate override.
  Failed checks: [list]
  Reader discretion advised. Manual review recommended before publication.
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **No quiet pass-throughs.** Every gate decision is logged. Even passes get an explicit PASS block.

2. **YMYL is strictest.** Health, finance, legal, safety topics get the highest thresholds. Auto-detect YMYL from keywords.

3. **Block, don't warn.** Default behavior is BLOCK. User can override but it's a deliberate choice with consequences (UNRELIABLE warning header).

4. **Specific remediation.** Not "improve confidence" — list every failed threshold and its specific fix from upstream skills.

5. **Re-run guidance.** After fixes, the user shouldn't have to re-run the entire pipeline. Specify which skills need to re-execute (typically just live-fact-verifier + the specific failed skill).

6. **Override is a feature, not a hack.** Some experienced users genuinely want to deliver content with known caveats — that's fine, but the override is logged and the content carries the warning.

7. **Content type detection.** If user didn't specify, infer from keywords:
   - YMYL: health, medical, finance, legal, safety, parenting, food safety
   - BOFU: pricing, buy, sign up, book demo, free trial
   - MOFU: vs, comparison, alternative, best, review
   - TOFU: what is, how to, guide, tutorial, introduction
