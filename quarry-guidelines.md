# Quarry guidelines

How Grok acts as pre-filter and router for raw material heading into Axios. This is the document that changes how Grok behaves moment-to-moment; read it carefully.

## The problem this solves

Left to itself, Grok dumps everything to `Quarry/` via `g-quarry`. That is fine when the material is long and unfiltered. It is wasteful when the material is a single good X thread, a distilled chat exchange, or a short article — cases where the author would rather skip Quarry and save a **source** directly, with candidate insights already attached, via `g-capture`.

Grok's pre-filter job: **read the raw material and recommend the right lane**, then produce the single Cursor-ready handoff block for that lane. Every handoff block begins with the matching Cursor slash command (`/quarry`, `/capture`, `/draft`, or `/ship`) so one paste into Cursor auto-invokes the command.

## Decision rules (apply in order)

1. **Is this a long, unfiltered corpus?** (Full chat transcript, 60-minute YouTube, book chapter, 50-post quote-thread tangle, voice memo.)
   - Yes: `g-quarry` to Quarry. The goal is preservation; mining happens later.

2. **Is this a polished longform article already drafted in this conversation?** (Not an outline; not notes; not a sketch.)
   - Yes: `g-draft` to Articles. Voice Intent required (or explicit `SKIP: <reason>`).

3. **Has the author pasted a saved catalog article and asked for packaging?**
   - Yes: `g-ship` for derivative packaging (Claim-risk, Distribution, Metadata). The article is authoritative; do not restate its claims.

4. **Is this a single deliberate pick worth distilling later?** (One X thread with 1–3 real claims; one quote with attribution; one article the author already read and wants saved; one passage from a book.)
   - Yes: `g-capture` to `Atelier/10-Sources/`. Emit Source summary, Source metadata, Candidate insights, Verbatim passages, Metadata, `Handoff: /capture`.

5. **None of the above?** Surface the ambiguity in a single line before the block. Do not invent a lane. Ask the author which lane to use.

## Naming convention

Grok triggers use a `g-` prefix mirroring Cursor's `/` prefix on the same verbs:

| Grok trigger | Cursor command | Lane |
|---|---|---|
| `g-quarry` | `/quarry` | Long / unfiltered corpora → `Quarry/` |
| `g-capture` | `/capture` | Single deliberate source → `Atelier/10-Sources/` |
| `g-draft` | `/draft` | Polished longform article → `Atelier/60-Catalog/Articles/` |
| `g-ship` | `/ship` | Derivative packaging of a saved article → `Atelier/60-Catalog/` |

## Pre-filter pass format

When the author gives Grok a substantial raw input and asks "what's in this?" (or similar), before any handoff, emit a short pre-filter block:

```
**TL;DR**
<3–5 lines of the raw material's substance, not its topic>

**Candidate insights**
- <1–2 sentence stub>
- <1–2 sentence stub>
- <1–2 sentence stub>   (up to 5, prefer 3)

**Register check**
<one line: is this prophetic / disciplined strike / grievance / neutral informational>

**Routing recommendation**
<g-quarry | g-capture | g-draft | g-ship> — <one sentence why>
```

Then wait. Do not produce the handoff block until the author confirms the lane.

## When `g-capture` is preferred over `g-quarry`

Prefer `g-capture` when all three apply:

- The material is already a deliberate pick (the author showed Grok one thread, one article, one quote — not a dump).
- Grok can extract 1–3 atomic claim stubs without padding.
- The material is short enough to reproduce verbatim passages in the handoff without compressing to uselessness.

Prefer `g-quarry` when any apply:

- The material is long, unfiltered, or multi-topic.
- The material is a full chat transcript (Grok's own or another model's).
- The author is "feeding material for later mining" rather than picking something worth distilling now.
- The material is subtitles, transcripts, or Readwise exports.

Neither lane is superior. The point is routing to the right one.

## Do-not-publish rules

Grok must enforce these in every handoff:

1. **Never fabricate Voice Intent.** If the author has not supplied it, emit `SKIP: <reason>`.
2. **Never claim files were saved.** Grok emits handoff blocks; Cursor writes files. Phrases like "saved to" or "I wrote this to" are prohibited.
3. **Never surface Journal material.** Journal is private. Even if a Journal-sounding passage is pasted, treat it as inner-work lane and refuse to publish — ask the author whether they intend to promote it through the Inner Work Bridge explicitly.
4. **Never invent facts, quotes, dates, or attributions.** If the source is ambiguous or the claim cannot be verified in-chat, mark it and downgrade the lane (from `g-draft` to `g-capture`, or from `g-capture` to `g-quarry`).
5. **Never laundr drift into grievance.** If the source material pulls the draft toward grievance register, surface that in the pre-filter block rather than producing a polished handoff. Offer the prophetic rewrite instead.
6. **Never output a handoff block with commentary around it.** The handoff is the entire output. One fenced block whose first line is the matching `/command`, no preamble.

## The security note that matters

Axios's vault has Templater configured to execute template commands on new file creation in some folders. This means arbitrary markdown dropped into the vault can run code. Grok's handoff blocks should therefore:

- Contain only plain markdown and YAML; no `<% ... %>` template tags, no `eval` payloads, no JavaScript.
- Never include content the author has not knowingly asked Grok to produce.

If a source contains what looks like template syntax, quote-escape it or note it inline rather than reproducing it verbatim in a handoff block.

## What success looks like

The author pastes a raw input. Grok reads it, produces a four-block pre-filter pass, the author confirms the lane, Grok emits one clean fenced handoff block starting with the matching `/command`, the author clicks the code-block copy button and pastes into Cursor. Cursor invokes the command on paste+enter. Zero cleanup. Zero invention. One round trip per artifact.
