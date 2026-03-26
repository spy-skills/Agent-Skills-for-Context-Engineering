---
date: 2026-03-26
type: learning
task: ceskills-ty0
tags: [context-fundamentals, foundational, attention-budget, progressive-disclosure, context-components]
---

# context-fundamentals -- Deep Dive Analysis

## Rubric Scores

### Completeness: 4/5
Covers all 5 context components (system prompts, tool definitions, retrieved docs, message history, tool outputs) with clear definitions and relationships. Includes attention mechanics, progressive disclosure, and context budgeting. Deducted 1 point for missing model-specific differences and lack of benchmark data on performance impact of different context sizes.

### Usability: 4/5
Two concrete examples (system prompt organization with XML tags, progressive document loading). Guidelines section provides 8 actionable rules. The "right altitude" concept is immediately applicable. Deducted 1 point because examples are relatively simple -- missing a complex real-world scenario showing context budgeting in action.

### Modularity: 5/5
Clean foundational skill with no unnecessary dependencies. Clear integration paths to 4 downstream skills (context-degradation, context-optimization, multi-agent-patterns, tool-design). References section well-organized with internal and external pointers. Works as standalone knowledge unit.

### Safety: 3/5
Mentions context limits and compaction triggers at 70-80% utilization but lacks explicit warnings about failure modes. No discussion of what happens when context budgeting fails, no graceful degradation patterns (those live in context-degradation). The script uses simplified token estimation (len/4) without sufficient warnings about production inaccuracy.

## Uu diem

- **Attention budget metaphor** cuc ky truc quan -- giai thich n^2 relationships de hieu, tao mental model cho moi concept tiep theo
- **Progressive disclosure principle** ap dung duoc NGAY vao moi agent system (CLAUDE.md, skills, tools)
- **"Right altitude" concept** cho system prompts -- can bang giua qua cu the (brittle) vs qua chung (vague), voi 3 vi du cu the (Too Low/Too High/Optimal)
- **Context quality > quantity** -- "smallest possible set of high-signal tokens" la golden rule
- **Tool outputs = 83.9% context usage** -- data point dinh huong optimization
- Script `context_manager.py` cung cap ContextBuilder class voi priority-based section management va usage reporting

## Nhuoc diem

- Thieu benchmark data cu the (khong co con so performance degradation)
- Khong co anti-patterns section (chi co guidelines chung)
- Examples hoi don gian -- thieu complex real-world scenario
- Khong de cap den model-specific differences (Claude vs GPT vs Gemini)
- Script dung len(text)//4 cho token estimation -- sai lech dang ke voi actual tokenization, dac biet cho code va non-English text
- Thieu visual diagrams cho attention distribution

## References Insights

### Insight 1: Document Chunking Strategy (tu context-components.md)
Reference cung cap pseudocode cho semantic chunking -- `find_section_headers()`, `find_paragraph_breaks()`, `find_logical_breaks()` -- voi explicit constraint ve MIN_CHUNK_SIZE va MAX_CHUNK_SIZE. SKILL.md chi noi chung ve "retrieved documents" nhung khong di sau vao chunking strategy. Day la gap quan trong vi chunking quality truc tiep anh huong den retrieval quality va context efficiency.

### Insight 2: Observation Masking Pattern (tu context-components.md)
Reference mo ta cu the observation masking: khi tool output vuot max_length, thay bang compact reference ID va luu full output rieng. Pattern nay: `store_observation(output)` -> return `[Previous observation elided. Full content stored at reference {id}]`. SKILL.md chi nhac den observation masking nhu 1 concept, nhung reference cho implementation pattern cu the voi `mask_observation()` function. Day la bridge truc tiep sang filesystem-context skill.

### Insight 3: Summary Injection Pattern (tu context-components.md)
Reference cung cap `inject_summaries()` function voi `summary_interval=20` turns, inject system-role summary messages vao conversation history. Pattern nay khong duoc mention trong SKILL.md nhung la practical implementation cua "message history management" concept.

## Cross-skill Comparison

### vs context-degradation
context-fundamentals giai thich WHAT context is va HOW it works. context-degradation giai thich WHY context fails. Fundamentals cung cap vocabulary (attention budget, progressive disclosure) ma degradation skill su dung de mo ta failure patterns (lost-in-middle, context poisoning). Fundamentals la prerequisite bat buoc.

### vs context-optimization
Fundamentals dinh nghia van de (finite resource, diminishing returns). Optimization cung cap solutions (compaction, caching, partitioning). Fundamentals mention "compaction triggers at 70-80%" nhung khong giai thich HOW to compact -- do la viec cua optimization.

### vs tool-design
Fundamentals noi tool definitions "live near the front of context" va "collectively steer agent behavior." Tool-design skill di sau vao consolidation principle, description engineering, va architectural reduction. Fundamentals cung cap context cho viec hieu tai sao tool design matters (tool descriptions consume attention budget).

### vs filesystem-context
Fundamentals introduce "just-in-time approach" va "file system provides structure." Filesystem-context skill bien concepts nay thanh 6 concrete patterns (scratch pad, plan persistence, sub-agent comms, dynamic loading, terminal persistence, self-modification). Fundamentals la theory, filesystem-context la implementation.

## Tich hop vao he thong hien tai

- **CLAUDE.md**: Restructure theo XML sections (BACKGROUND, INSTRUCTIONS, TOOL_GUIDANCE, OUTPUT) -- ap dung truc tiep "right altitude" concept
- **OMC skills**: Load skill names first, full content on activation -- dang lam dung progressive disclosure
- **MCP servers**: Tool descriptions nen answer 4 cau hoi (what, when, inputs, returns) -- audit tat ca MCP tool descriptions
- **Memory system**: "Attention-favored positions" = dat critical info o dau/cuoi context
- **Token budgeting**: Implement ContextBuilder pattern cho context assembly, voi priority-based inclusion va usage reporting
- **Observation masking**: Ap dung cho tool outputs > 2000 tokens -- bridge sang filesystem-context scratch pad pattern

## Ghi chu ky thuat

- Version 1.0.0 (Dec 2025) -- foundational, stable
- Prerequisites: None (day la root skill)
- Leads to: context-degradation -> context-optimization -> multi-agent-patterns -> tool-design
- Key metric: Tool outputs = 83.9% context -> focus optimization here
- Token estimation: ~4 chars/token cho English, but actual varies by model and content type
- Context budget allocation: System prompt 500-2000, Tool defs 100-500/tool, Retrieved docs variable (largest consumer), Message history variable (grows), Tool outputs variable (can dominate)
- Script provides 3 utilities: ContextBuilder (priority-based assembly), ProgressiveDisclosureManager (lazy loading), validate_context_structure (duplicate/empty detection)
- Reference cung cap implementation cho: section structure, altitude calibration, tool schema, document chunking, message history management, observation masking, context budget estimation, progressive disclosure
