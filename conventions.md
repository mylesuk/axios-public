# Conventions

The shared rules Grok should follow when producing metadata and handoff blocks. These are the subset of the vault's conventions that matter for Grok output; implementation details (Obsidian templates, Templater config, folder-template triggers) are vault-internal and deliberately omitted.

## Slugs

- Lowercase ASCII, digits, and hyphens only: `[a-z0-9-]+`.
- No spaces, underscores, em dashes, Unicode, or quotes.
- Kebab-case: words separated by single hyphens; no double hyphens, no leading or trailing hyphens.
- Keep under ~72 characters where feasible; favor the core thesis phrase.

**Valid:** `rising-tide-material-spiritual`, `ordo-amoris`, `cass-review-and-puberty-blockers`.

**Invalid:** `Rising Tide`, `rising_tide`, `rising--tide`, `rising-tide-`.

## Handoff block shape

Grok has one trigger: `xbridge`. It emits a single fenced markdown code block whose **first line is `/capture`**, followed by a blank line and the handoff body. The author clicks the code-block copy button on grok.com (which strips the outer backticks), pastes into Cursor, and presses enter — Cursor's `/capture` auto-invokes and runs a tolerance pass on intake. No second step.

## File naming (what Grok output implies)

Grok does not write files. Cursor commands write files based on handoff metadata. Grok's `xbridge` job is to produce metadata that results in the right path at `Atelier/10-Sources/`; everything downstream (drafting, packaging) happens in Cursor directly.

| Target | Path pattern | Where the metadata comes from |
|--------|--------------|-------------------------------|
| Atelier source (document mode) | `Atelier/10-Sources/source-{slug}.md` | `xbridge` handoff Metadata `content-id` |
| Canon Word encounter (word mode) | `Atelier/30-Canon/Words/canon-word-{slug}.md` | `xbridge` handoff Metadata `content-id` prefixed `word-` |
| Quarry dump | `Quarry/YYYY-MM-DD-{topic}-{source}.md` | Author's direct paste into Cursor `/quarry`; no Grok |
| Article | `Atelier/60-Catalog/Articles/YYYY-MM-DD-{content-id}.md` | Author-drafted in Cursor via `/draft` |
| Thread (derivative) | `Atelier/60-Catalog/Threads/YYYY-MM-DD-{content-id}.md` | Author-packaged in Cursor via `/ship` |
| Carousel (derivative) | `Atelier/60-Catalog/Carousels/YYYY-MM-DD-{content-id}.md` | Author-packaged in Cursor via `/ship` |

## Metadata keys (canonical shape)

Used in the `xbridge` handoff Metadata block:

- `content-id:` — kebab-case slug, stable, aligned with the core thesis (prefixed `word-` for term captures).
- `main-topic:` — short human-readable topic, 2–6 words.
- `tags:` — YAML inline list, 3–7 kebab-case tokens. Always include `source` (and `word` in word mode).

Additional keys used in Cursor-side commands (`/draft`, `/ship`) on the author's side, not Grok's:

- `ship-date:` — `YYYY-MM-DD`. Required for `/ship`, optional for `/draft`.
- `title:` *(optional)* — final title if distinct from `content-id`.
- `deck:` *(optional)* — one-sentence subtitle.
- `series:` *(optional)* — e.g. `truth-over-time`.
- `source:` *(optional on `/draft`)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).

When a block includes a `Truth over Time` section, its keys (`historical-anchor`, `modern-anchor`, `historical-relevance`, `present-relevance`, `fracture-line`) are filled from the conversation; never invented.

## H1 format

For any note Grok generates body text for:

```
# YYYY-MM-DD — {Title as written}
```

Spaced em dash between date and title. Title as the author would write it, capitalization preserved; not slugified.

## Voice Intent card (used by `/draft` in Cursor)

Required on any longform article body saved via `/draft`:

```
**Voice Intent**
- **Thesis:** <the central claim the piece stands on; what it actually argues>
- **Non-negotiables:** <what must not be diluted, hedged, or appeased away>
- **Landing:** <how you want the argument to settle with the reader>
```

Optional line: `- **My read:** <one-sentence author self-read>`.

**Never fabricate Voice Intent.** If the author has not supplied it, `/draft` records `SKIP: <reason>` rather than invention. Voice Intent is not part of the `xbridge` handoff — drafting and voice decisions happen in Cursor.

The earlier field names (`Edge` / `No-soften zone`) are retired — see [`vocabulary.md`](vocabulary.md) for the reasoning. Treat any pasted Voice Intent using the old field names as equivalent: `Edge` → `Thesis`, `No-soften zone` → `Non-negotiables`.

## Sourcing

For article bodies and longform pieces (drafted in Cursor via `/draft`):

- Cite URLs where available. Prefer canonical and permalink forms.
- Date-stamp quotations where the original carries a date.
- Distinguish **citation** (what the source said) from **interpretation** (what the author is doing with it).
- When evidence runs thin, say so plainly. The Steward names fracture lines; he does not paper over them.

## What Grok never does

- Fabricate quotations, dates, or attributions.
- Claim a file was saved, moved, or renamed (Grok produces handoff blocks; Cursor writes files).
- Use Journal material in any public handoff.
- Use vault-internal paths (`Support/Templates/`, `.obsidian/`, `.cursor/`) in output — those are implementation details Grok does not need.
- Default to Tier 1 packaging without reading the material first. The tier emerges from the content.
