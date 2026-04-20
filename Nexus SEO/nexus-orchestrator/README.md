# Nexus Orchestrator

Optional automation layer for the Nexus SEO family. Chains other Nexus skills automatically based on user goal. Replicates the unified Nexus menu experience but skill-aware — only offers options the user has installed.

This skill is OPTIONAL. Every other Nexus skill (nexus-research, nexus-content, nexus-audit, nexus-tech-seo, nexus-strategy) works completely standalone without it. Install Orchestrator only if you want automated chaining.

## What it does

- **Detects installed Nexus skills** and offers a unified menu
- **Goal classification** — maps user requests to skill chains
- **Chained execution** — runs research → content → audit (or other patterns) automatically
- **Mandatory output-block enforcement** — every chained skill emits structured outputs; next skill verifies before consuming
- **Drift detection** — catches the "Claude reads pipeline and skips to writing" failure mode and re-enters at missed point
- **Pre-flight check** — verifies brand profile, author profile, user preferences are loaded before chaining
- **Pipeline accountability log** — every chained run produces a structured log showing what executed, what skipped, why
- **Goal deduplication** — when user picks multiple goals, shared steps run once

## Pre-requisites

Install at least 1 of:
- `nexus-research`
- `nexus-content`
- `nexus-audit`
- `nexus-tech-seo`
- `nexus-strategy`

Single-skill installations don't need this orchestrator (just invoke the skill directly).

## Install

1. Ensure 2+ other Nexus skills are installed
2. Download `nexus-orchestrator.zip`
3. Unzip
4. Upload to your Claude project
5. Invoke with `/nexus` or describe what you want in natural language

## The Menu

When invoked, the orchestrator detects which Nexus skills are present and shows a menu:

```
What do you want to do?
 1. Write a blog post              [needs nexus-research + nexus-content]
 2. Write a landing page           [needs nexus-content]
 3. Keyword research               [needs nexus-research]
 4. Audit a URL                    [needs nexus-audit]
 5. Why isn't [URL] ranking?       [needs nexus-audit + nexus-tech-seo]
 ...
```

Greyed-out options indicate which skills you'd need to install for that workflow.

## Chain Examples

**"Write a blog on [topic]"** →
1. nexus-research (full research package)
2. nexus-content (standard_blog using research)
3. nexus-audit (full_audit on generated content)

**"Why isn't [URL] ranking?"** →
1. nexus-audit (full_audit)
2. nexus-tech-seo (full_audit) — runs in parallel
3. nexus-tech-seo (content_tech_bottleneck synthesis)

**"Full SEO strategy"** →
1. nexus-research (full research package)
2. nexus-strategy (full_strategy)

## Why use this vs invoking skills directly?

Direct skill invocation:
- Pro: minimal context use; you control flow
- Con: must invoke each skill separately; manual handoff of outputs

Orchestrated:
- Pro: automated chaining; deduplication; accountability log
- Con: heavier context use; loads multiple skills per session

For free users with limited context: invoke skills directly.
For pro users with 200K+ context: orchestrator gives the full automation experience.

## Version

v1.0.0 — initial release. Skill-aware menu, mandatory output-block enforcement, drift detection, pipeline accountability log.
