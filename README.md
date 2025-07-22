
# STX-Evidar - Enhanced Reputation Protocol

**STX-Evidar** is a Clarity-based smart contract system designed to manage decentralized reputation through staking, governance, and evaluator-based assessments. It enables robust identity and trust management across decentralized ecosystems.

---

## 🔧 Overview

The **Enhanced Reputation Protocol** ensures participants maintain credible reputations via a weighted scoring system, periodic evaluations, collateral-backed staking, and transparent governance.

Key Features:

* **Reputation Score System** with decay over time
* **Evaluator-Based Scoring** with accuracy tracking
* **Collateral Requirements** for participant trustworthiness
* **Governance Mechanism** with proposal thresholds and voting
* **Evaluation History Tracking** with optional metadata

---

## 📜 Contract Structure

### ⚠️ Error Codes

Errors are categorized for clarity:

| Range | Description          | Example                  |
| ----- | -------------------- | ------------------------ |
| `1xx` | Governance & Access  | `ERR-ACCESS-DENIED`      |
| `2xx` | Validation & Bounds  | `ERR-BOUNDS-VIOLATION`   |
| `3xx` | Entity State         | `ERR-ENTITY-NOT-FOUND`   |
| `4xx` | Economic Constraints | `ERR-INSUFFICIENT-FUNDS` |

---

## ⚙️ Protocol Constants

| Constant                | Value    | Description                            |
| ----------------------- | -------- | -------------------------------------- |
| `REPUTATION-MINIMUM`    | `u0`     | Minimum score a participant can have   |
| `REPUTATION-MAXIMUM`    | `u100`   | Maximum reputation score               |
| `EPOCH-LENGTH`          | `u144`   | \~1 day in blocks                      |
| `DECAY-INTERVAL`        | `u10000` | Blocks after which decay applies       |
| `DECAY-RATE`            | `u5`     | Score points lost per decay cycle      |
| `MINIMUM-COLLATERAL`    | `u1000`  | Minimum collateral to register         |
| `COLLATERAL-MULTIPLIER` | `u2`     | Multiplier for economic thresholds     |
| `PROPOSAL-THRESHOLD`    | `u75`    | Min rep required to propose governance |

---

## 🧑‍💼 Participants

Participants register by staking collateral and start with maximum reputation. They are evaluated over time, and their reputation decays if inactive.

**Data Stored:**

* `reputation-score`: Numeric score (0-100)
* `last-active-epoch`: Last active time
* `evaluation-count`: # of received evaluations
* `collateral-balance`: Locked stake
* `status`: "ACTIVE", "SUSPENDED", "PROBATION"

---

## 👨‍🔬 Evaluators

Evaluators are authorized to assess participants. Their accuracy influences the weight of their scores.

**Evaluator Fields:**

* `authorization-status`: Permission flag
* `evaluation-count`: Total evaluations made
* `accuracy-score`: Weight of their reputation influence
* `last-evaluation`: Last block they evaluated

---

## 🗳️ Governance

STX-Evidar includes a decentralized governance model:

**Proposal Fields:**

* `proposer`: Address initiating the proposal
* `description`: UTF-8 string (500 chars)
* `start-block` / `end-block`: Voting window
* `votes-for`, `votes-against`: Vote tallies
* `execution-payload`: Optional buffer payload

---

## 🧮 Key Functions

### Public

* `register-participant`: Register with collateral
* `submit-participant-evaluation`: Submit a reputation score
* `update-protocol-parameters`: Modify system constants

### Private

* `validate-participant-data`: Sanitize participant input
* `calculate-weighted-reputation`: Compute new rep score
* `apply-temporal-decay`: Adjust for inactivity
* `validate-metadata`, `validate-reputation-bounds`

### Read-Only

* `get-participant-profile`: Current score + stats
* `get-protocol-metrics`: System-level data
* `get-evaluation-history`: Lookup by epoch
* `is-protocol-administrator`: Admin check

---

## 🔄 Reputation Decay

If a participant is inactive for `DECAY-INTERVAL` blocks, their score reduces by `DECAY-RATE`. This ensures continuous participation.

---

## 💡 Example Workflow

1. **Alice** registers with 1000 STX
2. She is evaluated by an authorized evaluator
3. Her score is updated using a **weighted algorithm**
4. Governance can be proposed if a user has rep ≥ 75
5. Over time, if inactive, **reputation decays**

---

## 🔐 Access Control

* Only the protocol administrator can update critical parameters.
* Evaluators must be authorized to submit assessments.
* Governance must be enabled to submit proposals.

---

## ✅ Deployment Checklist

* [ ] Set initial protocol administrator
* [ ] Configure `evaluator-credentials`
* [ ] Initialize `global-statistics` and `protocol-parameters`
* [ ] Enable governance if needed

---
