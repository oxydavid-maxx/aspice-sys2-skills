# ISO/IEC/IEEE 29148:2018 — Requirement Quality Characteristics

Source: ISO/IEC/IEEE 29148:2018, Systems and software engineering — Life cycle processes — Requirements engineering

---

## The 9 Quality Characteristics

Every individual requirement must satisfy all 9 characteristics. Failure on any single characteristic is grounds for rejection.

---

### 1. Necessary
**Definition:** The requirement defines something essential to meeting stakeholder needs. Its absence would result in a deficiency that cannot be compensated by other requirements.

**Test:** "Would removing this requirement leave a gap in the system's ability to meet stakeholder needs?"
- PASS: Yes — the gap would exist
- FAIL: No — another requirement already covers it (duplicate); or it came from internal engineering preference with no stakeholder origin

**Automotive examples:**
- ✅ Necessary: "The system shall operate at -40°C to +105°C" (customer environment spec drives this)
- ❌ Not necessary: "The system shall use 28nm TSMC process" if no customer spec mandates it and alternatives exist

---

### 2. Appropriate
**Definition:** The requirement is at the correct level of abstraction for the entity. System requirements describe system behavior, not internal design.

**Test:** "Does this requirement specify WHAT the system does (black box), not HOW it does it?"
- PASS: Describes observable behavior, output, or constraint without prescribing implementation
- FAIL: Prescribes internal architecture, algorithm choice, or implementation detail

**Automotive examples:**
- ✅ Appropriate: "The system shall detect CAN bus-off state within 10ms and notify the host controller"
- ❌ Not appropriate (SW design leaked in): "The system shall use a ring buffer of 64 CAN frames to implement message queuing"

---

### 3. Unambiguous
**Definition:** The requirement has exactly one interpretation. Every reader arrives at the same understanding.

**Test:** "Can this be interpreted in more than one way? Does it contain vague terms?"

**Forbidden vague terms (trigger rejection):**
- Quantity: approximately, about, several, various, many, few, some, most, often
- Quality: sufficient, adequate, reasonable, appropriate, proper, suitable, acceptable
- Performance: fast, slow, small, large, minimal, maximum possible, high quality, good
- Temporal: as soon as possible, timely, promptly, regularly, frequently

**Automotive examples:**
- ✅ Unambiguous: "The system shall respond to a CAN remote frame within 2ms ± 100µs"
- ❌ Ambiguous: "The system shall respond quickly to CAN remote frames"
- ✅ Unambiguous: "The system shall support 8 independent CAN message buffers"
- ❌ Ambiguous: "The system shall support several CAN message buffers"

---

### 4. Complete
**Definition:** The requirement needs no additional information to be understood and verified. All conditions, exceptions, and boundaries are stated.

**Test:** "Is there any missing information needed to verify this requirement?"
- FAIL: Contains TBD, TBC, placeholder text, or references undefined external documents
- FAIL: Conditional requirement with unstated conditions ("if applicable", "when necessary")
- FAIL: Missing boundary conditions (only nominal specified, no error/edge case)

**Automotive examples:**
- ✅ Complete: "The system shall maintain CAN communication at 500 kbps under supply voltage 3.0V–3.6V, temperature -40°C to +105°C, and all documented operating modes"
- ❌ Incomplete: "The system shall maintain CAN communication (details TBD)"
- ❌ Incomplete: "The system shall support the operating temperature range as specified in [TBD document]"

---

### 5. Singular
**Definition:** The requirement states exactly one capability, characteristic, constraint, or quality factor. It contains exactly one "shall."

**Test:** "Does this requirement contain 'and', 'or', 'also', 'furthermore', 'in addition'?"
- Multiple "shall" statements in one requirement → split
- "shall X and Y" → split into two requirements
- "shall X or Y" → usually ambiguous; clarify which is required

**Automotive examples:**
- ✅ Singular: "The system shall support 25 MHz crystal clock input"
- ✅ Singular (separate): "The system shall support 50 MHz crystal clock input"
- ❌ Compound: "The system shall support 25 MHz and 50 MHz crystal clock inputs" → SPLIT

**Exception:** "X and Y" is acceptable when X and Y are inseparable atomic items (e.g., "The system shall support TX and RX on UART0" — these are genuinely paired)

---

### 6. Feasible
**Definition:** The requirement can be implemented within the project's known constraints (technology, cost, schedule, physics).

**Test:** "Is there any known reason this cannot be achieved with current or near-term technology within project constraints?"
- FAIL: Physics violation ("shall respond in 0 nanoseconds")
- FAIL: Contradicts known technology limits at the specified cost point
- FAIL: Requires technology not yet available for production

**Automotive examples:**
- ✅ Feasible: "The system shall achieve MTBF ≥ 100,000h at 40°C (Arrhenius, Ea=0.7eV)"
- ❌ Infeasible: "The system shall achieve 100% diagnostic coverage for all random hardware faults" (physically impossible per ISO 26262)
- ⚠️ Review: "The system shall operate at 250 GHz clock" — flag for technical feasibility review

---

### 7. Verifiable
**Definition:** The requirement can be verified at an acceptable cost using Inspection, Analysis, Demonstration, or Test (IADT).

**Test:** "Can I write a test procedure that produces a pass/fail result for this requirement?"
- FAIL: No quantitative threshold — cannot produce objective pass/fail
- FAIL: Subjective criteria — "user friendly", "intuitive", "aesthetically pleasing"
- FAIL: Requires undefined future state to verify — "shall be upgradeable"

**Automotive examples:**
- ✅ Verifiable: "The system shall complete self-test within 100ms of power-on reset"
- ❌ Not verifiable: "The system shall perform self-test efficiently"
- ✅ Verifiable: "MTBF ≥ 50,000h calculated per MIL-HDBK-217F at 40°C"
- ❌ Not verifiable: "The system shall be highly reliable"

**Note:** Verifiability requires BP5 (Verification Criteria) to be defined. A requirement cannot be marked "verifiable" without a corresponding BP5 record.

---

### 8. Correct
**Definition:** The requirement accurately represents a real stakeholder need and is consistent with all other requirements and constraints.

**Test:** "Does this requirement conflict with any other requirement, constraint, or stakeholder document?"
- FAIL: Conflicts with another SysReq
- FAIL: Contradicts the StRS it traces to
- FAIL: Exceeds the system boundary (requires something outside the system's scope)

**Automotive examples:**
- ✅ Correct: SysReq temperature -40°C to +105°C traces to StRS customer environment spec -40°C to +105°C
- ❌ Incorrect (conflict): SysReq says "max power 2W" while another SysReq requires functionality that physics shows needs 5W
- ❌ Incorrect (scope): SysReq mandates behavior of an external ECU that is out of scope

---

### 9. Conforming
**Definition:** The requirement adheres to the agreed writing style, templates, and terminology defined for the project.

**Test:** "Does this requirement follow the project's requirements writing standard?"
- Must use active voice ("The system shall..." not "It is required that...")
- Must use "shall" (mandatory), not "should" (desired), "will" (future state), "must" (policy)
- Must include measurement units for all numeric values
- Must use defined terminology from the project glossary
- Must follow the agreed template structure

---

## Quick Compliance Checklist

For each requirement, check:
```
[ ] Necessary   — traces to a real stakeholder need, not a gold-plated nice-to-have
[ ] Appropriate — black box behavior, not internal design
[ ] Unambiguous — no vague terms, one interpretation only
[ ] Complete    — no TBD/TBC, all conditions stated, no undefined references
[ ] Singular    — one "shall" per requirement
[ ] Feasible    — can be built within project constraints
[ ] Verifiable  — can write a T/A/I/D procedure with pass/fail criterion
[ ] Correct     — consistent with StRS and other SysReqs
[ ] Conforming  — follows template, active voice, units, "shall" language
```

**Acceptance rule:** All 9 must PASS. Any FAIL → requirement must be revised before acceptance.
