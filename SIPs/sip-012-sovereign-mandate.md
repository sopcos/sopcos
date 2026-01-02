# SOPCOS IMPROVEMENT PROPOSAL (SIP-012)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 012 |
| **Title** | The Sovereign Mandate & Delegated Agency |
| **Status** | PROPOSED STANDARD |
| **Type** | Standards Track (Core) |
| **Author** | The Architect (HQ) |
| **Date** | 2026-01-05 |
| **Layer** | Governance / Runtime |
| **Motto** | Responsibility cannot be destroyed, only delegated. |
| **Requires** | SIP-011, SIP-006, SIP-008, SIP-004 |

## 1. Abstract
This specification introduces the **"Sovereign Mandate" (MandateToken)** mechanism. It allows a Human Operator (*Sovereign*) to cryptographically delegate specific decision-making authority to a Machine (*Delegate*) for a future time window.

Crucially, this SIP establishes the **"Doctrine of Conservation of Liability,"** asserting that a machine can never hold liability. It acts solely as the **Executor** of a pre-signed human will. This standard solves the *"Monday Signature, Friday Accident"* liability gap.

## 2. Motivation: The "Inter-Temporal" Gap
SIP-011 defines liability for immediate physical overrides (Live Action). SIP-001 defines liability for static policy logic (Design).

However, a gap exists: *"What if the Operator is absent (offline) during a crisis, but wishes to authorize a risk-taking action in advance?"*

Without SIP-012, the machine must either:
1.  **Fail-Safe (Stop):** Potentially causing catastrophic passive damage.
2.  **Act Autonomously:** Creating a "Liability Vacuum" (Who pays if the AI errs?).

SIP-012 fills this gap by allowing **"Time-Shifted Liability."**

## 3. Core Philosophy & Axioms

### 3.1. The Innocence of the Machine
> **Axiom:** The Machine is not a Moral Agent. It is a Mirror.

A machine (Synapse) cannot be punished, sued, or jailed. Therefore, it cannot hold "Liability." It can only hold "Authority" derived from a human signature.

### 3.2. The Conservation of Liability
> **Axiom:** Liability implies the capacity to suffer consequences.

Since responsibility cannot be destroyed, any action taken by the machine must be traceable to a Human Signature (*The Source*).

* **No Unsigned Heroism:** A machine cannot take "Good Samaritan" initiative. If no Mandate exists, the machine **MUST** default to the safest state (SIP-004 Fail-Closed), even if inaction causes harm.

## 4. Data Specification: The MandateToken
The `MandateToken` is a signed JSON object acting as a "Digital Power of Attorney."

```json
{
  "schema": "sip-012-v1",
  "id": "uuid-v4",
  "issuer_did": "did:sopcos:operator:ahmet_88",   // The Liable Human (Sovereign)
  "delegate_did": "did:sopcos:synapse:node_01",   // The Executor Machine (Intuitu Personae)
  
  "constraints": {
    "valid_from": 1700000000,                      // Epoch Start
    "valid_until": 1700014400,                     // Epoch End (TTL)
    "max_offline_ttl": 600,                        // 10 Minutes (Grace Period)
    "scope": "boiler_room_1"                       // Spatial Constraint
  },

  "trigger_condition": {                           // The "If"
    "telemetry_key": "pressure",
    "operator": "GREATER_THAN",
    "value": 250
  },

  "authorized_action": {                           // The "Then"
    "verdict": "ALLOW",
    "justification_code": "CLASS_C"                // Continuity Override
  },

  "liability_acceptance": true,                    // Explicit Legal Waiver
  "signature": "sig_ed25519_..."                   // Issuer's Signature
}
```

### 4.1. Device Binding (Intuitu Personae)
The `delegate_did` **MUST** target a specific physical Node DID.
* **Rationale:** Authority is granted based on trust in a specific hardware's integrity (TPM/Security).
* **Replacement:** If `Node_01` fails and is replaced by `Node_02`, the Mandate is **VOID**. The Operator must sign a new Mandate for the new device. *"Trust follows the specific machine, not the location."*

## 5. Operational Semantics (Execution Logic)

### 5.1. The Connectivity Constraint (Anti-Zombie Rule)
To prevent "Zombie Mandates" (revoked rights used by offline nodes), the following liveness check is mandatory:

1.  **Online State:** If Synapse is connected to L1 Axon, it verifies the Mandate against the Revocation List (CRL). If valid, execute.
2.  **Offline State:** If Synapse loses connection:
    * It starts a countdown defined by `max_offline_ttl` (e.g., 10 mins).
    * **Within TTL:** Mandate is **VALID** (Grace Period).
    * **After TTL:** Mandate is **VOID**. System reverts to Fail-Safe.
    * **Rationale:** An offline node cannot know if the Operator has been fired or the key revoked. Indefinite offline authority is a security breach.

### 5.2. The Execution Stack (Revised Hierarchy)
When a decision is required, Synapse evaluates inputs in this strict order:

1.  **Level ∞ (Physics/Pre-Law):** [SIP-008] - Immutable.
2.  **Live Override (SIP-011):** [Active Human] - Supreme over Logic.
3.  **Sovereign Mandate (SIP-012):** [Delegated Human] - Conditional Authority.
4.  **Standard Policy (SIP-001):** [Algorithmic Default] - The Baseline.

## 6. Forensic Proof: The "Monday-Friday" Protocol
This mechanism resolves the temporal displacement of liability.



* **T-100 (Monday):** Operator signs the `MandateToken`.
    * *Legal State:* "Potential Liability" anchored on Chain.
* **T-0 (Wednesday):** Incident occurs. Operator is absent. Machine executes Mandate.
    * *Machine Log:* "Action: ALLOW. Authority: SIP-012 Mandate [ID]. Executor: Synapse."
* **T+48 (Friday):** Court Investigation.
    * *Defense:* "I was not there on Wednesday."
    * *Sopcos Verdict:* "Correct. But on Monday, you signed the specific instruction for this exact condition. The machine acted as your agent. Liability rests with the Monday-Signer."

## 7. Revocation & Lifecycle
* **Active Revocation:** The Issuer (or Admin) sends an `OP_REVOKE_MANDATE` transaction to L1 Axon.
* **Passive Revocation:** `valid_until` timestamp expires.
* **Security Revocation:** `max_offline_ttl` expires (Heartbeat loss).

In all cases, a revoked mandate behaves as if it never existed. The machine returns to its default programming.