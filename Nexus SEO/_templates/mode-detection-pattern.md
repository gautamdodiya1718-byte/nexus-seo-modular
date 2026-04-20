# MODE DETECTION PATTERN (Universal — applied per skill)

**Three-tier logic:** auto-detect → suggest → ask.

## Tier 1: Auto-Detect

Parse the user's prompt for trigger keywords mapped to this skill's modes. Each skill defines its own keyword-to-mode map.

**Example map (skill defines this in its SKILL.md):**

```
MODE_DETECTION_MAP = {
  "audit": "full_audit",
  "score": "quick_score",
  "headings": "heading_audit",
  "h2": "heading_audit",
  "e-e-a-t": "eeat_audit",
  "experience signals": "eeat_audit",
  "ai search": "aeo_geo_scoring",
  "vs": "competitor_compare",
  "compare to": "competitor_compare",
  ...
}
```

**Match strategy:**
- Exact phrase match (highest priority)
- Word stem match (lower priority)
- Multiple matches → pick the most specific (longer phrase wins over single word)
- Zero matches → drop to Tier 2

## Tier 2: Suggest

If auto-detect found one strong match, the heads-up shows the detected mode with detection basis, and asks user to confirm or change.

If auto-detect found multiple matches with similar strength, the heads-up suggests the top match and lists alternatives:

```
Detected mode: [top match]
Detection basis: [what in your prompt triggered this]

Other modes that might fit:
  → [alternative 1] — [one-line description]
  → [alternative 2] — [one-line description]

Reply 'go' to proceed with [top match], or pick a different number.
```

## Tier 3: Ask

If auto-detect found nothing, list all modes and ask:

```
I'm not sure which mode fits your request. Pick one:

  1. [Mode 1] — [one-line description] — [example trigger phrase]
  2. [Mode 2] — [one-line description] — [example trigger phrase]
  3. [Mode 3] — [one-line description] — [example trigger phrase]
  ...

Or describe in your own words what you want.
```

## Auto-Confirm Rule

If user's prompt explicitly names the mode (e.g., "run only PageSpeed analysis on this JSON"), auto-detect and the heads-up still shows but treats user input as pre-confirmed. Skill proceeds unless user says cancel.

## Escape Hatches

User can always:
- Say "list modes" — skill shows all modes with descriptions
- Say "change mode to X" mid-execution — skill halts, switches mode, re-shows heads-up
- Say "cancel" — skill halts cleanly
