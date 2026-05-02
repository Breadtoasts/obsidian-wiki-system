---
name: query
description: >
  Answer questions from an Obsidian wiki vault using the index as a router
  and citing sources as wikilinks. Trigger when the user asks about their
  knowledge base: "what do I know about X?", "find that note about Y",
  "what did I write about Z?", "summarize my notes on...", or any question
  that should be answered from existing vault content rather than general
  knowledge. Also trigger for "what's the status of..." or "where did I
  put..." in vault context. Do NOT trigger for inbox processing, health
  checks, or vault setup.
metadata:
  version: "1.0.0"
---

# Query — Answer From the Wiki

Answer questions using the vault's wiki pages. The vault is a structured knowledge base — use the index to find relevant pages efficiently, then synthesize an answer with citations.

## Steps

1. **Read `_map/_map.md` first.** This is the router — it has a one-line summary of every page. Locate the 3–8 most relevant pages from the index.
2. **Read the relevant wiki pages.** Follow wikilinks to adjacent pages if needed for a complete answer. Prefer wiki pages (small, dense) over raw sources (long, noisy) — only grep raw sources or `_inbox/` if the wiki genuinely can't answer.
3. **Answer with citations.** Use `[[wikilinks]]` inline when referencing vault content. This keeps answers traceable and navigable in Obsidian.
4. **File good answers back.** If the synthesis is non-trivial (connects multiple pages, produces a new insight, or would be useful to reference later), offer to save it as a new `🔍 analysis/` page or update an existing one. Tag it with `origin: ai-generated` and mark the filename with `**`. Don't let useful work disappear into chat.

## What makes a good query answer

- Grounded in what the vault actually contains, not general knowledge
- Cites specific pages so the user can verify and explore
- Acknowledges gaps — if the vault doesn't cover something, say so rather than filling in from training data
- Concise but complete — match the depth to the question
