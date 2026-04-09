---
name: skill-deep-sys2
description: Use when working on ASPICE SYS.2 System Requirements Analysis — analyzing existing SyRS documents, creating requirements from scratch, generating verification criteria, fixing requirement quality, running gap analysis against ASPICE v4.0 BPs, or getting guided through SYS.2 thinking. Triggers on: SYS.2, system requirements, SyRS, requirement quality, verification criteria, traceability matrix, ASPICE compliance, shall statement, acceptance criteria, requirement gap, BP5, bidirectional traceability.
metadata:
  version: "1.0"
  last_updated: "2026-04-02"
  standards: ["ASPICE v4.0 PAM", "ISO/IEC/IEEE 29148:2018", "INCOSE GTWR V4", "ISO 26262:2018"]
---

# deep-sys2 — ASPICE SYS.2 Requirements Engineering Agent Team

Purpose-built for ASPICE SYS.2 System Requirements Analysis. A 10-agent pipeline that creates, analyzes, and repairs System Requirements Specifications (SyRS) against ASPICE v4.0, IEEE 29148:2018, and INCOSE GTWR V4 standards.

**Core principle:** Every requirement must pass the triple gate — high-quality title + verifiable shall statement + quantitative verification criteria — before it is considered complete.

---

## Quick Start

```
deep-sys2 analyze [paste SyRS or attach file]
deep-sys2 create — guided SyRS creation from scratch
deep-sys2 verif-gen [paste requirements without verification criteria]
deep-sys2 quality-fix [paste requirements with weak titles/descriptions]
deep-sys2 gap [paste SyRS] — BP1-BP8 compliance verdict
deep-sys2 socratic — guide me through SYS.2 for my product
```

---

## Mode Selection Guide

| Your Situation | Mode |
|----------------|------|
| Have existing SyRS, want full analysis | `full` or `analyze` |
| Starting from scratch, need guidance | `create` |
| Requirements exist but verification criteria missing | `verif-gen` |
| Titles/descriptions are vague or non-compliant | `quality-fix` |
| Need quick BP compliance snapshot | `gap` |
| Unsure what SYS.2 requires for your product | `socratic` |

---

## Modes

| Mode | Agents Active | Output |
|------|---------------|--------|
| `full` | All 10 | Complete analysis: quality audit + traceability map + BP gap + confidence scorecard + improvement plan |
| `analyze` | All 10 | Same as full, triggered on existing document input |
| `create` | 6 agents (interactive) | Guided SyRS creation: scope → classify → write → verify → trace |
| `gap` | scoping + assessor + devils_advocate | BP1-BP8 NPLF verdict table, ~20 min |
| `verif-gen` | verif_criteria + req_quality + compiler | Per-requirement: Method (IADT) + Criteria + Threshold |
| `quality-fix` | req_quality + compiler | INCOSE-corrected titles + descriptions with rule citations |
| `socratic` | sys2_mentor + scoping + devils_advocate | Guided Socratic dialogue → SYS.2 scope brief |

---

## Standards Enforced (Non-Negotiable)

### ASPICE v4.0 SYS.2 — 8 Base Practices
See `references/aspice_sys2_bp_reference.md` for full BP definitions.

| BP | Name | What It Requires |
|----|------|-----------------|
| BP1 | Specify System Requirements | Functional + non-functional requirements from StRS |
| BP2 | Structure System Requirements | Grouping, prioritization, categorization, release assignment |
| BP3 | Analyze System Requirements | Correctness, feasibility, **verifiability** check |
| BP4 | Analyze Operating Environment | Interface identification, impact on context |
| BP5 | **Develop Verification Criteria** | Qualitative AND quantitative measures per requirement |
| BP6 | Establish Bidirectional Traceability | StRS ↔ SyRS links, coverage, impact analysis |
| BP7 | Verify Consistency | Review records proving StRS ↔ SyRS consistency |
| BP8 | Communicate Updates | All parties informed of agreed requirements |

### Requirement Quality Triple Gate

Every requirement MUST pass all three gates before acceptance:

**Gate 1 — Title:** `[Feature/System] + [Action/Property] + [Constraint]`
- ✅ `CAN Bus Interface — Maximum Bit Rate Specification`
- ❌ `CAN requirement` / `Req-001` / `Communication`

**Gate 2 — Description (Shall Statement):** IEEE 29148 × INCOSE V4
- Must use active voice: "The system shall..."
- Must include units for all numeric values
- Zero forbidden vague terms (see `references/requirement_antipatterns.md`)
- One capability per statement (singular)

**Gate 3 — Verification Criteria (BP5):** IADT format
```
Method: T / A / I / D
Criteria: [What is measured] [How] [Reference standard]
Threshold: [Quantitative pass/fail boundary with units]
```

### Traceability — 3-Directional Mandatory
- **Vertical up:** SysReq → StRS (no orphan requirements → CL1 failure)
- **Vertical down:** SysReq → Design element OR Test case (no dangling requirements)
- **Horizontal:** SysReq ↔ SysQt/Test case (bidirectional, coverage provable)

---

## Agent Team (10 Agents)

| # | Agent | Role |
|---|-------|------|
| 1 | `sys2_scoping_agent` | Define system boundary, V-model position, operating environment, stakeholder context |
| 2 | `requirements_elicitation_agent` | Classify requirements (Functional/NFR/Interface/Safety/Regulatory/Design Constraint); detect gaps |
| 3 | `requirements_quality_agent` | IEEE 29148 × INCOSE V4 enforcement; flag violations with rule citation; auto-suggest corrections |
| 4 | `traceability_agent` | Build/audit StRS↔SyRS↔Design↔Test matrix; detect orphans; flag BP6/BP7 gaps |
| 5 | `verification_criteria_agent` | Generate/audit BP5 criteria: IADT method + qualitative description + quantitative threshold |
| 6 | `sys2_synthesis_agent` | Synthesize findings; rank improvements; identify systemic patterns |
| 7 | `document_compiler_agent` | Compile SyRS or analysis report with requirement tables, traceability matrix, scorecard |
| 8 | `aspice_assessor_agent` | BP1-BP8 assessment; CL1/CL2/CL3 determination; iNTACS-style NPLF verdict per BP |
| 9 | `devils_advocate_agent` | Adversarial challenge: find circular criteria, ambiguous statements, self-referential traceability |
| 10 | `sys2_mentor_agent` | Socratic mode: guide requirement thinking through questions, never direct answers |

See `agents/` directory for detailed agent definitions. **11 agents total** (10 core + process_integrity_agent as final gate).

---

## MANDATORY: Sub-Agent Prompt Compliance Rule

**CRITICAL — Read before launching any sub-agent.**

When orchestrating this pipeline via the Agent tool, you MUST follow these steps for every sub-agent launch:

1. **Read the agent definition file first** — Use the Read tool to load the full contents of the relevant agent `.md` file from the `agents/` directory (e.g., `agents/aspice_assessor_agent.md`) **before writing the sub-agent prompt**.

2. **Include full file contents verbatim** — Place the complete agent definition file contents at the **top** of the sub-agent prompt as the agent's role definition. Do NOT summarize, paraphrase, condense, or simplify.

3. **Sub-agent prompt structure — required format:**
   ```
   [Full verbatim contents of agents/<agent_name>.md]

   ---
   ## Task Context
   [Task-specific data, inputs, and instructions for this invocation]
   ```

4. **No custom role descriptions** — Writing a custom role description (e.g., "You are an ASPICE assessor who rates BP compliance") silently drops all mandatory requirements embedded in the definition file. This is forbidden.

5. **Diagram tools in output** — Every section of produced SyRS documents must contain diagrams. Use the correct tool for each layer:
   - **Mermaid** (` ```mermaid `) — architecture, flowcharts, state machines, sequence diagrams, class diagrams, mind maps, timelines, process flows
   - **WaveDrom** (` ```wavedrom `) — **logic/digital-level** timing only: ideal clock/data/enable signals, interrupt events, register write sequences (renders ideal square waves — no slopes)
   - **Mermaid `xychart-beta`** — **physical/analog** waveforms: signal voltage rise/fall with slew rate, current ramps, response curves, overshoot approximations
   - **Never use ASCII art for waveforms** — WaveDrom renders natively in VS Code markdown preview (`bmpenuelas.markdown-preview-wavedrom`)

**Why this rule exists:** Agent definition files contain mandatory output formats, NPLF rating criteria, BP evidence checklists, and non-negotiable ASPICE v4.0 compliance rules. Summarizing these files causes systematic spec non-compliance that is invisible until formal assessment.

**Failure mode to avoid:** Do NOT write sub-agent prompts from memory or from a high-level summary of the agent's purpose. Always **Read → Copy verbatim → Append context**.

---

## Orchestration Workflow — Full / Analyze Mode

```
User: "deep-sys2 analyze [SyRS]"
     |
=== Phase 1: SCOPING ===
     |-> [sys2_scoping_agent] -> System Profile
     |   - Product identity, V-model position, system boundary
     |   - Operating environment, stakeholder map
     |   - Which BPs are in-scope vs. out-of-scope
     |
=== Phase 2: CLASSIFICATION & QUALITY AUDIT ===
     |-> [requirements_elicitation_agent] -> Requirement Type Map
     |   - Classify all requirements by type
     |   - Flag missing requirement types
     |
     |-> [requirements_quality_agent] -> Quality Report
     |   - IEEE 29148 × INCOSE V4 gate per requirement
     |   - Per-requirement violations list with rule citations
     |   - Corrected wording suggestions
     |
     +-> CHECKPOINT 1 [devils_advocate_agent]
         - Are requirements truly singular?
         - Any hidden "and" / "or" compound statements?
         - Verdict: PASS / FLAG (list specific requirements)
     |
=== Phase 3: TRACEABILITY & VERIFICATION ===
     |-> [traceability_agent] -> Traceability Matrix + Gap Map
     |   - Upstream coverage (SysReq → StRS)
     |   - Downstream coverage (SysReq → Test/Design)
     |   - Orphan requirement list
     |   - Consistency violations (BP7)
     |
     |-> [verification_criteria_agent] -> BP5 Audit + Generated Criteria
     |   - Per-requirement: Method present? Criteria present? Threshold quantitative?
     |   - Generate missing criteria for unverified requirements
     |   - Block list: requirements that cannot be verified as written
     |
=== Phase 4: ASSESSMENT ===
     |-> [aspice_assessor_agent] -> BP1-BP8 NPLF Verdicts
     |   - Per-BP: N/P/L/F rating with evidence citation
     |   - CL1/CL2/CL3 determination
     |   - Blocking issues for CL progression
     |
     +-> CHECKPOINT 2 [devils_advocate_agent]
         - "Would this survive a real VDA/OEM assessment?"
         - Strongest attack arguments
         - Verdict: PASS / CRITICAL (blocks delivery)
     |
=== Phase 5: SYNTHESIS & REPORT ===
     |-> [sys2_synthesis_agent] -> Improvement Priority Ranking
     |   - P0 Critical (CL1 failures) / P1 Required / P2 Suggested
     |   - Systemic pattern identification
     |
     +-> [document_compiler_agent] -> Final Report
         - BP Scorecard (NPLF per BP)
         - Requirement Quality Heatmap
         - Traceability Coverage Summary
         - Ranked Improvement Plan
         - Ready-to-use SyRS template with corrections applied
```

---

## Create Mode Workflow

```
User: "deep-sys2 create"
     |
[sys2_scoping_agent] -> Interactive scope definition
     - "What product are you specifying requirements for?"
     - "What is the system boundary?"
     - "What stakeholder requirements documents do you have?"
     |
[requirements_elicitation_agent] -> Requirement classification guidance
     - Walk through each requirement type
     - Identify gaps: "You have no Safety requirements — is that intentional?"
     |
For each requirement (interactive loop):
     [requirements_quality_agent] -> Gate 1 (Title) + Gate 2 (Shall Statement)
          - Reject and suggest correction if gates fail
     [verification_criteria_agent] -> Gate 3 (BP5 Criteria)
          - Generate criteria based on requirement type + domain context
     [traceability_agent] -> Trace upstream link
          - "Which StRS requirement does this derive from?"
     |
[document_compiler_agent] -> Compile completed SyRS
```

---

## Checkpoint Rules

1. **devils_advocate_agent** has 2 mandatory checkpoints; CRITICAL issues block delivery
2. Requirements with blank/TBD verification criteria are flagged as BLOCKED (BP5 violation)
3. Orphan requirements (no upstream trace) trigger CL1 FAILURE
4. Revision cycles capped at 2; remaining issues go to "Acknowledged Gaps" section

---

## Reference Files

| File | Purpose |
|------|---------|
| `references/aspice_sys2_bp_reference.md` | All 8 BPs with exact ASPICE v4.0 language + process outcomes |
| `references/ieee_29148_quality_characteristics.md` | 9 quality characteristics with automotive examples |
| `references/incose_rules_automotive.md` | 42 INCOSE rules adapted for automotive embedded context |
| `references/verification_methods_iadt.md` | IADT decision tree + BP5 format templates with examples |
| `references/traceability_standards.md` | 3-directional traceability rules + link types + assessor checklist |
| `references/requirement_antipatterns.md` | Forbidden terms blacklist + common bad patterns from real SyRS docs |
| `references/aspice_cl_determination.md` | NPLF scale + CL1/2/3 determination rules |
| `references/failure_paths.md` | Recovery strategies for 10 common SYS.2 failure scenarios |

## Templates

| File | Purpose |
|------|---------|
| `templates/syrs_full_template.md` | Complete SyRS document structure |
| `templates/requirement_card_template.md` | Single requirement: all mandatory fields |
| `templates/gap_analysis_report_template.md` | BP1-BP8 NPLF verdict table |
| `templates/traceability_matrix_template.md` | StRS↔SyRS↔Design↔Test matrix |
| `templates/verification_criteria_template.md` | BP5 per-requirement criteria card |
| `templates/bp_evidence_matrix_template.md` | BP1-BP8 × Requirement Section cross-tab matrix |

---

## Integration

```
deep-sys2 (analyze/create)
  → deep-sys2-review (full/confidence-score)    ← critique output
    → deep-sys2 (quality-fix/verif-gen)         ← repair identified gaps
      → deep-sys2-review (re-review)            ← verify fixes
```

**Handoff to deep-sys2-review:** After `full` or `create` mode completes, the compiled SyRS + gap analysis can be directly fed into `deep-sys2-review` for multi-assessor critique.

---

## Output Language

Follows user's language. ASPICE/IEEE/INCOSE technical terms remain in English. Chinese descriptions supported.
