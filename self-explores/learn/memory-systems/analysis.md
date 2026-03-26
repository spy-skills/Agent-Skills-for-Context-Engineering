---
date: 2026-03-26
type: learning
task: ceskills-u3d
tags: [memory-systems, Mem0, Zep, Graphiti, Letta, LangMem, temporal-KG, LoCoMo, benchmark, vector-store, property-graph]
---

# memory-systems

## Score

| Rubric | Score | Justification |
|--------|-------|---------------|
| **Completeness** | 5/5 | 5 frameworks compared with benchmarks, 5 memory layers defined, 4 retrieval strategies, error recovery, anti-patterns, progression path. Most comprehensive skill in collection (v2.0.0, most recently updated). |
| **Usability** | 4/5 | Clear decision framework ("start simple, add complexity"). Benchmark data enables informed choices. Script provides working VectorStore, PropertyGraph, TemporalKnowledgeGraph, IntegratedMemorySystem. Missing: migration guide between frameworks, production deployment checklist. |
| **Modularity** | 5/5 | SKILL.md (concepts + framework comparison), references/implementation.md (detailed code for vector store, graph, temporal KG, consolidation, context integration, framework quickstarts), scripts/memory_store.py (self-contained integrated system). Each layer independently usable. |
| **Safety** | 3/5 | Error recovery patterns (empty retrieval, stale, conflicting, storage failure) well-documented. Privacy mentioned as guideline but no implementation. Missing: data retention/deletion mechanics, PII detection, audit logging, encryption at rest. |

**Overall: 4.25/5**

## Uu diem

- **Counter-intuitive benchmark insight**: Letta filesystem (74% LoCoMo) beats Mem0 (68.5%) — "tool complexity matters less than reliable retrieval"
- **Zep performance**: 94.8% DMR accuracy, 90% latency reduction (2.58s vs 28.9s) — gold standard for temporal queries
- **5 memory layers** clear decision framework: Working -> Short-term -> Long-term -> Entity -> Temporal KG
- **Progressive architecture path**: filesystem -> Mem0 -> Zep/Graphiti -> Letta — avoids premature complexity
- **Anti-patterns** actionable: stuffing context, ignoring temporal validity, over-engineering early, no consolidation
- **Error recovery** complete: empty retrieval -> broaden search -> prompt user; stale -> check valid_until -> consolidate; conflicting -> prefer most recent; storage failure -> queue writes
- **Entity registry pattern**: `get_or_create_node()` in both references and script ensures identity consistency across interactions

## Nhuoc diem

- **Migration guide** absent: no path from Mem0 -> Zep or filesystem -> Mem0 in practice
- **Consolidation implementation** stub: `consolidate()` in script is `pass` — references have `MemoryConsolidator` but it's incomplete (rebuild_indexes, update_validity_periods undefined)
- **Privacy/GDPR** only mentioned as guideline 6 — no implementation for retention policies, deletion rights, right to be forgotten
- **Multi-agent memory sharing** referenced but not implemented — how do multiple agents share a TemporalKnowledgeGraph safely?
- **Embedding quality**: Both references and script use `np.random.seed(hash(text))` for pseudo-embeddings — works for demos but misleading about real embedding behavior (deterministic but not semantic)

## References Insights

### Insight 1: Graphiti Episode-Based Ingestion Pattern
References/implementation.md shows Graphiti's `add_episode()` API which ingests conversations as episodes with `EpisodeType.message`, then automatically builds the temporal knowledge graph from natural language. This is NOT in SKILL.md and represents a fundamentally different ingestion model: instead of manually creating nodes/edges, you feed conversations and the system extracts entities and relationships automatically. This dramatically reduces integration complexity.

### Insight 2: MemoryContextIntegrator with Entity Extraction
References/implementation.md provides `MemoryContextIntegrator` class that automatically extracts entities from task descriptions using `[[entity_name]]` convention, retrieves relevant memories per entity, formats them for context injection, and enforces token limits with truncation. SKILL.md mentions "just-in-time memory loading" and "strategic injection" but this concrete implementation pattern is only in references. The `[[entity]]` convention is a practical protocol for triggering memory retrieval.

### Insight 3: Dual-Index Vector Store Architecture
References/implementation.md shows `MetadataVectorStore` extending basic VectorStore with separate `entity_index` and `time_index` dictionaries for O(1) lookups by entity or time range, then scoring within those filtered sets. This hybrid indexing approach (semantic search + entity filter + time filter) implements the "hybrid retrieval" strategy mentioned in SKILL.md but with concrete implementation showing how multiple indexes compose.

## Cross-skill Comparison

### vs filesystem-context
- **Overlap**: Both use file system for persistence. Memory-systems' working/short-term layers map to filesystem-context's file-based state.
- **Memory-systems extends**: Adds semantic search (vector store), relationship modeling (graph), and temporal validity — capabilities filesystem alone cannot provide.
- **Key insight**: Memory-systems validates filesystem-context as the starting point (Letta 74% benchmark), then shows when to graduate to more structured approaches.

### vs multi-agent-patterns
- **Direct dependency**: Multi-agent patterns' file system coordination IS memory-systems' short-term layer.
- **Gap**: Neither skill fully addresses concurrent multi-agent memory access. Memory-systems provides entity registry for identity, but no locking. Multi-agent-patterns provides locking (FileSystemCoordination) but no semantic memory.
- **Integration needed**: An `IntegratedMemorySystem` with concurrent access control would bridge both skills.

### vs context-optimization
- **Complementary**: Memory-systems provides the "offload" destination for context-optimization's compaction. When context is compacted, important information should be stored in long-term memory rather than discarded.
- **Memory-aware optimization**: References show `MemoryAwareOptimizer` pattern in context-optimization that checks memory before adding to context — this is the integration seam between the two skills.

## Tich hop

- **Claude memory** (`~/.claude/projects/*/memory/`): Currently filesystem-based — validated by Letta benchmark (74% LoCoMo) as effective starting point.
- **OMC project-memory**: JSON-based at `.omc/project-memory.json` — could add `valid_from`/`valid_until` fields for temporal validity.
- **OMC state**: `.omc/state/` is session-scoped memory (short-term layer). Could add entity indexing for cross-session lookup.
- **self-explores/**: Filesystem memory pattern for learning notes — confirmed effective by benchmarks.
- **Entity tracking**: `get_or_create_node()` pattern applicable to tracking recurring concepts across OMC sessions.
- **Consolidation trigger**: When OMC notepad exceeds threshold, run consolidation (merge related notes, update validity).
- **Future upgrade path**: If OMC retrieval quality degrades with growth, consider Mem0 for semantic search over project-memory.

## Ghi chu ky thuat

- Version 2.0.0 (Feb 2026) — most recently updated skill in collection, reflects latest benchmarks
- Key insight: Letta filesystem (74%) > Mem0 (68.5%) on LoCoMo — complexity is not quality
- Zep Graphiti: 3-tier KG (episode -> semantic entity -> community subgraphs) + bi-temporal model (event time + ingestion time)
- Script memory_store.py: 415 lines, 4 classes (VectorStore, PropertyGraph, TemporalKnowledgeGraph, IntegratedMemorySystem)
- Script bug: `consolidate()` method is empty `pass` — references have partial implementation but incomplete
- Pseudo-embedding approach: `np.random.seed(hash(text))` is deterministic but NOT semantic — same text always maps to same vector, but similar texts do not map to nearby vectors
- LoCoMo benchmark: long-context multi-turn memory evaluation by Snap Research
- LongMemEval: Zep shows 18.5% accuracy improvement + 90% latency reduction
- DMR = Dynamic Memory Retrieval accuracy metric
