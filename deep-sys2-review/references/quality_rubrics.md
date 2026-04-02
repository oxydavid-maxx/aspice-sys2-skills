# Quality Rubrics — 0-100 Scoring per Review Dimension

---

## Dimension 1: Title Quality (20 pts per requirement)

| Score | Descriptor | Criteria |
|-------|-----------|---------|
| 18–20 | Excellent | Noun-phrase format; ≤80 chars; unique; no unexplained abbreviations; mirrors "what" not "how"; self-explanatory without reading body |
| 13–17 | Good | Mostly correct format; minor issues (slightly too long, or abbreviation without glossary) |
| 7–12 | Fair | Recognizable subject but missing property or constraint; needs rework |
| 1–6 | Poor | Single-word or generic title (e.g., "Temperature", "CAN"); not self-explanatory |
| 0 | Unacceptable | ID only (e.g., "Req-001"); blank; not a title |

---

## Dimension 2: Description Quality (40 pts per requirement)

### Sub-dimension 2a: Shall Statement Structure (15 pts)
| Score | Criteria |
|-------|---------|
| 13–15 | Active voice; one "shall"; clear subject; no passive voice; no design prescription |
| 9–12 | Mostly correct; minor passive voice or slight design leak |
| 5–8 | Passive voice; compound statement (multiple "shall") |
| 1–4 | Multiple violations; "should" / "will" used |
| 0 | No "shall" statement; not a requirement |

### Sub-dimension 2b: Unambiguity (15 pts)
| Score | Criteria |
|-------|---------|
| 13–15 | Zero forbidden terms; one interpretation; all conditions stated explicitly |
| 9–12 | One minor forbidden term; or one condition could be clearer |
| 5–8 | Multiple forbidden terms; or significant ambiguity |
| 1–4 | Major ambiguity; most reviewers would interpret differently |
| 0 | Completely vague; no objective content |

### Sub-dimension 2c: Completeness & Units (10 pts)
| Score | Criteria |
|-------|---------|
| 9–10 | All numeric values have units; no TBD/TBC; all conditions stated |
| 6–8 | Minor TBD or one missing unit |
| 3–5 | Multiple missing units or TBDs |
| 1–2 | Major TBDs; missing critical condition information |
| 0 | "TBD" in description or body is blank |

---

## Dimension 3: Verification Criteria (30 pts per requirement)

**Required format:** 5-field VC (per Realtek ASPICE training — gateway_present_20260402_ASPICE.md Slide 12)
Fields: Early Idea (Method) · Pre-condition · Input · Range · Expected result

**Additional cross-checks (affect sub-dimension scores):**
- Forbidden vocabulary in any VC field (reduces sub-dimension score)
- VC–Description alignment (misalignment penalizes the relevant sub-dimension)
- Method appropriateness for requirement type (affects 3a score)
- Execution feasibility (BLOCK condition — see below)

### Sub-dimension 3a: Early Idea / Method (6 pts)
| Score | Criteria |
|-------|---------|
| 5–6 | Correct IADT method for this requirement type; testing concept described (instrument/signal/measurement); no forbidden vocabulary |
| 3–4 | Method stated (T/A/I/D) but concept not described; OR method is suboptimal for requirement type (e.g., D for quantitative spec) |
| 1–2 | Method present but vague ("test it", "check", "verify"); no IADT classification; OR method stated without any concept |
| 0 | Blank, "TBD", or missing |

**Forbidden vocabulary in Early Idea:** "test it", "check", "verify", "confirm", "ensure", "as per standard" (without naming it)

**Method appropriateness quick-check:**
- Quantitative electrical/timing spec → must be **T**; D = automatic -2 pts
- Safety/FMEA/ASIL → must be **A**; T alone insufficient
- Document/interface format → **I**; T overkill but acceptable
- Behavioral with no measurable threshold → **D** acceptable

### Sub-dimension 3b: Pre-condition (6 pts)
| Score | Criteria |
|-------|---------|
| 5–6 | Specific: system state, configuration values, equipment type identified; covers all variables in the description (temperature, voltage, clock, etc.) |
| 3–4 | Partial — device powered on stated but critical configuration or equipment omitted; or misses one variable from description |
| 1–2 | Generic forbidden vocabulary used ("properly configured", "system ready", "normal operation"); no actionable setup |
| 0 | Blank, "N/A" without justification, or missing |

**Forbidden vocabulary in Pre-condition:** "properly configured", "adequately set up", "system ready", "normal operation", "device operational", "standard conditions"
- These are circular — they assume what needs to be demonstrated

**Alignment check:** Pre-condition must reference all variables bounded in the description.
- Description specifies temperature range → pre-condition must state temperature chamber
- Description specifies supply voltage → pre-condition must state power supply setting
- Description specifies clock frequency → pre-condition must state clock source configuration

### Sub-dimension 3c: Input (6 pts)
| Score | Criteria |
|-------|---------|
| 5–6 | Stimulus type + specific value/format + quantity all stated; no forbidden vocabulary; covers what description implies as the trigger |
| 3–4 | Stimulus described but missing value, format, or quantity (e.g., "send CAN frame" without count or pattern) |
| 1–2 | Forbidden vocabulary used ("appropriate signal", "valid data"); or stimulus vaguely described |
| 0 | Blank or missing |

**Forbidden vocabulary in Input:** "appropriate signal", "valid data", "normal input", "typical stimulus", "suitable packet", "apply input", "send data"
- Each of these defers the specification to test execution time — BP5 requires it to be defined now

**Alignment check:** Input must cover the stimulus implied by the description.
- Description: "500 kbps ±50 ppm" → Input must specify the baud rate being tested
- Description: "IP routing" → Input must specify packet type (unicast/multicast), destination IP, routing configuration

### Sub-dimension 3d: Range / Coverage (6 pts)
| Score | Criteria |
|-------|---------|
| 5–6 | Valid nominal cases + at least one boundary/edge case + at least one explicit negative/invalid case; specific values enumerated |
| 3–4 | Valid range stated but: no boundary cases, OR no negative test, OR "various/several/multiple" used without enumeration |
| 1–2 | Single test point only; or forbidden vocabulary used without any specifics |
| 0 | Blank or missing |

**Forbidden vocabulary in Range:** "various values", "several conditions", "multiple scenarios", "positive and negative tests" (without specifying them), "all valid inputs", "typical cases"

**Negative test requirement (MANDATORY for 5–6 score):**
Every Range must include at least one explicitly named negative case:
- Electrical: out-of-spec voltage, out-of-range frequency, wrong polarity
- Interface: invalid packet type, oversized frame, malformed header, CRC error
- Configuration: undefined register value, floating strapping pin
- Behavioral: input that should be rejected, disabled state behavior

**Alignment check:** Range must enumerate all operating conditions specified in the description.
- Description: "-40°C to +105°C" → Range must include -40°C, a midpoint, and +105°C
- Description: "3.0V to 3.6V supply" → Range must test 3.0V, 3.3V, 3.6V minimum

### Sub-dimension 3e: Expected Result (6 pts)
| Score | Criteria |
|-------|---------|
| 5–6 | Value + units + pass condition; source cited; no forbidden vocabulary; matches all numeric constraints in the description exactly |
| 3–4 | Numeric threshold present but: missing units, OR missing source citation, OR does not cover all conditions in description |
| 1–2 | Qualitative only; OR forbidden vocabulary used instead of quantitative threshold |
| 0 | Blank, "TBD", circular ("requirement is met", "functions normally"), or no numeric content |

**Forbidden vocabulary in Expected Result (Tier 1 — BLOCK):**
"Pass", "Fail", "Correct", "Acceptable", "Good", "OK", "Meets the requirement", "Functions normally", "Works as expected", "No errors" (without specifying the error detection method and threshold)

**Forbidden vocabulary in Expected Result (Tier 2 — WARN, score ≤ 3):**
"Within tolerance" (without value), "Less than specification" (without citing spec), "Approximately X", "Minimal errors", "Low latency"

**Alignment check — the most critical VC review step:**
The Expected result threshold must exactly match the description's numeric constraints:
- Description: "500 kbps ± 50 ppm" → Expected result must verify ±50 ppm (= ±25 bps), not just "500 kbps"
- Description: "replace DMAC and SMAC and VLAN ID" → Expected result must check all three fields, not just DMAC
- Description: "within 1ms of fault detection" → Expected result must include 1ms timing measurement

---

### Execution Feasibility — BLOCK Conditions (not scored, override all sub-dimensions)

Any of the following → BLOCK the entire VC record regardless of sub-dimension scores:
- References undefined test equipment ("specialized oscilloscope" without specifying capability)
- References undefined standard ("per internal test procedure" without document name/version)
- Pre-condition requires a state that cannot be achieved with available equipment
- Expected result requires measurement capability not established in Pre-condition
- Circular across fields: Expected result references Pre-condition without adding new information

---

## Dimension 4: Traceability (10 pts per requirement)

| Score | Criteria |
|-------|---------|
| 9–10 | Upstream StRS link present (specific ID, not generic); downstream test link present |
| 6–8 | Upstream link present; no test link yet (acceptable for Draft status) |
| 3–5 | No upstream link but requirement is clearly derivable; no test link |
| 0–2 | No upstream link with no apparent StRS basis (orphan) |

---

## Document-Level Scores

### Traceability Coverage Score (0–100)
```
Coverage score = (upstream% × 0.4) + (downstream_test% × 0.4) + (consistency_evidence% × 0.2)

Where:
  upstream% = % SysReqs with StRS link
  downstream_test% = % SysReqs with test case link
  consistency_evidence% = review records present (0 or 100)
```

### BP Compliance Score (0–100 per BP)
See `references/aspice_cl_determination.md` for NPLF → score mapping.

### Overall Document Score
```
Overall = (BP_avg × 0.50) + (req_quality_avg × 0.30) + (trace_coverage × 0.20)
```

### Score Interpretation
| Score | Assessment Readiness |
|-------|---------------------|
| 90–100 | Excellent — ready for assessment |
| 80–89 | Good — minor improvements recommended |
| 70–79 | Acceptable — targeted fixes needed before assessment |
| 60–69 | Developing — significant gaps, 2–4 weeks work |
| 50–59 | Insufficient — fundamental gaps, 1–2 months work |
| < 50 | Poor — major restructuring needed |
