# SOPCOS IMPROVEMENT PROPOSAL (SIP-011)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 011 |
| **Title** | Forensic Liability & Operational Semantics |
| **Status** | RATIFIED |
| **Type** | Standards Track (Core) |
| **Author** | Sopcos Architect (Nexus) |
| **Date** | 2026-01-01 |
| **Layer** | Synapse (Runtime) / Governance |
| **Requires** | SIP-008, SIP-009, SIP-010 |

## 1. Abstract
This document defines the operational semantics for **Human Intervention (Overrides)** and the resulting **Forensic Liability Assignment** within the Sopcos Protocol.

The objective of this SIP is to treat human intervention NOT as part of the normative policy graph, but as an **Execution Preemption** at the runtime layer. Furthermore, it mandates that the outcome of such preemption be cryptographically bound to an immutable and auditable **"Liability Record"** (via structural composition).

## 2. Motivation
In autonomous industrial systems, absolute adherence to algorithmic policies can pose risks during edge cases. Human operators must retain the ultimate authority to intervene. However, traditional "manual mode" switches often sever the digital audit trail and obscure responsibility.

SIP-011 establishes human intervention as a first-class concept, ensuring that while the machine may yield control, the specific human liability is cryptographically recorded.

## 3. Design Principle: Execution Preemption & Structural Composition
**Human intervention is not a decision; it is an execution preemption.**

Therefore:
* **Non-Hierarchical:** An override is not part of the `AuthorityLevel` hierarchy (0-255). Negative values (e.g., -1) are strictly prohibited to ensure L1 compatibility.
* **Structural Composition:** Override data is not flattened into the verdict fields but carried as a distinct **Override Structure**. This separates "Policy Authorship" from "Operational Liability."

## 4. Input Schema: The Override Token
For a human override to be considered valid, the execution context (`FrozenContext`) MUST include a cryptographically signed `OverrideToken`.

```json
{
  "signer_did": "did:sopcos:operator/ahmet",
  "target_scope": "site/A/boiler_1",
  "timestamp": 1767291969,
  "reason": "Emergency pressure release",
  "context_hash": "sha256(frozen_context_bytes)",
  "signature": "base64_signature"
}
```
## 5. Operational Semantics
The Synapse Engine MUST strictly enforce the following execution order:

1.  **Existence Check:** Is there a valid `OverrideToken` in the execution context?
2.  **Validation:**
    * Verify signature against L1 Registry.
    * Ensure `target_scope == context.scope`.
    * Verify freshness (timestamp window).
    * Verify state binding (`context_hash`).
3.  **Preemption:** If the token is valid, **HALT** all normative policy evaluations (SIP-010 resolution is skipped).
4.  **Record Generation:** Produce an **Override Execution Record**.

## 6. Output Schema: The Liability Record
When an override occurs, the engine MUST produce a `Verdict` adhering to the following structure.

**CRITICAL:** The `AuthorityLevel` MUST remain within standard `uint8` bounds (specifically `0`).

```go
type Verdict struct {
    Decision        Decision
    
    // In case of an Override, this MUST be set to 0 (Safety Level).
    // Negative values or magic numbers are prohibited.
    AuthorityLevel  uint8
    
    // Indicates the winning mechanism (e.g., "OPERATOR_INTERVENTION")
    WinningPolicy   string

    // The Override Record.
    // If this field is NOT NIL, the system is under human intervention.
    // This is the sole source of Operational Liability.
    Override        *OverrideRecord `json:"override,omitempty"`
    
    // Forensic evidence of what the machine would have done.
    OriginalVerdicts []Verdict      `json:"original_verdicts,omitempty"`
}

type OverrideRecord struct {
    OperatorDID string `json:"operator_did"`
    Reason      string `json:"reason"`
    TokenHash   string `json:"token_hash"`
    Timestamp   int64  `json:"timestamp"`
}

## 7. Chain Validation Logic (L1)
To ensure data integrity on the immutable ledger, the L1 Chain (Axon/Smart Contract) MUST enforce the following validation rule:

> **IF** `transaction.Override` is NOT NULL
> **THEN** `transaction.AuthorityLevel` MUST BE `0`.

This rule mathematically guarantees that no "Magic Numbers" (like -1) corrupt the on-chain state, while preserving the liability data in the attached `Override` struct.

## 8. Security Considerations
* **Replay Attacks:** The `OverrideToken` must be strictly bound to both time and the `context_hash`.
* **Capability Control:** An operator's right to sign an override token should be constrained by the SIP-008 authority graph or delegation mechanisms.
* **Key Compromise:** The compromise of an operator's private key is equivalent to the physical compromise of the asset.

## 9. Conclusion
SIP-011 ensures that the Sopcos Protocol does not push human intervention outside the system boundaries, nor does it force it into the normative logic using invalid data types.

Through this separation, SOPCOS can cryptographically prove the statement:
> **"The machine made the calculation, but the human took the liability."**

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*