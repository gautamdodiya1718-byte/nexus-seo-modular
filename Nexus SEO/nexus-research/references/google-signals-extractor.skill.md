# google-signals-extractor.skill.md

**Role:** Capture Google's organic signal ecosystem around a primary keyword — autocomplete suggestions, People Also Ask (PAA), and Related Searches — then filter them through a 2-layer intelligence gate (Brand Filter → Topic Relevance Filter) to produce a clean, approved signal set that makes content more relevant than competitors across the full keyword neighbourhood.

**Pipeline position:** After `serp-filter`, parallel with `deep-serp-analysis`  
**Feeds into:** `content-pattern-extractor`, `serp-blueprint-generator`, `content-generation-engine`, `optimization-engine`

---

## Why This Matters

Google's suggested terms are direct evidence of how Google models the topic neighbourhood around a keyword. When your content naturally covers these terms, Google recognises the article as comprehensively relevant — not just for the primary keyword, but for the adjacent queries users actually type. This is how you rank for 20 related queries with 1 article, not just for the primary keyword.

But Google also surfaces terms that are topically adjacent without being brand-relevant. Blindly including every suggestion creates misaligned, surface-level content that dilutes positioning and confuses both readers and Google. The filter layer exists to prevent that.

---

## Inputs Required

1. **Primary keyword** — required; the keyword the article is being written for
2. **Article topic / working title** — required; used in Layer 2 (Topic Relevance Filter)
3. **Brand context** — auto-loaded from active brand-[name].skill.md (fields: content_pillars, industry); no input needed

---

## Phase 1: Signal Capture

### Step 1: Autocomplete Suggestions

Use `web_search` with the primary keyword. Capture the suggested completions that appear in the search interface. Also run these autocomplete expansion patterns:

- `[primary keyword] +` — what follows the keyword (how, why, when, best, vs, without, for)
- `[primary keyword] [A–Z]` — alphabetical expansion to surface less obvious variants

**Target:** Capture 10–20 raw autocomplete suggestions.

**Practical method:** Run 3–5 `web_search` calls with slight keyword variations and note the query suggestions returned. Also search for `[primary keyword] how`, `[primary keyword] vs`, `[primary keyword] best` to surface common completion patterns.

---

### Step 2: People Also Ask (PAA)

Use `web_fetch` on `https://www.google.com/search?q=[url-encoded-primary-keyword]`.

Extract the **initial visible PAA block only** — typically 4 questions. Do not expand PAA by clicking.

If `web_fetch` returns a rendered page, look for the section labelled "People also ask" and extract each question verbatim.

**Failsafe:** If `web_fetch` on Google is blocked or returns no PAA, run `web_search` for the keyword and extract any question-format suggestions from the result snippets. Mark as PAA-FALLBACK in the output.

**Target:** 4 PAA questions (or up to 4 from fallback).

---

### Step 3: Related Searches

Using the same `web_fetch` response from Step 2, scroll to the bottom of the SERP page and extract the "Related searches" or "Searches related to [keyword]" block.

**Failsafe:** If the bottom block is not present in the fetched content, run a second `web_search` with `[primary keyword] related searches` and extract relevant terms from results. Mark as RELATED-FALLBACK.

**Target:** 6–10 related search terms.

---

### Raw Signal Log Format

After capture, log all raw signals before filtering:

```
SOURCE: Autocomplete
RAW TERMS: [list]

SOURCE: PAA
RAW QUESTIONS: [list]

SOURCE: Related Searches
RAW TERMS: [list]

TOTAL RAW SIGNALS: [count]
```

---

## Phase 2: 2-Layer Filter

Run Layer 1 first on ALL signals. Then run Layer 2 on only the signals that passed Layer 1.

---

### Layer 1: Brand Signal Filter

**Purpose:** Eliminate signals that fall outside this brand's credible topic domain.
Misaligned signals would pull content into territory where the brand has no authentic
perspective.

Load active brand file. Read:
  brand.content_pillars   → defines the brand's credible topic domain
  brand.industry          → used to recognize adjacent topics worth keeping

KEEP a signal if it relates to any topic in brand.content_pillars or their subtopics.
KEEP adjacent signals if they relate to brand.industry and the brand has an
  authentic perspective on them.
DISCARD signals that are completely outside brand.content_pillars and brand.industry.

When a signal is discarded, log it as:
  DISCARDED: [signal] — outside [brand.brand_name]'s topic domain
  ([brand.content_pillars summary])

**Layer 1 decision output for each signal:**
```
SIGNAL: [term or question]
LAYER 1: PASS / REJECT
REASON: [one line — what brand criterion triggered the decision]
```

---

### Layer 2: Topic Relevance Filter

**Purpose:** From the Layer 1 approved set, eliminate signals that are brand-relevant but not relevant to THIS specific article. An approved signal that doesn't fit the article topic creates bloat, not depth.

**For each Layer 1-approved signal, answer these 3 questions:**

**Q1 — Does it deepen the article's primary argument?**
Would covering this term add a new dimension to what the article is already explaining, or would it just repeat what the primary keyword coverage already handles?
- Deepens = PASS
- Redundant or tangential = REJECT

**Q2 — Is there a natural integration point in the article?**
Can this term fit into an existing planned section without forcing a new section that dilutes focus? Or could it become a dedicated H3 that genuinely belongs?
- Natural fit = PASS
- Forced addition = REJECT

**Q3 — Would including this term risk making the article surface-level?**
Sometimes a signal appears relevant but would require the article to go wide instead of deep — touching on the term briefly without real substance. If covering the signal means shallow treatment, reject it.
- Can be covered with substance = PASS
- Would only get surface-level coverage = REJECT

**Layer 2 decision:** Signal must answer PASS to all 3 questions to be approved.

```
SIGNAL: [term or question]
LAYER 2: PASS / REJECT
Q1 (deepens article): PASS / REJECT — [one line]
Q2 (natural integration): PASS / REJECT — [one line]
Q3 (avoids surface-level): PASS / REJECT — [one line]
```

---

## Phase 3: Output

### Approved Signal Set

Organise approved signals into 4 categories:

**1. Primary Support Terms**  
Approved terms that directly reinforce the primary keyword and should appear naturally in body copy, headings, and metadata.

**2. FAQ Candidates**  
Approved PAA questions that match real user queries and should become FAQ section entries. Format: question as written by Google → rewrite as direct answer opening sentence.

**3. Section Expanders**  
Approved terms that suggest a subtopic the article should cover with a dedicated H2 or H3.

**4. Internal Link Hooks**  
Approved terms where [brand.brand_name] has or should have a dedicated page. Flag for internal linking rather than in-content coverage. Read from brand.urls.

---

### Output Table Format

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GOOGLE SIGNALS REPORT
Primary Keyword: [keyword]
Article: [working title]
Raw Signals Captured: [count] (Autocomplete: X | PAA: X | Related: X)
Layer 1 Approved: [count] / [total]
Layer 2 Approved: [count] / [Layer 1 count]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

APPROVED SIGNALS — USE THESE

Primary Support Terms:
  • [term] — Source: [Autocomplete/PAA/Related] — Integration: [where in article]
  • ...

FAQ Candidates:
  • Q: [exact Google question]
    A opener: [first sentence of direct answer]
  • ...

Section Expanders:
  • [term] — Suggested placement: [H2/H3 under which section]
  • ...

Internal Link Hooks:
  • [term] — Link target: [matching URL from brand.urls, or flag as LINK_NEEDED: {term}]
  • ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

REJECTED SIGNALS — DO NOT USE

Layer 1 Rejections (Brand Mismatch):
  • [term] — [reason]
  • ...

Layer 2 Rejections (Topic/Depth Mismatch):
  • [term] — [Q1/Q2/Q3 failure + reason]
  • ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Downstream Handoffs

**→ content-pattern-extractor**  
Pass the full Approved Signal Set alongside competitor pattern data. `content-pattern-extractor` merges both inputs into the unified content brief. Approved signals that appear in competitor content AND in Google signals are marked HIGH-PRIORITY — they are both structurally expected and algorithmically favoured.

**→ serp-blueprint-generator**  
Section Expanders from the approved set become candidate H2/H3 sections in the content blueprint. FAQ Candidates populate the FAQ section of the blueprint.

**→ content-generation-engine**  
Primary Support Terms are passed as must-include terms. The writer integrates them naturally — never as keyword inserts, always as part of the argument. FAQ Candidates become the FAQ section verbatim questions with direct answer openers.

**→ optimization-engine** (Optimization Pipeline)  
Approved signals become a coverage checklist: is each approved term present in the existing article? Missing terms = coverage gaps to fix. Present but shallow terms = depth gaps to fix.

---

## Critical Rules

1. **Never pass rejected signals downstream.** The rejected log is for audit transparency only. optimization-engine, content-generation-engine, and serp-blueprint-generator receive approved signals only.
2. **PAA questions are intent signals, not H2 headings.** A PAA question tells you what the user wants answered — it becomes a FAQ entry or informs an H2 angle, not a direct copy-paste heading.
3. **Empty approved set is valid.** If all signals are filtered out, mark SIGNALS-FILTERED-EMPTY and continue the pipeline without signals. This is a correct outcome for niche or highly specific topics. Never force approvals to fill the set.
4. **Autocomplete is the most reliable signal.** Google's autocomplete reflects actual user typing patterns. Weight these higher than Related Searches when integration points are limited.
5. **Related Searches reflect topic neighbourhood, not article structure.** Use them to identify what the article should acknowledge exists (via internal links or brief mentions) rather than building sections around them.
6. **The filter reasoning is non-negotiable.** Never approve a signal without a documented reason. Never reject a signal without a documented reason. The log is the audit trail.
7. **Nexus cache rule:** If `serp-intelligence` already fetched the primary keyword SERP in this session (CACHE-HIT), reuse that fetched content for PAA and Related Searches extraction instead of re-fetching. Mark as CACHE-HIT in the pipeline log.

---

## Failsafe

If Google blocks `web_fetch` entirely:
1. Use `web_search` with the primary keyword and expand with variations (`how`, `vs`, `best`, `without`) to capture autocomplete patterns
2. Note any question-format suggestions as PAA-FALLBACK
3. Run `web_search` for `"[primary keyword]" related searches` and extract from results
4. Mark all signals as FALLBACK-MODE in the output
5. Continue — do not halt the pipeline

The approved signal set will be smaller in fallback mode. That is acceptable. Never fabricate signals.
