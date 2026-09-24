# Specification: Transaction Control Block (TCB) Fields

## Overview

The **Transaction Control Block (TCB)** is the coordination descriptor representing the active bilateral agreement between Org A (Buyer) and Org B (Seller). It holds the cryptographic identifiers, constraints, and locked commitments required to govern conditional execution and settlement.

> **Note**: This document provides an initial conceptual description of the TCB fields. It is not yet a finalized serialization schema or wire format.

---

## Core Fields

| Field Name | Type / Format (Conceptual) | Description |
|---|---|---|
| `TID` | String / UUID / Hash | **Transaction Identifier**: A globally unique identifier assigned to this bilateral transaction session. Used to reference the transaction across logs, MPC sessions, and external payment rails. |
| `Contract hash` | Cryptographic Hash (e.g., SHA-256) | **Contract Digest**: The cryptographic commitment of the mutually negotiated bilateral agreement. Anchors the task description, acceptance criteria, pricing, and obligations negotiated via upstream protocols (such as A2A). |
| `verifier ID` | Identifier / Public Key / Address | **Selected Verifier Identity**: The unique identity or cryptographic public key of the independent verification entity mutually agreed upon by Org A and Org B to inspect the deliverable and participate in the 2-of-3 MPC escrow. |
| `TTL` | Timestamp / Epoch (UTC) | **Time-to-Live**: The explicit expiration deadline for the transaction. If verification and threshold authorization do not conclude before the TTL, the transaction aborts and capital commitments are unlocked/refunded. |
| `risk-ceiling (TO-TS)` | Numerical / Range Descriptor | **Risk Ceiling & Operational Tolerance**: Specifies transaction exposure limits, financial risk boundaries, and operating tolerances agreed upon between Org A and Org B for the session. |

---

## Bilateral Commitment Inputs

Both counterparties lock their respective commitments into the TCB before task delivery proceeds:

### 1. Capital Commitment (Locked by Org A / Buyer)
* **Description**: A verifiable cryptographic reservation of funds committed by Org A's domestic runtime.
* **Purpose**: Guarantees to Org B that capital is provably allocated and will be released atomically upon successful verification, eliminating non-payment risk.

### 2. Deliverable Hash (Locked by Org B / Seller)
* **Description**: The cryptographic digest of the target deliverable (or pre-committed deliverable manifest).
* **Purpose**: Guarantees to Org A that the work delivered matches what was committed at contract initialization, preventing equivocation or post-hoc modifications.

---

## State Transition Context

The TCB transitions through the following conceptual states:
1. **Instantiated**: TCB fields defined from bilateral negotiation; awaiting locks.
2. **Locked**: Both Capital Commitment (Org A) and Deliverable Hash (Org B) are secured.
3. **Verified**: Selected Verifier validates the deliverable against the contract hash and criteria.
4. **Settled**: Settlement Coordinator executes DvP outputs (receipt appended, token log committed, fund transfer dispatched).
5. **Aborted / Expired**: Verification failed or TTL expired; commitments released or refunded.
