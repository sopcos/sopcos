# SOPCOS IMPROVEMENT PROPOSAL (SIP-009)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 009 |
| **Title** | Cryptographic Policy Envelope (SOP Format) |
| **Status** | RATIFIED |
| **Type** | Standards Track (Interface) |
| **Author** | The Architect (HQ) |
| **Date** | 2025-12-30 |
| **Layer** | Authoring, Axon, Synapse |
| **Related** | SIP-006, SIP-008 |

## 1. Abstract
This specification defines the **Sopcos Object Package (.sop)**, a cryptographically signed policy artifact format used for the distribution, verification, and execution of Sopcos Policy Code (WASM).

A `.sop` file is a **Policy Artifact**, not the policy itself. It is **never executable by default**. Execution is permitted **IFF** (if and only if) the artifact's hash is anchored and active on the Sopcos Chain (L1) via `OP_POLICY_ANCHOR`.

## 2. Motivation
Raw WASM binaries lack identity, provenance, authority binding, and temporal intent. In a governance-grade system, executable logic must be:

* **Authenticated:** Who wrote it?
* **Attributed:** Under what authority?
* **Immutable:** Has it changed?
* **Contextual:** Where and when is it valid?

The SOP format creates a **Cryptographic Envelope** that binds policy logic with identity, scope, authority, and intent, while remaining compatible with deterministic compilation and reproducible builds.

## 3. Conceptual Model
* **Policies are Laws.**
* **WASM Binaries are Artifacts.**
* **SOP is the Sealed Lawbook.**

A `.sop` package:
* Can be stored off-chain (IPFS, Vault, Object Storage).
* MUST be signed by an authorized issuer.
* MUST be hash-identical to the artifact anchored on L1.
* MUST be delivered to Synapse only via a verified Policy Envelope.

## 4. SOP Envelope Schema (Canonical)

All SOP files must conform to the following standard JSON schema:

```json
{
  "format": "sop/1.0",
  "header": {
    "issuer": "did:sopcos:org/siteA",
    "signer": "did:key:z6Mk...",
    "alg": "Ed25519",
    "kid": "key-1",
    "iat": 1700000000,
    "nbf": 1700000000,
    "exp": null
  },
  "payload": {
    "policy_did": "did:sopcos:policy/siteA/boiler/safety",
    "authority_level": 0,
    "scope": "siteA/boiler_room/*",
    "semantic_version": "1.2.0",
    "wasm": "<BASE64_WASM_BYTES>"
  },
  "signature": "<HEX_SIGNATURE>"
}
```
## 5. Cryptographic Binding Rules

### 5.1. Signing Scope (Normative)
The signature MUST be calculated over the canonical serialization of:
1.  `format`
2.  `header`
3.  `payload`

This guarantees that **identity, authority, scope, version, and executable logic** are inseparable.

### 5.2. Algorithms
* **Signature Algorithm:** Ed25519 (mandatory).
* **Hash Algorithm:** SHA-256 (for artifact identity).

## 6. Identity & Authority Resolution
* `signer`: Represents the cryptographic key holder.
* `issuer`: Represents the **Policy Authority**.

**Verification Rule:**
The DID matching the `policy_did` MUST match the controller registered on L1.
A valid signature is **necessary but not sufficient** for execution.

## 7. Lifecycle & Validity Semantics

### 7.1. SOP vs. L1 Authority
Time fields (`iat`, `nbf`, `exp`) express only the **author's intent**.
The **effective validity** of a policy is determined solely by:
1.  L1 Anchor State (`ACTIVE` / `REVOKED`).
2.  L1 Validity Period.

**Rule:** If SOP time fields and L1 registry conflict, **L1 prevails.**

### 7.2. Revocation
Revoking a policy on L1 does not invalidate past verdicts rendered while it was active.

## 8. Delivery & Execution Model

### 8.1. Axon Responsibilities (Normative)
Axon **MUST**:
1.  Retrieve the SOP artifact from storage.
2.  Verify the signature and authority.
3.  Verify the artifact hash against the L1 registry.
4.  Verify scope compatibility.
5.  Construct a signed **PolicyEnvelope**.

Axon **MUST NOT**:
1.  Execute code.
2.  Interpret policy semantics.

### 8.2. Synapse Responsibilities (Normative)
Synapse **MUST**:
1.  Accept ONLY verified Policy Envelopes.
2.  Execute WASM in a hermetic environment.
3.  Reject any artifact without L1 linkage.

Synapse **MUST NOT**:
1.  Perform trust establishment tasks.
2.  Receive SOP artifacts directly.

## 9. Security Considerations
This specification mitigates:
* Replay attacks.
* Binary substitution.
* Unauthorized policy injection.
* Runtime compromise via artifact spoofing.

The SOP format alone does not guarantee security. It operates within the constitutional constraints defined by the Governance Specification and SIP-008.

## 10. Backward Compatibility
This is the first normative definition of the SOP format. No backward compatibility is guaranteed.

## 11. Conclusion
The `.sop` format transforms executable policy code into a **verifiable, attributable, and governable legal artifact.**

It is the final seal between **The Law (Policy)** and **The Execution (Runtime).**

---
*Copyright © 2025 Sopcos Protocol Foundation. All Rights Reserved.*