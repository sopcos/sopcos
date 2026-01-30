# SOPCOS IMPROVEMENT PROPOSAL (SIP-018)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 018 |
| **Title** | Core Ledger (L1) Architecture and Native OpCodes |
| **Status** | Proposed Standard |
| **Type** | Standards Track |
| **Author** | Sopcos Foundation |
| **Date** | 2026-01-16 |
| **Layer** | Core Ledger (L1) |
| **Implementation** | sopcos-core-ledger (Go + WASM) |
| **Rrelated-sips** | SIP-008, SIP-014, SIP-016, SIP-017 |

---

> **"Record is Evidence."**
>
> *"The Ledger seals the truth; it leaves the weight to the Vault."*

## 1. Abstract
This proposal defines the architecture of the **Sopcos Core Ledger (L1)**. Unlike general-purpose blockchains that operate on the "Code is Law" principle, the Sopcos L1 is built on the doctrine of **"Record is Evidence"**.

This layer acts as a **"Mute Notary"**; it does not judge data quality or business outcomes but provides a cryptographic seal for the *Who*, *When*, and *What* of industrial events.

## 2. Philosophy: The Mute Notary and the Thin Ledger
The Sopcos Core Ledger follows the **"Thin Ledger"** principle to ensure industrial integrity and performance:

* **Not a Database:** It does not store heavy payloads like images, manuals, or raw telemetry; it only anchors **Hashes** and **Signatures**.
 * **Not a Judge:** It does not decide if a physical value is safe; that is the responsibility of Synapse nodes. The Core Ledger only records the fact that a specific verdict was issued.
* **Timestamp Authority:** It provides an immutable chronological order for responsibility events and the **"Chain of Custody"**.

## 3. Technical Architecture: Hybrid Evidence Engine
The ledger utilizes a **Hybrid Execution Model** to balance high-performance industrial requirements with user flexibility.

### 3.1. Layered Logic

| Layer | Technology | Managed By | Purpose |
| :--- | :--- | :--- | :--- |
| **Layer 0: Kernel** | **Native Go** | Sopcos Foundation | Processes **SIP-Standard OpCodes** (IDAS, Vault, Red Phone) with hard-coded, immutable logic. High performance, zero-latency. |
| **Layer 1: User** | **WASM** | Users / Factories | Registers evidence for **Custom Business Logic** (e.g., Supply Chain Agreements, custom automation).

### 3.2. Consensus & State
* **Consensus Mechanism:** The Core Ledger operates on a **Proof of Authority (PoA)** algorithm. Validators are known entities whose authority is strictly constrained by the **Sopcos Authority Matrix (SAM)** defined in SIP-008.
* **State Machine:** An **Account-Based Model** is used (similar to Ethereum/Cosmos) to simplify Decentralized Identifier (DID) management and permission handling.

## 4. Native OpCodes (Layer 0 Instruction Set)
Critical security and responsibility operations are processed as native instructions within the Kernel (Layer 0)

### A. Identity & Access (0x00 - 0x0F)
| Hex | Mnemonic | Description |
| :--- | :--- | :--- |
| `0x01` | **OP_REGISTER_DID** | Registers identities for Synapse, Axon, or Vinci nodes. |
| `0x02` | **OP_ROTATE_KEY** | Allows for cryptographic key rotation without changing the persistent DID. |
| `0x03` | **OP_DELEGATE_AUTH** | Assigns specific roles (e.g., Auditor, Regulator) based on SIP-008. |

### B. Liability & Verdicts (0x10 - 0x1F)
| Hex | Mnemonic | Description |
| :--- | :--- | :--- |
| `0x10` | **OP_LOG_VERDICT** | Anchors routine automated verdict records, maintaining a **Clean State**. |
| `0x11` | **OP_CONFESSION** | Anchors Human Override events, shifting the system to a **Dirty State**. (Proof of Liability). |
| `0x12` | **OP_STATE_RESET** | Requires a Level 5 Auditor's signature to clear the Dirty State and restore compliance. |

### C. Industrial Assets - IDAS (0x40 - 0x4F)
| Hex | Mnemonic | Description |
| :--- | :--- | :--- |
| `0x40` | **OP_MINT_ASSET** | Creates a Digital Twin (**Industrial NFT**) for a physical machine. |
| `0x41` | **OP_TRANSFER** | Transfers machine ownership and legal liability to another DID. |
| `0x44` | **OP_BURN_ASSET** | Decommissions equipment or revokes rogue AI models (Safety Kill-Switch). |

### D. Vault & Storage (0x50 - 0x5F)
| Hex | Mnemonic | Description |
| :--- | :--- | :--- |
| `0x50` | **OP_ARTIFACT_REG** | Registers a file stored in a WORM-compliant **Vault**. Anchors the content hash. |
| `0x52` | **OP_ARTIFACT_REVOKE**| Legally invalidates a policy or model ("Legal Nullification"). |
| `0x53` | **OP_ARTIFACT_SUPER** | Links a new version to an old one, maintaining **forensic continuity** (Supersede). |

##### E. Industrial Commodities - SIC (0x60 - 0x6F)
| Hex | Mnemonic | Description |
| ------ | ------ | ------ |
| 0x60 | **OP_DEFINE_CLASS** | Defines a new Asset Class (Commodity) or Collection Root. |
| 0x61 | **OP_MINT_COMMODITY** | Increases the supply of a fungible commodity (e.g. Energy Credits). |
| 0x62 | **OP_TRANSFER_SIC** | Transfers commodity balance between DIDs. |
| 0x63 | **OP_BURN_COMMODITY** | Permanently removes commodity units from circulation (Retirement). |

### F. Messaging - Red Phone (0x90 - 0x9F)
| Hex | Mnemonic | Description |
| :--- | :--- | :--- |
| `0x90` | **OP_MESSAGE** | Enables encrypted, hybrid messaging for sensitive alerts (SIP-017). |

## 5. Data Flow Example
1.  **Axon** generates a new AI model and uploads it to the **Vault**.
2.  The Vault returns a unique hash; Axon registers this via `OP_ARTIFACT_REG` on the **Sopcos Core Ledger**.
3.  The Core Ledger records only the metadata (Who, When, Hash),, keeping the ledger "thin".
4.  Synapse reads the valid hash from the Core Ledger (L1), verifies its status, and securely downloads the full model from the Vault.

## 6. Security & Governance
The Core Ledger (L1) serves as the **Root of Trust** for attestation and ordering, while evidence integrity is jointly ensured with Vault mechanisms defined in SIP-014.
Any attempt to forge native OpCode transactions or manipulate consensus triggers an immediate technical suspension of the node ID, followed by a mandatory **governance review** as defined in SIP-008.