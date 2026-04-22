---
date: 2026-04-22
type: reference
tags: [grok, quarry, trigger, instruction]
---

# `g-quarry` — Grok-side trigger for Quarry handoff

The Grok custom instruction the author triggers inside a Grok conversation to produce a single Cursor-ready markdown block. The emitted block **starts with `/quarry`** so that one paste into Cursor fires the command automatically — no second step.

Pairs with the Cursor command `/quarry`. Previously named `q-export`.

## How to install

In Grok (grok.com → your project / custom instructions), add the instruction below. Trigger it by typing `g-quarry` in any conversation. Grok returns one fenced block; the author clicks the code-block copy button (which strips the outer backticks) and pastes into Cursor. Because the pasted content begins with `/quarry`, Cursor invokes the command on paste+enter.

---

## The instruction

> **Trigger name:** `g-quarry`
>
> When the user types `g-quarry` anywhere in the conversation, produce a single markdown code-block that the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and include, in this exact order:
>
> 1. `/quarry` on its own line as the very first line of the block. This is the Cursor trigger; one paste fires the command.
> 2. One blank line.
> 3. A valid YAML frontmatter opening (`---`) with these keys, in this order:
>    - `date:` the date the export is produced, ISO `YYYY-MM-DD`.
>    - `type: quarry` (always exactly this).
>    - `source: grok` (always exactly this).
>    - `topic:` short kebab-case slug inferred from the main subject of this conversation. 2-5 words, lowercased, hyphenated, no quotes. Pick the topic that covers the majority of the exchange.
>    - `status: raw` (always exactly this).
>    - `tags:` a YAML inline list (`[tag-one, tag-two, ...]`) of 3-7 kebab-case tags derived from the conversation themes. Always include `grok`. Add subject-matter tags (e.g. `mstr`, `therapy`, `property-bali`, `content-strategy`, `looksmax`, `philosophy`, etc.).
>    - `status-closing:` optional — `draft` if the conversation is ongoing, omit otherwise.
>    - Optional fields when relevant: `source-url:`, `source-author:`, `source-title:`, `duration:`.
> 4. Closing `---` then one blank line.
> 5. A single `# <Human-readable title>` header describing the conversation's main subject (not just the topic slug — write it as a reader would expect to see it in an article TOC, 4-10 words, no trailing punctuation).
> 6. One blank line.
> 7. The **complete, verbatim conversation**, reproduced as faithfully as possible. Use `**You:**` and `**Grok:**` bold labels for speaker turns, preserve code blocks, lists, tables, inline math, and any images' alt text. Do **not** summarize. Do **not** paraphrase. Do **not** add commentary, editorial notes, or "(continued)" markers. Do **not** skip turns. If the conversation is too long to fit in one response, say so honestly in a single line at the end and continue on the user's next prompt.
> 8. Do **not** add closing remarks after the conversation body. The block ends with the last message.
>
> The *only* output when `g-quarry` is typed is the markdown block itself, wrapped in a triple-backtick fence so grok.com renders a click-to-copy button that strips the backticks. No preamble, no explanation, no "Here is your export:" line. Just the fenced block.
>
> If the conversation is too short to be worth exporting (fewer than ~3 meaningful exchanges), respond instead with a single line: `g-quarry: conversation too short — nothing to export yet.`

---

## Example emitted block (shape, not content)

````markdown
/quarry

---
date: 2026-04-22
type: quarry
source: grok
topic: example-topic-slug
status: raw
tags: [grok, example]
---

# Human-readable title of the conversation

**You:** ...

**Grok:** ...
````

When the author clicks Grok's copy button on this block, the paste that lands in Cursor is:

```
/quarry

---
date: 2026-04-22
...
```

Cursor sees `/quarry` on line 1, invokes the command, and consumes the rest as input.

## Cursor side

`/quarry` (defined in `the `/quarry` command on the Cursor side`) reads the pasted body, validates the frontmatter, fills gaps, picks the filename (`Quarry/{date}-{topic}-grok.md`), and saves. `Quarry/` is gitignored.

## Non-Grok sources

For YouTube transcripts, `.srt`/`.vtt` subtitle files, Readwise/Kindle highlights exports, ChatGPT/Claude/Gemini chats, or long voice-memo transcripts, the author can paste them directly into Cursor and run `/quarry`. `/quarry` will prompt for `source:` (picking from the controlled vocabulary) and the other fields. The `g-quarry` trigger is Grok-specific; other platforms don't need one.
