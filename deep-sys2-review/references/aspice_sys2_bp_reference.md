# ASPICE v4.0 SYS.2 — Base Practices Reference

Source: Automotive SPICE® Process Assessment Model v4.0, UL Solutions / VDA QMC

---

## Process Purpose

The purpose of the System Requirements Analysis Process is to transform the defined stakeholder requirements into a set of system requirements that will guide the design of the system.

---

## Process Outcomes

When successfully implemented, this process produces the following outcomes:

1. System requirements, including non-functional requirements, are structured and prioritized.
2. System requirements are analyzed for correctness and technical feasibility.
3. The impact of the system requirements on the operating environment is analyzed.
4. Consistency and bidirectional traceability are established between stakeholder requirements and system requirements.
5. System requirements are agreed and communicated to all affected parties.
6. Verification criteria are defined for each system requirement.

---

## Base Practices (BP1–BP8)

### BP1 — Specify System Requirements
**Statement:** Use the stakeholder requirements and changes to the stakeholder requirements to identify the required functions and capabilities of the system. Specify functional and non-functional system requirements in a system requirements specification.

**Evidence required:**
- System Requirements Specification (SyRS) containing functional requirements
- Non-functional requirements (performance, reliability, safety, security, EMC, environmental)
- Requirements derived from all relevant StRS items
- Requirements addressing design constraints and regulatory compliance

**Common gaps:**
- Only functional requirements documented; NFRs missing
- Requirements copied verbatim from StRS without system-level elaboration
- Interface requirements not specified as separate requirement type

---

### BP2 — Structure System Requirements
**Statement:** Structure the system requirements by: grouping into project-relevant clusters, establishing a logical order, categorizing requirements by type, prioritizing requirements by stakeholder value, and assigning functional content to planned releases.

**Evidence required:**
- Requirements organized into logical sections/categories
- Priority assignment (High/Medium/Low or MoSCoW)
- Release assignment for incremental development
- Requirement status tracking (Draft/Under Review/Approved/Implemented/Verified)

**Common gaps:**
- No priority assigned to requirements
- Flat unstructured list with no grouping
- No release assignment in incremental projects

---

### BP3 — Analyze System Requirements
**Statement:** Analyze the specified system requirements including their interdependencies to verify correctness, technical feasibility, and verifiability, and to support risk identification.

**Evidence required:**
- Review records showing correctness analysis
- Technical feasibility assessment (especially for performance/safety requirements)
- Verifiability assessment — can each requirement be tested/verified?
- Risk register or risk identification record for high-risk requirements
- Interdependency analysis (conflicting requirements flagged and resolved)

**Common gaps:**
- No evidence of feasibility analysis (requirement accepted without technical review)
- Verifiability not checked — requirements with "sufficient", "adequate" accepted
- No risk identification linked to requirements
- Conflicting requirements not identified

---

### BP4 — Analyze the Impact on the Operating Environment
**Statement:** Identify the interfaces between the specified system and other elements of the operating environment. Analyze the impact of the system requirements on these interfaces.

**Evidence required:**
- Interface specification or Interface Control Document (ICD)
- Context diagram showing system boundary and external entities
- Interface requirements for each external system/actor
- Impact analysis: how do requirements change the external environment?

**Common gaps:**
- No interface specification document
- System boundary not clearly defined
- External interfaces identified but not specified at requirement level

---

### BP5 — Develop Verification Criteria ⭐ CRITICAL
**Statement:** Develop verification criteria for each system requirement that represent qualitative and quantitative measures for the verification of a requirement within the constraints agreed with the customer.

**This is the most commonly missing BP in SyRS assessments.**

**Evidence required per requirement:**
- Verification method: Test (T), Analysis (A), Inspection (I), or Demonstration (D)
- Qualitative criteria: description of what is measured and how
- Quantitative threshold: measurable pass/fail boundary with units
- Reference to test standard or method (e.g., AEC-Q100, IEC 61000, ISO 16750)

**Format (mandatory):**
```
Method:    T / A / I / D
Criteria:  [Description of what is measured, how, and under what conditions]
Threshold: [Numeric pass/fail value with units]
```

**Example — Temperature Requirement:**
```
Method:    T (Test)
Criteria:  Power-on functional verification after 1000h exposure at -40°C and +105°C 
           per AEC-Q100 Grade 2 HTOL/LTOL test sequence, 3 sample units minimum
Threshold: 0 functional failures; all parametric values within ±10% of nominal
```

**Common gaps:**
- Verification method = "TBD" or blank
- Only qualitative description, no quantitative threshold
- Threshold without units ("shall pass test" — not quantitative)
- Circular criteria ("verified when requirement is met")

---

### BP6 — Establish Bidirectional Traceability
**Statement:** Establish bidirectional traceability between stakeholder requirements and system requirements.

**Evidence required:**
- Traceability matrix: every SysReq → at least one StRS item
- Reverse: every StRS item → at least one SysReq
- No orphan SysReqs (SysReq with no upstream StRS link)
- No uncovered StRS items (StRS item with no derived SysReq)
- Traceability maintained after changes (change history)

**Common gaps:**
- One-directional traceability only (StRS→SyRS but not reverse)
- Orphan requirements (SysReq not linked to any StRS)
- Traceability matrix reconstructed after the fact, not maintained live
- Coverage gaps: StRS items with no corresponding SysReqs

---

### BP7 — Verify Consistency
**Statement:** Ensure consistency between the stakeholder requirements and system requirements. Consistency and bidirectional traceability can be demonstrated by review records.

**Evidence required:**
- Review records (meeting minutes, review checklists) showing StRS ↔ SyRS consistency check
- Resolution records for identified inconsistencies
- Sign-off by stakeholder representative confirming consistency
- Change history showing SyRS updated when StRS changed

**Common gaps:**
- No formal consistency review documented
- SyRS updated but consistency with StRS not re-verified
- Review records do not reference specific SyRS↔StRS links

---

### BP8 — Communicate Updates
**Statement:** Communicate the agreed system requirements and updates to the system requirements to all relevant parties.

**Evidence required:**
- Distribution list for SyRS release
- Meeting minutes or email confirming stakeholder receipt/acknowledgment
- Baseline management: version control with formal release notification
- Change notification process for requirement updates

**Common gaps:**
- SyRS distributed informally (no acknowledgment trail)
- No version control; stakeholders working from different versions
- No process for communicating requirement changes after baseline

---

## Process Work Products (ASPICE v4.0)

| Work Product ID | Name | BP Relevance |
|----------------|------|-------------|
| 13-50 | System Requirements Specification (SyRS) | BP1, BP2, BP3 |
| 13-51 | Verification Criteria | BP5 |
| 13-52 | Communication Evidence | BP8 |
| 14-50 | Traceability Record | BP6, BP7 |
| 15-51 | Analysis Results | BP3, BP4 |
| 13-22 | Review Record | BP7 |

---

## CL1 Minimum Evidence (What You Need to Pass)

For Capability Level 1 (Performed), assessors expect:
1. SyRS exists with functional AND non-functional requirements (BP1)
2. Requirements are categorized/grouped (BP2 — minimal evidence)
3. Evidence requirements were reviewed for feasibility (BP3 — review record)
4. Interface list exists (BP4 — minimal)
5. **Verification criteria exist for at least the high-priority requirements (BP5)**
6. Traceability matrix StRS↔SyRS (BP6 — even partial is better than none)
7. Some evidence of consistency check (BP7 — meeting minutes sufficient)
8. SyRS was sent to relevant parties (BP8 — email trail sufficient)

**BP5 is the most commonly failed BP at CL1 assessments.** Teams have requirements but no verification criteria.
