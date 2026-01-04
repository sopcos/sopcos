# SOPCOS IMPROVEMENT PROPOSAL (SIP-009)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 009 |
| **Title** | Cryptographic Policy Envelope (SOP Artifact) |
| **Status** | LIVING STANDARD (REVISED - WASM EDITION) |
| **Type** | Standards Track (Interface) |
| **Author** | The Architect (HQ) |
| **Date** | 2026-01-05 |
| **Layer** | Tooling / Runtime |
| **Requires** | SIP-001 (Source), SIP-008 (Authority), SIP-014 (Vault) |
| **Related** | SIP-010 (Algebra), SIP-002 (Kernel) |

---

## 1. Abstract
This specification defines the **Sopcos Object Package (.sop)**, a cryptographically signed artifact containing hermetic, deterministic WebAssembly (WASM) bytecode.

To guarantee industrial safety and cross-platform consistency, Sopcos separates "Source Code" from "Executable Code":
1.  **Source:** Defined in **SIP-001** (JSON) for human auditability.
2.  **Artifact:** Defined in **SIP-009** (WASM) for machine determinism.

The SIP-009 Revision expands the concept of the "Envelope" to standardizing not just the content, but also physical storage hints and encryption parameters, making it fully compatible with the **SIP-014 Vault architecture**.

## 2. Motivation: The Hermetic Imperative
In safety-critical systems, relying on text-based interpretation (JSON Parsing) at runtime is dangerous. WASM provides a "Black Box" guarantee:

* **Write Once, Run Everywhere:** A policy compiled to WASM executes bit-by-bit identically on an ARM microcontroller and an x86 server.
* **Privacy & Availability:** With the introduction of SIP-014, the Envelope must now handle **Client-Side Encryption** (protecting trade secrets) and **Storage Hints** (locating the payload in a decentralized vault).

## 3. The Compilation & Packaging Pipeline
The creation of a valid SIP-009 Artifact follows a strict **"Chain of Custody"**:

1.  **Authoring:** The Policy Author defines logic in SIP-001 JSON.
2.  **Compilation:** The Sopcos Compiler (`scc`) verifies the JSON and compiles it into a non-Turing-complete WASM module.
3.  **Encryption (Optional):** If the policy contains proprietary logic, the author encrypts the WASM bytes using their local keys (AES-GCM).
4.  **Signing:** The Author calculates the hash of the Envelope (including metadata and encrypted blob) and signs it.
5.  **Publishing:** The signed artifact is uploaded to a SIP-014 Vault, and its URN is registered on the L1 Chain.

## 4. SOP Envelope Schema (v1.1)
The `.sop` file is a JSON container wrapping the binary payload. It **MUST** adhere to the following schema structure:

```json
{
  "header": {
    "magic": "SOPC",
    "version": "1.1",
    "type": "WASM_POLICY",
    "kid": "did:sopcos:author:123#key-1" // Signer Identity
  },
  "meta": {
    "source_hash": "sha256:...", // Hash of original SIP-001 JSON
    "compiler_version": "scc-v2.0.4",
    "authority_level": "LVL_3", // Declared Level (Verified via SIP-008)
    "urn": "urn:sopcos:artifact:sha256:..." // SIP-014 Canonical Identity
  },
  "encryption": {
    "enabled": true,
    "algo": "AES-256-GCM",
    "iv": "base64...", // Initialization Vector
    "recipient_context": "did:sopcos:group:factory-alpha" // Decryption Capability
  },
  "distribution": {
    "storage_hint": "s3://factory-local-vault/artifacts/" // Physical Location Hint
  },
  "payload": {
    "integrity": "sha256:...", // Hash of the (Encrypted) Blob
    "blob": "base64_encoded_bytes..." // The Asset
  },
  "signature": "base64_signature_of_canonical_json..."
}
```
## 5. Cryptographic Binding Rules

### 5.1. Signing Scope
The signature **MUST** be calculated over the canonical serialization of the JSON structure. Changing a single bit in the `payload.blob` or the `meta.urn` invalidates the signature.

### 5.2. URN Consistency
The `meta.urn` field must be mathematically derived from the `payload.integrity` hash. The Runtime must verify `Hash(Blob) == URN` before execution.

## 6. The Hermetic Sandbox (Synapse Runtime)
Synapse **MUST** execute the WASM payload in a restricted environment (Sandbox) defined as part of the Immutable Kernel.

### 6.1. Decryption Logic
Upon loading the Envelope:
* If `encryption.enabled` is true, Synapse uses its DID keys to decrypt the `payload.blob` in **volatile memory (RAM) only**.
* The decrypted bytecode is **NEVER** written to disk.

### 6.2. Import Restrictions (No Side Effects)
The WASM module **MUST NOT** import any functions except the **Sopcos Host Functions (SHF)**:
* `get_telemetry(key)`
* `get_context_var(key)`
* `emit_log(level, msg)`

**Prohibited:** Network access, File I/O, Random Number Generation, System Clock (Time must be injected via Context).

### 6.3. Resource Metering (Gas)
To prevent infinite loops, the Runtime **MUST** enforce strict instruction metering. If execution exceeds the limit, the Verdict defaults to **DENY (Fail-Safe)**.

## 7. Authority Resolution
The WASM binary does not contain the Authority Level. Authority is a property of the **Signer**. The runtime extracts the `kid` (Key ID) from the header and resolves the Authority Level from the L1 Registry as defined in SIP-008.

## 8. Delivery & Execution Model
1.  **Axon:** Verifies the Signature and checks the L1 Registry for `OP_ARTIFACT_REVOKE` or `OP_ARTIFACT_SUPERSEDE` status.
2.  **Synapse:**
    * Instantiates the WASM in the Sandbox.
    * Injects Input Telemetry (via SIP-013).
    * Invokes the `evaluate()` function.
    * Captures the returned Integer (0=DENY, 1=ALLOW, 2=WARN, 3=HALT).

## 9. Conclusion
SIP-009 bridges the gap between **Human Intent (JSON)** and **Machine Reality (WASM)**. It ensures that "The Law" is executed with mathematical precision, while the new Envelope structure ensures it is stored securely and permanently via SIP-014 Vaults.

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*