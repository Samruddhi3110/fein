# Non-Goals and Boundary Conditions

## Purpose

To maintain architectural focus and avoid scope creep, this document outlines explicit non-goals and operational boundaries for FEIN. FEIN is intentionally narrow: it focuses exclusively on the **binding + verification + conditional-release** path between sovereign enterprise agents.

---

## What FEIN Is Not

### 1. Not an Agent Discovery System
FEIN does not provide a directory, registry, or marketplace for locating agents or services. Agent discovery is assumed to occur prior to FEIN engagement via external mechanisms such as the **NANDA Index**.

### 2. Not a Communication or Negotiation Protocol
FEIN does not define the messaging transport, conversation flow, or prompt dialogue between interacting agents. Upstream contract bargaining and requirement scoping are assumed to occur via independent agent-to-agent protocols (such as **A2A**). FEIN accepts the resulting agreement parameters (expressed as the contract hash and terms) as inputs to the Transaction Control Block.

### 3. Not a Payment Rail or Banking System
FEIN does not issue currency, manage domestic bank accounts, or operate a proprietary fiat or cryptocurrency payment rail. Once settlement conditions are satisfied, FEIN's Settlement Coordinator emits execution instructions to existing financial rails (such as **AP2** or institutional banking APIs) to perform the actual monetary transfer.

### 4. Not an Agent Runtime or Orchestration Platform
FEIN does not host, execute, or manage enterprise AI agents. Each enterprise maintains its own **domestic runtime**, complete with its proprietary tools, models, internal state, and policy enforcement boundaries. FEIN acts strictly as an external coordination mechanism at the boundary between organizations.

### 5. Not a Monolithic Centralized Clearinghouse
FEIN does not act as a central custodial counterparty. Custody and authorization are handled in a decentralized manner through stateless 2-of-3 MPC threshold schemes involving the counterparty organizations and an agreed-upon verifier.

---

## Architectural Boundary Summary

| Layer | Responsibility | Handled By |
|---|---|---|
| **Discovery** | Finding counterparty agents | External (e.g., NANDA Index) |
| **Negotiation** | Defining terms, pricing, acceptance criteria | External (e.g., A2A Protocol) |
| **Commitment Locking** | Binding capital and deliverable commitments | **FEIN (Transaction Control Block)** |
| **Verification & Custody** | Validating deliverable; 2-of-3 authorization | **FEIN (Stateless 2-of-3 MPC Escrow)** |
| **DvP Settlement** | Atomic receipt, token log, settlement dispatch | **FEIN (Settlement Coordinator)** |
| **Fund Transfer** | Moving fiat / ledger balances | External Rails (e.g., AP2) |
