# installed-skill-detector.skill

**Role:** Detect which Nexus skills are present in the user's session/project so the orchestrator only offers menu options for installed skills.

**Loaded by:** `nexus-orchestrator` SKILL.md as the first step (before menu display)

---

## Why this skill exists

If orchestrator offers a menu option that requires `nexus-content` and the user hasn't installed it, the chain will fail mid-execution with confusing errors. Better: detect what's installed up front, only offer compatible options, and tell user which skills they'd need for the unavailable options.

---

## Inputs

None (this skill inspects the current session context).

---

## Process

### Step 1 — Inspect session for Nexus skill files

Look for these skill files in current session context (loaded as plugins, in skill directories, or as uploaded files):

- `nexus-research` — look for `nexus-research/SKILL.md` or skill descriptor `name: nexus-research`
- `nexus-content` — look for `nexus-content/SKILL.md` or descriptor
- `nexus-audit` — look for `nexus-audit/SKILL.md` or descriptor
- `nexus-tech-seo` — look for `nexus-tech-seo/SKILL.md` or descriptor
- `nexus-strategy` — look for `nexus-strategy/SKILL.md` or descriptor

For each detected skill:
- Note its version (from frontmatter)
- Note its file path / location
- Verify it has the expected entry sequence (heads-up pattern, mode detection)

### Step 2 — Build installed-skills map

Output structured map:
```
{
  "nexus-research": {"installed": true/false, "version": "X.Y.Z" or null, "path": "..." or null},
  "nexus-content": {"installed": true/false, ...},
  "nexus-audit": {"installed": true/false, ...},
  "nexus-tech-seo": {"installed": true/false, ...},
  "nexus-strategy": {"installed": true/false, ...}
}
```

### Step 3 — Classify orchestration capability

Based on installed skills, determine orchestration capability:

| Installed | Capability |
|---|---|
| 0 skills | Orchestrator can't do anything — exit gracefully, tell user to install ≥1 Nexus skill |
| 1 skill | Orchestrator can run that skill but no chaining value — recommend invoking directly |
| 2+ skills | Full orchestration possible — proceed to menu |

### Step 4 — Map menu options to availability

Cross-reference installed skills with menu options:

| Menu # | Required skills | Available if |
|---|---|---|
| 1 (blog) | research + content | both installed |
| 2 (landing page) | content | content installed |
| 3 (keyword research) | research | research installed |
| 4 (audit) | audit | audit installed |
| 5 (why not ranking) | audit + tech-seo | both installed |
| 6 (optimize) | audit + content | both installed |
| 7 (competitor) | research | research installed |
| 8 (full strategy) | research + strategy | both installed |
| 9 (semantic SEO) | research + strategy | both installed |
| 10 (programmatic) | strategy + content | both installed |
| 11 (tech SEO) | tech-seo | tech-seo installed |
| 12 (portfolio) | strategy | strategy installed |
| 13 (single skill) | any | any skill installed |

Mark each menu option AVAILABLE / UNAVAILABLE with reason.

### Step 5 — Version compatibility check

For installed skills, check that all are at compatible major versions (1.x.y compatible with 1.x.y).

If versions diverge across installed skills, warn user:
"Detected version mismatch: [skill A v1.0] vs [skill B v2.0]. Some chains may not work as expected. Update all to same major version."

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: installed-skill-detector
Status: COMPLETE
Inputs consumed: session context

INSTALLED NEXUS SKILLS:
  ✓ nexus-research v[X.Y.Z]      [path]
  ✓ nexus-content v[X.Y.Z]       [path]
  ✓ nexus-audit v[X.Y.Z]         [path]
  ✗ nexus-tech-seo               [not installed]
  ✗ nexus-strategy               [not installed]

ORCHESTRATION CAPABILITY: [FULL / LIMITED / NONE]

MENU OPTIONS AVAILABILITY:
  1. Write a blog post:                 ✓ AVAILABLE (research + content installed)
  2. Write landing page:                ✓ AVAILABLE (content installed)
  3. Keyword research:                  ✓ AVAILABLE (research installed)
  4. Audit URL:                         ✓ AVAILABLE (audit installed)
  5. Why isn't [URL] ranking?:          ✗ UNAVAILABLE (needs tech-seo — not installed)
  6. Optimize content:                  ✓ AVAILABLE (audit + content installed)
  7. Competitor analysis:               ✓ AVAILABLE (research installed)
  8. Full SEO strategy:                 ✗ UNAVAILABLE (needs strategy — not installed)
  9. Semantic SEO:                      ✗ UNAVAILABLE (needs strategy — not installed)
  10. Programmatic SEO:                 ✗ UNAVAILABLE (needs strategy — not installed)
  11. Tech SEO audit:                   ✗ UNAVAILABLE (needs tech-seo — not installed)
  12. Content portfolio status:         ✗ UNAVAILABLE (needs strategy — not installed)
  13. Single skill:                     ✓ AVAILABLE (any installed skill can run alone)

VERSION COMPATIBILITY: [PASS / WARN — list mismatches]

NEXT STEP: Display menu showing only AVAILABLE options. Mark UNAVAILABLE options with install hints.

[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **Inspect, don't assume.** Detect what's actually present, not what user says is installed.
2. **Graceful degradation.** Show what's possible, mention what's not (with install hint).
3. **Version compatibility matters.** Major version mismatches can break chains; warn user.
4. **Single-skill-installed = direct invocation recommended.** Orchestrator adds no value with 1 skill; tell user to invoke directly.
5. **Zero skills installed = exit gracefully.** Tell user this orchestrator only adds value with other Nexus skills present.
