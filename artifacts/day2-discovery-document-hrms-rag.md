# DISCOVERY DOCUMENT
## Product: HRMS RAG Chatbot — Enterprise AI Assistant
**Company:** One Network | Islamabad, Pakistan
**Role:** Senior AI Engineer & Technical Product Lead
**Document type:** Reconstructed discovery artifact — PM learning portfolio
**Date reconstructed:** July 2026

---

## 1. THE BUSINESS OUTCOME

**North Star:**
Enable real-time HR decision-making for executives so that staff rotation, scheduling, and attendance visibility are never delayed by reporting cycles or individual unavailability — reducing financial risk from manual scheduling gaps across a 5,000-person workforce.

**Why this outcome mattered:**
One Network operates staff across multiple toll plazas. Rotation scheduling is a financial controls mechanism — staff remaining at one plaza too long creates conditions for cash manipulation and fraud. The existing process depended on weekly Excel presentations prepared by department heads and reviewed in executive meetings. If a key executive was unavailable, rotation decisions were delayed by days or weeks. This was not a convenience problem. It was a financial risk problem.

**What success looked like to the CEO:**
- Ad-hoc HR questions answered in real time without scheduling a meeting
- Rotation and scheduling decisions made without waiting for weekly report cycles
- No dependency on any individual being available to surface data

---

## 2. OPPORTUNITY SOLUTION TREE

### Desired Outcome
Reduce time and human cost of executive HR decision-making from weekly reporting cycles to real-time answers — eliminating scheduling delays that create financial risk.

### Opportunities Identified (User Pains)

| # | Opportunity | User | Frequency | Business Impact |
|---|-------------|------|-----------|-----------------|
| 1 | CEO cannot get answers to ad-hoc HR questions without scheduling a meeting | CEO / Executive layer | Daily | High — decisions delayed by days |
| 2 | Department heads waste hours weekly preparing reports that are outdated by presentation time | Department Heads | Weekly | High — wasted resource, stale data |
| 3 | HR data exists in system but inaccessible without technical support or manual extraction | All executives | Daily | High — data trapped in database |
| 4 | Executive unavailability (leave, travel) causes full scheduling cycle to pause | CEO / Senior Managers | Monthly | Critical — financial fraud risk window opens |

### Solution Selected
On-premises RAG chatbot with natural language interface connecting to live SQL HR database and PDF policy documents — enabling any authorized executive to query HR data in real time without technical support, meetings, or manual reporting.

### Why this solution over alternatives
- Cloud-based solution rejected — sensitive HR and financial data cannot leave enterprise perimeter
- Dashboard rejected — static views do not support ad-hoc questions
- Report automation rejected — still dependent on scheduled cycles, does not solve real-time need
- On-prem RAG selected — natural language interface, live data, zero data exposure, works without internet

---

## 3. WHAT DISCOVERY DID NOT HAPPEN (HONEST AUDIT)

| Discovery Step | Done? | Consequence Observed Post-Launch |
|----------------|-------|----------------------------------|
| Define "good answer" with real users before building | No | Performance module delivered numeric scores (3.2/5) when executives expected qualitative narrative answers — data quality failure discovered at launch |
| Map all question types executives would actually ask | Partial | Classification model covered main modules but edge cases caused misclassification |
| Validate data richness before committing to architecture | No | Performance data in SQL was a single float value — insufficient to answer nuanced performance questions |
| Test with real executives before full rollout | No | Failure modes discovered in production, not in testing |
| Define acceptance threshold before building | No | No formal quality gate — shipped without a golden dataset or accuracy benchmark |

---

## 4. SYSTEM ARCHITECTURE (AS BUILT)

**Data Sources:**
- SQL database — structured HR data across four modules: attendance/availability, performance, financial, staff metadata
- PDF files — HR policies, procedures, and documents

**Ingestion Pipeline:**
PDFs → OCR text extraction → chunking → Hugging Face embedding model → Milvus vector database

**Query Pipeline:**
1. User question → Classification model → identifies relevant module(s) (finance / attendance / performance / general / scheduling)
2. Multi-label classification supported — question can belong to multiple modules simultaneously
3. Classified question + original query → SQL query generator → retrieves structured records
4. SQL results + original question → Llama 3.2 → generates natural language answer

**Infrastructure:**
Fully on-premises. Zero cloud dependency. Zero internet. Running on local servers inside One Network's enterprise perimeter. No data ever left the building.

---

## 5. POST-LAUNCH FAILURE ANALYSIS

### Failure #1 — Performance Module: Data Quality Failure
**What happened:** Questions about employee performance improvement over time returned answers like "Fatma improved 0.2 out of 5 in the past three months" — technically correct but useless to an executive making a people decision.

**Root cause:** Data quality failure. The SQL database stored performance as a single numeric score. No qualitative indicators, no contribution data, no comparative context existed in the data layer. The model retrieved and reported correctly. The data was simply insufficient to answer the real question.

**What discovery would have caught this:** One pre-build session asking the CEO "show me an example of a good performance answer" would have revealed that a numeric score alone was not sufficient — before the architecture was committed.

**PM lesson:** RAG systems are only as good as the data they retrieve. Validating data richness is a discovery activity, not an engineering activity. It belongs before the PRD is written.

### Failure #2 — Missing Quality Gate
**What happened:** No golden dataset was built before launch. No accuracy threshold was defined. The system shipped without a formal definition of "good enough."

**What should have been done:** Build a 50-question golden set covering all four modules before build begins. Define a threshold — for example, 85% of golden set questions answered correctly with correct source cited. Do not ship below that threshold regardless of timeline pressure.

---

## 6. WHAT I WOULD DO DIFFERENTLY TODAY

**Before writing a single requirement:**

1. Run five Mom Test interviews with the CEO and two senior executives — not "would you use this" but "walk me through the last time you needed HR data urgently, what did you do, and what made it hard"
2. Audit the SQL database for data richness before committing to RAG architecture — pull 20 sample questions executives would actually ask and check whether the database can answer them at the quality level expected
3. Define the acceptance criteria upfront: minimum accuracy threshold, supported question types, failure behavior when data is insufficient

**During build:**
4. Build a 50-question golden dataset covering all four modules before first model integration
5. Gate each sprint on golden set accuracy — not on feature completion

**Post-launch:**
6. Instrument every query — log classification decisions, retrieval results, and user feedback signals to build a continuous improvement loop

---

## 7. THE ONE-LINE LESSON

A RAG system that retrieves correctly but answers poorly is a data problem, not a model problem — and data problems are discovered in discovery, not in production.
