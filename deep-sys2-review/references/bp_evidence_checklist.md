# BP Evidence Checklist — What Assessors Accept as Proof

For each BP, this checklist defines what evidence FULLY satisfies (F rating), what LARGELY satisfies (L), and what constitutes gaps (P/N).

---

## BP1 — Specify System Requirements

**F (Fully, 86–100%):**
- SyRS contains all requirement types: Functional, NFR-Performance, NFR-Reliability, Interface, Design Constraint
- All requirements have unique IDs, titles, and descriptions
- No TBD/TBC fields; all conditions fully stated
- Requirements address all interfaces identified in system boundary

**L (Largely, 51–85%):**
- Most requirement types present; minor gaps (e.g., no security requirements)
- A few TBDs documented with owner and due date
- Interface requirements mostly complete

**P (Partially, 16–50%):**
- Only functional requirements; no NFRs
- Significant TBD count without resolution dates
- Interface requirements missing for some external interfaces

**N (Not, 0–15%):**
- No formal SyRS; or bullet list without structure
- Requirements copied from StRS without system-level elaboration
- No non-functional requirements of any kind

---

## BP2 — Structure System Requirements

**F:** Requirements organized by type; priority assigned (High/Medium/Low); status tracked; release assignment present for incremental projects
**L:** Requirements grouped; most have priority; some missing status
**P:** Flat list; no grouping; priority inconsistently assigned
**N:** Unstructured list; no priority; no status

---

## BP3 — Analyze System Requirements

**F:** Review records show requirements checked for correctness, feasibility, and **verifiability**; conflicts resolved with documented rationale; risk assessment for high-risk requirements
**L:** Review meeting minutes exist; most requirements reviewed; minor gaps in feasibility analysis
**P:** Some review records; not all requirements reviewed; no feasibility analysis
**N:** No evidence of any systematic review; no review records

---

## BP4 — Operating Environment

**F:** System context diagram; ICD or interface specification document; all external interfaces specified as requirements; operating environment parameters stated (temperature, voltage, EMC)
**L:** Context diagram present; interfaces mostly specified; minor environment gaps
**P:** Interface list exists but not specified as requirements; no context diagram
**N:** No interface documentation; system boundary undefined

---

## BP5 — Verification Criteria ⭐ (Most Critical)

**F:** EVERY requirement has: (1) IADT method specified, (2) qualitative criteria description, (3) quantitative threshold with units, (4) reference standard where applicable. Zero TBD/blank.
**L:** ≥86% of requirements have complete criteria; remaining have method but missing threshold
**P:** 16–85% of requirements have any criteria; most are partial (method only or no threshold)
**N:** <16% have any criteria; or all fields blank/TBD; or all criteria are circular

**Assessor check sequence:**
1. Count requirements with blank verification method → if >15%, N rating
2. Count requirements with no quantitative threshold → if >15%, at most P rating
3. Check for circular criteria → if >10% circular, at most P rating

---

## BP6 — Bidirectional Traceability

**F:** 100% SysReq→StRS coverage; 100% StRS→SysReq coverage; live tool with link history; no orphans; no uncovered StRS items; explicit link types
**L:** ≥85% upstream; ≥85% StRS coverage; minor orphans with documented rationale
**P:** Traceability matrix exists; 16–84% coverage; orphans present without rationale
**N:** No traceability matrix; or only one direction; or all-to-one trivial mapping

**Assessor red flags:**
- All links created on same date (reconstructed evidence)
- All SysReqs link to one top-level StRS (trivial mapping)
- No test case links (horizontal traceability missing)

---

## BP7 — Verify Consistency

**F:** Review records explicitly confirm StRS↔SyRS consistency; named reviewer, date, specific pairs checked; change history shows SyRS updated after StRS changes
**L:** Meeting minutes reference consistency check; most pairs confirmed; minor gaps
**P:** Some review records exist; not explicitly about consistency; or informal email trail only
**N:** No review records; or SyRS changed without any consistency check evidence

---

## BP8 — Communicate

**F:** Distribution list with acknowledgment; version control with formal release notification; change notification process documented
**L:** SyRS was sent to stakeholders; version history exists; minor gaps in formal acknowledgment
**P:** Informal distribution; no acknowledgment trail; or stakeholders have different versions
**N:** No evidence of distribution; or key stakeholders not informed
