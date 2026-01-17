# 🔁 SIP Lifecycle Diagram (Textual Spec)
## SOPCOS Improvement Proposal Process - State Machine & Transition Rules

> **Purpose:** Define the canonical lifecycle of a SIP document as a governance-controlled artifact.
> This is a **meta / non-normative** specification intended to remove ambiguity from process.
> Normative authority is derived from SIP-002 (Governance) and SIP-008 (Authority/Lifecycle Governance).

---

## 1. Lifecycle Overview

A SIP moves through a small, explicit set of states.

       ┌───────────────┐
       │     IDEA      │
       │ (off-chain)   │
       └───────┬───────┘
               │ submit
               ▼
       ┌───────────────┐
       │     DRAFT     │
       │ (non-binding) │
       └───────┬───────┘
               │ promote
               ▼
       ┌────────────────────┐
       │  PROPOSED STANDARD │
       │ (review candidate) │
       └───────┬────────────┘
               │ ratify
               ▼
 ┌────────────────────────────┐
 │      LIVING STANDARD       │
 │ (binding canonical law)    │
 └───────┬───────────┬────────┘
         │ revise    │ deprecate
         ▼           ▼
┌────────────────┐   ┌────────────────┐
│ REVISED LAW    │   │ DEPRECATED     │
│ (still living) │   │ (non-binding)  │
└───────┬────────┘   └───────┬────────┘
│   supersede       │     archive
▼                   ▼
┌────────────────┐ ┌────────────────┐
│ SUPERSEDED     │ │ ARCHIVED       │
│ (historical)   │ │ (historical)   │
└───────┬────────┘ └────────────────┘
│   revoke
▼
┌────────────────┐
│ REVOKED        │
│ (null & void)  │
└────────────────┘



### State Meaning (Short)
- **DRAFT:** Working document, not binding.
- **PROPOSED STANDARD:** Review candidate. Not binding but stable enough for evaluation.
- **LIVING STANDARD:** Binding canonical law. Implementations MUST follow.
- **REVISED LAW:** Same as LIVING STANDARD, but explicitly revised (still binding).
- **DEPRECATED:** Not recommended. Not binding for new deployments (kept for history).
- **SUPERSEDED:** Replaced by a newer SIP/version (kept for forensic continuity).
- **REVOKED:** Declared dangerous or legally invalid (kill-switch semantics).
- **ARCHIVED:** Frozen, retained purely for recordkeeping.

> **Canonical mapping rule:** “RATIFIED” is represented as **LIVING STANDARD** in SOPCOS repositories.

---

## 2. Minimal Transition Table

| From | To | Trigger | Binding? | Typical Reason |
|------|----|---------|----------|----------------|
| IDEA | DRAFT | Submit SIP | No | Start formalization |
| DRAFT | PROPOSED STANDARD | Promote | No | Ready for review |
| PROPOSED STANDARD | LIVING STANDARD | Ratify | Yes | Accepted as law |
| LIVING STANDARD | REVISED LAW | Revise | Yes | Non-breaking correction |
| LIVING STANDARD | DEPRECATED | Deprecate | No (for new) | Replaced conceptually |
| REVISED LAW | SUPERSEDED | Supersede | No (old) | Replaced by vNext |
| Any (except ARCHIVED) | REVOKED | Revoke | No | Security/legal invalidation |
| DEPRECATED | ARCHIVED | Archive | No | Historical freeze |
| SUPERSEDED | ARCHIVED | Archive | No | Historical freeze |

---

## 3. Authority & Who Can Move a SIP

SOPCOS does not use popularity voting for protocol law.
Authority is derived from:
- **Governance constraints** (SIP-002)
- **Normative authority graph and lifecycle governance** (SIP-008)

### 3.1 Roles (High-level)
- **The Foundation (Steward):** Maintains SIP process and canonical registry constraints.
- **Core Maintainers:** Technical consensus gate (SIP-002 constraint).
- **Higher Authority Actors (SIP-008):** May supersede/revoke lower-level artifacts where applicable.

### 3.2 Promotion / Ratification Constraint (SIP-002)
A SIP cannot become binding law (LIVING STANDARD) without:
- Foundation process completion
- AND technical consensus of **≥ 3 independent Core Maintainers** (not employed by the same legal entity)

---

## 4. Normative Rules for Each Transition

### 4.1 IDEA → DRAFT (Submit)
**Requirement**
- A SIP file exists with required metadata header:
  - SIP number, title, type, status, created date, author, layer, requires/related

**Result**
- SIP is visible and referenceable, but non-binding.

---

### 4.2 DRAFT → PROPOSED STANDARD (Promote)
**Requirements**
- Document is complete enough to be reviewed:
  - clear scope
  - normative language for MUST/SHOULD where applicable
  - backwards compatibility notes (if relevant)
  - security considerations (if relevant)

**Result**
- Ready for public review. Still not binding law.

---

### 4.3 PROPOSED STANDARD → LIVING STANDARD (Ratify)
**Requirements**
- Governance: Foundation acceptance + maintainer consensus threshold (SIP-002).
- Consistency: Must not contradict existing LIVING STANDARDS without explicitly superseding them.
- Determinism: Must preserve fail-closed / safety monotonicity expectations when touching runtime semantics.

**Result**
- Binding canonical law. Implementations MUST comply.

---

### 4.4 LIVING STANDARD → REVISED LAW (Revise)
This is a **non-breaking** update.

**Allowed Changes**
- Clarifications
- Errata fixes
- Tightening ambiguity
- Adding examples
- Strengthening security notes without changing protocol behavior

**Forbidden**
- Breaking changes without passing through a supersede path.

**Result**
- Still binding law (same status category: LIVING).

---

### 4.5 LIVING STANDARD → SUPERSEDED (Supersede)
Supersede means a newer SIP/version becomes the active law.

**Requirements**
- A replacement SIP (or a new version) exists and is ratified.
- The old SIP must explicitly reference the new canonical target.

**Result**
- Old SIP remains verifiable, but not the active law.

---

### 4.6 Any → REVOKED (Revoke)
Revocation is a **kill-switch** action.

**Allowed Reasons**
- Security vulnerability
- Legal invalidation
- Dangerous semantics discovered post-ratification

**Effects**
- REVOKED SIP is treated as “null and void”.
- Implementations must treat revoked artifacts as invalid (aligned with lifecycle registry semantics).

---

### 4.7 DEPRECATED / SUPERSEDED → ARCHIVED
Archive freezes the document for history.

**Requirements**
- Document is no longer active law
- Historical retention needed for audit continuity

**Result**
- Immutable reference, not active.

---

## 5. Artifact Lifecycle Alignment (Vault/Registry)

When SIPs are treated as first-class artifacts (or reference artifacts), lifecycle actions align with:
- **REGISTER / SUPERSEDE / REVOKE** semantics (infrastructure layer)

Practical rule:
- **SUPERSEDE** preserves forensic continuity: old remains retrievable and verifiable.
- **REVOKE** declares the artifact dangerous/invalid: nodes must halt/ignore as defined by runtime policy.

---

## 6. Implementation Notes (Repo Hygiene)

Recommended conventions:
- SIP filename: `sip-XYZ-short-title.md`
- Stable links from `SIPs/README.md`
- Status must be updated inside the SIP metadata table and reflected in the index.

---

## 7. Final Principle

> **Change is permitted.  
> Ambiguity is not.**

The SIP lifecycle exists to ensure SOPCOS evolves without creating a liability vacuum.
