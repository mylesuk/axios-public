---
date: 2026-04-23
type: reference
tags: [grok, route, router, trigger, instruction]
---

# `g-route` — Grok-side router: classify material, delegate to the right `g-*` lane

`g-route` is the **default entry point** for running material through Grok into Axios. The author pastes (or discusses) any material in a Grok conversation and types `g-route`. Grok classifies the material, surfaces a short **router note** naming the chosen lane and why, and then emits the full handoff block for that lane — exactly as if the author had typed the specific `g-*` trigger directly.

Pairs with no new Cursor command. The emitted fenced block still starts with `/quarry`, `/capture`, `/propose`, `/draft`, or `/ship` — whichever lane Grok chose — so one paste into Cursor still fires the right command. The router note is **outside the fenced block**, visible to the author in the Grok conversation but not pasted into Cursor. It is an audit trail for the classification choice, not part of the Cursor payload.

The specific `g-*` triggers remain fully supported. When the author already knows the lane, direct triggers are the faster path. `g-route` earns its keep when the lane is not obvious, when the author wants Grok to do the routing work it is meant to do, or when the material sits awkwardly between two lanes.

## When to use `g-route` vs a direct `g-*`

- **`g-route`** — the author has material but is not sure which lane, or wants Grok to act as router by default. Also the right move when material might be inner-work / vocabulary / a proposal / a content germ, and the author wants Grok to name that before any file is written.
- **Direct `g-*`** — the author has already decided the lane. Skip the router note; go straight to the handoff.

Both paths emit the same fenced block for a given lane. `g-route` adds a visible classification step; direct triggers skip it.

## How to install

In Grok (grok.com → Axios project / custom instructions), add the instruction below. Trigger it by typing `g-route` in any Grok conversation that has surfaced material the author wants routed. Grok returns a short router note (plain prose, no code fence) followed by — when confident — a fenced markdown block whose first line is the matching Cursor slash command. The author reads the router note, confirms the classification, clicks Grok's copy button on the fenced block, and pastes into Cursor.

---

## The instruction

> **Trigger name:** `g-route`
>
> When the user types `g-route` anywhere in the conversation, your job is to **classify the material in the chat and delegate to the correct `g-*` lane**. The authoritative routing rules live in [`quarry-guidelines.md`](../quarry-guidelines.md); follow them.
>
> The output has **two parts**, in this order:
>
> 1. A short **router note** in plain prose (no code fence), 2–4 lines, naming the chosen lane, the reason, and your confidence. Required shape:
>
>    ```text
>    **Routed to: g-<lane>**
>    - Why: <one sentence grounded in the material, naming the signal that decided the lane>
>    - Confidence: <high | medium | low>
>    ```
>
>    On low confidence, replace the fenced block with an **ambiguity note** (see below) and wait for the author.
>
> 2. One blank line.
>
> 3. The **full fenced handoff block** for the chosen lane, following the matching `g-*.md` spec **exactly** — same first line (`/quarry` / `/capture` / `/propose` / `/draft` / `/ship`), same required sections, same metadata, same closing `Handoff:` line where applicable. Do not compress, paraphrase, or omit required sections. Treat `g-route` as a dispatch layer on top of the existing contracts, not a new contract.
>
> The router note lives **outside the fence**. The fenced block is what the author pastes into Cursor; Cursor never sees the note. Both must be emitted (except in ambiguity / refusal cases) so the author has an audit trail for the routing choice.
>
> ## Classification procedure (apply in order, first match wins)
>
> 1. **Inner-work material** (journal-style reflection, therapy-adjacent, personal-relational, feelings-first writing). → **Refuse**. Emit the refusal note (see below). Do not emit a fenced block.
>
> 2. **Axios register / vocabulary / system-prose** (the author is restating Axios rules, coining a term for the vault, editing public-doc prose). → **Refuse**. Emit the refusal note pointing to `vocabulary.md` / `/propose` / Cursor workshop. Do not emit a fenced block.
>
> 3. **Polished longform article body drafted in this chat** (has a thesis, argumentative spine, ships as continuous prose). → **`g-draft`**. Emit the router note, then the full `g-draft` block.
>
> 4. **Saved catalog article pasted with YAML, author asking for packaging derivatives** (article body + frontmatter visible; the ask is thread / carousel / buff). → **`g-ship`**. Emit the router note, then the full `g-ship` block.
>
> 5. **Author's own original idea about how Axios itself should evolve** (system architecture, new product surface, process change, documentation refactor — forward thinking about the tool, not external material, not a content germ for a future piece). → **`g-propose`**. Emit the router note, then the full `g-propose` block.
>
> 6. **Single deliberate third-party pick worth distilling later** (one thread, one article, one quote with attribution, one book passage; 1–3 atomic claim stubs extractable without padding; short enough to preserve verbatim passages). → **`g-capture`** (document mode). Emit the router note, then the full `g-capture` document-mode block.
>
> 7. **Term / concept the author wants the vault to track** (the author encountered a term, or asked Grok to surface a working definition of a third-party concept). → **`g-capture` (word mode)**. Emit the router note (name that it is word mode), then the full `g-capture` word-mode block — including `source-type: word`, `content-id: word-{slug}`, `**Working definition**`, `**Encountered via**`, and `**Candidate insights**: SKIP: term capture...` where honest.
>
> 8. **Long, unfiltered, multi-topic corpus** (full chat transcript, 80-minute YouTube, long quote-thread tangle, voice-memo transcript, multi-topic conversation meant for later mining). → **`g-quarry`**. Emit the router note, then the full `g-quarry` block.
>
> If the material plausibly fits two lanes, pick the **lower-friction lane** and name the close call in the router note (`Confidence: medium. Alternative: g-<other-lane> if …`). The author reads the note and can override by re-running the specific trigger.
>
> ## Ambiguity note (low-confidence path)
>
> When no lane clearly dominates — usually a tie between `g-capture` and `g-quarry`, or a question whether the material is a content germ vs a proposal — **do not** emit a fenced block. Instead emit only a router note in this shape, then stop and wait for the author:
>
> ```text
> **Routed candidates: g-<lane-a> OR g-<lane-b>**
> - g-<lane-a> fits if: <the test that would confirm it>
> - g-<lane-b> fits if: <the test that would confirm it>
> - My read: g-<lane-a> — <one sentence>
> - Which lane do you want? (reply with g-<lane-a> or g-<lane-b>)
> ```
>
> Do not emit a handoff block until the author confirms.
>
> ## Refusal note (no Axios lane fits)
>
> When the material does not belong in Axios at all (inner-work, Axios register design, a life idea unrelated to the tool, or material that needs external processing first), emit a refusal note and stop. Required shape:
>
> ```text
> **g-route: no Axios lane fits.**
> - Why: <one sentence grounded in the material>
> - Suggested route: <Journal via Inner Work Bridge | edit Atelier/00-System/vocabulary.md in Cursor | workshop as a content germ in Cursor | Journal with #idea tag | Axios-Assets/Projects/ | develop the idea further in this chat before re-running g-route>
> ```
>
> Do not emit a fenced block on refusal. The router's job is to surface the mismatch, not to force-fit material.
>
> ## Discipline
>
> - **Do not fabricate.** If the material yields zero atomic claims, route to `g-quarry` (preservation) or refuse (inner-work). Do not pad `g-capture` with invented claims.
> - **Do not compress the delegated block.** Each `g-*` spec has required sections for a reason. `g-route` delegates to the full spec; it does not inline a shorter version.
> - **Do not invent Voice Intent.** On `g-draft`, if the author has not supplied Voice Intent, the delegated block emits `SKIP: voice-intent not supplied` as its own contract requires. `g-route` respects that.
> - **Do not claim files were saved.** `g-route`, like every `g-*`, produces blocks for Cursor to consume. Cursor writes files.
> - **Do not surface Journal material.** Refuse and route to the Inner Work Bridge.
> - **Classifier transparency beats tidiness.** The router note stays visible. The author audits the classification every time `g-route` fires.
>
> The *only* output when `g-route` is typed is (a) a router note, optionally followed by (b) a single fenced block. No preamble, no "Here is your routing:", no closing remarks.

---

## Example outputs (shape, not content)

### Confident route — `g-capture` document mode

```text
**Routed to: g-capture**
- Why: single X thread with three extractable atomic claims; author handle and URL visible.
- Confidence: high.
```

````markdown
/capture

**Source summary**
Three or four lines describing what the thread actually argues.

**Source metadata**
- **source-type:** x-thread
- **source-url:** https://x.com/...
- **source-author:** @handle

**Candidate insights**
- First atomic claim.
- Second atomic claim.
- Third atomic claim.

**Verbatim passages**
> The most load-bearing line.
— @handle, 2026-04-23

**Metadata**
- **content-id:** example-thread-claim
- **main-topic:** Example topic
- **tags:** [source, example, topic]

Handoff: /capture
````

### Ambiguity — `g-capture` vs `g-quarry`

```text
**Routed candidates: g-capture OR g-quarry**
- g-capture fits if: the single thread is what you want saved (three claims fit cleanly as candidate insights).
- g-quarry fits if: you want the whole multi-thread discussion preserved for later mining.
- My read: g-capture — the one thread is clearly the signal; the rest is commentary.
- Which lane do you want? (reply with g-capture or g-quarry)
```

No fenced block is emitted. The author replies with the chosen trigger; Grok emits the block on the next turn.

### Refusal — inner-work material

```text
**g-route: no Axios lane fits.**
- Why: this reads as personal reflection on a relational rupture — first-person, feelings-first, not an argument intended for any reader.
- Suggested route: Journal via the Inner Work Bridge in Cursor. If something shareable crystallises later, promote through the bridge ceremony, not through a public trigger.
```

No fenced block. The author takes the material to Journal.

### Refusal — Axios register / vocabulary

```text
**g-route: no Axios lane fits.**
- Why: this chat is about retiring a term from Axios's vocabulary — that is register design, not external material or a forward proposal.
- Suggested route: edit Atelier/00-System/vocabulary.md in Cursor directly. For a broader system change, use g-propose.
```

## Cursor side

Nothing new. `g-route` delegates to existing triggers (`g-quarry`, `g-capture`, `g-propose`, `g-draft`, `g-ship`). Each lane's Cursor command behaves exactly as documented in its own file; `g-route` is a Grok-side dispatcher, not a new command.

## Sequence

1. Paste or discuss material in a Grok conversation.
2. Type `g-route`.
3. Read the router note. If confident, Grok has already emitted the fenced handoff block — click copy, paste into Cursor, press enter.
4. If ambiguous, Grok has asked which lane to use. Reply with the specific `g-*` trigger.
5. If refused, take the material to the suggested external route (Journal via Inner Work Bridge, `vocabulary.md` edit, workshop in Cursor, etc.).

## Why this exists

The system can look intricate from outside because each `g-*` has lane-specific rules. In practice most material routes cleanly, but deciding-before-pasting shifts the routing burden onto the author when Grok is exactly the tool equipped to do it (long context, pattern recognition, lane rules loaded). `g-route` inverts the default: Grok classifies, the author confirms. Direct triggers stay available for the cases where the author already knows.

The router's only job is classification and delegation. It does not invent, summarise, or pad. Its transparency is non-negotiable: every `g-route` firing leaves an auditable trace of the decision — visible in the Grok conversation, separate from what Cursor sees.
