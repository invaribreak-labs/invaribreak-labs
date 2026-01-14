Pattern Name

Persistent Weight Not Cleared on Stake Exit

Description

A staking or boosting system maintains a persistent weight metric that is intended to represent long-term participation.
However, when stake exits are processed conditionally (e.g., based on time windows or epochs), persistent weight may not be fully decremented.

Common Preconditions

Time-based or epoch-based accounting.

Separation between principal balance and reward weight.

Conditional logic for state cleanup.

Failure Mode

State cleanup is gated by temporal conditions.

Matured positions bypass persistent state reduction.

Accounting state diverges from economic reality.

Observable Symptoms

Users with zero balance retain reward influence.

Total weight grows without corresponding supply.

Rewards per unit stake decrease over time.
