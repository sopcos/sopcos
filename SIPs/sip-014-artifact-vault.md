# SOPCOS IMPROVEMENT PROPOSAL (SIP-014)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 014 |
| **Title** | Decentralized Artifact Vault & Lifecycle Registry |
| **Status** | DRAFT |
| **Type** | Standards Track (Infrastructure) |
| **Author** | The Architect (HQ) & Codex Core Team |
| **Created** | 2026-01-05 |
| **Layer** | Infrastructure / Lifecycle |
| **Requires** | SIP-002, SIP-009, SIP-013 |
| **Motto** | *"The Chain holds the Truth; the Vault holds the Weight."* |

## 1. Abstract
This specification defines the **"Sopcos Vault Protocol,"** a standard for the storage, addressing, and lifecycle management of digital assets (WASM Policies, Telemetry Manifests, Calibration Certificates) within the ecosystem.

SIP-014 rejects the reliance on fragile physical URLs. Instead, it establishes a hybrid architecture combining **S3-Compatible Object Storage** (for payload retention) with an **L1 Chain Registry** (for legal status). It enforces **WORM (Write Once, Read Many)** compliance at the storage layer and **Cryptographic Evolution (Register/Supersede/Revoke)** at the chain layer, effectively functioning as a "Decentralized State Archive."

## 2. Motivation: The "Broken Pointer" Problem
In standard blockchain architectures, on-chain transactions often point to off-chain data via HTTP URLs (e.g., `https://s3.aws...`). This introduces a critical **Data Availability Risk**:

* **Financial Failure:** If the cloud provider bill is unpaid, the link dies.
* **Availability Gap:** The blockchain record remains (*"Hash X is valid"*), but the payload (*Hash X's content*) evaporates.

For a forensic liability protocol, this is unacceptable. A court case ten years from now requires the exact `policy.wasm` binary used today. SIP-014 solves this by abstracting the **"Physical Location"** from the **"Logical Identity"**.

## 3. Architectural Philosophy

### 3.1. Storage as "Dumb Vault" vs. Chain as "Smart Registry"
The storage layer must remain passive and immutable. The intelligence—who uploaded it, is it still valid, what replaced it—resides exclusively on the **Chain**.

### 3.2. Provider Agnosticism (The Abstracted Layer)
Sopcos does not mandate a specific vendor (e.g., AWS). It mandates the **S3 Protocol Interface**. This allows for a flexible deployment strategy:
* **Startups:** May use AWS S3 or Google Cloud Storage.
* **Factories:** May use On-Premise MinIO servers (Local LAN speed).
* **Sovereign States:** May use government-controlled Ceph clusters (Data Sovereignty).

### 3.3. The WORM Mandate
To serve as legal evidence, the underlying storage **MUST** support **Object Lock (Compliance Mode)**. Once an artifact is committed to the Vault, it cannot be deleted or overwritten by any user, including the root administrator, until the legal retention period expires.

## 4. Identification & Addressing (DRI)
Direct physical paths (URLs) are banned from the L1 Registry. Instead, **URNs (Uniform Resource Names)** are used.

### 4.1. URN Standard
The canonical format for referencing any Sopcos Artifact is:
```text
urn:sopcos:artifact:<hash_algo>:<hash_value>
```
`urn:sopcos:artifact:sha256:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`

### 4.2. CAS (Content Addressable Storage)
The Vault uses a deterministic folder structure based on the hash, functioning similarly to a Git repository.
* **Path Logic:** `s3://vault-bucket/artifacts/{hash_prefix}/{hash}.sop`
* **Benefit:** Deduplication is automatic. If two engineers upload the exact same file, it occupies only one slot in storage.

## 5. The Registry Bureaucracy (L1 OpCodes)
The Sopcos Chain manages the Bureaucratic Lifecycle of the document. This SIP introduces three new **Operation Codes**:

### 5.1. OP_ARTIFACT_REGISTER (Birth)
Registers a new artifact URN to the ledger.
* **Payload:** `{ URN, ContentType, Signer_DID, Storage_Hint_URI }`
* **Meaning:** *"This document exists, its integrity is verified by this Hash, and it was authored by this DID."*

### 5.2. OP_ARTIFACT_SUPERSEDE (Evolution)
Declares that an existing artifact has been replaced by a newer version.
* **Payload:** `{ Old_URN, New_URN, Reason_Code }`
* **Meaning:** *"Version 1 is historically preserved but logically deprecated. The current valid law is Version 2."*
* **Use Case:** Updating a Safety Policy or a Calibration Manifest.

### 5.3. OP_ARTIFACT_REVOKE (Death)
Declares an artifact as legally null and void (or dangerous).
* **Payload:** `{ Target_URN, Revocation_Reason (SECURITY | LEGAL) }`
* **Meaning:** *"Kill Switch. Any node running this Hash must halt immediately."*

### 5.4. ARTIFACT_TYPE_MODEL_WEIGHTS (Cognitive Assets)
To support SIP-015, the Vault recognizes AI Model Weights (e.g., .gguf, .onnx, .bin) as valid legal artifacts.
* **Rationale:** To ensure "Forensic Replayability," the specific neural network state (weights) used to make a decision must be versioned and immutable.
* **Payload:** { URN, Architecture (e.g., Llama-3), Quantization_Level, Training_Hash }
* **Lifecycle:** Unlike Policy files, Model Artifacts often have a large storage footprint. Implementations MAY support "Hot/Cold" storage tiers, provided the integrity hash remains anchored on L1.

### 5.5. ARTIFACT_TYPE_ASSET_METADATA 
JSON schemas defining the immutable properties of a SIP-016 Industrial Asset.
* **Rationale:**  SIP-016 NFTs store ownership on-chain, but their descriptive data (Specifications, Manuals, Blueprints) resides in the Vault.
* **Payload:**  { Name, SerialNumber, ManufacturingDate, Specifications{}, ImageURI, SchemaVersion }
* **Linkage:**  The MetadataHash in a SIP-016 asset MUST match the SHA-256 of this artifact.

## 6. Security & Privacy

### 6.1. Client-Side Encryption
Trade secrets (e.g., proprietary control logic) must not be stored in plaintext on the Vault.
* **Process:** The Sopcos CLI encrypts the payload using the uploader's keys (AES-GCM) **before** upload.
* **State:** The Vault stores only an opaque, encrypted blob.
* **Access:** Decryption rights are managed via the DID Key Agreement protocol on-chain.

## 7. Resolution Logic (The Fetch Routine)
When a Synapse Node or Auditor needs to resolve a `urn:sopcos...`:
1.  **Registry Check:** Query L1 Axon for the URN's status (Active/Revoked).
2.  **Local Cache:** Check if the file exists in local storage.
3.  **Primary Vault:** Connect to the configured S3 Endpoint (e.g., Factory MinIO) using the `Storage_Hint`.
4.  **Secondary Vault (Failover):** If the primary fails, check the Foundation's Public Archive (or IPFS/Arweave in future iterations).
5.  **Integrity Verify:** Hash the downloaded file. If it does not match the URN, discard with `TAMPER_DETECTED`.

## 8. Conclusion
SIP-014 transforms Sopcos from a mere "File Storage" system into a **"Civil Registry of Truth."**

By separating the **Payload (S3/Vault)** from the **Status (Chain)**, it allows for a system where:
* **Physics** is immutable (WORM Storage).
* **Bureaucracy** is managed (Chain Lifecycle).
* **Liability** is absolute (Cryptographic Signatures).

This structure ensures that 10 years post-incident, an auditor can reconstruct not just the data, but the exact legal validity of every document at the moment the incident occurred.

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*