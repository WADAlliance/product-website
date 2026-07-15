---
title: Dijkstra Hard Fork Planning
sidebar_label: Dijkstra Hard Fork
sidebar_position: 1
slug: /hardfork-planning/dijkstra
---

# Dijkstra-era Hard Fork (Protocol Version 12)

## The Case for a Thin Hard Fork

Dijkstra is designed as a focused, low-risk hard fork. Rather than bundling many speculative changes, it delivers a clear set of complete features, clears governance friction points, and lays structural groundwork in the block body for the protocol upgrades that will follow without requiring those upgrades to be ready.

Ouroboros Peras and Ouroboros Linear Leios are the most significant protocol improvements on Cardano's horizon, but neither is ready for mainnet inclusion: their implementations exist on dedicated development branches, not the mainline node. Dijkstra does not wait for them, nor does it ship them half-finished. Instead, it makes the block body changes they will need, so that when they are ready, they can be activated through intra-era hard forks without requiring another protocol version bump. If either is ready in time, it may be pulled into the Dijkstra hard fork itself.

## In Scope

### Nested Transactions ([CIP-118](https://cips.cardano.org/cip/CIP-0118))

The primary feature of the Dijkstra hard fork. Nested transactions allow a transaction to contain child transactions with independent witnesses and execution contexts, enabling significantly more expressive on-chain logic. Implementation is well advanced and this is the feature that justifies the hard fork.

### Observe Script Type / Guard Scripts ([CIP-112](https://cips.cardano.org/cip/CIP-0112))

Introduces a new script type that can observe transaction validity without being executed as part of spending or minting. Required for expressive transaction guards and a dependency for the PlutusV4 script context.

### Account Address Enhancement, Phase 1 ([CIP-159](https://cips.cardano.org/cip/CIP-0159))

Phase 1 of account address improvements. Delivers the ledger-level definitions needed to support account-style addresses on Cardano.

### Remove isValid from Transactions ([CIP-167](https://cips.cardano.org/cip/CIP-0167))

Removes the `isValid` field from transactions, simplifying the transaction structure and reducing complexity in script execution.

### Non-segregated Block Body Serialization ([CIP-176](https://cips.cardano.org/cip/CIP-0176))

Changes block body serialization to a non-segregated format, a prerequisite for the block body extensions needed by Leios and other future features.

### PlutusV4 Script Context

Introduces the updated script context for PlutusV4, enabling scripts to observe the new transaction and ledger structures introduced in Dijkstra.

### Fee Function Update (Reference Inputs)

Updates the fee function to account for reference inputs. No CIP; parameters will be hardcoded initially with a path to make them updatable later.

### Reference Script Pricing and Limits

Already active on Conway. Requires only a constitution update to make the parameters formally updatable via governance.

### Remove DRep Requirement for Reward Withdrawals ([CIP-181](https://cips.cardano.org/cip/CIP-0181))

Removes the requirement for a DRep delegation to be present when withdrawing staking rewards, reducing friction for ada holders who want to withdraw without participating in governance.

### Block Body Building Blocks

The following are not full feature activations but structural changes to the block body that future intra-era hard forks will depend on. Including them in Dijkstra means those later activations can happen without a protocol version increment.

| Change | Purpose |
|--------|---------|
| [CIP-140](https://cips.cardano.org/cip/CIP-0140) Peras header extensions | Reserves space for Peras voting fields without activating the voting layer |
| [CIP-164](https://cips.cardano.org/cip/CIP-0164) Leios block body extensions | Introduces input block and endorser block structures Leios requires |

## Conditional on Readiness

If Peras or Leios reach mainnet readiness by the time the Dijkstra scope above is delivered, they may be pulled forward and included in the hard fork itself. Otherwise, their block body groundwork ships with Dijkstra and they will be activated via intra-era hard forks planned within this era.

| CIP | Title |
|-----|-------|
| [CIP-140](https://cips.cardano.org/cip/CIP-0140) | Ouroboros Peras: Faster Settlement |
| [CIP-164](https://cips.cardano.org/cip/CIP-0164) | Ouroboros Linear Leios |

## Deferred to Intra-Era Hard Forks

The following are partial inclusions in Dijkstra. Their parameter definitions ship at Dijkstra, but full activation is deferred to a subsequent intra-era hard fork.

| CIP | Title |
|-----|-------|
| [CIP-23](https://cips.cardano.org/cip/CIP-0023) | Fair Min Fees (protocol parameter defined at Dijkstra, fee rule activation deferred) |

## Considered but Not Included in This Hard Fork

The following CIPs were evaluated for Dijkstra but will not be implemented in this era:

| CIP | Title | Rationale |
|-----|-------|-----------|
| [CIP-180](https://github.com/cardano-foundation/CIPs/pull/1157) | Producer Identification | Ledger team does not have the capacity to implement within this era, and alternative node implementations have not reached agreement on the approach. |
| [CIP-50](https://cips.cardano.org/cip/CIP-0050) | Pledge Leverage-Based Staking Rewards | Unlike CIP-23, there is no status-quo default value: at any value of L, pools with zero pledge earn zero rewards, which is a behaviour change from today. It cannot be introduced dormantly and activated later. The ledger team does not have the capacity to implement this safely within the Dijkstra era. Better targeted at Euler. |
| [CPS-0023](https://github.com/cardano-foundation/CIPs/pull/1103) | Cardano Multi Asset Treasury | Requires the CIP-159 account address infrastructure included in Dijkstra to lay the groundwork. Better targeted at Euler once that foundation is in place. |
| [CIP-156](https://cips.cardano.org/cip/CIP-0156) | Plutus Core Builtin Function - multiIndexArray | Not included in the PlutusV4 scope for Dijkstra due to limited Plutus team resources. As it requires no ledger changes, it could potentially be picked up in any upcoming intra-era hard fork if capacity allows. |
| [CIP-160](https://cips.cardano.org/cip/CIP-0160) | Receiving Script Purpose and Addresses | Insufficient ledger resources to implement within this era given its complexity. |
| [CIP-163](https://cips.cardano.org/cip/CIP-0163) | Time-Bound Delegation with Dynamic Rewards | Explicitly contentious: the same community poll saw 54.1% vote NO, making it the most rejected item on the shortlist. Including it would hand opponents a clear mandate to vote against the hard fork. |
| [CIP-173](https://github.com/cardano-foundation/CIPs/pull/1129) | Net Change Limit Parameter | A major overhaul that is not implementable in the current ledger state: the ledger does not retain history of past epoch inflows and outflows, which the NCL calculation requires. Needs a detailed implementation plan before it can be scoped. Better targeted at Euler or beyond. |
| [CIP-0175](https://github.com/cardano-foundation/CIPs/pull/1140) | Stake Pool Hot Credentials | Too complex to implement given existing ledger team commitments for this era. Better targeted at Euler. |

## Wishlist / Future Consideration

Items raised by the community that are not yet candidates for any scheduled hard fork:

| CIP | Title |
|-----|-------|
| [CIP-0182](https://github.com/cardano-foundation/CIPs/pull/1164) | Optimistic Constitutionality |
| [CIP-0168](https://github.com/cardano-foundation/CIPs/pull/1090) | More BuiltinValue Functions |
| [CIP-????](https://github.com/cardano-foundation/CIPs/pull/1072) | More Descriptive Script Purposes |
| (no CIP yet) | Quantum-secure signature schemes |
