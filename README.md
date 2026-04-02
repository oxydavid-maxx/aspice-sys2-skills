# ASPICE SYS.2 Skills for Claude Code

Two Claude Code skills for ASPICE v4.0 SYS.2 System Requirements Analysis — creating, analyzing, and reviewing System Requirements Specifications (SyRS).

## Skills

### `deep-sys2` — SyRS Requirements Engineering Agent Team
10-agent pipeline that creates, analyzes, and repairs System Requirements Specifications against ASPICE v4.0, IEEE 29148:2018, and INCOSE GTWR V4 standards.

**Modes:** `create` | `analyze` | `full` | `verif-gen` | `quality-fix` | `gap` | `socratic`

**Key features:**
- Requirement Triple Gate: high-quality title + verifiable shall statement + quantitative verification criteria (BP5)
- All 8 ASPICE SYS.2 Base Practices (BP1–BP8) enforced
- Threshold Source Citation Rule: every numeric value must cite its source (customer req, standard, or engineering analysis)
- IEEE 29148 × INCOSE GTWR V4 quality enforcement with forbidden terms blacklist
- 3-directional traceability (StRS ↔ SyRS ↔ Test)

### `deep-sys2-review` — Multi-Assessor Review Panel
7-assessor review panel that critiques SyRS documents from independent perspectives: ASPICE compliance, requirement quality, traceability, and adversarial attack.

**Modes:** `full` | `confidence-score` | `quick` | `flaws-only` | `re-review` | `guided`

**Key features:**
- 5 independent assessors + synthesis + process integrity gate
- Confidence scoring (0–100) with NPLF ratings per BP
- 3-type citation system: document cross-references + standard citations + ASPICE BP citations
- Devil's Advocate with 20 adversarial attack patterns
- Revision roadmap with P0/P1/P2 prioritized fix list

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
