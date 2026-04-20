# SKILL: core/output-formatter.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Core / Output Delivery
# EXECUTION PRIORITY: TERMINAL — The absolute last skill to execute in any pipeline. Nothing is delivered to the user without passing through this skill first.
# POSITION: Output delivery layer. Receives the combined raw output from nexus-connector.skill and transforms it into a clean, structured, export-ready document that the user can read, copy, and act on immediately.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill is the **final output delivery layer** of the Nexus SEO Operating System.

Every upstream skill in the Nexus system is optimized for analytical precision. The data structures, tables, logs, and records those skills produce are designed for computational processing and cross-skill data passing — not for direct human consumption. A raw opportunity score table passed between skills carries technical metadata that serves the pipeline; it does not serve the person who needs to read it and act on it.

This skill's job is to take the complete, validated, combined output from `nexus-connector.skill` and transform it into a document that a content director, SEO specialist, or founder can open, read from top to bottom, and immediately understand. Clean markdown headings that create clear visual hierarchy. Data in tables where tables communicate faster than prose. Paragraphs that breathe. No system notes in the final output. No pipeline log metadata in the deliverable sections. No raw internal data structures visible to the user.

The output of this skill is what the user receives. Everything that was built across the pipeline exists to make this final document worth reading.

### Primary Responsibilities

| Responsibility                         | Description                                                                                                           |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Section Identification**             | Parse the combined output and identify all content sections present in this run                                       |
| **Heading Structure Application**      | Apply consistent, hierarchical heading structure (H1/H2/H3/H4) across all sections                                   |
| **Table Formatting**                   | Convert all keyword lists, opportunity tables, roadmap items, and link plans into properly structured markdown tables  |
| **Content Formatting**                 | Apply spacing, paragraph breaks, and readability improvements to all prose sections                                   |
| **Markdown Generation**                | Produce a clean, export-ready markdown document                                                                       |
| **FULL Mode File Formatting**          | When execution mode is FULL: produce a complete, self-contained markdown file with no conversational elements         |
| **PARTIAL Mode Section Scoping**       | When execution mode is PARTIAL: format only the sections present; do not pad with empty section headers               |
| **SINGLE Mode Minimal Output**         | When execution mode is SINGLE: format only the target skill's output; minimal wrapper; maximum signal                 |
| **System Artifact Removal**            | Strip all pipeline execution logs, internal data references, system notes, and technical metadata from the user-facing output |
| **Section Hierarchy Balancing**        | Ensure no section is disproportionately long or short relative to its strategic importance                           |
| **Readability Optimization**           | Break up text walls, enforce maximum prose paragraph length, ensure table columns are readable widths                 |
| **Formatting Standardization**         | Apply the same visual treatment to equivalent elements across all sections — no inconsistent styling                  |
| **Data Integrity Preservation**        | Every number, keyword, URL, and recommendation from the combined output appears exactly as produced — formatting never modifies data |

### What This Skill Is NOT

- It is **not** a content editor. It formats what exists — it does not change, improve, or add content.
- It is **not** a summarizer. It does not condense outputs; it structures them.
- It is **not** a filter. Every section in the combined output gets formatted — nothing is silently dropped.
- It is **not** responsible for output quality. If the upstream analysis produced weak data, the formatted output reflects that weakness clearly — not obscured by formatting.
- It is **not** a PDF generator or export tool. It produces a clean markdown structure that can be exported — the actual file export is a downstream action.

### The Defining Standard of This Skill

> A document that contains brilliant analysis but is unreadable has failed its purpose.
> A document that is beautifully formatted but contains nothing useful has also failed.
>
> This skill exists at the intersection of those two failure modes.
> Its job is to ensure that the brilliant analysis is readable.
>
> The standard is: every piece of information the upstream pipeline produced
> must be visible, findable, and comprehensible by a reader encountering it for the first time.
>
> If a reader must scroll through a wall of text to find a specific keyword recommendation,
> formatting has failed. If a reader cannot tell where one section ends and another begins,
> formatting has failed. If a table is so wide it breaks the layout,
> formatting has failed.
>
> This skill does not fail.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Inputs

| Field                       | Description                                                                                         | Required |
|-----------------------------|-----------------------------------------------------------------------------------------------------|----------|
| `combined_output`           | The complete unified raw output from `nexus-connector.skill` — all sections, all skill outputs      | YES      |
| `execution_mode`            | FULL / PARTIAL / SINGLE — determines the formatting scope and output structure                      | YES      |
| `user_request`              | The original user request — used to generate the document title and context header                  | YES      |

### Optional Inputs

| Field                       | Description                                                                                | Used For                                             |
|-----------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------|
| `format_preference`         | User-specified format preference: MARKDOWN / PLAIN / STRUCTURED                           | Determines output format style                      |
| `export_target`             | Intended export destination: NOTION / GITHUB / GOOGLE_DOCS / CONFLUENCE / FILE           | Format-specific output adjustments                  |
| `sections_to_include`       | In PARTIAL mode: explicit list of sections to include in output                           | Section scoping (Step 7)                            |
| `brand_colors`              | Hex codes if generating for a specific branded output format                              | Not applied to markdown — noted for export          |
| `max_table_columns`         | Maximum columns per table before splitting (default: 8)                                   | Table formatting (Step 3)                           |
| `max_paragraph_words`       | Maximum words per prose paragraph before a break is forced (default: 100)                | Content formatting (Step 4)                         |

### Input Pre-Conditions

1. **Is `combined_output` non-empty?** If null → halt. "No output to format. Ensure nexus-connector.skill has completed."
2. **Is `execution_mode` specified?** If not → default to FULL mode formatting.
3. **Has `content-validation.skill` confirmed READY status?** If content was generated and validation returned NOT READY → the output formatter will flag this prominently in the output header and present the content with validation issues clearly marked.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — OUTPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Primary Output — Formatted SEO Intelligence Document

The output is a complete, clean markdown document. Its structure varies by execution mode but always follows this master template for FULL mode:

```markdown
# NEXUS SEO INTELLIGENCE REPORT
**Request:** [user request]
**Generated:** [date]
**Mode:** [FULL / PARTIAL / SINGLE]
**Domain / Topic:** [domain or keyword]
**Confidence:** [HIGH / MEDIUM / LOW]

---

## Table of Contents
1. [Strategic Executive Brief](#strategic-executive-brief)
2. [Keyword Intelligence](#keyword-intelligence)
3. [SERP Intelligence](#serp-intelligence)
4. [Content Blueprint](#content-blueprint)
5. [Authority & Gap Analysis](#authority--gap-analysis)
6. [Opportunity Intelligence](#opportunity-intelligence)
7. [Content Roadmap](#content-roadmap)
8. [Content Output](#content-output) *(if generated)*
9. [Metadata Package](#metadata-package)
10. [Internal Linking Plan](#internal-linking-plan) *(if applicable)*

---

## 1. Strategic Executive Brief

[Formatted strategist-ai.skill output]

---

## 2. Keyword Intelligence

[Formatted keyword-discovery.skill + semantic-clustering.skill output]

---

## 3. SERP Intelligence

[Formatted serp-intelligence.skill + deep-serp-analysis.skill output]

---

## 4. Content Blueprint

[Formatted serp-blueprint-generator.skill output]

---

## 5. Authority & Gap Analysis

[Formatted authority-engine.skill + gap-opportunity-engine.skill output]

---

## 6. Opportunity Intelligence

[Formatted opportunity-engine.skill + query-graph.skill output]

---

## 7. Content Roadmap

[Formatted roadmap-engine.skill output]

---

## 8. Content Output *(if generated)*

[Formatted full article from content-generation-engine pipeline]

---

## 9. Metadata Package

[Formatted metadata-generator.skill output]

---

## 10. Internal Linking Plan *(if applicable)*

[Formatted internal-linking.skill output]

---

*Generated by Nexus SEO Operating System | Session [ID]*
```

### Format Variant Outputs

**PARTIAL Mode Output:**
Same master template but contains only the sections that were executed. The Table of Contents lists only present sections. No empty section headers are included.

**SINGLE Mode Output:**
Minimal wrapper — document title + session metadata + one section containing the target skill's formatted output. No Table of Contents for single sections.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — STEP-BY-STEP LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ STEP 1 — SECTION IDENTIFICATION

Parse the combined output from `nexus-connector.skill` and build an inventory of all content sections present in this pipeline run.

**Phase 1A — Section Registry Construction**

Scan the combined output for the presence of each possible Nexus output section:

| Section Label                    | Source Skill(s)                                           | Present? |
|----------------------------------|-----------------------------------------------------------|----------|
| Strategic Executive Brief        | `strategist-ai.skill`                                    | [YES/NO] |
| Keyword Intelligence             | `keyword-discovery.skill` + `semantic-clustering.skill`  | [YES/NO] |
| SERP Intelligence                | `serp-intelligence.skill` + `deep-serp-analysis.skill`  | [YES/NO] |
| Content Blueprint                | `serp-blueprint-generator.skill`                         | [YES/NO] |
| Authority & Gap Analysis         | `authority-engine.skill` + `gap-opportunity-engine.skill`| [YES/NO] |
| Opportunity Intelligence         | `opportunity-engine.skill` + `query-graph.skill`         | [YES/NO] |
| Content Roadmap                  | `roadmap-engine.skill`                                   | [YES/NO] |
| Content Output                   | `content-generation-engine.skill` (post-processed)       | [YES/NO] |
| Metadata Package                 | `metadata-generator.skill`                               | [YES/NO] |
| Internal Linking Plan            | `internal-linking.skill`                                 | [YES/NO] |
| SEO Brief                        | `content-generation-engine.skill` Part 1                 | [YES/NO] |
| Validation Report                | `content-validation.skill`                               | [YES/NO] |
| Domain Analysis                  | `website-analysis.skill` + `content-inventory.skill`    | [YES/NO] |

**Phase 1B — Data Type Identification Within Sections**

For each present section, identify the types of data it contains:

| Data Type            | Detection                                                        | Formatting Treatment    |
|----------------------|------------------------------------------------------------------|-------------------------|
| Prose paragraphs     | Multi-sentence blocks of explanatory text                       | Step 4 (Content)        |
| Tabular data         | Rows and columns of structured records                          | Step 3 (Tables)         |
| Keyword lists        | Lists of keyword strings with associated metadata               | Step 3 (Tables)         |
| Code blocks          | Any code, CLI commands, or configuration examples               | Fenced code blocks      |
| Numbered lists       | Sequential items, steps, or ordered recommendations             | Ordered markdown lists  |
| Bullet lists         | Unordered items, features, or parallel items                    | Unordered markdown lists|
| Section headers      | Named sub-sections within a section                             | H3 or H4 headings       |
| URL references       | Page URLs appearing in content or link plans                    | Backtick-wrapped inline |
| Score/metric data    | Numerical scores, ratings, or measurements                      | Table cells or bold inline |
| Flag data            | Warning, validation, or status flags                            | Callout blocks or bold  |

**Phase 1C — Section Order Resolution**

Establish the final display order for all present sections:

The Strategic Executive Brief always displays first regardless of the order skills ran. After that, sections display in the master template order. Sections not present are skipped entirely — no empty headers, no placeholder sections.

---

### ▸ STEP 2 — STRUCTURE FORMATTING

Apply the heading hierarchy to the entire document, establishing clear visual structure.

**Phase 2A — Document-Level Heading Application**

```
H1 (#)     → Document title only: "NEXUS SEO INTELLIGENCE REPORT"
H2 (##)    → Major section names: "1. Strategic Executive Brief", "2. Keyword Intelligence"
H3 (###)   → Sub-sections within a major section: "### Current State Analysis", "### Keyword Clusters"
H4 (####)  → Sub-sub-sections when needed: "#### Pillar: [Cluster Name]", "#### HIGH Priority Items"
```

**Heading Rules:**

| Rule                                                                       | Application                                                    |
|----------------------------------------------------------------------------|----------------------------------------------------------------|
| Every H2 section begins on a new line after a horizontal rule (`---`)      | Creates clear visual separation between major sections        |
| No heading may be immediately followed by another heading without content  | A heading followed by a sub-heading must have at least one sentence between them |
| H4 headings are used sparingly — maximum 3 levels deep in any branch      | Prevents hierarchy collapse into a tree that is hard to navigate |
| Every section's H2 has a number prefix matching the Table of Contents     | "## 3. SERP Intelligence" — numbered for navigation          |
| The Table of Contents uses anchor links that match the section headings   | `[Keyword Intelligence](#keyword-intelligence)`               |

**Phase 2B — Section Divider Application**

Between every H2 section, insert a horizontal rule:

```markdown
---

## [Next Section]
```

Between H3 sub-sections within a long section, insert a half-rule (blank line above and below only — no `---` between H3s; that would over-fragment the document):

```markdown
### Sub-Section A

[content]

### Sub-Section B
```

**Phase 2C — Callout Block Formatting**

Important notices, warnings, flags, and critical alerts are formatted as callout blocks:

```markdown
> **⚠️ FLAG:** [flag content — specific warning or attention item]

> **✅ VERIFIED:** [verification confirmation]

> **📌 NOTE:** [important contextual note]

> **🚫 BLOCKER:** [blocking issue that prevents publishing or action]
```

Callout blocks are used for:
- Validation flags from `content-validation.skill`.
- Accuracy warnings from `technical-accuracy-checker.skill`.
- Priority alerts in the strategic brief.
- Dependency notes in the roadmap.
- Orphan page alerts in internal linking.

---

### ▸ STEP 3 — TABLE FORMATTING

Convert all tabular data into properly structured, readable markdown tables.

**Phase 3A — Standard Table Format**

All markdown tables follow this structure:

```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| data     | data     | data     |
```

**Table Formatting Rules:**

| Rule                                                                       | Application                                                              |
|----------------------------------------------------------------------------|--------------------------------------------------------------------------|
| Header row cells are title-cased                                           | "Target Keyword" not "target keyword"                                    |
| Column widths are normalized                                               | All cells in a column have consistent padding                            |
| Tables have a blank line before and after them                            | Prevents rendering issues in markdown parsers                            |
| Maximum 8 columns per table (adjustable via `max_table_columns`)          | Tables wider than 8 columns are split (see Phase 3C)                    |
| All table rows are populated — no empty cells                             | If data is absent for a cell → use `—` (em dash) not blank              |
| Numerical values in cells are right-aligned in the column header          | Use `:---:` for center or `---:` for right alignment                   |
| Priority columns use consistent labels: HIGH / MEDIUM / LOW / OPTIONAL   | Never vary the capitalization or spelling                                |

**Phase 3B — Table Types and Their Templates**

**KEYWORD TABLE (from keyword-discovery, opportunity-engine):**

```markdown
| # | Keyword | Intent | Funnel | Score | Priority | Cluster |
|---|---------|--------|--------|-------|----------|---------|
| 1 | [kw]   | [intent] | [funnel] | [N/10] | HIGH | [cluster] |
```

**ROADMAP TABLE (from roadmap-engine):**

```markdown
| Phase | Month | # | Topic | Target Keyword | Intent | Cluster | Priority | Type | Depends On |
|-------|-------|---|-------|----------------|--------|---------|----------|------|-----------|
```

**OPPORTUNITY TABLE (from opportunity-engine):**

```markdown
| # | Keyword | Cluster | D1 | D2 | D3 | D4 | D5 | Score | Priority |
|---|---------|---------|----|----|----|----|----|----|---------|
```

**INTERNAL LINK TABLE (from internal-linking):**

```markdown
| # | Source Page | Target Page | Anchor Text | Placement | Type | Priority |
|---|-------------|-------------|-------------|-----------|------|----------|
```

**AUTHORITY TABLE (from authority-engine):**

```markdown
| Cluster | Coverage Level | Auth Score | Gap Urgency | Action Required |
|---------|----------------|------------|-------------|-----------------|
```

**METADATA TABLE (from metadata-generator):**

```markdown
| Element | Option 1 | Option 2 | Option 3 | Recommended |
|---------|---------|---------|---------|-------------|
| Blog Title | [title] | [title] | [title] | Option [N] |
| Meta Title | [title] (Nch) | [title] (Nch) | [title] (Nch) | Option [N] |
| Meta Description | [desc] (Nch) | [desc] (Nch) | [desc] (Nch) | Option [N] |
| URL Slug | /[slug]/ | /[slug]/ | /[slug]/ | Option [N] |
```

**Phase 3C — Wide Table Splitting**

When a table exceeds 8 columns (or the user-specified `max_table_columns`):

1. Identify the natural split point — where the data logically divides into two logical groups.
2. Create two separate tables with a shared primary key column (usually #, URL, or Keyword).
3. Label the tables: "[Table Name] — Part 1 of 2" and "[Table Name] — Part 2 of 2".

Example: The Roadmap Table has 10 columns. Split into:
- Part 1: Phase | Month | # | Topic | Target Keyword | Intent
- Part 2: # | Cluster | Priority | Type | Depends On | Rationale

The `#` column appears in both parts as the join key.

**Phase 3D — Small Data List to Table Conversion**

When data is presented as a list of 3+ items with 2+ attributes per item, convert to a table:

**Before (raw list):**
```
- crm software for startups (BOFU, Opportunity: 9/10)
- best crm small business (MOFU, Opportunity: 7/10)
- crm implementation guide (TOFU, Opportunity: 6/10)
```

**After (formatted table):**
```markdown
| Keyword | Intent | Opp. Score |
|---------|--------|-----------|
| crm software for startups | BOFU | 9/10 |
| best crm small business | MOFU | 7/10 |
| crm implementation guide | TOFU | 6/10 |
```

The threshold for conversion is: 3+ items AND 2+ attributes per item. Below that threshold, a bullet list is more readable than a table.

---

### ▸ STEP 4 — CONTENT FORMATTING

Apply readability improvements to all prose sections to ensure no text walls, good rhythm, and clear paragraph structure.

**Phase 4A — Paragraph Length Control**

No prose paragraph may exceed the `max_paragraph_words` threshold (default: 100 words).

When a paragraph exceeds the threshold:
1. Identify the natural break point — a sentence that ends one thought and introduces another.
2. Split at that point.
3. Insert a blank line between the two resulting paragraphs.
4. Do not split mid-thought — a 105-word paragraph that logically breaks at word 90 is split there, not arbitrarily at word 100.

**Phase 4B — Spacing Standardization**

Apply consistent spacing throughout:

| Element                              | Spacing Rule                                                    |
|--------------------------------------|-----------------------------------------------------------------|
| Before an H2 heading                 | Two blank lines before (except document opening H1)             |
| After an H2 heading                  | One blank line after before content begins                      |
| Before an H3 heading                 | One blank line before                                           |
| After an H3 heading                  | One blank line after before content begins                      |
| Between paragraphs                   | One blank line                                                  |
| Before a table                       | One blank line                                                  |
| After a table                        | One blank line                                                  |
| Before a code block                  | One blank line                                                  |
| After a code block                   | One blank line                                                  |
| Before a callout block               | One blank line                                                  |
| After a callout block                | One blank line                                                  |
| Before a bullet list                 | One blank line                                                  |
| After a bullet list                  | One blank line                                                  |

**Phase 4C — Text Emphasis Standardization**

Apply emphasis consistently:

| Element Type                          | Formatting Applied                                              |
|---------------------------------------|-----------------------------------------------------------------|
| Section key terms (first mention)     | **Bold** on first mention within a section                      |
| Important metrics and scores          | **Bold** — e.g., "Authority Score **4/5**"                    |
| HIGH priority items                   | **HIGH** — always bold                                          |
| URLs and slugs                        | `backtick-wrapped` — e.g., `/crm-software-startups/`          |
| Keywords as data (not as prose)       | `backtick-wrapped` — e.g., `crm software for startups`        |
| Keywords used naturally in prose      | No special formatting — they read as normal text               |
| Skill names                           | `backtick-wrapped` — e.g., `serp-intelligence.skill`          |
| Flags and status labels               | **All caps bold** — e.g., **BLOCKER**, **VERIFIED**, **FLAGGED**|
| Section references                    | Italicized — e.g., *See Section 5 for gap details*            |

**Phase 4D — List Formatting Standardization**

**Unordered lists:**
- Use `-` (hyphen) for all bullet points — never `*` or `+`.
- Indent sub-items with two spaces.
- No trailing punctuation on list items unless they are complete sentences.

**Ordered lists:**
- Use `1.`, `2.`, `3.` format — not `a.`, `b.`, `c.` unless explicitly needed.
- Consistent capitalization within a list — all items start with the same case pattern.

**Mixed list rules:**
- Never mix bullet and numbered lists at the same indent level.
- If a list item has sub-items → both the parent item and sub-items must be of the same list type unless there is a clear structural reason to mix (e.g., numbered steps with bulleted sub-notes).

---

### ▸ STEP 5 — MARKDOWN GENERATION

Produce the complete, clean markdown document from all formatted sections.

**Phase 5A — Document Assembly**

Assemble the document in this order:
1. Document header (title, metadata, TOC).
2. Each section in the order determined by Step 1C.
3. Document footer (session information).

**Phase 5B — Table of Contents Generation**

Generate the Table of Contents from the section registry:

```markdown
## Table of Contents
1. [Strategic Executive Brief](#1-strategic-executive-brief)
2. [Keyword Intelligence](#2-keyword-intelligence)
3. [SERP Intelligence](#3-serp-intelligence)
...
```

TOC rules:
- Only include sections that are present in this run (from section registry).
- Anchor links are lowercase, hyphen-separated, matching the H2 heading text.
- Numbers in headings like "1. Strategic Executive Brief" produce anchor `#1-strategic-executive-brief`.

**Phase 5C — Document Header Format**

```markdown
# NEXUS SEO INTELLIGENCE REPORT

| Field | Value |
|-------|-------|
| **Request** | [user request] |
| **Date** | [ISO 8601 date] |
| **Session ID** | [session ID] |
| **Mode** | [FULL / PARTIAL / SINGLE] |
| **Domain / Topic** | [domain or keyword] |
| **Confidence** | [HIGH / MEDIUM / LOW] |
| **Sections** | [count] sections generated |
| **Skills Run** | [count] skills executed |
```

**Phase 5D — Document Footer Format**

```markdown
---

*Generated by Nexus SEO Operating System*
*Session [ID] | [timestamp] | [execution mode]*
*[count] skills executed | [count] sections | [word count] total words*
```

**Phase 5E — Markdown Compliance Check**

After assembly, verify:
- All tables have matching column counts in header and data rows.
- No unclosed code blocks (every opening ` ``` ` has a closing ` ``` `).
- No broken anchor links in the TOC.
- No orphaned heading levels (no H4 without an H3 parent in the same section).
- No raw HTML tags in the output (markdown only — no `<br>`, `<div>`, or `<span>`).
- All blockquotes (callout blocks) are properly formatted with `>` prefix.

---

### ▸ STEP 6 — FILE MODE HANDLING (FULL EXECUTION)

When execution mode is FULL, apply the complete file formatting treatment.

**Phase 6A — Complete Document Standards**

In FULL mode, the output is intended to be a standalone, self-contained reference document. Apply these additional standards:

| Standard                                                                 | Application                                                              |
|--------------------------------------------------------------------------|--------------------------------------------------------------------------|
| Every section is self-contained                                          | A reader who opens the document at any section understands it without needing to read prior sections |
| Cross-references are explicit                                            | "As scored in Section 6, this keyword's opportunity score is 8/10" — not just "see Section 6" |
| No orphaned data without context                                         | Every table appears after an explanatory sentence — tables never open cold |
| Section summaries are present for long sections                         | For any section exceeding 500 words, a 1–2 sentence summary appears at its opening |

**Phase 6B — Export-Ready Formatting**

In FULL mode, the document is formatted to be immediately export-ready to:

**Notion:**
- Tables render correctly.
- Callout blocks render as Notion callouts.
- Code blocks render with syntax highlighting.

**GitHub Markdown:**
- All standard markdown features are supported.
- Tables render in GitHub's markdown preview.
- No Notion-specific callout syntax.

**Google Docs (via copy-paste):**
- Heading structure translates to Google Docs heading styles.
- Tables copy correctly.
- Minimal special characters that might corrupt on paste.

**Generic Markdown (.md file):**
- Fully compliant CommonMark markdown.
- No proprietary syntax.
- Renders in any markdown viewer.

When `export_target` is specified → apply target-specific formatting adjustments. When not specified → use Generic Markdown as the default.

**Phase 6C — Conversational Text Removal**

In FULL mode, remove all conversational and meta-commentary text from the final output:

```
CONVERSATIONAL TEXT TO REMOVE IN FULL MODE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Phrases to strip:
  "I'll now provide..."
  "Let me explain..."
  "As you can see..."
  "Based on my analysis..."
  "Here is the [output]:"
  "I hope this helps..."
  "Feel free to ask..."
  "This section covers..."
  "Below, you'll find..."
  "In the following section..."

System notes to strip:
  Pipeline execution progress text
  Skill invocation confirmation messages
  Cache-hit notifications (these go in the log only)
  Internal processing status messages

All of the above are removed before delivering the FULL mode document.
The document contains only the intelligence and structured outputs — no commentary about the process.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### ▸ STEP 7 — PARTIAL MODE HANDLING

When execution mode is PARTIAL, format only the sections that were executed in the pipeline run.

**Phase 7A — Present-Only Section Rendering**

Enumerate the sections present in the combined output (from Step 1). Format only those sections. Do not include:
- Empty section headers.
- Placeholder sections with "No data available."
- Sections that were listed as skipped in the pipeline log.

**Phase 7B — PARTIAL Mode Header**

The document header in PARTIAL mode explicitly notes which sections are and are not present:

```markdown
# NEXUS SEO INTELLIGENCE REPORT (PARTIAL)

| Field | Value |
|-------|-------|
| **Sections Included** | [list of included sections] |
| **Sections Omitted** | [list of omitted sections with reason] |
```

**Phase 7C — Standalone Section Quality**

Each section in PARTIAL mode must be self-sufficient — it cannot assume the reader has access to sections that were not run. If a section normally references another section for context → replace the reference with the specific data point that would have appeared in the referenced section, or note: "For full context, run [section name] in FULL mode."

**Phase 7D — PARTIAL Mode Section Ordering**

Sections in PARTIAL mode are displayed in the master template order (not the order the user selected them). If the user selected sections [3, 7, 10] → they display as Section 3, Section 7, Section 10 with renumbered TOC entries but in the correct relative order.

---

### ▸ STEP 8 — SINGLE MODE HANDLING

When execution mode is SINGLE, apply minimal formatting that prioritizes signal over structure.

**Phase 8A — SINGLE Mode Document Structure**

```markdown
# NEXUS — [Skill Name] OUTPUT

**Keyword / Topic:** [input]
**Skill:** [skill-name.skill]
**Session:** [ID]

---

[Formatted skill output]

---

*Single-skill output | Nexus SEO System | [timestamp]*
```

**Phase 8B — SINGLE Mode Formatting Rules**

| Rule                                                                          |
|-------------------------------------------------------------------------------|
| No Table of Contents — only one section                                       |
| No document-level cross-references                                            |
| No empty section padding                                                      |
| All tables, lists, and prose formatting rules from Steps 3 and 4 still apply |
| Callout blocks still used for flags and warnings                              |
| Data integrity rules are the same as FULL mode                               |

**Phase 8C — SINGLE Mode Context Note**

At the top of the SINGLE mode output, include a one-line context note:

```markdown
> **📌 SINGLE MODE:** This output contains results from `[skill-name.skill]` only. For complete analysis, run in FULL mode.
```

---

### ▸ STEP 9 — CLEAN OUTPUT

Apply a final cleaning pass to the complete formatted document before delivery.

**Phase 9A — Redundant Text Removal**

Scan the document for:

| Redundancy Type                                                      | Removal Action                                           |
|----------------------------------------------------------------------|----------------------------------------------------------|
| The same section header appearing twice                             | Keep the first; remove the duplicate                    |
| The same table appearing with identical data in two sections        | Keep the table in its primary section; add a reference note in the secondary |
| The same recommendation appearing in both a table and prose form   | Keep the table form; remove the prose duplicate if it adds no additional context |
| Consecutive identical bullet items                                   | Remove the duplicate; note the merge in the formatter log |
| Empty rows in tables                                                 | Remove — every table row must have data                 |
| Double blank lines (more than 2 blank lines between any elements)   | Collapse to a single blank line                         |
| Trailing whitespace at line endings                                  | Strip                                                    |

**Phase 9B — System Note Removal**

Remove all system-level annotations that were helpful for pipeline processing but do not belong in the user-facing output:

```
SYSTEM NOTES TO REMOVE BEFORE DELIVERY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Internal pipeline metadata:
  "CACHE-HIT for [skill]"
  "INFERRED from keyword pattern"
  "DELTA MODE — only new items since [date]"
  "Skill invocation #N"
  "Running [skill-name].skill..."
  "Output from [skill-name].skill:"
  "Passed to [skill-name].skill:"
  "Session context loaded."

Exception — KEEP these system notes:
  [VERIFY] accuracy flags from technical-accuracy-checker
  [BLOCKER] validation blockers from content-validation
  Data confidence flags marked for user awareness (INFERRED:D2, etc.)
  Projection confidence level from strategist-ai
  "NEEDS VALIDATION" flags on specific items
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Phase 9C — Final Length and Balance Check**

After cleanup, verify section length balance:

| Check                                                                        | Pass Condition                                             |
|------------------------------------------------------------------------------|------------------------------------------------------------|
| No single section is more than 40% of the total document length             | If exceeded → apply section summary at section opening    |
| No section is fewer than 50 words (except SINGLE mode)                      | If a section is under 50 words → flag as THIN SECTION      |
| The TOC accurately reflects all present sections and their headings         | Every H2 in the document has a TOC entry                  |
| The document footer is present                                               | Every output has a footer with session and generation info |

**Phase 9D — Formatter Processing Log**

Produce an internal log of all formatting actions taken (this log is NOT delivered to the user — it is saved to memory for debugging and session records):

```
FORMATTER PROCESSING LOG (INTERNAL — NOT IN OUTPUT)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Tables formatted       : [count]
Tables split (wide)    : [count]
Lists converted to tables: [count]
Paragraphs split (long): [count]
System notes removed   : [count]
Conversational text removed: [count]
Duplicate sections removed: [count]
Callout blocks added   : [count]
TOC entries            : [count]
Total document words   : [count]
Output mode applied    : [FULL / PARTIAL / SINGLE]
Export target applied  : [target or GENERIC]
Processing time        : [timestamp]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — ADVANCED LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ FORMATTING STANDARDIZATION

Apply a cross-section standardization pass to ensure equivalent elements look identical regardless of which section they appear in.

**Standardization Matrix:**

| Element                        | Standard Format                    | Applied Across All Sections                 |
|--------------------------------|------------------------------------|---------------------------------------------|
| Priority HIGH                  | **HIGH** (bold, all caps)          | Roadmap, opportunity table, linking table   |
| Priority MEDIUM                | **MEDIUM** (bold, all caps)        | Same                                         |
| Priority LOW                   | LOW (all caps, no bold)            | Same                                         |
| Authority Score                | `[N]/5` or `Score [N]/5`           | Authority table, strategic brief            |
| Opportunity Score              | `[N]/10`                           | Opportunity table, action items             |
| Word count                     | `[N] words` (no abbreviation)      | Roadmap, content output, SEO brief          |
| Funnel stage                   | **TOFU** / **MOFU** / **BOFU** (bold) | All sections                             |
| Character count                | `([N] chars)` in parentheses       | Metadata table                              |
| URL format                     | `/slug/` (forward slashes, backtick) | All URL references                        |
| Confidence level               | HIGH / MEDIUM / LOW (all caps)     | Projection, accuracy score                  |
| Gap type                       | MISSING TOPIC / MISSING INTENT etc. (all caps, no bold) | Gap table       |
| Coverage level                 | COMPLETE / STRONG / MEDIUM / WEAK / MISSING (all caps) | Authority table |

**Cross-Section Consistency Enforcement:**

After formatting all sections, run a consistency scan:
1. Is the same data element formatted the same way in every section where it appears?
2. Are priority labels consistent? (If Section 6 uses **HIGH** in bold, Section 7 must also use **HIGH** in bold.)
3. Are score formats consistent? (If Section 6 shows `8/10`, Section 7 must not show `8 out of 10`.)
4. Are URL formats consistent? (`/crm-guide/` not `crm-guide` in some places and `/crm-guide/` in others.)

Inconsistencies found → apply the standard format from the matrix above.

---

### ▸ READABILITY OPTIMIZATION

Apply advanced readability analysis beyond basic paragraph length control.

**Readability Rules:**

| Check                                                                           | Standard                                                       |
|---------------------------------------------------------------------------------|----------------------------------------------------------------|
| No more than 3 consecutive bullet items without a paragraph break               | After 3 bullets, insert either a blank line or a prose sentence |
| No section opens with a table or list (must have at least one sentence of context) | Every section's first element after its H2 heading is a prose sentence |
| Code blocks are not wider than 80 characters                                   | Wrap long lines in code blocks with continuation characters    |
| Emphasis is not overused                                                        | No more than 1 bolded word/phrase per sentence; no more than 3 bold items per paragraph |
| No ALL CAPS prose (only permitted in defined formatting elements)               | Prose text is normal case; ALL CAPS only for defined labels    |
| Tables in long sections have a preceding description sentence                  | "The following table shows the 15 highest-priority keywords identified for this cluster:" |
| The strategic brief section (Section 1) has no tables                          | The strategic brief is prose-only — tables belong in analytical sections |

**Scannability Requirements:**

A reader should be able to scan the document at high speed and extract its key points. Ensure:
1. Every H2 section heading is descriptive enough to indicate its content without reading the section.
2. Every table has a heading or caption that identifies what the table contains.
3. Every callout block starts with a type label (**⚠️ FLAG**, **✅ VERIFIED**, etc.) that instantly communicates the callout's nature.
4. The document's most important finding (from the strategic brief) is visible without scrolling — it appears in the first 200 words.

---

### ▸ SECTION HIERARCHY BALANCING

Ensure the document's section lengths reflect the relative importance and depth of each section.

**Section Balance Rules:**

| Imbalance Detected                                                              | Correction                                                    |
|---------------------------------------------------------------------------------|---------------------------------------------------------------|
| Section 1 (Strategic Brief) is shorter than Section 6 (Opportunity)           | Section 1 should be substantive but concise; if it's very short, flag as THIN — the strategist may have underproduced |
| A single section is >40% of total document length                              | Apply a "Key Findings" summary box at the section opening to give readers a quick entry point |
| Multiple sections are each under 100 words                                     | Consider whether these should be combined under one H2 with H3 sub-sections |
| The roadmap table dominates visually but the strategic brief is thin           | Add a "Roadmap Summary" callout block in Section 1 that previews the top 3 roadmap items |

**Section Opening Summary Boxes (for long sections):**

When a section exceeds 500 words, add an opening summary box:

```markdown
> **📊 Section Summary:**
> - [Key finding 1 — one line]
> - [Key finding 2 — one line]
> - [Key finding 3 — one line]
```

This box appears after the H2 heading and before the first paragraph, giving readers a fast-scan entry point.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — DEDUPLICATION RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Rule 1 — No Repeated Section Headers

Each H2 section label appears exactly once in the document. If the combined output contains the same section name from two different sources → merge them under one H2 heading with H3 sub-headings for each source's contribution.

### Rule 2 — No Duplicate Tables

If the same data table (same data, same structure) appears in two sections of the combined output → render it once in its primary section. In the secondary section, replace the duplicate table with a reference: "*See [Primary Section Name] for the full [table name] table.*"

### Rule 3 — No Repeated Keyword Lists

If the same keyword list appears in multiple sections (once in keyword intelligence, once in the opportunity section) → render the full list once. In subsequent sections, show only the top 5 rows and link to the primary section for the full list.

### Rule 4 — No Duplicate Callout Blocks

If the same flag or warning appears in multiple sections of the combined output → display it once in the section where it is most relevant. In other sections, include a single inline mention: "*([Flag type] noted — see Section [N])*".

### Rule 5 — No Redundant Prose in Adjacent Sections

If Section 5 (Authority & Gap Analysis) and Section 6 (Opportunity Intelligence) both contain prose paragraphs that make the same point → keep the fuller version in whichever section is more authoritative for that finding; trim or remove the redundant passage from the other.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 7 — ANTI-FLUFF RULES
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Prohibited Formatting Behaviors

| Prohibited Behavior                                                                             | Reason                                                                           |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Adding explanatory text to sections that have no explanatory content in the source            | Formatting does not add content — it structures what exists                     |
| Padding thin sections with "no data was available for this section" paragraphs                | If a section is thin → it is thin; don't add filler prose to hide the thinness  |
| Adding motivational or encouraging language to the output document                             | The output is a professional intelligence document — not a coaching session      |
| Including the pipeline execution log in the user-facing final output                           | The log is an internal record — it belongs in memory, not in the deliverable    |
| Modifying any data value during formatting (rounding scores, truncating keywords)              | Data integrity is absolute — formatting never changes numbers or content         |
| Creating a Table of Contents for SINGLE mode outputs                                          | A TOC for one section adds no navigation value                                   |
| Applying FULL mode conversational text removal to flagged data markers                        | [VERIFY] and [BLOCKER] flags are user-facing warnings — never stripped           |
| Formatting code blocks as prose paragraphs for visual consistency                              | Code blocks must remain as fenced code blocks — they cannot become prose         |
| Adding section intro sentences that restate the heading                                        | "This section covers SERP Intelligence" immediately after "## 3. SERP Intelligence" is pure noise |
| Inserting decorative separators beyond `---` horizontal rules between sections                | Decorative Unicode separators (━━━━━) or ASCII art are for internal skill files only — not user output |

### Required Formatting Behaviors

- Every table must be preceded by at least one sentence of context.
- Every callout block must have a type label (**⚠️ FLAG**, **✅ VERIFIED**, etc.).
- Every H2 section must have at least one blank line after its heading before content begins.
- The Table of Contents must reflect exactly what sections are present — no phantom entries.
- The document footer must be present in every output regardless of mode.
- All [VERIFY] and [BLOCKER] flags from upstream validation must appear in the final output.

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 8 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### ▸ nexus-connector.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Upstream source — the only direct caller of this skill                                      |
| **Receives**     | Complete unified combined output (all sections from all executed skills)                    |
| **Protocol**     | `nexus-connector.skill` is the sole entry point — `output-formatter.skill` is never called directly by individual skills |
| **What it receives** | Raw, unformatted combined output, execution mode, and user request                    |

---

### ▸ All Upstream Skills (Indirect)

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Indirect — their outputs arrive via `nexus-connector.skill`                                |
| **Relationship** | Every Nexus skill's output eventually passes through this formatter                        |
| **Formatting contract** | Every skill produces structured output that the formatter knows how to render — the formatter does not need to interpret or re-analyze data, only structure it |

---

### ▸ memory-controller.skill

| Property         | Detail                                                                                       |
|------------------|----------------------------------------------------------------------------------------------|
| **Direction**    | Write (after formatting completes)                                                          |
| **Writes**       | Formatter processing log (internal), final formatted document reference, session completion record |
| **What is NOT saved** | The full formatted document is delivered to the user; its reference (session ID + timestamp) is saved, not the full text |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX A — FAILSAFE LOGIC
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Failure Scenario                                                          | Detection                                              | Recovery Action                                                                                          |
|---------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `combined_output` is null or empty                                        | Input validation — null field                          | Halt. Report: "No content to format. The pipeline produced no output — check skill execution logs."      |
| Section structure cannot be identified (no recognizable section markers) | Step 1 — section registry returns 0 entries           | Default to markdown sections. Wrap entire content in a single section: "## Analysis Output" with all content below it. Flag: "Section structure unrecognized — defaulted to single-section format." |
| A table cannot be parsed (mismatched column counts)                       | Step 3 — table parsing fails                          | Render the table data as a bullet list instead. Flag with: `> **📌 NOTE:** Table could not be rendered — displayed as list.` |
| A section exceeds 5,000 words                                             | Step 9C — section length check                        | Apply section summary box + internal section TOC (H3 links within the section). Flag in formatter log. |
| Callout blocks have no type label in the source data                      | Step 2C — callout parsing finds unlabeled blocks      | Apply **📌 NOTE:** label as default. Never deliver an unlabeled callout block.                            |
| Table column count exceeds `max_table_columns` after splitting is attempted | Step 3C — split attempt fails                       | Render as two separate tables with the split point at column 6. If still too wide → render as key-value pairs using a two-column `| Field | Value |` table. |
| The strategic brief section (Section 1) is absent from the combined output | Step 1 — section registry shows Strategic Brief: NO | Add a prominent callout at the top of the document: `> **⚠️ FLAG:** Strategic Brief not available — strategist-ai.skill did not execute in this run. Analysis sections are present.` |
| Content has validation NOT READY status                                   | Input — content-validation output includes NOT READY | Add document-top callout: `> **🚫 BLOCKER:** Content validation status: NOT READY — [blocker reasons]. Do not publish this content until blockers are resolved.` Include content below with blockers clearly marked inline. |
| Export target is specified but format conflicts with content structure     | Step 6B — format check                               | Apply Generic Markdown and flag: "Export target [target] formatting could not be applied — delivered in Generic Markdown. Convert manually after export." |
| Memory write fails after formatting completes                             | Step 9D — memory write error                         | Deliver output to user. Flag in output footer: "Session not persisted — memory unavailable. Save this output manually." |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX B — FORMATTING STANDARDS QUICK REFERENCE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
FORMATTING STANDARDS — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HEADINGS:
  # H1     → Document title only
  ## H2    → Major sections (numbered)
  ### H3   → Sub-sections within major sections
  #### H4  → Sub-sub-sections (use sparingly)

EMPHASIS:
  **bold**       → Key terms (first mention), metrics, HIGH/MEDIUM priority labels
  *italic*       → Cross-references: "See Section 5"
  `backtick`     → Keywords as data, URLs, slug paths, skill names, code inline

SEPARATORS:
  ---            → Between H2 sections only
  blank line     → Between paragraphs, before/after tables/lists/code blocks

TABLES:
  Max 8 columns  → Split if wider
  Always preceded by 1 sentence of context
  Header row title-cased
  Empty cells → use — (em dash)

LISTS:
  - (hyphen)     → Bullet points
  1. (number)    → Ordered lists
  Max 3 consecutive bullets without paragraph break

CALLOUT BLOCKS:
  > **⚠️ FLAG:**      → Warnings, accuracy concerns
  > **✅ VERIFIED:**   → Confirmed accurate items
  > **📌 NOTE:**       → Contextual information
  > **🚫 BLOCKER:**    → Blocking issues
  > **📊 Section Summary:** → Section opening summaries (>500 words)

DATA LABELS (ALWAYS ALL CAPS):
  Priority: HIGH / MEDIUM / LOW / OPTIONAL
  Funnel: TOFU / MOFU / BOFU
  Coverage: COMPLETE / STRONG / MEDIUM / WEAK / MISSING
  Gap type: MISSING TOPIC / MISSING INTENT / etc.
  Confidence: HIGH / MEDIUM / LOW

REMOVE FROM USER OUTPUT:
  ✗ Pipeline execution messages
  ✗ Skill invocation confirmations
  ✗ Cache-hit notifications
  ✗ Processing status updates
  ✗ Conversational filler text

ALWAYS KEEP IN USER OUTPUT:
  ✓ [VERIFY] accuracy flags
  ✓ [BLOCKER] validation blockers
  ✓ Confidence ratings on projections
  ✓ "NEEDS VALIDATION" item flags
  ✓ Data source attributions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX C — MODE FORMATTING COMPARISON
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
MODE FORMATTING COMPARISON
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                     FULL      PARTIAL   SINGLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Document title        ✓         ✓         ✓
Metadata header table ✓         ✓         ✓ (minimal)
Table of Contents     ✓         ✓ (present sections only) ✗
All sections          ✓         — (selected only)  — (one section)
Section dividers ---  ✓         ✓         ✗
Cross-references      ✓         ✓ (limited)  ✗
Section summaries     ✓ (>500w) ✓ (>500w)  ✗
Conversational text removed ✓   ✓         ✓
System notes removed  ✓         ✓         ✓
[VERIFY]/[BLOCKER] kept ✓       ✓         ✓
Document footer       ✓         ✓         ✓ (minimal)
Export-ready format   ✓         ✓         ✓
Single-section mode note ✗      ✗         ✓
"Sections omitted" note ✗      ✓         ✗
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## APPENDIX D — TABLE TEMPLATE LIBRARY
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
TABLE TEMPLATE LIBRARY — QUICK REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

KEYWORD TABLE:
| # | Keyword | Intent | Funnel | Opp. Score | Priority | Cluster |
|---|---------|--------|--------|-----------|----------|---------|

CLUSTER TABLE:
| Cluster Name | Tier | Keywords | Strength | Funnel Balance | Auth Score |
|-------------|------|----------|----------|----------------|-----------|

ROADMAP TABLE (Part 1):
| Phase | Month | # | Topic | Target Keyword | Intent |
|-------|-------|---|-------|----------------|--------|

ROADMAP TABLE (Part 2):
| # | Cluster | Priority | Type | Depends On | Rationale |
|---|---------|----------|------|-----------|-----------|

OPPORTUNITY TABLE:
| # | Keyword | D1 | D2 | D3 | D4 | D5 | Score | Priority |
|---|---------|----|----|----|----|----|----|---------|

AUTHORITY TABLE:
| Cluster | Auth Score | Coverage Level | Gap Urgency | Action |
|---------|-----------|----------------|-------------|--------|

GAP TABLE:
| # | Content Idea | Target Keyword | Intent | Cluster | Gap Type | Priority |
|---|-------------|----------------|--------|---------|----------|---------|

METADATA TABLE:
| Element | Option 1 | Option 2 | Option 3 | Recommended |
|---------|---------|---------|---------|-------------|

LINK TABLE:
| # | Source | Target | Anchor Text | Placement | Type | Priority |
|---|--------|--------|-------------|-----------|------|---------|

VALIDATION TABLE:
| Step | Check | Result | Issues | Action |
|------|-------|--------|--------|--------|

SWOT TABLE:
| Strengths | Weaknesses |
|-----------|-----------|
| [data] | [data] |

| Opportunities | Risks |
|--------------|-------|
| [data] | [data] |

NOTE: SWOT uses 2x2 split table format, not 4-column single table.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*SKILL FILE END: core/output-formatter.skill*
*Nexus SEO Operating System — Version 1.0.0*
