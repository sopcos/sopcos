# SOPCOS IMPROVEMENT PROPOSAL (SIP-003)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 003 |
| **Title** | Decision Semantics & Canonical State |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Standards Track (Core) |
| **Author** | Maestro & Nexus (The Foundation) |
| **Created** | 2025-12-24 |
| **Last Revised** | 2026-01-05 |
| **Layer** | Governance / Runtime |
| **Requires** | SIP-010, SIP-011 |
| **License** | CC0-1.0 (Public Domain) |

## 1. Abstract
This proposal defines the technical and legal definition of a **"Valid Decision"** within the Sopcos ecosystem. It establishes the atomic structure of a decision object using a **Composition Model**, capable of carrying both algorithmic verdicts and human interventions within a single schema.

This standard serves as the **"Source of Technical Truth"** for auditors, insurers, and judicial bodies.

## 2. The Decision Canon
For a data block to be considered a valid "Sopcos Decision" and committed to Axon, it must adhere to the following Atomic Structure.

### 2.1. Anatomy of a Valid Decision (Composition Schema)
The Decision Object is a hybrid structure acting as a container for either Algorithmic Logic or Human Liability. Any decision lacking these fields or failing cryptographic verification is considered **"NULL AND VOID."**

#### Verdict Vectors (Extended per SIP-010):
* **ALLOW:** Operation authorized.
* **DENY:** Operation strictly forbidden.
* **WARN:** Operation authorized (ALLOW logic) but flagged with specific risk codes. Liability shifts to the operator/system acknowledging the warning.
* **HALT:** Immediate cessation of operations requiring manual reset (Panic/Kill Switch).

#### Structural Composition
A valid decision **MUST** contain the following logical fields (represented here in pseudo-Go/JSON syntax):

```json
{
  "verdict": "ALLOW | DENY | WARN | HALT",
  "input_hash": "SHA256(Telemetry)",
  "gateway_signature": "Ed25519_Signature",
  
  // Algorithmic Provenance (Always Present)
  "policy_hash": "SHA256(SOP_Artifact)",
  
  // [NEW] Telemetry Provenance (The Physical Link)
  // This field links the logical data to the physical declaration defined in SIP-013.
  "binding_provenance": {
     "declaration_hash": "SHA256(Telemetry_Manifest_JSON)", 
     "sensor_timestamp": 1700000000
  },
  
  // Human Liability Composition (Nullable / Optional)
  // If this field is POPULATED, it acts as a 'Preemption' of the policy logic.
  "override": {
     "token": "Signed_Override_Token",
     "signer_did": "did:sopcos:operator:123",
     "justification_code": "CLASS_S_SAFETY",
     "liability_hash": "SHA256(Context + Intent)"
  } 
}
```

### 2.2. Validity Conditions
Validity is determined by the presence or absence of the `override` struct (Composition Logic):

#### Case A: Algorithmic Decision (Standard)
* **Condition:** `override` is **NIL / NULL**.
* **Validation:**
    1.  `policy_hash` MUST point to a valid, active Policy Artifact on Axon.
    2.  `verdict` MUST be the deterministic result of applying `policy_hash` logic to `input_hash`.

#### Case B: Human Intervention (Override)
* **Condition:** `override` is **POPULATED**.
* **Validation:**
    1.  The `override.token` MUST carry a valid signature from a registered DID.
    2.  The `override.justification_code` MUST be a valid enumeration from SIP-006.
    3.  **Note:** In this case, the verdict is forced by the human, effectively preempting the `policy_hash` logic. The `policy_hash` remains in the record for forensic comparison (*"What would the machine have done?"*).

## 3. The Liability Shield: Decision ≠ Action
This section defines the legal liability boundary of the Sopcos Protocol.

### 3.1. Definition of Roles
* **Sopcos (Synapse):** Acts as a **"Policy Decision Point (PDP)."**
* **Controller (PLC/SCADA):** Acts as a **"Policy Enforcement Point (PEP)."**

### 3.2. The Non-Action Clause
**"The Sopcos Protocol does not perform physical actions; it validates the legitimacy of intended actions."**

* **Incorrect Statement:** "Sopcos stopped the press machine."
* **Correct Statement:** "Sopcos **FORBADE (DENY)** the operation of the press machine. The execution of this prohibition (cutting power) is the responsibility of the Controller Software."

### 3.3. Legal Implication
If Sopcos issues a `DENY` verdict (signed and logged), but the machine continues to operate and causes an accident:
* **Liability:** Lies solely with the "Controller Software" (Integrator/Vendor) or the Operator who bypassed the system physically.
* **Proof:** The signed `DENY` record on Axon serves as immutable proof that the Protocol fulfilled its duty, exonerating the Foundation and the Protocol logic.

## 4. Audit Semantics
What guarantees does an Auditor or Court have when querying Axon?

### 4.1. Causality Guarantee
The system provides a mathematical proof of causality: *"At Time T, Decision Z was rendered based on [Rule Y OR Override O] triggered by Data X."* No link in this chain can be retroactively altered.

### 4.2. Non-Repudiation
A Gateway (or its Operator) cannot claim *"I didn't make that decision, the logs were tampered with."* Since the decision is signed by the Gateway's Private Key (or the Operator's Key in override cases), repudiation is mathematically impossible (equivalent to admitting key negligence).

### 4.3. Privacy by Design
Sopcos stores the `input_hash`, not the `raw_input`. This ensures that trade secrets or GDPR-sensitive data are not exposed on the Blockchain, while still allowing auditors to verify (by hashing their copy of the data) that the decision was correct.

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*