# Nexus Tech SEO

Standalone technical SEO diagnostic skill. Analyzes website source code, Google Search Console exports, and Google PageSpeed Insights reports to diagnose why a site isn't ranking — even when the content is great.

Part of the Nexus SEO family, but works completely independently. No other Nexus skill required.

## What it does

- **Source code analysis** — parses HTML/CSS/JS to extract SEO-relevant elements (titles, metas, headings, schema, links, alt text, OG/Twitter tags)
- **Crawlability check** — robots.txt, sitemap, canonical tags, noindex/nofollow, hreflang, redirect chains
- **On-page tech audit** — title length, meta description, H1 uniqueness, heading hierarchy, schema validity, image alt coverage, OG/Twitter card metadata
- **Core Web Vitals analysis** — parses PageSpeed Insights JSON; identifies LCP element, INP issues, CLS sources, render-blocking resources, image/font optimization opportunities
- **GSC insights** — parses GSC CSV/JSON exports; surfaces high-impression-low-CTR queries, position decline pages, intent mismatches
- **Cannibalization detection** — finds queries where multiple URLs compete; recommends consolidate-vs-differentiate
- **Ranking diagnostics** — backlink targets with specific numbers, AI crawler access check, technical fix list
- **Content-vs-tech bottleneck diagnosis** — when content quality is high but ranking is low, identifies whether content or tech is the actual issue

## Inputs (any combination)

- **Source code:** HTML files, CSS, JS, full site exports, .md exports of pages, ZIP files, or pasted content
- **GSC data:** queries report CSV, pages report CSV, performance JSON, coverage report
- **PageSpeed Insights:** full JSON output, screenshot, or pasted summary text
- **Optional:** robots.txt, sitemap.xml, hreflang config

The skill proactively asks for any missing inputs and works with whatever you provide. Confidence is lowered for missing inputs and flagged in the report.

## Install

1. Download `nexus-tech-seo.zip`
2. Unzip
3. Upload to your Claude project (or install as a skill)
4. Invoke with `/nexus-tech-seo` or describe what you want in natural language

## Brand profile (optional)

This skill works fine brand-agnostic. To add brand context (slightly more specific recommendations), create a brand profile from `brand-template.skill.md` and load it into your Claude session before invoking this skill.

## Modes

The skill auto-detects which mode you want from your prompt and shows a heads-up before running anything. You can change mode or cancel.

| Trigger phrase | Mode |
|---|---|
| "audit my site / tech SEO / why isn't my site ranking" | full audit |
| "PageSpeed / Web Vitals / LCP / INP / CLS" | Core Web Vitals only |
| "GSC / Search Console / queries" | GSC insights only |
| "cannibalization / two pages competing" | cannibalization only |
| "ranking diagnosis / backlink targets" | ranking diagnosis |
| "content is good but not ranking" | content vs tech bottleneck |
| "analyze my source code / HTML" | source code only |

## Output

A severity-ranked issue list:
- CRITICAL (blocking ranking)
- HIGH-PRIORITY (significantly limiting)
- MEDIUM/LOW (incremental)

Each issue includes: why it matters, specific fix action, estimated impact. Plus cannibalization flags, content-vs-tech diagnosis (when applicable), diagnostic confidence percentage, and re-audit trigger conditions.

## Version

v1.0.0 — initial release. Standalone, brand-agnostic-capable, multi-input.
