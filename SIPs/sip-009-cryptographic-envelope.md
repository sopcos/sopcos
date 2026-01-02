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
| **Requires** | SIP-001 (Source), SIP-008 (Authority) |
| **Related** | SIP-010 (Algebra), SIP-002 (Kernel) |

## 1. Abstract
This specification defines the **Sopcos Object Package (.sop)**, a cryptographically signed artifact containing hermetic, deterministic **WebAssembly (WASM)** bytecode.

To guarantee industrial safety and cross-platform consistency, Sopcos separates "Source Code" from "Executable Code":
1.  **Source:** Defined in **SIP-001 (JSON)** for human auditability.
2.  **Artifact:** Defined in **SIP-009 (WASM)** for machine determinism.

A `.sop` file is the compiled, immutable representation of intent, executed within a strict sandbox to prevent "Parser Drift" or environment-specific behaviors.

## 2. Motivation: The Hermetic Imperative
In safety-critical systems, relying on text-based interpretation (JSON Parsing) at runtime is dangerous due to minute differences in library implementations (e.g., Float handling in Rust vs. Go). WASM provides a **"Black Box"** guarantee:

* **Write Once, Run Everywhere:** A policy compiled to WASM executes bit-by-bit identically on an ARM microcontroller and an x86 server.
* **No Ambiguity:** The Runtime does not "interpret" intent; it executes instructions.

## 3. The Compilation Pipeline (Chain of Custody)
The creation of a valid SIP-009 Artifact follows a strict "Chain of Custody":

1.  **Authoring:** The Policy Author defines logic in SIP-001 JSON.
2.  **Compilation:** The Sopcos Compiler (`scc`) verifies the JSON against the schema and compiles it into a linear, non-Turing-complete WASM module.
3.  **Signing:** The Author calculates the hash of the WASM Bytecode and signs it, creating the SOP Envelope.
4.  **Distribution:** The signed SOP is anchored on Axon.

> **Legal Note:** The signature attests to the WASM. Since the WASM is deterministically generated from the JSON, the signature implicitly covers the Source Intent.

## 4. SOP Envelope Schema
The `.sop` file is a JSON container wrapping the binary payload.

```json
{
  "header": {
    "alg": "EdDSA",
    "typ": "SOP+WASM",
    "kid": "did:sopcos:admin:001#key-1",
    "compiler_version": "v1.2.4 (sha256:...)"
  },
  "payload": {
    "format": "wasm",
    "bytecode": "base64_encoded_wasm_binary...",
    "source_hash": "sha256(original_sip001_json)"
  },
  "signature": "base64url_encoded_signature"
}
```

## 5. Cryptographic Binding Rules

### 5.1. Signing Scope
The signature **MUST** be calculated over the canonical serialization of the payload object. Changing a single bit in the WASM bytecode invalidates the signature.

### 5.2. Source Linkage
The `source_hash` field allows auditors to verify that the WASM matches the original JSON.
* **Verification:** An auditor can take the claimed JSON, run it through the specific `compiler_version`, and verify that the resulting bytecode matches the `payload.bytecode`.

## 6. The Hermetic Sandbox (Synapse Runtime)
Synapse **MUST** execute the WASM payload in a restricted environment (Sandbox) defined as part of the Immutable Kernel (SIP-002).

### 6.1. Import Restrictions (No Side Effects)
The WASM module **MUST NOT** import any functions except the **Sopcos Host Functions (SHF)**:
* `get_telemetry(key)`
* `get_context_var(key)`
* `emit_log(level, msg)`

**Prohibited:** Network access, File I/O, Random Number Generation, System Clock (Time must be injected via Context).

### 6.2. Resource Metering (Gas)
To prevent infinite loops (even though the Compiler attempts to prevent them), the Runtime **MUST** enforce strict instruction metering (Gas Limit). If execution exceeds the limit, the Verdict defaults to `DENY` (Fail-Safe).

## 7. Authority Resolution
The runtime extracts the `kid` (Key ID) from the header and resolves the **Authority Level** from the L1 Registry as defined in **SIP-008**.

* *Clarification:* The WASM binary does not contain the Authority Level. Authority is a property of the Signer, not the Code.

## 8. Delivery & Execution Model

### 8.1. Axon Responsibilities
Axon verifies the Signature and the L1 Anchor. It does not execute the code.

### 8.2. Synapse Responsibilities
Synapse receives the Verified Envelope:
1.  Instantiates the WASM module in the Sandbox.
2.  Injects Input Telemetry and Context.
3.  Invokes the `evaluate()` exported function.
4.  Captures the returned Integer (`0=DENY`, `1=ALLOW`, `2=WARN`, `3=HALT`).

## 9. Security Considerations
* **Compiler Trust:** The security of the system relies on the correctness of the Sopcos Compiler. The Compiler is open-source and part of the audited Kernel.
* **Opaque Binaries:** While WASM is binary, the mandatory `source_hash` ensures it can always be decompiled or recompiled for verification against the SIP-001 Source.

## 10. Conclusion
SIP-009 bridges the gap between Human Intent (JSON) and Machine Reality (WASM). It ensures that "The Law" is executed with mathematical precision, unaffected by the vagaries of operating systems or parser libraries.