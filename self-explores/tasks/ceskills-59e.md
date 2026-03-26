---
date: 2026-03-26
type: task-worklog
task: ceskills-59e
title: "Deep Dive: advanced-evaluation"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [research, context-engineering, advanced-evaluation]
---

# Deep Dive: advanced-evaluation — Detailed Design

## 1. Objective
Nghien cuu sau skill advanced-evaluation bao gom SKILL.md (454 dong) VA references/ (3 files: bias-mitigation.md, implementation-patterns.md, metrics-guide.md). Tao analysis di sau hon draft cu bang cach doc reference files va cross-compare voi skills lien quan.

## 2. Scope
**In-scope:**
- Doc SKILL.md (454 dong) — da doc trong draft cu
- Doc TAT CA reference files: bias-mitigation.md, implementation-patterns.md, metrics-guide.md
- Doc script file: evaluation_example.py
- So sanh voi 2 skills lien quan: evaluation, tool-design
- Danh gia theo rubric 4 tieu chi (Completeness, Usability, Modularity, Safety) thang 1-5

**Out-of-scope:**
- Chay scripts (chi doc va phan tich)
- So sanh voi repos ben ngoai

## 3. Input / Output
**Input:**
- skills/advanced-evaluation/SKILL.md (454 dong)
- skills/advanced-evaluation/references/bias-mitigation.md
- skills/advanced-evaluation/references/implementation-patterns.md
- skills/advanced-evaluation/references/metrics-guide.md
- skills/advanced-evaluation/scripts/evaluation_example.py
- Draft cu: self-explores/learn/advanced-evaluation/analysis.md

**Output:**
- self-explores/learn/advanced-evaluation/analysis.md (GHI DE draft cu voi version day du hon)

## 4. Dependencies
- Cho ceskills-8pl (Tong quan) xong truoc

## 5. Flow xu ly

### Step 1: Doc lai SKILL.md + draft cu (~5 phut)
Doc SKILL.md va draft analysis.md hien tai de biet da cover gi.
**Verify:** Liet ke duoc sections da cover vs chua cover.

### Step 2: Doc TAT CA reference files (~15 phut)
Doc tung file trong references/:
- skills/advanced-evaluation/references/bias-mitigation.md
- skills/advanced-evaluation/references/implementation-patterns.md
- skills/advanced-evaluation/references/metrics-guide.md
Ghi chu insights MOI khong co trong SKILL.md.
**Verify:** Co >=2 insights moi tu references.

### Step 3: Doc script file (~5 phut)
Doc evaluation_example.py, phan tich code pattern.
**Verify:** Ghi chu 1-2 patterns tu code.

### Step 4: Cross-skill comparison (~10 phut)
So sanh voi evaluation, tool-design — tim overlap, complement, conflict.
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
| 3 ref files + 454 lines | Total content very large | Prioritize unique insights | Time-box ref reading |

## 7. Acceptance Criteria
- **Happy 1:** Given SKILL.md + references/, When analysis written, Then has >=2 insights NOT in SKILL.md (from references)
- **Happy 2:** Given rubric 4 criteria, When scored, Then each has numeric score 1-5 + justification sentence
- **Happy 3:** Given 3 reference files, When ref step done, Then insights cover at least 2 of 3 files
- **Negative:** Given longest SKILL.md (454 lines), When analysis written, Then does not simply repeat content but adds new value

## 8. Technical Notes
- Reference files paths: skills/advanced-evaluation/references/
- Script paths: skills/advanced-evaluation/scripts/
- Draft path: self-explores/learn/advanced-evaluation/analysis.md
- Cross-reference skills listed in SKILL.md "Integration" section
- Longest SKILL.md (454 lines) + most reference files (3) = richest material for analysis
- bias-mitigation.md is unique topic not covered by other skills

## 9. Risks
- Rubric scoring subjective — mitigation: justification required
- References may repeat SKILL.md content — mitigation: focus on NEW info only
- Volume of material (454 + 3 refs) may be overwhelming — mitigation: time-box reading, focus on unique insights

## Phan bien (2026-03-26)

### Diem chat luong: 5/10
*(Da address trong Detailed Design — bo sung 3 refs, cross-skill, rubric)*

## Worklog
*(Chua bat dau — task READY FOR DEV)*
