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

Every Grok trigger (`g-quarry`, `g-capture`, `g-draft`, `g-ship`) emits a single fenced markdown code block whose **first line is the matching Cursor slash command** (`/quarry`, `/capture`, `/draft`, or `/ship`) followed by a blank line and then the handoff body. The author clicks the code-block copy button on grok.com (which strips the outer backticks), pastes into Cursor, and presses enter — the command auto-invokes with the rest of the block as input. No second step.

The `g-` prefix on Grok triggers mirrors the `/` prefix on Cursor commands. Same verbs, different platforms.

## File naming (what Grok output implies)

Grok does not write files. Cursor commands write files based on handoff metadata. Grok's job is to produce metadata that results in the right path.

| Target | Path pattern | Metadata that drives it |
|--------|--------------|-------------------------|
| Quarry dump | `Quarry/YYYY-MM-DD-{topic}-grok.md` | `g-quarry` frontmatter with `date`, `topic` |
| Atelier source | `Atelier/10-Sources/source-{slug}.md` | `g-capture` metadata `content-id` |
| Article | `Atelier/60-Catalog/Articles/YYYY-MM-DD-{content-id}.md` | `g-draft` metadata `content-id`, `ship-date` |
| Thread (derivative) | `Atelier/60-Catalog/Threads/YYYY-MM-DD-{content-id}.md` | `g-ship` metadata `content-id`, `ship-date` |
| Carousel (derivative) | `Atelier/60-Catalog/Carousels/YYYY-MM-DD-{content-id}.md` | `g-ship` metadata `content-id`, `ship-date` |

## Metadata keys (canonical shape)

Used across `g-capture`, `g-draft`, and `g-ship`:

- `content-id:` — kebab-case slug, stable, aligned with the core thesis.
- `main-topic:` — short human-readable topic, 2–6 words.
- `tags:` — YAML inline list, 3–7 kebab-case tokens.
- `ship-date:` — `YYYY-MM-DD`. Required for `g-ship`, optional for `g-draft`, not used in `g-capture` (sources are timeless).
- `title:` *(optional)* — final title if distinct from `content-id`.
- `deck:` *(optional)* — one-sentence subtitle.
- `series:` *(optional)* — e.g. `truth-over-time`.
- `source:` *(optional on g-draft)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).

When a block includes a `Truth over Time` section, its keys (`historical-anchor`, `modern-anchor`, `historical-relevance`, `present-relevance`, `fracture-line`) are filled from the conversation; never invented.

## H1 format

For any note Grok generates body text for:

```
# YYYY-MM-DD — {Title as written}
```

Spaced em dash between date and title. Title as the author would write it, capitalization preserved; not slugified.

## Voice Intent card (g-draft)

Required on `g-draft` handoffs:

```
**Voice Intent**
- **Thesis:** <the central claim the piece stands on; what it actually argues>
- **Non-negotiables:** <what must not be diluted, hedged, or appeased away>
- **Landing:** <how you want the argument to settle with the reader>
```

Optional line: `- **My read:** <one-sentence author self-read>`.

**Never fabricate Voice Intent.** If the author has not supplied it in-chat, emit `SKIP: <reason>` instead of a Voice Intent card.

The earlier field names (`Edge` / `No-soften zone`) are retired — see [`vocabulary.md`](vocabulary.md) for the reasoning. Treat any pasted Voice Intent using the old field names as equivalent: `Edge` → `Thesis`, `No-soften zone` → `Non-negotiables`.

## Sourcing

For article bodies (`g-draft`) and longform pieces:

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
