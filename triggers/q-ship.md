---
date: 2026-04-22
type: reference
tags: [grok, ship, packaging, trigger, instruction]
---

# `q-ship` — Grok-side trigger for derivative packaging handoff

The Grok custom instruction the author triggers inside a Grok conversation **after** a polished article already exists in `Atelier/60-Catalog/Articles/`. The author pastes the saved article (with its YAML so Grok can see `voice-intent`) into Grok, lets Grok evaluate it, then types `q-ship`. Grok returns one Cursor-ready markdown block — the packaging handoff consumed by `/ship` in derivative mode (defined in the `/ship` command on the Cursor side). Cursor uses the block to buff the article, generate an X thread, and generate an Instagram carousel, all saved to `Atelier/60-Catalog/`.

## How to install

In Grok (grok.com → your project / custom instructions), add the instruction below. Trigger it by typing `q-ship` in any Grok conversation in which the full article (body + YAML) has been pasted and Grok has been able to read `voice-intent`. Grok returns one fenced block; the author copies it and pastes it into Cursor.

---

## The instruction

> **Trigger name:** `q-ship`
>
> When the user types `q-ship` anywhere in the conversation, produce a single markdown code-block that the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **derivative packaging handoff** to Cursor's `/ship` command. It must include, in this exact order:
>
> 1. A `**Claim-risk score**` block:
>    - A single integer score from 0 (benign, universally agreeable) to 10 (maximally provocative / likely to provoke strong pushback).
>    - A one- to two-sentence rationale naming which claims in the article carry the risk.
>    - Do **not** score above what the article actually argues. If the article hedges, the score is lower.
>
> 2. A `**Distribution score**` block:
>    - A single integer score from 0 (inert, no share pull) to 10 (high viral / save / share pull across the target audience).
>    - A one- to two-sentence rationale naming the hook, the emotional lever, or the classical anchor driving share probability.
>
> 3. *(Optional, include when genuinely relevant)* `**Meaning frame**` — one paragraph describing the lens the author is inviting the reader into (e.g. classical virtue, honest fracture, systems failure, developmental).
>
> 4. *(Optional, include when the article maps to a clear strategic posture)* `**Strategic Guidance**` — one short paragraph on what this piece does for positioning within the author's body of work. Which concentric circle it primarily serves. Whether it is a signature / supporting / exploratory piece.
>
> 5. *(Optional, include when the article has a classical or moral anchor)* `**Classical / Moral Resonance**` — one or two sentences naming the classical figure, text, virtue, or moral principle the piece places itself in conversation with.
>
> 6. A `**Metadata**` block listing these keys, one per line, in this order. Bold the key names.
>    - `content-id:` — kebab-case slug, ASCII. **Must match the saved article's basename stem** when the article is already in the catalog. If the user has pasted the article YAML, reuse the `content-id` verbatim.
>    - `main-topic:` — short human-readable topic (2-6 words).
>    - `tags:` — YAML inline list of 3-7 kebab-case tags.
>    - `ship-date:` — `YYYY-MM-DD`. Defaults to the saved article's `date`; otherwise today.
>    - `series:` *(optional)* — e.g. `truth-over-time`.
>    - Optional: `Article file: Atelier/60-Catalog/Articles/YYYY-MM-DD-{content-id}.md` on its own line. Include when the author has told Grok the exact saved path; Cursor uses this to resolve the file strictly.
>
> 7. If `series: truth-over-time`, a `**Truth over Time**` block with these keys, one per line. Reuse the values from the saved article's YAML when present; only fill new content if the author has explicitly updated one in-chat:
>    - `historical-anchor:`
>    - `modern-anchor:`
>    - `historical-relevance:`
>    - `present-relevance:`
>    - `fracture-line:`
>
>    Omit the block entirely when `series` is not `truth-over-time`.
>
> 8. *(Optional)* An **Editor recommendation** section with image prompts for the carousel key art — 3-5 short image prompts, each on its own bullet. Include only if the author asks for it or if the article clearly benefits from visual language. No image prompts inside the article body itself.
>
> 9. As the very last line of the block, an explicit handoff line, verbatim:
>
>    `Handoff: /ship`
>
> The *only* output when `q-ship` is typed is the markdown block itself, wrapped in a triple-backtick fence so it copies as one unit. No preamble, no explanation, no "Here is your packaging:" line. Just the fenced block.
>
> If the conversation does not yet contain the polished article body (Grok has not been shown the saved article), respond instead with a single line: `q-ship: no article visible in this chat yet — paste the saved article (body + YAML) first, then re-run q-ship.`
>
> Do not include an `Article draft` section in a `q-ship` output — that belongs to `q-draft`. The article already exists on disk; Cursor will read the saved file directly and trust it over anything in this block for claims and wording.

---

## Cursor side

After pasting the block into Cursor, run **`/ship`** (defined in the `/ship` command on the Cursor side). The command detects `Claim-risk`, `Distribution`, `Metadata`, and the `Handoff: /ship` line and enters **derivative mode**:

1. Resolves the canonical article path via `content-id` (or the optional `Article file:` hint).
2. Applies Voice Intent (precedence: pasted intent this run > article YAML `voice-intent`).
3. Buffs the article body per voice rules.
4. Generates the X thread and Instagram carousel from the post-buff article.
5. Writes all three files to `Atelier/60-Catalog/{Articles,Threads,Carousels}/YYYY-MM-DD-{content-id}.md`.

## Sequence

1. Save the article first with `q-draft` → `/draft` (see [`q-draft.md`](q-draft.md)). Polish with Hemingway Desktop + hand edits.
2. Paste the **final saved article, body + YAML**, into a fresh Grok conversation so `voice-intent` is visible.
3. Type `q-ship` — Grok emits the packaging block.
4. Paste into Cursor, run `/ship`.
5. Optional: ProWritingAid Premium polish on the thread and carousel before posting.

## Honesty rules

- Do not fabricate Claim-risk or Distribution scores. Ground them in the article as written.
- Do not add new factual claims in the packaging block. The article is authoritative for claims.
- If the packaging block drifts from the article, Cursor trusts the article.
