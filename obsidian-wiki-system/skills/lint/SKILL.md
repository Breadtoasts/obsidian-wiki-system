---
name: lint
description: >
  Health-check an Obsidian wiki vault for contradictions, orphan pages,
  missing cross-references, and stale content. ALWAYS trigger when the user
  says "health check", "lint my vault", "audit my wiki", "check my notes",
  "what are my gaps", "run the map", "show my gray matter", "probe my stubs",
  "weekly review", or any reference to finding problems, inconsistencies,
  or maintenance issues in their Obsidian vault. Do NOT trigger for inbox
  processing, querying, or vault setup.
metadata:
  version: "1.0.0"
---

# Lint — Wiki Health Check

Scan the vault for structural and content issues. **Report findings — do not silently fix anything.** Present a categorized list and let the user decide what to act on.

## Steps

1. **Read `_map/_map.md`** to get the full page inventory.
2. **Read all wiki pages** across `💡 ideas/`, `🔍 analysis/`, `📚 reference/`, `✓ action/`, and `_moc/`.
3. **Run each check** below and collect findings.
4. **Report** findings grouped by category with specific page names and quotes where relevant.

## Checks to run

### 1. Contradictions
Two pages making incompatible claims about the same fact. Quote both claims and name both pages.

### 2. Stale content
Claims that have been superseded by newer sources. Compare `updated:` dates — if a newer page contradicts or updates an older one, flag the older page.

### 3. Orphan pages
Pages with no inbound wikilinks from any other page. These are invisible in Obsidian's graph view and may indicate missing connections.

### 4. Missing pages
Entities or concepts mentioned across multiple pages (as plain text, not wikilinks) that are significant enough to warrant their own dedicated page. Check `_map/_map.md`'s "Entities referenced but without profile pages" section — are there entries that now have enough material to justify a page?

### 5. Missing cross-references
Pages that clearly relate (overlapping tags, shared entities, complementary topics) but don't link to each other.

### 6. Junk files
`.bak`, `.backup`, or other noise files anywhere in the vault.

### 7. Frontmatter issues
Pages missing required frontmatter fields (`tags`, `updated`), or using markdown links instead of wikilinks.

### 8. Stale action items
`✓ action/` pages with deadlines that have passed or tasks that appear completed based on related analysis pages.

## Output format

Group findings by category. For each finding, include the page name(s) and a brief description of the issue. If a category has no findings, say so — a clean bill of health is useful information too.

At the end, suggest a priority order for addressing the findings (highest-impact fixes first).
