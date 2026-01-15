# SOPCOS LITEPAPER v2.0
### The Industrial Trust Protocol

> **Status:** Informational / Non-Normative  
> **Scope:** Conceptual overview & system narrative  
> **Normative sources:** SOPCOS Improvement Proposals (SIPs)

---

## 🛡️ Executive Anchor

> *“SOPCOS is the industrial operating layer that makes machine decisions legally ownable.”*

Industry 4.0 focused on speed.  
Industry 5.0 focused on human–machine collaboration.  

SOPCOS completes the missing piece: **liability**.

> *“We combine the speed of automation with immutable records  
> and the certainty of law.”*

---

## 1. The Concept — Why SOPCOS?

Current industrial systems focus on **data**.  
SOPCOS focuses on the **verdict**.

When an accident occurs, SCADA logs can show **what** happened.  
They cannot prove **why** it happened, or **who is responsible**, with legal certainty.

The core thesis of SOPCOS is simple:

> **Speed requires reflex.  
> Trust requires record.  
> Liability requires signature.**

- **SOPCOS is not a SCADA.**  
  It is the notary that audits the SCADA.
- **SOPCOS is not an AI project.**  
  It is a governance layer that confines AI within explicit limits.

---

## 2. The Architecture — An Ecosystem, Not a Monolith

SOPCOS is not a single software product.  
It is an organism with distinct, constrained components.

---

### 🔌 2.1 SYNAPSE — The Reflex (Hot Path)

**“The hands and reflexes of the system.”**

The edge component that directly interfaces with machines.

- **Task:** Reads declared reality (**SIP-013**) and enforces policy (**SIP-001**).
- **Pre-Law:** Absolute physical and engineering constraints.
- **Hot Path:**  
  In case of imminent danger, SYNAPSE intervenes **immediately**,  
  deterministically, without latency, and without consultation.

> Safety is created first.  
> Evidence is written second.

---

### 🧠 2.2 AXON — The Cognition (Cold Path, Optional)

**“The reasoning capability of the system.”**

- **Bicameral Mind:**  
  Decision (Synapse) is separated from cognition (Axon).
- **Cold Path:**  
  Performs long-horizon analysis and predictive maintenance.
- **Silent Alarm:**  
  Using **SIP-017**, critical insights are delivered privately via encrypted channels.

AXON may **recommend**.  
It never authorizes.

---

### 🏛️ 2.3 CORE LEDGER — The Immutable Record

**“The memory and notary of the system.”**

- Does not decide  
- Does not judge  
- Only **proves**

Using the **SIP-014 Vault** architecture, only cryptographic fingerprints
are anchored. Evidence outlives infrastructure failure.

---

### 🆔 2.4 IDAS — Industrial Digital Assets

**“Machines with identity.”**

Under **SIP-016**, industrial devices carry immutable identities:

- Ownership
- Maintenance history
- Authority and licensing constraints

Unauthorized components cannot silently enter the system.

---

## 3. The Workflow — Hot Path vs Cold Path

### 🔥 Scenario 1 — Imminent Danger

- **Event:** Boiler pressure reaches 150 bar (limit: 140).
- **Synapse:** Reflex engages. AI is bypassed.
- **Action:** Valve command issued in <10 ms.
- **Record:** Intervention is immutably sealed.

---

### ❄️ Scenario 2 — Predictive Insight

- **Event:** Pressure normal, abnormal vibration detected.
- **Axon:** Analyzes historical patterns.
- **Result:** “Valve gasket ~80% worn.”
- **Delivery:** Encrypted maintenance alert via SIP-017.

---

## 4. The Guarantee — Trust & Liability

SOPCOS provides a **legal security envelope**, not just technology.

- **Human Override:**  
  Overrides are allowed, but under **SIP-006**, they require a signed  
  **Confession of Liability**. Responsibility becomes explicit.
- **Proof of Foreknowledge:**  
  **SIP-005** simulations eliminate “we did not know” defenses.
- **Data Sovereignty:**  
  Data remains in sovereign vaults. Only hashes are anchored.

---

## 5. Conclusion

SOPCOS moves AI and distributed ledgers beyond hype  
and into **industrial-grade governance infrastructure**.

We are not making machines smarter.  
We are making **decisions accountable**.

**SOPCOS — Trust, Executed.**
