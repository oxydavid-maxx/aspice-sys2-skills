# ASPICE SYS.2 Skills for Claude Code

Two Claude Code skills for ASPICE v4.0 SYS.2 System Requirements Analysis — creating, analyzing, and reviewing System Requirements Specifications (SyRS).

## Skills

### `deep-sys2` — SyRS Requirements Engineering Agent Team

11-agent pipeline (10 core + 1 integrity gate) that creates, analyzes, and repairs System Requirements Specifications against ASPICE v4.0, IEEE 29148:2018, and INCOSE GTWR V4 standards.

**Modes:** `create` | `analyze` | `full` | `verif-gen` | `quality-fix` | `gap` | `socratic`

**Key features:**
- Requirement Triple Gate: high-quality title + verifiable shall statement + quantitative verification criteria (BP5)
- All 8 ASPICE SYS.2 Base Practices (BP1–BP8) enforced
- Threshold Source Citation Rule: every numeric value must cite its source (customer req, standard, or engineering analysis)
- IEEE 29148 × INCOSE GTWR V4 quality enforcement with forbidden terms blacklist
- 3-directional traceability (StRS ↔ SyRS ↔ Test)

#### Agents

| # | Agent | Role |
|---|-------|------|
| 1 | `sys2_scoping_agent` | Define system boundary, V-model position, operating environment, stakeholder context. Produces a System Profile and Mermaid context diagram before any requirements work begins. |
| 2 | `requirements_elicitation_agent` | Classify requirements by type (Functional, NFR-Performance, NFR-Reliability, NFR-Safety, Interface, Design Constraint, Regulatory). Detect gaps in requirement type coverage. |
| 3 | `requirements_quality_agent` | IEEE 29148 × INCOSE GTWR V4 quality enforcer. Audit every requirement against all 9 quality characteristics and 42 INCOSE rules. Flag violations with exact rule citations and generate corrected alternatives. |
| 4 | `traceability_agent` | Build and audit StRS↔SyRS↔Design↔Test traceability matrix. Detect orphan requirements, uncovered StRS items, and BP6/BP7 consistency gaps. |
| 5 | `verification_criteria_agent` | BP5 specialist. Generate and audit IADT verification criteria for every requirement: method + qualitative criteria + quantitative threshold with mandatory source citation. Block requirements with circular or missing criteria. |
| 6 | `sys2_synthesis_agent` | Synthesize findings from all agents. Rank improvements by priority (P0/P1/P2). Identify systemic patterns across the document. |
| 7 | `document_compiler_agent` | Compile the final SyRS document with requirement cards, traceability matrix, Mermaid diagrams, revision history, and clickable footnote citations. Manages file output and revision archiving. |
| 8 | `aspice_assessor_agent` | BP1–BP8 assessment with iNTACS-style NPLF verdicts per BP. CL1/CL2/CL3 capability level determination. |
| 9 | `devils_advocate_agent` | Adversarial challenge: find circular criteria, ambiguous statements, self-referential traceability, gold-plating, and infeasible requirements. |
| 10 | `sys2_mentor_agent` | Socratic mode: guide requirement thinking through questions, never direct answers. Helps users clarify their requirements before writing. |
| 11 | `process_integrity_agent` | Final gate before delivery. Verify AI disclosure, reference accuracy (50% spot-check), traceability integrity (live vs. reconstructed), and requirement fabrication detection. |

---

### `deep-sys2-review` — Multi-Assessor Review Panel

8-agent review panel (5 independent assessors + synthesis + process integrity + document profiler) that critiques SyRS documents from independent perspectives: ASPICE compliance, requirement quality, traceability, and adversarial attack.

**Modes:** `full` | `confidence-score` | `quick` | `flaws-only` | `re-review` | `guided`

**Key features:**
- 5 independent assessors + synthesis + process integrity gate
- Confidence scoring (0–100) with NPLF ratings per BP
- 3-type citation system: document cross-references (clickable links to SyRS with verbatim quotes) + standard citations (footnotes with key findings) + ASPICE BP citations (BP statement quoted)
- Devil's Advocate with 20 adversarial attack patterns
- Revision roadmap with P0/P1/P2 prioritized fix list and Mermaid Gantt timeline

#### Agents

| # | Agent | Role |
|---|-------|------|
| 1 | `doc_profiler_agent` | Profile the SyRS before review: determine document maturity (Draft/Developing/Mature/Assessment-Ready), scope coverage, product domain, and configure assessor personas with domain-specific expertise. |
| 2 | `aspice_lead_assessor_agent` | iNTACS-certified lead assessor persona. Overall CL determination, weighted BP scoring (BP5 and BP6 at 20% each), process purpose fulfillment assessment. Minimum 3 genuine strengths identified. |
| 3 | `bp_compliance_reviewer_agent` | BP1–BP8 line-by-line compliance review. Assigns NPLF rating per BP with specific evidence found, evidence missing, and actionable fix for each gap. |
| 4 | `traceability_reviewer_agent` | Traceability completeness auditor. Checks upstream (SysReq→StRS), downstream (SysReq→Test), and horizontal (SysReq↔Test) coverage. Flags orphans, uncovered StRS items, trivial all-to-one mapping, and consistency gaps. |
| 5 | `requirement_quality_reviewer_agent` | IEEE 29148 × INCOSE V4 quality audit. Score every requirement 0–100 using 4-dimension rubric: Title (20pts) + Description (40pts) + Verification Criteria (30pts) + Traceability (10pts). Produce quality heatmap and top-5 violation patterns. |
| 6 | `devils_advocate_reviewer_agent` | Build the strongest possible case for why the document would fail a real ASPICE CL1 assessment. Screen all 20 attack patterns (circular criteria, orphan farm, TBD black hole, unitless numbers, vague performance, compound requirements, frozen traceability, missing safety/reliability, implementation leaking, etc.). Produce the "Strongest Counter-Argument" paragraph. |
| 7 | `synthesis_reviewer_agent` | Consolidate all reviewer reports into the Editorial Decision Package: confidence scorecard (Mermaid bar chart), reviewer consensus matrix, P0/P1/P2 prioritized improvement list, revision roadmap (Mermaid Gantt), and decision rationale. All findings use 3-type citation system. |
| 8 | `process_integrity_reviewer_agent` | Final integrity gate. Verify: AI disclosure present, reference accuracy (50% spot-check of cited standards), traceability integrity (live vs. reconstructed detection), requirement fabrication check (implausibly precise thresholds without sources), and scope integrity (no post-hoc justification). Verdict: CLEARED / CONDITIONAL / BLOCKED. |

## Installation

Copy both skill folders into your Claude Code skills directory:

```bash
# macOS/Linux
cp -r deep-sys2 ~/.claude/skills/deep-sys2
cp -r deep-sys2-review ~/.claude/skills/deep-sys2-review

# Windows
xcopy /E /I deep-sys2 %USERPROFILE%\.claude\skills\deep-sys2
xcopy /E /I deep-sys2-review %USERPROFILE%\.claude\skills\deep-sys2-review
```

## Usage

```
# Create a new SyRS from scratch
deep-sys2 create

# Analyze an existing SyRS
deep-sys2 analyze [paste or attach SyRS]

# Full multi-assessor review
deep-sys2-review full [paste or attach SyRS]

# Quick CL1 pass/fail check
deep-sys2-review quick [paste or attach SyRS]
```

## Standards Enforced

| Standard | Usage |
|----------|-------|
| ASPICE v4.0 | BP1–BP8 compliance, CL1/CL2/CL3 determination |
| IEEE 29148:2018 | 9 requirement quality characteristics |
| INCOSE GTWR V4 | 42 automotive-adapted writing rules |
| ISO 26262:2018 | Safety requirements (ASIL, SPFM, LFM, PMHF) |
| AEC-Q100 | Environmental qualification (Grade 2) |

## Integration Pipeline

```
deep-sys2 (create/analyze)
  → deep-sys2-review (full)
    → deep-sys2 (quality-fix/verif-gen)
      → deep-sys2-review (re-review)
```

## License

CC-BY-NC 4.0

## Author

Kuangyu

## Acknowledgments

Original ASPICE SYS.2 skill framework referenced from Cheng-I Wu's academic research skills architecture (CC-BY-NC 4.0).
