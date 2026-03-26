---
date: 2026-03-26
type: task-worklog
task: ceskills-s35
title: "Deep Dive: context-optimization"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [research, context-engineering, context-optimization]
---

# Deep Dive: context-optimization — Detailed Design

## 1. Objective
Nghien cuu sau skill context-optimization bao gom SKILL.md (179 dong) VA references/ (1 file: optimization_techniques.md). Tao analysis di sau hon draft cu bang cach doc reference files va cross-compare voi skills lien quan. PHAI differentiate ro rang voi context-compression.

## 2. Scope
**In-scope:**
- Doc SKILL.md (179 dong) — da doc trong draft cu
- Doc TAT CA reference files: optimization_techniques.md
- Doc script file: compaction.py
- So sanh voi 2 skills lien quan: context-compression, context-degradation
- DIFFERENTIATION: Optimization = improving RELEVANCE/QUALITY of context; Compression = reducing SIZE
- Danh gia theo rubric 4 tieu chi (Completeness, Usability, Modularity, Safety) thang 1-5

**Out-of-scope:**
- Chay scripts (chi doc va phan tich)
- So sanh voi repos ben ngoai

## 3. Input / Output
**Input:**
- skills/context-optimization/SKILL.md (179 dong)
- skills/context-optimization/references/optimization_techniques.md
- skills/context-optimization/scripts/compaction.py
- Draft cu: self-explores/learn/context-optimization/analysis.md

**Output:**
- self-explores/learn/context-optimization/analysis.md (GHI DE draft cu voi version day du hon)

## 4. Dependencies
- Cho ceskills-8pl (Tong quan) xong truoc

## 5. Flow xu ly

### Step 1: Doc lai SKILL.md + draft cu (~5 phut)
Doc SKILL.md va draft analysis.md hien tai de biet da cover gi.
**Verify:** Liet ke duoc sections da cover vs chua cover.

### Step 2: Doc TAT CA reference files (~15 phut)
Doc tung file trong references/:
- skills/context-optimization/references/optimization_techniques.md
Ghi chu insights MOI khong co trong SKILL.md.
**Verify:** Co >=2 insights moi tu references.

### Step 3: Doc script file (~5 phut)
Doc compaction.py, phan tich code pattern.
**Verify:** Ghi chu 1-2 patterns tu code.

### Step 4: Cross-skill comparison (~10 phut)
So sanh voi context-compression, context-degradation — tim overlap, complement, conflict.
QUAN TRONG: Lam ro:
- Optimization = curating WHAT context to include (relevance, ordering, prioritization)
- Compression = reducing HOW MUCH context takes up (summarization, truncation, encoding)
- Degradation = detecting WHEN context quality drops (monitoring, alerting)
**Verify:** Co section "So sanh voi skills lien quan" voi differentiation ro rang.

### Step 5: Danh gia rubric 4 tieu chi (~5 phut)
Score Completeness, Usability, Modularity, Safety (1-5) voi justification.
**Verify:** 4 scores + 4 justifications.

### Step 6: Viet analysis.md hoan chinh (~15 phut)
Ghi de draft cu voi version moi bao gom: rubric, insights tu refs, cross-skill, differentiation.
**Verify:** File co sections: Score, Uu/Nhuoc, References Insights, Cross-skill (voi differentiation), Tich hop, Ghi chu.

## 6. Edge Cases
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| Ref file qua dai | >500 dong | Doc tom tat + key sections | Skim headers, read key paragraphs |
| Script khong runnable | Syntax errors | Ghi nhan, phan tich logic | Note as limitation |
| Overlap voi compression | >50% content giong | MUST differentiate | Define taxonomy clearly |

## 7. Acceptance Criteria
- **Happy 1:** Given SKILL.md + references/, When analysis written, Then has >=2 insights NOT in SKILL.md (from references)
- **Happy 2:** Given rubric 4 criteria, When scored, Then each has numeric score 1-5 + justification sentence
- **Happy 3:** Given optimization vs compression overlap, When cross-skill written, Then has explicit differentiation paragraph
- **Negative:** Given shortest SKILL.md (179 lines), When analysis written, Then still produces comprehensive output by leveraging refs

## 8. Technical Notes
- Reference files paths: skills/context-optimization/references/
- Script paths: skills/context-optimization/scripts/
- Draft path: self-explores/learn/context-optimization/analysis.md
- Cross-reference skills listed in SKILL.md "Integration" section
- Shortest SKILL.md at 179 lines — references become MORE important
- Script name "compaction.py" suggests optimization may include compaction techniques

## 9. Risks
- Rubric scoring subjective — mitigation: justification required
- References may repeat SKILL.md content — mitigation: focus on NEW info only
- Optimization vs compression confusion is MAIN RISK — mitigation: dedicated differentiation section

## Phan bien (2026-03-26)

### Diem chat luong: 5/10
*(Da address trong Detailed Design — bo sung refs, cross-skill, rubric, differentiation voi compression)*

## Worklog
*(Chua bat dau — task READY FOR DEV)*
