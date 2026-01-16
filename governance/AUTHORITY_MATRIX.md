# 🏛️ SOPCOS Authority Matrix (SAM)

> **Status:** Normative Reference  
> **Scope:** Authority hierarchy, override semantics & liability transfer  
> **Derived from:**  SIP-006 (Emergency), SIP-008 (Hierarchy), SIP-011 (Forensic Liability), SIP-012 (Mandate), SIP-015 (Cognitive Verdicts)

This document defines the canonical hierarchy of actors within the SOPCOS ecosystem.
It specifies **who may do what**, **under which constraints**, and **how liability is transferred**
when rules are overridden.

All actors are identified by a **Decentralized Identifier (DID)** and operate within a
strictly ordered **Authority Level** system.

---

## Authority Ordering Rule

> **Lower numeric levels represent higher authority.**  
> Higher levels may **not** override lower levels except under **explicitly defined emergency semantics**.
> If ambiguity exists, the system **fails closed**.

---

## 🧬 Authority Levels

| Level | Hex Role | Actor | Description | Capabilities |
| :---: | :---: | :--- | :--- | :--- |
| **∞** | 0xFF | **PRE-LAW (Physics)** | Immutable physical and hardware limits. | **Absolute Ruler.** Cannot be overridden by any human or software. |
| **0** | 0x00 | **EMERGENCY** | Fire, Explosion, Life Safety. | Can override all normative levels (1-4) to prevent imminent harm. |
| **1** | 0x01 | **REGULATORY** | Legal limits (OSHA, EPA). | Defines the boundary of legal operation. Can revoke lower artifacts. |
| **2** | 0x10 | **SITE / ORG** | Factory owner / Asset authority. | Deploy site-specific policies and manage IDAS digital twins. |
| **3** | 0x20 | **OPTIMIZATION (AI)** | Default for AI Agents & Algorithms. | Efficiency-driven actions. Always overridable by humans. |
| **4** | 0x30 | **ADVISORY** | Low-confidence signals / Logging. | Non-binding recommendations. Minimal authority. |
| **5** | `0x99` | **AUDITOR** | Independent inspector / verifier. | Can sign **Reset_Transaction** to clear Dirty State after inspection and attestation. |

---

## 🛡️ Override Semantics (SIP-006)

### Default Rule
A lower-authority level **cannot** violate constraints set by a higher-authority level.

##### Execution Preemption & Confession of Liability
Human intervention is not a request for permission; it is an  **Execution Preemption**.
*   **Signed Confession:** Overrides are cryptographically signed admissions of full liability.
*   **Physics Constraint:** Overrides  **CANNOT**  bypass Level ∞ (Pre-Law) constraints. Physics always wins.
*   **Temporal Limit (TTL):** Every override MUST have a Time-To-Live. Reverts to Fail-Safe on expiry.


### Consequence: Dirty State
When an Emergency Override occurs:
- The system enters **DIRTY STATE**
- **Liability is cryptographically transferred** to the overriding actor’s DID
- System returns to normal ONLY via an Auditor's **Reset_Transaction** 
- The action and context are immutably recorded
- Normal optimization and AI execution paths are suspended

---
#### 📜 Sovereign Mandate (SIP-012)
A Human Sovereign may delegate specific authority to a Machine Delegate for a time window.
*   **Inter-Temporal Liability:** The "Monday Signer" remains liable for a "Friday Accident".
*   **Anti-Zombie Rule:** Mandates are voided if the node remains offline beyond `max_offline_ttl`.
*   **AI Constraint:** AI Delegates cannot hold Override or Level 0/1 authority.

---
> Overrides are not bypasses.  
> They are **signed confessions of responsibility**.

---

## 🧪 Dirty State Lifecycle

1. **Trigger:** Emergency Override is executed  
2. **Record:** Decision, context, and signer DID are immutably logged  
3. **Containment:** System operates under restricted semantics  
4. **Inspection:** Auditor reviews evidence and site conditions  
5. **Reset:** Auditor signs `Reset_Transaction`  
6. **Clear:** System exits Dirty State and resumes normal operation

If a reset is **not** signed, Dirty State **persists**.

---

## 🤖 AI Constraints (Fail-Closed)

-  **Cognitive Auditor Role:** AI is an observer and advisor, not an authority.
-  **No Autonomy:** AI agents  **cannot**  hold LVL 0 or LVL 1 authority.
- AI agents **cannot** initiate overrides
- AI agents **cannot** expand policy scope
- AI recommendations are **non-binding**
- If AI output conflicts with policy or authority constraints, execution is **denied**

---

## 🔐 Identity & Verification

- Authority is derived from the **signer DID anchored on the Sopcos Chain (L1)**
- Role assignments are verified via **Role Verifiable Credentials**
- File metadata or UI context **does not** grant authority

---

## 🔗 Relationship to Governance

This matrix is the **concrete instantiation** of the authority model defined in governance documents.

- Constitutional rules and change control: `governance/GOVERNANCE.md`
- Lifecycle permissions and hierarchy: SIP-008
- Emergency and override semantics: SIP-006

If conflicts arise, **binding SIP law prevails**.

---

## Final Principle

> SOPCOS does not prevent humans from acting.  
> It makes sure they **cannot act without being accountable**.
