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

### Sub-dimension 3a: Method (8 pts)
| Score | Criteria |
|-------|---------|
| 7–8 | Correct IADT method specified; consistent with requirement type |
| 5–6 | Method present but suboptimal (e.g., D instead of T for quantitative spec) |
| 2–4 | Method present but vague ("Testing" instead of T) |
| 0–1 | Blank or TBD |

### Sub-dimension 3b: Qualitative Criteria (12 pts)
| Score | Criteria |
|-------|---------|
| 10–12 | Specific executable procedure; cites reference standard; specifies equipment/conditions |
| 7–9 | Good procedure; minor gaps in equipment or standard citation |
| 4–6 | Vague procedure; lacks specifics; could not be executed as written |
| 1–3 | Generic statement ("perform test"); no procedure |
| 0 | Blank or circular ("verified when requirement is met") |

### Sub-dimension 3c: Quantitative Threshold (10 pts)
| Score | Criteria |
|-------|---------|
| 9–10 | Specific numeric value + units + pass condition; no ambiguity; covers worst-case |
| 6–8 | Numeric threshold present but missing units or worst-case conditions |
| 3–5 | Range estimate without reference; or "Pass/Fail" without numeric basis |
| 1–2 | "System shall pass" with no number |
| 0 | Blank or "TBD" |

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
