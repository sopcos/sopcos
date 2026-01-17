# SOPCOS IMPROVEMENT PROPOSAL (SIP-010)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 010 |
| **Title** | The Verdict Algebra (Decision Resolution Logic) |
| **Status** | LIVING STANDARD (REVISED) |
| **Type** | Standards Track (Core) |
| **Layer** | Synapse (Runtime) |
| **Motto** | Decisions are vectors. Conflicts are structural, not exceptional. |
| **Related** | SIP-008, SIP-009, SIP-003, SIP-011 |

## 1. Abstract
This specification defines the deterministic mathematical rules for resolving multiple **Policy Verdicts** into a single **Final Verdict**. It defines how multiple, simultaneous verdicts-originating from different authorities, scopes, and times-are composed into a single enforceable outcome.

SIP-010 eliminates:
* Arbitrary priority integers.
* Implicit “last-write-wins” logic.

It replaces them with:
* A normative authority hierarchy (SIP-008).
* A jurisdictional scope lattice.
* A mathematically deterministic resolution algorithm (O(N)).

## 2. Motivation
In any non-trivial Sopcos deployment, a target scope may be subject to multiple simultaneous verdicts. Without a formal algebra, systems revert to fragile heuristics.

SIP-010 establishes a foundational axiom:
> **"Conflict is not an error state. Conflict is the primary state."**

## 3. The Verdict Vector
Mathematically, a Verdict `V` is a 4-dimensional vector defined as:

$$V = \langle \delta, \alpha, \sigma, \tau \rangle$$

Where:
1.  **$\delta$ (Decision - Direction):** The scalar enumeration.
    * `HALT = 3`
    * `WARN = 2`
    * `ALLOW = 1`
    * `DENY = 0`
2.  **$\alpha$ (Authority - Magnitude):** The scalar weight derived from SIP-008 (Lower Value = Higher Rank).
3.  **$\sigma$ (Scope - Domain):** The spatial set where this force is applied.
4.  **$\tau$ (Timestamp - Validity):** The temporal coordinate used for freshness checks.

## 4. Formal Model & Resolution Logic

### 4.1. Verdict Space (V)
Let $V$ be the set of all possible well-formed verdict vectors.
$$v \in V \implies v = \langle \delta, \alpha, \sigma, \tau \rangle$$

### 4.2. Applicability Predicate ($\vdash$)
An operator defining if a verdict is juridically valid for a specific target scope $t$.
$$v \vdash t \iff t \subseteq v.\sigma$$

### 4.3. Authority Dominance Relation ($\succ$)
A strict partial order defining the hierarchy.
$$v_1 \succ v_2 \iff v_1.\alpha < v_2.\alpha$$
*(Note: Per SIP-008, Level 0 is greater than Level 4).*

### 4.4. Verdict Composition Operator ($\oplus$)
The binary operator resolving two verdicts of **Equal Authority** ($v_1.\alpha = v_2.\alpha$):

1.  **HALT absorbs everything.** ($HALT \oplus X = HALT$)
2.  **DENY absorbs ALLOW and WARN.** ($DENY \oplus ALLOW = DENY$)
3.  **WARN contaminates ALLOW.** ($ALLOW \oplus WARN = WARN$)
4.  **ALLOW is the identity element** only if no negative force exists.

## 5. Execution Algorithm (Normative)
Implementations of the Synapse Engine **MUST** strictly adhere to the following pipeline.

**Algorithm:** `Resolve(Context, Verdicts[]) -> FinalVerdict`



### Step 0: The Liability Preemption (SIP-011 Check)
* Check `Context.OverrideToken`.
* **If Valid:** STOP. Return logic defined in SIP-011 (Forensic Liability).
* **Rationale:** Human liability preempts algorithmic calculation.

### Step 1: Validation & Applicability ($\Phi$)
* Discard vectors where `TargetScope` is not in `v.Scope`.
* Discard vectors where `v.τ` (Timestamp) is outside the allowed **Freshness Window** (Replay Protection).
* **Result:** `ValidSet`.

### Step 2: Authority Projection ($\Pi$)
* If `ValidSet` is empty, goto Step 4.
* Find `BestAuthority = min(v.α)` in `ValidSet`.
* Discard all verdicts where `v.α > BestAuthority`.
* **Result:** `ProjectedSet` (All items share the highest available authority).

### Step 3: Composition ($\Sigma$)
* Initialize `FinalDecision = ALLOW`.
* Iterate through `ProjectedSet`:
    * If any `v.δ == HALT` → Return `HALT`.
    * If any `v.δ == DENY` → Set `FinalDecision = DENY` (and continue to check for HALT).
    * If any `v.δ == WARN` AND `FinalDecision != DENY` → Set `FinalDecision = WARN`.
* Return `FinalDecision`.

### Step 4: Default Handling (Vacuum State)
* If no verdicts remain (Empty Set):
* Return `DENY` (Fail-Closed).

## 6. Edge Cases

### 6.1. The Pre-Law Override (Physics)
SIP-008 **Level ∞ (Pre-Law)** constraints are handled by firmware/hardware. If the Algebra outputs `ALLOW` but the Hardware Interlock says `NO`, the physical reality prevails.

### 6.2. The Warn State
A `WARN` verdict implies: *"The operation is permitted (ALLOW), but a specific risk flag MUST be logged in the Decision Record (SIP-003)."*

## 7. Formal Guarantees
1.  **Determinism:** The output is independent of computation order.
2.  **Safety Monotonicity:** Adding a `DENY` policy to a set can never inadvertently flip the result to `ALLOW`.
3.  **Bounded Termination:** The algorithm is **O(N)**.