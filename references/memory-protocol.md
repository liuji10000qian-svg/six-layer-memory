# Memory Protocol

This protocol is platform-neutral. It can be used by Claude, Codex, Cursor, custom agents, or a human-maintained project memory folder.

## Folder Layout

```text
memory/
  MEMORY.md                 # compact entrypoint and index pointer
  _meta/
    index.md                # hot-path searchable summary index
    graph.md                # optional relation graph
  L1_active/
  L2_episodic/
  L3_semantic/
  L4_procedural/
  L5_reflective/
  L6_preference/
```

If the platform cannot use folders, preserve the same logical sections inside a database, notes app, vector store, or JSON store. For example, a single SQLite table:

```sql
CREATE TABLE memory (
  id           TEXT PRIMARY KEY,        -- short stable id, e.g. exp_debug
  layer        TEXT NOT NULL,           -- L1..L6
  title        TEXT NOT NULL,
  tags         TEXT NOT NULL,           -- JSON array
  links        TEXT NOT NULL DEFAULT '[]',
  confidence   TEXT NOT NULL,           -- high|medium|low
  created      TEXT NOT NULL,           -- YYYY-MM-DD
  updated      TEXT NOT NULL,
  recall_count INTEGER NOT NULL DEFAULT 0,
  ttl          TEXT NOT NULL,           -- 7d|30d|90d|permanent
  source       TEXT,
  summary      TEXT NOT NULL,
  body         TEXT NOT NULL
);
CREATE INDEX idx_memory_layer ON memory(layer);
CREATE INDEX idx_memory_updated ON memory(updated);
```

The `_meta/index.md` becomes a `SELECT id, layer, tags, summary FROM memory ORDER BY layer, updated DESC` view. Notion, Airtable, and vector stores follow the same column shape, with the body either inline or chunked into a vector index.

## Record Names

Use predictable names:

```text
{layer}_{topic-slug}_{YYYYMMDD}.md
```

Examples:

```text
L2_topic_slug_YYYYMMDD.md
L4_repeatable_process_YYYYMMDD.md
L6_interaction_preference_YYYYMMDD.md
```

## Record Schema

Every memory record should start with YAML frontmatter:

```yaml
---
layer: L1|L2|L3|L4|L5|L6
id: short-stable-id
title: One sentence title
tags: [tag1, tag2, tag3]
links: []
confidence: high|medium|low
created: YYYY-MM-DD
updated: YYYY-MM-DD
recall_count: 0
ttl: 7d|30d|90d|permanent
source: optional         # add when more than one agent writes to this store
summary: short searchable summary
---
```

Use at most 5 tags. Keep `summary` short enough for an index line.

The frontmatter `id` is the short stable form referenced from the index and graph (for example `pref_style`, `exp_debug`); the filename is the long form including the layer prefix and date (`L6_pref-style_20260519.md`). The same record carries both: short `id` for cross-references, long filename for storage.

## Body Templates

Headings below are shown in English so the templates remain portable. When writing real records, mirror the user's working language (for example, write `## 现状` instead of `## Current State` if the user works in Chinese). The section structure stays the same.

For L1 active:

```markdown
## Current State
[What is active now]

## Next Action
[The next useful step]

## Constraints
[Important limits or user instructions]
```

For L2 episodic:

```markdown
## What Happened
[Concrete event or debugging path]

## Outcome
[Result and evidence]

## Reuse
[When this matters again]
```

For L3 semantic:

```markdown
## Fact
[Stable knowledge]

## Implication
[How it changes future work]
```

For L4 procedural:

```markdown
## Procedure
[Ordered repeatable steps]

## Checks
[How to know it worked]
```

For L5 reflective:

```markdown
## Mistake Or Shift
[What was wrong, risky, or corrected]

## Better Rule
[How future behavior should change]
```

For L6 preference:

```markdown
## Preference
[User preference or style]

## Apply It By
[Concrete behavior the agent should follow]
```

## Index Format

Keep `_meta/index.md` tiny and readable:

```markdown
# Memory Index

## L6 Preference
- [pref_style] tags:[style,language] Follow the user's stated language and tone preferences.

## L5 Reflective
- [ref_safety] tags:[safety,files] Preserve user-owned changes when editing shared files.

## L4 Procedural
- [proc_task] tags:[workflow] Repeatable task flow with verification checks.

## L3 Semantic
- [know_system] tags:[architecture] Stable system fact or project decision.

## L2 Episodic
- [exp_debug] YYYY-MM-DD tags:[debug] Specific event, result, and reusable lesson.

## L1 Active
- [act_current] tags:[current] Current objective, state, and next action.
```

The index is the hot path. It should contain id, tags, summary, and date only. Full explanations stay in records.

## Relations

Use relations in `links` or `_meta/graph.md` only when they help reasoning:

- `derives_from`: created from another record.
- `extends`: adds detail to another record.
- `supersedes`: replaces stale information.
- `contradicts`: conflicts with another record and needs resolution.
- `requires`: depends on another record to be understood.

Prefer few meaningful links over dense graph noise.
