# 🏛️ SOPCOS Authority Matrix (SAM)

> **Status:** Normative Reference  
> **Scope:** Authority hierarchy, override semantics & liability transfer  
> **Derived from:** SIP-006 (Emergency Semantics), SIP-008 (Authority Hierarchy & Lifecycle Governance), SIP-015 (Cognitive Verdicts)

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
| **0** | `0x00` | **PHYSICS (Pre-Law)** | Immutable physical and safety constraints. | Cannot be overridden. (e.g., temperature > melting point, hard interlocks). |
| **1** | `0x01` | **SOPCOS FOUNDATION** | Protocol stewards and guardians. | Can initiate SIP lifecycle actions (ratification, revision, supersede, revoke) **with required consensus**. Cannot access user data. |
| **2** | `0x10` | **FACTORY / SITE ADMIN** | Asset owner and operational authority. | Can deploy SIP-001 policies, configure site constraints, register/burn IDAS assets. |
| **3** | `0x20` | **OPERATOR (Human-in-the-Loop)** | On-site human authority. | Can trigger **Emergency Override**. Cannot change policy definitions. |
| **4** | `0x30` | **AXON AI (Cognitive Agent)** | Advisory intelligence. | Can **suggest actions** and execute **only pre-authorized actions** within policy bounds. **Cannot expand scope. Cannot override.** |
| **5** | `0x99` | **AUDITOR** | Independent inspector / verifier. | Can sign **Reset_Transaction** to clear Dirty State after inspection and attestation. |

---

## 🛡️ Override Semantics (SIP-006)

### Default Rule
A lower-authority level **cannot** violate constraints set by a higher-authority level.

### Emergency Exception
The **Emergency Override** allows a lower-authority human actor (e.g., **Operator, LVL 3**)  
to temporarily violate a higher-level policy **only** to prevent imminent harm.

### Consequence: Dirty State
When an Emergency Override occurs:
- The system enters **DIRTY STATE**
- **Liability is cryptographically transferred** to the overriding actor’s DID
- The action and context are immutably recorded
- Normal optimization and AI execution paths are suspended

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

- AI agents **cannot** hold LVL 0 or LVL 1 authority
- AI agents **cannot** initiate overrides
- AI agents **cannot** expand policy scope
- AI recommendations are **non-binding**
- If AI output conflicts with policy or authority constraints, execution is **denied**

---

## 🔐 Identity & Verification

- Authority is derived from the **signer DID anchored on L1**
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
