---
name: brand-template
description: >-
  Universal brand profile template for Nexus SEO v3.3. Copy this file, rename to
  brand-[yourname].skill.md, fill in every section, and save to references/.
  Nexus auto-detects it at session start. The more detail you provide, the better
  Nexus performs across all 60 skills — especially audience targeting, internal
  linking, product mentions, CTA generation, and humanized content.
---

# Brand Profile Template — Nexus SEO v3.3
# ─────────────────────────────────────────────
# HOW TO USE
# ─────────────────────────────────────────────
# 1. Copy this file
# 2. Rename: brand-[yourname].skill.md
#    Examples: brand-mybakery.skill.md / brand-lawfirm.skill.md / brand-fitnessbrand.skill.md
# 3. Fill in every section — delete placeholder text, write real values
# 4. For deep audience profiles, also fill in brand-audience-template.skill.md
#    and reference it in Section 2 below
# 5. Save to references/ — Nexus auto-detects it on next session
#
# [REQUIRED] = system won't function properly without this
# [RECOMMENDED] = significantly improves output quality
# [OPTIONAL] = nice-to-have; won't break system if blank
# ─────────────────────────────────────────────


## SECTION 1 — BRAND IDENTITY [REQUIRED]

brand_name: "[Your Brand Name]"
# Exact name as it appears in content. Capitalization matters. E.g. "BrandName" not "brandname"

site_url: "https://yourdomain.com"
# Root domain. Used for cannibalization checks, sitemap fetching, internal link validation.

brand_type: "[product / service / both]"
# product = software, app, tool, platform, SaaS
# service = agency, consulting, outsourcing, freelance, professional services
# both = product + services bundled (e.g. software + implementation services)

industry: "[Your industry]"
# Examples: SaaS / QA Testing / Legal Services / Food & Beverage / Fitness & Wellness /
#           Financial Services / E-commerce / Healthcare / Real Estate / Education / Other

what_we_do: |
  [1–3 sentences. What the brand does, who it's for, and the core value it delivers.
  This is used in content introductions, brand mentions, CTAs, and landing page copy.

  Example (SaaS): "Acme Analytics is a real-time dashboarding platform for growth teams.
  It connects to any data source and surfaces revenue-impacting trends automatically.
  Marketing and product teams use it to make faster decisions without needing a data analyst."

  Example (service): "Greenleaf Legal is a family law firm serving clients in Gujarat.
  We handle divorce, custody, and property disputes with a focus on fast resolution
  and keeping costs predictable for families under financial stress."

  Example (local/retail): "Sunrise Solar Solution installs solar panels for homeowners
  in Surat and surrounding areas. We handle everything from site assessment to
  government subsidy paperwork so homeowners start saving from day one."]

tagline: "[Optional one-line tagline — used in metadata and CTAs]"


## SECTION 2 — TARGET AUDIENCE [REQUIRED]

# This section is the most important input for content quality.
# Every piece of content Nexus generates reads this to write for the right person.
# For deep persona profiles, fill in brand-audience-template.skill.md and paste
# the persona data there — then reference it here.

primary_audience:
  role: "[Job title or persona label — e.g. Engineering Manager / Head of QA / CMO / Personal Trainer]"
  seniority: "[junior / mid / senior / executive / mixed]"
  expertise_level: "[beginner / intermediate / expert / varies]"
  # Used by content-generation-engine to calibrate how much to explain vs assume

  pain_points:
    - "[Specific pain point 1 — the more specific the better]"
    - "[Specific pain point 2]"
    - "[Specific pain point 3]"
    - "[Specific pain point 4]"

  goals:
    - "[Goal 1 — what are they trying to achieve professionally]"
    - "[Goal 2]"
    - "[Goal 3]"

  what_they_search_for:
    - "[The kind of queries this person types into Google]"
    - "[Another search pattern]"
    - "[Another search pattern]"

  how_they_evaluate_solutions:
    - "[What matters most to them when choosing a tool/service — e.g. speed of implementation, price, peer reviews, enterprise compliance]"
    - "[Second evaluation criterion]"
    - "[Third evaluation criterion]"

  objections:
    - "[Common reason they don't buy or convert — e.g. 'We already have a tool for this']"
    - "[Another objection — e.g. 'Too expensive for our stage']"
    - "[Another objection]"

secondary_audience:
  role: "[Job title or persona label]"
  seniority: "[junior / mid / senior / executive]"
  expertise_level: "[beginner / intermediate / expert]"

  pain_points:
    - "[Pain point 1]"
    - "[Pain point 2]"

  goals:
    - "[Goal 1]"
    - "[Goal 2]"

# Add more audience segments by duplicating either block.
# Label them tertiary_audience, audience_segment_3, etc.

extended_audience_profile: ""
# Optional but highly recommended for better content quality.
# Set this to the filename of your filled-in audience profile:
#   extended_audience_profile: "brand-[yourname]-audience.skill.md"
# When set, content-research-engine and humanizer read:
#   - JTBD statements (shapes content angle)
#   - Buying committee (shapes landing page copy)
#   - Language patterns — terms_they_use / terms_they_avoid (shapes humanizer voice check)
#   - primary_content_voice (direct writing instruction for generation + humanization)
# Without this file, Nexus works from the basic audience fields above.
# With it, every piece of content sounds like it was written by someone who knows the reader personally.

audience_language_patterns:
  # Words and phrases your audience actually uses (for humanizer to match)
  terms_they_use:
    - "[Industry jargon or preferred terminology]"
    - "[Another term]"
    - "[Another term]"
  terms_they_avoid:
    - "[Term that sounds out of touch or salesy to this audience]"
    - "[Another]"


## SECTION 3 — BRAND TONE & VOICE [REQUIRED]

tone: "[e.g. technical and direct / conversational and warm / authoritative and formal / witty and casual / empathetic and supportive]"

technical_level: "[beginner-friendly / intermediate / expert / varies by piece]"

writing_style_notes: |
  [Describe exactly how content should sound. Be specific. Include:
  - Sentence length preference
  - First-person / second-person / third-person preference
  - Preferred vocabulary and framing
  - How claims should be backed up (data? stories? examples? all three?)
  - What the brand never does in writing

  Example: "Write like a senior engineer talking to another senior engineer.
  No hand-holding, no fluff. Short sentences. Use 'you' directly. Back every
  claim with real data or a concrete example. Avoid buzzwords like 'leverage',
  'synergy', 'game-changer'. Prefer 'use', 'build', 'ship', 'debug'.
  The reader is smart — respect their time."]

banned_words:
  # Words or phrases to NEVER use in content for this brand.
  # The humanizer checks every draft against this list.
  - "[banned word or phrase]"
  - "[banned word or phrase]"
  - "[banned word or phrase]"
  # Add as many as needed. Common ones: "leverage", "synergy", "best-in-class",
  # "cutting-edge", "game-changer", "seamlessly", "robust", "innovative"


## SECTION 4 — CONTENT PILLARS [REQUIRED]

# Main topic areas the brand creates content about.
# keyword-discovery seeds research from these pillars.
# roadmap-engine structures the content roadmap around them.

content_pillars:
  - pillar: "[Pillar 1 — broad topic area]"
    subtopics:
      - "[Subtopic A]"
      - "[Subtopic B]"
      - "[Subtopic C]"

  - pillar: "[Pillar 2]"
    subtopics:
      - "[Subtopic A]"
      - "[Subtopic B]"

  - pillar: "[Pillar 3]"
    subtopics:
      - "[Subtopic A]"
      - "[Subtopic B]"

# Add more pillars as needed. Aim for 3–6 pillars total.


## SECTION 5 — PRODUCTS & SERVICES [REQUIRED]

# List every product, feature, or service offering.
# Maturity labels control what Nexus will and won't promote.
# technical-accuracy-checker verifies every feature mention against these labels.

features:
  - name: "[Feature or product/service name]"
    description: "[1–2 sentences: what it does and who it's for]"
    maturity: "[STABLE / BETA / COMING_SOON / UNRELIABLE]"
    url: "https://yourdomain.com/feature-page"

  - name: "[Feature or product/service name]"
    description: "[description]"
    maturity: "[STABLE / BETA / COMING_SOON / UNRELIABLE]"
    url: ""

# Maturity label rules:
#   STABLE      = promote fully; no qualifications needed
#   BETA        = mention with "currently in beta" qualifier
#   COMING_SOON = do not mention in content unless user explicitly asks
#   UNRELIABLE  = never promote; flag if it appears in drafts

differentiators:
  # What makes this brand genuinely better than alternatives.
  # landing-page-engine uses these for superiority framing.
  # Be specific. Not "we're faster" — "we cut test report generation time by 80%".
  - "[Differentiator 1 — specific and evidence-backed]"
  - "[Differentiator 2]"
  - "[Differentiator 3]"

pricing_model: "[freemium / subscription / one-time / usage-based / custom / not disclosed]"
# Used when writing about pricing-adjacent topics.


## SECTION 6 — INTERNAL URL REPOSITORY [REQUIRED]

# The richer this section, the better your internal linking.
# Nexus pulls internal links ONLY from this list — it never invents URLs.
# Organize by content type. Add every page that should ever be linked to.

site_size: "[small / medium / large]"
# small  = under 50 indexable pages  → Nexus targets 4–6 internal links per post
# medium = 50–200 indexable pages    → Nexus targets 7–10 internal links per post
# large  = 200+ indexable pages      → Nexus targets 12+ internal links per post

urls:

  blog:
    - url: "https://yourdomain.com/blog/post-slug"
      title: "[Exact post title]"
      topic: "[1–3 word topic tag — used for relevance matching]"

    - url: "https://yourdomain.com/blog/post-slug-2"
      title: "[Post title]"
      topic: "[topic tag]"

    # Add every blog post. The more you add, the better the linking.

  features:
    - url: "https://yourdomain.com/features/feature-name"
      title: "[Feature page title]"
      topic: "[topic tag]"

  landing_pages:
    - url: "https://yourdomain.com/vs/competitor-name"
      title: "[Comparison page title]"
      topic: "[topic tag]"

    - url: "https://yourdomain.com/alternatives/competitor-name"
      title: "[Alternatives page title]"
      topic: "[topic tag]"

  docs:
    - url: "https://docs.yourdomain.com/getting-started"
      title: "[Doc title]"
      topic: "[topic tag]"

  use_cases:
    - url: "https://yourdomain.com/use-cases/use-case-name"
      title: "[Use case page title]"
      topic: "[topic tag]"

  case_studies:
    - url: "https://yourdomain.com/case-studies/client-name"
      title: "[Case study title]"
      topic: "[topic tag]"

  integrations:
    - url: "https://yourdomain.com/integrations/tool-name"
      title: "[Integration page title]"
      topic: "[topic tag]"

  pricing:
    - url: "https://yourdomain.com/pricing"
      title: "Pricing"
      topic: "pricing"

  homepage:
    - url: "https://yourdomain.com"
      title: "[Brand Name] — [Tagline]"
      topic: "homepage brand overview"

# Add more URL categories as needed. Use whatever matches your site structure.
# The category names are flexible — rename, remove, or add any of these:
#
# For SaaS / B2B tech:
#   docs, changelog, integrations, use_cases, case_studies, webinars
#
# For agencies / consulting / services:
#   services, team, process, industries, results, testimonials
#
# For e-commerce / retail:
#   products, collections, lookbooks, size_guides, reviews, store_locator
#
# For food / hospitality:
#   menu, reservations, locations, events, catering, gallery
#
# For fitness / health / wellness:
#   programs, trainers, schedules, nutrition, client_results, booking
#
# For legal / professional services:
#   practice_areas, attorneys, results, resources, faqs, contact
#
# For local businesses:
#   services, locations, gallery, reviews, booking, about
#
# For content creators / media:
#   articles, videos, courses, newsletter, tools, about
#
# Common to all: about, contact, faqs, resources, ebooks, guides, tools


## SECTION 7 — COMPETITORS [REQUIRED]

# Starting list for competitor-analysis and gap-opportunity-engine.
# Also used for landing page positioning.

competitors:
  - name: "[Competitor brand name]"
    domain: "https://competitordomain.com"
    their_strengths:
      - "[What they genuinely do well — be honest]"
      - "[Another strength]"
    our_advantage_over_them: "[Why your brand wins vs this specific competitor — be specific]"

  - name: "[Competitor brand name]"
    domain: "https://competitordomain2.com"
    their_strengths:
      - "[Strength]"
    our_advantage_over_them: "[Advantage]"

# Add as many competitors as relevant. Even 2–3 is enough to seed analysis.


## SECTION 8 — CTA & UTM PREFERENCES [REQUIRED]

primary_cta_text: "[e.g. Start Free Trial / Book a Demo / Get Started / Try Free / Contact Us]"
primary_cta_url: "https://yourdomain.com/signup"

secondary_cta_text: "[e.g. See Pricing / View Demo / Read Case Study / Download Guide]"
secondary_cta_url: "https://yourdomain.com/pricing"

utm_format:
  source: "[e.g. blog / organic / content / newsletter]"
  medium: "[e.g. article / post / content / referral]"
  # Nexus generates: ?utm_source=[source]&utm_medium=[medium]&utm_campaign=[keyword-slug]
  # Example: ?utm_source=blog&utm_medium=article&utm_campaign=playwright-test-reporting

cta_style: "[button / inline-text / both]"
# button      = CTA appears as a standalone button or callout block
# inline-text = CTA woven into paragraph text naturally
# both        = use button style at end of post, inline style mid-article


## SECTION 9 — BRAND MENTION RULES [REQUIRED]

mention_frequency: "[low / medium / high]"
# low    = 1–2 brand mentions per piece (subtle, editorial tone — works for thought leadership)
# medium = 3–5 brand mentions per piece (balanced branded + educational)
# high   = 6+ brand mentions per piece (heavily brand-led — works for bottom-funnel content)

mention_style: |
  [Describe exactly how to mention the brand in content.
  Example: "Introduce the brand in the intro paragraph as a direct solution to the
  reader's problem — don't just drop the name, tie it to a specific pain point.
  Use feature names (not just 'our tool'). Avoid sounding like an ad: every mention
  should give the reader something — a fact, a capability, a result. End pieces with
  a CTA section that names the brand and its primary benefit."]

first_mention_format: "[e.g. 'BrandName (yourdomain.com)' / 'BrandName' / 'the BrandName platform']"
# How brand name appears the very first time in any article.


## SECTION 10 — CONTENT DEFAULTS [REQUIRED]

code_examples: "[ON / OFF]"
# ON  = code-generation-preview activates when SERP competitors show code
#       (typical for dev tools, engineering platforms, technical SaaS)
# OFF = skip code examples — correct for non-technical brands
#       (food, fitness, legal, lifestyle, retail, consulting, etc.)
# Even when ON, Nexus only includes code if SERP competitors for that keyword show code.

infographic_style: |
  [Describe your brand's visual style for infographics, or write "default" to use
  Nexus standard styling.
  Example: "Use dark navy (#0F172A) as background, electric blue (#3B82F6) as accent,
  white text. Geist Mono for code. Inter for body. Clean, minimal, no gradients."]

schema_preference: "[auto / Article / HowTo / FAQPage / Product / Review / LocalBusiness / SoftwareApplication / Service / Other]"
# "auto" = Nexus picks schema by content type (recommended for most brands)
# Specify a type only if your brand always needs a specific schema.

content_language: "[en-US / en-GB / en-IN / fr-FR / de-DE / es-ES / other locale code]"
# Used by metadata-generator and output-formatter for locale-specific formatting.


## SECTION 11 — CROSS-LINKING RULES [AUTO-ENFORCED]

# Nexus automatically enforces: content for this brand NEVER links to any other
# registered brand's URLs. This section is for exceptions only.

allowed_external_brands: []
# Controls cross-linking exceptions. Leave empty [] for strict brand isolation (correct for almost all brands).
#
# Format: list of brand_name values (the exact string from brand_name field in each brand file)
# Example: ["SisterBrand", "PartnerBrand"] — only populate if intentional cross-linking exists
#
# Only populate if this brand intentionally shares audience or content with another registered brand
# AND you want Nexus to allow cross-linking between them.
# The value here must exactly match the brand_name field in the other brand's profile file.
# If the names don't match exactly, the exception is ignored and strict isolation applies.


## SECTION 12 — GSC & ANALYTICS [RECOMMENDED]

gsc_property: "[https://yourdomain.com or sc-domain:yourdomain.com or leave blank]"
# Your Google Search Console property string. Used by gsc-integration skill.

primary_geo_markets:
  - "[Primary market — e.g. United States / India / United Kingdom / Global / Southeast Asia]"
  - "[Secondary market if applicable]"
# Used by seo-aeo-geo-sxo-optimizer for GEO scoring.
# Leave as ["Global"] if the brand targets all markets equally.

---
# END OF BRAND PROFILE
# Save as: brand-[yourname].skill.md in references/
# For deeper audience profiles: also fill in brand-audience-template.skill.md
