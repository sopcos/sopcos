---
sip: 019-A
title: Canonical Industrial Incident Scenario - Boiler Override Profile
status: Draft
type: Informational / Reference Standard
layer: Cross-Layer
author: Sopcos Foundation
created: 2026-01-18
related-sips:
  - SIP-001
  - SIP-005
  - SIP-006
  - SIP-008
  - SIP-011
  - SIP-013
  - SIP-014
  - SIP-016
  - SIP-017
  - SIP-018
---

# SIP-019-A: Canonical Industrial Incident Scenario  
## Boiler Override Profile

> **Safety is enforced by physics.  
> Responsibility is enforced by record.**

---

## 1. Purpose and Scope

This document defines **Profile A** of the SIP-019 Scenario Standards family.

It provides a **canonical, end-to-end reference scenario** demonstrating how SOPCOS components interact during a real industrial incident involving:

- automated safety enforcement,
- predictive analysis,
- explicit human override,
- and post-incident forensic liability attribution.

This SIP introduces **no new protocol rules**.  
All behavior described herein is derived from existing SOPCOS Improvement Proposals.

---

## 2. Initial State - Normal Operation (Clean State)

A high-pressure industrial boiler operates under SOPCOS governance.

The boiler is registered as an **Industrial Digital Asset (IDAS)** under **SIP-016** and possesses:
- a persistent Decentralized Identifier (DID),
- a verified maintenance and ownership history,
- and an active policy envelope defined via **SIP-001**.

A **Synapse** node is deployed at the edge, enforcing deterministic safety limits in real time.

An **Axon** node is connected in cold-path mode for analysis and forecasting only.
Axon has no execution authority.

The system operates with an active **Telemetry Binding Manifest**
as defined in **SIP-013**, stored immutably in a **WORM-compliant Vault (SIP-014)**, attesting to:
- sensor calibration,
- physical installation integrity,
- and reality binding of incoming telemetry.

The **Core Ledger (L1)** reflects a **Clean State** for the asset.
All relevant artifacts (policies, manifests, models) are referenced by hash.

---

## 3. Predictive Signal - Axon Analysis (Cold Path)

Over time, Axon observes:
- increasing micro-vibration,
- anomalous acoustic resonance,
- and deviation from historical operating baselines.

Using **SIP-005 simulation semantics**, Axon projects an elevated failure risk within an upcoming operational window.

Axon does **not**:
- issue commands,
- alter system state,
- or interact with actuators.

A confidential foreknowledge alert is transmitted to the factory administrator via **SIP-017 (Red Phone)**.

**Normative Note:**  
Axon output constitutes *foreknowledge*, not authority.

---

## 4. Safety Intervention - Synapse Enforcement (Hot Path)

Shortly thereafter, real-time boiler pressure exceeds the deterministic threshold defined in the active policy envelope.

**Synapse** engages reflexively under **Pre-Law constraints**.

A physical mitigation action (e.g., pressure relief valve activation) is executed within milliseconds.

Synapse emits an automated verdict and anchors it on the **Core Ledger** via `OP_LOG_VERDICT (0x10)`.

The system remains in **Clean State**.

**Normative Note:**  
The Core Ledger records *that* an intervention occurred, not *whether the outcome was optimal*.

---

## 5. Human Override - Vinci Intervention (Dirty State)

An on-site operator observes contextual factors not visible to sensors (e.g., external fire risk, structural obstruction, evacuation dynamics).

The operator decides to override the automated safety action.

Using the **Vinci Wallet**, the operator is required to select a **Justification Code** as defined in **SIP-006**.

In this scenario, the operator selects:

**CLASS_C - Continuity Override**  
(to prevent catastrophic production loss)

The operator cryptographically signs:
- the override request,
- the selected justification,
- and explicit acceptance of liability.

The override is anchored on the Core Ledger via `OP_CONFESSION (0x11)`.

Immediate consequences:
- The system enters **Dirty State**.
- Operational authority shifts from automated enforcement to human mandate.
- Legal liability is cryptographically transferred to the signing identity.

**Normative Note:**  
Override is permitted, but **never silent**.

### Pre-Law Boundary Condition

The human override remains strictly constrained by **Pre-Law (Level ∞)** physical limits as defined in **SIP-011**.

Had the operator attempted to exceed absolute physical constraints (e.g., material melting point), the system would have rejected the command due to **physical impossibility**, regardless of human mandate.

---

## 6. Forensic Mode - Black Box Protocol

Upon execution of `OP_CONFESSION (0x11)`,
the system immediately enters **Forensic Mode** as defined in **SIP-006**.

A cryptographic **Forensic Snapshot** is created, sealing:
- sensor data hashes,
- policy references,
- telemetry manifests,
- and system state context.

This snapshot activates the **Black Box Protocol** and is immutably anchored on the Core Ledger.

This mechanism prevents post-incident claims such as “the sensors were unreliable” or “the data was altered after the fact.”

---

## 7. Incident Resolution & Evidence Preservation

The incident concludes (with or without physical damage).

All relevant evidence is preserved:
- raw telemetry and logs in a **WORM Vault (SIP-014)**,
- policies, models, and manifests by cryptographic reference,
- event ordering sealed by the **Core Ledger**.

An **Incident Evidence Package (IEP)** is assembled per **SAF v1.1**, ensuring that evidence integrity is mathematically enforceable.

---

## 8. Audit & State Restoration

A Level 5 Auditor reviews the IEP.

If procedures were followed correctly, the auditor signs a `OP_STATE_RESET (0x12)` transaction.

The system returns to **Clean State**.
The audit itself becomes part of the immutable record.

---

## 9. Normative Derivations

From this scenario, the following interpretations apply:

- **SIP-001:** Policies constrain automation but do not eliminate human agency.
- **SIP-005:** Foreknowledge removes plausible deniability.
- **SIP-006:** Override requires explicit justification and liability acceptance.
- **SIP-011:** No authority may violate physical law.
- **SIP-013:** Reality binding is required for admissible telemetry.
- **SIP-017:** Sensitive risk communication need not be publicly disclosed.
- **SIP-018:** The Core Ledger provides evidence, not judgment.
- **SAF v1.1:** Liability attribution is derived from cryptographic fact, not testimony.

---

## 10. Conclusion

This canonical scenario demonstrates that SOPCOS does not attempt to prevent every industrial incident.

Instead, it ensures that when incidents occur:
- safety is enforced first,
- responsibility is explicit,
- and disputes are resolved through evidence rather than interpretation.

**Trust is not assumed.  
It is executed.**
