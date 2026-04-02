# Verification Methods — IADT Reference (ASPICE SYS.2 BP5)

---

## IADT Method Definitions

| Method | Code | Definition | Use When |
|--------|------|-----------|----------|
| **Inspection** | I | Examination of a work product without execution; visual review, document check, code review | Interface definitions, document structure, format compliance, design constraints |
| **Analysis** | A | Systematic evaluation using mathematical models, simulations, or logical reasoning | Performance margins, timing analysis, power budget, safety analysis (FMEA), mathematical derivation |
| **Demonstration** | D | Functional exercise showing the system operates as intended, without detailed measurement | Behavioral requirements, use cases, feature presence, operational modes |
| **Test** | T | Execution with measured inputs/outputs compared against defined thresholds | All requirements with quantitative pass/fail criteria, environmental limits, electrical specs |

---

## IADT Decision Tree

```
Does the requirement have a numeric threshold (value + units)?
  YES → T (Test) — measure it
  NO  →
    Can it be proven by calculation/simulation?
      YES → A (Analysis)
      NO  →
        Is it about document content, structure, or interface format?
          YES → I (Inspection)
          NO  → D (Demonstration)
```

**Default rule:** When in doubt between T and D — use T. Quantitative evidence is always stronger.

---

## Verification Criteria Format (Mandatory for BP5)

Every requirement must have ALL THREE components:

```
Verification Method:  [T | A | I | D]
Verification Criteria: [What is measured] + [How/procedure] + [Reference standard if applicable]
Acceptance Threshold:  [Numeric value + units] or [Pass condition with objective criteria]
```

---

## Examples by Requirement Type

### Functional — CAN Bus Bit Rate

**Requirement:** The system shall support CAN 2.0B communication at 500 kbps ± 50 ppm.

```
Verification Method:   T (Test)
Verification Criteria: Apply CAN analyzer (e.g., Vector CANalyzer) to CAN bus pins under nominal 
                       supply voltage (3.3V ± 5%) and measure bit rate over 10,000 consecutive frames
Acceptance Threshold:  Measured bit rate = 500,000 bps ± 25 bps (50 ppm); 
                       BER < 1×10⁻⁶; 0 error frames in 10,000-frame test
```

### Non-Functional — Temperature Range

**Requirement:** The system operating temperature shall conform to AEC-Q100 Grade 2, defined as -40°C to +105°C ambient.

```
Verification Method:   T (Test)
Verification Criteria: Subject device to HTOL (1000h at +105°C, Vcc_max) and 
                       LTOL (1000h at -40°C) per AEC-Q100 Rev-H Table 1; 
                       3 sample units minimum; verify functionality at start/mid/end
Acceptance Threshold:  0 functional failures; all datasheet parameters within spec; 
                       MTBF ≥ 10,000h at 25°C (Arrhenius model, Ea=0.7eV)
```

### Performance — Boot Time

**Requirement:** The system shall complete initialization and be ready to receive CAN frames within 500 ms of power-on.

```
Verification Method:   T (Test)
Verification Criteria: Apply power ramp (0V to Vcc in <10ms); timestamp power-on 
                       event; measure time to first valid CAN frame reception 
                       (ACK bit transmitted); repeat 50 cycles at 25°C
Acceptance Threshold:  T_boot ≤ 500 ms for 100% of test cycles; 
                       no single cycle exceeds 500 ms
```

### Interface — Pin Strapping

**Requirement:** The system shall support configuration via strapping pins sampled at power-on reset.

```
Verification Method:   I (Inspection) + T (Test)
Verification Criteria: I: Review schematic to confirm strapping pins routed to GPIO with pull-up/down; 
                       verify pin assignment document matches design
                       T: Apply each valid strapping combination; verify register reads match expected config
Acceptance Threshold:  I: 100% pin assignment match between spec and schematic
                       T: All N strapping combinations produce correct register values; 
                       0 mismatches
```

### Safety — Functional Safety ASIL

**Requirement:** The system shall implement diagnostic coverage for the watchdog timer meeting ASIL B requirements per ISO 26262 Part 5.

```
Verification Method:   A (Analysis)
Verification Criteria: Perform FMEA/FTA analysis for watchdog timer hardware; 
                       calculate diagnostic coverage (DC) per ISO 26262 Table D.1; 
                       review against ASIL B DC target
Acceptance Threshold:  DC ≥ 90% (High, per ISO 26262 Table D.1 for ASIL B); 
                       SPFM ≥ 97%; LFM ≥ 80%
```

### Design Constraint — Process Node

**Requirement:** The system shall be fabricated using a 28nm CMOS process or smaller.

```
Verification Method:   I (Inspection)
Verification Criteria: Review fabrication specification, tape-out checklist, and 
                       foundry process selection document
Acceptance Threshold:  Process node ≤ 28nm confirmed in foundry process selection document 
                       and tape-out sign-off
```

### Regulatory — EMC

**Requirement:** The system shall comply with CISPR 25 Class 3 radiated emissions limits.

```
Verification Method:   T (Test)
Verification Criteria: Radiated emissions test per CISPR 25:2021, Class 3 limits; 
                       frequency range 150kHz–1000MHz; test board with reference 
                       power setup per CISPR 25 Annex A
Acceptance Threshold:  All measurements ≤ CISPR 25 Class 3 limits at all frequencies; 
                       6dB margin preferred; 3dB minimum margin for release
```

---

## Common Verification Criteria Failures

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| `Method: TBD` | BP5 failure; no planned verification | Assign method immediately using decision tree |
| `Criteria: Verify the requirement is met` | Circular; no information | Describe actual measurement procedure |
| `Threshold: Pass` | Not quantitative | Add numeric value with units |
| `Method: Test` with no threshold | Partial — what counts as pass? | Add measurable pass/fail criterion |
| `Criteria: As per customer spec` | External reference not attached | Quote or attach the specific limit |
| Verification method inconsistent with threshold type | e.g., D (Demo) for a precision timing spec | Change to T (Test) |

---

## IADT Method by Requirement Type (Quick Reference)

| Requirement Type | Primary Method | Secondary |
|----------------|---------------|-----------|
| Electrical spec (voltage, current, timing) | T | A |
| Mechanical / dimensional | I | T |
| Thermal / environmental | T | A |
| Communication protocol | T | I |
| Safety (ASIL, FMEA) | A | I |
| Software functional behavior | T | D |
| Document/interface format | I | — |
| Performance (throughput, latency) | T | A |
| EMC/EMI | T | A |
| Regulatory compliance | T | I |
| Configuration / strapping | T | I |
