---
date: 2026-04-23
type: reference
tags: [grok, xbridge, trigger, instruction, x-bridge]
---

# `xbridge` — Grok's single trigger: X-bridge to Axios

The Grok custom instruction the author triggers when material lives on **X** (grok.com is the only tool that can read X threads, posts, quote chains, profiles) or was produced in the **current Grok conversation** (summaries, definitions, term lookups). Grok reads the material and emits a single Cursor-ready markdown block — **first line `/capture`** — that the author pastes into Cursor.

`xbridge` is the *only* Grok trigger. The retired `g-*` dispatcher family (`g-route`, `g-quarry`, `g-capture`, `g-propose`, `g-draft`, `g-ship`) is gone — Grok proved unreliable at structured multi-lane routing, and most non-X material (YouTube transcripts, podcasts, subtitles, highlights, other-model chat exports, longform articles the author is drafting) does not need Grok in the loop at all. The author pastes that material directly into Cursor.

## What `xbridge` is for

- **X content**: threads, single posts, quote chains, replies worth preserving with author handle and URL.
- **Current Grok conversation**: a short summary or key exchange from this chat that the author wants captured as a source (treat Grok as a legitimate thinking partner whose turns can be quoted, not hidden).
- **Term / word capture** (*word mode*): third-party philosophical, theological, or technical terms the author wants the vault to track. Grok may act as reference book — surface a working definition from training data, flag the provenance honestly.

## What `xbridge` is **not** for

- **External transcripts** (YouTube, podcast, `.srt`/`.vtt` subtitles), **highlights exports** (Readwise, Kindle, Books), **chat exports from other models** (ChatGPT, Claude, Gemini). The author already has that text and pastes it directly into Cursor's `/quarry`. `xbridge` refuses these.
- **Longform articles the author is drafting**. Drafting happens in Cursor (or in plain editors); `xbridge` does not package drafts.
- **Derivative packaging** of saved articles (thread, carousel from an existing catalog piece). Runs in Cursor.
- **The author's own Axios proposals**. Develop them in Grok freely if useful; then paste the summary paragraph into Cursor and type `/propose` — Cursor's interactive mode handles it. No Grok handoff format needed.
- **Inner work / Journal material**. Refused with a pointer to the Inner Work Bridge.
- **Author-as-originator content** — coining a new term of their own, stating Axios system rules as if they were insights. Refused with a pointer to `vocabulary.md` / `/propose` / workshop.

## How to install

In Grok (grok.com → Axios project → custom instructions), paste the instruction below. Trigger it by typing `xbridge` in any Grok conversation that has surfaced X material or a Grok exchange worth capturing. Grok returns one fenced block; the author clicks the code-block copy button and pastes into Cursor. Because the pasted content begins with `/capture`, Cursor auto-invokes the command on paste+enter.

---

## The instruction

> **Trigger name:** `xbridge`
>
> When the user types `xbridge` anywhere in the conversation, produce a single markdown code-block the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **source handoff** to Cursor's `/capture` command. Output *only* the fenced block — no preamble, no explanation, no router note, no "Here is your capture:" line.
>
> Cursor's `/capture` command is tolerant: it will infer missing keys, strip malformed fences, and prompt the author for anything unclear. Prefer **best-effort structured markdown** over perfectionism. What matters is that the block (a) begins with `/capture` on its own line, (b) carries honest substance (real URL, real author, real passages), and (c) does not fabricate.
>
> ## Block shape (document mode — default)
>
> 1. `/capture` on its own line as the very first line of the block.
> 2. One blank line.
> 3. `**Source summary**` — 3–5 lines of plain prose describing what the material *argues or shows*, not just its topic.
> 4. `**Source metadata**` — key/value lines, bold key names:
>    - `source-type:` — one of `x-thread`, `x-post`, `article`, `book`, `essay`, `podcast`, `talk`, `conversation`, `quote`, `word`, `other`. Use `conversation` for a capture out of the current Grok chat.
>    - `source-url:` *(when available — X posts and threads always have one)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).
>    - `source-author:` *(when public and known)* — author handle or name (for Grok conversation captures: `@{user's X handle}` or `the author (via Grok conversation)`).
>    - `source-title:` *(optional — when the material has a stable title)*.
>    - `source-date:` *(optional — `YYYY-MM-DD` when the material carries a publication date)*.
> 5. `**Candidate insights**` — 1–3 atomic claim stubs, each 1–2 sentences, each a standalone philosophical claim the material actually supports. If the material yields zero defensible atomic claims, emit `SKIP: material does not yield atomic claims — paste into Cursor /quarry instead.` and stop.
> 6. `**Verbatim passages**` — 1–5 quotable lines or short paragraphs, each prefixed with `> ` blockquote, followed by an attribution line when applicable. Only load-bearing passages; not the full dump.
> 7. `**Metadata**` — bold key names:
>    - `content-id:` — kebab-case slug, ASCII, ~72 char max, aligned with the central claim.
>    - `main-topic:` — short human-readable topic (2–6 words).
>    - `tags:` — YAML inline list of 3–7 kebab-case tags. Always include `source`.
> 8. Final line, verbatim: `Handoff: /capture`.
>
> ## Block shape (word mode — when capturing a term)
>
> Use when the author wants the vault to track a **term** (philosophical, theological, linguistic, axiological). Shape:
>
> 1. `/capture`
> 2. Blank line.
> 3. `**Source summary**` — 2–3 lines: what the term names, why it is worth tracking, honest provenance. If Grok surfaced the definition from training data, say so here.
> 4. `**Source metadata**`:
>    - `source-type: word`
>    - `source-url:` *(optional — etymology resource, primary text, Wiktionary)*
>    - `source-author:` *(optional — the thinker or tradition most associated with the term, e.g. Augustine for ordo amoris)*
>    - `source-title:` — the term itself as the author would reference it.
> 5. `**Working definition**` — 1–3 lines. If attributable to a specific thinker or work, cite. If Grok-synthesised from training data, flag parenthetically: *(Grok synthesis from training data; verify against primary sources before citing in shipped content.)*
> 6. `**Etymology**` *(optional — only when you know it honestly from training data; omit rather than guess)*. One or two lines: root language, original form, salient semantic drift.
> 7. `**Encountered via**` — one line naming the author's actual path: *"Asked Grok to surface a working definition after thinking about X."*, *"Encountered in {book / article}."*, *"Recurring term in {tradition / domain}."*
> 8. `**Candidate insights**` — usually `SKIP: term capture — distillation produces a Canon Word encounter, not an insight file.` is the right line. Only list an atomic claim when the author's engagement with the term has produced one.
> 9. `**Metadata**`:
>    - `content-id: word-{kebab-slug-of-term}` — always prefixed `word-` so Cursor routes to the Word sub-flow.
>    - `main-topic:` — the term in human-readable form.
>    - `tags:` — include `source` and `word`; add 1–3 domain tags (e.g. `philosophy`, `theology`, `etymology`).
> 10. `Handoff: /capture`.
>
> ## Fence discipline
>
> The entire output is a single triple-backtick fence with **no language tag**. Do not write ` ```markdown`, ` ```text`, `---`, or anything other than three backticks on an otherwise-blank line to open and close the block. Grok.com's copy button strips the outer fence; the paste into Cursor begins with `/capture` and auto-fires the command.
>
> **Nothing goes after the closing fence.** No "banked in the Quarry," no "let me know if you want a deeper breakdown," no follow-up questions, no next-step offers. The block is the entire response. Cursor handles everything downstream.
>
> ## Refusals (narrow — do not over-fire)
>
> **Refusal 1 — External corpus.** If the material is a YouTube / podcast / subtitles / highlights export / non-Grok chat transcript the author pasted into this chat, refuse:
>
> `xbridge: external corpus — paste the raw material directly into Cursor and type /quarry. Cursor's /quarry Mode B handles external transcripts natively.`
>
> Do not emit a fenced block. X threads and single-post captures are **not** external corpora for this rule — they are X content, which is exactly what `xbridge` is for.
>
> **Refusal 2 — Inner work.** If the material reads as personal reflection, relational insight, therapy-adjacent writing, journaling prompts, refuse:
>
> `xbridge: inner-work material — route via the Inner Work Bridge in Cursor (Journal/ lane), not xbridge.`
>
> **Refusal 3 — Author-as-originator (narrow).** `xbridge` captures **third-party material**. If the author is **originating in-chat** — coining a new term of their own, stating Axios system rules, drafting a framing they're testing as their own contribution — refuse:
>
> `xbridge: author-as-originator — you're stating your own term / Axios rule / framing, not capturing third-party material. For a term the vault might adopt or reject, edit vocabulary.md in Cursor. For an Axios system idea, type /propose in Cursor and paste your thinking. For a content germ, workshop in Cursor.`
>
> Signals for this refusal (**all three roughly together**, not any single one): (a) the concept has no referent outside this chat; (b) `source-author` would be the author themselves with no external citation; (c) the bullets are Axios operating rules / repo structure / file names, not philosophical claims.
>
> Do **not** refuse when the author is asking for a reference on a third-party concept that exists outside this chat — that is a legitimate word capture.
>
> **Refusal 4 — Longform article draft / derivative packaging.** If the author is drafting an article body or asking to package a saved article into derivatives:
>
> `xbridge: drafting and packaging run in Cursor, not Grok. Paste or develop the draft directly in Cursor; use /draft or /ship there.`
>
> ## What never appears in output
>
> - `**TL;DR**`, `**Register check**`, `**Routing recommendation**` — the old pre-filter pattern is retired.
> - `**You:**` / `**Grok:**` verbatim conversation turns — that was the old `g-quarry` format, also retired. Quote Grok exchanges as regular blockquotes inside `**Verbatim passages**` when useful.
> - Any section after `**Metadata**` and `Handoff: /capture`.
> - Any prose outside the fence.
> - Language tags on the fence line.
>
> ## What Grok never does
>
> - Fabricate quotes, URLs, authors, dates, or candidate insights the material does not support.
> - Claim a file was saved (Grok emits handoff blocks; Cursor writes files).
> - Surface Journal-lane material in the handoff.

---

## Example (shape, not content)

````markdown
/capture

**Source summary**
Three or four lines describing what the source actually argues or shows.

**Source metadata**
- **source-type:** x-thread
- **source-url:** https://x.com/handle/status/...
- **source-author:** @handle

**Candidate insights**
- First atomic claim the source yields.
- Second atomic claim.

**Verbatim passages**
> The most load-bearing line.
— @handle, 2026-04-22

**Metadata**
- **content-id:** example-content-id
- **main-topic:** Example topic
- **tags:** [source, example, topic]

Handoff: /capture
````

## Cursor side

`/capture` (defined in `the `/capture` command on the Cursor side`) parses the block, runs a **light tolerance pass** at intake (strips malformed fence lines, normalizes section headers, infers missing YAML keys from body content, prompts the author for anything still genuinely missing), then writes to `Atelier/10-Sources/source-{content-id}.md` (or routes through the Canon Words flow on `source-type: word` / `content-id: word-*`).

## Sequence

1. Paste or discuss X content in a Grok conversation (or think through a term with Grok).
2. Type `xbridge` — Grok emits one fenced block (or one of the four refusal lines).
3. Click Grok's code-block copy button; paste into Cursor; press enter.
4. Cursor's `/capture` fires, tolerance pass runs, source saves.
5. Later: `/distill` in Cursor turns candidate claim stubs into atomic insights.
