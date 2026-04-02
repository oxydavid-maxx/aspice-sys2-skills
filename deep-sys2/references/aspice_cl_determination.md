# ASPICE Capability Level Determination — SYS.2

Source: ASPICE v4.0 PAM, NPLF Rating Scale, iNTACS assessor guidance

---

## NPLF Rating Scale

| Rating | Code | Achievement Level | Score Range |
|--------|------|------------------|-------------|
| Not achieved | N | 0–15% | 0–15 |
| Partially achieved | P | >15–50% | 16–50 |
| Largely achieved | L | >50–85% | 51–85 |
| Fully achieved | F | >85–100% | 86–100 |

---

## Capability Level Determination Rules

### CL0 — Incomplete
- At least one BP in CL1 is rated N
- The process does not achieve its purpose

### CL1 — Performed
**Required:** All 8 SYS.2 BPs rated L or F
- If any BP is rated N or P → CL1 NOT achieved
- **Critical BPs** (most commonly failed): BP5 (Verification Criteria), BP6 (Traceability)

**CL1 evidence minimum:**
- SyRS document exists (BP1, BP2)
- Review record showing requirements were analyzed (BP3)
- Interface list or context diagram (BP4)
- Verification criteria for at least high-priority requirements (BP5)
- Traceability matrix StRS↔SyRS (BP6)
- Review records confirming consistency (BP7)
- Distribution evidence (BP8)

### CL2 — Managed
**Required:** CL1 achieved AND all 4 Generic Practices for CL2 rated L or F:
- GP2.1.1: Define performance objectives for the process
- GP2.1.2: Plan process performance to fulfill objectives
- GP2.1.3: Monitor process performance
- GP2.1.4: Adjust process performance to meet objectives

**Additional requirements (Generic Resources GR 2.1):**
- Resources identified and available
- Responsibilities assigned
- Process performance documented

**CL2 evidence minimum:**
- Process definition (how SYS.2 is performed in the organization)
- Work product identification and control (SyRS under version control)
- Quality review records
- Progress monitoring evidence (metrics, milestone tracking)

### CL3 — Established
**Required:** CL2 achieved AND:
- Standard process defined for SYS.2
- Process is tailored from the standard process
- Process assets (templates, guidelines) used

---

## Per-BP NPLF Quick Reference

### BP1 — Specify System Requirements
| Rating | Evidence |
|--------|---------|
| F (86–100%) | SyRS covers all requirement types; complete; no TBDs; all StRS items addressed |
| L (51–85%) | SyRS mostly complete; minor gaps in NFRs or edge cases |
| P (16–50%) | SyRS exists but significant gaps; many TBDs; limited requirement types |
| N (0–15%) | No SyRS; or SyRS is a list of bullet points with no structure |

### BP5 — Develop Verification Criteria (Most Failed)
| Rating | Evidence |
|--------|---------|
| F (86–100%) | All requirements have Method + qualitative Criteria + quantitative Threshold |
| L (51–85%) | Most requirements have criteria; some missing thresholds |
| P (16–50%) | Fewer than half have any verification criteria |
| N (0–15%) | No verification criteria field; or all fields blank/TBD |

### BP6 — Bidirectional Traceability
| Rating | Evidence |
|--------|---------|
| F (86–100%) | 100% SysReq→StRS; 100% StRS→SysReq; evidence in live tool |
| L (51–85%) | >85% coverage; minor orphans documented with rationale |
| P (16–50%) | Traceability matrix exists but <85% coverage; orphans present |
| N (0–15%) | No traceability matrix; or only one direction; or generic all-to-all links |

---

## Scoring Approach for Confidence Scores

For `deep-sys2-review confidence-score` mode, map NPLF to 0–100:

```
N (0–15%) → Score 0–15   (use midpoint: ~8)
P (16–50%) → Score 16–50  (use midpoint: ~33)
L (51–85%) → Score 51–85  (use midpoint: ~68)
F (86–100%) → Score 86–100 (use midpoint: ~93)
```

**Overall CL1 confidence = weighted average of all 8 BP scores:**
- BP5 weight: 20% (highest — most commonly missing)
- BP6 weight: 20% (highest — traceability is critical evidence)
- BP1 weight: 15% (foundation)
- BP3 weight: 15% (feasibility + verifiability analysis)
- BP2, BP4, BP7, BP8: 7.5% each

**Threshold:**
- Overall score ≥ 51% → CL1 achievable (some BPs may still need work)
- Overall score ≥ 86% → CL1 highly likely to pass assessment
- Any single BP score = N (0–15%) → CL1 BLOCKED regardless of total score
