# Per-Reviewer Report Template

```markdown
## [Reviewer Name/Role] — Review Report
**Reviewer Persona:** [iNTACS assessor / Requirements engineer / Traceability specialist / Quality reviewer / Devil's Advocate]
**Date:** YYYY-MM-DD

### Summary
[2-3 sentences: overall impression from this reviewer's perspective]

### Strengths
- [Specific positive finding with evidence]

### Findings

#### [Finding-ID]: [Title]

- **Location**: [SysReq-XXX](syrs_file.md#heading-anchor) — [field name]
- **Severity**: CRITICAL / MAJOR / MINOR
- **Evidence** (verbatim quote from SyRS):
  > "[exact text from the requirement/section being flagged]"
- **Standard violated**: [Standard name §clause]<sup>[[N]](#fn-N)</sup>
  > "[key text from the standard that defines the violation]"
- **BP impact**: BP[N] — [BP name]<sup>[[N]](#fn-N)</sup>
  > "[ASPICE BP statement text]"
- **Fix**: [specific corrective action with example rewrite if applicable]

### Score from this perspective: [X]/100
```

## Citation Rules (Mandatory)

Every finding that recommends a revision MUST include ALL THREE citation types:

### Type 1: Document Cross-Reference
Link directly to the reviewed SyRS location using relative path + anchor:
```markdown
[SysReq-C2C-010](can-gateway_syrs.md#sysreq-c2c-010-can-to-can-routing--bus-off-detection-and-recovery)
```
Plus a verbatim `>` blockquote of the exact text being flagged.

### Type 2: Standard Citation
Clickable footnote to the violated standard:
```markdown
IEEE 29148:2018 §5.2.5 (Singular)<sup>[[1]](#fn-1)</sup>
```
With full citation + key finding quote in the References section.

### Type 3: ASPICE BP Citation
Which BP is impacted with the BP's exact statement:
```markdown
**BP3 — Analyze System Requirements**<sup>[[4]](#fn-4)</sup>:
> "Analyze the specified system requirements... to verify correctness, technical feasibility, and verifiability"
```
