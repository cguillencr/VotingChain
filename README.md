# VotingChain

**VotingChain** is a **privacy-preserving, verifiable voting system** built on a **permissioned blockchain**, designed to enable transparent elections **without cryptocurrencies, tokens, or speculative economics**.

The system enforces election rules on-chain while protecting voter privacy off-chain, allowing **anyone to audit the process and independently verify results**.

---

## Key Principles

- No cryptocurrency
- No mining
- No speculation
- Permissioned infrastructure
- Public auditability
- Strong voter privacy

> The blockchain enforces the rules.  
> The client protects privacy.  
> Anyone can verify the result.

---

## Goals

- Enable **verifiable elections** without trusting a single central authority
- Preserve **voter anonymity**
- Prevent **double voting**
- Ensure **tamper-proof and auditable results**
- Provide **individual voter receipts** without enabling coercion

---

## Explicitly Out of Scope

- Token economics
- Cryptocurrency trading
- Proof-of-Work
- Mining incentives
- Financial or DeFi use cases

---

## High-Level Architecture

```
+--------------------+        +--------------------------+
|   Voting Client    |        | Registration Authority   |
|  (Web / Mobile)    |        | (Eligibility Verification)|
+---------+----------+        +------------+-------------+
          |                                   |
          | anonymous voting token            |
          |                                   |
+---------v-----------------------------------v---------+
|                     VotingChain Platform              |
|                                                        |
|  +------------------+     +-------------------------+ |
|  | Token Issuer     |     | Indexer & Verifier      | |
|  | (Off-chain)      |     | (Public Audit Tools)   | |
|  +------------------+     +-------------------------+ |
|               |                       |                |
|               v                       v                |
|        +------------------------------------------------+
|        |   Hyperledger Besu (IBFT, PoA)                |
|        |   - Commit storage                            |
|        |   - Reveal validation                         |
|        |   - Immutable audit trail                     |
|        +------------------------------------------------+
+--------------------------------------------------------+
```

---

## Blockchain Platform

### Selected Platform (POC)

**Hyperledger Besu**

- Permissioned Ethereum-compatible client
- IBFT 2.0 consensus (immediate finality)
- No native token usage
- No real gas costs
- Deterministic and auditable execution

Besu is used strictly as a **rule-enforcement and audit layer**, not as a financial or economic system.

---

## Core Cryptographic Pattern

VotingChain uses a **Commit–Reveal scheme**:

1. The voter commits `hash(vote + salt)`
2. The commitment is stored immutably on-chain
3. After the commit phase, the voter reveals `(vote, salt)`
4. The blockchain recomputes and validates the commitment
5. Final results are derived deterministically from valid reveals

This ensures:
- Vote secrecy during the election
- Verifiable and auditable results after the election

---

## Actors & Responsibilities

### Voter
- Registers and receives an anonymous voting token
- Commits and later reveals a vote
- Keeps a receipt to verify inclusion

### Registration Authority (RA)
- Verifies voter eligibility off-chain
- Never sees votes
- Never interacts with vote data

### Token Issuer
- Issues one-time anonymous voting tokens
- Stores only token hashes
- Cannot link tokens to voter identities

### VotingChain (Product)
- Orchestrates all flows
- Provides client, APIs, verifier, and dashboard
- Never stores identities or raw votes

### Blockchain (Besu)
- Enforces voting rules
- Prevents double voting
- Stores immutable commitments
- Emits auditable events

### Auditor
- Verifies issued tokens vs commits vs reveals
- Independently recomputes the tally
- Requires no privileged access

---

## Voting Flow

1. **Registration**  
   Voter eligibility is verified off-chain.

2. **Token Issuance**  
   A one-time anonymous voting token is issued.

3. **Commit Phase**  
   Voters submit cryptographic commitments of their votes.

4. **Reveal Phase**  
   Votes are revealed and validated on-chain.

5. **Counting & Results**  
   Results are derived from on-chain events.

6. **Verification**  
   Voters and auditors independently verify correctness.

---

## Receipts & Verifiability

Each voter receives a **cryptographic receipt** that proves:
- Their vote was included
- Their reveal was accepted
- Their vote was counted

Receipts **do not reveal vote choice**, preserving resistance to coercion.

---

## Security Properties

| Property | Status |
|--------|--------|
| Voter anonymity | ✅ |
| Public auditability | ✅ |
| Double-vote prevention | ✅ |
| Result verifiability | ✅ |
| Strong coercion resistance | ⚠️ Partial (POC) |

## 📄 License

This project is licensed under the **Apache License 2.0 with the Commons Clause**.

- Open source for study, research, and non-commercial use
- **Commercial, production, or hosted use requires explicit written permission**

See the `LICENSE` file for full details.
