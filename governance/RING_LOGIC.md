# ⭕ Ring Logic - The Hierarchy of Reality

> **Status:** Normative Reference  
> **Scope:**  Reality hierarchy, conflict resolution, liability boundaries & cognitive oversight
> **Related:**  SAM (Authority Matrix), SIP-006, SIP-008, SIP-010, SIP-011, SIP-012, SIP-015, SIP-017

SOPCOS applies a **Ring Protection Model**, inspired by operating system kernels, to **industrial liability and automated enforcement**.

Each ring represents a different layer of reality. Higher rings may influence lower rings **only within explicitly defined limits**. Some rings can **never** be overridden.

---

## The Rings

### 🔴 Ring 0 - The Core (Pre-Law)

**Domain:** Hardware & Physics (**Level ∞**)
**Authority:** Absolute Ruler 
**Overridable:** ❌ Never

**Examples**
- Hardwired Emergency Stop
- Thermal fuse
- Physical interlocks
- Material limits (e.g. melting point)

**SOPCOS Role**
- *Synapse Pre-Law*

> Ring 0 represents reality itself (Level ∞). 
> SOPCOS does not argue with physics.

---

### 🟠 Ring 1 - The Constitution

**Domain:** Enforceable Law (SPL) (**Level 1-2**)  
**Authority:** Regulatory & Site Constraints
**Overridable:** ⚠️ Only by Ring 2 (with liability)

**Examples**
- “Do not open door if RPM > 0”
- “Maximum pressure = X bar”
- “Human presence requires reduced speed”

**SOPCOS Role**
- *Synapse Policy Engine*

> Ring 1 is  **Law Made Enforceable** .
> It defines what the system is allowed to do.

---

### 🟡 Ring 2 - The Operator

**Domain:** Human Will & Judgment (**Level 0 / 3**)  
**Authority:** Execution Preemption  
**Overridable:** ❌ Cannot override Ring 0

**Capabilities**
- Cannot modify policy definitions
- **Execution Preemption:** Can suspend Ring 1 policies under SIP-006 semantics.
- **Sovereign Mandate (SIP-012):** Can delegate specific authority to Ring 3 for a fixed time window.
- Cannot bypass physical constraints

**SOPCOS Role**
- *Vinci Wallet Signer*

> Ring 2 exists because humans (Sovereigns) are the sole owners of liability 
> hazards that sensors cannot.

---

### 🟢 Ring 3 - The Intelligence

**Domain:** Cognitive Oversight (**Level 3-4**) 
**Authority:** Recommendation & Advisory  
**Overridable:** Always

**Capabilities**
- **Cognitive Auditor:** Generates probabilistic risk assessments and recommendations.
- **Silent Alarm (SIP-017):** Delivers sensitive alerts via "The Red Phone" without public broadcast
- Perform **only pre-authorized actions**

**Restrictions**
- **No Autonomy:** Cannot hold Level 0 or Level 1 authority [11, 12].
- Cannot initiate Overrides.
- Cannot expand scope

**SOPCOS Role:** 
*Axon* - Intelligence that advises and prepares final Core Ledger records.

> Intelligence without authority is safe.  
> Authority without intelligence is dangerous.  
> SOPCOS separates the two.

---

## ⚖️ Conflict Resolution Rules

### Case 1 - AI vs Policy

If **Ring 3 (AI)** says:
> “Accelerate”

But **Ring 1 (Policy)** says:
> “Limit reached”

**Verdict:**  `DENY` (Authority Dominance) 

**Rationale:** Level 1-2 (Policy) strictly overrides Level 3

---

### Case 2 - Human vs Policy

If **Ring 2 (Human)** says:
> “Open door”

But **Ring 1 (Policy)** says:
> “Unsafe”

**Verdict:** `ALLOW + DIRTY_STATE` (Execution Preemption)

**Rationale:**  
The human preempts the algorithmic verdict and accepts full cryptographic liability

---

### Case 3 - Any Ring vs Physics

If any ring attempts to violate **Ring 0 (Level ∞)**:

**Verdict:** `DENY` (Pre-Law Absolute) 

**Rationale:**  
The system accepts the liability (Override) but acknowledges the impossibility (Physics).

---

## 🛡️ Dirty State Semantics

When Ring 2 overrides Ring 1:

- The system enters **DIRTY STATE**
- **Compliance Debt:** A procedural debt that must be recorded
- All actions are logged with full context
- Normal AI optimization is suspended
- **Recovery:** Requires a Level 5 Auditor's **Reset Transaction**

> Overrides are not shortcuts.  
> They are **explicit admissions of responsibility**.

---

## Relationship to SOPCOS Governance

- **Ring ordering** aligns with the SOPCOS Authority Matrix (SAM)
- **Override rules** derive from SIP-006 (Emergency Semantics)
- **Verdict resolution** is enforced by SIP-010 (Verdict Algebra)
- **Lifecycle authority** is governed by SIP-008

If ambiguity exists, the system **fails closed**.

---

## Final Principle

> SOPCOS does not ask *who is smart*.  
> It proves *who was responsible* -  
> and respects reality above all else.
