---
date: 2026-03-26
type: learning
task: ceskills-5nu
tags: [tool-design, consolidation, architectural-reduction, MCP, prompt-engineering, vercel-d0, self-optimization]
---

# tool-design -- Deep Dive Analysis

## Rubric Scores

### Completeness: 5/5
Covers tool-agent interface, consolidation principle, architectural reduction with production case study, description engineering (4 questions framework), response format optimization, error message design, MCP naming, self-optimizing tools pattern, and testing. Two reference documents add case study depth (Vercel d0) and best practices detail. 12 guidelines cover the full tool design lifecycle. Most comprehensive skill by line count (311 lines) with strongest evidence base.

### Usability: 5/5
Well-designed vs poorly-designed tool examples are immediately educational. Anti-patterns section provides clear "don't do this." Vercel d0 case study (17 tools -> 2 tools, 3.5x faster, +20% success) is compelling production evidence. MCP naming pattern (`ServerName:tool_name`) is copy-paste actionable. Tool selection framework provides step-by-step workflow.

### Modularity: 4/5
Connects to context-fundamentals (how tools interact with context), multi-agent-patterns (specialized tools per agent), and evaluation (tool testing). The architectural reduction reference could be a standalone skill. Deducted 1 because the skill covers both tool description engineering AND architectural philosophy -- these could be separate skills for clarity.

### Safety: 4/5
Error message design section explicitly addresses agent recovery. The consolidation principle prevents tool selection confusion. "When Not to Use" sections for both consolidation and reduction provide guardrails. Deducted 1 because architectural reduction section could be dangerous without sufficient caveats -- "trust model reasoning" advice could lead to unsafe tool access in production without sandbox constraints being emphasized more strongly.

## Uu diem

- **Consolidation Principle** -- "If human can't pick tool, agent can't either" -- golden rule, testable heuristic
- **Architectural Reduction** -- Vercel d0 evidence: 17 tools -> 2 primitives, 80% -> 100% success, 274s -> 77s, 37% fewer tokens
- **Tool descriptions = prompt engineering** -- paradigm shift: descriptions steer agent behavior, not just documentation
- **Self-optimizing tools** -- Agent analyzes failure -> improves descriptions -> 40% task completion time reduction
- **MCP naming** -- `ServerName:tool_name` pattern prevents "tool not found" errors
- **Anti-patterns** section ro rang voi examples tot/xau
- **Error message design** cho dual audience (developers + agents) voi structured JSON template
- **Response format optimization** -- concise/detailed options cho token efficiency

## Nhuoc diem

- Thieu TypeScript examples (MCP servers thuong viet TypeScript)
- Khong cover tool versioning / backward compatibility
- Response format section hoi mong -- could use more depth on when to use each format
- Self-optimization loop thieu implementation details (chi pseudocode)
- Architectural reduction could be over-applied without proper safety constraints
- Khong de cap tool testing automation (chi manual evaluation criteria)

## References Insights

### Insight 1: Detailed Failure Mode Analysis from Production (tu architectural_reduction.md)
Reference cung cap worst-case data cuc ky valuable: original architecture (17 tools) worst case = 724 seconds, 100 steps, 145,463 tokens, AND a failure. Reduced architecture (2 tools) solved SAME query in 141 seconds, 19 steps, 67,483 tokens, successfully. SKILL.md chi noi averages (274s vs 77s) nhung reference cho extreme case comparison. Day la evidence manh nhat cho architectural reduction -- worst case improvement is MORE dramatic than average case (5.1x faster, 2.1x fewer tokens, failure -> success).

### Insight 2: "Addition by Subtraction" Design Philosophy (tu architectural_reduction.md)
Reference articulate design principle: "The best agents may be the ones with the fewest tools. Every tool is a choice made for the model. Sometimes the model makes better choices when given primitive capabilities rather than constrained workflows." Va: "Invest in Context, Not Tooling -- The foundation matters more than clever tooling: clear file naming, well-structured documentation, consistent data organization." SKILL.md mentions architectural reduction nhung reference frames it as a DESIGN PHILOSOPHY -- not just a technique but a mindset shift from "build tools to help model" to "remove barriers to model reasoning."

### Insight 3: Evaluation Framework for Reduction Decisions (tu architectural_reduction.md)
Reference provides 5-point evaluation framework khi considering architectural reduction: (1) Maintenance overhead -- time maintaining tools vs improving outcomes, (2) Failure analysis -- failures from model limitations or tool constraints?, (3) Documentation quality -- could model navigate data layer directly?, (4) Constraint necessity -- protecting against real or hypothetical risks?, (5) Model capability -- has model improved since tools were designed? SKILL.md chi noi "question whether tools enable or constrain" nhung reference cho systematic evaluation checklist.

### Insight 4: Tool Design Checklist (tu best_practices.md)
Reference cung cap pre-deployment checklist: verify description states what+when, verify parameter names are descriptive with types, verify return values documented with structure+examples, verify error cases covered with actionable messages, verify naming conventions consistent, verify examples demonstrate common patterns, verify format options available. SKILL.md has guidelines nhung reference cho concrete verification checklist -- actionable quality gate.

## Cross-skill Comparison

### vs context-fundamentals
Fundamentals noi "tool definitions live near the front of context" va "collectively steer agent behavior." Tool-design explains WHY and HOW to optimize this steering. Fundamentals provides context theory, tool-design provides implementation practice. Tool descriptions consuming attention budget is a fundamentals concept; tool consolidation reducing that budget is a tool-design solution.

### vs multi-agent-patterns
Multi-agent gives each agent specialized tools. Tool-design's consolidation principle applies WITHIN each agent's tool set. Architectural reduction challenges whether specialized tools are needed at all -- maybe each agent just needs bash + domain-specific primitive. Tension between "specialized tools per agent" (multi-agent) and "fewer tools is better" (tool-design) resolved by context: consolidate within agent, specialize across agents.

### vs filesystem-context
Filesystem-context's Pattern 4 (Dynamic Skill Loading) is essentially tool-design's progressive disclosure applied to skills. Tool-design's architectural reduction pattern (file system agent) directly uses filesystem-context's core concept -- agents navigate filesystem with ls/grep/cat instead of custom tools. The two skills converge: filesystem IS the tool.

### vs context-fundamentals (attention budget)
Tool definitions consume 100-500 tokens each (from fundamentals' budget table). With 17 tools (original Vercel architecture), that's 1,700-8,500 tokens just for definitions. Reducing to 2 tools saves 1,500-7,500 tokens. This is a concrete example of fundamentals' "context quality > quantity" principle applied to tool design.

## Tich hop vao he thong hien tai

- **MCP servers**: Audit tat ca tool descriptions -> rewrite theo What/When/Inputs/Returns framework
- **OMC tools**: Review 30+ tools -> consolidate overlapping ones, apply consolidation principle
- **Trello MCP**: 30+ tools -> evaluate which can be consolidated (list_boards + get_board -> board_info?)
- **Notion MCP**: Review tool naming, ensure ServerName: prefix consistent
- **Agent catalog**: Moi agent description la effectively a tool description -> ap dung same 4-question framework
- **Self-optimization**: Implement failure tracking cho MCP tools -> analyze -> improve descriptions -> 40% improvement potential
- **Architectural reduction audit**: For each custom tool, ask 5-point evaluation (maintenance overhead, failure analysis, documentation quality, constraint necessity, model capability)
- **Error messages**: Restructure MCP tool errors voi structured JSON (error code, message, resolution, example)

## Ghi chu ky thuat

- Version 1.1.0 (Dec 2025) -- updated, indicates active refinement
- Key insight: 10-20 tools optimal cho hau het applications, namespace khi > 20
- MCP: Always use `ServerName:tool_name` format -- prevents tool not found errors
- Consolidation test: "If human can't pick which tool, agent can't either"
- Architectural reduction: 17 -> 2 tools, worst case 724s/failure -> 141s/success
- Self-optimization loop: agent -> failures -> analysis -> improved descriptions -> 40% faster
- Design philosophy: "Addition by subtraction" -- fewer tools = better agent decisions
- Pre-deployment checklist: 7 verification points from best_practices.md
- Script provides: ToolDescriptionEvaluator (clarity/completeness/accuracy/actionability/consistency), ToolSchemaBuilder (fluent API for schema construction), ErrorMessageGenerator (NOT_FOUND/INVALID_INPUT/RATE_LIMITED templates)
- Two references: best_practices.md (guidelines + checklist) and architectural_reduction.md (Vercel d0 case study)
