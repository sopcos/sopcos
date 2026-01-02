# SOPCOS IMPROVEMENT PROPOSAL (SIP-008)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 008 |
| **Title** | Authority Hierarchy & Jurisdiction Lattice |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Standards Track (Core) |
| **Author** | The Architect (HQ) |
| **Created** | 2025-12-30 |
| **Last Revised** | 2026-01-05 |
| **Layer** | Synapse (Runtime) & Axon (Resolution) |
| **Related** | SIP-006 (DID), SIP-010 (Verdict), SIP-005 (Risk) |
| **License** | CC0-1.0 (Public Domain) |

## 1. Abstract
This specification establishes a rigid **Normative Authority Graph** and a **Jurisdiction Lattice** to resolve conflicts between competing policies. It replaces arbitrary priority integers with a constitutionally defined hierarchy and introduces **Pre-Law Constraints** based on immutable physical reality.

## 2. Core Axioms
1.  **Source of Power:** Power is derived from Authority Definition (DID) anchored on the L1 Registry, not from the policy payload or computational precedence.
2.  **The Pre-Law Reality:** Policy Code is Executable Law, but Physics is Immutable Reality.
3.  **Silence of the Machine:** If two equal laws contradict, the machine **MUST** remain silent (or fail-safe).

## 3. Policy Identity & Verification (The Registry)
Authority is not a self-proclaimed integer within the policy file; it is a verified status derived from the L1 Chain.

* **Identity Standard:** Policies are signed and identified by DIDs (e.g., `did:sopcos:policy:global:safety`).
* **Verification Logic:** The Runtime (Synapse) **MUST** verify the `authority_level` by querying the L1 Registry using the signer's DID.
* **Spoofing Protection:** If a policy claims Level 0 in its metadata but the Registry reports Level 5 (or no record) for that signer, the policy is executed at the Registry's level (or rejected).

## 4. The Normative Authority Graph
Sopcos operates on two planes of reality: Ontological (Immutable) and Normative (Mutable).

| Level | Name | Description | Override Semantics |
| :--- | :--- | :--- | :--- |
| **∞** | **PRE-LAW** | Physics / Bio-Safety. Hard-coded firmware limits & physical interlocks. | **IMPOSSIBLE** (Even for Humans) |
| **LVL 0** | **EMERGENCY** | Fire, Earthquake, Evacuation directives. | Only by Pre-Law |
| **LVL 1** | **REGULATORY** | Legal limits, Environmental laws (EPA/OSHA). | By Emergency |
| **LVL 2** | **SITE / ORG** | Corporate or Factory-wide policies. | By Regulatory/Emergency |
| **LVL 3** | **OPTIMIZATION** | Efficiency, Cost-saving algorithms. | Always overridable |
| **LVL 4** | **ADVISORY** | Recommendations, Logging, Telemetry config. | Lowest priority |



## 5. Jurisdiction Lattice & Scope Logic
Scope is not a tag; it is a Normative Jurisdiction.

### 5.1. Scope Containment
A verdict is valid **IFF** (If and Only If) the target exists within the `policy.scope`.

### 5.2. Asymmetric Control (No Force-Allow)
A Global Authority can **SHUTDOWN** (restrict) a Local Operation, but it **CANNOT FORCE-ALLOW** (permit) an operation against a Local Safety Constraint.
* **Principle:** Global can restrict Local, but Global cannot violate Local Physics.

## 6. Conflict Resolution Semantics
*(Technical Note: The mathematical implementation of these semantics is defined in **SIP-010 Verdict Algebra**.)*

This section defines the Hierarchical Intent, while SIP-010 defines the Execution Algorithm.

### 6.1. Vertical Resolution (Authority Dominance)
In any conflict between different levels, the **Higher Authority** (Lower Level Number) **STRICTLY PREVAILS**.
* **Example:** A Level 3 (Optimization) `DENY` cannot stop a Level 0 (Emergency) `ALLOW`.
* **Mechanism:** See "Authority Projection" in SIP-010.

### 6.2. Horizontal Resolution (Deny-Absorption)
In conflicts between policies of **EQUAL** authority (e.g., two Level 2 policies overlapping in scope):
* **The Deny-Absorption Axiom:** The `DENY` verdict acts as the absorbing element.
* **Logic:** If one valid policy says `ALLOW` and another equal policy says `DENY`, the result is `DENY`.
* **Safety:** Ambiguity resolves to safety (Silence/Halt).

## 7. Human Override: Liability Substitution
A Human Override is not a "Super-Policy". It is the suspension of the Normative Graph, replaced by Personal Liability.

> **Constitutional Clause:** "A Human Override does not create a higher authority. It suspends the authority graph for a bounded scope and time, replacing it with a Sole Liability Record anchored to the signer's DID (as defined in **SIP-011**)."

## 8. Authority Escalation Cost Matrix
To prevent abuse ("Fake Emergencies" or "Casual Regulation Bending"), activating high-authority levels incurs a cryptographic and bureaucratic cost.

> **CRITICAL CONSTRAINT:** These requirements establish the minimum identity threshold. The **SIP-005 Simulation Risk Assessment** may impose *additional* requirements (e.g., wider Consensus) based on the specific blast radius of the policy.

| Level | Identity Requirement | Time Lock | Max TTL (Sunset) |
| :--- | :--- | :--- | :--- |
| **Pre-Law (∞)** | Immutable | N/A | Permanent |
| **Emergency (L0)** | Single Sig (Authorized Operator) | 0s (Immediate) | 15 Minutes (Strict Limit) |
| **Regulatory (L1)** | Multi-Sig (3+ diverse roles) | 24 Hours | 1 Year |
| **Site/Org (L2)** | Multi-Sig (2 roles) | 1 Hour | 30 Days |
| **Optimization (L3)** | Single Sig (Engineer/Algo) | 0s | Unlimited |