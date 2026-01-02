# SOPCOS IMPROVEMENT PROPOSAL (SIP-011)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 011 |
| **Title** | Forensic Liability & Operational Semantics |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Standards Track (Core) |
| **Author** | Sopcos Architect (Nexus) |
| **Date** | 2026-01-05 |
| **Layer** | Synapse (Runtime) / Governance |
| **Requires** | SIP-008, SIP-009, SIP-010, SIP-006 |

## 1. Abstract
This document defines the operational semantics for **Human Intervention (Overrides)** and the resulting **Forensic Liability Assignment**. It treats human intervention NOT as a request for permission, but as an **Execution Preemption**.

It codifies the philosophy:
> "The System never rejects a valid Human Override regarding Policy, but it cannot enforce an Override against Physics."

## 2. Motivation
Absolute adherence to algorithmic policies is risky. SIP-011 establishes that while the machine yields control to human will, the specific liability is cryptographically recorded. It distinguishes between **Normative Law** (Software Rules) which is overridable, and **Pre-Law** (Physical Limits) which is immutable.

## 3. Design Principle: Execution Preemption
Human intervention is not a decision; it is a **Command of Will**.

* **Structural Composition:** Override data is carried as a distinct structure, separating "Policy Authorship" from "Operational Liability".

## 4. Input Schema: The Override Token
*(Refer to SIP-003 and Source Definition for the JSON Schema. The token must contain the Signer DID, Target Scope, Justification Code, and Context Hash.)*

## 5. Operational Semantics (The Sopcos Ring Logic)
The Synapse Engine **MUST** strictly enforce the following execution order:



### Step 1: Existence Check
Is there a valid `OverrideToken` in the execution context?

### Step 2: Validation
* Verify signature against L1 Registry.
* Verify scope and freshness.
* Verify `justification_code` matches SIP-006 Enums.

### Step 3: Preemption & The Reality Check (CRITICAL UPDATE)
If the token is valid:

1.  **Suspend Policy:** **HALT** all normative policy evaluations (Levels 0-4). The machine's opinion on "Rules" is silenced.
2.  **Respect Physics (The Pre-Law Exception):** Check against Level ∞ (Pre-Law) constraints defined in SIP-008.
    * **Logic:** If the Override demands an action that violates a Hard-Coded Firmware Limit (e.g., RPM > Physical Max), the Engine **MUST** fail-safe or clamp the value to the physical limit.
    * **Philosophy:** *"The system accepts the Liability (Override), but acknowledges the Impossibility (Physics)."*

### Step 4: Record Generation
Produce an **Override Execution Record**.
* The Verdict reflects the Override (unless physically blocked).
* The Liability is fully assigned to the Signer.

## 6. Output Schema: The Liability Record
When an override occurs, the engine **MUST** produce a Verdict where:
* The `AuthorityLevel` **MUST BE** `0` (Emergency).
* The `Override` struct contains the signed token.

## 7. Chain Validation Logic (L1)
```text
IF transaction.Override is NOT NULL 
THEN transaction.AuthorityLevel MUST BE 0
```

## 8. Conclusion
SIP-011 ensures that Sopcos respects the hierarchy of reality:

1.  **Physics (Pre-Law):** Absolute Ruler.
2.  **Human (Override):** Sovereign over Software.
3.  **Algorithm (Policy):** Advisor to the Human.

Through this, we prove:
**"The machine calculated the limit, the Human demanded the breach, and Physics delivered the verdict."**

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*