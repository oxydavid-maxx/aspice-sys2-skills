# Failure Paths — SYS.2 Process Recovery Strategies

---

| # | Failure Scenario | Trigger | Recovery |
|---|-----------------|---------|---------|
| F1 | No StRS available | User has system requirements but no upstream stakeholder requirements | Create skeleton StRS from known customer inputs; tag SysReqs as "StRS-TBD-XX"; flag for BP6 gap |
| F2 | All requirements in one document, mixed levels | Requirements mix system-level and HW/SW-level details | Classify each requirement; move HW/SW-level items to SWE.1/HWE.1; keep only system-level in SyRS |
| F3 | Verification criteria all blank | SyRS exists but BP5 never done | Use `verif-gen` mode; process requirements by type using IADT template; prioritize high-priority requirements first |
| F4 | No traceability matrix | Requirements exist but no StRS↔SyRS links | If StRS exists: build traceability now; if no StRS: document as "internal system requirements" with rationale; flag BP6 gap |
| F5 | Requirements are design specifications | Requirements describe internal implementation | Rewrite as black-box behavioral requirements; move design details to architecture document |
| F6 | Contradictory requirements | Two SysReqs conflict | Mark both as "Under Review — Conflict"; escalate to system architect; document resolution decision |
| F7 | Requirements contain TBD | Cannot accept into baseline | Hold at Draft; identify TBD owner; set deadline; do not baseline until resolved |
| F8 | No review evidence | SyRS exists but no review records | Conduct peer review now; document in review meeting minutes; retroactive review is acceptable with caveats |
| F9 | Requirements exceed system scope | SysReqs specify behavior of external systems | Identify out-of-scope requirements; move to interface specification; replace with interface requirements |
| F10 | Vague performance requirements | Requirements use forbidden terms | Use `quality-fix` mode; apply INCOSE rules; generate quantitative alternatives |
