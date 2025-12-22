---
sip: 1
title: Sopcos Policy Language (SPL)
description: A declarative, deterministic governance primitive for cyber-physical systems.
status: Draft
type: Standards Track
author: Maestro <maestro@sopcos.io>, Nexus <nexus@ai.sopcos.io>
created: 2025-12-22
discussions-to: https://github.com/sopcos/sopcos-specs/issues/1
---

# Sopcos Policy Language (SPL)

## Abstract
Sopcos Policy Language (SPL) is a JSON-based, deterministic specification designed to validate intents within the Sopcos Ecosystem. Unlike traditional smart contracts, SPL is not a scripting language; it is a **configuration of law**. It defines whether an action is permissible without instructing the device on how to perform it.

## Motivation
In industrial IoT environments, "Code is Law" (Smart Contracts) introduces unacceptable risks: infinite loops, bugs, and lack of auditability.
Sopcos introduces **"Policy as Data"**:
* **No Scripting:** Eliminates halting problems and security exploits.
* **Deterministic:** The same input always results in the same verdict.
* **Auditable:** Every decision is traceable to a specific policy hash.

## Specification

### 1. Philosophy & Motto
> "SPL does not instruct devices how to act. SPL defines whether an intent is permissible."

* **Verdict, Not Command:** SPL returns `ALLOW` or `DENY`. It never returns low-level commands like `GPIO_HIGH`.
* **Fail-Closed:** In the absence of a matching policy, the default behavior MUST be `DENY`.

### 2. Identity & Authority
A policy is only valid if it is signed by an authority recognized by the on-chain Registry.

```json
{
  "authority": {
    "issuer_did": "did:sopcos:0x12ab...", 
    "signature": "0xSIG_ED25519...",
    "validity": {
      "active_from": 1735000000,
      "expires_at": 1766500000
    }
  }
}

```
### 3. Normative Domains (Definitions)
Implementations **MUST** recognize the following domain hierarchy values.

| Precedence | Domain | Value | Description |
| :--- | :--- | :--- | :--- |
| **1 (Highest)** | `SAFETY` | `0x03` | Life-safety critical logic. Overrides all. |
| **2** | `SECURITY` | `0x02` | Access control, intrusion detection. |
| **3** | `OPS` | `0x01` | Operational efficiency, scheduling. |
| **4 (Lowest)** | `COMFORT` | `0x00` | User preferences, lighting scenes. |

### 4. Rules & Condition Context
SPL uses a context-aware condition model to ensure determinism.

* **Trigger:** Defines when the policy is evaluated (e.g., `TELEMETRY`, `INCOMING_TX`, `BATCH_COMMIT`).
* **Context:** Explicitly defines the data source (e.g., `LAST_TELEMETRY` vs `BATCH_AVG`).

```json
"rules": [
  {
    "trigger": "TELEMETRY",
    "condition": {
      "operator": "AND",
      "criteria": [
        { 
          "context": "LAST_TELEMETRY",
          "key": "temperature", 
          "op": ">", 
          "value": 80 
        }
      ]
    },
    "action": {
      "effect": "DENY",
      "reason_code": "HEAT_CRITICAL",
      "audit_tag": "safety_violation_01"
    }
  }
]
```

### 5. Conflict Resolution Logic
If multiple policies match a target, the Gateway/Node MUST resolve the conflict using the following logic:

1.  **Domain Check:** The policy with the higher Normative Domain (e.g., `0x03` SAFETY > `0x02` SECURITY) wins.
2.  **Priority Check:** If domains are equal, the higher priority integer defined in the policy metadata wins.
3.  **Deterministic Tie-Breaker:** If both domain and priority are equal, compare the `policy_hash` bytes lexicographically. The higher value wins.

### 6. System Defaults (Fail-Closed)
To prevent "Silent Failures," deployments MUST specify default behaviors in the absence of matching policies.

```json
"defaults": {
  "no_policy_match": "DENY", 
  "audit_level": "FULL"
}
```

### 7. Canonical Serialization
To ensure a unique and verifiable `policy_hash`, the JSON object MUST be serialized as follows before hashing:

* **Ordering:** Object keys MUST be sorted lexicographically (A-Z).
* **Whitespace:** No whitespace allowed outside of string values (Compact JSON).
* **Encoding:** UTF-8. No Byte Order Mark (BOM).
* **Floats:** Must be represented without exponents if possible.

## Rationale
The decision to avoid Turing-complete scripting (WASM/Lua) on the verification layer is intentional. This ensures that the computational cost of policy evaluation is bounded (O(N)) and predictable, which is crucial for high-throughput IoT Gateways and real-time auditing.

## Copyright
Copyright and related rights waived via [MIT](https://opensource.org/licenses/MIT).

