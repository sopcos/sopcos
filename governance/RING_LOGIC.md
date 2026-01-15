# ⭕ Ring Logic — The Hierarchy of Reality

> **Status:** Normative Reference  
> **Scope:** Reality hierarchy, conflict resolution & liability boundaries  
> **Related:** SOPCOS Authority Matrix (SAM), SIP-006, SIP-008, SIP-010

SOPCOS applies a **Ring Protection Model**, inspired by operating system kernels,
to **industrial liability and automated decision-making**.

Each ring represents a different layer of reality.
Higher rings may influence lower rings **only within explicitly defined limits**.
Some rings can **never** be overridden.

---

## The Rings

### 🔴 Ring 0 — The Core (Pre-Law)

**Domain:** Hardware & Physics  
**Authority:** Absolute  
**Overridable:** ❌ Never

**Examples**
- Hardwired Emergency Stop
- Thermal fuse
- Physical interlocks
- Material limits (e.g. melting point)

**SOPCOS Role**
- *Synapse Pre-Law*

> Ring 0 represents reality itself.  
> SOPCOS does not argue with physics.

---

### 🟠 Ring 1 — The Constitution

**Domain:** SOPCOS Policy (SPL)  
**Authority:** Legal, safety, and regulatory limits  
**Overridable:** ⚠️ Only by Ring 2 (with liability)

**Examples**
- “Do not open door if RPM > 0”
- “Maximum pressure = X bar”
- “Human presence requires reduced speed”

**SOPCOS Role**
- *Synapse Policy Engine*

> Ring 1 is **law made executable**.  
> It defines what the system is allowed to do.

---

### 🟡 Ring 2 — The Operator

**Domain:** Human Will & Judgment  
**Authority:** Intervention  
**Overridable:** ❌ Cannot override Ring 0

**Capabilities**
- Can override Ring 1 under emergency semantics
- Cannot modify policy definitions
- Cannot bypass physical constraints

**SOPCOS Role**
- *Vinci Wallet Signer*

> Ring 2 exists because humans may perceive
> hazards that sensors cannot.

---

### 🟢 Ring 3 — The Intelligence

**Domain:** AI, optimization, analytics  
**Authority:** Recommendation & efficiency  
**Overridable:** Always

**Capabilities**
- Suggest actions
- Optimize within constraints
- Execute **only pre-authorized actions**

**Restrictions**
- Cannot override Ring 1
- Cannot override Ring 2
- Cannot expand scope

**SOPCOS Role**
- *Axon Node*

> Intelligence without authority is safe.  
> Authority without intelligence is dangerous.  
> SOPCOS separates the two.

---

## ⚖️ Conflict Resolution Rules

### Case 1 — AI vs Policy

If **Ring 3 (AI)** says:
> “Accelerate”

But **Ring 1 (Policy)** says:
> “Limit reached”

**Verdict:** `DENY`

**Rationale:**  
Safety and law outrank optimization.

---

### Case 2 — Human vs Policy

If **Ring 2 (Human)** says:
> “Open door”

But **Ring 1 (Policy)** says:
> “Unsafe”

**Verdict:** `ALLOW + DIRTY_STATE`

**Rationale:**  
The human may perceive an emergency not captured by sensors.  
The system allows the action **but transfers liability**.

---

### Case 3 — Any Ring vs Physics

If any ring attempts to violate **Ring 0**:

**Verdict:** `DENY`

**Rationale:**  
Physical reality cannot be overridden.

---

## 🛡️ Dirty State Semantics

When Ring 2 overrides Ring 1:

- The system enters **DIRTY STATE**
- Liability is cryptographically bound to the human signer
- All actions are logged with full context
- Normal AI optimization is suspended
- Recovery requires auditor inspection and signed reset

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
> It proves *who was responsible* —  
> and respects reality above all else.
