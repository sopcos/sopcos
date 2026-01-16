# SOPCOS LITEPAPER v2.0
### The Industrial Trust Protocol

> **Status:** Informational / Non-Normative  
> **Scope:** Conceptual overview & system narrative  
> **Normative sources:** SOPCOS Improvement Proposals (SIPs)

---

## 🛡️ Executive Anchor

> *“SOPCOS is the industrial operating layer that makes machine decisions legally ownable and non-repudiable.”*

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

> **Speed requires reflex (Synapse).  
> Trust requires record (Core Ledger).  
> Liability requires signature (Human/Vinci).**

- **SOPCOS is not a SCADA.**  
  It is the notary that audits the SCADA.
- **SOPCOS is not an AI project.**  
  It is a governance layer that confines AI within explicit limits.

#### 1.1 The Ring Logic (Hierarchy of Reality)
SOPCOS applies a Ring Protection Model to industrial decision-making:
- **Ring 0 (Level ∞): PHYSICS (Pre-Law).** Hard-coded firmware and physical limits. Cannot be overriden. 
- **Ring 1 (Level 1-2): CONSTITUTION.** SPL policies (Regulatory/Site). Law made executable. 
- **Ring 2 (Level 3): OPERATOR.** Human will and judgment. Sovereignty over software. 
- **Ring 3 (Level 4-5): INTELLIGENCE.** AI, optimization, analytics. Always overridable. 


---

## 2. The Architecture — An Ecosystem, Not a Monolith

SOPCOS is not a single software product.  
It is an organism with distinct, constrained components.

---

### 🔌 2.1 SYNAPSE — The Reflex (Hot Path)

**“The hands and reflexes of the system.”**

The edge component that directly interfaces with machines.

- **Task:** Reads declared reality (**SIP-013**) and enforces policy (**SIP-001**).
- Pre-Law (Level ∞):** Synapse enforces hard interlocks that human overrides cannot bypass (e.g., physical melting points). 
- **Hot Path:**  
  In case of imminent danger, SYNAPSE intervenes **immediately**, deterministically, without latency, and without consultation.

> Safety is created first.  
> Evidence is written second.

---

### 🧠 2.2 AXON — The Cognition (Cold Path, Optional)

**“The Cognitive Auditor.”**

**Privacy (SIP-017):** Critical alerts (e.g., Zero-Day vulnerabilities) are sent via "The Red Phone" directly to the admin, keeping secrets off the public ledger. 
**Cognitive Constraint:** AI can recommend but cannot authorize Level 0/1 actions.

AXON may **recommend**.  
It never authorizes.

---

### 🏛️ 2.3 CORE LEDGER — The Immutable Record

**“The memory and notary of the system.”**

- Does not decide  
- Does not judge  
- Only **proves**

**Vault Protocol (SIP-014):** Artifacts (WASM, Manifests) are stored in WORM (Write Once, Read Many) compliant vaults. The chain holds the truth; the vault holds the weight. 

---

### 🆔 2.4 IDAS — Industrial Digital Assets

**Industrial NFTs (IDAS):** Machines and AI models are unique, transferable digital twins with verifiable provenance and service history. 

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

- **Human Override:**  Overrides are allowed, but under **SIP-006**, they require a signed  
  **Confession of Liability**. Responsibility becomes explicit.
  **Dirty State:** Any override triggers a "Dirty State" (Compliance Contamination). Only an independent Auditor can sign a **Reset_Transaction** to restore "Clean State."
  **Sovereign Mandate (SIP-012):** Allows humans to delegate authority to machines for a specific time window, solving the "Monday Signature, Friday Accident" gap. 
- **Data Sovereignty:** Data remains in sovereign vaults. Only hashes are anchored.

---

## 5. Conclusion

SOPCOS moves AI and distributed ledgers beyond hype and into **industrial-grade governance infrastructure**.

We are not making machines smarter.  
We are making **decisions accountable**.

**SOPCOS — Trust, Executed.**
 