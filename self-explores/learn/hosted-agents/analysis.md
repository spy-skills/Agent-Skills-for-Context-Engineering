---
date: 2026-03-26
type: learning
task: ceskills-6qg
tags: [hosted-agents, infrastructure, sandbox, warm-pool, multiplayer, server-first, self-spawning, predictive-warmup]
---

# hosted-agents - Deep Analysis

## Score

| Rubric | Score | Justification |
|--------|-------|---------------|
| Completeness | 4/5 | Covers sandbox infrastructure, image registry, warm pools, self-spawning agents, API layer, multiplayer, authentication, client implementations (Slack/Web/Chrome). Missing: cost modeling, disaster recovery, multi-region deployment, observability/logging patterns. |
| Usability | 3/5 | Infrastructure-heavy with pseudocode patterns. sandbox_manager.py provides typed classes but requires significant adaptation for any specific platform (Modal, Fly.io, etc.). Not immediately applicable without infrastructure expertise. Reference file has concrete Modal/Cloudflare code. |
| Modularity | 4/5 | Clear three-layer architecture: sandbox infrastructure, API layer, client interfaces. SKILL.md for concepts, infrastructure-patterns.md for implementation details, sandbox_manager.py for executable patterns. Each layer is independently implementable. |
| Safety | 4/5 | Explicitly addresses sandbox isolation (network restrictions, resource limits, blocked commands), token handling (short-lived installation tokens, encrypted user tokens), user-based commit attribution. Missing: secrets rotation, audit logging, sandbox escape detection. |

## Uu diem va Nhuoc diem

**Uu diem:**
- Image registry pattern with 30-minute cadence is the core insight: pre-build environments so users never wait for setup
- Predictive warm-up (start warming when user begins typing) is innovative UX optimization -- reduces perceived latency to near-zero
- Parallel file reading pattern: allow reads before git sync completes, block only writes -- critical for fast start
- Server-first architecture principle: agent framework as server, all UIs as thin clients -- enables multi-platform consistency
- "Code as source of truth" principle: agent reads its own source to prevent hallucination about capabilities
- Self-spawning agents with check-in capability enable agent-driven parallelism
- Multiplayer is "nearly free" with proper sync architecture -- data model must not tie sessions to single authors
- Slack integration as adoption flywheel: visibility creates virality loop
- Chrome extension DOM extraction instead of screenshots reduces token usage while maintaining precision

**Nhuoc diem:**
- Very infrastructure-focused -- limited value for single-developer or small-team workflows
- Missing cost analysis: what does maintaining warm pools cost? What's the ROI at different team sizes?
- Platform-specific details (Modal, Cloudflare Durable Objects) may not transfer to other infrastructure
- sandbox_manager.py is well-structured but most methods have `pass` implementations -- more pseudocode skeleton than working code
- No discussion of multi-region deployment or edge cases (sandbox failure during execution, network partitions)
- Missing observability: how to debug a failed sandbox session, log aggregation patterns
- Follow-up message handling (queue vs insert) discussion is superficial

## References Insights

**Insight 1 - Cloudflare Durable Objects for Per-Session State (from infrastructure-patterns.md):**
The infrastructure-patterns reference provides a complete TypeScript `SessionDO` (Durable Object) implementation with per-session SQLite, WebSocket broadcasting, and three tables (messages, artifacts, events). This architecture ensures zero cross-session interference because each session gets its own Durable Object with isolated storage. SKILL.md mentions "dedicated database per session (SQLite per session works well)" but doesn't show the WebSocket + SQL + event broadcasting integration. The Durable Object pattern also supports hibernation APIs to reduce compute costs during idle periods -- a cost optimization not covered in SKILL.md.

**Insight 2 - Chrome Extension React Fiber Extraction (from infrastructure-patterns.md):**
The infrastructure-patterns reference shows how to extract React component names from the DOM using `__reactFiber` keys on DOM elements. This extracts `fiber.type.name` or `fiber.type.displayName` to get the actual React component name, not just the HTML element. Combined with bounding rect information, this gives the agent precise component-level understanding of the UI without sending expensive screenshot images. SKILL.md mentions "DOM and React internals extraction instead of raw images" but doesn't detail the __reactFiber inspection technique. This technique reduces token usage by 10-50x compared to sending screenshots.

**Insight 3 - SandboxSecurityConfig as Declarative Policy (from infrastructure-patterns.md):**
The reference defines a `SandboxSecurityConfig` class with declarative security policies: allowed_hosts whitelist (github.com, registry.npmjs.org, pypi.org), resource limits (4GB memory, 2 CPU cores, 10GB disk, 4-hour runtime), secrets injection list, and blocked commands (curl, wget, ssh). This declarative approach to sandbox security is more maintainable than imperative checks scattered through code. SKILL.md discusses authentication but doesn't provide this security-as-configuration pattern.

**Insight 4 - Metrics Aggregation with Repository Breakdown (from infrastructure-patterns.md):**
The `MetricsAggregator` class provides `get_repository_metrics()` that calculates `agent_pr_percentage` (agent-written PRs / total PRs * 100) per repository. This is a powerful adoption metric: tracking what percentage of a codebase's PRs come from the agent system. It also tracks `avg_prompts_per_session` for efficiency analysis. SKILL.md mentions "agent-written code percentage across repositories" as a metric but doesn't show the per-repo breakdown implementation.

## Cross-skill Comparison

**hosted-agents vs multi-agent-patterns:**
hosted-agents provides the INFRASTRUCTURE for running agents, while multi-agent-patterns provides the COORDINATION logic between agents. Self-spawning agents in hosted-agents follow the supervisor pattern from multi-agent-patterns. The warm pool manager in hosted-agents is the infrastructure that makes multi-agent-patterns' parallel execution feasible at scale. They are complementary: multi-agent-patterns defines the "what" of agent coordination, hosted-agents defines the "how" of running it.

**hosted-agents vs filesystem-context:**
filesystem-context focuses on using the filesystem as a context engineering tool within a single agent's session. hosted-agents extends this by providing the sandboxed filesystem WHERE that context lives. The snapshot/restore mechanism in hosted-agents preserves filesystem-context state across sessions. The parallel read/blocked write pattern in hosted-agents' AgentSession is a filesystem-context access pattern specific to distributed environments.

**Unique strength of hosted-agents:**
The predictive warm-up pattern (start sandbox preparation when user begins typing) is unique in the collection and represents a UX-first approach to infrastructure design. No other skill addresses the user-facing latency problem with this level of proactive optimization.

## Tich hop

- **OMC team workers**: OMC's team pipeline already uses background Claude agents. hosted-agents patterns (warm pool, snapshot/restore) could optimize team worker startup times
- **Background coding agents**: The sandbox manager pattern is directly applicable when OMC needs to run agents on remote infrastructure rather than local machine
- **Slack/Discord integration**: Repository classification pattern for OMC's remote agent control via chat platforms
- **Server-first principle**: Relevant to OMC's architecture -- OMC already functions as a server with CLI as client
- **Self-spawning**: OMC's `/team` skill already implements agent-spawned sessions; hosted-agents provides the infrastructure patterns to scale this
- **Multiplayer**: Potential for collaborative OMC sessions with multiple team members interacting with the same agent workspace

## Ghi chu ky thuat

- Version 1.0.0, created 2026-01-12. 279 lines in SKILL.md.
- Primary source: Ramp's blog post "Why We Built Our Own Background Agent" -- real production experience.
- sandbox_manager.py (495 lines): SandboxState enum, ImageBuilder, WarmPoolManager, SandboxManager, AgentSession classes. Well-typed with dataclasses but many methods are stubs (`pass`).
- infrastructure-patterns.md (700 lines): Concrete Modal integration, Cloudflare Durable Objects (TypeScript), Slack bot with repository classification, Chrome extension DOM extraction, multiplayer session management, security config, token management.
- Key technologies referenced: Modal Sandboxes, Cloudflare Durable Objects, Cloudflare Agents SDK, GitHub Apps, Slack Bolt, Chrome Extension APIs, OpenCode.
- The 30-minute image build cadence is a pragmatic choice: frequent enough that git sync is fast, infrequent enough that build costs are manageable.
- Primary success metric: "sessions resulting in merged PRs" -- not session count, not PR count, but MERGED PRs.
- Dependency: multi-agent-patterns (coordination), tool-design (agent tools), context-optimization (distributed context), filesystem-context (session state).
