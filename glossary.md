# Glossary

Short definitions for the terms Axios uses idiosyncratically. Grok should treat these as authoritative when they conflict with general usage.

## Axios

The whole system — a single long-horizon thinking and content project. Implemented as an Obsidian vault with four top-level folders: `Quarry/`, `Journal/`, `Atelier/`, `Support/`. Only the last two are partially visible here; `Quarry/` and `Journal/` are private by design.

## Atelier

The philosophical thinking studio and content engine inside Axios. Organized by pipeline stage, not by topic:

- `10-Sources/` — raw captures worth distilling.
- `20-Insights/` — atomic, evaluated claims. The **primary unit of value** in the system.
- `30-Canon/` — classical references (thinkers, works, artworks, words).
- `40-Themes/` — Maps of Content clustering insights by topic.
- `50-Workshop/` — works in progress.
- `60-Catalog/` — shipped content by tier (threads, carousels, articles, major works).
- `70-Reviews/` — retrospectives and audience signal.

Content is **downstream emergence** from accumulated insight. The Atelier does not start with a content calendar and work backward.

## Quarry

Raw external corpora: Grok/Claude/ChatGPT chat exports, YouTube transcripts, `.srt`/`.vtt` subtitles, Readwise highlights, long voice-memo transcripts. Flat folder, tag-driven. Mined later via `/capture` into `Atelier/10-Sources/`. **Gitignored in the vault; never published.**

## Journal

The single flat reflection folder: journaling, therapy, inquiry, shadow work, dream work, ideas, protocols. Tag-driven, one file per entry. **Private. Grok does not read, surface, or simulate Journal content.**

## Source

A single external artifact worth distilling — one X thread, one article, one book passage, one conversation, one talk. Lives in `Atelier/10-Sources/` as `source-{slug}.md`. Distinct from the broader **Quarry** (raw dumps). A source is already a deliberate pick.

## Insight

An **atomic, evaluated claim** the author holds after engagement with a source. Lives in `Atelier/20-Insights/` as `insight-{slug}.md`. Carries metadata for sensitivity, share-clearance, classical anchors, value categories. The unit of value the rest of the system composes from.

## Theme

A Map of Content (MOC) that clusters related insights and classical anchors around a topic. Lives in `Atelier/40-Themes/` as `theme-{slug}.md`. Themes mature from emerging to deep.

## Catalog

The shipped work. Lives in `Atelier/60-Catalog/`, organized by tier and format:

- `Threads/` — Tier 1 X threads.
- `Carousels/` — Tier 1 Instagram carousels.
- `Articles/` — Tier 2 longform essays.
- `Major-Works/` — Tier 3 books, video essays, exhibitions.

## Steward of Value

The public **persona**. Integrated value across spiritual, intellectual, aesthetic, relational, vital, and material categories; tends concentric circles in priority order (self → family → friends → community → beyond); authority earned by the quality of life and thought, not claimed. See [`persona-voice.md`](persona-voice.md).

## Considered Witness

The public **voice**. Calm, classically literate, candidly skeptical. No outrage. No hustle. No guru. No contrarian cosplay. No performative vulnerability. See [`persona-voice.md`](persona-voice.md).

## Truth-over-time

A framing applied to certain pieces: places a **historical anchor** (classical source, figure, or event) alongside a **modern anchor** (present-day expression or fracture) and names the **fracture line** between them. Signals a piece as a durable, cross-temporal claim rather than a news cycle reaction. See [`content-tiers.md`](content-tiers.md).

## Inner Work Bridge

A categorical distinction, not a mechanism Grok needs to model: inner-work material (Journal) enters the Atelier only through a deliberate gating step with sensitivity and share-clearance fields. **Grok's rule:** do not publish or surface any Journal-derived material in Atelier handoffs unless the author explicitly treats a piece as approved for public work.

## Value categories

Six canonical value categories the Steward tracks and integrates:

- **Spiritual** / holy
- **Intellectual**
- **Aesthetic**
- **Philial** / relational
- **Vital** / physical
- **Material** / stewardship

**Virtue** is cross-cutting — the quality of how each category is inhabited, not a seventh category.

## Concentric circles

Priority order of care: **self → family → friends → community → country → humanity**. The Steward tends the inner circles well before making grand universalist claims. Pieces can be tagged with which circle they primarily serve.

## Claim-risk, Distribution

Two scoring axes applied at packaging time:

- **Claim-risk** (0–10): how provocative / likely to provoke pushback the piece's actual claims are.
- **Distribution** (0–10): how much share pull the piece has across the target audience.

Neither is a ceiling or a target. They are honest reads used to tune packaging, not to optimize for virality or outrage.
