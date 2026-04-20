# SKILL: content/technical-accuracy-checker.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Post-Generation Processing
# EXECUTION PRIORITY: POST-HUMANIZATION — Runs after humanizer.skill and before content-validation.skill.
# POSITION: Factual integrity gate. Every claim, statistic, code block, tool reference, and technical assertion must pass through this skill before content reaches validation.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **factual and technical integrity layer** of the Nexus content pipeline.

AI-generated content carries a specific category of risk that tone improvement and structural validation cannot address: the risk of publishing factually incorrect, technically wrong, or hallucinated information as if it were authoritative. An article that ranks well but contains incorrect tool instructions, outdated API syntax, fabricated statistics, or contradictory technical claims damages the site's credibility with readers and erodes the E-E-A-T signals that determine long-term ranking stability. A single incorrect step in a tutorial, a wrong version number, or a statistic that does not trace to a real source is enough to undermine an otherwise excellent piece.

This skill systematically extracts every verifiable claim in the content, maps each claim to its validation source, confirms accuracy against that source, corrects what is incorrect, and flags what cannot be verified. It produces a corrected article, a complete corrections log, and a confidence score that tells the content team and downstream skills exactly how much of the article was verified and at what level of certainty.

This skill is mandatory for all content that contains tool-specific instructions, code, API references, framework usage, statistics, pricing data, named feature descriptions, version-specific claims, and any factual assertion that a reader might act on.

### Primary Responsibilities

| Responsibility                         | Description                                                                                                           |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Verifiable Section Identification**  | Scan the full article and extract every sentence or block containing a verifiable factual claim                       |
| **Source Mapping**                     | For each verifiable claim, identify the most authoritative source for validation                                      |
| **Accuracy Validation**                | Check each claim for correctness, completeness, and currency — identifying wrong, incomplete, and outdated content    |
| **Correction Layer**                   | Rewrite incorrect claims with accurate information; reference the correction's source                                 |
| **Code Verification**                  | Validate code snippets for syntax correctness, logical correctness, and real-world executability                     |
| **Consistency Check**                  | Detect internal contradictions — where two claims in the same article cannot both be true                            |
| **Confidence Scoring**                 | Produce a 0–100% confidence score reflecting the proportion and severity of verified vs. unverified claims            |
| **Hallucination Detection**            | Identify claims that cannot be traced to any known source and are likely fabricated by the generation process          |
| **Outdated Information Detection**     | Flag claims that were accurate when the model was trained but are no longer current                                   |
| **Cross-Source Validation**            | When a claim appears in multiple sources with conflicting information, identify the conflict and resolve it            |
| **Corrections Log Production**         | Document every issue found — original claim, problem type, corrected version, and source                             |

### Mandatory Application Triggers

This skill is **always invoked** for content containing:

| Content Type                         | Why Mandatory                                                                   |
|--------------------------------------|---------------------------------------------------------------------------------|
| Tool-specific instructions            | Incorrect steps cause reader frustration and loss of trust                     |
| Code snippets or CLI commands         | Incorrect code causes operational failures for readers who execute it          |
| API references or endpoint syntax     | APIs change — outdated references break integrations                           |
| Framework or library usage            | Version-specific syntax differences can make instructions non-functional        |
| Named feature descriptions            | Features change, get renamed, or are deprecated                                |
| Pricing or plan data                  | Pricing changes frequently — outdated pricing misinforms purchase decisions    |
| Statistics or data points             | Fabricated or unsourced statistics damage credibility                          |
| Named competitor comparisons          | Incorrect competitor feature claims carry legal and reputational risk           |
| Regulatory or compliance claims       | Incorrect compliance information can have legal consequences for readers       |
| Medical, financial, or legal content  | Any inaccuracy in these categories produces direct reader harm                 |

### What This Skill Is NOT

- It is **not** a plagiarism checker. It validates factual accuracy — not content originality.
- It is **not** a style editor. Phrasing decisions made by `humanizer.skill` are not reversed here unless they introduced a factual error.
- It is **not** a live web search tool. It validates against knowledge base, provided source of truth, and cross-reference reasoning — it does not perform live search queries during each run.
- It is **not** optional for technical content. Technical content without accuracy verification is not production-ready.
- It is **not** a content quality assessor. It evaluates correctness — not depth, completeness, or readability.

### The Defining Standard of This Skill

> This skill answers one question about every verifiable claim:
>
> "Is this claim correct, current, and traceable to a source a reader could verify?"
>
> If YES → the claim passes verification.
> If NO (incorrect) → the claim is corrected with accurate information from the correct source.
> If UNCERTAIN (cannot be verified or refuted) → the claim is flagged UNVERIFIED.
> If FALSE (demonstrably wrong) → the claim is corrected, not merely flagged.
>
> "It may be incorrect" is never an acceptable output.
> Every finding must be specific: what is wrong, what is right, and where the right answer comes from.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                     | Description                                                                             | Required |
|---------------------------|-----------------------------------------------------------------------------------------|----------|
| `generated_content`       | The humanized article produced by `humanizer.skill` — the full article text            | YES      |
| `topic`                   | The article's target keyword or topic — used to establish domain context for validation | YES      |

### Optional Inputs

| Field                     | Description                                                                    | Used For                                                          |
|---------------------------|--------------------------------------------------------------------------------|-------------------------------------------------------------------|
| `source_of_truth`         | URLs, documents, or named data sources provided by the user                   | Step 2 (Source Mapping) — highest-priority validation reference   |
| `tool_names`              | List of specific tools or software named in the content                       | Source identification — official documentation lookup            |
| `version_context`         | Specific tool/framework version numbers the content is written for            | Outdated information detection — version-specific validation     |
| `content_type`            | Technical tutorial / Comparison / Conceptual guide / Tool review              | Adjusting validation depth — tutorials require stricter code checks |
| `niche_context`           | Domain: Medical / Legal / Financial / Technical / General                     | Mandatory application trigger escalation for regulated domains   |
| `knowledge_cutoff_date`   | The generation model's knowledge cutoff — used for outdated information flags  | Outdated detection — everything after this date is flagged       |

### Source of Truth Priority Hierarchy

When validating a claim, sources are consulted in this priority order:

| Priority | Source Type                                          | Reliability              |
|----------|------------------------------------------------------|--------------------------|
| 1        | User-provided `source_of_truth` URLs or documents   | HIGHEST — user-confirmed |
| 2        | Official product documentation (`docs.*/developer.*`)| HIGH — primary source   |
| 3        | Official company websites for product/feature claims | HIGH — authoritative    |
| 4        | Official standards bodies for protocol/standard claims | HIGH — authoritative  |
| 5        | Peer-reviewed publications for scientific claims      | HIGH — verified         |
| 6        | Cross-source consensus (same claim in 3+ independent authoritative sources) | MEDIUM-HIGH |
| 7        | Widely-used technical references (MDN, RFC docs, etc.) | MEDIUM-HIGH            |
| 8        | Knowledge base inference (claim patterns consistent with established facts) | MEDIUM |
| 9        | No source available                                   | UNVERIFIED — flag only  |

### Input Pre-Conditions

1. **Is `generated_content` present and non-empty?** If not → halt. No content to validate.
2. **Has the content already been humanized?** If `humanizer.skill` has not run → log a warning but proceed. Accuracy validation is not dependent on humanization.
3. **Is `source_of_truth` provided?** If yes → load and index it as Priority 1 validation source. If no → proceed with internal validation sources only.
4. **Is `content_type` provided?** If yes → adjust validation depth (tutorials and code-focused content trigger full code verification). If no → infer from content structure (presence of code blocks → trigger code verification; step-by-step structure → trigger tool instruction validation).
5. **Is `niche_context` medical, financial, or legal?** If yes → escalate all claims in those domains to HIGH-PRIORITY verification. Flag any unverified claim in these domains as HIGH RISK.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Technical Accuracy Package

```
NEXUS TECHNICAL ACCURACY REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session ID              : [ID]
Topic / Keyword         : [topic]
Content Type            : [article type]
Niche Context           : [domain]
Total Verifiable Claims : [count]
  Verified Correct      : [count] — [%]
  Corrected             : [count] — [%]
  Unverified (flagged)  : [count] — [%]
  High-Risk Flags       : [count]
Code Blocks Validated   : [count]
  Code: PASS            : [count]
  Code: CORRECTED       : [count]
  Code: NEEDS VALIDATION: [count]
Confidence Score        : [N]% — [HIGHLY ACCURATE / MOSTLY ACCURATE / NEEDS REVIEW / UNRELIABLE]
Generated By            : content/technical-accuracy-checker.skill
Timestamp               : [ISO 8601]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[VALIDATED + CORRECTED ARTICLE CONTENT]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CORRECTIONS LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Section 3 — Corrections Log Format]

UNVERIFIED CLAIMS (VERIFY BEFORE PUBLISHING)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[See Section 3 — Unverified Claims Log Format]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Corrections Log Format

```
CORRECTION LOG ENTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Issue #         : [sequential number]
Location        : [Section name + paragraph or code block reference]
Claim Type      : [INCORRECT FACT / OUTDATED INFO / CODE SYNTAX / CODE LOGIC / HALLUCINATION / CONTRADICTION / INCOMPLETE]
Original Text   : "[exact quote of the incorrect text]"
Problem         : [specific description of what is wrong — not vague]
Corrected Text  : "[exact corrected replacement]"
Correction Source: [source name / URL / document reference / knowledge base reasoning]
Severity        : [CRITICAL / HIGH / MEDIUM / LOW]
Status          : [CORRECTED / CANNOT CORRECT — user action required]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Unverified Claims Log Format

```
UNVERIFIED CLAIM ENTRY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claim #         : [sequential number]
Location        : [Section name + paragraph reference]
Claim Text      : "[exact quote of the unverified text]"
Claim Type      : [STATISTIC / FEATURE CLAIM / PROCESS CLAIM / GENERAL FACT / REGULATORY / BENCHMARK]
Why Unverified  : [specific reason — source unavailable / claim is too recent / domain-specific knowledge required]
Risk Level      : [HIGH RISK (medical/legal/financial) / MEDIUM RISK / LOW RISK]
Flag in Article : [YES — [VERIFY] marker added to article] / [DISCLOSED inline]
Recommended Action: [specific action for the content team]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Confidence Score Output

```
CONFIDENCE SCORE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Raw Score Calculation:
  Verified Correct  : [count] × 1.0 = [score]
  Corrected (fixed) : [count] × 0.7 = [score]  (deducted for requiring correction)
  Unverified (low risk): [count] × 0.5 = [score] (partial credit — claim may be correct)
  Unverified (high risk): [count] × 0.0 = [score] (no credit — risk too high)
  Code PASS         : [count] × 1.0 = [score]
  Code Corrected    : [count] × 0.7 = [score]
  Code Needs Valid. : [count] × 0.3 = [score]
  Internal Contradiction: -10 points per contradiction pair

Total Points        : [sum]
Maximum Points      : [total verifiable claims × 1.0 + code blocks × 1.0]
Confidence Score    : [Total / Maximum × 100]% = [N]%

Score Interpretation:
  90–100% → HIGHLY ACCURATE — content is safe to publish after reviewing unverified flags
  70–89%  → MOSTLY ACCURATE — minor issues corrected; review unverified sections
  50–69%  → NEEDS REVIEW — significant corrections made; content team should validate key sections
  < 50%   → UNRELIABLE — substantial accuracy problems; major revision recommended before publishing
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — IDENTIFY VERIFIABLE SECTIONS

Scan the entire article and extract every sentence, passage, or block that contains a verifiable factual claim. Not every sentence requires verification — only those that make a claim that could be correct or incorrect.

**Phase 1A — Verifiable Claim Categories**

Extract sentences and blocks matching these categories:

**Category 1 — Technical Claims:**

A technical claim is any assertion about how a system, tool, process, or technology works — including its capabilities, limitations, architecture, or behavior.

Signals:
- Describes what a tool or software does.
- Explains how a technical process operates.
- States that a feature exists, works in a specific way, or does not exist.
- Claims compatibility, integration capability, or restriction.

Examples:
- `"HubSpot CRM supports up to 1,000,000 contacts on the free plan."`
- `"Google Search Console updates performance data with a 2-4 day delay."`
- `"React's useEffect hook runs after every render by default."`

**Category 2 — Tool Usage Steps:**

Any sequential or instructional content describing how to accomplish something with a specific tool, platform, or system.

Signals:
- Numbered steps involving specific product UI elements (menu names, button labels, navigation paths).
- References to specific settings, configuration parameters, or interface locations.
- Instructions that, if incorrect, would cause the reader to fail the task.

Examples:
- `"In HubSpot, navigate to Settings → Account Defaults → Privacy & Consent."`
- `"To enable HTTPS in nginx, add ssl_certificate and ssl_certificate_key directives to your server block."`

**Category 3 — Code Snippets and CLI Commands:**

Any inline code, code block, command-line instruction, query, configuration file excerpt, or API request example.

All code is extracted for Step 5 (Code Verification). No code passes without validation.

**Category 4 — Statistics and Data Points:**

Any numerical claim presented as a factual measure of the real world.

Signals:
- Percentages with a quantitative claim.
- Absolute numbers attributed to a phenomenon.
- Rankings or benchmark data.
- Dollar amounts or financial data.
- Time-based measurements presented as facts.

Examples:
- `"67% of CRM implementations fail within the first year."`
- `"The global CRM market was valued at $58.82 billion in 2022."`
- `"Nucleus Research reports $8.71 ROI for every $1 invested in CRM."`

**Category 5 — Definitions:**

Any statement that defines a technical term, acronym, concept, or named entity.

Signals:
- "X is defined as..."
- "X refers to..."
- "[Term] means..."
- "[Acronym] stands for..."

Definitions are verified for accuracy — a wrong definition of a technical term is a factual error.

**Category 6 — Named Feature Claims:**

Any claim about a specific named feature of a specific named product.

Signals:
- "[Product] has a feature called..."
- "[Tool]'s [Feature Name] allows you to..."
- "[Platform] recently released [Feature]..."
- "[Service] supports [capability]..."

**Category 7 — Version-Specific Claims:**

Any claim that references a specific version number of a tool, framework, library, or operating system.

Signals:
- "As of version X.Y..."
- "In Python 3.11..."
- "This feature was introduced in [Tool] v2.0..."
- "Deprecated in version..."

**Category 8 — Pricing and Plan Data:**

Any claim about the price, plan structure, feature access tier, or trial availability of a product.

Pricing claims have a very high staleness risk and must always be verified or flagged.

**Category 9 — Regulatory and Compliance Claims:**

Any claim about legal requirements, regulatory standards, compliance frameworks, or official certifications.

These claims carry HIGH RISK — an incorrect compliance claim can directly harm a reader who acts on it.

**Phase 1B — Claim Extraction Record**

For each extracted claim, create a record:

```
CLAIM RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claim ID        : [sequential]
Location        : [Section name + paragraph]
Claim Text      : "[exact text]"
Claim Category  : [Category 1–9 from above]
Verifiability   : [VERIFIABLE / CONDITIONALLY VERIFIABLE / LOW CONFIDENCE]
Priority        : [HIGH / MEDIUM / LOW]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 1C — Claim Priority Assignment**

| Category                      | Default Priority | Escalation Condition                                 |
|-------------------------------|------------------|------------------------------------------------------|
| Technical Claims              | MEDIUM           | Tool is named → HIGH                                 |
| Tool Usage Steps              | HIGH             | Always HIGH — reader will execute these              |
| Code Snippets                 | HIGH             | Always HIGH — code errors cause operational failures |
| Statistics                    | MEDIUM           | Source cannot be identified → HIGH                   |
| Definitions                   | LOW              | Technical term in a regulated domain → MEDIUM        |
| Named Feature Claims          | HIGH             | Features change frequently — always HIGH             |
| Version-Specific Claims       | HIGH             | Version is older than 18 months → HIGH               |
| Pricing Data                  | HIGH             | Always HIGH — pricing changes constantly             |
| Regulatory / Compliance       | HIGH RISK        | Always HIGH RISK — no escalation needed              |

---

### ▸ STEP 2 — SOURCE MAPPING

For each extracted claim, identify the most authoritative source for validation.

**Phase 2A — Source Identification by Claim Type**

| Claim Type                        | Primary Source                                             | Secondary Source                                  |
|-----------------------------------|------------------------------------------------------------|---------------------------------------------------|
| Tool features and capabilities    | Official product website (features page, product docs)    | Official blog / changelog / release notes         |
| Tool usage steps and navigation   | Official product documentation (support.product.com, docs.product.com) | Official video tutorials / onboarding guides |
| API endpoints and syntax          | Official API documentation                                 | Official SDK documentation                        |
| Framework / library usage         | Official framework documentation                          | Official GitHub repository README                 |
| Code syntax (language-specific)   | Official language specification or MDN/docs.python.org    | Authoritative technical reference                 |
| Statistics and data points        | Original published research (academic, industry report)   | Official platform-published data reports          |
| Definitions (technical)           | Official specification document or authoritative glossary  | Official documentation defining the term          |
| Pricing and plan data             | Official pricing page (pricing.product.com)               | Official announcement blog post                   |
| Regulatory claims                 | Official regulatory body website (.gov, official standards body) | Legal text of the regulation itself          |
| Named company / product claims    | Official company website or SEC/government filings         | Official press releases                           |
| Scientific/medical claims         | Peer-reviewed publication (PubMed, Cochrane, etc.)        | Meta-analysis or systematic review                |

**Phase 2B — Source Availability Assessment**

For each claim, assess whether the source is available for validation:

| Source Status                                    | Assessment        | Action                                                   |
|--------------------------------------------------|-------------------|----------------------------------------------------------|
| `source_of_truth` provided and covers this claim | SOURCE AVAILABLE  | Validate directly against provided source                |
| Official documentation known and accessible      | SOURCE AVAILABLE  | Validate against official documentation                   |
| Source type identified but specific content unavailable | SOURCE TYPE KNOWN | Apply knowledge-based validation with domain reasoning  |
| No source identifiable                            | SOURCE UNAVAILABLE | Flag claim as UNVERIFIED — cannot validate              |
| Claim references events or data after knowledge cutoff | SOURCE UNAVAILABLE | Flag as POTENTIALLY OUTDATED — cannot verify currency  |

**Phase 2C — Source Record**

For each claim, document the source mapping:

```
SOURCE MAPPING RECORD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claim ID        : [ID]
Source Type     : [Priority level from hierarchy]
Source Name     : [Official docs / user-provided URL / specific publication name]
Source Reference: [URL or document reference if available]
Source Status   : [SOURCE AVAILABLE / SOURCE TYPE KNOWN / SOURCE UNAVAILABLE]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 3 — ACCURACY VALIDATION

For each claim with an identified source, validate the claim against that source. This step produces a determination of CORRECT, INCORRECT, OUTDATED, INCOMPLETE, or UNVERIFIED for every extracted claim.

**Phase 3A — Correctness Validation**

Compare the claim's content against what the source establishes:

| Validation Result | Condition                                                                                  |
|-------------------|--------------------------------------------------------------------------------------------|
| **CORRECT**       | The claim accurately reflects what the source states — including any applicable caveats    |
| **INCORRECT**     | The claim states something that contradicts what the source establishes                   |
| **OUTDATED**      | The claim was accurate at a prior point in time but the source now states something different |
| **INCOMPLETE**    | The claim is partially correct but omits a critical qualifier or condition the source specifies |
| **UNVERIFIED**    | No source is available to confirm or deny the claim                                       |
| **HALLUCINATION** | The claim references a specific feature, statistic, or fact that has no traceable source and appears to be fabricated |

**Phase 3B — Correctness Check Dimensions**

For each claim, apply these three dimensions of correctness:

**Dimension 1 — Factual Correctness:**

Does the claim state the right information?

| Check                                              | Example                                                                                |
|----------------------------------------------------|----------------------------------------------------------------------------------------|
| Named entity exists                                | "HubSpot has a feature called Sales Hub" — does Sales Hub exist?                      |
| Named feature works as described                  | "Sales Hub includes email tracking" — does it, specifically at which tier?             |
| Numerical claims are accurate                     | "CRM market was $58B in 2022" — what does the actual market data show?                |
| Process description matches actual process         | "Navigate to Settings → Privacy" — is that the actual navigation path?                |

**Dimension 2 — Completeness:**

Does the claim include all critical qualifiers that change its practical meaning?

| Missing Qualifier Type          | Example                                                                                        |
|---------------------------------|------------------------------------------------------------------------------------------------|
| Plan/tier qualifier             | "HubSpot CRM is free" — TRUE for basic features, but paid features exist at specific tiers   |
| Version qualifier               | "React hooks are available" — TRUE since React 16.8 specifically                             |
| Condition qualifier             | "This feature is available in the US" — geographic restriction omitted                       |
| Technical constraint qualifier  | "The API supports 1,000 requests per minute" — TRUE for paid plans; lower for free            |

**Dimension 3 — Currency:**

Is the claim still accurate at the present time (or as of the knowledge cutoff)?

| Staleness Indicator                                           | Action                                               |
|---------------------------------------------------------------|------------------------------------------------------|
| Claim references pricing that changes frequently              | Flag as POTENTIALLY OUTDATED — verify currency       |
| Claim references a software version older than 18 months     | Check for version-specific changes                   |
| Claim references a product feature or UI that changes often  | Flag as VERIFY CURRENCY — interface may have changed |
| Claim references a regulation with a date of effect          | Verify the regulation is still in force              |
| Claim references a statistic with a specific year            | Verify the year and look for updated data            |

---

### ▸ STEP 4 — CORRECTION LAYER

For every claim determined to be INCORRECT, OUTDATED, or INCOMPLETE in Step 3 — produce a correction.

**Phase 4A — Correction Types**

| Issue Type      | Correction Approach                                                                                   |
|-----------------|-------------------------------------------------------------------------------------------------------|
| **INCORRECT**   | Replace the incorrect claim with the accurate version. State the correct information from the source. |
| **OUTDATED**    | Update the claim to reflect current information. If current information is unavailable → flag as OUTDATED and annotate in the article. |
| **INCOMPLETE**  | Add the missing qualifier to the existing claim. Do not rewrite the full claim if only the qualifier is missing. |
| **HALLUCINATION**| Remove the claim entirely OR replace it with accurate information from a verified source. Hallucinated specifics (fabricated statistics, invented feature names) must not be allowed to remain. |

**Phase 4B — Correction Writing Standards**

Every correction must:
1. **Preserve the sentence's purpose** — the corrected version must continue to serve the same informational role in the article.
2. **Be specific** — the corrected version contains the actual correct information, not a vague hedge.
3. **Be traceable** — the correction source must be documentable.
4. **Maintain the article's voice** — corrections integrate seamlessly without changing tone or register.

**Prohibited correction forms:**

| Prohibited                                                  | Problem                                               |
|-------------------------------------------------------------|-------------------------------------------------------|
| "This claim may be inaccurate."                             | Vague — no correction provided                        |
| "Please verify this information."                           | Not a correction — delegates to reader                |
| Removing the correctable claim without replacement          | Loss of informational content without substitution    |
| Replacing specific claim with "According to [source]..."   | Only if actual content from that source is provided  |
| Changing "X is $50/month" to "X pricing varies"            | Correction that removes information without providing accurate alternative |

**Phase 4C — Cannot-Correct Handling**

Some claims cannot be corrected because:
- The correct information is not available in the validation sources.
- The claim requires live data (current pricing, current user count) that the system cannot verify.
- The domain requires professional expertise to correct accurately (medical dosages, legal interpretations).

For these cases: mark status as CANNOT CORRECT — USER ACTION REQUIRED in the corrections log. Flag the claim in the article with `[VERIFY]` marker. Do not remove the claim — allow the content team to replace it with verified information.

---

### ▸ STEP 5 — CODE VERIFICATION

Validate every code snippet, command-line instruction, API request example, configuration file excerpt, and query in the article.

**Phase 5A — Code Extraction**

Extract all code blocks and inline code elements. For each:
- Record the programming language or command type.
- Record the claimed purpose ("This code will...")
- Record the context (which section it appears in, what it is demonstrating).

**Phase 5B — Syntax Verification**

Check each code block for syntactic correctness in its stated language:

| Language Category          | Syntax Checks Applied                                                                           |
|----------------------------|-------------------------------------------------------------------------------------------------|
| **Python**                 | Indentation, colon placement, parentheses matching, function call syntax, import statements    |
| **JavaScript / TypeScript**| Bracket matching, semicolons, async/await syntax, arrow function syntax, module imports        |
| **HTML / CSS**             | Tag closure, attribute quoting, property name validity, value format correctness               |
| **SQL**                    | Keyword ordering, table/column reference syntax, JOIN type validity, function call syntax      |
| **Bash / Shell**           | Command existence (for well-known commands), flag syntax, pipe syntax, variable quoting         |
| **JSON / YAML**            | Structural validity, key quoting, value type correctness, nesting validity                     |
| **API calls (curl, etc.)** | Endpoint structure, required parameter presence, header format, method correctness             |
| **Framework-specific**     | Method names, hook usage, decorator syntax (React, Vue, Django, etc.)                          |

**Phase 5C — Logical Verification**

Beyond syntax, verify that the code accomplishes what the article claims it does:

| Logic Check                                          | Example                                                                                        |
|------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Does the code achieve the stated goal?               | A function described as "filtering an array" should actually filter, not map or sort          |
| Are required dependencies imported/required?         | A code block using axios must import axios                                                     |
| Are there obvious runtime errors?                    | Division by zero, null pointer dereference, infinite loop without break condition              |
| Is the logic correct for the described scenario?     | An "if authenticated" check placed after the database query is a security logic error          |
| Are variable names consistent throughout the block?  | A variable declared as `userConfig` but referenced as `user_config` later                    |
| Are the output/result comments accurate?             | Comment says "returns the user ID" but the function returns the whole user object              |

**Phase 5D — Real-World Usability Check**

Verify that the code block is usable by a real reader without additional, undisclosed setup:

| Usability Check                                              | Action if Failed                                              |
|--------------------------------------------------------------|---------------------------------------------------------------|
| Are all required dependencies stated?                        | Add a dependencies note (e.g., "Requires Node.js 18+ and the express package") |
| Is the code version-specific without noting the version?    | Add a version note (e.g., "For Python 3.10+")                |
| Does the code require environment variables not mentioned?  | Add a note specifying required environment variables         |
| Is the code a fragment that cannot run standalone?           | Add a note: "This is a partial example — place within [function/class/file type]" |

**Phase 5E — Code Verification Outcomes**

| Outcome               | Condition                                                            | Action                                                     |
|-----------------------|----------------------------------------------------------------------|------------------------------------------------------------|
| **PASS**              | Syntax correct, logic correct, usable with stated context           | No action — mark as verified                               |
| **CORRECTED**         | Syntax or logic error found and correctable with confidence          | Apply correction; document in corrections log              |
| **NEEDS VALIDATION**  | Code involves domain-specific logic that cannot be verified internally | Add [CODE — NEEDS VALIDATION] marker; flag for expert review |
| **CANNOT VERIFY**     | Code uses proprietary API or platform-specific environment          | Add [CODE — PLATFORM SPECIFIC] marker; recommend testing  |

---

### ▸ STEP 6 — CONSISTENCY CHECK

Detect internal contradictions — places where the article makes two claims that cannot both be true simultaneously.

**Phase 6A — Contradiction Type Detection**

| Contradiction Type                | Detection Method                                                                 |
|-----------------------------------|----------------------------------------------------------------------------------|
| **Factual contradiction**         | Same entity described with two different properties in different sections         |
| **Numerical contradiction**       | Different numbers given for the same measurement in different sections           |
| **Process contradiction**         | Two different sequences or methods described for the same task                   |
| **Feature availability contradiction** | A feature described as available in one section and unavailable in another |
| **Recommendation contradiction**  | Opposite recommendations given for the same decision in different sections       |
| **Temporal contradiction**        | Two dates or time periods given for the same event that cannot both be correct   |

**Phase 6B — Contradiction Detection Protocol**

1. Build a claim registry from Step 1 — all verifiable claims indexed by entity (tool name, concept name, process name).
2. For each entity, compare all claims associated with it across the article.
3. If two claims about the same entity are mutually exclusive → CONTRADICTION detected.
4. Determine which claim is correct using source mapping from Step 2.
5. Remove or correct the inaccurate claim.
6. If both claims appear potentially correct in different contexts → add the contextual qualifier that resolves the apparent contradiction.

**Phase 6C — Consistency Fix Protocol**

| Contradiction Resolution               | Action                                                            |
|----------------------------------------|-------------------------------------------------------------------|
| One claim is clearly wrong             | Correct the wrong claim; leave the right one unchanged           |
| Both claims are conditional (different contexts) | Add the context qualifier that makes both true simultaneously |
| Cannot determine which is correct      | Flag both as CONTRADICTORY — USER RESOLUTION REQUIRED; do not remove either |
| One claim is outdated                  | Update the outdated claim; cross-reference with Step 3 outdated detection |

---

### ▸ STEP 7 — CONFIDENCE SCORING

Calculate the article's overall accuracy confidence score.

**Phase 7A — Score Calculation**

Apply the scoring formula defined in the output section:

```
CONFIDENCE SCORE FORMULA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Points Earned:
  Verified Correct × 1.0
  Corrected Issues × 0.7
  Unverified Low-Risk × 0.5
  Unverified High-Risk × 0.0
  Code PASS × 1.0
  Code Corrected × 0.7
  Code Needs Validation × 0.3

Deductions:
  Internal Contradiction Pair × -10 points each

Confidence % = (Points Earned / Maximum Possible Points) × 100
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 7B — Score Interpretation and Action Triggers**

| Score Range | Label            | Required Action Before Publishing                                              |
|-------------|------------------|--------------------------------------------------------------------------------|
| 90–100%     | HIGHLY ACCURATE  | Review unverified flags; publish after content team reviews flags             |
| 70–89%      | MOSTLY ACCURATE  | Content team reviews all corrections; verifies unverified sections           |
| 50–69%      | NEEDS REVIEW     | Significant corrections made; content team validates all HIGH priority claims |
| < 50%       | UNRELIABLE       | Do not publish — major revision required; run full pipeline again after fixes |

**Phase 7C — Score Supplementary Notes**

The confidence score is supplemented with:
1. The single most impactful issue (the correction or flag that reduced the score the most).
2. The sections with the lowest individual claim verification rates.
3. A specific recommendation for what the content team should prioritize before publishing.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ HALLUCINATION DETECTION

AI language models can generate claims that appear specific and authoritative but have no basis in real data — fabricated statistics, invented feature names, non-existent API endpoints, made-up version numbers. This advanced module specifically targets these fabrications.

**Hallucination Indicators:**

| Indicator                                                                               | Action                                                           |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------|
| Statistic with a specific organization name and precise number that cannot be verified  | Flag as POTENTIAL HALLUCINATION — verify against named source    |
| API endpoint or method that does not appear in official documentation                  | Flag as POTENTIAL HALLUCINATION — verify before publishing       |
| Feature name that does not appear on the official product website                      | Flag as POTENTIAL HALLUCINATION — verify against product         |
| Specific date or version number that cannot be confirmed                               | Flag as POTENTIAL HALLUCINATION — verify against changelog       |
| Quote attributed to a named person that cannot be sourced                             | Flag as POTENTIAL HALLUCINATION — source cannot be verified     |
| Named case study or client result without a verifiable reference                      | Flag as UNVERIFIED — source needed                               |

**Hallucination vs. Knowledge Gap Distinction:**

Not every unverifiable claim is a hallucination. A claim may be unverifiable because:
- The source exists but is not accessible (behind a paywall, internal documentation).
- The claim is recent (after knowledge cutoff) and the model simply doesn't know.
- The topic is too niche for the validation sources to cover.

A hallucination is distinguishable by:
- The claim has suspiciously precise specificity (exact numbers, exact feature names) without any traceable origin.
- The claim cannot be found in any related source — not just the primary source.
- The named entity (organization, product, person) in the claim does not exist or does not have the described relationship to the claim's content.

**Hallucination Handling:**

- If CONFIRMED HALLUCINATION (fabricated specifics with no traceable source) → remove the specific fabricated element and replace with a verified version or a general statement with a [VERIFY] flag.
- If POTENTIAL HALLUCINATION (suspicious specificity but source unavailable) → flag with [VERIFY — POTENTIAL HALLUCINATION] and the reason for suspicion.
- NEVER leave a confirmed hallucination in the published article.

---

### ▸ OUTDATED INFORMATION DETECTION

AI models have knowledge cutoffs. Claims about tool features, pricing, APIs, and software versions that were accurate at the training cutoff may no longer be accurate at publication time.

**Outdated Information Detection Signals:**

| Signal                                                             | Staleness Risk |
|--------------------------------------------------------------------|----------------|
| References a specific year that is ≥2 years ago                   | HIGH           |
| Describes pricing for a SaaS product                              | HIGH           |
| References a named software version by number                     | HIGH           |
| Describes a UI navigation path (interface changes frequently)     | HIGH           |
| References a regulatory framework compliance state                | HIGH           |
| References an API version or endpoint                             | MEDIUM-HIGH    |
| Describes a market statistic or industry data point               | MEDIUM         |
| References a company's team size or funding stage                 | MEDIUM         |
| Describes an integration availability                             | MEDIUM         |
| Explains a foundational technical concept that changes slowly     | LOW            |
| Defines a stable technical term or acronym                        | LOW            |

**Outdated Information Handling:**

For every HIGH staleness risk claim:
1. Determine if the claim can be verified as still current.
2. If YES → mark as VERIFIED CURRENT.
3. If NO or UNCERTAIN → flag as POTENTIALLY OUTDATED with: the specific element that may have changed (pricing, version, feature availability) and a recommended verification action.
4. For critical claims (pricing, regulatory compliance) → add inline disclosure in the article: "As of [knowledge cutoff date] — verify current [pricing/status/availability] before acting on this information."

---

### ▸ CROSS-SOURCE VALIDATION

When a claim appears in multiple sources with conflicting information, this module identifies the conflict and determines the most authoritative resolution.

**Conflict Detection:**

A conflict exists when:
- Two different authoritative sources provide different values for the same measurable fact.
- One source says a feature exists; another says it doesn't.
- Two publications cite the same statistic with different numbers.

**Conflict Resolution Protocol:**

| Conflict Type                                  | Resolution Method                                                                        |
|------------------------------------------------|------------------------------------------------------------------------------------------|
| Official source vs. secondary source conflict  | Official source wins — note the discrepancy                                             |
| Two official sources conflict (version-specific) | Use the more recent source — note the version context                                  |
| Two equal-authority sources conflict           | Flag the conflict; present both versions with their sources; do not make the choice for the reader |
| Statistical data from different research years | Use the most recent data; note the research year                                        |

**Cross-Source Validation Output:**

When a cross-source conflict is resolved by choosing one source:
```
CROSS-SOURCE RESOLUTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claim           : "[claim text]"
Source A        : [source name] — states: "[value]"
Source B        : [source name] — states: "[value]"
Conflict Type   : [value discrepancy / existence conflict / version conflict]
Resolution      : Used [Source A/B] because [specific reason — recency / authority / relevance]
Article Updated : [YES — uses resolved value / NO — both presented]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Corrections

If the same error appears in multiple locations in the article (e.g., the same incorrect statistic used three times) → the corrections log records this as ONE correction with a note: "Appears in [N] locations — corrected in all instances." The correction is applied to every instance in the article.

Do not create separate correction log entries for what is the same error repeated.

### Rule 2 — Merge Similar Issues

When two claims have the same underlying problem (e.g., both lack a plan-tier qualifier for a feature claim about the same tool) → merge them into a single corrections log entry with a thematic note: "Feature availability claims for [Tool Name] require plan-tier qualifiers throughout."

Merging is applied when: same root cause, same correction type, same tool or entity, addressable with the same fix.

### Rule 3 — One Flag Per Unverified Claim

An unverified claim is flagged once in the unverified log — not once per paragraph it appears in. If the same unverified claim appears multiple times → one log entry with all locations listed.

### Rule 4 — Code Block Consolidation

When multiple code blocks in the same article share the same issue type (e.g., all Python blocks are missing import statements for a particular dependency) → one log entry covering all blocks with a note: "Applied to code blocks in sections [list]."

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Accuracy-Checking Behaviors

| Prohibited Behavior                                                                             | Reason                                                                            |
|-------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Flagging a claim as "may be inaccurate" without specifying what is potentially wrong           | Vague flags provide no actionable information — every flag must name the specific concern |
| Marking a claim as UNVERIFIED without specifying why it could not be verified                 | The reason for unverifiability is as important as the flag itself                |
| Removing a claim from the article instead of correcting it (when correction is possible)       | Removal of content is not an accuracy correction — it is an editorial decision   |
| Adding speculative hedges to correct claims ("This may have changed")                         | Correct claims do not need hedges — only genuinely outdated claims need flags    |
| Applying [VERIFY] flags to every statistic even when verified                                 | Applying flags to verified claims undermines the signal value of genuine flags   |
| Leaving confirmed hallucinations in the article                                               | Hallucinated specifics are false information — they must be removed or corrected |
| Correcting a claim to something that is also uncertain without noting the uncertainty          | A correction that introduces new unverified content is no better than the original |
| Skipping code verification because the code "looks correct"                                   | Code is validated systematically — visual impression is not a verification method|
| Not testing for logical correctness in code because syntax is valid                           | Syntactically correct code can be logically wrong and practically non-functional |
| Producing a confidence score without showing the calculation                                  | The score must be traceable — the calculation is part of the report               |

### Required Accuracy-Checking Behaviors

- Every verifiable claim must receive a validation determination (CORRECT / INCORRECT / OUTDATED / INCOMPLETE / UNVERIFIED).
- Every correction must cite the source of the corrected information.
- Every [VERIFY] flag must include the specific concern and the recommended verification action.
- The confidence score calculation must be shown, not just the final number.
- Confirmed hallucinations must be removed or replaced — never left with only a flag.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ content-generation-engine.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Called by content-generation-engine (Post-Generation Phase 7B)                             |
| **Receives**     | Humanized article, topic, source of truth (if provided), content type, niche context        |
| **Protocol**     | Invoked as Phase T — after humanizer, before content-validation                            |
| **Returns**      | Corrected article, corrections log, confidence score, unverified claims list               |

---

### ▸ humanizer.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream processing layer — runs before technical accuracy checker                         |
| **Relationship** | Technical accuracy checker validates the humanized output — confirms humanization did not introduce inaccuracies through paraphrase |
| **Critical check** | Any sentence where humanization changed phrasing near a verifiable claim is re-validated  |

---

### ▸ content-validation.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | technical-accuracy-checker → content-validation (downstream consumer)                     |
| **Sends**        | Corrected, accuracy-verified article + confidence score + corrections log                  |
| **Used for**     | Content validation receives the accuracy-checked article; confidence score informs validation priority |
| **Critical flag**| If confidence score < 70% → content-validation escalates its completeness checks          |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after verification completes)                                                        |
| **Writes**       | Corrections log, confidence score, unverified claims list, code validation results         |
| **Memory layer** | content-memory.skill                                                                        |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                               | Recovery Action                                                                                           |
|---------------------------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| `generated_content` is empty                                              | Input pre-condition — null content                      | Halt. Log: "Cannot verify empty content. Accuracy check requires article input."                          |
| No verifiable claims found in the article                                 | Step 1 — claim extraction returns 0 claims             | Log: "No verifiable claims extracted — article may be entirely editorial/opinion content. Confidence Score: N/A. Proceeding without corrections." |
| `source_of_truth` provided but URL is inaccessible                        | Step 2 — URL returns error                             | Log the inaccessibility. Downgrade source to SOURCE TYPE KNOWN. Continue with secondary sources.          |
| A code block is in an unknown or unsupported language                     | Step 5 — language not recognized                       | Mark code as [CODE — LANGUAGE UNRECOGNIZED — NEEDS MANUAL VALIDATION]. Log the language identifier found. |
| A correction would change the article's argument or structure significantly | Step 4 — semantic impact of correction is major       | Do not apply correction automatically. Flag as CORRECTION REQUIRES REVIEW — STRUCTURAL IMPACT. Present the issue and suggested correction for content team decision. |
| Confidence score drops to < 50% during calculation                        | Step 7 — score < 50%                                   | Do not block delivery — but prepend article with: "⚠️ ACCURACY ALERT: Confidence score [N]% — substantial corrections were required. Review all flagged sections before publishing." |
| A `preserve_exactly` phrase (from humanizer) is determined to be factually incorrect | Step 3 — preserved phrase fails accuracy check | Flag as PRESERVE_EXACTLY CONFLICT — preserved phrase may contain inaccuracy. Present both the preservation instruction and the accuracy concern to the user. Never automatically override `preserve_exactly`. |
| Niche is medical, legal, or financial and >5 unverified claims exist      | Step 1 / niche context check                           | Escalate to HIGH RISK STATUS. Flag article: "⚠️ HIGH RISK UNVERIFIED CLAIMS in [Medical/Legal/Financial] content — do not publish without professional review of flagged sections." |
| The same claim appears to be both CORRECT (from one source) and INCORRECT (from another) | Step 3 / cross-source conflict detected         | Route to cross-source validation module. Present both sources and their claims. Do not auto-resolve if sources have equal authority. |
| All code blocks in the article fail verification                           | Step 5 — all code CORRECTED or NEEDS VALIDATION       | Prepend code sections with: "Code examples in this article have been flagged for validation — verify before using in production." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — CLAIM CATEGORY QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
NINE VERIFIABLE CLAIM CATEGORIES — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CAT 1 — TECHNICAL CLAIMS
  What : How systems/tools/processes work
  Source: Official product documentation
  Priority: HIGH if tool is named

CAT 2 — TOOL USAGE STEPS
  What : Step-by-step instructions with UI paths
  Source: Official product documentation / support
  Priority: HIGH (always) — reader will execute

CAT 3 — CODE SNIPPETS
  What : Any code, CLI command, or query
  Source: Language spec / official framework docs
  Priority: HIGH (always) — code errors cause failures

CAT 4 — STATISTICS
  What : Numerical claims about real-world phenomena
  Source: Original research / official reports
  Priority: HIGH if source cannot be identified

CAT 5 — DEFINITIONS
  What : Technical term meanings and acronym expansions
  Source: Official specification / authoritative glossary
  Priority: LOW unless in regulated domain

CAT 6 — NAMED FEATURE CLAIMS
  What : Specific product features and capabilities
  Source: Official product website / product docs
  Priority: HIGH (features change frequently)

CAT 7 — VERSION-SPECIFIC CLAIMS
  What : Software versions, version-specific behaviors
  Source: Official changelog / release notes
  Priority: HIGH (version-specific changes are common)

CAT 8 — PRICING AND PLAN DATA
  What : Product pricing, tier structure, trial availability
  Source: Official pricing page
  Priority: HIGH (pricing changes constantly)

CAT 9 — REGULATORY / COMPLIANCE
  What : Legal requirements, standards, certifications
  Source: Official regulatory body / legal text
  Priority: HIGH RISK (always) — errors harm readers
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — CODE VERIFICATION CHECKLIST
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CODE VERIFICATION CHECKLIST (Apply to Every Code Block)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SYNTAX CHECKS:
□ Brackets, parentheses, and braces are properly matched
□ Strings are properly quoted and terminated
□ Statement terminators are present (where language requires)
□ Indentation is consistent (critical in Python)
□ Keywords are correctly spelled and cased for the language
□ Variable and function names are consistent throughout the block

LOGIC CHECKS:
□ The code accomplishes what the article says it will
□ Required imports / require statements are present
□ Variables are initialized before use
□ Functions are called with correct argument types and counts
□ Loop conditions will eventually terminate
□ Error handling is appropriate for the described use case

USABILITY CHECKS:
□ Language/runtime version is noted if version-specific
□ Required dependencies are stated
□ Required environment variables are documented
□ The code is a complete example or is explicitly marked as partial
□ Comments in the code are accurate about what the code does
□ Output / return value matches what the article describes

FINAL STATUS:
  PASS             → All checks clear
  CORRECTED        → Issues found and fixed with confidence
  NEEDS VALIDATION → Domain-specific logic requires expert review
  CANNOT VERIFY    → Platform-specific — recommend testing
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — CONFIDENCE SCORE INTERPRETATION REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
CONFIDENCE SCORE REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
90–100% — HIGHLY ACCURATE:
  Most claims verified. Minor unverified flags only.
  Publishing recommendation: Review flags; publish after review.
  Typical cause of < 100%: Pricing/version flags or minor stat unverifiable.

70–89% — MOSTLY ACCURATE:
  Claims verified with some corrections applied.
  Publishing recommendation: Team reviews all corrections; verify flags.
  Typical cause: Several corrected feature claims or outdated stats.

50–69% — NEEDS REVIEW:
  Significant corrections made. Multiple unverified sections.
  Publishing recommendation: Team validates all HIGH priority items.
  Typical cause: Multiple incorrect tool instructions or unverified statistics.

< 50% — UNRELIABLE:
  Substantial accuracy problems. Do not publish.
  Publishing recommendation: Major revision required.
  Typical cause: Multiple hallucinations, confirmed incorrect technical claims,
                 or tool-specific instructions that are systematically wrong.

SPECIAL CASES:
  Any HIGH RISK unverified claim (medical/legal/financial) → requires
  professional review regardless of overall confidence score.

  Any confirmed hallucination → requires removal regardless of score.

  Any internal contradiction pair → -10 points each from the raw score;
  resolve before publishing.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: content/technical-accuracy-checker.skill*
*Nexus SEO Operating System — Version 1.0.0*
