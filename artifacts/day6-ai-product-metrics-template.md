# AI PRODUCT METRICS TEMPLATE
## Reusable Framework for Any Enterprise AI Product
**Author:** Irtaqa Naveed, Senior AI Product Manager
**Date:** July 2026
**License:** Free to use and adapt

---

## HOW TO USE THIS TEMPLATE

Copy this template for any AI product you are building or evaluating. Fill in each section with your specific product context. The structure works for RAG systems, LLM copilots, AI agents, recommendation systems, and any other AI-powered product.

The four-layer structure is non-negotiable. Every AI product needs all four layers or your metrics system has blind spots.

---

## LAYER 1: NORTH STAR METRIC

**Definition template:**
"Number of [time periods] where [primary user] completed [core job to be done] using the AI product WITHOUT reverting to the previous manual workflow."

**How to fill this in:**
Identify the behavior the AI product is supposed to replace. Your North Star measures whether that replacement actually happened, not whether the product was used.

**Example (HRMS Copilot):**
Number of weeks the CEO completed the work week without requesting a manual HR presentation and without making ad-hoc clarification calls to staff.

**The test for a good North Star:**
Ask: could this number go up while the product is actually failing users? If yes, it is a vanity metric, not a North Star. A good North Star cannot be gamed.

**Your North Star:**
[Fill in]

**Target:**
[Fill in — must be a specific number, not a direction]

**Measurement method:**
[Fill in — how will you actually track this behavior change?]

---

## LAYER 2: INPUT METRICS

These are the four to six things that must be true for the North Star to improve. Think of them as the levers. Pull them up, North Star follows.

For AI products, always include these three baseline input metrics plus product-specific ones:

**Baseline Input 1: Output Accuracy Rate**
Definition: Percentage of AI outputs that are factually correct, verified against ground truth.
Why always include: Every AI product can produce wrong outputs. If you do not measure accuracy you do not know whether your product is delivering value or confident nonsense.
Your target: [Fill in — must be a number, not "high"]
Measurement: Golden dataset run weekly. Score each output pass or fail against verified ground truth.

**Baseline Input 2: Task Completion Rate**
Definition: Percentage of user intents that result in a completed, useful output without the user abandoning or rephrasing more than once.
Why always include: Measures whether the AI understood what the user needed. A technically accurate answer to the wrong question is still a failure.
Your target: [Fill in]
Measurement: Session logs. Flag sessions where user rephrased the same intent more than once or abandoned without receiving an answer.

**Baseline Input 3: Response Latency**
Definition: Time from user input to AI output displayed, end-to-end including all retrieval and generation steps.
Why always include: Speed is a trust signal. Slow AI feels unreliable even when accurate. Above 10 seconds in most enterprise contexts causes abandonment.
Your target: [Fill in — P95 latency in seconds]
Measurement: Log timestamps at input submission and output render. Calculate P50, P90, P95 daily.

**Product-specific Input 4:**
[Fill in — what is unique to your product that drives the North Star?]
Definition: [Fill in]
Target: [Fill in]
Measurement: [Fill in]

**Product-specific Input 5:**
[Fill in]

---

## LAYER 3: GUARDRAIL METRICS

These are your red lines. Unlike input metrics which you optimize toward, guardrails are thresholds you must never cross. A single breach triggers incident response, not a quarterly review.

**Standard AI Product Guardrails:**

**Guardrail 1: Hallucination Rate on High-Stakes Outputs**
Definition: Any instance where the AI returns a confidently stated factual claim that is verifiably false on a query category that carries real-world consequences.
Target: Zero on your highest-stakes query types.
Response if breached: Suspend the affected feature. Root cause analysis before re-launch.

**Guardrail 2: Unauthorized Data Exposure**
Definition: Any instance where the AI returns data to a user who does not have access rights to that data.
Target: Zero.
Response if breached: Immediate system audit. Suspend until access control is verified.

**Guardrail 3: Data Staleness Without Warning**
Definition: Any instance where the AI returns an answer based on data older than your defined freshness threshold, without displaying a staleness warning to the user.
Target: Zero.
Response if breached: Add freshness timestamps to all answers within 24 hours.

**Product-specific Guardrail 4:**
[Fill in — what is the catastrophic failure mode unique to your product?]
Definition: [Fill in]
Target: [Fill in]
Response if breached: [Fill in]

---

## LAYER 4: INSTRUMENTATION PLAN

You cannot measure what you do not log. This section defines the minimum events every AI product must capture.

**Universal minimum logging requirements:**

| Event | Minimum data to capture |
|-------|------------------------|
| User input submitted | Timestamp, user ID, input text or intent category, session ID |
| AI processing steps | Which modules activated, confidence scores, processing time per step |
| Output generated | Output text, generation time, sources cited or retrieved |
| User response to output | Did user accept, rephrase, abandon, or follow up? |
| Session outcome | Did user complete their intended task? |
| Error or failure event | What failed, at which step, error type |

**Behavior change logging (hardest but most important):**
For any AI product replacing a manual workflow, you must track whether the old workflow is still being used alongside or instead of the AI product. This often cannot be automated. Define a manual tracking protocol.

Manual tracking method: [Fill in — weekly survey, assistant log, calendar analysis, etc.]

---

## WEEKLY METRICS REVIEW CHECKLIST

Run this every Monday before any product work. Takes 30 to 60 minutes. Non-negotiable.

1. Run golden dataset. Record accuracy score vs target.
2. Pull P95 latency from logs. Flag if above target.
3. Review task completion rate. Identify top abandonment points.
4. Check all guardrail metrics. If any breach detected, stop all other work.
5. Update North Star tracker. Record whether target behavior occurred last week.
6. Identify the one input metric furthest from target. Make it the team's focus this week.

---

## RED FLAGS: YOUR METRICS SYSTEM IS BROKEN IF...

Any of these are true, fix your metrics system before building new features:

Your North Star went up but users are still using the old manual workflow.
Your accuracy rate is high but users keep rephrasing the same question.
You have no way to tell whether the AI's output was correct or not without checking manually every time.
A guardrail breach happened and nobody noticed for more than 24 hours.
Your team disagrees on what the North Star number was last week.
You are measuring the volume of AI outputs instead of the quality of outcomes.

---

*Template created as part of a Senior AI PM portfolio. Based on real product experience building and shipping on-premises RAG and LLM systems in regulated enterprise environments. Free to use and adapt with attribution.*
