---
date: 2026-04-22
type: reference
tags: [grok, draft, article, trigger, instruction]
---

# `q-draft` — Grok-side trigger for longform article handoff

The Grok custom instruction the author triggers inside a Grok conversation once a longform article body has been drafted in chat. It produces a single Cursor-ready markdown block. Paste the output into Cursor and run `/draft`; the block is consumed by the handoff mode defined in the `/draft` command on the Cursor side and saved to `Atelier/60-Catalog/Articles/`.

## How to install

In Grok (grok.com → your project / custom instructions), add the instruction below. Trigger it by typing `q-draft` in any Grok conversation where a polished longform article has been drafted in the thread. Grok returns one fenced block; the author copies it and pastes it into Cursor.

---

## The instruction

> **Trigger name:** `q-draft`
>
> When the user types `q-draft` anywhere in the conversation, produce a single markdown code-block that the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **longform article handoff** to Cursor's `/draft` command. It must include, in this exact order:
>
> 1. A `**Voice Intent**` card. Required shape:
>
>    ```text
>    **Voice Intent**
>    - **Edge:** <the claim or angle to sharpen and lead with>
>    - **No-soften zone:** <what must not be diluted, softened, or appeased>
>    - **Landing:** <how you want it to feel to the reader>
>    ```
>
>    Optional line: `- **My read:** <one-sentence author self-read>`.
>    If the author cannot or will not supply Voice Intent, emit a single line instead: `SKIP: <reason>`. Do not fabricate Voice Intent under any circumstances.
>
> 2. A `**Metadata**` block listing these keys, one per line, in this order. Bold the key names.
>    - `content-id:` — kebab-case slug, ASCII, ~72 char max. Matches the core thesis of the article.
>    - `main-topic:` — short human-readable topic (2-6 words).
>    - `tags:` — YAML inline list of 3-7 kebab-case tags.
>    - `ship-date:` *(optional)* — `YYYY-MM-DD` if the author has named a target ship date in-chat; otherwise omit.
>    - `title:` *(optional)* — final article title if distinct from `content-id`.
>    - `deck:` *(optional)* — 1-sentence subtitle / standfirst.
>    - `series:` *(optional)* — e.g. `truth-over-time`.
>    - `source:` *(optional)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).
>
> 3. If `series: truth-over-time`, a `**Truth over Time**` block with these keys, one per line:
>    - `historical-anchor:` — the classical source / figure / event being placed.
>    - `modern-anchor:` — the modern-day expression or fracture the article lands on.
>    - `historical-relevance:` — why the anchor mattered in its time.
>    - `present-relevance:` — why it matters now.
>    - `fracture-line:` — the honest tension or limitation the article names.
>
>    Omit the block entirely when `series` is not `truth-over-time`.
>
> 4. A `## Article draft` heading, followed by the **complete polished article body** as continuous ship prose. Not thread fragments. Not tweet order. Not bullet points unless the article genuinely uses them. Write it the way Cursor will save it — consistent with a longform essay, FK ~9-10, clear argumentative spine. Preserve any source attribution inline. Include a single epistemic-honesty paragraph if the article surfaces one.
>
> 5. As the very last line of the block, an explicit handoff line, verbatim:
>
>    `Handoff: /draft`
>
> The *only* output when `q-draft` is typed is the markdown block itself, wrapped in a triple-backtick fence so it copies as one unit. No preamble, no explanation, no "Here is your draft:" line. Just the fenced block.
>
> If the conversation does not yet contain a drafted article body (only an outline, notes, or an early sketch), respond instead with a single line: `q-draft: no polished article body in this chat yet — draft the article first, then re-run q-draft.`

---

## Cursor side

After pasting the block into Cursor, run **`/draft`** (defined in the `/draft` command on the Cursor side). The command detects the Voice Intent card + Metadata + `Handoff: /draft` line and enters **handoff mode**: it validates the gate, normalizes `content-id`, resolves the save date from `ship-date` (or today's local date), polishes the body to ship prose, and writes to `Atelier/60-Catalog/Articles/{date}-{content-id}.md`.

Do **not** include `Claim-risk`, `Distribution`, `Meaning frame`, or `Strategic Guidance` blocks in a `q-draft` output. Those belong to the downstream `q-ship` packaging pass run **after** the article is saved.

## Sequence

1. Draft the article in a Grok conversation (`q-draft` is a handoff, not a writing aid).
2. Type `q-draft` — Grok emits the block.
3. Paste into Cursor, run `/draft`.
4. Hemingway Desktop + hand edits on the saved article file.
5. When ready to package: paste the final article (with its YAML so Grok can see `voice-intent`) back into Grok and run `q-ship`. See [`q-ship.md`](q-ship.md).
