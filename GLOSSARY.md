# Sopcos Ecosystem Glossary

This document defines the core terminology used across all Sopcos Improvement Proposals (SIPs).

| Term | Definition | Source |
| :--- | :--- | :--- |
| **SPL (Sopcos Policy Language)** | A deterministic, JSON-based policy language that defines permissible intents without instructing devices on "how" to act. | SIP-001 |
| **Synapse** | The reference implementation of the Sopcos Gateway node that executes SPL and signs decisions. | SIP-002 |
| **Axon** | The distributed ledger or audit trail where decisions, logs, and policy hashes are immutably stored. | SIP-002 |
| **Canonical Axon** | The main chain recognized by The Foundation as the "Legal Truth" for dispute resolution. | SIP-002 |
| **Verdict** | The output of a policy evaluation (`ALLOW`, `DENY`, `WARN`). It is a judgment, not a command. | SIP-003 |
| **Lying Gateway** | A node that is cryptographically compliant but falsifies physical reality (The Oracle Problem). | SIP-004 |
| **Lawful Evil** | A scenario where the system functions correctly according to a valid but flawed policy, causing damage. | SIP-004 |
| **Simulated Decision** | A decision generated in "Simulation Mode" for advisory purposes. It is logged but not executed. | SIP-005 |
| **Blast Radius** | The calculated impact scope of a policy, including affected devices, safety zones, and human proximity. | SIP-005 |
| **Proof of Foreknowledge** | An immutable log proving that an operator saw a simulation warning before deploying a risky policy. | SIP-005 |
| **Transfer of Liability** | The semantic definition of an "Override." The system shifts from Decision Maker to Witness. | SIP-006 |
| **Override Block** | A signed record containing the operator's confession, justification code, and the forced action. | SIP-006 |
| **Compliance Debt** | The "cost" incurred by a node entering a Dirty State, repayable only via an Auditor's Post-Mortem Report. | SIP-006 |
| **Dirty State (Regulatory Taint)** | The status of a node after an Override. It remains non-compliant until the Compliance Debt is paid. | SIP-006 |
| **Forensic Snapshot** | A cryptographic lock of the raw sensor data and active policy hash at the exact moment of an Override. | SIP-006 |