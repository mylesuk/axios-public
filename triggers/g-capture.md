---
date: 2026-04-22
type: reference
tags: [grok, capture, source, trigger, instruction]
---

# `g-capture` — Grok-side trigger for Atelier source handoff

The Grok custom instruction the author triggers after reviewing a **single deliberate piece of raw material** in a Grok conversation — one X thread, one article, one quote with attribution, one clean exchange, one book passage. Grok produces a single Cursor-ready markdown block, **beginning with `/capture`**, that saves the material directly to `Atelier/10-Sources/` with 1–3 candidate insight stubs already attached, **skipping Quarry**.

Pairs with the Cursor command `/capture`. Previously named `q-capture`. This is the pre-filter lane that makes Grok earn its keep on the intake side. Distinct from [`g-quarry`](g-quarry.md) (raw corpora dump for later mining) and from [`g-draft`](g-draft.md) / [`g-ship`](g-ship.md) (article drafting / packaging).

## When to use `g-capture` vs alternatives

- **`g-capture`** when: the material is a deliberate pick, Grok can extract 1–3 atomic claim stubs without padding, and the material is short enough to reproduce key passages verbatim.
- **`g-quarry`** when: the material is long, unfiltered, multi-topic, or a full chat transcript meant for later mining.
- **`g-draft`** when: a polished longform article body has been drafted in-chat.
- **`g-ship`** when: a saved catalog article exists and the author is packaging it into derivatives.

Routing rules are canonical in [axios-public/quarry-guidelines.md](../quarry-guidelines.md).

## How to install

In Grok (grok.com → your project / custom instructions), add the instruction below. Trigger it by typing `g-capture` in any Grok conversation that has surfaced a single piece of raw material worth distilling. Grok returns one fenced block; the author clicks the code-block copy button and pastes into Cursor. Because the pasted content begins with `/capture`, Cursor invokes the command on paste+enter.

---

## The instruction

> **Trigger name:** `g-capture`
>
> When the user types `g-capture` anywhere in the conversation, produce a single markdown code-block that the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **source capture handoff** to Cursor's `/capture` command. It must include, in this exact order:
>
> 1. `/capture` on its own line as the very first line of the block. This is the Cursor trigger; one paste fires the command.
>
> 2. One blank line.
>
> 3. A `**Source summary**` block — 3–5 lines of the raw material's **substance** (what it argues or shows), not just its topic. Plain prose, no bullets.
>
> 4. A `**Source metadata**` block, one key per line, bold key names:
>    - `source-type:` — one of `x-thread`, `article`, `video`, `podcast`, `book-passage`, `conversation`, `talk`, `quote`, `word`.
>    - `source-url:` *(when available)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).
>    - `source-author:` *(when public and known)* — author, handle, or speaker.
>    - `source-title:` *(when the material has a stable title)* — the title as the author would reference it.
>    - `source-date:` *(when the material carries a publication date)* — `YYYY-MM-DD`.
>
> 5. A `**Candidate insights**` block — 1–3 atomic claim stubs, each 1–2 sentences, each a standalone claim. Not summaries. Not outline points. Claims. If the material yields zero defensible atomic claims, emit `SKIP: material does not yield atomic claims — route to g-quarry instead.` and stop.
>
> 6. A `**Verbatim passages**` block — 1–5 quotable lines or short paragraphs worth preserving, each prefixed with `> ` blockquote, followed on its own line by a source attribution when applicable. Not the full source dump; only the load-bearing passages.
>
> 7. A `**Metadata**` block, one key per line, bold key names:
>    - `content-id:` — kebab-case slug, ASCII, ~72 char max, aligned with the central claim.
>    - `main-topic:` — short human-readable topic (2–6 words).
>    - `tags:` — YAML inline list of 3–7 kebab-case tags. Always include `source`.
>
> 8. As the very last line of the block, an explicit handoff line, verbatim:
>
>    `Handoff: /capture`
>
> The *only* output when `g-capture` is typed is the markdown block itself, wrapped in a triple-backtick fence so grok.com renders a click-to-copy button that strips the backticks. No preamble, no explanation, no "Here is your capture:" line. Just the fenced block.
>
> If the conversation does not yet contain a single deliberate piece of raw material (only a request, a question, or a multi-topic chat), respond with a single line: `g-capture: no single source in this chat — paste the material (thread, article, quote, passage) first, then re-run g-capture.`
>
> Never fabricate the source, its author, its URL, or its date. Never invent candidate insights the material does not support. If the material contains Journal-like personal reflection, stop and emit: `g-capture: this looks like inner-work material — Journal lane, not Atelier. Route via the author's Inner Work Bridge, not g-capture.`

---

## Example emitted block (shape, not content)

````markdown
/capture

**Source summary**
Three or four lines describing what the source actually argues or shows.

**Source metadata**
- **source-type:** x-thread
- **source-url:** https://x.com/...
- **source-author:** @handle

**Candidate insights**
- First atomic claim the source yields.
- Second atomic claim.
- Third atomic claim.

**Verbatim passages**
> The most load-bearing line.
— @handle, 2026-04-22

**Metadata**
- **content-id:** example-content-id
- **main-topic:** Example topic
- **tags:** [source, example, topic]

Handoff: /capture
````

When the author clicks Grok's copy button, the paste that lands in Cursor starts with `/capture` and fires the command.

## Cursor side

`/capture` (defined in `the `/capture` command on the Cursor side`) parses the block, resolves the save path as `Atelier/10-Sources/source-{content-id}.md` (or `Atelier/10-Sources/Experiments/experiment-{content-id}-{YYYY-MM}.md` when `source-type` signals a lived experiment), and writes the file from the source template:

- YAML: `date` (today), `type: source`, `source-type`, `url` (from `source-url`), `author` (from `source-author`), `status: captured`, `tags` (from Metadata, deduplicated with template defaults).
- Body: `Source summary` → "Why captured"; `Verbatim passages` → "Raw content / key passages"; `Candidate insights` → the distillation section as unchecked candidate claim stubs ready for `/distill`.

The command prints the saved path and offers the standard next-step choice (`/distill` / `/triage` / bank).

Do **not** include Voice Intent, Claim-risk, Distribution, or Article draft sections in a `g-capture` output. Those belong to downstream lanes (`g-draft`, `g-ship`) run only after the source has been distilled into insights and a draft has emerged.

## Sequence

1. Paste or point Grok at a single piece of raw material in a conversation.
2. Ask Grok for a pre-filter pass (TL;DR, candidate insights, register check, routing recommendation).
3. Confirm `g-capture` is the right lane.
4. Type `g-capture` — Grok emits the block.
5. Click copy, paste into Cursor, press enter. `/capture` fires on its own.
6. Later: `/distill` turns the candidate claim stubs into atomic insights in `Atelier/20-Insights/`.
