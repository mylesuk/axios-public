---
date: 2026-04-23
type: reference
tags: [grok, propose, proposal, trigger, instruction]
---

# `g-propose` — Grok-side trigger for Atelier proposal handoff

The Grok custom instruction the author triggers after **developing an original idea about how Axios should evolve** in a Grok conversation — a system-architecture change, a new product surface, a process change, or a documentation pass. Grok produces a single Cursor-ready markdown block, **beginning with `/propose`**, that saves the idea directly to `Atelier/00-System/proposals/` as `proposal-{content-id}.md` with `status: draft`.

Pairs with the Cursor command `/propose`. This is the **original-idea lane** — distinct from `g-capture` (external sources to distill) and from `g-draft` (drafts of pieces being published). Grok and the author develop the idea together in chat; one trigger at the end packages it cleanly for the vault.

## When to use `g-propose` vs alternatives

- **`g-propose`** when: the author has developed **their own** idea about Axios — how the system should evolve, a new product surface, a process change, or a documentation refactor. The idea is forward-looking thinking about the tool itself.
- **`g-capture`** when: the material is **someone else's** argument / thread / article / quote being captured as external source material for later distillation.
- **`g-quarry`** when: the material is a long, unfiltered corpus for later mining.
- **`g-draft`** when: a polished longform article body has been drafted in-chat.
- **`g-ship`** when: a saved catalog article exists and the author is packaging derivatives.

Routing rules are canonical in [axios-public/quarry-guidelines.md](../quarry-guidelines.md).

## How to install

In Grok (grok.com → Axios project / custom instructions), add the instruction below. Trigger it by typing `g-propose` after a conversation in which the author has developed an original proposal. Grok returns one fenced block; the author clicks the code-block copy button and pastes into Cursor. Because the pasted content begins with `/propose`, Cursor invokes the command on paste+enter.

---

## The instruction

> **Trigger name:** `g-propose`
>
> When the user types `g-propose` anywhere in the conversation, produce a single markdown code-block the user can copy wholesale and paste into Cursor. The block must be self-contained, Cursor-ready, and represent a **proposal handoff** to Cursor's `/propose` command. It must include, in this exact order:
>
> 1. `/propose` on its own line as the very first line of the block. This is the Cursor trigger; one paste fires the command.
>
> 2. One blank line.
>
> 3. A `**Proposal summary**` block — 3–6 lines of what the proposal **argues for** (not just its topic). Plain prose, no bullets. State the proposal clearly: what should change, why it is worth the cost. Preserve the author's voice; do not compress it into agent register.
>
> 4. A `**Scope**` line — exactly one of `system`, `product`, `process`, `documentation`. If the proposal fits more than one, pick the primary. If the proposal fits none of these, emit `SKIP: not a system/product/process/documentation proposal — this looks like <content germ | life idea | inner-work | external source>. Route via <suggested lane>.` and stop.
>
> 5. A `**Key elements**` block — 3–7 bullets, each 1–2 sentences, each naming a concrete piece the proposal would introduce or change. Not a manifesto; concrete elements.
>
> 6. An `**Open questions**` block — 2–5 bullets of honest unresolved questions. A proposal with zero open questions has not been thought through; if the conversation yielded none, surface at least two plausible ones based on the proposal's scope (e.g. *what breaks? who is affected? what must be true for this to be worth the cost?*).
>
> 7. A `**Metadata**` block, one key per line, bold key names:
>    - `content-id:` — kebab-case slug, ASCII, ~72 char max, aligned with the central claim of the proposal (e.g. `paideia-formation-path-and-llm-pluralism`, `sweep-cadence-trigger-based`, `canon-substructure-v2`).
>    - `main-topic:` — short human-readable topic (2–6 words).
>    - `tags:` — YAML inline list of 3–7 kebab-case tags. Always include `proposal`. Never fabricate tags; draw from the proposal's actual content.
>    - `relates-to:` *(optional, when the proposal clearly touches constitutional docs or Canon entries already named in the conversation)* — YAML inline list of wiki-links in the form `[[path/doc-name]]` (e.g. `[[00-System/ai-partnership]]`, `[[30-Canon/Words/canon-word-paideia]]`). Do **not** guess at paths; omit this key entirely if the conversation did not name specific docs.
>    - `why-now:` *(optional)* — one short paragraph on why the proposal earns attention now, not later. Include when the conversation established this explicitly.
>    - `revisit-when:` *(optional)* — bullet list of triggers under which the proposal becomes ripe (or obsolete). Include when the conversation established these explicitly.
>
> 8. As the very last line of the block, an explicit handoff line, verbatim:
>
>    `Handoff: /propose`
>
> The *only* output when `g-propose` is typed is the markdown block itself, wrapped in a triple-backtick fence so grok.com renders a click-to-copy button that strips the backticks. No preamble, no explanation, no "Here is your proposal:" line. Just the fenced block.
>
> If the conversation has not developed an actual proposal yet (only a question, a musing, a half-sentence), respond with a single line: `g-propose: no developed proposal in this chat — think through what you want to change, why now, and what breaks, then re-run g-propose.`
>
> Never fabricate scope, elements, open questions, or links. If the author has not stated a reason *why now*, omit the `why-now:` key rather than inventing one. If the author has not named related docs, omit `relates-to:` rather than guessing paths. Never invent Canon entries the vault may not contain.
>
> If the proposal is about **content** (an argument for a future article / thread), not about Axios-the-system, emit: `g-propose: this looks like a content germ, not a system proposal — route as a 50-Workshop/ draft stub instead.` and stop.
>
> If the proposal is about **inner work** or personal/relational life (not about the Axios tool), emit: `g-propose: this looks like inner-work or a life idea, not a system proposal — Journal lane, not Atelier proposals.` and stop.
>
> If the material is someone **else's** argument being captured (external source), emit: `g-propose: this looks like external material to capture, not an original proposal — re-run as g-capture.` and stop.

---

## Example emitted block (shape, not content)

````markdown
/propose

**Proposal summary**
Two or three paragraphs in the author's voice stating what the proposal argues for, why it is worth the cost, and what it would change. State the claim directly.

**Scope**
system

**Key elements**
- First concrete element the proposal would introduce or change.
- Second concrete element.
- Third concrete element.

**Open questions**
- An honest unresolved question.
- Another honest unresolved question.

**Metadata**
- **content-id:** example-proposal-id
- **main-topic:** Example proposal topic
- **tags:** [proposal, system, example]
- **relates-to:** [[00-System/ai-partnership]], [[00-System/persona]]
- **why-now:** Short paragraph on why the proposal earns attention now, not later.
- **revisit-when:**
  - After N pieces shipped under current frame.
  - When model X stabilises at Y quality.

Handoff: /propose
````

When the author clicks Grok's copy button, the paste that lands in Cursor starts with `/propose` and fires the command.

## Cursor side

`/propose` (defined in `the `/propose` command on the Cursor side`) parses the block, resolves the save path as `Atelier/00-System/proposals/proposal-{content-id}.md`, and writes the file:

- YAML: `id`, `date` (today's local date), `type: proposal`, `status: draft`, `scope` (from handoff), `relates-to` (from handoff, omitted if absent), `tags` (`proposal` + merged tags from Metadata, deduplicated).
- Body: `Proposal summary` → *What it proposes*; `why-now` (if provided) → *Why now* (placeholder otherwise); `Key elements` → *Key elements*; `Open questions` → *Open questions*; *Impact if adopted* is always an author-fill placeholder (it is the piece Grok cannot honestly write without vault access); `revisit-when` (if provided) → *Revisit when* (placeholder otherwise).

The command prints the saved path and offers the standard next-step choice (leave at draft / fill *Impact if adopted* now / move to `reviewing`).

Do **not** include Voice Intent, Claim-risk, Carry band, candidate insights, or verbatim passages in a `g-propose` output. Those belong to other lanes (`g-capture`, `g-draft`, `g-ship`) and have no home in a proposal file.

## Sequence

1. Develop the idea in conversation with Grok. Think out loud, pressure-test, pull in relevant vault concepts.
2. When the proposal is clear enough to capture (even if incomplete), ask Grok for a quick honesty pass: is this a proposal about Axios-the-system, or has it drifted into a content germ / inner-work / external source? Confirm lane.
3. Type `g-propose` — Grok emits the block.
4. Click copy, paste into Cursor, press enter. `/propose` fires on its own and writes the file at `status: draft`.
5. Later (during `/sweep` or `/review`): pressure-test, fill in *Impact if adopted*, and either move to `reviewing`, `adopted` (with a decision-log entry), `deferred`, or `rejected`.
