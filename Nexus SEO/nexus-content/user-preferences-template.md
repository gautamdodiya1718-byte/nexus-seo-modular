# User Preferences Template

Personal rules that persist across all your Nexus sessions. Copy this file. Rename to `user-preferences-[your-name].md`. Fill in only the sections you care about (leave others blank or delete). Save to your project. Nexus skills will auto-detect it.

User preferences are personal — they apply to YOU regardless of which brand or author you're producing for. They're applied on top of brand profile and author profile.

---

## Always-Include Rules

Things you ALWAYS want in content, regardless of brand/author/topic.

### Universal structural elements
- [ ] Always include a TL;DR / summary block at the top of every article
- [ ] Always include a comparison table when discussing 2+ options
- [ ] Always include a "common mistakes to avoid" section
- [ ] Always include a final "what to do next" or call-to-action block
- [ ] Always include estimated reading time
- [ ] Always include author byline + bio link
- [ ] Always include publish date AND update date when content is refreshed
- [ ] Other: [describe]

### Universal content requirements
- [ ] Always include at least one specific numerical example
- [ ] Always include at least one quote or citation per H2
- [ ] Always include at least one diagram or infographic
- [ ] Always include external links to primary sources
- [ ] Always include disclaimer for [topic types — e.g., legal, medical, financial]
- [ ] Other: [describe]

---

## Never-Include Rules

Things you NEVER want in content. The humanizer will scrub these.

### Banned phrases (anti-vocabulary at user level)
List phrases you don't want appearing in your content, regardless of brand or author profile.

Example:
1. "It's important to note that"
2. "In conclusion"
3. "When it comes to"
4. "At the end of the day"
5. [your additions]

[Your list:]
1. [phrase]
2. ...

### Banned structural patterns
- [ ] No em dashes (—) anywhere
- [ ] No triple-dash horizontal rules (---)
- [ ] No passive voice (or: minimize passive voice)
- [ ] No paragraphs longer than 3 sentences
- [ ] No nested lists deeper than 2 levels
- [ ] No emojis in headings
- [ ] Other: [describe]

---

## Structural Preferences

How you prefer content to be structured.

### Intro structure
Pick or describe your preferred intro structure:
- [ ] Always 3 paragraphs (industry trend → pain point → what this guide does)
- [ ] Always opens with a specific stat or number
- [ ] Always opens with a specific scenario / story
- [ ] Always opens with a definition
- [ ] Always opens with a counter-intuitive observation
- [ ] Other: [describe]

### Section structure preferences
- [ ] Each H2 should start with a 1-2 sentence preview of what's coming
- [ ] Each H2 should end with a one-sentence takeaway
- [ ] Each H2 should include at least one specific example
- [ ] H3s should always be questions, not statements
- [ ] Other: [describe]

### Closing structure
- [ ] End with a specific next action
- [ ] End with a question for the reader
- [ ] End with a recap of the key points
- [ ] End with a "common mistakes to avoid" summary
- [ ] Other: [describe]

---

## Custom Quality Checks

Additional checks you want run on every piece of content. The system will apply these on top of standard quality gates.

### Custom checks (describe each)

1. **Check name:** [e.g., "Disclosure check"]
   **What to check:** [e.g., "If content reviews any product I have a commercial relationship with, must include disclosure block"]
   **Action on failure:** [BLOCK / WARN / FIX_AUTOMATICALLY]

2. **Check name:**
   **What to check:**
   **Action on failure:**

[Add as many as you want]

---

## Confidence Gate Override Settings

Default behavior: confidence gate BLOCKS content delivery if accuracy <75% (or higher thresholds for MOFU/BOFU/YMYL).

You can change this:
- [ ] **Strict mode (default)** — gate always blocks, no override allowed without explicit per-session approval
- [ ] **Standard mode** — gate blocks but I can say 'override confidence gate' per-session
- [ ] **Permissive mode** — gate warns but doesn't block; UNRELIABLE warning still appears in output
- [ ] **Override always granted** — gate runs but content always delivered with warnings (NOT recommended for YMYL content)

---

## Mode Defaults

Auto-confirm certain modes without showing heads-up:

- [ ] Auto-confirm `quick_score` mode (in nexus-audit)
- [ ] Auto-confirm `metadata_package` mode (in nexus-research)
- [ ] Auto-confirm `keyword_research` mode (in nexus-research)
- [ ] Other: [list modes you want auto-confirmed]

For all other modes, always show heads-up and wait for confirmation.

---

## Output Format Preferences

- [ ] Markdown (default)
- [ ] HTML
- [ ] Plain text
- [ ] WordPress-flavored markdown (with shortcodes for callouts, etc.)
- [ ] Other: [describe]

---

## Brand Defaults

If you work with multiple brands and have a default:

**Default brand:** [brand-[name].skill.md OR none — ask each session]

If specified: skills auto-load this brand without asking unless overridden.

---

## How Skills Use This Profile

| Skill | What it reads |
|---|---|
| All skills | Always-include rules, never-include rules, structural preferences |
| `humanizer` (in nexus-content) | Banned phrases, banned structural patterns |
| `confidence-gate` | Override settings |
| `content-generation-engine` | Intro structure, section structure preferences, closing structure preferences |
| `content-validation` | Custom quality checks |
| All entry points | Mode defaults, output format, default brand |

---

*User preferences are personal. They apply to YOU across all brands and projects you produce content for.*
