# user-research-injector.skill

**Role:** Ingest user-provided research (interview transcripts, proprietary data, internal benchmarks, client documents, original studies, survey results) and inject it into the content generation pipeline as Priority 1 evidence. Generic claims that contradict user data get flagged. Claims that can be supported by user data get cited.

**Loaded by:** `nexus-content` SKILL.md in all generation modes when user research is provided

---

## Why this skill exists

User-supplied research is the highest-value input for differentiated content. A blog post citing the user's own customer interview data, internal benchmark, or proprietary survey results contains evidence no competitor can replicate. This is the single biggest ranking differentiator — and the easiest to ignore without a structured injection pipeline.

This skill turns "here's some research" into structured evidence the content engine actively uses.

---

## Inputs

Required:
- User-provided research artifacts (any format):
  - Interview transcripts (text)
  - Data tables (CSV, JSON, pasted)
  - Survey results (PDF, slides, pasted)
  - Internal benchmark reports
  - Client case studies (with permission)
  - Original research notes
  - Customer feedback compilations
  - Internal product usage data

Optional:
- Source attribution preferences (cite as "[Brand] internal data" vs "based on a recent survey we conducted of N users")
- Confidentiality flags (some data may need to be cited generically without specifics)

---

## Process

### Step 1 — Parse and structure user research

For each provided artifact:

**Identify what the artifact contains:**
- Quantitative data (specific numbers, percentages, counts, time series)
- Qualitative data (quotes, themes, observations)
- Findings (conclusions, insights, recommendations)
- Methodology (how the research was conducted — needed for credibility)

**Extract usable claims:**
- For each data point, note: what it measures, what value, when collected, sample size, methodology
- For each qualitative quote, note: the quote, source role/context, theme it illustrates
- For each finding, note: the finding, supporting evidence, confidence level

**Build evidence package:**
```
EVIDENCE PACKAGE:
  Quantitative claims: [N]
    1. Metric: [X], Value: [Y], Sample: [N], Method: [...], Date: [...]
    2. ...
  Qualitative quotes: [N]
    1. Quote: "[...]", Source: [role], Theme: [...]
    2. ...
  Findings: [N]
    1. Finding: [...], Evidence: [supporting data], Confidence: [HIGH/MEDIUM/LOW]
    2. ...
  Methodology summary: [if available — needed for credibility]
```

### Step 2 — Map evidence to content blueprint

For the proposed content outline:
- For each section, identify: which user research evidence is relevant?
- Build a section-by-section evidence map
- Mark sections that have NO supporting user research vs sections that have rich support

### Step 3 — Inject evidence into generation

During content generation, the content-generation-engine reads from this evidence package as **Priority 1**:

**For quantitative claims:**
- Where content needs a number, use user data first (with attribution)
- Generic statistic ("Studies show X% of users") should be replaced with user data ("Our internal survey of N users showed X%")
- Cite methodology where relevant for credibility

**For qualitative quotes:**
- Where content benefits from a customer voice, use user-provided quotes (with attribution and context)
- Real quotes are far more credible than synthesized "customer says" quotes

**For findings:**
- Where content makes claims, prioritize user findings over generic industry consensus
- Distinguish between user findings (proprietary, citable as primary) and industry consensus (secondary)

### Step 4 — Conflict detection

If a generic claim in the content contradicts user research:
- Flag the conflict
- Default: trust user research over generic training-data claim
- Surface the conflict to user for confirmation if it's a major divergence

Example:
- Content draft says: "Most teams adopt Playwright over Cypress for speed"
- User research shows: "Internal benchmark of 50 teams: 60% chose Cypress over Playwright"
- Conflict: surfaced for resolution. Default action: rewrite content to reflect user data.

### Step 5 — Attribution formatting

Apply user's attribution preferences:
- Specific: "Our 2024 customer survey of 1,247 product managers found X" — high credibility, specific
- Semi-specific: "Based on our research with hundreds of teams, X" — good credibility, less specific
- Generic: "We've found X in practice" — lower credibility, but acceptable for less critical claims
- Cite original methodology where relevant

For confidential data, use attribution that respects confidentiality:
- "An anonymous survey of N PMs found X" rather than naming clients

### Step 6 — Differentiation amplification

User research is the primary source of unique differentiation angles. Mark each user-research-backed claim as:
- ANGLE_NOT_IN_TOP_10 (high differentiation value)
- ANGLE_BARELY_COVERED_IN_TOP_10 (moderate differentiation value)
- ANGLE_COMMON_IN_TOP_10 (low differentiation value but adds your specific data)

These flags feed into differentiation-enforcer.

---

## Output

```
[NEXUS-STEP-N OUTPUT]
Skill: user-research-injector
Status: COMPLETE
Inputs consumed: [list of artifacts provided]

EVIDENCE PACKAGE BUILT:
  Quantitative claims extracted: [N]
  Qualitative quotes extracted: [N]
  Findings extracted: [N]
  Methodology summary: [available / not available]

EVIDENCE-TO-CONTENT MAPPING:
  Total content sections: [N]
  Sections with user-research support: [N]
  Sections without user-research support: [N — these will use generic evidence]

  Per-section evidence map:
    Section 1 [name]: [evidence items mapped — list]
    Section 2 [name]: [...]
    ...

INJECTION ACTIONS DURING GENERATION:
  Quantitative claims using user data: [N — replacing generic stats]
  Qualitative quotes using user data: [N — replacing synthesized quotes]
  Findings using user research: [N — replacing generic consensus]

CONFLICT DETECTION:
  Generic content claims that contradict user data: [N — list each with resolution]
    1. Generic claim: "[X]"
       User data: "[Y]"
       Resolution: [USE_USER_DATA / FLAGGED_FOR_USER_REVIEW]

ATTRIBUTION:
  User-data claims attributed as: [specific/semi-specific/generic per user preference]
  Confidentiality respected: [yes/no — examples]

DIFFERENTIATION VALUE:
  ANGLE_NOT_IN_TOP_10: [N user-research claims contribute unique angles]
  ANGLE_BARELY_COVERED: [N]
  ANGLE_COMMON: [N]

IMPACT ON DIFFERENTIATION-ENFORCER:
  User-research claims contribute [N] of the required ≥3 unique angles for this content type.

CONFIDENCE: HIGH (rich user research, methodology clear) / MEDIUM (research provided but methodology partial) / LOW (research thin or unstructured)
[/NEXUS-STEP-N OUTPUT]
```

---

## Behaviors

1. **User research is Priority 1.** When user data exists, use it. Generic claims supplement, not replace.

2. **Don't fabricate methodology.** If user provides data without methodology, attribute carefully ("based on our internal data") and don't invent N values or sampling claims.

3. **Conflicts default to user data.** When user research contradicts generic training-data claims, trust user. Flag major conflicts for confirmation.

4. **Respect confidentiality.** User-flagged confidential data gets generic attribution; never name confidential clients/sources.

5. **Surface differentiation value explicitly.** User research often produces the highest-differentiation content — make this visible so user knows what's unique.

6. **Section-level mapping matters.** Show user which sections have research support and which don't — they may want to provide more research for thin sections.

7. **Methodology builds credibility.** Where methodology is available, include it. "Based on a 2024 survey of 1,247 PMs using random sampling" beats "based on our research."
