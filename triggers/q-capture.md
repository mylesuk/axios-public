---
date: 2026-04-22
type: reference
tags: [grok, capture, source, trigger, instruction]
---

# `q-capture` — Grok-side trigger for Atelier source handoff

The Grok custom instruction the author triggers after reviewing a **single deliberate piece of raw material** in a Grok conversation — one X thread, one article, one quote with attribution, one clean exchange, one book passage. Grok produces a single Cursor-ready markdown block that saves the material directly to `Atelier/10-Sources/` with 1–3 candidate insight stubs already attached, **skipping Quarry**.

This is the pre-filter lane that makes Grok earn its keep on the intake side. It is distinct from [`q-export`](q-export.md) (raw corpora dump for later mining) and from [`q-draft`](q-draft.md) / [`q-ship`](q-ship.md) (article drafting / packaging).

## When to use `q-capture` vs alternatives

- **`q-capture`** when: the material is a deliberate pick, Grok can extract 1–3 atomic claim stubs without padding, and the material is short enough to reproduce key passages verbatim.
- **`q-export`** when: the material is long, unfiltered, multi-topic, or a full chat transcript meant for later mining.
- **`q-draft`** when: a polished longform article body has been drafted in-chat.
- **`q-ship`** when: a saved catalog article exists and the author is packaging it into derivatives.

Routing rules are canonical in [axios-public/quarry-guidelines.md](../quarry-guidelines.md).

## How to install

In Grok (grok.com → your project / custom instructions), add the instruction below. Trigger it by typing `q-capture` in any Grok conversation that has surfaced a single piece of raw material worth distilling. Grok returns one fenced block; the author copies it and pastes it into Cursor.

---

## The instruction

> **Trigger name:** `q-capture`
>
> When the user types `q-capture` anywhere in the conversation, produce a single markdown code-block that the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **source capture handoff** to Cursor's `/capture` command. It must include, in this exact order:
>
> 1. A `**Source summary**` block — 3–5 lines of the raw material's **substance** (what it argues or shows), not just its topic. Plain prose, no bullets.
>
> 2. A `**Source metadata**` block, one key per line, bold key names:
>    - `source-type:` — one of `x-thread`, `article`, `video`, `podcast`, `book-passage`, `conversation`, `talk`, `quote`, `word`.
>    - `source-url:` *(when available)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).
>    - `source-author:` *(when public and known)* — author, handle, or speaker.
>    - `source-title:` *(when the material has a stable title)* — the title as the author would reference it.
>    - `source-date:` *(when the material carries a publication date)* — `YYYY-MM-DD`.
>
> 3. A `**Candidate insights**` block — 1–3 atomic claim stubs, each 1–2 sentences, each a standalone claim. Not summaries. Not outline points. Claims. If the material yields zero defensible atomic claims, emit `SKIP: material does not yield atomic claims — route to q-export instead.` and stop.
>
> 4. A `**Verbatim passages**` block — 1–5 quotable lines or short paragraphs worth preserving, each prefixed with `> ` blockquote, followed on its own line by a source attribution when applicable. Not the full source dump; only the load-bearing passages.
>
> 5. A `**Metadata**` block, one key per line, bold key names:
>    - `content-id:` — kebab-case slug, ASCII, ~72 char max, aligned with the central claim.
>    - `main-topic:` — short human-readable topic (2–6 words).
>    - `tags:` — YAML inline list of 3–7 kebab-case tags. Always include `source`.
>
> 6. As the very last line of the block, an explicit handoff line, verbatim:
>
>    `Handoff: /capture`
>
> The *only* output when `q-capture` is typed is the markdown block itself, wrapped in a triple-backtick fence. No preamble, no explanation, no "Here is your capture:" line. Just the fenced block.
>
> If the conversation does not yet contain a single deliberate piece of raw material (only a request, a question, or a multi-topic chat), respond with a single line: `q-capture: no single source in this chat — paste the material (thread, article, quote, passage) first, then re-run q-capture.`
>
> Never fabricate the source, its author, its URL, or its date. Never invent candidate insights the material does not support. If the material contains Journal-like personal reflection, stop and emit: `q-capture: this looks like inner-work material — Journal lane, not Atelier. Route via the author's Inner Work Bridge, not q-capture.`

---

## Cursor side

After pasting the block into Cursor, run **`/capture`** (defined in [`the `/capture` command on the Cursor side`](the `/capture` command on the Cursor side)). The command detects the `Source summary` + `Source metadata` + `Metadata` + `Handoff: /capture` line and enters **handoff mode**:

1. Resolves the save path as `Atelier/10-Sources/source-{content-id}.md` (or `Atelier/10-Sources/Experiments/experiment-{content-id}-{YYYY-MM}.md` when `source-type` signals a lived experiment).
2. Writes the source file from [`Atelier/10-Sources/_template.md`](the source template), populating:
   - YAML: `date` (today), `type: source`, `source-type`, `url` (from `source-url`), `author` (from `source-author`), `status: captured`, `tags` (from Metadata, deduplicated with the template's defaults).
   - Body: `Source summary` → "Why captured"; `Verbatim passages` → "Raw content / key passages"; `Candidate insights` → the distillation section as unchecked candidate claim stubs ready for `/distill`.
3. Prints the saved path and offers the standard next-step choice (`/distill` / `/triage` / bank).

Do **not** include Voice Intent, Claim-risk, Distribution, or Article draft sections in a `q-capture` output. Those belong to downstream lanes (`q-draft`, `q-ship`) run only after the source has been distilled into insights and a draft has emerged.

## Sequence

1. Paste or point Grok at a single piece of raw material in a conversation.
2. Ask Grok for a pre-filter pass (TL;DR, candidate insights, register check, routing recommendation).
3. Confirm `q-capture` is the right lane.
4. Type `q-capture` — Grok emits the block.
5. Paste into Cursor, run `/capture`.
6. Later: `/distill` turns the candidate claim stubs into atomic insights in `Atelier/20-Insights/`.
