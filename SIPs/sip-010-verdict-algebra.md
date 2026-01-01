# SOPCOS IMPROVEMENT PROPOSAL (SIP-010)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 010 |
| **Title** | The Verdict Algebra (Decision Resolution Logic) |
| **Status** | RATIFIED |
| **Type** | Standards Track (Core) |
| **Layer** | Synapse (Runtime) |
| **Motto** | *Decisions are vectors. Conflicts are structural, not exceptional.* |
| **Related** | SIP-008, SIP-009 |

## 1. Abstract
This specification defines the deterministic mathematical rules for resolving multiple Policy Verdicts (`.sop`) into a single Final Verdict.

It defines how multiple, simultaneous verdicts—originating from different authorities, scopes, and times—are composed into a single enforceable outcome.

**SIP-010 eliminates:**
* Arbitrary priority integers
* Temporal race conditions
* Implicit “last-write-wins” logic

**It replaces them with:**
* A normative authority hierarchy (SIP-008)
* A jurisdictional scope lattice
* A mathematically deterministic resolution algorithm

**The runtime does not decide. It calculates.**

## 2. Motivation
In any non-trivial Sopcos deployment, a target scope may be subject to multiple simultaneous verdicts:
* A **Local Safety Policy** denies operation (e.g., pressure too high).
* A **Global Efficiency Policy** allows continuation (e.g., demand is high).
* A **Regulatory Constraint** restricts operation hours.
* An **Emergency Directive** orders an immediate shutdown.

Without a formal resolution algebra, systems typically revert to fragile heuristics:
* Timestamp ordering ("Last one wins").
* Configuration load order.
* Undocumented precedence rules.

Such behavior is **legally indefensible** and **operationally unsafe**.

SIP-010 establishes a foundational axiom:
**Conflict is not an error state. Conflict is the primary state.**

**A verdict is not a boolean. It is a directed force with jurisdiction.**

## 3. The Verdict Vector
Mathematically, a Verdict $V$ is a 4-dimensional vector defined as:

$$V = \langle \delta, \alpha, \sigma, \tau \rangle$$

Where:
1.  **$\delta$ (Decision - Direction):** The vector direction (`HALT`, `DENY`, `ALLOW`).
2.  **$\alpha$ (Authority - Magnitude):** The scalar weight derived from SIP-008.
3.  **$\sigma$ (Scope - Domain):** The spatial set where this force is applied.
4.  **$\tau$ (Timestamp - Point):** The temporal coordinate (used for freshness, not priority).

## 4. Formal Model & Resolution Logic
The resolution logic is defined over the Verdict Space using the following predicates, operators, and algorithms.

### 4.1. Verdict Space ($\mathbb{V}$)
Let $\mathbb{V}$ be the set of all possible well-formed verdict vectors.
$$v \in \mathbb{V} \implies v = \langle \delta, \alpha, \sigma \rangle$$

**Key Properties:**
* **Comparability:** Verdicts are comparable only under defined operators.
* **Ordering:** No total ordering exists within $\mathbb{V}$. It is a partially ordered set under $\succ$.
* **Composition:** The operation is partial and conditional.

### 4.2. Applicability Predicate ($\vdash$)
An operator defining if a verdict is juridically valid for a specific target scope $t$.
$$v \vdash t \iff t \subseteq v.\sigma$$

**Constraint:**
Verdicts failing this predicate are **non-participating** and **MUST** be discarded before resolution.

### 4.3. Authority Dominance Relation ($\succ$)
A strict partial order defining the hierarchy between two verdicts.
$$v_1 \succ v_2 \iff v_1.\alpha < v_2.\alpha$$

**Key Properties:**
* **Source:** Authority is derived strictly from the DID namespace.
* **Immutability:** It is constitutionally fixed and **is not configurable at runtime**.
* **Metric Nature:** Numerical comparison defines a strict **ordinal ordering** (Precedence), not a **cardinal magnitude**.

### 4.4. Directional Conflict Predicate ($\perp$)
Defines a state of opposition between two vectors in the same authority plane.
$$v_1 \perp v_2 \iff v_1.\delta \neq v_2.\delta$$

### 4.5. Verdict Composition Operator ($\oplus$)
The binary operator that resolves a set of applicable verdicts into a single outcome.
$$V_{final} = \bigoplus_{v \in S_{valid}} v$$

## 5. Execution Algorithm (Normative)
Implementations of the Synapse Engine **MUST** strictly adhere to the following evaluation pipeline. The runtime is **stateless** with respect to the resolution process; it calculates the output solely from the current input vectors.

**Algorithm: `Resolve(Verdicts[]) -> FinalVerdict`**

1.  **Validation & Filtering ($\Phi$):**
    * Discard any verdict where `TargetScope` is not a subset of `VerdictScope`.
    * Discard any malformed or unsigned verdicts (SIP-009 check).
    * *Result:* `ValidSet`

2.  **Authority Projection ($\Pi$):**
    * If `ValidSet` is empty, goto **Step 4**.
    * Find `BestAuthority` = `min(v.AuthorityLevel)` in `ValidSet`.
    * Discard all verdicts where `v.AuthorityLevel > BestAuthority`.
    * *Result:* `ProjectedSet` (All items share the same, highest authority).

3.  **Composition ($\Sigma$):**
    * Iterate through `ProjectedSet`:
        * If any `v.Decision == HALT` $\rightarrow$ Return `HALT`.
        * If any `v.Decision == DENY` $\rightarrow$ Mark `Denied = true`.
    * If `Denied == true` $\rightarrow$ Return `DENY`.
    * Return `ALLOW`.

4.  **Default Handling:**
    * If no verdicts remain (Empty Set), apply **Section 6.1**.

## 6. Edge Cases & Fail-Safes

### 6.1. The Vacuum State (Empty Set)
If no policy matches the target scope (or all are filtered out), the system enters a **Vacuum State**.
* **Standard Rule:** The default behavior is **DENY** (Safe Failure).
* **Rationale:** An unmanaged device is an unsafe device. Implicit allowances are forbidden.

### 6.2. The Pre-Law Override
Physical safety interlocks (SIP-008 Pre-Law) exist **outside** this algebra.
* If a hardware interlock triggers (e.g., E-Stop), the software verdict is irrelevant.
* The Synapse Engine MUST report the override but CANNOT veto it.

### 6.3. The Ambiguous Authority Conflict
In the rare theoretical event of "Duplicate Authority" (e.g., two different signers with valid signatures claiming the exact same Authority Level and Scope but giving opposite commands):
* The **Directional Conflict Predicate** applies.
* The system treats them as standard vectors in the same plane.
* **DENY** absorbs **ALLOW**.

## 7. Formal Guarantees
SIP-010 provides the following mathematical guarantees to the Sopcos ecosystem:

1.  **Determinism:** For any given set of inputs, the output is always identical, regardless of computation order, concurrency, or system locale.
2.  **Safety Monotonicity:** Adding a `DENY` verdict to any existing set of `ALLOW` verdicts can **only** change the result to `DENY`. It is mathematically impossible for a new policy to "accidentally" override a denial without possessing higher authority.
3.  **Bounded Termination:** The resolution algorithm is $O(N)$ (Linear Time) and contains no recursion or infinite loops. It is guaranteed to halt.

## 8. Closing Principle
The algebra defined herein is designed to eliminate "interpretation" from the runtime.

> **The machine must not guess. Silence is safer than error.**
>
> *If the math does not yield a clear ALLOW, the answer is NO.*

---
*Copyright © 2025 Sopcos Protocol Foundation. All Rights Reserved.*