# PRODUCT REQUIREMENTS DOCUMENT
## Enterprise HRMS Copilot — Natural Language HR Intelligence

**Author:** Irtaqa Naveed, Senior AI Product Manager
**Status:** Draft v1.0
**Date:** July 2026
**Classification:** Portfolio artifact — based on real domain experience, greenfielded as pre-build discovery

---

## 1. PROBLEM STATEMENT

One Network's executive layer — CEO and senior leadership — had zero real-time visibility into their 5,000-person workforce. HR data existed in structured databases and policy documents but was inaccessible without technical support or scheduled reporting cycles.

The existing process: department heads prepared weekly Excel presentations covering attendance, performance, financial, and scheduling data. These presentations were prepared manually, were often outdated by the time they were reviewed, and could not support ad-hoc questions. If the CEO needed to know which employees were underperforming, or when a specific person last transferred stations, the answer required a human to extract it — taking hours or days.

This created three real business problems. First, executive decisions about staffing, rotation, and performance were made on stale data. Second, scheduling gaps at financial checkpoints — such as toll plaza rotation — created conditions for cash manipulation and fraud. Third, individual unavailability (leave, travel) caused the entire reporting cycle to pause, leaving the organization operationally blind.

---

## 2. GOAL

Enable any authorized executive to query live HR data in natural language and receive accurate, sourced answers in real time — without scheduling a meeting, waiting for a report, or involving a human intermediary.

**North Star Metric:** Time from executive question to accurate answer, measured from days or hours to under 60 seconds.

---

## 3. NON-GOALS

These are explicitly out of scope for this version. They are not future maybes — they are decisions not to build.

- This product does not allow executives to write or update HR records. Read-only access only.
- This product does not support employee-facing queries. The interface is for the executive layer only.
- This product does not generate reports or scheduled summaries. It answers questions on demand.
- This product does not integrate with external systems or cloud APIs. All data stays on-premises.
- This product does not support voice input in v1. Text only.
- This product does not make decisions. It surfaces data. All decisions remain with the executive.

---

## 4. USER PERSONAS

**Primary User: The CEO**
Needs immediate, accurate answers to workforce questions without involving staff. Has limited patience for complex interfaces. Asks questions in natural language the way they would ask a human assistant. Does not know or care about the underlying database schema.

Example questions the CEO actually asks:
- When did X join the company?
- When was Y last transferred from their station?
- Who are the 10 lowest performing employees in the HR team?
- Who has been the best performer over the past 6 months?
- Which employees are currently underpaid relative to their grade?
- Is Z going on leave next month and what category?

**Secondary User: Senior Executives and Department Heads**
Same query needs as the CEO but scoped to their own department. Need visibility into attendance, performance, and scheduling without waiting for weekly reports.

---

## 5. USE CASES — PRIORITIZED

| Priority | Use Case | Data Module | Complexity |
|----------|----------|-------------|------------|
| P0 | Query individual employee records — join date, transfer history, current station | Staff Metadata | Low |
| P0 | Query attendance and availability — who is on leave, when, what category | Attendance | Low |
| P1 | Query performance rankings — top and bottom performers by team and period | Performance | Medium |
| P1 | Query financial data — salary grades, underpaid employees, allowance eligibility | Financial | Medium |
| P2 | Cross-module queries — combine attendance and performance for rotation decisions | Multi-module | High |

---

## 6. SYSTEM ARCHITECTURE — PRODUCT DECISIONS

This section documents the product decisions behind the architecture. Engineering implementation details are in the technical spec.

**Decision 1: On-premises deployment only.**
Rationale: HR and financial data is classified as sensitive. No data leaves the enterprise perimeter. Cloud APIs are not an option. All models run on local servers.

**Decision 2: Dual data source architecture.**
The system connects to two data sources: a structured SQL database covering attendance, performance, financial, and staff metadata; and a PDF document store covering HR policies, allowance rules, and employment terms. Both are required because some questions require structured data retrieval and some require policy context.

**Decision 3: Query classification before retrieval.**
A classification model routes each question to the correct data module before retrieval begins. This prevents cross-module confusion and reduces hallucination risk by narrowing the retrieval surface.

**Decision 4: SQL generation for structured queries.**
For structured data questions, the system generates a SQL query rather than relying solely on vector retrieval. This ensures factual accuracy on countable, recordable data — join dates, transfer counts, salary figures — where vector similarity is insufficient.

**Decision 5: Llama 3.2 for generation.**
Selected after benchmarking on the client's on-premises hardware. Chosen for performance within the hardware constraints, not for capability in isolation.

---

## 7. SUCCESS METRICS

These are the acceptance criteria before launch. The product does not ship unless these thresholds are met.

| Metric | Threshold | Measurement Method |
|--------|-----------|-------------------|
| Golden set accuracy | 85% of 50 pre-defined questions answered correctly | Manual review against ground truth |
| Response latency | Under 10 seconds for 90% of queries | Instrumented query logs |
| Retrieval precision | Correct data module identified in 95% of queries | Classification model evaluation |
| Source citation rate | 100% of answers include data source reference | Automated check |
| Executive adoption | CEO and 3 senior executives using weekly within 30 days of launch | Usage logs |

---

## 8. FAILURE MODES AND GUARDRAILS

This section defines what happens when the product is wrong — and what prevents it from being confidently wrong.

**Failure Mode 1: Data quality failure**
The database contains incomplete or incorrect records. Example: performance module stores only a single numeric score — insufficient to answer qualitative performance questions. The model retrieves correctly but the answer is useless.

Guardrail: Pre-launch data audit across all four modules. 20 sample questions run against raw database before any model integration. Data gaps are fixed before the product is built, not after.

**Failure Mode 2: Hallucination on leave categories**
The model invents a leave category that does not exist in the database. Example: CEO asks about an employee's upcoming leave and the model returns a marriage leave with associated allowances — when no such leave has been applied for. The CEO makes a staffing decision based on false data.

Guardrail: All leave-related answers must be sourced directly from the attendance database record. The model is instructed to return "no record found" if the data does not exist, rather than generating a plausible answer.

**Failure Mode 3: Cross-module confusion**
A question spanning multiple modules — such as "which employees have been underperforming and are also due for rotation" — triggers incorrect module classification and returns partial or mixed data.

Guardrail: Multi-label classification is supported. Questions classified to multiple modules trigger parallel retrieval and explicit result merging before generation. Each result set is tagged with its source module in the response.

**Failure Mode 4: Retrieval failure on policy documents**
A question about HR policy — such as allowance eligibility for marriage leave — fails to retrieve the correct policy document because the PDF was poorly chunked or the embedding did not capture the right context.

Guardrail: Policy documents are chunked by section, not by character count. Section headings are preserved as metadata and used in retrieval ranking.

---

## 9. CONSTRAINTS

- All models and infrastructure run on-premises on client-owned hardware
- No internet connectivity during inference
- No third-party API calls of any kind
- Data never leaves the enterprise perimeter
- Access is restricted to authorized executives only — role-based access control enforced at the application layer
- The system must operate without a dedicated ML engineer on-call post-launch

---

## 10. RISKS AND OPEN QUESTIONS

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| HR data is too incomplete to answer executive questions accurately | High | High | Pre-launch data audit; data completeness sprint before build begins |
| Executive adoption fails if UX is too complex | Medium | High | User testing with actual CEO before full launch |
| Performance module answers are qualitatively insufficient | High | Medium | Supplementary survey data collected during build phase |
| Model performance degrades on client hardware vs development environment | Medium | High | All benchmarking done on client hardware, not development machines |
| Scope creep — executives request write access or employee-facing features | High | Medium | Non-goals section is contractually referenced in acceptance criteria |

---

## 11. DEFINITION OF DONE

Version 1 is complete when:

1. All P0 and P1 use cases return accurate answers on the golden set at 85% or above
2. Response latency is under 10 seconds for 90% of queries on client hardware
3. Every answer includes a source citation
4. The CEO has completed a live demo session and confirmed the product answers their real questions
5. The system has run for 5 business days without a hallucinated answer that reached the CEO uncorrected
6. A handover document exists so the system can be maintained without the original engineering team

---

## 12. WHAT I WOULD DO DIFFERENTLY TODAY

This PRD was reconstructed from a product that was built without a formal discovery process. The following gaps were discovered post-launch and would be addressed pre-build if starting today:

- A data richness audit would run before architecture decisions were made
- A golden dataset of 50 real executive questions would be built before model selection
- Performance module data would be supplemented with qualitative survey data before the feature was scoped
- A paper prototype of the interface would be tested with the CEO before any engineering work began
- Acceptance criteria would be contractually agreed before the first sprint
