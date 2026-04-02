# Traceability Matrix Template

```markdown
## Requirements Traceability Matrix (RTM)
**Project:** [Name]  **Version:** X.X  **Date:** YYYY-MM-DD

### Upstream Traceability: StRS → SyRS

| StRS ID | StRS Title | SysReq ID(s) | Coverage |
|---------|-----------|--------------|---------|
| StRS-100 | [Title] | SysReq-001, SysReq-002 | ✅ |
| StRS-200 | [Title] | SysReq-010 | ✅ |
| StRS-300 | [Title] | ❌ NONE | ❌ Gap |

### Full Traceability: StRS ↔ SyRS ↔ Design ↔ Test

| SysReq ID | Title | Type | StRS Link | Design Link | Test Link | BP5? | Status |
|-----------|-------|------|-----------|-------------|-----------|------|--------|
| SysReq-001 | CAN Bit Rate | IR | StRS-100 | HWD-001 | SysQt-001 | ✅ | Approved |
| SysReq-002 | Boot Time | NFR-P | StRS-300 | SWArch-001 | SysQt-020 | ✅ | Draft |
| SysReq-003 | Temperature | DC | StRS-200 | HWD-THERM-001 | SysQt-010 | ❌ Missing | Draft |
| SysReq-004 | [Title] | FR | ❌ ORPHAN | — | — | ❌ Missing | Draft |

### Coverage Metrics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| SysReq upstream coverage | X% | 100% | ✅/❌ |
| StRS downstream coverage | X% | ≥95% | ✅/❌ |
| Test coverage | X% | ≥80% | ✅/❌ |
| BP5 coverage | X% | ≥80% | ✅/❌ |

### Traceability Flow Diagram
```mermaid
graph TD
  [auto-generated from traceability_agent]
```
```
