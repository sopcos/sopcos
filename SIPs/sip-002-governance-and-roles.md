# SOPCOS IMPROVEMENT PROPOSAL (SIP-002)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 002 |
| **Title** | Sopcos Governance, Roles & Licensing Strategy |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Process / Governance |
| **Author** | Maestro & Nexus (The Foundation) |
| **Created** | 2025-12-24 |
| **Last Revised** | 2026-01-05 |
| **Layer** | Governance |
| **License** | CC0-1.0 (Public Domain) |

## 1. Abstract
This proposal defines the governance structure, participation roles, and licensing strategy for the Sopcos Ecosystem. It establishes a model of **"Controlled Freedom,"** designed to balance Open Source innovation with industrial-grade reliability and legal accountability.

The goal is to prevent fragmentation and ensure that while the code remains free, the **"Legitimacy"** and **"Trust"** of the network remain secured by The Foundation.

## 2. Governance & Roles

### 2.1. The Foundation (Authority with Constraints)
The Foundation acts as the steward of the protocol, not a dictator.

* **Role:** Manages the SIP (Sopcos Improvement Proposal) process and holds trademark rights.
* **Procedural Constraint:** The Foundation cannot finalize a SIP or introduce "Breaking Changes" to the core SPL specification without the technical consensus of at least **3 independent Core Maintainers** (not employed by the same legal entity). This ensures decentralization of technical decisions.

### 2.2. Vendors & Integrators (The Kernel Boundary)
Vendors are free to build gateways and proprietary GUIs/Wrappers around Sopcos. However, strict boundaries apply to the **"Kernel."**

* **Kernel Definition:** The "Kernel" includes the following immutable components:
    1.  **SPL Parser:** Logic for interpreting policy syntax.
    2.  **Decision Engine:** The deterministic evaluation logic.
    3.  **Audit Commit Logic:** The mechanism for hashing and committing decisions to Axon.
    4.  **Cryptographic Primitives:** Signature and verification libraries.
    5.  **Deterministic Simulation Engine:** The logic producing "Proof of Foreknowledge" (as defined in SIP-005).

* **The Evolution Clause:** The Kernel is **IMMUTABLE** by Vendors/Integrators. It can **ONLY** be updated or patched through the official SIP process managed by The Foundation. Modifications outside this process result in the immediate loss of "Sopcos Compliant" status.

### 2.3. Policy Authors & Oversight (Signature Requirements)
*(Renamed from "Operators" to avoid conflict with SIP-006)*

Policy Authors define logic but do not write code. To mitigate human error and insider threats, authorship requires cryptographic oversight:

* **Role Definition:** A Policy Author is an administrator, engineer, or committee designing the rules (e.g., *"If Temp > 100 Deny"*). They are distinct from "Operators" (Field Personnel defined in SIP-006 who perform manual Overrides).
* **Signature Requirement:** All SPL policies generated via GUI/DSL must be digitally signed according to the Authority Hierarchy defined in **SIP-008**.
    * *Example:* Regulatory Policies (LVL 1) require **Multi-Sig** (Four-Eyes Principle).
    * *Example:* Optimization Policies (LVL 3) may accept Single-Sig.
* **Enforcement:** Synapse nodes will reject any policy that does not meet the signature threshold of its declared Authority Level.

### 2.3.1. The Burden of Authorship (Liability of Design)
While the Operator is liable for their interventions (Overrides), the Policy Author is strictly liable for the autonomous execution of the logic they sign.

**The Foreseeability Clause:**
> "The Policy Author is liable for the predictable consequences of their logic in the absence of a valid Override."

Liability is determined by the **"Proof of Foreknowledge"** defined in **SIP-005**:

1.  **Strict Liability:** If the logic causes damage without generating a `PREDICTION_CRITICAL` warning during the simulation phase (i.e., the Simulator failed), the Author is liable for **Negligence (Bad Design)**.
2.  **Intentional Malice:** If the logic causes damage and the simulation log confirms that a `PREDICTION_CRITICAL` warning was issued but ignored (Signed via *Force-Deploy*), the Author is liable for **Willful Misconduct (Malice)**.

**The Operator's Defense:**
An Operator **CANNOT** be held liable for failing to override a **"Lawful Evil"** policy (Scenario C in SIP-004). If the machine says `ALLOW` based on the Author's rule, and the Operator does nothing, the Operator is compliant. The fault lies entirely with the Author's Logic.

### 2.4. Operational Role Mapping (RBAC)
While the protocol identifies entities via DID/Public Keys, operational contexts require specific role attributes.

* **Mapping:** The Foundation or the local Enterprise Admin **MUST** sign a "Role Verifiable Credential" linking a specific DID to a human-readable role (e.g., "CISO", "HSE Officer").
* **Verification:** Synapse checks this credential before allowing actions restricted to specific roles.

### 2.5. The Axiom of Moral Separation (Separation of Moral Powers)
Sopcos Governance enforces a strict **"Separation of Moral Powers"** based on the legal principle *nemo iudex in causa sua* (no one should be a judge in their own cause).

**The Universal Constraint:**
> "No role can unilaterally absolve a liability state that they themselves created."

This axiom is applied across the ecosystem as follows:

1.  **The Operator's Constraint (Action vs. Absolution):**
    * **Action:** An Operator creates a "Dirty State" via an Override (SIP-006).
    * **Constraint:** The Operator **CANNOT** sign the "Reset Transaction" to clear that state. Only an independent Auditor can absolve the system.

2.  **The Author's Constraint (Design vs. Approval):**
    * **Action:** An Author designs a "High-Risk Policy" (SIP-005).
    * **Constraint:** The Author **CANNOT** be the sole signatory for that policy's deployment. A distinct Approver (or Multi-Sig Quorum) is required to validate the risk.

3.  **The Auditor's Constraint (Observation vs. Participation):**
    * **Action:** An Auditor validates compliance.
    * **Constraint:** An Auditor **CANNOT** hold operational keys (Operator Role) for the same scope they are auditing. One cannot audit a system one operates.

**Implementation:**
The Kernel (SIP-002, Sec 2.2) **MUST** reject any transaction where the `Creator_DID` and the `Absolver_DID` are identical, if the transaction type involves liability clearance (e.g., `STATE_CLEARANCE`, `POLICY_DEPLOY_CRITICAL`).

## 3. Licensing Strategy: "Network Copyleft"
To protect the ecosystem from proprietary forks while encouraging enterprise adoption, a Dual-Licensing Model is adopted.

### 3.1. Primary License: AGPLv3 (Network Copyleft)
* **Scope:** The core codebase (Synapse/Axon) is licensed under **AGPLv3**.
* **Clarification (The SaaS Exception):** Using Sopcos Synapse as a service (consuming its API via HTTP/MQTT for ERP/SCADA integration) does **not** constitute a derivative work and does not force the consuming application to be open-sourced.
* **Viral Protection:** If a vendor modifies the source code of the Kernel (e.g., changes the simulation logic) and provides it as a service/product, they **must** release their modifications under AGPLv3. This prevents "Black Box" forks that hide safety risks.

### 3.2. Commercial License (Enterprise Option)
* Vendors wishing to modify the Kernel and keep their changes proprietary (e.g., for specialized military hardware) must acquire a **Commercial License** from The Foundation. This ensures a sustainable revenue model.

## 4. Legitimacy & Trust Framework

### 4.1. Dynamic Certification (Revocable Trust)
Certification is treated as a living mechanism, not a static label.

* **Time-Bound:** "Certified Node Keys" are issued with an expiration date and must be renewed periodically.
* **Revocation (CRL):** The Foundation maintains a Certificate Revocation List (CRL). If a node or vendor is found to be malicious, non-compliant (e.g., modified Kernel), or compromised, their certificate is revoked immediately.

### 4.2. Canonical Axon (Root of Legal Truth)
* **Definition:** "Canonical Axon" is the main chain recognized by The Foundation.
* **Legal Standing:** In the event of industrial disputes, audits, or insurance claims, only records found on the Canonical Axon are considered **"Legal Truth."**

### 4.3. The Transparency of Exception (Public Health Dashboard)
While the content of an Override is private (protected by the `Input Hash` per SIP-003 to ensure GDPR/Trade Secret compliance), the *occurrence* of an Override is a public concern regarding the safety of the network.

**The Glass House Mandate:**
The Foundation reserves the right to aggregate and publish the "Override Frequency Metrics" of any Sopcos-certified node or organization.

1.  **Public Metadata:** The following data points are defined as **"Public Network Property"** and cannot be obfuscated:
    * Total number of Overrides (Transfer of Liability events).
    * Ratio of "Clean State" vs. "Dirty State" uptime.
    * Frequency of `PREDICTION_CRITICAL` warnings ignored (Force-Deploys).

2.  **The "Reliability Score":** The Foundation will maintain a dynamic "Reliability Score" for Certified Entities.
    * **Low Frequency:** Indicates a well-tuned policy and stable operation.
    * **High Frequency:** Indicates "Policy Drift," unsafe conditions, or "Normalization of Deviance."

**Implication:**
An organization with 47 Overrides in 6 months may technically be "Compliant" (signatures are valid), but their **Reliability Score** will plummet. This signals to Insurers, Regulators, and Partners that the node is operating in a zone of high volatility.

## 5. Threat Mitigation (The Hostile Takeover Defense)
**Scenario:** Can a hostile entity fork the code and take over the ecosystem?

**Defense:**
1.  **Code:** They *can* fork the code (Open Source).
2.  **Brand:** They *cannot* use the "Sopcos" name (Trademark).
3.  **Trust:** They *cannot* issue valid certificates (Root of Trust).
4.  **Adoption:** Without valid certificates, their nodes are rejected by industry-standard systems.

**Conclusion:** The Foundation retains a **"Monopoly on Legitimacy,"** ensuring the ecosystem's integrity remains intact even if the code is copied.