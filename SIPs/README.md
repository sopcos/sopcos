# 📜 SOPCOS Improvement Proposals (SIPs)

## Governance, Evolution & Canonical Law of the SOPCOS Protocol

> **SIPs are not feature requests.  
> SIPs are law.**

The SOPCOS Improvement Proposal (SIP) system defines how the protocol is specified, evolved, interpreted, and held legally accountable.  
Unlike informal RFC-style processes, SIPs function as a **normative governance and legal framework**.

Each SIP has an explicit **Status**, **Scope**, and **Authority**.  
If it is not ratified here, it is not law.

---

## SIP Status Definitions

| Status | Meaning |
|------|--------|
| **LIVING STANDARD** | Canonical and enforceable. May receive non-breaking revisions via the SIP lifecycle. |
| **PROPOSED STANDARD** | Technically mature, under formal review. Not yet binding. |
| **DRAFT** | In design or infrastructure phase. Subject to change. |
| **INFORMATIONAL** | Explanatory or illustrative. Not normative. |
| **NON-NORMATIVE** | Examples, walkthroughs, appendices. |

---

## SIP Index (Canonical)

### 🧠 Core Logic, Authority & Liability (Kernel Law)

| SIP | Title | Status | Layer |
|----|------|--------|-------|
| **SIP-001** | Sopcos Policy Language (SPL) Definition | **LIVING STANDARD** | Authoring / Parsing |
| **SIP-002** | Governance, Roles & Licensing Strategy | **LIVING STANDARD** | Governance |
| **SIP-003** | Decision Semantics & Canonical State | **LIVING STANDARD** | Runtime / Legal |
| **SIP-004** | Threat Model & Accountability Framework | **LIVING STANDARD** | Security / Legal |
| **SIP-005** | Policy Simulation & What-If Semantics | **LIVING STANDARD** | Authoring / Runtime |
| **SIP-006** | Operational Intervention (Override) Semantics | **LIVING STANDARD** | Governance / Runtime |
| **SIP-008** | Authority Hierarchy & Lifecycle Governance | **LIVING STANDARD** | Runtime / Resolution |
| **SIP-010** | Verdict Algebra (Decision Resolution Logic) | **LIVING STANDARD** | Runtime |
| **SIP-011** | Forensic Liability & Operational Semantics | **LIVING STANDARD** | Runtime / Legal |

> These SIPs collectively define **who decides, how decisions are resolved, and who is legally responsible**.

---

### 🧾 Identity, Reality & Evidence (Forensic Chain)

| SIP | Title | Status | Layer |
|----|------|--------|-------|
| **SIP-009** | Cryptographic Policy Envelope (.sop) | **LIVING STANDARD** | Tooling / Runtime |
| **SIP-013** | Declared Reality Interface & Telemetry Binding | **LIVING STANDARD** | Metrology / Hardware |
| **SIP-014** | Decentralized Artifact Vault & Lifecycle Registry | **DRAFT** | Infrastructure |

> These SIPs ensure that **law, reality, and evidence remain cryptographically bound** across time.

---

### 🤖 Delegation, AI & Cognitive Systems

| SIP | Title | Status | Layer |
|----|------|--------|-------|
| **SIP-012** | Sovereign Mandate & Delegated Agency | **PROPOSED STANDARD** | Governance / Runtime |
| **SIP-015** | Cognitive Verdicts & Agentic Consensus | **PROPOSED STANDARD** | L2 / Axon |

> SOPCOS explicitly rejects autonomous liability.  
> AI may **recommend**, but only humans **authorize**.

---

### 🏭 Assets, Networking & Extended Capabilities

| SIP | Title | Status | Layer |
|----|------|--------|-------|
| **SIP-016** | Industrial Digital Assets (IDAS) | **DRAFT** | Core / Assets |
| **SIP-017** | Secure M2M Messaging (S-M2M) | **PROPOSED STANDARD** | Networking / Privacy |

---

### 📚 Informational & Reference

| Document | Status |
|--------|--------|
| **SIP-Appendix-A** – Canonical Incident Walkthrough | **INFORMATIONAL / NON-NORMATIVE** |
| **Glossary** | **REFERENCE** |
| **Release_Strategy_v1.0.md** | **META / NON-NORMATIVE** |

---

## Release Strategy

The SOPCOS release strategy documents **how canonical SIP law is introduced to the public**.
These documents:

- Are **not SIPs**
- Do **not** define protocol law
- Do **not** modify governance rules

They exist to declare **scope, intent, and boundaries** of a specific public release.

- 👉 **[Release Strategy v1.0 — “Anchor Release”](./Release_Strategy_v1.0.md)**

---

## What SIPs Are *Not*

- Not a DAO governance process  
- Not “rough consensus and running code”  
- Not optional documentation  
- Not community voting artifacts  

**If a behavior is not specified in a ratified SIP, it MUST be treated as undefined or forbidden.**

---

## Lifecycle & Authority

- SIPs are authored, revised, superseded, or revoked **only** via the governance rules in **SIP-002** and **SIP-008**.
- Lower-authority actors **cannot** override higher-authority law.
- Deprecated or revoked SIPs remain **historically verifiable**.

---

## Final Principle

> **SOPCOS does not ask who is powerful.  
> It proves who was responsible.**
