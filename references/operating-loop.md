# Operating Loop

The loop has four parts: recall, write, compress, and maintain. Keep each part small and observable.

## Recall

Before answering or acting:

1. Read the compact index.
2. Match the user request against tags and summaries.
3. Select at most 5 relevant full records.
4. Always apply inline L6 preferences when present.
5. Prefer high-confidence L5 reflections when they warn about a likely mistake.

Do not load the full memory folder. Cold records should cost zero context until selected.

Skip recall entirely for trivial reads (`ls`, `cat`, casual chat, single-shot factual questions) where context cost outweighs likely benefit.

## Write

Write a memory only when it will help future work:

| Trigger | Layer |
| --- | --- |
| New current focus or unfinished work | L1 |
| Completed non-trivial task or debug trail | L2 |
| Stable fact, architecture, or decision | L3 |
| Reusable workflow or recipe | L4 |
| Mistake, correction, risk lesson, changed assumption | L5 |
| User preference or working style | L6 |

Before writing:

1. Search the index for near-duplicates.
2. Update the existing record if the same memory already exists.
3. Create a new record only when the fact, experience, or rule is materially distinct.
4. Write the record first, then update the index.
5. Add graph relations only when they help resolve provenance, contradiction, or replacement.

## Compression

Compression is transfer, not blind deletion.

- Summarize noisy tool output immediately.
- After long work, keep the current objective, constraints, changed files, decisions, and next action.
- Before dropping older context, save durable facts to L3, reusable procedures to L4, mistakes to L5, and preferences to L6.
- Pure dead-end logs, duplicated reads, and superseded intermediate attempts can be discarded.

## Maintenance

Run maintenance at natural boundaries: task completion, handoff, or explicit cleanup.

- Delete L1 records whose `updated` is older than 7 days AND that no live record links to. Keep an L1 record when it was touched within the window or when a current task still references it.
- Compress low-recall L2 records after 90 days into a shorter L3/L4/L5 record if they still matter.
- Mark replaced records with `supersedes` or lower `confidence`; do not silently rewrite history.
- Rebuild the index from records when it drifts.
- Remove placeholder examples once real records exist.

## Anti-Bloat Rules

- Do not create installer scripts unless the user explicitly asks to integrate with a named platform.
- Do not keep both `memory/L1_active` and root-level `L1_active` copies.
- Do not store raw transcripts as memory records.
- Do not vectorize everything by default; structured summaries are usually enough.
- Do not make the index a second full memory store.
- Do not add automation that cannot be inspected, tested, or turned off.

## Handoff Checklist

Before delivering a memory system or skill:

- The core instructions fit in one short `SKILL.md` or entrypoint file.
- Detailed but optional rules are in references.
- Platform-specific glue is absent or isolated.
- The file tree has no empty duplicate folders.
- Examples are clearly templates or removed.
- A future agent can answer: what to recall, where to write, when to compress, and how to handle conflicts.
