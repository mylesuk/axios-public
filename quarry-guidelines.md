# Quarry guidelines

How material reaches Axios. This document names **what belongs where** — what Grok bridges, what the author brings directly to Cursor, and what is refused.

The architecture is deliberately small: Grok has one job (the X-bridge), Cursor does everything else. Earlier iterations delegated routing to Grok through a family of `g-*` triggers; that proved unreliable for structured multi-lane output, and most non-X material does not need Grok in the loop at all. The retired dispatcher is gone.

## Three lanes of raw material

| Shape of material | Where it goes | Why |
|---|---|---|
| **X content** — threads, posts, quote chains, profiles, replies | Grok → `xbridge` → paste into Cursor | Only grok.com can read X natively. Grok is the bridge. |
| **Current Grok conversation** — a summary or key exchange worth capturing as a source; a third-party term lookup (word mode) | Grok → `xbridge` → paste into Cursor | Grok is the conversation host; it can produce the handoff block cleanly. |
| **Everything else** — YouTube transcripts, podcast transcripts, `.srt`/`.vtt` subtitles, Readwise / Kindle / Books highlights exports, chat exports from other models, pasted article text, books, your own drafts, your own proposals for Axios | Paste directly into Cursor | The text is already in your hands. No pre-filter or URL fetch is needed. |

`xbridge` is the *only* Grok trigger. The author is the router for lane 3 — Cursor's commands (`/quarry`, `/capture`, `/distill`, `/draft`, `/ship`, `/propose`, `/reflect`, `/review`, `/sweep`, `/audit`, `/push`) cover the rest of the pipeline.

## Lane 1 + 2: what `xbridge` emits

See [`xbridge.md`](xbridge.md) for the full specification. Summary:

- Single triple-backtick fence, no language tag.
- First line: `/capture` (Cursor auto-fires the command on paste+enter).
- Body: `**Source summary**` / `**Source metadata**` / `**Candidate insights**` / `**Verbatim passages**` / `**Metadata**` in document mode; a variant with `**Working definition**` / `**Etymology**` / `**Encountered via**` in word mode.
- Final line: `Handoff: /capture`.
- Nothing before or after the fence.

Cursor's `/capture` is tolerant at intake — it strips malformed fence lines, normalizes section headers, infers missing YAML keys from body content, and prompts only for what is genuinely missing. Grok should prefer **best-effort structured markdown** over perfectionism, but must never fabricate.

## Lane 3: Cursor commands for direct input

| Material | Cursor command | Lands at |
|---|---|---|
| Long raw corpus (YouTube transcript, podcast, subtitles, highlights, another model's chat) | `/quarry` | `Quarry/YYYY-MM-DD-{slug}-{source}.md` (gitignored, flat) |
| Single deliberate passage worth distilling (a quote with attribution, a short article, a passage the author already read) | `/capture` | `Atelier/10-Sources/source-{slug}.md` |
| An existing source ready to become insight(s) | `/distill` | `Atelier/20-Insights/` + `Atelier/40-Themes/` |
| A longform article draft | `/draft` | `Atelier/60-Catalog/Articles/` |
| An already-saved article being packaged into thread + carousel | `/ship` | `Atelier/60-Catalog/{Threads,Carousels}/` |
| The author's own original idea about how Axios should evolve | `/propose` | `Atelier/00-System/proposals/proposal-{slug}.md` |
| A dialogic moment of self-clarification | `/reflect` | `Atelier/50-Workshop/Sessions/` |

Commands accept either interactive input (paste + dialogue) or the single `xbridge` handoff block (for lanes 1 and 2). No separate handoff format exists for the other Cursor commands — paste your own thinking directly and the command will shape it.

## Refusals (lane 1/2 only)

`xbridge` refuses four shapes of material with a short prose line (no fenced block):

1. **External corpus** — *"`xbridge`: external corpus — paste the raw material directly into Cursor and type `/quarry`."*
2. **Inner work** — *"`xbridge`: inner-work material — route via the Inner Work Bridge in Cursor."*
3. **Author-as-originator** (narrow — three signals must co-occur) — *"`xbridge`: author-as-originator — you're stating your own term / Axios rule / framing, not capturing third-party material."* Route: `vocabulary.md` / `/propose` / workshop.
4. **Drafting or derivative packaging** — *"`xbridge`: drafting and packaging run in Cursor, not Grok."*

Narrow refusal fires only when the signals combine; any single signal alone (e.g. no URL, empty `source-author`) is not grounds to refuse — many legitimate captures lack one or the other.

## Do-not-publish rules

These are enforced on every `xbridge` output:

1. **Never fabricate.** URLs, authors, dates, passages, candidate insights — honest substance or `SKIP: <reason>`.
2. **Never claim a file was saved.** Grok emits blocks; Cursor writes files.
3. **Never surface Journal material.** Inner-work material reaches Atelier only through the Inner Work Bridge ceremony inside Cursor.
4. **Never put prose outside the fence.** No router note, no pre-filter sections, no follow-up questions.
5. **Never launder drift into grievance.** Default register on civilizational topics is prophetic; refuse grievance.

## Template-safety note

Axios's vault has Templater configured to execute template commands on new file creation in some folders. Handoff blocks should contain only plain markdown and YAML — no `<% ... %>` template tags, no JavaScript, no `eval` payloads. If a source contains template-like syntax, quote-escape it or note it inline.

## What success looks like

The author pastes (or discusses) X content in Grok. Types `xbridge`. Grok emits one clean fenced block beginning with `/capture`. The author clicks the code-block copy button, pastes into Cursor, and Cursor's `/capture` fires — tolerance pass runs, source saves, distillation waits its turn. Zero cleanup. Zero invention. One round trip per artifact. For everything else, Cursor is already open — paste directly and type the right command.
