# SIP-021: Organizational Access Control for Native IDAS (ABAC)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 021 |
| **Title** | Organizational Access Control for Native IDAS (ABAC) |
| **Status** | DRAFT |
| **Type** | Standards Track (Core) |
| **Category** | Core / Gateway / Authorization |
| **Author** | Sopcos Core Developers |
| **Created** | 2026-01-30 |
| **Requires** | SIP-016 (IDAS), SIP-002 (Roles), SIP-008 (Authority) |
| **Motto** | "Ownership is on-chain. Authority is contextual." |

---

## 1. Abstract

This proposal defines an Attribute-Based Access Control (ABAC) architecture for Industrial Digital Assets (IDAS). It shifts the authorization paradigm from "User-Centric ACLs" to "Organization-Centric Attributes."

Under SIP-021, an asset is owned by an Organization DID. Access authority is dynamically computed by the Gateway based on the intersection of Ledger Attributes (User Role + Org Match) and Anchored Policy. This enables O(1) scalability for workforce management while preserving the L1 Ledger as the immutable Source of Truth.

---

## 2. Motivation

The native IDAS standard (SIP-016) assigns asset ownership to a specific DID. In industrial operations, this creates critical friction:

1.  **The Shift Change Problem:** If Operator A owns the machine, Operator B cannot operate it without an on-chain transfer transaction.
2.  **The M&A Problem:** If Factory A is sold to Factory B, thousands of assets must be individually transferred, and thousands of user permissions revoked.
3.  **The "Chain Bloat" Risk:** Recording every "Door Open" event as an L1 transaction creates unsustainable latency and gas costs.

SIP-021 resolves this by separating Ownership (On-Chain) from Authority (Contextual).

---

## 3. Core Principles & Constraints

### 3.1. The Parsing Ban (Security Constraint)

**NORMATIVE:** Authorization MUST NOT be derived by parsing the DID string (e.g., extracting "Acorp" from `did:sop:Acorp:Alice`).

* **Rationale:** String parsing is fragile and prone to spoofing attacks.
* **Requirement:** All attributes (Organization, Role) MUST be explicitly resolved via the Identity Ledger.

### 3.2. Separation of Powers

* **L1 Ledger:** Stores the immutable Truth (Who owns the asset? Who is the user?).
* **Axon (Gateway):** Resolves ledger truths and enforces the access boundary.
* **Synapse (Policy Runtime):** Executes deterministic authorization policy and returns typed verdicts.
* **Access Verdict Proof:** Records the Decision (Off-Chain Proof with optional L1 Anchoring capability).

---

## 4. Specification

### 4.1. Data Model Extensions

This SIP standardizes the interpretation of existing fields without breaking SIP-016 schemas.

**A. Asset Ownership (SIP-016 Extension)**
The `owner_did` field of an IDAS SHOULD resolve to an Organization DID (e.g., `did:sop:acorp-admin`) rather than an individual employee.

**B. Identity Attributes (Explicit Linkage)**
The Identity Ledger MUST store an immutable reference to the `OrganizationDID` during the identity registration.

The `get_identity` interface is updated:

```go
func GetIdentityAttributes(userDID string) IdentityContext {
    return IdentityContext{
        OrgDID: "did:sop:acorp-hq",
        Roles:  ["OPERATOR", "SAFETY_OFFICER"],
        Status: "ACTIVE",
        ValidUntil: 1893456000
    }
}
```
### 4.2. Policy Logic (The ABAC Formula)

Access permissions are computed deterministically by the Synapse Policy Runtime.

**Primary Logic (Internal Staff):**
Permission = (Asset.Org == User.Org) ∧ (User.Role ∈ AllowedRolesAction)

**Default Role Matrix:**

| Role | Read (Telemetry) | Write (Control) | Admin (Transfer) |
| :--- | :---: | :---: | :---: |
| **OPERATOR** | ✅ ALLOW | ✅ ALLOW | ❌ DENY |
| **AUDITOR** | ✅ ALLOW | ❌ DENY | ❌ DENY |
| **MANAGER** | ✅ ALLOW | ✅ ALLOW | ✅ ALLOW |

### 4.3. Guest Access Policies (Vendor Support)

To support external maintenance (e.g., OEM Technicians), the Policy Object MAY include an explicit `GuestAllowList`.

* **Logic:** `IF User.DID IN Policy.GuestList AND User.Role == 'VENDOR' THEN ALLOW.`
* **Constraint:** Guest access MUST be time-bound and explicitly defined in the Anchored Policy.

---

## 5. Operational Semantics

### 5.1. Policy Hash Anchoring

To prevent Gateways from running "Secret Rules," the active Authorization Policy (WASM) MUST be anchored on L1 via `OP_POLICY_ANCHOR`.

* **Verification:** Upon receiving a request, the Gateway verifies that its loaded Policy Binary Hash matches the anchored hash for the applicable **policy scope**.

### 5.2. Execution Flow

1.  **Request:** Vinci Client sends `{UserDID, AssetID, Intent: WRITE}`.
2.  **Resolution:** Axon queries L1 for `Asset.Owner` and `User.Attributes`.
3.  **Evaluation:** Synapse executes the ABAC Policy (Internal Match OR Guest Match).
4.  **Verdict:** Access Granted/Denied.

### 5.3. The Access Verdict (Off-Chain Proof)

To avoid network congestion (Gas/Latency), routine Access Verdicts SHOULD NOT be written to L1 as individual transactions. Instead, the Gateway MUST generate a cryptographically signed Off-Chain Proof.

**Structure (JSON):**

```json
{
  "op": "ACCESS_VERDICT_PROOF",
  "gateway_sig": "0x...",
  "request_id": "0x...",
  "asset_id": "0x123...",
  "user_did": "did:sop:ali",
  "intent": "WRITE",
  "decision": "PERMIT",
  "access_level": "WRITE",
  "policy_hash": "0xabc...",
  "not_before": 1735689600,
  "not_after": 1735689900,
  "nonce": "0x...",
  "timestamp": 1735689600
}
```
* **Storage:** These proofs are stored in the Gateway's local immutable log (BoltDB/SQLite).
* **Dispute:** In case of an incident, the specific Proof is uploaded to L1 as evidence.
* **Checkpoint:** The Gateway MAY periodically anchor the Merkle Root of these proofs to L1.

## 6. Use Cases

### 6.1. The "Factory Sale" Scenario (Instant Revocation)

* **Context:** "Acorp" sells the factory to "Bcorp".
* **Action:** Acorp executes `OP_TRANSFER_IDAS` to `did:sop:bcorp`.
* **Result:** Instantly, all 500 Acorp employees lose access.
    * *Logic:* `Asset.Org (Bcorp) != User.Org (Acorp)`. Access **DENIED**.
    * *Benefit:* Zero latency, zero overhead, 100% secure.

### 6.2. The "Vendor Maintenance" Scenario

* **Context:** A Siemens technician (`did:sop:siemens:hans`) needs to fix an Acorp boiler.
* **Action:** Acorp updates the Asset Policy to include `did:sop:siemens:hans` in `GuestAllowList` with a 4-hour TTL.
* **Result:** Hans gains temporary access despite the Organization Mismatch.

---

## 7. Security Considerations

* **Spoofing:** By banning string parsing, we prevent Name Collision Attacks.
* **Replay:** Access Proofs include timestamps, nonces, and validity windows.
* **Availability:** Local verification ensures operation even if L1 connectivity is intermittent (cached state), provided the attributes are fresh.

---

## 8. Backwards Compatibility

SIP-021 is fully compatible with SIP-016. Assets with a single-user DID as the owner will simply fail the OrgMatch check for other users, defaulting to the legacy single-owner model behavior.

---

**Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.**