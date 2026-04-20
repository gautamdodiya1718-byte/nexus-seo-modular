# HEADS-UP BLOCK FORMAT (Universal — every Nexus skill uses this)

**When to show:** Always, immediately after mode is detected. BEFORE any pipeline step runs. No exceptions.

**Format:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS [SKILL-NAME] — HEADS-UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand:           [active brand name OR "none — running brand-agnostic"]
Detected mode:   [auto-detected mode name]
Detection basis: [what in the user's prompt triggered this mode]

What I'll do:
  1. [Step 1 — short description]
  2. [Step 2]
  3. [Step 3]
  ...

Inputs you provided:
  ✓ [input 1]
  ✓ [input 2]

Inputs needed but missing:
  ✗ [input 3] — [what this would have shown — I'll work without it but flag where I'm guessing]
  ✗ [input 4] — [what this would have shown]

Estimated steps: [N]

Proceed? Reply with:
  → 'go' / 'y' / 'yes' to proceed
  → 'change mode' to pick a different mode (I'll list all available)
  → 'cancel' to abort
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Behavior rules:**

1. If user has already specified the mode explicitly in their prompt (e.g., "audit my page speed only"), still show the heads-up but auto-confirm and proceed unless user has set `auto_confirm = false` in their preferences.

2. If brand profile is missing AND the skill needs brand-aware output, the heads-up MUST list it under "Inputs needed but missing" and offer to proceed brand-agnostic OR pause for brand setup.

3. If user says "change mode" — list all modes for this skill with one-line descriptions and wait for selection.

4. If user says "cancel" — abort cleanly, no partial execution.

5. The heads-up is the contract. After user proceeds, every step listed MUST execute. Skipped steps appear in the final accountability log with reasons.
