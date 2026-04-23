---
date: 2026-04-23
type: reference
tags: [grok, capture, source, trigger, instruction]
---

# `g-capture` — Grok-side trigger for Atelier source handoff

The Grok custom instruction the author triggers after reviewing a **single deliberate piece of raw material** in a Grok conversation — one X thread, one article, one quote with attribution, one clean exchange, one book passage. Grok produces a single Cursor-ready markdown block, **beginning with `/capture`**, that saves the material directly to `Atelier/10-Sources/` with 1–3 candidate insight stubs already attached, **skipping Quarry**.

Pairs with the Cursor command `/capture`. Previously named `q-capture`. This is the **pre-filter lane** — Grok reads the raw material, pulls 1–3 atomic claim stubs, and hands the material to Cursor already shaped for distillation. Distinct from [`g-quarry`](g-quarry.md) (raw corpora dump for later mining) and from [`g-draft`](g-draft.md) / [`g-ship`](g-ship.md) (article drafting / packaging).

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
>
> **Author-as-originator refusal (narrow — not the same as author-as-requester).** `g-capture` captures **third-party material** you will later distil. There is one shape of input it must refuse: content the author is **originating in-chat** — coining a new term of their own, stating Axios system rules, drafting a framing they're testing as their own contribution. Refuse with:
>
> `g-capture: author-as-originator — you're stating your own term / Axios rule / framing, not capturing third-party material. For a term you want the vault to adopt or reject, edit Atelier/00-System/vocabulary.md (ask Cursor). For an Axios change idea, use g-propose. For a content germ, workshop in Cursor.`
>
> Do **not** refuse when the author is **requesting a reference on a third-party concept** that exists outside this chat (a philosophical term, theological term, linguistic root, historical concept). That is a legitimate capture — use **word mode** below.
>
> Signals that trigger author-as-originator refusal (**all three roughly together**, not any one alone): (a) the concept has no referent outside this chat — it lives only because the author just defined it; (b) `source-author` would be the author themselves with no external citation; (c) the bullets you would emit are Axios operating rules, public-repo read order, or file / folder names — not philosophical claims that could become `20-Insights/` files. One of those signals in isolation (e.g. no URL) is **not** grounds to refuse — many legitimate captures lack URLs.
>
> **Candidate insights — what belongs.** Each bullet must be a **standalone philosophical claim** the raw material supports — the kind of sentence that could become one insight's declarative opening after distillation. It must **not** be: repo structure, public-doc read order, Grok/Cursor operating rules, marketing of the system, or a restatement of `quarry-guidelines.md` / `README.md` content. If the only "insights" left after that filter are meta-system bullets, the chat is not a `g-capture` job — use the refusal line above. For word-mode captures (see next section), `SKIP: term capture — distillation produces a Canon Word encounter, not an insight file.` is a valid and usually honest substitute.
>
> **No extra sections.** The handoff ends at `**Metadata**` and `Handoff: /capture`. Do **not** append "Routing recommendation", "Non-negotiables", or other sections — they tempt you to launder system meta into a capture block and confuse Cursor.
>
> **`source-type` must be from the allowed set only.** Use exactly one of: `x-thread`, `x-post`, `article`, `book`, `essay`, `podcast`, `talk`, `conversation`, `documentary`, `quote`, `word`, `other` (map close variants yourself — e.g. `video` → `talk` or `podcast`). Never invent a free-text type like "user-provided definition".
>
> ---
>
> **Word mode (term capture).** When the author wants to track a **term / concept** that exists in the wider world rather than a specific document, use `g-capture` with `source-type: word`. This covers three common shapes:
>
> - **Term encountered in reading or conversation** — *"I met 'ordo amoris' in Augustine today — capture it."* Provenance: the author's actual encounter.
> - **Term recurring in a tradition** — *"'Hesed' keeps turning up in theology; capture it so the vault tracks it."* Provenance: the tradition itself; the author may not have one specific text.
> - **Grok-as-reference-book** — *"I've been thinking about 'timeless principles'; give me a working definition so I can track the term."* Provenance: **honest about the chat itself** — the "Encountered via" line must say so (e.g. `Asked Grok to surface a working definition after considering X.`). A Grok-synthesised definition is a working aid, not an authoritative source; flag it as such.
>
> Word-mode handoff block has a slightly different shape from a document capture:
>
> 1. `/capture` on its own line (as always).
> 2. Blank line.
> 3. `**Source summary**` — 2–3 lines: what the term names, why it is worth tracking, honest provenance (where the author encountered it, or *that Grok is acting as reference book*). No marketing language.
> 4. `**Source metadata**`:
>    - `source-type: word`
>    - `source-url:` *(optional — link to an etymology resource, a primary text, or Wiktionary if useful; omit if none)*
>    - `source-author:` *(optional — the thinker or tradition most associated with the term, e.g. "Augustine" for *ordo amoris*; omit if diffuse)*
>    - `source-title:` — the term itself as the author would reference it (e.g. "ordo amoris", "timeless principles")
> 5. `**Working definition**` — 1–3 lines. If attributable to a specific thinker or work, cite it. If Grok-synthesised from training data, end the block with a parenthetical flag: *(Grok synthesis from training data; verify against primary sources before citing in shipped content.)*
> 6. `**Etymology**` *(optional — include only when Grok knows it honestly from training data; omit rather than guess)*. One or two lines: root language, original form, salient semantic drift. Do not invent.
> 7. `**Encountered via**` — one line naming the author's actual encounter path. Honest options include: *"Asked Grok to surface a working definition after thinking about X."*, *"Encountered in {book / talk / article / thread}."*, *"Recurring term in {tradition / domain}."*
> 8. `**Candidate insights**` — usually `SKIP: term capture — distillation produces a Canon Word encounter, not an insight file.` is the right line. Only list an atomic claim when the author's engagement with the term has actually produced one (e.g. *"'Timeless principles' overlaps with Truth-over-Time but is broader and less load-bearing; prefer the sharper term."*).
> 9. `**Metadata**`:
>    - `content-id: word-{kebab-slug-of-term}` — always prefixed `word-` so Cursor routes to the Word sub-flow.
>    - `main-topic:` — the term, in human-readable form.
>    - `tags:` — include `source` and `word`; add 1–3 domain tags (e.g. `philosophy`, `theology`, `etymology`).
> 10. `Handoff: /capture`.
>
> Word-mode example (shape, not content):
>
> ````markdown
> /capture
>
> **Source summary**
> A Latin-via-Greek Augustinian term naming "ordered love" — the disciplined proportion in which a person loves lesser and greater goods. Worth tracking because it recurs in Christian-formed moral reasoning and carries weight the modern word "priorities" cannot.
>
> **Source metadata**
> - **source-type:** word
> - **source-url:**
> - **source-author:** Augustine
> - **source-title:** ordo amoris
>
> **Working definition**
> The right ordering of loves — each good loved in proportion to its worth, neither under- nor over-prized. Augustine, *De Doctrina Christiana* I.27: *"That man lives in justice and sanctity who is an unprejudiced assessor of the intrinsic value of things."*
>
> **Etymology**
> Latin *ordo* (order, arrangement) + *amoris* (genitive of *amor*, love). Classical and patristic moral vocabulary; distinct from modern "priorities" in carrying moral weight, not merely optimisation weight.
>
> **Encountered via**
> Recurring term in Christian-formed moral writing; captured today after a re-reading of Augustine surfaced it as worth tracking.
>
> **Candidate insights**
> SKIP: term capture — distillation produces a Canon Word encounter, not an insight file.
>
> **Metadata**
> - **content-id:** word-ordo-amoris
> - **main-topic:** ordo amoris
> - **tags:** [source, word, philosophy, theology, ethics, augustine]
>
> Handoff: /capture
> ````

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

The command prints the saved path and offers the standard next-step choice (`/distill` or bank).

Do **not** include Voice Intent, Claim-risk, Carry band, or Article draft sections in a `g-capture` output. Those belong to downstream lanes (`g-draft`, `g-ship`) run only after the source has been distilled into insights and a draft has emerged.

## Sequence

1. Paste or point Grok at a single piece of raw material in a conversation.
2. Ask Grok for a pre-filter pass (TL;DR, candidate insights, register check, routing recommendation).
3. Confirm `g-capture` is the right lane.
4. Type `g-capture` — Grok emits the block.
5. Click copy, paste into Cursor, press enter. `/capture` fires on its own.
6. Later: `/distill` turns the candidate claim stubs into atomic insights in `Atelier/20-Insights/`.
