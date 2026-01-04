# SOPCOS IMPROVEMENT PROPOSAL (SIP-013)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 013 |
| **Title** | Declared Reality Interface (DRI) & Telemetry Binding |
| **Status** | DRAFT |
| **Type** | Standards Track (Interface) |
| **Author** | The Architect (HQ) |
| **Date** | 2026-01-05 |
| **Layer** | Hardware Abstraction / Metrology |
| **Motto** | Translation is Interpretation. Interpretation is Risk. |
| **Requires** | SIP-003, SIP-009, SIP-004 |

## 1. Abstract
This specification defines the **"Telemetry Binding Manifest"**, a cryptographically signed artifact that maps abstract logical keys (used in SIP-001 Policy) to specific physical I/O sources (Registers, GPIOs).

It introduces the **"Doctrine of Metrological Attestation,"** asserting that SOPCOS does not measure physical reality but records the *declared configuration* of the measurement chain. This ensures that the liability for "Lying Sensors" or "Malicious Scaling Factors" rests explicitly with the Integrator who signed the manifest, not the Protocol.

## 2. Motivation: The "Oracle Gap"
In a courtroom, a defense attorney might argue: *"Your logs say 'Temperature: 100', but how do we know the system was reading the correct sensor? Or that the calibration coefficient wasn't tampered with to hide the danger?"*

Without SIP-013, a policy executing `IF temp > 100` is legally vulnerable because `temp` is an abstract variable. SIP-013 closes this gap by treating the I/O Configuration as a signed affidavit. It answers the question: *"Who defined that Register 40001 represents 'Temperature'?"*

## 3. Core Philosophy: The Epistemic Boundary

### 3.1. "Truth is Physical; Data is an Assertion"
> **Axiom:** SOPCOS is not a multimeter. It does not touch the physical wire.

SOPCOS accepts a digital value solely because a liable human entity (The Metrologist) has declared: *"I certify that this digital signal represents that physical reality."*

### 3.2. Translation is Risk
Every conversion (Analog to Digital, Raw to Scaled) is an interpretation. SIP-013 mandates that this interpretation chain must be explicit, visible, and signed.

* **The System's Defense:** *"I did not measure the wrong value. I processed the value provided by the binding configuration signed by [Integrator_DID]."*

## 4. The Artifact: Telemetry Binding Manifest
This JSON structure acts as the "Driver Configuration" for the Synapse Node. It separates the Logical World (SIP-001) from the Physical World (PLC/Sensors).

```json
{
  "schema": "sip-013-v1",
  "scope_id": "boiler_room_1",
  "version": "1.0.4",
  "valid_from": 1700000000,
  "declared_by": "did:sopcos:engineer:metrology_01", // The Liable Integrator

  "bindings": {
    "temperature_core": {
      "logical_key": "temperature",             // Used in WASM
      "description": "Main Boiler Core Temp (PT100)",
      
      "physical_source": {                      // Where does it come from?
        "protocol": "MODBUS_TCP",
        "interface": "eth0",
        "address": "40001",
        "data_type": "UINT16",
        "poll_rate_ms": 100
      },

      "transform_chain": [                      // How is it interpreted?
        { "op": "SCALE", "value": 0.1 },        // Raw 1000 -> 100.0
        { "op": "OFFSET", "value": -10.0 }      // Calibration offset
      ],

      "metrology_proof": {                      // Why do we trust it?
        "sensor_sn": "PT100-XJ-99",
        "calibration_cert_ref": "ipfs://QmHash.../cert.pdf",
        "last_calibrated": 1690000000
      }
    }
  },
  "signature": "sig_ed25519_..."                // Integrator's Liability
}
```

## 5. Operational Semantics (Runtime Logic)

### 5.1. The Binding Handshake
When Synapse boots up:
1.  Loads the **SIP-001 Policy** (Logic).
2.  Loads the **SIP-013 Manifest** (Translation).
3.  **Validation:** Checks if every `get_telemetry("key")` call in the Policy has a corresponding entry in the Manifest. If not, the system halts (Configuration Mismatch).

### 5.2. Runtime Execution
When a WASM module calls `host_get_telemetry("temperature")`:
1.  **Read:** Synapse driver reads Modbus Address 40001.
2.  **Transform:** Applies the `transform_chain` (Scale/Offset).
3.  **Return:** Passes the final float value to WASM.
4.  **Attest:** (Crucial Step) The system calculates the `measurement_declaration_hash` (The hash of the active SIP-013 Manifest) and attaches it to the Decision Context (SIP-003).

## 6. Forensic Semantics: The Chain of Legitimacy
In a post-accident audit, the Verdict Record on Axon now proves:
1.  **The Decision:** `DENY`.
2.  **The Logic:** SIP-001 Policy Hash X.
3.  **The Reality Definition:** SIP-013 Manifest Hash Y.

### Scenarios:
* **Case A (Faulty Logic):** *"The temperature was 150, but the Policy didn't stop it."*
    * **Verdict:** Policy Author Liable.
* **Case B (Lying Sensor):** *"The temperature was 150, but the system saw 50."*
    * **Check:** Did Synapse execute the Manifest correctly?
    * **Yes:** Then the fault lies with the physical sensor or the scaling factor defined in the Manifest.
    * **Verdict:** Integrator (Metrologist) Liable.

## 7. Privacy & Performance Note
SOPCOS does **NOT** store the raw stream of Modbus registers on the blockchain. This would be inefficient and violate data privacy (Trade Secrets). Instead, it stores the **Manifest Hash** (The definition of truth) and anchors the **Input Hash** (The snapshot of values) at the moment of critical decisions.

## 8. Conclusion
SIP-013 completes the "liability triangulation":

1.  **SIP-001:** Who defined the Rules?
2.  **SIP-006:** Who took the Action?
3.  **SIP-013:** Who defined the Reality?

It ensures that no "Magic Numbers" exist in the system. Every input is a signed attestation.