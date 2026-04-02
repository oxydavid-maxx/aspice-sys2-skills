# BP Evidence Matrix Template

## Purpose
Cross-tabulate requirement sections/groups against BP1-BP8 to map where evidence is present, partial, or missing. Used by `sys2_synthesis_agent` before writing the improvement plan. Equivalent of `literature_matrix_template.md` in deep-research.

## Basic Matrix

```markdown
## BP Evidence Matrix: [Product Name] SyRS
**Date compiled:** YYYY-MM-DD
**Total requirements:** N
**Sections reviewed:** N

| Section / Evidence Item | BP1\nSpecify | BP2\nStructure | BP3\nAnalyze | BP4\nEnv | BP5⭐\nVerif | BP6\nTrace | BP7\nConsist | BP8\nCommun |
|------------------------|------------|--------------|------------|---------|------------|---------|------------|---------|
| Sec 6.1 Env Requirements | ✓ | ✓ | — | ✓ | ✗ | P | — | — |
| Sec 6.2 Interface Req | ✓ | ✓ | P | ✓ | ✗ | P | — | — |
| Sec 6.3 Functional Req | ✓ | P | — | — | ✗ | ✗ | — | — |
| Sec 6.4 Safety Req | P | ✓ | P | — | ✗ | P | — | — |
| Traceability Matrix | — | — | — | — | — | ✓ | — | — |
| Review Records | — | — | ✓ | — | — | — | ✓ | — |
| Distribution Evidence | — | — | — | — | — | — | — | ✓ |
| **BP Coverage** | **✓** | **P** | **P** | **P** | **✗ CRITICAL** | **P** | **P** | **P** |

Legend:
- ✓ Adequate evidence present
- P Partial evidence; gaps exist
- ✗ Missing — no evidence found
- — Not applicable to this section
```

## Extended Matrix (with detail)

```markdown
## Extended BP Evidence Matrix

| Section | Req Count | BP1 | BP5 Evidence | BP6 Evidence | BP3 Evidence |
|---------|-----------|-----|-------------|-------------|-------------|
| Sec 6.1 Env | 5 | All types present | 0/5 have criteria | 4/5 traced to StRS | Review minutes exist |
| Sec 6.2 Interface | 8 | Interface reqs present | 0/8 have criteria | 6/8 traced | No feasibility analysis |
| Sec 6.3 Functional | 22 | Functional only | 2/22 have partial criteria | 10/22 traced | — |

```

## BP Convergence Summary

```markdown
## BP Evidence Convergence

| BP | Evidence Present | Evidence Missing | Net | Strength | CL1 Status |
|----|-----------------|-----------------|-----|----------|------------|
| BP1 Specify | Functional + Interface + Design Const. | Safety NFRs missing | +4 types | Strong | ✅ |
| BP2 Structure | Priority on 20/35 reqs | 15 reqs no priority | Partial | Moderate | ⚠️ |
| BP3 Analyze | Review minutes for Sec 6.1 | No feasibility analysis | Partial | Weak | ⚠️ |
| BP4 Env | Context diagram + interface list | No ICD document | Partial | Moderate | ⚠️ |
| BP5 Verif | 2/35 have partial criteria | 33/35 missing criteria | CRITICAL | None | ❌ |
| BP6 Trace | Matrix exists | 40% orphan requirements | Weak | Partial | ❌ |
| BP7 Consist | Meeting minutes | No StRS↔SyRS specific check | Partial | Weak | ⚠️ |
| BP8 Commun | SyRS sent to design team | No customer acknowledgment | Partial | Moderate | ⚠️ |

### Interpretation
- **Strong** (✓ in most sections, evidence consistent): Confident finding
- **Moderate** (P in most sections): Likely meets minimum but needs attention
- **Weak** (✗ or P in most sections): Significant gap; at risk
- **Critical Gap** (✗ across board): CL1 blocker
```

## Gap Identification

```markdown
## Requirement Gaps

| Gap | Type | Impact | Priority |
|-----|------|--------|---------|
| No verification criteria for any requirement | Process | BP5 N rating → CL1 blocked | P0 |
| 40% orphan requirements | Traceability | BP6 P rating | P0 |
| No safety requirements despite ASIL product | Coverage | BP1 P rating | P1 |
| No ICD document | Interface | BP4 P rating | P1 |
| No review records referencing StRS | Consistency | BP7 P rating | P1 |
```

## Usage Notes
- Start with Basic Matrix; upgrade to Extended as synthesis deepens
- Convergence Summary feeds directly into `sys2_synthesis_agent` priority rankings
- Gap Identification table becomes the P0/P1 items in the improvement plan
- Update matrix after each revision cycle — it is a living document
