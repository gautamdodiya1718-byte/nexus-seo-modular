# heading-rhetoric.skill.md

**Role:** H2/H3 rewrite reference. Use when blog-post-auditor reaches Section 3 (Heading Audit) and Phase 3 (Rewritten H2 Structure).

---

## The Triple Test

Every H2 must pass all 3:

1. **Keyword Test** — Contains the primary keyword or a close semantic variant
2. **Answerability Test** — Can be rephrased as a question an LLM would answer with this section
3. **Tension Test** — Creates an information gap, promise, or challenge

---

## 7 Rhetorical H2 Patterns

| Pattern | Structure | Example |
|---|---|---|
| **The Paradox** | "Why [common belief] [contradicts reality]" | "Why faster test generation creates slower CI pipelines" |
| **The Hidden Cost** | "[Thing everyone does] costs you [specific thing]" | "MCP's 114K token sessions cost you $X/month at scale" |
| **The Counter-Narrative** | "The case against [popular approach]" | "The case against generating Playwright tests one at a time" |
| **The Specificity Hook** | "[Exact number] + [concrete outcome]" | "5 Claude Code prompts that produce CI-stable Playwright tests" |
| **The Insider Knowledge** | "What [authority/docs] won't tell you about [X]" | "What Playwright's docs skip about MCP token overhead" |
| **The Decision** | "When to use [X] vs [Y] for [outcome]" | "When to use MCP vs CLI for Playwright test generation" |
| **The Proof** | "How [we/team] [achieved specific result]" | "How we generated 82 E2E tests from a single Claude Code session" |

---

## Generic Heading Replacements

When you encounter a generic heading, transform it using this lookup:

| Generic Heading | Replacement Pattern | Example Rewrite |
|---|---|---|
| Prerequisites | Specificity Hook or Context | "What you need before Claude Code writes a single test" |
| Getting Started | Counter-Narrative or Specificity | "Skip the setup guide. Here's what actually matters." |
| Best Practices | Specificity Hook | "The 3 rules that separate stable tests from flaky ones" |
| Conclusion | Immediacy or delete | "What to try first Monday morning" or remove entirely |
| Overview | Insider Knowledge or Paradox | "What most Playwright guides get wrong about architecture" |
| Introduction | Remove — intro content belongs before the first H2 | Remove. Move content to the introduction section. |
| Summary | Delete or convert to action | "The one-page workflow you can share with your team" |
| Configuration | Hidden Cost or Decision | "The 4 config options that determine whether your tests pass in CI" |
| Examples | Proof | "How this looks in a real project with 200+ tests" |
| Comparison | Decision | "When to choose [X] over [Y] (and when the answer flips)" |
| Troubleshooting | Specificity Hook | "The 5 errors you'll hit and exactly how to fix each one" |
| Setup | Counter-Narrative | "The setup step most guides skip that saves hours later" |
| Next Steps | Immediacy | "What to run first — this week" |
| Resources | Delete or convert | Remove. If links are valuable, embed them inline. |
| FAQ | Keep but reframe | "FAQ: [Primary Keyword]" — lead questions with LLM query patterns |

---

## CTA Heading Audit

When auditing CTAs, check against these patterns:

| CTA Type | Good Example | Bad Example |
|---|---|---|
| The Implication | "Your [problem area] already generates data. See what's hiding in it." | "Try [brand.brand_name] today!" |
| The Specific Outcome | "See which tests flaked on WebKit but passed on Chromium this week." | "Sign up for free!" |
| The Challenge | "Still copy-pasting CI logs into Slack?" | "Transform your testing workflow" |
| The Contrast | "Without [brand.what_we_do core capability], you're guessing. With [brand.brand_name], you know." | "Leverage AI-powered insights" |

Note: [brand.brand_name] and [brand.what_we_do] are read from the active brand file.
Never hardcode a specific brand name inside this file.

If a CTA matches a "Bad Example" pattern, flag it and suggest a rewrite using a "Good Example" pattern.

---

## H3 Rules

- H3s must be subordinate to their parent H2 in both topic and depth
- A cluster of 4+ H3s under 1 H2 is a signal the H2 should be split
- H3s can use a simpler form of rhetorical patterns — specificity hook is the most common
- Orphan H3s (no parent H2 in scope) must be restructured, not just renamed

---

## Heading Hierarchy Errors to Flag

| Error | Signal | Fix |
|---|---|---|
| Orphan H3 | H3 appears without a prior H2 in the same section | Add a parent H2 or promote the H3 |
| H2 that should be H3 | Very narrow subtopic that could nest under the prior H2 | Demote to H3 |
| Keyword-stuffed heading | Same keyword phrase repeated in 3+ consecutive headings | Vary with semantic alternatives |
| Double-duty H2 | Heading covers 2 distinct topics | Split into 2 H2s |
| Generic ladder | Introduction → Overview → Getting Started → Best Practices → Conclusion | Full rewrite using rhetoric patterns |
