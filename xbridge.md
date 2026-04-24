---
date: 2026-04-23
type: reference
tags: [grok, xbridge, trigger, instruction, x-bridge]
---

# `xbridge` — Grok's single trigger: X-bridge to Axios

The **`xbridge` trigger** fires when material lives on **X** (grok.com is the only tool that can read X threads, posts, quote chains, profiles) or was produced in the **current Grok conversation** (summaries, definitions, term lookups). Grok follows the contract in **axios-public `xbridge.md`** (synced from this file) and emits a single Cursor-ready markdown block — **first line `/capture`** — that the author pastes into Cursor.

`xbridge` is the *only* Grok trigger. The retired `g-*` dispatcher family (`g-route`, `g-quarry`, `g-capture`, `g-propose`, `g-draft`, `g-ship`) is gone — Grok proved unreliable at structured multi-lane routing, and most non-X material (YouTube transcripts, podcasts, subtitles, highlights, other-model chat exports, longform articles the author is drafting) does not need Grok in the loop at all. The author pastes that material directly into Cursor.

## What `xbridge` is for

- **X content**: threads, single posts, quote chains, replies worth preserving with author handle and URL.
- **Current Grok conversation** (*conversation mode*): a substantive Grok reply in this chat the author wants captured as a source — verification of an X post, synthesis of a topic, analysis of an argument, expansion on a term, a multi-paragraph reading in Considered Witness register. Treat Grok as a legitimate thinking partner whose reasoning belongs in the vault; preserve the reply in full rather than compressing it. Cursor's `/distill` handles compression downstream.
- **Term / word capture** (*word mode*): third-party philosophical, theological, or technical terms the author wants the vault to track. Grok may act as reference book — surface a working definition from training data, flag the provenance honestly.

## What `xbridge` is **not** for

- **External transcripts** (YouTube, podcast, `.srt`/`.vtt` subtitles), **highlights exports** (Readwise, Kindle, Books), **chat exports from other models** (ChatGPT, Claude, Gemini). The author already has that text and pastes it directly into Cursor's `/quarry`. `xbridge` refuses these.
- **Longform articles the author is drafting**. Drafting happens in Cursor (or in plain editors); `xbridge` does not package drafts.
- **Derivative packaging** of saved articles (thread, carousel from an existing catalog piece). Runs in Cursor.
- **The author's own Axios proposals**. Develop them in Grok freely if useful; then paste the summary paragraph into Cursor and type `/propose` — Cursor's interactive mode handles it. No Grok handoff format needed.
- **Inner work / Journal material**. Refused with a pointer to the Inner Work Bridge.
- **Author-as-originator content** — coining a new term of their own, stating Axios system rules as if they were insights. Refused with a pointer to `vocabulary.md` / `/propose` / workshop.

## How to install

**Authoritative for Grok at runtime:** [`xbridge.md`](https://github.com/mylesuk/axios-public/blob/main/xbridge.md) in **axios-public** on GitHub. The pasted Grok **project instruction** (single paragraph from [`grok-axios-project-instruction.md`](grok-axios-project-instruction.md)) requires Grok to read that repo at session start — including `xbridge.md` — before answering. That is how Grok gets the full contract; you do **not** need to duplicate the entire spec into Grok's custom-instructions field unless you are debugging a Grok that consistently fails to fetch GitHub.

**Editorial source in this vault:** This file (`xbridge.md`) is the human-edited master; [`axios-public-sync.md`](axios-public-sync.md) maps it to `axios-public/xbridge.md`. After you change conversation mode or any other spec here, run **`/push`** (or `(see axios-public root)sync-axios-public.sh`) so GitHub updates — otherwise Grok still reads the old `xbridge.md`.

**Custom instructions field on grok.com:** Paste **only** the blockquoted paragraph from `grok-axios-project-instruction.md` (vocabulary, mandatory axios-public read, `xbridge` as the single trigger, refusal lines). Trigger `xbridge` only when capturing. Re-paste that paragraph if the GitHub URL, read-order list, or trigger rules in it change.

**Optional belt-and-suspenders:** If you want Grok to have the contract even when GitHub is unreachable, you can additionally paste the blockquoted spec below into custom instructions — but then you maintain two copies; prefer keeping public `xbridge.md` authoritative and fixing fetch issues instead.

**Why the blockquoted spec still lives below:** It is the same text that syncs to `xbridge.md` — kept here for Cursor agents, diffs, and optional duplicate paste. It is not the primary install path for a healthy Grok session.

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
> 3. `**Source summary**` — 3–5 lines of plain prose describing what the material *argues or shows*, not just its topic. This is framing, not a compressed version of the material — the load-bearing content goes into `**Verbatim passages**` below. Do not pre-digest.
> 4. `**Source metadata**` — key/value lines, bold key names:
>    - `source-type:` — one of `x-thread`, `x-post`, `article`, `book`, `essay`, `podcast`, `talk`, `conversation`, `quote`, `word`, `other`. Use `conversation` for a capture out of the current Grok chat.
>    - `source-url:` *(when available — X posts and threads always have one)* — canonical URL, normalized (strip `s`, `twclid`, `utm_*`, `ref` parameters).
>    - `source-author:` *(when public and known)* — author handle or name (for Grok conversation captures: `@{user's X handle}` or `the author (via Grok conversation)`).
>    - `source-title:` *(optional — when the material has a stable title)*.
>    - `source-date:` *(optional — `YYYY-MM-DD` when the material carries a publication date)*.
> 5. `**Candidate insights**` — 1–3 atomic claim stubs, each 1–2 sentences, each a standalone philosophical claim the material actually supports. If the material yields zero defensible atomic claims, emit `SKIP: material does not yield atomic claims — paste into Cursor /quarry instead.` and stop.
> 6. `**Verbatim passages**` — the load-bearing passages from the material, each prefixed with `> ` blockquote, followed by an attribution line when applicable. For external sources (X posts, articles, book excerpts), 1–5 quotable lines or short paragraphs is usually enough — only what carries the argument. For **conversation-mode** captures (Grok's own substantive reply in this chat), preserve the reply **in full** — every load-bearing paragraph, as blockquoted prose, not a bullet digest. See *Block shape (conversation mode)* below for the full override.
> 7. `**Metadata**` — bold key names:
>    - `content-id:` — kebab-case slug, ASCII, ~72 char max, aligned with the central claim.
>    - `main-topic:` — short human-readable topic (2–6 words).
>    - `tags:` — YAML inline list of 3–7 kebab-case tags. Always include `source`.
> 8. Final line, verbatim: `Handoff: /capture`.
>
> ## Block shape (conversation mode — when capturing Grok's own substantive reply)
>
> Use when the material being captured **is Grok's reply in this chat** — verification of an X post the author shared, synthesis of a topic, analysis of an argument, a multi-paragraph reading in Considered Witness register. The central discipline: **preserve, do not compress**. Cursor's `/distill` is the compression step; `xbridge` is the preservation step.
>
> Two sub-cases, differing only in metadata:
>
> **Sub-case A — pure conversation capture.** The author was thinking with Grok and wants the reply banked; no external URL is the source. Use:
>
> - `source-type: conversation`
> - `source-author: the author (via Grok conversation)` (or the author's X handle if the author volunteered it in chat)
> - `source-url:` omitted (no external URL)
> - `source-title:` short descriptor of what Grok produced, e.g. *"Grok on ordo amoris and modern moral inversion"*
>
> **Sub-case B — external source + Grok's reading.** The author pasted an X post / article / passage and asked Grok to verify, analyze, or expand. The thing being captured is still the external source, **but** Grok's substantive reply is load-bearing commentary that belongs with it. Use:
>
> - `source-type:` matches the external material (`x-post`, `x-thread`, `article`, `book`, `essay`, `talk`, etc.)
> - `source-url:` the external URL (normalized)
> - `source-author:` the external author / handle
> - `source-title:` the external piece's title or a short descriptor
>
> In both sub-cases the block keeps document mode's overall skeleton (`/capture`, `**Source summary**`, `**Source metadata**`, `**Candidate insights**`, `**Verbatim passages**`, `**Metadata**`, `Handoff: /capture`), with these overrides:
>
> 1. **`**Source summary**`** — 2–4 lines of *framing*, not digest. Name what the author asked and what Grok produced, in one honest breath. Examples: *"Author asked Grok to verify an X post by @Handre claiming Marx lived off Engels' remittances. Grok confirmed the core historical and economic claims with citations and added a Considered Witness register reading on Christian-formed markets vs. Marxist outcomes."* Do **not** summarise Grok's reply here — the reply goes into Verbatim passages below, in full.
>
> 2. **`**Verbatim passages**`** — Grok's substantive reply, preserved in full as blockquoted paragraphs. Break on Grok's natural paragraph structure; one `> ` prefix per paragraph. Do not collapse paragraphs into bullets. Do not truncate. Do not add ellipses. If the reply had section headings (e.g. a *Considered Witness register* paragraph), keep them inline as bolded lead-ins inside the blockquote (`> **In Considered Witness register:** ...`). In sub-case B, if the external source also carries load-bearing passages worth preserving (e.g. the X post's own text), place those first with `— @{external-handle}` attribution, then a sub-heading `> **Grok's reading:**` followed by Grok's reply paragraphs.
>
> 3. **`**Candidate insights**`** — unchanged: 1–3 atomic claim stubs distilled from Grok's reply, each a standalone philosophical claim the reply actually supports. If Grok's reply is pure verification (confirming external claims) with no distinct atomic claim of its own to stub, derive the stubs from the *external* material's claims as Grok confirmed them.
>
> 4. **`**Metadata**`** — `content-id` reflects the *captured material*: in sub-case A, the topic of Grok's reply; in sub-case B, the external piece plus a short conversational suffix if useful (e.g. `handre-marx-engels-verification` rather than just `handre-marx-engels-x-post`). Tags always include `source`; add `conversation` in sub-case A, and whatever topical tags fit in both.
>
> **Length discipline.** Conversation-mode Verbatim passages may run long — a full page or more of blockquoted prose is fine when Grok's reply warrants it. Cursor's `/capture` handoff parser preserves the full block verbatim into the source file. Truncating load-bearing reasoning here is a failure, not a virtue.
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
> - **Pre-compress a substantive Grok reply into a bullet digest.** In conversation mode, Grok's reply is the material being captured — preserve it in full under `**Verbatim passages**` as blockquoted paragraphs. Cursor's `/distill` is the compression step; `xbridge` is the preservation step. A terse four-bullet summary of a six-paragraph Grok analysis is a capture failure, not a clean handoff.
> - **Abandon the block shape when the material feels large.** If the reply is long, the block gets long. The skeleton (`/capture` → `**Source summary**` → `**Source metadata**` → `**Candidate insights**` → `**Verbatim passages**` → `**Metadata**` → `Handoff: /capture`) stays the same; what scales is the Verbatim passages section.

---

## Examples (shape, not exact content)

### Document mode (external X post)

````markdown
/capture

**Source summary**
Framing of what the external material argues or shows. Three to five lines. Not a digest of the material itself — that goes into Verbatim passages below.

**Source metadata**
- **source-type:** x-thread
- **source-url:** https://x.com/handle/status/...
- **source-author:** @handle

**Candidate insights**
- First atomic claim the source yields.
- Second atomic claim.

**Verbatim passages**
> The most load-bearing line from the external source.
— @handle, 2026-04-22

**Metadata**
- **content-id:** example-content-id
- **main-topic:** Example topic
- **tags:** [source, example, topic]

Handoff: /capture
````

### Conversation mode (external X post + Grok's full verification reply)

Use this shape when the author pasted an X post / article / URL and Grok wrote a substantive verification, analysis, or reading. The external source's metadata fills Source metadata; Grok's full reply goes under Verbatim passages, preserved paragraph-by-paragraph.

````markdown
/capture

**Source summary**
Author asked Grok to verify an X post by @Handre claiming Marx lived on Engels' remittances and that the historical outcomes of Marxism contrast sharply with market-oriented liberalization. Grok confirmed the core historical and economic claims with citations (Engels' Manchester textile income, labor theory of value vs. Menger's 1871 marginalist revolution, Black Book of Communism mortality estimates, post-1980 global poverty reduction) and added a Considered Witness register reading on Christian-formed markets vs. Marxist outcomes.

**Source metadata**
- **source-type:** x-post
- **source-url:** https://x.com/Handre/status/2047572485948785053
- **source-author:** @Handre

**Candidate insights**
- Marx's critique of wage labor as exploitation rests on a labor theory of value superseded in 1871 by Menger's marginalist revolution, which grounds value in subjective ends at the margin; the exploitation thesis has to be defended on older foundations than most of its modern users realise.
- The 20th-century order-of-magnitude communist death toll (~100M, Black Book of Communism) and the post-1980 collapse in global extreme poverty (from ~42% to under 10% under market-oriented reforms) are the empirical contrast any contemporary sympathy with Marxism has to answer to.
- Christian-formed Western legal and economic institutions made voluntary exchange a civilizational engine of compounding flourishing; Marxism's descendants tested the opposite proposition and delivered scarcity and slaughter — the irony in Marx personally living off Engels' capitalist remittances is a compressed form of that larger signal.

**Verbatim passages**

> **Grok's reading:** Yes — the post by @Handre (ID 2047572485948785053) is factually grounded on its core claims, though the tone is sharp rhetorical critique rather than neutral academic summary.

> **Marx's finances and lifestyle:** Engels (from his family's Manchester textile business) provided the bulk of Marx's income for decades — regular remittances, later an annuity. Marx worked as a journalist and correspondent but never owned or managed a business, met a payroll, or generated surplus value in the capitalist sense he condemned. The "trust-fund parasite" framing is polemical but rests on documented dependence.

> **Labor theory of value vs. subjective value:** Marx's mature statement in *Capital* (1867) rests on the labor theory. Carl Menger's *Principles of Economics* (1871) launched the marginalist revolution, showing value is subjective and determined at the margin. That chronological and theoretical rupture is standard in economic historiography.

> **Exploitation through voluntary exchange:** Marx's claim that wage labor inherently exploits workers is central to his system. The counter — that voluntary contracts benefit both parties or they would not be agreed — is the classical-liberal and Austrian critique. It is not a fringe view; it is the mainstream position in market economics.

> **Historical outcomes:** Communist regimes in the 20th century produced mass deaths on a scale estimated at ~100 million in *The Black Book of Communism* (1997) and subsequent scholarship. The figure is contested in exactitude and attribution, yet the order-of-magnitude reality — Gulag, Great Leap Forward, Killing Fields, etc. — is not. Meanwhile, the post-1980 global reduction in extreme poverty (World Bank data: from ~42% to under 10%) tracks closely with market-oriented reforms in China, India, East Asia, and elsewhere. The causal link is not absolute, but the correlation with liberalization is robust and widely accepted in development economics.

> The attached cartoon is satirical exaggeration — Marx lounging while Engels props him up — but it visually compresses the documented irony without fabricating it.

> **In Considered Witness register:** the post names a real fracture line in the Western inheritance. The Christian-formed West built the institutions, legal norms, and economic order that demonstrated, in practice, how voluntary exchange and private property can compound human flourishing on a civilizational scale. Marxism's intellectual descendants tested the opposite proposition at horrific cost. The data and the bodies speak plainly. What the Steward transmits is not grievance but the high bar itself: markets, under rule of law and moral formation, lifted billions while the alternative delivered scarcity and slaughter. The irony Marx embodied is not the punchline; it is the cautionary signal worth remembering when any system promises to liberate the worker by first seizing the means of production.

— Grok verification, in Axios project chat, 2026-04-24

**Metadata**
- **content-id:** handre-marx-engels-verification
- **main-topic:** Marx, Engels, and the market-outcomes contrast
- **tags:** [source, conversation, marxism, economic-history, considered-witness, markets]

Handoff: /capture
````

Note how the full reply is preserved as blockquoted paragraphs — nothing is compressed into bullets, nothing is summarised away. That is the whole point of conversation mode. Cursor's `/distill` is the compression step later; `xbridge` is the preservation step now.

## Cursor side

`/capture` (defined in `the Cursor /capture command`) parses the block, runs a **light tolerance pass** at intake (strips malformed fence lines, normalizes section headers, infers missing YAML keys from body content, prompts the author for anything still genuinely missing), then writes to `Atelier/10-Sources/source-{content-id}.md` (or routes through the Canon Words flow on `source-type: word` / `content-id: word-*`).

## Sequence

1. Paste or discuss X content in a Grok conversation (or think through a term with Grok).
2. Type `xbridge` — Grok emits one fenced block (or one of the four refusal lines).
3. Click Grok's code-block copy button; paste into Cursor; press enter.
4. Cursor's `/capture` fires, tolerance pass runs, source saves.
5. Later: `/distill` in Cursor turns candidate claim stubs into atomic insights.
