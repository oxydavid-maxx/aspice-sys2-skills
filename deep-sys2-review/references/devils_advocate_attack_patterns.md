# Devil's Advocate Attack Patterns — 20 Adversarial Assessment Angles

Used by `devils_advocate_reviewer_agent`. Each attack pattern has: the angle, how to detect it, and the expected severity.

---

## Pattern 1: The Circular Verification Criterion
**Attack:** "This verification criterion is self-referential — it cannot be executed."
**Detection:** Criteria text contains: "verified when the system meets the requirement", "confirmed when operational", "tested per requirement"
**Severity:** CRITICAL (BP5 failure)
**Quote format:** `SysReq-XXX Criteria: "[quote circular text]" — this is circular; no executable procedure`

---

## Pattern 2: The Orphan Farm
**Attack:** "X out of N requirements have no upstream StRS link. These are unsolicited gold-plating."
**Detection:** SysReqs without StRS upstream link
**Severity:** CRITICAL if >15% orphans (BP6 N rating)
**Quote format:** `SysReq-XXX, SysReq-YYY, [N more] — no StRS upstream link`

---

## Pattern 3: The Trivial All-to-One Traceability
**Attack:** "All requirements trace to a single 'The system shall comply with customer requirements' StRS item. This proves nothing — it's a paper trail, not real traceability."
**Detection:** >50% of SysReqs link to one StRS item
**Severity:** MAJOR (BP6 P at best despite 100% coverage)

---

## Pattern 4: The TBD Black Hole
**Attack:** "This requirement says [TBD] in its verification criteria. No test can be written from this. Who owns this TBD and what is the due date?"
**Detection:** "TBD", "TBC", "to be defined" in any mandatory field
**Severity:** CRITICAL for verification criteria TBDs; MAJOR for description TBDs

---

## Pattern 5: The Unitless Number
**Attack:** "This requirement says '100' with no unit. 100 what? Microseconds? Milliseconds? Volts? This cannot be verified."
**Detection:** Numeric value without unit in description or threshold
**Severity:** MAJOR (IEEE 29148 §5.2.4 violation; INCOSE R6)

---

## Pattern 6: The Vague Performance Promise
**Attack:** "The system shall perform 'quickly' / 'efficiently' / 'reliably'. These are marketing claims, not requirements. No test engineer can write a test for these."
**Detection:** Forbidden terms in description (see INCOSE forbidden terms blacklist)
**Severity:** MAJOR (fails verifiability characteristic)

---

## Pattern 7: The Compound Requirement
**Attack:** "This single requirement ID covers two independent behaviors. When one passes and one fails, how do you track status? You need two requirements."
**Detection:** Multiple "shall" statements; "and" between two verbs in one requirement
**Severity:** MAJOR (IEEE 29148 singular characteristic violation)

---

## Pattern 8: The Frozen Traceability Matrix
**Attack:** "The traceability matrix shows all links created on the same date. Requirements existed for months before. This was assembled for the assessment, not maintained throughout development."
**Detection:** Link timestamps all identical or concentrated in one session
**Severity:** CRITICAL (BP7 assessment integrity concern)

---

## Pattern 9: The Missing Extreme Conditions
**Attack:** "The verification criteria tests at 25°C only. This product goes in a car — -40°C and +105°C are operating conditions. Where are the extreme condition tests?"
**Detection:** No temperature variation in test conditions; no voltage extremes; no worst-case scenarios
**Severity:** MAJOR (BP5 — incomplete verification criteria)

---

## Pattern 10: The Implementation Leaking Through
**Attack:** "This requirement says the system 'shall use an SPI EEPROM.' That's a design decision, not a requirement. If someone later wants to use I2C EEPROM, this requirement blocks them needlessly."
**Detection:** Component names, internal architecture terms in requirements
**Severity:** MAJOR (IEEE 29148 appropriate characteristic violation; INCOSE R3)

---

## Pattern 11: The No Safety Requirements on a Safety-Relevant System
**Attack:** "This is an automotive electronic control unit. Where are the ISO 26262 ASIL requirements? The diagnostic coverage requirements? The FMEA-derived requirements? Their absence is itself a gap."
**Detection:** No NFR-Safety requirements in a clearly safety-relevant product
**Severity:** CRITICAL for safety-relevant products

---

## Pattern 12: The Self-Fulfilling Priority
**Attack:** "100% of requirements are marked 'High Priority.' This is meaningless. Priority exists to guide implementation order. If everything is High, nothing is high."
**Detection:** All requirements with same priority
**Severity:** MINOR (BP2 gap)

---

## Pattern 13: The Missing Interface Requirement
**Attack:** "The scope diagram shows 4 external interfaces: CAN, SPI, GPIO, and Power. There are CAN requirements, but I see no SPI requirements, no GPIO requirements, and no power supply requirements. How will the system interact with these interfaces?"
**Detection:** Interface present in context diagram but no corresponding IR requirements
**Severity:** MAJOR (BP4 gap; incomplete BP1)

---

## Pattern 14: The Passive Voice Requirement
**Attack:** "The requirement says 'shall be detected by the system.' By what? Who detects? The subject is the system — it should be 'The system shall detect.' Passive voice creates ambiguity about responsibility."
**Detection:** Passive voice in description (INCOSE R2)
**Severity:** MINOR (conforming characteristic violation)

---

## Pattern 15: The No Reliability Requirements
**Attack:** "I see functional requirements, but where are the MTBF requirements? The FPMH budget? For automotive grade, reliability specifications are standard customer requirements. Are they missing or intentionally excluded?"
**Detection:** No NFR-Reliability requirements
**Severity:** MAJOR for automotive products (assess product context)

---

## Pattern 16: The Uncovered StRS
**Attack:** "StRS-XXX says the customer needs [X], but I cannot find any SysReq that addresses this. Is this intentionally out of scope? If so, where is the rationale?"
**Detection:** StRS items with no downstream SysReq
**Severity:** CRITICAL if customer-critical requirement; MAJOR otherwise

---

## Pattern 17: The Unreviewable Consistency
**Attack:** "There are no review records. BP7 requires demonstrating consistency between StRS and SyRS. Without review records, this cannot be demonstrated — regardless of whether the content is actually consistent."
**Detection:** No meeting minutes, review checklists, or sign-off records
**Severity:** CRITICAL for BP7 (no evidence = N rating)

---

## Pattern 18: The Missing Mermaid Context
**Attack:** "There is no system context diagram. BP4 requires identifying interfaces with the operating environment. Without a context diagram, it is impossible to verify that all interfaces were identified."
**Detection:** No context diagram or system boundary diagram in document
**Severity:** MAJOR (BP4 evidence gap)

---

## Pattern 19: The Infeasible Requirement
**Attack:** "This requirement specifies 0ms response time. This violates the laws of physics. Either this is a typo or the author does not understand the system. Either way, it cannot be verified and should not be in the spec."
**Detection:** Requirements with physically impossible or technically unsupported thresholds
**Severity:** MAJOR (feasibility characteristic violation)

---

## Pattern 20: The Gold-Plated Requirement
**Attack:** "SysReq-XXX requires 10 Gbps throughput. The rest of the system uses CAN bus at 500 kbps. No StRS requirement motivates this. This looks like internal over-engineering with no customer justification."
**Detection:** Orphan requirements with significantly higher specs than related requirements
**Severity:** MAJOR (necessary characteristic violation; no StRS derivation)
