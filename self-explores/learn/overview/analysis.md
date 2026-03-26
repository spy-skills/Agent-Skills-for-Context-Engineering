---
date: 2026-03-26
type: learning
task: ceskills-8pl
tags: [overview, skills, context-engineering, categorization, rubric]
verified: true
---

# Phân tích Tổng quan — Context Engineering Skills (Verified)

## Thống kê (verified bằng wc -l, 2026-03-26)

| Metric | Giá trị |
|--------|---------|
| Tổng skills | 13 (verified: `ls skills/ \| wc -l`) |
| Tổng dòng SKILL.md | 3,518 (verified: `wc -l`) |
| Trung bình/skill | 271 dòng |
| Skills có scripts/ | 12/13 (**bdi-mental-states thiếu**) |
| Skills có references/ | 13/13 |
| Tổng reference files | 21 |
| Tổng examples | 5 |

## Phân nhóm — README chính thức (5 nhóm)

Đối chiếu trực tiếp từ README.md "Skills Overview" section:

### 1. FOUNDATIONAL (3 skills)
| Skill | Lines | Refs | Scripts | Mô tả README |
|-------|-------|------|---------|--------------|
| context-fundamentals | 185 | 1 | 1 | Understand what context is, anatomy of context |
| context-degradation | 231 | 1 | 1 | Patterns of context failure: lost-in-middle, poisoning, distraction, clash |
| context-compression | 265 | 1 | 1 | Compression strategies for long-running sessions |

### 2. ARCHITECTURAL (5 skills)
| Skill | Lines | Refs | Scripts | Mô tả README |
|-------|-------|------|---------|--------------|
| multi-agent-patterns | 255 | 1 | 1 | Orchestrator, peer-to-peer, hierarchical architectures |
| memory-systems | 186 | 1 | 1 | Short-term, long-term, graph-based memory |
| tool-design | 311 | 2 | 1 | Build tools agents can use effectively |
| filesystem-context | 321 | 1 | 1 | Dynamic context discovery, tool output offloading |
| hosted-agents | 279 | 1 | 1 | **NEW** Background coding agents, sandboxed VMs |

### 3. OPERATIONAL (3 skills)
| Skill | Lines | Refs | Scripts | Mô tả README |
|-------|-------|------|---------|--------------|
| context-optimization | 179 | 1 | 1 | Compaction, masking, caching strategies |
| evaluation | 231 | 1 | 1 | Evaluation frameworks for agent systems |
| advanced-evaluation | 454 | 3 | 1 | LLM-as-Judge: direct scoring, pairwise, bias mitigation |

### 4. DEVELOPMENT METHODOLOGY (1 skill)
| Skill | Lines | Refs | Scripts | Mô tả README |
|-------|-------|------|---------|--------------|
| project-development | 342 | 2 | 1 | LLM projects from ideation through deployment |

### 5. COGNITIVE ARCHITECTURE (1 skill)
| Skill | Lines | Refs | Scripts | Mô tả README |
|-------|-------|------|---------|--------------|
| bdi-mental-states | 295 | 4 | **0** | **NEW** BDI ontology for agent mental states |

**Tổng kiểm chứng:** 3+5+3+1+1 = 13 skills. Khớp với `ls skills/ | wc -l`.

## Rubric đánh giá — 4 tiêu chí × thang 1-5

| Tiêu chí | Trọng số | Mô tả |
|----------|----------|-------|
| **Completeness** (C) | 25% | Bao quát edge cases, có depth, examples, references |
| **Usability** (U) | 30% | Dễ áp dụng, workflow rõ, actionable cho người mới |
| **Modularity** (M) | 20% | Dễ tách rời, tái sử dụng, ít phụ thuộc cứng |
| **Safety** (S) | 25% | Theo Anthropic guidelines, có caveats, không mislead |

### Bảng điểm

| # | Skill | C | U | M | S | Avg | Justification |
|---|-------|---|---|---|---|-----|---------------|
| 1 | context-fundamentals | 4 | 5 | 5 | 5 | 4.8 | Nền tảng rõ, dễ hiểu, ít dependency, caveats tốt |
| 2 | tool-design | 5 | 5 | 4 | 4 | 4.5 | Case study mạnh (Vercel), consolidation principle actionable |
| 3 | filesystem-context | 5 | 5 | 4 | 4 | 4.5 | 6 patterns concrete với code, self-modification có caveat |
| 4 | context-degradation | 5 | 4 | 4 | 5 | 4.4 | RULER data + model thresholds, caveats rõ (data có thể outdated) |
| 5 | multi-agent-patterns | 5 | 4 | 4 | 4 | 4.3 | Token economics + telephone game fix, framework comparison |
| 6 | project-development | 5 | 5 | 3 | 4 | 4.3 | Pipeline methodology hoàn chỉnh, 2 case studies, phụ thuộc nhiều skill khác |
| 7 | memory-systems | 4 | 4 | 5 | 4 | 4.3 | Benchmark data mới (Feb 2026), framework comparison honest |
| 8 | context-compression | 4 | 3 | 4 | 5 | 3.9 | Tokens-per-task insight hay, nhưng thiếu implementation code |
| 9 | advanced-evaluation | 5 | 3 | 4 | 4 | 3.9 | Rất comprehensive (454 dòng) nhưng dense, khó cho người mới |
| 10 | evaluation | 4 | 4 | 4 | 4 | 4.0 | "95% Finding" hay, base cho advanced-eval |
| 11 | context-optimization | 3 | 3 | 4 | 4 | 3.5 | Overlap với compression, thiếu implementation examples |
| 12 | hosted-agents | 4 | 3 | 3 | 4 | 3.5 | Infrastructure-specific, ít áp dụng ngay cho solo dev |
| 13 | bdi-mental-states | 5 | 2 | 3 | 4 | 3.3 | Rất sâu (4 refs) nhưng niche, cần RDF/ontology knowledge |

## Examples (5 projects)

| # | Example | Skills áp dụng | Status |
|---|---------|----------------|--------|
| 1 | book-sft-pipeline | project-dev, compression, multi-agent, eval | Design + code (runnable with Tinker) |
| 2 | digital-brain-skill | fundamentals, memory, tool-design, optimization | Design + data structures |
| 3 | interleaved_thinking | degradation, eval, fundamentals | Design + Python code |
| 4 | llm-as-judge-skills | advanced-eval, eval | **Production-ready** (19 tests, TypeScript) |
| 5 | x-to-book-system | multi-agent, memory, optimization, tool-design, eval | Design only (PRD) |

## Ưu điểm chung
1. **Research-backed** — Mỗi skill trích dẫn papers cụ thể (MT-Bench, RULER, BrowseComp)
2. **Production evidence** — Case studies thực tế (Netflix, Vercel d0, Karpathy HN)
3. **Consistent format** — SKILL.md + references/ + scripts/ đồng nhất 13/13
4. **Progressive disclosure** — Collection SKILL.md → individual SKILL.md → references/
5. **Actionable** — Pseudo-code, decision frameworks, anti-patterns trong hầu hết skills
6. **Academic recognition** — Cited in Peking University 2026 paper

## Nhược điểm
1. **context-optimization overlap** với context-compression — ranh giới chưa rõ
2. **bdi-mental-states** thiếu scripts/ — skill duy nhất không có executable code
3. **Cross-skill integration** yếu — mỗi skill tương đối độc lập, ít cross-reference sâu
4. **Examples không đồng đều** — llm-as-judge-skills production-ready vs x-to-book design-only
5. **Model thresholds** trong context-degradation có thể outdated nhanh

## Khả năng tích hợp vào hệ thống hiện tại
- **OMC team**: multi-agent-patterns (telephone game fix), tool-design (consolidation)
- **/viec**: filesystem-context (worklog = scratch pad pattern), context-compression (session continuity)
- **MCP servers**: tool-design (audit descriptions, MCP naming)
- **CLAUDE.md**: context-fundamentals (attention-favored positions, progressive disclosure)
- **Memory**: memory-systems (validate filesystem approach — 74% > 68.5%)
- **Quality**: evaluation + advanced-evaluation (rubric-based review gates)
