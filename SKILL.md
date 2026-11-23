---
name: market-intelligence-brief
description: |
  Produces a CXO-ready weekly consumer market- and competitor-intelligence brief for Northwind
  Corporation Ltd leadership as Northwind enters the nutrition supplements and protein bar category,
  grounded in cited sources.
  Selects five geography-relevant tracked competitor brands based on sourced evidence across configurable
  thematic pillars, with per-trend driver classification, confidence calibration, CXO read-outs,
  business-function implications, and scheduled recommendations — and appends a Week-over-Week
  comparison when a prior brief is available. Use when the user asks to "generate this week's
  market intelligence brief", "create a competitor intelligence report", "write the weekly market
  brief", "produce the protein bar / supplements market update", or supplies a date range / news
  corpus to turn into a brief.
license: MIT
metadata:
  author: Cowork Scenario Playbook
  version: "1.0"
---

# Market Intelligence Brief — Generator (FMCG consumer health, configurable)

This skill authors a weekly competitor- and market-intelligence brief for Northwind Corporation Ltd's
executive readers. Northwind is an FMCG company entering the nutrition supplements and protein bar
category — starting with the United States and then expanding to other regions. The brief is written by
Northwind's market intelligence function and addressed to Northwind leadership.
Organisation-specific settings — thematic pillars, business functions, personas, brand
styling, section gates, and filename pattern — are supplied in the **Configuration** block below.
The configuration below is pre-filled for a fictional demonstration organisation; the authoring workflow then runs unchanged.

> Evaluation of a finished brief is a **separate** skill (`market-intelligence-brief-eval`). This
> skill's job is to **create** the brief to the same specification the evaluator scores against, so
> the two stay in lockstep.

> **Demo note:** This Skill is pre-configured for Northwind Corporation Ltd, a fictional FMCG company
> planning to launch protein bars and nutrition supplements. Do not ask participants to edit the configuration.
> The geography is selected interactively at run time. Participants do not edit the Skill.
> The five-brand watchlist is selected for the requested geography using sourced evidence, not a fixed global ranking.

---

## Configuration (pre-filled for Cowork Camp demonstration)

```yaml
organisation_name: Northwind Corporation Ltd
prepared_by: Northwind Market Intelligence
audience: Northwind Corporation Ltd leadership
industry: FMCG / Consumer Packaged Goods
target_category: Nutrition supplements and protein bars
market_entry_sequence: United States first, then other regions
brief_cadence: Weekly
spec_version: v1.0
thematic_pillars:
- Consumer Demand, Pricing & Channels
- Product Innovation, Formulation & Clean Label
- Regulation, Health Claims & Consumer Trust
tracked_peers: []  # populated at runtime: select five relevant, source-supported protein bar / supplement brands or brand owners for the requested geography
business_functions:
- Brand & Marketing
- R&D / Product Development
- Supply Chain & Manufacturing (incl. co-manufacturing)
- Sales & Channels (Retail, Club, Convenience, E-commerce / DTC)
- Regulatory, Quality & Food Safety
cxo_personas:
- CEO
- CFO
- CMO
- COO / Chief Supply Chain Officer
- Chief R&D Officer
confidence_enum:
- tier: High
  badge: 🟢
- tier: Medium
  badge: 🟡
- tier: Low
  badge: 🔴
source_type_labels:
- '[Press Release]'
- '[News Article]'
- '[Market Research Report]'
- '[Earnings Call]'
- '[Regulatory Document]'
- '[Retailer Announcement]'
brand_spec:
  body_font: Arial 14px
  h1_colour: '#005EB8'
  h2_colour: '#003D7A'
  hyperlink_style: brand colour, underlined
  confidence_badges: coloured circles (🟢/🟡/🔴) or named labels (High/Medium/Low)
sections:
- id: S1
  name: News Summary
  floor: 300
  cap: 400
- id: S2
  name: Trends
  floor: 400
  cap: null
- id: S3
  name: Competitor
  floor: 300
  cap: null
- id: S4
  name: Implications
  floor: 250
  cap: null
- id: S5
  name: Recommendations
  floor: 200
  cap: null
optional_comparison_section:
  id: S6
  name: Week-over-Week Comparison
  required_when: a genuine prior brief for the same organisation and geography is explicitly supplied or unambiguously identified
recommendation_schema:
- timeframe
- owner
- action
- rationale
timeframe_enum:
- Immediate
- Short
- Long
filename_pattern: MarketIntel_YYYY-MM-DD.html
source_hints:
- competitor brand newsrooms & press-release pages
- investor-relations / earnings pages of brand owners
- reputable food, beverage, nutrition and consumer-goods trade press
- market research and category data publishers
- retailer and e-commerce marketplace announcements
- relevant food, supplement and advertising regulatory publications
```

---

## Interactive geography and competitor selection

- **Ask first:** Before researching, if the user has not specified a geography in the current request, ask exactly one concise question: "Which geography should this week's Market Intelligence Brief cover (for example, United States, Canada, or UK)?" Wait for the answer. Do not assume the United States from a previous lab or from existing files.
- **Runtime geography:** Use the user's response as the market scope for this run. Focus on that geography's protein bar and nutrition supplement category: consumer demand and need-states (e.g. high-protein, low-sugar, sports nutrition, weight management, healthy snacking), pricing and promotions, retail and e-commerce channels, new product launches, M&A and partnerships, and relevant regulatory developments (labelling, health and nutrient-content claims, supplement rules, advertising standards). Broader regional or global developments are in scope only when a supported implication for the chosen geography is clear.
- **Runtime tracked peers:** After the geography is known, identify up to five real brands or brand-owning companies with credible evidence of selling protein bars and/or nutrition supplements in that geography. Prefer established and fast-growing branded competitors; label retailer private labels, ingredient suppliers and contract manufacturers distinctly if they are mentioned. Treat the resulting names as the `tracked_peers` allow-list for this run. If fewer than five are verifiable, disclose the gap and do not invent names. Cite the evidence for selection in the report.
- **Evidence:** Use credible web publications, company newsrooms, trade press, market research and regulatory sources. The Skill defines methodology; it is not itself evidence of market developments.
- **Exclude preparation material:** Do not use Cowork Camp PPTs, lab handouts, demo instructions, or Skill documentation as market evidence or as a prior intelligence report. Do not search those materials to fill gaps.
- **Prior report:** Only compare against a genuine previous Market Intelligence Brief for the same organisation and geography, explicitly supplied by the user or unambiguously identified. If none exists, omit week-over-week comparison.
- **Coverage:** If a tracked brand has no verified relevant news during the window, disclose that evidence is unavailable rather than inventing an announcement.

## Inputs

Before writing, establish:

0. **Geography** — ask the user for it if missing, then wait for their answer before selecting competitors or researching.
1. **Reporting window** — the date range the brief covers (e.g. "19–25 Mar 2026").
2. **Source material** — an indexed news corpus, supplied links, and/or a connected web/search or
   SharePoint tool. If live tooling is available, name it in the workflow (e.g. "use the
   `search_news` tool"); otherwise work only from material the user provides.
3. **Prior brief** — use only a genuine previous Market Intelligence Brief for the same organization and geography,
   explicitly supplied or unambiguously identified. Never treat training material as a prior brief.

If the reporting window or source material is missing, ask for it. Never fabricate sources.

## Grounding contract (mandatory)

- **Every factual claim carries a citation**: an inline hyperlink to a named source **plus** one
  `source_type_labels` tag (e.g. `[Press Release]`). No hyperlink → the claim is not usable.
- **No invented sources, URLs or market figures.** If you cannot cite a claim (including market size,
  growth rates, prices or shares), drop it or mark it explicitly as an internal inference (not attributed to any outlet).
- **Tracked peers only.** Populate `tracked_peers` at runtime using the selected geography and source-supported peer selection. Only brands in that run-specific list may be framed as tracked competitors.
  Other companies may appear in general news context but never as a tracked peer.
- **Confidence only on the enum.** Use exactly the tiers in `confidence_enum` — never "Medium-High"
  or similar.
- **No health or medical advice.** Report on market, product and regulatory developments; do not make or
  endorse health, efficacy or medical claims about products or ingredients.

## Workflow

1. **Confirm geography first.** Ask the user to choose a geography if it is not specified; wait for their answer. Then identify up to five source-supported tracked competitor brands for that geography. Confirm remaining configuration is filled; do not invent values.
2. **Collect** items published in the reporting window across the `source_hints`, focused on the selected geography, run-specific `tracked_peers`, and the three `thematic_pillars`. Capture the URL and source type for each item as you go.
3. **Classify** each item to the most relevant pillar; discard off-topic noise. Keep peer coverage inside the allow-list.
4. **Select trends.** Distil the collected items into a small set of material trends (aim for ~5). For each trend, determine a **driver classification** (what is causing it — e.g. consumer behaviour, pricing/inflation, retail/channel shift, ingredient/technology, regulation, competitor action), a **confidence tier** from the enum, and a **CXO read-out** (why a named Northwind executive should care for Northwind's market entry).
5. **Read the genuine prior brief for the same geography** (if explicitly supplied or unambiguously identified) to enable the Week-over-Week comparison; otherwise omit it.
6. **Draft the sections** in order, honouring each section's word-count gate and the grounding contract (see Section specifications).
7. **Write recommendations** — each carries all `recommendation_schema` fields, with `timeframe` on the enum and a named `owner`.
8. **Render** the brief as a brand-compliant HTML report per `brand_spec`, name it per `filename_pattern`.
9. **Deliver** (when connectors/tooling are available): save the report to the document store (e.g. SharePoint) and email it to the distribution list. Name the tools explicitly if present; otherwise return the HTML for the user to place.

## Section specifications

Address all three `thematic_pillars` across the brief, and tag trends to their pillar.

### S1 — News Summary (300–400 words)
A tight executive digest of the window's most important developments across tracked competitor brands
and the protein bar / nutrition supplement category. Every claim cited. No analysis yet — this is "what happened."

### S2 — Trends (≥ 400 words)
The analytical core. For **each** trend present:
- a one-line trend statement tagged to a pillar,
- **driver classification** (the underlying cause),
- **confidence** rendered as a badge from `confidence_enum`,
- a **CXO read-out** naming the persona who should act and why.
Write in narrative prose, not bare bullets or tables.

### S3 — Competitor (≥ 300 words)
Per-brand movements for the `tracked_peers` only (aim to cover all of them): launches, reformulations,
pricing and promotions, channel or retailer wins, marketing campaigns, M&A or partnerships. What each did
in the window and what it signals. No out-of-scope companies framed as tracked peers.

### S4 — Implications (≥ 250 words)
Translate trends into consequences for Northwind Corporation Ltd's market entry. Address **≥ 3 `cxo_personas` by name**,
and map implications to **≥ 3 `business_functions`** with specific "so-what" for each.

### S5 — Recommendations (≥ 200 words)
Concrete, prioritised actions. **Every** recommendation carries the full `recommendation_schema`:
`timeframe` (from `timeframe_enum`), a named `owner` role, a concrete `action`, and a `rationale`
tied back to a specific trend or implication. Owners are named Northwind roles (e.g. CMO, Head of
E-commerce, Market Intelligence Lead).

### S6 — Week-over-Week Comparison (only when a prior brief exists)
What changed since last week: trends that escalated/resolved, confidence shifts, new or dropped
competitor stories. If this is the first run (no prior brief), omit S6
acknowledging the baseline.

## Output format (brand-compliant HTML report)

Render an HTML document styled per `brand_spec`:

- Body in the configured font; `H1` in `h1_colour`, `H2` in `h2_colour`.
- Section headings: `S1 News Summary`, `S2 Trends`, `S3 Competitor`, `S4 Implications`,
  `S5 Recommendations`, and `S6 Week-over-Week Comparison` when applicable.
- Citations as underlined brand-colour hyperlinks, each followed by its source-type label.
- Confidence rendered as the configured badges (🟢/🟡/🔴 or High/Medium/Low).
- File named per `filename_pattern`.

```html
<!doctype html>
<html>
  <head><meta charset="utf-8"><title>Northwind Corporation Ltd Market Intelligence — [geography] — [window]</title></head>
  <body style="font-family: Arial; font-size:14px;">
    <h1 style="color:#005EB8">Northwind Corporation Ltd Weekly Market Intelligence: Protein Bars &amp; Nutrition Supplements — [geography] — [window]</h1>
    <p>Prepared by Northwind Market Intelligence for Northwind Corporation Ltd leadership.</p>
    <h2 style="color:#003D7A">1. News Summary</h2>
    <!-- ...cited prose... -->
    <h2 style="color:#003D7A">2. Trends</h2>
    <!-- per-trend: statement · driver · confidence badge · CXO read-out -->
    <h2 style="color:#003D7A">3. Competitor</h2>
    <h2 style="color:#003D7A">4. Implications</h2>
    <h2 style="color:#003D7A">5. Recommendations</h2>
    <!-- optional --> <h2 style="color:#003D7A">6. Week-over-Week Comparison</h2>
  </body>
</html>
```

## Guardrails (mandatory)

- **Confirm configuration first.** Never write against unfilled `{{PLACEHOLDER}}` values — ask.
- **Cite everything.** Every external claim gets a hyperlink + a source-type label. No invented URLs or market figures.
- **Stay inside the allow-list** for tracked peers; **stay on the enum** for confidence.
- **Respect the word-count gates** for each section; write CXO-register prose, not bullet/table dumps.
- **Full recommendation schema** on every recommendation, with a named owner and an enum timeframe.
- **No health or medical claims.** Describe market and regulatory developments only.
- **Do not self-score.** Quality scoring is the separate `market-intelligence-brief-eval` skill.

## When to ask for human review

- The reporting window has thin or conflicting source coverage for one or more pillars.
- A potentially material story relies on a single unverified source.
- A new competitor brand appears that may need to be added to (or rejected from) the tracked-peer allow-list.
- A regulatory or product-safety development (e.g. recall, warning letter, claims challenge) could affect launch plans.
- The brief would need a structural change to the template (new/removed/reordered section).

## Version

- Spec version: **v1.0** (`spec_version`) — keep aligned with the eval rubric version so the
  generator and evaluator never drift.
