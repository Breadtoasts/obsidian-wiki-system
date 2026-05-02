# Obsidian Wiki System

Turn any Obsidian vault into a structured, LLM-maintained personal wiki.

## What it does

This plugin gives Claude four focused skills for managing an Obsidian vault, plus an auto-context hook:

| Skill | Trigger phrases | What it does |
|---|---|---|
| **scaffold** | "set up my vault", "initialize wiki" | Creates folder structure, generates CLAUDE.md and _map index. Run once. |
| **ingest** | "process my inbox", "organize my notes" | 6-step pipeline: read → classify → write wiki page → cross-link → update MOCs → update index → move source |
| **query** | "what do I know about X?", "find that note about..." | Answers from the wiki using the index as router, cites sources as wikilinks |
| **lint** | "health check", "audit my wiki", "weekly review" | Scans for contradictions, orphans, missing pages, stale content. Reports without fixing. |

**Auto-context hook:** At session start, automatically loads CLAUDE.md and _map.md so Claude knows your vault state before you ask anything.

## Vault structure

```
Vault/
├── _inbox/          ← Drop raw notes, web clips, meeting transcripts here
├── 💡 ideas/        ← Seeds and underdeveloped brainstorms
├── 🔍 analysis/     ← Synthesis, meeting debriefs, working thinking
├── 📚 reference/    ← Settled knowledge (transcription only — no editorial framing)
├── ✓ action/        ← Goal-focused plans with timelines
├── _moc/            ← Maps of Content (connective essays, not flat lists)
├── _map/_map.md     ← Index + operation log
├── Attachments/     ← Images and binary assets
└── CLAUDE.md        ← Authoritative schema (generated on scaffold)
```

## Getting started

1. Install the plugin
2. Connect your Obsidian vault folder
3. Say **"set up my vault"** — Claude scaffolds the structure and generates CLAUDE.md
4. Drop notes into `_inbox/` and say **"process my inbox"**

## Key design principles

- **Separation of raw and synthesized**: Users write into `_inbox/`. The LLM owns the wiki folders.
- **Index-first routing**: `_map/_map.md` summarizes everything. Claude reads it before touching anything else.
- **Reference = transcription only**: `📚 reference/` contains only what the source says. Synthesis goes in `🔍 analysis/`.
- **Wikilinks only**: Everything uses `[[Page Name]]` for Obsidian graph view compatibility.
- **AI transparency**: AI-generated pages are marked with `**` in filenames and `origin:` in frontmatter.
- **Content richness**: Notes capture the source's full arc — not filtered by "is this actionable?"

## Plugin structure

```
obsidian-wiki-system/
├── .claude-plugin/plugin.json
├── skills/
│   ├── scaffold/SKILL.md        ← First-time vault setup
│   ├── ingest/                  ← Inbox processing
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── conventions.md   ← Shared formatting rules
│   ├── query/SKILL.md           ← Wiki querying
│   └── lint/SKILL.md            ← Health checks
├── hooks/hooks.json             ← Auto-context loading
└── README.md
```

## Requirements

- An Obsidian vault (or any folder)
- Claude Code, Claude Desktop (Cowork), or any Claude environment with file access

## License

MIT
