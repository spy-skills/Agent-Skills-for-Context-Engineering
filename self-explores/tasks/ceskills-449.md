---
date: 2026-03-26
type: task-worklog
task: ceskills-449
title: "Xep hang uu tien - Danh gia muc do quan trong 1-5"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [research, context-engineering, priority-ranking]
---

# Xep hang uu tien — Detailed Design

## 1. Objective
Xep hang 13 skills theo muc do quan trong cho 2 doi tuong: beginner (nguoi moi hoc context engineering) va experienced (nguoi da co kinh nghiem xay dung AI agents). Tao 2 danh sach ranking rieng biet voi justification cho tung vi tri.

## 2. Scope
**In-scope:**
- Doc tat ca 13 draft analysis files tu self-explores/learn/*/analysis.md
- Doc tong quan analysis: self-explores/learn/overview/analysis.md
- Xay dung tieu chi ranking cho tung audience
- Tao 2 rankings voi justification
- Ghi de draft cu self-explores/learn/priority-ranking/analysis.md

**Out-of-scope:**
- So sanh voi repos/skills ben ngoai
- Tao learning path chi tiet (chi ranking + justification)

## 3. Input / Output
**Input:**
- self-explores/learn/overview/analysis.md (tong quan)
- self-explores/learn/{13-skill-names}/analysis.md (13 draft analyses)
- Rubric scores tu cac deep dive tasks (neu da hoan thanh)

**Output:**
- self-explores/learn/priority-ranking/analysis.md (GHI DE draft cu)

## 4. Dependencies
- Cho ceskills-8pl (Tong quan) xong truoc
- Nen co it nhat cac P1 deep dives xong de co rubric scores

## 5. Flow xu ly

### Step 1: Doc tong quan + tat ca drafts (~10 phut)
Doc overview/analysis.md de hieu phan nhom. Doc lan luot 13 draft analysis files.
**Verify:** Co danh sach 13 skills voi summary 1 dong moi skill.

### Step 2: Xay dung tieu chi ranking (~5 phut)
Dinh nghia tieu chi cho moi audience:
- **Beginner:** Foundational value, learning curve, immediate applicability, prerequisite importance
- **Experienced:** Advanced technique depth, production readiness, unique insight value, integration complexity
**Verify:** Co 4 tieu chi/audience voi dinh nghia ro rang.

### Step 3: Score tung skill cho Beginner audience (~10 phut)
Cham diem tung skill theo 4 tieu chi beginner, tinh tong, xep hang.
**Verify:** 13 skills co score, sorted descending.

### Step 4: Score tung skill cho Experienced audience (~10 phut)
Cham diem tung skill theo 4 tieu chi experienced, tinh tong, xep hang.
**Verify:** 13 skills co score, sorted descending. Rankings khac beginner o it nhat 3 vi tri.

### Step 5: Viet justification cho moi vi tri (~10 phut)
Moi skill trong moi ranking can 1-2 cau giai thich tai sao o vi tri do.
**Verify:** 26 justifications (13 x 2 audiences).

### Step 6: Viet analysis.md hoan chinh (~10 phut)
Ghi de draft cu voi: methodology, 2 rankings, justifications, key observations.
**Verify:** File co sections: Methodology, Beginner Ranking, Experienced Ranking, Key Observations.

## 6. Edge Cases
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| Skills co diem bang nhau | 2+ skills same score | Tiebreak theo breadth of content | Note tiebreak logic |
| Draft analysis thieu | Skill chua co analysis | Rank dua tren SKILL.md truc tiep | Note as lower confidence |
| Overlap skills | compression vs optimization | Rank rieng, note overlap | Explain differentiation |

## 7. Acceptance Criteria
- **Happy 1:** Given 13 skill analyses, When ranked for beginner, Then produces ordered list 1-13 with justification per position
- **Happy 2:** Given 13 skill analyses, When ranked for experienced, Then produces ordered list 1-13 with justification per position
- **Happy 3:** Given 2 rankings, When compared, Then at least 3 positions differ between beginner and experienced
- **Negative:** Given incomplete draft analyses, When ranking, Then notes confidence level for skills without full analysis

## 8. Technical Notes
- Draft analysis paths: self-explores/learn/{skill-name}/analysis.md
- Overview path: self-explores/learn/overview/analysis.md
- Output path: self-explores/learn/priority-ranking/analysis.md
- 13 skills: context-fundamentals, context-degradation, context-compression, tool-design, filesystem-context, multi-agent-patterns, memory-systems, project-development, context-optimization, evaluation, advanced-evaluation, hosted-agents, bdi-mental-states

## 9. Risks
- Ranking subjective — mitigation: explicit criteria + justification required
- Beginner vs experienced distinction may be blurry — mitigation: define audience profile clearly
- Draft analyses may have uneven depth — mitigation: note confidence level per ranking

## Phan bien (2026-03-26)

### Diem chat luong: 5/10
*(Da address trong Detailed Design — bo sung audience definition, consistent scale, justification)*

## Worklog
*(Chua bat dau — task READY FOR DEV)*
