# SIP-016: Industrial Digital Assets Standard (IDAS)

| Field | Value |
| :--- | :--- |
| **SIP** | 016 |
| **Title** | Industrial Digital Assets & Provenance Standard |
| **Author** | Sopcos Core Developers |
| **Status** | Draft |
| **Type** | Standards Track (Core) |
| **Category** | Core / Asset Management |
| **Created** | 2026-01-09 |
| **Requires** | SIP-015 (v2 OpCode Standard) |

## Abstract

This proposal introduces a native **Industrial Non-Fungible Token (NFT)** standard on the Sopcos Blockchain. Unlike traditional art-centric NFTs, this standard is engineered specifically for **Industrial IoT (IIoT)** use cases. It provides a protocol for managing the digital lifecycle of physical machinery (Digital Twins), certifying Artificial Intelligence models (Model Provenance), and securing immutable maintenance logs (Service History) using native, high-performance OpCodes.

## Motivation

In the context of Industry 4.0 and Edge AI, three critical trust challenges exist that cannot be solved by traditional databases:

1.  **Device Identity & Ownership:** Physical assets (e.g., industrial boilers, turbines) lack a cryptographically verifiable, transferable digital identity that persists across different owners.
2.  **AI Model Security:** There is no standardized method to verify the integrity and origin of AI models running on edge devices (Axon Agents), creating risks of model poisoning.
3.  **Audit Trails:** Maintenance and service logs are often siloed in centralized servers, making them susceptible to manipulation for insurance or warranty claims.

SIP-016 addresses these issues by embedding an atomic asset management layer directly into the Sopcos Core, avoiding the overhead and complexity of WASM smart contracts for standard use cases.

## Specification

This standard utilizes the **Extended OpCode Standard v2** (defined in `core/transaction.go`) and reserves the OpCode range **40-49**.

### Data Structure

An Industrial Asset Record is defined by the following schema:

```json
{
  "collection_id": "string (UUID or Namespace)",
  "token_id": "uint64 (Sequential)",
  "issuer": "string (Sopcos Address)",
  "owner": "string (Sopcos Address)",
  "metadata_uri": "string (IPFS/DataStore URL)",
  "status": "string (ACTIVE | MAINTENANCE | BURNED)",
  "data_hash": "string (SHA-256 of the physical spec or model file)",
  "minted_at": "int64 (Unix Timestamp)"
}
```

### Operation Codes (OpCodes)

The following native operations are introduced to manage the asset lifecycle:

#### 1. OpCreateAsset (ID: 1)
* **Description:** Initializes a new Asset Class or Collection.
* **Usage:** Used by manufacturers to establish a product line (e.g., "Boiler Series-X").
* **Flag:** Requires the `IsNFT: true` flag to be set during creation.

#### 2. OpMintNFT (ID: 40)
* **Description:** Issues a unique digital certificate for a specific physical item or software artifact.
* **Restriction:** Only the `Issuer` (Collection Creator) can execute this.
* **Payload Example:**
```json
{
  "collection_id": "550e8400-e29b...",
  "recipient": "s1_distributor_address...",
  "metadata_uri": "ipfs://QmXyz...",
  "immutable_attributes": {
    "serial_number": "SN-2026-001",
    "production_date": "2026-01-09"
  }
}
```

### Operation Codes (OpCodes)

The following native operations are introduced to manage the asset lifecycle:

#### 1. OpCreateAsset (ID: 1)
* **Description:** Initializes a new Asset Class or Collection.
* **Usage:** Used by manufacturers to establish a product line (e.g., "Boiler Series-X").
* **Flag:** Requires the `IsNFT: true` flag to be set during creation.

#### 2. OpMintNFT (ID: 40)
* **Description:** Issues a unique digital certificate for a specific physical item or software artifact.
* **Restriction:** Only the `Issuer` (Collection Creator) can execute this.
* **Payload Example:**
```json
{
  "collection_id": "550e8400-e29b...",
  "recipient": "s1_distributor_address...",
  "metadata_uri": "ipfs://QmXyz...",
  "immutable_attributes": {
    "serial_number": "SN-2026-001",
    "production_date": "2026-01-09"
  }
}
```

#### 3. OpTransferNFT (ID: 41)
* **Description:** Transfers ownership of the asset to a new wallet address.
* **Logic:** Used when a machine is sold or assigned to a new operator.

#### 4. OpListNFTForSale (ID: 42)
* **Description:** Lists the asset on the native on-chain marketplace.
* **Logic:** The asset remains in the owner's wallet but is marked `ForSale`. A price (in SOPC) is set.

#### 5. OpBuyNFT (ID: 43)
* **Description:** Executes an atomic swap of funds for ownership.
* **Security:** This operation guarantees that funds are transferred to the seller and the NFT is transferred to the buyer in the same block execution, eliminating counterparty risk.

#### 6. OpBurnNFT (ID: 44)
* **Description:** Permanently destroys the digital asset.
* **Usage:** Used when a physical machine is scrapped or an AI model is deprecated/revoked due to security flaws.

## Rationale

### Why Native OpCodes instead of WASM Contracts?

1.  **Performance:** Industrial IoT environments require low-latency validation. Native Go implementation executes significantly faster than VM-based smart contracts.
2.  **Safety:** By standardizing the logic at the protocol level, we eliminate the risk of "smart contract bugs" (e.g., re-entrancy attacks) common in general-purpose NFT contracts.
3.  **Cost:** Native operations consume less computational gas, making high-frequency updates (e.g., maintenance logging) economically viable.

## Use Cases

### 1. Digital Twin Identity
Axon Agents can cryptographically verify their authority over a physical machine.
* *Flow:* Agent starts -> Checks Wallet -> Finds `TokenID` corresponding to the machine's Serial Number -> Authorization Granted.

### 2. Certified AI Model Provenance
Ensuring that Edge AI agents only run authorized code.
* *Flow:* Developer mints an NFT containing the SHA-256 hash of the new Llama-3 model. The Axon Agent downloads the model, hashes it, and verifies it against the on-chain NFT. If the hash mismatches, the model is rejected.

### 3. Service History Log
Service technicians can append immutable metadata to the asset (via linked transactions) to prove maintenance was performed, facilitating automated insurance claims.

## Backwards Compatibility

This SIP relies on the **OpCode Namespace Segmentation** introduced in previous updates. It utilizes the reserved range `40-49` and does not conflict with Core (0-39) or L2 Governance (100+) operations. Older clients will recognize the transactions as valid but may display them as "Unknown Asset Operation" until updated.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).