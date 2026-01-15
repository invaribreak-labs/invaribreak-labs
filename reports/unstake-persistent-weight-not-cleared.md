# Canonical Report  
## Unstake Does Not Clear Persistent Weight

This document is the **primary analytical entry** for this vulnerability class.

It defines the authoritative threat model, invariant violation, and impact analysis.
All related invariant abstractions, pattern generalizations, and exploit-path analyses
are **methodological expansions** of this report and SHOULD NOT be cited independently.

Related documents:
- Invariant abstraction: ../invariants/INV-persistent-weight-must-track-active-stake.md
- Pattern generalization: ../patterns/persistent-weight-not-cleared-on-exit.md
- Exploit path analysis: ../exploit-paths/epoch-gated-weight-retention.md

Threat Model

Protocol implements a time-weighted or boosted staking mechanism.

Rewards are distributed proportionally based on a persistent weight metric.

Users can stake and unstake tokens permissionlessly.

No privileged roles are required to affect staking state.

Assumption

When a user unstakes their full position, all weight contributions associated with that stake are fully removed.

Persistent weight reflects only active, currently staked capital.

Invariant

Total persistent weight must equal the sum of weight contributions from all active stakes.

A user with zero staked balance must have zero persistent weight.

Break Point

Persistent weight is decremented only when unstaking positions from the current accounting window.

Positions matured beyond the current window do not reduce persistent weight on unstake.

Exploit Path

Stake tokens.

Wait until the stake matures beyond the current accounting window.

Unstake the full amount.

Persistent weight remains unchanged despite zero balance.

Repeat staking cycles to accumulate persistent weight without capital lockup.

Impact

Persistent weight becomes decoupled from actual stake supply.

Attackers accrue boosted rewards without holding capital.

Legitimate stakers experience reward dilution.

Results in theft of unclaimed yield.

Fix

Always decrement persistent weight when stake principal is removed.

Persistent weight updates must be independent of stake age or accounting window.
