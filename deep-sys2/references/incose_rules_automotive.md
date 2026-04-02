# INCOSE Guide to Writing Requirements V4 — Automotive Adaptation

Source: INCOSE Requirements Working Group, Guide to Writing Requirements V4 (2023)
Adapted for: Automotive embedded systems (ASPICE SYS.2 context)

---

## Critical Rules (Must Apply to Every Requirement)

### R1 — Use "Shall" for Mandatory Requirements
- **Shall** = mandatory (use in all SyRS requirements)
- **Should** = desired, not mandatory (do NOT use in requirements — creates ambiguity)
- **Will** = future state or commitment (do NOT use — requirement is a current specification)
- **May** = permitted but not required (OK for optional capabilities only)
- **Must** = used for constraints imposed by external entities (regulations, standards)

```
✅ The system shall support CAN 2.0B protocol.
❌ The system should support CAN 2.0B protocol.  ← ambiguous obligation level
❌ The system will support CAN 2.0B protocol.    ← "will" = future promise, not requirement
```

---

### R2 — Use Active Voice
The subject of the requirement must act, not be acted upon.

```
✅ The system shall transmit a status frame every 100ms.
❌ A status frame shall be transmitted by the system every 100ms.
❌ Status frames shall be generated every 100ms.  ← no subject
```

Identify who/what performs the action. If subject is unclear, the requirement may belong to a different system element.

---

### R3 — State Requirements, Not Implementation
Requirements describe WHAT, not HOW.

```
✅ The system shall detect loss of CAN bus communication within 10ms.
❌ The system shall implement a 10ms watchdog timer to detect CAN bus loss.  ← prescribes design
✅ The system shall store up to 256 fault codes in non-volatile memory.
❌ The system shall use SPI EEPROM to store fault codes.  ← prescribes component
```

**Exception:** When a specific implementation is a customer requirement (e.g., customer mandates a specific protocol), it is appropriate.

---

### R6 — Include Units for All Numeric Values
Every number must be followed by its unit of measurement.

```
✅ 500 kbps    ✅ -40°C    ✅ 3.3V ± 5%    ✅ 100ms    ✅ 1.5A max
❌ 500         ❌ -40      ❌ 3.3          ❌ 100      ❌ 1.5
```

For ratios and percentages: state what 100% represents.
For tolerances: state ±value or min/max range explicitly.

---

### R8 — One Requirement Per Statement (Singularity)
One "shall" per requirement. Split compound requirements.

```
✅ SysReq-001: The system shall support CAN 2.0A protocol.
✅ SysReq-002: The system shall support CAN 2.0B protocol.
❌ The system shall support CAN 2.0A and CAN 2.0B protocol.  ← compound → split
```

**Warning signals for compound requirements:**
- "...and..." between two verbs
- "...as well as..."
- Multiple bullets in one requirement cell
- Requirement with two verification methods needed

---

### R10 — Avoid Negative Requirements Where Possible
Negative requirements ("shall not") create verification challenges.

```
✅ The system shall transmit only valid CAN frames (DLC ≥ 0, ≤ 8 bytes).
❌ The system shall not transmit invalid CAN frames.  ← hard to verify exhaustively
```

When negative requirements are necessary, provide a positive verification criterion.

---

## Forbidden Terms Blacklist (Auto-Reject)

If any of these terms appear in a requirement description, the requirement FAILS the Unambiguous gate and must be rewritten:

### Vague Quantities
| Term | Replace With |
|------|-------------|
| approximately | exact value ± tolerance |
| about | exact value ± tolerance |
| several | specific number |
| various | enumerate the items |
| many | specific number |
| few | specific number |
| some | specific number |
| most | percentage (≥ X%) |
| often | frequency (every Xms, X times/hour) |
| rarely | MTTF or failure rate |
| a number of | specific number |
| multiple | specific number |

### Vague Quality
| Term | Replace With |
|------|-------------|
| sufficient | specific amount or threshold |
| adequate | specific criteria |
| reasonable | specific criteria or reference standard |
| appropriate | specific criteria |
| proper | specific criteria |
| suitable | specific criteria |
| acceptable | specific threshold |
| good | specific measurable characteristic |
| high quality | specific quality metrics |
| state of the art | specific technology or standard |
| best effort | specific guaranteed minimum |
| flexible | specific range or modes |
| robust | specific failure modes and recovery |
| reliable | MTBF value, failure rate |
| stable | specific stability criterion (e.g., drift rate) |
| user friendly | specific usability metrics |

### Vague Temporal
| Term | Replace With |
|------|-------------|
| as soon as possible | within X ms/s |
| timely | within X ms/s |
| quickly | within X ms/s |
| rapidly | within X ms/s |
| promptly | within X ms/s |
| periodically | every X ms, X times/s |
| regularly | every X ms |
| frequently | every X ms or X times/hour |

### Escape Clauses (Weaken Requirements)
| Term | Action |
|------|--------|
| if possible | remove — it's either required or not |
| if applicable | specify when it applies |
| where necessary | specify the condition |
| as required | reference the specific requirement |
| to the extent possible | remove — weaken to unenforceable |
| etc. | enumerate all items explicitly |

---

## Title Writing Rules

### R-TITLE-1: Use Noun-Phrase Format
`[Feature/System Component] + [Property/Action] + [Constraint/Qualifier]`

```
✅ CAN Bus Interface — Maximum Bit Rate
✅ Operating Temperature — AEC-Q100 Grade 2 Compliance Range
✅ Watchdog Timer — Reset Response Time
✅ Power Supply — Nominal Input Voltage Range
✅ Strapping Pin Configuration — Sampling Timing at POR

❌ Temperature           ← too vague, no property
❌ CAN                   ← not descriptive
❌ Req-201246            ← ID only, not a title
❌ System requirement 1  ← meaningless
```

### R-TITLE-2: Max 80 Characters
Titles longer than 80 characters are hard to display in tools and traceability matrices.

### R-TITLE-3: No Abbreviations Without Glossary Entry
If an abbreviation is used in a title, it must appear in the project glossary.
Exception: Universally recognized standards abbreviations (CAN, SPI, UART, AEC-Q100, ISO, IEC).

### R-TITLE-4: Unique and Self-Explanatory
Two requirements in the same SyRS must not have the same or near-identical titles.
Reading the title alone should give a reader a clear idea of the requirement's subject.

### R-TITLE-5: Mirror the "What" Not the "How"
```
✅ "Ethernet PHY — Maximum Latency for Frame Forwarding"
❌ "Use the MAC hardware buffer to minimize Ethernet frame forwarding delay"  ← describes how
```

---

## Common Requirement Anti-Patterns with Corrections

| Anti-Pattern | INCOSE Violation | Corrected Version |
|-------------|-----------------|------------------|
| "The system shall be fast" | R-Unambiguous, R6 | "The system shall process interrupt requests within 1µs" |
| "The system should support..." | R1 | "The system shall support..." |
| "Temperature requirement" (title only) | R-TITLE-1 | "Operating Temperature — Extended Industrial Range -40°C to +85°C" |
| "The system shall be reliable" | R-Unambiguous | "The system shall achieve MTBF ≥ 50,000h at 40°C per MIL-HDBK-217F" |
| "shall be designed to handle X and Y" | R3, R8 | Split into two requirements; remove "designed to" |
| "as specified in TBD document" | R-Complete | Reference specific document name and revision |
| "The system must not fail" | R10, R-Unambiguous | "The system shall achieve FPMH ≤ X per MIL-HDBK-217F" |
| "system shall support various protocols" | R-Unambiguous, R8 | List each protocol as a separate requirement |
| "approximately 100ms" | R-Unambiguous, R6 | "100ms ± 10ms" or "≤ 100ms" |
