# METRICS TREE
## Product: Enterprise HRMS Copilot
**Author:** Irtaqa Naveed, Senior AI Product Manager
**Date:** July 2026
**Framework:** North Star → Input Metrics → Guardrail Metrics → Instrumentation Plan

---

## THE NORTH STAR METRIC

**"Number of weeks the CEO completed the work week without requesting a manual HR presentation AND without making any ad-hoc clarification calls to department heads or HR staff."**

Why this is the right North Star:

It captures customer value. The CEO is getting accurate, real-time HR intelligence on demand. The old workflow — weekly Excel presentations, mid-week clarification calls, dependency on staff availability — has been replaced.

It captures business health. An executive who has permanently stopped the old workflow is a retained user. Retention in enterprise B2B is the primary driver of contract renewal and expansion.

It is impossible to fake. Query volume can be inflated. This metric cannot. Either the CEO stopped calling people for HR data or they did not.

Target: North Star score of 4 out of 4 weeks per month within 90 days of launch.

---

## INPUT METRICS

These are the four things that must be true for the North Star to move in the right direction. If any one of these drops, the North Star will follow.

**Input 1: Answer Accuracy Rate**
Definition: Percentage of queries where the system returns a factually correct answer, verified against ground truth in the database.
Target: Above 90% on all query types. Above 99% on P0 identity queries (name, join date, grade, location).
Why it matters: A CEO who receives one wrong answer about an employee's leave status or performance will not trust the system again. Accuracy is the foundation of adoption.
Measurement: Weekly golden dataset run of 50 questions. Score each answer pass or fail against verified database records.

**Input 2: Context Retention Rate**
Definition: Percentage of multi-turn conversations where the model correctly maintains context from the previous query without requiring the user to repeat subject information.
Target: Above 95%.
Why it matters: If the CEO asks "when did Fatima join?" and then asks "what is her performance this quarter?" the system must know he is still asking about Fatima. If it asks for clarification, the conversation breaks and the CEO loses confidence in the product.
Measurement: Log all multi-turn sessions. Flag any session where the system asked a clarifying question that repeated information already provided in the same conversation.

**Input 3: Response Latency**
Definition: Time from query submission to answer displayed on screen, measured end-to-end including retrieval, generation, and rendering.
Target: Under 8 seconds for 95% of queries on production hardware.
Why it matters: An executive asking a question in a meeting cannot wait 30 seconds for an answer. Latency above 10 seconds causes abandonment and forces a return to the old workflow.
Measurement: Log timestamp at query submission and at answer render. Calculate P50, P90, and P95 latency daily.

**Input 4: Interface Friction Score**
Definition: Number of distinct actions required to receive an answer from query initiation. One action means type or speak, receive answer. Any additional click, selection, or confirmation counts as friction.
Target: Maximum one action per query.
Why it matters: The product replaces a workflow that, while slow, required no learning curve. If the interface introduces any complexity, executives will not adopt it. One action is the bar.
Measurement: Session recording analysis. Count actions per successful query completion. Flag any session requiring more than one action.

---

## GUARDRAIL METRICS

These are the red lines. The product must never cross these thresholds even while optimizing for the North Star. A guardrail breach triggers an immediate incident response regardless of overall product health.

**Guardrail 1: Identity Hallucination Rate**
Definition: Any instance where the system returns a factually wrong answer about a specific employee's identity, employment record, leave category, or grade.
Target: Zero. Absolute zero tolerance.
Why it is a guardrail not an input: One hallucinated identity fact destroys trust permanently. A CEO who receives a wrong answer about an employee's leave category — especially if it triggers an incorrect financial allowance — will never use the system again. This is not a metric to optimize. It is a line that must never be crossed.
Response if breached: Immediate system suspension. Root cause analysis. No re-launch without golden dataset passing at 100% on identity queries.

**Guardrail 2: Unauthorized Data Access Rate**
Definition: Any instance where the system returns data to a user who does not have access rights to that data. For example a department head receiving compensation data for employees outside their department.
Target: Zero.
Why it is a guardrail: One unauthorized data disclosure converts the product from a productivity tool into a compliance liability. In a regulated enterprise this is a termination-level incident for the product.
Response if breached: Immediate access audit. System suspension until access control layer is verified.

**Guardrail 3: Data Staleness Rate**
Definition: Percentage of answers returned based on data that is more than 24 hours out of sync with the live database, without a freshness warning displayed to the user.
Target: Zero stale answers served without a staleness indicator.
Why it is a guardrail: A CEO making a rotation or transfer decision based on yesterday's attendance data is making a decision on wrong information. The system must always display the data timestamp alongside the answer. Serving stale data silently is a product failure regardless of how accurate the underlying answer appears.
Response if breached: Add data freshness timestamp to all answers within 24 hours.

---

## INSTRUMENTATION PLAN

What must be logged for this metrics system to work.

| Event | Data to capture | Used for |
|-------|----------------|---------|
| Query submitted | Timestamp, user ID, query text, session ID | Latency calculation, usage patterns |
| Classification decision | Module routed to, confidence score, multi-label flag | Context retention, misclassification analysis |
| Retrieval result | Documents retrieved, retrieval method, retrieval time | Accuracy analysis, retrieval failure detection |
| Answer generated | Answer text, generation time, source citations | Hallucination detection, accuracy scoring |
| User follow-up query | Whether query referenced prior context, session ID | Context retention rate |
| User abandonment | Session ended without answer, time to abandonment | Latency and friction analysis |
| Manual presentation requested | CEO or executive requested traditional report | North Star tracking |
| Ad-hoc clarification call | Executive contacted staff for HR data outside system | North Star tracking |

The last two events are the most important and the hardest to instrument. They require a weekly self-reported log from the CEO's assistant or a manual review process. Instrumentation of behavior change cannot always be automated. This is a known gap.

---

## WEEKLY METRICS REVIEW PROTOCOL

Every Monday morning before any product work begins:

1. Run golden dataset. Record accuracy score.
2. Pull latency P95 from logs. Flag if above 8 seconds.
3. Review all multi-turn sessions from prior week. Flag context failures.
4. Check for any guardrail breach events. If yes, stop all other work.
5. Ask CEO's assistant: did the CEO request a manual presentation or make a clarification call last week? Record yes or no.
6. Update North Star tracker.

Total time: 45 minutes. Non-negotiable weekly ritual.

---

*This metrics tree was built as part of a Senior AI PM portfolio development program based on real product experience at One Network.*
