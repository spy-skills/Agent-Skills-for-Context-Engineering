---
date: 2026-03-26
type: learning
task: ceskills-449
tags: [priority, ranking, learning-path, skills, rubric]
verified: true
---

# Xếp hạng ưu tiên — 2 Audiences (Verified)

## Rubric Data (từ Tổng quan task)

| Skill | C | U | M | S | Avg |
|-------|---|---|---|---|-----|
| context-fundamentals | 4 | 5 | 5 | 5 | 4.8 |
| tool-design | 5 | 5 | 4 | 4 | 4.5 |
| filesystem-context | 5 | 5 | 4 | 4 | 4.5 |
| context-degradation | 5 | 4 | 4 | 5 | 4.4 |
| multi-agent-patterns | 5 | 4 | 4 | 4 | 4.3 |
| project-development | 5 | 5 | 3 | 4 | 4.3 |
| memory-systems | 4 | 4 | 5 | 4 | 4.3 |
| evaluation | 4 | 4 | 4 | 4 | 4.0 |
| context-compression | 4 | 3 | 4 | 5 | 3.9 |
| advanced-evaluation | 5 | 3 | 4 | 4 | 3.9 |
| context-optimization | 3 | 3 | 4 | 4 | 3.5 |
| hosted-agents | 4 | 3 | 3 | 4 | 3.5 |
| bdi-mental-states | 5 | 2 | 3 | 4 | 3.3 |

---

## Ranking A: Người mới bắt đầu (Agent Builder beginner)

Tieu chi: Usability (cao) + foundational concepts + quick wins.

| Hạng | Skill | Score | Lý do học trước |
|------|-------|-------|----------------|
| 1 | **context-fundamentals** | 4.8 | BẮT BUỘC — mọi thứ build trên đây. Dễ hiểu, 185 dòng, không cần prerequisite |
| 2 | **context-degradation** | 4.4 | Hiểu WHY context fails trước khi optimize. RULER data cụ thể, model thresholds làm reference |
| 3 | **filesystem-context** | 4.5 | 6 patterns ÁP DỤNG NGAY: scratch pad, plan persistence. Code examples runnable |
| 4 | **tool-design** | 4.5 | Viết tool descriptions tốt hơn NGAY. Consolidation principle dễ nhớ, MCP naming practical |
| 5 | **project-development** | 4.3 | Pipeline methodology cho BẤT KỲ LLM project. Manual prototype step saves hours |
| 6 | **evaluation** | 4.0 | "95% Finding" thay đổi cách nghĩ. Base cho quality gates |
| 7 | **memory-systems** | 4.3 | Biết khi nào KHÔNG cần complex memory (filesystem beats Mem0!) |
| 8 | **context-compression** | 3.9 | Cần khi sessions dài, nhưng không urgent cho beginner |
| 9 | **multi-agent-patterns** | 4.3 | Quan trọng nhưng complexity cao. Học sau khi single-agent vững |
| 10 | **context-optimization** | 3.5 | Overlap compression. Học khi cần optimize cụ thể |
| 11 | **advanced-evaluation** | 3.9 | Dense (454 dòng). Chỉ cần khi build eval pipeline |
| 12 | **hosted-agents** | 3.5 | Infrastructure. Chỉ cần khi scale production |
| 13 | **bdi-mental-states** | 3.3 | Niche research. Skip trừ khi làm cognitive AI |

### Learning Path (Beginner)
```
Week 1: context-fundamentals → context-degradation → filesystem-context
Week 2: tool-design → project-development
Week 3: evaluation → memory-systems
Khi cần: compression → multi-agent → optimization → advanced-eval
Skip: hosted-agents, bdi-mental-states (trừ khi relevant)
```

---

## Ranking B: Người có kinh nghiệm (Agent Architect)

Tiêu chí: Completeness (cao) + architectural depth + production patterns.

| Hạng | Skill | Score | Lý do học trước |
|------|-------|-------|----------------|
| 1 | **multi-agent-patterns** | 4.3 | Token economics data + telephone game fix → trực tiếp cải thiện existing systems |
| 2 | **tool-design** | 4.5 | Architectural reduction (Vercel 17→2) → challenge your existing tool design |
| 3 | **context-compression** | 3.9 | Tokens-per-task metric + artifact trail problem → production-critical |
| 4 | **memory-systems** | 4.3 | Benchmark comparison (Zep 94.8% vs Mem0 68.5%) → inform architecture decisions |
| 5 | **advanced-evaluation** | 3.9 | LLM-as-Judge pipeline + bias mitigation → production eval system |
| 6 | **context-degradation** | 4.4 | Model-specific thresholds + counterintuitive findings → debug existing issues |
| 7 | **filesystem-context** | 4.5 | Sub-agent file communication pattern → improve multi-agent coordination |
| 8 | **project-development** | 4.3 | Architectural reduction + iteration methodology → validate your approach |
| 9 | **hosted-agents** | 3.5 | Infrastructure patterns cho production deployment |
| 10 | **context-fundamentals** | 4.8 | Review — likely already know, but attention budget framing valuable |
| 11 | **evaluation** | 4.0 | Base concepts — skip if already have eval framework |
| 12 | **context-optimization** | 3.5 | Specific techniques — read when optimizing |
| 13 | **bdi-mental-states** | 3.3 | Research direction — interesting for explainable AI |

### Learning Path (Experienced)
```
Day 1: multi-agent-patterns → tool-design (challenge existing design)
Day 2: context-compression → memory-systems (production patterns)
Day 3: advanced-evaluation → context-degradation (debug + eval)
Reference: filesystem-context, project-development, hosted-agents
Skip: context-fundamentals (review), evaluation (base), context-optimization (overlap)
```

---

## So sánh 2 Rankings

| Skill | Beginner | Experienced | Delta | Ghi chú |
|-------|----------|-------------|-------|---------|
| context-fundamentals | #1 | #10 | -9 | Beginner cần nền tảng, experienced đã biết |
| multi-agent-patterns | #9 | #1 | +8 | Experienced cần architectural patterns |
| context-compression | #8 | #3 | +5 | Production-critical cho experienced |
| advanced-evaluation | #11 | #5 | +6 | Eval pipeline quan trọng cho production |
| tool-design | #4 | #2 | +2 | Quan trọng cho cả 2, nhưng experienced cần sâu hơn |
| evaluation | #6 | #11 | -5 | Beginner cần base, experienced skip |

## Ghi chú
- Rankings dựa trên rubric scores + context-specific weighting
- Beginner = chưa từng build agent system, đang học concepts
- Experienced = đã có production agent system, muốn improve
- Cả 2 rankings đều skip bdi-mental-states (niche) và context-optimization (overlap)
