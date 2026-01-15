# SOPCOS IMPROVEMENT PROPOSAL (SIP-017)

| Metadata | Value |
| :--- | :--- |
| **SIP** | 017 |
| **Title** | Secure M2M Messaging Standard (S-M2M) |
| **Status** | PROPOSED STANDARD |
| **Type** | Standards Track (Core) |
| **Author** | Sopcos Core Developers (Nexus, Enigma & Codex) |
| **Created** | 2026-01-10 |
| **Layer** | Networking / Privacy |
| **Requires** | SIP-002 (Kernel), SIP-015 (Agent Identity) |
| **Motto** | "Public Ledger, Private Conversation." |

---

## 1. Abstract

This proposal defines a standard for **End-to-End Encrypted (E2EE)** communication between nodes, wallets, and Axon Agents using the native **OpCode 9 (OpMessage)** transaction type. By leveraging the existing **secp256k1** elliptic curve infrastructure, SIP-017 enables a secure "Transport Layer" directly on the blockchain. This allows industrial entities to exchange sensitive commands, private telemetry, and trade secrets without exposing data to the public ledger.

## 2. Motivation

In Industrial IoT (IIoT), transparency is not always a virtue. While the integrity of a transaction must be public, the *content* often requires strict confidentiality.

* **The CIA Gap:** Sopcos already provides Integrity (Chain) and Availability (Nodes). SIP-017 provides the missing **Confidentiality**.
* **The "Red Phone" Necessity:** Critical alerts (e.g., Zero-Day vulnerabilities found by AI) or C2 (Command & Control) signals must be transmitted securely. Broadcasting them in cleartext is a security risk.

## 3. Specification

This standard utilizes **OpCode 9** as the transport carrier but enforces a strict cryptographic schema for the payload.

### 3.1. Cryptographic Primitives (Hardened)

To ensure industrial-grade security and interoperability, implementations **MUST** use the following parameters:

* **Curve:** `secp256k1` (Native to Sopcos/Ethereum).
* **Key Exchange:** `ECDH` (Elliptic Curve Diffie-Hellman) with Ephemeral Keys.
* **KDF:** `HKDF-SHA256` (HMAC-based Key Derivation Function).
* **Symmetric Cipher:** `AES-256-GCM` (Galois/Counter Mode).
* **GCM Authentication Tag:** 128-bit (Standard Length).

### 3.2. Forward Secrecy (Mandatory)

Static ECDH (using long-term wallet keys only) is **FORBIDDEN** for SIP-017 payloads.

* **The Rule:** The sender **MUST** generate a fresh, random ephemeral keypair for every message.
* **Rationale:** If a node's private key is compromised in the future, past messages must remain unreadable.

### 3.3. The Envelope Schema (On-Chain Payload)

The raw transaction data field in `OpMessage` **MUST** contain the following JSON structure:

```json
{
  "p": "sip-017",            // Protocol Identifier
  "enc": "AES-256-GCM",      // Encryption Standard
  "iv": "hex_string",        // Initialization Vector (Randomness)
  "ephem_pub": "hex_string", // Ephemeral Public Key (REQUIRED for Forward Secrecy)
  "blob": "ENCRYPTED_HEX_DATA..."
}
```

 ### 3.4. The Internal Schema (Decrypted Payload)

Once decrypted using the Shared Secret, the blob MUST resolve to the following JSON structure. Receivers **MUST** reject payloads that do not match this schema or fail the nonce check. 

```json
{
  "type": "CMD | LOG | ALERT | KEY_EXCHANGE",  // Uppercase ASCII
  "timestamp": 1736524000,                     // Sender's Time
  "nonce": "random_96bit_hex",                 // Replay Protection
  "body": { ... }                              // Variable Content
}
```

* **Forward Compatibility:** Unknown message type values **MUST** be ignored without error. This ensures that nodes running older software versions do not crash when receiving new message types (e.g., from v2 protocol upgrades).

## 4. Operational Semantics

### 4.1. Replay Protection Logic

Industrial controllers are vulnerable to "Replay Attacks" (e.g., resending a valid "Open Valve" encrypted packet). To prevent this:

1.  **Time Window:** Receiver checks timestamp. If `abs(now - timestamp) > 30 seconds`, **DROP**.
2.  **Nonce Cache:** Receiver caches the nonce of valid messages within the time window. If a duplicate nonce arrives, **DROP**.

> **Note (Clock Drift):** Implementations **MAY** allow configurable time windows (e.g., up to 5 minutes) to accommodate clock drift in isolated industrial networks (Air-Gapped) lacking NTP synchronization.

### 4.2. Traffic Analysis Warning

SIP-017 encrypts the content, but the metadata (Sender Address -> Receiver Address) remains visible on the public ledger. For scenarios requiring metadata privacy, users should utilize one-time addresses.

## 5. Use Cases

### 5.1. The Silent Alarm (Privacy Preserving Telemetry)
* **Context:** Axon AI detects a critical fault.
* **Action:** Sends an encrypted `ALERT` to the Admin.
* **Result:** The public chain records the timestamp of the warning (Proof of Notification), but competitors cannot see the nature of the fault.

### 5.2. Authenticated C2 (Command & Control)
* **Context:** HQ sends a firmware parameter update.
* **Action:** Sends `type: CMD`, `body: {"set_pressure": 80}`.
* **Security:** Only the target device holding the Private Key can decrypt and execute the command. Replay attacks fail due to the nonce.

### 5.3. Off-Chain Negotiation (Handshake)
* **Context:** Two factories negotiating a raw material price.
* **Action:** They verify identity and exchange session keys via SIP-017, then move high-bandwidth negotiation to a private TCP channel. SIP-017 is for signaling, not streaming.

## 6. Backwards Compatibility

Nodes that do not implement SIP-017 logic will treat these transactions as standard OpCode 9 messages. They will display the raw JSON envelope (ciphertext) in the memo field. Non-supporting nodes **MUST NOT** attempt to partially decode the payload.

---
*Copyright © 2026 Sopcos Protocol Foundation. All Rights Reserved.*