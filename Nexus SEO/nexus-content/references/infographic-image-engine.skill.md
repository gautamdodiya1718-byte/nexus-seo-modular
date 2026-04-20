# SKILL: content/infographic-image-engine.skill
# SYSTEM: Nexus — Modular AI-Powered SEO Operating System
# VERSION: 1.0.0
# LAYER: Content / Visual Media
# EXECUTION PRIORITY: DURING CONTENT GENERATION — Triggered by content-engagement.skill when a visual break is needed, or by content-generation-engine when a concept requires visual explanation.
# POSITION: Generates infographics for any brand. Reads brand.infographic_style from the active brand file. If brand.infographic_style = "default", applies Nexus standard styling. If a custom style is defined, applies that exactly.
# DEPENDENCIES: brand-[name].skill.md (fields: infographic_style, urls, brand_name)

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 1 — ROLE
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill generates visual content for any brand — infographics, flowcharts, comparison tables, architecture diagrams, checklists, timelines, and process diagrams. Design tokens are read from brand.infographic_style. Images are NOT text-heavy.

### Core Workflow
```
1. Identify what visual is needed → 2. Select image type → 3. Generate HTML
   → 4. Validate (no overlapping, text density check) → 5. Export SVG → 6. Generate alt text
```

### Critical Rules
1. **NOT text-heavy:** Maximum 40 words per infographic panel, maximum 6 panels per infographic
2. **Brand design system:** Read brand.infographic_style for fonts and colors. Default: Inter (body), monospace (code/labels). Apply brand-defined values when present.
3. **Grey/black/white palette** with light gradient backgrounds — no dark/black section flips
4. **No brand promotion inside infographics** — keep infographics editorial, not promotional. No "Powered by [brand]" text.
5. **No `position: absolute` for callouts near content** — overlap prevention is paramount
6. **Default width:** 720px, skill can adjust based on content type
7. **Zero content overlapping** — validate before export; if any element overlaps, fix and re-render

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 2 — IMAGE TYPE TAXONOMY
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Image Type | When to Use | Max Text | Example |
|---|---|---|---|
| **Flowchart** | Process with decision points | 30 words total | "How to choose a locator strategy" |
| **Architecture diagram** | System components and connections | 25 words total | "Playwright test runner architecture" |
| **Comparison table** | Side-by-side feature comparison | 40 words (labels + values) | "Playwright vs Cypress capabilities" |
| **Checklist** | Steps or requirements list | 35 words total | "Pre-merge test checklist" |
| **Timeline** | Chronological progression | 30 words total | "Testing framework market evolution" |
| **Process diagram** | Sequential workflow steps | 35 words total | "CI/CD test reporting pipeline" |
| **Matrix/Quadrant** | 2-axis comparison | 25 words total | "Coverage Priority Matrix" |
| **Data chart** | Quantitative data visualization | 20 words (labels only) | "Flaky test rates by root cause" |
| **Before/After** | Transformation comparison | 30 words total | "Before [brand solution] vs After [brand solution]" |
| **Hierarchy/Pyramid** | Priority or layered concepts | 25 words total | "Playwright locator priority pyramid" |
| **Code architecture** | Code structure/file organization | 30 words total | "Project folder structure" |

### Decision Logic: Which Image Type to Use
```
Is there a sequence of steps? → Flowchart or Process diagram
Is there a comparison between 2+ things? → Comparison table or Before/After
Is there a system with components? → Architecture diagram
Is there a list of items to check? → Checklist
Is there chronological data? → Timeline
Is there 2-axis evaluation? → Matrix/Quadrant
Is there numerical data? → Data chart
Is there a hierarchy or priority? → Hierarchy/Pyramid
Is there code organization? → Code architecture
```

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 3 — DESIGN SYSTEM ENFORCEMENT
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### DESIGN SYSTEM (resolved per brand)

  Background:   read brand.infographic_style for background color, else #0F172A (default)
  Accent:       read brand.infographic_style for accent color, else #3B82F6 (default)
  Text:         read brand.infographic_style for text color, else #FFFFFF (default)
  Body font:    read brand.infographic_style for font, else Inter (default)
  Code font:    read brand.infographic_style for code font, else monospace (default)
  If brand.infographic_style = "default" or blank: use the default values above.

### Component Rules
- Use shadcn-style component patterns: proper borders, rounded corners (`border-radius: 8px`), shadow-sm
- Minimum padding: 16px inside components
- Minimum spacing between elements: 12px
- Arrow styles: 2px stroke, rounded caps, grey (#9CA3AF) or black
- Connector lines: 1.5px stroke, dashed for optional, solid for required

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 4 — GENERATION PROCESS
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### Step 1: Generate HTML
Create the infographic as an HTML file using inline CSS. This allows:
- Live preview and iteration in Claude
- Easy adjustment of layout, spacing, and text
- Font loading via Google Fonts CDN
- Responsive behavior testing

```html
<!DOCTYPE html>
<html>
<head>
  <link href="https://fonts.googleapis.com/css2?family=Geist:wght@400;500;600&family=Geist+Mono&display=swap" rel="stylesheet">
  <style>
    /* Brand design system tokens — read from brand.infographic_style */
    body { font-family: 'Geist', sans-serif; margin: 0; padding: 24px; background: #FFFFFF; }
    .mono { font-family: 'Geist Mono', monospace; }
    /* ... component styles ... */
  </style>
</head>
<body>
  <!-- Infographic content -->
</body>
</html>
```

### Step 2: Validation Checks
Before proceeding to SVG export, run these checks:

| Check | Rule | Action if Failed |
|---|---|---|
| **Text density** | Count total words. Must be under the limit for the image type. | Remove text, replace with icons/visual elements |
| **Overlap detection** | No element may overlap any other element. Zero tolerance. | Increase spacing, resize elements, reflow layout |
| **Font check** | All text uses Geist or Geist Mono | Replace any non-system fonts |
| **Color check** | All colors from the approved palette | Replace unauthorized colors |
| **Width check** | Total width is 720px (or content-appropriate) | Resize to target width |
| **Readability** | All text is minimum 12px rendered size | Increase font size |
| **Branding** | No "Powered by [brand]" or similar promotional text | Remove any branding |

### Step 3: SVG Export
Convert the validated HTML to SVG:
- Flatten all styles into inline SVG attributes
- Embed fonts as paths (or reference Google Fonts)
- Ensure all text is selectable (not rasterized)
- Optimize SVG (remove unnecessary groups, simplify paths)
- Validate SVG renders correctly at 720px width

### Step 4: Alt Text Generation
Generate SEO-optimized alt text for every image:
- Describe the visual content, not the concept
- Include the primary keyword naturally
- Keep under 125 characters
- Example: "Flowchart showing Playwright locator priority: getByRole first, then getByText, then CSS selectors as last resort"

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 5 — VIDEO EMBEDDING (YOUTUBE)
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

### YouTube Embed Strategy
When the content-engagement skill identifies a video insertion point:

1. **Video source:** Check brand.urls for any YouTube or video URLs. If present, prefer those.
   If no brand video URLs exist, find the most authoritative publicly available video on
   the topic. Never hardcode a specific brand's YouTube channel.

2. **Selection criteria:**
   - Must be directly relevant to the section topic (not just vaguely related)
   - Prefer videos under 15 minutes
   - Prefer videos from last 12 months
   - Prefer brand's own videos first, then authoritative third-party channels

3. **Embed format:**
```html
<iframe width="720" height="405" src="https://www.youtube.com/embed/{VIDEO_ID}" 
  title="{descriptive title}" frameborder="0" 
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
  allowfullscreen loading="lazy"></iframe>
```

4. **Context text:** Always add 1-2 sentences before the embed explaining what the video covers and why it's relevant.

### When NOT to Embed Video
- No relevant video exists on any of the 3 approved channels
- The section is too short (under 200 words) to justify a video break
- Two videos already appear in the article (max 2 video embeds per article unless the article is specifically a video compilation)

---

## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## SECTION 6 — GIF HANDLING
## ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This skill does NOT create GIFs. When the content-engagement skill identifies a GIF insertion point:
1. Suggest to the user: "A GIF showing [specific interaction] would be effective here. Do you have one, or would you like to record one?"
2. If user provides a GIF → integrate it with proper alt text and sizing
3. If user declines → skip and use an alternative visual (static screenshot or infographic)

---

*Infographic & Image Engine — v1.0.0 | Nexus SEO Operating System*
