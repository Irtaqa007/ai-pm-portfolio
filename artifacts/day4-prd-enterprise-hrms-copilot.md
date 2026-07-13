# PRODUCT REQUIREMENTS DOCUMENT
## Enterprise HRMS Copilot: Natural Language HR Intelligence for Executive Decision-Making

**Author:** Irtaqa Naveed, Senior AI Product Manager
**Status:** Draft v1.0
**Date:** July 2026
**Classification:** Portfolio artifact based on real product experience at One Network

---

## 1. PROBLEM STATEMENT

One Network's executive team manages a 5,000-person workforce across multiple locations. Before this product, HR intelligence was delivered through weekly Excel-based presentations prepared by department heads. This process had three critical failure points.

First, it was slow. Data was always at least a week old by the time it reached the CEO. Decisions about rotation, transfers, and performance were made on stale information.

Second, it was fragile. If a department head or the CEO was unavailable, the entire reporting cycle paused. Rotation scheduling delays created financial risk because staff remaining at one location too long created conditions for cash manipulation at toll plazas.

Third, it was shallow. Executives could only ask questions that fit the prepared report format. Ad-hoc questions like "who are my ten lowest performers in the HR team right now?" required a new manual pull taking hours or days.

---

## 2. GOAL

Enable any authorized executive to ask any HR question in natural language and receive an accurate, real-time answer from live enterprise data without scheduling a meeting, waiting for a report, or involving any other person.

---

## 3. NON-GOALS

These are explicitly out of scope for v1. They will not be built, prototyped, or discussed as stretch goals.

Self-service access for non-executive employees. HR data entry or editing through the copilot interface. Integration with external HR benchmarking data. Predictive analytics or forecasting. Mobile application. Any cloud-based deployment or external API calls. Automated decision-making because the copilot surfaces information only and humans make all decisions.

---

## 4. USERS

**Primary user:** CEO and C-suite executives at One Network.

**User profile:** Senior leaders managing a 5,000-person workforce. High-stakes decisions, low tolerance for delay, limited time, and no appetite for learning new interfaces. They communicate in natural language. They expect instant answers. They do not want to interact with filters, dropdowns, or search fields.

**Secondary user:** HR Director and Department Heads who need to prepare briefings and verify data accuracy.

---

## 5. USE CASES IN PRIORITY ORDER

These are the real questions the CEO will ask on Day 1. Each must work correctly before launch.

| Priority | Question type | Example query |
|----------|--------------|---------------|
| P0 | Employee metadata lookup | When did Ahmad join the company? |
| P0 | Transfer and movement history | When was the last time Fatima was transferred from Lahore? |
| P0 | Attendance and availability | Who is on leave this week in the operations team? |
| P1 | Performance ranking | Who are the ten lowest performing employees in the HR department? |
| P1 | Performance history | Who has been the best performer in finance over the past six months? |
| P1 | Compensation analysis | Who in the engineering team is underpaid relative to their grade? |
| P2 | Leave planning | Who has applied for leave next month and what category? |
| P2 | Headcount summary | How many people are currently posted at the Lahore location? |

---

## 6. SUCCESS METRICS

The product succeeds when the following thresholds are met and sustained for 30 days post-launch.

**Accuracy:** 90% of P0 and P1 queries return a factually correct answer verified against ground truth in the database. Measured using a 50-question golden dataset run weekly.

**Retrieval correctness:** The right data source is retrieved for 95% of queries meaning the classification model routes to the correct module and the SQL query or vector retrieval returns the relevant record.

**Response latency:** End-to-end response time under 8 seconds for 95% of queries on the production hardware configuration.

**Executive adoption:** CEO and at least three C-suite executives use the system for real HR queries at least twice per week within 30 days of launch replacing at least one weekly Excel presentation cycle.

**Zero hallucination on identity queries:** P0 metadata queries covering name, join date, grade, and location must return 100% accurate answers. A single hallucinated identity fact is a severity-1 incident.

---

## 7. CONSTRAINTS AND AI-SPECIFIC RISKS

This section is mandatory for any AI product PRD. It answers what failure looks like and what we do about it.

**Constraint 1: On-premises only.**
The system runs entirely on One Network's internal servers. No data leaves the building. No cloud APIs. No internet connectivity during inference. All models, embeddings, and vector databases must be self-hosted. This is a hard constraint, not a preference.

**Constraint 2: Data quality dependency.**
The system is only as accurate as the data in the HRMS database. HR staff must complete all 75 induction fields for every employee before the system can answer questions about that employee. Incomplete records produce incomplete answers. A data validation sprint must complete before the product launches.

**Constraint 3: Leave category hallucination risk.**
The highest-risk failure mode is incorrect leave category attribution. If the model hallucinates a marriage leave record for an employee who is not getting married, the company may incorrectly trigger marriage allowance payments. This is a financial fraud risk. All leave category answers must cite the exact database record and display the source document reference alongside the answer. Any leave query without a verifiable source record must return "no record found" rather than an inferred answer.

**Constraint 4: Performance data thinness.**
The current database stores performance as a single numeric score per review cycle. The system cannot answer qualitative performance questions like "is Ahmad a reliable leader?" from this data alone. Questions exceeding the data depth must return a clear "insufficient data to answer this question" response rather than an inferred answer.

**Constraint 5: Classification model failure on multi-module queries.**
When a query spans multiple modules such as "show me employees who are underperforming and have had recent transfers," the classification model may fail to route correctly. Multi-module queries must be flagged to the user as higher-uncertainty responses until the classification model is validated against a multi-label test set.

---

## 8. SYSTEM ARCHITECTURE OVERVIEW

This section is for technical alignment only. Engineering owns the implementation.

**Data sources:** SQL database covering four modules including attendance and availability, performance, financial data, and staff metadata. PDF policy documents covering HR procedures and allowance rules.

**Ingestion pipeline:** PDFs processed through OCR, chunked, embedded via Hugging Face embedding model, stored in Milvus vector database. SQL data accessed directly via query generation.

**Query pipeline:** Natural language query enters classification model, routes to relevant module, generates SQL query or vector retrieval, passes results to Llama 3.2 for natural language response generation.

**Infrastructure:** Fully on-premises. Zero cloud dependency. Runs on One Network's internal servers.

---

## 9. ACCEPTANCE CRITERIA: DEFINITION OF DONE

The product is not ready to ship until every item below is verified.

50-question golden dataset built and reviewed by CEO or HR Director. Golden dataset accuracy above 90% on P0 and P1 queries. Zero hallucinated answers on identity queries across 200 test runs. Leave category answers always display source record reference. Insufficient data response functioning correctly on out-of-scope queries. Response latency under 8 seconds on production hardware for 95% of queries. HR data completeness above 95% across all active employee records. CEO has completed one live demo session and confirmed the product answers real questions correctly.

---

## 10. RISK REGISTER

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| HR data incomplete at launch | High | High | Data validation sprint mandatory before build begins |
| Leave category hallucination causes incorrect payment | Medium | Critical | Source citation required on all leave answers |
| Classification model misroutes multi-module queries | Medium | Medium | Flag multi-module queries as higher uncertainty |
| Executive adoption fails due to interface complexity | Low | High | CEO demo session with real questions before launch |
| Performance data too thin for qualitative questions | High | Medium | Insufficient data response for out-of-scope queries |

---

## 11. OPEN QUESTIONS

These must be answered before build begins.

What is the minimum data completeness threshold required before launch and who owns the data validation sprint? Which executive is the primary sponsor and will attend the weekly demo sessions during build? Is there an existing authentication layer for the HRMS system that the copilot interface should integrate with or does access control need to be built from scratch? What is the hardware specification of the production server where the system will run because this determines which Llama variant is feasible. Are there any HR data fields that should never be surfaced through the copilot such as salary data for employees below a certain grade?

---

*This PRD is a portfolio artifact produced as part of a Senior AI PM development program. It is based on a real product built at One Network, reconstructed with full product discovery rigor applied retroactively.*
