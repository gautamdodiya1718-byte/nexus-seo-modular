# pre-flight-check.skill

**Role:** Verify all required resources (brand profile, author profile, user preferences, required inputs for the goal) are present before any chained pipeline runs. Block with specific instructions if anything is missing.

**Loaded by:** `nexus-orchestrator` SKILL.md before any chain execution

---

## Why this skill exists

Without a pre-flight check, chained pipelines fail mid-execution when they hit a missing brand profile or absent input. That wastes context and produces confusing partial outputs.

Pre-flight catches missing resources before any execution starts. Either the user provides what's missing, or the orchestrator runs in degraded-but-complete mode (brand-agnostic, etc.).

---

## Inputs

Required:
- The chain plan (which skills will run, what each needs)
- The detected installed skills (from installed-skill-detector)
- Session context (what's currently loaded)

---

## Resource check categories

### Category 1 — Brand profile

For chains that need brand context (most chains do):

Check:
- Is exactly 1 brand profile in context?
- Does the brand profile have all required fields populated for the chain's needs?
  - For nexus-content chains: brand voice, banned words, urls, mention rules, CTA preferences
  - For nexus-research chains: content pillars, competitors, primary geo
  - For nexus-strategy chains: content pillars, differentiators, competitors

Action:
- All present: PASS
- Missing brand: BLOCK — tell user "Brand profile required for this chain. Either upload one or run individual skills standalone (some support brand-agnostic mode)."
- Brand present but missing required fields: WARN — tell user which fields are missing and what default values will be used

### Category 2 — Author profile (when needed)

For chains that include nexus-content with author voice:

Check:
- Is exactly 1 author profile in context?
- If multiple, which to use?
- If none, content uses brand voice only (less specific)

Action:
- Present: PASS — note which author will be used
- Multiple: ASK user which to use
- None: WARN — content will use brand voice only; offer to load an author profile

### Category 3 — User preferences (when present)

If user-preferences-*.md exists in context:
- Load it
- Verify it doesn't have rules that conflict with the chain's required behavior (e.g., user preference "no schema" conflicts with nexus-content's mandatory schema generation)

Action:
- Present and compatible: PASS — apply preferences across chain
- Present and conflicting: WARN — tell user about the conflict, ask how to resolve

### Category 4 — Required inputs for the goal

For each goal, specific inputs are required:

| Goal | Required inputs |
|---|---|
| Write blog | Topic / keyword |
| Landing page | Topic / target audience / page type (vs / alternatives / features / service) |
| Keyword research | Topic / seed keywords |
| Audit URL | URL or content (and target keyword) |
| Why not ranking | URL + target keyword + ideally GSC + PageSpeed |
| Optimize | URL + content (and target keyword) |
| Competitor | Competitor URL or topic |
| Full strategy | Brand context (no specific topic needed) |
| Semantic SEO | Topic / cluster center |
| Programmatic SEO | Pattern + brand context |
| Tech SEO audit | Source code + GSC + PageSpeed (any combination) |
| Portfolio status | Brand + content-memory populated |

Check:
- Are all REQUIRED inputs present?
- Are all RECOMMENDED inputs present? (Note absences but don't block.)

Action:
- All required present: PASS
- Required missing: BLOCK — list what's missing, ask user to provide
- Required present but recommended missing: WARN with note about reduced output quality

### Category 5 — Memory state (for chains that use memory)

If chain includes nexus-strategy (which uses memory layers):

Check:
- Does prior session memory exist for this brand?
- If yes: offer "Continue from prior session" vs "Start fresh"
- If no: confirm fresh start

### Category 6 — External tool availability

For chains that need:
- web_search (live-fact-verifier, live-serp-verifier, etc.) — verify available
- HTTP requests (live-link-verifier) — verify available

If not available: WARN that those checks will be SKIPPED with reason, lowering confidence.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: pre-flight-check
Status: COMPLETE / BLOCK
Inputs consumed: chain plan, installed skills, session context

PRE-FLIGHT CHECK RESULTS:

Brand profile:        [PASS / WARN / BLOCK]
  Detected:           [name or "none"]
  Required fields:    [PASS / missing fields list]

Author profile:       [PASS / WARN]
  Detected:           [name or "none"]
  Note:               [brand voice will be used / specific author voice will be used]

User preferences:     [PASS / WARN / N/A]
  Detected:           [yes / no]
  Conflicts:          [none / list]

Required inputs for goal "[goal name]":
  ✓ [input 1]: [present]
  ✓ [input 2]: [present]
  ✗ [input 3]: [MISSING]
  ⚠ [input 4]: [recommended but missing — proceeding with reduced quality]

Memory state (if applicable):
  Prior session: [found / none]
  Action: [continue from prior / start fresh]

External tools:
  web_search:         [AVAILABLE / UNAVAILABLE]
  HTTP requests:      [AVAILABLE / UNAVAILABLE]

PRE-FLIGHT DECISION: PASS / BLOCK [reasons]

NEXT STEP:
  IF PASS: proceed to heads-up + chain execution
  IF BLOCK: provide missing resources, then re-run pre-flight
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Block early.** Better to halt at pre-flight than fail mid-chain.

2. **Specific block reasons.** Not "missing inputs" — list each missing input by name with what it's used for.

3. **WARN ≠ BLOCK.** Recommended-but-missing inputs reduce quality but don't halt execution. Required-and-missing halts.

4. **Memory continuation offer is opt-in.** Don't auto-load prior memory; ask user.

5. **Tool availability transparency.** If web_search unavailable, user should know live-fact-verification will be SKIPPED — sets expectations.

6. **Author profile is always optional.** Even chains that benefit from author profile work fine without one. Just note the trade-off.
