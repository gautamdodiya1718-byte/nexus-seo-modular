# SKILL: orchestration/master-orchestrator.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Orchestration / Pipeline Control
# EXECUTION PRIORITY: ENTRY POINT — This skill manages the complete content production pipeline. It can auto-chain all skills or allow manual invocation.
# POSITION: Token-optimization-aware orchestration layer that breaks large tasks into manageable steps, chains skills in the correct order, and produces a final consolidated report with scores across all dimensions.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **conductor** of the entire Nexus content production system. It knows which skills to invoke, in what order, and when to break work into separate steps to manage token budgets and context window limitations.

### Two Operating Modes

| Mode | Trigger | Behavior |
|---|---|---|
| **AUTO-CHAIN** | User says "write a full article on [X]" or "run the full pipeline" | Executes all skills in sequence, pausing for user approval at key checkpoints |
| **MANUAL** | User invokes a specific skill by name | Runs only the requested skill(s), provides integration recommendations |

### Token Awareness
This skill monitors task complexity and proactively breaks work into phases when needed:

| Complexity Level | Indicator | Strategy |
|---|---|---|
| **Light** | Single skill, short content, simple topic | Run in 1 session |
| **Medium** | 3-5 skills, standard article, moderate research | Run in 1-2 sessions |
| **Heavy** | Full pipeline, long-form content, extensive research | Break into 3-5 separate sessions, consolidate at end |
| **Massive** | Full pipeline + code examples + infographics + GSC analysis | Break into 5-8 sessions with clear handoff points |

When breaking into sessions, the orchestrator:
1. Tells the user: "This task requires extensive work. I'll break it into [N] phases."
2. Defines each phase with clear inputs and outputs
3. Saves intermediate outputs for the next phase
4. At the end, consolidates everything and produces the final report

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — FULL CONTENT PIPELINE (AUTO-CHAIN)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Phase 1: Research & Planning
```
[01] content-research-engine → Comprehensive content brief
     ↓
[02] User reviews brief → APPROVE / REVISE
```
**Checkpoint 1:** User approves brief + outline before writing begins.

**After Checkpoint 1 approval: everything from Phase 2 through Phase 5 is AUTONOMOUS.
No further stops. Run all remaining steps without pausing.**

### Phase 2: Content Generation
```
[03] content-generation-engine → Full draft
     ↓
[04] code-generation-preview → Generate + run + screenshot all code examples
     ↓
[05] real-world-examples → Find + validate real-world evidence
     ↓
[06] content-engagement → Audit pacing, insert visual break markers
     ↓
[07] infographic-image-engine → Generate all infographics + find YT videos
```

### Phase 3: Product Integration & Optimization
```
[08] Brand product mentions → Insert brand references + CTAs (read from brand file)
     ↓
[09] internal-linking → Place internal links per brand.site_size:
     small (under 50 pages): 4-6 links
     medium (50-200 pages): 7-10 links
     large (200+ pages): 12+ links
     Read brand.site_size from active brand file before setting the target.
     ↓
[10] humanizer → Remove AI patterns, enforce writing rules
     ↓
[11] technical-accuracy-checker → Verify all technical claims
```

### Phase 4: Quality Assurance
```
[12] seo-aeo-geo-sxo-optimizer → Score across all 4 paradigms
     ↓
[13] content-validation → Final quality gate (E-E-A-T, completeness)
     ↓
[14] metadata-generator → Title, meta, excerpt, OG, schema
     ↓
[15] blog-post-auditor → Pre-publish forensic audit (optional, for FULL mode)
```

### Phase 5: Final Report
```
[16] Consolidate all outputs into FINAL CONTENT REPORT
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — FINAL CONTENT REPORT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

After all phases complete, produce:

```
═══════════════════════════════════════
FINAL CONTENT REPORT
Article: [title]
Keyword: [primary keyword]
Date: [date]
═══════════════════════════════════════

## CONTENT SUMMARY
- Word count: [N]
- Sections (H2s): [N]
- Reading time: ~[N] minutes

## MEDIA INVENTORY
- Images/infographics: [N] (types: [list])
- Code examples: [N] (all validated: [YES/NO])
- Code preview screenshots: [N]
- YouTube video embeds: [N]
- Tables: [N]
- Callout boxes: [N] (Tip: [N], Warning: [N], Insight: [N], Brand Feature: [N])
- GIF suggestions for user: [N]

## REAL-WORLD EXAMPLES
- Verified examples: [N]
- Synthetic examples: [N]
- Sources: [list company names + types]

## BRAND INTEGRATION
- Funnel stage: [TOFU/MOFU/BOFU]
- Mentions: [N] (Soft: [N], Medium: [N])
- CTAs: [N] (placement: [list])
- Features referenced: [list]

## LINK INVENTORY
- Internal links: [N] (unique destinations: [N])
- External links: [N] (all nofollow: [YES/NO])
- Links in intro (first 300 words): [N]

## OPTIMIZATION SCORES
| Paradigm | Score | Grade |
|---|---|---|
| SEO | [X]/100 | [grade] |
| AEO | [X]/100 | [grade] |
| GEO | [X]/100 | [grade] |
| SXO | [X]/100 | [grade] |
| **OVERALL** | **[X]/100** | **[grade]** |

## ENGAGEMENT SCORE
- Visual-to-text ratio: [X] (target: 1 per 250-350 words)
- Max consecutive text: [N] words (target: under 300)
- Element variety: [N] types used

## METADATA
- Title: [title] ([N] chars)
- Meta description: [description] ([N] chars)
- Excerpt: [excerpt] ([N] chars)
- URL slug: /blog/[slug]/
- Schema types: [list]
- OG image: [suggestion]

## CONTENT QUALITY
- Humanizer pass: [CLEAN / X flags]
- Technical accuracy: [VERIFIED / X issues]
- Content validation: [PASS / X issues]
- Blog audit score: [X]/100 (if run)

## REMAINING ACTION ITEMS
[Things the user needs to do manually:]
- [ ] Record GIF: [description] — for section [X]
- [ ] Upload hero image
- [ ] Review + publish
═══════════════════════════════════════
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — MANUAL INVOCATION MODE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

When the user invokes a specific skill:
1. Run that skill only
2. Show the output
3. Suggest which related skills would add value: "You ran content-engagement. Related skills that would complement this: code-generation-preview (for code breaks), infographic-image-engine (for visual breaks)."
4. Let the user decide what to run next

### Quick-Invoke Map (New Skills)

| User Says | Skill(s) to Run |
|---|---|
| "Research [keyword]" | content-research-engine |
| "Generate code for [topic]" | code-generation-preview |
| "Create an infographic for [concept]" | infographic-image-engine |
| "Find real examples of [topic]" | real-world-examples |
| "Check engagement pacing" | content-engagement |
| "Add brand mentions" | brand-[name].skill.md (mention_frequency + mention_style fields) |
| "Optimize for all search types" | seo-aeo-geo-sxo-optimizer |
| "Why isn't this ranking?" | ranking-diagnostics |
| "Analyze GSC data" | gsc-integration |
| "Check all links are correct" | official-links-repository (verification mode) |
| "Verify brand features" | brand-[name].skill.md (features field — verify maturity labels before publishing) |
| "Run full pipeline" | This skill (AUTO-CHAIN mode) |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — TOKEN OPTIMIZATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### General Rules
1. **Load only necessary reference files.** Don't load all 50+ skill files at once.
2. **Batch related operations.** Run all code examples together, all infographics together.
3. **Save intermediate outputs.** When breaking into phases, save phase outputs as files.
4. **Don't re-research.** If content-research-engine already ran, downstream skills use its brief — don't re-search.
5. **Compress outputs between phases.** Pass only the essential data between phases, not full skill outputs.

### Phase Break Decision Logic
```
Estimated tokens for full pipeline > 80% of context window?
  → YES → Break into phases
  → NO → Run in single session

Number of code examples to generate > 5?
  → YES → Run code generation as a separate phase
  → NO → Include in content generation phase

Number of infographics to generate > 3?
  → YES → Run infographic generation as a separate phase
  → NO → Include in content generation phase
```

---

*Master Orchestrator — v1.0.0 | Nexus SEO Operating System*
