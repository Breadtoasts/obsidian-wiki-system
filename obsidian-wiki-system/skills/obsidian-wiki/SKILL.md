---
name: obsidian-wiki
description: >
  Turn any Obsidian vault into a structured, LLM-maintained wiki with inbox
  processing, querying, and health checks. ALWAYS trigger when the user says
  "process my inbox", "organize my notes", "run the vault", "process notes",
  "end of day processing", "clean up my inbox", or any reference to processing,
  sorting, tagging, or organizing Obsidian notes. Also trigger for "run the map",
  "update my knowledge map", "what are my gaps", "show my gray matter",
  "check my plans", "probe my stubs", "weekly review", or any reference to
  reviewing inactive plans or underdeveloped notes. Also trigger when the user
  says "set up my vault", "scaffold my vault", "initialize wiki", or wants to
  start using this system from scratch. Also trigger for "health check",
  "lint my vault", "audit my wiki", or any reference to finding contradictions,
  orphans, or stale content. Do NOT trigger for general Obsidian plugin
  recommendations, theme customization, or vault migration between devices.
metadata:
  version: "1.0.0"
---

# Obsidian Wiki System

A complete system for maintaining a personal knowledge wiki inside Obsidian. The vault follows a three-layer architecture: raw sources (user writes), wiki pages (LLM writes), and schema/index (routing layer). The LLM reads raw captures from `_inbox/`, synthesizes them into richly linked wiki pages, and maintains an index that makes the whole vault queryable without re-reading everything.

**`CLAUDE.md` at the vault root is the authoritative schema.** If anything in this skill conflicts with `CLAUDE.md`, defer to `CLAUDE.md`. After scaffolding, always read it first.

---

## Operation 1: Scaffold (first-time setup)

Run this when the vault has no `CLAUDE.md` yet — the user is starting fresh or adopting this system on an existing vault.

### What to create

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

### Scaffold steps

1. Check which folders already exist. Only create missing ones.
2. Generate `CLAUDE.md` using the template below — adapt the description line to whatever domains the user's existing notes cover (or leave generic if starting empty).
3. Generate `_map/_map.md` with a header and empty sections.
4. If the vault already has notes outside `_inbox/`, do NOT move them. Tell the user what exists and ask how they want to handle it.

### CLAUDE.md template

Generate this file at the vault root. Replace `[DOMAIN_DESCRIPTION]` with a one-line summary of the user's knowledge domains, or use "personal knowledge across all domains" if unknown.

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

- **Filenames** carry the emoji prefix of their folder (`💡`, `🔍`, `📚`, `✓`, `🗺️`). Matches what's already in the vault.
- **AI origin markers** (analysis notes only): `*` = AI-assisted (user wrote base, AI added content); `**` = AI-generated (AI created the synthesis). No marker = organic (user's own note). Marker goes after the folder emoji: `🔍 * Note Title` or `🔍 ** Note Title`. Organic notes carry no marker and are the default.
- **Section-level AI markers**: within a `*` (AI-assisted) note, prefix every AI-added section with a `> 🤖` blockquote on the line immediately before the section header. This makes it visually clear which parts of a mixed note are AI-added vs. user-written at a glance.
- **Frontmatter** on every wiki page: `tags:` (domain only, no type tags), `updated:` (YYYY-MM-DD). AI-marked notes also carry `origin: ai-assisted` or `origin: ai-generated`.
- **`📚 reference/` pages are transcription only.** No editorial inference, synthesis, or framing. What the source says, verbatim or faithfully paraphrased. If synthesis is worth capturing, create a separate `🔍 analysis/` page and link back.
- **`📚 reference/` atomization is allowed for token efficiency.** A single source may be split into multiple child pages if and only if: (1) the topics are distinct enough that they'd rarely be needed together in the same query, and (2) the split follows the source's own topic boundaries — not editorial judgment. The split must be exhaustive (no content lost) and each child page must link back to the parent/sibling pages.
- **Links are wikilinks** (`[[Page Name]]`) not markdown links, so Obsidian's graph view works.
- **Dates are absolute** (2026-04-20), never relative.
- **No `.bak` / `.backup` files.** If you see them, they're noise from past runs — do not read them, do not create them. Git is the version history.

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
4. **Good answers get filed back.** If the synthesis is non-trivial, offer to save it as a new `🔍 analysis/` page or update an existing one. Don't let useful work disappear into chat.

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

### _map/_map.md template

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

---

## Operation 2: Ingest (process `_inbox/`)

This is the core operation. It turns raw captures into linked wiki pages.

> **HARD RULE: Only process files found inside `_inbox/`. Never process, merge, or delete files found at the vault root or any other folder. If a file exists outside `_inbox/`, stop and tell the user — do not touch it.**

### Step 0 — Read context

1. Read `CLAUDE.md` for any convention updates.
2. Read `_map/_map.md` to understand what already exists.
3. List files in `_inbox/`.

### Step 1 — Classify each note

Read the note end to end, then decide the destination:

| Destination | When to use |
|---|---|
| `💡 ideas/` | Seeds, brainstorms, underdeveloped concepts that have a topic but no structure yet |
| `🔍 analysis/` | Synthesis, meeting debriefs, working thinking, strategic analysis, comparative studies |
| `📚 reference/` | Settled knowledge from external sources — transcription only, no editorial framing |
| `✓ action/` | Goal-focused plans with timelines, action items with clear completion points |

If a note is too vague or fragmented to classify (no topic, no context), leave it in `_inbox/` and tell the user.

### Step 2 — Write the wiki page

**Filename:** emoji prefix matching the destination folder (`💡`, `🔍`, `📚`, `✓`).

**Frontmatter:**
```yaml
---
tags:
  - domain-tag-1
  - domain-tag-2
updated: YYYY-MM-DD
---
```
- Tags are domain-level only (e.g., `#climate-finance`, `#machine-learning`). No type tags like `#reference` or `#analysis` — the folder handles that.
- If the page is AI-generated, add `origin: ai-generated`. If AI-assisted (user wrote base, AI added content), add `origin: ai-assisted`.

**Content richness:** Do not simplify or filter through an "is this actionable?" lens. This vault is a personal knowledge base. Write full, rich synthesis capturing the source's complete arc — structure, data, arguments, examples, details. A reader returning in 2 years should find the page informative on its own terms.

**Wikilinks:** Insert `[[wikilinks]]` inline where related entities and pages naturally appear. But only link to pages that actually exist — check `_map/_map.md` before creating any wikilink. If no page exists for an entity, keep it as plain text. Ghost links break trust in the graph.

### Step 3 — Update related pages

A single source typically touches 5–15 existing pages. Update entity pages, concept pages, and relevant MOCs where the source adds new information or shifts the narrative.

### Step 4 — Update or create MOCs

If the source creates or reshapes a cross-domain relationship, update or create a `_moc/🗺️ …` essay. MOCs are **connection essays** (200–300 words), not flat note lists. They explain *why* and *how* domains connect, with `[[wikilinks]]` inline as evidence.

### Step 5 — Update `_map/_map.md`

Append under today's date: `## [YYYY-MM-DD] ingest | <Source Title>` followed by a short list of pages touched. Also update the index sections at the top — add new pages, update summaries of changed pages, update working-note and MOC counts.

### Step 6 — Move the source

Move the processed file out of `_inbox/` (or delete if the raw capture isn't worth keeping separately from the wiki page).

### Critical rules for reference pages

- **`📚 reference/` is transcription only.** What the source says, verbatim or faithfully paraphrased. No editorial inference or synthesis. If synthesis is worth capturing, create a separate `🔍 analysis/` page and link back.
- **Atomization is allowed.** A single source may be split into multiple `📚 reference/` pages if topics are distinct enough they'd rarely be needed together. The split must follow the source's own topic boundaries, be exhaustive (no content lost), and each child page must link back to parent/sibling pages.

### AI origin markers (analysis notes only)

- `*` in filename = AI-assisted: `🔍 * Note Title`
- `**` in filename = AI-generated: `🔍 ** Note Title`
- No marker = organic (user's own note — default)
- Add `origin: ai-assisted` or `origin: ai-generated` to frontmatter
- **Section-level marker**: within a `*` note, place `> 🤖` on the line before each AI-added section header

---

## Operation 3: Query

When the user asks a question about their knowledge base:

1. Read `_map/_map.md` first to locate relevant pages. Do not grep raw sources unless the wiki can't answer.
2. Read the 3–8 most relevant wiki pages, following wikilinks as needed.
3. Answer with citations as `[[wikilinks]]`.
4. If the synthesis is non-trivial, offer to save it as a new `🔍 analysis/` page or update an existing one. Good answers should be filed back into the wiki so the work compounds.

---

## Operation 4: Lint (health check)

When the user asks to audit or health-check the wiki, **report findings — do not silently fix.**

Scan for:

1. **Contradictions** — two pages making incompatible claims
2. **Stale content** — claims superseded by newer sources (compare `updated:` dates)
3. **Orphan pages** — no inbound wikilinks from any other page
4. **Missing pages** — concepts or entities mentioned across multiple pages but lacking their own dedicated page
5. **Missing cross-references** — pages that clearly relate but don't link to each other
6. **Junk files** — `.bak`, `.backup`, or other noise

Present findings as a list grouped by category. Let the user decide what to act on.

---

## General conventions

- Links are wikilinks (`[[Page Name]]`), not markdown links — Obsidian's graph view depends on this.
- Dates are absolute (`YYYY-MM-DD`), never relative ("last week").
- No `.bak` / `.backup` files — Git is the version history.
- Never create new top-level folders beyond the schema above.
- `CLAUDE.md` + `_map/_map.md` are stable across a session. Read them once at the start, don't reshuffle mid-session — this helps with token efficiency via prompt cache hits.
