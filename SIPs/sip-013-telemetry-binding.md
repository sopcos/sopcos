# SOPCOS IMPROVEMENT PROPOSAL (SIP-013)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 013 |
| **Title** | Declared Reality Interface (DRI) & Telemetry Binding |
| **Status** | LIVING STANDARD (REVISED - VAULT READY) |
| **Type** | Standards Track (Interface) |
| **Author** | The Architect (HQ) |
| **Date** | 2026-01-05 |
| **Layer** | Hardware Abstraction / Metrology |
| **Requires** | SIP-003, SIP-009, SIP-014 |
| **Motto** | *"Translation is Interpretation. Interpretation is Risk."* |

---

## 1. Abstract
This specification defines the **"Telemetry Binding Manifest"**, a cryptographically signed artifact that maps abstract logical keys (used in SIP-001 Policy) to specific physical I/O sources (Registers, GPIOs).

With the integration of **SIP-014**, the Manifest is elevated to a **"First-Class Artifact."** It is stored in the Decentralized Vault, addressed via URNs, and managed via the L1 Lifecycle Registry. This ensures that the "Definition of Reality" is versioned, immutable, and revocable.

## 2. Motivation: The "Oracle Gap"
In a courtroom, a defense attorney might argue: *"Your logs say 'Temperature: 100', but how do we know the system was reading the correct sensor? Or that the calibration coefficient wasn't tampered with to hide the danger?"*

Sopcos solves this by treating the I/O Configuration not as a local setting, but as a **Signed Affidavit** stored in the Vault.
* **SIP-001** defines the Rule (*"If Temp > 100"*).
* **SIP-013** defines the Reality (*"Register 40001 * 0.5 = Temp"*).

## 3. Core Philosophy: The Metrological Attestation

### 3.1. "Truth is Physical; Data is an Assertion"
**Axiom:** SOPCOS is not a multimeter. It does not touch the physical wire. SOPCOS accepts a digital value solely because a liable human entity (The Metrologist/Integrator) has declared: *"I certify that this digital signal represents that physical reality."*

### 3.2. Translation is Risk
Every conversion (Analog to Digital, Raw to Scaled) is an interpretation. SIP-013 mandates that this interpretation chain must be explicit, visible, and signed.

## 4. The Artifact: Telemetry Binding Manifest (Schema v1.1)
The Manifest is a JSON document stored as a `.json` or `.sop` artifact in the SIP-014 Vault. It **MUST** adhere to the following schema structure:

```json
{
  "header": {
    "magic": "DRI_M",
    "version": "1.1",
    "type": "TELEMETRY_MANIFEST",
    "kid": "did:sopcos:integrator:factory-alpha#key-1" // The Metrologist
  },
  "meta": {
    "urn": "urn:sopcos:artifact:sha256:...", // Canonical Vault Identity
    "valid_from": "2026-01-01T00:00:00Z",
    "calibration_cert": "urn:sopcos:artifact:sha256:..." // Link to external Cert
  },
  "bindings": {
    "temperature_c": {
      "source": "modbus://192.168.1.10:502",
      "address": 40001,
      "type": "UINT16",
      "transform": "x * 0.1", // Scaling Factor
      "unit": "Celsius"
    },
    "pressure_psi": {
      "source": "mqtt://broker.local/sensors/p1",
      "json_path": "payload.pressure",
      "unit": "PSI"
    }
  },
  "signature": "base64_signature..."
}
```
## 4.1 (Binding Types)
**INPUT_TYPE_STREAM (Non-Deterministic Source)** Represents unstructured continuous data flows required for the Cold Path (Cognitive Analysis). Unlike Registers, these are not polled in the strict real-time loop but are sampled for the AI Agent.
* **Examples:** Audio Spectrum (/dev/audio0), CCTV Frame (rtsp://camera1), System Logs (/var/log/syslog).
* **Handling:** Data is buffered and passed to the SIP-015 Engine. It is **NOT** used for SIP-001 Logic evaluation to preserve determinism.

## 5. Vault Integration & Lifecycle (SIP-014)
Unlike static config files, SIP-013 Manifests have a lifecycle managed by the L1 Chain.

### 5.1. Registration (OP_ARTIFACT_REGISTER)
When a factory is commissioned, the Integrator uploads the Manifest to the Vault and registers its URN. This establishes the **"Baseline Reality."**

### 5.2. Recalibration (OP_ARTIFACT_SUPERSEDE)
Sensors drift over time. When a recalibration occurs:
1.  The Metrologist updates the transform factor.
2.  Signs a New Manifest (v2).
3.  Issues an `OP_ARTIFACT_SUPERSEDE` transaction on-chain (Old_URN -> New_URN).
4.  **Forensic Continuity:** Old logs remain verifiable against the v1 Manifest, while new logs use v2.

### 5.3. Revocation (OP_ARTIFACT_REVOKE)
If a sensor batch is recalled or a scaling formula is found to be dangerous:
1.  The Authority issues a `REVOKE` transaction for the specific Manifest URN.
2.  **Kill Switch:** Synapse nodes using this Manifest immediately **HALT** operations, forcing an update to a valid configuration.

## 6. Operational Semantics (Runtime Logic)

### 6.1. The Binding Handshake
When Synapse boots up, it resolves the policy's required keys against the active Manifest URN fetched from the Registry.
* **Validation:** If the Policy asks for `get_telemetry("flux_capacitor")` but the Manifest URN has no such binding, the kernel **PANICS** (Safe Fail).

### 6.2. Attestation Loop
When a Verdict is produced (SIP-003), the system attaches the Manifest Hash to the context.
* **Legal Meaning:** *"This decision was made using the reality definition signed by [Integrator_DID] at [Timestamp]."*

## 7. Conclusion
SIP-013 completes the **"Liability Triangulation"**:
1.  **SIP-001 (Logic):** Who defined the Rules?
2.  **SIP-006 (Agency):** Who took the Action?
3.  **SIP-013 (Reality):** Who defined the Inputs?

By moving Manifests into the **SIP-014 Vault**, Sopcos ensures that "Reality" itself is an immutable, versioned, and auditable artifact.

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*