# SOPCOS IMPROVEMENT PROPOSAL (SIP-004)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 004 |
| **Title** | Threat Model & Accountability Framework |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Informational / Standard |
| **Author** | Maestro & Nexus (The Foundation) |
| **Created** | 2025-12-24 |
| **Last Revised** | 2026-01-05 |
| **Layer** | Governance / Security |
| **Related** | SIP-005, SIP-006, SIP-011 |
| **License** | CC0-1.0 (Public Domain) |

## 1. Abstract
This document defines the Sopcos Threat Model, explicitly acknowledging that cryptographic validity does not equate to truthfulness. It establishes the **"Accountability Framework"** to adjudicate disputes in scenarios involving malicious actors, broken hardware, or flawed policies.

## 2. The Core Axiom: "Evil but Compliant"
**"A Synapse node may be malicious yet protocol-compliant."**

This axiom asserts that a Gateway can technically follow all protocol rules (valid signature, valid hash, valid format) while simultaneously lying about the physical reality. Sopcos cannot prevent lies; it ensures that lies are recorded, signed, and legally actionable.

## 3. Threat Scenarios (Caselaw)

### Scenario A: The "Lying Gateway" (Fraud vs. Override)
**Context:** The factory temperature is 200°C (Fire). The Gateway Operator wants to maintain production.
**Action:** Instead of using the legitimate Override Mechanism (SIP-006) which creates a visible Liability Record, the operator manually hacks the input driver to report "50°C".

* **The Crime:** Falsifying the Input (The Oracle Problem) & Evasion of Audit.
* **Blame:** The Gateway Operator or Node Owner.
* **Proof:**
    1.  **Physical Evidence:** Burnt machine (Reality).
    2.  **Digital Evidence:** Signed decision on Axon based on "50°C".
    3.  **Forensic Conclusion:** The lack of a SIP-006 Override Token implies the operator intentionally sought to hide the intervention, proving **Malice (Bad Faith)** rather than operational necessity.
* **Punishment:**
    * **Protocol Level:** Certificate Revocation (SIP-002).
    * **Legal Level:** Admissible evidence of fraud.

### Scenario B: The "Garbage In, Garbage Out" (Honest Gateway, Broken Sensor)
**Context:** A sensor malfunctions and sends 0.0 value continuously. The Synapse Gateway receives 0.0, checks the rule `IF < 100 ALLOW`, and permits operation. The machine freezes/breaks.

* **The Issue:** Faithful execution of bad data.
* **Blame:** The Sensor Manufacturer or Maintenance Team.
* **Proof:**
    * The `input_hash` on Axon matches the raw log of the 0.0 signal.
    * **Verdict:** Synapse is innocent. It acted as a faithful mirror of the sensor.

### Scenario C: The "Lawful Evil" (Negligence vs. Intent)
**Context:** A Policy Author writes a rule: `IF Temp > 1000 THEN ALLOW` (Typo: meant 100). The machine reaches 500°C and explodes.

* **The Issue:** Correct execution of a disastrous instruction.
* **Blame:** The Policy Author.
* **Proof (Enhanced by SIP-005):**
    * The `policy_hash` matches the flawed version.
    * **Crucial Evidence:** The **Simulation Audit Log (SIP-005)** is queried.
        * **If Log shows `PREDICTION_CRITICAL`:** The Author saw the warning that 500°C would be allowed and signed it anyway. This upgrades the charge from "Negligence" to **"Willful Misconduct."**
* **Punishment:**
    * **Protocol Level:** None (Protocol worked).
    * **Legal Level:** High-liability lawsuit backed by **Proof of Foreknowledge**.

## 4. Conclusion: The Scope of Sopcos
Sopcos guarantees **Integrity** (The record hasn't changed) and **Authenticity** (Who signed it). While it does not strictly guarantee **Truth** (Is the input real?) or **Wisdom** (Is the rule smart?), it provides the tools (**SIP-005 Simulation** and **SIP-006 Override**) to document the lack thereof.

By strictly defining these boundaries, Sopcos shifts the burden of "Truth" to the edge (Sensor/Operator) and the burden of "Wisdom" to the core (Author), remaining a neutral, immutable witness to history.