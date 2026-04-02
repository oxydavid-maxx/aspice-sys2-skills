# Requirement Card Template

## MANDATORY FIELDS — All fields required before baselined

```markdown
### [SysReq-XXX]: [Title: Feature/System — Property — Constraint, max 80 chars]

| Field | Content |
|-------|---------|
| **ID** | SysReq-XXX |
| **Version** | 1.0 |
| **Type** | [Functional / NFR-Performance / NFR-Reliability / NFR-Safety / Interface / Design Constraint / Regulatory] |
| **Priority** | [High / Medium / Low] |
| **Status** | [Draft / Under Review / Approved / Implemented / Verified] |
| **Upstream Trace** | [StRS-XXX] `derives-from` |
| **Downstream Trace** | Design: [HWD/SWArch ID] · Test: [SysQt-XXX] |
| **Review Date** | YYYY-MM-DD |
| **Reviewer** | [Name] |

---

#### Description

The [system/component] shall [active verb] [object] [quantified condition] [unit] [constraint/standard reference].

> **IEEE 29148 Checklist:**
> - [ ] Unambiguous: no forbidden vague terms
> - [ ] Complete: no TBD/TBC
> - [ ] Singular: one "shall"
> - [ ] Verifiable: can write T/A/I/D test
> - [ ] Conforming: active voice, units on all numbers

---

#### Verification Criteria (BP5 — 5-Field Format)

Source format: gateway_present_20260402_ASPICE.md Slide 12

| Field | Content |
|-------|---------|
| **Early Idea (Method)** | [T=Test / A=Analysis / I=Inspection / D=Demo — describe testing concept] |
| **Pre-condition** | [What must be set up/enabled/configured before the test can run] |
| **Input** | [What is sent, applied, or triggered to stimulate the system] |
| **Range** | [Valid values + boundary/edge cases + negative/invalid cases] |
| **Expected result** | [Measurable outcome — value + units + pass condition] — Source: <sup>[[N]](#fn-N)</sup> |

> **Threshold Source Rule (MANDATORY):** Every numeric value in Expected result MUST have a clickable citation<sup>[[N]](#fn-N)</sup> to its source — one of:
> - **Customer requirement**: → Leapmotor meeting transcript, customer spec, StRS document
> - **Standard mandate**: → ISO, AEC-Q100, CISPR, IEEE standard + clause
> - **Engineering analysis**: → Simulation report, calculation with methodology cited
> - **Design target**: Must be explicitly labeled "Design target — to be confirmed during HW characterization"
>
> A bare number without a citation is **not acceptable** — the assessor will ask "where does this number come from?" and the requirement will fail the Correct characteristic (IEEE 29148 §5.2.8).

---

#### Diagram

*[Include a Mermaid diagram if the requirement is behavioral (state machine), interface (sequence diagram), functional flow (flowchart), or involves complex conditions. Skip only for simple threshold requirements with no behavioral complexity.]*

**[Select diagram type based on requirement nature:]**

**For behavioral/state requirements:**
```mermaid
stateDiagram-v2
  [*] --> [Initial State]
  [Initial State] --> [Active State]: [Trigger Condition]
  [Active State] --> [Fault State]: [Error Condition]
  [Fault State] --> [Initial State]: [Recovery Condition]
```

**For interface/communication requirements:**
```mermaid
sequenceDiagram
  participant Host
  participant System
  participant External
  Host->>System: [Command/Request]
  System->>External: [Protocol Frame]
  External-->>System: [Response within Xms]
  System-->>Host: [Status/Ack]
```

**For functional flow requirements:**
```mermaid
flowchart LR
  Input["[Input Condition]"] --> Process["[System Processing]"]
  Process --> Condition{[Decision]}
  Condition -->|"Normal"| Output["[Expected Output]"]
  Condition -->|"Error"| Fault["[Error Handling]"]
```

**For verification flow:**
```mermaid
flowchart LR
  req["SysReq-XXX\n[Title]"] -->|"[Method]"| proc["[Test Procedure]"]
  proc --> threshold["Threshold:\n[Value + Units]"]
  threshold -->|"≥ Threshold"| pass["✅ PASS"]
  threshold -->|"< Threshold"| fail["❌ FAIL → Rework"]
```

---

#### Rationale *(optional but recommended)*
[Why this requirement exists; which stakeholder need it satisfies; any design decisions it constrains]

---

#### Change History

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 1.0 | YYYY-MM-DD | [Name] | Initial |
```

---

## Example — Completed Requirement Card

```markdown
### SysReq-001: CAN Bus Interface — Maximum Bit Rate and Tolerance

| Field | Content |
|-------|---------|
| **ID** | SysReq-001 |
| **Version** | 1.2 |
| **Type** | Interface |
| **Priority** | High |
| **Status** | Approved |
| **Upstream Trace** | StRS-100 `derives-from` |
| **Downstream Trace** | Design: HWD-CAN-001 · Test: SysQt-001, SysQt-002 |
| **Review Date** | 2026-04-01 |
| **Reviewer** | J. Chen |

#### Description

The system shall support CAN 2.0B communication at a nominal bit rate of 500 kbps ± 50 ppm under all specified supply voltage (3.0V–3.6V) and temperature (-40°C to +105°C) conditions.

> **IEEE 29148 Checklist:** ✅ All 9 characteristics pass

#### Verification Criteria (BP5)

| Field | Content |
|-------|---------|
| **Early Idea (Method)** | T (Test) — Measure bit rate on CAN H/L pins using a CAN protocol analyzer (e.g., Vector CANalyzer). |
| **Pre-condition** | Device powered at nominal Vcc (3.3V); CAN transceiver enabled; CAN controller configured for 500 kbps mode; temperature chamber stabilized. |
| **Input** | Transmit 10,000 consecutive CAN 2.0B frames with known data pattern. |
| **Range** | Voltage: 3.0V, 3.3V, 3.6V. Temperature: -40°C, +25°C, +105°C (9 conditions total). Negative: invalid baud rate register — expect no transmission. |
| **Expected result** | Bit rate = 500,000 bps ± 25 bps (50 ppm); BER ≤ 1×10⁻⁶; 0 error frames per 10,000-frame run per condition. Source: StRS-100. |

#### Diagram

```mermaid
sequenceDiagram
  participant Host as Host MCU
  participant SUT as System (SUT)
  participant CANBus as CAN Bus Network
  
  Host->>SUT: CAN TX frame (8-byte DLC)
  SUT->>CANBus: CAN 2.0B frame @ 500kbps
  Note over SUT,CANBus: Bit timing: ±50ppm tolerance
  CANBus-->>SUT: ACK bit (within 1 bit time)
  SUT-->>Host: TX Complete status
  
  Note over Host,SUT: Test: 10,000 frames<br/>All temps & voltages<br/>BER ≤ 1×10⁻⁶
```
```
