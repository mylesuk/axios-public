# axios-public

The public slice of **Axios** — the author's long-horizon thinking and content system. This repository is the authoritative reference for vocabulary, pipeline, conventions, and the Grok-side trigger contracts that hand raw material off to the private vault.

The **private vault** (`Axios/`) contains journals, therapy, inquiry, raw corpora, and operational records. None of that lives here. What lives here is the small set of documents Grok needs so it stays aligned with the system across sessions.

## Purpose

1. Give Grok a stable, linkable source of truth for how Axios thinks and how its pipeline works.
2. Document the `q-*` trigger contracts Grok uses to hand material off to the private vault via Cursor commands.
3. Make the persona, voice, and content-tier frame legible to any model or collaborator the author chooses to share them with.

## Read in this order

1. [`glossary.md`](glossary.md) — the terms (Atelier, Quarry, Journal, Steward of Value, etc.).
2. [`README.md`](README.md) — you are here. Pipeline below.
3. [`persona-voice.md`](persona-voice.md) — public persona and voice.
4. [`content-tiers.md`](content-tiers.md) — how output is tiered; Truth-over-time.
5. [`conventions.md`](conventions.md) — slugs, metadata, filename rules.
6. [`quarry-guidelines.md`](quarry-guidelines.md) — how Grok acts as pre-filter and router.
7. [`triggers/`](triggers/) — the `q-*` contracts.

## Pipeline at a glance

Raw material flows one direction. Nothing skips stages.

```
Raw input                  (X thread, YouTube transcript, article, chat, voice memo)
    |
    v
Grok pre-filter/router     (TL;DR, candidate insights, routing recommendation)
    |
    +---> q-export   ->  Quarry/                      (raw dump, mine later)
    +---> q-capture  ->  Atelier/10-Sources/          (short and good; skip Quarry)
    +---> q-draft    ->  Atelier/60-Catalog/Articles/ (longform article handoff)
    +---> q-ship     ->  Atelier/60-Catalog/          (packaging: thread + carousel)
```

From `10-Sources/`, the vault pipeline continues:

```
10-Sources/  ->  /distill   ->  20-Insights/   ->  /cluster  ->  40-Themes/
                                                                     |
                                                                     v
                                             50-Workshop/  ->  /publish  ->  60-Catalog/
```

**Journal/** runs in parallel (inner reflection, therapy, inquiry). It is **not** part of the Atelier pipeline. Grok does not surface Journal material and does not blend inner-work voice with public content.

## Where Grok fits

Grok's leverage is on the **intake side**:

- Native access to X (threads, quote chains, profiles, replies).
- Long context — an 80-minute transcript or a long chat summarized to what justifies a capture.
- Tolerance for messy pastes — output is always a single structured block.
- Multi-pass summarization and routing.

Grok's job, in one sentence: **read the raw material and produce the single Cursor-ready handoff block for the right lane**, preferring `q-capture` over `q-export` when the material is short and insight-dense, and `q-export` when the material is long, unfiltered, or likely to reward later mining.

Full routing rules: [`quarry-guidelines.md`](quarry-guidelines.md).

## Source of truth

The vault (`Axios/`) is canonical. This repository is a **derived artifact** synced from the vault by a script. Do not hand-edit files here; edit in the vault and re-run the sync.

## License and scope

This repository contains system documentation only. It is published so the author's tools (Grok, other models, collaborators) have a stable reference. It is not a product, is not open to contribution, and carries no warranty.
