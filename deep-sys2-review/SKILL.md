---
name: deep-sys2-review
description: Use when reviewing, critiquing, or scoring an ASPICE SYS.2 System Requirements Specification — full multi-assessor review, confidence scoring, quick CL1 check, re-review after revisions, guided review dialogue, or fast flaw detection. Triggers on: review SyRS, critique requirements, confidence score, ASPICE assessment, rate requirements, point out flaws, BP compliance check, requirement quality audit, re-review, revision verification.
metadata:
  version: "1.0"
  last_updated: "2026-04-02"
  standards: ["ASPICE v4.0 PAM", "ISO/IEC/IEEE 29148:2018", "INCOSE GTWR V4", "iNTACS assessor guidance"]
---

# deep-sys2-review — ASPICE SYS.2 Multi-Assessor Review Panel

5-assessor review panel that critiques System Requirements Specifications from independent perspectives: ASPICE compliance, requirement quality, traceability, and adversarial attack. Produces confidence scores (0–100), ranked flaws, and a revision roadmap.

**Core principle:** Every review finding must cite specific evidence (requirement ID, field, violation). No vague feedback allowed.

---

## Quick Start

```
deep-sys2-review full [paste SyRS or attach file]
deep-sys2-review confidence-score [paste SyRS]
deep-sys2-review quick [paste SyRS] — CL1 pass/fail in 5 minutes
deep-sys2-review flaws-only [paste SyRS] — P0/P1 flaws fast
deep-sys2-review re-review [paste revised SyRS + prior revision roadmap]
deep-sys2-review guided [paste SyRS] — Socratic guided improvement
```

---

## Mode Selection

| Your Situation | Mode |
|----------------|------|
| Complete review before assessment | `full` |
| Need a score to report to management | `confidence-score` |
| Quick pre-review sanity check | `quick` |
| Just want the worst problems fast | `flaws-only` |
| Verify fixes after revision | `re-review` |
| Want to learn by doing | `guided` |

---

## Modes

| Mode | Agents | Output | Time |
|------|--------|--------|------|
| `full` | All 7 | 5 reports + Editorial Decision + Revision Roadmap + Mermaid scorecard | Full |
| `confidence-score` | profiler + bp_compliance + quality + synthesis | BP scorecard (0–100) + req quality heatmap | Medium |
| `quick` | profiler + lead_assessor | CL1 pass/fail table + blocking issues | ~15 min |
| `re-review` | profiler + lead_assessor + synthesis | Revision checklist + residual issues + new verdict | Medium |
| `guided` | All + Socratic dialogue | Guided issue discovery + self-formulated plan | Interactive |
| `flaws-only` | profiler + devils_advocate + synthesis | P0/P1/P2 flaw list with severity | Fast |

---

## Confidence Score System

### Per-BP Score (0–100)
| Range | NPLF | Meaning |
|-------|------|---------|
| 86–100 | F — Fully | Strong evidence; would pass CL1 for this BP |
| 51–85 | L — Largely | Good evidence; minor gaps |
| 16–50 | P — Partially | Significant gaps; CL1 at risk |
| 0–15 | N — Not | Missing or inadequate evidence; CL1 blocked |

### Per-Requirement Quality Score (0–100)
- **Title quality (20 pts):** Noun-phrase format, ≤80 chars, unique, self-explanatory
- **Description quality (40 pts):** IEEE 29148 × 9 characteristics, INCOSE rules, no forbidden terms, units present
- **Verification criteria (30 pts):** IADT method, qualitative criteria, quantitative threshold
- **Traceability (10 pts):** Upstream (StRS) + downstream (Test) links present

### Document Overall Score
- BP compliance: 50%
- Requirement quality (avg): 30%
- Traceability coverage: 20%

**Score interpretation:**
- ≥ 87: Ready for CL1 assessment
- 70–86: Needs targeted fixes (1–2 weeks work)
- 50–69: Significant gaps (1–2 months work)
- < 50: Fundamental restructuring needed

---

## Agent Team (7 Agents)

| # | Agent | Adapted From | Role |
|---|-------|-------------|------|
| 1 | `doc_profiler_agent` | field_analyst_agent | Profile document maturity, coverage, domain; configure assessor personas |
| 2 | `aspice_lead_assessor_agent` | eic_agent | Lead assessor: overall CL determination, process purpose fulfillment |
| 3 | `bp_compliance_reviewer_agent` | methodology_reviewer_agent | BP1-BP8 line review: NPLF per BP with evidence citations |
| 4 | `traceability_reviewer_agent` | domain_reviewer_agent | Upstream/downstream/horizontal traceability completeness and consistency |
| 5 | `requirement_quality_reviewer_agent` | perspective_reviewer_agent | IEEE 29148 × INCOSE V4 audit: per-requirement quality score 0–100 |
| 6 | `devils_advocate_reviewer_agent` | devils_advocate_reviewer_agent | Strongest attack arguments: circular criteria, gold-plating, stale traceability |
| 7 | `synthesis_reviewer_agent` | editorial_synthesizer_agent | Confidence scorecard + priority list + revision roadmap + Mermaid summary |

---

## MANDATORY: Sub-Agent Prompt Compliance Rule

**CRITICAL — Read before launching any sub-agent.**

When orchestrating this pipeline via the Agent tool, you MUST follow these steps for every sub-agent launch:

1. **Read the agent definition file first** — Use the Read tool to load the full contents of the relevant agent `.md` file from the `agents/` directory (e.g., `agents/synthesis_reviewer_agent.md`) **before writing the sub-agent prompt**.

2. **Include full file contents verbatim** — Place the complete agent definition file contents at the **top** of the sub-agent prompt as the agent's role definition. Do NOT summarize, paraphrase, condense, or simplify.

3. **Sub-agent prompt structure — required format:**
   ```
   [Full verbatim contents of agents/<agent_name>.md]

   ---
   ## Task Context
   [Task-specific data, inputs, and instructions for this invocation]
   ```

4. **No custom role descriptions** — Writing a custom role description (e.g., "You are a BP compliance reviewer who rates each BP as NPLF") silently drops all mandatory requirements embedded in the definition file. This is forbidden.

5. **Diagram tools in output** — Every section of produced review reports must contain diagrams. Use the correct tool for each layer:
   - **Mermaid** (` ```mermaid `) — architecture, flowcharts, state machines, sequence diagrams, class diagrams, mind maps, timelines, process flows
   - **WaveDrom** (` ```wavedrom `) — **logic/digital-level** timing only: ideal clock/data/enable signals, interrupt events, register write sequences (renders ideal square waves — no slopes)
   - **Mermaid `xychart-beta`** — **physical/analog** waveforms: signal voltage rise/fall with slew rate, current ramps, response curves, overshoot approximations
   - **Never use ASCII art for waveforms** — WaveDrom renders natively in VS Code markdown preview (`bmpenuelas.markdown-preview-wavedrom`)

**Why this rule exists:** Agent definition files contain mandatory confidence scoring formulas (0–100 per BP), NPLF decision criteria, evidence citation requirements, and Mermaid scorecard templates. Summarizing these files causes systematic spec non-compliance that is invisible until formal CL1 assessment.

**Failure mode to avoid:** Do NOT write sub-agent prompts from memory or from a high-level summary of the agent's purpose. Always **Read → Copy verbatim → Append context**.

---

## Orchestration — Full Mode

```
User: "deep-sys2-review full [SyRS]"
     |
=== Phase 0: PROFILING ===
     |-> [doc_profiler_agent]
     |   - Document maturity score
     |   - Scope coverage check  
     |   - Configure 5 assessor personas
     |   → Present configuration to user for confirmation
     |
=== Phase 1: PARALLEL REVIEW (5 assessors) ===
     |-> [aspice_lead_assessor_agent]   → Overall CL verdict + process purpose assessment
     |-> [bp_compliance_reviewer_agent] → BP1-BP8 NPLF ratings with evidence
     |-> [traceability_reviewer_agent]  → Traceability coverage + consistency gaps
     |-> [requirement_quality_reviewer_agent] → Per-req 0-100 quality scores
     +-> [devils_advocate_reviewer_agent] → Strongest failure arguments
     |
=== Phase 2: SYNTHESIS ===
     +-> [synthesis_reviewer_agent]
         - Consolidate 5 reports
         - Compute confidence score
         - Produce revision roadmap (P0/P1/P2)
         - Generate Mermaid scorecard
     |
=== Phase 2.5: REVISION COACHING (if score < 87) ===
     +-> [aspice_lead_assessor_agent] Socratic coaching:
         "After seeing the review, what surprised you most?"
         → User-formulated revision strategy
         → Reprioritized roadmap
     (User can skip: "just show me the list")
```

---

## Checkpoint Rules

1. **After Phase 0**: Present assessor configuration; user can adjust
2. **Phase 1**: Reviewers work independently; no cross-referencing
3. **Phase 2**: Synthesis based only on Phase 1 evidence; no fabrication
4. **Devil's Advocate CRITICAL**: Cannot result in "Approved" verdict
5. **CL1 BLOCKED** if any single BP score < 16 (N rating) — no exceptions

---

## Review Quality Standards

Every reviewer finding MUST:
- Cite specific SysReq ID, field, or section
- State which standard is violated (ASPICE BPx, IEEE 29148 §x.x, INCOSE Rx)
- Provide a specific actionable fix (not "improve this")
- Include evidence of the gap (quote from the document)

---

## Re-Review Mode

```
Input: Revised SyRS + Prior Revision Roadmap (P0/P1/P2 items)

For each P0/P1 item from prior roadmap:
  → Check revised document for corresponding fix
  → Rate: FULLY_ADDRESSED / PARTIALLY_ADDRESSED / NOT_ADDRESSED / MADE_WORSE
  → P0 items: All must be FULLY_ADDRESSED for approval
  → P1 items: ≥80% must have response

New issue detection:
  → Scan revision for newly introduced problems
  → Flag new issues as NEW-X

Output: Revision checklist + new decision + residual issues
```

---

## Reference Files

| File | Purpose |
|------|---------|
| `references/aspice_sys2_bp_reference.md` | BP1-BP8 definitions (shared with deep-sys2) |
| `references/bp_evidence_checklist.md` | Per-BP: what evidence satisfies each BP |
| `references/nplf_rating_guide.md` | NPLF decision criteria with automotive examples |
| `references/quality_rubrics.md` | 0–100 scoring rubrics per review dimension |
| `references/requirement_antipatterns.md` | Known failure patterns from real SyRS assessments |
| `references/devils_advocate_attack_patterns.md` | 20 adversarial attack angles |

## Templates

| File | Purpose |
|------|---------|
| `templates/review_report_template.md` | Per-assessor structured report |
| `templates/confidence_scorecard_template.md` | BP + overall score with Mermaid |
| `templates/revision_roadmap_template.md` | P0/P1/P2 prioritized list |
| `templates/re_review_checklist_template.md` | Verify prior findings addressed |
| `templates/requirement_quality_heatmap_template.md` | Per-req quality scores |

---

## Integration

```
deep-sys2 (create/analyze)
  → deep-sys2-review full           ← this skill
    → deep-sys2 quality-fix/verif-gen   ← repair
      → deep-sys2-review re-review      ← verify fixes
```

---

## Output Language
Follows document language. Technical terms (ASPICE, IEEE 29148, NPLF) remain in English.

---

## Additional Agent (Phase 2.7: Integrity Gate)

After synthesis, before delivery, the `process_integrity_reviewer_agent` runs as a final gate:
- AI disclosure in requirements
- Reference integrity check (50% spot-check of cited standards)
- Traceability reconstruction detection
- Verdict: CLEARED / CONDITIONAL / BLOCKED

See `agents/process_integrity_reviewer_agent.md`
