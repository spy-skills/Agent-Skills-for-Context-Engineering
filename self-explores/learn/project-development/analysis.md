---
date: 2026-03-26
type: learning
task: ceskills-6mb
tags: [project-development, pipeline, task-model-fit, case-study, Karpathy, Vercel-d0, Manus, Anthropic, architectural-reduction, batch-processing]
---

# project-development

## Score

| Rubric | Score | Justification |
|--------|-------|---------------|
| **Completeness** | 5/5 | Full lifecycle coverage: task-model fit -> manual prototype -> architecture selection -> cost estimation -> pipeline implementation -> iteration. 4 production case studies (Karpathy, Vercel d0, Manus, Anthropic). Most comprehensive methodology skill. |
| **Usability** | 5/5 | Immediately actionable: task-model fit matrix (6 suited + 6 unsuited), "3-minute manual prototype" rule, pipeline template script with CLI, cost estimation formula. Can be applied to ANY LLM project today. |
| **Modularity** | 4/5 | SKILL.md (methodology), case-studies.md (4 detailed cases), pipeline-patterns.md (detailed patterns with code), pipeline_template.py (working CLI template). Slight coupling: case studies and pipeline patterns have some overlap in content. |
| **Safety** | 3/5 | Cost estimation with 20-30% buffer, graceful degradation in parsing, idempotent stages prevent data loss. Missing: security considerations for file system state, API key management, data validation between stages, PII in prompts. |

**Overall: 4.25/5**

## Uu diem

- **Task-model fit matrix** saves hours: 6 suited (synthesis, subjective judgment, NL output, error tolerance, batch, domain knowledge) vs 6 unsuited (precise computation, real-time, perfect accuracy, proprietary data, sequential deps, deterministic output)
- **Manual prototype golden rule**: "Copy one input -> test with model -> takes MINUTES, prevents HOURS" — most high-ROI practice in the collection
- **Canonical pipeline**: acquire -> prepare -> process -> parse -> render — only Stage 3 is expensive + non-deterministic
- **File system as state machine**: file existence = stage completion — elegant idempotency, debugging by reading files
- **4 production case studies** with real numbers:
  - Karpathy: $58 total, 930 items, 1 hour, 15 workers
  - Vercel d0: 17 tools -> 2 tools, 80% -> 100% success, 274s -> 77s
  - Manus: 5 refactors, KV-cache as #1 metric, todo.md recitation trick
  - Anthropic: 95% variance = token usage (80%) + tool calls + model choice
- **Architectural reduction evidence**: "Removing specialized tools often IMPROVES performance" — counter-intuitive but proven
- **Pipeline template script**: 678 lines, complete CLI with acquire/prepare/process/parse/render/clean/estimate stages

## Nhuoc diem

- **Monitoring/alerting** absent for production pipelines — no patterns for tracking pipeline health
- **A/B testing** for prompts not covered — critical for iterating prompt quality at scale
- **Deployment strategies** missing (blue-green, canary, rollback for pipeline changes)
- **Error handling between stages** underdeveloped — what happens when parse fails for 30% of items?
- **Data validation** between stages not formalized — how to verify Stage 3 output before Stage 4 consumes it
- **Security**: no mention of API key management, PII scrubbing, or access control for file system state

## References Insights

### Insight 1: Manus KV-Cache as #1 Production Metric
Case-studies.md reveals Manus's core insight: KV-cache hit rate is the single most important metric for production agents. Cached tokens cost $0.30/MTok vs $3.00/MTok uncached (10x difference). With 100:1 input:output ratio in agentic workloads, cache optimization dominates cost. The SKILL.md mentions cost estimation but does NOT highlight KV-cache as a cost lever. Specific anti-pattern: putting timestamps at the beginning of system prompts kills cache hit rate entirely. Specific technique: "mask, do not remove" tools mid-iteration — use logit masking during decoding instead of modifying tool definitions.

### Insight 2: Manus Recitation Pattern for Attention Management
Case-studies.md documents Manus's `todo.md` recitation technique: the agent creates a todo.md file and rewrites it step-by-step, pushing the global plan into the model's recent attention span. This is NOT just organization — it's an attention engineering technique that exploits the U-shaped attention curve (lost-in-middle). By constantly rewriting objectives at the END of context, the agent maintains goal alignment over 50+ tool calls. SKILL.md does not mention attention management at all.

### Insight 3: Anthropic's Parallel Tool Calling Pattern
Case-studies.md details Anthropic's multi-agent research system using 3-5 subagents in parallel with 3+ tools per subagent in parallel, cutting research time by up to 90%. Their prompting principle "let agents improve themselves" — Claude 4 models can diagnose prompt failures and suggest improvements, and tool-testing agents can rewrite tool descriptions. This self-improvement loop is absent from SKILL.md.

### Insight 4: Pipeline-Patterns PipelineCheckpoint for Long-Running Jobs
Pipeline-patterns.md provides `PipelineCheckpoint` class with `mark_complete()`, `mark_failed()`, and `get_remaining()` for resumable batch processing. This addresses the gap in SKILL.md where clean/retry is mentioned but checkpoint/resume for crashes is not. Combined with the `log_error()` pattern (JSONL error logging per item), this provides production-grade resilience.

## Cross-skill Comparison

### vs tool-design
- **Direct link**: Project-development's "architectural reduction" (Vercel d0: 17->2 tools) IS tool-design's consolidation principle in action.
- **Complementary**: Project-development says WHEN to reduce tools (when scaffolding constrains more than enables). Tool-design says HOW to design the remaining tools (clear descriptions, error messages, consistent naming).
- **Shared evidence**: Both reference Vercel d0 as key case study.

### vs evaluation
- **Dependency**: Project-development's "manual prototype step" is evaluation's simplest form. Both emphasize measuring output quality.
- **Gap**: Project-development mentions "track costs throughout development" but doesn't provide eval frameworks. Evaluation skill provides rubrics and LLM-as-judge patterns that should be used in project-development's parse stage.

### vs multi-agent-patterns
- **Sequential relationship**: Project-development covers "choosing single vs multi-agent" as architecture decision. Multi-agent-patterns provides the detailed patterns once multi-agent is chosen.
- **Shared data**: Both cite Anthropic's 95% finding (token usage = 80% variance) and token economics table.
- **Key difference**: Project-development frames multi-agent as context isolation decision; multi-agent-patterns provides implementation patterns.

### vs context-optimization
- **Manus bridge**: Manus case study in project-development references KV-cache optimization, observation masking, and todo.md recitation — all context-optimization techniques applied in project context.
- **Integration point**: Pipeline's process stage output (response.md) can be context-optimized before parse stage if responses are too large.

## Tich hop

- **Any LLM project**: Task-model fit check FIRST (3-minute manual prototype) — apply as gate before any AI task
- **Pipeline pattern**: Apply canonical pipeline to marketing content, research analysis, data processing workflows
- **File system state**: `self-explores/tasks/` already uses file-existence-as-state pattern
- **Cost estimation**: `items x tokens x price + 30% buffer` — apply to every AI task estimate
- **OMC autopilot**: Pipeline stages map cleanly: acquire=team-plan, prepare=team-prd, process=team-exec, parse=team-verify, render=team-fix
- **Architectural reduction**: Review OMC agent catalog periodically — are specialized agents constraining more than enabling?
- **Karpathy benchmark**: $0.06/item cost target for batch LLM processing
- **Manus KV-cache**: Apply to OMC system prompts — avoid timestamps at beginning, keep tool definitions stable
- **Checkpoint/resume**: OMC team state should implement PipelineCheckpoint pattern for crash recovery

## Ghi chu ky thuat

- Version 1.0.0 (Dec 2025) — comprehensive but could benefit from v2 update with monitoring patterns
- Most immediately actionable skill in collection — can be applied to any LLM project TODAY
- 4 case studies provide strongest evidence base of any skill
- Pipeline template script: 678 lines, full CLI, ready to customize for real projects
- Key metrics: Karpathy $58/930 items = $0.06/item; Vercel d0 274s->77s (3.5x faster), 80%->100% success
- Manus: 5 framework refactors since launch — "build for change"
- Bitter Lesson: "structures added for current model limitations become constraints as models improve"
- Parsing resilience: regex flexible, defaults for missing sections, log failures (don't crash)
- Structured output trick: "Follow this format exactly because I will be parsing it programmatically" — rationale disclosure improves compliance
