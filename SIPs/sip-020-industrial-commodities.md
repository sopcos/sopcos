# SIP-020: Standard Industrial Commodities (SIC)

| Field | Value |
| :--- | :--- |
| **SIP** | 020 |
| **Title** | Standard Industrial Commodities (SIC) |
| **Status** | DRAFT |
| **Type** | Standards Track (Core) |
| **Category** | Core / Asset Management |
| **Author** | Sopcos Core Developers |
| **Created** | 2026-01-23 |
| **Requires** | SIP-015 (OpCode Namespace), SIP-016 (IDAS) |
| **Motto** | *"Identity is absolute. Quantity is fluid."* |

## 1. Abstract

This proposal defines a native standard for managing fungible, divisible, and countable industrial assets on the Sopcos Core Ledger.

Assets traditionally referred to as "tokens" in general-purpose blockchains are redefined here as **Standard Industrial Commodities (SIC)**. This standard represents measurable industrial units-such as energy credits, carbon offsets, raw material inventories, and machine operating hours-that serve to answer the question *"How much?"* rather than *"Which one?"*.

SIC complements **SIP-016 (IDAS)**, creating a bipartite asset architecture:
* **SIP-016 (IDAS):** Manages unique identities (Machines, Models, Licenses).
* **SIP-020 (SIC):** Manages quantifiable flows (Resources, Currencies, Credits).

## 2. Motivation

While SIP-016 efficiently handles discrete assets (e.g., a specific Turbine Serial #99), it is technically inefficient and semantically incorrect for quantity-based assets.

### 2.1. The Granularity Problem
Tracking 10,000 liters of fuel using 10,000 individual NFTs (IDAS) would bloat the state and incur prohibitive computation costs. Industrial resources require granular divisibility (e.g., transferring 0.5 MWh).

### 2.2. Semantic Integrity
The term "Token" typically implies speculative financial instruments. In an industrial context, terms like "Inventory," "Commodity," or "Credit" align better with ERP systems and regulatory frameworks. SIC standardizes this terminology at the protocol level.

### 2.3. Relation to Native Currency (SOPC)
**This standard (SIP-020) does not replace or modify the network's native currency, SOPC.**

* **SOPC** remains the primary value used to pay transaction fees (Gas) and secure Validators (Stake).
* **SIC Commodities** are secondary assets carried on the Sopcos network.
* **Gas Logic:** When a user transfers a Commodity (e.g., `SOLAR-CREDIT`), they must pay the network transaction fee using SOPC.

## 3. Use Cases

### 3.1. Energy & Carbon Credits (Sustainability)
A solar plant mints 1 `SOLAR-CREDIT` (SIC) for every 1 MWh generated. These commodities are transferred to a factory's wallet to offset carbon usage. Upon regulatory audit, the credits are **Burned** (Retired) to prove consumption.

### 3.2. Supply Chain Inventory
A steel mill defines a `RAW-STEEL-304` commodity. When 50 tons of physical material enter the warehouse, 50,000 units (kg basis) are minted on-chain. As material moves to production, the on-chain balance is transferred or burned, ensuring the digital ledger mirrors physical inventory.

### 3.3. Machine-as-a-Service (MaaS) Credits
A CNC machine is programmed via SIP-001 Policy to operate only when its wallet holds a positive balance of `MACHINE-HOUR` credits. The operator purchases credits; the machine consumes (burns) them in real-time.

## 4. Specification

This standard allocates the OpCode range `0x60 – 0x6F` within the Sopcos Kernel.

### 4.1. OpCode Allocation

| Hex | OpCode Name | Description |
| :--- | :--- | :--- |
| **0x60** | `OpDefineClass` | Defines a new Asset Class (Commodity or IDAS Collection Root). |
| **0x61** | `OpMintCommodity` | Increases the supply of a fungible commodity. |
| **0x62** | `OpTransferCommodity` | Transfers commodity balance between DIDs. |
| **0x63** | `OpBurnCommodity` | Permanently removes commodity units from circulation. |

### 4.2. Unit of Account & Precision

To ensure high precision without floating-point errors, all SIC quantities are stored as integers (Micro-Units).

* **Storage:** The ledger stores only the `uint64` raw value.
* **Decimals:** Defined in `OpDefineClass`.
    * *Example:* If Decimals = 6, a stored value of 1,000,000 represents 1.0 Unit.
* **SOPC Distinction:** While the native SOPC coin uses 6 decimals by default, SIC Commodities may define their own precision (0 to 18) based on industrial needs (e.g., `RAW-STEEL` might use 0 for whole kg, while `ENERGY` uses 6 for kWh).

### 4.3. Data Structures

#### 4.3.1. Class Definition Payload (Op 0x60)
This operation acts as the "Genesis" for an asset type.

```go
type ClassType uint8

const (
    ClassCommodity      ClassType = 1 // SIP-020 Fungible
    ClassIDASCollection ClassType = 2 // SIP-016 Non-Fungible Root
)

type DefineClassPayload struct {
    Name       string    // e.g., "Carbon Offset 2026"
    Symbol     string    // e.g., "CO2-26"
    Type       ClassType // 1 or 2
    MaxSupply  uint64    // 0 = Unlimited
    OwnerDID   string    // The DID authorized to Mint
    Decimals   uint8     // Default: 6. Must be 0 for IDAS.
}
```
#### 4.3.2. Mint Payload (Op 0x61)
Strictly restricted to the OwnerDID of the Class.

```go

type MintCommodityPayload struct {
    ClassID  uint64
    Amount   uint64 // In Raw Units (reflecting Decimals)
    ToDID    string // Recipient
}
```

#### 4.3.3. Transfer Payload (Op 0x62)
Standard value transfer logic.

```go

type TransferCommodityPayload struct {
    ClassID  uint64
    Amount   uint64 // In Raw Units
    // ToDID is explicit. FromDID is implicit (Signer).
    ToDID    string
    Memo     string // Non-consensus log (e.g., Invoice #)
}
```

#### 4.3.4. Burn Payload (Op 0x63)
Used for retiring credits or consuming inventory.

```go
type BurnCommodityPayload struct {
    ClassID  uint64
    Amount   uint64 // In Raw Units
    Reason   string // Non-consensus log (e.g., "Used in production")
}
```
## 5. Normative Rules & Logic

### 5.1. Definition & Identity
1.  **ClassID Derivation:** The ClassID is deterministic to support same-symbol/different-precision definitions. It MUST be derived as: `Trunc64(Hash(OwnerDID || Symbol || Type || Decimals || ChainSalt))`
2.  **Symbol Uniqueness:** The uniqueness constraint is **Owner-Scoped**. The pair `(OwnerDID, Symbol)` MUST be unique. Multiple owners MAY define `RAW-STEEL` independently.
3.  **Ownership Transfer:** There is no specific OpCode for transferring Class ownership in this SIP. Ownership transfer is handled outside the scope of this SIP via the DID key rotation mechanisms defined in SIP-015/Core.

### 5.2. Type Restrictions (IDAS vs Commodity)
1.  **IDAS Exclusion:** If `Type == ClassIDASCollection`, the operations `OpMintCommodity`, `OpTransferCommodity`, and `OpBurnCommodity` MUST fail. (IDAS assets are managed via SIP-016 OpCodes 0x40-0x49).
2.  **Decimal Constraint:** If `Type == ClassIDASCollection`, `Decimals` MUST be 0.

### 5.3. Transaction Logic
1.  **Implicit Sender:** For `OpTransferCommodity` and `OpBurnCommodity`, the Sender is implicitly defined as the transaction signer's DID.
2.  **Solvency Check:** A transfer or burn operation MUST fail (Deny) if `balance[Signer] < Amount`.
3.  **Supply Cap:** If `MaxSupply > 0`, any `OpMintCommodity` that would cause `TotalSupply > MaxSupply` MUST fail.
4.  **Immutability:** Once defined, `Symbol` and `Decimals` cannot be altered.

## 6. Backwards Compatibility

Older nodes will treat `0x60`-series operations as unknown but valid transactions, preserving chain validity without updating SIC-related state. This ensures that the introduction of SIC does not cause a hard fork for nodes only validating L1 core consensus.

## 7. Security Considerations

* **Integer Overflow:** All arithmetic operations on `uint64` balances MUST use the Kernel's `SafeMath` library to panic on overflow/underflow.
* **Access Control:** Only the `OwnerDID` defined in the Class State can execute `OpMintCommodity`.
* **Precision Hygiene:** Front-end applications must respect the `Decimals` field to prevent "off-by-factor" accounting errors in ERP integrations.

---

## Appendix A: Test Vectors (Non-Normative)

### A.1. Definition & Mint (Solar Credit)
* **Input DefineClass:**
    * Name: "Solar Energy Credit 2026"
    * Symbol: "SOLAR-26"
    * Type: Commodity (1)
    * OwnerDID: `did:sop:plant01`
    * Decimals: 6
* **Derived:** `ClassID = 0xA1B2C3D4` (Hash(did:sop:plant01 || SOLAR-26 || 1 || 6 ...))
* **Input Mint:**
    * ClassID: `0xA1B2C3D4`
    * Amount: `1_000_000` (1.000000 Unit)
    * ToDID: `did:sop:plant01`
* **Expected State:**
    * `TotalSupply = 1,000,000`
    * `balance[plant01] = 1,000_000`

### A.2. Transfer
* **Input Transfer:**
    * Signer: `did:sop:plant01`
    * To: `did:sop:factoryA`
    * Amount: `250_000` (0.250000 Unit)
* **Expected State:**
    * `balance[plant01] = 750_000`
    * `balance[factoryA] = 250_000`

### A.3. Burn / Retire
* **Input Burn:**
    * Signer: `did:sop:factoryA`
    * Amount: `200_000` (0.200000 Unit)
    * Reason: "Used in production"
* **Expected State:**
    * `balance[factoryA] = 50_000`
    * `TotalSupply = 800_000`

---
**Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.**