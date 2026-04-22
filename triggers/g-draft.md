---
date: 2026-04-22
type: reference
tags: [grok, draft, article, trigger, instruction]
---

# `g-draft` — Grok-side trigger for longform article handoff

The Grok custom instruction the author triggers inside a Grok conversation once a longform article body has been drafted in chat. It produces a single Cursor-ready markdown block, **beginning with `/draft`**, so one paste into Cursor fires the command and saves the article to `Atelier/60-Catalog/Articles/`.

Pairs with the Cursor command `/draft` (handoff mode defined in the `/draft` command on the Cursor side). Previously named `q-draft`.

## How to install

In Grok (grok.com → your project / custom instructions), add the instruction below. Trigger it by typing `g-draft` in any Grok conversation where a polished longform article has been drafted in the thread. Grok returns one fenced block; the author clicks the code-block copy button and pastes into Cursor. Because the pasted content begins with `/draft`, Cursor invokes the command on paste+enter.

---

## The instruction

> **Trigger name:** `g-draft`
>
> When the user types `g-draft` anywhere in the conversation, produce a single markdown code-block that the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **longform article handoff** to Cursor's `/draft` command. It must include, in this exact order:
>
> 1. `/draft` on its own line as the very first line of the block. This is the Cursor trigger; one paste fires the command.
>
> 2. One blank line.
>
> 3. A `**Voice Intent**` card. Required shape:
>
>    ```text
>    **Voice Intent**
>    - **Thesis:** <the central claim the piece stands on; what it actually argues>
>    - **Non-negotiables:** <what must not be diluted, hedged, or appeased away>
>    - **Landing:** <how you want the argument to settle with the reader>
>    ```
>
>    Optional line: `- **My read:** <one-sentence author self-read>`.
>    If the author cannot or will not supply Voice Intent, emit a single line instead: `SKIP: <reason>`. Do not fabricate Voice Intent under any circumstances.
>
> 4. A `**Metadata**` block listing these keys, one per line, in this order. Bold the key names.
>    - `content-id:` — kebab-case slug, ASCII, ~72 char max. Matches the core thesis of the article.
>    - `main-topic:` — short human-readable topic (2-6 words).
>    - `tags:` — YAML inline list of 3-7 kebab-case tags.
>    - `ship-date:` *(optional)* — `YYYY-MM-DD` if the author has named a target ship date in-chat; otherwise omit.
>    - `title:` *(optional)* — final article title if distinct from `content-id`.
>    - `deck:` *(optional)* — 1-sentence subtitle / standfirst.
>    - `series:` *(optional)* — e.g. `truth-over-time`.
>    - `source:` *(optional)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).
>
> 5. If `series: truth-over-time`, a `**Truth over Time**` block with these keys, one per line:
>    - `historical-anchor:` — the classical source / figure / event being placed.
>    - `modern-anchor:` — the modern-day expression or fracture the article lands on.
>    - `historical-relevance:` — why the anchor mattered in its time.
>    - `present-relevance:` — why it matters now.
>    - `fracture-line:` — the honest tension or limitation the article names.
>
>    Omit the block entirely when `series` is not `truth-over-time`.
>
> 6. A `## Article draft` heading, followed by the **complete polished article body** as continuous ship prose. Not thread fragments. Not tweet order. Not bullet points unless the article genuinely uses them. Write it the way Cursor will save it — consistent with a longform essay, clear argumentative spine, prose that moves without friction. **Do not target a Flesch-Kincaid or equivalent grade score.** The Considered Witness register is not built to hit one; voice and precision are the priority. Preserve any source attribution inline. Include a single epistemic-honesty paragraph if the article surfaces one.
>
> 7. As the very last line of the block, an explicit handoff line, verbatim:
>
>    `Handoff: /draft`
>
> The *only* output when `g-draft` is typed is the markdown block itself, wrapped in a triple-backtick fence so grok.com renders a click-to-copy button that strips the backticks. No preamble, no explanation, no "Here is your draft:" line. Just the fenced block.
>
> If the conversation does not yet contain a drafted article body (only an outline, notes, or an early sketch), respond instead with a single line: `g-draft: no polished article body in this chat yet — draft the article first, then re-run g-draft.`

---

## Example emitted block (shape, not content)

````markdown
/draft

**Voice Intent**
- **Thesis:** ...
- **Non-negotiables:** ...
- **Landing:** ...

**Metadata**
- **content-id:** example-thesis-slug
- **main-topic:** Short topic
- **tags:** [tag-one, tag-two, tag-three]

## Article draft

<the polished article body as continuous ship prose>

Handoff: /draft
````

When the author clicks Grok's copy button, the paste that lands in Cursor starts with `/draft` and fires the command.

## Cursor side

`/draft` (handoff mode, defined in the `/draft` command on the Cursor side) validates the Voice Intent gate, normalizes `content-id`, resolves the save date from `ship-date` (or today's local date), polishes the body to ship prose, and writes to `Atelier/60-Catalog/Articles/{date}-{content-id}.md`.

Do **not** include `Claim-risk band`, `Carry band`, `Meaning frame`, or `Strategic Guidance` blocks in a `g-draft` output. Those belong to the downstream `g-ship` packaging pass run **after** the article is saved.

## Sequence

1. Draft the article in a Grok conversation (`g-draft` is a handoff, not a writing aid).
2. Type `g-draft` — Grok emits the block.
3. Click copy, paste into Cursor, press enter. `/draft` fires on its own.
4. Hand edits + optional clarity pass on the saved article file (see [`toolkit.md`](toolkit.md)). No readability-grade target — voice and precision come first.
5. When ready to package: paste the final article (with its YAML so Grok can see `voice-intent`) back into Grok and run `g-ship`. See [`g-ship.md`](g-ship.md).
