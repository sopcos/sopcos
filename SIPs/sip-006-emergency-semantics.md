# SOPCOS IMPROVEMENT PROPOSAL (SIP-006)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 006 |
| **Title** | Operational Intervention (Override) & Post-Mortem Semantics |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Standards Track (Core) |
| **Author** | Maestro & Nexus (The Foundation) |
| **Created** | 2025-12-24 |
| **Last Revised** | 2026-01-05 |
| **Layer** | Governance / Runtime |
| **Requires** | SIP-011, SIP-008 |
| **Related** | SIP-003, SIP-005 |
| **License** | CC0-1.0 (Public Domain) |

## 1. Abstract
This standard defines the semantics for human intervention in protocol decisions. It rejects the concept of "Bypass" or "God Mode," replacing it with **"Explicit Responsibility Transfer."**

It establishes that an Override is a signed **confession of liability** that creates a "Dirty State" (Compliance Contamination).

> **Note:** This SIP defines the *Process*. For the technical Data Structure (Override Token), refer to **SIP-011**.

## 2. The Definition of Override: "Confession Mode"
In Sopcos, overriding a rule is not "turning the system off." It is a specific transaction type called `TRANSFER_OF_LIABILITY`.

**The Axiom of Intervention:**
*"When a human overrides the protocol, the protocol ceases to be the 'Decision Maker' and becomes the 'Witness'. The human operator becomes the sole 'Owner' of the outcome."*

### 2.1. Structural Reference (SIP-011)
Implementation **MUST** adhere to the Composition Model defined in SIP-003 and the Override Token structure defined in SIP-011. The operator must declare their legal defense strategy (`justification_code`) at the moment of action via the signed token.

## 3. Constraints & The Pre-Law Limit
Override is powerful, but not omnipotent.

### 3.1. The Pre-Law Exclusion (SIP-008)
**CRITICAL:** An operator **CANNOT** override Level ∞ (Pre-Law) constraints defined in SIP-008.

* **Example:** If a firmware logic says "Hard Limit 5000 RPM (Physics)", sending a "Continuity Override" command will avail nothing. The physical safety interlock prevails.
* **Scope:** Overrides only suspend the **Normative Graph** (Level 0 - Level 4).

### 3.2. Dead-Man Switch (TTL)
An override cannot be infinite. It **MUST** have a Time-To-Live (TTL). If the session expires or connection is lost, the system **MUST** revert to `FAILSAFE_DEFAULT` (typically causing a `HALT` verdict per SIP-003).

## 4. Taxonomy of Override (Classification)
The system requires the operator to select a `justification_code` (as verified in SIP-011) from the following allowed values:

### 4.1. CLASS_S: SAFETY OVERRIDE (HUMAN_LIFE_RISK)
* **Intent:** "The rule stops the machine, but a human is trapped inside."
* **Audit Consequence:** High Scrutiny. Justified only by proof of immediate danger to life.
* **Notification:** Immediate alert to HSE (Health Safety Environment) Officer (Role defined in SIP-002).

### 4.2. CLASS_C: CONTINUITY OVERRIDE (CASCADE_DAMAGE)
* **Intent:** "Stopping production now causes cascading economic damage > risk cost."
* **Audit Consequence:** Economic Audit.
* **Notification:** Alert to Plant Manager & Insurer.

### 4.3. CLASS_F: FORENSIC OVERRIDE (SENSOR_DISPUTE)
* **Intent:** "I believe the sensors are lying (Scenario A in SIP-004)."
* **Audit Consequence:** Technical Review. If the system was correct, treated as incompetence.

## 5. The "Black Box" Protocol (Evidence Collection)
When an Override is active (`override` struct is populated per SIP-003), Synapse shifts from "Optimization Mode" to "Forensic Mode."

### 5.1. Cryptographic Binding
The system generates a **Forensic Snapshot (Context Hash)** that cryptographically locks the "Crime Scene" (Sensor Inputs) to the "Law" (Policy Hash) that was violated. This prevents *ex post facto* narrative fabrication.

## 6. Post-Mortem Semantics: "Compliance Contamination"

### 6.1. System Taint (DIRTY_STATE)
Once an Override is committed, the node enters a `DIRTY_STATE`.
* **Effect:** The node continues to function, but all subsequent telemetry/decisions are flagged as "Non-Compliant" or "Under Investigation."
* **Lock:** The node cannot return to `NORMAL_STATE` automatically.

### 6.2. Clearing the Debt (Reset Transaction)
To clear the taint, a "Compliance Debt" must be paid via a **Reset Transaction** on Axon.
* **Requirement:** This transaction requires a signature from a certified **Auditor** (separate DID from the Operator).
* **Payload:** Must reference the `OverrideToken` and include an Auditor Verdict (`JUSTIFIED` or `UNJUSTIFIED`).

## 7. The Prosecutor Test (Conclusion)
**Scenario:** A factory explosion occurs after an override.

**SIP-006 Proves:**
1.  **The Law:** "Policy Hash X explicitly forbade this."
2.  **The Act:** "Operator Y initiated `CONTINUITY_OVERRIDE` (Signed SIP-011 Token)."
3.  **The Defense:** "Operator Y claimed `CASCADE_DAMAGE`."
4.  **The Reality:** "Black Box data proves pressure was critical, contradicting the claim."

**Verdict:** The Operator's chosen justification contradicts the forensic reality. Guilty of **Willful Negligence**.