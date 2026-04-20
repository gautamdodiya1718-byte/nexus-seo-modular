---
name: brand-audience-template
description: >-
  Extended audience profile template for Nexus SEO v3.3. Use this when you need
  deeper persona intelligence beyond what fits in the brand profile — jobs-to-be-done,
  decision journey, buying committee, language patterns, content consumption habits.
  Fill this in alongside brand-template.skill.md for maximum content quality.
  Reference this file from your brand-[name].skill.md or paste personas directly.
---

# Brand Audience Profile — Extended Template
# ─────────────────────────────────────────────
# HOW TO USE
# ─────────────────────────────────────────────
# Option A (recommended):
#   Copy this file, rename to brand-[yourname]-audience.skill.md
#   Fill in each persona
#   Save to references/ alongside your brand-[name].skill.md
#   Set extended_audience_profile: "brand-[yourname]-audience.skill.md" in your brand file
#
# Option B:
#   Copy the filled-in persona blocks and paste them directly into
#   Section 2 of your brand-[name].skill.md
#
# ─────────────────────────────────────────────
# WHICH SKILLS READ THIS FILE AND WHAT THEY USE
# ─────────────────────────────────────────────
#
# content-research-engine (M1 — MANDATORY)
#   Reads: jtbd_primary/secondary/tertiary, evaluation_criteria,
#          who_else_is_involved_in_the_decision, high_performing_content_types
#   Uses them to: shape the content brief angle, identify objections to address,
#                 select evidence type that resonates with this persona
#
# content-generation-engine (M3 — MANDATORY)
#   Reads: primary_content_voice, a_day_in_their_life, what_keeps_them_up_at_night,
#          what_success_looks_like_for_them
#   Uses them to: open articles with the reader's specific reality, not a generic hook
#
# humanizer (M7 — MANDATORY)
#   Reads: primary_content_voice, words_they_use, words_that_lose_them,
#          phrases_that_resonate, phrases_that_fall_flat, banned_words (from brand file)
#   Uses them to: rewrite AI-sounding sentences in the audience's actual vocabulary;
#                 voice check — would this person forward this article to a colleague?
#
# serp-blueprint-generator
#   Reads: content_strategy_by_funnel, high_performing_content_types, content_format_preference
#   Uses them to: align H2 structure with what this persona actually engages with
#
# landing-page-engine
#   Reads: jtbd_primary, objections (with counters), who_else_is_involved_in_the_decision,
#          evaluation_criteria, what_success_looks_like_for_them
#   Uses them to: write conversion copy that addresses the full buying committee,
#                 frame superiority claims around what this persona actually measures success by
#
# ─────────────────────────────────────────────

brand_name: "[Your Brand Name — must match brand_name in brand-[name].skill.md]"


## PERSONA 1 — PRIMARY BUYER / DECISION MAKER

persona_id: "P1"
persona_label: "[Short name — e.g. 'The QA Lead' / 'The Solo Founder' / 'The Head Chef']"

### Who They Are
role: "[Job title]"
seniority: "[junior / mid / senior / executive / owner]"
company_size: "[startup / SMB / mid-market / enterprise / solo]"
industry_they_work_in: "[Their industry — may differ from yours]"
reports_to: "[Who they answer to]"
team_size: "[How many people they manage or work with]"

### Their Reality
a_day_in_their_life: |
  [2–4 sentences describing a typical workday. What they're stressed about.
  What they're measured on. What they spend most of their time doing.
  Example: "Maya starts each morning triaging test failures from the overnight CI run.
  Half her day is investigating which failures are real bugs vs. flaky infrastructure.
  She's accountable to the dev lead for release readiness. She rarely has time to
  improve the test suite because she's always fighting fires."]

what_keeps_them_up_at_night:
  - "[Their biggest professional fear or anxiety]"
  - "[Another one]"
  - "[Another one]"

what_success_looks_like_for_them: |
  [What does a great week/month/quarter look like for this person?
  Example: "A week where the CI pipeline is green, no regressions shipped to production,
  and she had time to write two new test cases instead of just debugging old ones."]

### Jobs to Be Done (JTBD)
# The real reason they're looking for a solution — the job they're trying to accomplish.
# Format: "When [situation], I want to [motivation], so I can [expected outcome]."
jtbd_primary: "[When ___, I want to ___, so I can ___.]"
jtbd_secondary: "[When ___, I want to ___, so I can ___.]"
jtbd_tertiary: "[When ___, I want to ___, so I can ___.]"

### Information Behavior
where_they_learn:
  - "[Where they consume content — e.g. dev.to / Hacker News / LinkedIn / YouTube / newsletters / Slack communities]"
  - "[Another source]"
  - "[Another source]"

content_format_preference: "[long-form blog / quick how-tos / video tutorials / case studies / documentation / community posts / podcasts]"

search_behavior: |
  [How they search. Are they solution-aware or problem-aware?
  Do they search for tools by name or search for the problem they're solving?
  Example: "Searches for problems first ('why are my playwright tests flaky') before
  searching for tools. Only searches for specific products once they've validated
  the category is worth investing in."]

### The Buying Journey
how_they_discover_solutions:
  - "[Discovery channel 1 — e.g. colleague recommendation / Google search / conference / review site]"
  - "[Channel 2]"
  - "[Channel 3]"

evaluation_criteria:
  # What they actually care about when evaluating options. Be specific.
  - criterion: "[e.g. Ease of integration with existing stack]"
    weight: "[high / medium / low]"
  - criterion: "[e.g. Price relative to team size]"
    weight: "[high / medium / low]"
  - criterion: "[e.g. Quality of documentation]"
    weight: "[high / medium / low]"
  - criterion: "[e.g. Peer reviews and social proof]"
    weight: "[high / medium / low]"

who_else_is_involved_in_the_decision:
  # The buying committee — others who influence or block the decision.
  - role: "[e.g. CTO / CFO / Procurement / Security team]"
    their_concern: "[What this person cares about that could block the deal]"
  - role: "[role]"
    their_concern: "[concern]"

buying_timeline: "[impulse / days / weeks / months / quarters]"
# How long does it typically take from first touch to decision?

budget_authority: "[full / partial / none — needs approval]"

### Objections They Raise
objections:
  - objection: "[The exact words they use when pushing back]"
    our_counter: "[The honest, specific response that addresses this]"

  - objection: "[Another objection]"
    our_counter: "[Response]"

  - objection: "[Another objection]"
    our_counter: "[Response]"

### Language Patterns
# Critical for humanizer — content sounds authentic when it uses the reader's own language.
words_they_use:
  - "[Technical term or jargon they naturally use]"
  - "[Another]"
  - "[Another]"

words_that_lose_them:
  - "[Marketing speak that makes them tune out]"
  - "[Another]"

phrases_that_resonate:
  - "[A phrase or framing that clicks for this audience]"
  - "[Another — think about what gets upvoted in their communities]"

phrases_that_fall_flat:
  - "[A phrase that sounds hollow or out of touch to this person]"
  - "[Another]"

### Content That Works for This Persona
high_performing_content_types:
  - "[Content type that drives action for this persona — e.g. 'step-by-step tutorials with real code', 'before/after case studies', 'benchmark comparisons']"
  - "[Another]"
  - "[Another]"

what_makes_them_share_content:
  - "[What would make this person send an article to a colleague]"
  - "[Another]"

what_makes_them_ignore_content:
  - "[What causes them to bounce or not engage]"
  - "[Another]"

---

## PERSONA 2 — INFLUENCER / CHAMPION (the person who finds you but doesn't sign)

persona_id: "P2"
persona_label: "[Short label — e.g. 'The Senior Dev' / 'The Store Manager' / 'The Personal Trainer']"

role: "[Job title]"
seniority: "[junior / mid / senior / executive]"
relationship_to_P1: "[How this person interacts with the decision maker — e.g. reports to them / works alongside / advises]"

their_role_in_the_decision: |
  [Are they the one who finds solutions and recommends them up?
  Or do they veto solutions that don't meet technical standards?
  Example: "Arjun is the senior developer who actually uses the tool daily.
  He doesn't control budget but his thumbs-up or thumbs-down carries enormous weight.
  If he finds the tool difficult to use or poorly documented, the deal dies."]

what_they_care_about:
  - "[Their primary concern — e.g. implementation complexity / learning curve / quality of API]"
  - "[Secondary concern]"
  - "[Tertiary concern]"

how_to_write_for_them: |
  [What content strategy works for this persona.
  Example: "Write tutorials they can run themselves in under 30 minutes.
  Show real code. Show the output. Don't talk about ROI — they don't care.
  Talk about how it makes their life less painful technically."]

---

## PERSONA 3 — END USER (if different from buyer and influencer)

persona_id: "P3"
persona_label: "[Short label]"

role: "[Job title]"
seniority: "[junior / mid / senior]"
how_they_encounter_the_brand: "[Do they choose it or is it handed to them by their manager?]"

their_relationship_with_the_product: |
  [Daily user? Occasional user? Is it part of their workflow or optional?
  Do they love it, resent it, or don't think about it?]

what_content_would_help_them:
  - "[Content type that would genuinely help this user — e.g. onboarding guides, feature deep-dives, troubleshooting guides]"
  - "[Another]"

---

## AUDIENCE SUMMARY FOR CONTENT STRATEGY

# Fill this in AFTER completing the personas above.
# This is the synthesized version that content-research-engine uses as its brief input.

content_strategy_by_funnel:

  top_of_funnel:
    target_persona: "[P1 / P2 / P3 / all]"
    intent: "Problem-aware but solution-unaware"
    content_approach: "[e.g. Educational content about the problem category. No product mention or subtle at most.]"
    example_topics:
      - "[Topic idea that fits TOFU for this audience]"
      - "[Another]"

  middle_of_funnel:
    target_persona: "[P1 / P2 / P3 / all]"
    intent: "Solution-aware, evaluating options"
    content_approach: "[e.g. Comparison content, category guides, use case breakdowns. Product mentioned as solution.]"
    example_topics:
      - "[Topic idea that fits MOFU]"
      - "[Another]"

  bottom_of_funnel:
    target_persona: "[P1 / P2 / P3]"
    intent: "Ready to decide, needs final validation"
    content_approach: "[e.g. Case studies, vs pages, ROI calculators, detailed feature content. Strong CTA.]"
    example_topics:
      - "[Topic idea that fits BOFU]"
      - "[Another]"

primary_content_voice: |
  [One paragraph that synthesizes everything above into a writing instruction.
  This is read by content-generation-engine and humanizer to set the voice for every piece.

  Example: "Write for Maya — a senior QA lead at a 50-person engineering team.
  She's smart, time-pressed, and allergic to marketing language. She reads to solve
  problems, not to be sold to. Use her language: 'flaky tests', 'CI pipeline',
  'test reliability', 'failure rate'. Never say 'leverage' or 'robust'.
  Start with the problem. Back everything with data or a real example.
  She'll forward this article to her dev lead if it's genuinely useful —
  write at that standard."]

---
# END OF AUDIENCE PROFILE
# Save as: brand-[yourname]-audience.skill.md in references/
# Or copy persona blocks directly into Section 2 of brand-[yourname].skill.md
