# Requirement Quality Heatmap Template

```markdown
## Requirement Quality Heatmap
**Document:** [SyRS name/version]

```mermaid
xychart-beta
  title "Requirement Quality Scores (Top 10 Worst)"
  x-axis ["SysReq\n001","SysReq\n002","SysReq\n003","SysReq\n004","SysReq\n005"]
  y-axis "Score /100" 0 --> 100
  bar [X,X,X,X,X]
  line [70,70,70,70,70]
```

### Score Table
| SysReq | Title | Title/20 | Desc/40 | Verif/30 | Trace/10 | Total | Status |
|--------|-------|---------|---------|---------|---------|-------|--------|
| SysReq-001 | [title] | X | X | X | X | X | ✅/⚠️/❌ |

### Most Common Violations
1. [Violation type]: affects N/X requirements (Y%) — [fix pattern]
```
