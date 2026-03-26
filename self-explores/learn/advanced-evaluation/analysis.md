---
date: 2026-03-26
type: learning
task: ceskills-59e
tags: [advanced-evaluation, LLM-as-Judge, bias-mitigation, pairwise-comparison, rubric-generation, PoLL, confidence-calibration]
---

# advanced-evaluation - Deep Analysis

## Score

| Rubric | Score | Justification |
|--------|-------|---------------|
| Completeness | 5/5 | Covers the full LLM-as-Judge landscape: direct scoring, pairwise comparison, rubric generation, 5 bias types with mitigation, confidence calibration, pipeline architecture, scaling strategies (PoLL, hierarchical), anti-patterns. References provide deep implementation code. 454 lines + 3 reference files. |
| Usability | 4/5 | Strong prompt templates, decision trees, and code examples. evaluation_example.py demonstrates all three patterns with runnable code. However, examples are largely pseudocode and require real API integration to be production-ready. Anti-patterns section is immediately actionable. |
| Modularity | 5/5 | Excellent separation: SKILL.md for concepts/guidelines, bias-mitigation.md for bias-specific techniques, implementation-patterns.md for pipeline patterns, metrics-guide.md for metric selection. Each reference stands alone as a usable resource. |
| Safety | 4/5 | Explicitly addresses self-enhancement bias (cross-model evaluation), authority bias (evidence requirements), and includes BiasMonitor for systematic detection. Missing: adversarial prompt injection in evaluation prompts, privacy concerns when evaluating user data, cost guardrails for PoLL approaches. |

## Uu diem va Nhuoc diem

**Uu diem:**
- Evaluation taxonomy (direct scoring vs pairwise comparison) with clear decision tree -- immediately applicable
- Five bias types comprehensively covered with primary AND secondary mitigations: position (swap + multi-shuffle), length (explicit prompting + normalization), self-enhancement (cross-model + anonymization), verbosity (relevance weighting + rubric penalties), authority (evidence requirement + fact-checking)
- Chain-of-thought requirement for scoring improves reliability by 15-25% -- research-backed
- Rubric generation reduces variance by 40-60% -- quantified claim
- PoLL (Panel of LLM Judges) pattern for high-stakes evaluation
- Hierarchical evaluation (cheap screen -> expensive detail -> human review) for cost efficiency
- Pipeline architecture diagram with clear layer separation
- Research-grounded: MT-Bench (Zheng 2023), G-Eval (Liu 2023), Wang 2023

**Nhuoc diem:**
- Dense at 454 lines -- longest skill in the collection, may overwhelm during first read
- Use case narrower than foundational skills -- specifically about LLM evaluation, not general agent evaluation
- evaluation_example.py uses simulated outputs rather than real API calls
- Missing cost modeling: PoLL with 3 models costs 3x, hierarchical screening threshold calibration not detailed
- No guidance on evaluation versioning -- how to track rubric changes over time
- Confidence calibration formula is presented without empirical validation

## References Insights

**Insight 1 - Multi-Shuffle Position Bias Mitigation (from bias-mitigation.md):**
Beyond the basic two-pass position swap described in SKILL.md, the bias-mitigation reference provides a `multi_shuffle_comparison` function that uses n_shuffles (default 3) with majority voting. This is significantly more robust for high-stakes evaluation: with 3 shuffles, a true winner must win at least 2/3 passes, dramatically reducing false position bias. The agreement rate (winners.count(final_winner) / len(winners)) also serves as a natural confidence measure. SKILL.md only covers the two-pass protocol.

**Insight 2 - Aggregate BiasMonitor with Statistical Detection (from bias-mitigation.md):**
The BiasMonitor class provides production-grade systematic bias detection that goes well beyond SKILL.md's conceptual treatment. It uses z-score testing for position bias (z > 2 indicates significant bias) and Spearman correlation with scipy for length bias detection (corr > 0.3 and p < 0.05). These are proper statistical methods for ongoing monitoring. SKILL.md mentions "monitor for systematic bias" as a guideline but doesn't provide the statistical framework.

**Insight 3 - Confidence Calibration Formula (from implementation-patterns.md):**
The implementation-patterns reference provides a concrete `calibrate_confidence` function that combines three signals: raw model confidence, position consistency (0.6 multiplier if inconsistent), and evidence count (capped at 3 pieces, contributing 0.3 weight). The formula ensures confidence never reaches 1.0 (capped at 0.99). This multi-signal calibration is more nuanced than SKILL.md's simple "confidence = average of individual confidences" for consistent passes.

**Insight 4 - Graceful Degradation and Retry Patterns (from implementation-patterns.md):**
The implementation-patterns reference includes error handling patterns absent from SKILL.md: `evaluate_with_fallback` degrades from full evaluation to simple evaluation on RateLimitError, and returns partial results with error flags on ParseError. The `evaluate_with_retry` uses exponential backoff (2^attempt seconds). These are critical for production systems where LLM API reliability varies.

**Insight 5 - Quality Monitoring with Rolling Windows (from metrics-guide.md):**
The QualityMonitor class uses a deque with configurable window_size (default 100) for rolling metrics calculation. It tracks length-score correlation alongside human agreement, providing continuous bias monitoring. The minimum threshold of 10 human spot-checks before reporting prevents premature conclusions. This sliding window approach is more practical than batch evaluation for production systems.

## Cross-skill Comparison

**advanced-evaluation vs evaluation:**
evaluation provides the foundation (what to measure, rubric design, test sets), while advanced-evaluation provides the execution machinery (how to measure with LLMs). evaluation introduces the "95% Finding" and end-state evaluation concepts that are absent from advanced-evaluation. advanced-evaluation contributes bias mitigation, pairwise comparison, and confidence calibration. Together they form a complete stack. Dependency: advanced-evaluation should reference evaluation as prerequisite.

**advanced-evaluation vs tool-design:**
The evaluation pipeline architecture (Criteria Loader -> Primary Scorer -> Bias Mitigation -> Confidence Scoring) mirrors tool-design principles: clear input/output schemas, error handling, and layered architecture. The structured output patterns (ScoreResult, EvaluationResult dataclasses) in implementation-patterns.md exemplify tool-design's emphasis on typed responses. advanced-evaluation's evaluation tools ARE tools that should follow tool-design guidelines.

**Unique strength of advanced-evaluation:**
The bias mitigation framework is unique in the collection. No other skill addresses systematic bias in AI outputs with this depth. The five-bias taxonomy with dual mitigations (primary + secondary) for each is the most comprehensive treatment of AI reliability in the skill set.

## Tich hop

- **OMC quality-reviewer agent**: Implement direct scoring with rubrics for code review quality assessment
- **OMC code-reviewer**: Pairwise comparison for comparing alternative implementations
- **Team pipeline verification**: Hierarchical evaluation at team-verify stage (cheap screen with haiku, detailed with opus)
- **/viec phanbien**: Rubric generation for task-specific quality criteria with strictness calibration
- **Content quality**: LLM-as-judge for marketing/documentation quality with bias-aware prompts
- **A/B testing**: Pairwise comparison protocol for prompt engineering experiments
- **Production monitoring**: BiasMonitor + QualityMonitor for ongoing OMC agent quality tracking

## Ghi chu ky thuat

- Version 1.0.0, created 2024-12-24 by Muratcan Koylan. 454 lines (longest skill).
- 3 reference files: bias-mitigation.md (278 lines), implementation-patterns.md (315 lines), metrics-guide.md (330 lines). Total reference material exceeds SKILL.md.
- evaluation_example.py (337 lines) demonstrates direct scoring, pairwise comparison with position swap, and rubric generation -- all runnable but with simulated outputs.
- Temperature recommendation: 0.3 for scoring consistency (from implementation-patterns.md).
- External citations: MT-Bench (Zheng et al. 2023), G-Eval (Liu et al. 2023), Wang et al. 2023, Eugene Yan blog.
- Metric thresholds from metrics-guide.md: Spearman rho > 0.8 good, Cohen kappa > 0.7 good, position consistency > 0.9 good, length correlation < 0.2 good.
- The summary table in bias-mitigation.md (Bias | Primary | Secondary | Detection) is the most compact reference for the entire bias framework.
- Dependency: evaluation (foundational). Peer: tool-design (evaluation tools are tools).
