---
name: scaffold
description: >
  Set up a new Obsidian vault with the wiki system's folder structure,
  CLAUDE.md schema, and _map index. ALWAYS trigger when the user says
  "set up my vault", "scaffold my vault", "initialize wiki",
  "create vault structure", or wants to start using this system from scratch
  on a new or existing Obsidian vault. Also trigger when the user asks
  "how do I start?" or "what folders do I need?" in the context of
  organizing an Obsidian vault. Do NOT trigger for general Obsidian plugin
  recommendations, theme customization, or vault migration between devices.
metadata:
  version: "1.0.0"
---

# Scaffold — First-Time Vault Setup

Create the complete wiki structure on a new or existing Obsidian vault. Run this once.

**CLAUDE.md at the vault root is the authoritative schema.** This skill generates it. After scaffolding, always defer to CLAUDE.md if anything conflicts with skill instructions.

## What to create

```
Vault/
├── _inbox/          ← raw captures (user writes here)
├── _moc/            ← Maps of Content — connective essays, not flat lists
├── _map/
│   └── _map.md      ← index + operation log
├── 💡 ideas/        ← seeds, underdeveloped brainstorms
├── 🔍 analysis/     ← synthesis in progress, meeting debriefs, working thinking
├── 📚 reference/    ← settled, reusable knowledge (transcription only)
├── ✓ action/        ← goal-focused plans with timelines
├── Attachments/     ← images and binary assets
└── CLAUDE.md        ← entrypoint, conventions, workflows (authoritative)
```

## Steps

1. **Check existing state.** List top-level folders. Only create what's missing.
2. **Ask about domains.** If the vault already has notes, scan them to identify knowledge domains. If empty, ask the user what topics they'll track — or default to "personal knowledge across all domains."
3. **Generate CLAUDE.md** using the template in the references section below. Replace `[DOMAIN_DESCRIPTION]` with the identified domains.
4. **Generate `_map/_map.md`** with empty sections ready to be populated.
5. **Handle existing notes.** If notes exist outside `_inbox/`, do NOT move them. List what you found and ask the user how to handle them (ingest them? leave them?).
6. **Report.** Tell the user what was created and what to do next ("drop notes into _inbox/ and say 'process my inbox'").

## CLAUDE.md template

Generate this file at the vault root:

```markdown
# Vault Schema

This vault is a persistent, LLM-maintained wiki over [DOMAIN_DESCRIPTION]. Pattern: raw sources → synthesized wiki pages → connective MOCs. You read, the LLM writes.

## Three layers

**Raw sources** (immutable, you own)
- `_inbox/` — unprocessed captures (web clips, meeting notes, pasted transcripts)
- `Attachments/` — images and binary assets referenced by notes

**Wiki** (the LLM owns — creates, updates, cross-links)
- `💡 ideas/` — seeds and underdeveloped brainstorms
- `🔍 analysis/` — synthesis in progress, meeting debriefs, working thinking
- `📚 reference/` — settled, reusable knowledge (entities, frameworks, methodologies)
- `✓ action/` — goal-focused plans with timelines
- `_moc/` — 200–300 word connective essays naming relationships between domains

**Schema & index**
- `CLAUDE.md` (this file) — entrypoint, conventions, workflows. Authoritative.
- `_map/_map.md` — index + log. Read first every session to know what exists.

## Conventions

- **Filenames** carry the emoji prefix of their folder (`💡`, `🔍`, `📚`, `✓`, `🗺️`).
- **AI origin markers** (analysis notes only): `*` = AI-assisted (user wrote base, AI added content); `**` = AI-generated (AI created the synthesis). No marker = organic (user's own note). Marker goes after the folder emoji: `🔍 * Note Title` or `🔍 ** Note Title`.
- **Section-level AI markers**: within a `*` (AI-assisted) note, prefix every AI-added section with a `> 🤖` blockquote on the line immediately before the section header.
- **Frontmatter** on every wiki page: `tags:` (domain only, no type tags), `updated:` (YYYY-MM-DD). AI-marked notes also carry `origin: ai-assisted` or `origin: ai-generated`.
- **`📚 reference/` pages are transcription only.** No editorial inference, synthesis, or framing. If synthesis is worth capturing, create a separate `🔍 analysis/` page and link back.
- **`📚 reference/` atomization is allowed for token efficiency.** A single source may be split into multiple child pages if and only if: (1) the topics are distinct enough that they'd rarely be needed together in the same query, and (2) the split follows the source's own topic boundaries — not editorial judgment. The split must be exhaustive (no content lost) and each child page must link back to the parent/sibling pages.
- **Links are wikilinks** (`[[Page Name]]`) not markdown links, so Obsidian's graph view works.
- **Dates are absolute** (2026-04-20), never relative.
- **No `.bak` / `.backup` files.** Git is the version history.

## Operations

### Ingest — user drops something into `_inbox/`

> **HARD RULE: Only process files found inside `_inbox/`. Never process, merge, or delete files found at the vault root or any other folder. If a file exists outside `_inbox/`, stop and tell the user — do not touch it.**

1. Read the source end to end.
2. Classify and decide the destination folder based on the vault structure above.
3. Write the wiki page with correct frontmatter and wikilinks to related existing pages.
4. Update every related page touched by this source — entity pages, concept pages, relevant MOCs. A single source typically touches 5–15 pages.
5. If the source creates or reshapes a cross-domain relationship, update or create a `_moc/🗺️ …` essay.
6. Append an entry to `_map/_map.md` under today's date: `## [YYYY-MM-DD] ingest | <Source Title>` followed by a short list of pages touched.
7. Move the source out of `_inbox/` once integrated (or delete if raw capture isn't worth keeping).

### Query — user asks a question
1. Read `_map/_map.md` first to locate relevant pages. Do **not** grep raw sources unless the wiki can't answer.
2. Read the 3–8 most relevant wiki pages, follow wikilinks as needed.
3. Answer with citations as wikilinks.
4. **Good answers get filed back.** If the synthesis is non-trivial, offer to save it as a new `🔍 analysis/` page or update an existing one.

### Lint — user says "health check the wiki"
Report, don't silently fix. Look for:
- Contradictions between pages (two pages making incompatible claims)
- Stale claims superseded by newer sources
- Orphan pages (no inbound wikilinks)
- Concepts mentioned across multiple pages but lacking their own page
- Missing cross-references between pages that clearly relate
- `.bak` / `.backup` file accumulation

Present findings as a list; let the user pick what to act on.

## Token discipline

The wiki exists partly to save tokens. To keep that working:
- Read `_map/_map.md` before touching anything else — it's the router.
- Read wiki pages (small, dense) before raw sources (long, noisy).
- Keep wiki pages tight. A reference page is not a transcript — it's the distilled claim with a pointer back to the raw source.
- `CLAUDE.md` + `_map/_map.md` are stable across a session → prompt cache hits on repeat queries. Don't reshuffle them mid-session without reason.
```

## _map/_map.md template

```markdown
# Knowledge Map

**Updated**: [TODAY] · **Working notes**: 0 · **MOCs**: 0

Index of every wiki page with a one-line summary, plus the chronological log. Read this first. If the answer is here (or one hop away via wikilink), don't open raw sources.

---

## MOCs — `_moc/`

Connective essays naming relationships between domains.

(none yet)

## Ideas — `💡 ideas/`

Seeds. Underdeveloped but with identifiable topic and domain.

(none yet)

## Analysis — `🔍 analysis/`

Synthesis in progress: meeting debriefs, research digests, working thinking.

(none yet)

## Reference — `📚 reference/`

Settled, reusable knowledge. Update only when fundamentals change.

(none yet)

## Action — `✓ action/`

Goal-focused plans with timelines.

(none yet)

---

## Entities referenced but without profile pages

(none yet)

---

## Tags (MECE)

| Tag | Notes |
|---|---|
| (add as content arrives) | |

---

## Ingest Log
```
