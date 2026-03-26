---
date: 2026-03-26
type: task-worklog
task: ceskills-i63
title: "Deep Dive: bdi-mental-states"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [research, context-engineering, bdi-mental-states]
---

# Deep Dive: bdi-mental-states — Detailed Design

## 1. Objective
Nghien cuu sau skill bdi-mental-states bao gom SKILL.md (295 dong) VA references/ (4 files: bdi-ontology-core.md, framework-integration.md, rdf-examples.md, sparql-competency.md). KHONG co scripts. Tao analysis di sau hon draft cu bang cach doc reference files va cross-compare voi skills lien quan.

## 2. Scope
**In-scope:**
- Doc SKILL.md (295 dong) — da doc trong draft cu
- Doc TAT CA reference files: bdi-ontology-core.md, framework-integration.md, rdf-examples.md, sparql-competency.md
- SKIP script step (bdi-mental-states co 0 scripts)
- So sanh voi 1 skill lien quan: multi-agent-patterns
- Danh gia theo rubric 4 tieu chi (Completeness, Usability, Modularity, Safety) thang 1-5

**Out-of-scope:**
- Chay scripts (khong co scripts)
- So sanh voi repos ben ngoai

## 3. Input / Output
**Input:**
- skills/bdi-mental-states/SKILL.md (295 dong)
- skills/bdi-mental-states/references/bdi-ontology-core.md
- skills/bdi-mental-states/references/framework-integration.md
- skills/bdi-mental-states/references/rdf-examples.md
- skills/bdi-mental-states/references/sparql-competency.md
- Draft cu: self-explores/learn/bdi-mental-states/analysis.md

**Output:**
- self-explores/learn/bdi-mental-states/analysis.md (GHI DE draft cu voi version day du hon)

## 4. Dependencies
- Cho ceskills-8pl (Tong quan) xong truoc

## 5. Flow xu ly

### Step 1: Doc lai SKILL.md + draft cu (~5 phut)
Doc SKILL.md va draft analysis.md hien tai de biet da cover gi.
**Verify:** Liet ke duoc sections da cover vs chua cover.

### Step 2: Doc TAT CA reference files (~20 phut)
Doc tung file trong references/:
- skills/bdi-mental-states/references/bdi-ontology-core.md
- skills/bdi-mental-states/references/framework-integration.md
- skills/bdi-mental-states/references/rdf-examples.md
- skills/bdi-mental-states/references/sparql-competency.md
Ghi chu insights MOI khong co trong SKILL.md.
**Verify:** Co >=2 insights moi tu references (4 ref files should yield many insights).

### Step 3: SKIP script step (~0 phut)
bdi-mental-states la skill duy nhat khong co scripts/. Ghi nhan nhu limitation.
**Verify:** Note "No scripts available" trong analysis.

### Step 4: Cross-skill comparison (~10 phut)
So sanh voi multi-agent-patterns — tim overlap, complement, conflict.
BDI mental states cung cap cognitive framework cho multi-agent coordination.
**Verify:** Co section "So sanh voi skills lien quan" trong analysis.

### Step 5: Danh gia rubric 4 tieu chi (~5 phut)
Score Completeness, Usability, Modularity, Safety (1-5) voi justification.
Note: Modularity may score lower due to niche BDI/ontology focus.
**Verify:** 4 scores + 4 justifications.

### Step 6: Viet analysis.md hoan chinh (~15 phut)
Ghi de draft cu voi version moi bao gom: rubric, insights tu refs, cross-skill, practical value assessment.
**Verify:** File co sections: Score, Uu/Nhuoc, References Insights, Cross-skill, Tich hop, Ghi chu, Practical Value.

## 6. Edge Cases
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| Ref file qua dai | >500 dong | Doc tom tat + key sections | Skim headers, read key paragraphs |
| No scripts available | 0 scripts | Skip step 3 gracefully | Note as limitation in analysis |
| Niche topic | BDI/ontology unfamiliar | Focus on practical applications | Explain why BDI matters for CE |
| RDF/SPARQL content | Semantic web specific | Extract CE-relevant insights | Skip pure ontology details |

## 7. Acceptance Criteria
- **Happy 1:** Given SKILL.md + references/, When analysis written, Then has >=2 insights NOT in SKILL.md (from references)
- **Happy 2:** Given rubric 4 criteria, When scored, Then each has numeric score 1-5 + justification sentence
- **Happy 3:** Given 4 reference files (most of any skill), When ref step done, Then insights cover at least 3 of 4 files
- **Negative:** Given skill has 0 scripts, When script step reached, Then skips gracefully with note "No scripts — unique among 13 skills"

## 8. Technical Notes
- Reference files paths: skills/bdi-mental-states/references/
- NO script paths (only skill without scripts)
- Draft path: self-explores/learn/bdi-mental-states/analysis.md
- Cross-reference skills listed in SKILL.md "Integration" section
- 4 reference files (tied with advanced-evaluation for most refs) — compensates for lack of scripts
- BDI = Beliefs, Desires, Intentions — cognitive architecture for agents
- RDF/SPARQL content is semantic web specific — extract CE-relevant parts only

## 9. Risks
- Rubric scoring subjective — mitigation: justification required
- References may be highly specialized (ontology, RDF) — mitigation: focus on practical CE applications
- Niche topic may seem irrelevant — mitigation: explicitly explain connection to context engineering
- No scripts means less code insight — mitigation: 4 reference files compensate

## Phan bien (2026-03-26)

### Diem chat luong: 5/10
*(Da address trong Detailed Design — bo sung 4 refs, cross-skill, rubric, practical value)*

## Worklog
*(Chua bat dau — task READY FOR DEV)*
