# SOPCOS IMPROVEMENT PROPOSAL (SIP-008)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 008 |
| **Title** | Authority Hierarchy & Lifecycle Control |
| **Status** | LIVING STANDARD (REVISED - LIFECYCLE READY) |
| **Type** | Standards Track (Core) |
| **Author** | The Architect (HQ) |
| **Date** | 2026-01-05 |
| **Layer** | Synapse (Runtime) & Axon (Resolution) |
| **Related** | SIP-014 (Vault), SIP-010 (Algebra) |
| **License** | CC0-1.0 (Public Domain) |

---

## 1. Abstract
This specification establishes the **Normative Authority Graph** defining "Who outranks Whom" in decision-making, and extends this logic to the **Lifecycle Control** of Artifacts stored in the SIP-014 Vault.

It answers two fundamental questions:
1.  **Resolution:** If two active policies conflict, which one prevails?
2.  **Evolution:** Who has the right to update (`SUPERSEDE`) or destroy (`REVOKE`) an existing law?

## 2. Core Axioms
1.  **Source of Power:** Authority is derived from the **DID (Decentralized Identifier)** anchored on the Core Ledger registry, not from metadata inside the file.
2.  **The Pre-Law Reality:** Policy Code is Executable Law, but Physics is Immutable Reality.
3.  **Bureaucratic Sovereignty:** An entity can only evolve (supersede) artifacts they own or artifacts owned by strictly lower authorities.

---

## 3. The Normative Authority Graph (Authority Levels)
Sopcos defines a strict vertical hierarchy. Lower numerical values indicate higher authority.

| Level | Name | Description | Override Semantics |
| :--- | :--- | :--- | :--- |
| **∞** | **PRE-LAW** | Physics / Bio-Safety. Hard-coded firmware limits & physical interlocks. | **IMPOSSIBLE** (Even for Humans) |
| **LVL 0** | **EMERGENCY** | Fire, Earthquake, Evacuation directives. | Only by Pre-Law |
| **LVL 1** | **REGULATORY** | Legal limits, Environmental laws (EPA/OSHA). | By Emergency |
| **LVL 2** | **SITE / ORG** | Corporate or Factory-wide policies. | By Regulatory/Emergency |
| **LVL 3** | **OPTIMIZATION** | Efficiency algorithms. **Default level for AI-derived artifacts (Synapse/Axon).** | Always overridable |
| **LVL 4** | **ADVISORY** | Recommendations, Logging. **Low-confidence AI signals (<50%) default here.** | Lowest priority |
**Constraint (v1.2):** An AI-derived artifact, regardless of its confidence score, **CANNOT** hold Level 0 (Emergency) or Level 1 (Regulatory) authority. These levels require a biological signature (Human) or a deterministic hard-coded rule (Pre-Law).

---

## 4. Lifecycle Control Matrix (The "Supersede" Logic)
This section defines the permissions for **SIP-014 Vault Operations**. It enforces the "Chain of Custody" and prevents lower-tier actors from modifying higher-tier laws.

### 4.1. Definitions
* **Author:** The DID that signed the original artifact.
* **Target:** The artifact being acted upon.
* **Actor:** The DID attempting the transaction.

### 4.2. The Permission Matrix

| Operation | Actor vs. Target Relation | Permission | Rationale |
| :--- | :--- | :--- | :--- |
| **REGISTER** | Any Valid DID | **ALLOW** | Anyone can create new laws (at their own level). |
| **SUPERSEDE** | Actor == Author | **ALLOW** | **Self-Correction:** An author can always update their own document (v1 -> v2). |
| **SUPERSEDE** | Level(Actor) < Level(Target) | **ALLOW** | **Authority Projection:** A Regulator (LVL 1) can forcibly update a Factory Policy (LVL 2). |
| **SUPERSEDE** | Level(Actor) >= Level(Target) | **DENY** | **Insubordination:** A Factory (LVL 2) cannot overwrite a Regulation (LVL 1). |
| **REVOKE** | Actor == Author | **ALLOW** | **Recall:** Author admits fault and pulls the document. |
| **REVOKE** | Level(Actor) == LVL 0 or LVL 1 | **ALLOW** | **Supreme Court:** Regulators or Emergency actors can strike down any lower document. |

---

## 5. Conflict Resolution Semantics (Evaluation Time)
While Section 4 deals with "Updating Files," this section deals with "Resolving Active Conflicts" (Logic defined in SIP-010).

### 5.1. Vertical Resolution (Dominance)
In any conflict between different levels, the **Higher Authority (Lower Level Number)** STRICTLY PREVAILS.
* *Example:* A Level 3 (Optimization) "DENY" cannot stop a Level 0 (Emergency) "ALLOW".

### 5.2. Horizontal Resolution (Deny-Absorption)
In conflicts between policies of **EQUAL** authority (e.g., two Level 2 policies overlapping in scope):
* **The Deny-Absorption Axiom:** The `DENY` verdict acts as the absorbing element.
* `ALLOW` + `DENY` = `DENY`.
* **Principle:** Ambiguity resolves to safety (Silence/Halt).

---

## 6. Jurisdiction Lattice & Scope
Scope is not a tag; it is a Normative Jurisdiction.

### 6.1. Asymmetric Control
A Global Authority can **SHUTDOWN** (restrict) a Local Operation, but it **CANNOT FORCE-ALLOW** (permit) an operation against a Local Safety Constraint if that constraint derives from Pre-Law or Equal Authority.

---

## 7. Human Override: Liability Substitution
A Human Override (SIP-006) does not create a "Level -1" policy.
* **Suspension:** It suspends the Normative Graph (Levels 0-4).
* **Substitution:** It replaces the algorithm with a **Sole Liability Record** anchored to the signer's DID on the Core Ledger.
* **Constraint:** It CANNOT suspend Level ∞ (Pre-Law). Physics always wins.