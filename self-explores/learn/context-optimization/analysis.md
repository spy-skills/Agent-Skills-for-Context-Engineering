---
date: 2026-03-26
type: learning
task: ceskills-s35
tags: [context-optimization, compaction, observation-masking, KV-cache, partitioning, budget-management, relevance-filtering]
---

# context-optimization

## Score

| Rubric | Score | Justification |
|--------|-------|---------------|
| **Completeness** | 4/5 | 4 strategies (compaction, observation masking, KV-cache, partitioning) with decision framework and performance targets. Missing: real-world benchmark data (unlike memory-systems), production case studies (unlike project-development). |
| **Usability** | 4/5 | Decision framework clear: "tool outputs dominate -> masking; docs dominate -> summarization; history dominates -> compaction." Script provides working ObservationStore, ContextBudget, cache utilities. Missing: end-to-end optimization workflow example. |
| **Modularity** | 4/5 | SKILL.md (concepts + decision framework), references/optimization_techniques.md (detailed patterns + benchmarks + pitfalls), scripts/compaction.py (working utilities). Good separation but references overlap significantly with SKILL.md content. |
| **Safety** | 4/5 | "Never mask recent turn observations", "never compact system prompt", explicit thresholds (70% warning, 80% trigger, 90% aggressive). Alert thresholds documented. Missing: rollback mechanism if optimization removes critical info. |

**Overall: 4/5**

## IMPORTANT: context-optimization vs context-compression

These two skills address DIFFERENT problems:

| Dimension | context-optimization | context-compression |
|-----------|---------------------|---------------------|
| **Primary goal** | RELEVANCE filtering — keep what matters, remove noise | SIZE reduction — make same content smaller |
| **Core question** | "What should be in context?" | "How to fit more into less space?" |
| **Techniques** | Observation masking, budget allocation, KV-cache, partitioning | Anchored iterative summarization, opaque compression, regenerative summary |
| **When to use** | Context has noise/irrelevant content mixed with signal | Context has too much relevant content that exceeds window |
| **Optimization target** | Signal-to-noise ratio in context | Tokens per task (total tokens to complete including re-fetch costs) |
| **Key metric** | Quality preservation per token | Compression ratio with fidelity |

**In practice**: optimization PRECEDES compression. First filter irrelevant content (optimization), then compress what remains if still too large (compression).

## Uu diem

- **Observation masking** highest-ROI technique: tool outputs = 80%+ of token usage -> masking achieves 60-80% reduction
- **4 strategies** well-differentiated with clear selection criteria based on context composition
- **Performance targets** concrete: compaction 50-70% reduction with <5% quality degradation; masking 60-80% reduction with <2% quality impact; cache 70%+ hit rate
- **Budget management framework**: explicit allocation by category (system prompt, tool definitions, retrieved docs, message history, tool outputs, buffer)
- **Decision framework** tied to context composition — not one-size-fits-all
- **Trigger-based optimization**: 70% warning, 80% compaction, 90% aggressive — graduated response
- **Script quality**: ObservationStore with LRU eviction, ContextBudget with category tracking, selective masking based on relevance + age

## Nhuoc diem

- **No production case studies** — unlike project-development (Karpathy, Vercel) or memory-systems (Zep benchmarks)
- **KV-cache section** infrastructure-level — most users cannot control inference infrastructure
- **Overlap with context-compression** creates confusion — SKILL.md mentions "compression" as synonym for optimization but they are distinct
- **No attention-aware placement** — context-degradation's lost-in-middle is relevant but not integrated
- **Missing rollback**: what if compaction removes info needed 5 turns later? No recovery mechanism beyond "retrievable reference"
- **Performance targets lack source**: "50-70% token reduction with <5% quality degradation" — where does this come from? No citation.

## References Insights

### Insight 1: Three-Tier Compaction Thresholds with Model-Specific Behavior
References/optimization_techniques.md specifies warning (70%), trigger (80%), aggressive (90%) thresholds AND notes that "some models show graceful degradation while others exhibit sharp performance cliffs." This model-specific behavior is NOT in SKILL.md and is critical: a 80% threshold for Claude (graceful) might need to be 70% for a model with sharp cliffs. The implication is that compaction thresholds should be calibrated per model, not universal.

### Insight 2: Cache-Unfriendly Anti-Pattern with Concrete Example
References provides a direct code comparison showing that `f"Current time: {datetime.now().isoformat()}"` at the START of a system prompt kills KV-cache hit rate entirely because any prefix change invalidates all downstream cached computations. The fix: move dynamic content out of stable prefixes or use separate variables. This concrete anti-pattern is only hinted at in SKILL.md but fully demonstrated in references. Combined with Manus case study from project-development (same insight), this is one of the highest-ROI optimizations.

### Insight 3: Monitoring and Alerting Framework
References provides a complete monitoring specification with 5 key metrics (context token count over time, cache hit rates, response quality by context size, cost per conversation by context length, latency by context size) and 4 alert thresholds (utilization >80%, cache hit rate <50%, quality drop >10%, cost increase above baseline). SKILL.md mentions "monitor and iterate" but does not provide this operational framework. This bridges the gap between development-time optimization and production monitoring.

### Insight 4: MemoryAwareOptimizer Integration Pattern
References shows `MemoryAwareOptimizer` that checks memory system before adding information to context — if information already exists in memory with sufficient importance, it can be excluded from context. This memory-optimization integration seam is referenced in SKILL.md's integration section but only implemented in references.

## Cross-skill Comparison

### vs context-compression
- **CRITICAL DISTINCTION**: Optimization = RELEVANCE filtering (what belongs in context?). Compression = SIZE reduction (how to fit more?).
- **Sequence**: Optimization first (remove irrelevant), then compression (shrink what remains).
- **Overlap source**: Both mention "compaction" and "summarization" — but optimization's compaction is about selecting WHAT to compact (budget-driven), while compression's compaction is about HOW to compact (technique-driven: anchored iterative, opaque, regenerative).
- **context-compression's unique value**: "tokens per task" metric (total cost including re-fetch), anchored iterative summarization (structured sections force preservation), opaque compression (99%+ ratio).
- **context-optimization's unique value**: observation masking (80% of tokens are tool outputs), KV-cache optimization, budget management framework.

### vs context-degradation
- **Dependency**: Context-degradation defines the PROBLEMS (lost-in-middle, poisoning, distraction, confusion, clash). Context-optimization provides the SOLUTIONS (masking, compaction, cache, partitioning).
- **Missing link**: Optimization should explicitly reference degradation patterns as triggers. E.g., "If you observe lost-in-middle symptoms, apply attention-aware placement before compaction."

### vs multi-agent-patterns
- **Context partitioning = multi-agent isolation**: Optimization's partitioning strategy IS multi-agent patterns' context isolation, seen from different angle.
- **Optimization informs architecture**: If context budget analysis shows single agent cannot fit task, partitioning into multi-agent is the optimization response.

### vs memory-systems
- **Offload destination**: When optimization removes content from context, memory-systems provides the persistence layer to store it for later retrieval.
- **MemoryAwareOptimizer**: References explicitly shows this integration — check memory before loading into context.

## Tich hop

- **OMC tools**: Implement observation masking for large tool outputs (LSP results, file reads, search results) — highest ROI optimization
- **OMC system prompts**: Avoid dynamic content (timestamps, session IDs) at beginning of prompts — KV-cache killer
- **OMC notepad**: Apply compaction when notepad exceeds threshold — 70% warning, 80% trigger
- **Context budget**: Apply ContextBudget framework to OMC agent context allocation — track usage by category
- **Tool definitions**: Keep tool definitions stable across turns — "mask, do not remove" from Manus
- **Monitoring**: Track context token count and quality metrics per OMC session — detect degradation early
- **Agent outputs**: Mask old tool outputs in long-running OMC sessions with `[Obs:{ref_id} elided. Key: {summary}]` pattern
- **Integration with memory**: Before adding retrieved docs to context, check if entity is already in OMC project-memory

## Ghi chu ky thuat

- Version 1.0.0 (Dec 2025) — oldest version, could benefit from v2 update with case studies and compression differentiation
- Key insight: tool outputs = 80%+ of context tokens -> observation masking = highest ROI
- Performance targets: compaction 50-70% reduction (<5% quality loss), masking 60-80% reduction (<2% quality loss), cache 70%+ hit rate
- Script compaction.py: 380 lines, key classes: ObservationStore (LRU eviction, ref-based retrieval), ContextBudget (category allocation, trigger detection), cache utilities
- Compaction thresholds should be model-specific: graceful degradation vs sharp performance cliffs
- Cache anti-pattern: timestamps/session IDs at prompt start kill cache hit rate
- Missing from entire skill collection: a unified "context lifecycle" skill that sequences fundamentals -> optimization -> compression -> degradation as a coherent workflow
