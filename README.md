# axios-public

The public slice of **Axios** — the author's long-horizon thinking and content system. This repository is the authoritative reference for vocabulary, pipeline, conventions, and the single Grok-side trigger (`xbridge`) that bridges X content and Grok conversations into the private vault.

The **private vault** (`Axios/`) contains journals, therapy, inquiry, raw corpora, and operational records. None of that lives here. What lives here is the small set of documents Grok needs so it stays aligned with the system across sessions.

## Purpose

1. Give Grok a stable, linkable source of truth for how Axios thinks and how its pipeline works.
2. Document the single Grok trigger contract (`xbridge`) Grok uses to hand X content and Grok conversations off to the private vault via Cursor's `/capture` command.
3. Make the persona, voice, and content-tier frame legible to any model or collaborator the author chooses to share them with.

## Read in this order

Matches the Grok Axios project custom instruction (`mission.md` → `mission-primer.md` → `glossary.md` → …):

1. [`mission.md`](mission.md) — the single paragraph. What Axios is for.
2. [`mission-primer.md`](mission-primer.md) — how models should *apply* the mission (not a second mission text).
3. [`glossary.md`](glossary.md) — the terms (Atelier, Quarry, Journal, the Integrating Warrior, the Integrating Voice, etc.) plus mission-alignment notes for models. Retired terms (Steward of Value, Considered Witness — both retired 2026-04-25) are kept in the glossary so legacy material remains legible.
4. [`vocabulary.md`](vocabulary.md) — preferred vs retired terms, with reasons. The antibody against register drift.
5. [`persona-voice.md`](persona-voice.md) — public persona and voice.
6. [`content-tiers.md`](content-tiers.md) — how output is tiered; Truth-over-time.
7. [`conventions.md`](conventions.md) — slugs, metadata, filename rules.
8. [`quarry-guidelines.md`](quarry-guidelines.md) — the three lanes: what Grok bridges, what goes directly to Cursor, what is refused.
9. [`xbridge.md`](xbridge.md) — the single Grok trigger contract.

## Pipeline at a glance

Raw material flows one direction. Nothing skips stages.

```
X content / Grok conversation / term lookup
    |
    v
Grok: xbridge  --->  single /capture block  --->  Cursor /capture  --->  Atelier/10-Sources/

Everything else (transcripts, highlights, other-model chats, own drafts, own proposals)
    |
    v
Direct paste into Cursor  --->  /quarry | /capture | /draft | /ship | /propose | /reflect
```

Grok has exactly one trigger: `xbridge`. It exists because Cursor cannot fetch X content directly — Grok is the X-bridge. Everything else the author already has in text form; it goes straight to Cursor.

The earlier dispatcher family (`g-route` / `g-quarry` / `g-capture` / `g-propose` / `g-draft` / `g-ship`) is retired. Grok proved unreliable at structured multi-lane routing and most non-X material does not benefit from a Grok round-trip.

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

Grok's leverage is on the **intake side, narrowed**:

- Native access to X (threads, quote chains, profiles, replies) — the one surface Cursor cannot reach.
- Long context inside the current conversation — useful for summaries, exchanges, and term lookups the author wants to capture as sources.

That is the whole job. For YouTube transcripts, podcast transcripts, subtitles, highlights, another model's chat exports, or material the author is drafting or packaging, Grok adds round-trip overhead without adding value — Cursor handles those directly.

Full routing rules: [`quarry-guidelines.md`](quarry-guidelines.md). Full trigger spec: [`xbridge.md`](xbridge.md).

## Source of truth

The private vault (`Axios/`) is canonical for the **whole system**. In *this* repo, only **`mission.md`** and **`xbridge.md`** are copied from the vault by the whitelist sync script in the vault (`Support/Resources/sync-axios-public.sh`). **Every other markdown file here is edited in this repository** (those paths are not overwritten by sync). After changing the vault’s mission or the `xbridge` spec, run that script or vault `/push` so GitHub stays current.

## License and scope

This repository contains system documentation only. It is published so the author's tools (Grok, other models, collaborators) have a stable reference. It is not a product, is not open to contribution, and carries no warranty.
