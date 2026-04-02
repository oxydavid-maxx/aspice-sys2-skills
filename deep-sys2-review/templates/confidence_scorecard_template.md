# Confidence Scorecard Template

```markdown
## SYS.2 Confidence Scorecard
**Document:** [SyRS name/version]  **Score Date:** YYYY-MM-DD

### Overall Score: [X]/100 — [APPROVE/MINOR/MAJOR/REJECT]

```mermaid
xychart-beta
  title "SYS.2 Confidence Scores vs. Approval Threshold"
  x-axis ["BP1","BP2","BP3","BP4","BP5⭐","BP6","BP7","BP8","Quality","Trace"]
  y-axis "Score" 0 --> 100
  bar [X,X,X,X,X,X,X,X,X,X]
  line [87,87,87,87,87,87,87,87,87,87]
```

### BP Score Detail
| BP | Name | Score | NPLF | Key Evidence Gap |
|----|------|-------|------|-----------------|
| BP5 | Verification Criteria | X% | N | No criteria for any requirement |

### Per-Requirement Quality Sample (Top 5 Lowest)
| SysReq | Title Score | Desc Score | Verif Score | Trace Score | Total |
|--------|------------|-----------|------------|-------------|-------|
| SysReq-XXX | X/20 | X/40 | X/30 | X/10 | X/100 |

### Weighted Overall Computation
- BP compliance avg: X% × 50% = X
- Req quality avg: X% × 30% = X  
- Traceability coverage: X% × 20% = X
- **Total: X/100**
```
