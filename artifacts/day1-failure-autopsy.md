# FAILURE AUTOPSY MEMO
**Product:** Inter-Services Secure Communication System
**Role:** Technical Product Owner
**Organization:** Pakistan Air Force (Air Headquarters)
**Document type:** PM learning artifact, no system internals, no classified information

---

## THE PROBLEM THAT EXISTED

Inter-services communication between PAF, Army, Navy, ISI, and JSHQ ran entirely on physical correspondence. A request requiring action from a sister service would take seven to ten days to complete a single round trip. The process involved drafting, signing through a chain of four or more officials, physical dispatch, receipt, response, and return. In time-sensitive operational scenarios, this was not a process inefficiency. It was a mission risk.

---

## WHAT WAS BUILT AND WHY

Executives commissioned a secure Office Automation System to digitize this correspondence chain. The decision on what to build, which features to include, and by when came entirely from the senior official layer. Air Commodores, AVMs, and above made these calls. The PM role was to deliver the feature list on time and within budget. Discovery was not part of the mandate.

---

## WHAT DISCOVERY DID NOT HAPPEN

| Discovery Step | Was it done? | Consequence |
|----------------|--------------|-------------|
| User interviews with actual operators (PAs, junior staff) | No | Interface designed for executives, not operators |
| Workflow observation of existing letter process | No | System replicated bureaucratic structure instead of simplifying it |
| Low-fidelity prototype tested with real users | No | Complexity discovered at launch, not before |
| Training needs assessment | No | Multiple remedial training rounds across all PAF bases |
| Usability testing before rollout | No | Navigation required 6 or more sequential screens for one letter |

---

## ROOT CAUSES

**1. Feature team, not product team.**
The PM had no authority to question what was being built or run discovery. The roadmap was handed down. The job was execution.

**2. Wrong user in the room.**
The executives who commissioned the product were approvers of letters. The actual users were PAs and junior staff who drafted, routed, and dispatched them. No one in the decision room represented the real user.

**3. No discovery phase.**
Zero interviews. Zero observation. Zero prototype validation. The first time real users touched the system was at launch.

**4. Design reflected organizational hierarchy, not user workflow.**
Six sequential screens to draft one letter. Terminology matched formal military correspondence structure, not how a PA actually thinks about their job. Training failed because the interface required unlearning existing mental models entirely.

**5. Success defined as shipping, not adoption.**
The product launched on time. That was considered success. Adoption struggles, repeated training cycles, and user resistance were treated as implementation problems rather than product failures.

---

## WHAT I WOULD DO DIFFERENTLY TODAY

Before writing a single requirement:

1. Spend two weeks shadowing three PAs across different bases. Watch exactly how they draft, route, and dispatch a letter today.
2. Run five Mom Test interviews with junior staff. Not "would you use this system" but "walk me through the last letter you sent and what made it hard."
3. Build a paper prototype of the new flow and put it in front of a PA. Watch where they get confused before writing one line of code.

During build:

4. Define adoption rate and time-to-send reduction as the success metrics, not launch date.
5. Gate each phase on usability scores from actual operators, not executive sign-off.

---

## THE ONE-LINE LESSON

When the people who commission a product are not the people who use it, and no one bridges that gap before building, you will ship on time and fail anyway.
