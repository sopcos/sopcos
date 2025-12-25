# SIP-Appendix-A: Canonical Incident Walkthrough ("The Legal Shield")

| Metadata | Value |
| :--- | :--- |
| **SIP** | Appendix A |
| **Title** | Canonical Incident Walkthrough: "The Legal Shield" |
| **Status** | Informational / Non-Normative |
| **Related SIPs** | [SIP-003](./SIP-003.md), [SIP-005](./SIP-005.md), [SIP-006](./SIP-006.md) |
| **Maintainer** | The Sopcos Foundation |

## Abstract
This document provides a canonical walkthrough of a critical incident lifecycle within the Sopcos Protocol. It serves as a non-normative example to illustrate the separation of powers between the **PDP** (Policy Decision Point) and **PEP** (Policy Enforcement Point), the semantics of the **Override Certificate**, and the immutable nature of the **Dirty State**.

> **Note:** This walkthrough is intended for system architects, legal auditors, and forensic analysts to understand the "Chain of Custody" for digital liability.

---

## The Scenario: "Asset Protection vs. Protocol Safety"

**Context:** A critical industrial node (e.g., Cold Storage Unit or Reactor Cooling Pump) controlled by a Sopcos-compliant Gateway.

### Phase 1: Separation of Powers (The Routine)
* **Trigger:** Telemetry indicates a deviation within safe operating limits but approaching a threshold.
* **Process (SIP-003 Compliance):**
    * **PDP (Synapse):** Evaluates Policy against current input. Verdict: `ALLOW`.
    * **PEP (Controller):** Receives the cryptographically signed verdict. Executes the operation.
* **Constitutional Principle:** The Controller does not act on its own volition; it acts solely on verified verdicts provided by the PDP.

### Phase 2: The Verdict (The Incident)
* **Trigger:** Telemetry breaches the safety threshold (e.g., `Temp > Limit`).
* **Process:**
    * **PDP (Synapse):** Evaluates Policy. Verdict: `DENY` (Reason: Safety Violation).
    * **PEP (Controller):** Validates the `DENY` signature. Mechanically disengages the power/actuator.
* **Legal Standing:** The Gateway did not "cut the power." It issued a verdict that the operation was unsafe. The Controller executed its safety mandate based on that verdict.

### Phase 3: The Forecast (SIP-005 Simulation)
* **Action:** Operator requests a simulation of a forced restart under current fault conditions to assess impact.
* **Input (Virtual Context):**
    ```json
    { "virtual_context": { "state": "FAULT_ACTIVE", "action": "FORCE_RUN" } }
    ```
* **Output:**
    * **Verdict:** `DENY`
    * **Blast Radius:** `Class 2 (Fire/Radiation Risk)`
* **Significance:** The system provides foresight, preventing the future legal defense of "I didn't know the consequences."

### Phase 4: The Confession (SIP-006 Override)
* **Action:** Operator initiates the **System Override Protocol**.
* **Parameters:**
    * **Scope:** `DeviceID:88` (Targeted intervention).
    * **TTL:** `10 Minutes` (Temporal limitation).
    * **Justification:** `ASSET_PROTECTION`.
* **The Act:** Operator cryptographically signs the **Liability Acceptance Statement**.
* **Result:** Generation of a valid **Override Certificate**.

### Phase 5: The Witness & The Execution (Dirty State)
* **State Transition:** The Synapse node transitions from `HEALTHY` to `DIRTY_STATE`. It ceases to be the "Judge" and becomes the "Witness."
* **Execution (PEP):**
    * The Controller receives the command to restart alongside the certificate.
    * **Validation:** The Controller verifies the cryptographic signature of the **Override Certificate**.
    * **Logic:** *"Valid Override Certificate verified. Authorizer: Operator [ID]. Execution: FORCE_START."*
* **Immutable Record (Axon):** The certificate, timestamp, and state transition are anchored to the Sopcos Chain as a permanent transaction.

### Phase 6: The Audit (Return to Innocence)
* **Action:** Operator attempts to transition the node back to `CLEAN_STATE`.
* **Denial:** Synapse rejects the transition.
    * **Error:** `E_CANNOT_SELF_CLEAR_DIRTY_STATE`.
    * **Principle:** The actor cannot clear the evidence of their own action.
* **Resolution:**
    * An accredited **Auditor** reviews the Axon logs and the outcome.
    * Auditor signs a `STATE_CLEARANCE` transaction.
    * Synapse validates the Auditor's signature and transitions to `HEALTHY`.

---

## Jurisprudence Motto
> *"This system does not judge bad decisions; it judges bad intentions."*
>
> — *Sopcos Protocol Foundation*