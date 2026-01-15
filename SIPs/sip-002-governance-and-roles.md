# SOPCOS IMPROVEMENT PROPOSAL (SIP-002)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 002 |
| **Title** | Sopcos Governance, Roles & Licensing Strategy |
| **Status** | LIVING STANDARD (REVISED - VAULT READY) |
| **Type** | Process / Governance |
| **Author** | Maestro & Nexus (The Foundation) |
| **Date** | 2026-01-05 |
| **Layer** | Governance |
| **License** | CC0-1.0 (Public Domain) |

---

## 1. Abstract
This proposal defines the governance structure, participation roles, and licensing strategy for the Sopcos Ecosystem. It establishes a model of **"Controlled Freedom,"** designed to balance Open Source innovation with industrial-grade reliability and legal accountability.

The goal is to prevent fragmentation and ensure that while the code remains free, the **"Legitimacy"** and **"Trust"** of the network remain secured by The Foundation.

## 2. Governance & Roles

### 2.1. The Foundation (Authority with Constraints)
The Foundation acts as the steward of the protocol, not a dictator.
* **Role:** Manages the SIP (Sopcos Improvement Proposal) process and holds trademark rights.
* **Procedural Constraint:** The Foundation cannot finalize a SIP or introduce "Breaking Changes" to the core SPL specification without the technical consensus of at least **3 independent Core Maintainers** (not employed by the same legal entity). This ensures decentralization of technical decisions.

### 2.2. Vendors & Integrators (The Kernel Boundary)
Vendors are free to build gateways and proprietary GUIs/Wrappers around Sopcos. However, strict boundaries apply to the "Kernel."

**Kernel Definition (Revised per SIP-014):**
The "Kernel" includes the following **IMMUTABLE** components. Modifying these logics disqualifies a node from the network:

1.  **SPL Parser:** Logic for interpreting SIP-001 policy syntax.
2.  **Decision Engine:** The deterministic evaluation logic (SIP-010 Algebra).
3.  **Audit Commit Logic:** The mechanism for hashing and committing decisions to Axon.
4.  **Cryptographic Primitives:** Implementation of Ed25519 signatures and AES-GCM (SIP-009 Encryption).
5.  **Deterministic Simulation Engine:** The logic producing "Proof of Foreknowledge" (SIP-005).
6.  **Artifact Resolution Logic (New):** The specific routine defined in **SIP-014** for resolving URNs to Vault paths and verifying SHA-256 integrity before execution. A node explicitly **CANNOT** load code from arbitrary URLs.
7.  **Industrial Asset Engine:** The native logic handling OpCodes 40-49 (SIP-016). It manages the creation, ownership transfer, and burning of Industrial NFTs. This logic MUST be deterministic and validated by all nodes. A failure in asset logic constitutes a consensus failure.

* **The Evolution Clause:** The Kernel can **ONLY** be updated or patched through the official SIP process managed by The Foundation. Modifications outside this process result in the immediate loss of "Sopcos Compliant" status.

### 2.3. Policy Authors & Oversight (Signature Requirements)
Policy Authors define logic but do not write code. To mitigate human error and insider threats, authorship requires cryptographic oversight:

* **Role Definition:** A Policy Author is an administrator, engineer, or committee designing the rules. They are distinct from "Operators" (Field Personnel defined in SIP-006 who perform manual Overrides).
* **Signature Requirement:** All SPL policies generated via GUI/DSL must be digitally signed according to the Authority Hierarchy defined in SIP-008.
    * *Example:* Regulatory Policies (LVL 1) require Multi-Sig (Four-Eyes Principle).
* **Enforcement:** Synapse nodes will reject any policy that does not meet the signature threshold of its declared Authority Level.

### 2.4. Operational Role Mapping (RBAC)
While the protocol identifies entities via DID/Public Keys, operational contexts require specific role attributes.
* **Mapping:** The Foundation or the local Enterprise Admin **MUST** sign a "Role Verifiable Credential" linking a specific DID to a human-readable role (e.g., "CISO", "Metrologist", "HSE Officer").
* **Verification:** Synapse checks this credential before allowing actions restricted to specific roles.

## 3. Licensing Strategy: "Network Copyleft"
To protect the ecosystem from proprietary forks while encouraging enterprise adoption, a Dual-Licensing Model is adopted.

### 3.1. Primary License: AGPLv3 (Network Copyleft)
* **Scope:** The core codebase (Synapse/Axon/Vault-Drivers) is licensed under **AGPLv3**.
* **Clarification (The SaaS Exception):** Using Sopcos Synapse as a service (consuming its API via HTTP/MQTT for ERP/SCADA integration) does not constitute a derivative work.
* **Viral Protection:** If a vendor modifies the source code of the Kernel (e.g., bypasses the SIP-014 Vault check) and provides it as a service/product, they must release their modifications under AGPLv3.

### 3.2. Commercial License (Enterprise Option)
* **Vendors wishing to modify the Kernel and keep their changes proprietary must acquire a Commercial License from The Foundation.**

## 4. Legitimacy & Trust Framework

### 4.1. Dynamic Certification (Revocable Trust)
Certification is treated as a living mechanism.
* **Time-Bound:** "Certified Node Keys" are issued with an expiration date.
* **Revocation (CRL):** The Foundation maintains a Certificate Revocation List (CRL). If a node ignores a SIP-014 Revocation Order, its certificate is revoked immediately.

### 4.2. Canonical Axon (Root of Legal Truth)
* **Definition:** "Canonical Axon" is the main chain recognized by The Foundation.
* **Legal Standing:** In the event of industrial disputes, audits, or insurance claims, only records found on the Canonical Axon are considered "Legal Truth."

## 5. Threat Mitigation (The Hostile Takeover Defense)
**Scenario:** Can a hostile entity fork the code and take over the ecosystem?
**Defense:**
1.  **Code:** They can fork the code (Open Source).
2.  **Brand:** They cannot use the "Sopcos" name (Trademark).
3.  **Trust:** They cannot issue valid certificates (Root of Trust).
4.  **Adoption:** Without valid certificates, their nodes are rejected by industry-standard systems.

**Conclusion:** The Foundation retains a **"Monopoly on Legitimacy,"** ensuring the ecosystem's integrity remains intact even if the code is copied.

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*