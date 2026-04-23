---
date: 2026-04-22
type: reference
tags: [grok, ship, packaging, trigger, instruction]
---

# `g-ship` — Grok-side trigger for derivative packaging handoff

The Grok custom instruction the author triggers inside a Grok conversation **after** a polished article already exists in `Atelier/60-Catalog/Articles/`. The author pastes the saved article (with its YAML so Grok can see `voice-intent`) into Grok, lets Grok evaluate it, then types `g-ship`. Grok returns one Cursor-ready markdown block, **beginning with `/ship`**, so one paste into Cursor fires the command. Cursor uses the block to buff the article, generate an X thread, and generate an Instagram carousel, all saved to `Atelier/60-Catalog/`.

Pairs with the Cursor command `/ship` (derivative mode defined in the `/ship` command on the Cursor side). Previously named `q-ship`.

## How to install

In Grok (grok.com → your project / custom instructions), add the instruction below. Trigger it by typing `g-ship` in any Grok conversation in which the full article (body + YAML) has been pasted and Grok has been able to read `voice-intent`. Grok returns one fenced block; the author clicks the code-block copy button and pastes into Cursor. Because the pasted content begins with `/ship`, Cursor invokes the command on paste+enter.

---

## The instruction

> **Trigger name:** `g-ship`
>
> When the user types `g-ship` anywhere in the conversation, produce a single markdown code-block that the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **derivative packaging handoff** to Cursor's `/ship` command. It must include, in this exact order:
>
> 1. `/ship` on its own line as the very first line of the block. This is the Cursor trigger; one paste fires the command.
>
> 2. One blank line.
>
> 3. A `**Claim-risk band**` block:
>    - A single band from `weak | adequate | strong | exceptional` naming how provocatively the article pushes. `weak` means benign / universally agreeable; `exceptional` means maximally provocative / likely to provoke strong pushback.
>    - A one- to two-sentence rationale naming which claims in the article carry the risk.
>    - Do **not** band above what the article actually argues. If the article hedges, the band is lower.
>
> 4. A `**Carry band**` block (previously called *Distribution score*):
>    - A single band from `weak | adequate | strong | exceptional` naming the piece's natural pull — whether it *carries* on its own intrinsic shape (save-worthiness, format fit, classical anchor strength, the clarity of the opening). Not algorithmic amplification, not engagement chasing.
>    - A one- to two-sentence rationale naming the opening, the classical anchor, or the move in the piece that does the carrying.
>
> 5. *(Optional, include when genuinely relevant)* `**Meaning frame**` — one paragraph describing the lens the author is inviting the reader into (e.g. classical virtue, honest fracture, systems failure, developmental).
>
> 6. *(Optional, include when the article maps to a clear strategic posture)* `**Strategic Guidance**` — one short paragraph on what this piece does for positioning within the author's body of work. Which concentric circle it primarily serves. Whether it is a signature / supporting / exploratory piece.
>
> 7. *(Optional, include when the article has a classical or moral anchor)* `**Classical / Moral Resonance**` — one or two sentences naming the classical figure, text, virtue, or moral principle the piece places itself in conversation with.
>
> 8. A `**Metadata**` block listing these keys, one per line, in this order. Bold the key names.
>    - `content-id:` — kebab-case slug, ASCII. **Must match the saved article's basename stem** when the article is already in the catalog. If the user has pasted the article YAML, reuse the `content-id` verbatim.
>    - `main-topic:` — short human-readable topic (2-6 words).
>    - `tags:` — YAML inline list of 3-7 kebab-case tags.
>    - `ship-date:` — `YYYY-MM-DD`. Defaults to the saved article's `date`; otherwise today.
>    - `series:` *(optional)* — e.g. `truth-over-time`.
>    - Optional: `Article file: Atelier/60-Catalog/Articles/YYYY-MM-DD-{content-id}.md` on its own line. Include when the author has told Grok the exact saved path; Cursor uses this to resolve the file strictly.
>
> 9. If `series: truth-over-time`, a `**Truth over Time**` block with these keys, one per line. Reuse the values from the saved article's YAML when present; only fill new content if the author has explicitly updated one in-chat:
>    - `historical-anchor:`
>    - `modern-anchor:`
>    - `historical-relevance:`
>    - `present-relevance:`
>    - `fracture-line:`
>
>    Omit the block entirely when `series` is not `truth-over-time`.
>
> 10. *(Optional)* An **Editor recommendation** section with image prompts for the carousel key art — 3-5 short image prompts, each on its own bullet. Include only if the author asks for it or if the article clearly benefits from visual language. No image prompts inside the article body itself.
>
> 11. As the very last line of the block, an explicit handoff line, verbatim:
>
>     `Handoff: /ship`
>
> The *only* output when `g-ship` is typed is the markdown block itself, wrapped in a triple-backtick fence so grok.com renders a click-to-copy button that strips the backticks. No preamble, no explanation, no "Here is your packaging:" line. Just the fenced block.
>
> If the conversation does not yet contain the polished article body (Grok has not been shown the saved article), respond instead with a single line: `g-ship: no article visible in this chat yet — paste the saved article (body + YAML) first, then re-run g-ship.`
>
> **Unsaved-article refusal.** If an article body is visible in the chat but no YAML frontmatter is present and the author has not confirmed the article exists on disk, do **not** emit packaging. The packaging lane assumes a saved file; shipping a never-saved draft risks Cursor looking for a file that does not exist or writing derivatives against a body that will be edited on disk. Emit: `g-ship: article body present but not confirmed saved — run g-draft → /draft first to persist the article, then paste the saved file (body + YAML) and re-run g-ship.` and stop.
>
> **YAML-invisible refusal.** If the pasted article body is present but lacks YAML frontmatter, Grok cannot see `voice-intent` and cannot ground packaging honestly. Emit: `g-ship: article YAML not visible — paste the saved file verbatim (frontmatter included) so voice-intent can ground the packaging.` and stop. Do not reconstruct voice-intent from the body; if the author will not paste YAML, default to `SKIP: voice-intent not visible` rather than inventing one.
>
> Do not include an `Article draft` section in a `g-ship` output — that belongs to `g-draft`. The article already exists on disk; Cursor will read the saved file directly and trust it over anything in this block for claims and wording.

---

## Example emitted block (shape, not content)

````markdown
/ship

**Claim-risk band**
- Band: strong
- Rationale: names a specific institutional failure by name.

**Carry band**
- Band: strong
- Rationale: classical anchor + clear fracture line; saves well on both platforms.

**Metadata**
- **content-id:** example-thesis-slug
- **main-topic:** Short topic
- **tags:** [tag-one, tag-two, tag-three]
- **ship-date:** 2026-04-22

Handoff: /ship
````

When the author clicks Grok's copy button, the paste that lands in Cursor starts with `/ship` and fires the command.

## Cursor side

`/ship` (derivative mode) detects `Claim-risk band`, `Carry band`, `Metadata`, and the `Handoff: /ship` line and:

1. Resolves the canonical article path via `content-id` (or the optional `Article file:` hint).
2. Applies Voice Intent (precedence: pasted intent this run > article YAML `voice-intent`).
3. Buffs the article body per voice rules.
4. Generates the X thread and Instagram carousel from the post-buff article.
5. Writes all three files to `Atelier/60-Catalog/{Articles,Threads,Carousels}/YYYY-MM-DD-{content-id}.md`.

## Sequence

1. Save the article first with `g-draft` → `/draft` (see [`g-draft.md`](g-draft.md)). Hand edit + optional clarity pass (see [`toolkit.md`](toolkit.md)).
2. Paste the **final saved article, body + YAML**, into a fresh Grok conversation so `voice-intent` is visible.
3. Type `g-ship` — Grok emits the packaging block.
4. Click copy, paste into Cursor, press enter. `/ship` fires on its own.
5. Optional: clarity pass on the thread and carousel before posting (see [`toolkit.md`](toolkit.md)).

## Honesty rules

- Do not fabricate Claim-risk or Carry bands. Ground them in the article as written.
- Do not add new factual claims in the packaging block. The article is authoritative for claims.
- If the packaging block drifts from the article, Cursor trusts the article.
