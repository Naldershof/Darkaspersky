# Darkaspersky — Elden Ring Lore Wiki

> A compounding knowledge base of Elden Ring lore, built on the Karpathy LLM Wiki framework.

## Overview

This project follows Andrej Karpathy's LLM Wiki pattern: raw source materials are compiled by an LLM into a structured, cross-referenced wiki that compounds knowledge over time. The domain is **Elden Ring lore and story** — the notoriously fragmented narrative by FromSoftware and George R.R. Martin.

## Architecture

### Three Layers

1. **`raw/`** — Immutable source material. The LLM reads but **never modifies** these files. Contains item descriptions, NPC dialogue, environmental observations, cinematics, and external analysis.

2. **`docs/`** — The wiki layer. LLM-generated and LLM-maintained markdown pages. Also serves as the MkDocs content directory for the static site. Every page has YAML frontmatter, cross-references, and a Connections & Analysis section.

3. **`CLAUDE.md`** (this file) — The schema. Defines structure, conventions, and workflows.

### Tech Stack

- **MkDocs Material** for static site generation (dark theme, mobile-responsive, search)
- **GitHub Pages** for hosting (auto-deploys via `.github/workflows/deploy.yml`)
- **Markdown** throughout — compatible with Obsidian if desired

---

## Page Conventions

### Frontmatter Format

Every wiki page in `docs/` must have YAML frontmatter:

```yaml
---
title: "Malenia, Blade of Miquella"
category: demigods
tags:
  - outer-god-influence
  - scarlet-rot
  - the-haligtree
  - miquella
related:
  - demigods/miquella.md
  - lore-concepts/outer-god-of-rot.md
  - locations/the-haligtree.md
  - npcs/millicent.md
sources:
  - raw/item-descriptions/remembrances/malenia.md
  - raw/dialogue/millicent.md
  - raw/environmental/haligtree-observations.md
last_updated: 2026-04-16
---
```

### Page Structure

Every wiki page follows this structure:

```markdown
# [Title]

> One-sentence summary of this entity/concept.

## Overview
2-3 paragraph summary of what is known.

## Lore Details
Detailed breakdown with cited sources. Use footnotes for specific source references.

## Connections & Analysis
**THE CORE VALUE-ADD.** Cross-reference insights from multiple raw sources.
Surface non-obvious relationships, hidden motivations, timeline implications.
Every claim must trace back to specific raw source files.

## Unresolved Mysteries
!!! question "Why did [X]?"
    **Interpretation A:** [Evidence from source 1, source 2]
    **Interpretation B:** [Evidence from source 3, source 4]
    **Gap:** [What additional evidence would resolve this]

## Sources
Bulleted list of all raw/ files consulted for this page.
```

### Naming Conventions

- **Filenames:** kebab-case (`marika-radagon.md`, `the-shattering.md`)
- **Cross-references:** Standard markdown links relative to docs/ (`[Malenia](../demigods/malenia.md)`)
- **Tags:** kebab-case lore threads (`outer-god-influence`, `rebellion-against-golden-order`)

### Lore Thread Tags

Tags represent thematic threads that weave across multiple pages. Key threads:

- `the-shattering` — Events of the Shattering and its aftermath
- `golden-order` — The Golden Order's rise, tenets, and corruption
- `outer-god-influence` — Influence of the Greater Will, Rot God, Formless Mother, Dark Moon, Frenzied Flame
- `scarlet-rot` — The rot's origin, spread, and those afflicted
- `destined-death` — The Rune of Death, its sealing, and consequences
- `rebellion` — Those who defied the Golden Order (Ranni, Marika, the Tarnished)
- `empyrean` — The empyreans and their destinies (Malenia, Miquella, Ranni)
- `omen-curse` — The Omen, the shunning grounds, Morgott and Mohg
- `crucible` — The primordial crucible and its connection to the Erdtree's origins
- `age-of-the-erdtree` — The Erdtree's dominion and what came before
- `tarnished` — The Tarnished, their exile, and return
- `miquella` — Miquella's plans, manipulations, and the DLC revelations
- `land-of-shadow` — The hidden history of the Land of Shadow

---

## Workflows

### Ingest (adding new raw sources)

When new raw material is added to `raw/`:

1. **Read** the new source file(s) completely
2. **Scan** `docs/index.md` to understand existing wiki structure
3. **Identify** which existing wiki pages are affected by the new information
4. **Update** affected pages:
   - Add new details to Lore Details section
   - Update Connections & Analysis with any new cross-references
   - Add/update Unresolved Mysteries if the new source introduces contradictions or answers existing questions
   - Update `sources:` in frontmatter
   - Update `related:` in frontmatter if new connections are discovered
5. **Create** new wiki pages if the source introduces entities/concepts not yet covered
6. **Update** `docs/index.md` with any new pages
7. **Update** category index pages (`docs/[category]/index.md`)
8. **Append** to `docs/log.md`:
   ```
   ## [Date] — Ingest: [source file(s)]
   - Updated: [list of modified wiki pages]
   - Created: [list of new wiki pages]
   - New connections: [brief description of discovered connections]
   ```

### Query (answering questions using the wiki)

When asked a question about Elden Ring lore:

1. **Read** `docs/index.md` to find relevant pages
2. **Read** the relevant wiki pages
3. **Synthesize** an answer with citations to specific wiki pages
4. **If the answer reveals new connections**, file them back into the wiki:
   - Update relevant Connections & Analysis sections
   - Optionally create a new wiki page if the synthesis is substantial
   - Append to `docs/log.md`

### Lint (health checks)

Periodic maintenance pass across the entire wiki:

1. **Contradictions**: Flag where two pages make conflicting claims. In Elden Ring, contradictions are often *features* — surface them as Unresolved Mysteries, not errors.
2. **Orphan pages**: Pages with no inbound links from other pages.
3. **Missing cross-references**: Entity names mentioned in body text but not linked to their wiki page.
4. **Gap detection**: Entities or events referenced in `raw/` sources that have no corresponding wiki page.
5. **Connection discovery**: Two pages that reference the same concept/entity but aren't linked to each other.
6. **Stale tags**: Tags in frontmatter that don't match the page content.
7. **Source coverage**: Raw source files not cited by any wiki page (suggests unprocessed material).

Output lint results to `docs/log.md` and fix issues inline.

---

## Raw Source Guidelines

### Directory Structure

```
raw/
├── item-descriptions/     # Verbatim in-game item text
│   ├── weapons/
│   ├── armor/
│   ├── talismans/
│   ├── spells-incantations/
│   ├── spirit-ashes/
│   ├── key-items/
│   ├── remembrances/
│   ├── consumables/
│   └── ashes-of-war/
├── dialogue/              # NPC dialogue transcripts
├── cinematics/            # Cinematic narration and descriptions
├── environmental/         # Level design observations
├── loading-screens/
├── maps-and-paintings/
├── dlc/                   # Shadow of the Erdtree (same sub-structure)
├── external/              # Interviews, GRRM contributions, cut content
└── community-theories/    # Notable community analyses
```

### Source File Format

```markdown
# [Topic/Item/NPC Name]

## [Entry Name]

> "[Exact in-game text or dialogue]"

[Source: Item name / NPC name, context]
[Context: When/where this is encountered]

---
(repeat for each entry in the file)
```

### Tagging

- `[Source: ...]` — exact in-game source
- `[Context: ...]` — when/where encountered
- `[CUT CONTENT]` — datamined, not in final game
- `[DLC]` — Shadow of the Erdtree content
