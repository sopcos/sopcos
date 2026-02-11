# SOPCOS IMPROVEMENT PROPOSAL (SIP-015)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 015 |
| **Title** | Cognitive Verdicts & Agentic Consensus |
| **Status** | **PROPOSED STANDARD (REVISED - CHRONOS READY)** |
| **Type** | Standards Track (Core) |
| **Author** | Sopcos Core Architects (Maestro & Codex) |
| **Created** | 2026-01-07 |
| **Last Revised** | 2026-02-12 |
| **Layer** | L2 (Axon Protocol) |
| **Requires** | SIP-012, SIP-013, SIP-014 |
| **Motto** | "Intelligence is Probabilistic; Liability is Deterministic." |

---

## 1. Abstract

This standard defines the processes for data generation, risk analysis, and decision recommendation by Artificial Intelligence Agents (Synapse/Axon Nodes) operating on the Sopcos Network. SIP-015 positions Artificial Intelligence (AI) not as an "Authority," but as a **"Cognitive Auditor."**

The AI observes the physical world and generates **probabilistic** recommendations; however, the execution of these recommendations is subject to **Deterministic Policy (SIP-001)** rules and **Human Mandate (SIP-012)**.

**Revision Note (v1.1):** This update introduces **Temporal Semantics (Chronos Class)**, enabling AI agents to issue predictive warnings regarding *future* states (e.g., "Failure in 600s") without violating the deterministic safety of the Hot Path.
(SIP-001)** rules and **Human Mandate (SIP-012)**.

## 2. Philosophy: The Caged Genius

Industrial systems cannot accept uncertainty (hallucination). Therefore, SIP-015 encloses "AI Autonomy" within a three-layered security cage:

1.  **The Observer is not the Judge:** The machine generates signals, scores, and predictions. **Intent** and **Liability** are born only with a Human signature (or a Mandate signed by a human).
2.  **Confidence Score:** AI output is never just "DO" or "DON'T". The output is a vector: `Verdict + Confidence Level`. The Chain utilizes this confidence score as a mathematical threshold when taking action.
3. **Hierarchical Compliance:**
    *   **Pre-Law (SIP-008):**  Physical limits supersede AI recommendations.
    *   **Override (SIP-006):**  Human intervention immediately invalidates AI analysis.
4. **Non-Binding Guarantee:** Predictive engines **MUST NOT** generate enforceable `VerdictRecords` directly. They **MAY** only emit `RecommendationEnvelopes` to be evaluated by a deterministic SIP-001 Policy.

## 3. Data Structures

The cognitive process is standardized via a 5-stage data flow, termed **"The Tunnel Architecture."**

### 3.1. ObservationRecord
The AI does not look at "raw" data, but at "signed reality" declared via SIP-013.

```json
{
  "obs_id": "uuid-v4",
  "timestamp": 1736240000,
  "telemetry_snapshot": { "temp": 120.5, "vibration": 0.04 },
  "binding_provenance": {
    "manifest_urn": "urn:sopcos:artifact:sha256:manifest_v2" // Link to SIP-013
  },
  "input_hash": "sha256:..." // SIP-003 Privacy Hash
}
```

##### 3.2. RiskAssessment (The Thinking - REVISED)
This is the machine's "thought bubble." It contains analysis, not judgment. Structure updated to support Temporal Semantics:

```go
type RiskAssessment struct {
    ModelHash      string  // SIP-014 URN of the Brain
    RiskScore      float64 // 0.0 to 1.0
    RiskCategory   string  // e.g., "BEARING_FAIL_PREDICTED"
    
    // Temporal Semantics (New in v1.1)
    Mode           string  // "SNAPSHOT" | "PREDICTIVE"
    WindowSeconds  int     // Lookback window (e.g., 3600s used for inference)
    HorizonSeconds int     // Prediction horizon (e.g., "Failure in 600s")
    
    Timestamp      int64
}
```
* **Model Provenance:**  The hash of the AI model weights used (e.g., Llama-3) (model_hash) must point to a URN registered in the SIP-014 Vault.


### 3.3. RecommendationEnvelope (The Offer)

Synapse transforms its analysis into an "Offer." This is not an order.

* **SuggestedVerdict:** `ALLOW`, `DENY`, or `ALERT`.

* **ValidFor:** Expiration duration (AI decisions spoil quickly).

* **Sensitive Recommendations (Cross-Ref SIP-017):** If the detected risk_category involves sensitive security vulnerabilities (e.g., Zero-Day) or trade secrets, the Agent **SHOULD** encapsulate the RecommendationEnvelope within a **SIP-017 Secure Message** targeting the System Admin, instead of broadcasting it as a cleartext `VerdictRecord` on L1. This constitutes the "Silent Alarm" protocol.

```go
type RecommendationEnvelope struct {
    RecID    string   `json:"recommendation_id"`
    BasedOn  []string `json:"based_on"` // References [ObsID, AssessmentID]

    SuggestedVerdict  string `json:"suggested_verdict"`  // "DENY", "ALLOW", "ALERT"
    JustificationCode string `json:"justification_code"` // "CLASS_C", "CLASS_S"

    Confidence float64 `json:"confidence"` // Confidence score of the recommendation

    Scope           string `json:"scope"`             // "boiler_room_1"
    ValidForSeconds int    `json:"valid_for_seconds"` // Recommendation TTL

    MachineSignature string `json:"machine_signature"` // Synapse Node Signature (NO Liability)
}
```

### 3.4. DecisionContext (Context & Authority Check)
The AI's recommendation is reconciled with the SIP-012 Sovereign Mandate.

* **Rule:** The AI can generate a `VerdictRecord` only if a valid Mandate (Human Proxy) exists AND the Confidence Score exceeds the threshold defined in the Policy (SIP-001).
    * *Example Policy:* `IF ai.confidence > 0.95 THEN ALLOW ELSE ALERT.`

### 3.5. VerdictRecord (The Final Verdict)
The single record written to the Chain (Axon L1) possessing legal binding.

```json
{
  "verdict": "DENY",
  "authority_level": 3, // Optimization / Agent Level
  "provenance": {
    "recommendation_id": "rec-123",
    "mandate_id": "urn:sopcos:mandate:operator-88-delegate-axon-01"
  },
  "final_signature": "sig_of_axon_node"
}
```

## 4. Operational Semantics

### 4.1. The "No-Hallucination" Check (Verification)
Before generating a `VerdictRecord`, an AI Agent must present `ModelHash` and `PromptHash` values as proof. This ensures that in a future audit, the question "Why did you make this decision?" can be answered by re-running the exact same model with the exact same input (**Replayability**).

**License Verification (Rights Check):** If a LicenseTokenID is provided in the RiskAssessment, the Agent **MUST** verify via the L1 State that the current Node Owner holds the specific NFT required to operate this Model. If the NFT is burned or transferred, the Agent MUST refuse to generate a verdict (DENY).

### 4.2. Mandate Dependency
AI agents (Nodes) do not possess "Asset Ownership" on their own. Under SIP-012, an AI cannot function without a valid `MandateToken` signed by a Human Operator (Sovereign).
*  If the Human Operator's authority is revoked (OP_ARTIFACT_REVOKE), all AI Agents dependent on it are instantly silenced.

#### 4.3. Predictive Advisory Mode (Chronos Class)
This section governs agents operating in `mode="PREDICTIVE"`.
*   **Rule 1 (Policy Evaluation):** A prediction (e.g., "Horizon: 600s") has NO effect unless a SIP-001 policy explicitly subscribes to it.
*   **Rule 2 (No Bypass):** Predictive alerts **MUST NOT** bypass the Hot Path safety interlocks (SIP-008 Level ∞).
*   **Rule 3 (Safe Degradation):** If the AI Agent fails, crashes, or disconnects (Heartbeat Loss), the system **MUST** degrade safely to "Dumb Mode" (Standard PID Control) without halting, unless the policy explicitly requires AI for safety.
*   **Rule 4 (Expiration):** All predictive recommendations **MUST** have a strict TTL. Old predictions are dangerous noise.

#### 5. Security Restrictions (Red Lines)
The following restrictions are **Hard-Coded** at the protocol level:

1.  **No Wallet Ownership:** AI Agents cannot hold Private Keys capable of fund transfer. They only possess "Data Signing" keys.
2.  **Absolute Override:** A SIP-006 Override Token sent by a human operator immediately crushes and invalidates all running AI processes and recommendations (even at 99.9% Confidence).
3.  **Strict Audit Trail:** No cognitive verdict is accepted on-chain without complete `InputHash` or `ModelHash` information.

## 6. Integration with SIP-001 (Policy Language Update)
New operators have been added to the SIP-001 language to process AI recommendations:
*  `axon.recommendation`: The decision recommended by AI (ALLOW/DENY).
*  `axon.confidence`: The confidence score of the AI (Float).
*  `axon.risk_category`: The detected risk label.
*  `axon.risk_score`: The probabilistic confidence of the risk (Float 0.0 - 1.0).
*  `axon.horizon`: The predicted time-to-failure in seconds (Int).
*  `axon.mode`: The operation mode ("SNAPSHOT" / "PREDICTIVE").

**Canonical "Trend-to-Lock" Policy Example:**
```json
{
  "id": "PREDICTIVE-BEARING-PROTECTION",
  "condition": "axon.mode == 'PREDICTIVE' AND axon.risk_score > 0.85 AND axon.horizon < 600",
  "action": "WARN",
  "reason": "AI_PREDICTS_FAILURE_WITHIN_10_MINUTES",
  "priority": 50
}
```

Logic: If the AI is >85% sure that the bearing will fail within 10 

## 7. Annex: Canonical Incident Generator
**Normative Requirement:**
To validate compliance with SIP-015, a reference simulator producing deterministic telemetry and ground-truth labels **MUST** be provided.
*   **Purpose:** To verify that the "Predictive Engine" correctly triggers the "Policy" within the defined Horizon.
*   **Principle:** "An AI that cannot be tested against a Ground Truth is not an Industrial Asset; it is a liability."
minutes, issue a WARN verdict (which may trigger a graceful shutdown sequence via the Controller).


---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*