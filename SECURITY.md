# Security Policy

## Reporting Vulnerabilities & Incident Handling for SOPCOS

> **Status:** Operational / Normative-Adjacent  
> **Scope:** Security disclosure, incident response, revocation & dirty-state handling  
> **Aligned with:** SIP-006 (Emergency Semantics), SIP-008 (Authority Hierarchy), SIP-014 (Artifact Vault), SIP-017 (Secure M2M Messaging)

SOPCOS is designed for environments where security failures
may result in **physical harm, legal liability, or regulatory breach**.
This document defines how vulnerabilities and incidents are reported,
handled, disclosed, and resolved.

If this document conflicts with binding SIP law,
the SIP **prevails**.

---

## Core Security Principle

> **Security incidents are governance events, not bug tickets.**

When ambiguity exists, SOPCOS systems **fail closed**.

---

## What Constitutes a Security Issue

Security issues include, but are not limited to:

- Ability to bypass authority hierarchy or override semantics
- Expansion of AI authority beyond defined bounds
- Circumvention of policy enforcement or pre-law constraints
- Forged, replayed, or misattributed signatures
- Artifact substitution, vault tampering, or hash collision risks
- Failure to enter, persist, or clear **DIRTY STATE** correctly
- Any condition that may obscure **who authorized what, and when**

---

## Responsible Disclosure

### How to Report

Security issues **must not** be reported via public issues.

Please report privately with:
- A clear description of the issue
- Affected SIPs or components
- Reproduction steps (if applicable)
- Potential safety or liability impact

**Disclosure Channel**
- Contact: security@sopcos.foundation  
- Encrypted contact details may be published separately

---

## Incident Severity Classification

SOPCOS classifies incidents by **liability impact**, not exploit novelty.

### 🟥 Critical (Immediate Action)
- Safety boundary violations
- Authority escalation
- Override misuse
- Evidence integrity compromise

**Response:** Immediate containment, potential revocation, public advisory.

---

### 🟧 High
- Policy bypass under specific conditions
- Incorrect verdict resolution
- Vault access control weaknesses

**Response:** Expedited fix, coordinated disclosure.

---

### 🟨 Medium
- Misleading diagnostics
- Incomplete audit trails
- Non-deterministic edge cases without safety impact

**Response:** Scheduled fix, documented mitigation.

---

### 🟩 Low
- Documentation issues
- Theoretical weaknesses without exploit path

**Response:** Informational update.

---

## Emergency Handling & Dirty State

### Trigger Conditions

An incident triggers **DIRTY STATE** when:
- Emergency overrides are abused or misfiring
- Authority boundaries are violated
- Security assumptions become uncertain

### Effects

When DIRTY STATE is active:
- AI execution paths are suspended
- Only minimum-safe operations are permitted
- All actions are immutably logged
- Liability is explicitly bound to signer DIDs

### Clearance

DIRTY STATE may only be cleared by:
- A qualified **Auditor** role
- A signed **Reset_Transaction**
- After inspection and attestation

---

## Revocation Semantics

When a vulnerability invalidates trust in an artifact, policy, or component:

- **REVOKE** actions may be issued per authority hierarchy
- Revoked artifacts are treated as **null and void**
- Implementations must halt or deny usage of revoked items
- Supersession does **not** override revocation

> Revocation is a kill switch, not a deprecation.

---

## Public Disclosure Policy

SOPCOS favors **responsible transparency**.

- Critical incidents may be disclosed publicly
- Disclosures focus on **impact and resolution**, not exploitation
- Timelines prioritize safety and legal clarity over speed

---

## Compliance & Implementations

- SOPCOS does not certify implementations by default
- Claims of “SOPCOS Compliant” require adherence to:
  - SIP-defined authority boundaries
  - Fail-closed semantics
  - Correct dirty-state handling
  - Proper revocation support

Implementations that violate these constraints
must not claim compliance.

---

## Safe Harbor

We value responsible security research.

Researchers acting in good faith,
without data exfiltration or harm,
and following this disclosure policy,
will not be pursued legally by the SOPCOS Foundation.

---

## Final Note

> SOPCOS treats security as a matter of **trust continuity**.

A system is not insecure because it fails.
It is insecure when responsibility cannot be proven.

Thank you for helping keep SOPCOS accountable.
 