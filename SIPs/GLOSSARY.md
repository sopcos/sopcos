## A. Philosophy & Actors

| Term | Definition | Source |
| :--- | :--- | :--- |
| **The Foundation** | The steward of the protocol. It manages the "Sopcos" trademark and the Root of Trust. It cannot unilaterally change the Kernel without the consensus of independent Core Maintainers. | SIP-002 |
| **Policy Author** | The administrator, engineer, or committee responsible for designing the system's logic (SPL). Unlike the Operator, they handle "Wisdom" and design but do not perform physical interventions. They are liable for the logic they sign. | SIP-002 |
| **Operator** | Field personnel with the authority to physically intervene in the system. They can Override system decisions but must accept full Liability for the outcome. They manage immediate risk rather than long-term policy. | SIP-002 |
| **Evil but Compliant** | A threat model axiom stating that a Node may technically follow all protocol rules (valid signatures, hashes, formats) while simultaneously lying about physical reality (The Oracle Problem). Sopcos ensures such lies are recorded and signed. | SIP-004 |
| **Sopcos Ring Logic** | The philosophical model defining the **Hierarchy of Reality**: <br>1. **Physics (Pre-Law):** Absolute Ruler (Cannot be overridden).<br>2. **Human (Override):** Sovereign over Software.<br>3. **Algorithm (Policy):** Advisor to the Human. | SIP-011 |

## B. Architecture & Technical Components

| Term | Definition | Source |
| :--- | :--- | :--- |
| **SPL (Sopcos Policy Language)** | The protocol's constitution. It is not a scripting language but a deterministic, JSON-based "Configuration of Law." It eliminates halting problems and defines permissible intents. | SIP-001 |
| **SOP Artifact (.sop)** | The compiled, signed, and hermetic **WebAssembly (WASM)** package. It serves as the bridge between the human-readable "Source Code" (SIP-001) and the deterministic "Machine Code" (SIP-009). | SIP-009 |
| **Kernel** | The immutable core components of the system: SPL Parser, Decision Engine, Cryptographic Primitives, and the Simulation Engine. These cannot be modified by vendors without losing certification. | SIP-002 |
| **Synapse (Runtime)** | The operational engine (Node) of Sopcos. It acts as a **PDP (Policy Decision Point)**. It does not perform physical actions; it issues Verdicts regarding their legitimacy. | SIP-002 |
| **Axon (L2 Protocol)** | The cognitive and analytical layer (Cold Path) that serves as an L2 bridge between Synapse and the Sopcos Chain. It performs long-horizon analysis and anchors finalized verdicts to L1. | SIP-015
| **Sopcos Chain (L1)** | The core immutable ledger and "Immortal Notary" of the system. It anchors the ultimate legal truth, DIDs, and cryptographic fingerprints of all artifacts. | SIP-002
| **Core Ledger** | A functional role of the Sopcos Chain (L1) that ensures the immutability of recorded verdicts and responsibility transfers. | README
| **Override Token** | A digital structure created during a human intervention. It contains the signature, justification code, and context hash. It is the proof that responsibility has transferred from the machine to the human. | SIP-011 |
| **Forensic Snapshot** | A cryptographic lock (Hash) of the raw sensor data and active policy hash at the exact moment of an Override. | SIP-006 |
| **Simulated Decision** | A decision generated in "Simulation Mode" for advisory purposes. It is logged but not executed. | SIP-005 |

## C. Verdict Algebra & Logic

| Term | Definition | Source |
| :--- | :--- | :--- |
| **Verdict Vector** | The mathematical representation of a decision: `V=⟨δ,α,σ,τ⟩` (Decision Direction, Authority Magnitude, Scope, Timestamp). | SIP-010 |
| **Pre-Law (Level ∞)** | Physical realities and hardware constraints (e.g., Maximum RPM, Safety Interlocks). These define the "Impossible" and cannot be overridden by humans or software. | SIP-008 |
| **Blast Radius** | The calculated impact scope of a policy, including affected devices, safety zones, and human proximity (Used in Simulation Risk Analysis). | SIP-005 |
| **Deny Absorption** | The principle where a `DENY` verdict absorbs and nullifies an `ALLOW` verdict if they are of equal authority (`DENY ⊕ ALLOW = DENY`). | SIP-010 |
| **Authority Projection** | The mathematical process where a Higher Authority (e.g., Emergency) filters out and overrides verdicts from Lower Authorities (e.g., Optimization). | SIP-010 |
| **Vacuum State** | A state where no matching policy exists for a given scope. The system defaults to **Fail-Closed (DENY)**. | SIP-010 |
| **Input Hash** | The cryptographic digest of raw sensor data stored on-chain. This supports "Privacy by Design" (GDPR/Trade Secrets) while allowing for forensic verification. | SIP-003 |

## D. Operational & Legal States

| Term | Definition | Source |
| :--- | :--- | :--- |
| **Dirty State** | A non-compliant state a Node enters after an Override. While in this state, all subsequent telemetry and decisions are flagged as "Under Investigation" or "Tainted". | SIP-006 |
| **Compliance Debt** | The procedural debt incurred by entering a Dirty State. It can only be cleared via a **"Reset Transaction"** signed by an independent Auditor. | SIP-006 |
| **Proof of Foreknowledge** | Logs generated by the Simulation Engine. They prove that the system warned the operator or author of specific risks before an incident occurred, upgrading liability from "Negligence" to "Intent". | SIP-005 |
| **Execution Preemption** | The act of a human override halting the algorithmic evaluation to impose a command of will. This power is absolute over software but limited by Pre-Law. | SIP-011 |
| **Justification Code** | The legal defense strategy an operator must select during an override: `CLASS_S` (Safety), `CLASS_C` (Continuity), `CLASS_F` (Forensic). | SIP-006 |
| **Liability Shield** | The legal protection provided by the protocol, separating the Decision (Sopcos) from the Action (Controller). *"Sopcos forbade the action; the Controller executed it anyway."* | SIP-003 |
| **Black Box Protocol** | The process triggered during an override where the system shifts from "Optimization Mode" to "Forensic Mode," cryptographically locking inputs. | SIP-006 |