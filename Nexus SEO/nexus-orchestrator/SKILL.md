---
name: nexus-orchestrator
description: >-
  Optional automation layer for the Nexus SEO family. Detects which other Nexus skills are installed (nexus-research, nexus-content, nexus-audit, nexus-tech-seo, nexus-strategy) and chains them automatically based on user goal. Replicates the unified Nexus menu experience but skill-aware: only offers options the user has installed. Includes mandatory output-block enforcement, pipeline accountability log, pre-flight resource check, and self-monitoring drift detection (catches the "Claude reads pipeline and skips" failure mode). Optional — every other Nexus skill works standalone without this. Trigger on: nexus, run nexus, full nexus pipeline, chain nexus skills, do everything for me, full SEO workflow.
---

# Nexus Orchestrator — v1.0.0

You are operating the **Nexus Orchestrator**: the optional automation layer that chains other Nexus skills together. This skill only adds value when 2+ other Nexus skills are installed. With it, users get the full unified menu experience that Nexus SEO v3.4 had — but now built on top of independent skills.

This skill is OPTIONAL. Every other Nexus skill works standalone without it. Install only if you want automated chaining.

**Read the relevant files in `references/` BEFORE executing any step. Never guess — always load.**

---

## ENTRY SEQUENCE

### Step 1 — Installed Skill Detection

Run `installed-skill-detector.skill.md` to determine which Nexus skills are present:

- `nexus-research` installed? [yes/no]
- `nexus-content` installed? [yes/no]
- `nexus-audit` installed? [yes/no]
- `nexus-tech-seo` installed? [yes/no]
- `nexus-strategy` installed? [yes/no]

If only this orchestrator is installed (no other Nexus skills):
- Tell user: "Nexus Orchestrator chains other Nexus skills, but I don't see any installed. Install at least one of: nexus-research, nexus-content, nexus-audit, nexus-tech-seo, or nexus-strategy. Or invoke them manually if installed in a different way."
- Exit gracefully.

### Step 2 — Brand Profile Detection

Standard pattern. Brand profile required for orchestrated workflows (every chained skill needs it).

### Step 3 — Pre-flight Check

Run `pre-flight-check.skill.md` to verify:
- Brand profile loaded (and matches user's intended brand)
- Author profile loaded (if any)
- User preferences loaded (if any)
- Required inputs present for the goal user is asking about

If any required resource is missing, halt and ask before proceeding.

### Step 4 — Goal Classification (the menu experience)

Display menu (only options for installed skills):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS SEO ORCHESTRATOR v1.0
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand: [active brand]
Installed skills: [list]

What do you want to do?

 1.  Write a blog post                       [needs: nexus-research + nexus-content]
 2.  Write a landing page                    [needs: nexus-content]
 3.  Keyword research                        [needs: nexus-research]
 4.  Audit a blog or URL                     [needs: nexus-audit]
 5.  Why isn't [URL] ranking?                [needs: nexus-audit + nexus-tech-seo]
 6.  Optimize existing content               [needs: nexus-audit + nexus-content]
 7.  Competitor analysis                     [needs: nexus-research]
 8.  Full SEO strategy + roadmap             [needs: nexus-research + nexus-strategy]
 9.  Semantic SEO (topic clusters + entities) [needs: nexus-research + nexus-strategy]
10.  Programmatic SEO (template pages)       [needs: nexus-strategy + nexus-content]
11.  Tech SEO audit (source + GSC + PageSpeed) [needs: nexus-tech-seo]
12.  Content portfolio status                [needs: nexus-strategy]
13.  Single skill (just run 1 specific Nexus skill)

Pick 1 or combine: "1, 3, 7" or "write blog + competitor analysis"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If a menu option requires a skill that ISN'T installed, mark it grayed-out / unavailable and tell user which skill to install.

If user's intent is already crystal clear from prompt: skip menu, pre-confirm the matching option, proceed on user 'go'.

### Step 5 — Heads-Up Block (orchestration plan)

Show the full chain plan:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS ORCHESTRATOR — HEADS-UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Brand:                  [name]
Goal:                   [classified goal]
Skills to chain:        [ordered list]

Chain plan:
  1. [skill A] — [mode] — [purpose in chain]
  2. [skill B] — [mode] — [consumes skill A's output]
  3. [skill C] — [mode] — [consumes skill A+B output]
  ...

Inputs you provided: [list]
Inputs needed but missing: [list]
Estimated total steps across all chained skills: [N]
Estimated context use: ~[N]K tokens

Proceed? Reply 'go' / 'change goal' / 'cancel'.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 6 — Chained Execution with Accountability

Run the chain via `master-orchestrator.skill.md` and `auto-pilot.skill.md` with these added enforcements:

1. **Mandatory output-block enforcement** (per `pipeline-accountability.skill.md`):
   - Every chained skill must emit its output blocks
   - Next skill in chain verifies the previous skill's output blocks exist before consuming
   - If absent: halt, report, do not proceed

2. **Drift detection** (per `drift-detector.skill.md`):
   - Watch the orchestrator's own output stream
   - If it starts producing prose without invoking a skill that should run: stop, report drift, re-enter pipeline at missed point
   - This catches the "Claude reads pipeline and skips to writing" failure mode

3. **Cross-skill output handoff**:
   - Each skill's structured output is consumed by next skill
   - Standard handoff format: structured deliverables in `[SKILL OUTPUT]` blocks

### Step 7 — Final Pipeline Accountability Log (mandatory)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXUS ORCHESTRATOR — PIPELINE ACCOUNTABILITY LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Goal:                       [used]
Brand:                      [name]
Skills planned:             [list]
Skills executed:            [list with checkmarks]
Skills skipped:             [list with reasons]
Drift events detected:      [N — each with where + correction taken]
Output blocks verified:     [N of N expected]
Confidence:                 [HIGH/MEDIUM/LOW — lowest of any chained skill]
Pipeline integrity:         COMPLETE / PARTIAL [reason]
Total execution context:    ~[N]K tokens
Output delivered:           [type]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CHAIN MAPS BY GOAL

### Goal 1 — Write a Blog Post

```
nexus-research (full_research_package mode)
  ↓ (research package consumed by content)
nexus-content (standard_blog mode, with research package as input)
  ↓ (generated content optionally audited)
nexus-audit (full_audit mode, optional final QA)
```

### Goal 2 — Landing Page

```
nexus-research (serp_analysis mode for the keyword)
  ↓
nexus-content (landing_page mode)
```

### Goal 4 — Audit URL

```
nexus-audit (full_audit mode)
  + (optional)
nexus-tech-seo (ranking_diagnosis mode if URL provided + tech inputs)
```

### Goal 5 — Why Isn't [URL] Ranking?

```
nexus-audit (full_audit mode)
  + (parallel)
nexus-tech-seo (full_audit mode)
  ↓ (cross-reference outputs)
nexus-tech-seo (content_tech_bottleneck mode — final synthesis)
```

### Goal 6 — Optimize Existing Content

```
nexus-audit (full_audit mode for the URL — establishes baseline)
  ↓ (audit output informs optimization)
nexus-content (content_refresh mode — applies improvements within ranking-status-check constraints)
```

### Goal 8 — Full SEO Strategy

```
nexus-research (full_research_package — assembles current state + opportunities)
  ↓ (research feeds strategy)
nexus-strategy (full_strategy mode — builds strategy + roadmap)
```

### Goal 9 — Semantic SEO

```
nexus-research (semantic_seo mode)
  ↓
nexus-strategy (topical_authority mode)
```

### Goal 10 — Programmatic SEO

```
nexus-research (keyword_research — for the keyword matrix)
  ↓
nexus-strategy (programmatic_seo mode — template + quality gates + rollout plan)
  ↓ (template approved by user)
nexus-content (standard_blog mode — generates pages from template)
```

### Goal 11 — Tech SEO Audit

```
nexus-tech-seo (full_audit mode)
```

### Goal 12 — Content Portfolio Status

```
nexus-strategy (content_portfolio_status mode)
```

### Multi-goal selection (e.g., user picks "1, 3, 7")

Orchestrator orders by dependency:
- Research goals (3, 7, 9) run first
- Generation goals (1, 2, 10) consume research
- Evaluation goals (4, 5, 6, 12) run on existing or just-generated content
- Strategy (8) synthesizes everything

Shared steps run ONCE (deduplication). Output reused across goals.

---

## REFERENCE FILE MAP

| File | Role |
|---|---|
| `task-router.skill.md` | Intent detection, request classification (from existing Nexus) |
| `nexus-connector.skill.md` | Pipeline orchestration, dependency mapping (from existing Nexus) |
| `master-orchestrator.skill.md` | Token-aware controller + final report assembly (from existing Nexus) |
| `auto-pilot.skill.md` | Phase 0 pre-flight + execution + handoff (from existing Nexus) |
| `execution-mode.skill.md` | FULL / PARTIAL / SINGLE logic (from existing Nexus) |
| `output-formatter.skill.md` | Final output formatting (from existing Nexus) |
| `installed-skill-detector.skill.md` (NEW) | Detects which Nexus skills are installed |
| `pre-flight-check.skill.md` (NEW) | Verifies resources before chaining |
| `pipeline-accountability.skill.md` (NEW) | Enforces output blocks, generates accountability log |
| `drift-detector.skill.md` (NEW) | Catches Claude skipping skills mid-execution |

---

## EXECUTION RULES

1. **Optional layer.** This skill adds value when 2+ Nexus skills installed. Solo install does nothing useful.
2. **Detect installed skills first.** Don't offer menu options that aren't available.
3. **Heads-up before chaining.** User sees the full chain before any step runs.
4. **Mandatory output blocks.** Every chained skill emits structured outputs; next skill verifies before consuming. No silent skips.
5. **Drift detection always on.** Catches Claude going off-rails mid-pipeline.
6. **Pipeline accountability log mandatory.** Every chained run produces the final log. Even partial runs.
7. **Brand-required for chaining.** Orchestration requires brand context. Brand-agnostic chains are blocked.
8. **Memory shared via files.** Orchestrator coordinates skills via shared files (brand profile, author profile, generated outputs); no proprietary IPC.
9. **Single skill mode allowed.** Option 13 lets user fire one specific skill without chaining.
10. **Goal-aware deduplication.** When user picks multiple goals, shared steps run once.

---

## QUICK-INVOKE EXAMPLES

| User says | What runs |
|---|---|
| "Write a blog on [topic]" | Goal 1 chain (research + content + audit) |
| "Why isn't [URL] ranking?" | Goal 5 chain (audit + tech-seo + bottleneck synthesis) |
| "Full SEO strategy for [brand]" | Goal 8 chain (research + strategy) |
| "Optimize this article: [URL]" | Goal 6 chain (audit + content refresh) |
| "Just keyword research for [topic]" | Goal 3 (single skill: nexus-research keyword mode) |
| "Audit my site tech SEO + GSC + PageSpeed" | Goal 11 (single skill: nexus-tech-seo) |
| "Run full nexus pipeline" / "Do everything for [brand]" | Multi-goal: 8 + 1 + 12 (full strategy + new content + portfolio refresh) |

---

*Nexus Orchestrator — v1.0.0 | Optional automation layer | Skill-aware menu | Mandatory accountability + drift detection*
