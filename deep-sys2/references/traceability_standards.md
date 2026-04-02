# Traceability Standards — ASPICE SYS.2 BP6/BP7

Source: ASPICE v4.0 PAM + Sodius Willert ASPICE Traceability Guide + iNTACS assessor guidance

---

## Overview

Traceability is the existence of meaningful references or links between work products. ASPICE does not mandate a specific format — it mandates demonstrable outcomes: coverage, consistency, and impact analysis capability.

---

## 3-Directional Traceability Model

### Direction 1 — Vertical Upward (BP6: Upstream Traceability)
**What:** SysReq → StRS item (Stakeholder Requirement)
**Purpose:** Prove every system requirement has a stakeholder origin (no gold-plating)
**Mandatory:** YES — no SysReq without an upstream link = CL1 failure

```
StRS-100 Customer needs reliable CAN communication
  └── SysReq-001 CAN Bus Bit Rate Specification          [derives-from StRS-100]
  └── SysReq-002 CAN Bus Error Detection Capability      [derives-from StRS-100]
  └── SysReq-003 CAN Bus-Off Recovery Time               [derives-from StRS-100]
```

**Orphan requirement** = SysReq with no StRS link → CRITICAL BP6 violation

---

### Direction 2 — Vertical Downward (Downstream Traceability)
**What:** SysReq → Design element / Architecture component / Test case
**Purpose:** Prove every requirement is implemented and verified
**Mandatory:** YES for CL2+; Best effort at CL1

```
SysReq-001 CAN Bus Bit Rate Specification
  └── HWDesign-CAN-PHY-001    [satisfied-by] (downstream to HW design)
  └── SWArch-CAN-Driver-001   [satisfied-by] (downstream to SW architecture)
  └── SysQt-001 CAN Bit Rate Test [verified-by] (downstream to test case)
```

**Dangling requirement** = SysReq with no downstream link to test case → unverifiable (BP5 + BP6 gap)

---

### Direction 3 — Horizontal (Verification Traceability)
**What:** SysReq ↔ Test case (bidirectional)
**Purpose:** Prove test coverage (every requirement has a test; every test has a requirement)
**Mandatory:** YES — one of ASPICE's most-checked evidence items

```
SysReq-001 ←→ SysQt-001  [verifies / verified-by]
SysReq-002 ←→ SysQt-002  [verifies / verified-by]
SysReq-002 ←→ SysQt-003  [verifies / verified-by]  ← one req, multiple tests: OK
SysQt-004 ←→ ???          ← test with no requirement: orphan test (FAIL)
```

---

## Link Types (Use Explicit Types in Traceability Matrix)

| Link Type | Direction | Meaning |
|-----------|-----------|---------|
| `derives-from` | SysReq → StRS | System requirement derived from stakeholder requirement |
| `refines` | SysReq → SysReq | One requirement adds detail to another |
| `satisfies` | Design → SysReq | Design element implements the requirement |
| `verifies` | Test → SysReq | Test case verifies the requirement |
| `verified-by` | SysReq → Test | Reverse link of "verifies" |
| `conflicts-with` | SysReq → SysReq | Requirements are in conflict (must be resolved) |
| `depends-on` | SysReq → SysReq | Implementation requires another requirement first |

---

## Completeness Rules

### Coverage Metrics (Minimum for ASPICE CL1)

| Metric | Minimum | Target |
|--------|---------|--------|
| Upstream coverage: % SysReq with ≥1 StRS link | 100% | 100% |
| StRS coverage: % StRS items with ≥1 SysReq | 95% | 100% |
| Test coverage: % SysReq with ≥1 test case | 80% | 100% |
| BP5 coverage: % SysReq with verification criteria | 80% | 100% |

**Note:** 95% StRS coverage (not 100%) allows for StRS items that are intentionally excluded from scope (with documented rationale).

---

## Consistency Rules (BP7)

### What "Consistent" Means:
1. SysReq content aligns with the StRS item it traces to (not contradictory, not a super-set beyond what StRS says)
2. When StRS changes → linked SysReqs are reviewed and updated (or explicitly confirmed unchanged)
3. Review records document the consistency check (meeting minutes, checklist)

### Change Impact Workflow:
```
StRS change detected
  → Identify all SysReqs linked to changed StRS item
  → Review each linked SysReq for needed update
  → If SysReq changed: new version, record reason
  → If SysReq unchanged: document "reviewed, no change needed" with reviewer name + date
  → Update traceability matrix
  → Notify downstream: HW/SW design, test cases
```

### Consistency Review Evidence (What Assessors Check):
- Review meeting minutes listing which SysReqs were checked against which StRS
- Sign-off sheet showing reviewer confirmed each traced pair is consistent
- Change history in requirements tool showing SyRS updated after StRS change

---

## What Assessors Actually Check (iNTACS Guidance)

### CL1 Assessment Checklist:
1. Does a traceability matrix exist? (Yes/No)
2. Does every SysReq have at least one upstream StRS link? (% orphans)
3. Does every StRS item have at least one SysReq? (% uncovered)
4. Are there test cases linked to system requirements? (% linked)
5. Is there evidence that consistency was reviewed? (review record)

### Red Flags That Trigger Downgrade:
- Matrix was clearly assembled after the fact (all links created same day, no change history)
- Links are trivial (all-to-all mapping without meaningful derivation)
- Traceability to test cases missing (0% horizontal coverage)
- No consistency review evidence anywhere in the project

### Green Flags That Impress Assessors:
- Live tool (CodeBeamer, DOORS, Jira) with timestamped link history
- Change notifications showing downstream teams were alerted
- Consistency review checklist items signed off by named engineers
- Test coverage report generated from the traceability matrix

---

## Traceability Anti-Patterns

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| All SysReqs link to StRS-001 "System shall meet customer needs" | Trivial links — no meaningful derivation | Each SysReq links to the specific StRS requirement it derives from |
| SysReqs created without StRS, StRS written later as cover | Reverse-engineered traceability — assessors can tell from timestamps | Start with StRS, derive SysReqs forward |
| Traceability matrix in Excel, no change history | Cannot prove consistency maintenance | Use ALM tool with link history, or at minimum date-stamped versions |
| Test cases link to all requirements | Overly broad test scope claims | Each test case links only to requirements it specifically verifies |
| SysReqs with no test links | Cannot demonstrate verifiability | Every requirement must have at least one test (or Analysis/Inspection record) |
| Deleted StRS items still linked in SyRS | Stale traceability | Regular traceability health checks; use tool-level orphan detection |

---

## Traceability Matrix Format (Minimum Viable)

| SysReq ID | SysReq Title | StRS Link(s) | Design Link(s) | Test Link(s) | Status |
|-----------|-------------|-------------|----------------|-------------|--------|
| SysReq-001 | CAN Bit Rate | StRS-100 | HWD-CAN-001 | SysQt-001 | Approved |
| SysReq-002 | CAN Error Detection | StRS-100, StRS-101 | HWD-CAN-002 | SysQt-002, SysQt-003 | Approved |
| SysReq-003 | Temp Range | StRS-200 | HWD-THERM-001 | SysQt-010 | Draft |
| SysReq-004 | Boot Time | StRS-300 | SWArch-BOOT-001 | SysQt-020 | Approved |

**Minimum columns:** SysReq ID, StRS Link (upstream), Test Link (horizontal), Status
**Extended columns:** Design links, link types, review status, consistency check date
