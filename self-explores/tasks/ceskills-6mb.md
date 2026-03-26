---
date: 2026-03-26
type: task-worklog
task: ceskills-6mb
title: "Deep Dive: project-development"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [research, context-engineering, project-development]
---

# Deep Dive: project-development — Detailed Design

## 1. Objective
Nghien cuu sau skill project-development bao gom SKILL.md (342 dong) VA references/ (2 files: case-studies.md, pipeline-patterns.md). Tao analysis di sau hon draft cu bang cach doc reference files va cross-compare voi skills lien quan.

## 2. Scope
**In-scope:**
- Doc SKILL.md (342 dong) — da doc trong draft cu
- Doc TAT CA reference files: case-studies.md, pipeline-patterns.md
- Doc script file: pipeline_template.py
- So sanh voi 2 skills lien quan: tool-design, evaluation
- Danh gia theo rubric 4 tieu chi (Completeness, Usability, Modularity, Safety) thang 1-5

**Out-of-scope:**
- Chay scripts (chi doc va phan tich)
- So sanh voi repos ben ngoai

## 3. Input / Output
**Input:**
- skills/project-development/SKILL.md (342 dong)
- skills/project-development/references/case-studies.md
- skills/project-development/references/pipeline-patterns.md
- skills/project-development/scripts/pipeline_template.py
- Draft cu: self-explores/learn/project-development/analysis.md

**Output:**
- self-explores/learn/project-development/analysis.md (GHI DE draft cu voi version day du hon)

## 4. Dependencies
- Cho ceskills-8pl (Tong quan) xong truoc

## 5. Flow xu ly

### Step 1: Doc lai SKILL.md + draft cu (~5 phut)
Doc SKILL.md va draft analysis.md hien tai de biet da cover gi.
**Verify:** Liet ke duoc sections da cover vs chua cover.

### Step 2: Doc TAT CA reference files (~15 phut)
Doc tung file trong references/:
- skills/project-development/references/case-studies.md
- skills/project-development/references/pipeline-patterns.md
Ghi chu insights MOI khong co trong SKILL.md.
**Verify:** Co >=2 insights moi tu references.

### Step 3: Doc script file (~5 phut)
Doc pipeline_template.py, phan tich code pattern.
**Verify:** Ghi chu 1-2 patterns tu code.

### Step 4: Cross-skill comparison (~10 phut)
So sanh voi tool-design, evaluation — tim overlap, complement, conflict.
**Verify:** Co section "So sanh voi skills lien quan" trong analysis.

### Step 5: Danh gia rubric 4 tieu chi (~5 phut)
Score Completeness, Usability, Modularity, Safety (1-5) voi justification.
**Verify:** 4 scores + 4 justifications.

### Step 6: Viet analysis.md hoan chinh (~15 phut)
Ghi de draft cu voi version moi bao gom: rubric, insights tu refs, cross-skill.
**Verify:** File co sections: Score, Uu/Nhuoc, References Insights, Cross-skill, Tich hop, Ghi chu.

## 6. Edge Cases
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| Ref file qua dai | >500 dong | Doc tom tat + key sections | Skim headers, read key paragraphs |
| Script khong runnable | Syntax errors | Ghi nhan, phan tich logic | Note as limitation |
| Overlap voi skill khac | >50% content giong | Highlight diff, not repeat | Focus on unique value |

## 7. Acceptance Criteria
- **Happy 1:** Given SKILL.md + references/, When analysis written, Then has >=2 insights NOT in SKILL.md (from references)
- **Happy 2:** Given rubric 4 criteria, When scored, Then each has numeric score 1-5 + justification sentence
- **Negative:** Given 2 reference files, When ref step done, Then insights cover BOTH case-studies AND pipeline-patterns

## 8. Technical Notes
- Reference files paths: skills/project-development/references/
- Script paths: skills/project-development/scripts/
- Draft path: self-explores/learn/project-development/analysis.md
- Cross-reference skills listed in SKILL.md "Integration" section
- Largest SKILL.md at 342 lines — allocate extra reading time
- 2 reference files: case-studies (practical) + pipeline-patterns (structural)

## 9. Risks
- Rubric scoring subjective — mitigation: justification required
- References may repeat SKILL.md content — mitigation: focus on NEW info only
- Project-development is broad topic — mitigation: focus on context engineering specific development patterns

## Phan bien (2026-03-26)

### Diem chat luong: 6/10
*(Da address trong Detailed Design — bo sung refs, cross-skill, rubric)*

## Worklog
*(Chua bat dau — task READY FOR DEV)*
