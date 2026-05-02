---
name: ingest
description: >
  Process raw notes from an Obsidian vault's _inbox/ into classified, linked
  wiki pages. ALWAYS trigger when the user says "process my inbox",
  "organize my notes", "run the vault", "process notes", "end of day processing",
  "clean up my inbox", or any reference to processing, sorting, tagging, or
  organizing Obsidian notes. Also trigger when the user drops files into _inbox/
  and wants them integrated into the wiki. Do NOT trigger for querying existing
  notes, health checks, or vault setup — those have their own skills.
metadata:
  version: "1.0.0"
---

# Ingest — Process `_inbox/` Into Wiki Pages

Turn raw captures into richly linked wiki pages. This is the core operation of the vault system.

> **HARD RULE: Only process files found inside `_inbox/`. Never process, merge, or delete files found at the vault root or any other folder. If a file exists outside `_inbox/`, stop and tell the user — do not touch it.**

Read `references/conventions.md` for frontmatter, linking, and formatting rules.

## Step 0 — Load context

1. Read `CLAUDE.md` at the vault root for any convention updates.
2. Read `_map/_map.md` to know what pages already exist (needed for wikilink safety).
3. List files in `_inbox/`.

If `CLAUDE.md` doesn't exist, tell the user to run the scaffold skill first.

## Step 1 — Classify each note

Read the note end to end, then decide the destination:

| Destination | When to use |
|---|---|
| `💡 ideas/` | Seeds, brainstorms, underdeveloped concepts with a topic but no structure |
| `🔍 analysis/` | Synthesis, meeting debriefs, working thinking, comparative studies |
| `📚 reference/` | Settled knowledge from external sources — transcription only, no editorial framing |
| `✓ action/` | Goal-focused plans with timelines, action items with clear completion points |

If a note is too vague or fragmented to classify (no topic, no context), leave it in `_inbox/` and tell the user.

## Step 2 — Write the wiki page

**Filename:** emoji prefix matching the destination folder + descriptive title.

**Frontmatter:** see `references/conventions.md` for the exact format. Key rules:
- Tags are domain-level only (no type tags)
- Include `origin: ai-generated` or `origin: ai-assisted` when applicable
- Always include `updated: YYYY-MM-DD`

**Content richness:** Do not simplify or filter through an "is this actionable?" lens. Write full, rich synthesis capturing the source's complete arc — structure, data, arguments, examples, details. A reader returning in 2 years should find the page informative on its own terms.

**Wikilinks:** Insert `[[wikilinks]]` inline where related pages naturally appear. Only link to pages confirmed in `_map/_map.md` — no ghost links.

**Reference pages are transcription only.** What the source says, verbatim or faithfully paraphrased. Synthesis goes in a separate `🔍 analysis/` page with a link back. Atomization into multiple pages is allowed if topics are distinct — see `references/conventions.md` for rules.

## Step 3 — Update related pages

A single source typically touches 5–15 existing pages. Update:
- Entity pages where the source adds new information
- Concept pages where the source contributes
- Any page whose claims are modified, extended, or contradicted by the source

## Step 4 — Update or create MOCs

If the source creates or reshapes a cross-domain relationship, update or create a `_moc/🗺️ …` essay. MOCs are **connection essays** (200–300 words), not flat note lists. They explain *why* and *how* domains connect, with `[[wikilinks]]` inline as evidence for the narrative.

## Step 5 — Update `_map/_map.md`

Two updates needed:
1. **Index sections** at the top: add new pages with one-line summaries, update summaries of changed pages, update the working-note and MOC counts.
2. **Ingest log** at the bottom: append `## [YYYY-MM-DD] ingest | <Source Title>` followed by a short list of pages touched.

## Step 6 — Move the source

Move the processed file out of `_inbox/` once integrated. Delete it if the raw capture isn't worth keeping separately from the wiki page. If the source has lasting reference value beyond what the wiki page captures, move it to `Attachments/`.

## After processing all files

Report what was ingested: list each source and what pages were created or updated.
