# SOPCOS IMPROVEMENT PROPOSAL (SIP-005)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 005 |
| **Title** | Policy Simulation & What-If Semantics |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Standards Track (Core) |
| **Author** | Maestro & Nexus (The Foundation) |
| **Created** | 2025-12-24 |
| **Last Revised** | 2026-01-05 |
| **Layer** | Authoring / Runtime |
| **Related** | SIP-002, SIP-006, SIP-008 |
| **License** | CC0-1.0 (Public Domain) |

## 1. Abstract
This proposal integrates a **"Deterministic Simulation Engine"** into the Sopcos ecosystem. Its purpose is to prevent "Lawful Evil" policies from entering the production environment by transforming human error into actionable foresight.

It establishes Sopcos as a **"Witness with Foreknowledge,"** ensuring that ignorance of a policy's consequences is no longer a valid defense.

## 2. Simulation Semantics
Sopcos Synapse now operates in two distinct modes.

### 2.1. Execution Mode (Binding)
* **Target:** Physical Reality.
* **Output:** Signed Decision (`Verdict`) on Canonical Axon.
* **Effect:** Actuation or Prohibition.

### 2.2. Simulation Mode (Advisory)
* **Target:** Virtual Sandbox.
* **Output:** `SimulatedDecision` object logged in the Simulation Audit Trail.
* **Effect:** No physical change, but generates **"Proof of Foreknowledge."**

## 3. Determinism & Versioning Guarantee
To ensure that a simulation run today is valid evidence 5 years from now, the simulation environment must be strictly versioned and deterministic (part of the **Immutable Kernel** per SIP-002).

### 3.1. The Engine Hash
Every simulation record must include the specific fingerprint of the logic engine used.
* **Legal Value:** This prevents the defense: *"The simulator back then was different/buggy."*

## 4. Blast Radius & Human Impact
The impact analysis is expanded beyond devices to include semantic human safety metrics.

### 4.1. Impact Metrics
1.  **Device Spread:** Number of affected IoT nodes.
2.  **Safety Zones:** Criticality of the zone (e.g., Office vs. High Voltage Line).
3.  **Human Proximity Class:**
    * **CLASS_0:** No humans expected (Fully Autonomous Zone).
    * **CLASS_1:** Maintenance personnel only.
    * **CLASS_2:** High human traffic (Assembly line).

## 5. Governance: Risk-Based Approval
Approval requirements scale with the calculated Blast Radius.

**Constraint:** The Risk-Based requirement serves as a **minimum floor**. It creates an intersection with SIP-008. If Simulation Risk demands higher scrutiny than the Signer's Authority Level suggests, the Risk Requirement prevails.

| Risk Level | Trigger | Requirement |
| :--- | :--- | :--- |
| **LOW** | Minor config, Class 0 Zone | Single Signature (Admin) |
| **MEDIUM** | Critical Config, Class 1 Zone | Dual Signature (Four-Eyes) |
| **HIGH** | Global Shutdown, Class 2 Zone | Consensus (Admin + CISO + Operations) |

### 5.1. The "Emergency Deployment" Clause (Governance Bypass)
*(Renamed from "Emergency Override" to avoid conflict with SIP-006)*

In catastrophic scenarios where time is more critical than consensus (e.g., imminent explosion), strict governance can be bypassed.

* **Mechanism:** An Admin can force-deploy a High-Risk policy with a Single Signature using the `FORCE_DEPLOY` flag.
* **Consequence:** This action immediately generates a `CRITICAL_FOREKNOWLEDGE` log entry.
* **Meaning:** *"I acknowledge the high risk and the system's warning, but I am taking full personal liability to prevent a greater disaster."*

## 6. Audit Semantics: The Foreknowledge Tier
Simulation logs are classified to prevent "alert fatigue" and clarify legal liability.

### 6.1. Tier 1: PREDICTION_ADVISORY
* **Meaning:** "The system detects potential side effects or minor inefficiencies."
* **Liability:** Negligible. Ignored warnings are considered "acceptable operational trade-offs."

### 6.2. Tier 2: PREDICTION_CRITICAL (The Nightmare Log)
* **Meaning:** "The system calculates a >90% probability of device failure, safety violation, or production halt."
* **Liability:** **Absolute.**
* **Scenario:** If an operator sees a `PREDICTION_CRITICAL` warning and uses `FORCE_DEPLOY`, and an accident occurs, this log serves as proof of **"Willful Negligence"** or **"Intent."**
* **Defense Nullified:** The operator cannot claim *"I didn't know it would happen."*

## 7. Conclusion
With SIP-005, Sopcos evolves from a passive ledger to an active **"Co-Pilot."** It does not stop a human from making a mistake (or a heroic emergency deployment), but it ensures that the intent and knowledge at the moment of decision are preserved forever.