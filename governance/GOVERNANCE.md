# SOPCOS Governance
## Constitutional Rules for the SOPCOS Protocol Repository

> **Status:** Meta / Non-Normative  
> **Scope:** Repository governance & change control  
> **Normative Sources:** SIP-002 (Governance, Roles & Licensing), SIP-008 (Authority Hierarchy & Lifecycle Governance)

This document defines how the SOPCOS repository is managed, how specifications evolve, and how binding protocol law is ratified, revised, superseded, or revoked.

If this document conflicts with a **LIVING STANDARD** SIP, the SIP prevails.

---

## 1. Governance Model

SOPCOS follows a model of **Controlled Freedom**: open participation with strict constraints designed to prevent fragmentation and preserve industrial-grade accountability. 

### 1.1 The Foundation (Steward, Not Dictator)

The Foundation is the protocol steward:
- Manages the SIP process
- Holds trademark stewardship
- Coordinates releases and canonical documentation

**Hard constraint:** The Foundation cannot finalize a SIP or introduce breaking changes to core SPL without technical consensus from **at least 3 independent Core Maintainers** (not employed by the same legal entity).

---

## 2. Roles & Responsibilities

### 2.1 Core Maintainers
Core Maintainers:
- Review SIPs for determinism, safety, and conflict consistency
- Gate changes to the Immutable Kernel boundary
- Provide the consensus required for ratification and breaking change approval (see Section 3)

**Independence requirement:** “3 independent” means **not employed by the same legal entity**.

### 2.2 SIP Editors (Process Operators)
SIP Editors ensure:
- Metadata consistency
- Correct status labeling
- Index updates in `SIPs/README.md`
- Editorial clarity without altering normative meaning

### 2.3 Policy Authors & Operational Oversight
Policy authors define rules but do not write protocol code. SPL policies must be signed according to authority rules.

### 2.4 Role Mapping (RBAC via Credentials)
Operational roles (e.g., CISO, HSE Officer, Metrologist) are bound to DIDs via a signed Role Verifiable Credential, issued by the Foundation or enterprise admin, and verified by runtime for restricted actions.

---

## 3. Decision-Making & Ratification

### 3.1 What Counts as “Protocol Law”
A SIP is binding protocol law only when its status is:
- **LIVING STANDARD** (including “revised” variants)

All other statuses are non-binding.

### 3.2 Consensus Threshold for Binding Changes
To ratify a SIP into binding law, or to approve breaking changes affecting the core SPL/kernel semantics:
- The Foundation process must be completed **and**
- Technical consensus must be recorded from **≥ 3 independent Core Maintainers**.

### 3.3 Non-Breaking Revisions vs Breaking Changes
- **Non-breaking revisions** (clarifications, errata, disambiguation) may be merged with standard maintainer review.
- **Breaking changes** to the Immutable Kernel boundary require the consensus threshold above.

---

## 4. The Immutable Kernel Boundary

SIP-002 defines an **IMMUTABLE Kernel**. Modifying these logics disqualifies a node from SOPCOS compliance.

Immutable Kernel includes (non-exhaustive, canonical list):
1. SPL Parser (SIP-001)
2. Decision Engine (SIP-010)
3. Audit Commit Logic
4. Cryptographic Primitives (incl. envelope crypto per SIP-009)
5. Deterministic Simulation Engine (SIP-005)
6. Artifact Resolution Logic (SIP-014 path/URN verification; no arbitrary URLs)
7. Industrial Asset Engine (SIP-016 OpCodes 40–49)

**Evolution Clause:** The Kernel can ONLY be updated through the official SIP process managed by the Foundation; out-of-process modifications lose “Sopcos Compliant” status. 

---

## 5. Authority Model & Lifecycle Governance

SOPCOS authority is derived from the **signer DID anchored on Sopcos Chain**, not from file metadata.

### 5.1 Authority Levels (Execution)
SOPCOS defines a strict hierarchy (lower number = higher authority):
- **∞ PRE-LAW:** Physics / bio-safety / hard-coded interlocks (non-overridable)
- **LVL 0 EMERGENCY**
- **LVL 1 REGULATORY**
- **LVL 2 SITE / ORG**
- **LVL 3 OPTIMIZATION** (default for AI agents)
- **LVL 4 ADVISORY**

**Constraint:** AI agents cannot hold LVL 0 or LVL 1 authority.

### 5.2 Artifact Lifecycle Permissions (REGISTER / SUPERSEDE / REVOKE)
Lifecycle governance follows the permission matrix:
- **REGISTER:** any valid DID may register artifacts at their own level
- **SUPERSEDE:** author may supersede their own artifact; higher authority may supersede lower authority; lower/equal authority cannot supersede higher authority
- **REVOKE:** author may revoke their own artifact; LVL 0/1 may revoke lower artifacts as “supreme court”

---

## 6. Repository Change Control

### 6.1 What Requires a SIP
A SIP is required for any change that:
- Adds/changes protocol semantics
- Touches Immutable Kernel boundary
- Alters authority, lifecycle, or verdict logic
- Changes signing, evidence, or audit semantics

### 6.2 What Does Not Require a SIP
No SIP required for:
- Typos, formatting, and editorial clarity that does not change meaning
- Non-normative explanatory docs (release notes, lifecycle diagrams)
- Examples and appendices (unless they implicitly redefine behavior)

### 6.3 Required PR Contents for SIP Changes
Any PR proposing SIP changes MUST include:
- The updated SIP file
- Updated `SIPs/README.md` index entry (status, layer)
- A short “Compatibility & Risk” note (even if “N/A”)
- Explicit statement: “Non-breaking revision” vs “Breaking change”

---

## 7. Licensing & Contributions (High-Level)

SIP-002 defines the licensing strategy and “network copyleft” model for core code. This document does not restate licenses; it defines governance flow.

Contributions are welcome, but:
- Compliance claims (“Sopcos Compliant”) are reserved for implementations that obey the Immutable Kernel boundary and SIP process.

---

## 8. Dispute Handling

When disputes arise:
1. Interpret the canonical SIP text first
2. Apply authority and lifecycle rules (SIP-008)
3. If ambiguity remains, default to fail-closed semantics and request formal SIP clarification

---

## 9. Final Principle

> SOPCOS governance is designed to prevent a liability vacuum.  
> Change is allowed. Ambiguity is not.
 