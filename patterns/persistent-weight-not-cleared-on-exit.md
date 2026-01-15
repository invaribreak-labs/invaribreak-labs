# Pattern  
## Persistent Weight Not Cleared on Stake Exit

This pattern describes a recurring failure mode in staking or boosting systems
where persistent reward weight is not fully cleared when stake exits occur.

It is an abstracted pattern derived from the canonical report:

→ ../reports/unstake-persistent-weight-not-cleared.md


Pattern Description

A protocol maintains a persistent weight metric to represent long-term participation
or loyalty-based reward entitlement.

When stake exits are processed conditionally—for example, gated by epochs,
time windows, or accounting periods—the persistent weight associated with exited stake
may not be fully decremented.

As a result, reward weight becomes decoupled from active economic exposure.

Common Preconditions

This pattern commonly appears in systems with:

Time-based or epoch-based accounting

Separation between stake principal and reward weight

Incremental or windowed state updates

Conditional cleanup logic for historical positions

Failure Mechanism

Persistent state cleanup is gated by temporal conditions

Stake positions that mature beyond the current accounting window
bypass full weight reduction

Weight state reflects historical participation rather than active stake

This produces a state divergence between accounting variables and real capital.

Observable Symptoms

Users with zero active stake retain reward influence

Total persistent weight increases without matching stake supply

Rewards per unit stake decline over time

Historical participants are structurally favored over active ones

Pattern Scope

This pattern is protocol-agnostic and applies to:

Boosted staking systems

Loyalty or aging-based reward mechanisms

Vote-escrow or time-weighted incentive designs

Epoch-based reward distribution models

Notes

This pattern does not describe a specific exploit.
Concrete exploitation paths and impact analysis
are covered in the associated canonical report.
