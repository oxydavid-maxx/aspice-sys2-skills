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

### Primary Format — 5-Field VC (from internal Realtek ASPICE training)

Source: gateway_present_20260402_ASPICE.md, Slide 12 — "Develop Verification Criteria"

Every requirement must have ALL FIVE fields:

```
Early Idea (Method): [Testing approach concept — e.g., inject packet, apply voltage, read register]
Pre-condition:       [What must be set up/enabled before the test can run]
Input:               [What is sent, applied, or triggered to stimulate the system]
Range:               [Coverage scope: valid cases + edge cases + negative/invalid cases]
Expected result:     [Measurable outcome — what the system must produce, with quantitative threshold]
```

**Why 5-field, not just 3-field (Method + Criteria + Threshold)?**
- **Pre-condition** forces engineers to think about test setup — a missing pre-condition causes test failures that look like requirement failures
- **Input** separates the stimulus from the outcome — clarifies the test boundary (black-box thinking)
- **Range** mandates coverage thinking at the requirement level — "are we testing the edges?"
- Together, these 5 fields map directly to an executable test case

### Relationship to IADT

The IADT method selection (see above) determines the **Early Idea** field. The 5-field format replaces the 3-field format as the BP5 artifact. Both are valid ASPICE evidence; 5-field is preferred for completeness.

| 5-Field | Maps to IADT | Notes |
|---------|-------------|-------|
| Early Idea (Method) | Method (T/A/I/D) | Select method using IADT decision tree |
| Pre-condition | Part of Criteria | Setup conditions needed before execution |
| Input | Part of Criteria | Stimulus applied to the system |
| Range | Part of Criteria | Coverage scope including negative tests |
| Expected result | Acceptance Threshold | Must include quantitative pass/fail |

---

## Examples by Requirement Type

### Functional — MAC Address Replacement (from gateway training)

**Requirement:** If the IP routing is ready to transmit the packet, the switch shall replace destination MAC address according to the configured EGRIF entry.

```
Early Idea (Method): Inject a test packet at the Ingress port and capture it at the Egress port 
                     using a network packet analyzer.
Pre-condition:       IP routing is enabled, and an EGRIF entry is strictly configured with a 
                     specific Unicast DMAC, SMAC, and VLAN ID.
Input:               Send an IP packet with a destination IP that matches the routing rule 
                     mapped to the configured EGRIF.
Range:               Cover valid Unicast MACs / VLANs (1-4094), and include negative tests for 
                     Multicast/Broadcast MACs or missing EGRIF.
Expected result:     The captured Egress packet's DMAC, SMAC, and VLAN ID must be exactly 
                     replaced according to the EGRIF configuration.
```

### Functional — CAN Bus Bit Rate

**Requirement:** The system shall support CAN 2.0B communication at 500 kbps ± 50 ppm.

```
Early Idea (Method): Measure bit rate on CAN bus pins using a CAN protocol analyzer 
                     (e.g., Vector CANalyzer).
Pre-condition:       Device powered at nominal Vcc (3.3V ± 5%); CAN transceiver enabled; 
                     CAN controller configured for 500 kbps mode.
Input:               Transmit 10,000 consecutive CAN 2.0B frames with known data pattern.
Range:               Valid bit rates: 125/250/500/1000 kbps. Negative: invalid baud rate 
                     register values (expect no transmission or error frame).
Expected result:     Measured bit rate = 500,000 bps ± 25 bps (50 ppm); 
                     BER < 1×10⁻⁶; 0 error frames in 10,000-frame test.
```

### Non-Functional — Temperature Range

**Requirement:** The system operating temperature shall conform to AEC-Q100 Grade 2 (-40°C to +105°C ambient).

```
Early Idea (Method): Environmental stress testing per AEC-Q100 Rev-H (HTOL + LTOL).
Pre-condition:       3 sample units; baseline electrical characterization at 25°C complete; 
                     test boards prepared per AEC-Q100 test setup.
Input:               HTOL: 1000h at +105°C, Vcc_max. LTOL: 1000h at -40°C.
Range:               Temperatures: -40°C, 25°C (room), +85°C, +105°C. 
                     Negative: operation outside range — expect functional failure or 
                     thermal shutdown (document behavior, do not pass).
Expected result:     0 functional failures across all 3 units; all datasheet electrical 
                     parameters within spec at start/mid/end checkpoints; 
                     MTBF ≥ 10,000h at 25°C (Arrhenius model, Ea=0.7eV).
```

### Performance — Boot Time

**Requirement:** The system shall complete initialization and be ready to receive CAN frames within 500 ms of power-on.

```
Early Idea (Method): Power ramp + timestamp measurement; oscilloscope or logic analyzer 
                     to capture power-on and first CAN ACK events.
Pre-condition:       CAN bus connected; CAN test node sending frames from T=0; 
                     all strapping pins in default configuration.
Input:               Apply power ramp (0V to Vcc in <10ms); repeat 50 cycles at 25°C.
Range:               Temperature variants: -40°C, 25°C, +105°C. 
                     Negative: power ramp >50ms — boot time may exceed spec; document result.
Expected result:     T_boot ≤ 500 ms for 100% of 50 test cycles at each temperature; 
                     no single cycle exceeds 500 ms.
```

### Interface — Pin Strapping

**Requirement:** The system shall support configuration via strapping pins sampled at power-on reset.

```
Early Idea (Method): I (Inspection) + T (Test) — schematic review followed by functional 
                     verification of each strapping combination.
Pre-condition:       Schematic available; test board with strapping pin access; 
                     register read capability via debug interface.
Input:               Apply each valid strapping combination (all N combinations) 
                     before power-on; read configuration registers after reset.
Range:               All valid strapping combinations. Negative: floating pins (no pull-up/down) 
                     — expect defined default or error state.
Expected result:     I: 100% pin assignment match between spec and schematic.
                     T: All N combinations produce correct register values; 0 mismatches; 
                     floating pin produces documented default (not undefined behavior).
```

### Safety — Functional Safety ASIL

**Requirement:** The system shall implement diagnostic coverage for the watchdog timer meeting ASIL B requirements per ISO 26262 Part 5.

```
Early Idea (Method): A (Analysis) — FMEA/FTA on watchdog timer hardware.
Pre-condition:       Watchdog timer hardware design finalized; FMEA worksheet template 
                     available per ISO 26262 Part 5 Annex D.
Input:               Hardware design documentation, fault injection analysis results.
Range:               All single-point and residual fault scenarios per ISO 26262 Table D.1; 
                     include latent faults (LFM). Negative: insufficient DC — blocks ASIL B.
Expected result:     DC ≥ 90% (High category, per ISO 26262 Table D.1 for ASIL B); 
                     SPFM ≥ 97%; LFM ≥ 80%.
```

### Design Constraint — Process Node

**Requirement:** The system shall be fabricated using a 28nm CMOS process or smaller.

```
Early Idea (Method): I (Inspection) — foundry process selection review.
Pre-condition:       Tape-out checklist and foundry process selection document available.
Input:               Review fabrication specification and tape-out sign-off records.
Range:               Valid: ≤28nm. Negative: >28nm process selected — requirement fails.
Expected result:     Process node ≤ 28nm confirmed in both foundry process selection 
                     document and tape-out sign-off; no waiver required.
```

### Regulatory — EMC

**Requirement:** The system shall comply with CISPR 25 Class 3 radiated emissions limits.

```
Early Idea (Method): T (Test) — radiated emissions measurement per CISPR 25:2021.
Pre-condition:       Test board with reference power setup per CISPR 25 Annex A; 
                     anechoic chamber or OATS calibrated; device in operational mode.
Input:               Device operating at full functional load; frequency sweep 150 kHz–1000 MHz.
Range:               Full frequency range; test at multiple orientations; 
                     negative: disabled mode — verify emissions drop significantly.
Expected result:     All measurements ≤ CISPR 25 Class 3 limits at all frequencies; 
                     6 dB margin preferred; 3 dB minimum margin for release.
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
