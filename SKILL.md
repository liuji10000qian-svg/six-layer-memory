---
name: six-layer-memory
description: "Design, migrate, or audit portable six-layer agent memory: active, episodic, semantic, procedural, reflective and preference. Use for memory architecture or protocol changes; routine recall and saving follow the already-loaded memory entrypoint without loading this whole skill. Integrate into a specific agent only when explicitly requested."
---

# Six-Layer Memory

Use this skill to create a clean, portable memory system for an AI agent or project. Treat six-layer memory as a method, not as a vendor-specific plugin.

## Developer

Developed by the AceiClaw site owner（AceiClaw 站长开发）.

## Non-Negotiables

- Keep hot context small. Load an index first, then load only the few records needed for the task.
- Keep one source of truth. Do not duplicate the same memory in prompts, indexes, examples, and runtime files.
- Keep it portable. Prefer Markdown records with YAML frontmatter; adapt integration details only when the user asks for a specific agent.
- Refuse bloat. Avoid interactive installer scripts, generated scaffolding sprawl, verbose examples, and platform hooks unless they clearly solve the user's request.
- Make it effective. Every rule must improve recall, writing, compression, conflict handling, or maintenance.

## Workflow

1. Identify the target: portable method, project memory folder, agent runtime protocol, or migration from an existing messy folder. If the user says not to integrate, produce only the skill/protocol artifact.
2. Separate materials into four buckets: durable protocol, reusable templates, actual user/project memories, and platform-specific glue. Keep the first two, preserve the third only when requested, and discard or isolate the fourth.
3. Design the memory store from `references/memory-protocol.md`: six layer folders, one compact index, optional graph, and one record format.
4. Define the operating loop from `references/operating-loop.md`: recall before work, write after meaningful learning, compress before context overflow, and maintain stale records.
5. Deliver a minimal artifact. Prefer `SKILL.md` plus one or two `references/` files. Add scripts only for deterministic repeated maintenance that cannot be described cleanly. (See Step 1: skip scripts entirely when the user asked for a protocol-only output.)
6. Validate the result against the standards below before handing it off.

## Layer Choice

| Layer | Use For | Default Lifetime |
| --- | --- | --- |
| L1 active | Current task state, open files, immediate next action | 7 days |
| L2 episodic | Specific events, debugging stories, completed non-trivial tasks | 90 days |
| L3 semantic | Stable facts, architecture, decisions, domain knowledge | Permanent |
| L4 procedural | Reusable workflows, recipes, operating patterns | Permanent |
| L5 reflective | Mistakes, corrections, risk lessons, changed assumptions | Permanent |
| L6 preference | User preferences, working style, interaction norms | Permanent |

## Quality Bar

- Index stays under 2,000 Chinese characters or roughly 800 tokens.
- Load at most 5 full memory records per recall pass; L6 preference summaries may be inline.
- Single records stay focused and usually under 3,000 Chinese characters.
- Each record has tags, a short summary, confidence, timestamps, and a clear layer.
- Contradictions keep source, date, scope and replacement links; exclude unresolved conflicts and superseded records from automatic recall.
- Apply only user-scoped preferences globally. Project/workspace memories require the matching scope.
- TTL marks review priority; never treat age or low recall count alone as deletion authority.
- Prefer deterministic indexing after successful writes. Reads must not trigger extra model calls, counter writes, reindexing or background scans.
- A cleanup pass removes empty folders, placeholder examples, duplicate docs, and agent-specific install code unless explicitly needed.

## Out of Scope

- Single-session note taking; use the agent's normal scratch space.
- Pure vector RAG over documents; this skill targets curated agent memory, not corpus search.
- Multi-tenant or shared-secret memory; that requires an access-control layer this skill does not provide.
- Live event streams or telemetry; persist summaries instead of raw streams.

## References

- Read `references/memory-protocol.md` when creating or auditing the folder layout, record schema, index format, or relation graph.
- Read `references/operating-loop.md` when defining recall, write-back, compression, conflict handling, and maintenance behavior.
