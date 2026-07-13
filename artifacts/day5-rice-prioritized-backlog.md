# RICE PRIORITIZED BACKLOG
## Product: Enterprise HRMS Copilot
**Author:** Irtaqa Naveed, Senior AI Product Manager
**Date:** July 2026
**Framework:** RICE Scoring (Reach x Impact x Confidence / Effort)

---

## RICE FORMULA

Score = (Reach x Impact x Confidence) / Effort

Reach: number of executives who will use this feature per month (total executive user base: 12)
Impact: 1 = low, 2 = medium, 3 = high impact on user goal
Confidence: 100% = high, 80% = medium, 50% = low confidence in estimates
Effort: person-weeks required to build

---

## SCORED BACKLOG

| Rank | Use Case | Reach | Impact | Confidence | Effort | RICE Score |
|------|----------|-------|--------|------------|--------|------------|
| 1 | Employee metadata lookup | 12 | 3 | 1.0 | 1 | 36 |
| 1 | Attendance and availability | 12 | 3 | 1.0 | 1 | 36 |
| 3 | Leave planning | 12 | 2 | 1.0 | 1 | 24 |
| 4 | Compensation analysis | 5 | 3 | 1.0 | 1 | 15 |
| 5 | Transfer and movement history | 12 | 3 | 0.8 | 2 | 14.4 |
| 6 | Headcount summary | 12 | 2 | 0.8 | 2 | 9.6 |
| 7 | Performance ranking | 12 | 3 | 0.5 | 3 | 6 |
| 7 | Performance history | 12 | 3 | 0.5 | 3 | 6 |

---

## SPRINT PLAN DERIVED FROM RICE SCORES

**Sprint 1: Quick Wins (2 person-weeks)**
Employee metadata lookup and attendance and availability both score 36. Highest confidence, lowest effort, highest reach. These two features prove the system works and deliver immediate value to all 12 executives. Ship together.

**Sprint 2: High Value, High Confidence (2 person-weeks)**
Leave planning at 24 and compensation analysis at 15. Both are high confidence and low effort. Leave planning serves all 12 executives. Compensation analysis has lower reach (5 users) but high impact for those users.

**Sprint 3: Moderate Confidence Features (4 person-weeks)**
Transfer and movement history at 14.4 and headcount summary at 9.6. Both have 80% confidence due to data lag issues in the system. Transfer data has informal update delays. Headcount data has location assignment lag. These features are buildable but require closer monitoring post-launch.

**Sprint 4: Performance Features (6 person-weeks, after data validation sprint)**
Both performance features score 6. Not because they are unimportant but because they are not ready. Performance data exists as a single numeric score per review cycle. Qualitative performance data does not exist yet. A dedicated data validation sprint must run before Sprint 4 begins. HR must complete a structured performance survey across the organization. Only then does confidence rise enough to justify the build effort.

---

## KEY INSIGHT FROM SCORING

The RICE exercise revealed that the two features executives care most about (performance ranking and performance history) score lowest not because they lack reach or impact but because confidence is 50%. That 50% confidence is entirely driven by data quality, not model quality. The fix is not a better model. The fix is better data. This is a discovery finding that saves three to six weeks of wasted engineering effort.

---

## DATA VALIDATION SPRINT (PREREQUISITE FOR SPRINT 4)

Before performance features are built the following must be completed:

HR must deploy a 10 to 15 question structured performance survey across all active employees covering dependability, work quality under pressure, team leadership, and commitment.

HR must audit completion rates on all 75 induction fields across the employee database and reach 95% completeness before Sprint 4 begins.

The PM must validate that the performance data can answer the ten most common CEO performance questions before any model integration begins.

---

*This backlog was scored using real product context from the HRMS RAG Copilot built at One Network. All RICE estimates are based on an executive user base of 12 people and real engineering constraints observed during the original build.*
