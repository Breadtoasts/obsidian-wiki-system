# Vault Conventions Reference

Shared rules that apply across all vault operations. Read once per session — don't reload unless CLAUDE.md has been updated.

## Folder → Emoji mapping

| Folder | Emoji prefix | Purpose |
|---|---|---|
| `💡 ideas/` | 💡 | Seeds, underdeveloped brainstorms |
| `🔍 analysis/` | 🔍 | Synthesis in progress, meeting debriefs, working thinking |
| `📚 reference/` | 📚 | Settled, reusable knowledge (transcription only) |
| `✓ action/` | ✓ | Goal-focused plans with timelines |
| `_moc/` | 🗺️ | Maps of Content — connective essays (200–300 words) |

## Frontmatter

Every wiki page must have:
```yaml
---
tags:
  - domain-tag    # domain only, no type tags like #reference or #analysis
updated: YYYY-MM-DD
---
```

AI-origin notes additionally carry:
```yaml
origin: ai-generated   # or ai-assisted
```

## AI origin markers

Apply only to analysis notes (`🔍 analysis/`):

- `**` in filename = AI-generated (AI created the synthesis): `🔍 ** Note Title`
- `*` in filename = AI-assisted (user wrote base, AI added content): `🔍 * Note Title`
- No marker = organic (user's own note — default)

**Section-level markers:** within a `*` (AI-assisted) note, place `> 🤖` on the line immediately before each AI-added section header.

## Linking rules

- **Always use wikilinks** (`[[Page Name]]`), never markdown links. Obsidian's graph view depends on this.
- **No ghost links.** Only link to pages that actually exist — check `_map/_map.md` before creating any wikilink. If no page exists for an entity, keep it as plain text.
- **Dates are absolute** (`2026-04-20`), never relative ("last week").

## Reference page rules

- **Transcription only.** `📚 reference/` pages contain only what the source says — verbatim or faithfully paraphrased. No editorial inference, synthesis, or framing. If synthesis is worth capturing, create a separate `🔍 analysis/` page and link back.
- **Atomization allowed.** A single source may be split into multiple child pages if topics are distinct enough they'd rarely be needed together. Rules: follow the source's own topic boundaries (not editorial judgment), be exhaustive (no content lost), link back to parent/sibling pages.

## Content richness

Do not simplify or filter through an "is this actionable?" lens. This vault is a personal knowledge base. Write full, rich synthesis capturing the source's complete arc — structure, data, arguments, examples, details. A reader returning in 2 years should find the page informative on its own terms.

## Prohibited

- No `.bak` / `.backup` files. Git is the version history.
- No new top-level folders beyond the schema.
- Never process, merge, or delete files outside `_inbox/`.
