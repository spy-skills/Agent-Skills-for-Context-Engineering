---
date: 2026-03-26
type: learning
task: ceskills-rpp
tags: [context-compression, tokens-per-task, artifact-trail, structured-summary, probe-evaluation, anchored-iterative]
---

# context-compression -- Deep Dive Analysis

## Rubric Scores

### Completeness: 5/5
Covers 3 compression approaches (anchored iterative, opaque, regenerative) with quantitative comparison data. Introduces tokens-per-task metric, structured summary pattern with 5 explicit sections, probe-based evaluation with 4 probe types and 6 evaluation dimensions. Includes compression trigger strategies, three-phase workflow for large codebases, and honest admission of artifact trail weakness (2.2-2.5/5.0). The most data-rich skill in the collection.

### Usability: 5/5
Two detailed examples (debugging session compression with before/after, probe response quality comparison good vs poor). Structured summary template is copy-paste ready. Compression trigger table provides decision matrix. Evaluation framework is actionable with concrete rubrics. Highest practical value of all 5 skills analyzed.

### Modularity: 4/5
Connects cleanly to context-degradation (compression as mitigation), context-optimization (one technique among many), evaluation (probe-based testing), and memory-systems (scratchpad patterns). Deducted 1 because the evaluation framework is tightly coupled to the compression approaches -- could be extracted as standalone evaluation skill.

### Safety: 4/5
Explicitly warns about artifact trail weakness and re-fetching costs. "Tokens-per-task" metric prevents naive over-compression. Probe-based evaluation provides safety net for compression quality. Deducted 1 because opaque compression section lacks warnings about failure modes when reconstruction fails, and no rollback strategy is described.

## Uu diem

- **Tokens-per-task** paradigm shift: tong tokens cho TOAN BO task, bao gom re-fetching cost -- thay doi cach nghi ve optimization
- **3 production approaches** well-differentiated voi quantitative comparison (98.6% / 3.70 vs 99.3% / 3.35)
- **Structured summary sections** pattern practical -- Session Intent, Files Modified, Decisions, Current State, Next Steps
- **"Structure forces preservation"** -- sections = checklists summarizer MUST populate, prevent silent drift
- **Probe-based evaluation** -- functional quality > lexical similarity (ROUGE fails for compression)
- **Artifact trail problem** (2.2-2.5/5.0) honest admission voi solution hints
- **Compression ratio data**: 0.7% more tokens (anchored vs opaque) buys 0.35 quality points
- Script cung cap full ProbeGenerator, CompressionEvaluator, va StructuredSummarizer implementation

## Nhuoc diem

- Artifact trail WEAKNESS (2.2-2.5/5.0) acknowledged but not solved -- "likely requires specialized handling"
- Opaque compression approach hoi vague (no concrete implementation, chi concept)
- Missing benchmark for re-fetching cost savings (noi "20% more re-fetching" nhung khong co data backing)
- Script dung heuristic scoring thay vi actual LLM judge -- production gap
- Khong co rollback strategy khi compression mat critical info

## References Insights

### Insight 1: Blinding Protocol for Evaluation (tu evaluation-framework.md)
Reference explicitly states: "The judge should not know which compression method produced the response being evaluated. This prevents bias toward known methods." SKILL.md khong mention blinding -- day la critical methodological detail cho fair evaluation. Khi implement probe-based evaluation, phai ensure judge khong biet compression method de avoid confirmation bias. Day la research methodology insight anh huong den evaluation reliability.

### Insight 2: Statistical Robustness Data (tu evaluation-framework.md)
Reference cung cap sample size context: "36,611 messages across hundreds of compression points." Differences of 0.26-0.35 points are "consistent across task types and session lengths" -- holds for debugging, feature implementation, AND code review tasks. SKILL.md chi noi chung ve benchmark results nhung reference confirm robustness across diverse scenarios. Day la confidence booster cho viec adopt anchored iterative approach -- khong phai cherry-picked result.

### Insight 3: Weighted Criterion Scoring (tu evaluation-framework.md)
Reference cung cap detailed weights cho moi criterion: accuracy_factual (0.6) > accuracy_technical (0.4), artifact_files_modified (0.4) > artifact_files_created (0.3) = artifact_key_details (0.3), continuity_work_state (0.4) > continuity_todo_state (0.3) = continuity_reasoning (0.3). SKILL.md chi list 6 dimensions nhung reference cho weighting scheme reflects relative importance. File modifications matter MORE than file creations in artifact tracking -- practical prioritization.

## Cross-skill Comparison

### vs context-degradation
Compression la MOT TRONG BON buckets cua degradation's mitigation framework (Write/Select/Compress/Isolate). Degradation giai thich WHY compression can thiet (performance degrades with context growth). Compression giai thich HOW to compress voi 3 approaches va quantitative comparison. Degradation la diagnostic, compression la therapeutic.

### vs context-optimization
Optimization covers broader set of techniques (caching, partitioning, observation masking). Compression la deep dive vao 1 specific technique. Optimization noi "compress" -- compression skill noi "how to compress, measure quality, and choose approach." Compression adds tokens-per-task metric khong co trong optimization.

### vs filesystem-context
Filesystem-context's scratch pad pattern la lightweight form of "compression" -- write to file, return reference. Compression skill addresses the harder problem: SUMMARIZING context khi ca file reference khong du (context window full). Filesystem offloading avoids compression; compression handles when offloading isn't enough. Complementary approaches.

### vs context-fundamentals
Fundamentals noi "context is finite, treat as resource with diminishing returns." Compression provides concrete strategies for extending that finite resource. Fundamentals' "compaction triggers at 70-80%" directive la implemented bang compression's trigger strategies table.

## Tich hop vao he thong hien tai

- **OMC notepad**: Implement anchored iterative summarization -- update existing sections rather than regenerate
- **Session continuity**: Implement compaction trigger at 70% context, dung sliding window + structured summaries
- **Claude auto-compress**: Understand Claude's built-in compression has artifact trail weakness -> supplement voi explicit file tracking (separate artifact index)
- **Probe evaluation**: After compression, test voi recall/artifact/continuation/decision questions -> validate quality
- **/viec worklog**: Already uses structured summary pattern (Session Intent, Files Modified, etc.) -- validate against 6 dimensions
- **self-explores/tasks/**: Artifact trail tracking -- maintain separate files_modified section
- **Three-phase workflow**: Ap dung cho large codebase tasks -- Research -> Planning -> Implementation

## Ghi chu ky thuat

- Version 1.1.0 (Dec 2025) -- updated version indicates active development
- Key data: 0.7% more tokens (anchored vs opaque) buys 0.35 quality points -- worth the trade-off
- Sliding window + structured summaries = best balance for coding agents
- Artifact trail: separate tracking needed (not solved by any compression method, 2.2-2.5/5.0)
- Netflix 3-phase: 5M tokens -> 2,000 words spec -> implementation
- Sample size: 36,611 messages, consistent across task types
- Script provides: ProbeGenerator (extract facts/files/decisions from history), CompressionEvaluator (6-dimension rubric), StructuredSummarizer (anchored iterative implementation with merge logic)
- Evaluation weights: accuracy_factual 0.6, artifact_files_modified 0.4, continuity_work_state 0.4
- Blinding required for fair evaluation -- judge must not know compression method
