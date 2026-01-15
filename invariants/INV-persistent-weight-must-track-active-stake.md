# Subordinate Invariant  
## Persistent Weight Must Track Active Stake

This invariant is a **formal abstraction** derived from the canonical report:

→ ../reports/unstake-persistent-weight-not-cleared.md

It MUST be interpreted in the context of that report and SHOULD NOT be cited independently.

Invariant Statement

A participant with no active stake must not retain any persistent reward weight.

Formally:

If activeStake(user) == 0

Then persistentWeight(user) == 0

Rationale

Persistent weight directly determines future reward allocation.

Any residual weight without backing capital represents:

Unearned influence over reward distribution

A violation of capital-based incentive alignment

Weight persistence without stake breaks the fundamental assumption that
rewards track ongoing economic exposure.

Violation Condition

Stake principal is fully withdrawn.

Persistent weight remains non-zero.

This indicates that weight accounting is not strictly coupled
to stake lifecycle events.

Systemic Consequences

Reward allocation becomes manipulable.

Capital efficiency assumptions collapse.

Honest participation is economically penalized.

The system implicitly rewards historical presence instead of active risk.

Notes

This invariant is intentionally minimal and reusable.

It applies broadly to:

Time-weighted staking

Boosted reward systems

Loyalty or epoch-based incentive mechanisms

Concrete exploit realization and protocol-specific analysis
are covered exclusively in the canonical report.
