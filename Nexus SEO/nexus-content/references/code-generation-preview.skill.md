# SKILL: content/code-generation-preview.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Technical Media
# EXECUTION PRIORITY: DURING CONTENT GENERATION — Runs when content-generation-engine or content-engagement identifies a code example insertion point.
# POSITION: Generates technically accurate, runnable Playwright code examples, executes them in a sandboxed environment, captures annotated screenshots of the results, and produces embeddable code blocks + preview images for blog content.
# DEPENDENCIES: brand-[name].skill.md (fields: urls, features, code_examples)

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill generates runnable code examples for technical content. It activates only
when brand.code_examples = ON AND SERP competitors for the target keyword show code.
Code examples must be 100% accurate, runnable, and appropriate to the brand's
technical domain (read brand.industry and brand.content_pillars to determine language,
framework, and domain).

### Core Workflow
```
1. Generate code → 2. Lint it → 3. Run it → 4. If fails, fix + retry (max 3x)
   → 5. Capture screenshot → 6. Annotate screenshot → 7. Produce embeddable output
```

### What This Skill Produces
| Output | Format | Purpose |
|---|---|---|
| Code block | Syntax-highlighted code | Embeddable in WordPress via wordpress-code-format skill |
| Terminal output screenshot | Annotated PNG | Shows test pass/fail results |
| Browser screenshot | Annotated PNG | Shows what Playwright sees in the browser |
| Code + Preview composite | HTML block | Code block with preview image below it |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — INPUT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Required Inputs
| Input | Source | Description |
|---|---|---|
| Code context | Content pipeline | What the code example should demonstrate (e.g., "show how to use getByRole for a login form") |
| Article topic | Content pipeline | The broader article topic for context |
| Target audience level | Content pipeline or inferred | Beginner / Intermediate / Advanced |

### Configuration Defaults
| Setting | Default | Override Rule |
|---|---|---|
| Language | Match to audience (TS for advanced, JS for beginner) | User can override |
| Demo/test URL | Read from brand.urls (homepage or docs section). If none suitable, use a publicly accessible, real URL relevant to the code example. Never hardcode a URL from a specific brand. |
| Playwright version | Latest stable (pin in package.json) | Always state version in code comments |
| Screenshot type | Annotated (arrows + highlights) | User can request raw output |
| Max retries on failure | 3 | Fixed |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — EXECUTION STEPS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Step 1: Environment Setup (Per Session)
Run once at the start of any content generation session that includes code:
```bash
# Initialize Playwright project
mkdir -p /home/claude/playwright-demo && cd /home/claude/playwright-demo
npm init -y
npm install @playwright/test@latest
npx playwright install chromium
```
Record the installed Playwright version for code comments.

### Step 2: Code Generation
Based on the context provided by the content pipeline:

#### 2a. Determine Language
| Audience Level | Default Language | Rationale |
|---|---|---|
| Beginner | JavaScript | Lower barrier to entry, no type annotations to explain |
| Intermediate | TypeScript | Community standard, shows real-world patterns |
| Advanced | TypeScript | Expected by advanced users, enables type-safe examples |

#### 2b. Code Style Rules
- **Import style:** Always use `import { test, expect } from '@playwright/test'` (not require)
- **Async/await:** Always use async/await (never .then chains)
- **Naming:** Use descriptive test names that explain what's being tested
- **Comments:** Include 2-3 inline comments explaining the "why" (not the "what")
- **Length:** Keep examples focused — 15-40 lines for simple examples, up to 80 lines for complex patterns
- **Version pinning:** Add a comment at the top: `// Tested with Playwright v{version}`
- **Flat scripts for simple examples, POM for advanced tutorials**
- **No placeholder URLs:** Always use real, accessible URLs (read from brand.urls or use a publicly accessible URL relevant to the example)

#### 2c. Code Templates by Example Type
| Example Type | Template Pattern |
|---|---|
| Basic locator | Single test, 1 page.goto, 2-3 locator actions, 1-2 assertions |
| Form interaction | test with page.goto → fill → click → assert result |
| API testing | test with request.get/post → status check → body assertion |
| Visual comparison | test with expect(page).toHaveScreenshot() |
| Network mocking | test with page.route → mock response → assert UI change |
| Multi-page flow | test with sequential page navigations + assertions |
| Parallel/sharding | playwright.config.ts showing worker/shard configuration |
| Custom reporter | Custom class implementing Reporter interface |

### Step 3: Validation
```
3a. Lint: Run the code through TypeScript/ESLint checks
3b. Execute: npx playwright test {file} --reporter=list
3c. Check exit code:
    - Exit 0 → PASS → proceed to screenshot
    - Exit 1 → FAIL → analyze error → fix → retry (max 3 attempts)
    - After 3 failures → FLAG TO USER with error details, do not publish broken code
```

### Step 4: Screenshot Capture

#### 4a. Terminal Output Screenshots
- Run the test and capture terminal output
- Take a screenshot of the terminal output showing pass/fail results
- Crop to show only relevant output (no empty space, no system prompts)

#### 4b. Browser Screenshots
- If the test involves visual interaction, capture the browser state at key moments
- Use `page.screenshot({ path: 'output.png', fullPage: false })` for viewport shots
- Use `page.screenshot({ path: 'output.png', fullPage: true })` for full-page shots

#### 4c. Trace Screenshots
- If demonstrating trace viewer, capture the trace viewer UI
- Use `npx playwright show-trace trace.zip` and screenshot the viewer

### Step 5: Screenshot Annotation (Default Behavior)
Add visual annotations to screenshots to highlight key information:

| Annotation Type | When to Use | Style |
|---|---|---|
| **Red arrow** | Point to the specific line/element being discussed | 3px red arrow, drop shadow |
| **Highlight box** | Emphasize a section of terminal output | Semi-transparent yellow overlay |
| **Callout text** | Label a specific area | White text on dark background badge |
| **Number markers** | When explaining sequential steps | Circled numbers (1, 2, 3) |

**Ask user preference before applying annotations.** Default is annotated. If user requests raw output, provide both annotated and raw versions.

### Step 6: Output Assembly
Produce the final embeddable output:

```
[CODE BLOCK - syntax highlighted, with copy button, language tag]
// Tested with Playwright v{version}
// ... code ...

[PREVIEW IMAGE - annotated screenshot below the code]
Alt text: "{descriptive alt text for SEO}"
Width: 720px (or content-appropriate)
Format: PNG (for screenshots), SVG (for diagrams)
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — ERROR HANDLING
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### When Code Fails to Run

| Attempt | Action |
|---|---|
| 1st failure | Analyze error message → fix the most likely cause → retry |
| 2nd failure | Re-examine the approach → consider if the test target URL is accessible → try alternative approach → retry |
| 3rd failure | **STOP.** Do not publish broken code. Report to user: "This code example failed after 3 attempts. Error: [error]. The test target [URL] may be down or the approach needs manual verification." |

### Common Failure Patterns and Fixes
| Error Pattern | Likely Cause | Fix |
|---|---|---|
| `page.goto: net::ERR_NAME_NOT_RESOLVED` | Test target URL is inaccessible | Switch to a known public URL or notify user |
| `Timeout 30000ms exceeded` | Element not found on page | Check locator accuracy, add explicit waits |
| `strict mode violation` | Multiple elements match locator | Make locator more specific (add .first(), .nth(), or more specific selector) |
| `expect(received).toBeVisible()` | Element exists but is hidden | Check if element needs interaction to become visible |
| `browser.newContext: browser has been closed` | Browser crashed or didn't install | Re-run `npx playwright install chromium` |

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — QUALITY STANDARDS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Every Code Example MUST
1. Run successfully without modification by the reader (assuming they have Playwright installed)
2. Use the pinned Playwright version stated in the comment
3. Target a real, accessible URL (not example.com or localhost)
4. Include meaningful assertions (not just navigation)
5. Follow Playwright best practices (no hard waits, accessibility-first locators, no force: true)
6. Have a descriptive test name that explains the scenario
7. Include at least 2 inline comments explaining the "why"

### Every Screenshot MUST
1. Be cropped to show only relevant content
2. Have descriptive alt text for SEO
3. Be annotated by default (arrows, highlights) unless user requests raw
4. Not exceed 720px width for blog display
5. Be saved in PNG format at reasonable quality (not blurry, not oversized)

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — INTEGRATION WITH OTHER SKILLS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Skill | How This Skill Integrates |
|---|---|
| `content-generation-engine` | Receives code example requests; returns code blocks + preview images |
| `content-engagement.skill` | Engagement skill identifies "insert code example here" points; this skill fulfills them |
| `infographic-image-engine.skill` | For architecture diagrams that accompany code; this skill handles code, infographic skill handles diagrams |
| `wordpress-code-format` | Code blocks are formatted using the wordpress-code-format skill for final HTML output |
| `brand-[name].skill.md (field: features)` | When code examples reference a specific brand feature, check maturity label first. Never generate code for COMING_SOON or UNRELIABLE features. |

---

*Code Generation & Live Preview — v1.0.0 | Nexus SEO Operating System*
