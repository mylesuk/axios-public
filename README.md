# axios-public

The public slice of **Axios** — the author's long-horizon thinking and content system. This repository is the authoritative reference for vocabulary, pipeline, conventions, and the Grok-side trigger contracts that hand raw material off to the private vault.

The **private vault** (`Axios/`) contains journals, therapy, inquiry, raw corpora, and operational records. None of that lives here. What lives here is the small set of documents Grok needs so it stays aligned with the system across sessions.

## Purpose

1. Give Grok a stable, linkable source of truth for how Axios thinks and how its pipeline works.
2. Document the `g-*` trigger contracts Grok uses to hand material off to the private vault via Cursor commands.
3. Make the persona, voice, and content-tier frame legible to any model or collaborator the author chooses to share them with.

## Read in this order

1. [`glossary.md`](glossary.md) — the terms (Atelier, Quarry, Journal, Steward of Value, etc.).
2. [`README.md`](README.md) — you are here. Pipeline below.
3. [`persona-voice.md`](persona-voice.md) — public persona and voice.
4. [`vocabulary.md`](vocabulary.md) — preferred vs retired terms, with reasons. The antibody against register drift.
5. [`content-tiers.md`](content-tiers.md) — how output is tiered; Truth-over-time.
6. [`conventions.md`](conventions.md) — slugs, metadata, filename rules.
7. [`quarry-guidelines.md`](quarry-guidelines.md) — how Grok acts as pre-filter and router.
8. [`triggers/`](triggers/) — the `g-*` contracts. Start with [`g-route.md`](triggers/g-route.md) (default dispatcher); the direct-lane triggers (`g-quarry`, `g-capture`, `g-propose`, `g-draft`, `g-ship`) remain fully supported for when the author already knows the lane.

## Pipeline at a glance

Raw material flows one direction. Nothing skips stages.

```
Raw input                  (X thread, YouTube transcript, article, chat, voice memo, term)
    |
    v
g-route (default)          Grok classifies, prints a router note, delegates to:
    |
    +---> g-quarry   ->  Quarry/                      (raw dump, mine later)
    +---> g-capture  ->  Atelier/10-Sources/          (short external source, or word mode)
    +---> g-propose  ->  Atelier/00-System/proposals/ (author's own idea about Axios)
    +---> g-draft    ->  Atelier/60-Catalog/Articles/ (longform article handoff)
    +---> g-ship     ->  Atelier/60-Catalog/          (packaging: thread + carousel)
```

`g-route` is the default entry point — use it when the lane is not obvious. The direct `g-*` triggers remain available for when the author already knows the lane; they skip the router note and go straight to the handoff. Each direct trigger emits a single fenced markdown block whose first line is the matching Cursor slash command (`/quarry`, `/capture`, `/propose`, `/draft`, `/ship`). Click Grok's code-block copy button, paste into Cursor, press enter — the command auto-fires.

`g-propose` is distinct: it is for the **author's own forward thinking about how Axios should evolve** (system, product, process, documentation). It is not for capturing external material (that is `g-capture`), not for content germs (those are drafts), and not for inner-work or personal life ideas (those are Journal).

From `10-Sources/`, the vault pipeline continues:

```
10-Sources/  ->  /distill  ->  20-Insights/  +  40-Themes/
                                                    |
                                                    v
                                 50-Workshop/  ->  /ship  or  /draft  ->  60-Catalog/
```

`/distill` runs the full chain in one conversation: source → atomic insight(s) in `20-Insights/` → theme match in `40-Themes/` → ship / support-existing-draft / bank recommendation. The default recommendation is **bank** — compounding inside a theme beats shipping a single-insight post.

**Journal/** runs in parallel (inner reflection, therapy, inquiry). It is **not** part of the Atelier pipeline. Grok does not surface Journal material and does not blend inner-work voice with public content.

## Where Grok fits

Grok's leverage is on the **intake side**:

- Native access to X (threads, quote chains, profiles, replies).
- Long context — an 80-minute transcript or a long chat summarized to what justifies a capture.
- Tolerance for messy pastes — output is always a single structured block.
- Multi-pass summarization and routing.

Grok's job, in one sentence: **read the raw material and produce the single Cursor-ready handoff block for the right lane**, preferring `g-capture` over `g-quarry` when the material is short and insight-dense, and `g-quarry` when the material is long, unfiltered, or likely to reward later mining.

Full routing rules: [`quarry-guidelines.md`](quarry-guidelines.md).

## Source of truth

The vault (`Axios/`) is canonical. This repository is a **derived artifact** synced from the vault by a script. Do not hand-edit files here; edit in the vault and re-run the sync.

## License and scope

This repository contains system documentation only. It is published so the author's tools (Grok, other models, collaborators) have a stable reference. It is not a product, is not open to contribution, and carries no warranty.
