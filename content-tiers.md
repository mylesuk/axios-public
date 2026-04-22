# Content tiers

Axios outputs three tiers of public work, plus one framing layer (Truth-over-time) that cross-cuts them. Grok should know which tier a handoff targets and shape output accordingly.

## Tier 1 — Threads and carousels

Short public pieces. High frequency relative to the other tiers. Not throwaways; distilled from mature insights in `Atelier/20-Insights/` or finished articles.

- **Format:** X thread (`60-Catalog/Threads/`) or Instagram carousel (`60-Catalog/Carousels/`).
- **Scope:** one clear claim, one anchor, one payoff.
- **Source:** a mature insight, a finished article (derivative via `g-ship`), or a clean self-contained observation.
- **Voice:** Considered Witness, tightened for the medium. No hustle cadences. No engagement bait hooks.
- **Constraint:** if the piece cannot survive without visual punch or a cliffhanger, it is not ready.

## Tier 2 — Articles

Longform essays. Lower frequency. The tier most of the author's written work lives in.

- **Format:** longform markdown (`60-Catalog/Articles/`), produced through `/draft` (or `g-draft` handoff).
- **Scope:** a full argumentative spine. Evidence. Classical resonance where it clarifies. An honest fracture paragraph where the evidence runs thin.
- **Clarity, not grade level:** prose should move without friction — no run-on sentences, no clutter, no nested clauses obscuring the argument. Voice and precision take priority over any readability score. No Flesch-Kincaid or equivalent grade target is set; the Considered Witness register is not built to land at a specific grade and should not try. Respect the reader's intelligence; do not over-simplify, do not pad.
- **Sourcing:** verifiable pointers — URLs, authors, dates, canonical editions. Distinguish citation from interpretation.
- **Ending:** lands. No hedge, no shrug, no "thoughts?".

## Tier 3 — Major works

Books, long video essays, exhibitions. Rare by design.

- **Format:** `60-Catalog/Major-Works/` (markdown index + external production).
- **Scope:** a multi-month or multi-year project. Usually the convergence of a mature theme with years of accumulated insight.
- **Constraint:** a major work is earned by the accumulated body, not scheduled. Grok should not propose major works out of a single session.

## Truth-over-time (cross-cutting framing)

A framing applied to certain Tier 2 and Tier 1 pieces that makes their durability explicit. The piece names:

- `historical-anchor` — the classical source, figure, or event being placed.
- `modern-anchor` — the present-day expression or fracture.
- `historical-relevance` — why the anchor mattered in its time.
- `present-relevance` — why it matters now.
- `fracture-line` — the honest tension or limitation the piece names.

Not every piece uses this framing. Only those where the historical anchor is doing real load-bearing work — not decoration. When a `g-draft` or `g-ship` handoff sets `series: truth-over-time`, the Truth-over-time block is required in the handoff.

## Routing: which tier is this material aiming for?

| Signal | Likely tier |
|--------|-------------|
| One clean claim, one anchor, one payoff | Tier 1 |
| Full argumentative spine with evidence and fracture line | Tier 2 |
| Accumulated body of a mature theme over months | Tier 3 |
| A single lived quote or passage worth preserving | Not a tier yet — it is a **source** for `Atelier/10-Sources/` (use `g-capture`) |
| A long raw corpus to mine later | Not a tier — it is **Quarry** material (use `g-quarry`) |

Grok should name the likely tier in a pre-filter pass when the material plausibly targets one; when it does not, route it upstream to Sources or Quarry and let the author decide whether a tier emerges later.
