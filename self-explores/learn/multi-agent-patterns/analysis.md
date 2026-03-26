---
date: 2026-03-26
type: learning
task: ceskills-2v1
tags: [multi-agent, supervisor, swarm, context-isolation, token-economics, telephone-game, consensus, circuit-breaker]
---

# multi-agent-patterns

## Score

| Rubric | Score | Justification |
|--------|-------|---------------|
| **Completeness** | 4/5 | Ba patterns (supervisor, swarm, hierarchical) covered thoroughly with failure modes, consensus, and isolation mechanisms. Missing: dynamic agent spawning, cost optimization cho 15x multiplier, production observability. |
| **Usability** | 4/5 | Actionable guidelines, code examples in SKILL.md va references. Script coordination.py production-ready voi SupervisorAgent, HandoffProtocol, ConsensusManager, FailureHandler. Thieu migration path giua patterns. |
| **Modularity** | 5/5 | Clean separation: SKILL.md (concepts + guidelines), references/frameworks.md (framework implementations), scripts/coordination.py (reusable utilities). Moi component doc lap, co the su dung rieng. |
| **Safety** | 3/5 | Circuit breaker + retry with backoff co trong script. Thieu: rate limiting per agent, resource quotas, deadlock detection, infinite loop prevention beyond TTL mention. Production safety gaps. |

**Overall: 4/5**

## Uu diem

- **Token economics data-driven**: 1x (chat) vs 4x (tools) vs 15x (multi-agent) — concrete multipliers cho cost planning
- **"95% Finding"** tu BrowseComp: token usage = 80% variance, tool calls + model choice = 20% — justifies multi-agent approach
- **Telephone game problem + solution**: LangGraph benchmark evidence, forward_message tool cai thien 50% — practical fix
- **Context isolation as DESIGN PRINCIPLE**: "Sub-agents exist primarily to isolate context, not to anthropomorphize role division" — counter-intuitive but correct
- **3 isolation mechanisms** well-differentiated: full context delegation vs instruction passing vs file system memory
- **Framework comparison** cross-cutting: LangGraph (graph state), AutoGen (GroupChat), CrewAI (hierarchical)
- **Script quality**: coordination.py implements complete message system (AgentMessage + MessageType), supervisor workflow, handoff protocol, consensus manager, failure handler with circuit breaker — production-grade utilities

## Nhuoc diem

- Thieu **cost optimization strategies** cho 15x token multiplier — biggest practical concern unaddressed
- Khong cover **dynamic agent spawning** (agents creating sub-agents at runtime)
- **Consensus mechanisms** underdeveloped — voting + debate mentioned but sycophancy detection only hinted
- Missing **production monitoring/observability** patterns (metrics, tracing, dashboards)
- **Weighted voting** in references lacks expertise_factor implementation — only confidence used
- Script's `calculate_weighted_consensus` co bug: docstring noi "Weight = confidence * expertise_factor" nhung chi dung confidence

## References Insights

### Insight 1: File System Coordination with Locking
References/frameworks.md provides `FileSystemCoordination` class with `acquire_lock()` method — a concurrency pattern NOT in SKILL.md. This is critical for production multi-agent systems where agents write to shared state. The lock-based approach prevents race conditions when multiple agents access shared files simultaneously. SKILL.md mentions "file system memory" as isolation mechanism but does not address concurrent access.

### Insight 2: Checkpoint and Resume Pattern
References/frameworks.md includes `CheckpointManager` for saving/loading workflow state with workflow_id, step, and timestamp. This enables crash recovery and long-running workflow persistence — essential for production multi-agent systems but not mentioned in SKILL.md. Combined with the circuit breaker pattern, this provides a complete failure recovery stack: circuit breaker handles transient failures, checkpointing handles system crashes.

### Insight 3: CrewAI ManagerAgent Review Loop
The `ManagerAgent.review_output()` method in references implements a quality gate with threshold-based approval or revision routing. This quality assurance loop (delegate -> review -> approve/revise) is absent from SKILL.md's pattern descriptions. It provides a concrete mechanism for ensuring sub-agent output quality before aggregation.

## Cross-skill Comparison

### vs tool-design
- **Overlap**: Both address agent-tool interface design. Tool-design's "consolidation principle" (if a human can't decide which tool, agent can't either) directly applies to multi-agent tool allocation.
- **Complementary**: Multi-agent-patterns covers WHEN to distribute tools across agents; tool-design covers HOW to design those tools. Vercel d0 case (17 tools -> 2) shows that simpler tools per agent outperform complex tool sets.
- **Gap**: Neither skill addresses tool conflict resolution when multiple agents attempt same tool simultaneously.

### vs memory-systems
- **Overlap**: Both use file system as coordination mechanism. Memory-systems' entity registry pattern maps directly to multi-agent shared state.
- **Complementary**: Memory-systems provides the persistence layer that multi-agent-patterns' file system coordination requires. Temporal validity from memory-systems could track agent state changes over time.
- **Integration point**: memory-systems' `IntegratedMemorySystem` could serve as shared memory for multi-agent supervisor, replacing the simpler `FileSystemCoordination` in references.

### vs context-optimization
- **Direct link**: Context partitioning (context-optimization) IS multi-agent isolation (multi-agent-patterns) — same concept, different framing.
- **Multi-agent-patterns provides the "why" (context isolation), context-optimization provides the "how" (compaction, masking before partitioning).**

## Tich hop

- **OMC team pipeline**: Supervisor pattern = current OMC architecture. Consider adding `forward_message` for sub-agents to bypass supervisor synthesis in team-exec stage.
- **OMC agent catalog**: Each agent (explore, executor, verifier) = specialized context window — isolation principle validated.
- **Team ralph**: Hierarchical pattern (plan -> execute -> verify) maps to 3-layer structure in skill.
- **Token budget**: 15x multiplier means explicit budget limits per team task are essential. Track with OMC state.
- **File system memory**: `.omc/state/` already implements Pattern 3 (sub-agent file communication).
- **Model routing**: "Better model > more tokens" confirms OMC's opus for architecture, sonnet for execution strategy.
- **Consensus**: OMC `critic` agent implements debate protocol; could add weighted voting for multi-reviewer scenarios.
- **Circuit breaker**: Apply to OMC team-fix loop — currently bounded by max attempts, could add circuit breaker for persistent failures.

## Ghi chu ky thuat

- Version 1.0.0 (Dec 2025) — has not been updated since initial creation
- Key frameworks: LangGraph (graph state machines), AutoGen (GroupChat), CrewAI (role-based hierarchy)
- Telephone game fix: `forward_message` tool -> 50% improvement in supervisor architecture
- Sycophancy trigger: agents mimic each other without unique reasoning -> detect and break
- Time-to-live limits: prevent infinite agent loops
- Script coordination.py: 440 lines, 6 classes (AgentMessage, AgentCommunication, SupervisorAgent, HandoffProtocol, ConsensusManager, AgentFailureHandler)
- Bug in script: `calculate_weighted_consensus` docstring promises expertise_factor weighting but only uses confidence scores
- FileSystemCoordination in references has no timeout on locks — potential deadlock in production
